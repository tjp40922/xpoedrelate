1.使用安卓studio 新建一个安卓工程





2.导入相应的jar包

  	1.切换到project视图，在 app目录下新建一个lib，并导入XposedBridgeApi-xx.jar

​	  2.把XposedBridgeApi-xx.jar加为外部库





3.找到AndroidManifest.xml

​	加入一下三行代码，让这个模块变成克识别的xposed模块

```html
        <meta-data
            android:name="xposedmodule"
            android:value="true" />
        <meta-data
            android:name="xposeddescription"
            android:value="This is a plug-in that can stop advertising" />
        <meta-data
            android:name="xposedminversion"
            android:value="82" />
```



4.在src新建assets目录，并在assets目录下新建一个xposed_init文本，在文本连麦你下入hook包的包名.Hook类的类名：

​	如当前hook程序为包名为：com.hookdemo ,hook程序的类名为Main，

​	 则该填入：

```
com.hookdemo.Main
```



5.修改编译的配置：app/src/build.gradle

```


provided file(‘de.robv.android.xposed:api:82')
```





6.编写hook代码，类名和xpoed_init类名一样

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