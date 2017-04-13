<properties 
    pageTitle="Windows Phone Silverlight SDK päivityksen ohjeita" 
    description="Windows Phone Silverlight SDK Azure Mobile välitys päivityksen ohjeita"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK päivityksen ohjeita

Jos jo on vanhempi versio, tutustu SDK on integroitu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Saatat joutua noudata useita ohjeita, jos vastattu SDK useita versioita. Esimerkiksi jos voit siirtää 0.10.1 0.11.0, sinun on ensin noudattamalla "lähettäjä, 0.10.1 0.9.0" sitten "lähettäjä, 0.11.0 0.10.1" kuvatulla tavalla.

##<a name="from-200-to-330"></a>Valitse 2.0.0 3.3.0

### <a name="test-logs"></a>Testaa lokit

Konsolin lokit SDK tuottamat voi nyt käytössä/poissa käytöstä/suodatettu. Voit mukauttaa tätä Päivitä ominaisuus `EngagementAgent.Instance.TestLogEnabled` johonkin käytettävissä arvo `EngagementTestLogLevel` luettelointi, esimerkiksi:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Valitse 1.1.1 2.0.0

Seuraavassa kerrotaan, miten SDK-integroinnin siirtäminen tarjoaman Capptain SAS sovellukseen Azure Mobile välitys tarjoaa Capptain palvelun. 

> [Azure.IMPORTANT] Capptain ja Mobile välitys eivät ole samoja palveluja ja alla kuvatulla tavalla korostaa vain siirtäminen asiakas-sovellus. Siirtyminen SDK-sovelluksessa ei siirretä tietojen Capptain-palvelimien Mobile välitys-palvelimiin

Jos olet siirtymässä aiemmasta versiosta, tilanteeseesi Capptain sivuston siirtäminen 1.1.1 ensin sitten Käytä seuraavasti

### <a name="nuget-package"></a>Nuget-paketti

Korvaa **Capptain.WindowsPhone** **MicrosoftAzure.MobileEngagement** Nuget paketti.

### <a name="applying-mobile-engagement"></a>Käyttämällä Mobile välitys

SDK käytetään termiä `Engagement`. Sinun täytyy päivittää projektin vastaamaan tämän muutoksen.

Sinun on poistettava nykyisen Capptain nuget paketti. Ota huomioon, että kaikki muutokset Capptain resurssit-kansiossa, poistetaan. Jos haluat säilyttää nämä tiedostot ja kopioida ne.

Tämän jälkeen Asenna uusi Microsoft Azure välitys nuget paketti projektiin. Löydät sen suoraan [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement)käyttöön. Tämä toiminto korvaa kaikki välitys käyttämät resurssit-tiedostot ja Lisää uusi välitys DLL projektiviitteet.

Sinulla on siivottava projektiviitteet poistamalla Capptain DLL viittauksia. Jos et tee näin, Capptain versio on ristiriidassa ja virheet tapahtuu.

Jos olet mukauttanut Capptain resursseja, vanha tiedostojen sisältöä kopioida ja liittää ne uusi välitys-tiedostoissa. Huomaa, että xaml- ja cs tiedostot on ehkä päivitettävä.

Kun nämä vaiheet on suoritettu sinun on vain korvaa vanhan Capptain viittaukset uusi välitys viittausten mukaan.

1. Kaikki Capptain nimitilan on päivitettävä.

    Ennen siirtoa:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Kun siirto:
    
        using Microsoft.Azure.Engagement;

2. Kaikki Capptain luokat, jotka sisältävät "Capptain" pitäisi olla "Välitys".

    Ennen siirtoa:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Kun siirto:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml-tiedostot Capptain nimitilan ja määritteet muuttaa myös.

    Ennen siirtoa:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Kun siirto:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Muut resurssit kuten Capptain kuvia Huomaa, että ne myös on nimetty uudelleen käyttämään "Välitys".

### <a name="application-id--sdk-key"></a>Sovellustunnus / SDK avain

Välitys käyttää yhteysmerkkijonon. Sinun ei tarvitse määrittää Mobile välitys tunnus ja SDK-näppäintä, vain on määritettävä yhteysmerkkijonon. Voit määrittää sen EngagementConfiguration-tiedoston.

Välitys-määritys voi määrittää oman `Resources\EngagementConfiguration.xml` projektin tiedosto.

Voit määrittää tämän tiedoston muokkaaminen:

-   Sovelluksen yhteysmerkkijono tunnisteiden väliin `<connectionString>` ja `<\connectionString>`.

Jos haluat määrittää sen suorituksen aikana, voit soittaa ennen välitys agentti alustus seuraavalla tavalla:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy perinteinen Azure-portaalissa.

### <a name="items-name-change"></a>Kohteiden nimen muuttaminen

Kaikkien kohteiden nimeltä *capptain* on nimennyt *välitys*. Lisää vastaavasti varten *Capptain* *välitys*.

Esimerkkejä tavallisimmat Capptain kohteet:

-   Nykyään EngagementConfiguration CapptainConfiguration
-   Nykyään EngagementAgent CapptainAgent
-   Nykyään EngagementReach CapptainReach
-   Nykyään EngagementHttpConfig CapptainHttpConfig
-   Nykyään GetEngagementPageName GetCapptainPageName

Huomaa, että nimeäminen uudelleen myös vaikuttaa ohittaa tavoista.



 
