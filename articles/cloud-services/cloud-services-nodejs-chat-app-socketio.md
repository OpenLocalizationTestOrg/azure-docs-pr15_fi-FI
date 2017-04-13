<properties 
    pageTitle="Node.js-sovelluksesta valitsemalla Socket.io | Microsoft Azure" 
    description="Opettele käyttämään socket.io isännöimät Azure node.js-sovelluksessa." 
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
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Muodosta Node.js keskustelu-sovellusta, jonka Socket.IO Azure pilvipalvelussa

Socket.IO on reaaliaikainen välisessä node.js palvelimen ja asiakkaiden välillä. Tässä opetusohjelmassa edetään ohjatusti isännöivän socket. IO perusteella Azure keskustelu-sovellus. Saat lisätietoja Socket.IO <http://socket.io/>.

Näyttökuva valmiin sovelluksen on alla:

![Näyttäminen isännöimät Azure palvelun selainikkunassa.][completed-app]  

## <a name="prerequisites"></a>Edellytykset

Varmista, että seuraavat tuotteet ja versiot on asennettu suorittaminen tämän artikkelin Esimerkki:

* [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) : n asentaminen
* Asenna [Node.js](https://nodejs.org/download/)
* [Python version 2.7.10](https://www.python.org/) asentaminen

## <a name="create-a-cloud-service-project"></a>Cloud palvelun projektin luominen

Seuraavat vaiheet Luo cloud palvelun projekti, joka isännöi Socket.IO-sovelluksen.

1. **Käynnistä-valikko** tai **Aloitusnäytössä**Etsi **Windows PowerShell**. Lopuksi **Windows PowerShellin** hiiren kakkospainikkeella ja valitse **Suorita järjestelmänvalvojana**.

    ![Azure PowerShell-kuvake][powershell-menu]

2. Luo kansio nimeltä **c:\\solmu**. 
 
        PS C:\> md node

3. Muuta hakemistoja **c:\\solmu** hakemisto
 
        PS C:\> cd node

4. Kirjoita uuden ratkaisun nimeltä **chatapp** ja työntekijä roolia **WorkerRole1**seuraavista komennoista:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    Näyttöön tulee seuraava vastaus:

    ![Uuden azureservice ja lisää azurenodeworkerrolecmdlets tulos](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Lataa keskustelu-Esimerkki

Käytämme [Socket.IO GitHub säilöön]keskustelu esimerkiksi tälle projektille. Seuraavien toimien esimerkin lataaminen ja sen lisääminen aiemmin luotu projekti.

1.  Säilön paikallisen kopion luominen **Kloonaa** -painikkeella. **ZIP** -painikkeen avulla voi myös ladata projektin.

    ![Selainikkunassa tarkasteleminen https://github.com/LearnBoost/socket.io/tree/master/examples/chat, ZIP-Lataa-kuvake korostettuna][chat-example-view]

3.  Siirry paikallisen säilön directory rakenteen, kunnes olet päässyt **esimerkkejä\\keskustelu** hakemisto. Kopioi tämän kansion sisältö **C:\\solmu\\chatapp\\WorkerRole1** aiemmin luotu kansio.

    ![Esimerkkejä sisällön näyttäminen Resurssienhallinnassa\\poimittujen arkisto keskustelu-kansio][chat-contents]

    Yllä olevassa näyttökuvassa Korostetut kohteet ovat kopioitu tiedostoja **esimerkkejä\\keskustelu** hakemisto

4.  Valitse **C:\\solmu\\chatapp\\WorkerRole1** kansioon, poista **server.js** -tiedosto ja nimeä uudelleen **app.js** tiedoston **server.js**. Tämä poistaa oletusarvon **server.js** tiedoston aiemmin luonut **Lisää AzureNodeWorkerRole** cmdlet-komento ja korvaa se keskustelu-Esimerkki sovelluksen tiedoston.

### <a name="modify-serverjs-and-install-modules"></a>Muokkaa Server.js ja asenna moduulit

Ennen testaamista sovelluksen Azure emulaattorin, emme on tehtävä joitakin pieniä muutoksia. Seuraavien toimien server.js-tiedosto:

1.  Avaa Visual Studiossa tai tahansa tekstieditorilla **server.js** -tiedosto.

2.  Määrittää ja etsiä **moduulin riippuvuudet** osan alussa server.js rivi, joka sisältää **sio = require('.. //.. lib//Socket.IO ")** , **sio = require('socket.io')** alla kuvatulla tavalla:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Jotta sovelluksen seuraa portin avaamalla server.js Muistioon tai tuttuja editorin ja muuta sitten seuraavan rivin korvaamalla **3000** **process.env.port** alla kuvatulla tavalla:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Kun olet tallentanut muutokset **server.js**, seuraavien vaiheiden avulla voit asentaa tarvittavat moduulit ja testaa sovelluksen Azure emulaattorin:

1.  **PowerShellin Azure**, muuta hakemistoja **C:\\solmu\\chatapp\\WorkerRole1** hakemistosta ja käytä seuraava komento moduulit vaatii tämän sovelluksen asentamiseksi:

        PS C:\node\chatapp\WorkerRole1> npm install

    Asennetaan moduulit-package.json-tiedostossa. Komennon jälkeen pitäisi näkyä tulosteen seuraavankaltaiselta:

    ![Tulos npm Asenna komento][The-output-of-the-npm-install-command]

4.  Koska tässä esimerkissä on alun perin Socket.IO GitHub säilö osa ja Socket.IO kirjaston viittaavat suoraan suhteellinen polku, Socket.IO ei viittausta package.json-tiedostossa on täytyy asentaa sen lähettämällä seuraava komento:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testaa ja käyttöönotto

1.  Käynnistä emulaattori kirjoittamalla seuraava komento:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Avaa selain ja siirry **http://127.0.0.1**.

3.  Kun selaimen ikkuna avautuu, Kirjoita kutsumanimi ja anna osumien.
    Tämä on kaikki voit kirjata viestit tiettyyn kutsumanimi. Voit testata usean käyttäjän toimintoja, Avaa URL-Osoitetta käyttämällä Muut selainikkunat ja kirjoita eri lempinimiä.

    ![Näyttää viestit Käyttäjä1 ja käyttäjä2 kaksi selainikkunat](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Sovelluksen testauksen jälkeen pysäyttää emulaattori kirjoittamalla seuraava komento:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Ottaa käyttöön sovelluksen Azure **Julkaise AzureServiceProject** cmdlet-komennon avulla Esimerkki:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Varmista, että Käytä yksilöivä nimi, muuten julkaisuprosessin epäonnistuu. Kun asennus on valmis, selain avaa ja siirry käyttöön palvelun.
    > 
    > Jos näyttöön tulee virhesanoma, jossa sanotaan, että annettu tilauksen nimi ei ole tuotu Julkaise-profiili, lataa ja tuo julkaisun profiilin tilauksen ennen kuin otat käyttöön Azure. Katso [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/) **Azure sovelluksen käyttöönotto** -osio

    ![Näyttäminen isännöimät Azure palvelun selainikkunassa.][completed-app]

    > [AZURE.NOTE] Jos näyttöön tulee virhesanoma, jossa sanotaan, että annettu tilauksen nimi ei ole tuotu Julkaise-profiili, lataa ja tuo julkaisun profiilin tilauksen ennen kuin otat käyttöön Azure. Katso [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/) **Azure sovelluksen käyttöönotto** -osio

Sovellus on nyt käytössä Azure ja voit välittää keskustelun viestejä eri asiakkaita käyttämällä Socket.IO välillä.

> [AZURE.NOTE] Yksinkertaisuuden tässä esimerkissä on rajoitettu keskusteleminen yhteydessä samassa esiintymässä käyttäjien välillä. Tämä tarkoittaa, että jos pilvipalvelussa sijaitsevaan Luo kaksi työntekijän rooli esiintymää, käyttäjien vain voi keskusteleminen muiden yhteydessä työntekijän rooli samassa esiintymässä. Skaalata sovelluksen toimimaan roolin useita kertoja, voi jakaa Socket.IO kaupan tilan eri esiintymien välillä tekniikka, kuten palvelun Bus avulla. Esimerkkejä on palvelun Bus olevien ja aiheet käyttö esimerkkejä [Azure SDK Node.js GitHub säilöön](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit isännöidään Azure-pilvipalvelussa basic keskustelu-sovelluksen luominen. Lisätietoja isännöidä tämän sovelluksen Azure-sivustossa on artikkelissa [Node.js Chat sovellus, jolla Socket.IO luodun Azure Web-sivuston luominen][chatwebsite].

Lisätietoja on Katso myös [Node.js Developer Center](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub säilöön]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
