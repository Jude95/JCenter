# JCenter
上传Jcenter的gradle配置

首先在项目的gradle里加上  
`classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'`  
`classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'`  
并且给仓库加上`mavenCentral()`
就像：

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

然后在你的moudel的gradle里加上

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

然后到local.properties里加上：
bintray.apikey=********************
bintray.user=****
