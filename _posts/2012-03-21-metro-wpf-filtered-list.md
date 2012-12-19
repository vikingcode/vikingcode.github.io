---
layout : page
title: WPF List Filtering
date: 2012-03-21 17:22:55 +11:00
tags: "metro mahapps.metro wpf"
---

I've recently had a request to [MahApps.Metro](http://mahapps.com/MahApps.Metro/) to include a listbox with autocompletion/search, ala the Windows 8 "search" screen. There isn't a really awesome way to do this sort of filtering in a generic fashion, but there are a couple of different techniques.

In both cases, there is still the UI that needs to be handled.

##UI
The Windows 8 "search" screen looks like this  

![](/images/postimages/filtering_1.png)

As this was a request for [MahApps.Metro](http://github.com/mahapps/mahpps.metro), it makes sense to start there and install it via nuget.

####MainWindow.xaml

	<Controls:MetroWindow x:Class="FilteringDemo.MainWindow"
	        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	        xmlns:Controls="clr-namespace:MahApps.Metro.Controls;assembly=MahApps.Metro"
	        xmlns:FilteringDemo="clr-namespace:FilteringDemo" Title="MainWindow" Height="500" Width="750" ShowTitleBar="False">
	    <Controls:MetroWindow.Resources>
	        <ResourceDictionary>
	            <ResourceDictionary.MergedDictionaries>
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Colours.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Fonts.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Controls.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Controls.AnimatedSingleRowTabControl.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/Blue.xaml" />
	                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/BaseDark.xaml" />
	            </ResourceDictionary.MergedDictionaries>
	            <DataTemplate DataType="{x:Type FilteringDemo:AppThing}"> 
	                <StackPanel Width="250" Height="35" Orientation="Horizontal" Margin="10">
	                    <Rectangle Width="35" Height="35" Fill="{StaticResource AccentColorBrush}" />
	                    <TextBlock Width="205" Text="{Binding Name}" FontWeight="Light" FontSize="16" Margin="10,0,0,0" TextWrapping="Wrap"/>
	                </StackPanel>
	            </DataTemplate>
	             <SolidColorBrush x:Key="{x:Static SystemColors.HighlightBrushKey}"  Color="{DynamicResource AccentColor3}"/>
            		<SolidColorBrush x:Key="{x:Static SystemColors.ControlBrushKey}" Color="{DynamicResource AccentColor3}"/>
	            <CollectionViewSource x:Key="Apps" Source="{Binding Apps}" />
	        </ResourceDictionary>
	    </Controls:MetroWindow.Resources>
	    <Grid>
	    	<Grid.ColumnDefinitions>
	    		<ColumnDefinition Width=".75*"/>
	    		<ColumnDefinition Width=".25*"/>
	    	</Grid.ColumnDefinitions>
	        <Rectangle Fill="#FF3B3B3B" Grid.Column="1"/>
	        <ListBox Margin="33,88,8,8" BorderBrush="{x:Null}" ItemsSource="{Binding Source={StaticResource Apps}}" ScrollViewer.VerticalScrollBarVisibility="Disabled">
	        	<ListBox.ItemsPanel>
	        		<ItemsPanelTemplate>
	        			<WrapPanel Orientation="Vertical" />    		
					</ItemsPanelTemplate>     	
				</ListBox.ItemsPanel>      
			</ListBox>
	        <TextBox x:Name="txtSearch" TextChanged="TxtSearchTextChanged" TextWrapping="Wrap" VerticalAlignment="Top" RenderTransformOrigin="7.604,0.385" Margin="55,27,8,0" Grid.Column="1"/>
	        <TextBlock HorizontalAlignment="Left" TextWrapping="Wrap" VerticalAlignment="Top" Margin="33,9,0,0" FontSize="48" FontWeight="Light"><Run Language="en-au" Text="Apps"/></TextBlock>
	        <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="165.6,40.653,0,0" FontWeight="Light" FontSize="18.667"><Run Language="en-au" Text="Results for &quot;w&quot;"/></TextBlock>
	    </Grid>
	</Controls:MetroWindow>

This gives us the nice UI we need and looks like  
![](images/postimages/filtering_2.png)

##Filtering
### Filtering on the UI using a CollectionViewSource

Using filtering on a `CollectionViewSource` is the easiest way to filter in the various XAML frameworks, and for ten, twenty or even thirty objects can be fine. Once you start getting above that though, everything starts to come to a bit of a stand still. Specifically, the UI thread is blocked every time you filter. 

####MainWindow.xaml.cs

    public partial class MainWindow
    {
        public ObservableCollection<AppThing> Apps { get; set; }

        public MainWindow()
        {
            InitializeComponent();
            Apps = new ObservableCollection<AppThing>
                       {
                           new AppThing("Windows Explorer"),
                           new AppThing("Windows Speech Recognition"),
                           new AppThing("Weather"),
                           new AppThing("Windows Reader"),
                           new AppThing("Windows Defender"),
                           new AppThing("Windows Media Center"),
                           new AppThing("Windows Media Player"),
                           new AppThing("Windows Fax and Scan"),
                           new AppThing("WordPad"),
                           new AppThing("Wiggle Wiggle"),
                       };

            DataContext = this;
        }

        private void TxtSearchTextChanged(object sender, System.Windows.Controls.TextChangedEventArgs e)
        {
            var cvs = Resources["Apps"] as CollectionViewSource;
            if (cvs == null) 
                return;
            cvs.Filter -= CvsFilter;
            cvs.Filter += CvsFilter;
        }

        void CvsFilter(object sender, FilterEventArgs e)
        {
            e.Accepted = ((AppThing) e.Item).Name.ToLowerInvariant().Contains(txtSearch.Text.ToLowerInvariant());
        }
    }


In this scenario, we're searching every time the search `TextBox` is changed (`TextChanged` event). If we don't update on every keystroke, but instead on a timer of ~500ms after the last change, performance increases dramatically, but it's still a UI thread operation. As it is, this example is pretty fast. Increase the list of apps to 50, and all of a sudden there is a one or two second pause on each key stroke.

### Filtering using a background worker

[Derek Lakin](https://twitter.com/#!/dereklakin) quickly threw together a sample that I've adapted, which uses a very simple ViewModel for the data and a background worker to perform the actual filtering. This works by keeping a list of all items in ListA, binding to ListB, performing the filtering on a different thread, then clearing and updating ListB.

####ViewModel.cs

    public class ViewModel
    {
        public ObservableCollection<AppThing> Results { get; set; }
        
        public string Query
        {
            get { return _query; }
            set
            {
                _query = value;
                BeginFilter(value.ToLowerInvariant());
            }
        }

        private string _query = string.Empty;
        private readonly List<AppThing> _apps;
        private readonly BackgroundWorker _worker;

        public ViewModel(IEnumerable<AppThing> list)
        {
            _apps = new List<AppThing>(list);
            Results = new ObservableCollection<AppThing>(_apps);
            _worker = new BackgroundWorker
                          {
                              WorkerSupportsCancellation = true
                          };
            _worker.DoWork += Filter;
            _worker.RunWorkerCompleted += FilterComplete;
        }

        private void Filter(object sender, DoWorkEventArgs e)
        {
             var query = (string)e.Argument;
		e.Result = string.IsNullOrWhiteSpace(query) ? _apps : _apps.Where(a => a.Name.ToLowerInvariant().Contains(query));
        }

        private void FilterComplete(object sender, RunWorkerCompletedEventArgs e)
        {
            if (e.Cancelled) 
                return;

            var filtered = (IEnumerable<AppThing>)e.Result;
            Results.Clear();

            foreach (var f in filtered)
                Results.Add(f);
        }

        private void RefilterOnCompletion(object sender, RunWorkerCompletedEventArgs e)
        {
            _worker.RunWorkerCompleted -= RefilterOnCompletion;
            _worker.RunWorkerAsync(_query.ToLowerInvariant());
        }


        private void BeginFilter(string filterText)
        {
            if (_worker.IsBusy)
            {
                if (!_worker.CancellationPending)
                {
                    _worker.RunWorkerCompleted += RefilterOnCompletion;
                    _worker.CancelAsync();
                }
            }
            else
            {
                _worker.RunWorkerAsync(filterText.ToLowerInvariant());
            }
        }
    }

####MainWindow.xaml.cs
If I had of set up the ViewModel properly the TextChanged event handler wouldn't be required, and you could just use a `{Binding Query}`. For the sake of this demo, it isn't needed.

    public partial class MainWindow
    {
        private ViewModel vm;
        public MainWindow()
        {
            InitializeComponent();
            Apps = new ObservableCollection<AppThing> { /* apps */ };

            vm = new ViewModel(Apps);
            DataContext = vm;
        }

        private void TxtSearchTextChanged(object sender, System.Windows.Controls.TextChangedEventArgs e)
        {
            vm.Query = txtSearch.Text;
        }
    }

And finally changing the XAML to use `Results`

	<ListBox Margin="33,88,8,8" BorderBrush="{x:Null}" ItemsSource="{Binding Results}"..
	
###Delayed binding
As I mentioned earlier, "delayed bindings" - that is delaying the `TextChanged` event from triggering a filter until after user input as finished. .NET 4.5 specifically has `Delay` as a property on a binding, but until that is mainstream a similar effect can be achieved with a timer.
	
    public partial class MainWindow
    {
        public ObservableCollection<AppThing> Apps { get; set; }
        private ViewModel vm;
        private DispatcherTimer _t;
        public MainWindow()
        {
            InitializeComponent();
            Apps = new ObservableCollection<AppThing> { /* apps */ };

            vm = new ViewModel(Apps);
            DataContext = vm;
            
            _t = new DispatcherTimer(TimeSpan.FromMilliseconds(300), DispatcherPriority.DataBind, Search, Dispatcher) { IsEnabled = false };
        }

        private void Search(object sender, EventArgs e)
        {
	      _t.Stop();
            vm.Query = txtSearch.Text;
        }

        private void TxtSearchTextChanged(object sender, System.Windows.Controls.TextChangedEventArgs e)
        {
            _t.Stop();
            _t.Start();
        }
    }
    
Every time the user inputs text, the timer is restarted. While the background thread method benefits from it, the real winner is the `CollectionViewSource` filtering which becomes much more usable.

###Complex filtering
A `stringA.contains(stringB)` filter is very simple and relatively quick - but what if each of those apps had a description or author field? Searching other properties on the objects would greatly improve the search results but increase processing required - and if you're doing that on the UI thread by using a `CollectionViewSource`, you're blocking the UI for even longer.

This is why its very difficult to create a generic filterable list control - generally you're not databinding to a list of strings, but usually a list of objects. Even with markup extensions, as soon as you get into reflection territory, it really doesn't matter what filtering technique you use, your performance is shot out the window.