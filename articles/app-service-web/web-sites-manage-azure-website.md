<properties 
    pageTitle="Azure-sovelluksen palvelun verkkosovellukseen hallinta" 
    description="Resurssien hallinnan verkkosovellukseen Azure-sovelluksen palvelun linkkejä." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Azure-sovelluksen palvelun verkkosovellukseen hallinta

Tässä artikkelissa on resurssien hallinnan verkkosovellukseen [Azure](http://go.microsoft.com/fwlink/?LinkId=529714)-sovelluksen palvelun linkkejä. Hallinta sisältää kaikki tehtävät, joka säilyttää toimivuuden koodiin. 

Verkkosovellukseen elinkaaren päälle voit suorittaa eri hallintatehtäviä, kun siirrät ensimmäisen käyttöönoton normaalin käytön, ylläpito ja päivitykset.

Monta web app tehtävät voidaan suorittaa Azure-portaalissa.

## <a name="before-you-deploy-your-web-app-to-production"></a>Ennen kuin otat koodiin tuotannon

### <a name="choose-a-tier"></a>Valitse taso

Azure App palvelun tarjotaan viisi tasoa: vapaa, jaettu, Basic, Vakio ja Premium. Katso tietoja ominaisuudet ja kunkin tason hinnoittelua [hinnoittelua tiedot](/pricing/details/app-service/). 

- [Sovelluksen palvelusopimusten vaihtoehdot](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) avulla voit ryhmitellä useiden verkkosovelluksissa kohdassa samalla tasolla.
- Voit aina [vaihtaa tasoa](web-sites-scale.md) , kun olet luonut web Appissa.

### <a name="configuration"></a>Määritys

Käytä [Azure-portaalin](https://portal.azure.com/) asetuksia. Lisätietoja on artikkelissa [Azure App palvelun Configure web App-sovellusten](web-sites-configure.md). Seuraavassa on lyhyt muistilista:

- Valitse **runtime versiot** .NET, PHP, Java tai Python, tarvittaessa.
- Ota käyttöön **WebSockets** , jos web-sovelluksen käyttää WebSocket-protokollaa. (Mukaan lukien sovellukset, jotka käyttävät [ASP.NET SignalR](http://www.asp.net/signalr) tai [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- On käytössäsi jatkuvasti web työt? Jos näin on, ota **Aina käyttöön**.
- Määritä **oletusarvoinen asiakirjan**, kuten index.html.

Lisäksi peruskokoonpano-asetuksia voit määrittää seuraavat asiat:

- **Secure Socket Layer (SSL)** -salausta. Jos haluat käyttää mukautettua toimialuenimeä SSL, hanki SSL-varmenteen ja web App-sovelluksen käyttämään sitä määrittäminen. Katso [Azure-sovelluksen palvelun web-sovelluksen käyttöön HTTPS](web-sites-configure-ssl-certificate.md).
- **Mukautetun toimialuenimi.** Web-sovelluksessa on automaattisesti alitoimialueen azurewebsites.net-kohdassa. Voit yhdistää mukautettua toimialuenimeä, esimerkiksi contoso.com. Lisätietoja on kohdassa [Configure Azure-sovelluksen palvelun mukautettua toimialuenimeä](web-sites-custom-domain-name.md).

Kielikohtaiset määritykset:

- **PHP**: [Määritä PHP Azure App palvelun verkkosovelluksissa](web-sites-php-configure.md).
- **Python**: [määrittäminen Python Azure App palvelun Web Apps-sovellusten kanssa](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Web-sovellus on käynnissä

Web-sovellus on käynnissä, kun haluat Varmista, että se on käytettävissä ja sitä Skaalaa täyttävän käyttäjien tietoliikenne. Haluat ehkä myös vianmääritys.

### <a name="monitoring"></a>Seuranta

- Palvelun Azure-portaalissa voit [lisätä suorituskyvyn mittarit](web-sites-monitor.md) kuten suorittimen käyttö ja määrää.
- [Skaalaa koodiin](web-sites-scale.md) liikenne saatuaan. Sen mukaan, että taso skaalata VMs määrän ja/tai AM esiintymät kokoa. Valitse vakio- ja Premium-tasoa voit myös määrittää automaattisen skaalauksen poistaminen, jotta koodiin Skaalaa automaattisesti kiinteä aikataulussa tai ladata vastauksena.  
 
### <a name="backups"></a>Varmuuskopioiden

- Määritä [automaattisen varmuuskopioinnin](web-sites-backup.md) koodiin. Lisätietoja varmuuskopioiden [Tämä video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Tutustu erilaisiin [tietokannan palauttaminen](../sql-database/sql-database-business-continuity.md) Azure SQL-tietokantaan.

### <a name="troubleshooting"></a>Vianmääritys

- Jos järjestelmässä vikaan, voit [vianmääritys Visual Studiossa](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), vianmäärityslokit ja live virheenkorjaus pilveen. 
- Visual Studio ulkopuolella on monia tapoja kerätä vianmäärityslokit. Katso [Ota lokiin kirjaaminen web Apps-sovellukset Azure App palvelun diagnostiikka](web-sites-enable-diagnostic-log.md).
- Tutustu Node.js sovellusten [Virheenkorjaus Node.js verkkosovellukseen Azure sovelluksen-palvelussa](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Tietojen palauttaminen

- [Palauttaa](web-sites-restore.md) web-sovellusta, joka oli aiemmin varmuuskopioidut.


## <a name="when-you-update-your-web-app"></a>Kun päivität web Appissa

Jos et ole ottanut automaattisen varmuuskopioinnin, voit luoda [manuaalisesti](web-sites-backup.md).

Harkitse [vaiheistettu käyttöönotto](web-sites-staged-publishing.md). Tämän asetuksen avulla voit julkaista päivitykset väliaikaisen käyttöönoton, joka suoritetaan rinnakkain tuotannon käyttöönoton kanssa. 

Jos käytät Visual Studio Team Services, voit määrittää jatkuva käyttöönoton hallinnassa:

- [Käyttämällä ryhmän Foundation versionhallinta (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Git käyttäminen](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
