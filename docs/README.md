# .Net 6 实现 EF Core 之CodeFirst模式

> ef 已.net平台的一种ORM框架，初次之外还有Dapper、FreeSql等
>
> IDE 选择：Visual Studio 2022 
>
> 客户端：Win10
>
> 数据库：MySql 版本 5.0

### 开始

新建ASP.NET CORE WEB API项目 Scheme.Core.API 

.NET Core 类库 Scheme.Core 

模型类库项目放实体类Scheme.Models

#### 项目结构

Scheme.Core.API 

Scheme.Core 

Scheme.Models

#### Nuget引用：

```
Microsoft.EntityFrameworkCore
Microsoft.EntityFrameworkCore.Design
Microsoft.EntityFrameworkCore.Tools
Pomelo.EntityFrameworkCore.MySql
```

### 配置

项目Scheme.Core 下新建上下文 SchemeContext 并继承 DbContext

```
namespace Scheme.Core
{
    public class SchemeContext : DbContext
    { 
        public SchemeContext(){}
        public SchemeContext(DbContextOptions options) : base(options){} 
        //数据库实体类
        public DbSet<Userinfo> Userinfos { get; set; } 
        //继续按照8行的写下去
    }
}

```

在appsettings.json 配置数据库连接字符串

```
"ConnectionStrings": {
    "DefaultConnection": "server=localhost;user id=root;password=123456;database=base_program;SslMode=none;Charset=utf8;"
  }
```



在Program下注册

```
//获取配置数据库连接字符串
var connectionString = builder.Configuration["ConnectionStrings:DefaultConnection"];
var serverVersion = new MySqlServerVersion(new Version(5, 6, 22));
// mysql ef core字符串
builder.Services.AddDbContext<SchemeContext>(options => options.UseMySql(connectionString, serverVersion));
```



### 迁移和更新

#### 相关命令

##### Add-Migration   install 

##### update-database  install

打开程序包管理器控制台

​      工具 - NuGet包管理器 - 程序包管理器控制台 



默认项目选择Scheme.Core

​     控制台输入：Add-Migration 

若输出一下结果及成功

```
PM> Add-Migration sku
Build started...
Build succeeded.
Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 6.0.7 initialized 'SchemeContext' using provider 'Pomelo.EntityFrameworkCore.MySql:6.0.2' with options: ServerVersion 5.6.22-mysql 
To undo this action, use Remove-Migration.
```

更新并创建数据库

update-database 

```
PM> update-database sku
Build started...
Build succeeded.
Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 6.0.7 initialized 'SchemeContext' using provider 'Pomelo.EntityFrameworkCore.MySql:6.0.2' with options: ServerVersion 5.6.22-mysql 
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (7ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT 1 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA='base_program' AND TABLE_NAME='__EFMigrationsHistory';
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (26ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `__EFMigrationsHistory` (
          `MigrationId` varchar(150) CHARACTER SET utf8mb4 NOT NULL,
          `ProductVersion` varchar(32) CHARACTER SET utf8mb4 NOT NULL,
          CONSTRAINT `PK___EFMigrationsHistory` PRIMARY KEY (`MigrationId`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (2ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT 1 FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA='base_program' AND TABLE_NAME='__EFMigrationsHistory';
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (2ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT `MigrationId`, `ProductVersion`
      FROM `__EFMigrationsHistory`
      ORDER BY `MigrationId`;
Microsoft.EntityFrameworkCore.Migrations[20402]
      Applying migration '20220727034356_sku'.
Applying migration '20220727034356_sku'.
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      ALTER DATABASE CHARACTER SET utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (14ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `sku_eb_problem` (
          `id` int NOT NULL AUTO_INCREMENT,
          `classification_id` longtext CHARACTER SET utf8mb4 NULL,
          `Problems_or_needs` longtext CHARACTER SET utf8mb4 NULL,
          `solution_priority` longtext CHARACTER SET utf8mb4 NULL,
          `significance_id` longtext CHARACTER SET utf8mb4 NULL,
          `proposer` longtext CHARACTER SET utf8mb4 NULL,
          `related_id` longtext CHARACTER SET utf8mb4 NULL,
          `items` longtext CHARACTER SET utf8mb4 NULL,
          `state_id` longtext CHARACTER SET utf8mb4 NULL,
          `completion_date` datetime(6) NOT NULL,
          `enter_date_input` datetime(6) NOT NULL,
          `personnel_id` longtext CHARACTER SET utf8mb4 NULL,
          `internal_id` longtext CHARACTER SET utf8mb4 NULL,
          `problem_map` longtext CHARACTER SET utf8mb4 NULL,
          `answer_map` longtext CHARACTER SET utf8mb4 NULL,
          `approved` longtext CHARACTER SET utf8mb4 NULL,
          CONSTRAINT `PK_sku_eb_problem` PRIMARY KEY (`id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (17ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `sku_problem_significance` (
          `id` int NOT NULL AUTO_INCREMENT,
          `significance` longtext CHARACTER SET utf8mb4 NULL,
          CONSTRAINT `PK_sku_problem_significance` PRIMARY KEY (`id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (13ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `sku_problem_status` (
          `id` int NOT NULL AUTO_INCREMENT,
          `status` longtext CHARACTER SET utf8mb4 NULL,
          CONSTRAINT `PK_sku_problem_status` PRIMARY KEY (`id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (23ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `sku_userinfo` (
          `id` int NOT NULL AUTO_INCREMENT,
          `userName` longtext CHARACTER SET utf8mb4 NULL,
          `related` longtext CHARACTER SET utf8mb4 NULL,
          `personnel` longtext CHARACTER SET utf8mb4 NULL,
          CONSTRAINT `PK_sku_userinfo` PRIMARY KEY (`id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (11ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `sky_problem_classif` (
          `id` int NOT NULL AUTO_INCREMENT,
          `classification` longtext CHARACTER SET utf8mb4 NULL,
          CONSTRAINT `PK_sky_problem_classif` PRIMARY KEY (`id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (12ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE `userinfo` (
          `Id` int NOT NULL AUTO_INCREMENT,
          `UserName` longtext CHARACTER SET utf8mb4 NOT NULL,
          `PassWord` longtext CHARACTER SET utf8mb4 NOT NULL,
          `RoleName` longtext CHARACTER SET utf8mb4 NULL,
          `Name` longtext CHARACTER SET utf8mb4 NULL,
          `status` int NOT NULL,
          CONSTRAINT `PK_userinfo` PRIMARY KEY (`Id`)
      ) CHARACTER SET=utf8mb4;
Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (4ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO `__EFMigrationsHistory` (`MigrationId`, `ProductVersion`)
      VALUES ('20220727034356_sku', '6.0.7');
Done.
PM> 
```



成功生成数据库 

最后出现数据库表字段变动，使用update-database 更新

# xshell工具使用
 [xshell工具使用](docs/xshell工具使用.md)
 
# 最便捷的Log4Net使用方法

[Log4Net教程](https://github.com/Justin1107-good/dotnetResource/blob/23c60b055963cd4213907881daaff93aa2da5b1b/docs/Log4Net.md)
