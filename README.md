# Walle(Cover meituan:walle)

Walle（瓦力）：Android Signature V2 Scheme签名下的新一代渠道包打包神器

瓦力通过在Apk中的`APK Signature Block`区块添加自定义的渠道信息来生成渠道包，从而提高了渠道包生成效率，可以作为单机工具来使用，也可以部署在HTTP服务器上来实时处理渠道包Apk的升级网络请求。

## 修改者自述

鉴于美团对此仓库长期不更新, 各个依赖库版本都比较低, 使用了过时的API等问题。本人fork了Walle仓库, 进行了升级的操作。
本人秉承开源精神, 仅进行开放学习性的尝试, 并非将其重新包装据为己有的意思。大家理性看待此仓库，谢谢！

修改部分如下:
 - 1 support -> AndroidX
 - 2 修改了groupId(因为我无权使用walle原来的groupId), 发布到了jcenter
 - 3 升级了gradle各插件的版本
 - 3 升级了compileSdkVersion、targetSdkVersion等配置

## 使用

### 1 walle
先在根目录`build.gradle`的`buildscript`节点下的`dependencies`节点中, 添加下面这行代码:
```groovy
classpath "wang.relish.walle:walle-plugin:2.0.0"
```
像这样
```groovy
buildscript {
    repositories {
        mavenLocal()
        jcenter()
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.3'
        classpath "wang.relish.walle:walle-plugin:2.0.0" // add this line
    }
}
```
然后在`app`目录下的`build.gradle`中添加以下代码:
```groovy
apply plugin: 'walle'
implementation "wang.relish.walle:walle:2.0.0"
```
接着配置渠道参数:
```groovy
walle {
    apkOutputFolder = new File("${project.buildDir}/outputs/channels")
    apkFileNameFormat = '${appName}-${packageName}-${channel}-${buildType}-v${versionName}-${versionCode}-${buildTime}-${flavorName}.apk'
    //configFile与channelFile两者必须存在一个，否则无法生成渠道包。两者都存在时优先执行configFile
    channelFile = new File("${project.getProjectDir()}/channel")
    //configFile = new File("${project.getProjectDir()}/config.json")
}
```
记得在`app`目录下创建`channel`文件或`config.json`(根据自己实际需求选择)

### 2 cli
walle-cli我也重新打包了一份上传到了github release。
使用方法参看原仓库文档即可

## 本仓库所有构件
```groovy
classpath "wang.relish.walle:walle-plugin:2.0.0" // Android工程渠道打包插件(依赖walle-reader和walle-writer)
implementation "wang.relish.walle:walle:2.0.0" // Android平台用于读取渠道及渠道信息(依赖walle-reader)
implementation "wang.relish.walle:walle-writer:2.0.0" // 用于写入渠道及渠道信息(依赖walle-reader)
implementation "wang.relish.walle:walle-reader:2.0.0" // Java平台用于读取渠道及渠道信息
walle-cli.jar // 程序命令行, 用于给Apk文件写入渠道信息(依赖walle-reader和walle-writer)
```

**FAQ:**
为什么不用`com.meituan.android.walle`作为`GROUP_ID`？
因为meituan已经使用此id上传了构建到jcenter。其他人无法使用。故我只能修改`GROUP_ID`再进行上传。

**以下是原仓库文档, Enjoy it.**

[Meituan-Dianping/walle/README.md](./README_SOURCE.md)