---
title: jenkins-pipeline记录
date: 2019-03-07 09:55:20
thumbnail: https://i.imgur.com/X3YCfRh.png #/images/Jenkins/actor.pngs
categories: 运维
tags: jenkins
---
## 前言
因为项目需求，现需学习持续集成相关技术，多次权衡以后，决定使用jenkins实现功能。关于jenkins的介绍我就不多说了，能看到这篇博客的人都应该有所了解，就算没听过相信各位对搜索引擎的使用也是非常熟练。

## 项目结构
这是一个模拟Maven多模块的项目，暂时就只有一个子模块，不用纠结是否为真正的‘多’模块。

&emsp;&emsp;back_end    (父级)
&emsp;&emsp;&emsp;&emsp;一pom.xml
&emsp;&emsp;&emsp;&emsp;一back_end_web  (Web层)
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;一pom.xml
<!-- more -->

此时项目内已经有了几个接口以供测试，因重点不在这上面，这部分就结束了。

## 创建jenkins pipelin项目
假定你已经安装了jenkins，也将Maven代码上传到Git进行管理，接下来我们就要利用jenkins来创建一个从Git版本库来下载代码并自动打包、部署测试的
<font color=#008000 size=4>创建项目</font>
点击主页面右上角的**New 任务**来进入创建界面，然后输入你的**项目名**，选择**流水线**项目，点击**OK**来完成创建。
<font color=#008000 size=4>配置项目</font>
![](https://i.imgur.com/ZgTFMD7.png)
<!-- /images/Jenkins/1.png-->
直接点击最上方的Pipeline进行流水线的设置，其它的配置暂时先不管。
1.直接点解**Pipeline**进入流水线的设置，其它的可以先不管;
2.点击下拉框选择**Pipeline script from SCM**;
3.点击下拉框选择**Git**，因为我使用的是Git来管理项目代码，可以视情况选择;
4.设置Git版本库的路径，为了方便测试我设置为本地仓库的绝对路径，也就是Git clone后存储位置的绝对路径，和填网络版本库地址的效果是一样的，只是可以不做push这个操作，节省时间。**注：如果是用容器启动的jenkins，一定要将代码所在的目录在jenkins容器内做个映射，这样jenkins才能取得你的代码，待会儿我给的参考链接里有详细教程**；
5.设置要操作的分支，默认是master分支，视情况选择；
6.设置流水线脚本的路径，填写在你项目里的相对路径。
最后点击保存以后，就完成了简单的项目配置。

## 编辑流水线脚本和自动化构建脚本
<font color=#008000 size=4>编辑pipelin的运行脚本</font>
首先，我们先编辑Pipeline的脚本文件，名字和路径一定要和上面第六步对应，脚本内容如下。**请注意，以下注释只是我为了方便理解，请不要在需要运行的脚本内这样写。**
```
pipeline {
    agent any
    stages {
        stage('Build Tests') {
            agent {
                docker {
                    image 'maven:3.6-alpine'
                    args '-v /work/jenkins/root/.m2:/root/.m2 --link mysql:mysql' #这里是启动容器时的参数，第二个link是因为连接数据库碰到了一个bug，稍后再解释
                }
            }
            steps {
            sh './jenkins/scripts/tests.sh' #执行打包和测试脚本
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deploy.sh' #执行部署脚本
            }
        }
    }
}
```
**-v /work/jenkins/root/.m2:/root/.m2：**为了让Maven仓库得到保存，方便下一次构建。
**--link mysql:mysql：**将name为**mysql**的容器链接起来，并给予别名为**mysql**，为什么要这样做？
mysql和java服务运行在同一个物理机不同的两个容器内，因此通信的方式可以通过上述link的链接方式，以及访问物理机IP地址的方式来进行连接。我首先使用的是用IP地址的方式来访问，结果在启动时连接MySQL数据库，日志打印出了这样的一个错误：<font color=#FFA500 size=4>Communications link failure</font>。这个问题我在网络上搜索了很久，有3种解决方案，数据库连接URL增加参数、修改MySQL数据库的等待时间、修改URL请求的地址为localhost。很明显，以上3种方案都不适合我，我在其它物理机上是可以正常连接的，而且我们碰到问题时机器的环境不同。最终我只能改用docker运行时创建别名的方式，连接地址直接填写MySQL数据库的的别名，其实我不是很喜欢这种方式，增加耦合度，不便部署。如果有碰到这种情况的，也可以将你的解决方式在下面回复我，我还在寻找产生这样问题的原因。
<font color=#008000 size=4>编辑tests.sh</font>
现在要编辑执行打包、测试功能的脚本了。
```
#!/usr/bin/env bash
#Just a Tests shell script
#2019/3/4    kun    First release

#日志文件路径
LOGFILE="./start.log"
#Springboot项目启动成功时日志会有'Server Started'输出，以此来检测是否成功启动
START='Server Started'

#执行打包操作
mvn -B -DskipTests package

#后台启动项目，并且将日志输入到指定文件，以检查是否成功启动
java -jar back_end_web/target/*.jar >"${LOGFILE}" &
sleep 30    #等待30秒，项目启动比较费时
cat "${LOGFILE}"    #将日志输出，方便我在Jenkins控制台查看结果
#判断是否正常启动，否则退出本次脚本
grep "${START}" ${LOGFILE} && echo "${START}" || exit 1
#将jar包移动到指定的工作目录，方便我在接下来的deploy脚本内进行构建docker容器操作
cp back_end_web/target/*.jar /home/git/back_end/
```
<font color=#008000 size=4>编辑deploy.sh</font>
```
#!/usr/bin/env bash
#Just a Deploy shell script
#2019/3/4    Kun    First release

cd /home/git/back_end/  #进入工作目录
#取得jar包的名称
JARNAME="$(ls ./*.jar | awk 'NR==1{print $1}')"

#取出版本号，稍后image将以jar版本号作为标签，区分每个版本的image
VERSION=${JARNAME#*-} && VERSION=${VERSION%-*}
IMAGENAME="pipixia:${VERSION}"

#构建docker镜像，构建完成后删除jar包
docker build -t ${IMAGENAME} . && rm ${JARNAME}

#检查是否已经有名为'pipixia'的容器在运行中，如果有就将其干掉
docker ps -a | grep 'pipixia' && docker stop pipixia && docker rm pipixia

#启动刚刚构建完成的容器，link的原因我已经说过了，就不再赘述了，而卷映射只是我的需求，各位可以试实际情况来操作
docker run -d -p 8080:8080 -v /work/pipixia/upload:/upload --link mysql:mysql --name pipixia ${IMAGENAME}
```
<font color=#008000 size=4>编辑Dockerfile</font>
```
FROM openjdk:8-jdk-alpine
MAINTAINER "kun@kungege.cn"

#设置时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

#将打包好的脚本复制到指定目录
COPY ./*.jar /jar/pipixia.jar

#启动命令
CMD ["java", "-jar", "/jar/pipixia.jar"]
```
<font color=#008000 size=4>测试运行</font>
接下来就到了检验结果的时候了，手动点击**立即构建**，测试一下能否正常跑起来，成功后的结果应该如图，碰到什么问题可以在下面留言。
![](https://i.imgur.com/Ta450pQ.png)
<!-- /images/Jenkins/2.png-->

## 自动部署
通过了上面的步骤，此时我们的pipeline已经可以正常工作了，但是不可能每次都手动去构建，这时候只需要在jenkins的项目内设置一下。首先在创建pipeline项目的第4步，将版本库的地址更改为网络地址，将分支更改为需要自动部署的分支。然后设置轮询SCM的方案，每10分钟自动检查一次版本库是否更新，因为我使用的是GitBlit，不支持推送后发送提醒到Jenkins(也许是我没找到相关文档)。GitHub和GitLab用户可以使用更智能的方法，更新后给Jenkins主动发送信号。
![](https://i.imgur.com/MsxFnZo.png)
<!-- /images/Jenkins/3.png-->
完成了以上操作，再随意将项目更新一点东西，并提交测试，等待10分钟内，就会发现Jenkins自动完成了一次构建，这时候算是完成了一个简单持续集成项目。

## 结尾&参考链接
时隔3个多月，又一次写了博客，记录一下工作时使用的技术，免得过段时间又忘记了。一天天的，有时候都不知道自己究竟在做什么啊，唉，人生啊！
纵观全文，一些解决问题的方式可能太过低级，需要更优雅的方式，大家路过了还希望能够留下一些自己的意见，谢谢！
<font color=#008000 size=4>参考链接</font>
1.[使用Maven构建Java程序](https://jenkins.io/zh/doc/tutorials/build-a-java-app-with-maven/)：来自Jenkins官网，如果你去看过，就会发现我上面步骤基本是照着这个教程修改。
2.[Jenkins流水线](https://jenkins.io/zh/doc/book/pipeline/)：了解pipeline相关的概念以及语法。
3.[Jenkins语法-多个stage关系:顺序和并行](https://blog.csdn.net/u011541946/article/details/83627600)和[pipeline使用之语法详解](https://www.cnblogs.com/YatHo/p/7856556.html)：我从中得到了什么？这都忘了...我是不是不太适合干这一行了？？
4.[使用Jenkins来构建Docker容器](https://www.cnblogs.com/Leo_wl/p/4314792.html)：参考了一些想法。