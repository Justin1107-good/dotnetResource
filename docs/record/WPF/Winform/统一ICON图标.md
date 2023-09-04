## 设置界面统一ICON图标

### 新建统一图标WindowHooker 类

```
 public class WindowHooker
    {
        /// <summary>
        /// 页面统一图标
        /// 创建时间：2022.06.17
        /// </summary>
        public class HookControlEventArgs : EventArgs
        {
            public Control Control;

            public HookControlEventArgs(Control c)
            {
                this.Control = c;
            }
        }

        #region win32 api
        internal delegate IntPtr HOOKPROC(int nCode, IntPtr wParam, IntPtr lParam);
        [DllImport("User32.dll", CharSet = CharSet.Auto)]
        internal static extern IntPtr SetWindowsHookEx(int idHook, HOOKPROC lpfn, int hMod, int dwThreadId);
        [DllImport("User32.dll", CharSet = CharSet.Auto)]
        internal static extern IntPtr CallNextHookEx(IntPtr hhk, int nCode, IntPtr wParam, IntPtr lParam);
        #endregion

        internal IntPtr PHook = IntPtr.Zero;
        internal HOOKPROC PHookProc = null;

        public event EventHandler<HookControlEventArgs> OnHookControl;

        public void Hook()
        {
            PHookProc = new HOOKPROC(FnHookProc);
            // 如果代码改为：SetWindowsHookEx(5, FnHookProc, 0, AppDomain.GetCurrentThreadId());
            // 则会报如下错误：
            // ”类型的已垃圾回收委托进行了回调。这可能会导致应用程序崩溃、损坏和数据丢失。向非托管代码传递委托时，托管应用程序必须让这些委托保持活动状态，直到确信不会再次调用它们。”
            //PHook = SetWindowsHookEx(5, PHookProc, 0, AppDomain.GetCurrentThreadId());
        }

        private IntPtr FnHookProc(int nCode, IntPtr wParam, IntPtr lParam)
        {
            switch (nCode)
            {
                case 5:
                    {
                        var control = Control.FromHandle(wParam);
                        if (control != null)
                        {
                            var frm = control as Form;
                            if (frm != null)
                            {
                                if (OnHookControl != null)
                                {
                                    OnHookControl(this, new HookControlEventArgs(frm));
                                }
                            }
                        }
                        break;
                    }
            }
            return CallNextHookEx(PHook, nCode, wParam, lParam);
        }
    }
```



```c#
  static void setIcon()
        {
            WindowHooker hooker = new WindowHooker();
            hooker.OnHookControl += (o, e) =>
            {
                Console.WriteLine(e.Control.GetType().ToString());
                var frm = e.Control as Form;
                frm.Icon = new System.Drawing.Icon(Application.StartupPath + "\\resouse " + "\\skycore.ico");
            };
            hooker.Hook();
        }
```

### **注意：**以上方案都写入到Program 的main方法内，完整代码

```c#
static class Program
    {
        /// <summary>
        /// 应用程序的主入口点。
        /// </summary>
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            
            //获得当前登录的Windows用户标示

            WindowsIdentity identity = WindowsIdentity.GetCurrent();

            WindowsPrincipal principal = new WindowsPrincipal(identity);
            //判断当前登录用户是否为管理员

            if (principal.IsInRole(WindowsBuiltInRole.Administrator))
            {
                setIcon();
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


        }
        static void setIcon()
        {
            WindowHooker hooker = new WindowHooker();
            hooker.OnHookControl += (o, e) =>
            {
                Console.WriteLine(e.Control.GetType().ToString());
                var frm = e.Control as Form;
                frm.Icon = new System.Drawing.Icon(Application.StartupPath + "\\resouse " + "\\skycore.ico");
            };
            hooker.Hook();
        }
    }
```
 
