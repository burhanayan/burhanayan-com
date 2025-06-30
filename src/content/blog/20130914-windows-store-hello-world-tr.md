---
title: "Windows 8 #1 – Merhaba Dünya Uygulaması"
description: 'Bildiğiniz gibi Windows 8 RTM yeni piyasaya sürüldü. Çeşitli yeniliklerle beraber geldi. Windows 8”in önceki sürümleri olsun, bu konu hakkında verilen seminerler olsun, ufak tefek kulak aşinalığınız oluşmuştur. Bu yeni işletim sisteminde her ne kadar Windows masaüstü kısmında belli bir takım değişiklikler olmuş olsa da, asıl odaklanılan kısım Silverlight tarafı. Okuyacağınız makalede Silverlight tarafına Metro Stili”nde uygulama geliştireceğiz'
pubDate: 2013-09-14T17:04:00.000Z
heroImage: '../../assets/Hello-World-1024x295.png'
---

Bildiğiniz gibi Windows 8 RTM yeni piyasaya sürüldü. Çeşitli yeniliklerle beraber geldi. Windows 8”in önceki sürümleri olsun, bu konu hakkında verilen seminerler olsun, ufak tefek kulak aşinalığınız oluşmuştur. Bu yeni işletim sisteminde her ne kadar Windows masaüstü kısmında belli bir takım değişiklikler olmuş olsa da, asıl odaklanılan kısım Silverlight tarafı. Okuyacağınız makalede Silverlight tarafına Metro Stili”nde uygulama geliştireceğiz.Bu yazıdan sonra:

*   Yeni bir proje nasıl oluşturulur,
*   Click/Touch olayları nasıl kontrol edilir, hakkında bilgi sahibi olacaksınız.

Uygulamayı yaparken kullanacağımız platform Visual Studio 2012 olacak. Ayrıca bilgisayarınızda Windows 8 kurulu olması gerekiyor.

*   Windows 8 Release Preview için [buraya tıklayınız](http://windows.microsoft.com/en-US/windows-8/download?SignedIn=1).
*   Visual Studio Express 2012 for Windows 8 için [buraya tıklayınız](http://msdn.microsoft.com/en-us/windows/apps/hh852659.aspx).

### Windows 8 Sistem Gereksinimleri

Windows 8 Release Preview, Windows 7’yi çalıştıran makinalarda sorunsuz çalışmaktadır:

* İşlemci: 1 gigahertz (GHz) ya da daha fazla
* RAM: 1 gigabyte (GB) (32-bit) ya da 2 GB (64-bit)
* Hard disk alanı: 16 GB (32-bit) ya da 20 GB (64-bit)
* Grafik kartı: WDDM sürücüsü içeren Microsoft DirectX 9 Grafik kartı

### Visual Studio Express 2012 for Windows 8 Sistem Gereksinimleri

Destekleyen işletim sistemleri: Windows 8  
Destekleyen mimariler: 32-bit (x86) or 64-bit (x64)  
Donanım Gereksinimleri:
* 1.6 GHz ya da daha hızlı bir işlemci
* 1 GB of RAM ( Sanal makinada çalışıyorsa 1.5 GB )
* 5.0 GB boş hard disk alanı
* 5400 RPM hard disk sürücü
* DirectX 9-destekli, 1024 x 768 ya da daha yüksek çözünürlüklü grafik kartı

İlk olarak yapıcağımız şey, Visual Studio Express 2012 for Windows 8 programını açıyoruz. Ben bu uygulamayı oluştururken Visiual Studio 2012 Ultimate kullandım. Sorun değil, yapıcağımız uygulamada herhangi bir fark olmayacak.

[![](http://i.imgur.com/L0wU0.jpg)](http://i.imgur.com/L0wU0.jpg)

Görüğünüz gibi eski Visual Studio’lara benzer bir görünümü var nesnelerin yerleri açısından. Fakat tasarım çok farklı. Yapılırken daha çok Metro Stili’ne uygun olmasına dikkat edilmiş.

Eğer yukarıdaki ekran çıktısı gibi gelmiyorsa ekranınız,  View -> Start Page ”inizi açın.

Buradan **New Project** diyerek yeni bir proje oluşturacağız.

[![](http://i.imgur.com/pe7l9.jpg)](http://i.imgur.com/pe7l9.jpg)

**Templates -> Visual C# -> Windows Store ->Blank App**‘ı seçerek, **OK** ‘e tıklıyoruz. Dilerseniz kaydedeceğiniz yeri değiştirir, projenize yeni isim verebilirsiniz.

[![](http://i.imgur.com/TAZjr.jpg)](http://i.imgur.com/TAZjr.jpg)

**Solution Explorer**‘ımızda MainPage.xaml ve App.xaml sayfalarını görmekteyiz(Eğer Solution Explorer”ınız açık değilse, **View** sekmesinden bu özelliği açabilirsiniz). Kısaca açıklamak gerekirse:

- `MainPage.xaml` sayfası uygulamamız çalıştığında ilk ekrana gelen sayfadır.
- `App.xaml` sayfası ise kaynakların bulunduğu sayfadır. Eğer uygulamanın her sayfasından ulaşılabilir bir nesne kullanacaksak, kodlarımızı bu sayfaya yazacağız.

Şimdiki adım Blank Page ( Hatırlarsanız, projemizi oluştururken seçmiştik) ‘imizi Basic Page’e dönüştürmüş olacak. Böylelikle Basic Page ile gelen yardımcı sınıflar sayesinde uygulamamızda stil değişiklikleri yapabileceğiz.

Main Page”in dönüştürülmesi

- **Solution Explorer** penceresinde, `MainPage.xaml`dosyasını sağ klik yapıp **Delete**”i seçerek silin.
- Yine **Solution Explorer** penceresinden projenize sağ klik yaparak **Add** -> **New Item**”i seçin.[![](http://i.imgur.com/zynEi.jpg)](http://i.imgur.com/zynEi.jpg)
- **Blank Page**”i seçtikten sonra, sayfanızı `MainPage.xaml`olarak adlandırıp, **Add** butonuna tıklıyoruz.![](http://i.imgur.com/D3TT2.jpg)
- Bir uyarı gelecektir. Yes butonuna tıklayıp bu adımı da bitiriyoruz. ![](http://i.imgur.com/dVxAH.jpg)

Şimdi Common dosyasını genişletirseniz, eklenen yeni sınıfları görüyor olmalısınız.  
![](http://i.imgur.com/dUIRT.jpg)

F5'e tıkladığınızda benzer görüntüyü alacaksınız.  
![](http://i.imgur.com/Bk2RP.jpg)

Çalışan projenizi Stop Debugging diyerek durdurabilirsiniz.

Bu arada `MainPage.xaml.cs` ve `App.xaml.cs` sayfalarına baktığınzda yapıcı metod içerisinde `InitializeComponent()` metodunun ne işe yaradığını merak edebilirsiniz. Bu metod dizayn sayfasını ( `xaml` ) C Sharp kodlarını yazığınız sayfayla ( `xaml.cs` ) birleştirmeye yarar. Böylelikle kolayca dizayn tarafından sürükle bırakla attığımız bileşenleri kod bölümünden (.cs) direk yönetebiliriz.

`App.xaml` :

```xml
<Application    x:Class="HelloWorldApp.App"    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"   xmlns:local="using:HelloWorldApp">
   <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
               <!-- 
                    Styles that define common aspects of the platform look and feel
                    Required by Visual Studio project and item templates\\r\\n                 -->
                <ResourceDictionary Source="Common/StandardStyles.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Sizinde görüdüğünüz gibi `StandardStyles.xaml` kaynak olarak belirtilmiş. Bu stil bizim Metro Stili'nde uygulama geliştirmemize olanak sağlayacaktır.

Şimdi isterseniz ana sayfamızda (`MainPage.xaml`) değişiklikler yapalım:
- MainPage.xaml içerisinde `x:String`'i bulun ( Ctrl+F yaparak daha hızlı bir şekilde bulabilirsiniz) . Sonrasında “**My Application**” yazısını değiştirelim. Dilediğiniz başlığı verebilirsiniz. Ben Hello World! yazdım.

```xml
<Page.Resources>
    <!-- TODO: Delete this line if the key AppName is declared in App.xaml -->
    <x:String x:Key="AppName">Hello World!</x:String>
</Page.Resources>
```

Aşağıdaki kodu `<VisualStateManager.VisualStateGroups>` etiketinin hemen öncesindeki boşluğa yapıştırın.

```xml
<StackPanel Grid.Row="1">
    <Button x:Name="btnHelloWorld" Content="Say Hello" HorizontalAlignment="Center" VerticalAlignment="Center"/>
    <TextBlock x:Name="outputHelloWorld" Text="" HorizontalAlignment="Center" VerticalAlignment="Center"></TextBlock>
</StackPanel>
```

Klavyeden F5 butonuna basıp uygulamanızı çalıştırın. Aşağıdaki resme benzer bir görüntüyle karşılaşacaksınız. Eğer **Say Hello** butonuna tıklarsanız, çalışmayacaktır. Çünkü daha butonumuza herhangi bir olay atamadık.[![](http://i.imgur.com/ySOd9.jpg)](http://i.imgur.com/ySOd9.jpg)Şimdi uygulamamızı Alt+Tab yaptıktan sonra, **Stop Debugging** diyerek durduralım.

Bundan sonra xaml sayfasının dizayn tarafında butona bir kez tıklayarak seçelim. Bu seçim işlemi xaml sayfasının kod bölümünden ilgili kod satırını bulduğumuzda **Button** yazısı üzerine çift tıklayarak da yapabiliriz. Properties penceresinden ![](http://i.imgur.com/fHTw8.jpg) simgesine tıkladıktan sonra, ![](http://i.imgur.com/UUi1k.jpg) **Click** olayının belirtildiği satırdaki textbox’a çift tıklayın ve aşağıdaki kodu ilgili metoda aşağıdaki gibi ekleyin.

```csharp
private void btnHelloWorld\_Click(object sender, RoutedEventArgs e)
{
     outputHelloWorld.Text = "Hello World!";
}
```

Uygulamayı F5'e basarak çalıştıralım.

[![](http://i.imgur.com/rD1TL.jpg)](http://i.imgur.com/rD1TL.jpg)

Eklediğimiz `TextBlock` görünür oldu, ama fontumuzun boyutu sizi tatmin etmemiş olabilir. Birazdan fontumuzda değişiklikleri kendi başımıza yapacağız. 😉

### Stili Düzenlemek

Metro Stil ile gelen koyu ve açık olmak üzere iki tane tema vardır. Default olarak koyu olan aktiftir. Aşağıdaki satırı ( `RequestedTheme="Light"` ) App.xaml içerisine eklersek uygulamamızın  temasını açık renk olarak ayarlayabiliriz.

```xml
<Application
    x:Class="HelloWorldApp.App"   xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorldApp"
    RequestedTheme="Light">
```

Kodu eklediğimize göre şimdi uygulamamızdaki değişiklikleri görmek için F5'e basabiliriz.

[![](http://i.imgur.com/yLiNl.jpg)](http://i.imgur.com/yLiNl.jpg)

Şimdi dilerseniz `TextBlock`'umuzu  daha okunur bir hale getirelim. Hatırlarsanız `StandardStyles.xaml`'den bahsetmiştik makalemizin başlarında. Biz `StandardStyles.xaml` gibi olacak şekilde kendi stilimizi oluşturacağız. Fontumuzun boyutunu arttırmak istiyoruz, ama nasıl ?..

Pekala şimdi:

1.  **TextBlock**'umuzu seçelim.
2.  **Properties** penceresine gidip, **Miscellaneous**'u genişletelim
3.  **Style** yazılı satırın sonundaki küçük kareye tıklayıp, **Local Resources -> BasicTextStyle**'ı seçelim.

Bunları yaptıktan sonra eğer uygulamamızı debug ederseniz, butona tıklandığında çıkan TextBlock'un Metro UI'a benzediğini göreceksiniz.

Yaptığımız stil değişikliklerinden sonra `MainPage.xaml`içerisindeki **TextBlock**'umuzun kodunun içerisine stilimiz, **BasicTextStyle** olarak bağlandı.

```xml
<TextBlock x:Name="outputHelloWorld" Style="{StaticResource BasicTextStyle}"/>
```

### Kendi Stilinimizi Oluşturalım

**TextBlock**'un seçili olduğundan emin olduktan sonra **Properties -> Miscellaneous**'ı seçin.  
**Style**'ın bulunduğu satırdaki küçük kareye tıklayın. **Style -> Convert to New Resource**'u seçin.

![](http://i.imgur.com/kcm8P.jpg)

İsimlendirdikten sonra, **Application**'ı seçip **OK**'e tıklıyoruz.

```xml
Style="{StaticResource TextBlockBlueStyle}
```

`MainPage.xaml` içerisinde `TextBlock`'a baktığınızda değişiklikleri görüyor olacaksınız. Sizin de gördüğünüz gibi, `StaticResource` stile bağlanmıştır. Bu arada `StaticResource` gördüğünüz zaman, aklınıza `App.xaml` sayfasındaki `Resources` etiketleri gelsin. Şimdi, `App.xaml` içerisine giderek, parametreleri istediğimiz şekilde değiştiriyoruz. Ben şu şekilde değiştirdim :

```xml
</ResourceDictionary.MergedDictionaries>
    <Style x:Key="TextBlockBlueStyle" TargetType="TextBlock">
        <Setter Property="Foreground" Value="Blue"/>
        <Setter Property="FontSize" Value="40"/>
        <Setter Property="FontFamily" Value="{StaticResource ContentControlThemeFontFamily}"/>
        <Setter Property="TextTrimming" Value="WordEllipsis"/>
        <Setter Property="TextWrapping" Value="Wrap"/>
        <Setter Property="Typography.StylisticSet20" Value="True"/>
        <Setter Property="Typography.DiscretionaryLigatures" Value="True"/>
        <Setter Property="Typography.CaseSensitiveForms" Value="True"/>
    </Style>
</ResourceDictionary>
```

F5'e basarak sayfamızı ne hale getirdiğimize bir bakalım 😀

Eğer benimle aynı kodları yazdıysanız alacağınız görüntü aşağıdaki gibi olacaktır.[![](http://i.imgur.com/lqfNw.jpg)](http://i.imgur.com/lqfNw.jpg)

Bu makalemizin de sonuna gelmiş bulunmaktayız. Umarım makaleden beklediğinizi bulabilmişsinizdir. Biraz olsun bir şey öğrendiyseniz ne mutlu bana. Bir sonraki makalede görüşmek üzere 🙂