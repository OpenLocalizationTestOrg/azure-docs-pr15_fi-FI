<properties
    pageTitle="Java-web-sovelluksen luominen Azure App palvelun | Microsoft Azure"
    description="Tässä opetusohjelmassa näytetään, miten Java-web-sovelluksen käyttöön Azure App palvelu."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Java-web-sovelluksen luominen Azure sovelluksen-palvelu

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tässä opetusohjelmassa näytetään, miten voit luoda Java [web app sovelluksen Azure-palvelu ja] [Azure-portaalissa]. Azure-portaali on Internet-käyttöliittymä, joiden avulla voit hallita Azure resursseja.

> [AZURE.NOTE] Tässä opetusohjelmassa suorittamiseen tarvitset Microsoft Azure-tiliin. Jos sinulla ei ole tiliä, voit [aktivoida Visual Studio tilaajan etuja] tai [maksuttoman kokeiluversion käyttäjäksi rekisteröityminen].
>
> Jos haluat aloittaa Azure App palvelun, ennen kuin kirjaudut Azure-tili, siirry [Yritä App kääntäjä]. Siellä voit luoda lyhytkestoinen starter verkkosovellukseen heti App palvelussa; ilman luottokortti tarvitaan, ja ei ole sitoumukset.

## <a name="java-application-options"></a>Java-sovelluksen asetukset

Voit määrittää Java-sovellus App palvelun web App-sovelluksessa useilla tavoilla. 

1. Sovelluksen luominen ja määritä sitten **asetukset**.

    Sovelluksen palvelun tarjoaa useita Tomcat ja jota versioita, oletusarvon määrittäminen. Jos sovellus, jossa isännöinnin työskentelee jonkin valmiin version, tätä menetelmää web säilön määrittämisestä on helpoin ja on sopivaa, kun kaikki haluamasi toimet on sodan tiedoston lataaminen web säilön. Tämä menetelmä-sovelluksen luominen Azure-portaaliin ja siirry sitten, kun sovellus, valitse haluamasi Java web-säilön sekä Java-versiosi **sovelluksen asetukset** -sivu. Kun käytät tätä tapaa Java- ja web-säilön suoritetaan ohjelman tiedostoista. Muita tapoja sijoittaa web-säilö ja mahdollisesti JVM levytilaa. Kun käytät tätä mallia, sinulla ei ole tässä osassa tiedostojärjestelmän tiedostojen muokkaamiseen. Tämä tarkoittaa, että et voi esimerkiksi määrittää *server.xml* tiedoston tai sijoita kirjaston */lib* -kansioon. Lisätietoja on artikkelissa [Luo ja määritä Java verkkosovellukseen](#appsettings) jäljempänä tässä opetusohjelmassa kohdassa.
    
2. Mallin käyttäminen Azure Marketplacesta.

    Azure Marketplace sisältää malleja, automaattisesti luominen ja määrittäminen Java web Apps-sovellusten Tomcat tai jota web säilöä sisältävällä. Mallien luominen, web-säilöt ovat määritettäviä. Lisätietoja on tässä opetusohjelmassa kohdassa [Java mallin pohjalta Azure Marketplacesta](#marketplace) .
  
3. Sovelluksen luominen ja kopioida manuaalisesti ja tiedostojen muokkaaminen 

    Voit halutessasi isännöidä mukautettu Java-sovellus, joka ei käyttöönotto kaikista web App-palvelua varten säilöt. Esimerkki:
    
    * Java-sovellus edellyttää Tomcat tai jota, joka ei ole suoraan App palvelun tukemat tai valikoiman annettu version.
    * Java-sovellus tulee pyyntöjen ja ota nimellä SODAN olemassa web säilöön.
    * Haluat määrittää WWW-säilö alusta alkaen itse. 
    * Haluat käyttää Java, joita ei tueta App palvelun versio ja haluat ladata itse.

    Tapausten tällaisia Azure-portaalissa-sovelluksen luominen ja anna sitten tarvittavat runtime tiedostot manuaalisesti. Tässä tapauksessa tiedostot lasketaan vastaan tallennustilan tilaa-kiintiön App palvelun palvelupaketissasi. Lisätietoja on artikkelissa [mukautetun Java verkkosovellukseen Azure haluat ladata].

## <a name="portal"></a>Luo ja määritä Java verkkosovellukseen

Tässä osassa esitellään web-sovelluksen luominen ja määrittäminen Java käyttämällä portaalin **sovelluksen asetukset** -sivu.

1. Kirjautuminen [Azure Portal].

2. Valitse **Uusi > Web + Mobile > Web-sovelluksen**.

    ![Uusi Web App][newwebapp]

4. Kirjoita nimi web-sovelluksen **Web app** -ruutuun.

    Tämän nimen on oltava yksilöllinen azurewebsites.net toimialueen, koska web Appin URL-osoite on {name}. azurewebsites.net. Jos kirjoittamasi nimi ei ole yksilöllisiä, punainen huutomerkki näkyy teksti-ruutuun.

5. Valitse **Resurssiryhmä** tai luoda uuden.

    Saat lisätietoja resurssiryhmät [Azure-portaalissa voit hallita Azure resursseja].

6. Valitse **Sovelluksen palvelun suunnitelman/sijainti** tai luoda uuden.

    Saat lisätietoja sovelluksen palvelusopimusten vaihtoehdot artikkelissa [Azure App palvelun suunnitelmien yleiskatsaus].

7. Valitse **Luo**.

    ![Web-sovelluksen luominen][newwebapp2]
 
8. Kun web-sovellus on luotu, valitse **verkkosovelluksissa > {web Appin}**.
 
    ![Valitse Web Appissa][selectwebapp]

9. Valitse **asetukset** **Online** -sivu.

10. Valitse **sovelluksen asetukset**.

11. Valitse haluamasi **Java-versio**. 

12. Valitse haluamasi **Java aliversio**. Jos valitset **uusimmasta**, sovellus käyttää uusin aliversio, joka on käytettävissä sovelluksen-palvelussa, Java pääversion. **Uusimmasta** kohde on yksilöllinen Java-sovellukset, jotka on luotu **sovelluksen asetukset**. Jos luot Java-sovelluksen valikoimasta, sinun on hallita omia säilö ja JVM muutokset. 

12. Valitse haluamasi **Web säilö**. Jos valitset säilön nimi, joka alkaa **uusimmasta**, sovelluksen pidettävä kyseisen web säilö pääversion, joka on saatavilla App palvelun uusimman version. 

    ![Web-säilön versiot][versions]

13. Valitse **Tallenna**.

    Web-sovelluksen muuttuu sisällä hetken Java-pohjainen ja määritetty käyttämään web-säilö, jonka valitsit.

14. Valitse **WWW-sovellukset > {Uusi web-sovellus}**.

15. Valitse Siirry uuden sivuston **URL-osoite** .

    Verkkosivun vahvistaa, että olet luonut Java-pohjainen web App-sovelluksessa.

## <a name="marketplace"></a>Java-mallin käyttäminen Azure Marketplacesta

Tässä osassa esitellään käyttämisestä Java-web-sovelluksen luominen Azure Marketplacesta. Saman yleinen kulku voidaan myös Luo Java-pohjainen mobile tai API-sovellus. 

1. Kirjautuminen [Azure Portal]

2. Valitse **Uusi > Marketplace**.

    ![Uusi Marketplace][newmarketplace]

3. Valitse **Web + Mobile**.

    Sinun on ehkä vierittää vasemmalle Nähdäksesi **Marketplace** -sivu, jossa voit valita **Web + Mobile**.

4. Haku-ruutuun Java sovelluspalvelin, kuten **Apache Tomcat** tai **jota**nimi ja paina sitten Enter-näppäintä.

5. Valitse hakutuloksista Java-sovelluspalvelin.

    ![Web-mobiilisovelluksen jota][webmobilejetty]

6. Valitse ensimmäinen **Apache Tomcat** tai **jota** sivu, **Luo**.

    ![Satamien portaalin sivu][jettyblade]

7. Kirjoita seuraava **Apache Tomcat** tai **jota** sivu-web-sovelluksen **Web app** -ruutuun nimi.

    Tämän nimen on oltava yksilöllinen azurewebsites.net toimialueen, koska web Appin URL-osoite on {name}. azurewebsites.net. Jos kirjoittamasi nimi ei ole yksilöllisiä, punainen huutomerkki näkyy teksti-ruutuun.

8. Valitse **Resurssiryhmä** tai luoda uuden.

    Saat lisätietoja resurssiryhmät [Azure-portaalissa voit hallita Azure resursseja].

9. Valitse **Sovelluksen palvelun suunnitelman/sijainti** tai luoda uuden.

    Saat lisätietoja sovelluksen palvelusopimusten vaihtoehdot artikkelissa [Azure App palvelun suunnitelmien yleiskatsaus].

10. Valitse **Luo**.

    ![Satamien Portal luominen][jettyportalcreate2]

    Lyhyt aika, yleensä alle minuutin Azure on luonut uuden web-sovelluksen.

11. Valitse **WWW-sovellukset > {Uusi web-sovellus}**.

12. Valitse Siirry uuden sivuston **URL-osoite** .

    ![Satamien URL-osoite][jettyurl]

    Tomcat mukana oletusarvoiset sivujen; Jos valitsit Tomcat, näkyy seuraavassa esimerkissä samalla sivulla.

    ![Web Appin Apache Tomcat][tomcat]

    Jos valitsit jota, näkyy seuraavassa esimerkissä samalla sivulla. Jota ei ole sivun oletusarvoiset, niin saman JSP, jota käytetään tyhjän Java-sivuston käytetään uudelleen tähän.

    ![Web-sovellukseen, jota käyttämällä][jetty]

Nyt kun olet luonut web app-sovelluksen säilöön, on kohdassa [vaiheisiin](#next-steps) tietoja lataaminen sovelluksesi web App-sovellukseen.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä vaiheessa sinun on sovelluksen palvelimen Java-sovellukseen Azure sovelluksen-palvelussa. Oman koodin käyttöön web app-kohdassa [Lisää sovellus tai verkkosivulle Java web App-sovellukseen].

Saat lisätietoja Azure Java-sovellusten kehittäminen [Java Developer Center].

<!-- URL List -->

[Lisää sovellus tai verkkosivulle Java web App-sovellukseen]: ./web-sites-java-add-app.md
[Azure App palvelun suunnitelmien yleiskatsaus]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure Portal]: https://portal.azure.com/
[Aktivoi Visual Studio tilaajan etuja]: http://go.microsoft.com/fwlink/?LinkId=623901
[maksuttoman kokeiluversion käyttäjäksi rekisteröityminen]: http://go.microsoft.com/fwlink/?LinkId=623901
[Kokeile sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=523751
[Web app sovelluksen Azure-palvelu]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java-kehityskeskus]: /develop/java/
[Azure-portaalin avulla voit hallita Azure resursseja]: ../azure-portal/resource-group-portal.md
[Mukautetun Java-web-sovelluksen lataaminen Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
