<properties 
    pageTitle="Azure App palvelun web Apps-sovellusten määrittäminen" 
    description="Web-sovelluksen määrittäminen Azure App Services-palveluissa" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Azure App palvelun web Apps-sovellusten määrittäminen #

Tässä ohjeaiheessa kerrotaan, miten määrittäminen [Azure Portal]verkkosovellukseen.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Sovelluksen asetukset

1. [Azure-portaalissa]Avaa web-sovelluksen sivu.
2. Valitse **kaikkia asetuksia**.
3. Valitse **sovelluksen asetukset**.

![Sovelluksen asetukset][configure01]

**Sovelluksen asetukset** -sivu on ryhmitelty useaan eri luokkaan.

### <a name="general-settings"></a>Yleiset asetukset

**Framework-versiot**. Määritä nämä asetukset, jos sovellus on käytössä jokin näistä kehysten: 

- **.NET Framework**: Määritä .NET framework-versio. 
- **PHP**: PHP versio tai **pois** käytöstä PHP. 
- **Java**: Valitse Java-versio tai **pois** käytöstä Java. **Web-säilö** -vaihtoehdon avulla voit valita Tomcat ja jota versioiden välillä.
- **Python**: Valitse Python versio tai **pois** käytöstä Python.

Teknisistä syistä Javan, kun sovellus estää .NET, PHP ja Python-asetukset.

<a name="platform"></a>
**Ympäristössä**. Valitsee onko koodiin suoritetaan ympäristössä, jossa on 32-bittisen tai 64-bittinen. 64-bittisessä ympäristössä edellyttää Basic tai vakio-tilassa. Vapauta ja jaettujen tilat suoritetaan aina 32-bittinen ympäristössä.

**Web-Sockets**. Määritä **käytössä** , jos haluat ottaa käyttöön WebSocket; protokolla Jos esimerkiksi web-sovelluksen käyttää [ASP.NET SignalR] tai [socket.io].

<a name="alwayson"></a>
**Aina**. Oletusarvon mukaan verkkosovelluksissa ovat lataus, jos ne käyttämättömiä joitakin ajan kuluessa. Näillä oikeuksilla järjestelmä säästää resursseja. Basic tai Standard-tilassa voit ottaa **Aina** pitää sovelluksen lataaminen aina. Sovelluksen käytetään jatkuvasti web työt, ota **Aina käyttöön**tai web-töitä ei ehkä toimi luotettavasti.

**Hallitun myyntijakso versio**. Asettaa IIS [myyntijakso-tilassa]. Jätä tämän kohdan arvoksi integroitu (oletusasetus), jos sinulla ei ole vanha sovellus, joka edellyttää IIS vanhempi versio.

**Automaattinen vaihtaminen**. Jos otat automaattisen Vaihda käyttöönoton vapaan, sovelluksen palvelun automaattisesti Vaihda web-sovelluksen kyselyjä tuotannon kun painat päivitys paikkaa. Lisätietoja on artikkelissa [väliaikaisen paikkojen verkkosovelluksissa Azure-sovelluksen käytössä, ota käyttöön] (web-sivustojen-vaiheistettu-publishing.md).

### <a name="debugging"></a>Virheenkorjaus

**Remote virheenkorjaus**. Mahdollistaa remote virheenkorjaus. Kun käytössä, voit käyttää remote virheenkorjaus Visual Studiossa yhteyden muodostamiseen web Appissa. Remote virheenkorjaus pysyy käytössä 48 tuntia. 

### <a name="app-settings"></a>Sovelluksen asetukset

Tässä osassa on nimi-arvoa paria, jotka olet web-sovelluksen lataaminen käynnistettäessä. 

- .NET-sovellusten nämä asetukset ovat syötetään .NET-kokoonpanosi `AppSettings` suorituksen ohittaminen olemassa olevat asetukset. 

- PHP, Python, Java ja solmu sovellukset voivat käyttää näitä asetuksia kuin ympäristömuuttujia suorituksen aikana. Sovelluksen kunkin asetuksen kaksi ympäristömuuttujat luodaan; yksi sovellus asetus tapahtuma, ja toinen APPSETTING_ etuliite määrittämää nimellä. Sisältävät kumpikin on sama arvo.

### <a name="connection-strings"></a>Yhteysmerkkijonon

Linkitetyn resursseja yhteyden merkkijonoja. 

.NET-sovellusten yhteyden merkkijonon ovat syötetään .NET-kokoonpanosi `connectionStrings` suorituksen asetusten ohittaminen vanhoihin, jossa avain on linkitetty tietokannan nimi. 

PHP, Python, Java ja solmu-sovelluksia nämä asetukset ovat käytettävissä ympäristömuuttujat suorituksen etuliite yhteystyyppiä. Ympäristön muuttujan etuliitteitä ovat seuraavat: 

- SQL Server:`SQLCONNSTR_`
- MySQL:`MYSQLCONNSTR_`
- SQL-tietokantaan:`SQLAZURECONNSTR_`
- Mukautettu:`CUSTOMCONNSTR_`

Jos MySql yhteysmerkkijonon on nimeltä esimerkiksi `connectionstring1`, se käyttää kautta ympäristömuuttuja `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Oletustiedostoja

Oletustiedosto on web-sivu, joka näkyy verkkosivuston päämyöntäjien URL-osoitteessa.  Luettelon ensimmäisen vastaavia tiedoston käytetään. 

Verkkosovelluksissa voidaan tarvita moduulit, että URL-Osoitteen perusteella reitin sijaan käsittelevä staattiseksi sisällöksi, jolloin ei ei ole oletusarvon asiakirja sellaisenaan.    

### <a name="handler-mappings"></a>Tapahtumakäsittelijän yhdistäminen

Lisää mukautettu komentosarja-suorittimet tietyn tiedostotunnisteet pyyntöjen tällä alueella avulla. 

- **Tunniste**. Jos haluat käsitellä, kuten *.php tai handler.fcgi tunniste. 
- **Komentosarjan suoritin polku**. Komentosarjan suoritin absoluuttinen polku. Tiedostot, jotka vastaavat tiedostotunniste pyynnöt käsitellään komentosarja-suoritin. Käytä polku `D:\home\site\wwwroot` viittaamaan sinua sovelluksen pääkansioon.
- **Lisää argumentit**. Valinnainen komentorivin argumentit komentosarja-suoritin 


### <a name="virtual-applications-and-directories"></a>Virtuaalinen sovellukset ja hakemistojen selaaminen 
 
Määritä virtual sovellukset ja hakemistojen selaaminen, Määritä kunkin näennäiskansio ja sen vastaavan fyysinen polku suhteessa sivuston ylimmällä tasolla. Vaihtoehtoisesti voit valita **sovelluksen** -valintaruutu, jos haluat merkitä näennäiskansio sovelluksena.


## <a name="enabling-diagnostic-logs"></a>Vianmäärityslokit ottaminen käyttöön

Jos haluat ottaa käyttöön Vianmäärityslokit:

1. Valitse sivu web-sovelluksen, **kaikki asetukset**.
2. Valitse **vianmäärityslokit**. 

Kirjoittaminen vianmäärityslokit WWW-sovelluksesta, joka tukee kirjaaminen asetukset: 

- **Sovelluksen lokiin kirjaaminen**. Kirjoittaa sovelluksen lokit tiedostojärjestelmän. Lokiin kirjaaminen kestää 12 tunnin ajan. 

**Taso**. Sovelluksen kirjaaminen on otettu käyttöön, kun tämä asetus määrittää tietomäärän, joka on tallennettu (virhe, varoitus, tiedot tai yksityiskohtainen).

**Verkkopalvelin lokiin kirjaaminen**. Lokit tallennetaan W3C laajennettu log-tiedostomuodossa. 

**Yksityiskohtaiset virhesanomat**. Tallentaa yksityiskohtaisen virheen viestit .htm-tiedostoja. Tiedostot on tallennettu /LogFiles/DetailedErrors. 

**Epäonnistui pyynnön jäljittäminen**. Lokit epäonnistui pyynnöt XML-tiedostoja. Tiedostot tallennetaan /LogFiles/W3SVC*xxx*, xxx on yksilöllinen tunnus-kohdassa. Tämä kansio sisältää XSL-tiedoston ja vähintään yksi XML-tiedosto. Varmista, että XSL-tiedoston lataaminen koska se sisältää toimintoja muotoilun ja suodattamisesta XML-tiedostojen sisältöä.

Lokitiedostojen katselemista sinun on luotava FTP tunnistetietoja seuraavasti:

1. Valitse sivu web-sovelluksen, **kaikki asetukset**.
2. Valitse **käyttöönoton tunnistetiedot**.
3. Kirjoita käyttäjänimi ja salasana.
4. Valitse **Tallenna**.

![Määritä käyttöönoton tunnistetiedot][configure03]

Koko FTP käyttäjänimi on "app\username", jossa *sovellus* on web Appissa. Käyttäjänimi näkyy web app-sivu, valitse **Essentials**.  

![FTP käyttöönoton tunnistetiedot][configure02]

## <a name="other-configuration-tasks"></a>Muita määritystehtäviä

### <a name="ssl"></a>SSL 

Basic tai Standard-tilassa voit ladata mukautetun toimialueen SSL-varmenteita. Lisätietoja on artikkelissa [web-sovelluksen käyttöön HTTPS]. 

Voit tarkastella ladattuja varmenteet valitsemalla **Kaikki asetukset** > **Mukautetut toimialueet ja SSL**.

### <a name="domain-names"></a>Toimialueiden nimet

Lisää mukautettuja toimialuenimiä web Appissa. Lisätietoja on artikkelissa [määrittäminen verkkosovellukseen Azure-sovelluksen palvelun mukautetun toimialuenimi].

Voit tarkastella toimialuenimiä, valitsemalla **Kaikki asetukset** > **Mukautetut toimialueet ja SSL**.

### <a name="deployments"></a>Ominaisuuksissa

- Jatkuva käyttöönoton määrittämisen. Katso [Käyttämällä Git Azure App palvelussa Web Apps -sovellusten käyttöön](./web-sites-deploy.md).
- Käyttöönoton paikkaa. Katso [väliaikaisen ympäristöissä verkkosovelluksissa Azure sovelluksen-palvelun käyttöön].


Voit tarkastella käyttöönoton paikkaa napsauttamalla **Kaikkia asetuksia** > **käyttöönoton paikkaa**.

### <a name="monitoring"></a>Seuranta

Basic tai Standard-tilassa voit testata HTTP tai HTTPS päätepisteet enintään kolme geo distributed sijainneista käytettävyyttä. Seurannan testi epäonnistuu, jos HTTP-vastauksen koodi on virhe (4xx tai 5xx) tai vastauksen kestää yli 30 sekuntia. Päätepisteen pidetään käytettävissä, jos seurantaa testejä onnistu tiettyihin sijainteihin. 

Lisätietoja on artikkelissa [Toimintaohje: web päätepisteen tilaa].

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu], jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Mukautetun toimialuenimen määrittäminen Azure App palvelun]
- [Ota käyttöön HTTPS sovelluksen Azure sovelluksen-palvelussa]
- [Skaalaa verkkosovellukseen Azure sovelluksen-palvelussa]
- [Seurannan perusteet Web Apps-sovellukset App Azure-palvelu]

<!-- URL List -->

[ASP.NET-SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Mukautetun toimialuenimen määrittäminen Azure App palvelun]: ./web-sites-custom-domain-name.md
[Ota käyttöön Web Apps-Azure App palvelun väliaikaisen Ympäristöt]: ./web-sites-staged-publishing.md
[Ota käyttöön HTTPS sovelluksen Azure sovelluksen-palvelussa]: ./web-sites-configure-ssl-certificate.md
[Toimintaohje: valvoa web päätepisteen tila]: http://go.microsoft.com/fwLink/?LinkID=279906
[Seurannan perusteet Web Apps-sovellukset App Azure-palvelu]: ./web-sites-monitor.md
[myyntijakso-tilassa]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Skaalaa verkkosovellukseen Azure sovelluksen-palvelussa]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Kokeile sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
