<properties
    pageTitle="Node.js aloitusopas | Microsoft Azure"
    description="Opettele yksinkertainen Node.js web-sovelluksen luominen ja ota se käyttöön Azure pilvipalvelussa."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Tässä opetusohjelmassa näytetään, miten voit luoda yksinkertaisen Node.js-sovelluksen käytössä Azure-pilvipalvelussa. Cloud Services-palvelut ovat kaavioiden rakennusosia skaalattava cloud sovellusten Azure-tietokannassa. Ne mahdollistavat etäisyyttä, riippumaton hallinta ja sovelluksen edusta- ja osien asteikko-kohtaa.  Pilvipalveluihin avulla tehokkaat erillinen virtual machine isännöinnin rooleille luotettavasti.

Saat lisätietoja pilvipalveluihin ja miten ne poikkeaa Azure sivustoihin ja näennäiskoneiden on artikkelissa [Azure-sivustoja, pilvipalveluihin ja näennäiskoneiden vertailu].

>[AZURE.TIP] Haluatko luoda yksinkertaisen sivuston? Jos käyttämässäsi skenaariossa vain yksinkertaisia sivuston edusta, kannattaa [käyttää kevyt verkkosovellukseen]. Voit helposti päivittää pilvipalveluun koodiin kasvaa ja tarpeen muuttaa.

Noudattamalla tässä opetusohjelmassa voit luoda yksinkertaisen web-sovelluksen Isännöidyt sisäpuolelle web-rooli. Sovelluksen paikallisesti Laske emulaattori avulla ja sitten ottaa raporttinäkymän käyttöön PowerShell komentorivin työkaluilla.

Sovellus on yksinkertainen "Tervetuloa maailman" sovelluksen:

![Hei maailma web-sivun näyttäminen web-selaimessa][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Edellytykset

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään, joka edellyttää Windows PowerShellin Azure.

- Asenna ja määritä [PowerShellin Azure].
- Lataa ja asenna [Azure SDK.NET 2.7]. Valitse asennuksen asetukset-kohdassa:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Azure pilvipalvelussa projektin luominen

Suorita seuraavat toimet voit luoda uuden Azure pilvipalvelussa projektin, basic Node.js rakennustelineet sekä:

1. **Windows PowerShellin** Suorita järjestelmänvalvojana; **Käynnistä-valikko** tai **Aloitusnäytössä**Etsi **Windows PowerShell**.

2. [Yhdistä PowerShell] -tilaukseen.

3. Kirjoita Luo luomaan projektin seuraavan PowerShell cmdlet-komennon:

        New-AzureServiceProject helloworld

    ![Uusi AzureService helloworld-komennon tuloksen][The result of the New-AzureService helloworld command]

    **Uusi AzureServiceProject** cmdlet-komento luo perusrakenteen julkaisemisen Node.js-sovelluksen pilvipalveluun. Se sisältää Azure julkaiseminen tarvittavat määritykset tiedostot. Cmdlet muuttuu käytön hakemiston myös palvelun kansio.

    Cmdlet luo seuraavat tiedostot:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** ja **ServiceDefinition.csdef**: Azure-kohtaisia tiedostoihin, joita tarvitaan sovelluksen julkaisemista varten. Lisätietoja on artikkelissa [Yleistä luominen Azure isännöidään palvelun].

    -   **deploymentSettings.json**: tallentaa paikallisen asetuksia, jotka käyttävät PowerShellin Azure käyttöönoton Cmdlet-komentoja.

4.  Kirjoita Lisää uusi web-roolin seuraava komento:

        Add-AzureNodeWebRole

    ![Lisää AzureNodeWebRole-komento][The output of the Add-AzureNodeWebRole command]

    **Lisää AzureNodeWebRole** cmdlet-komento luo basic Node.js-sovellus. Lisääminen Muokkaa myös, jos haluat lisätä uuden roolin määrittäminen tapahtumille **.csfg** ja **.csdef** tiedostot.

    > [AZURE.NOTE] Jos roolinimi ei määritetä, käytetään oletusarvoista nimeä. Voit määrittää ensimmäisen cmdlet-parametrin nimi:`Add-AzureNodeWebRole MyRole`

Node.js-sovellus on määritetty tiedoston **server.js**, joka sijaitsee siinä hakemistossa web-roolin (**WebRole1** oletusarvoisesti). Näin koodi:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Koodin arvo on käytettävä sama kuin "Hei-maailman" otosten [nodejs.org] -sivustossa mutta se käyttää porttinumero määrittämän cloud-ympäristössä.

## <a name="deploy-the-application-to-azure"></a>Azure sovelluksen käyttöönotto

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Lataa Azure julkaisemisen asetukset

Ottaa käyttöön Azure sovellusta, sinun on ladattava julkaisun asetukset Azure tilauksen.

1.  Suorita seuraavat Azure PowerShell cmdlet-komennon:

        Get-AzurePublishSettingsFile

    Tämä Julkaise asetukset lataussivulle Siirry selaimen avulla. Sinua voidaan pyytää kirjautumaan sisään Microsoft-Account. Jos näin on, käytä Azure-tilaukseen liittyvää tiliä.

    Tallenna ladattu profiili on helposti saatavilla sijaintiin.

2.  Suorita seuraavat cmdlet-komento, tuo lataamaasi julkaisun profiilin:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Olet tuonut Julkaise-asetukset ja harkitse poistamista ladatut .publishSettings tiedostoa, koska se sisältää tiedot, joita saattaa hallintaoikeuden antaminen toiselle tiliäsi.

### <a name="publish-the-application"></a>Sovelluksen julkaiseminen

Jos haluat julkaista, suorita seuraavat komennot:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-Palvelun nimi** määrittää nimen käyttöönottoa varten. Tämä on oltava yksilöivä nimi, muuten julkaisuprosessin epäonnistuu. Valitse päivämäärä ja kellonaika-merkkijono, joka pitäisi tehdä nimi yksilöllisen **Get-päivämäärä** -komento muut.

- **-Sijaintiin** määrittää palvelinkeskukseen, sovelluksen sovelluksessa. Saat luettelon käytettävissä palvelinkeskusten, käytä **Get-AzureLocation** cmdlet-komento.

- **-Käynnistä** selain avautuu ja siirtyy isännöityä palvelun käyttöönoton jälkeen.

Kun julkaiseminen on muodostettu, näet vastausta seuraavankaltaiselta:

![Julkaise AzureService-komento][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Voi kestää useita minuutteja sovelluksen käyttöönotto ja ole käytettävissä, kun se julkaistaan.

Kun asennus on valmis, selainikkunassa Avaa ja siirry pilvipalvelussa.

![Hei maailma sivun näyttäminen selainikkunassa. URL-osoite näkyy sivun isännöidään Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

Sovellus on nyt käytössä Azure.

**Julkaise AzureServiceProject** cmdlet-komento tekee seuraavat toimet:

1.  Luo paketin käyttöönotto. Paketti sisältää kaikki sovelluksen-kansiossa olevat tiedostot.

2.  Luo uusi **tallennustilan tilin** , jos sellaista ei ole. Azure-tallennustilan tilin käytetään tallentamiseen sovelluspaketin käyttöönoton aikana. Voit poistaa tilin tallennustilan turvallisesti käyttöönoton jälkeen.

3.  Luo uusi **pilvipalvelussa** , jos sellaista ei ole jo. **Käytössäsi** on säilö, joka sovelluksesi sijaitsee, kun se on otettu käyttöön Azure. Lisätietoja on artikkelissa [Yleistä luominen Azure isännöidään palvelun].

4.  Julkaisee asennuspaketin Azure.


## <a name="stopping-and-deleting-your-application"></a>Lopettaminen ja sovelluksen poistaminen

Jälkeen käyttöönotto sovelluksen, voit poistaa sen käytöstä, jotta voit välttää lisäkustannuksia. Azure laskut web-roolin esiintymät palvelimen ajan tunnissa. Palvelimen kellon käytetään, kun sovellus otetaan käyttöön, vaikka esiintymät ei ole käynnissä ja pysäytetty tilassa.

1.  Windows PowerShell-ikkunassa estää palvelun käyttöönoton edellisessä osassa seuraavan cmdlet-komennon avulla luotu:

        Stop-AzureService

    Palvelun pysäyttäminen voi kestää useita minuutteja. Kun palvelu on pysäytetty, näyttöön tulee sanoma, että se on lopetettu.

    ![Lopeta AzureService-komennon tila][The status of the Stop-AzureService command]

2.  Jos haluat poistaa palvelun, Soita seuraavan cmdlet-komennon:

        Remove-AzureService

    Kirjoita pyydettäessä **Y** palvelun poistaminen.

    Palvelun poistaminen saattaa kestää useita minuutteja. Kun palvelu on poistettu näyttöön tulee sanoma, joka ilmaisee, että palvelu on poistettu.

    ![Poista AzureService-komennon tila][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Palvelun poistaminen ei poista tallennustilan tilin, joka on luotu, kun palvelua julkaistiin ja tallennustilaa laskutetaan säilyvät. Jos mitään muuta käyttää tallennustilaa, haluat ehkä poistaa sen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Node.js Developer Center].

<!-- URL List -->

[Azuren näennäiskoneiden sivustot ja pilvipalveluihin vertailu]: ../app-service-web/choose-web-site-cloud-service-vm.md
[Kevyt web Appin avulla]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[.NET 2.7 Azure SDK]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Yhdistä PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Yleistä Azure isännöityä palvelun luomisesta]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
