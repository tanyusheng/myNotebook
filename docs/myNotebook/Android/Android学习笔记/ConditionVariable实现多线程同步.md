ConditionVariable是一个实现多线程同步的类，是对wait()和notify()方法的封装。其提供了三个方法open()、close()、block()，简化了多线程的同步操作；

### 使用ConditionVariable实现生产者消费者模型

```Java
public class TestActivity extends Activity {

    private ConditionVariable mCondition;
    private int mNumber;
    private boolean isRunnable;
    private static final String TAG = "TestActivity";

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mCondition = new ConditionVariable();
        mNumber = 0;
        isRunnable = true;

        new Thread(new Runnable() {
            @Override
            public void run() {
                while (isRunnable){
                    //消费者
                    mCondition.close();
                    mCondition.block();
                    Log.d(TAG, "Customer:"+mNumber);
                }
            }
        }).start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                while (isRunnable){
                    //生产者
                    mNumber++;
                    Log.d(TAG, "Producer:"+mNumber);
                    mCondition.open();
                    try {
                        Thread.sleep(1000);
                    }catch (InterruptedException e){
                        e.printStackTrace();
                    }
                }
            }
        }).start();

    }

    @Override
    protected void onPause() {
        super.onPause();
        isRunnable = false;
    }
}
```

### 运行效果：

```
2024-01-14 20:23:41.881  7339-7637  TestActivity            cn.yusheng123.androiddemo            D  Producer:4
2024-01-14 20:23:41.882  7339-7636  TestActivity            cn.yusheng123.androiddemo            D  Customer:4
2024-01-14 20:23:42.882  7339-7637  TestActivity            cn.yusheng123.androiddemo            D  Producer:5
2024-01-14 20:23:42.883  7339-7636  TestActivity            cn.yusheng123.androiddemo            D  Customer:5
2024-01-14 20:23:43.883  7339-7637  TestActivity            cn.yusheng123.androiddemo            D  Producer:6
2024-01-14 20:23:43.884  7339-7636  TestActivity            cn.yusheng123.androiddemo            D  Customer:6
2024-01-14 20:23:44.884  7339-7637  TestActivity            cn.yusheng123.androiddemo            D  Producer:7
2024-01-14 20:23:44.885  7339-7636  TestActivity            cn.yusheng123.androiddemo            D  Customer:7
```

通过例子，我们发现close()和block()是一起配合使用的，目的就是阻塞线程，等待open()解除线程阻塞。

这里如果单独使用block()方法就达不到阻塞的效果，在1s内会输出很多次；