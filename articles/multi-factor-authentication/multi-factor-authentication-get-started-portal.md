<properties 
    pageTitle="Käyttöönotto käyttäjän portaalin Azure multi-factor Authentication-palvelin"
    description="Tämä on Azure multi-factor authentication-sivu, jossa kuvataan Azure MFA ja käyttäjä-portaalin käytön aloittaminen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Käyttäjä-portaalin käyttöönotto Azure multi-factor Authentication-palvelin

Käyttäjä-portaalin avulla asennetaan ja määritetään Azure multi-factor Authentication käyttäjä-portaalissa järjestelmänvalvoja. Käyttäjä-portaali on IIS-sivusto, jonka avulla käyttäjät voivat Azure Monimenetelmäisen todentamisen rekisteröidä ja ylläpitää niiden tilit. Käyttäjä voi muuttaa niiden puhelinnumero, niiden PIN-tunnuksen muuttaminen tai ohittaa Azure Monimenetelmäisen todentamisen aikana niiden Seuraava merkki.

Käyttäjät niiden Normaali käyttäjänimellä ja salasanalla käyttäjän portaalin kirjautuminen ja joko loppuun Azure Monimenetelmäisen todentamisen puhelun tai suojauksen kysymyksiin suorittamiseen niiden todennusta. Jos käyttäjän rekisteröitymisen on sallittu, käyttäjän määrittää niiden puhelinnumero ja PIN-tunnuksen ensimmäisen kerran kirjautuvat käyttäjä-portaaliin.

Käyttäjien Portal järjestelmänvalvojat voivat määrittäminen ja oikeutta Lisää uusia käyttäjiä ja päivittää aiemmin luodut käyttäjät.

<center>![Asetukset](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Käyttäjän portaalin samassa palvelimessa kuin Azure multi-factor Authentication-palvelimen käyttöönotto

Käyttäjät-portaalin asentamisessa samaan palvelimeen Azure multi-factor Authentication-palvelimeksi tarvitaan seuraavat vaatimukset:

- IIS on asennettava asp.net ja IIS 6 metatiedot perus yhteensopivuus (IIS 7 tai uudempi versio)
- Kirjautuneena sisään käyttäjällä on oltava tietokone ja toimialueen järjestelmänvalvojan oikeudet, jos sellainen on.  Tämä johtuu siitä, että tili on oikeus luoda Active Directory-käyttöoikeusryhmät.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Ottaa käyttöön käyttäjän portaalin Azure multi-factor Authentication-palvelin

1. Azure multi-factor Authentication Serverissä: vasemmanpuoleisessa valikossa käyttäjän Portal-kuvaketta, napsauta asentaa käyttäjän Portal-painiketta.
1. Valitse Seuraava.
1. Valitse Seuraava.
1. Jos tietokone on liitetty toimialueeseen ja käyttäjä-portaalin ja Azure multi-factor Authentication-palvelun välisen suojaamiseen Active Directory-määritys ei ole täydellinen, Active Directory-vaihe näkyvät. Valitse automaattinen täydentäminen määritysten Seuraava-painiketta.
1. Valitse Seuraava.
1. Valitse Seuraava.
1. Valitse Sulje.
1. Avaa selain miltä tahansa tietokoneelta, ja siirry URL-osoite, johon käyttäjä portaalissa on asennettu (esimerkiksi https://www.publicwebsite.com/MultiFactorAuth). Varmista, että sertifikaatti varoituksia tai virheitä näkyvät.

<center>![Asetukset](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Käyttöönotto muussa palvelimessa Azure Monimenetelmäisen todentamisen palvelimen käyttäjä-portaalissa

Jotta voit käyttää Azure multi-factor Authentication-sovellusta, seuraavassa on tarvittavat niin, että sovellus onnistuneesti viestiä käyttäjän-portaalissa:

Lue laite- ja ohjelmistovaatimukset laitteiston ja ohjelmistovaatimukset:

- Sinun on käytettävä 6.0 tai sitä uudempi versio Azure multi-factor Authentication-palvelimeen.
- Käyttäjän portaalissa on oltava asennettuna Microsoft® Internet Information Services (IIS) käynnissä Internetiin yhteydessä oleva WWW-palvelimessa 6.x, IIS 7.x tai uudempi versio.
- Kun käytät IIS 6.x, varmista ASP.NET v2.0.50727 on asennettuna, rekisteröity ja määritä sallitut.
- Pakollinen rooli services käytettäessä IIS 7.x tai uudempi versio ASP.NET ja IIS 6 metakannan yhteensopivuus.
- Käyttäjän Portal on suojattu SSL-varmenteen.
- Azure multi-factor Authentication Web palvelun SDK on oltava asennettuna IIS 6.x, IIS 7.x tai uudempi versio, joka on asennettu Azure multi-factor Authentication Server-palvelimeen.
- Azure multi-factor Authentication Web palvelun SDK on suojattu SSL-varmenteella.
- Käyttäjän Portal on voitava muodostaa Azure multi-factor Authentication Web Service SDK SSL-yhteyden kautta.
- Käyttäjän portaalissa on voitava muodostaa todentaa palvelutilin, joka kuuluu nimeltä "PhoneFactor järjestelmänvalvojat-käyttöoikeusryhmän tunnistetiedoilla Azure multi-factor Authentication Web Service SDK-paketissa. Tämän palvelutili ja ryhmä ovat Active Directory Jos Azure multi-factor Authentication Server suoritetaan palvelimessa toimialueeseen liittymistä. Tämän palvelutili ja ryhmän olemassa paikallisesti Azure multi-factor Authentication-palvelimeen jos se ei ole liitetty toimialue.

Käyttäjä-portaalin asentaminen palvelimessa kuin Azure multi-factor Authentication-palvelin edellyttää kolme vaihetta:

1. WWW-palvelun SDK asentaminen
2. Asenna käyttäjä-portaalissa
3. Portaalin käyttäjäasetusten määrittäminen Azure Monimenetelmäisen todentamisen Serverissä


### <a name="install-the-web-service-sdk"></a>WWW-palvelun SDK asentaminen

Jos Azure multi-factor Authentication Web palvelun SDK ei ole jo asennettu Azure multi-factor Authentication-palvelimeen, siirry tähän palvelimeen ja avaa Azure multi-factor Authentication-palvelimeen. Web-palvelu SDK-kuvaketta, valitse Asenna Web SDK... -painiketta ja noudata ohjeita, esitetään. Web-palvelu SDK on suojattu SSL-varmenteella. Itse allekirjoitetun varmenteen on kunnossa tähän tarkoitukseen, mutta se on tuotava "Luotettujen päämyöntäjien" store paikallistietokoneen tilin käyttäjän Portal-WWW-palvelimessa niin, että se luottaa sertifikaatin, kun aloitetaan SSL-yhteyttä.

<center>![Asetukset](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Asenna käyttäjä-portaalissa

Ennen kuin asennat käyttäjän portaalin muussa palvelimessa, ota huomioon seuraavat:

- Kannattaa lisätä Avaa Internetiin yhteydessä oleva WWW-palvelimessa selain ja siirry Web palvelun SDK-paketissa, joka on syötetty seuraavan koodin korostetut URL-osoite. Jos selaimen siirtyä web-palveluun onnistuneesti, se kannattaa pyytää tunnistetietoja. Kirjoita käyttäjänimi ja salasana, joka on kirjoitettu seuraavan koodin korostetut juuri sellaisena kuin se näkyy tiedostossa. Varmista, että sertifikaatti varoituksia tai virheitä näkyvät.
- Jos palomuuri tai käänteisen välityspalvelimen istuu käyttäjän Portal verkkopalvelin eteen ja suorittamiseen SSL purkaminen, voit muokata seuraavan käyttäjän Portal koodin korostetut ja lisää seuraavat avaimen <appSettings> osan niin, että käyttäjä-portaalissa voit käyttää http sijaan https. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Asenna käyttäjä-portaalissa

1. Avaa Windowsin Resurssienhallinta Azure multi-factor Authentication Server-palvelimessa ja Selaa kansioon, johon on asennettu Azure multi-factor Authentication-Server (kuten C:\Program Files\Multi kertoimen todennus Server). Valitse MultiFactorAuthenticationUserPortalSetup asennustiedosto sopiva palvelimen, käyttäjän Portal asennetaan 32-bittisen tai 64-bittinen versio. Kopioi asennustiedosto Internetiin yhteydessä oleva palvelimeen.
2. Internetiin yhteydessä oleva WWW-palvelimessa asennustiedosto on suoritettava järjestelmänvalvojan oikeudet. Helpoin tapa on Avaa komentokehote järjestelmänvalvojana ja siirry sijaintiin, johon asennustiedosto on kopioitu.
3. Suorita MultiFactorAuthenticationUserPortalSetup64 asennustiedosto-sivuston ja näennäiskansio nimen muuttaminen tarvittaessa.
4. Käyttäjän portaalin asennuksen jälkeiset Selaa C:\inetpub\wwwroot\MultiFactorAuth (tai asianmukaiseen kansioon näennäiskansio nimen perusteella) ja Muokkaa seuraavan koodin korostetut.
5. Etsi USE_WEB_SERVICE_SDK-näppäintä ja muuttaa arvon false tosi. Etsi WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ja WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD näppäimet ja arvot arvoksi käyttäjänimi ja salasana, joka kuuluu PhoneFactor järjestelmänvalvojat-suojauksen palvelutilin käyttäjäryhmä (katso yllä koskevat vaatimukset-kohdassa). Kirjoita käyttäjänimi ja salasana rivin lopussa lainausmerkkien välissä, että (arvo = "" / >). On suositeltavaa käyttää täydellinen käyttäjänimi (esimerkiksi TOIMIALUE\käyttäjänimi tai machine\username)
6. Etsi pfup_pfwssdk_PfWsSdk-asetus ja muuta arvon "http://localhost:4898/PfWsSdk.asmx" WWW-palvelun SDK, jossa on käytössä Azure multi-factor Authentication-palvelimessa (kuten https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx) URL-osoite. Koska SSL käytetään tätä yhteyttä, koska SSL-varmenne on annettu palvelimen nimi ja URL-osoite on vastattava olevan varmenteen nimi on viitattava WWW-palvelun SDK palvelimen nimi ja ei IP-osoite. Jos palvelimen nimi ei ratkaise IP-osoite Internetiin yhteydessä oleva palvelimesta, tekstin lisääminen yhdistämään Azure multi-factor Authentication-palvelimen nimi IP-osoite palvelimen isännät-tiedosto. Tallenna seuraavan koodin korostetut jälkeen tehdyt muutokset.
7. Jos sivuston käyttäjän Portal asennettiin kohdassa (kuten oletussivuston) ei vielä ole binded julkisesti kirjautunut sertifikaatilla, palvelin varmenteen asentaminen jollei jo asennettu, Avaa IIS Manager ja sitoa varmenteen verkkosivustoon.
8. Avaa selain miltä tahansa tietokoneelta, ja siirry URL-osoite, johon käyttäjä portaalissa on asennettu (esimerkiksi https://www.publicwebsite.com/MultiFactorAuth). Varmista, että sertifikaatti varoituksia tai virheitä näkyvät.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Portaalin käyttäjäasetusten määrittäminen Azure multi-factor Authentication-palvelimessa
Nyt kun portaalissa on asennettu, sinun on ‑palvelin Azure multi-factor Authentication portaalin käyttöä varten.

Azure Monimenetelmäisen todentamisen palvelin on useita vaihtoehtoja käyttäjän-portaalin.  Seuraavassa taulukossa on luettelo näistä asetuksista ja explaination, miten niitä käytetään.

Portaalin Käyttäjäasetukset|Kuvaus|
:------------- | :------------- |
Käyttäjän portaalin URL-osoite| Voit URL-osoite, jossa portaalin sijaitsee.
Ensisijainen käyttöoikeuden| Voit määrittää todennus käyttämään portaalin kirjautumisessa.  Windows-, säde- tai LDAP-todennusta.
Käyttäjät voivat kirjautua|Käyttäjät voivat kirjoittaa käyttäjänimi ja salasana kirjautumissivulla käyttäjä-portaalin.  Jos tämä ei ole valittu, ruutujen harmaana.
Salli käyttäjien rekisteröinti|Sallii käyttäjän rekisteröityä monimenetelmäisen todentamisen tehokkaasta ne asennusnäyttö, jossa pyydetään lisätietoja, kuten puhelinnumero.  Kehote, varmuuskopion puhelimen avulla käyttäjät voivat määrittää toissijaisen puhelinnumero.  Kysy kolmannen osapuolen sijasta tunnuksen avulla käyttäjät voivat määrittää 3 osapuolen sijasta tunnusta.
Käyttäjät voivat aloittaa One-Time ohitus| Tämän avulla käyttäjät voivat aloittaa erikseen Ohita.  Jos tämä siihen kestää käyttäjä-joukot vaikuttavat seuraavan kerran käyttäjä kirjautuu.  Kysy ohituksen sekuntia sallii käyttäjän ruutu, jotta he voivat muuttaa 300 sekunnin oletusarvon.  Muussa tapauksessa erikseen ohituksen on vain hyvä 300 sekunnin ajan.
Käyttäjät voivat valita menetelmä| Käyttäjät voivat määrittää niiden ensisijainen yhteydenottotapa.  Tämä voi olla puhelun, tekstiviesti-mobiilisovelluksen tai sijasta tunnuksen.
Käyttäjät voivat valita kieli|  Käyttäjä voi muuttaa kielen, jota käytetään puhelun, tekstiviesti-mobiilisovelluksen tai sijasta tunnuksen.
Käyttäjät voivat aktivoida mobiilisovelluksessa| Avulla käyttäjät voivat luoda aktivointi-koodin suorittamiseen mobiilisovelluksen aktivointi, jota käytetään palvelimen kanssa.  Voit myös määrittää niitä voi aktivoida-laitteiden määrää.  Väliltä 1 – 10.
Käytä varalomakkeena tietoturvaan liittyvät kysymykset|Voit käyttää tietoturvaan liittyvät kysymykset, jos monimenetelmäisen todentamisen epäonnistuu.  Voit määrittää minuuttimäärän, jonka on vastattava onnistuneesti tietoturvaan liittyvät kysymykset.
Käyttäjät voivat yhdistää kolmannen osapuolen sijasta tunnus| Käyttäjät voivat määrittää kolmannen osapuolen sijasta-tunnuksen.
Käytä sijasta tunnuksen varalomakkeena|Sallii sijasta tunnuksen käyttö tapahtuma, monimenetelmäisen todentamisen ei onnistu.  Voit myös määrittää istunnon aikakatkaisu minuutteina.
Kirjaamisen ottaminen käyttöön|Ottaa lokiin kirjaaminen käyttäjä-portaalissa.  Lokitiedostojen sijaitsevat: C:\Program Files\Multi kertoimen todennus Server\Logs.

Useimpia nämä asetukset ovat näkyvissä käyttäjän, kun ne ovat käytössä ja käyttäjä-portaaliin käyttäjä-merkit.

![Portaalin Käyttäjäasetukset](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Portaalin käyttäjäasetusten määrittäminen Azure multi-factor Authentication-palvelin




1. Napsauta Azure multi-factor Authentication-palvelimen käyttäjän Portal-kuvaketta. Asetukset-välilehden Kirjoita käyttäjän portaaliin käyttäjän portaalin URL-tekstiruutuun URL-osoite. Tätä URL-Osoitetta lisätty sähköpostit, jotka lähetetään käyttäjille, kun ne tuodaan Azure multi-factor Authentication-palvelimeen Jos sähköposti-toiminto on otettu käyttöön.
2. Valitse asetukset, jota haluat käyttää käyttäjä-portaalissa. Esimerkiksi jos käyttäjät voivat hallita niiden todennusmenetelmät, varmista, että käyttäjät voivat valita menetelmä kuitattu sekä menetelmiä, he voivat valita.
3. Napsauta ikkunan oikeassa yläkulmassa ohjeita tietoja kaikista näkyvät asetukset Ohje-linkkiä.

<center>![Asetukset](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Järjestelmänvalvojat-välilehti
Tässä välilehdessä riittää, että voit lisätä käyttäjät, joilla on järjestelmänvalvojan oikeudet.  Kun lisäät järjestelmänvalvoja, voit hienosäätää käyttöoikeudet, jotka he saavat.  Näin voit varmistaa tarvittavien käyttöoikeuksien myöntäminen vain järjestelmänvalvoja.  Valitse Lisää-painiketta ja valitse sitten ja käyttäjän ja käyttöoikeudet ja valitse sitten Lisää.

![Käyttäjien portaalin Järjestelmänvalvojat](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Tietoturvaan liittyvät kysymykset
Tässä välilehdessä voit määrittää suojauksen kysymykset, jotka käyttäjät tarvitsevat antamaan vastauksia, jos Käytä tietoturvaan liittyvät kysymykset varakyselyjen asetus on valittuna.  Azure multi-factor Authenticaton palvelimen sisältyy, jota voit käyttää oletuskysymyksiä.  Voit myös muuttaa järjestystä, tai lisätä omia kysymyksiä.  Kun lisäät oma kysymyksesi, voit määrittää haluat näkyvän sekä näiden kysymys kieli.

![Käyttäjän portaalin tietoturvaan liittyvät kysymykset](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Välitetty istunnot

## <a name="saml"></a>SAML
Sallii käyttäjän portaalin Hyväksy vaateita, jotka tunnistetietojen toimittaja, käyttämällä SAML-asetukset.  Voit määrittää istunnon aikakatkaisu, Määritä Varmenteen todentaminen ja kirjaudu ulos uudelleenohjauksen URL-osoite.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Luotettu IP-osoitteet
Tässä välilehdessä voit määrittää yksittäisen IP-osoitteet tai joka voidaan lisätä niin, että jos käyttäjä on kirjautumisessa olevista nämä IP-osoitteet, on ohitettu monimenetelmäisen todentamisen IP-osoitealueet.

![Käyttäjän portal Luotetut IP-osoitteet](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Omatoiminen käyttäjän rekisteröinti
Jos haluat, että käyttäjät voivat kirjautua sisään ja rekisteröi, valitse Salli käyttäjien ottaa kirjautuminen ja sallia käyttäjän rekisteröitymisen asetukset. Muista, että valitset asetukset vaikuttavat käyttäjän kirjautuminen.

Esimerkiksi, kun käyttäjä kirjautuu sisään käyttäjä-portaaliin ja napsauttaa Kirjaudu sisään-painiketta, ne sitten otetaan Azure multi-factor Authentication-käyttäjäasetusten sivulle.  Sen mukaan, miten olet määrittänyt Azure Monimenetelmäisen todentamisen käyttäjä voi olla pysty valitsemaan niiden todentamismenetelmän.  

Jos ne valita todentamismenetelmän äänipuhelu tai on valmiiksi määritetyn tämän käyttämistä, sivun kehote käyttäjä voi syöttää Ensisijainen puhelinnumero ja mahdollinen alanumero.  He voivat myös voi varmuuskopion puhelinnumero.  

![Käyttäjän portal Luotetut IP-osoitteet](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Jos käyttäjä on tarvitse käyttää PIN-tunnusta, kun ne todentavat, sivun myös pyydä heitä PIN-tunnus.  Kun olet lisännyt puhelinnumerot- ja PIN-tunnus (jos saatavilla), käyttäjä napsauttaa Soita minulle nyt Tarkista-painike.  Azure Monimenetelmäisen todentamisen suorittaa puhelu-todennus käyttäjän ensisijaisen puhelinnumeroon.  Käyttäjän täytyy vastaa puheluun ja kirjoita niiden PIN-tunnus (jos saatavilla) ja paina #, siirry seuraavaan vaiheeseen itsenäisen laitteen käyttöönottoprosessin yhteydessä.   

Jos käyttäjä valitsee SMS tekstin todennusmenetelmän tai on jo valmiiksi määritetyn tämän käyttämistä, sivun kysyy käyttäjältä, niiden matkapuhelinnumero.  Jos käyttäjä on tarvitse käyttää PIN-tunnusta, kun ne todentavat, sivun myös pyydä heitä PIN-tunnus.  Kun olet lisännyt henkilön puhelinnumero ja PIN-tunnus (jos saatavilla), käyttäjä valitsee tekstin Me nyt Tarkista-painike.  Azure Monimenetelmäisen todentamisen suorittaa SMS-todennus käyttäjän matkapuhelimeen.  Käyttäjän on saatava SMS, joka sisältää yhden-aika-tunnuskoodi (OTP-Portaalin) ja vastaa viestiin, OTP-Portaalin sekä niiden PIN-tunnus, jos sellainen on), siirry seuraavaan vaiheeseen itsenäisen laitteen käyttöönottoprosessin yhteydessä.

![Käyttäjän portal SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Jos käyttäjä valitsee mobiilisovelluksen todennusmenetelmän tai on jo valmiiksi määritetyn tämän käyttämistä, sivun kysyy käyttäjältä Azure multi-factor Authentication-sovelluksen asentaminen niiden laitteen ja luo aktivointikoodi.  Asennettuasi Azure multi-factor Authentication-sovelluksen käyttäjä napsauttaa Luo aktivointikoodi-painiketta.    

>[AZURE.NOTE]Jotta voit käyttää Azure multi-factor Authentication-sovellusta, käyttäjän on otettava käyttöön push-ilmoitukset niiden laitteen.

Sivun näyttää aktivointi-koodin ja sekä viivakoodin kuvan URL-osoite.  Jos käyttäjä on tarvitse käyttää PIN-tunnusta, kun ne todentavat, sivun myös pyydä heitä PIN-tunnus.  Käyttäjän aktivoinnin koodi ja URL-Osoitteen toimivana Azure multi-factor Authentication-sovelluksen tai käyttää Viivakoodiskanneri skannaa viivakoodi-kuva ja napsauttaa Aktivoi-painike.    

Aktivointi on valmis, kun käyttäjä napsauttaa todennetaan nyt-painiketta.  Azure Monimenetelmäisen todentamisen suorittaa mobiilisovelluksen käyttäjän todennusta.  Käyttäjän on niiden PIN-tunnus (jos saatavilla) ja paina valitsemalla Tarkista niiden-mobiilisovelluksen Siirry seuraavaan vaiheeseen itsenäisen laitteen käyttöönottoprosessin yhteydessä.  


Jos järjestelmänvalvojat on määritetty kerätä tietoturvaan liittyvät kysymykset ja vastaukset Azure multi-factor Authentication-palvelimen, käyttäjän on otettava tietoturvaan liittyvät kysymykset-sivulle.  Käyttäjän on neljä tietoturvaan liittyvät kysymykset Valitse ja kirjoita vastauksen valitun kysymyksiin.    

![Käyttäjän portaalin tietoturvaan liittyvät kysymykset](./media/multi-factor-authentication-get-started-portal/secq.png)  

Käyttäjän itsenäisen rekisteröinti on nyt valmis ja käyttäjä on kirjautunut käyttäjä-portaaliin.  Käyttäjät voi kirjautua takaisin käyttäjän portaalin milloin tahansa myöhemmin, jos haluat muuttaa puhelinnumerot, PIN, todennusmenetelmät ja tietoturvaan liittyvät kysymykset sallii hänen järjestelmänvalvojat.
