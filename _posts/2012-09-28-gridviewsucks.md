---
layout: page
title: Bending GridView to your will - tricks, limitations and making a 'hub'
date: 2012-10-07
---

I'm not going to sugar coat this - `GridView` sucks. `GridView` is a rather powerful and very useful control, but large portions of it suck. `GridView` could be considered a retake of Windows Phone's Panorama control - it provides a horizontally scrolled plane for content, and `GridView` specifically adds support for enumerable content, usually in the form of tiles.

![](/images/postimages/gridview.png)

While this may not be blindingly obvious just yet, hopefully the below sections cover each of the areas of interest in how to create a 'hub page'

##Grouping
`GridView` comes with the cleverly named `GridViewItem`. In an ideal world, you would be able to do:

	<GridView>
		<GridViewItem Header="Hi"> 
 			<!-- static/non databound controls here -->
		</GridViewItem>
		<GridViewItem Header="Items" ItemsSource="{Binding Items1}" />
		<GridViewItem Header="Additional Items" ItemsSource="{Binding Items2}" />
	</GridView>

This would let you build a very rich, flexible 'hub' view, with an awesome design time experience. *Unfortunately*, this is not an ideal world - you have two options - all 'static' or all databound. 

In the 'static' approach, yes, you could have an `ItemsControl` of some sort providing the databound goodness you need, but I'll stop you right there. It'll suck until you embed it inside... another `GridView`, and even then you'll be fighting other issues which are only solved by grouping.

The *Grid App (XAML)* template in Visual Studio 2012 and Blend provides a basic example of grouping, but it can be distilled into

* "Groups" is an `IEnumerable<Group>`
* "Group" is a poco with an `IEnumerable<Item>`, with a title and any other random properties you want to throw on there
* "Item" is whatever you want it to be, generally the model or viewmodel of the data you want to present.

The minimum useful `Group` looks like

    public class Group<T> : BindableBase, IGroup
    {
        private readonly ObservableCollection<T> _items = new ObservableCollection<T>();
        private string _title = string.Empty;

        public Group(string title)
        {
            Title = title;
        }

        public string Title
        {
            get { return _title; }
            set { SetProperty(ref _title, value); }
        }

        public ObservableCollection<T> Items
        {
            get { return _items; }
        }
    }
    
    public interface IGroup
    {
    	string Title {get; set;}
    }

### But how do I get my 'static' hub?
Thats all well and good for the databound content you wanted to present, but what about the 'static' portions? When adding to the Groups collection, create a new class ("MyHub", implementing IGroup), and then you can use a `ContainerStyleSelector`. The `ContainerStyleSelector` on `GridView` is a `StyleSelector` that lets you choose which style/template is going to be used on a *group-by-group* basis. Add in a dummy group, then you can make your `StyleSelector` choose the appropriate static UI for that group.

	public class ContainerStyleSelector : StyleSelector
	{
	    public Style VariationsTemplate { get; set; }
	
	    public Style DefaultTemplate { get; set; }
	
	    public Style LigatureTemplate { get; set; }
	
	    protected override Style SelectStyleCore(object item, DependencyObject container)
	    {
	        var group = (ICollectionViewGroup)item;
	        if (group != null)
	        {
  			var g = ((IGroup)group.Group);
                	if (g is Variation)
                    	return VariationsTemplate;
	        }
	        return DefaultTemplate;
	    }
	}

While this is a crude example, it shows how you can switch the template on whatever property you want. The easiest way to get design time experience is to put your 'hubs' in separate UserControls.


        <Style x:Key="AnatomyOfButtons" TargetType="GroupItem">
            <Setter Property="IsTabStop" Value="False"/>
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="GroupItem">
                        <Border HorizontalAlignment="Left" 
                        BorderBrush="{TemplateBinding BorderBrush}" 
                        BorderThickness="{TemplateBinding BorderThickness}" 
                        Background="{TemplateBinding Background}" 
                        Margin="0,0,20,0">
                            <Views:ButtonAnatomy />
                        </Border>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>

##Item Styling
If you override the styles for each list control inside your groups, it ignores the ItemTemplate, as thats reserved/set on the GridView itself. Much like the container, you need to provide an ItemTemplateSelector
	
	    public class ItemSelector : DataTemplateSelector
	    {
	        public DataTemplate AddAlbumTemplate { get; set; }
	        public DataTemplate AlbumTemplate { get; set; }
	        public DataTemplate PopularTemplate { get; set; }
	        
	        protected override DataTemplate SelectTemplateCore(object item, DependencyObject container)
	        {
	            if (item is AddAlbum)
	                return AddAlbumTemplate;
	            if (item is Album)
	                return AlbumTemplate;
	            if (item is PopularImage)
	                return PopularTemplate;
	
	            return base.SelectTemplateCore(item, container);
	        }
	    }
	    
You can then use it in XAML:

	<Page.Resources>
        <DataTemplate x:Key="PopularTemplate">
            <Grid Width="277" Height="155" >
                <Image Source="{Binding Thumb}" Stretch="UniformToFill" />
            </Grid>
        </DataTemplate>
	    
        <Common:ItemSelector x:Key="ItemSelector" 
                             AddAlbumTemplate="{StaticResource AddAlbumTemplate}"
                             AlbumTemplate="{StaticResource AlbumTemplate}"
                             PopularTemplate="{StaticResource PopularTemplate}"/>
	</Page.Resources>
	<GridView ... ItemTemplateSelector="{StaticResource ItemSelector}" />

##Item limitations 
A `GridView` can handle roughly 1000[^2] items before you'll hit a fantastic exception
> Not enough quota is available to process this command. (Exception from HRESULT: 0x80070718)

If you're like me, you'll scratch your head for awhile and try and interpret that. The best I can determine, there is a maximum size (ie, quota) of the *animation* queue and animations are queued/triggered when you add items to any sort of `ItemsControl`. The animation lasts for less than half a second, but when your `GridView` suddenly goes from 0 to 1000, it gets overwhelmed. 

###Solution: "Drip feed loading"
Because there needs to be some processing of data -> group -> GridView databinding, the intermediate step to grouping can be put on a timer. Every 50ms or so, add another 200 items to the group. You may need to tweak the delay or number of items being added based on display template complexity.

##Memory limitations
#### Alternative title, 'Performance enhancing groups'
If you have 1000 items, 10 groups of 100 items performs considerably worse than 100 groups at 10 items. Seems bizarre, doesn't it? There is a good reason[^1], though. In `GridView`, the *groups* are virtualised but the *items* cannot be.   

Once you hit 200MB memory usage in WinRT, you start noticing random pausing and slowdowns. At 450-480MB, you start noticing random crashes - some will throw you out of the app, others will just lock it up indefinitely.  

**Without virtualising your items into multiple groups, at >500 items you're almost guaranteed to start hitting these limits.**  

###Solution #1: Just group it
Set an arbitrary number that you're happy with the performance of, and go with that. Performance will vary based on the complexity of your item templates - the more elements inside the template, the fewer items per group before things get a bit funky.  

Remember that drip feeding before? Every time it hits the 'tick', make it add in a new group. This might mean you need to change from every 20ms to 50ms, but things should still be considerably faster.

###Solution #2: Group it, but fake it.
There isn't really an alternate solution, but there are ways to make it appear more seamless.  You'll still need your 100 groups of 100 items, but by customising the template you can remove the border and the header. The first group should still have the header, but the rest can remove it. 

## The Complete Solution
![](/images/postimages/groupstyle_example.png)

Well, this is a complete-ish solution, with all the relevant classes for generating the above picture.

    public class ButtonAnatomyGroup : IGroup
    {
        public string Title { get; set; }
    }
    
    public class ButtonGroup : Group<string>
    {
        public ButtonGroup(string title)
            : base(title)
        {

        }
    }
    
    public interface IGroup
    {
        string Title { get; set; }
    }

    public class Group<T> : BindableBase, IGroup
    {
        private ObservableCollection<T> _items = new ObservableCollection<T>();
        private string _title = string.Empty;

        public Group(string title)
        {
            Title = title;
        }

        public string Title
        {
            get { return _title; }
            set { SetProperty(ref _title, value); }
        }

        public ObservableCollection<T> Items
        {
            get { return _items; }
            set { _items = value; }
        }
    }
    
    public class ContainerStyleSelector : StyleSelector
    {
        public Style AnatomyOfButtons { get; set; }

        public Style DefaultTemplate { get; set; }
        
        protected override Style SelectStyleCore(object item, DependencyObject container)
        {
            var group = (ICollectionViewGroup)item;
            if (group != null)
            {
                var g = ((IGroup)group.Group);
                if (g is ButtonAnatomyGroup)
                    return AnatomyOfButtons;
            }

            return base.SelectStyleCore(item, container);
        }
    }

Then in the Loaded event for the Page...

	var bg = new ButtonGroup("Buttons") { Items = new ObservableCollection<string>() };
	/* Load button information*/
	var groups = new ObservableCollection<IGroup> { new ButtonAnatomyGroup(), bg };
	DefaultViewModel["Groups"] = groups;
		
		
And on the XAML front, in Page.Resources

	<DataTemplate x:Key="ButtonTemplate">
		<Button Style="{Binding Converter={StaticResource ButtonStyleConverter}}" />
	</DataTemplate>
	
	<Style x:Key="AnatomyOfButtons" TargetType="GroupItem">
		<Setter Property="IsTabStop" Value="False"/>
		<Setter Property="Template">
			<Setter.Value>
				<ControlTemplate TargetType="GroupItem">
					<Border HorizontalAlignment="Left" 
						BorderBrush="{TemplateBinding BorderBrush}"
						BorderThickness="{TemplateBinding BorderThickness}" 
						Background="{TemplateBinding Background}" 
						Margin="0,0,20,0">
					    <Views:ButtonAnatomy />
					</Border>
				</ControlTemplate>
			</Setter.Value>
		</Setter>
	</Style>
	
	<Style x:Key="DefaultTemplate" TargetType="GroupItem">
		<Setter Property="IsTabStop" Value="False"/>
		<Setter Property="Template">
			<Setter.Value>
				<ControlTemplate TargetType="GroupItem">
				<Border 
					BorderBrush="{TemplateBinding BorderBrush}" 
					BorderThickness="{TemplateBinding BorderThickness}" 
					Background="{TemplateBinding Background}">
				    <Grid>
				        <Grid.RowDefinitions>
				            <RowDefinition Height="Auto"/>
				            <RowDefinition Height="*"/>
				        </Grid.RowDefinitions>
				        <ContentControl
					        x:Name="HeaderContent" 
					        ContentTemplate="{TemplateBinding ContentTemplate}" 
					        ContentTransitions="{TemplateBinding ContentTransitions}"
					        ContentTemplateSelector="{TemplateBinding ContentTemplateSelector}" 
					        Content="{TemplateBinding Content}" 
					        IsTabStop="False" 
					        Margin="{TemplateBinding Padding}" 
					        TabIndex="0"/>
				        <ItemsControl 
					        x:Name="ItemsControl"
					        IsTabStop="False" 
					        ItemsSource="{Binding GroupItems}" 
					        Grid.Row="1" 
					        TabIndex="1"
					        TabNavigation="Once" />
				    </Grid>
				</Border>
				</ControlTemplate>
			</Setter.Value>
		</Setter>
	</Style>
	
	<local:ContainerStyleSelector 
		x:Key="ContainerStyleSelector"
		AnatomyOfButtons="{StaticResource AnatomyOfButtons}"  
		DefaultTemplate="{StaticResource DefaultTemplate}"/>

	<CollectionViewSource
		x:Name="groupedItemsViewSource"
		Source="{Binding Groups}"
		IsSourceGrouped="true"
		ItemsPath="Items"/>

And finally, the GridView itself

    <GridView
        x:Name="itemGridView"
        AutomationProperties.AutomationId="ItemGridView"
        AutomationProperties.Name="Grouped Items"
        Grid.RowSpan="2"
        ItemsSource="{Binding Source={StaticResource groupedItemsViewSource}}"
        Padding="116,137,40,46"
        SelectionMode="None"
        IsSwipeEnabled="false"
        ItemTemplate="{StaticResource ButtonTemplate}"
        IsItemClickEnabled="True">

        <GridView.ItemsPanel>
            <ItemsPanelTemplate>
                <VirtualizingStackPanel Orientation="Horizontal"/>
            </ItemsPanelTemplate>
        </GridView.ItemsPanel>
        <GridView.GroupStyle>
            <GroupStyle ContainerStyleSelector="{StaticResource ContainerStyleSelector}">
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <Grid Margin="1,0,0,6">
                            <Button AutomationProperties.Name="Group Title" Style="{StaticResource TextPrimaryButtonStyle}">
                                <StackPanel Orientation="Horizontal">
                                    <TextBlock Text="{Binding Title}" Margin="3,0,10,10" Style="{StaticResource GroupHeaderTextStyle}" />
                                    <TextBlock Text="{StaticResource ChevronGlyph}" FontFamily="Segoe UI Symbol" Margin="0,0,0,10" Style="{StaticResource GroupHeaderTextStyle}"/>
                                </StackPanel>
                            </Button>
                        </Grid>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
                <GroupStyle.Panel>
                    <ItemsPanelTemplate>
                        <VariableSizedWrapGrid Orientation="Vertical" Margin="0"/>
                    </ItemsPanelTemplate>
                </GroupStyle.Panel>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>


[^1]: For various definitions of 'good'.
[^2]: You could probably argue that 'metro' designs aren't suitable for that many items, and in many cases you would be right. In this particular case, I'm creating a character map - some fonts have 3000 glyphs. 