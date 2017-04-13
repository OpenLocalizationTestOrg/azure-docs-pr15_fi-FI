<properties
    pageTitle="Aloita Azure Mobile välitys verkkosovelluksissa | Microsoft Azure"
    description="Opettele käyttämään Azure Mobile välitys analyysin ja push-ilmoitusten Web Apps-sovellusten kanssa."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Aloita Azure Mobile välitys Web Apps-sovellukset

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa kerrotaan, miten Azure Mobile välitys avulla voit selvittää Web App-käyttö.

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Visual Studio 2015 tai jollakin muulla editorilla valittua
+ [Web SDK-paketissa](http://aka.ms/P7b453) 

Web-SDK esikatselu on vain Analytics tukee tällä hetkellä ja ei tue lähettämisen selaimessa tai sovelluskohtaisesta-push-ilmoitukset vielä. 

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Asennuksen Mobile välitys Web App

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic-integroinnin" tarvitse kerää tietoja vähimmäismäärää eli.

Luodaan Visual Studiossa osoittamaan integrointi, mutta voit noudattaa millä tahansa web-sovelluksella luodut ulkopuolella Visual Studio myös basic web App-sovelluksessa. 

###<a name="create-a-new-web-app"></a>Uusi Web-sovelluksen luominen

Seuraavissa vaiheissa oletetaan Visual Studio 2015 käyttöä, vaikka vaiheet ovat samanlaiset Visual Studio aiemmissa versioissa. 

1. Käynnistä Visual Studio ja valitse **Aloitus** -ruudussa **Uusi projekti**.

2. Valitse Ponnahdusvalikossa kohdan **Web** -> **ASP.Net Web-sovelluksen**. Kirjoita sovelluksen **nimi**, **sijainti** ja **ratkaisun nimeä**ja valitse sitten **OK**.

3. Valitse **Valitse malli** -valikko valitsemalla **Tyhjä** **ASP.Net 4.5 mallit** -kohdassa ja valitse **OK**. 

Olet nyt luonut uuden tyhjän Web Appin projektin, johon on integroida Azure Mobile välitys Web SDK-paketissa.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Luo uusi kansio nimeltä **javascript** -ratkaisun ja lisää siihen Web SDK JS tiedoston **azure engagement.js** . 

2. Lisää uusi tiedosto nimeltä **main.js** seuraava koodi javascript kansiossa. Varmista, että päivitettävä yhteysmerkkijono. Tämä `azureEngagement` objektin käytetään Web SDK tavoista käyttää. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio js-tiedostoja][1]

##<a name="enable-real-time-monitoring"></a>Ota reaaliaikainen seuranta

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yksi tehtävä Mobile välitys Taustajärjestelmä. Tehtävän verkkosovellukseen kontekstissa on web-sivu. 

1. Luo uusi sivu nimeltään **home.html** ratkaisu ja tekee siitä web App-ohjelman aloitussivulle. 
2. Lisää kaksi javascripts on lisätty aiemmassa tämän sivun lisäämällä seuraava sisällä leipäteksti-tunniste. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Päivitä Soita EngagementAgent's leipäteksti-tunnisteen `startActivity` menetelmä
        
        <body onload="engagement.agent.startActivity('Home')">

4. Mitä oman **home.html** näyttää
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Laajenna analytics

Seuraavassa on tällä hetkellä käytettävissä Web SDK-paketissa, joita voit käyttää analytics kaikki menetelmät:

1. Toimintojen/sivuston sivut:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Tapahtumat
        
        engagement.agent.sendEvent(name, extras);

3. Virheet

        engagement.agent.sendError(name, extras);

4. Projektit

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

