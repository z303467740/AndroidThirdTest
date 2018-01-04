## Intent
### 1.自定义WebView
配置文件
```
    <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <data android:scheme="http" />
            </intent-filter>
```
配置文件极其重要。调用时会自动匹配”http“。
activity_my_browser.xml
```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="edu.fjnu.cse.mybrowser.MyBrowserActivity">

    <WebView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/webView">
    </WebView>
</RelativeLayout>
```
MyBrowseractivity.java
```
 protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my_browser);
        Intent intent = getIntent();
        Uri data = intent.getData();
        URL url = null;
        try {
            url = new URL(data.getScheme(), data.getHost(),
                    data.getPath());
        } catch (Exception e) {
            e.printStackTrace();
        }
        startBrowser(url);
    }
    private void startBrowser(URL url) {
        WebView webView = (WebView) findViewById(R.id.webView);
        //WebView加载web资源
        webView.loadUrl(url.toString());
        //覆盖WebView默认使用第三方或系统默认浏览器打开网页的行为，使网页用WebView打开
        webView.setWebViewClient(new WebViewClient(){
            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                // TODO Auto-generated method stub
                //返回值是true的时候控制去WebView打开，为false调用系统浏览器或第三方浏览器
                view.loadUrl(url);
                return true;
            }
        });
    }
```
### 2.获取URL地址并启动隐式Intent的调用
intent.xml
```
   <EditText
            android:id="@+id/et"
            android:layout_height="40dp"
            android:layout_width="match_parent"
            >
            </EditText>
        <Button
            android:id="@+id/bn"
            android:layout_width="120dp"
            android:layout_height="40dp"
            android:layout_marginLeft="120dp"
            android:text="浏览该网页"/>
```
先定义一个LinearLayout，然后editText获取URL，Button触发事件，启动隐式的intent调用。
intentURL.java
```
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.intent_url);
        Button bn = (Button)findViewById(R.id.bn);
        final EditText et = (EditText)findViewById(R.id.et);
        bn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                /* String edittext=et.getText().toString();
                String data = edittext;
                Uri uri = Uri.parse(data);
                intent.setAction(Intent.ACTION_VIEW);
                intent.setData(uri);
                startActivity(intent);*/
                String url=et.getText().toString();
                Intent intent = new Intent();
                intent.setAction(Intent.ACTION_VIEW);
                intent.setData(Uri.parse(url));
                startActivity(intent);

            }
        });
    }
```
注释的部分采用的是显示调用。下面那部分是隐式调用。获取editText里面的字符转换成字符串。intent会隐式调用URL，检测到mybrower的存在，会让用户选择打开的浏览器。



## 效果截图

![enter description here][1]

![enter description here][2]

![enter description here][3]


  [1]: ./1.png "1"
  [2]: ./2.png "2"
  [3]: ./3.png "3"