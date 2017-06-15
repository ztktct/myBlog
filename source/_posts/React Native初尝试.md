---
title: React Native初尝试
date: 2017/06/13 23:10
tags: [react, javascript, React Native]
categories: [javascript, react]
---

上一篇{% post_link 'Ubuntu下搭建React Native环境' 'Ubuntu下搭建React Native环境' %}搭建好了开发环境后，便迫不及待的想来尝试下React Native了，这篇就来记录下一款APP的开发步骤吧，算是供自己和后来人做参考吧！项目已上传到[github](https://github.com/ztktct/react-native-jdly)
<!-- more -->

# 开发首个React Native APP
先来张效果图吧，这是利用React Native制作的一款小应用，功能不多，算是给个基于React Native开发APP思路和过程吧：
<div align=center>![JDLY](/images/React Native初尝试/view.gif)</div>
一款绅士小应用:D,感谢[J站](http://www.jdlingyu.moe/)提供图片资源。

## 项目搭建
假定我们已经搭建好了React Native所需的环境(可以参考:{% post_link 'Ubuntu下搭建React Native环境' 'Ubuntu下搭建React Native环境' %}),我们先把项目搭起来：
```bash
$ react-native init JDLY
```
再启动Genymotion模拟器，然后启动项目:
```bash
# 可能需要先执行这条命令：
$ react-native start
$ react-native run-android
```
首次启动可能会出现以下错误:
![项目首次启动失败](/images/React Native初尝试/2017-06-15 22-38-23屏幕截图.png)
其实很简单，打开Android Studo,打开项目下的`android`文件夹，Android Studo会自动帮你处理一些依赖啥的，基本能解决大部分问题:D，再次启动项目，你会发现，App已经成功安装了，你可以启动它，会得到如下一张图：
<div align='center'>![项目安装成功](/images/React Native初尝试/2017-06-15 22-49-40屏幕截图.png)</div>

## 项目组成
项目的具体编写过程写个大概就够了，就不详细讲述了，项目地址：[react-native-jdly](https://github.com/ztktct/react-native-jdly),欢迎Fork :D!

### 项目结构
下图是整个项目的结构，看上去可能很多，但其实大部分都是React Native自动帮我们生成的：
![项目结构](/images/React Native初尝试/2017-06-15 23-20-30屏幕截图.png)
* android: android源码相关文件
* ios: ios源码相关文件
* index.android.js: React Native项目Android平台入口文件
* app: 我们实际代码所在的文件

### 项目用到的依赖
这些都是项目中用到的一些依赖，不过项目可能并没有用到他们的全部功能，仅供参考，实际请查阅官方文档，介绍的更为全面！
* `native-base`： 一个Material Design风格的组件库，用法也很简单，这是他的官网，文档介绍的很详细：[native-base](https://docs.nativebase.io)
* `react-native-image-zoom-viewer`: 一个幻灯片?组件，项目地址:[react-native-image-zoom-viewer](https://github.com/Anthonyzou/react-native-image-zoom)
* `react-native-vector-icons`： 一个图标库，github地址：[react-native link](https://github.com/oblador/react-native-vector-icons),使用时需要注意以下步骤(针对Android)：
    1. 安装依赖：npm install react-native-vector-icons --save
    2. 编辑`android/app/build.gradle`:
    ```gradle
    apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
    ```
    3. 编辑`android/settings.gradle`,添加以下代码:
    ```gradle
    include ':react-native-vector-icons'
    project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
    ```
    4. 编辑`android/app/build.gradle`:
    ```gradle
    dependencies {
        ...
        compile project(':react-native-vector-icons')
    }
    ```
    5. 编辑`MainApplication.java`文件，在`android/app/src/main/java/...`深层目录下
    ```java
    import com.oblador.vectoricons.VectorIconsPackage;
    ...
    @Override
    protected List<ReactPackage> getPackages() {
        return Arrays.<ReactPackage>asList(
        new MainReactPackage(),
        new VectorIconsPackage()
        );
    }
    ```
    6. 执行`react-native link`链接原生库
* `react-navigation`: 一个导航库，Facebook官方推荐的一个库，应该是不错的，这是官方文档:[react-navigation](https://reactnavigation.org/docs/intro/)
* `mobx` + `mobx-react`: 一个状态管理库，原谅我引用了进来，其实这个小应用可以不用任何状态管理库就能运行起来，但是mobx用起来还是挺舒服的，比Redux写起来方便，不过由于它使用了ES2017的Decorator特性，所以我们需要编辑项目根目录下的`.babelrc`文件，添加`"plugins": ["transform-decorators-legacy"]`,以启用Decorator特性，同时还需要安装`babel-plugin-transform-decorators-legacy`和`babel-preset-react-native-stage-0`两个依赖。mobx的文档可参考[官方文档](https://mobx.js.org/index.html),或者{% post_link '以前的博客' 'Mobx学习笔记' %}

## 项目打包
### 签名密钥
我们先需要生成一个签名密钥：
```bash
$ keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```
这条命令会要求你输入密钥库（keystore）和对应密钥的密码，然后设置一些发行相关的信息。最后它会生成一个叫做`my-release-key.keystore`的密钥库文件。
在运行上面这条语句之后，密钥库里应该已经生成了一个单独的密钥，有效期为10000天。--alias参数后面的别名是你将来为应用签名时所需要用到的，所以记得记录这个别名。
> 注意：请记得妥善地保管好你的密钥库文件，不要上传到版本库或者其它的地方。

### 设置gradle变量
把`my-release-key.keystore`文件放到项目中的`android/app`文件夹下。
编辑`~/.gradle/gradle.properties`（没有这个文件就创建一个），添加如下的代码（注意把其中的`****`替换为相应密码）
```
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```
### 添加签名到项目的gradle配置文件
编辑项目目录下的`android/app/build.gradle`:
```gradle
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP_RELEASE_STORE_FILE)
            storePassword MYAPP_RELEASE_STORE_PASSWORD
            keyAlias MYAPP_RELEASE_KEY_ALIAS
            keyPassword MYAPP_RELEASE_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```
### 生成发行APK包
```bash
$ cd android && ./gradlew assembleRelease
```
> 注：cd android表示进入android目录（如果你已经在android目录中了那就不用输入了）。./gradlew assembleRelease在macOS和Linux系统中表示执行当前目录下的名为gradlew的脚本文件，运行参数为assembleRelease，注意这个./不可省略；而在windows命令行下则需要去掉./。

生成的APK文件位于`android/app/build/outputs/apk/app-release.apk`，它已经可以用来发布了。
### 测试应用的发行版本
```bash
$ cd android && ./gradlew installRelease
```

## 更改APP图标
自带的图标太丑了？我们可以更换图标为自己想要的，打开`android\app\src\main\res`目录，有以下目录：
![](/images/React Native初尝试/2017-06-16 00-52-54屏幕截图.png)
前四个就是我们要更改的图标的目录了。
>注：values文件夹下的strings.xml文件可以更改APP名称哦！

## 结束语
这样，一个React Native App就开发好了，由于鄙人没有MAC，IOS还没开发过，/(ㄒoㄒ)/~~，一些经验也还不足，难免会出错，尽请指出与批判，与君共勉！

## 参考资料
* [react-native-jdly](https://github.com/ztktct/react-native-jdly)
* [native-base](https://docs.nativebase.io)
* [react-native-image-zoom-viewer](https://github.com/Anthonyzou/react-native-image-zoom)
* [react-native link](https://github.com/oblador/react-native-vector-icons)
* [react-navigation](https://reactnavigation.org/docs/intro/)
* [Mobx](https://mobx.js.org/index.html)
* [React Native中文网](http://reactnative.cn/docs/0.44/getting-started.html)
