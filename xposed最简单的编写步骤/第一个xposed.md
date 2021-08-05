1.使用安卓studio 新建一个安卓工程





2.导入相应的jar包

  	1.切换到project视图，在 app目录下新建一个lib，并导入XposedBridgeApi-xx.jar
  	2.把XposedBridgeApi-xx.jar加为外部库

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\1-1628177390654.png)

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\2.png)



3.在AndroidManifest.xml中增加三个meta-data用来标示Xposed模块以及模块信息

```html
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="cn.lovelywhite.wxautologin">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme" >
        <meta-data
            android:name="xposedmodule"
            android:value="true" />
        <meta-data
            android:name="xposeddescription"
            android:value="This is a plug-in that can stop advertising" />
        <meta-data
            android:name="xposedminversion"
            android:value="82" />
    </application>

</manifest>
```



![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\5.png)

4.在src新建assets目录，并在assets目录下新建一个xposed_init文本，在文本写入hook包的包名.Hook类的类名：

​	如当前hook程序为包名为：com.hookdemo ,hook程序的类名为Main，

​	 则该填入：

```
com.hookdemo.Main
```

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\7-1628177450614.png)

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\8-1628177463556.png)

5.修改编译的配置：

app/src/build.gradle

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\4-1628177482498.png)

两个位置

```
sourceSets{main{assets.srcDirs=['src/main/asets','src/main/assets/']}}
provided file(‘de.robv.android.xposed:api:82')
```





6.编写hook代码，类名和xpoed_init.类名一样

```
package com.example.hookdemo;

import android.os.Bundle;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.TextView;

public class Main extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        final TextView tv = findViewById(R.id.tv_hook);

        FloatingActionButton fab = findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                tv.setText(sRes());
            }
        });
    }

    String sRes() {
        return "你未被劫持";
    }
}
```





注：要修改五个地方

![](C:\Users\tjp40922\Desktop\xpoedrelate\xposed最简单的编写步骤\第一个xposed.assets\090712lo6u3i5luoxis5vu-1628177514607.png)







可参考：

https://blog.csdn.net/ASSYIRAN/article/details/86139496

https://www.52pojie.cn/thread-873366-1-1.html

https://www.zhaokeli.com/article/8389.html

https://lovelywhite.cn/android/wxautologin.html

https://www.cnblogs.com/gordon0918/p/6732100.html