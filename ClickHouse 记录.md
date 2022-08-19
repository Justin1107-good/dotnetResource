# ClickHouse 记录

[官网这里哦]: https://clickhouse.com/docs



## ClickHouse的安装

linux  环境：CentOS 8 x64

权限：root

打开命令输入：切换至root

在本地下载 ClickHouse 的最简单方法是运行以下命令。

```linux
curl https://clickhouse.com/ | sh
```

运行该命令，该命令定义了有用的符号链接的集合以及 ClickHouse 使用的文件和文件夹 - 您可以在安装脚本的输出中看到所有这些内容：`install`

```
sudo ./clickhouse install
```

在安装脚本结束时，系统会提示您输入用户的密码。您可以随意输入密码，也可以选择将其留空：`default`

```
Creating log directory /var/log/clickhouse-server.
Creating data directory /var/lib/clickhouse.
Creating pid directory /var/run/clickhouse-server.
 chown -R clickhouse:clickhouse '/var/log/clickhouse-server'
 chown -R clickhouse:clickhouse '/var/run/clickhouse-server'
 chown  clickhouse:clickhouse '/var/lib/clickhouse'
Enter password for default user:
```

您应看到以下输出：

```
ClickHouse has been successfully installed.

Start clickhouse-server with:
 sudo clickhouse start

Start clickhouse-client with:
 clickhouse-client
```

运行以下命令以启动 ClickHouse 服务器：

```
sudo clickhouse start
```

## 连接到 ClickHouse

默认情况下，ClickHouse 服务器侦听端口 8123 上的 HTTP 客户端。有一个内置的UI用于在 http://127.0.0.1:8123/play 运行SQL查询（相应地更改主机名）。

![image-20220804160544280](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804160544280.png)

请注意，在 Play UI 中，用户名已使用**默认值**填充，密码文本字段留空。如果为用户分配了密码，请在密码字段中输入密码。`default`

尝试运行查询。例如，以下命令返回预定义数据库的名称：

```
SHOW databases
```

单击“**Run**”按钮，响应将显示在“播放”UI 的下半部分：

![image-20220804160754474](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804160754474.png)

## 创建表

与大多数数据库管理系统一样，ClickHouse 在逻辑上将表分组到**数据库中**。使用以下命令在 ClickHouse 中创建新数据库：`CREATE DATABASE`

```
CREATE DATABASE IF NOT EXISTS tb_220804
```

 ![image-20220804161100966](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804161100966.png)

ClickHouse 中最简单的表也必须指定**表引擎**。引擎确定有关表的详细信息，例如：

- 数据的存储方式和位置，
- 支持哪些查询，以及
- 是否复制数据。

有许多引擎可供选择，但对于单节点 ClickHouse 服务器上的简单表，[MergeTree](https://clickhouse.com/docs/en/engines/table-engines/mergetree-family/mergetree) 可能是您的选择。运行以下命令以创建数据库中命名的表：`my_first_table``tb_220804`

```sql
CREATE TABLE tb_220804.my_first_table
(
    user_id UInt32,
    message String,
    timestamp DateTime,
    metric Float32
)
ENGINE = MergeTree()
PRIMARY KEY (user_id, timestamp)
```

![image-20220804161126142](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804161126142.png)

## 插入数据

ClickHouse的最佳做法是每批插入大量行 - 一次插入数万甚至数百万行

一个简单的示例，让我们一次插入多行

```
INSERT INTO tb_220804.my_first_table (user_id, message, timestamp, metric) VALUES
    (101, 'Hello, ClickHouse!',                                 now(),       -1.0    ),
    (102, 'Insert a lot of rows per batch',                     yesterday(), 1.41421 ),
    (102, 'Sort your data based on your commonly-used queries', today(),     2.718   ),
    (101, 'Granules are the smallest chunks of data read',      now() + 5,   3.14159 )
```

查看插入结果

```
SELECT * FROM tb_220804.my_first_table
```

![image-20220804161418386](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804161418386.png)

其他操作看这里

[官网介绍]: https://clickhouse.com/docs/en/quick-start



# Grafana 插件

[官网介绍]: https://clickhouse.com/docs/en/connect-a-ui/grafana-and-clickhouse

## Grafana 安装

linux  环境：CentOS 8 x64

## grafana简介

 Grafana允许您查询，可视化，警报和了解您的指标，无论它们存储在何处。创建、浏览仪表板并与团队共享仪表板，并培养数据驱动的文化：

- **可视 化：**具有多种选项的快速灵活的客户端图形。面板插件提供了许多不同的方法来可视化指标和日志。
- **动态仪表板：**使用模板变量创建动态和可重用的仪表板，这些模板变量在仪表板顶部显示为下拉列表。
- **探索指标：**通过即席查询和动态深入分析来探索数据。拆分视图并并排比较不同的时间范围、查询和数据源。
- **浏览日志：**体验使用保留的标签筛选器从指标切换到日志的魔力。快速搜索所有日志或实时流式传输。
- **提醒：**直观地为最重要的指标定义警报规则。Grafana将持续评估并向Slack，PagerDuty，VictorOps，OpsGenie等系统发送通知。
- **混合数据源：**在同一图表中混合不同的数据源！可以基于每个查询指定数据源。这甚至适用于自定义数据源。

安装

root或者本机权限输入一下命令，安装grafana-enterprise-8.4.4-1.x86_64.rpm的时候要求root安装

```
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-8.4.4-1.x86_64.rpm
sudo yum install grafana-enterprise-8.4.4-1.x86_64.rpm
```

其他看这里 

[官网介绍]: https://grafana.com/grafana/download?platform=linux

启动

```
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server
```

开始访问

启动完成后，默认端口是3000，linux内访问localhost即可，同网络下其他人访问localhost修改为IP+3000。

默认账号和密码是：admin(第一次登陆他会要求你改密码)

![image-20220804162656065](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804162656065.png)

![image-20220804162734933](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804162734933.png)

我了解到General和clickhouse是好伙伴

## 为 ClickHouse 安装 Grafana 插件

在Grafana可以与ClickHouse交谈之前，您需要安装相应的Grafana插件。假设您已登录到 Grafana（这是前提），请按照下列步骤操作：

1. 从**“配置”**页中，选择“**插件**”选项卡。
2. 搜索**ClickHouse**，然后单击Grafana Labs的**签名**插件：

![image-20220804163441306](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804163441306.png)

在下一个屏幕上，单击“**安装**”按钮

![image-20220804163524064](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804163524064.png)

## 定义 ClickHouse 数据源

安装完成后，单击“**创建 ClickHouse 数据源**”按钮。（也可以从**“配置”**页上的“数据源”选项卡添加**数据源**。

![image-20220804165447474](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804165447474.png)

![添加数据源](https://clickhouse.com/docs/assets/images/grafana_04-8084541dcc06398a53298ad31f104b4a.png)

1. 输入您的服务器设置和凭据。关键设置包括：

- **名称：**Grafana 设置 - 为数据源指定您喜欢的任何名称
- **服务器地址：**您的 ClickHouse 服务的 URL
- **服务器端口：**9000 表示不安全，9440 表示安全（除非您修改了 ClickHouse 端口）
- **用户名和密码****：输入**您的 ClickHouse 用户凭据。如果尚未配置用户和密码，请尝试**默认**用户名，并将密码留空。
- **默认数据库：**Grafana 设置 - 可以指定 Grafana 在使用此数据源时默认使用的数据库（此属性可以留空）

1. 单击“**保存并测试**”按钮以验证Grafana是否可以连接到您的ClickHouse服务。如果成功，您将看到**“数据源正在工作”**消息：

   ![选择“保存并测试”](https://clickhouse.com/docs/assets/images/grafana_05-e1d5bffa00f4461e8f618efc658a95fe.png)

## 构建仪表板

1. 在左侧菜单中，单击**“仪表板”图标，**然后选择“**浏览**”。然后选择“**新建仪表板”**按钮：

![image-20220804170033059](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804170033059.png)

1. 单击**添加新面板**按钮。

   ![image-20220804170119771](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804170119771.png)

在这里，您可以基于查询构建可视化效果。从“**数据源**”下拉列表中，选择您之前定义的 ClickHouse 数据源。然后，您可以使用**查询生成器**以可视方式生成查询，也可以切换到 **SQL 编辑器**并输入 SQL 查询（如下所示）：

![image-20220804170700927](C:\Users\Justin\AppData\Roaming\Typora\typora-user-images\image-20220804170700927.png)

其他的数据源 例如jSON API接口数据生成仪表，可以自己尝试

完结 就是这样！您现在已准备好在 Grafana 中[构建可视化效果](https://grafana.com/docs/grafana/latest/visualizations/)和[仪表板](https://grafana.com/docs/grafana/latest/dashboards/)。

这块需要的小伙伴可查这里 有仪表盘的构建 可详细学习

[官网介绍]: https://clickhouse.com/docs/en/connect-a-ui/grafana-and-clickhouse

