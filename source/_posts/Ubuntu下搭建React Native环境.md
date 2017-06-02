---
title: Ubuntu下搭建React Native环境
date: 2017/05/22 00:16
tags: [react, javascript, React Native]
categories: [javascript, react]
---

一直对APP开发很感兴趣，可惜对JAVA和swift不熟悉，迟迟没有去搞一款像样的APP(明明是个没有Mac的穷屌丝，还想搞IOS开发？)。之前有用过Vue写过一款web App,用cordova封装的伪APP,属于小打小闹的那种，完全没有原生的好用啊！前端界有一句名言：任何可以用JavaScript编写的应用程序，最终都会用JavaScript编写。React Native就是一个可以用JS来编写出原生APP的框架。万事开头难，先来搭建React Native的开发环境吧！
<!-- more -->

## Node 安装
windows下我们可以直接从[Nodejs官网](https://nodejs.org/en/)下载安装包，双击安装；
linux下我们虽然也可以下载node源代码来安装，但推荐使用[nvm](https://github.com/creationix/nvm/blob/master/README.md)来安装node，切换版本非常方便！

## JAVA环境配置
React Native目前需要Android Studio2.0或更高版本，而Android Studio又需要JAVA环境，所以先来配置JAVA环境吧(如果已经安装了JAVA, 请跳过这一步，可通过`java -version`来查看当前java版本)！
1. 我们先去[JAVA官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载所需的JDK。
![JDK](/images/React Native环境搭建/2017-05-31 23-17-45屏幕截图.png)

2. 在 `/usr/local` 目录下新建 `java` 文件夹,并将下载的JDK复制到该文件夹下：
```bash
$ sudo mkdir /usr/local/java
$ sudo cp jdk-8u131-linux-x64.tar.gz /usr/local/java
```
3. 解压源码
```bash
$ sudo tar xvf jdk-8u131-linux-x64.tar.gz
```
4. 设置环境变量
    ```bash
    $ sudo gedit /etc/profile
    ```
    ```
    export JAVA_HOME=/usr/local/java/jdk1.8.0_25  # 配置jdk的主目录,此处根据JDK的路径来配置
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=${JAVA_HOME}/bin:$PATH
    ```
    配置完成后执行命令`source /etc/profile`, 然后在终端输入`java -version`查看是否安装完毕：
    ![JAVA 安装成功](/images/React Native环境搭建/2017-05-31 23-46-10屏幕截图.png)

## 安装 Android Studio
1. 下载[Android Studio](https://developer.android.com/studio/index.html)
2. 解压下载到的安装包，cd 到bin文件夹下，执行 `./studio.sh`命令，我们就能看到如下界面
   ![Android Studio](/images/React Native环境搭建/2017-05-31 23-55-58屏幕截图.png)
3. 先不急着安装，按照[React Native官网](https://facebook.github.io/react-native/)上来讲，我们需要在安装的时候做些改动(英文不好的可以去[React Native中文网](http://reactnative.cn/docs/0.44/getting-started.html)查看)
   ![Android Studio](/images/React Native环境搭建/2017-06-01 00-02-51屏幕截图.png)
   选择`Android Virtual Device`:
   ![Android Studio](/images/React Native环境搭建/2017-06-01 00-04-04屏幕截图.png)
   然后就是漫长的等待他下载完毕啦：
   ![Android Studio](/images/React Native环境搭建/2017-06-01 00-09-29屏幕截图.png)
4. 当下载完毕后，我们还需要下载一些SDK：
    > 在`SDK Platforms`窗口中，选择`Show Package Details`，然后在`Android 6.0 (Marshmallow)`中勾选`Google APIs`、`Android SDK Platform 23`、`Intel x86 Atom System Image`、`Intel x86 Atom_64 System Image`以及`Google APIs Intel x86 Atom_64 System Image`。
    在`SDK Tools`窗口中，选择`Show Package Details`，然后在`Android SDK Build Tools`中勾选`Android SDK Build-Tools 23.0.1`（必须是这个版本）。然后还要勾选最底部的`Android Support Repository`.
    
   ![Android Studio](/images/React Native环境搭建/2017-06-02 00-07-28屏幕截图.png)
   ![Android Studio](/images/React Native环境搭建/2017-06-02 00-11-16屏幕截图.png)
5. 当SDK下载好后，我们还需要配置ANDROID_HOME环境变量：
   确保ANDROID_HOME环境变量正确地指向了你安装的Android SDK的路径。
   具体的做法是把下面的命令加入到`~/.profile`文件中。如果你使用的是其他的shell，则选择对应的配置文件:
   ```bash
    # 如果你不是通过Android Studio安装的sdk，则其路径可能不同，请自行确定清楚。
    export ANDROID_HOME=${HOME}/Android/Sdk
   ```
    然后使用下列命令使其立即生效（否则重启后才生效）：
   ```bash
    source ~/.profile
    ```
    可以使用`echo $ANDROID_HOME`检查此变量是否已正确设置。
6. 将Android SDK的Tools目录添加到PATH变量中，以便在终端中运行一些Android工具，在`~/.profile`文件中添加:
    ```bash
    export PATH=${PATH}:${ANDROID_HOME}/tools
    export PATH=${PATH}:${ANDROID_HOME}/platform-tools
    ```

## 安装React Native命令行工具
```bash
$ npm install -g react-native-cli
```
由于一些大家都懂的原因，在国内下载npm包比较慢，还好我们勤劳的淘宝帮我们备份了npm源：
```bash
# 此条命令将npm源设置成国内淘宝提供的镜像，速度更快哦
$ npm config set registry https://registry.npm.taobao.org 
```
当然如果你使用yarn,可以这么设置:
```bash
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```
安装完后，我们就可以来尝试构建第一个项目了：
```bash
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```
如果你想修改后能实时看到结果，那么你应该先运行以下命令:
```bash
$ react-native start
```
如果一切顺利，你将会在机器上看到一下这张图：
![Android Studio](/images/React Native环境搭建/2017-06-03 00-13-12屏幕截图.png)


## 参考资料
 * [React Native中文网](http://reactnative.cn/docs/0.44/getting-started.html)
 * [React Native官网](https://facebook.github.io/react-native/)