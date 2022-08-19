# xshell工具使用以及.net core项目发布

**目的：**将本地软件发布到linux服务器，或者传输本地文件到linux

#### 一、XShell简介

`XShell`是一个强大的安全终端模拟软件，它支持SSH1,SSH2，以及Microsoft Windows平台的TELNET协议。
`XShell`可以在Windows界面下用来访问远端不同系统下的服务器，从而比较好的达到远程控制终端的目的。

由于在虚拟机中操作`Linux`系统需要频繁切换鼠标，缺乏个性化设置，不支持中文显示，所以我们将使用`XShell`来连接并使用安装好Linux系统的虚拟机。

以上介绍来源简书作者：若兮缘
链接：https://www.jianshu.com/p/4716cc35750f

#### 二、XShell安装

这里因为是个人回顾学习，所以安装了学校学生免费使用版本

下载地址：[家庭/学校免费 - NetSarang Website (xshell.com)](https://www.xshell.com/zh/free-for-home-school/)

填写自己的邮箱下载![image-20220630093923074](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630093923074.png)

![image-20220630094019744](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630094019744.png)

安装过程不做讲解，只要注意一下安装位置即可

#### 1.远程连接 

官方动态演示：[XSHELL - NetSarang Website](https://www.xshell.com/zh/xshell/)

![image-20220630094521011](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630094521011.png)

2.0选择导航菜单的文件 --> 新建，输入名称和主机IP，协议默认SSH，端口默认22

3.0如果不知道主机IP可以登录虚拟机的Linux系统，输入命令`ifconfig`查看`(inet addr)`

![image-20220630094346408](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630094346408.png)

4.0或者直接查看NewWork

![image-20220630094257973](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630094257973.png)



5.0用户身份验证

![image-20220630094619803](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630094619803.png)

###### 1.1 传输文件

在`Linux`系统如果想要与本机传输文件，推荐安装与`XShell`配套的`Xftp`工具。好用

![image-20220630095257303](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630095257303.png)

## 三、.NET CORE 项目发布到linux

[[转载\]虚拟机上Linux部署.NET Core项目 - qishidz - 博客园 (cnblogs.com)](https://www.cnblogs.com/qishidz/p/14271213.html)

### 1. 发布.NET Core项目

发布项目前需要给linux安装运行环境，Core 3.1或者更高

#### 1.1 终端切换成root权限

输入：su

![image-20220630101324336](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630101324336.png)

#### 1.2先注册 Microsoft 密钥；注册产品存储库；安装必需的依赖项

```
#注册 Microsoft 密钥。注册产品存储库。安装必需的依赖项。
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
```

![image-20220630101446883](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630101446883.png)

#### 1.3安装.NET Core Runtime 可根据自己的情况选择

```
#安装 .NET Core 运行时
sudo yum -y install aspnetcore-runtime-3.1
```

![image-20220630104121169](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104121169.png)

![image-20220630104147940](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104147940.png)

#### 1.4安装.NET Core SDK

```
#安装.NET Core SDK
sudo yum -y install dotnet-sdk-3.1
```

![image-20220630104304381](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104304381.png)

![image-20220630104327517](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104327517.png)

#### 1.5查看Dotnet 版本信息

```
#查看Dotnet 版本信息
dotnet --version
#查看Dotnet 版本信息
dotnet --info
```

![image-20220630104418439](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104418439.png)

#### 1.6发布项目到文件夹

![img](https://img2020.cnblogs.com/blog/983950/202010/983950-20201023160717501-1622229656.png)

![image-20220630101043081](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630101043081.png)

#### 1.7使用XFTP传输文件到linux

推荐博客：[本地文件上传到Linux服务器的几种方法_世界更美好的技术博客_51CTO博客](https://blog.51cto.com/superw/1943250)

![image-20220630104646251](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104646251.png)

#### 1.8进入要发布的目录

```
cd /home/linux/limux/pulish
ls 
```

![image-20220630104735882](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104735882.png)

![image-20220630104919846](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630104919846.png)

#### 1.9发布dll

```
# dll文件根据实际需求更改 端口需求更改
dotnet ReleaseWindowAndLinux.dll --urls="http://*:8001" --environment=Production
```

![image-20220630105113479](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630105113479.png)

```
#访问启动的站点
curl http://localhost:8001/
#停止站点 Ctrl+c
```

#### 2.0发布结果

![image-20220630105319733](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630105319733.png)

 

#### 2.1 #停止站点 Ctrl+c

![image-20220630105553519](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220630105553519.png)



Windows 发布.net core项目到IIS

环境安装：[.NET 下载(Linux、macOS 和 Windows) (microsoft.com)](https://dotnet.microsoft.com/zh-cn/download/dotnet)



linux 

```
[linux@localhost ~]$ su
Password: 
[root@localhost linux]# cd /home/linux/limux
[root@localhost limux]# rmdir elasticsearch-8.3.3
rmdir: failed to remove 'elasticsearch-8.3.3': Directory not empty
[root@localhost limux]# rm -rf elasticsearch-8.3.3
[root@localhost limux]# cd /home
[root@localhost home]# rmdir justin
rmdir: failed to remove 'justin': Directory not empty
[root@localhost home]# rm -rf justin
[root@localhost home]# rm -rf justinwang
[root@localhost home]# 

```

