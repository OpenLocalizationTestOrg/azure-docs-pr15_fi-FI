<properties
    pageTitle="Web-sovelluksen luominen Azure Marketplace | Microsoft Azure"
    description="Lue, miten voit luoda uuden WordPress verkkosovellukseen Azure-portaalissa Azure Marketplacesta."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Web-sovelluksen luominen Azure Marketplace-sivustosta

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure Marketplacesta on käytettävissä monien suosittujen web Apps-sovellusten kehittämä Microsoft kolmansien osapuolien ja Avaa lähde ohjelmiston hankkeita. Esimerkiksi WordPress, Umbraco CMS, Drupal, jne. Web-sovellukset on suunniteltu monien suosittujen kehysten, kuten tässä WordPress esimerkki, [.NET], [Node.js], [Java]ja [Python], muun muassa [PHP] käyttöön. Web-sovelluksen luominen Azure Marketplacesta, tarvitset vain ohjelmisto on selaimessa, jota käytetään [Azure-portaalissa].

Tässä opetusohjelmassa opit miten:

* Etsi ja web App-sovelluksen luominen Azure App palvelu, joka perustuu Azure Marketplace-malliin.
* Uusi web-sovelluksen Azure App palvelun asetusten määrittäminen.
* Käynnistä ja hallita web Appissa.

Tässä opetusohjelmassa varten käyttöönottoa WordPress Blogisivuston Azure Marketplacesta. Kun olet suorittanut vaiheet Tässä opetusohjelmassa, sinun on määrittäminen WordPress oman sivuston ja suorittamalla pilveen.

![Esimerkki WordPress wep sovelluksen koontinäyttö][WordPressDashboard1]

WordPress-sivusto, joka käyttöön tässä opetusohjelmassa käyttää MySQL tietokantaan. Jos haluat käyttää sen sijaan tietokannalle SQL-tietokantaan, katso [Projektin Nami], joka on myös käytettävissä Azure Marketplacesta.

> [AZURE.NOTE]
> Tässä opetusohjelmassa suorittamiseen tarvitset Microsoft Azure-tiliin. Jos sinulla ei ole tiliä, voit [aktivoida Visual Studio tilaajan etuja] [ activate] tai [maksuttoman kokeiluversion käyttäjäksi rekisteröityminen][free trial].
>
> Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App kääntäjä]. Sieltä voit luoda lyhytkestoinen starter verkkosovellukseen App palvelun heti, ei ole luottokorttia ja ei ole sitoumukset.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Etsi ja luo verkkosovellukseen Azure sovelluksen-palvelussa

1. Kirjaudu sisään [Azure Portal].

1. Valitse **Uusi**.
    
    ![Luo uusi Azure resurssi][MarketplaceStart]
    
1. Etsi **WordPress**ja valitse sitten **WordPress**. (Jos haluat käyttää SQL-tietokantaan sen sijaan, että MySQL-etsiä **Projektin Nami**.)

    ![Etsi WordPress Marketplacesta][MarketplaceSearch]
    
1. Luettuasi WordPress sovelluksen kuvausta valitsemalla **Luo**.

    ![Luo WordPress web app][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Uusi Web-sovellus App Azure palvelun asetusten määrittäminen

1. Kun olet luonut uuden online-WordPress asetukset-sivu näkyvät, jotka voit tehdä seuraavat toimet:

    ![WordPress web app-asetusten määrittäminen][ConfigStart]

1. Kirjoita nimi web-sovelluksen **Web app** -ruutuun.

    Tämän nimen on oltava yksilöllinen azurewebsites.net toimialueen, koska web Appin URL-osoite on *{name}*. azurewebsites.net. Jos kirjoittamasi nimi ei ole yksilöllisiä, punainen huutomerkki näkyy teksti-ruutuun.

    ![Määritä WordPress web app-nimi][ConfigAppName]

1. Jos sinulla on useita tilauksia, valitse haluamasi vaihtoehto, jota haluat käyttää. 

    ![Tilauksen web-sovelluksen määrittäminen][ConfigSubscription]

1. Valitse **Resurssiryhmä** tai luoda uuden.

    Katso lisätietoja resurssiryhmiin liittyviä [Azure resurssin hallinnassa: yleiskatsaus][ResourceGroups].

    ![Resurssiryhmä web-sovelluksen määrittäminen][ConfigResourceGroup]

1. Valitse **Sovelluksen palvelun suunnitelman/sijainti** tai luoda uuden.

    Saat lisätietoja sovelluksen palvelusopimusten vaihtoehdot artikkelissa [Azure App palvelun suunnitelmien yleiskatsaus][AzureAppServicePlans]. 

    ![Palvelusopimus web-sovelluksen määrittäminen][ConfigServicePlan]

1. Valitse **tietokanta**ja anna sitten tarvittavat arvot **Uuteen MySQL-tietokantaan** sivu määrittämiseen MySQL-tietokantaan.

    a. Kirjoita uusi nimi tai jätä oletusnimeä.

    b. Jätä arvoksi **jaetun** **Tietokannan laji** .

    c-näppäinyhdistelmää. Valitse valitsemasi web-sovelluksen samaan sijaintiin.

    d. Valitse hinnoittelu taso. Tässä opetusohjelmassa on hieno **elohopeaa** – joka on ilmainen mahdollisimman vähän yhteydet ja levytilan.

    e. Valitse **Uusi MySQL-tietokantaan** , sivu juridiset ehdot ja valitse sitten **OK**. 

    ![Web-sovelluksen tietokanta-asetusten määrittäminen][ConfigDatabase]

1. **WordPress** -sivu-juridiset ehdot ja valitse sitten **Luo**. 

    ![Lopeta web app-asetukset ja valitse OK][ConfigFinished]

    Azure App palvelun Luo verkkosovellus, yleensä alle minuutissa. Voit katsoa edistymistä napsauttamalla portaalin sivun yläosassa Bellin-kuvaketta.

    ![Tilanneilmaisin][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Käynnistä ja hallita WordPress koodiin
    
1. Kun web-sovelluksen luominen on valmis, siirry Azure portaalin resurssiryhmä, jossa sovellus on luotu ja näet web app- ja tietokanta.

    Hehkulampun kuvakkeella ylimääräisiä resurssi on [Hakemuksen tiedot][ApplicationInsights], joka tarjoaa seurantaa web Appissa.

1. **Resurssiryhmä** -sivu valitsemalla web app-rivi.

    ![Valitse WordPress koodiin][WordPressSelect]

1. Valitse Web app-sivu valitsemalla **Selaa**.

    ![Selaa WordPress web App-sovellukseen][WordPressBrowse]

1. Jos sinua kehotetaan WordPress blogin kieli, valitse haluamasi kieli ja valitse sitten **Jatka**.

    ![Määritä WordPress web App-sovelluksen kieli][WordPressLanguage]

1. WordPress **Tervetuloa** -sivulla WordPress vaatii määritysten tiedot ja valitse sitten **Asenna WordPress**.

    ![WordPress online-asetusten määrittäminen][WordPressConfigure]

1. Kirjaudu sisään käyttämällä luomaasi **aloitussivulla** tunnistetietoja.  

1. Sivuston Dashboard-sivu avautuu ja annetut tiedot näkyviin.    

    ![Tarkastele WordPress Raporttinäkymät-ikkunan][WordPressDashboard2]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa nähnyt, miten voit luoda ja ota käyttöön Azure Marketplacesta Esimerkki web-sovellus.

Saat lisätietoja sovelluksen palvelun Web Apps-sovellusten käsittelemisestä linkkejä (for leveä selainikkunat)-sivun vasemmassa reunassa tai (, kapea selainikkunat)-sivun yläreunassa.

Katso lisätietoja Azure WordPress web Apps-sovellusten kehittämisestä [Kehittäminen WordPress Azure App palvelun][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Kokeile sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure Portal]: https://portal.azure.com/
[Projektin Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
