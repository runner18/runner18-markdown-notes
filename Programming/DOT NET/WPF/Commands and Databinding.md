
## Binding Properties
Highly recommend using the MVVM Community Toolkit, it implements the data binding code so you don't have to. Most of the time its code is fine, unless you need something really custom.

Here is how you bind the "Classic" INotifyPropertyChanged way:
```cs
private bool _isStopLightOn;
public bool isStopLightOn
{
	get => isStopLightOn;
	set 
	{
		if(_isStopLightOn != value)
		{
			_isStopLightOn = value;
			OnPropertyChanged();
		}
	}
}
```

Here is how you bind the "Classic" way but with some CommunityToolkit assisstance:
Note the "SetProperty" method:

```CS
private IQuantity? _localValue;
public IQuantity? LocalValue 
{
    get => _localValue;
    set => SetProperty(ref _localValue, value);
}
```

DO THIS ONE: Here is the latest CommunityToolkit way to data bind:
```CS
public partial class ExampleViewMode: ObservableObject
{
	// ...
	[ObservableProperty]
	public partial string ExampleString { get; set; } = string.Empty;
}
```

## XAML Binding
```XML
<Grid DataContext="Employee">
	<TextBox Text="{Binding Path=Salary, Mode=TwoWay}"/>
</Grid>
```
![](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-10.png?w=755)

## Commands
``` XML
<Button Content="Example Button" Command = "{Binding PrintReportCommand}"/>
```

```CS
[RelayCommand]
public async Task PrintReport()
{
	// Code
}
```

CommunityToolkit will match up the names automatically.
Make sure the xaml Binding phrase has "Command" at the end.

## Call function from Across ViewModels
Let's say you have two ViewModels: MainViewModel and ConfigureViewModel.
- Say ConfigureViewModel reacts to a button that configures WiFi
- However, only the MainViewModel is actually capable of configuring the WiFi

Here's how you would call a function in MainViewModel from a function in ConfigureViewModel:

![](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-36.png?w=1024)

ConfigurationConfigureWifi() is called after a button press in the ConfigurationView. 
See Messenger.SendConfigureWifiRequest() as well.

![478](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-37.png?w=802)

![449](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-38.png?w=718)

![500](https://18adiehl-rewpo.wordpress.com/wp-content/uploads/2025/12/image-39.png?w=844)

## Call Code-Behind function from ViewModel
### In the ViewModel:
Create an action to subscribe to:
```CS
public delegate void ClearDaqSettingsAction();
public event ClearDaqSettingsAction ClearDaqSettings;

// Call that when you need to
ClearDaqSettings?.Invoke();
```

### In the Code-behind
Create a function that gets called when the control's ViewModel is changed/created.
In that function, add this code:
```CS
private void Vm_ClearDaqSettings()
{
	if(DataContext is DaqSetupViewModel vm)
	{
	}
}
```

```CS
private void UserControl_DataContextChanged(object sender, DependencyPropertyChangedEventArgs e)
{
	//l looks at the DaqExpansionModule in the ViewModel for channel information
	if(DataContext is DaqSetupViewModel vm)
	{
		Vm_ClearDaqSettings();
		vm.ClearDaqSettings += Vm_ClearDaqSettings;
	}
}
```

Parameters can be used as well. Anywhere in these code snippets where there are empty parenthesis "()" those parenthesis can include parameters.

Warning! If you try to invoke the ViewModel event in the constructor, the code-behind may not be created yet if you are using **dependency injection.** You are better off in this case calling the function in the code-behind's constructor.

### Set Binding in Code-Behind
```CS
// set binding for value text block
Binding tempBinding = new Binding();
tempBinding.Source = valueBindingSource;
tempBinding.Path = new PropertyPath(valuePath);
tempBinding.Mode = BindingMode.TwoWay;
tempBinding.NotifyOnSourceUpdated = true;
tempBinding.NotifyOnTargetUpdated = true;
BindingOperations.SetBinding(valueTextBlock, TextBlock.TextProperty, tempBinding);
```

```CS
// set binding for value text block
Binding tempBinding = new Binding();
tempBinding.Source = valueBindingSource;
tempBinding.Path = new PropertyPath(valuePath);
tempBinding.Mode = BindingMode.TwoWay;
tempBinding.NotifyOnSourceUpdated = true;
tempBinding.NotifyOnTargetUpdate = true;
BindingOperations.SetBinding(valueTextBlock, TextBlock.TextProperty, tempBinding);
```

```CS
if (ctrl.FindName("tbValue") is TextBlock tbValue)
{
	_ = tbValue.SetBinding(TextBlock.TextProperty, new Binding($"AnalogEng[{i}]"))
	{
		Source = dc.NIDaqMxReadings,
		Converter = new UnitsNetQuantityConverter(),
		ConverterParameter = "#0,0",
	}
}
```