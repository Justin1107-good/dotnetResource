## 获得当前登录的Windows用户标示

```C#
 //获得当前登录的Windows用户标示

WindowsIdentity identity = WindowsIdentity.GetCurrent();

WindowsPrincipal principal = new WindowsPrincipal(identity);
```
## 判断当前登录用户是否为管理员

```c#
 //判断当前登录用户是否为管理员

            if (principal.IsInRole(WindowsBuiltInRole.Administrator))
            {
                //设置ICON图标
                setIcon();
                //窗体启动设置
                Application.Run(new MainForm());
            }
            else
            {
                //创建启动对象

                ProcessStartInfo startInfo = new ProcessStartInfo();

                startInfo.UseShellExecute = true;

                startInfo.WorkingDirectory = Environment.CurrentDirectory;

                startInfo.FileName = Application.ExecutablePath;

                //设置启动动作,确保以管理员身份运行

                startInfo.Verb = "runas";

                try

                {
                   //设置ICON图标
                   setIcon();
                    Process.Start(startInfo);

                }

                catch

                {

                    return;

                }

                //退出

                Application.Exit();
            }
```


 
