---
title: 'Windows Store Hello World App'
description: 'Before “Windows 8 Developers Workshop – Techdays” in Microsoft Turkey, I wanted to have more knowledge about Metro Style apps and share it with you guys. In this tutorial we will learn how to create a Project and how to handle with Click/Touch.. events.'
pubDate: 2013-09-09T17:04:00.000Z
heroImage: '../../assets/Hello-World-1024x295.png'
---

Before “Windows 8 Developers Workshop – Techdays” in Microsoft Turkey, I wanted to have more knowledge about Metro Style apps and share it with you guys. In this tutorial we will learn

How to:
- Create a Project
- Handle with Click/Touch.. events.

We will use Visual Studio 2012, you can able to get **Visual Studio Express 2012 for Windows 8** free of charge from this [link](http://msdn.microsoft.com/en-us/windows/apps/hh852659.aspx).

Here we go..

Let”s open Visual Studio 2012. Start page fronts us at the first hand.[![](http://i.imgur.com/L0wU0.jpg)](http://i.imgur.com/L0wU0.jpg)

If you do not see any Start Page, you can enable it by selecting from View tab on the toolbar. As you can see, there are many useful things on start page. You can access the free resources from MSDN and such. Now, we are going to select New Project..[![](http://i.imgur.com/pe7l9.jpg)](http://i.imgur.com/pe7l9.jpg)

We have to select Windows Store and then Blank App. You can name your app and change the location of contents of the app. Click OK button.[![](http://i.imgur.com/TAZjr.jpg)](http://i.imgur.com/TAZjr.jpg)

Now we are able to see `MainPage.xaml` and `App.xaml` pages in Solution Explorer window(If you don’t see the Solution Explorer Window, you can enable it from View tab). Alright, `MainPage.xaml` is the page which you see firstly when you run your app. `App.xaml` is where you express resources which are used in the app.

We are going to replace our Blank Page( We selected it while creating a new project ) into Basic Page, because Basic Page gives us chance to change style of our app ( Includes some helper classes ).

Replacing Main Page
- In the Solution Explorer window, find `MainPage.xaml` file, right click and delete it.
- In the Solution Explorer window, right click on your project file Add->New Item [![](http://i.imgur.com/zynEi.jpg)](http://i.imgur.com/zynEi.jpg)

- Select Blank Page, name it as `MainPage.xaml`, then click add. ![](http://i.imgur.com/D3TT2.jpg)
- Click Yes &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  ![](http://i.imgur.com/dVxAH.jpg)

Now you can see the additional classes right below the Common folder in solution explorer. Then if you press F5 button, you will get the same as below.

[![](http://i.imgur.com/dUIRT.jpg)](http://i.imgur.com/dUIRT.jpg) <img src="http://i.imgur.com/Bk2RP.jpg" alt="" width="50%" style="float:right;">

Stop Debugging..

While you are taking a look at codes both in `MainPage.xaml.cs` and `App.xaml.cs`, you may ask the code what `InitializeComponent()` does in the our constructor method. Well, this code  combines your design page ( `xaml` ) with your code page ( `xaml.cs` ). After all, it makes you be able to see the `MainPage.xaml`.

App.xaml :

```xml
<Application    x:Class="HelloWorldApp.App"
	xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	xmlns:local="using:HelloWorldApp">
	<Application.Resources>
		<ResourceDictionary>
			<ResourceDictionary.MergedDictionaries>
				<!--  Styles that define common aspects of the platform look and feel Required by Visual Studio project and item templates -->
				<ResourceDictionary Source="Common/StandardStyles.xaml"/>
			</ResourceDictionary.MergedDictionaries>
		</ResourceDictionary>
	</Application.Resources>
</Application>
```

As you can see StandardStyles.xaml is declared as source of ResourceDictionary. This is the style that makes our app as Metro Style.

Let's modify our Main Page

– Find the `x:String` code in `MainPage.xaml` . Now you can change “My Application” into whichever you want. I typed Hello World

```xml
<Page.Resources>
	<!-- TODO: Delete this line if the key AppName is declared in App.xaml -->
	<x:String x:Key="AppName">Hello World!</x:String>
</Page.Resources>
```

Paste this code just before <VisualStateManager.VisualStateGroups> tag.

```xml
<StackPanel Grid.Row="1">            
	<Button x:Name="btnHelloWorld" Content="Say Hello" HorizontalAlignment="Center" VerticalAlignment="Center"/>
	<TextBlock x:Name="outputHelloWorld" Text="" HorizontalAlignment="Center" VerticalAlignment="Center"></TextBlock>
</StackPanel>
```

Press F5 button. You will see the same page like the picture below. If you click the Say Hello button, you see that doesn’t work. It is because we haven’t assigned any event into the button.[![](http://i.imgur.com/ySOd9.jpg)](http://i.imgur.com/ySOd9.jpg)

Stop Debugging..

Now  tap the button, then go to properties window and click  ![](http://i.imgur.com/fHTw8.jpg) , double click the highlighted place ![](http://i.imgur.com/UUi1k.jpg).  We are in the method of related event.

```csharp
private void btnHelloWorld_Click(object sender, RoutedEventArgs e) {
  outputHelloWorld.Text = "Hello World!";
}
```

We can run our application now by pressing F5.[![](http://i.imgur.com/rD1TL.jpg)](http://i.imgur.com/rD1TL.jpg)

Our Textblock is visible ,but you may not like its font size. We will edit it by ourselves 😉

Editing The Style

Firstly there are two themes, light and dark. By default the dark one is active. We can adding the following code into our App.xaml page.

<Application    x:Class="HelloWorldApp.App"    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"   xmlns:local="using:HelloWorldApp"    RequestedTheme="Light">

Copy the last line, and paste into your code like as above. Then press F5 to see the changes.

[![](http://i.imgur.com/yLiNl.jpg)](http://i.imgur.com/yLiNl.jpg)

Let’s change the style of the TextBlock. Remember we mentioned before about StandardStyles.xaml . The text is not suitable enough for comfortable  reading. We will increase the font size, but how?..

Ok, select the TextBlock from design. You may not be able to select it from MainPage.xaml ‘ s design.

– Find the text block we added it before. Double click on it.

– Go to properties window,  expand Miscellaneous.

– Click the little square next to Style -> Local Resources -> BasicTextStyle

[![](http://i.imgur.com/0vUIQ.jpg)](http://i.imgur.com/0vUIQ.jpg)

If you debug it, you can see that the text is more like Metro UI  when you click the Say Hello button.There is a code occured after these in the TextBlock in MainPage.xaml.

<TextBlock x:Name="outputHelloWorld" Style="{StaticResource BasicTextStyle}"></TextBlock>

Let”s create our own style

Make sure that you select the TextBlock and go to Properties -> Miscellaneous

Click the little square beside the Style -> Convert to New Resource

![](http://i.imgur.com/kcm8P.jpg)

Name it, select Application, then click OK.

Style="{StaticResource TextBlockBlueStyle}

You can see the changes in TextBlock. As you see there is StaticResource. When you  see StaticResource, remember the resources in App.xaml page. Now, open the App.xaml change the parameters as you wish. I did it like this :

</ResourceDictionary.MergedDictionaries>    <Style x:Key="TextBlockBlueStyle" TargetType="TextBlock">        <Setter Property="Foreground" Value="Blue"/>        <Setter Property="FontSize" Value="40"/>
<Setter Property="FontFamily" Value="{StaticResource ContentControlThemeFontFamily}"/>        <Setter Property="TextTrimming" Value="WordEllipsis"/>        <Setter Property="TextWrapping" Value="Wrap"/>        <Setter Property="Typography.StylisticSet20" Value="True"/>        <Setter Property="Typography.DiscretionaryLigatures" Value="True"/>        <Setter Property="Typography.CaseSensitiveForms" Value="True"/>    </Style>
</ResourceDictionary>

Press F5 to see what you did 🙂

You will see your screen close to the image below.

[![](http://i.imgur.com/lqfNw.jpg)](http://i.imgur.com/lqfNw.jpg)

We are done for this post 🙂 I hope you enjoyed with what you learned from this tutorial.

[DOWNLOAD THE CODE](http://sdrv.ms/PPHkuH)