---
title: "Windows 8 #2 – Uygulama Yaşam Döngüsü ve Durum Yönetimi"
description: "Windows 8'de, sistemin yavaşlaması ve batarya kullanımını dert etmeden bir sürü uygulamayı aynı anda açabilirsiniz. Windows 8 işletim sistemi, sizin için arkaplanda, uygulamaların hazır bekletilmesine olanak tanır. İyi bir Windows 8 uygulaması geliştirirken, uygulamanın bekletilmesi, beklemeden sonra tekrar çalıştırılabilmesi, kapanması durumlarını göz önünde bulundurmak gerekiyor."
pubDate: 2013-09-15T17:04:00.000Z
heroImage: '../../assets/AppLifeCycle-1024x295.png'
---

Windows 8'de, sistemin yavaşlaması ve batarya kullanımını dert etmeden bir sürü uygulamayı aynı anda açabilirsiniz. Windows 8 işletim sistemi, sizin için arkaplanda, uygulamaların hazır bekletilmesine olanak tanır. İyi bir Windows 8 uygulaması geliştirirken, uygulamanın bekletilmesi, beklemeden sonra tekrar çalıştırılabilmesi, kapanması durumlarını göz önünde bulundurmak gerekiyor.

Bu eğitimde:
*   Kaydetme durumunda(Save State), farklı depolama alanlarına kayıtların gerçekleştirilmesi,
*   Uygulamanın tekrar çalıştırılması durumunda, uygulamanın son durumunu korumayı öğreneceğiz.

Başlamadan önce…
*   [Windows 8 Merhaba Dünya Uygulaması](http://www.burhanayan.com/windows-8-merhaba-dunya-uygulamasi/)'na bakmanızı tavsiye ediyorum.
*   Bu bölümün kodlarını [buradan indirebilirsiniz](http://sdrv.ms/10lOqib).

### Uygulamanın Yaşam Döngüsü Hakkında

Kod kısmına geçmeden önce kısaca yaşam döngüsünden bahsedelim. Uygulamanın 3 tane eylemi bulunmaktadır. Bunlar Running(Çalışıyor), NotRunning(Çalışmıyor), bir de Suspended(Bekliyor).

![http://i.msdn.microsoft.com/dynimg/IC576232.png](http://i.msdn.microsoft.com/dynimg/IC576232.png)

Uygulamamız, başka bir uygulama başlatıldığında veya Windows düşük batarya durumuna geçtiğinde, “Bekliyor' konumuna geçebilir. Bu durumda hafızada uygulamaya belli yer ayrılır ki, tekrar uygulama açıldığında hızlıca kaldığı yerden devam edebilsin. Ayrıca çeşitli nedenlerle Windows'un kendisi de bu uygulamayı kapatabilir. Bunu yapmadan önce uygulamaya bildirir. Böylece uygulama kapanmadan önce verilerin kaybı sorunu çözülmüş olur.

Sonraki adımda _app data_ ve _session data_'yı nasıl güncelleyeceğimizi öğreneceğiz.

### 1. SuspensionManager Kullanımı

Merhaba Dünya uygulamasını yaparken kullandığımız _Basic Page_ şablonu, _Common_ klasörüne birkaç dosya eklemiştir. Bunlardan biri olan _SuspensionManager_ sayesinde uygulamamızın yaşam döngüsünü daha kolay bir biçimde kontrol edebilmekteyiz. Sayfalar arası geçiş sırasında, uygulamanın sayfalarını tutan _Frame_'in geçiş durumunu kaydetmek ve geri yüklemek, tek sayfaya sahip uygulamalar için bir önemi olmasa da, birden çok sayfaya sahip uygulamalarda çok önem kazanmaktadır. Bu sınıf, sayfa durumu verisini XML dosyası halinde, uygulamanın lokalinde barındırır.

_SuspensionManager_ sınıfını kullanabilmek için, ilk olarak ana _Frame_'i kaydetmek gerekiyor. Bu işlem tamamlandığında , bu sınıf, uygulamanın herbir sayfasından haberdar olacaktır. Bu kayıt işlemini gerçekleştirmek, _App.xaml.cs_ içindeki _OnLaunched_ metodunun oluşturulmasından hemen sonra olacaktır.

**Bu sınıfın kullanımı için;**

* _Solution Explorer_ penceresinden _App.xaml.cs_'e çift tıklayıp açıyoruz.
*  _OnLaunched_ metodunda aşağıdaki değişiklikleri yaparak, _Frame_'i kaydetmiş oluyoruz.

```csharp
if (rootFrame == null)
{
// Create a Frame to act as the navigation context and navigate to the first page
rootFrame = new Frame();
MerhabaDunya.Common.SuspensionManager.RegisterFrame(rootFrame, "appFrame");<br>
```

### 2.Uygulamanın Durumunun Kaydedilmesi

Uygulamamızda kontrol etmemiz gereken 2 tip veri çeşidi vardır: _app data_ ve _session data_. _App data_, _TextBox_'ımızın _Text_'idir. Uygulamanın bekleme moduna geçerken 5 saniyesi vardır. Bu süre içerisinde önemli _app data_'ları bir an önce kaydetmek gerekir.

_Windows.Storage.ApplicationData_ objesiyle _app data_'mızı yönetebiliriz. Bu obje, _ApplicationDataContainer'_i döndüren, _RoamingSettings_ özelliğine sahiptir.

**_App data_'yı kaydetmek için;**

* _MainPage.xaml_'a çift tıklıyoruz.
* Bir tane _TextBox_ sürükleyip bırakıyoruz ve _Name_ niteliğini belirliyoruz. Benim kodda _TextBox1_ olarak geçiyor.
* Properties penceresinde, olaylar butonuna![](http://i.imgur.com/fHTw8.jpg) tıklıyoruz.
* TextChanged olayının yanındaki _textbox_'a çift tıklıyoruz.
* Metodumuzda aşağıdaki şekilde _TextBox_'ımızın _Text_ niteliğini kaydediyoruz. Sonrasında F5'e basıp uygulamamızı çalıştırıyoruz. _TextBox_ içerisine yazdığınız isminiz kaydedilmiş olacaktır.

```csharp
Windows.Storage.ApplicationDataContainer dolasimAyarlari = Windows.Storage.ApplicationData.Current.RoamingSettings;
   dolasimAyarlari.Values["kullaniciAdi"] = TextBox1.Text;
```

_Session data_, ilgili kullanıcının o anki uygulamadaki oturumunun saklandığı geçici veridir. Bir oturum, kullanıcı kapatma hareketini yaptığında ya da Alt + F4 kısayolunu kullandığında, bilgisayarını tekrardan başlattığında, ya da bilgisayarı kapattığında sona erer. Bizim uygulamamızda ise, _greetingOutput TextBlock_'unun _Text_ özelliği _session data_'dır. Bu veriyi, Windows uygulamanızı beklettiğinde ya da sonlandırdığında yenileyebilirsiniz. Öncelikle bizim uygulamamızın _Frame_'inin navigasyon durumunu kaydetmeliyiz ki, bu sayede  uygulama uygulama açıldığında, kendini son kapatıldığı andaki sayfadan başlatabilsin ve _SuspensionManager_ hangi sayfanın durumunun tekrar yüklenmesi gerektiğini bilebilsin. Bununla birlikte sayfamızın kendisinin durumunu da kaydetmemiz gerekiyor.  Hatırladığınız gibi _greetingOutput_'un _Text_ özelliğini buraya kaydetmiştik.

**_Session data_'yı kaydetmek için;**

*   _App.xaml.cs_'i _Solution Explorer_ penceresinden bulup açıyoruz.
*   _App.xaml.cs_ içerisinde, _OnSuspending_ metodunun başına async anahtar kelimesini ekliyoruz.
*   _OnSuspending_ metodu içerisinde, _SuspensionManager.SaveAsync_ metodunu çağırıyoruz. _SaveAsync_ metodunu çağırmak, _Frame_'in navigasyon durumunu kaydeder ve sayfanızın içeriğinin kaydedilmesine olanak sağlar.

```csharp
async private void OnSuspending(object sender, SuspendingEventArgs e) {
    var deferral = e.SuspendingOperation.GetDeferral();
    //TODO: Save application state and stop any background activity
    await MerhabaDunya.Common.SuspensionManager.SaveAsync();
    deferral.Complete();
}
```

*   _MainPage.xaml.cs_ sayfasını _Solution Explorer_ penceresinden bulup çift tıklayın.
*   _MainPage.xaml.cs_ sayfasındaki _SaveState_ metodu içerisini aşağıdaki gibi düzenleyerek sayfanın durumunu kaydedin.

```csharp
protected override void SaveState(Dictionary<String, Object> pageState) {
    pageState["greetingOutputText"] = greetingOutput.Text;
}
```

*   Veriler _pageState_(_SaveState_ metodunun hemen üzerindeki .xml dosyasında tanımlanmıştır.) içerisinde sadece bu oturum için saklanmıştır. Yukarıdaki ifadede de _greetingOutput.Text_'i kaydetmiş olduk. Şimdi **Build -> Build Solution** yaparak uygulamamızda herhangi bir hata olmadığından emin olalım.

Uygulama sonlandırılmadan önce uygulamamızın durumunu kaydetmeyi öğrendik. Şimdi ise uygulamımızın tekrar çalıştırılması durumunda uygulamamızın durumu geri yüklemeyi öğreneceğiz.

### 3.Uygulamanın Durumunu Tekrar Yüklemek

Şimdi _App.xaml.cs_ dosyasına yani uygulamanın aktivasyonlarının yönetimine bir bakalım.  

Öncelikle, aşağıdaki kodda bir _Frame_'in tanımlandığını ve o andaki pencerenin içeriğine atandığını görüyoruz.

```csharp
Frame rootFrame = Window.Current.Content as Frame;
```

Tabi penceremiz bir _Frame_'e sahipse, bu tanımlamayı yapmaya gerek yoktur.

Aşağıdaki kodda uygulamanın sonki kapatılmasından önceki çalıştırılma durumunu kontrol ediliyor. Önceki çalıştırılma durumu Sonlandırıldı(_Terminated_) olması, Windows'un başarılı bir şekilde bekletilip(Suspend) daha sonra sonlandırıldığı anlamına gelmektedir. Bu durumda o zamanki uygulamanın durumu geri yüklenecektir.

```csharp
if (rootFrame == null) {
    // Create a Frame to act as the navigation context and navigate to the first page
    rootFrame = new Frame();
    MerhabaDunya.Common.SuspensionManager.RegisterFrame(rootFrame, "appFrame");
    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated) {
        //TODO: Load state from previously suspended application\\r\\n                }
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
}
```

Bir sonraki kod is _Frame_'in herhangi bir içeriğe sahip olup olmadığını kontrol edecektir. Eğer uygulama çalışıyor durumda is ya da navigasyon durumu geri yüklenmiş(_Restored_) ise, _Frame_'in içeriği mevcuttur. Değilse, _Frame_ kendisini uygulamanın ilk sayfasına yönlendirecektir. Yani bu örnekte _MainPage_'e yönlendirmiş olacak.

```csharp
if (rootFrame.Content == null) {
    // When the navigation stack isn't restored navigate to the first page,
    // configuring the new page by passing required information as a navigation
    // parameter
    if (!rootFrame.Navigate(typeof(MainPage), args.Arguments)) {
        throw new Exception("Failed to create initial page");
    }
}
```

Ve son olarak kodumuz, penceremizi aktif hale getirir.

```csharp
Window.Current.Activate();
```

Artık uygulama başlatıldığında neler olduğunu biliyoruz, şimdi de nasıl uygulamanın durumunu geri yükleriz ona bakalım.

#### Uygulamanın Durumunu Geri Yüklemek

* _App.xaml.cs_ içerisinde, _OnLaunched_ metodunun imza kısmına **async** anahtar kelimesini ekleyelim.
    ```csharp
    async private void OnSuspending(object sender, SuspendingEventArgs e)
    ```
    

* Uygulama sonlandırılmış ise, _SuspensionManager.RestoreAsync_ metodunu çağırmalıyız. Bu metodu çağırmak _Frame_'imizin navigasyon durumunu tekrardan yükleyip ve sonrasında sayfamızın(_Page_) içeriğinin tekrardan yüklenmesine olanak tanıyacaktır.
    ```csharp
    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated) {
        //TODO: Load state from previously suspended application
        await MerhabaDunya.Common.SuspensionManager.RestoreAsync(); 
    }
    ```

*   _MainPage.xaml.cs_ içerisinde, _LoadState_ metoduna, sayfa durumunun geri yüklenmesi için aşağıdaki kodu ekleyelim
* Öncelikle, greetingOutputText isimli anahtara sahip _pageState_ sözlüğü var mı yok mu kontol edelim. Eğer anahtar mevcutsa, _greetingOutput_'un _Text_'ini bu anahtar aracılığıyla geri yükleyelim.
    ```csharp
    if (pageState != null && pageState.ContainsKey("greetingOutputText"))
        greetingOutput.Text = pageState["greetingOutputText"].ToString();
    ```

* Sonra kullanıcı adını yükleyelim. Çünkü kullanıcı adı verisiyle birden fazla oturumun bilgisine ulaşıp, geri yüklenmesi işlemini gerçekleştireceğiz.  Bu veriyi de _RoamingSettings_ uygulama veri konteynerinde depoluyoruz. Şimdi kullanıcı adının var olup olmadığını kontrol edip, eğer varsa kullanıcı adını gösterelim.
    ```csharp
    Windows.Storage.ApplicationDataContainer dolasimAyarlari =    Windows.Storage.ApplicationData.Current.RoamingSettings;
    if (dolasimAyarlari.Values.ContainsKey("kullaniciAdi"))
        TextBox1.Text = dolasimAyarlari.Values["kullaniciAdi"].ToString();
    ```
* _LoadState_ metodu aşağıdaki gibi olacaktır.
    ```csharp
    /// <summary>
    /// Populates the page with content passed during navigation.  Any saved state is also
    /// provided when recreating a page from a prior session.
    /// </summary>
    /// <param name="navigationParameter">The parameter value passed to
    /// <see cref="Frame.Navigate(Type, Object)"/> when this page was initially requested.
    /// </param>
    /// <param name="pageState">A dictionary of state preserved by this page during an earlier
    /// session.  This will be null the first time a page is visited.</param>
    protected override void LoadState(Object navigationParameter, Dictionary<String, Object\> pageState)
    {
        if (pageState != null && pageState.ContainsKey("greetingOutputText"))
            greetingOutput.Text = pageState["greetingOutputText"].ToString();
        Windows.Storage.ApplicationDataContainer dolasimAyarlari =
            Windows.Storage.ApplicationData.Current.RoamingSettings;
        if (dolasimAyarlari.Values.ContainsKey("kullaniciAdi"))
            TextBox1.Text = dolasimAyarlari.Values["kullaniciAdi"].ToString();
    }
    ```
        
* Artık uygulamamızı çalıştırdığımızda yapmış olduğumuz oturum durumunu kaydetme ve geri yükleme işlemlerini görebiliriz. Fakat şöyle bir durum var, uygulamamızı çalıştırıp _TextBox1_ içerisine veriyi girdikten sonra **Debug -> Stop Debugging** yaptığımızda bekletme(_Suspending_) işlemi gerçekleşmeyecektir.
        
**Bekletme(_Suspending_), sonlandırma(_Terminating_) ve geri yükleme(_Restore_) işlemlerini gerçekleştirmek için**
        
* F5'e basarak uygulamamızı **debug** modda çalıştırıyoruz.
* İsmimizi TextBox'a girip “Merhaba Dünya' butonuna tıklıyoruz. Karşılama, ekranda gözüküyor.
* Şimdi Alt+Tab yapıp Visual Studio'ya dönelim.
* Suspend yazılı yeri genişletelim ve Suspend and Shutdown'a tıklayalım. ![](http://i.imgur.com/seFtjON.png)
* F5'e basalım ve uygulamayı tekrardan çalıştıralım. Uygulamamuz önceki durumuna geri yüklendi.
* TextBox içerisindeki ismi değiştirip,  “Merhaba Dünya' butonuna tıklayalım.
* Alt+Tab yapıp Visual Studio'ya dönerek **Debug -> Stop Debugging** diyoruz.
* F5 ile tekrar uygulamayı çalıştırıyoruz.
        
Kullanıcı adı geri yüklendi çünkü o yazarken kaydediliyordu. Karşılama ise geri yüklenmedi. Çünkü uygulama bekletilmedi(Suspend) ve oturumun durumu kaydedilmemiş oldu.
        
_Kaynak : Windows Store app using C# or Visual Basic, Getting Started Guide – [http://msdn.microsoft.com/en-us/library/windows/apps/hh974581.aspx](http://msdn.microsoft.com/en-us/library/windows/apps/hh974581.aspx)_