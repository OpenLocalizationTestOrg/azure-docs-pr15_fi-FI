<properties
    pageTitle="Määrittäminen avaimen säilö avaimen lopusta loppuun-kiertoa ja tarkistaminen | Microsoft Azure"
    description="Tämä vaiheittaiset käyttäminen helpottaa asennusta avaimen kierto sekä avaimen säilö lokit"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Määrittäminen avaimen säilö pääty avaimen kierto ja tarkistaminen

##<a name="introduction"></a>Johdanto

Kun olet luonut Azure-avain säilöön, osaat Käynnistä hyödyntäminen kyseisen säilö näppäimet ja tietoja tallentamiseen. Sovellusten enää on toistuvat avaimet tai tietoja, vaan voit pyytää ne avaimen säilöstä tarpeen mukaan. Voit päivittää näppäimet ja tietoja ilman vaikuttavat toiminta-sovelluksen, joka avautuu, kaikki mahdollisuudet ympärille key-tuotetunnuksen ja salainen hallinta toiminta.

Tässä artikkelissa käydään läpi Esimerkki hyödyntäminen Azure avaimen säilö tallentamiseen salainen, tässä tapauksessa Azure-tallennustilan tilin-näppäintä, joka on käyttää sovelluksen. Se myös osoittaa soveltaminen tallennustilan tilin avaimen ajoitetun kierto. Lopuksi se käy läpi osoittamista valvoa avaimen säilö valvontalokien ja nosta ilmoituksia, kun odottamattomia pyynnöt tehdään.

> \[AZURE. Huomautus\] Tässä opetusohjelmassa ei ole tarkoitettu kerrotaan yksityiskohtaisesti ensimmäisen joukon ylös, Azure-avain säilö. Tämä on artikkelissa [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md). Tai katso ohjeet Office kaikissa ympäristöissä käyttöliittymä [vastaavat tässä opetusohjelmassa](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>KeyVault määrittäminen

Jos haluat ottaa käyttöön sovelluksen noutaminen salaisuus Azure avain säilöstä, on ensin luoda toiminta ja lataa se lisääminen säilöön. Tämä onnistuu helposti PowerShellin kautta alla kuvatulla tavalla.

PowerShellin Azure-istunnon aloittaminen ja kirjaudu sisään Azure tiliisi seuraavalla komennolla:

```powershell
Login-AzureRmAccount
```

Kirjoita ponnahdusikkunoiden selainikkunassa Azure tilin käyttäjänimi ja salasana. Azure PowerShell saavat kaikki haluamasi tilaukset, jotka liittyvät tällä tilillä ja oletusarvoisesti, käyttää ensimmäistä.

Jos sinulla on useita tilauksia, voit joutua määrittämään tarkasti, joka luotiin Azure-avain säilö. Kirjoita näet tilisi tilaukset:

```powershell
Get-AzureRmSubscription
```

Tämän jälkeen voit määrittää tilaus, johon on liitetty oman avaimen säilö logging kirjoittamalla

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Kuin tässä artikkelissa esitellään tallentaminen nimellä salaisuus avaimen tallennustilan tilin, sinun on saat tallennustilan tilin näppäimelle.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Hakemisen oman salaisuus jälkeen tällöin tallennustilan-tiliavain, sinun on muuntaminen suojatun merkkijonon, ja luoda sitten kyseistä arvoa avaimen säilöön salaisuus.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Seuraava, sinun kannattaa hankkia URI juuri luomasi toiminta. Tämä käytetään myöhemmin, kun soitat avaimen hakemiseen oman salaisuus säilö. Suorita seuraava komento PowerShell ja Merkitse muistiin "Tunnus"-arvo, joka on salainen URI.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Sovelluksen määrittäminen

Nyt kun olet luonut salaisuus, joka on tallennettu, jonka haluat hakea kyseisen salaisuus ja käyttää sitä koodista. On muutama edellyttää tätä etu- ja tärkeimmät, joka on sovelluksen rekisteröimistä Azure Active Directory ja sitten kertoo Azure avaimen säilö tiedot niin, että toistettaviksi pyynnöt sovelluksestasi.

> \[AZURE. Huomautus\] sovelluksen on luotava samaan Azure Active Directory-vuokraajan kuin avaimen säilö. 

Avaa ensin Azure Active Directory sovellukset-välilehti

![Azure AD-sovellukset](./media/keyvault-keyrotation/AzureAD_Header.png)

Valitse Lisää, jos haluat lisätä uuden Azure AD-sovelluksen

![Valitse sovelluksen lisääminen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Jätä nimellä 'Web Application ja/tai WEB API' sovelluksen tyyppi ja kirjoita sovelluksesi nimi.

![Sovelluksen nimi](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Anna sovelluksen 'Sign-On URL- ja 'sovelluksen tunnus URI". Nämä voi olla mitä tahansa Tämä videoesittely ja voi muuttaa myöhemmin tarvittaessa.

![Anna tarvittavat URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Kun sovellus lisätään Azure AD-saatetaan sovellus-sivulle. Lähtien "Määrittäminen"-välilehden ja Etsi ja kopioi "Asiakastunnus"-arvo. Kirjoita muistiin Ostajantunnus myöhemmin ohjeita.

Seuraavaksi tarvitset Luo sovelluksen voivat käsitellä Azure AD avain. Voit luoda 'näppäimet-kohdassa "Määritys"-välilehti. Kirjoita muistiin myöhemmin käytettäväksi juuri luotu avain Azure AD-sovelluksesta.

![Azure AD-sovelluksen näppäimet](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Ennen kuin vahvistamiseksi sovelluksestasi kyselyjä avaimen säilö puhelujen sinun on avaimen säilö Kerro sovelluksesi ja sen ' käyttöoikeudet. Seuraava komento tulee säilö nimi ja Ostajantunnus Azure AD-sovellukset ja antaa avaimen säilö, Get-lauseeseen access-sovelluksen.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Tässä vaiheessa olet jatkaa rakentaminen sovelluksen puhelut. Sovellus ensin täytyy asentaa pakollinen vuorovaikutuksessa Azure avaimen säilö ja Azure Active Directory NuGet paketit. Visual Studio pakettien hallinta-konsolin Kirjoita seuraavat komennot. Huomaa, että kirjoitus on tämän artikkelin Active Directory-paketin nykyinen versio on 3.10.305231913, jotta voit vahvistaa uusimpaan versioon ja Päivitä vastaavasti.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Sovelluksen koodissa pitoon Active Directory käyttöoikeuksien menetelmä luokan luominen Tässä esimerkissä luokan kutsutaan "Utils". Sinun on Lisää seuraavat avulla.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Lisää seuraavaksi JWT tunnuksen noutaminen Azure AD seuraavasti. Koodattu, haluat ehkä siirtää kiintolevyillä kunnossapidettävyys merkkijonoarvoa verkossa tai sovelluksen kokoonpanon kyselyjä.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Lopuksi voit lisätä tarvittavat koodin kutsu avaimen säilö ja Nouda salainen arvo. Ensin sinun on lisättävä seuraavat lauseella.

```csharp
using Microsoft.Azure.KeyVault;
```

Lisää seuraavaksi menetelmäkutsujen kutsua avaimen säilö ja hakea oman salaisuus. Tämä menetelmä antaa salaisuus URI, johon tallensit edellisessä vaiheessa. Huomautus edellä luotu Utils luokasta GetToken-menetelmää.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Kun käynnistät sovelluksen, sen pitäisi nyt olla todennustapa Azure Active Directory ja sitten noutaminen Azure-avain säilö salainen arvo.

##<a name="key-rotation-using-azure-automation"></a>Käyttämällä Azure automaatio avain kierto

Soveltamisesta kierron strategia arvoja, voit tallentaa Azure avaimen säilö tietoja eri vaihtoehtoja. Tietoja voi kiertää manuaalisen yhteydessä, he voivat käännettävä ohjelmallisesti mukaan hyödyntäminen API-kutsuja tai ne voi kiertää automaatio-komentosarjojen kautta. Tarkoitetaan tässä artikkelissa on olla hyödyntäminen Azure automaatio, voit muuttaa Azure-tallennustilan tilin pikanäppäin yhdistettynä PowerShellin Azure ja sitten päivitämme avaimen säilö salaisuus, jonka uusi avain. 

Tarvitset Azure automaatio Määritä salainen arvot avaimen säilöön, jotta saat Asiakastunnus nimeltä "AzureRunAsConnection" joka on luotu, kun Azure automaatio-esiintymä muodostaa yhteyden. Voit siirtyä tämän tunnuksen valitsemalla "Varat" Azure automaatio-esiintymä. Sieltä Valitse 'yhteydet- ja valitse sitten "AzureRunAsConnection" palvelun tarpeen. Haluat Huomaa sovelluksen tunnus. 

![Azure automaatio Ostajantunnus](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Kalusto-ikkunassa ollessasi haluat valita "Moduulit". Moduuleista Valitse 'Valikoima' ja Hae sekä 'Tuo' päivitettyjen versioiden seuraavat moduulit.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Huomautus\] on tämän artikkelin kirjoittaminen vain edellä merkille moduulit tarvitaan jaettu alla komentosarjan päivittäminen kestää. Jos huomaat, että automaatio-työ epäonnistui tuntemattomasta, varmista, että sinulla on kaikki tarvittavat moduulit ja niiden riippuvuuksia tuotu.

Kun on haettu Sovellustunnus Azure automaatio-yhteyden, sinun on onko Azure-avain säilö tämän sovelluksen on pääsy päivittää oman säilöön tietoja. Tämä onnistuu käyttämällä seuraavaa PowerShell-komentoa.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Seuraavaksi valitaan 'Runbooks' resurssin Azure automaatio-esiintymä-kohdassa ja valitse "Lisää Runbookin". Valitse pika Luo. Nimeä oman runbookin ja runbookin tyypiksi "PowerShell". Lisää halutessasi kuvaus. Valitse lopuksi "Luo".

![Luo Runbookin](./media/keyvault-keyrotation/Create_Runbook.png)

Uusi runbookin editorin näkyvän ohjelma liittää seuraavaa PowerShell-komentosarjaa.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Editorin-ruudussa voit valita 'Testi-ruudussa' Testaa komentosarja. Kun komentosarja on käynnissä virheettömästi voit valita "Julkaise"-vaihtoehto ja tee uudelleen milloin runbookin määritys-ruudussa runbookin aikataulu.

##<a name="key-vault-auditing-pipeline"></a>Myyntijakso avaimen säilö tarkistaminen

Azure-avain säilö asetuksissa voit poistaa valvonnasta kerätä lokit avain säilöön tehdyt käyttöoikeuspyyntöjen. Lokitiedostot määritetyn Azure-tallennustilan tilin tallennetaan, ja voit sitten koottava, valvoa ja analysoida. Seuraavassa kerrotaan tilanne, jossa hyödyntää Azure-funktioiden avulla Azure logiikan sovellukset ja avaimen säilö valvontalokit putkijohto ja Lähetä sähköpostiviesti, kun tietoja säilöstä noutamien sovellus, joka vastaa sovellustunnus web-sovelluksen luominen.

Sinun on ensin, voit ottaa kirjaamisen käyttöön avaimen säilö. Tämän voi tehdä seuraavat PowerShell-komennot (tarkat tiedot näkyvät [Tässä](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Kun tämä on otettu käyttöön, valitse valvontalokien alkaa kerääminen nimettyjen tallennustilan-tilille. Lokitiedostot sisältää käyttämisestä ja luettaessa avaimen Vaults, ja kuka tapahtumat. 

> \[AZURE. Huomautus\] voit käyttää lokiin kirjaaminen on enintään, 10 minuutin avain säilöön-toiminto. Useimmissa tapauksissa se on nopeampaa kuin tämä.

Seuraavaksi voit myös [luoda Azure-palvelu Bus jonon](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Tämä on, jossa tärkeimmistä säilö valvontalokien siirretty. Kerran jonon logiikan-sovellus ne Nosta ja niiden toimia. Luo palvelun Bus on suhteellisen vakava eteenpäin ja korkean tason seuraavasti:

1. Luo palvelun Bus nimitilaa (Jos sinulla jo on yksi, jota haluat käyttää tämän jälkeen siirry vaiheeseen 2).
2. Palvelun Bus portaalissa selaamalla ja valitse luotavan-jonossa nimitilan.
3. Valitse uusi ja valitse palvelu Bus -> jonossa ja anna vaaditut tiedot.
4. Käyttää palvelun Bus yhteystietoja valitsemalla nimitilan ja valitsemalla _Yhteystiedot_. Tarvitset näitä tietoja seuraavassa osassa.

Seuraavaksi sinun tulee äänestyksen järjestäminen tallennustilan tilin avain säilöön-lokit ja nosta uusille tapahtumille [luominen Azure-funktio](../azure-functions/functions-create-first-azure-function.md) . Tämä on funktion, joka on saatu aikataulun.

Luo Azure-funktio (uusi portaalissa-funktion sovellus >). Voit käyttää isännöintipalvelu suunnitelman tai luoda uuden luonnin aikana. Voi myös valita, dynaaminen isännöimiseen. Lisätietoja funktiosta isännöinnin asetukset löytyy [tähän](../azure-functions/functions-scale.md).

Azure-funktion luomisen sen Siirry ja valitse ajastin-funktio ja C\# Valitse aloitusnäytössä **Luo** .

![Azure Funktiot aloittaminen-sivu](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Valitse _kehittäminen_ -välilehdessä korvaa run.csx koodin seuraavasti:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Huomautus\] Varmista, että korvaa yllä olevan tallennustilan tilin missä avaimen säilö lokitiedostot kirjoitetaan, aiemmin luomasi palvelun Bus ja avaimen säilö tallennustilan lokit tietyn polku osoittavan koodin muuttujien.

Funktio vastataan uusimman lokitiedosto tallennustilan tililtä missä avaimen säilö lokitiedostot kirjoitetaan, grabs tiedoston uusimman tapahtumien ja vie ne palvelun Bus jonossa. Koska yksittäinen tiedosto voi olla useita tapahtumia, kuten päälle täyden tunnin on luoda _sync.txt_ tiedostoon, jonka funktio myös tarkastelee määrittämään aikaleiman viimeisen tapahtuman, joka on noudettu. Näin varmistat, että Microsoft ei push saman tapahtuman useita kertoja. _Sync.txt_ tiedoston nimi sisältää vain havaittiin viimeinen aikaleimaa. Lokit, kun se ladataan, on voidaan lajitella mukaan aikaleima, jotta ne on järjestetty oikein.

Tämä funktio on viitattava pari kirjastoja, jotka eivät ole käytettävissä Azure-Funktiot-ruutuun. Sisällytä ne tarvitsemme Azure-Funktiot, voit tuoda ne nuget avulla. _Tiedostojen tarkasteleminen_ -vaihtoehto 

![Näytä tiedostot-asetus](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

ja lisätä uuden tiedoston nimeltä _project.json_ seuraavat sisältöä seuraavasti:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Yhteydessä _Tallenna_ Tämä käynnistää lataa tarvittavat binaaritiedostot Azure-funktioita. 

**Liittää** -välilehteen ja anna ajastin-parametrin käyttämään funktion sisällä kuvaava nimi. Yllä olevan koodin odottaa kutsuttava _myTimer_ajastin. Määritä [CRON lausekkeen](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) seuraavasti: 0 \* \* \* \* \* , mikä aiheuttaa toiminto, joka suoritetaan minuutin välein ajastin. 

Lisää syötteen, joka on tyyppiä _Azure-Blob-säiliö_samassa **liittää** -välilehdessä. Tämä osoittaa _sync.txt_ tiedosto, joka sisältää toiminnon tarkastellut viimeinen aikaleimaa. Tämä on saatavana funktion sisällä parametrin nimi. Yllä olevan koodin Azure-Blob-säiliö syötteen odottaa parametrin nimi on _inputBlob_. Valitse tallennustilan tili, johon _sync.txt_ -tiedosto tallennetaan (se voi olla samassa tai eri tallennustilan tilin) ja anna polku-kenttään polku tiedosto sijaitsee missä muodossa {container-name}/path/to/sync.txt.

Lisää tulosteen, joka on tyyppiä _Azure-Blob-säiliö_ tulos. Tämä osoita syötteen vain määrittämäsi _sync.txt_ -tiedostoon. Tämä käytetään-toiminto kirjoittaa tarkastellut viimeinen aikaleimaa. Yllä olevan koodin odottaa, että tämä parametri kutsuttava _outputBlob_.

Tässä vaiheessa toiminto on valmis. Varmista, että voit siirtyä takaisin **kehittäminen** -välilehti ja _Tallenna_ koodi. Tarkista käännöksen virheitä tulostusikkunassa ja korjaa ne vastaavasti. Jos se käännetään koodin pitäisi nyt käynnissä ja minuutin välein lokista avainta säilö ja push minkä tahansa määritetyn palvelun Bus jonossa sivulle uusille tapahtumille. Raportissa pitäisi näkyä lokiin kirjaaminen loki ikkunan päivityksen yhteydessä tapahtuvan toiminto käynnistyy kirjoittamaan.

###<a name="azure-logic-app"></a>Azure logiikan App

Seuraavaksi annettava Azure logiikan-sovelluksen luominen, joka Nosta tapahtumat, jotka funktio on valitseminen palvelun Bus jonossa, jäsentää sisältö ja Lähetä sähköpostiviesti on vastannut ehdon perusteella.

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md) valitsemalla Uusi -> logiikan sovelluksen. 

Kun logiikan-sovellus on luotu, sen Siirry ja valitse _Muokkaa_. Logiikan sovelluksen-editorissa Valitse _Palvelu Bus jonon_ hallittu ohjelmointirajapinta ja muodostaa jonossa palvelun Bus tunnistetiedot.

![Azure logiikan App palvelun Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Valitse seuraavaksi Lisää _ehto_. Ehto, siirry _muokkaus_ -ja seuraavat, APP_ID tilalle koodiin todellinen APP_ID:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Tämä lauseke pääosin palauttaa **Epätosi** Jos saapuvan tapahtuman (joka on palvelu Bus viestin tekstiosaan) **sovelluksen tunnus** -ominaisuutta ei ole sovelluksen **sovelluksen tunnus** . 

Luo nyt toiminto kohdasta _ei, jos... ei toimi_ .

![Azure logiikan App-toimintoa](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Valitse toiminto- _Office 365 - sähköpostin lähettäminen_. Täytä kentät, jos haluat luoda sähköpostiviestin lähettäminen, kun määritetyn ehdon palauttaa arvon EPÄTOSI. Jos sinulla ei ole Office 365: ssä voi tarkastella vaihtoehtoja saada samat.

Tässä vaiheessa sinun on lopusta loppuun-myyntijakso, minuutin välein, etsii uudet avaimen säilö valvontalokien. Se löytää uuden lokit, se siirtää ne palvelun Bus jonon. Logiikan sovellus käynnistyy heti, kun uusi viesti viedään maihin jonossa ja sovelluksen tunnus sisällä tapahtumaa ei vastaa puheluja sovelluksen sovellustunnus Valitse lähettää sähköpostia. 
