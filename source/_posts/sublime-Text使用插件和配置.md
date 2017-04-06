---
title: sublime Text使用插件和配置
date: 2017-04-05 14:52:33
tags: [sublime, 笔记]
---

使用Package Control组件安装：
> 按 Ctrl+` 调出console（注：安装有QQ输入法的这个快捷键会有冲突的，输入法属性设置-输入法管理-取消热键切换至QQ拼音）粘贴以下代码到底部命令行并回车：
```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

```
> 重启Sublime Text 3。如果在Perferences->package settings中看到package control这一项，则安装成功。按下Ctrl+Shift+P调出命令面板输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。不好使的话直接访问[Installation](https://packagecontrol.io/installation)

一些我开发过程中常用的插件以及配置使用技巧，超实用

**基础用户设置：**

工具栏 Preferences – Settings-User 加入下面的代码：

```
"ensure_newline_at_eof_on_save": true,		保存时文件末尾自动保留一个空行。
"save_on_focus_lost": true,			窗口失焦自动保存。
"highlight_line": true,				当前行高亮显示。
"highlight_modified_tabs": true,	高亮未保存文件。
```
***

推荐安装的插件

```
Emmet			前端神器
AutoFileName 	自动补全文件名
JsFormat		将JS格式化的插件
SideBarEnhancements		右键菜单增强插件
CssComb			是为CSS属性进行排序和格式化插件(需要nodejs依赖)，快捷键Ctrl+Shift+C
Autoprefixer	CSS3私有前缀自动补全插件

```
