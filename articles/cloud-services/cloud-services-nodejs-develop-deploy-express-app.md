<properties 
    pageTitle="Web-sovelluksen Expressin (Node.js) | Microsoft Azure" 
    description="Opetusohjelma, joka perustuu cloud service-opetusohjelma ja esitellään Express-moduulin käyttämisestä." 
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






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Express käyttäminen Azure-pilvipalvelussa Node.js web-sovelluksen luominen

Node.js sisältää toimintoja vähimmäismäärää core suorituksen aikana.
Kehittäjät käyttää usein 3 osapuolen moduulit antamaan lisätoimintoja, kun Node.js-sovelluksen. Tässä opetusohjelmassa luot uuden sovelluksen [Express][] -moduulin, joka tarjoaa MVC framework Node.js web-sovellusten luomisesta.

Näyttökuva valmiin sovelluksen on alla:

![Tervetuloa näyttäminen Express Azure-web-selaimessa](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Cloud palvelun projektin luominen

Seuraavien toimien voit luoda uuden cloud palvelun projekti nimeltä "expressapp":

1. **Käynnistä-valikko** tai **Aloitusnäytössä**Etsi **Windows PowerShell**. Lopuksi **Windows PowerShellin** hiiren kakkospainikkeella ja valitse **Suorita järjestelmänvalvojana**.

    ![Azure PowerShell-kuvake](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Muuta hakemistoja **c:\\solmu** directory ja kirjoita sitten seuraavat komennot voit luoda uuden ratkaisun nimeltä **expressapp** ja **WebRole1**-roolia:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] **Lisää AzureNodeWebRole** käyttää oletusarvon mukaan Node.js vanhempi versio. Yllä **Määrittäminen AzureServiceProjectRole** lauseen ohjaa Azure käyttämään v0.10.21 solmun.  Huomautus parametrit kirjainkoko on merkitsevä.  Voit varmistaa oikean version Node.js on valittu valitsemalla **WebRole1\package.json** **ohjelmien** -ominaisuutta.

##<a name="install-express"></a>Asenna Express

1. Asenna Express-luontitoiminnon kirjoittamalla seuraava komento:

        PS C:\node\expressapp> npm install express-generator -g

    Npm-komennon pitäisi nyt muistuttaa alla tulokseen. 

    ![Windows PowerShell npm tulosteen näyttäminen Asenna pika-komentoa.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Vaihda kansioita **WebRole1** hakemiston ja pika-komennon avulla voit luoda uuden sovelluksen:

        PS C:\node\expressapp\WebRole1> express

    Voit pyydetään korvaamaan aiemman sovelluksen. Kirjoita **y** tai **Kyllä** , jatka. Express Luo app.js-tiedosto ja kansiorakenne etsimisen sovelluksen.

    ![Pika-komento](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Jos haluat asentaa muita riippuvuuksia määritetty package.json-tiedostossa, kirjoita seuraava komento:

        PS C:\node\expressapp\WebRole1> npm install

    ![Tulos npm Asenna komento](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  Seuraavalla komennolla kopioi **server.js** **bin/www** -tiedosto. Tämä on niin, pilvipalvelussa löydät aloituskohdan tämän sovelluksen.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Kun toiminto on valmis, taulukossa pitäisi olla **server.js** tiedoston WebRole1 hakemistossa.

7.  Muokkaa **server.js** , voit poistaa jonkin ".' seuraava rivi merkkejä.

        var app = require('../app');

    Kun olet tehnyt tämän muutoksen, rivin pitäisi näkyä seuraavasti.

        var app = require('./app');

    Tämä muutos tarvitaan jälkeen on siirretty (aiemmalta nimeltään **bin-WWW-tunnistetta**,)-tiedoston samaan kansioon sovelluksen-tiedostona edellytetään. Kun olet tehnyt tämän muutoksen, Tallenna **server.js** -tiedosto.

8.  Käytä seuraavaa komentoa Azure emulaattorin sovelluksen suorittamiseen:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Web-sivun Tervetuloa express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Näkymän muokkaaminen

Nyt voit muuttaa näytettävä sanoma "Tervetuloa, Express-Azure".

1.  Kirjoita seuraava komento index.jade-tiedoston avaaminen:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![Index.jade-tiedoston sisällön.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade on oletusarvon näkymä-ohjelma käyttää Express sovellusten. Lisätietoja Jade näkymä-ohjelma on artikkelissa [http://jade-lang.com][].

2.  Muokkaa teksti rivin liittämällä **Azure-tietokannassa**.

    ![Index.jade-tiedoston viimeisen rivin lukee: p Tervetuloa \#{title} Azure-tietokannassa](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Tallenna tiedosto ja sulje Muistio.

4.  Päivitä selain ja näet muutokset.

    ![Selain-sivu sisältää Tervetuloa Express Azure-tietokannassa](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Sovelluksen testauksen jälkeen Lopeta emulaattori **Stop AzureEmulator** cmdlet-komennon avulla.

##<a name="publishing-the-application-to-azure"></a>Azure sovelluksen julkaiseminen

PowerShellin Azure-ikkunassa **Julkaise AzureServiceProject** -cmdlet sovellus käyttöön pilvipalveluun

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Kun käyttöönotto-toiminto päättyy, selaimen Avaa ja avaa web-sivu.

![Express-sivun näyttämistä selain. URL-osoite näkyy sitä isännöidään nyt Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Node.js Developer Center](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-Lang.com]: http://jade-lang.com

 
