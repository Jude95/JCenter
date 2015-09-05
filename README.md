# 库上传JCenter教程

1. 去[Bintray](https://bintray.com/)注册  
一般使用github登录就好，很方便，最好翻墙上，不然卡死你。  
然后记录下2个值：  
name(头像旁的)   
key  
![APIKey](http://cdn.saymagic.cn/o_19e91jjrp3iu5mo1p631qjvff9.gif)


2. 在Project的gradle里加上  
`classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'`  
`classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'`  
并且给仓库加上`mavenCentral()`  
就像  

        buildscript {
            repositories {
                mavenCentral()
                jcenter()
            }
            dependencies {
                classpath 'com.android.tools.build:gradle:1.2.3'
                classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
                classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
            }
        }
        allprojects {
            repositories {
                mavenCentral()
                jcenter()
            }
        }
如果报错请先把gradle版本升到2.4以上。  
For Future:如果还有报错。保证你的gradle与这3个插件全都是最新。

3. 在要上传的moudel里的gradle里最外层加上

          ext {
              bintrayRepo = 'maven'////bintray上的仓库名，一般为maven
              bintrayName = 'requestvolley'//bintray上的项目名
          
              publishedGroupId = 'com.jude'//JCenter的GroupId
              artifact = 'requestvolley'//JCenter的ArtifactId
          
              siteUrl = 'https://github.com/Jude95/RequestVolley'
              gitUrl = 'https://github.com/Jude95/RequestVolley'
          
              libraryVersion = '1.0.4'//版本号
              libraryName = 'requestvolley'//项目名字，没什么用
              libraryDescription = 'A tool for Android'//项目描述，没什么用
          
              //开发者信息
              developerId = 'jude95'
              developerName = 'jude95'
              developerEmail = 'jude@helloworld.moe'
              
              //以上所有信息自行修改，以下不变
              
              licenseName = 'The Apache Software License, Version 2.0'
              licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
              allLicenses = ["Apache-2.0"]
          }
          apply from:'https://raw.githubusercontent.com/Jude95/JCenter/master/install.gradle'
          apply from:'https://raw.githubusercontent.com/Jude95/JCenter/master/bintray.gradle'

4. 在local.properties里加上(第一步记下的)：  
`bintray.apikey=********************`  
`bintray.user=****`  

5. 打开控制台，输入`gradle bintrayupload`然后坐等SUCCESS。  
如果找不到gralde命令，确定你把gradle加入了你的环境变量。
有时候注释里的一些特殊字符会造成编译失败。提示哪句不对就改一下那部分注释吧。  

6. 成功过后到[Bintray](https://bintray.com/)找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核通过(= = 美国时间上班了)。然后你会收到一条通知。  
然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。以后有更新直接重复第5部即可，会自动同步到jcenter仓库。

###这些坑不要再跳了  
1.上面你设置了artifact也并没有什么卵用。你的meaven仓库里显示是artifact，但Jcenter还是一定会**用你moudel的名字作为artifactId**的。  
具体原因参考[如何使用Android Studio把自己的Android library分发到jCenter和Maven Central](http://www.devtf.cn/?p=760)。写的很全面详细，然而他没有解决第3条的问题。  

2.如果依赖不上可以去[http://jcenter.bintray.com](http://jcenter.bintray.com)找到你的group目录看看你到底上传上去没有。  
比如：`com.jude:easyrecyclerview:1.0.2`就是http://jcenter.bintray.com/com/jude/easyrecyclerview   
因为，有时候你上次到了你的meaven仓库，而jcenter并没有同步过去。这种情况很少见，重新上传个新版本即可解决。  

3.**网上常见的上传JCenter的gradle配置无法解决你的库依赖其他库的问题**。务必用我上面的配置。  
此条详情参考[Android 项目打包到 JCenter 的坑](http://www.jianshu.com/p/c721f9297b2f?utm_campaign=hugo&utm_medium=reader_share&utm_content=note),写的很好，然而他的代码里手抖了打错了代码。  
我修复了他的问题并结合第1条的代码做了这个库。愿各位不会在jcenter上传的问题上再被虐出翔。
