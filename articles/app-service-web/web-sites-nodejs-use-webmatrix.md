<properties 
    pageTitle="Muodosta ja ota käyttöön Node.js verkkosovellukseen Azure WebMatrix käyttäminen" 
    description="Opetusohjelma, avulla opit käyttämään WebMatrix kehittää Node.js-sovellus ja Azure App palvelun Web Apps-sovellusten käyttöön." 
    services="app-service\web" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Muodosta ja ota käyttöön Node.js verkkosovellukseen Azure WebMatrix käyttäminen

Tässä opetusohjelmassa näytetään, miten WebMatrix avulla voit kehittää Node.js-sovellus ja [Azure App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps-sovellusten käyttöön. WebMatrix on ilmainen web kehitysympäristö Microsoftilta, joka sisältää kaikki sivuston tai web app-kehitystä varten. WebMatrix sisältää useita ominaisuuksia, jotka helpottavat esimerkiksi koodi valmistuminen valmiita malleja ja Jade-editorin tuki vähemmän helppokäyttöisiä Node.js ja CoffeeScript. Lisätietoja [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Kun olet suorittanut tämän oppaan, on käynnissä Azure App palvelun Node.js verkkosovellukseen.
 
Näyttökuva valmiin sovelluksen on alla:

![Azure solmu Web-sivusto][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="sign-into-azure"></a>Kirjaudu sisään Azure

Web-sovelluksen luominen Azure App palvelussa seuraavasti.

1. Käynnistä WebMatrix
2. Jos tämä on ensimmäinen kerta käyttänyt WebMatrix, näyttöön tulee kehotus Azure kirjautumalla.  Voit, valitse **Kirjaudu sisään** -painike ja valitse **Lisää tili**.  Valitse **Kirjaudu sisään** käyttämällä Microsoft-Account.

    ![Tilin lisääminen][addaccount]

3. Jos olet rekisteröinyt Azure-tili, voi kirjautua sisään käyttämällä Microsoft Account:

    ![Kirjaudu sisään Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Sisäisten mallin avulla saat Azure sivuston luominen

1. Aloitusnäytön **Uusi** -painiketta ja valitse **Mallit-osa** Luo uuden sivuston mallivalikoimasta:

    ![Uuden sivuston mallivalikoimasta][sitefromtemplate]

2. **Sivuston mallista** -valintaikkunassa valitse **solmu** ja valitse sitten **Express sivuston**. Valitse lopuksi **Seuraava**. Jos jokin edellytyksistä **Express** sivustomallin puuttuu, sinun tulee kehotus asentaa ne.

    ![Valitse pika-malli][webmatrix-templates]

3. Jos olet kirjautunut Azure, sinulla on nyt vaihtoehto App palvelun web-sovelluksen paikallisen sivuston luominen.  Valitse yksilöllinen nimi ja valitse palvelinkeskukseen, jossa haluat luoda sovelluksen palvelun koodiin: 

    ![Sivuston luominen Azure][nodesitefromtemplateazure]
    
4. Kun WebMatrix on tehty muodostaminen paikalliseen sivustoon ja sovelluksen palvelun web-sovelluksen, näkyviin tulee WebMatrix IDE.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Julkaise sovelluksesi Azure

1. Valitse **Julkaise** WebMatrix, **Home** -valintanauhasta Näytä sivuston **Julkaiseminen esikatselu** -valintaikkunassa.

    ![Julkaise esikatselu][webmatrix-node-publishpreview]

2. Valitse **Jatka**. Kun julkaiseminen on valmis, sovelluksen palvelun web Appin URL-osoite näkyy WebMatrix IDE alareunassa

    ![Julkaise valmis][webmatrix-publish-complete]

3. Valitse linkki sovelluksen palvelun web Appin Avaa selaimessa.

    ![Express web Appissa][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Muokata ja julkaista sovelluksesi

Voit muokata ja julkaista sovelluksesi uudelleen. Tässä tekemäsi yksinkertaisen **index.jade** tiedoston otsikon muuttaminen ja julkaise sovellus.

1. Valitse WebMatrix **tiedostoja**ja laajenna **näkymät** -kansioon. Avaa **index.jade** tiedosto kaksoisnapsauttamalla sitä.

    ![webmatrix tarkasteleminen index.jade][webmatrix-modify-index]

2. Muuta kappaleen rivin seuraavasti:

        p Welcome to #{title} with WebMatrix on Azure!

3. Tallenna muutokset ja valitse sitten Julkaise-kuvaketta. Lopuksi valitsemalla **Julkaise esikatselu** -valintaikkunassa **Jatka** ja odota, kunnes julkaistaan päivityksen.

    ![Julkaise esikatselu][webmatrix-republish]

4. Kun julkaiseminen on valmis, käytä palautti julkaisuprosessin päätyttyä on päivitetty sovellus palvelun online-linkkiä.

    ![Azure solmu web Appissa][webmatrix-node-completed]

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure ja voit määrittää, jota käytetään sovelluksen version mukana toimitettuja Node.js versiot-kohdassa [määrittäminen Node.js versio Azure-sovelluksessa](../nodejs-specify-node-version-azure-apps.md).

Jos kohtaat ongelmia sovelluksesi sen jälkeen, kun se on otettu käyttöön Azure, katso, [miten voit korjata Node.js verkkosovellukseen Azure-sovelluksen palvelun](web-sites-nodejs-debug.md) lisätietoja ohjelmistossa ongelma.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 