# TV_T9_KeyBordView

库引用方法：
```
// 根目录的buidl.gradle里设置
allprojects {
		repositories {
			...
			maven { url 'https://www.jitpack.io' }
		}
	}
...
// 项目App目录的buidl.gradle里引用
dependencies {
	        implementation 'com.github.jaychou2012:TV_T9_KeyBordView:1.0.0'
	}
```


效果图：

![运行效果](https://raw.githubusercontent.com/jaychou2012/TV_T9_KeyBordView/master/Screenshot_1556891580.png)

![运行效果](https://raw.githubusercontent.com/jaychou2012/TV_T9_KeyBordView/master/Screenshot_1556891594.png)

![运行效果](https://raw.githubusercontent.com/jaychou2012/TV_T9_KeyBordView/master/Screenshot_1556891741.png)

![运行效果](https://raw.githubusercontent.com/jaychou2012/TV_T9_KeyBordView/master/Screenshot_1556891744.png)


使用方法：
```java
package com.td.t9keybord;

import android.app.Activity;
import android.graphics.drawable.Drawable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.Toast;

import com.td.keybordlib.KeyBordView;
import com.td.keybordlib.Utils;

public class MainActivity extends Activity implements KeyBordView.KeySelectIml {
    private KeyBordView keyBordView;
    private EditText editText;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        keyBordView = findViewById(R.id.keyBordView);
        editText = findViewById(R.id.editText);
        keyBordView.setDefaultKeyboard(true);
        // 键盘按键监听
        keyBordView.setKeySelectListener(this);
        // 是否键盘按键正方形显示，不按照宽度自适应方式显示
        keyBordView.setSquareKey(true);

        Drawable icon_search = getResources().getDrawable(R.mipmap.icon_search);
        icon_search.setBounds(0, 0, Utils.dp2px(this, 20),
                Utils.dp2px(this, 20));
        editText.setCompoundDrawablePadding(5);
        editText.setCompoundDrawables(icon_search, null, null, null);
    }

    // 键盘按键监听
    @Override
    public void keySelect(String key, boolean t9) {
        StringBuilder stringBuilder = new StringBuilder(editText.getText().toString().trim());
        stringBuilder.append(key);
        editText.setText(stringBuilder);
    }

    // 键盘点击后退按键删除监听
    @Override
    public void keyDelete(boolean t9) {
        String string = editText.getText().toString().trim();
        if (string.length() == 0) {
            return;
        }
        String subString = string.substring(0, string.length() - 1);
        editText.setText(subString);
        if (subString.length() == 0) {
            editText.setHint("输入片名/人名的首字母或全拼");
        }
    }

    // 键盘点击清空按键监听
    @Override
    public void keyClear(boolean t9) {
        editText.setText("");
        editText.setHint("输入片名/人名的首字母或全拼");
    }

    // 当keyBordView.setSquareKey(true);时，这个布局回调可以获取键盘的宽度，来设置其他布局的宽度和键盘宽度相等
    @Override
    public void onLayoutComplete(int width) {
        editText.setLayoutParams(new LinearLayout.LayoutParams(width, LinearLayout.LayoutParams.WRAP_CONTENT));
    }
}
```
