## Service基础知识总结

## 基本概念
Service是Android的四大组件之一，可以看成是没有UI界面的Activity，所有的操作都是在后台完成；

## 使用
定义一个MyService类的方法：

```java
public class MyService extends Service {
    public MyService() {
    }

    // 创建服务
    @Override
    public void onCreate() {
        super.onCreate();
    }

    // 启动
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }

    // 绑定
    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        throw new UnsupportedOperationException("Not yet implemented");
    }

    // 解绑
    @Override
    public boolean onUnbind(Intent intent) {
        return super.onUnbind(intent);
    }


    // 摧毁
    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

### 启动式服务的生命周期

类似于Activity，Service也是可以通过intent对象唤醒的；
```java
Intent it1 = new Intent(this,MyService.class);
startService(it1);
```
执行这个方法，Service生命周期会经过onCreate和onStartCommand两个阶段;
如果不销毁这个服务继续执行startService方法的话，系统不会重新进入onCreate阶段，而是继续进入onStartCommand阶段，除非用户手动进行销毁操作；

销毁服务同样可以通过intent对象进行销毁
```
Intent it2 = new Intent(this, MyService.class);
stopService(it2);
```
### 绑定式服务的生命周期
通过bindService方法进行绑定操作，需要传入三个参数：intent对象、ServiceConnection对象、绑定选项标记flags;
创建一个ServiceConnection对象：
```java
private ServiceConnection conn = new ServiceConnection() {
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {

    }

    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }
};
```
使用bindService方法
```java
Intent it4 = new Intent(this, MyService.class);
bindService(it4,conn,BIND_AUTO_CREATE);
```
解绑操作是通过unbindService方法执行的
```java
unbindService(conn);
```
> 注意事项：
> 如果服务未启动，执行绑定式服务操作Service的生命周期为：
> onCreate --> onBind --> onUnBind --> onDestory；
> 此时服务没有在后台运行，它会随着Activity的摧毁而解绑并销毁；
> 如果服务已经启动，那么bindService方法只能进入onBind阶段，unbindService方法只能进入onUnbind阶段，onDestory阶段还得通过stopService方法来实现；

## 理解IBinder
IBinder是Android中用于远程操作对象的一个基本接口,在Service的绑定方法onBind()就需要返回一个IBinder接口，如果直接创建一个IBinder内部类对象，则需要实现多个方法，这样会导致代码复杂化。

由于Binder类实现了IBinder接口，我们可以自定义一个MyBinder类继承自Binder类，这样onBind()方法只需要返回一个MyBinder对象即可；
## ServiceConnection的作用
在Service的bindService()方法中传入的第二个参数就是ServiceConnection对象；
```java
private ServiceConnection conn = new ServiceConnection() {
    // 当客户端正常连接着服务时，执行服务的绑定操作会被调用
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        Log.d(TAG, "onServiceConnected: 连接正常");
    }

    // 当客户端和服务的连接丢失了，
    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }
};
```
如果Service已经启动了，再执行绑定和解绑操作，Service的生命周期并不会进行onBind和onUnbind的切换。

如果需要进行服务进度监控工作，就需要ServiceConnection对象的onServiceConnected方法来实现，因为在服务已经启动的状态下进行绑定和解绑操作，ServiceConnection对象内的方法每次都会被触发。
## Service通过Binder实现的组件之间的通信
将耗时操作放在Service的onCreate方法中开启一个线程执行，但是这个耗时操作的进度是怎么传递给Activity的呢？
实际上Service类中的onBind方法会返回一个IBinder对象，我们可以将Service中的需要发送的信息通过IBinder对象传递出去，Activity或其他组件在执行绑定或者解绑操作时，bindService()传入的ServiceConnection对象其中的onServiceConnected()方法会传入Service中传出的IBinder对象；
Service中定义的耗时操作，这里把i设置为全局变量，方便后面的Binder对象获取；
```java
private int i;
// 创建服务
@Override
public void onCreate() {
    super.onCreate();
    Log.d(TAG, "onCreate: 服务被创建");
    // 开启一个线程，从1数到100，模拟耗时的任务
    new Thread(){
        @Override
        public void run() {
            super.run();
            try{
                for (i = 0; i < 100; i++) {
                    sleep(1000);
                }
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }.start();
}
```
服务绑定，传入自定义Binder对象：
```java
// 绑定
@Override
public IBinder onBind(Intent intent) {
    Log.d(TAG, "onBind: 服务被绑定");
    // TODO: Return the communication channel to the service.
    return new MyBinder();
}

class MyBinder extends Binder{
    // 定义自己需要的方法（实现进度监控）
    public int getProcess(){
        return i;
    }
}
```
Activity组件中拿取对应的Binder对象：
```java
private ServiceConnection conn = new ServiceConnection() {
    // 当客户端正常连接着服务时，执行服务的绑定操作会被调用
    @Override
    public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
        Log.d(TAG, "onServiceConnected: 连接正常");
        MyService.MyBinder step = (MyService.MyBinder)iBinder;
        int process = step.getProcess();
        Log.d(TAG, "当前进度是:"+process);
    }

    // 当客户端和服务的连接丢失会调用该方法
    @Override
    public void onServiceDisconnected(ComponentName componentName) {

    }
};
```
这样就可以成功在Activity中获取Service中的信息了。

## 以上案例的全部代码：
MainActivity.java
```java
import androidx.appcompat.app.AppCompatActivity;

import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.util.Log;
import android.view.View;

public class MainActivity extends AppCompatActivity {
    public static final int START = R.id.start;
    public static final int STOP = R.id.stop;
    public static final int BIND = R.id.bind;
    public static final int UNBIND = R.id.unbind;
    private static final String TAG = "MyService";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    private ServiceConnection conn = new ServiceConnection() {
        // 当客户端正常连接着服务时，执行服务的绑定操作会被调用
        @Override
        public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
            Log.d(TAG, "onServiceConnected: 连接正常");
            MyService.MyBinder step = (MyService.MyBinder)iBinder;
            int process = step.getProcess();
            Log.d(TAG, "当前进度是:"+process);
        }

        // 当客户端和服务的连接丢失会调用该方法
        @Override
        public void onServiceDisconnected(ComponentName componentName) {

        }
    };


    public void operate(View view) {
        int resourceID = view.getId();
        if(resourceID == START){
            // 启动服务
            Intent it1 = new Intent(this,MyService.class);
            startService(it1);
        } else if (resourceID == STOP) {
            // 停止服务
            Intent it2 = new Intent(this, MyService.class);
            stopService(it2);

        } else if (resourceID == BIND) {
            Intent it4 = new Intent(this, MyService.class);
            bindService(it4,conn,BIND_AUTO_CREATE);
        } else if (resourceID == UNBIND) {
            unbindService(conn);
        }
    }
}
```
MyService.java

```java
import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;
import android.util.Log;

public class MyService extends Service {
    private static final String TAG = "MyService";
    private int i;

    public MyService() {
    }

    // 创建服务
    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate: 服务被创建");
        // 开启一个线程，从1数到100，模拟耗时的任务
        new Thread(){
            @Override
            public void run() {
                super.run();
                try{
                    for (i = 0; i < 100; i++) {
                        sleep(1000);
                    }
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }.start();
    }

    // 启动
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG, "onStartCommand: 服务被启动");
        return super.onStartCommand(intent, flags, startId);
    }

    // 绑定
    @Override
    public IBinder onBind(Intent intent) {
        Log.d(TAG, "onBind: 服务被绑定");
        // TODO: Return the communication channel to the service.
        return new MyBinder();
    }

    class MyBinder extends Binder{
        // 定义自己需要的方法（实现进度监控）
        public int getProcess(){
            return i;
        }
    }

    // 解绑
    @Override
    public boolean onUnbind(Intent intent) {
        Log.d(TAG, "onUnbind: 服务解绑");
        return super.onUnbind(intent);
    }


    // 摧毁
    @Override
    public void onDestroy() {
        Log.d(TAG, "onDestroy: 服务被摧毁");
        super.onDestroy();
    }
}
```