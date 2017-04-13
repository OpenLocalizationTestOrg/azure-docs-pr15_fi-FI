<properties
    pageTitle="MUUT avulla varmuuskopioiminen ja palauttaminen App Service-sovellukset"
    description="Opettele käyttämään varmuuskopioiminen ja palauttaminen Azure App Service-sovelluksen RESTful-Ohjelmointirajapinta puhelut"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>MUUT avulla varmuuskopioiminen ja palauttaminen App Service-sovellukset

> [AZURE.SELECTOR]
- [PowerShellin](../app-service/app-service-powershell-backup.md)
- [REST-OHJELMOINTIRAJAPINNALLA](websites-csm-backup.md)

[Sovelluksen Service-sovellukset](https://azure.microsoft.com/services/app-service/web/) voi varmuuskopioida kuin BLOB Azuren tallennustilaan. Varmuuskopion voi sisältää myös sovelluksen tietokannat. Jos sovellus poistetaan vahingossa koskaan tai jos sovellus on palautettu aiemman version, se voi palauttaa kaikki varmuuskopio. Varmuuskopioiden voidaan toteuttaa milloin tahansa pyydettäessä tai varmuuskopiot voidaan ajoittaa sopivan väliajoin.

Tässä artikkelissa kerrotaan, miten varmuuskopioiminen ja palauttaminen sovelluksen RESTful-Ohjelmointirajapinta pyyntöjen. Jos haluat luoda ja hallita sovelluksen varmuuskopioiden graafisesti palvelun Azure-portaalissa, katso [verkkosovellukseen Azure-sovelluksen palvelun varmuuskopiointi](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Käytön aloittaminen
Voit lähettää REST-pyynnöt, on tiedettävä sinua sovelluksen **nimi**, **resurssiryhmä**ja **Tilaustunnus**. Tiedot löytyvät napsauttamalla sovelluksen [Azure portal](https://portal.azure.com)- **Sovellus Service** -sivu. Tämän artikkelin esimerkkejä on määritetään sivuston **backuprestoreapiexamples.azurewebsites.net**. Se on tallennettu oletusarvo-Web-WestUS resurssiryhmä ja on käynnissä ja tunnus 00001111-2222-3333-4444-555566667777-tilauksessa.

![Esimerkki sivusto-tiedot][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Varmuuskopiointi ja palauttaminen REST-Ohjelmointirajapinnalla
Nyt käsittelemme esimerkkien käytöstä REST-Ohjelmointirajapinnalla varmuuskopioiminen ja palauttaminen sovelluksen. Esimerkin sisältää URL-osoite ja HTTP-pyyntö tekstissä. Esimerkki URL-osoite sisältää paikkamerkit aaltosulkeet {Tilaustunnus} esimerkiksi kiedottu. Korvaa paikkamerkit vastaavat tiedot, kun sovellus. Käyttöä seuraavassa on selitetty paikkamerkit, joka näkyy esimerkki URL-osoitteet.

* Tilaustunnus – sisältävän sovelluksen Azure tilauksen tunnus
* resurssi-ryhmä-nimi – resurssiryhmä, joka sisältää sovelluksen nimi
* nimi – sovelluksen nimeä
* Varmuuskopiointi-tunnus – tunnus app varmuuskopiointi

Katso niitä API, mukaan lukien valinnaisten parametrien, joka voidaan lisätä HTTP-pyyntö [Azure resurssin Explorer](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Sovelluksen pyydettäessä varmuuskopiointi
Voit varmuuskopioida sovelluksen heti, Lähetä pyyntö **Kirjaa** **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Seuraavassa on URL-osoite näyttää samalla tavoin kuin Esimerkki sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Anna JSON-objektin pyyntö Määritä tallennustilan tili, minkä avulla voit tallentaa varmuuskopion tekstiosaan. JSON-objekti on oltava ominaisuutta nimeltä **storageAccountUrl**, joka sisältää [SAS URL-Osoitteen](../storage/storage-dotnet-shared-access-signature-part-1.md) myöntäminen Azuren tallennustilaan-säilö, joka sisältää varmuuskopioidut Blob-objektien kirjoitusoikeudet. Jos haluat varmuuskopioida tietokantoja, sinun on annettava myös luettelon, joka sisältää nimet, sisältötyypit ja yhteyden merkkijonot varmuuskopioida tietokanta.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Varmuuskopion sovellus käynnistyy heti, kun pyyntö on vastaanotettu. Varmuuskopiointi saattaa kestää kauan suorittamiseen. HTTP-vastaus sisältää tunnuksen, jota voit käyttää toisen pyynnössä varmuuskopioinnin tilan tarkasteleminen. Tässä on esimerkki Microsoftin varmuuskopion pyyntö HTTP vastauksen tekstiosaan.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] HTTP-vastaus log-ominaisuus löytyy virhesanomia.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Automaattisen varmuuskopioinnin
Lisäksi varmuuskopioiminen sovelluksen pyydettäessä, voit ajoittaa varmuuskopiointi tapahtuu automaattisesti.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Uuden automaattisen varmuuskopioinnin aikataulun määrittäminen
Voit määrittää varmuuskopioinnin aikataulu, Lähetä **valitseminen** pyyntö **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Seuraavassa on esimerkiksi näyttää URL-osoite, esimerkiksi sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Pyyntö tekstissä on oltava JSON-objekti, joka määrittää varmuuskopioinnin määritykset. Tässä on esimerkki ja kaikki tarvittavat parametrit.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Tässä esimerkissä määrittää sovelluksen varmuuskopioitavien automaattisesti seitsemän päivän välein. Parametrien **frequencyInterval** ja **frequencyUnit** yhdessä määrittävät, kuinka usein varmuuskopioista tapahtua. Kelvolliset **frequencyUnit** arvot ovat **Tunti** ja **päivä**. Esimerkiksi varmuuskopioida sovelluksen 12 tunnin välein, Määritä frequencyInterval 12 ja frequencyUnit tunti.

Vanhojen varmuuskopioiden poistetaan automaattisesti tallennustilan tilin. Voit määrittää, kuinka vanha varmuuskopioista voi olla **retentionPeriodInDays** -parametrin määrittämällä. Jos haluat aina oltava vähintään yksi varmuuskopiointi tallennettu, riippumatta siitä, miten kauan se, määritetään **keepAtLeastOneBackup** tosi.

### <a name="get-the-automatic-backup-schedule"></a>Saat automaattisen varmuuskopioinnin aikataulu
Saat sovelluksen varmuuskopioinnin määritys-lähetettävä **viesti** pyyntö URL-osoite **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Tässä esimerkissä-sivuston URL-osoite on **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Varmuuskopion tilan
Sen mukaan, kuinka suuri sovellus on varmuuskopion voi kestää jonkin aikaa. Varmuuskopioiden voi myös epäonnistua aikakatkaisu, tai osittain onnistu. Kaikki sovelluksen varmuuskopioiden tilan tarkasteleminen lähettää **HAE** pyyntö URL-Osoitteen **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Tietyn varmuuskopioinnin tilan tarkasteleminen Lähetä GET-pyyntö URL-Osoitteen **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Seuraavassa on esimerkiksi näyttää URL-osoite, esimerkiksi sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

Vastauksen tekstissä esiintyy samalla tavalla kuin tässä esimerkissä on JSON-objekti.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Varmuuskopion tila on numeroitu tyyppi. Seuraavassa on kaikki mahdolliset tila.

* 0 – aktiivisuustila: Varmuuskopiointi aloitettu, mutta se ei ole vielä suoritettu.
* 1 – epäonnistui: Varmuuskopiointi ei onnistunut.
* 2 – onnistui: Varmuuskopiointi onnistui.
* 3 – aikakatkaisu: Varmuuskopioinnin ei päättynyt samanaikaisesti, ja on peruutettu.
* 4 – luodaan: Varmuuskopion pyyntö on jonossa, mutta ei ole käynnistetty.
* 5 – ohittaa: Varmuuskopioinnin Jatka käynnistävä liikaa varmuuskopioiden aikataulun vuoksi.
* 6 – PartiallySucceeded: Varmuuskopiointi onnistui, mutta tiedostoja ei varmuuskopioidut koska niitä ei voi lukea. Tämä tapahtuu yleensä, koska yksityinen lukitus on sijoitettu tiedostot.
* 7 – DeleteInProgress: Varmuuskopioinnin on pyydetty poistetaan, mutta ei vielä ole poistettu.
* 8 – DeleteFailed: Varmuuskopioinnin ei voi poistaa. Tämä voi johtua siitä, että SAS URL-osoite, joka luotiin varmuuskopiointi on päättynyt.
* 9 – poistettu: Varmuuskopioinnin on poistettu.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Sovelluksen palauttaminen varmuuskopiosta
Jos sovellus on poistettu tai jos haluat palata sovelluksen aiemman version, voit palauttaa sovelluksen varmuuskopiosta. Käynnistää palautuksen lähettää **viestin** pyyntö URL-osoite **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Seuraavassa on esimerkiksi näyttää URL-osoite, esimerkiksi sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**

Pyyntö tekstiosaan Lähetä JSON-objekti, joka sisältää palautustoiminto ominaisuudet. Tässä on esimerkki, joka sisältää kaikki pakolliset ominaisuudet:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Uuden sovelluksen palauttaminen
Haluat ehkä joskus uuden sovelluksen luominen palauttaminen varmuuskopiosta korvaaminen jo olemassa olevan sovelluksen sijaan. Voit tehdä tämän muuta haluat luoda uuden sovellus pyyntö URL-osoite ja muuttaa JSON **Korvaa** -ominaisuuden arvoksi **Epätosi**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Poista sovellus-varmuuskopiointi
Jos haluat poistaa varmuuskopion, Lähetä pyyntö **poistaa** URL-Osoitteen **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Seuraavassa on esimerkiksi näyttää URL-osoite, esimerkiksi sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Hallitse on varmuuskopio SAS URL-osoite
Azure App palvelu yrittää poistaa varmuuskopion Azuren tallennustilaan käyttämällä SAS URL-Osoitetta, joka on annettu varmuuskopio on luotu. Jos tätä SAS URL-Osoitetta ei ole enää voimassa, varmuuskopioinnin ei voi poistaa REST API-Liittymän kautta. Voit kuitenkin päivittää liittyvä varmuuskopion lähettämällä **Kirjaa** pyyntö URL-Osoitteen SAS URL-osoite **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Seuraavassa on esimerkiksi näyttää URL-osoite, esimerkiksi sivustossamme. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/List**

Lähetä pyyntö tekstiosaan JSON-objekti, joka sisältää uusi SAS URL-osoite. Tässä on esimerkki.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Tietoturvasyistä varmuuskopion liittyvä SAS URL-osoite palautetaan ei lähetettäessä GET-pyynnössä tietyn varmuuskopiointiin. Jos haluat tarkastella varmuuskopion liittyvä SAS URL-osoite, lähettää viestin pyyntö URL-Osoitetta edellä. Lisää tyhjä JSON-objektin pyynnön tekstiosaan. Palvelimen vastausta sisältää kaikki kyseisen varmuuskopion tiedot, mukaan lukien sen SAS URL-osoite.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
