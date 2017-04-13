<properties
    pageTitle="Käyttämisestä Xamarin-Blob-objektien tallennustilaan | Microsoft Azure"
    description="Xamarin Azure tallennustilan asiakas-kirjaston kehittäjät voivat luoda niiden alkuperäisen käyttöliittymien iOS-, Android- ja Windows-kaupan sovellukset. Tässä opetusohjelmassa kerrotaan, miten Xamarin avulla voit luoda Azure-Blob-säiliö käyttävä sovellus."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>Opi käyttämään Xamarin-Blob-objektien tallennustilaan

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Yleiskatsaus

Xamarin avulla kehittäjät voivat käyttää jaettua C# codebase luomiseen niiden alkuperäisen käyttöliittymien iOS-, Android- ja Windows-kaupan sovellukset. Tässä opetusohjelmassa näytetään, miten Azure-Blob-säiliö Xamarin-sovelluksen kanssa käytettävät. Jos haluat lisätietoja Azure-tallennustilan ennen Sukeltava koodiksi, tutustu [artikkeleihin Johdanto Microsoft Azuren tallennustilaan](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Luo uusi Xamarin-sovellus

Tämä käytön aloittamista, että luot sovelluksen, joka sopii Android-, iOS- ja Windows. Sovelluksen hiiren luoda säilön ja ladata blob tässä säilössä esittely. Tässä videossa käytetään Visual Studio Windows käytön aloittamista, mutta saman learnings voidaan käyttää, kun Studiossa Xamarin Mac OS-sovelluksen luominen.

Voit luoda sovelluksen seuraavasti:

1. Jos et ole jo, lataa ja asenna [Visual Studio Xamarin](https://www.xamarin.com/download).
2. Avaa Visual Studio ja luo tyhjästä sovelluksesta (alkuperäisen jaettu): **Tiedosto > Uusi > Projekti > Office kaikissa ympäristöissä > tyhjä App(Native Shared)**.
3. Ratkaisu ratkaisunhallinnassa-ruudussa hiiren kakkospainikkeella ja valitse **Ratkaisu NuGet pakettien hallinta**. Etsi **WindowsAzure.Storage** ja asenna uusin vakaata ratkaisu kaikissa projekteissa.
4. Muodosta ja suorita projektin.

Sinulla pitäisi nyt sovellus, jonka avulla voit napsauttamalla painiketta, joka kasvattaa laskuri.

> [AZURE.NOTE] Azure tallennustilan asiakkaan kirjasto Xamarin tukee tällä hetkellä seuraavia projektityypit: alkuperäisen jaettu, Xamarin.Forms jaettu, Xamarin.Android ja Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Luo säilö ja lataa blob

Seuraavaksi lisäkoodin lisääminen jaettuun luokan `MyClass.cs` , joka luo säilön ja lataa blob tässä säilössä kyselyjä. `MyClass.cs`pitäisi näyttää seuraavalta:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Varmista, että korvaa "your_account_name_here" ja "your_account_key_here" todellinen tilin nimi ja tili-näppäintä. Voit käyttää tämän jaetun luokan iOS-, Android-ja Windows Phone-sovellusta. Voit lisätä vain `MyClass.createContainerAndUpload()` kuhunkin projektiin. Esimerkki:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Suorita sovellus

Voit suorittaa tämän sovelluksen nyt Android- ja Windows Phone-emulaattorin. Voit myös suorittaa tämän sovelluksen iOS-emulaattorin, mutta tämä vaatii Mac Ohjeita tähän Lue ohjeissa [yhteyden Visual Studiossa, Mac-tietokoneeseen](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Kun käynnistät sovelluksen, se luo säilön `mycontainer` tallennustilan tilissäsi. Sen pitäisi sisältää blob- `myblob`, joka sisältää muutettavan tekstin `Hello, world!`. Voit tarkistaa tämän [Microsoft Azure-tallennustilan Explorerin](http://storageexplorer.com/)avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Tämä aloittaminen-opit Office kaikissa ympäristöissä-sovelluksen luominen Xamarin, joka käyttää Azure-tallennustilan. Tämän Aloitusoppaan erityisesti kohdistettu yksi tilanne Blob-objektien tallennustilaan. Kuitenkin mahdollisuuksista usein useampaa, ei vain Blob-objektien tallennustilaan, mutta myös taulukon, tiedosto ja jonon tallennustilan. Tarkista, Lisätietoja on seuraavissa artikkeleissa:
- [Azure-Blob-säiliö .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md)
- [Pääset alkuun käyttämällä .NET Azure-taulukkotallennus](storage-dotnet-how-to-use-tables.md)
- [Käyttämällä .NET Azure jonon tallennustilan käytön aloittaminen](storage-dotnet-how-to-use-queues.md)
- [Windows Azure tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
