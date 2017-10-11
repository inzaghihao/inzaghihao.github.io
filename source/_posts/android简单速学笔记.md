---
title: android简单速学笔记
date: 2017-03-24 18:37:53
tags: [笔记, android]
---


新创建的项目自动创建一个activity_main.xml 布局文件和一个MainActivity.java 入口文件，`并且在 setContentView(R.layout.activity_main);`设置启动页面

### 通过id 找到元素按钮并赋值

- this.findViewById(R.id.id) 参数中是int类型 通过定义@+id 定义元素id，自动生成在R.java类文件中生成转换成相应的16进制int整型，使用R.id 寻找该元素
- Button btn = (Button)findViewById(R.id.btn) 强制转换成botton 类型的元素
- OnclickListener 绑定监听器


### Intent 类起到了应用之间数据交换的桥梁

#### 界面之间的跳转 在使用前务必先在AndroidManifest.xml进行注册 `<activity android:name=".Index"></activity>`
```
Intent intent = new Intent();
intent.setClass(MainActivity.this,Index.class);
startActivity(intent);`
```

#### Intent Extra 属性实现页面直接的数据传递

通过 `intent.putExtra(key,value)` 把需要传递的内容放到Extra 中
在第二个Activity 中得到Extra
```
Intent intent = getIntent();
intent.getExtras().get("key");
```

```
//设置传递
intent.putExtra("name","hao");

//获取
Intent intent =  this.getIntent();
String name = intent.getExtras().get("name").toString();
((TextView)findViewById(R.id.textView2)).setText(name);

```

#### Intent Action 和 Data 属性

```
//设置启动一个网页
intent.setAction(intent.ACTION_VIEW);
intent.setData(Uri.parse("http://www.baidu.com"));
startActivity(intent);

//设置打电话
intent.setAction(intent.ACTION_CALL);
intent.setData(Uri.parse("tel://110"));
startActivity(intent);  //启动一个定义的意图
```


如需要权限在 AndroidManifest.xml 文件中 application 标签下加入以下权限
```
<uses-permission android:name="android.permission.CALL_PHONE"></uses-permission>
```

隐式启动界面,在 AndroidManifest.xml 中 设置要启动的界面的 `<intent-filter>`

```
<intent-filter>
    <action android:name="abc"/>
    <category android:name="android.intent.category.DEFAULT"/>
</intent-filter>


//在第一个界面中设置 setAction 启动即可
Intent intent = new Intent();
intent. setAction("abc");
startActivity(intent);
```

使用 `intent.setClassName(包名,包名+activity 名)` 打开外部程序

```
intent.setClassName("com.android.deskclock","com.android.deskclock.DeskClock");

intent.setComponent(new ComponentName("com.android.deskclock","com.android.deskclock.DeskClock"));
```



