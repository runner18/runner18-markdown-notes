## App Icon
![](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-84.png?w=768)

## Types of UI (buttons, etc.)
### Datagrid / ListView /  GridView

DataGrid is used when you need to be able to edit stuff.
Also supports automatic column generation.
```XML
<!--table #1-->
<ListView ItemsSource="{Binding Path=TestSearchResults}" SelectedIndex="{Binding Path=SelectedTestResultIndex}">
```

**Call function when selected item changes**
What I do is bind the selected index to a number, then do an OnPropertychanged for that number.
```xml
SelectedIndex="{Binding Path=SelectTestResultIndex}"
```

In the ViewModel
``` CS
[ObservableProperty]
[NotifyCanExecuteChangedFor(nameof(UpdateLogEntryTableCommand))]
int selectedTestResultIndex;

partial void OnSelectedTestResultIndexChanged(int value)
{
	_ = UpdateLogEntryTable();
}

[RelayCommand]
public async Task UpdateLogEntryTable()
{
	
}
```

Editing a DataGrid in the code-behind: [link](https://stackoverflow.com/questions/704724/programmatically-add-column-rows-to-wpf-datagrid)

```CS
DataGrid.Items.Add(new DataItem());

DataGridTextColumn textColumn = new DataGridTextColumn();
textColumn.Header = "First Name";
textColumn.Binding = new Binding("FirstName");
dataGrid.Columns.Add(textColumn);
```

To set the content of DataGrid:

- set dataGrid.ItemsSource to List\<ExampleClass\> listOfObjects
- Each object in listOfObjects will be a **row** in dataGrid
- each property in ExampleClass can be **column** in dataGrid

DataGrid auto generates each column based on each public property in ExampleClass. Set dataGrid.AutoGenerateColumns to **false** to add the columns yourself (don't forget to bind the column to a property in the object's class).

A helpful example from [https://wpf-tutorial.com/datagrid-control/custom-columns/](https://wpf-tutorial.com/datagrid-control/custom-columns/)

```xml
<DataGrid Name="dgUsers" AutoGenerateColumns="False">
  <DataGrid.Columns>

      <DataGridTextColumn Header="Name" Binding="{Binding Name}" />

      <DataGridTemplateColumn Header="Birthday">
          <DataGridTemplateColumn.CellTemplate>
              <DataTemplate>
                  <DatePicker SelectedDate="{Binding Birthday}" BorderThickness="0" />
              </DataTemplate>
          </DataGridTemplateColumn.CellTemplate>
      </DataGridTemplateColumn>

  </DataGrid.Columns>
</DataGrid>
```

```CS
using System;
using System.Collections.Generic;
using System.Windows;

namespace WpfTutorialSamples.DataGrid_control
{
	public partial class DataGridColumnsSample : Window
	{
		public DataGridColumnsSample()
		{
			InitializeComponent();

			List<User> users = new List<User>();
			users.Add(new User() { Id = 1, Name = "John Doe", Birthday = new DateTime(1971, 7, 23) });
			users.Add(new User() { Id = 2, Name = "Jane Doe", Birthday = new DateTime(1974, 1, 17) });
			users.Add(new User() { Id = 3, Name = "Sammy Doe", Birthday = new DateTime(1991, 9, 2) });

			dgUsers.ItemsSource = users;
		}
	}

	public class User
	{
		public int Id { get; set; }

		public string Name { get; set; }

		public DateTime Birthday { get; set; }
	}
}
```

![](Pasted%20image%2020261012125223.png)

### ComboBox / ListBox
In the CS:
```CS
public class Person
{
	public string Name;
	public int Age;
	public string Gender;
}

// assume this collectoin is already full of Person objects
ObservableCollection<Person> PersonCollection { get; } = new ObservableCount();

public Person PersonYouWantToBindTo = new Person();
public string GenderYouWantToBindTo = new String.Empty;
```

XAML Scenario #1 - the combo box will show the names in the dropdown UI, and will give you a person object
```XML
<ComboBox ItemsSource = "{Binding PersonCollection}"
	DisplayMemberPath = "Name"
	SelectedItem = "{Binding PersonYouWantToBindTo}"/>
```

XAML Scenario #2 - the combo box will show the names in the dropdown UI, and will give you a  gender string
```XML
<ComboBox ItemsSource = "{Binding PersonCollection}"
	DisplayMemberPath = "Name"
	SelectedValuePath = "Gender"
	SelectedValue = "{Binding GenderYouWantToBindTo}"/>
```

## Resources / Styles
```XML
<Style x:Key="NavigationButton" TargetType="{x:Type Button}" 
	BasedOn="{StaticResource DefaultButtonControlStyle}">
	<Setter Property="Border.CornerRadius" Value="0" />
	<Style.Triggers>
		<Trigger Property="IsMouseOver" Value="True">
			<Setter Property="Background" 
				Value="{StaticResource Button.MouseOver.Background}" />
		</Trigger>
	</Style.Triggers>
</Style>
```

## Adding Integers to Resource Dictionary
Add this to the namespace:
```
xmlns:sys="clr-namespace:System;assembly=System.Runtime"
```

Add this to Resource Dictionary;
```XML
<sys:Int32 x:Key="FuelChannelNameWidth">215</sys:Int32>
```

Use this in Code-behind
```CS
object? widthResource = this.TryFindResource("FuelChannelNameWidth");
if(widthResource is int channelWidth)
{
	b.DescriptionWidth = channelWidth;
}
```

## DataTrigger - Change Style Automatically
This lets you changed the style of an element based on what value a variable in the ViewModel is:
``` XML
<StackPanel Orientation="Vertical">
    <StackPanel.Style>
        <Style TargetType="StackPanel">
            <Style.Triggers>
                <DataTrigger Binding="{Binding IsSettingUpAdmin}" Value="True">
                    <Setter Property="Visibility" Value="Visible"/>
                </DataTrigger>
                <DataTrigger Binding="{Binding IsSettingUpAdmin}" Value="False">
                    <Setter Property="Visibility" Value="Collapsed"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </StackPanel.Style>
</StackPanel>
```

### Grab DataContext from Code-Behind
You have to wait for the DataContext to get set in the XAML, apparently.
``` CS
public DaqSetupSettingsPreviewControl()
{
	InitializeComponent();
	DataContextChanged += DaqSetupSettingPreviewControl_DataContextChanged();
}

private void DaqSetupSettingsPreviewControl_DataContextChanged()
{
	if (DataContext is DaqSetupViewModel vm)
	{
		AddUIElements_AddThermocupleSettings(vm);
	}
}
```

Luckily, there is a DataContextChanged event you can subscribe to. Cool!

Alternatively, if using dependency injection, do this instead;
```CS
private async void UserControl_Loaded(object sender, RouterEventArgs e)
{
	
}
```

In the XAML:
```XML
d:DataContext="{d:DesignInstance vms:TestCellOperationsViewModel}"
Loaded="UserControl_Loaded">
```

## Set DataContext (width D.I.)
You may need to pass in the ViewModel through the code-behind constructor and set the DataContext
```CS
public partial class TestCellOperationsControl : UserControl
{
	public TestCellOperationsControl(TestCellOperationsViewModel vm)
	{
		DataContext = vm;
		InitializeComponent();
	}
}
```

## Create Custom Window Chrome (— \[ \]] X)
[https://stackoverflow.com/questions/6792275/how-to-create-custom-window-chrome-in-wpf](https://stackoverflow.com/questions/6792275/how-to-create-custom-window-chrome-in-wpf)

[https://engy.us/blog/2020/01/01/implementing-a-custom-window-title-bar-in-wpf/](https://engy.us/blog/2020/01/01/implementing-a-custom-window-title-bar-in-wpf/)

Click on the second link.

## Threads and Async
### Task\<T\> vs Task\<T\>.Result
In short, use "await task", not ".Result" or "StartNew".

"Result" and "StartNew" block the thread while waiting for the task to complete:
[https://stackoverflow.com/questions/27464287/what-is-the-difference-between-await-taskt-and-taskt-result](https://stackoverflow.com/questions/27464287/what-is-the-difference-between-await-taskt-and-taskt-result)