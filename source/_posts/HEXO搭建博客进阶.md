---
title: HEXO搭建个人博客（进阶）
date: 2017/05/06 14:20
tags: [blog, hexo, nodejs]
categories:
- nodejs
---
上篇{% post_link HEXO搭建博客 HEXO搭建博客 %}讲述了如何搭建基于HEXO的博客，这篇就来讲讲博客的进阶功能吧！
<!-- more -->

## 自定义域名
如果你的博客是搭建在github pages上，而又想用自己的域名，这时候你可以绑定自己的域名。

### 申请域名
要绑定域名首先你得要有一个域名，你可以在喜欢的域名提供商那里购买一个域名(比如：[万网](https://wanwang.aliyun.com/))，便宜的只要几块钱就够了。这里安利一个国外的免费域名的网站[Freenom](https://my.freenom.com/clientarea.php)(手动滑稽~~)。

### 添加域名解析
一般购买域名后，我们可以在管理后台设置域名解析，我这边使用的是国外的Freenom提供的免费域名(:D),国内的[CloudXNS](https://www.cloudxns.net)做的接管，所以以CloudXNS后台为例(操作大同小异):

我们需要添加三条记录，两条A记录和一条CNAME记录：
添加两条A记录，IP分别是：192.30.252.153 和 192.30.252.154，前缀为`www`:

![第一条A记录](/images/hexo博客搭建/1494054258.jpg)
![第二条A记录](/images/hexo博客搭建/1494054280.jpg)

添加一条CNAME记录, 前缀为`@`, 记录值为`你的github账号.github.io.`,(注意,最后有一个点)：
![CNAME记录](/images/hexo博客搭建/1494054305.jpg)

### 添加CNAME文件
最后在我们的博客项目下的`source`文件夹下创建一个`CNAME`文件(文件名注意大写),里面的内容就是我们想要的域名，这里我写的是自己的域名：
![CNAME](/images/hexo博客搭建/1494054866.jpg)

### 部署
```bash
$ hexo d
```
当重新部署到github pages之后，你会发现可以使用自己的域名来访问博客啦！！
![ztktct.tk](/images/hexo博客搭建/1494055375.jpg)

## 阅读统计
### 字数、时长统计(搭配NexT主题)
1. 安装 `hexo-wordcount` 插件
```bash
$ npm install hexo-wordcount --save
```
2. 开启配置
编辑 `themes\next\_config.yml` 文件:
```yml
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true # 字数统计
  min2read: true # 阅读时长预计
```
3. 显示
当我们配置好之后，我们就可以在页面上看到效果了：
![hexo-wordcount](/images/hexo博客搭建/1494056139.jpg)
但是此时并没有显示单位，这让一个具有强迫症的人不能忍啊，要修改其实也很简单，我们找到 `themes\next\_macro\post.swig` 文件，修改其中两行代码即可:
![post.swig](/images/hexo博客搭建/1494057471.jpg)
修改后效果：
![hexo-wordcount](/images/hexo博客搭建/1494057495.jpg)

### 阅读次数统计(搭配NexT主题)
1. 前往[leancloud](https://leancloud.cn)注册账号，并创建应用

2. 获取应用的 `app_id` 和 `app_key`:
![leancloud](/images/hexo博客搭建/1494058351.jpg)

3. 创建名为`Counter`的class,注意，名称必须为`Counter`
![Counter](/images/hexo博客搭建/1494058727.jpg)

4. 编辑 `themes\next\_config.yml` 文件:
```yml
# Show number of visitors to each article.
# You can visit https://leancloud.cn get AppID and AppKey.
leancloud_visitors:
  enable: true
  app_id: #yourapp_id
  app_key: #yourapp_key
```

5. 效果：
![阅读统计](/images/hexo博客搭建/1494058898.jpg)

## 为外部链接添加nofollow
有时候我们不想让爬虫通过我们博客的外链而离开，要怎么办呢？a标签有个 `rel=nofollow` 的属性用于禁止爬虫怕取，但是markdown格式该怎么用呢？这儿有一个 [hexo-autonofollow](https://github.com/liuzc/hexo-autonofollow) 插件可用:
```bash
$ npm install hexo-autonofollow --save
```
安装完后我们在项目的`_config.yml`文件中修改即可：
```yml
# Nofollow
nofollow:
  enable: true
```

## 网易云跟帖
是时候为我们的博客添加评论功能了！
1. 我们先去[网易云跟帖](https://manage.gentie.163.com/)中注册
2. 获取代码，拿到`productKey`
3. 编辑 `themes\next\_config.yml` 文件:
```yml
gentie_productKey: # 你的productKey
```

## 参考资料
{% post_link HEXO搭建博客 HEXO搭建博客 %}
[hexo高阶教程next主题优化](http://cherryblog.site/Hexo-high-level-tutorialcloudmusic,bg-customthemes-statistical.html)
