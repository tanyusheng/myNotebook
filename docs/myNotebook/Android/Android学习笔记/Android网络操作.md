

## Android网络操作

### 网络权限

网络权限申请：在AndroidManifest.xml的application标签前面声明Intentnet连接权限；

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

Android9.0之后，http请求有了进一步限制

需要创建安全配置文件：

在res文件夹下创建一个资源文件夹`xml`,并创建一个`network_security_config`文件,添加`cleartextTrafficPermitted`属性，具体写法为：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true"/>
</network-security-config>
```

添加安全配置文件：

AndroidManifest.xml中的Application声明，具体写法如下：

```xml
android:networkSecurityConfig="@xml/network_security_config"
```

### get方法

①实例化一个URL对象

② 获取HttpURLConnection实例

③ 设置和请求相关属性

④ 获取响应码

⑤ 判断响应码并获取响应数据

使用get请求的示例代码如下：

```java
public class MainActivity extends AppCompatActivity {
    public static final int BTN_GET = R.id.btn_get;
    public static final int BTN_POST = R.id.btn_post;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void myClick(View view){
        int viewId = view.getId();
        if(viewId == BTN_GET){
            // 网络请求操作需要开辟一个新线程
            new Thread(){
                @Override
                public void run() {
                    super.run();
                    urlGet();
                }
            }.start();
        } else if (viewId == BTN_POST) {

        }
    }

    private void urlGet() {
        //①实例化一个URL对象
        try {
            URL url = new URL("http://www.imooc.com/api/teacher?type=3&cid=1");
            //② 获取HttpURLConnection实例
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            //③ 设置和请求相关属性
            // 请求方式
            conn.setRequestMethod("GET");
            // 请求超时时长
            conn.setConnectTimeout(3000);
            //④ 获取响应码
            if(conn.getResponseCode() == HttpURLConnection.HTTP_OK){
                //⑤ 判断响应码并获取响应数据
                InputStream in = conn.getInputStream();
                // 在循环中读取输出流
                byte[] b = new byte[1024];
                int len = 0;
                ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
                while ((len = in.read(b)) > -1){
                    // 将字节流里的内容存入缓存流
                    // 参数1：待写入的数组
                    // 参数2：起点
                    // 参数3：长度
                    byteArrayOutputStream.write(b,0,len);
                }
                String msg = new String(byteArrayOutputStream.toByteArray());
                Log.e("TAG", msg+"==========");
            }

        } catch (MalformedURLException e) {
            throw new RuntimeException(e);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

#### 网络请求中常见异常处理：

**IllegalStateException**

如果遇到java.lang.IllegalStateException: Could not execute method for android:onClick这种异常，说明存在相关网络请求操作的方法写在了主线程中，解决方法是重新开辟一个新线程执行耗时操作；

**RuntimeException**

如果遇到java.lang.RuntimeException: java.io.IOException: Cleartext HTTP traffic to www.xxxx.com not permitted这种异常，说明没有添加安全配置文件，因此需要再xml目录下创建network_security_config文件并添加cleartextTrafficPermitted属性，并且在清单文件中声明安全配置文件的位置；

**SecurityException**

如果遇到java.lang.SecurityException: Permission denied (missing INTERNET permission?)这种异常，说明没有在清单文件的application标签前添加Internet权限，

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

**SocketException**

如果遇到SocketException: socket failed: EPERM(Opreation not permitted)这种异常，通常是由于第一次安装时没有添加安全配置文件，导致后续添加并再次运行时配置文件依然未生效。

解决方法是：卸载应用，重新运行安装；

### post方法

```java
public class MainActivity extends AppCompatActivity {
    public static final int BTN_GET = R.id.btn_get;
    public static final int BTN_POST = R.id.btn_post;
    private EditText mAccount,mPwd;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mAccount = findViewById(R.id.account);
        mPwd = findViewById(R.id.pwd);
    }

    public void myClick(View view){
        int viewId = view.getId();
        if(viewId == BTN_GET){
            // 网络请求操作需要开辟一个新线程
            new Thread(){
                @Override
                public void run() {
                    super.run();
                    urlGet();
                }
            }.start();
        } else if (viewId == BTN_POST) {
            // 内部类使用局部变量，需要用final修饰
            final String account = mAccount.getText().toString();
            final String pwd = mPwd.getText().toString();
            new Thread(){
                @Override
                public void run() {
                    super.run();
                    urlPost(account,pwd);
                }
            }.start();
        }
    }

    private void urlGet() {
        ......
    }

    private void urlPost(String account,String pwd) {
        //①实例化一个URL对象
        try {
            URL url = new URL("https://www.imooc.com/api/okhttp/postmethod");
            //② 获取HttpURLConnection实例
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            //③ 设置和请求相关属性
            // 请求方式
            conn.setRequestMethod("POST");
            // 请求超时时长
            conn.setConnectTimeout(3000);
            // 设置允许输出
            conn.setDoOutput(true);
            // 设置提交数据的类型
            conn.setRequestProperty("Content-Type","application/x-www-form-urlencoded");
            // 获取输出流(请求正文)
            OutputStream out = conn.getOutputStream();
            out.write(("acount="+account+"&pwd="+pwd).getBytes());
            //④ 获取响应码
            if(conn.getResponseCode() == HttpURLConnection.HTTP_OK){
                //⑤ 判断响应码并获取响应数据
                InputStream in = conn.getInputStream();
                // 在循环中读取输出流
                byte[] b = new byte[1024];
                int len = 0;
                ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
                while ((len = in.read(b)) > -1){
                    // 将字节流里的内容存入缓存流
                    // 参数1：待写入的数组
                    // 参数2：起点
                    // 参数3：长度
                    byteArrayOutputStream.write(b,0,len);
                }
                String msg = new String(byteArrayOutputStream.toByteArray());
                Log.e("TAG", msg+"==========");
            }

        } catch (MalformedURLException e) {
            throw new RuntimeException(e);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

## JSON数据解析

利用JSONObject解析

* `getJSONObject(String name)`获取JSONObject对象 ;
* `toString()`把JSONObject对象转为json字符串;
* 对于JSON对象可以使用getInt()、getString()方法获取对应的值;

案例：使用JSONObject解析JSON字符串，并将结果显示在ListView中：

```java
public class JSONActivity extends AppCompatActivity {
    public static final String TAG = "TYS";
    private TextView txt1,txt2;
    private ListView listView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_json);
        listView = findViewById(R.id.list_view);
        findViewById(R.id.parseBtn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Log.e("TAG", "onClick:click-------");
                parseByJSONObject();
            }
        });
    }

    public void parseByJSONObject(){
        new Thread(){
            @Override
            public void run() {
                super.run();
                String str = urlGet();
                // 利用JSONObject对象(满足JSON格式要求的字符串)
                try {
                    JSONObject jsonObject = new JSONObject(str);
                  	// 准备适配器的数据源
                    List<Map<String, String>> list = new ArrayList<>();
                    // -------- 获取JSON数组
                    // 解析JSON数组
                    JSONArray arr = jsonObject.getJSONArray("data");
                    // 遍历JSON数组
                    for(int i = 0; i < arr.length(); i++){
                        JSONObject obj = arr.getJSONObject(i);
                        String name = obj.getString("name");
                        String id = obj.getString("id");
                        Log.e(TAG, "id："+id+"，name："+name);
                        Map<String,String> map = new HashMap<>();
                        map.put("name",name);
                        map.put("id",id);
                        list.add(map);
                    }
                    // 创建SimpleAdapter(List<Map<String,Object>>,布局资源，from,to)
                    String[] from = {"name","id"};
                    int[] to = {R.id.item_name,R.id.item_id};
                    final SimpleAdapter adapter = new SimpleAdapter(JSONActivity.this,list,R.layout.item,from,to);
                    // 显示到界面上
                    // 在子线程中如何解决UI线程的显示问题，使用runOnUiThread讲操作权由子线程移交给主线程
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            listView.setAdapter(adapter);
                        }
                    });

                } catch (JSONException e) {
                    throw new RuntimeException(e);
                }

            }
        }.start();
    }

    private String urlGet() {
        //①实例化一个URL对象
        try {
            URL url = new URL("http://www.imooc.com/api/teacher?type=2&cid=1");
            //② 获取HttpURLConnection实例
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            //③ 设置和请求相关属性
            // 请求方式
            conn.setRequestMethod("GET");
            // 请求超时时长
            conn.setConnectTimeout(3000);
            //④ 获取响应码
            if(conn.getResponseCode() == HttpURLConnection.HTTP_OK){
                //⑤ 判断响应码并获取响应数据
                InputStream in = conn.getInputStream();
                // 在循环中读取输出流
                byte[] b = new byte[1024];
                int len = 0;
                ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
                while ((len = in.read(b)) > -1){
                    // 将字节流里的内容存入缓存流
                    // 参数1：待写入的数组
                    // 参数2：起点
                    // 参数3：长度
                    byteArrayOutputStream.write(b,0,len);
                }
                String msg = new String(byteArrayOutputStream.toByteArray());
                Log.e("TAG", msg+"==========");
                return msg;
            }

        } catch (MalformedURLException e) {
            throw new RuntimeException(e);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return null;
    }
}
```

### 异常处理：

**CallFromWrongThreadException**

如果系统报错：Only the original thread that create a view hierarchy can touch its views，则表明主线程创建了组件，也只能在主线程修改组件；

解决方案：

使用方法`runOnUiThread()`将操作权由子线程移交给主线程；

```java
runOnUiThread(new Runnable() {
    @Override
    public void run() {
        txt1.setText(msg);
    }
});
```

不能在子线程中修改UI线程

## GSON解析

使用JSON解析框架GSON能极大提高效率，

如果要使用GSON需要再对应模块的`build.gradle`文件下添加编译依赖：

```
implementation 'com.google.code.gson:gson:2.8.6'
```

常用方法：

`toJson()` 将bean对象转为json字符串

`fromJson()` 将json字符串转为bean对象

使用案例：

```java
private void parseByGSON() {
    //添加依赖
    // 实例化GSON对象
    final Gson gson = new Gson();
    // 将对象转变为JSON字符串
    Book b = new Book("第一行代码","郭霖","学习Android基础宝典");
    String str = gson.toJson(b);
    Log.e(TAG, "parseByGSON: "+ str );
    // 将JSON字符串转为对象（前提是存在这个类）
    Book b2 = gson.fromJson(str, Book.class);
    Log.e(TAG, "b2: "+b2 );
    Log.e(TAG, "标题:"+b2.getTitle()+",内容:"+b2.getContent());
    // 通过真实的JSON字符串测试一下fromJson方法
    new Thread(){
        @Override
        public void run() {
            super.run();
            // 这里调用get方法获取到对应的字符串
            String msg = urlGet();
            // 这里的Test类需要自己创建，与JSON对象的结构需要保持一致
            Test test = gson.fromJson(msg, Test.class);
            Log.e(TAG,test+"=====" );
            Log.e(TAG, test.getStatus()+"---"+test.getMsg()+"---"+test.getData().getContent());
        }
    }.start();
}
```

