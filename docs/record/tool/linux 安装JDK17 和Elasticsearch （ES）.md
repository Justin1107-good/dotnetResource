linux 安装JDK17 和Elasticsearch （ES）

### 文章目录

- - [1、安装JDK ]
  - [2、安装es数据库]
  - - [2.1启动报错处理]
  - [3、安装kibana]
  - [4、ES设置开机自启动]
  - [5、es设置用户密码]

检查Linux 是否有jdk (它有，我想要装个17如何做)

```
[linux@localhost ~]$ java -version
openjdk version "1.8.0_312"
OpenJDK Runtime Environment (build 1.8.0_312-b07)
OpenJDK 64-Bit Server VM (build 25.312-b07, mixed mode)
[linux@localhost ~]$ which java
/usr/bin/java
```

下载JDK17 到 Downloads 目录

```
[linux@localhost ~]$ cd /home/linux/Downloads
[linux@localhost Downloads]$ wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz
--等待安装完成
```

解压压缩包

将压缩包上传到 /usr/bin/java 下

解压：

```
tar -zxvf jdk-17_linux-x64_bin.tar.gz -C /usr/bin/java/
```

3、设置环境变量

进入/etc/profile文件下

```bash
vi /etc/profile
```

在末尾添加

export JAVA_HOME=/usr/bin/java/jdk1.8.0_11
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH 

使环境变量生效

```bash
source /etc/profile
1
```

检查

```bash
java -version
```

## 安装es数据库