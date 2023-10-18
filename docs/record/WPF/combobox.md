
https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.combobox.dropdownclosed?view=netframework-4.8

```
 <ComboBox Margin="10" x:Name="combox" StaysOpenOnEdit="true" IsEditable="true" DropDownOpened="OnDropDownOpened" 
        DropDownClosed="OnDropDownClosed" ItemsSource="{Binding cmblist}" Width="200" Height="35" HorizontalAlignment="Left" VerticalAlignment="Top"/>
```

```
 void OnDropDownOpened(object sender, EventArgs e)
        {
            if (combox.IsDropDownOpen == true)
            {
                combox.Text = "Combo box opened";
            }
        }

        void OnDropDownClosed(object sender, EventArgs e)
        {
            if (combox.IsDropDownOpen == false)
            {
                combox.Text = combox.SelectedValue.ToString();
            }
        }
```
