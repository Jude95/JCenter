# JCenter
上传Jcenter的gradle配置

1. 在project的gradle里加上  
`classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'`  
`classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'`  
并且给仓库加上`mavenCentral()`就像  

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
如果报错请先把gradle版本升到2.4以上

2. 在moudel的gradle里最外层加上

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

3. 在local.properties里加上，这个到bintray.com去注册账号就有：  
`bintray.apikey=********************`  
`bintray.user=****`  

4. 到bintray.com找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核。  
然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。

5. 这些坑不要再跳了  
上面你设置了artifact也并没有什么卵用。Jcenter还是一定会**用你moudel的名字作为artifactId**的。具体为什么欢迎补充。  
如果依赖不上可以去http://jcenter.bintray.com/找到你的group目录看看具体情况。  
比如：`com.jude:easyrecyclerview:1.0.2`就是`http://jcenter.bintray.com/com/jude/easyrecyclerview/`  
网上常见的gradle配置无法解决你的库依赖其他库的问题。务必用我上面的配置。
