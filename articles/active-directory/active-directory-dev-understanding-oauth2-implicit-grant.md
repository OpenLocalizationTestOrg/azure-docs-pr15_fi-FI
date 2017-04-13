<properties
   pageTitle="Tietoja implisiittinen OAuth2 myöntää työnkulku Azure Active Directoryn | Microsoft Azure"
   description="Lue lisätietoja Azure Active Directory soveltaminen implisiittinen OAuth2 myöntää työnkulku- ja onko sen sovelluksesi oikealle."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Tietoja OAuth2 implisiittinen myöntäminen työnkulku-Azure Active Directory (AD)

OAuth2 implisiittinen myöntäminen on notorious kyseisenä myöntäminen suojaus koskee OAuth2 määrityksen pisimmän luettelon kanssa. Ja vielä, jotka on toteutettu ADAL JS ja esitys, on suositeltavaa, kun kirjoitat SPA sovellusten toimintatapa sen vuoksi. Mitä avulla? Se on kaikki ohjeisiin ja kompromissien: ja poistaa, implisiittinen myöntäminen on paras tapa voit ratkaista sovelluksissa, jotka kuluttavat verkko-Ohjelmointirajapinnan kautta JavaScript selaimesta.

## <a name="what-is-the-oauth2-implicit-grant"></a>Mikä on OAuth2 implisiittinen myöntäminen?

Käyttöoikeuksien myöntämistä joka käyttää eri päätepisteet quintessential [OAuth2 luvan koodin myöntää](https://tools.ietf.org/html/rfc6749#section-1.3.1) on. Todennus-päätepisteen käytetään käyttäjän vuorovaikutus vaihe, joka johtaa todennus-koodi. Suojaustunnuksen päätepisteen käytetään asiakkaan siirtämisen koodi käyttöoikeustietue ja usein Päivitä-tunnusta. Verkkosovellusten tarvitaan esittää suojaustunnuksen päätepisteen sovelluksen omia tunnukset niin, että luvan palvelimen voi todentaa asiakkaan.

[Implisiittinen OAuth2 myöntää](https://tools.ietf.org/html/rfc6749#section-1.3.2) on variantin salliminen asiakas, voit hankkia käyttöoikeustietue (ja id_token kyseessä [OpenId yhteyden](http://openid.net/specs/openid-connect-core-1_0.html)) suoraan luvan päätepisteen ilman yhteyttä suojaustunnuksen päätepisteen eikä asiakassovellus todennustapa. Tämä on suunniteltu erityisesti n JavaScript selaimessa sovellusten: alkuperäinen OAuth2-määrityksessä tunnusten palautetaan URI-osa. Joka mahdollistaa suojaustunnuksen bittien JavaScript-koodia työpöytäsovelluksessa, mutta se takaa Uudelleenohjaa kohti palvelin ei sisällytetä ne. Selaimen kautta, joka palauttaa tunnusten ohjaa suoraan luvan päätepiste. Siinä on myös hyödyntää poistamisesta minkä tahansa vaatimukset cross origin soittamiseen, joita tarvitaan, jos JavaScript-sovelluksen tarvitaan yhteystiedot suojaustunnuksen päätepiste.

Tärkeää ominaisuus OAuth2 implisiittinen myöntäminen on tärkeä, kuten jatkuu koskaan palauttaa päivityksen tunnusten asiakkaalle. Olemme huomaamaan seuraavan osion, joka ei ole välttämätön todella ja olisi mahdollista tietosuojaongelmien vuoksi.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Sopivan tilanteita, joissa OAuth2 implisiittinen myöntäminen

OAuth2 määrityksen itse ilmoittaa, kun implisiittinen myöntäminen on ole tehneet mahdolliseksi kehittää käyttöön käyttäjäagentti sovellusten – toisin sanoen JavaScript-sovellukset suoritetaan selaimessa. Näiden sovellusten määrittämistä ominaisuus on käytettävä JavaScript-koodia palvelinresurssien (yleensä verkko-Ohjelmointirajapinnan) avaaminen ja päivittää sovelluksen UX vastaavasti. Jaettu muiden käyttäjien sovellukset, kuten Gmail- tai Outlook Web Accessin: Kun valitset viesti Saapuneet-kansiosta, viestin visualisoinnin Ohjauspaneelin muutoksista näyttää uuden valinnan, koko sivun sijaan säilyy ole muokattu. Tämä on in contrast with perinteinen redirect-pohjainen Web Apps-sovellusten, jossa jokaisen vuorovaikutteisuuden tuottaa koko sivun paluuviestissä ja uuden palvelimen vastauksen koko sivun muodostamisen.

Sovellukset, jotka vievät mukaan JavaScript-menetelmä sen hyvin voimakas kutsutaan yksittäisen sivun sovellusten tai kylpylät: idea on, että näiden sovellusten yhteyshenkilönä vain HTML-Aloitussivu ja liittyvän JavaScript, kanssa vuorovaikutuksessa parhaillaan perustuva verkko-Ohjelmointirajapinnan puhelut suorittaa JavaScript kautta. Kuitenkin hybrid tavoista, jossa sovellus on lähinnä takaisinlähetyksen perustuva, mutta suorittaa ajoittaiset JS puhelut, eivät ole epätavallisten – implisiittinen työnkulku käyttö keskustelu on merkitystä käyttäjille, myös.

Redirect-sovelluksissa suojatun yleensä pyyntöjä kautta evästeet, joka lähestymistavan ei toimi sekä JavaScript-sovelluksissa. Evästeiden toimivat vain ne on luotu, kun JavaScript-puhelut voi suunnattu muiden toimialueiden toimialueelle. Itse asiassa, jotka usein ovat kirjainkokoa: jaettu muiden käyttäjien sovellusten käynnistettäessä Microsoft Graph API Office API Azure API – kaikki asuviin-kohtaa, johon sovelluksen served toimialueen ulkopuolella. JavaScript-sovellusten kasvavan trendin on lainkaan ei taustatietokannan, käyttävä 100 %: n 3 osapuolen verkko-ohjelmointirajapinnan toteuttamisesta niiden suorittamisen.

Tällä hetkellä ensisijainen tapa suojata puhelut verkko-Ohjelmointirajapinnan on käyttää OAuth2 haltijan suojaustunnuksen menetelmää, jossa OAuth2-käyttöoikeustietue liitettävä jokaisen kutsun. Verkko-Ohjelmointirajapinnan tutkii saapuvat käyttöoikeustietue, ja jos lukee ei tarvittavat alueet, se myöntää pääsyn toimintoa. Implisiittinen työnkulku on kätevä järjestelmä JavaScript-sovellusten, voit hankkia Accessin tunnusten verkko-Ohjelmointirajapinnan tarjoavat evästeet osin monia etuja:

- Tunnusten saadaan luotettavasti tarvitsematta cross alkuperän puhelut – pakolliseksi, joihin tunnusten on palautettavan URI takaa, että tunnusten ei joutuneita uudelleenohjaus
- JavaScript-sovelluksia voit hankkia niin monta access tunnuksia, kun he tarvitsevat, niin monta Web ohjelmointirajapinnan kohde – ei rajoitus toimialueiden kanssa
- HTML5-ominaisuudet, kuten istunnon tai paikallinen tallennus myöntää täydet oikeudet suojaustunnuksen välimuistiin ja elinkaaren hallinta-evästeet hallinta on peittävä-sovellukseen
- Access-tunnusten eivät ole alttiita pyynnön väärennös (CSRF) sivustojen välillä

Implisiittinen myöntäminen kulun myönnä päivityksen tunnusten enimmäkseen tietoturvasyistä. Päivitä-tunnuksen ei ole kuin narrowly määritetty kuin access-tunnusten myöntämistä paljon enemmän power inflicting paljon enemmän vioittuneet näin ollen siltä varalta, että se on vuotamaan. Implisiittinen työnkulun tunnusten toimitetaan URL-osoite, näin ollen kaappaamiselta riski on suurempi kuin luvan koodin myöntäminen.

Huomaa kuitenkin, että JavaScript-sovellus on toisen järjestelmä varten access tunnusten uudistamalla kysymättä toistuvasti käyttäjän tunnistetiedot käytettävissään. Sovellus käyttää piilotetun iframe-kehyksen suorittamiseen uuden tunnuksen pyynnöt Azure AD luvan päätepisteelle: kun selain on edelleen aktiivisen istunnon (Lue: on istunnon eväste) vastaan Azure AD-toimialueen todennuspyynnön tapahtuvat onnistuneesti tarvitsematta käyttäjän toimia. 

Tämän mallin myöntää JavaScript-sovelluksen uusia itsenäisesti access tunnusten ja hankkia uusi API uusia jopa (edellyttäen, että käyttäjän hyväksynyt aikaisemmin niistä. Tämä ei lisätty yrityksille hankkiminen, ylläpito ja suojaaminen suuren arvon Palvelutietojen, kuten päivitys-tunnuksen. Palvelutietojen joka mahdollistaa automaattista uusimista Azure AD-istunnon eväste, hallitaan sovelluksen ulkopuolella. Toisen tämän menetelmän etuna on se käyttäjän voit kirjautua ulos from Azure AD-sovelluksia, kirjautunut Azure AD-käynnissä kaikista selaimen välilehtien avulla. Tuloksena on Azure AD-istunnon evästeiden poistaminen ja JavaScript-sovelluksen häviää mahdollisuus uusia tunnusten allekirjoitetun ulos käyttäjälle.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Sopii sovelluksen implisiittinen myöntäminen?

Implisiittinen myöntäminen näkyy enemmän kuin muiden myöntää riskejä. Haluat kiinnittää huomiota alueilla on myös kuvattu (katso esimerkiksi [Väärin, Access suojaustunnuksen tekeytyminen resurssin omistajalle-implisiittinen Flow] [ OAuth2-Spec-Implicit-Misuse] ja [OAuth 2.0 malli ja suojausasiat][OAuth2-Threat-Model-And-Security-Implications]). Korkeampi riskin profiili on kuitenkin suurelta johtua, koska se on tarkoitus sovellusten, joka suorittaa aktiivisen koodia, served remote resurssin selaimeen. Jos suunnittelet SPA-arkkitehtuuri Taustajärjestelmä ei ole osia tai aiot käynnistää verkko-Ohjelmointirajapinnan kautta JavaScript, on suositeltavaa käyttää implisiittinen kulun tunnuksen saamiseksi.

Jos sovellus on native client, implisiittinen työnkulku ei ole hyvien Sovita. Azure AD-istunnon eväste native client kontekstissa puuttuessa deprives sovelluksesi niistä ylläpito pitkään lived istunnon. Mikä tarkoittaa, että sovellus kysyy toistuvasti käyttäjältä, access tunnusten uusi resurssien noudettaessa.

Jos kehität Web-sovelluksen mukaan lukien taustassa ja muissa Ohjelmointirajapinnan Taustajärjestelmä sen koodista, implisiittinen työnkulku ei myöskään ole hyvin ratkaistavaan. Muut myöntää antavat paljon enemmän power. Esimerkiksi OAuth2 asiakkaan tunnistetiedot myöntäminen tarjoaa mahdollisuuden saada tunnuksia, jotka eikä käyttäjien edustajia itse sovelluksen käyttöoikeuksien mukaisiksi. Tämä tarkoittaa asiakkaan on mahdollisuuden pitää ohjelmallisesti resurssien parillinen, kun käyttäjä ei ole aktiivisesti kytketty istunnon ja niin edelleen. Ei ainoastaan, mutta tällaisen myöntää avulla suurempi suojaus-oikeudet. Esimerkiksi access tunnusten kulkea koskaan käyttäjän selaimen kautta, ne eivät julkisuuteen tallennu selaimen historia ja niin edelleen. Asiakassovellus voi tehdä myös vahva todennus, kun pyydetään tunnusta.

## <a name="next-steps"></a>Seuraavat vaiheet

- Developer resurssien kattavaan luetteloon mukaan lukien lisätietoja protokollat ja OAuth2 käyttöoikeuksien myöntäminen kulkee tukemaan Azure AD, viitata [Azure AD Kehittäjän ohje][AAD-Developers-Guide]
- Katso, [miten voit integroida hakemus Azure AD]  [ ACOM-How-To-Integrate] muita syvyys-integroinnin hakeminen.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

