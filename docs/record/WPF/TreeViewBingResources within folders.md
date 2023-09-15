# 文件夹资源数据绑定到TreeView


```C#
<TreeView x:Name="folderTreeView" HorizontalAlignment="Left" VerticalAlignment="Top">
            <!-- 树的节点将在后面的步骤中通过代码添加"-->
</TreeView>
```

CS HidCode

```
public partial class MyFolderResourcesPage : Page
{
       public MyFolderResourcesPage()
        {
            InitializeComponent();
LoadFolderResources("D:\\DataPortal\\Test");
        }
private void LoadFolderResources(string folderPath)
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(folderPath);

            // 添加根节点
            TreeViewItem rootNode = new TreeViewItem();
            rootNode.Header = directoryInfo.Name;
            folderTreeView.Items.Add(rootNode);

            // 递归添加子节点
            AddChildNodes(rootNode, directoryInfo);
        }
private void AddChildNodes(TreeViewItem parentNode, DirectoryInfo directoryInfo)
        {
            foreach (DirectoryInfo subDir in directoryInfo.GetDirectories())
            {
                TreeViewItem childNode = new TreeViewItem();
                childNode.Header = subDir.Name;
                childNode.Tag = subDir.FullName;
                parentNode.Items.Add(childNode);

                // 递归添加子节点
                AddChildNodes(childNode, subDir);
            }
            // 添加文件到文件列表
            foreach (FileSystemInfo fileOrDir in directoryInfo.GetFileSystemInfos())
            {
                TreeViewItem item = new TreeViewItem();
                item.Header = fileOrDir.Name;
                item.Tag = fileOrDir.FullName;
                parentNode.Items.Add(item); 
            }
            
        } 
}
```


