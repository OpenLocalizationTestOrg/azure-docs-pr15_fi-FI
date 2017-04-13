<properties 
    pageTitle="MFA Server Mobile-sovelluksen verkkopalvelun käytön aloittaminen"
    description="Azure multi-factor Authentication-sovellus on Luokittele todennus-vaihtoehto.  Push-ilmoitukset käyttäjiä käyttämään MFA-palvelimen avulla."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>MFA Server Mobile-sovelluksen verkkopalvelun käytön aloittaminen

Azure multi-factor Authentication-sovellus on Luokittele todennus-vaihtoehto. Sen sijaan, että sijoitat automaattinen puhelun tai SMS käyttäjälle kirjautumisen yhteydessä, Azure Monimenetelmäisen todentamisen Vie ilmoituksen Azure multi-factor Authentication-sovellukseen valitsemalla käyttäjän älypuhelimella tai taulutietokoneella. Käyttäjän napauttaa yksinkertaisesti "Tarkista" (tai Lisää PIN-tunnuksen ja napauttaa "Tarkista")-sovelluksessa kirjautua sisään.

Jotta voit käyttää Azure multi-factor Authentication-sovellusta, seuraavat tarvitaan niin, että sovellus voi onnistuneesti yhteydenpito Mobile-sovelluksen Web-palvelu:

- Katso laite- ja ohjelmistovaatimukset laitteisto- ja ohjelmistovaatimukset
- Sinun on käytettävä 6.0 tai sitä uudempi versio Azure multi-factor Authentication-palvelin
- Mobile-sovelluksen Web-palvelu on oltava asennettuna Microsoft® Internet Information Services (IIS) IIS käynnissä Internetiin yhteydessä oleva WWW-palvelimessa 7.x tai uudempi versio.  Katso lisätietoja IIS- [IIS.NET](http://www.iis.net/).
- ASP.NET-v4.0.30319 on asennettu, rekisteröity ja sallitut asettaminen
- Pakollinen roolipalvelut ovat ASP.NET ja IIS 6 metakannan yhteensopivuus
- Mobile-sovelluksen Web-palvelu on oltava käytettävissä julkisen URL-Osoitteen kautta
- Mobile-sovelluksen Web-palvelu on suojattu SSL-varmenteella.
- Azure multi-factor Authentication Web palvelun SDK on oltava asennettuna IIS 7.x tai uudempi versio palvelimesta, Azure multi-factor Authentication-palvelin
- Azure multi-factor Authentication Web palvelun SDK on suojattu SSL-varmenteella.
- Mobile-sovelluksen Web-palvelu on voitava muodostaa Azure multi-factor Authentication Web palvelun SDK SSL: n
- Mobile-sovelluksen Web-palvelu on voi todentaa palvelutilin, joka kuuluu nimeltä "PhoneFactor järjestelmänvalvojat-käyttöoikeusryhmän tunnistetiedoilla Azure multi-factor Authentication Web palvelun SDK-paketissa. Tämän palvelutili ja ryhmä ovat Active Directory Jos Azure multi-factor Authentication Server suoritetaan palvelimessa toimialueeseen liittymistä. Tämän palvelutili ja ryhmän olemassa paikallisesti Azure multi-factor Authentication-palvelimeen jos se ei ole liitetty toimialue.


Käyttäjä-portaalin asentaminen palvelimessa kuin Azure multi-factor Authentication-palvelin edellyttää kolme vaihetta:

1. WWW-palvelun SDK asentaminen
2. Mobiilisovelluksen WWW-palvelun asentaminen
3. Azure multi-factor Authentication Server mobiilisovelluksen-asetusten määrittäminen
4. Aktivoi Azure multi-factor Authentication-sovelluksen käyttäjille

## <a name="install-the-web-service-sdk"></a>WWW-palvelun SDK asentaminen

Jos Azure multi-factor Authentication Web palvelun SDK ei ole jo asennettu Azure multi-factor Authentication-palvelimeen, siirry tähän palvelimeen ja avaa Azure multi-factor Authentication-palvelimeen. Web-palvelu SDK-kuvaketta, valitse Asenna Web SDK... -painiketta ja noudata ohjeita, esitetään. Web-palvelu SDK on suojattu SSL-varmenteella. Itse allekirjoitetun varmenteen on kunnossa tähän tarkoitukseen, mutta se on tuotava "Luotettujen päämyöntäjien" store paikallistietokoneen tilin käyttäjän Portal-WWW-palvelimessa niin, että se luottaa sertifikaatin, kun aloitetaan SSL-yhteyttä.

<center>![Asetukset](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>Mobiilisovelluksen WWW-palvelun asentaminen
Ennen kuin asennat mobiilisovelluksen web-palvelu, otettava huomioon seuraavasti:

- Jos Azure multi-factor Authentication käyttäjän portaalissa on jo asennettu Internetiin yhteydessä oleva palvelimessa, käyttäjänimi ja salasana SDK WWW-palvelun URL-Osoitteen voi kopioidaan käyttäjän Portal korostetut.
- Kannattaa lisätä Avaa Internetiin yhteydessä oleva WWW-palvelimessa selain ja siirry Web palvelun SDK-paketissa, joka on syötetty seuraavan koodin korostetut URL-osoite. Jos selaimen siirtyä web-palveluun onnistuneesti, se kannattaa pyytää tunnistetietoja. Kirjoita käyttäjänimi ja salasana, joka on kirjoitettu seuraavan koodin korostetut juuri sellaisena kuin se näkyy tiedostossa. Varmista, että sertifikaatti varoituksia tai virheitä näkyvät.
- Jos käänteisen välityspalvelimen tai palomuurin istuu Mobile-sovelluksen verkkopalvelun verkkopalvelin eteen ja suorittamiseen SSL purkaminen, voit muokata seuraavan Mobile-sovelluksen verkkopalvelun koodin korostetut ja lisää seuraavat avaimen <appSettings> osan niin, että Mobile-sovelluksen verkkopalvelun käyttää http sijaan https. SSL on kuitenkin yhä pakollinen Mobile-sovelluksen välityspalvelimeen palomuurin tai päinvastoin. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>Mobiilisovelluksen WWW-palvelun asentaminen

<ol>
<li>Avaa Windowsin Resurssienhallinta Azure multi-factor Authentication-palvelimeen ja Selaa kansioon, johon on asennettu Azure multi-factor Authentication-Server (kuten C:\Program Files\Azure multi-factor Authentication). Valitse Azure multi-factor AuthenticationPhoneAppWebServiceSetup asennustiedosto sopiva palvelimen Mobile-sovelluksen verkkopalvelun asennetaan, 32-bittisen tai 64-bittinen versio. Kopioi asennustiedosto Internetiin yhteydessä oleva palvelimeen.</li>

<li>Internetiin yhteydessä oleva WWW-palvelimessa asennustiedosto on suoritettava järjestelmänvalvojan oikeudet. Helpoin tapa on Avaa komentokehote järjestelmänvalvojana ja siirry sijaintiin, johon asennustiedosto on kopioitu.</li>  

<li>Suorita multi-factor AuthenticationMobileAppWebServiceSetup asennustiedosto, muuttaa sivuston halutessasi ja vaihda Virtual kansio, kuten "PA" lyhyt nimi. Lyhyt näennäiskansio nimi on suositeltavaa, koska käyttäjien on syötettävä Mobile App WWW-palvelun URL kyselyjä mobiililaitteeseen aktivoinnin aikana.</li>

<li>Azure multi-factor AuthenticationMobileAppWebServiceSetup asennuksen jälkeiset selaamalla C:\inetpub\wwwroot\PA (tai asianmukaiseen kansioon näennäiskansio nimen perusteella) ja Muokkaa seuraavan koodin korostetut.</li>  

<li>Etsi WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ja WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD näppäimet ja arvot arvoksi käyttäjänimi ja salasana, joka kuuluu PhoneFactor järjestelmänvalvojat-suojauksen palvelutilin käyttäjäryhmä (katso yllä koskevat vaatimukset-kohdassa). Tämä voi olla samaa tiliä käytössä Azure multi-factor Authentication käyttäjän portaalin tunnus, jos, jotka on aiemmin asennettu. Kirjoita käyttäjänimi ja salasana rivin lopussa lainausmerkkien välissä, että (arvo = "" / >). On suositeltavaa käyttää täydellinen käyttäjänimi (esimerkiksi TOIMIALUE\käyttäjänimi tai machine\username).</li>  

<li>Etsi pfMobile Web App-Service_pfwssdk_PfWsSdk-asetus ja muuta arvon "http://localhost:4898/PfWsSdk.asmx" WWW-palvelun SDK, jossa on käytössä Azure multi-factor Authentication-palvelimessa (kuten https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) URL-osoite. Koska SSL käytetään tätä yhteyttä, koska SSL-varmenne on annettu palvelimen nimi ja URL-osoite on vastattava olevan varmenteen nimi on viitattava WWW-palvelun SDK palvelimen nimi ja ei IP-osoite. Jos palvelimen nimi ei ratkaise IP-osoite Internetiin yhteydessä oleva palvelimesta, tekstin lisääminen yhdistämään Azure multi-factor Authentication-palvelimen nimi IP-osoite palvelimen isännät-tiedosto. Tallenna seuraavan koodin korostetut jälkeen tehdyt muutokset.</li>  

<li>Jos Mobile-sovelluksen Web-palvelu on asennettu-kohdassa (kuten oletussivuston) sivuston ei vielä ole binded julkisesti kirjautunut sertifikaatilla, palvelin varmenteen asentaminen Jos et jo asennettu, Avaa IIS Manager ja sitoa varmenteen verkkosivustoon.</li>  

<li>Avaa selain miltä tahansa tietokoneelta, ja siirry URL-osoite, johon on asennettu Mobile-sovelluksen Web-palvelu (esimerkiksi https://www.publicwebsite.com/PA). Varmista, että sertifikaatti varoituksia tai virheitä näkyvät.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure multi-factor Authentication Server mobiilisovelluksen-asetusten määrittäminen
Nyt kun mobiilisovelluksen web-palvelu on asennettu, sinun on ‑palvelin Azure multi-factor Authentication portaalin käyttöä varten.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Mobiilisovelluksen asetusten Azure multi-factor Authentication-palvelimessa

1. Napsauta Azure multi-factor Authentication-palvelimen käyttäjän Portal-kuvaketta. Jos käyttäjät voivat hallita todennusmenetelmät, valitse Asetukset-välilehden Salli käyttäjien tapa, Tarkista Mobile-sovelluksesta. Tämä toiminto on käytössä, ilman loppukäyttäjät edellyttää yhteyttä Viimeistele aktivointi Mobile-sovelluksen Ohje työpisteesi.
2. Valitse Salli käyttäjien Aktivoi Mobile-sovelluksen ruutu.
3. Valitse Salli käyttäjien rekisteröinti-valintaruutu.
4. Mobile-sovelluksen kuvaketta.
5. Kirjoita URL-osoite ja jotka on luotu, kun asennat Azure multi-factor AuthenticationMobileAppWebServiceSetup näennäiskansio käytössä. Asiakkaan nimi voidaan kirjoittaa varattuun tilaan. Tämä yrityksen nimi näkyy mobiilisovelluksen. Jos tyhjä, luotu Azure hallinta-portaalin multi-factor Auth tarjoajan nimi näkyy.



<center>![Asetukset](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
