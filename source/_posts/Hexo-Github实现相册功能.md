---
title: Hexo+Github实现相册功能
date: 2019-05-12 11:17:38
tags: yillia
declare: true
toc: true
---
- 最终效果请看这里：https://javacfox.github.io/photos/
- 本文参考了Lawlite大神的实现方法，如果想看具体原理，请访问：http://lawlite.me/
# 一、具体实现流程
## 1、github操作
- 在github上新建一个仓库，用于存储相册
- github的相关命令操作，请参考：https://blog.csdn.net/weixin_43935907/article/details/89469444
## 2、博客操作
- 进入到你的博客目录下执行 hexo new page "photos",就会出现一个这样的新目录,这个photos目录在source文件下，删除里面的文件index.md
- 配置yilia主题让其显示出来
```java
menu:
  主页: /
  随笔: /tags/随笔/
  相册: /photos/
```
- 生成photos.html文件,具体怎么生成的我也不是很清楚，不知道就先模仿别人吧：https://github.com/litten/BlogBackup ，把图片中的那些文件复制到/source/photos文件夹下
![image](https://note.youdao.com/yws/public/resource/98b35365d81e26a1aecc6e9187adfa64/xmlnote/4EF1AEC0A14C457CA31FCFFCD1906458/3429)
- 修改 ins.js 文件的 render()函数.这个函数是用来渲染数据的
修改图片的路径地址.minSrc 小图的路径. src 大图的路径.修改为自己的图片路径(七牛的, github的路径)
```java
    var render = function render(res) {
      var ulTmpl = "";
      for (var j = 0, len2 = res.list.length; j < len2; j++) {
        var data = res.list[j].arr;
        var liTmpl = "";
        for (var i = 0, len = data.link.length; i < len; i++) {
          var minSrc = 'https://raw.githubusercontent.com/javacfox/Blog_Back_Up/master/min_photos/' + data.link[i];
          var src = 'https://raw.githubusercontent.com/javacfox/Blog_Back_Up/master/photos/' + data.link[i];
```
值得注意的是这里的地址是图片的下载地址，不是图片的存储地址，比如我的：
![image](https://note.youdao.com/yws/public/resource/8619289468746fa20a1b8fcc75ad45d6/xmlnote/C8F730B050C24F688A1003D26104CC58/3460)
点击download后地址栏的地址
- 然后把tool.py和ImageProcess.py文件复制到博客根目录下
```java
这两个文件主要实现了如下几个功能：
1.裁剪图片
2.压缩图片
3.github提交
4.json数据处理
```
- 由于需要执行python语言，需要安装Python环境，[下载地址](https://www.python.org/downloads/release/python-352/)
![image](https://note.youdao.com/yws/public/resource/c27f5eb48d0bc82574e39f9aa46e402b/xmlnote/7AE5AE196F3E466A8CA97B67C8CC8695/3541)
下载完成后，直接点击安装，把自动配置环境变量勾选上，完成后就可以执行python脚本
- 在博客根目录下创建photos文件夹和min_photos文件夹
![image](https://note.youdao.com/yws/public/resource/7e53bd7f3d8f10e645bd918f8b4e1585/xmlnote/4F0491E696254AAD8F9F73FA2B01A102/3497)
到这里为止基本上完成了所有的准备工作，接下来我们试一下吧！
## 3.上传图片操作
- 将图片存入博客目录下的photos文件夹中,图片名字格式为:xxxx-xx-xx_xxxx.jpg/png 比如2019-04-23_小编.jpg
- 代开cmd，cd到博客目录下，执行如下命令
```java
> py tool.py
```
这里有一个坑，python too.py 命令的时候出现no module named PIL错误，出现这错误的原因是: PIL模块找不到,PIL 模块已经过时了.

解决办法：执行下面命令
```java
> pip install pillow
```
- 图片准备好了，然后开始生成photos.html
```java
> hexo clean
> hexo g
> hexo s
访问本地http://localhost:4000，检查是否成功
> crtl c
> y
> hexo d
```

