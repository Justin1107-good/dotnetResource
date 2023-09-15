## TreeView TreeNode Add Right Click

```C#
<Page.Resources>
       <ContextMenu x:Key="fileContextMenu">
            <MenuItem Header="打开" Click="OpenMenuItem_Click"/>
            <MenuItem Header="复制" Click="CopyMenuItem_Click"/>
            <MenuItem Header="文件路径" x:Name="FilePathMenuItem" Click="FilePathMenuItem_Click"/>
            <MenuItem Header="上传PDM" x:Name="UploadPDMMenuItem" Click="UploadPDMMenuItem_Click"/>
            <!-- 添加其他菜单项根据需求 -->
        </ContextMenu>

</Page.Resources>

<Grid>
 <TreeView x:Name="folderTreeView" HorizontalAlignment="Left" VerticalAlignment="Top"  MouseRightButtonDown="folderTreeView_MouseRightButtonDown">
            <!-- 树的节点将在后面的步骤中通过代码添加 "-->
        </TreeView>
</Grid>

```

CS HideCode

```c#
public partial class MyFolderResourcesPage : Page
{
public MyFolderResourcesPage()
        {
            InitializeComponent();
       LoadFolderResources("C://");
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
 private void folderTreeView_MouseRightButtonDown(object sender, MouseButtonEventArgs e)
        {
            TreeViewItem treeViewItem = folderTreeView.SelectedItem as TreeViewItem;
            if (treeViewItem != null)
            {
                // 获取所选项的完整路径
                string fullPath = GetItemFullPath(treeViewItem);

                // 判断是否为文件
                bool isFile = File.Exists(fullPath);

                if (isFile)
                {
                    // 如果是文件节点，显示右键菜单
                    ContextMenu contextMenu = (ContextMenu)this.Resources["fileContextMenu"];
                    treeViewItem.ContextMenu = contextMenu;
                } 
            } 
        }



}
```
