# LOG4NET 配置日志记录器

### 引用

using log4net;
using log4net.Appender;
using log4net.Layout;
using log4net.Repository.Hierarchy;

```
public static ILog LogConfinuration(Type type)
        {
            // 配置日志记录器
            Hierarchy hierarchy = (Hierarchy)LogManager.GetRepository();
            hierarchy.Root.RemoveAllAppenders();

            // 定义轮廓布局
            PatternLayout patternLayout = new PatternLayout();
            patternLayout.ConversionPattern = "%date [%thread] %-5level %logger - %message%newline";
            patternLayout.ActivateOptions();

            // 定义文件写入器appender
            FileAppender fileAppender = new FileAppender();
            fileAppender.AppendToFile = true;
            fileAppender.File = $"C:\\ProgramData\\Aucotec\\Engineering Base\\Logfiles\\{System.Environment.UserName}_Test_log4net_log.log";
            fileAppender.Layout = patternLayout;
            fileAppender.ActivateOptions();

            // 将appender添加到root记录器
            hierarchy.Root.AddAppender(fileAppender);
            hierarchy.Root.Level = log4net.Core.Level.Debug;
            hierarchy.Configured = true;

            return LogManager.GetLogger(type);
        }
 ```
