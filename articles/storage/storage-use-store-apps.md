<properties
    pageTitle="Azure-tallennustilan käyttäminen Windows-kaupan sovellukset | Microsoft Azure"
    description="Opettele luomaan Windows-kaupan sovellus, joka käyttää Azure-Blob, jonossa, taulukko tai tiedoston tallennuspaikkaa."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Azuren tallennustilaan käyttämisestä Windows-kaupan sovellukset

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa kerrotaan, miten aloittaa kehittäminen Windows-kaupan sovellus, jossa käytetään Azure-tallennustilan.

## <a name="download-required-tools"></a>Lataa tarvittavat työkalut

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) on helppo luoda, korjata, lokalisoidaan-paketti, ja otetaan käyttöön Windows-kaupan sovellukset. Visual Studio 2012 tai uudempi versio vaaditaan.
- [Azure tallennustilan asiakkaan kirjasto](https://www.nuget.org/packages/WindowsAzure.Storage) sisältää Windows Runtimen luokkakirjasto käsittelyyn Azure-tallennustilan.
- [WCF Data Services Työkalut varten Windows kaupan sovellukset](http://www.microsoft.com/download/details.aspx?id=30714) laajentaa asiakkaan OData-tuki Windows-kaupan sovellukset Visual Studiossa palvelun viitteen lisääminen kokemusta.

## <a name="develop-apps"></a>Kehitä sovelluksia

### <a name="getting-ready"></a>Valmistautuminen

Luo uusi Windows-kaupan sovelluksen projekti-Visual Studio 2012 tai sitä uudemmassa versiossa:

![myymälä-sovellukset-tallennustilan-ja-projekti][store-apps-storage-vs-project]

Lisää seuraavaksi **viittaukset**hiiren kakkospainikkeella, valitsemalla **Lisää viittaus**ja selaamalla sitten-tallennustilan kirjaston for Windowsin ajonaikaista olet ladannut viittauksen Azure-tallennustilan asiakas-kirjastoon:

![Store-sovellukset-tallennustilan-Valitse-kirjasto][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Kirjaston käytöstä Blob-objektien ja jono-palveluihin

Tässä vaiheessa sovellus on valmis Soita Azure-Blob-objektien ja jono-palvelut. Lisää seuraavista **käyttämällä** väittämistä niin, että Azure sijainteihin viittaa suoraan:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Lisää seuraavaksi painike sivulle. Lisää seuraava koodi **sitten** sen tapahtuman ja muokata oman Tapahtumakäsittelijämenetelmä [asynkroninen avainsanan](http://msdn.microsoft.com/library/vstudio/hh156513.aspx)avulla:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Tämä koodi oletetaan, että kaksi merkkijonomuuttujat, *accountName* ja *accountKey*. Ne edustavat tallennustilan tilin ja tilin avainta, joka on liitetty tilin nimiä.

Muodosta ja suorita sovellus. Vaihtoehdon Tarkista, onko tilisi säilö *container1* ja luo se, jos näin ei ole.

### <a name="using-the-library-with-the-table-service"></a>Kirjaston käytöstä taulukon-palvelussa

Tiedostotyypit, joita käytetään Azure-taulukosta-palvelun kanssa kommunikoimiseen määräytyvät WCF-tietopalvelut Windows-kaupan sovelluksen kirjaston. Lisää seuraavaksi tarvittavat WCF-kirjastot viittaus Package hallinta-konsolin avulla:

![kaupan sovellukset-tallennustilan-paketti-hallinta][store-apps-storage-package-manager]

Käytä seuraavaa komentoa pisteen Package Manager tietokoneen sijaintiin:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Tämä komento lisää automaattisesti kaikki tarvittavat viittaukset projektiin. Jos et halua käyttää paketin hallinta-konsolin, voit lisätä WCF Data Services NuGet kansio paikallisessa tietokoneessa paketin lähteisiin ja Lisää viittaus Käyttöliittymän välityksellä kuvatulla tavalla [Hallinta NuGet pakettien-valintaikkunan avulla](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Kun on viitattu WCF Data Services NuGet-paketti, muuta sitten painiketta **napsauttamalla** tapahtuman koodi:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Koodi tarkistaa, onko tilisi *table1* -taulukon, ja luo sen, jos näin ei ole.

Voit myös lisätä viittauksen Microsoft.WindowsAzure.Storage.Table.dll, joka on käytettävissä, jotka olet ladannut paketissa. Tämä kirjasto sisältää muita toimintoja, kuten heijastus-pohjainen Sarjatoiminto ja Yleinen kyselyt. Huomaa, että tämä kirjasto ei tue JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
