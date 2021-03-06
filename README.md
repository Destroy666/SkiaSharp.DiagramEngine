# SkiaSharp.DiagramEngine
Using SkiaSharp with Xaml,Bindings and DataTemplates. Compatible with Xamarin Forms

## Using SkiaSharp.DiagramEngine
Install the [NuGet package SynodeTechnologies.SkiaSharp.DiagramEngine](https://www.nuget.org/packages/SynodeTechnologies.SkiaSharp.DiagramEngine):
```
nuget install SynodeTechnologies.SkiaSharp.DiagramEngine
```

## Getting Started
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:de="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Controls;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             x:Class="Demos.GetStarted">
    <de:Canvas>
        <de:Box Width="50" Height="50" X="30" Y="30" BackgroundColor="green" />
        <de:Circle Width="50" Height="50" X="130" Y="70" BackgroundColor="#FF000000" />
    </de:Canvas>
</ContentPage>
```

![Image of GetStarted](https://raw.githubusercontent.com/kevinbrunet/SkiaSharp.DiagramEngine/master/Docs/Images/GetStarted.png)

## Layouts
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:de="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Controls;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:layouts="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Layouts;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             x:Class="Demos.WithLayout">
    <de:Canvas>
        <de:Container>
            <de:Container.Layout>
                <layouts:Grid>
                    <layouts:Grid.ColumnDefinitions>
                        <layouts:ColumnDefinition Width="70"/>
                        <layouts:ColumnDefinition Width="Auto"/>
                        <layouts:ColumnDefinition Width="*"/>
                        <layouts:ColumnDefinition Width="Auto"/>
                    </layouts:Grid.ColumnDefinitions>
                    <layouts:Grid.RowDefinitions>
                        <layouts:RowDefinition Height="140"/>
                        <layouts:RowDefinition Height="*"/>
                    </layouts:Grid.RowDefinitions>
                </layouts:Grid>
            </de:Container.Layout>
            <de:Box layouts:Grid.Column="0" BackgroundColor="green" />
            <de:TextBlock Text="Hello World !" Color="black" layouts:Grid.Column="1" BackgroundColor="whitesmoke" BorderColor="black" />
            <de:Oval layouts:Grid.Column="3" Width="100" BackgroundColor="blue" />
        </de:Container>
    </de:Canvas>
</ContentPage>
```

![Image of Layout](https://raw.githubusercontent.com/kevinbrunet/SkiaSharp.DiagramEngine/master/Docs/Images/Layout.PNG)

## TemplatedItems
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:de="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Controls;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:layouts="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Layouts;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:touchs="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.TouchListeners;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             x:Class="Demos.TemplateItems">
    <de:Canvas>
        <de:Canvas.TouchListener>
            <touchs:XamlCommand>
                <touchs:Simple/>
            </touchs:XamlCommand>
        </de:Canvas.TouchListener>
        <de:Container>
            <de:Container.Layout>
                <layouts:Grid>
                    <layouts:Grid.ColumnDefinitions>
                        <layouts:ColumnDefinition Width="*"/>
                    </layouts:Grid.ColumnDefinitions>
                    <layouts:Grid.RowDefinitions>
                        <layouts:RowDefinition Height="50"/>
                        <layouts:RowDefinition Height="*"/>
                    </layouts:Grid.RowDefinitions>
                </layouts:Grid>
            </de:Container.Layout>
            <de:TextBlock Text="Add" Color="black" BackgroundColor="gray" touchs:XamlCommand.Command="{Binding AddItemCommand}" />
            <de:ItemsTemplate ItemsSource="{Binding Items}" Y="30" layouts:Grid.Row="1">
                <de:ItemsTemplate.Layout>
                    <layouts:Stack/>
                </de:ItemsTemplate.Layout>
                <DataTemplate>
                    <de:TextBlock Color="green" Text="{Binding Name}"/>
                </DataTemplate>
            </de:ItemsTemplate>
        </de:Container>
    </de:Canvas>
</ContentPage>
```

```csharp
[XamlCompilation(XamlCompilationOptions.Compile)]
	public partial class TemplateItems : ContentPage
	{
        public class Item : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;


            private string name;
            public string Name
            {
                get
                {
                    return name;
                }
                set
                {
                    name = value;
                    PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
                }
            }
        }

        private ObservableCollection<Item> items = new ObservableCollection<Item>();
        public ObservableCollection<Item> Items
        {
            get
            {
                return items;
            }
           
        }

        public ICommand AddItemCommand { get; }

        public TemplateItems()
		{
			InitializeComponent ();
            this.AddItemCommand = new Command(() =>
            {
                Items.Add(new Item() { Name = "Item" + Items.Count });
            });
            this.BindingContext = this;
		}
	}
```

![Image of TemplatedItems](https://raw.githubusercontent.com/kevinbrunet/SkiaSharp.DiagramEngine/master/Docs/Images/ItemsTemplate.PNG)

## ZoomCanvas
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:de="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Controls;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:layouts="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Layouts;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
                xmlns:touchs="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.TouchListeners;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
         x:Class="Demos.ZoomCanvas">
    <de:ZoomCanvas x:Name="zoomCanvas">
        <de:ZoomCanvas.TouchListener>
            <touchs:XamlCommand>
                <touchs:Simple/>
            </touchs:XamlCommand>
        </de:ZoomCanvas.TouchListener>
        <de:ZoomCanvas.Overlay>
            <de:Container>
                <de:Container.Layout>
                    <layouts:Grid>
                        <layouts:Grid.ColumnDefinitions>
                            <layouts:ColumnDefinition Width="*"/>
                            <layouts:ColumnDefinition Width="100"/>
                        </layouts:Grid.ColumnDefinitions>
                        <layouts:Grid.RowDefinitions>
                            <layouts:RowDefinition Height="100"/>
                            <layouts:RowDefinition Height="100"/>
                            <layouts:RowDefinition Height="*"/>
                        </layouts:Grid.RowDefinitions>
                    </layouts:Grid>
                </de:Container.Layout>
                <de:Box BackgroundColor="black" Margin="10,10,10,10" layouts:Grid.Row="0" layouts:Grid.Column="1" touchs:XamlCommand.Command="{Binding Source={x:Reference zoomCanvas}, Path=IncrementalZoomCommand}" touchs:XamlCommand.CommandParameter="1.2">
                    <de:Polygon Path="M114.352,297.47l-97.509,97.501c-22.448,22.463-22.466,58.9,0,81.372c22.48,22.47,58.906,22.462,81.386,0l97.434-97.44c-16.619-9.588-32.092-21.416-46.023-35.349C135.567,329.482,123.816,313.959,114.352,297.47z
                                          M438.242,54.94C402.814,19.514,355.699,0.005,305.594,0.005c-50.109,0-97.205,19.519-132.641,54.952c-73.148,73.147-73.148,192.156,0,265.296c35.436,35.442,82.531,54.958,132.641,54.958c50.105,0,97.206-19.517,132.635-54.95c35.449-35.423,54.969-82.53,54.969-132.644C493.197,137.502,473.678,90.387,438.242,54.94z 
                                          M403.28,285.305c-26.1,26.098-60.79,40.477-97.687,40.477c-36.897,0-71.592-14.378-97.688-40.477c-53.873-53.872-53.873-141.529,0-195.4c26.097-26.091,60.791-40.467,97.688-40.467c36.91,0,71.601,14.369,97.7,40.46c26.102,26.107,40.469,60.807,40.469,97.72C443.763,224.52,429.396,259.213,403.28,285.305z
                                          M369.1,162.893l-38.794,0.003v-38.766c0-13.653-11.066-24.715-24.712-24.715c-13.645,0-24.717,11.062-24.717,24.715v38.769l-38.764,0.003c-13.645,0-24.716,11.069-24.716,24.715c0,13.652,11.071,24.715,24.716,24.715l38.764-0.002v38.765c0,13.654,11.072,24.717,24.717,24.717c13.646,0,24.712-11.063,24.712-24.717v-38.768l38.794-0.002c13.646,0,24.719-11.072,24.719-24.717C393.818,173.955,382.746,162.893,369.1,162.893z" BackgroundColor="white" />
                </de:Box>

                <de:Box BackgroundColor="black" Margin="10,10,10,10" layouts:Grid.Row="1"  layouts:Grid.Column="1" touchs:XamlCommand.Command="{Binding Source={x:Reference zoomCanvas}, Path=IncrementalZoomCommand}" touchs:XamlCommand.CommandParameter="0.8">
                    <de:Polygon Path="M114.352,297.47l-97.509,97.501c-22.448,22.463-22.466,58.9,0,81.372c22.48,22.47,58.906,22.462,81.386,0l97.434-97.44c-16.619-9.588-32.092-21.416-46.023-35.349C135.567,329.482,123.816,313.959,114.352,297.47z
                                          M438.242,54.94C402.814,19.514,355.699,0.005,305.594,0.005c-50.109,0-97.205,19.519-132.641,54.952c-73.148,73.147-73.148,192.156,0,265.296c35.436,35.442,82.531,54.958,132.641,54.958c50.105,0,97.206-19.517,132.635-54.95c35.449-35.423,54.969-82.53,54.969-132.644C493.197,137.502,473.678,90.387,438.242,54.94z 
                                          M403.28,285.305c-26.1,26.098-60.79,40.477-97.687,40.477c-36.897,0-71.592-14.378-97.688-40.477c-53.873-53.872-53.873-141.529,0-195.4c26.097-26.091,60.791-40.467,97.688-40.467c36.91,0,71.601,14.369,97.7,40.46c26.102,26.107,40.469,60.807,40.469,97.72C443.763,224.52,429.396,259.213,403.28,285.305z
                                          M368.34299,166.67809 c -48.23954,0.12401 -82.13915,0.006 -126.987,0.009 -13.645,0 -24.716,11.069 -24.716,24.715 0,13.652 11.071,24.715 24.716,24.715 126.987,-0.007 0,0 126.987,-0.007 13.646,0 24.719,-11.072 24.719,-24.717 -10e-4,-13.653 -11.073,-24.715 -24.719,-24.715 z" BackgroundColor="white" />
                </de:Box>

            </de:Container>
        </de:ZoomCanvas.Overlay>
        <de:Box Width="50" Height="50" X="30" Y="30" BackgroundColor="green" />
        <de:Circle Width="50" Height="50" X="130" Y="70" BackgroundColor="#FF000000" />
    </de:ZoomCanvas>
</ContentPage>
```

![Image of ZoomCanvas](https://raw.githubusercontent.com/kevinbrunet/SkiaSharp.DiagramEngine/master/Docs/Images/Zoom.PNG)

## HierarchicalItemsTemplate
```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:de="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Controls;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:layouts="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.Layouts;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             xmlns:touchs="clr-namespace:SynodeTechnologies.SkiaSharp.DiagramEngine.TouchListeners;assembly=SynodeTechnologies.SkiaSharp.DiagramEngine"
             x:Class="Demos.HierarchicalItemsTemplate">
    <de:Canvas>
        <de:Canvas.TouchListener>
            <touchs:XamlCommand>
                <touchs:Simple/>
            </touchs:XamlCommand>
        </de:Canvas.TouchListener>
        <de:Container>
            <de:Container.Layout>
                <layouts:Grid>
                    <layouts:Grid.ColumnDefinitions>
                        <layouts:ColumnDefinition Width="*"/>
                    </layouts:Grid.ColumnDefinitions>
                    <layouts:Grid.RowDefinitions>
                        <layouts:RowDefinition Height="*"/>
                    </layouts:Grid.RowDefinitions>
                </layouts:Grid>
            </de:Container.Layout>
            <de:HierarchicalItemsTemplate BindingContext="{Binding Root}"  ItemsSourceSelector="Children" layouts:Grid.Row="0">
                <de:HierarchicalItemsTemplate.Layout>
                    <layouts:HierarchicalTree VerticalSpacing="16" />
                </de:HierarchicalItemsTemplate.Layout>
                <DataTemplate>
                        <de:TextBlock Color="black" Text="{Binding Name}" BorderWidth="1" BorderColor="black" Padding="10" FontSize="32"/>
                </DataTemplate>
            </de:HierarchicalItemsTemplate>
        </de:Container>
    </de:Canvas>
</ContentPage>
```

```csharp
public partial class HierarchicalItemsTemplate : ContentPage
	{
        public class HierarchicalItem : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            public HierarchicalItem(string name)
            {
                this.name = name;
            }

            private string name;
            public string Name
            {
                get
                {
                    return name;
                }
                set
                {
                    name = value;
                    PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
                }
            }

            private ObservableCollection<HierarchicalItem> children = new ObservableCollection<HierarchicalItem>();

            public ObservableCollection<HierarchicalItem> Children

            {
                get
                {
                    return children;
                }
            }
        }

        private HierarchicalItem root;
        public HierarchicalItem Root
        {
            get
            {
                return root;
            }
           
        }


        public HierarchicalItemsTemplate()
		{
			InitializeComponent ();
            root = new HierarchicalItem("Root");
            root.Children.Add(new HierarchicalItem("1"));
            root.Children[0].Children.Add(new HierarchicalItem("1.1"));
            root.Children[0].Children.Add(new HierarchicalItem("1.2"));
            root.Children[0].Children.Add(new HierarchicalItem("1.3"));
            root.Children.Add(new HierarchicalItem("2"));
            root.Children[1].Children.Add(new HierarchicalItem("2.1"));
            root.Children[1].Children.Add(new HierarchicalItem("2.2"));
            root.Children[1].Children.Add(new HierarchicalItem("2.3"));
            root.Children.Add(new HierarchicalItem("Lorem ipsum dolor sit amet, consectetur adipiscing elit.\nSed non risus. Suspendisse lectus tortor, dignissim sit amet, adipiscing nec, ultricies sed, dolor."));
            root.Children[2].Children.Add(new HierarchicalItem("3.1"));
            root.Children[2].Children.Add(new HierarchicalItem("3.2"));
            root.Children[2].Children.Add(new HierarchicalItem("3.3"));
            root.Children[2].Children[0].Children.Add(new HierarchicalItem("Lorem ipsum dolor sit amet"));
            this.BindingContext = this;
		}
	}
```


![Image of ZoomCanvas](https://raw.githubusercontent.com/kevinbrunet/SkiaSharp.DiagramEngine/master/Docs/Images/HierarchicalItemsTemplate.PNG)
