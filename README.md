# JenkinsSample
Jekins+Gradle最佳实践

##### Jenkins是一款开源的持续集成工具,可用于工程自动化测试和打包。支持Web，iOS，Android，Monkey测试等。
#### [Jenkins官网](https://jenkins.io/)

本文是Jenkins+Gradle打包apk的最佳示例，鉴于网上的各种教程存在的问题和坑，所以记录一下搭建过程和使用时候存在的坑。

##### 由于gradle的相关配置比较复杂,可定制的内容也比较多,需要有一定经验的构建.

> 文中用到的gradle相关配置示例
> [gradle配置](https://github.com/Cicinnus0407/JenkinsSample)

##### 效果图
[![效果图](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152820.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152820.png)


##### 1.到官网下载Jenkins
> [点击跳转下载地址](https://jenkins.io/download/)

![Jenkins下载列表](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922155023.png)
**Note**:下载列表的最后一个是war包,可以直接部署到tomcat无需安装
> 本文基于Windows,Jenkins为安装版

##### 2.下载安装后可以在浏览器打开:http://localhost:8080/jenkins

> **如果在服务器环境下8080端口很可能是被web项目占用,可以通过修改Jenkins安装目录下的jenkins.xml文件,修改httpPort后的端口号**
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922155629.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922155629.png)
> 如果使用的是war包部署,端口号就和tomcat一样了

##### 3.打开jenkins后的第一个页面,根据提示到安装目录找到第一次登录的秘钥
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152337.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152337.png)

##### 4.安装默认插件
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152541.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922152541.png)

##### 5.等待安装结束后进入主界面,配置全局变量,Android SDK的目录
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922153145.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922153145.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922160953.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922160953.png)

##### 6.配置相关工具目录,如果没有可以选择自动下载,其中gradle版本建议和平时开发环境的版本一致,否则可能会导致编译失败

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922153514.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922153514.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922161606.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922161606.png)

##### 7.安装必备插件
需要安装的列表(注意搜索的时候大小写和空格):
- Date Parameter Plugin
- build-name-setter
- description setter plugin
- Environment Injector Plugin

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171304.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171304.png)


##### 8.新建一个构建项目,选择自由风格的项目
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922161834.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922161834.png)

##### 9.填写构建参数,这一步非常重要,编写的内容要配合Gradle
##### 填写构建参数,这一步非常重要,编写的内容要配合Gradle
##### 填写构建参数 ,这一步非常重要,编写的内容要配合Gradle

**先看一下gradle的输出apk脚本,主要内容就是判断是否由Jenkins构建,如果是那就根据对应参数生成apk到对应目录,否则正常输出到本地apk目录**
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922163358.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922163358.png)

**填写构建参数,注意Choice Parameter,String Parameter,Date Parameter ,如果发现没有Date Parameter请回到第七步安装好插件**

###### 9-1配置输出apk的渠道
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171842.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171842.png)
> **这里的choices是根据gradle的productFlavors而定,请注意第一个字母要大写,具体请参考示例gradle [示例gradle点击跳转](https://github.com/Cicinnus0407/JenkinsSample/blob/master/build.gradle)**
###### 9-2,配置apk输出名字
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171856.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170922171856.png)
> **这个参数是输出后的apk文件名组成,建议和构建平台一致,注意字母大小写**

###### 9-3配置apk的环境,是debug还是release

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923130611.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923130611.png)
> **其中空格是为了需要输出所有渠道apk时候需要的**

###### 9-4.配置是否由Jenkins构建,是和否的apk会生成到不同和目录和不同名字,具体配置还是参考示例gradle

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131210.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131210.png)

###### 9-5.配置App版本号
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131428.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131428.png)
###### 9-6配置apk输出文件名加上时间
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131652.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131652.png)

###### 9-7配置构建信息
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131923.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923131923.png)

###### 9-8配置Git信息
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923132459.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923132459.png)
###### 9-9配置输出命令
> clean assemble'${PRODUCT_FLAVOR_BUILD}''${ENVIRONMENT_TYPE}' 这段动态脚本相当于 clean assembleOfficalRelease 

> 所以应该明白为什么PRODUCT_FLAVOR_BUILD配置的第一个英文要大写了

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133032.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133032.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133046.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133046.png)


##### 10.选择好需要构建的参数,选择开始构建,每一次构建都会自动从git拉取最新的代码.第一次构建Jenkins需要下载相应的jar包,耗时会比较久(如果构建失败,请继续阅读接下步骤)
> 由于由于gradle打包也会引用jdk,所以在打包的时候会消耗比较大的资源,建议将Jenkins配置在服务器,这样在打包的时候就不会影响开发,同时也可以随时随地在其他地方进行打包,不需要依赖发开环境

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133528.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133528.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133539.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133539.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133656.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923133656.png)

###### 11.编译成功,根据gradle配置的输出路径找到apk
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923134857.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923134857.png)
[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923142840.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923142840.png)

---
##### 注意事项:

###### 由于是直接由Gradle进行打包,和AS的generated apk还是有区别的,所以我们还需要进行额外配置才能正常构建,

[![](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923134857.png)](http://cicinnus-blog.oss-cn-shenzhen.aliyuncs.com/2017/09/20170923134857.png)
> 在windows系统中，由于对文件路径有长度限制，256个字节，如果图片的路径长度超过了这个限制，就会报这个错误

解决办法:在gradle.properties指定cache路径
##### android.buildCacheDir=缓存路径
> [示例gradle.properties](https://github.com/Cicinnus0407/JenkinsSample)




