# 库上传JCenter教程

1. 去[Bintray](https://bintray.com/)注册  
一般使用github登录就好，很方便，最好翻墙上，不然卡死你。  
然后记录下2个值：  
name(头像旁的)   
key  
![APIKey](http://cdn.saymagic.cn/o_19e91jjrp3iu5mo1p631qjvff9.gif)


2. 在Project的gradle里加上  
`classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'`  
`classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'`  
并且给仓库加上`mavenCentral()`  
就像  

        buildscript {
            repositories {
                mavenCentral()
                jcenter()
            }
            dependencies {
                classpath 'com.android.tools.build:gradle:2.2.0'
                classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7'
                classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
            }
        }
        allprojects {
            repositories {
                jcenter()
            }
        }
如果报错请先把gradle版本升到2.11以上。  
For Future:如果还有报错。保证你的gradle与这3个插件全都是最新。

3. 在要上传的moudel里的gradle里最外层加上

          ext {
              package_userOrg = 'jinuo' // 组织名 不填默认用户名
              package_repo = 'maven' // bintray上的仓库名，一般为maven
              package_type = 'aar'  // 输出类型
              package_group = 'com.example' // JCenter的GroupId
              package_artifact = 'demo' // JCenter的ArtifactId
              package_version = '1.0'  // JCenter的VersionId
              package_description = 'A tool for Android'

              siteUrl = 'https://github.com/'
              gitUrl = 'https://github.com/'

              //开发者信息
              developerId = ''
              developerName = ''
              developerEmail = ''

              //开源协议
              licenseName = 'The Apache Software License, Version 2.0'
              licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
              allLicenses = 'Apache-2.0'
          }

          apply from:'https://raw.githubusercontent.com/Jude95/JCenter/v2/bintray.gradle'

4. 在local.properties里加上(第一步记下的)：  
`bintray.apikey=********************`  
`bintray.user=****`  

5. 打开控制台，输入`gradle bintrayupload`然后坐等SUCCESS。  
![terminal](https://raw.githubusercontent.com/Jude95/JCenter/master/image/terminal.png)
如果找不到gralde命令，确定你把gradle加入了你的环境变量。
有时候注释里的一些特殊字符会造成编译失败。提示哪句不对就改一下那部分注释吧。  

6. 成功过后到[Bintray](https://bintray.com/)找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核通过(= = 美国时间上班了)。然后你会收到一条通知。  
然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。以后有更新直接重复第5部即可，会自动同步到jcenter仓库。

如果上传后却依赖不了可以去[http://jcenter.bintray.com](http://jcenter.bintray.com)找到你的group目录看看你到底上传上去没有。  
比如：`com.jude:easyrecyclerview:1.0.2`就是http://jcenter.bintray.com/com/jude/easyrecyclerview   
因为，有时候你上次到了你的meaven仓库，而jcenter并没有同步过去。这种情况很少见，重新上传个新版本即可解决。  
