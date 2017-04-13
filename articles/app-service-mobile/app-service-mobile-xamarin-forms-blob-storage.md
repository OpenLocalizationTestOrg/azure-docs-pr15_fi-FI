<properties
    pageTitle="Yhteyden muodostaminen Azuren tallennustilaan Xamarin.Forms-sovelluksessa"
    description="Kuvien lisääminen todo luettelon Xamarin.Forms mobiilisovelluksen yhdistämällä Azure-blob-säiliö"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Yhteyden muodostaminen Azuren tallennustilaan Xamarin.Forms-sovelluksessa

## <a name="overview"></a>Yleiskatsaus

Azure-mobiilisovellukset asiakkaan ja palvelimen SDK tukevat offline-synkronoinnin Jäsennettyjen tietojen CRUD työvaiheita vastaan /tables päätepiste. Yleensä nämä tiedot on tallennettu tietokantaan tai samanlaisia kaupan ja yleensä näiden tietojen stores ei voi tallentaa suuri binaaritietojen tehokkaasti. Jotkin sovellukset on myös toisiinsa liittyviä tietoja, jotka on tallennettu muualle (esimerkiksi blob storage-tiedostojen) ja se on hyödyllinen voi luoda suhteet /tables päätepisteen tietueiden ja muiden tietojen välille.

Tässä ohjeaiheessa esitellään tuki kuvien lisääminen Mobile-sovellusten todo luettelon pikaopas. Ensin sinun on suoritettava opetusohjelman [Xamarin.Forms sovelluksen luominen].

Tässä opetusohjelmassa tallennustilan tilin luominen ja lisää yhteysmerkkijonon Mobile-sovelluksen Taustajärjestelmä. Valitse Lisää uusi periytymisen tyypistä Mobile-sovellusten `StorageController<T>` palvelimen projektiin.

>[AZURE.TIP] Tässä opetusohjelmassa on [avustaja otoksen](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) käytettävissä, jotka voidaan ottaa käyttöön oman Azure-tili. 

## <a name="prerequisites"></a>Edellytykset

* Suorita [Xamarin.Forms-sovelluksen luominen] opetusohjelman, jossa on luettelo muista edellytykset. Tässä artikkelissa käytetään kyseisen opetusohjelma valmis-sovellus.

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App kääntäjä](https://tryappservice.azure.com/?appServiceName=mobile). Siellä voit luoda lyhytkestoinen starter mobiilisovelluksessa heti App palvelussa – ei luottokortti ja ei ole sitoumukset.

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

1. Tallennustilan tilin luominen [Azure-tallennustilan tilin luominen]opetusohjelma. 

2. Azure-portaalissa Siirry juuri luomasi tallennustilan tilin ja **näppäimet** -kuvaketta. Kopioi **ensisijainen yhteysmerkkijonon**.

3. Siirry mobiilisovelluksen Taustajärjestelmä. **Kaikki asetukset**-kohdassa -> **Sovellusasetukset** -> **Yhteyden merkkijonoja**, Luo uusi avain nimeltä `MS_AzureStorageAccountConnectionString` ja käyttää tallennustilan tilin kopioitu arvoa. Määritä **Mukautettu** avaimen tyyppi.

## <a name="add-a-storage-controller-to-the-server"></a>Lisää tallennustilaa ohjauskoneen palvelimeen

Haluat lisätä uuden ohjaimen, joka vastata SAS tunnuksen for Azure-tallennustilan sekä luettelo tiedostoista, jotka vastaavat palaa tietueen palvelimen projektin:

- [Tallennustilan ohjauskoneen lisääminen palvelimen projektiin](#add-controller-code)
- [Tiet rekisteröityjen ohjain](#routes-registered)
- [Asiakas- ja viestintä](#client-communication)

###<a name="add-controller-code"></a>Tallennustilan ohjauskoneen lisääminen palvelimen projektiin

1. Avaa Visual Studio .NET server projektin. Lisää Nuget paketti [Microsoft.Azure.Mobile.Server.Files]. Varmista, valitse **Sisällytä prerelease**.

2. Avaa Visual Studio .NET server projektin. Napsauta **ohjaimet** -kansiota hiiren kakkospainikkeella ja valitse **Lisää** -> **ohjauskoneen** -> **Verkko-Ohjelmointirajapinnan 2-ohjain - tyhjä**. Nimi ohjauskoneen `TodoItemStorageController`.

3. Lisää seuraava lauseiden käyttämisestä:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Muuta perus luokan `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Lisää luokan seuraavista tavoista:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Päivitä verkko-Ohjelmointirajapinnan kokoonpanon määrittäminen määrite reititys. **Startup.MobileApp.cs**, Lisää seuraava rivi `ConfigureMobileApp()` menetelmä määritelmän jälkeen `config` muuttujan:

        config.MapHttpAttributeRoutes();

7. Julkaiseminen palvelimeen projektin mobiilisovelluksen Taustajärjestelmä.

###<a name="routes-registered"></a>Tiet rekisteröityjen ohjain

Uuden `TodoItemStorageController` paljastaa kaksi aliraportti resursseja hallitsee tietue:

- StorageToken

    + HTTP POST: Luo tallennustilan tunnus
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Noutaa luetteloa tietueeseen liitetyt tiedostot
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP-Poista: Poistaa määritetyn resurssin tiedostotunnisteen tiedoston
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Asiakas- ja viestintä

Huomaa, että `TodoItemStorageController` onko *ei* ole lataus kokonaan blob reitin. Tämä johtuu mobiilisovellukseen toimii kanssa blob storage *suoraan* , jotta voit suorittaa nämä toiminnot jälkeen käytön SAS-tunnuksen (jaettu Access allekirjoitus) käyttämään suojatusti tietyn Blob-objektien tai säilö. Tämä on tärkeää arkkitehtuuri rakenne-tallennustilan muutoin pääsy rajoitettava skaalattavuus ja käytettävyyden mobile Taustajärjestelmä. Muodostamalla yhteyden suoraan Azure-tallennustilan, mobiilisovelluksen voit sen sijaan, voit hyödyntää sen ominaisuuksia, kuten automaattinen jakaminen ja geo-jakauman.

Jaettu käyttö allekirjoituksen pääsee valtuutetun resurssien tallennustilan tilissäsi. Tämä tarkoittaa, että voit myöntää asiakkaan rajoitetut käyttöoikeudet objektien tallennustila-tilisi vuosipoiston annettuna kautena ajan ja käyttöoikeudet-määritetyn eikä sinun tarvitse jakaa tilin pikanäppäimet. Lisätietoja on artikkelissa [Tietoja jaetun Access allekirjoitukset].

Seuraavassa kuvassa näkyy asiakkaan ja palvelimen vuorovaikutukset. Ennen tiedoston lataamisessa asiakas pyytää SAS tunnus-palvelusta. Palvelun käyttää tallennustilan yhteysmerkkijonon Luo uusi SAS, johon se sitten palauttaa asiakkaalle. Suojaussidokset on aika rajoitettu ja rajoittaa käyttöoikeudet vain tiettyyn tiedostoon tai säilö. Mobiilisovelluksen käyttää sitten tämän Suojaussidosten ja Azure-tallennustilan asiakkaan SDK tiedosto ladataan Blob-objektien tallennustilaan.

![Pyytää SAS tunnus](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Lisää kuva-tuki asiakkaan sovelluksen päivittäminen

Avaa Xamarin.Forms pikaopas projekti Visual Studiossa tai Xamarin Studio. Asennetaan Nuget-paketit ja kannettavat kirjaston projektin ja iOS, Android tai Windows päivittäminen asiakkaan projekteja:

- [Lisää Nuget-paketit](#add-nuget)
- [Lisää IPlatform käyttöliittymä](#add-iplatform)
- [Lisää FileHelper luokka](#add-filehelper)
- [Lisää tiedostojen synkronointi käsittelytoiminto](#file-sync-handler)
- [Päivitä TodoItemManager](#update-todoitemmanager)
- [Lisää tiedot-tyyli](#add-details-view)
- [Päivitä ensisijainen näkymä](#update-main-view)
- [Päivitä Android-projekti](#update-android), [iOS project](#update-ios)- [Windows-projekti](#update-windows)

>[AZURE.NOTE] Tässä opetusohjelmassa sisältää vain Android-, iOS- ja Windows-kaupan ympäristöjen, ei ole Windows Phone-ohjeet.

###<a name="add-nuget"></a>Lisää Nuget-paketit

Napsauta ratkaisun ja valitse **ratkaisun pakettien hallinta Nuget**. Lisää seuraavat Nuget paketit ratkaisun **kaikissa** projekteissa. Muista tarkistaa **Sisällytä prerelease**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Asiasta tässä esimerkissä käytetään [PCLStorage] kirjaston, mutta sitä ei edellytä Azure-mobiilisovellukset asiakkaan SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Lisää IPlatform käyttöliittymä

Luo uuden käyttöliittymän `IPlatform` tärkeimmät kannettavat kirjaston Projectissa. Tämä noudattaa [Xamarin.Forms DependencyService] lataamaan suorituksen oikealle käyttöjärjestelmäkohtaiset-luokka. Lisää myöhemmin käyttöjärjestelmäkohtaiset käyttöotot kunkin asiakkaan projektit.

1. Lisää seuraava lauseiden käyttämisestä:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Korvaa käyttöönoton seuraavasti:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Lisää FileHelper luokka

1. Luo uusi luokka `FileHelper` tärkeimmät kannettavat kirjaston Projectissa. Lisää seuraava lauseiden käyttämisestä:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Lisää luokan määritys:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Lisää tiedostojen synkronointi käsittelytoiminto

Luo uusi luokka `TodoItemFileSyncHandler` tärkeimmät kannettavat kirjaston Projectissa. Tähän luokkaan sisältää takaisinkutsuja Azure SDK: n koodisi ilmoittaa, kun tiedosto lisätään tai poistetaan.

Azure Mobile asiakkaan SDK ei tallenneta tiedostotietoja: asiakkaan SDK käynnistää käyttöönoton `IFileSyncHandler` joka vuorostaan määrittää, miten ja tiedostot on tallennettu paikallisen laitteen.

1. Lisää seuraava lauseiden käyttämisestä:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Korvaa luokan määritys seuraavasti: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>Päivitä TodoItemManager

1. Valitse **TodoItemManager.cs**kommentointi rivin `#define OFFLINE_SYNC_ENABLED`.

2. Lisää seuraavat **TodoItemManager.cs**lauseiden käyttämisestä:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Konstruktorissa `TodoItemManager`, Lisää seuraava jälkeen puhelu `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Korvaa konstruktoria, puhelu `InitializeAsync` seuraavaan. Näin varmistat, että on takaisinkutsuja kun tietueita on muokattu paikallista. Tiedoston synkronointi-ominaisuus käyttää näitä takaisinkutsuja käynnistettävän tiedoston synkronointi-käsittely.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. Valitse `SyncAsync()`, Lisää seuraava jälkeen puhelu `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Lisää seuraavista menetelmistä `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Lisää tiedot-tyyli

Tässä osassa lisäät uuden tietonäkymä todo kohteen. Näkymän luodaan, kun käyttäjä valitsee todo kohteen ja sen avulla uusia kuvia voidaan lisätä kohteen.

1. Lisää uusi luokka **TodoItemImage** kannettavat kirjaston projektin seuraavat toteutukseen:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Muokkaa **App.cs**. Korvaa alustaminen `MainPage` kanssa seuraavasti:
    
        MainPage = new NavigationPage(new TodoList());

3. Lisää **App.cs**seuraavaa ominaisuutta:

        public static object UIContext { get; set; }

4. Kannettava kirjaston projektin hiiren kakkospainikkeella ja valitse **Lisää** -> **Uuden kohteen** -> **Office kaikissa ympäristöissä** -> **Lomakkeiden Xaml-sivua**. Näkymän nimi, `TodoItemDetailsView`.

5. Avaa **TodoItemDetailsView.xaml** ja korvaa ContentPage tekstiin seuraavasti:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Muokkaa **TodoItemDetailsView.xaml.cs** ja lisää seuraava lauseiden käyttämisestä:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Korvaa käyttöönoton `TodoItemDetailsView` kanssa seuraavasti:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Päivitä ensisijainen näkymä 

Päivitä tärkeimmät näkymän Avaa tiedot-näkymässä, kun todo kohde valitaan.

Korvaa **TodoList.xaml.cs**käyttöönoton `OnSelected` kanssa seuraavasti:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Päivitä Android-projekti

Lisää käyttöjärjestelmäkohtaiset koodi Android projektin, mukaan lukien koodiin tiedoston lataamisen sekä uuden kuvan kameran avulla. 

Tämä koodi käyttää Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) lataamaan oikealle käyttöjärjestelmäkohtaiset luokan suorituksen aikana.

1. Lisää osa **Xamarin.Mobile** Android projektiin.

2. Lisää uusi luokka `DroidPlatform` seuraavat toteutukseen. Korvaa "YourNamespace" projektin tärkeimmät nimitilan.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Muokkaa **MainActivity.cs**. Valitse `OnCreate`, Lisää seuraavat toimet ennen kutsun `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Päivitä iOS-projekti

Voit lisätä käyttöjärjestelmäkohtaiset koodin iOS projektiin.

1. Lisää osa **Xamarin.Mobile** iOS projektiin.

2. Lisää uusi luokka `TouchPlatform` seuraavat toteutukseen. Korvaa "YourNamespace" projektin tärkeimmät nimitilan.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Muokkaa **AppDelegate.cs** ja kommentointi puhelun `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Päivitä Windows-projekti

1. Asenna Visual Studio laajennus [SQLite Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Lisätietoja on artikkelissa opetusohjelman [käyttöön offline-synkronoinnin, kun Windows-sovellus](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Muokkaa **Package.appxmanifest** ja tarkista **kamera** -ominaisuus.

3. Lisää uusi luokka `WindowsStorePlatform` seuraavat toteutukseen. Korvaa "YourNamespace" projektin tärkeimmät nimitilan.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Yhteenveto

Tässä artikkelissa kuvataan käyttämisestä uusi tiedostotuki Azure Mobile asiakkaan ja palvelimen SDK Azure-tallennustilan käyttöä varten. 

- Tallennustilan tilin luominen ja lisää yhteysmerkkijonon mobiilisovelluksen Taustajärjestelmä. Vain taustaan on salaisuus Azuren tallennustilaan: mobiilisovelluksen pyytää SAS-tunnuksen (jaettu Access allekirjoitus) aina, kun se täytyy käyttää Azure-tallennustilan. Lisätietoja SAS tunnusten Azuren tallennustilaan, katso [Tietoja jaetun Access allekirjoitukset].

- Luo valvonta on kyseisen aliluokkien `StorageController` SAS suojaustunnuksen pyyntöjen ja Hae tiedostot, jotka on liitetty tietueeseen. Oletusarvon mukaan tiedostot ovat tietueeseen käyttämällä säilö; nimeen Tietuetunnus ongelma voi mukauttaa määrittämällä toteutus `IContainerNameResolver`. SAS suojaustunnuksen käytännön myös voi mukauttaa.

- Azure Mobile asiakkaan SDK ei tallenna tallenneta tiedostotietoja. Sen sijaan asiakkaan SDK käynnistää oman `IFileSyncHandler`, jolloin päättää kuinka (ja, jos) tiedostot on tallennettu paikallisen laitteen. Synkronoinnin käsittelijä rekisteröity seuraavasti:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`kutsutaan, kun Azure Mobile asiakkaan SDK tarvitsee tiedoston tiedot (esimerkiksi osana lataus). Näin voit hallita kuinka (ja, jos) tiedostot tallennetaan paikallisen laitteen ja palauttaa tietoja tarpeen mukaan.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`tiedostojen synkronointi kulun osana käynnistyy. Tiedostoviittauksen ja FileSynchronizationAction luettelointi arvon toimitetaan, niin voit päättää, miten sovelluksesi käsittelee kyseisen tapahtuman (kuten automaattisesti tiedoston lataamisen, kun se luodaan tai päivitetään, tiedoston poistamisen paikallisen laitteen, kun tiedosto on poistettu palvelimesta).

- A `MobileServiceFile` voidaan käyttää online- tai offline-tilassa käyttämällä `IMobileServiceTable` tai `IMobileServiceSyncTable`, vastaavassa järjestyksessä. Offline-tilassa tilanteessa lataus tapahtuu, kun sovellus kutsuu `PushFileChangesAsync`. Tämä aiheuttaa offline-toiminto jonossa käsitellään; tiedoston kutakin toimintoa Azure Mobile-asiakasohjelma SDK kutsuu `GetDataSource` -menetelmä `IFileSyncHandler` esiintymän noutaa tiedoston sisällön lataamisen.

- Jotta voit palauttaa kohteen tiedostoja, soita "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` esiintymä. Tämä menetelmä palauttaa luettelo tieto-osa liittyvät tiedostot. (Huomautus: Tämä on *paikallisen* käyttö ja palauttaa tiedostoja perusteella objektin tilan, kun se on viimeksi synkronoitu. Saat päivitetyn tiedostoluettelon palvelimesta-olisi käynnistät Synkronointitoiminto ensin.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Tiedoston synkronointiominaisuudesta käyttää paikallista tietueen muutosilmoituksia haluat hakea tietueet, jotka asiakas vastaanotti osana push tai salaus puretaan toimintoa. Tämä on mahdollista ottaminen käyttöön ja palvelimen synkronoinnin yhteydessä käyttämällä ilmoitukset `StoreTrackingOptions` parametria. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Muut kaupan seuranta-asetukset ovat käytettävissä vain paikallinen- tai vain palvelimen ilmoitukset esimerkiksi. Voit lisätä tai omia mukautettuja takaisinsoitto avulla `EventManager` -ominaisuuden `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- On mahdollista lisätä tai poistaa tietueen tiedostot muokkaamalla Blob-objektien tallennustilaan suoraan, koska suhteen saavutetaan nimeämiskäytäntöä kautta. Tässä tapauksessa kannattaa kuitenkin aina **Päivitä tietue kellonaika, jolloin liittyvät BLOB-objektit on muokattu**. Azure Mobile-asiakasohjelma SDK päivittää aina kun lisäämällä tai poistamalla tiedoston tietueen. 

    Tämä vaatimus syy on, että jotkin mobiilisovellukset on jo tietueen paikallisen tallennustilaan. Kun nämä asiakkaat lisäävät erotettu, tämä tietue ei palautetaan ja asiakkaan pyytää ei uuden liittyvän tiedostoille. Voit välttää tämän ongelman, on suositeltavaa Päivitä tietue aikaleima blob storage muutoksia, joka ei käytä Azure Mobile-asiakasohjelma SDK suoritettaessa.

- Asiakkaan project käyttää [Xamarin.Forms DependencyService] kuvio oikealle käyttöjärjestelmäkohtaiset luokan lataaminen suorituksen aikana. Tässä esimerkissä määritimme liittymän `IPlatform` käyttöotot kussakin käyttöjärjestelmäkohtaiset projektien kanssa.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Xamarin.Forms sovelluksen luominen]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Tietoja jaetuista Access allekirjoitukset]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Azure-tallennustilan tilin luominen]:  ../storage/storage-create-storage-account.md#create-a-storage-account
