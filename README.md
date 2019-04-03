###  库上传JCenter教程

1. 去[Bintray](https://bintray.com/)注册 


  然后保存自己的用户名以及API Key,如下图


![APIKey](http://cdn.saymagic.cn/o_19e91jjrp3iu5mo1p631qjvff9.gif)


2. 新建仓库

如下图。新建一个仓库，记得仓库类型设置为maven并为其取一个名字
![](https://raw.githubusercontent.com/CB2Git/JCenter/v2/image/微信截图_20190403134352.png)

![](https://raw.githubusercontent.com/CB2Git/JCenter/v2/image/微信截图_20190403134631.png)


3. 在Project的gradle里加上  

```
buildscript {
    repositories {
        mavenCentral()
        google()
        jcenter()
        
    }
    dependencies {
    	//gradle版本要与android-maven-gradle-plugin匹配
        classpath 'com.android.tools.build:gradle:3.3.2'

        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.4'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
    }
}
```
如果报错请到 [https://github.com/dcendents/android-maven-gradle-plugin](https://github.com/dcendents/android-maven-gradle-plugin)查看gradle对应的android-maven-gradle-plugin插件版本


​    

4. 在要上传的moudel里的gradle里最外层加上

          ext {
              package_userOrg = 'jinuo' //  组织名或者用户名 不填默认用户名
              package_repo = 'maven' // bintray上的仓库名，就是我们在第二步中创建的仓库名字，如果package_userOrg设置的为组织名，那么这个仓库名得属于组织的
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

5. 在local.properties里加上(第一步记下的)：  
    `bintray.apikey=********************`  
    `bintray.user=****`  

6. 打开控制台，输入`gradle bintrayupload`然后坐等SUCCESS。  
    ![terminal](https://raw.githubusercontent.com/Jude95/JCenter/master/image/terminal.png)
如果找不到gralde命令，确定你把gradle加入了你的环境变量
或者尝试使用`gradlew bintrayupload`
有时候注释里的一些特殊字符会造成编译失败。提示哪句不对就改一下那部分注释吧。  

7. 成功过后到[Bintray](https://bintray.com/)找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核通过(= = 美国时间上班了)。然后你会收到一条通知。  
    然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。以后有更新直接重复第5部即可，会自动同步到jcenter仓库。

如果上传后却依赖不了可以去[http://jcenter.bintray.com](http://jcenter.bintray.com)找到你的group目录看看你到底上传上去没有。  
比如：`com.jude:easyrecyclerview:1.0.2`就是http://jcenter.bintray.com/com/jude/easyrecyclerview   
因为，有时候你上次到了你的meaven仓库，而jcenter并没有同步过去。这种情况很少见，重新上传个新版本即可解决。  



异常情况处理:

+ 现象
`Could not get unknown property 'package_version' for project ':mylibrary' of type org.gradle.api.Project.`

解决方案

将`from:'https://raw.githubusercontent.com/Jude95/JCenter/v2/bintray.gradle'`放置到gradle脚本最后一行

+ 现象

gradle 不是内部或外部命令，也不是可运行的程序或批处理文件。

解决方案

查看项目根目录,如果存在gradlew或gradlew.bat，执行`gradlew bintrayupload`


+ 现象

 Could not publish '********': HTTP/1.1 404 Not Found [message:Repo 'maven' was not found]

解决方案

在你的账户下面没有name为maven的仓库，在bintray中新建仓库，仓库类型为maven，仓库名为`package_repo = 'maven'`中配置的仓库名
如下图

![](image\微信截图_20190403134352.png)

![](image\微信截图_20190403134631.png)