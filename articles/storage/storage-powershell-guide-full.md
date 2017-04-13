<properties
    pageTitle="Azure PowerShellin käyttäminen Azure tallennustilan | Microsoft Azure"
    description="Opettele käyttämään voit luoda ja hallita tallennustilan tilit; Azuren tallennustilaan Azure PowerShellin cmdlet-komennot BLOB-objektit, taulukoita tai olevien tiedostojen; käsitteleminen Määritä kyselyn tallennustilan analytics ja jaettua käyttöä allekirjoitusten luominen."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Azure PowerShellin käyttäminen Azure-tallennustilan

## <a name="overview"></a>Yleiskatsaus

Azure PowerShell on moduuli, joka sisältää cmdlet-komentojen Azure Windows PowerShellin kautta. On tehtävä-pohjainen komentorivin runko ja suunniteltu erityisesti järjestelmänvalvontaan kieltä. PowerShellin voit helposti valita ja automatisoida Azure palveluiden ja sovellusten hallinta. Voit esimerkiksi käyttää samoja toimintoja, joita voidaan suorittaa palvelun [Azure Portal](https://portal.azure.com)-Cmdlet-komentoja.

Tämän oppaan tutustutaan [Azure tallennustilan cmdlet-komentojen](https://msdn.microsoft.com/library/azure/mt269418.aspx) käyttämisestä suorittaa erilaisia kehittämisen ja hallinnan tehtävät, joiden Azure-tallennustilan.

Tässä oppaassa oletetaan, että edellisen kokemus [Azuren tallennustilaan](https://azure.microsoft.com/documentation/services/storage/) ja [Windows PowerShellin](http://technet.microsoft.com/library/bb978526.aspx)avulla. Opas sisältää useita komentosarjojen osoittamaan PowerShellin Azure-tallennustilan ja käyttö. Sinun on päivitettävä kokoonpanosi ennen kuin suoritat kukin komentosarja script muuttujat.

Tässä oppaassa ensimmäinen osa on silmäyksellä Azuren tallennustilaan ja PowerShell. Lisätietoja ja ohjeita Käynnistä [edellytyksistä Azure PowerShellin Azure-tallennustilan kanssa](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Käytön aloittaminen Azuren tallennustilaan ja PowerShell-5 minuuttia

Tässä osassa näytetään, miten voit käyttää Azuren tallennustilaan PowerShellin kautta 5 minuuttia.

**Uusi Azure:** Hanki Microsoft Azure-tilausta ja kyseiseen tilaukseen liitetyllä Microsoft-tiliä. Lisätietoja Azure hankkiminen asetukset saat [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/), [Osta asetukset](https://azure.microsoft.com/pricing/purchase-options/)ja [Jäsenen on](https://azure.microsoft.com/pricing/member-offers/) (MSDN-, Microsoft Partner Network- ja BizSpark, ja muihin Microsoft-ohjelmiin jäsenet).

[Järjestelmänvalvojan roolien määrittäminen Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Katso lisätietoja Azure tilaukset.

**Kun olet luonut Microsoft Azure-tilauksen ja tilin:**

1.  Lataa ja asenna [PowerShellin Azure](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Käynnistä Windows PowerShell integroitu komentosarjat-ympäristö (ise:): Paikallisessa tietokoneessa, siirry **Käynnistä** -valikkoon. Kirjoita **Valvontatyökalut** ja valitse sen suorittamiseen. **Valvontatyökalut** -ikkunassa napsauta hiiren kakkospainikkeella **Windows PowerShell ise:**, valitse **Suorita järjestelmänvalvojana**.
3.  Valitse **Windows PowerShell ise:** **Tiedosto** > Luo uusi komentosarjatiedosto**Uusi** .
4.  Nyt uutiskirjeessä voit yksinkertaisen komentosarjan, joka näyttää basic PowerShell-komennoilla voit käyttää Azure-tallennustilan. Komentosarjan ensin kysyä oman Azure-tilin tunnistetiedot, voit lisätä oman Azure tilin paikallisessa PowerShell-ympäristössä. Komentosarja asettaminen Azure tilaus ja sitten tallennustilan uuden tilin luominen Azure. Seuraavaksi komentosarja luo uuden säilön uusi tallennustilan tilillesi ja lataa aiemmin luotu kuvatiedosto (blob) kyseisen säilön. Sen jälkeen, kun komentosarja on lueteltu kaikki Blob-objektien kyseisen säilön, se luodaan uusi kohdekansiota paikalliseen tietokoneeseen ja ladata kuvatiedoston.
5.  Valitse seuraava koodi-osiossa komentosarja välillä huomautukset **#begin** ja **#end**. Paina näppäinyhdistelmää CTRL + C, kopioi se Leikepöydälle.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  **Windows PowerShell ise:**painamalla näppäinyhdistelmää CTRL + V kopioidaksesi komentosarja. Valitse **Tiedosto** > **Tallenna**. Kirjoita komentosarjatiedosto, kuten "mystoragescript" nimi **Tallenna nimellä** -valintaikkuna. Valitse **Tallenna**.

6.  Nyt on päivitettävä komentosarjan muuttujat määritysasetukset perusteella. Sinun on päivitettävä **$SubscriptionName** muuttujan oman tilaus. Voit säilyttää muiden muuttujien komentosarja mukaisesti tai voit päivittää ne.

    - **$SubscriptionName:** Oman tilauksen nimi ja muuttuja on päivitettävä. Tekemällä jommankumman seuraavista Etsi tilauksen nimeä seuraavilla kolmella tavalla:

        a. Valitse **Windows PowerShell ise:** **Tiedosto** > Luo uusi komentosarjatiedosto**Uusi** . Kopioi seuraavaa komentosarjaa komentosarja uuteen tiedostoon ja valitse sitten **Virheenkorjaus** > **Suorita**. Seuraavaa komentosarjaa ensin kysyä oman Azure-tilin tunnistetiedot, voit lisätä oman Azure tilin PowerShell paikallisen ympäristön ja valitse Näytä kaikki haluamasi tilaukset, jotka ovat yhteydessä paikallisen PowerShell-istunnon. Huomaa, jota haluat käyttää tämä opetusohjelma tilauksen nimeä:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Paikanna ja kopioi tilauksen nimi [Azure Portal](https://portal.azure.com)-toiminnossa-valikon vasemmassa reunassa sitten **tilaukset**. Kopioi, jota haluat käyttää komentosarjat tässä oppaassa tilauksen nimi.

        ![Azure Portal][Image2]

        c-näppäinyhdistelmää. Paikanna ja kopioi tilauksen nimi [Azure perinteinen Portal](https://manage.windowsazure.com/)-alaspäin ja valitse **asetukset** -portaalin vasemmalla puolella. Valitse luettelo tilauksistasi **tilaukset** . Kopioi, jota haluat käyttää annettu komentosarjojen tässä oppaassa tilauksen nimi.

        ![Azure perinteinen Portal][Image1]

    - **$StorageAccountName:** Määritetyn nimen käyttäminen komentosarja tai kirjoita uusi tallennustilan tilin nimi. **Tärkeä:** Tallennustilan tilin nimi on oltava yksilöllinen Azure-tietokannassa. Se on kirjoitettava pienellä, liian!

    - **$Location:** Käytä annetun "Länsi USA" komentosarja tai valita muita Azure sijainneissa, kuten Itä US, Pohjois-Eurooppa, ja niin edelleen.

    - **$ContainerName:** Määritetyn nimen käyttäminen komentosarja tai oman säilö uusi nimi.

    - **$ImageToUpload:** Kirjoita polku kuvan paikallisessa tietokoneessa seuraavasti: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Kirjoita polku paikallisen hakemiston tallentamiseen Azure-tallennustilan, kuten ladatut tiedostot: "C:\DownloadImages".

7.  Kun olet päivittänyt komentosarjan muuttujien "mystoragescript.ps1"-tiedosto, valitse **Tiedosto** > **Tallenna**. Valitse sitten **Virheenkorjaus** > **suorittaa** tai paina **F5-näppäintä** komentosarjan suorittamista.  

Kun komentosarja on suoritettu, on oltava paikallisen kohdekansiota, joka sisältää ladatut kuvatiedoston. Seuraavassa näyttökuvassa näkyy esimerkki tulos:

![Lataa BLOB-objektit][Image3]


> [AZURE.NOTE] "Aloittaminen Azure säilytys- ja PowerShell-5 minuuttia"-osassa annettujen lyhyt esittely PowerShellin Azure käyttäminen Azure-tallennustilan. Lisätietoja ja ohjeita on suositeltavaa lukea seuraavissa osissa.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Edellytyksistä PowerShellin Azure käyttäminen Azuren tallennustilaan
Tarvitset Azure tilauksen ja tilin Suorita tässä oppaassa alan PowerShellin cmdlet-komennot yllä olevien ohjeiden mukaisesti.

Azure PowerShell on moduuli, joka sisältää cmdlet-komentojen Azure Windows PowerShellin kautta. Saat lisätietoja asentamalla ja määrittämällä PowerShellin Azure- [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md). On suositeltavaa, lataa ja asentaa tai päivittää uusimmat Azure PowerShell-moduulin ennen tämän oppaan avulla.

Voit suorittaa Cmdlet-komentoja vakio Windows PowerShell console tai Windows PowerShell integroitu komentosarjat ympäristössä (ise:). Esimerkiksi Avaa **Windows PowerShell ise:**Siirry aloitusvalikkoon Valvontatyökalut ja kirjoittamalla sen suorittamiseen. Valvontatyökalut-ikkunassa napsauta hiiren kakkospainikkeella Windows PowerShell ise:, valitse Suorita järjestelmänvalvojana.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Azure tilien tallennustilan hallinta

### <a name="how-to-set-a-default-azure-subscription"></a>Oletusasetusten Azure tilauksen määrittäminen
Jos haluat Azure PowerShellin Azure-tallennustilan hallinta-asiakasohjelman ympäristön ja Azure Azure Active Directory-todennuksen tai varmenteen todentamiseen. Lisätietoja on kohdassa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) opetusohjelma. Tämän oppaan käyttää Azure Active Directory-todennusta.

1.  Kirjoita Windows PowerShell ise: Voit lisätä oman Azure seuraava komento PowerShell paikallisen ympäristön tilin:

    `Add-AzureAccount`

2.  Kirjoita sähköpostiosoite ja salasana tilisi "Kirjaudu sisään, Microsoft Azure"-ikkunaan. Azure todentaa ja tallentaa tunnistetietoja ja sulkee ikkunan.

3.  Suorita seuraavaksi tarkastella Azure tilejä PowerShell paikallisen ympäristön ja varmista, että tili näkyy seuraava komento:

    `Get-AzureAccount`

4.  Suorita seuraavat cmdlet-komennolla voit tarkastella kaikkien tilausten, jotka ovat yhteydessä paikallisen PowerShell-istunnon ja varmista, että tilauksesi näkyy:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Oletusasetusten Azure tilauksen määrittäminen, valitse AzureSubscription cmdlet-komennon suorittaminen

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Tarkista oletusarvo-tilauksesi nimen suorittamalla Get-AzureSubscription cmdlet-komennon:

    `Get-AzureSubscription -Default`

7.  Haluat nähdä kaikki käytettävissä olevat PowerShell-cmdlet Azure-tallennustilan, suorita:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Voit luoda uuden Azure-tallennustilan tilin
Azure tallennuskiintiön käyttämään on tallennustilan tilin. Voit luoda uuden Azure-tallennustilan tilin, kun olet määrittänyt tietokone muodostaa tilaukseen.

1.  Suorittamalla Get-AzureLocation cmdlet-komento, voit etsiä kaikki käytettävissä olevat palvelinkeskuksen sijainnit:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Suorita seuraavaksi uusi AzureStorageAccount cmdlet-komento, Luo uusi tallennustilan-tili. Seuraavassa esimerkissä luodaan uusi tallennustilan tili "Länsi USA" joten.

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Tallennustilan tilin nimi on oltava yksilöllinen Azure, ja se on kirjoitettava pienellä. Nimeämiskäytännöt ja rajoituksista on artikkelissa [Tietoja Azure tallennustilan asiakkaat](storage-create-storage-account.md) ja [Naming ja viittaava säilöjen ja BLOB-metatiedot](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Azure-tallennustilan oletustilin määrittäminen
Voit määrittää useita tallennustilan tilejä tilauksen. Voit valita yhden niistä ja määrittää sen tallennustilan oletustiliksi tallennusvälineiden komennot samassa PowerShell-istunnossa. Näin voit suorittaa PowerShellin Azure-tallennustilan komentoja määrittämättä tallennustilan yhteydessä erikseen.

1.  Jos haluat määrittää tilauksen tallennustilan oletustili, voit suorittaa määrittäminen AzureSubscription cmdlet-komento.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Seuraavaksi suorittamalla Get-AzureSubscription cmdlet-komento, varmista, että tallennustilan-tili on liitetty tilauksen oletustilin. Tämä komento palauttaa tilauksen ominaisuudet, mukaan lukien sen nykyistä tallennustilan tilin voimassa oleva tilaus.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Luettele kaikki tilauksen Azure-tallennustilan tilin määrittämisestä
Jokaisen Azure tilauksen voi olla enintään 100 tallennustilan tilit. Katso uusimmat tiedot rajoitukset, [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md).

Suorita seuraavat cmdlet-komennon avulla selvää, nimi ja nykyisen tilauksesi tallennustilaa tilien tilan:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Azure-tallennustilan kontekstin luominen
Azure-tallennustilan konteksti on objektin PowerShell tärkeintä tallennustilan tunnistetiedot. Käyttämistä tallennustilan kontekstissa, kun käynnissä kaikki seuraavat cmdlet-komennon avulla voit todentaa pyyntö määrittämättä tallennustilan tilin ja sen pikanäppäin erikseen. Voit luoda tallennustilan kontekstin useilla tavoilla, kuten käyttämällä tallennustilan tilin nimi ja käyttää avainta, jaettu allekirjoitus (SAS) käyttöoikeustietue, yhteysmerkkijono- tai julkinen. Lisätietoja on artikkelissa [Uusi AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Käytä jotakin luominen tallennustilan kontekstin seuraavilla kolmella tavalla:

- Ensisijainen tallennustilan pikanäppäin Azure-tallennustilan tilin kieliominaisuuksien [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) cmdlet-komennon suorittaminen Soita seuraavaksi tallennustilan kontekstin Luo [Uusi AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) cmdlet-komennon:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Luo jaettu käyttö allekirjoituksen tunnus Azure säilytykseen ja tallennustilaa kontekstin luoda sen avulla:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Lisätietoja on artikkelissa [Uusi AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) ja [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- Haluat ehkä määrittää Palvelupäätepisteet, kun luot uuden tallennustilan kontekstissa. Tämä voi olla tarpeen, kun olet rekisteröinyt mukautettua toimialuenimeä tallennustilan tilin Blob-palvelussa tai haluat käyttää jaettua käyttöä allekirjoituksen käyttämiseen tallennustilan resurssit. Määrittää Palvelupäätepisteet yhteysmerkkijonon ja luoda uuden tallennustilan kontekstin alla kuvatulla tavalla:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Lisätietoja tallennustilan yhteysmerkkijonon määrittämisestä on artikkelissa [Määrittäminen yhteyden merkkijonoja](storage-configure-connection-string.md).

Nyt kun olet tietokoneen määrittäminen ja oppinut tilaukset ja tallennustilaa tilille PowerShellin Azure, siirry seuraavaan osaan ja lue, miten voit hallita Azure BLOB-objektit ja blob tilannevedoksia.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Voit hakea ja luo Azure tallennustilan näppäimet

Azure-tallennustilan tilin sisältyy kaksi tiliä avaimet. Seuraavan cmdlet-komennon avulla voit hakea avaimien.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Seuraavan cmdlet-komennon avulla voit hakea tietyn avaimen. Kelvolliset arvot ovat ensisijainen ja toissijainen.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Jos haluat luoda avaimien, käytä seuraavan cmdlet-komennon. Kelvolliset - KeyType arvot ovat "Ensisijainen" ja "Toissijainen"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Voit hallita Azure BLOB-objektit
Azure-Blob-säiliö on paljon erimuotoisia tietoja, kuten binaaritietoja tai tiedot-, jota voi käyttää mitä tahansa HTTP tai HTTPS maailmanlaajuisesti tallentamista varten. Tässä osassa oletetaan, että olet jo aiemmin Azure-Blob-objektien Tallennuspalvelu käsitteitä. Lisätietoja on artikkelissa [pääset alkuun käyttämällä .NET-Blob-säiliö](storage-dotnet-how-to-use-blobs.md) ja [Blob-objektien palvelun käsitteitä](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Säilön luominen
Jokaisen Blob-objektien tallennustilaan Azure on oltava säilö. Voit luoda yksityinen säilön uusi AzureStorageContainer cmdlet-komennolla:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Anonyymi lukuoikeudet kolme tasoa: **käytöstä**, **Blob-objektien**ja **säilö**. Anonyymin käytön estämiseksi BLOB-arvoksi käyttöoikeus-parametrin **käytöstä**. Oletusarvon mukaan uusi säilö on yksityinen, ja niitä voi käyttää vain tilin omistaja. Anna anonyymi julkisen lukuoikeudet blob resursseja, mutta ei oikeutta säilö metatietojen tai BLOB-säilö luetteloon, **Blob-objektien**käyttöoikeudet-parametrin arvoksi. Anna koko julkisen lukuoikeudet blob-resursseja, säilön metatietojen ja BLOB-säilö luettelo, **säilön**käyttöoikeus-parametrin arvoksi. Lisätietoja on artikkelissa [Hallitse anonyymi lukuoikeudet säilöjä ja BLOB-objektit](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Lataaminen blob säilöön
Azure-Blob-säiliö tukee estä BLOB-objektit ja sivun BLOB-objektit. Lisätietoja on artikkelissa [tietoja estä BLOB-objektit, Liitä BLOB ja sivun BLOB-objektit](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Lataa BLOB-säilö, voit käyttää [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) cmdlet-komento. Oletusarvoisesti tämä komento lataa paikallisina tiedostoina estä Blob-objektien. Voit määrittää blob varten, voit käyttää - BlobType parametrin.

Seuraavassa esimerkissä suoritetaan [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet-komento, saat kaikki määritetyn kansiossa olevat tiedostot ja välittää ne sitten seuraavan cmdlet-komento myyntijakso-operaattorilla. [Määritä AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) cmdlet-komento latauksia paikallisina tiedostoina oman-säilöön:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Lataaminen BLOB-säilö
Seuraavassa esimerkissä kerrotaan, miten voit ladata BLOB-säilö. Esimerkki ensin muodostaa yhteyden käyttämällä tallennustilan tilin kontekstissa, joka sisältää tallennustilan tilin nimi ja sen ensisijainen pikanäppäin Azure-tallennustilan. Esimerkki noutaa blob-viittauksen [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-komennolla. Seuraavaksi esimerkissä käytetään [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) cmdlet-komento lataa BLOB paikallisen kohde-kansioon.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Kopioimisesta BLOB storage säilöstä toiseen
Voit kopioida BLOB storage tilit ja alueiden välillä asynkronisesti. Seuraavassa esimerkissä kerrotaan, miten voit kopioida BLOB storage säilöstä toiseen kaksi eri tallennustilan tilin. Esimerkki määrittää ensin muuttujat lähde- ja tallennustilaa-tileissä ja luo sitten tallennustilan kontekstin jokaiselle tilille. Seuraavaksi esimerkin kopioi BLOB lähde-säilö kohdesäilön [Käynnistä AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) -cmdlet-komennolla. Esimerkissä oletetaan, että lähde- ja tallennustilaa tilit ja säilöjä jo olemassa.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Huomaa, että tässä esimerkissä suorittaa asynkroninen kopio. Voit valvoa kopioita tilan suorittamalla [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) cmdlet-komento.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Kopioimisesta BLOB toissijaiseen sijaintiin
Voit kopioida BLOB GRS RA käytössä tilin toissijainen sijainnista.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Voit poistaa blob
Voit poistaa blob-haluat ensin blob viitata ja kutsua sitten Poista AzureStorageBlob cmdlet-komento. Seuraavassa esimerkissä poistaa kaikki tietyn säilön BLOB. Esimerkki määrittää ensin muuttujat tallennustilan tilin ja luo sitten tallennustilan kontekstissa. Seuraavaksi Esimerkki noutaa blob-viittauksen [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-komennolla ja suorittaa [Poista AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) cmdlet-komento, voit poistaa BLOB säilön Azuren tallennustilaan.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Azure-blob-tilannevedoksia hallinta
Azure voit luoda tilannevedoksen blob. Tilannevedoksen on vain luku-versio, joka on otettu vaiheessa ajassa Blob-objektien. Kun tilannevedoksen on luotu, se voi lukea, kopioitu, tai poistettu, mutta ei muokata. Tilannevedoksia lisäämistapaa varmuuskopioida blob sellaisena kuin se näkyy hetkellä. Lisätietoja on artikkelissa [Blob tilannevedoksen luominen](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Blob-tilannevedoksen luominen
Luo päivittymätön, blob haluat ensin blob viitata ja valitse Soita `ICloudBlob.CreateSnapshot` menetelmä sitä. Seuraavassa esimerkissä määrittää ensin muuttujat tallennustilan tilin ja luo sitten tallennustilan kontekstissa. Seuraavaksi Esimerkki noutaa blob-viittauksen [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-komennolla ja suorittaa tilannevedoksen luominen [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -menetelmää.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Miten luettelo Blob-objektien tilannevedokset
Voit luoda niin monta tilannevedoksia kuin haluat blob. Voit lisätä oman Blob-objektien seurata nykyisen tilannevedoksia liittyvät tilannevedoksia. Seuraavassa esimerkissä käyttää ennalta määritettyjä Blob-objektien ja soittaa luettelon kyseiseen blob tilannevedosten [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-komento.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Tilannevedoksen blob kopioiminen
Voit kopioida tilannevedoksen Blob-objektien palauttaa tilannevedoksen. Yksityiskohtaiset tiedot ja rajoituksista on artikkelissa [Blob tilannevedoksen luominen](http://msdn.microsoft.com/library/azure/hh488361.aspx). Seuraavassa esimerkissä määrittää ensin muuttujat tallennustilan tilin ja luo sitten tallennustilan kontekstissa. Seuraavaksi esimerkki määrittää säilö ja Blob-objektien nimi muuttujat. Esimerkki noutaa blob-viittauksen [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) cmdlet-komennolla ja suorittaa tilannevedoksen luominen [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -menetelmää. Esimerkki suorittaa [Käynnistä AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) cmdlet-komennolla kopioi Blob-objektien käyttäminen ICloudBlob objektin lähde-blob tilannevedoksen. Muista päivittää kokoonpanosi ennen kuin suoritat esimerkin muuttujat. Huomaa, että seuraavassa esimerkissä oletetaan, että lähde ja kohde säilöjen ja lähde-blob jo olemassa.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Nyt oppinut voit hallintaan Azure BLOB-objektit ja tilannevedoksia PowerShellin Azure blob, siirry seuraavaan osaan ja opi hallitsemaan taulukoita, olevien ja tiedostoja.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Azure-taulukoiden ja taulukon kohteiden hallinnasta
Azure taulukon tallennuspalvelu on NoSQL datastore, jonka avulla voit tallentaa ja kyselyjen rakenteellisia ja relaatio erittäin suuri tietojoukkoa. Palvelun pääkomponentit on taulukoita, kohteet ja ominaisuudet. Taulukko on kokoelma kohteita. Kohde on joukko ominaisuuksia. Kunkin kohteen voi olla enintään 252 ominaisuudet, jotka ovat kaikki nimi-arvo-pareina. Tässä osassa oletetaan, että olet jo aiemmin Azure-taulukosta Tallennuspalvelu käsitteitä. Lisätietoja on artikkeleissa [taulukon Service-tietomallin perusteet](http://msdn.microsoft.com/library/azure/dd179338.aspx) ja [Azure-taulukkotallennus käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-tables.md).

Seuraavissa jaksoissa opit hallinnasta Azure-taulukosta tallennuspalvelu Azure PowerShellin avulla. Tilanteita, joissa kattaa sisältyvät **luominen**, **poistaminen**ja **hakeminen** **taulukot**, sekä **lisäämällä**, **Kyselyt**ja **taulukon kohteiden poistaminen**.

### <a name="how-to-create-a-table"></a>Taulukon luominen
Jokaisen taulukon on sijaittava Azure-tallennustilan tilin. Seuraavassa esimerkissä kerrotaan, miten voit luoda taulukon Azuren tallennustilaan. Esimerkki ensin muodostaa yhteyden käyttämällä tallennustilan tilin kontekstissa, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi se käyttää [Uutta AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) cmdlet-komennolla voit luoda taulukon Azuren tallennustilaan.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Taulukon noutaminen
Voit kyselyn ja hakea yhden tai kaikkien taulukoiden tallennustilan tilillä. Seuraavassa esimerkissä kerrotaan, miten hakea tietyn taulukon [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-komennolla.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Jos olet soittanut Get-AzureStorageTable cmdlet-komento ilman parametreja, se saa kaikkien tallennustilan taulukoiden tallennustilan tilin.

### <a name="how-to-delete-a-table"></a>Taulukon poistaminen
Voit poistaa taulukon tallennustilan tililtä [Poista AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) cmdlet-komennolla.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Taulukon kohteiden hallinta
Tällä hetkellä PowerShellin Azure ei tarjoa cmdlet-komentojen taulukon kohteiden suoraan. Suorita taulukon kohteita, voit annettu [Azure tallennustilan asiakkaan kirjasto .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)-luokat.

#### <a name="how-to-add-table-entities"></a>Voit lisätä taulukon kohteiden
Jos haluat lisätä kohteen taulukkoon, Luo objekti, joka määrittää kohteen ominaisuudet. Kohteen voi olla enintään 255 ominaisuudet, mukaan lukien 3 järjestelmän ominaisuudet: **PartitionKey**, **RowKey**ja **aikaleiman**. Olet vastuussa lisääminen ja päivitys **PartitionKey** ja **RowKey**arvot. Palvelimen hallitsee **aikaleiman**, joita ei voi muokata-arvoa. Yhdessä **PartitionKey** ja **RowKey** yksilöivät jokaisen kohteen taulukon sisällä.

-   **PartitionKey**: määrittää osio, joka kohde on tallennettu.
-   **RowKey**: yksilöi kohde osiossa.

Voi määrittää enintään 252 mukautettuja ominaisuuksia kohteen. Lisätietoja on artikkelissa [taulukon Service-tietomallin perusteet](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Seuraavassa esimerkissä kerrotaan, miten kohteiden lisääminen taulukkoon. Esimerkki voit noutaa Työntekijä-taulukosta ja lisätä siihen useita kohteita. Ensimmäisen kerran se muodostaa yhteyden käyttämällä tallennustilan tilin kontekstissa, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi hakee annettuun taulukkoon [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-komennolla. Jos taulukossa ei ole, [Uusi AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) cmdlet-komento käytetään taulukon luominen Azure-tallennustilan. Seuraavaksi esimerkki määrittää mukautetun funktion lisääminen-kohteen kohteiden lisääminen taulukon määrittämällä kunkin kohteen osio ja rivi-näppäintä. Lisää kohde-funktio kutsuu [Uusi objekti](http://technet.microsoft.com/library/hh849885.aspx) cmdlet-komento ja Luo objekti kohteen [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) -luokka. Myöhemmin esimerkissä kutsuu [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -menetelmää tämän kohteen-objektin lisääminen taulukkoon.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Taulukon kohteiden kyselyjen tekeminen
Kyselyn taulukkoon, käytä [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) -luokka. Seuraavassa esimerkissä oletetaan, että olet jo suorittanut komentosarja alan miten voit lisätä tämän oppaan osio kohteita. Esimerkki ensin muodostaa yhteyden käyttämällä tallennustilan yhteydessä, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi yritetään noutaa aiemmin luodun [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-komennolla "Työntekijät"-taulukko. [Uusi objekti](http://technet.microsoft.com/library/hh849885.aspx) cmdlet-komennon kutsumista Microsoft.WindowsAzure.Storage.Table.TableQuery luokan Luo uuden kyselyobjektin. Esimerkissä etsitään henkilöt, joilla on ""-tunnussarake, jonka arvo on 1 merkkijonon suodattimen mukaisesti. Lisätietoja on artikkelissa [Kyselyt taulukot ja kohteiden](http://msdn.microsoft.com/library/azure/dd894031.aspx). Kun suoritat kyselyn, kysely palauttaa kaikki kohteet, jotka vastaavat suodatusehdot.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Taulukon kohteiden poistaminen
Voit poistaa kohteen sen osio ja rivi-näppäimen avulla. Seuraavassa esimerkissä oletetaan, että olet jo suorittanut komentosarja alan miten voit lisätä tämän oppaan osio kohteita. Esimerkki ensin muodostaa yhteyden käyttämällä tallennustilan yhteydessä, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi yritetään noutaa aiemmin luodun [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) cmdlet-komennolla "Työntekijät"-taulukko. Jos taulukko on olemassa, esimerkin kutsuu noutaa sen osio ja rivin avainarvot perusteella kohteen [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) -menetelmää. Siirtää sitten [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) tapa poistaa kohteen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Azure olevien ja sanomien hallinnasta
Azure jonon tallennustila on palvelu, projektiin paljon viestejä, jotka voi käyttää mitä tahansa maailmanlaajuisesti todennetut puhelujen soittaminen HTTP tai HTTPS kautta. Tässä osassa oletetaan, että olet jo aiemmin Azure jonon Tallennuspalvelu käsitteitä. Lisätietoja on artikkelissa [Azure jonon tallennustilan käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-queues.md).

Tässä osassa näkyy hallinnasta Azure jonon tallennuspalvelu Azure PowerShellin avulla. Tilanteita, joissa kattaa Sisällytä **lisääminen** ja **poistaminen** sanomien, sekä **luominen**, **poistaminen**ja **haetaan olevien**.

### <a name="how-to-create-a-queue"></a>Voit luoda jonon
Seuraavassa esimerkissä ensin muodostaa yhteyden käyttämällä tallennustilan tilin kontekstissa, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi kutsuu [Uusi AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) cmdlet-komento, voit myös luoda jonon nimeltä "queuename".

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Artikkelissa Azure jonon palvelun nimeämiskäytännöt tietojen, [nimeäminen olevien ja metatiedot](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Voit hakea jonossa
Voit kyselyn ja hakea tietyn jonon tai kaikki tallennustilan tilin olevien luettelo. Seuraavassa esimerkissä kerrotaan, miten hakemiseen määritetyn jonon [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet-komennolla.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Jos olet soittanut [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet-komento ilman parametreja, se saa kaikkien olevien luettelon.

### <a name="how-to-delete-a-queue"></a>Jonon poistaminen
Jos haluat poistaa jono, ja kaikki sen sisältävät viestit, Soita Poista AzureStorageQueue cmdlet-komento. Seuraavassa esimerkissä esitetään poistamisesta määritetyn jonon, poista AzureStorageQueue-cmdlet-komennolla.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Miten voit lisätä viestiin jonossa
Jos haluat lisätä viestiin aiemmin jonossa, luo [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) luokan uuden esiintymän. Seuraavaksi kutsua [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -menetelmää. CloudQueueMessage voi luoda merkkijono (UTF-8-muodossa) tai DBCS-matriisi.

Seuraavassa esimerkissä, viestin lisääminen jonossa. Esimerkki ensin muodostaa yhteyden käyttämällä tallennustilan tilin kontekstissa, joka sisältää tallennustilan tilin nimi ja sen pikanäppäin Azure-tallennustilan. Seuraavaksi hakee määritettyyn jonossa [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet-komennolla. Jos jonossa olemassa, [Uusi objekti](http://technet.microsoft.com/library/hh849885.aspx) cmdlet-komento käytetään [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) -luokan esiintymän luomiseen. Myöhemmin esimerkissä kutsuu [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -menetelmää tämän viestin objektin jono kohtaan. Näin koodi, joka hakee jonon ja lisää viesti "MessageInfo":

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Voit poistaa jonon seuraavan sanoman
Koodin jonot poistaa viestin jonon seuraavalla tavalla. Kun soitat [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) menetelmä, saat seuraavan viestin jonossa. Viestin palauttamien **GetMessage** lukuoikeudet muut viestit luetaan Tämä jono koodi. Viestin poistaminen jonossa lopuksi on myös soittaa [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) -menetelmää. Poistaa viestin tavalla varmistaa, että jos koodi ei käsitellä viestiä laitteiston ja ohjelmiston virheen vuoksi, koodisi esiintymässä voi saman sanoma ja yritä uudelleen. Koodisi kutsuu **DeleteMessage** oikealle, kun viesti on käsitelty.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Azure tiedostoresurssit ja tiedostojen hallinnasta
Azure tiedostojen tallentamisesta on jaettu tallennustilan sovellusten vakio SMB-protokollan avulla. Microsoft Azuren näennäiskoneiden ja pilvipalveluihin jakaa tiedoston tiedot sovelluksen osia kautta liitetyn osakkeet ja paikallisen sovellukset voivat avata jaetun tiedoston tallennustilan API tai PowerShellin Azure-tiedoston tietojen.

Lisätietoja Azure tiedostojen tallentamisesta on artikkelissa [Windows Azure-tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md) ja [Tiedoston palvelun REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Voit määrittää ja tallennustilaa analytics kyselyn
[Azure-tallennustilan analyysin](storage-analytics.md) avulla voit kerätä mittaukset Azure tallennustilan tilien ja lokitiedot tietoja tallennustilan-tiliisi lähetetyt pyynnöt. Tallennustilan arvot avulla voit tarkkailla tallennustilan tilin ja tallennustilaa kirjautuminen ja tilin tallennustilan liittyviä.
Oletusarvon mukaan tallennustilan arvot ei ole käytössä liittyviä palveluja. Voit ottaa seuranta Azure-portaalista tai Windows PowerShellin avulla tai käyttämällä ohjelmallisesti tallennustilan asiakas-kirjasto. Tallennustilan kirjaaminen tapahtuu palvelinpuolen ja mahdollistaa tietuetietoja sekä onnistuneiden ja epäonnistuneiden pyyntöjen tallennustilan tilissäsi. Lokitiedostot avulla voit tarkastella tietoja lukea, kirjoittaa ja poistaa toimintojen vastaan taulukoiden, olevien, ja BLOB-objektit sekä epäonnistuneiden pyyntöjen syyt.

Lisätietoja ottaminen käyttöön ja tarkastella tallennustilan arvot tietoja PowerShellin avulla on artikkelissa [How to enable tallennustilan arvot PowerShellin avulla](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Lisätietoja ottaminen käyttöön ja hakea tallennustilan kirjaaminen tietoja PowerShellin avulla on artikkelissa [ottamisesta käyttöön tallennustilan kirjaaminen PowerShellin avulla](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) ja [tallennustilaa kirjaaminen lokiin tietojen](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Lisätietoja tallennustilan vianmääritys tallennustilan arvot ja tallennustilaa kirjaamisen avulla on artikkelissa [seuranta, Diagnosing, ja vianmääritys Microsoft Azuren tallennustilaan](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Jaettu Access allekirjoitus (SAS) ja tallennettu käyttöoikeuskäytäntö hallinnasta
Jaettu käyttö allekirjoitukset ovat tärkeä osa suojausmalli minkä tahansa-sovelluksen käyttäminen Azure-tallennustilan. Ne ovat hyödyllisiä rajoitetut käyttöoikeudet tallennustilan tiliisi antaminen ohjelmat, joita ei saa olla tili-näppäintä. Oletusarvon mukaan vain tallennustilan tilin omistaja voi käyttää BLOB-objektit, taulukoita ja olevien kyseisen tilin. Jos palvelun tai sovelluksen tarvitsee käyttöön näiden resurssien muiden asiakkaiden ilman jakaminen access-näppäintä, sinulla on kolmesta vaihtoehdosta:

- Säilön käyttöoikeudet sallimaan anonyymi lukuoikeus säilö ja sen BLOB-objektit. Taulukoiden ja olevien ei sallita.
- Käytä myöntää rajoitetut käyttöoikeudet säilöjen, BLOB tai olevien taulukoiden tietyn aikavälin jaetun access-allekirjoitusta.
- Tallennetun access-käytännön avulla voit hankkia lisää taso hallita jaettua käyttöä allekirjoitukset säilön tai sen BLOB-objektit, jono tai taulukossa. Tallennetun access-käytännön avulla voit muuttaa alkamisaika, voimassaolon ajan tai käyttöoikeuksien allekirjoituksen tai kumota sen jälkeen on annettu.

Jaettu käyttö allekirjoituksen voi olla jokin kahdenlaisia:

- **Luonnoslehtiössä SAS**: Kun luot alkamisaika, voimassaolon ajan luonnoslehtiössä SAS ja Suojaussidokset käyttöoikeudet, on määritetty SAS URI. Tällaista SAS voi luoda säilön, blob, taulukko tai jonossa ja se on ei-revokable.
- **Tallennetun käyttöoikeuskäytäntö Suojaussidokset**: tallennetun access-käytäntö on määritetty resurssin säilö blob-säilö, taulukko tai jono - ja sen avulla voit hallita jaettua käyttöä allekirjoituksia rajoitukset. Kun SAS liittää tallennetun käyttöoikeuskäytäntö Suojaussidokset perii rajoitteet - alkamisaika, voimassaolon ajan ja käyttöoikeudet - määritetty tallennetut käyttöoikeuskäytäntö. Tällaista Suojaussidokset on revokable.

Lisätietoja on artikkelissa [Käyttämällä jaettu Access allekirjoitukset (SAS)](storage-dotnet-shared-access-signature-part-1.md) ja [Hallitse anonyymi lukuoikeudet säilöjä ja BLOB-objektit](storage-manage-access-to-resources.md).

Seuraavissa osissa kerrotaan jaettua käyttöä allekirjoituksen tunnuksen ja tallennettu käytännön Azure-taulukoiden luomisesta. Azure PowerShell on samanlainen cmdlet-komennot säilöjen BLOB-objektit ja olevien sekä. Suorita komentosarjat sisältö, Lataa [PowerShellin Azure versio 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) tai uudempi versio.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Käytännön perustuva jaettu Access allekirjoitus-tunnuksen luominen
Uusi AzureStorageTableStoredAccessPolicy cmdlet-komennon avulla voit luoda uuden käytännön tallennetut access. Soita sitten [Uusi AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) cmdlet-komennolla voit luoda uuden käytännön perustuvan jaettujen käyttöoikeuksien allekirjoituksen suojaustunnuksen Azuren tallennustilaan taulukolle.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Luonnoslehtiössä (ei-revokable) jakaa Access allekirjoitus-tunnuksen luominen
[Uusi AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) cmdlet-komennon avulla voit luoda uuden luonnoslehtiössä (ei-revokable) jakaa Access allekirjoituksen tunnuksen Azuren tallennustilaan taulukolle:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Tallennetun access-käytännön luominen
Luo uusi tallennetun access-käytäntö Azuren tallennustilaan taulukolle uusi AzureStorageTableStoredAccessPolicy-cmdlet-komennolla:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Tallennetun käyttöoikeuskäytäntö päivittämisestä
Määritä AzureStorageTableStoredAccessPolicy cmdlet-komennon avulla voit päivittää aiemmin tallennetut access-käytäntö Azuren tallennustilaan taulukolle:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Voit poistaa tallennetut käyttöoikeuskäytäntö
Poista AzureStorageTableStoredAccessPolicy cmdlet-komennon avulla voit poistaa tallennetut käyttöoikeuskäytäntö Azuren tallennustilaan taulukkoon:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Miten voit käyttää US government ja Azure Kiinan Azuren tallennustilaan
Azure-ympäristö on itsenäinen käyttöönoton, Microsoft Azure, kuten [Azure Government US government varten](https://azure.microsoft.com/features/gov/), [Yleinen Azure AzureCloud](https://portal.azure.com)ja [Azure Kiinassa 21vianetin ylläpitämä AzureChinaCloud](http://www.windowsazure.cn/). Voit ottaa käyttöön uuden Azure ympäristöissä amerikkalainen government ja Azure Kiinan.

Jos haluat käyttää Azuren tallennustilaan AzureChinaCloud, haluat luoda tallennustilan kontekstin, joka on liitetty AzureChinaCloud. Toimi näiden ohjeiden avulla pääset alkuun:

1.  Suorittamalla [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) -cmdlet-komento on käytettävissä Azure ympäristöt:

    `Get-AzureEnvironment`

2.  Windows PowerShellin Azure kiina-tilin lisääminen:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Tallennustilan kontekstin AzureChinaCloud-tilin luominen

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Azuren tallennustilaan käytettäväksi [Yhdysvaltain Azure hallituksen](https://azure.microsoft.com/features/gov/), olisi määrittäminen uuteen ympäristöön ja luo sitten uusi tallennustilan kontekstin tämän ympäristön kanssa:

1.  Suorittamalla [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) -cmdlet-komento on käytettävissä Azure ympäristöt:

    `Get-AzureEnvironment`

2.  Windows PowerShellin Azure US Government-tilin lisääminen:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Tallennustilan kontekstin AzureUSGovernment-tilin luominen

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Lisätietoja on artikkelissa:

- [Microsoft Azure Government Sovelluskehittäjän opas](../azure-government-developer-guide.md).
- [Yleistä erot luotaessa sovelluksen kiina-palvelusta](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä oppaassa oppinut Azure PowerShellin Azure-tallennustilan hallinta. Seuraavassa on joitakin liittyvät artikkeleissa ja ohjeaiheissa on lisätietoja niitä.

- [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-tallennustilan PowerShellin cmdlet-komennot](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShell-viittaus](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
