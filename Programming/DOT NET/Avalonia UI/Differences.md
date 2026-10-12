## Trigger becomes Selector

In WPF:
```xml
<ControlTemplate.Triggers>
	<Trigger Property="IsMouseOver" Value="True">
		<Setter TargetName="border" Property="Background" 
			Value="{StaticResource Surface.Alt}"/>
		<Setter TargetName="border" Property="BorderBrush" 
			Value="{StaticResource Accent}"/>
	</Trigger>   
</ControlTemplate.Triggers>
```

In AvaloniaUI:
```XML
<Style Selector="^:pointerover /template/ Border#border">
    <Setter Property="Background" Value="{StaticResource Surface.Alt}"/>
    <Setter Property="BorderBrush" Value="{StaticResource Accent}"/>
</Style>
```

## DataTrigger

In WPF:
```xml
<Ellipse Width="9" Height="9" VerticalAlignment="Center" Margin="0,0,7,0" 
	StrokeThickness="1.5">
	<Ellipse.Style>
		<Style TargetType="Ellipse">
			<Setter Property="Fill" Value="Transparent"/>
			<Setter Property="Stroke" 
				Value="{StaticResource Text.OnDarkSubtle}"/>
			<Style.Triggers>
				<DataTrigger Binding="{Binding IsConnected}" 
					Value="True">
					<Setter Property="Fill" Value="White"/>
					<Setter Property="Stroke" Value="White"/>
				</DataTrigger>
			</Style.Triggers>
		</Style>
	</Ellipse.Style>
</Ellipse>
```

In AvaloniaUI:
```XML
<Ellipse Width="9" Height="9" VerticalAlignment="Center" Margin="0,0,7,0"
		 StrokeThickness="1.5" Classes.connected="{Binding IsConnected}">
	<Ellipse.Styles>
		<Style Selector="Ellipse">
			<Setter Property="Fill" Value="Transparent"/>
			<Setter Property="Stroke" 
				Value="{StaticResource Text.OnDarkSubtle}"/>
		</Style>
		<Style Selector="Ellipse.connected">
			<Setter Property="Fill" Value="White"/>
			<Setter Property="Stroke" Value="White"/>
		</Style>
	</Ellipse.Styles>
</Ellipse>
```

## Trigger becomes Selector

In WPF:
```XML
<ControlTemplate TargetType="{x:Type Button}">
	<ControlTemplate.Triggers>
		<Trigger Property="IsMouseOver" Value="True">
			<Setter TargetName="border" Property="Background" 
				Value="{StaticResource Surface.Alt}"/>
			<Setter TargetName="border" Property="BorderBrush" 
				Value="{StaticResource Accent}"/>
		</Trigger>
	</ControlTemplate.Triggers>
</ControlTemplate>
```
\<Trigger\> Is inside the control template.


In AvaloniaUI:

``` XML
<Setter Property="Template">
    <ControlTemplate>
        
    </ControlTemplate>
</Setter>
<Style Selector="^:pointerover /template/ Border#border">
    <Setter Property="Background" Value="{StaticResource Surface.Alt}"/>
    <Setter Property="BorderBrush" Value="{StaticResource Accent}"/>
</Style>
```

Instead of placing a \<Trigger\> inside of a Template
