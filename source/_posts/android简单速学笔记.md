---
title: android简单速学笔记
date: 2017-03-24 18:37:53
tags: [笔记, android]
---

## android 速学简单语法

新创建的项目自动创建一个activity_main.xml 布局文件和一个MainActivity.java 入口文件，`并且在 setContentView(R.layout.activity_main);`设置启动页面

### 通过id 找到元素按钮并赋值

- this.findViewById(R.id.) 参数中是int类型 通过定义@+id 定义元素id，自动生成在R.java类文件中生成转换成相应的16进制int整型，使用R.id 寻找该元素
- Button btn = (Button)findViewById(R.id.btn) 强制转换成botton 类型的元素
- OnclickListener 绑定监听器


### Intent 类起到了应用之间数据交换的桥梁

#### 界面之间的跳转 在使用前务必先在AndroidManifest.xml进行注册 `<activity android:name=".Index"></activity>`
```
Intent intent = new Intent();
intent.setClass(MainActivity.this,Index.class);
startActivity(intent);`
```

### Intent Extra 属性实现页面直接的数据传递

`intent.putExtra(key,value)`
