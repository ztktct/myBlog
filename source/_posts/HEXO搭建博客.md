---
title: HEXO搭建个人博客
date: 2017/04/28 22:20
tags:
- blog
- hexo
- nodejs
categories:
- nodejs
---
好久以前就想搞自己的博客了，拖了好久(懒啊！)，趁者这次闲着，就来搞一下博客吧！博客采用[Hexo](https://hexo.io/)搭建,主题拿的是[NexT](http://theme-next.iissnan.com/)，它提供了三套样式，而且还支持响应式，效果相当不错！(你作为一个前端，好意思拿别人的主题？:D)
<!-- more -->
## 博客搭建

### 环境配置
1. 安装Nodejs，请从[Nodejs](https://nodejs.org/en/download/)官网下载Nodejs，推荐使用[nvm](https://github.com/creationix/nvm)来管理Nodejs版本！
2. 安装[Git](https://git-scm.com/downloads)，开发必备，推荐使用！
3. 申请[Github](https://github.com/)，我们以后可以直接拿[GitHub Pages](https://pages.github.com/)来当我们的博客地址，比如我的博客地址[ztktct](https://ztktct.github.io/)！

### HEXO安装与配置
#### 安装HEXO
在安装好Nodejs和Git之后，我们就可以来安装HEXO了：
```bash
$ sudo npm install hexo -g
```
#### 创建HEXO项目
在安装好HEXO后，我们就可以开始搭建自己的项目了：
```bash
$ hexo init myBlog
```
之后你会看到在当前目录下多了一个`myBlog`文件夹,进入该目录，执行`hexo server`命令，之后你就可以在浏览器中打开 http://localhost:4000 来访问刚刚搭建好的博客了，没错，就是这么简单！

#### 配置HEXO主题
搭建好博客之后，默认提供了一套简洁的主题，但是总觉得不够优雅，不够美观，怎么办呢？其实，[HEXO](https://github.com/hexojs/hexo/wiki/Themes)提供了很多套主题供我们使用，可以选择自己喜欢的主题，我这个博客用的就是其中一个叫做[NexT](https://github.com/iissnan/hexo-theme-next)的主题，就以该主题为例来说明吧！
首先，我们先从[NexT](https://github.com/iissnan/hexo-theme-next)上克隆该项目到我们的博客项目中去：
```bash
$ cd myBlog
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
之后你会发现在`myBlog/theme`目录下多了一个`next`文件夹，这就是NexT主题文件夹了。博客项目下有一个`_config.yml`文件，我们的博客配置基本上都在这里改了。其中有个theme属性将其改为`next`即可使用该主题，主题配置请参考官方网站[NexT](http://theme-next.iissnan.com/)

### 开始写作
HEXO提供了创建新文章的命令：
```bash
$ hexo new [layout] <title>
```
或者我们可以在`source/_posts`目录下手动添加Markdown文件，以此开始一篇新的文章

#### Front-Matter
每个markdown文件应该提供`Front-Matter`，以`---`开头，以`---`结尾，一般常用的有：
* title: 文章标题
* date: 2017/04/28 22:30
* tags: 标签
* categories: 分类

更多参数请直接参考[HEXO官网](https://hexo.io/zh-cn/docs/writing.html)

#### 提示
现在写的文章可能是整个文章都展示在外面了，但我们只想展示摘要该怎么办呢？其实只要加入一个<!-- more -->这样的占位符在文章正文里面即可:-D

### Gitpages
先在博客创建好了，文章也写好了，但是要怎么发布呢？Github为我们提供了免费的，无限流量的GitHub Pages服务，是不是想想都激动呢O(∩_∩)O！

#### xxxx.github.io
我们首先先以我们的用户名在Github上创建一个`用户名.github.io`的项目，然后我们修改`_config.yml`文件：
```yml
deploy:
  type: git
  repository: https://github.com/ztktct/ztktct.github.io.git
  branch: master
```
这里的`ztktct`是我在github上的用户名，实际请改成自己的用户名！

#### 部署
我们已经配置好了所有的相关配置，但是现在还不能部署到GitHub Pages上，我们还需要装一个依赖`hexo-deployer-git`:
```bash
$ npm install hexo-deployer-git --save
```
之后我们执行以下命令即可部署到`GitHub Pages`上了:
```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```
要是觉得要执行三个命令过于麻烦，我们可以修改`package.json`文件，添加以下代码：
```JSON
"scripts": {
  "deploy": "hexo clean && hexo generate && hexo deploy"
}
```
之后只要执行`npm run deploy`即可部署啦！！

### 结束语
一个基于HEXO的博客基本上就搭建好了，浏览器打开 http://你的用户名.github.io 就可以访问你的博客了，共勉！

### 相关资料
* [HEXO](https://hexo.io/zh-cn/)
* [NexT](http://theme-next.iissnan.com/)
* [Github Pages](https://pages.github.com/)
* [HEXO搭建个人博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
