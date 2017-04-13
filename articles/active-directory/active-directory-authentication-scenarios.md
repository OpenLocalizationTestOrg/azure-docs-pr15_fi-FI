
<properties
   pageTitle="Todennus tilanteita, joissa Azure AD | Microsoft Azure"
   description="Yleiskatsaus viisi yleisimmät todennus-skenaariot varten Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Azure AD todennus tilanteita, joissa

Azure Active Directory (Azure AD) yksinkertaistaa kehittäjät todennus käyttöön tarjoamalla tunnistetietojen palvelun yleisesti protokollia, kuten OAuth 2.0 ja OpenID yhteyden tukeen sekä Avaa lähteenä kirjastojen avulla voit aloittaa nopeasti coding eri ympäristöissä. Tämän asiakirjan auttaa sinua ymmärtämään eri skenaarioissa Azure AD-tukee ja kerrotaan, kuinka pääset alkuun. Se on jaettu seuraavissa kohdissa:

- [Azure AD-todennus perusteet](#basics-of-authentication-in-azure-ad)

- [Azure AD-suojauksen tunnusten saatavat](#claims-in-azure-ad-security-tokens)

- [Sovelluksen rekisteröinti Azure AD perusteet](#basics-of-registering-an-application-in-azure-ad)

- [Sovelluksen tiedostotyypit ja -skenaariot](#application-types-and-scenarios)

  - [Web-sovelluksen Web-selaimessa](#web-browser-to-web-application)

  - [Yksittäisen sivun sovelluksen (SPA)](#single-page-application-spa)

  - [Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen](#native-application-to-web-api)

  - [Web-sovelluksen verkko-Ohjelmointirajapinnan](#web-application-to-web-api)

  - [Daemon tai verkko-Ohjelmointirajapinnan palvelimen sovelluksesta](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Azure AD-todennus perusteet

Jos olet perehtynyt peruskäsitteet Azure AD-todennuksen, lue tämä osio. Muussa tapauksessa haluat ohittaa [sovelluksen tiedostotyypit ja -skenaariot](#application-types-and-scenarios)alaspäin.

Seuraavassa missä tunnistetietojen yleisin skenaariota: selaimessa edellyttää, että käyttäjän tarkistamiseen verkkosovellukseen. Tässä skenaariossa on kuvattu tarkemmin [Selaimen verkkosovellukseen](#web-browser-to-web-application) -osassa, mutta se on hyödyllinen aloituskohdan kuvaavat Azure AD ominaisuuksia ja conceptualize skenaarion toiminta. Tutustu tämän skenaarion seuraavassa kaaviossa:

![Kertakirjauksen verkkosovellukseen yleiskatsaus](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Huomioon avulla näin huomioitavia seikkoja sen eri osat:

- Azure AD on tunnistetietojen toimittaja, vastuussa vahvistusta käyttäjät ja sovellukset, jotka ovat organisaation hakemiston henkilöllisyys ja varmenteiden kädessä suojauksen tunnusten todentaminen onnistuu näiden käyttäjien ja sovellusten yhteydessä.


- Sovellus, joka haluaa ulkoistaa Azure AD-todennus on oltava rekisteröity Azure AD, joka rekisteröi ja yksilöi sovelluksen hakemistossa.


- Kehittäjät voivat käyttää Avaa lähde Azure AD-todennus-kirjastoja, jotta todennus helposti sisällyttämällä protokolla-tietoja puolestasi. Lisätietoja on kohdassa [Azure Active Directory käyttöoikeuksien kirjastoihin](active-directory-authentication-libraries.md) .


•, Kun käyttäjä on tarkistettu, sovellus on vahvistettava käyttäjän suojaustunnus varmistamiseksi todennuksen onnistui tarkoitettu osapuolille. Kehittäjät voivat käyttää annettu todennus-kirjastojen käsittelemään vahvistamisen minkä tahansa Azure AD, mukaan lukien JSON Web tunnusten (JWT) tai SAML 2.0-tunnuksen. Jos haluat tehdä vahvistus manuaalisesti, on [JWT suojaustunnuksen käsittelijä](https://msdn.microsoft.com/library/dn205065.aspx) ohjeissa.


> [AZURE.IMPORTANT] Azure AD käytetään julkisen avaimen salaus Kirjaudu tunnusten ja varmista, että ne ovat kelvollisia. Katso lisätietoja tarvittavat logiikan sovelluksesi, jotta se on oltava [Tärkeitä tietoja tietoja kirjautuminen avaimen palauttaminen Azure AD-](active-directory-signing-key-rollover.md) päivittyy aina uusimmat näppäimet.


• Etenemisen todennusprosessin vastaukset määräytyy todennusprotokolla, jota käytettiin OAuth 2.0-OpenID yhteyden, kuten WS Federation tai SAML 2.0. Protokollat käsitellään tarkemmin [Azure Active Directory käyttöoikeuksien protokollat](active-directory-authentication-protocols.md) -artikkelin ja alla olevissa osioissa.

> [AZURE.NOTE] Azure AD tukee OAuth-2.0 ja OpenID yhteyden standardeja, jotka hyödyntämään monipuolisesti käyttäminen haltijan-tunnukset, mukaan lukien esitetään JWTs haltijan-tunnukset. Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Tässä mielessä "haltijan" on osapuolen, joka esittää tunnuksen. Vaikka osapuolen todentamismenetelmä on ensin Azure AD vastaanottamaan haltijatunnukseen, jos tarvittavat vaiheet ei oteta suojaaminen tunnuksen siirron ja tallennustilaa, se voidaan siepata ja käyttää tahattomia osapuoli. Kun joitakin suojauksen tunnusten on valmiita järjestelmä luvattoman estää niiden käyttämisen, haltijan-tunnukset ei ole tämä järjestelmä ja on kuljetus-suojatun kanavan, kuten transport layer security (HTTPS). Haltijatunnukseen siirretään Poista, jos jokin mies on keskimmäisen hyökkäys voidaan haittaohjelmien osapuolen hankkia tunnuksen ja käyttää sitä suojatun resurssin luvatonta pääsyä. Samojen suojauksen periaatteiden käyttää, kun tallentaminen tai tallentaminen välimuistiin haltijan-tunnukset myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti. Saat lisätietoja suojausasiat-haltijan-tunnukset, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).


Nyt kun olet luonut yleiskatsaus perustiedot, Lue miten valmistelu toimii Azure AD-ja yleisiä tilanteita, joissa Azure AD tukee seuraavia ohjeita.


## <a name="claims-in-azure-ad-security-tokens"></a>Azure AD-suojauksen tunnusten saatavat

Suojauksen tunnusten Azure AD myöntämä sisältää vaateet ja tietoja, jotka on tarkistettu aiheesta vahvistukset. Näitä väitteitä voidaan käyttää sovelluksen eri tehtävien. Esimerkiksi ne voidaan tarkistaa tunnuksen, tunnistaa hakijan directory vuokraajan, näyttää käyttäjätiedot, määrittää hakijan luvan ja niin edelleen. Esitä minkä tahansa määritetyn suojaustunnus saatavat riippuvat tunnuksen, todennetaan käyttäjä ja sovelluksen määritysten tunnistetiedon tyyppi tyyppi. Alla olevassa taulukossa on annettu kunkin ryhmän lähettämän Azure AD-tyyppisen lyhyt kuvaus. Lisätietoja on lisätietoja [tuettu tunnuksen ja varaa tyypit](active-directory-token-and-claims.md).


| Varaa | Kuvaus |
|-------|-------------|
| Tunnus | Tunnistaa sovellus, jossa on käytössä tunnuksen.
| Käyttäjäryhmälle | Yksilöi vastaanottajan resurssin tunnus on tarkoitettu. |
| Sovelluksen käyttöoikeuksien kontekstin luokan viittaus | Ilmaisee, miten asiakas ei todennettu (julkisen asiakas ja luottamuksellisista asiakas). |
| Pikaviestien todennus | Tietueiden päivämäärä ja kellonaika, jolloin todennus on tapahtunut. |
| Todentamismenetelmä | Ilmaisee, miten tunnuksen aihe todennettu (salasana, varmenteen jne.). |
| Etunimi | Sisältää käyttäjän etunimi joukkona Azure AD. |
| Ryhmät | Sisältää käyttäjä kuuluu tunnukset, Azure AD-objektiryhmiä. |
| Tunnistetietojen toimittaja | Tietueet, jotka todennettu tunnuksen aihe tunnistetietojen toimittaja. |
| Myöntää | Tietueiden, jolla tunnuksen julkaistiin, suojaustunnuksen tuoreus usein käytetty aika. |
| Myöntäjä | Tunnistaa, lähetetty tunnuksen sekä Azure AD-vuokraajan STS. |
| Sukunimi | Sisältää käyttäjän Sukunimi joukkona Azure AD. |
| Nimi | Sisältää ihmisten luettavissa arvo, joka määrittää tunnuksen aihe. |
| Objektitunnus | Sisältää Azure AD aihe pysyvä ja yksilöivä tunnus. |
| Roolit | Sisältää Azure AD-sovelluksen roolit, käyttäjä, jolle on myönnetty helpossa muodossa nimet. |
| Laajuus | Osoittaa asiakassovellukseen käyttöoikeudet. |
| Aihe | Ilmaisee, mitkä tunnuksen vahvistukset tiedot lyhennys. |
| Vuokraajan tunnus | Sisältää pysyvä ja yksilöivä tunnus, joka on myöntänyt tunnuksen directory vuokraajan. |
| Suojaustunnuksen elinaika | Määrittää aikavälin, jonka tunnus on voimassa. |
| Täydellinen käyttäjätunnus | Sisältää tärkeimmät käyttäjänimi ja aihe. |
| Versio | Sisältää tunnuksen versionumero. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Sovelluksen rekisteröinti Azure AD perusteet

Sovellus, jossa outsources Azure AD-todennus on rekisteröitävä hakemistossa. Tämä vaihe koskee kertoo Azure AD tietoja sovelluksesi, mukaan lukien URL-osoite, missä se sijaitsee, vastausten lähettämisen jälkeen todennus-ja sovelluksen URI URL-osoite. Tämä tieto on pakollinen joitakin keskeisiä syistä:

- Azure AD on koordinaatit pitää yhteyttä sovelluksen Sign tai vaihtamiseksi tunnusten käsiteltäessä. Näitä ovat seuraavat:

  - Sovelluksen tunnus URI: Sovelluksen tunnus. Tämän arvon lähetetään Azure AD todennuksen aikana ilmaisemaan, mihin sovellukseen soittajan haluaa tunnuksen varten. Lisäksi tämän arvon sisältyy tunnuksen niin, että sovelluksen tietää, se on tarkoitettu kohde.


  - Vastaa URL-osoite ja uudelleenohjata URI: Verkko-Ohjelmointirajapinnan tai web-sovelluksen vastaa URL-osoite on sijainti, johon Azure AD lähettää vastauksen, kuten tunnuksen, jos todennus on onnistunut. Alkuperäisen sovelluksen uudelleenohjata URI on yksilöllinen, johon Azure AD ohjaa käyttäjäagentti OAuth 2.0-pyynnössä.


  - Asiakastunnus: Sovellus, joka luodaan Azure AD, kun sovellus rekisteröidään tunnus. Kun pyydetään lupaa koodi tai tunnuksen, Asiakastunnus ja avain lähetetään Azure AD todennuksen aikana.


  - Avain: Avain, joka lähetetään sekä Ostajantunnus todennuksessa Azure AD Soita verkko-Ohjelmointirajapinnan.


- Azure AD on varmistettava sovellus on tarvittavat oikeudet käyttää hakemiston tietoja muiden sovellusten organisaatiossa, ja niin edelleen

Valmistelu muuttuu selkeä, kun tiedät, että jakautuvat kahteen luokkaan sovellukset, jotka on kehittänyt ja Azure Active Directory-integrointi:

- Yksittäinen vuokraajan sovelluksen: yhtä vuokraajan-sovellus on tarkoitettu yhden organisaation käytössä. Nämä ovat yleensä-liiketoiminta () Toimialasovellusten yrityksen developer kirjoittama. Yhtä vuokraajan sovelluksen tarvitsee vain yhden kansion käyttäjät käyttää, ja tuloksena se vain on valmisteltu samassa kansiossa. Organisaation kehittäjä yleensä rekisteröityjen nämä sovellukset.


- Usean vuokraajan sovelluksen: usean vuokraajan-sovellus on tarkoitettu käytettäväksi useissa yrityksissä ei vain yhden organisaation. Nämä ovat yleensä ohjelmiston nimellä--palvelun (SaaS) sovelluksilla riippumattomat toimittaja (ISV). Usean vuokraajan sovellukset on valmisteltu kunkin hakemiston, jossa niitä käytetään, ja joka edellyttää käyttäjän tai järjestelmänvalvojan suostumus rekisteröi ne. Suostumus prosessia käynnistyy, kun sovellus on rekisteröity hakemistossa ja annetaan Graph-Ohjelmointirajapinnan tai ehkä toiseen verkko-Ohjelmointirajapinnan käyttöoikeus. Kun käyttäjän tai järjestelmänvalvojan eri organisaatiostasi Rekisteröi sovelluksen käyttöä varten, he saavat valintaikkuna, jossa näkyy sovellus edellyttää käyttöoikeudet. Käyttäjän tai järjestelmänvalvojan voit hyväksyminen sitten sovellus, joka antaa sovellusten käyttö määritetyt tiedot ja lopuksi Rekisteröi sovelluksen niiden Directoryyn. Lisätietoja on artikkelissa [Yleistä suostumus puitteissa](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Muutamia muita huomioon otettavia seikkoja syntyy, kun usean vuokraajan-sovelluksen yhtä vuokraajan sovelluksen sijaan. Esimerkiksi jos teet sovelluksen käytettävissä käyttäjille useita kansioita, sinun on selvittää, mitkä vuokraajan, jolla ne ovat. Tarkistettava omassa Directory-käyttäjän, kun usean vuokraajan-sovellus on määritettävä tietyn käyttäjän kaikista kansioista Azure AD-tarvitsee vain yhtä vuokraajan-sovellus. Voit tehdä tämän Azure AD tarjoaa yleisiä todennus päätepiste, jossa usean vuokraajan minkä tahansa sovelluksen ohjata kirjautumisen pyynnöt vuokraajan kielikohtaiset päätepisteen sijaan. Tämä päätepiste on kaikki kansiot https://login.microsoftonline.com/common Azure AD-vuokraajan kielikohtaiset päätepisteen saattaa olla https://login.microsoftonline.com/contoso.onmicrosoft.com. Yleisiä päätepiste on erityisen tärkeä ottaa huomioon, kun kehittäminen sovelluksesi, sillä sinun on tarpeen logiikan käsittelemään useita alihallinnat Kirjaudu sisään, kirjaudu ulos- ja suojaustunnuksen vahvistuksen aikana.

Jos parhaillaan kehittävät yhtä vuokraajan-sovelluksen, mutta haluat ottaa sen käyttöön useissa organisaatioissa, voit helposti tehdä muutoksia sovellukseen ja Azure AD sen määrittäminen, jotta se usean vuokraajan pystyvät. Lisäksi Azure AD käyttää samaa allekirjoitetun avainta tunnusten kaikissa kansioissa, onko kirjoitit vuokraajan yhden tai usean vuokraajan sovelluksen käyttöoikeuksien.

Jokainen tässä asiakirjassa mainitut skenaariossa sisäinen osion, jossa kuvataan valmistelu vaatimukset. Tarkemmin tietoja valmistelu Azure AD-sovellus ja yksittäisten ja useiden vuokraajan sovellusten erot Saat lisätietoja [Azure Active Directory-integrointi sovelluksiin](active-directory-integrating-applications.md) . Jatka lukemista ymmärtää yleisiä sovelluksen skenaarioita Azure AD.

## <a name="application-types-and-scenarios"></a>Sovelluksen tiedostotyypit ja -skenaariot

Jokainen tässä asiakirjassa on kuvattu tilanteita voidaan kehittää käyttämällä eri kielillä ja ympäristöissä. He ovat kaikki palautettua mukaan valmis MALLIKOODEJA, jotka ovat käytettävissä Microsoftin [MALLIKOODEJA opas](active-directory-code-samples.md)tai suoraan vastaavan [Github otoksen säilöjen tietoihin](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Lisäksi, jos sovellus on tietyn kappaleen tai lopusta loppuun-skenaario-osioon, useimmissa tapauksissa vastaavia toimintoja voidaan lisätä erikseen. Esimerkiksi jos alkuperäisen sovellus, jossa verkko-Ohjelmointirajapinnan soittaa, voit helposti lisätä soittaa myös verkko-Ohjelmointirajapinnan web-sovelluksen. Seuraavassa kaaviossa on kuvattu nämä skenaariot ja sovelluksen tyypit, ja miten eri osat voidaan lisätä:

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Azure AD tukemat viisi ensisijainen sovelluksen skenaariot ovat seuraavat:

- [Verkkosovelluksen selaimen](#web-browser-to-web-application): käyttäjä on web-sovelluksen, joka on suojattu Azure AD kirjautuminen.

- [Yksittäisen sivun sovelluksen (SPA)](#single-page-application-spa): käyttäjä on yksi sivu-sovellus, joka on suojattu Azure AD kirjautuminen.

- [Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen](#native-application-to-web-api): alkuperäisen sovellus, joka suoritetaan puhelimella, tabletilla tai PC on resurssien käyttämistä verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD käyttäjä todennetaan.

- [Web-sovelluksen verkko-Ohjelmointirajapinnan](#web-application-to-web-api): web-sovelluksen täytyy hakemaan resurssien verkko-Ohjelmointirajapinnan, jotka on suojattu Azure AD.

- [Daemon tai palvelimen sovelluksesta verkko-Ohjelmointirajapinnan](#daemon-or-server-application-to-web-api): daemon sovelluksen tai palvelimen sovelluksen ilman WWW-käyttöliittymää tarvitsee resurssien käyttämistä verkko-Ohjelmointirajapinnan, jotka on suojattu Azure AD.

### <a name="web-browser-to-web-application"></a>Web-sovelluksen Web-selaimessa

Tässä osassa kuvataan sovellus, joka todentaa käyttäjän selaimessa verkkosovellukseen. Tässä skenaariossa verkkosovelluksen ohjaa käyttäjän selaimen Kirjaudu ne Azure AD. Azure AD palauttaa käyttäjän selaimessa, joka sisältää saatavat suojaustunnus käyttäjästä kautta kirjautumisen vastausta. Tässä skenaariossa tukee Sign WS federaatio, SAML 2.0 ja OpenID yhteyden-protokollia käyttäen.


#### <a name="diagram"></a>Kaavio
![Verkkosovelluksen selaimen todennus-työnkulku](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Kuvaus protokolla-työnkulku


1. Kun käyttäjä käy sovelluksen ja tarvitsee kirjautua sisään, ne ohjataan Azure AD-todennus päätepisteen kirjautumisen pyyntö kautta.


2. Käyttäjä kirjautuu sisään-sivulla.


3. Jos todennus on onnistunut, Azure AD Luo todennus-tunnuksen ja palauttaa kirjautumisen vastauksen sovelluksen vastaa URL-Osoitetta, joka on määritetty Azure hallinta-portaalissa. Tuotannon-sovelluksen tämän vastaa URL-Osoitteen tulisi HTTPS. Palautetut tunnuksen sisältää käyttäjän ja Azure AD saatavat, joita tarvitaan vahvistamiseen tunnuksen sovelluksen.


4. Sovelluksen vahvistaa tunnuksen käyttämällä allekirjoitetun julkinen avain ja myöntäjän tietoja käytettävissä osoitteessa federation metatietojen asiakirja Azure AD. Kun sovellus tarkistaa tunnuksen, Azure AD aloittaa uuden istunnon käyttäjän kanssa. Tässä istunnossa avulla käyttäjä voi käyttää sovellusta, ennen kuin se päättyy.


#### <a name="code-samples"></a>MALLIKOODEJA


Saat Web-selaimessa MALLIKOODEJA verkkosovelluksen skenaarioita. Ja tarkista usein--kaavaan lisätään uusi näytteiden aina. [Selain verkkosovellukseen](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Rekisteröiminen


- Yhtä vuokraajan: Jos kokoat sovelluksen juuri organisaatiollesi, sen on oltava rekisteröity yrityksen Directoryn avulla Azure hallinta-portaalin.


- Usean vuokraaja: Jos olet muodostamassa sovellus, joka voidaan käyttää organisaation ulkopuolisten käyttäjien, sen on oltava rekisteröity yrityksen hakemistosta, mutta myös on oltava rekisteröity kunkin organisaation hakemiston, jotka käyttävät sovellus. Jotta sovelluksesi niiden hakemistossa, voit lisätä rekisteröintiprosessi loppuun asiakkaille, että ne voivat sovelluksen suostumus. Kun vastaanottajiksi sovelluksesi, ne esitetään valintaikkuna, jossa näkyy sovellus edellyttää käyttöoikeudet, ja voit hyväksyminen. Tarvittavat käyttöoikeudet, riippuen muiden organisaation järjestelmänvalvoja saattaa edellyttää suostumus. Käyttäjän tai järjestelmänvalvojan suostuu, kun sovellus rekisteröidään niiden hakemisto. Lisätietoja on artikkelissa [Azure Active Directory-integrointi sovelluksiin](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Tunnuksen vanhentumisen

Käyttäjän istunto päättyy, kun Azure AD myöntämä tunnuksen käyttöaika päättyy. Sovelluksen lyhentää tänä halutessasi esimerkiksi uloskirjautuminen käyttäjille ajan, perusteella. Kun istunto vanhenee, käyttäjää pyydetään kirjautua sisään uudelleen.





### <a name="single-page-application-spa"></a>Yksittäisen sivun sovelluksen (SPA)

Tässä osassa kuvataan yhden sivun-sovelluksen, joka käyttää Azure AD- ja OAuth 2.0 implisiittinen käyttöoikeuksien myöntäminen suojattu sen takaisin API lopettaa web-todennus. Yksi sivu-sovellukset ovat yleensä rakenne JavaScript esityksen kerros (edusta), joka suoritetaan selaimessa ja verkko-Ohjelmointirajapinnan taustatietokannaksi, joka suoritetaan palvelimessa ja toteuttaa sovelluksen liiketoimintalogiikan. Lisätietoja implisiittinen luvan myöntäminen ja päättää, onko juuri sovelluksen skenaarion ohjeen kohdassa [tietoa OAuth2 implisiittinen myöntäminen kulun Azure Active Directoryn](active-directory-dev-understanding-oauth2-implicit-grant.md).

Tässä skenaariossa käyttäjän kirjautuessa sisään, JavaScript-koodia eteen käyttötarkoitukset [Active Directory käyttöoikeuksien kirjaston varten JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) ja implisiittinen luvan myöntäminen saada Azure AD tunnus (id_token). Tunnuksen välimuistin ja asiakkaan liittää sen pyynnön nimellä haltijatunnukseen takaisin loppuun, joka on suojattu OWIN middleware sen verkko-Ohjelmointirajapinnan soittamista. 
#### <a name="diagram"></a>Kaavio

![Yksittäisen sivun sovelluksen-kaavio](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Kuvaus protokolla-työnkulku

1. Kun käyttäjä siirtyy web-sovelluksen.


2. Sovelluksen palauttaa JavaScript-edusta (esityksen layer) selaimessa.


3. Käyttäjä aloittaa Kirjaudu sisään, esimerkiksi valitsemalla Allekirjoita-linkkiä. Selain lähettää GET Azure AD luvan päätepisteen pyytää ID-tunnusta. Pyyntö sisältää asiakkaan tunnuksen ja vastaa URL kyselyparametrit.


4. Azure AD vahvistaa vastaa URL-Osoitteen vastaan rekisteröity vastaa URL-Osoitetta, joka on määritetty Azure hallinta-portaalissa.


5. Käyttäjä kirjautuu sisään-sivulla.


6. Jos todennus on onnistunut, Azure AD Luo ID-tunnus ja palauttaa sen URL-Osoitteen osa (#) sovelluksen vastaa URL-osoitteeseen. Tuotannon-sovelluksen tämän vastaa URL-Osoitteen tulisi HTTPS. Palautetut tunnuksen sisältää käyttäjän ja Azure AD saatavat, joita tarvitaan vahvistamiseen tunnuksen sovelluksen.


7. JavaScript-asiakasohjelman koodia käynnissä selaimessa poimii tunnuksen käyttäminen suojaaminen puhelut sovelluksen web API takaisin lopettaa vastaus.


8. Selaimen puhelujen sovelluksen verkko-Ohjelmointirajapinnan takaisin pääty käyttöoikeustietue authorization-otsikko.

#### <a name="code-samples"></a>MALLIKOODEJA


Katso MALLIKOODEJA skenaarioissa yksittäisen sivun sovelluksen (SPA). Tarkista usein--kaavaan lisätään uusi näytteiden aina. [Yksittäisen sivun sovelluksen (SPA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Rekisteröiminen


- Yhtä vuokraajan: Jos kokoat sovelluksen juuri organisaatiollesi, sen on oltava rekisteröity yrityksen Directoryn avulla Azure hallinta-portaalin.


- Usean vuokraaja: Jos olet muodostamassa sovellus, joka voidaan käyttää organisaation ulkopuolisten käyttäjien, sen on oltava rekisteröity yrityksen hakemistosta, mutta myös on oltava rekisteröity kunkin organisaation hakemiston, jotka käyttävät sovellus. Jotta sovelluksesi niiden hakemistossa, voit lisätä rekisteröintiprosessi loppuun asiakkaille, että ne voivat sovelluksen suostumus. Kun vastaanottajiksi sovelluksesi, ne esitetään valintaikkuna, jossa näkyy sovellus edellyttää käyttöoikeudet, ja voit hyväksyminen. Tarvittavat käyttöoikeudet, riippuen muiden organisaation järjestelmänvalvoja saattaa edellyttää suostumus. Käyttäjän tai järjestelmänvalvojan suostuu, kun sovellus rekisteröidään niiden hakemisto. Lisätietoja on artikkelissa [Azure Active Directory-integrointi sovelluksiin](active-directory-integrating-applications.md).

Kun sovellus rekisteröidään, sen on oltava määritettynä OAuth 2.0 implisiittinen Grant-protokollaa. Oletusarvoisesti tämä protokolla ei ole käytettävissä sovellukset. Sovelluksen OAuth2 implisiittinen Grant-protokollan käyttöön lataa sen sovellusluettelo Azure hallinta-portaalin, aseta arvoksi true "oauth2AllowImplicitFlow" ja lataa sitten portaaliin luettelon Edellinen. Lisätietoja on artikkelissa [Ottaminen käyttöön OAuth 2.0 implisiittinen myöntäminen yksittäisen sivun sovellusten](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Tunnuksen vanhentumisen

Kun ADAL.js avulla voit hallita todentaminen Azure AD-etuja useita ominaisuuksia, jotka päivittäminen päättyneen tunnuksen sekä muut verkko-Ohjelmointirajapinnan resurssien, jotka voidaan kutsua sovellus tunnusten käytön helpottamiseksi. Kun käyttäjä todentaa onnistuneesti Azure AD-istunnon eväste suojattu muodostetaan käyttäjän selaimen ja Azure AD välillä. On tärkeää muistaa istunnon olemassa käyttäjä- ja Azure AD sekä ei käyttäjä ja palvelimessa web-sovelluksen välillä. Kun tunnusta vanhenee, ADAL.js käyttää toisen tunnuksen hankkiminen äänettömästi tässä istunnossa. Se tekee piilotetun iframe-kehyksen avulla voit lähettää ja vastaanottaa pyynnön OAuth implisiittinen Grant-protokollan avulla. ADAL.js myös käyttämällä tämän saman järjestelmän äänettömästi hankkia Accessin tunnusten Azure AD muiden Web API resurssien avulla sovelluksen puhelut, kunhan nämä resurssit tukevat rajat origin resurssien jakaminen (CORS) on rekisteröity käyttäjän kansio ja kaikki tarvittavat suostumusta annettiin käyttäjän kirjautumisen aikana.


### <a name="native-application-to-web-api"></a>Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen


Tässä osassa kuvataan alkuperäisen sovellus, jossa puhelut web API käyttäjän puolesta. Tässä skenaariossa on muodostettu julkisen avulla OAuth 2.0 luvan koodin myöntäminen tyyppi [OAuth 2.0-määrityksen](http://tools.ietf.org/html/rfc6749)4.1 kohdassa kuvatulla tavalla. Alkuperäisen sovelluksen hakee käyttöoikeustietue, käyttäjän OAuth 2.0-protokollan avulla. Tämä käyttöoikeustietue lähetetään pyyntö verkko-Ohjelmointirajapinnan, joka sallii käyttäjän ja palauttaa halutun resurssi.

#### <a name="diagram"></a>Kaavio

![Alkuperäisen sovelluksen verkko-Ohjelmointirajapinnan kaavio](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Alkuperäisen sovelluksen API todennus-työnkulku

#### <a name="description-of-protocol-flow"></a>Kuvaus protokolla-työnkulku


Jos käytät AD todennus-kirjastoja, suurin osa jäljempänä protokolla tiedot käsitellään, kuten selaimen ponnahdusikkuna, suojaustunnuksen välimuistin ja Päivitä tunnusten käsittely.

1. Ponnahdusikkunoiden alkuperäisestä sovelluksesta tekee pyyntö luvan päätepisteen Azure AD-selaimen avulla. Pyyntö sisältää Asiakastunnus ja uudelleenohjaus URI alkuperäisen sovelluksen hallinta-portaalin ja verkko-Ohjelmointirajapinnan tunnus URI-sovelluksen esitetyllä tavalla. Jos käyttäjä ei ole vielä kirjautunut sisään, heitä pyydetään kirjautumaan uudelleen


2. Azure AD todentaa käyttäjän. Jos se on usean vuokraajan sovellus ja suostumusta edellyttää sovelluksen käyttöä varten, käyttäjän tarvitaan suostumus, jos niitä ei ole vielä lisätty. Jälkeen luvan myöntämistä ja todentaminen onnistuu yhteydessä Azure AD ongelmat luvassa koodi-vastauksen takaisin asiakassovellukseen uudelleenohjaus URI.


3. Kun Azure AD ongelmat luvassa koodi-vastauksen takaisin uudelleenohjaus URI-asiakassovellus lopettaa selaimen vuorovaikutus ja luvan koodin poimii vastaus. Käyttämällä tätä luvan asiakassovellus lähettää pyynnön Azure AD-tunnuksen päätepisteen, joka sisältää todennus-koodi, tiedot-asiakassovellukseen (Ostajantunnus ja uudelleenohjaus URI) ja haluamasi resurssi (verkko-Ohjelmointirajapinnan tunnus URI hakeminen).


4. Azure AD vahvistaa luvan koodi ja asiakkaan sovelluksen ja verkko-Ohjelmointirajapinnan tietoja. Onnistuneen vahvistuksen yhteydessä Azure AD palauttaa kahden tunnusten: JWT käyttöoikeustietue ja JWT Päivitä-tunnuksen. Lisäksi Azure AD palauttaa perustietoja käyttäjän, kuten näytön nimi ja vuokraajan tunnuksen perusteella.


5. Päälle HTTPS-asiakassovellus avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.


6. Kun access-tunnuksen vanhenee, asiakassovellus tulee virhe, joka ilmaisee käyttäjän täytyy suorittaa todennusta uudelleen. Jos sovellus on kelvollinen päivityksen tunnistetta, se voidaan hankkia uuden käyttöoikeustietue kysymättä lupaa käyttäjältä kirjautua sisään uudelleen. Jos päivitys-tunniste vanhentuu-sovellus on todentaessaan käyttäjän vuorovaikutteisesti uudelleen.


> [AZURE.NOTE] Päivitä tunnuksen Azure AD myöntämä voidaan käyttää useita resursseja. Esimerkiksi jos asiakassovellus käyttöoikeuskin sallii kaksi web ohjelmointirajapinnan kutsu, Päivitä-tunnuksen voidaan voit käyttää tunnuksen muiden verkko-Ohjelmointirajapinnan sekä.


#### <a name="code-samples"></a>MALLIKOODEJA


Katso MALLIKOODEJA alkuperäisen sovelluksen verkko-Ohjelmointirajapinnan skenaarioita. Ja tarkista usein--kaavaan lisätään uusi näytteiden aina. [Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Rekisteröiminen


- Yhtä vuokraajan: Sekä alkuperäisessä sovelluksen ja verkko-Ohjelmointirajapinnan on oltava rekisteröity samassa kansiossa Azure AD. Verkko-Ohjelmointirajapinnan voi määrittää voit näyttää joukon käyttöoikeudet, joita käytetään alkuperäisen sovelluksen rajoittaa sen resursseja. Valitse asiakassovellus valitsee haluamasi käyttöoikeudet "Oikeudet ja muut sovellukset" avattavasta valikosta Azure hallinta-portaalissa.


- Usean vuokraaja: Ensin alkuperäisen sovelluksen vain koskaan rekisteröity kehittäjä tai julkaisijan hakemistossa. Seuraavaksi alkuperäisen sovellus on määritetty osoittamassa, että se vaatii toimivan käyttöoikeudet. Tarvittavat käyttöoikeudet luettelo näkyy valintaikkuna, kun käyttäjä tai kohdekansion järjestelmänvalvoja antaa suostumus-sovellukseen, mikä helpottaa niiden organisaation käytettäväksi. Jotkin sovellukset edellyttävät vain käyttäjätason käyttöoikeudet, joten kaikki organisaation käyttäjät voivat suostuu. Muut sovellukset edellyttävät järjestelmänvalvojan oikeudet, jossa organisaation käyttäjä et voi suostuu. Vain kansion järjestelmänvalvoja antaa suostumus sovellukset edellyttävät tämä käyttöoikeustaso. Kun käyttäjän tai järjestelmänvalvojan suostuu, vain verkko-Ohjelmointirajapinnan on rekisteröity niiden hakemisto. Lisätietoja on artikkelissa [Azure Active Directory-integrointi sovelluksiin](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Tunnuksen vanhentumisen


Kun alkuperäisen sovellus käyttää luvan sen koodin hankkiminen JWT access tunnuksen, se saa JWT Päivitä-tunnuksen. Kun access-tunnuksen vanhenee, Päivitä-tunnuksen voidaan todennetaan uudelleen niin, ettei niitä kirjautua sisään uudelleen. Päivitä-tunnuksen käytetään todennetaan, joka johtaa uusi käyttöoikeustietue ja Päivitä tunnuksen.





### <a name="web-application-to-web-api"></a>Web-sovelluksen verkko-Ohjelmointirajapinnan


Tässä osassa kuvataan verkkosovelluksen resurssien käyttämistä verkko-Ohjelmointirajapinnan korjattava. Tässä skenaariossa kahdenlaisia tunnistetiedot, web-sovelluksen avulla voit todentaa ja soita verkko-Ohjelmointirajapinnan: sovelluksen jäsenyyden tai valtuutetun käyttäjätiedot.

*Sovelluksen käyttäjätiedot:* Tässä skenaariossa käyttää OAuth 2.0 asiakkaan tunnistetiedot myöntäminen sovelluksen todennetaan ja käyttää verkko-Ohjelmointirajapinnan. Sovelluksen käyttäjätiedot-Ohjelmointirajapinnan voi tunnistaa, että web-sovelluksen soittaa, web käytettäessä kuin verkossa API eivät saa käyttäjän tietoja. Jos sovellus vastaanottaa käyttäjän tietoja, se lähetetään sovelluksen-protokollan kautta ja sitä ei ole allekirjoittanut Azure AD. Verkko-Ohjelmointirajapinnan luottaa verkkosovelluksen todennetun käyttäjän. Tästä syystä tätä mallia kutsutaan luotettujen alirakenne.

*Valtuutetun käyttäjätiedot:* Tässä skenaariossa voit tehdä kahdella tavalla: OpenID muodosta ja OAuth 2.0 luvan koodin myöntäminen luottamuksellisia avulla. Web-sovelluksen hakee käyttäjälle, joka näyttää verkko-Ohjelmointirajapinnan käyttäjän todentaminen onnistui web-sovelluksen ja että verkkosovelluksen voitiin hankkia Soita verkko-Ohjelmointirajapinnan valtuutetun käyttäjätieto käyttöoikeustietue. Tämä käyttöoikeustietue lähetetään pyyntö verkko-Ohjelmointirajapinnan, joka sallii käyttäjän ja palauttaa halutun resurssi.

#### <a name="diagram"></a>Kaavio

![Web-sovelluksen verkko-Ohjelmointirajapinnan kaavioon](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Kuvaus protokolla-työnkulku

Sovelluksen käyttäjätiedot ja valtuutetun käyttäjän tunnistetiedot tyypit käsitellään alla työnkulku. Avaimen niiden välisen eron on, että valtuutetun käyttäjätiedot ensin voi hankkia todennus-koodin ennen käyttäjä voi Kirjaudu sisään ja käyttämään verkko-Ohjelmointirajapinnan.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Sovelluksen käyttäjätiedot OAuth 2.0-asiakasohjelman tunnistetiedot myöntäminen

1. Käyttäjä on kirjautunut Azure AD-web-sovelluksen (katso [Selaimen verkkosovellukseen](#web-browser-to-web-application) yllä).


2. Web-sovelluksen on hankittava käyttöoikeustietue, niin, että voi todentaa verkko-Ohjelmointirajapinnan ja hakea haluamasi resurssi. Se on pyyntö Azure AD suojaustunnuksen päätepisteen, tunnistetieto, Asiakastunnus ja API-verkkosovelluksen tunnus URI.


3. Azure AD todentaa sovellus, ja palauttaa JWT käyttöoikeustietue, jota käytetään verkko-Ohjelmointirajapinnan kutsu.


4. Päälle HTTPS-web-sovelluksen avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.

##### <a name="delegated-user-identity-with-openid-connect"></a>Valtuutetun käyttäjätietojen kanssa OpenID yhdistäminen

1. Käyttäjä on kirjautunut verkkosovelluksen Azure AD (katso yllä [Selaimen verkkosovellukseen](#web-browser-to-web-application) -osio). Web-sovelluksen käyttäjä on ei ole vielä hyväksynyt salliminen Soita verkko-Ohjelmointirajapinnan puolesta web-sovelluksen, jos käyttäjä on suostumus. Sovellus näkyy se vaatii käyttöoikeudet ja jos jokin näistä on järjestelmänvalvojan oikeudet, hakemistossa Normaali käyttäjä ei voi suostumus. Suostumus prosessia koskee vain usean vuokraajan sovellukset, ei ole yhtä vuokraajan-sovellukset, kun sovellus on jo tarvittavat käyttöoikeudet. Kun käyttäjä on kirjautunut sisään, web-sovelluksen saanut ID-tunnuksen, jonka tietoja käyttäjän sekä todennus-koodi.


2. Azure AD myöntämä todennus-koodia web-sovelluksen lähettää pyynnön Azure AD-tunnuksen päätepisteen, joka sisältää todennus-koodi, tiedot-asiakassovellukseen (Ostajantunnus ja uudelleenohjaus URI) ja haluamasi resurssi (verkko-Ohjelmointirajapinnan tunnus URI hakeminen).


3. Azure AD vahvistaa luvan koodi ja web-sovelluksen ja verkko-Ohjelmointirajapinnan tietoja. Onnistuneen vahvistuksen yhteydessä Azure AD palauttaa kahden tunnusten: JWT käyttöoikeustietue ja JWT Päivitä-tunnuksen.


4. Päälle HTTPS-web-sovelluksen avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Valtuutetun käyttäjätietojen kanssa OAuth 2.0 luvan koodin myöntäminen

1. Käyttäjä on jo kirjautunut sisään web-sovelluksen, jonka todennus-järjestelmä on erillinen Azure AD.


2. Web-sovelluksen edellyttää todennus-koodin hankkia käyttöoikeustietue, niin se huomioitavia selaimen kautta pyyntö Azure AD-todennus päätepisteen antamisen Asiakastunnus ja uudelleenohjata URI verkkosovelluksen onnistuneen todennuksen jälkeen. Käyttäjä kirjautuu Azure AD.


3. Web-sovelluksen käyttäjä on ei ole vielä hyväksynyt salliminen Soita verkko-Ohjelmointirajapinnan puolesta web-sovelluksen, jos käyttäjä on suostumus. Sovellus näkyy se vaatii käyttöoikeudet ja jos jokin näistä on järjestelmänvalvojan oikeudet, hakemistossa Normaali käyttäjä ei voi suostumus. Suostumus prosessia koskee vain usean vuokraajan sovellukset, ei ole yhtä vuokraajan-sovellukset, kun sovellus on jo tarvittavat käyttöoikeudet.


4. Kun käyttäjä on hyväksynyt, web-sovelluksen vastaanottaa luvan tunnus, jolla hankkia access-tunnusta tarvitaan.


5. Azure AD myöntämä todennus-koodia web-sovelluksen lähettää pyynnön Azure AD-tunnuksen päätepisteen, joka sisältää todennus-koodi, tiedot-asiakassovellukseen (Ostajantunnus ja uudelleenohjaus URI) ja haluamasi resurssi (verkko-Ohjelmointirajapinnan tunnus URI hakeminen).


6. Azure AD vahvistaa luvan koodi ja web-sovelluksen ja verkko-Ohjelmointirajapinnan tietoja. Onnistuneen vahvistuksen yhteydessä Azure AD palauttaa kahden tunnusten: JWT käyttöoikeustietue ja JWT Päivitä-tunnuksen.


7. Päälle HTTPS-web-sovelluksen avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.

#### <a name="code-samples"></a>MALLIKOODEJA

Katso MALLIKOODEJA Web-sovelluksen verkko-Ohjelmointirajapinnan skenaarioita. Ja tarkista usein--kaavaan lisätään uusi näytteiden aina. Web- [sovelluksen verkko-Ohjelmointirajapinnan](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Rekisteröiminen

- Yhtä vuokraajan: Sekä sovelluksen käyttäjätiedot ja valtuutetun käyttäjän tunnistetiedot tapauksissa web-sovelluksen ja verkko-Ohjelmointirajapinnan on rekisteröitävä samassa kansiossa Azure AD. Voit näyttää joukon käyttöoikeudet, joita käytetään sen resurssien web-sovelluksen käytön rajoittaminen voi määrittää verkko-Ohjelmointirajapinnan. Valtuutetun käyttäjätyyppi tunnistetietojen käytettäessä web-sovelluksen on valita haluamasi "Oikeudet ja muut sovellukset" avattavasta valikosta Azure hallinta-portaalissa. Tämä vaihe ei tarvita, jos sovellus identity-tyyppi on käytössä.


- Usean vuokraaja: Ensin verkkosovellus on määritetty osoittamassa, että se vaatii toimivan käyttöoikeudet. Tarvittavat käyttöoikeudet luettelo näkyy valintaikkuna, kun käyttäjä tai kohdekansion järjestelmänvalvoja antaa suostumus-sovellukseen, mikä helpottaa niiden organisaation käytettäväksi. Jotkin sovellukset edellyttävät vain käyttäjätason käyttöoikeudet, joten kaikki organisaation käyttäjät voivat suostuu. Muut sovellukset edellyttävät järjestelmänvalvojan oikeudet, jossa organisaation käyttäjä et voi suostuu. Vain kansion järjestelmänvalvoja antaa suostumus sovellukset edellyttävät tämä käyttöoikeustaso. Kun käyttäjän tai järjestelmänvalvojan suostuu, web-sovelluksen ja verkko-Ohjelmointirajapinnan molemmat rekisteröidään niiden hakemistossa.

#### <a name="token-expiration"></a>Tunnuksen vanhentumisen

Kun web-sovelluksen käyttää luvan sen koodin hankkiminen JWT access tunnuksen, se saa JWT Päivitä-tunnuksen. Kun access-tunnuksen vanhenee, Päivitä-tunnuksen voidaan todennetaan uudelleen niin, ettei niitä kirjautua sisään uudelleen. Päivitä-tunnuksen käytetään todennetaan, joka johtaa uusi käyttöoikeustietue ja Päivitä tunnuksen.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon tai verkko-Ohjelmointirajapinnan palvelimen sovelluksesta


Tässä osassa kuvataan daemon tai palvelimen sovellus, joka hakee resurssien verkko-Ohjelmointirajapinnan on. Kaksi aliraportti mahdollista, jotka koskevat tämän osan: soita verkko-Ohjelmointirajapinnan, OAuth 2.0 tunnistetietojen myöntäminen Asiakastyyppi; rakennettu korjattava daemon ja palvelin-sovelluksen (kuten sivuston API) on Soita verkko-Ohjelmointirajapinnan, rakennettu OAuth 2.0 On-Behalf-Of luonnos määritys.

Skenaarion kun daemon sovellus täytyy kutsua verkko-Ohjelmointirajapinnan, on tärkeää ymmärtää muutama seikka. Ensin käyttäjän toimia ei ole mahdollista demonisovellukset, joka edellyttää sovelluksen on oma käyttäjätietojen kanssa. Esimerkki daemon-sovellus on erän työn tai käyttöjärjestelmä palvelun käynnissä taustalla. Tällaista sovellus pyytää access-tunnuksen käyttämällä sen sovelluksen käyttäjätiedot ja esittäminen sen asiakkaan tunnus, tunnistetiedon (salasanan tai varmenteen) ja sovelluksen Azure AD URI ID-tunnusta. Onnistuneen todennuksen jälkeen daemon saa Azure AD, jota käytetään sitten Soita verkko-Ohjelmointirajapinnan access-tunnuksen.

Skenaarion kun server-sovellusta tarvitsee Soita verkko-Ohjelmointirajapinnan, kannattaa käyttää esimerkiksi. Kuvitellaan, että käyttäjä on todennus-alkuperäisen sovelluksen ja alkuperäisen sovelluksen on verkko-Ohjelmointirajapinnan kutsu. Azure AD-ongelmat JWT käyttöoikeustietue, Soita verkko-Ohjelmointirajapinnan. Jos haluat soittaa toisen edeltävät verkko-Ohjelmointirajapinnan verkko-Ohjelmointirajapinnan, se edustajien käyttäjän käyttäjätiedot ja todentaminen toisen tason verkko-Ohjelmointirajapinnan käyttöön-puolesta-ja työnkulun avulla.

#### <a name="diagram"></a>Kaavio

![Daemon tai palvelimen sovelluksen verkko-Ohjelmointirajapinnan kaavioon](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Kuvaus protokolla-työnkulku

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Sovelluksen käyttäjätiedot OAuth 2.0-asiakasohjelman tunnistetiedot myöntäminen

1. Sovelluksen on ensin todentamismenetelmä Azure AD kuin itse ilman ihmisten toimia esimerkiksi vuorovaikutteinen Sign-valintaikkuna. Se on pyyntö Azure AD suojaustunnuksen päätepisteen, tunnistetiedon, Asiakastunnus ja sovelluksen tunnus URI.


2. Azure AD todentaa sovellus, ja palauttaa JWT käyttöoikeustietue, jota käytetään verkko-Ohjelmointirajapinnan kutsu.


3. Päälle HTTPS-web-sovelluksen avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Valtuutetun käyttäjätiedot OAuth 2.0--puolesta-, luonnos määritystä

Kulun käsitellyt alla oletetaan, että käyttäjä on tarkistettu toiseen sovellukseen (esimerkiksi alkuperäisen sovelluksen)- ja hankkimaan access-tunnuksen, ensimmäisen tason verkko-Ohjelmointirajapinnan on käytetty niiden käyttäjätiedot.

1. Alkuperäisen sovelluksen lähettää käyttöoikeustietue ensimmäisen tason verkko-Ohjelmointirajapinnan.


2. Ensimmäisen tason verkko-Ohjelmointirajapinnan lähettää pyynnön Azure AD suojaustunnuksen päätepisteen tarjoaa sen asiakkaan tunnus ja tunnistetiedot sekä käyttäjän käyttöoikeustietue. Lisäksi pyyntö lähetetään kanssa parametri, joka ilmaisee verkossa on_behalf_of API pyytää uuden tunnusten Soita edeltävät web API alkuperäisen käyttäjän puolesta.


3. Azure AD tarkistaa, että ensimmäisen tason verkko-Ohjelmointirajapinnan on toisen tason verkko-Ohjelmointirajapinnan käyttöoikeudet ja vahvistaa JWT käyttöoikeustietue, joka palauttaa pyynnön ja JWT Päivitä tunnuksen ensimmäisen tason verkko-Ohjelmointirajapinnan.


4. HTTPS-kautta ensimmäisen tason verkko-Ohjelmointirajapinnan kutsuu sitten toisen tason verkko-Ohjelmointirajapinnan liittämällä Authorization-otsikko-pyynnössä suojaustunnuksen merkkijonon. Ensimmäisen tason verkko-Ohjelmointirajapinnan jatkaa Soita toisen tason verkko-Ohjelmointirajapinnan, kunhan käyttöoikeustietue ja Päivitä tunnusten ovat kelvollisia.

#### <a name="code-samples"></a>MALLIKOODEJA

Katso MALLIKOODEJA Daemon tai palvelimen sovelluksesta verkko-Ohjelmointirajapinnan skenaarioita. Ja tarkista usein--kaavaan lisätään uusi näytteiden aina. [Palvelimen tai verkko-Ohjelmointirajapinnan Daemon sovelluksesta](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Rekisteröiminen

- Yhtä vuokraajan: Sekä sovelluksen käyttäjätiedot ja valtuutetun käyttäjän tunnistetiedot tapauksissa daemon tai palvelin-sovellus on rekisteröitävä samassa kansiossa Azure AD. Voit näyttää joukon käyttöoikeudet, joita käytetään raja daemon tai sen resurssien käyttöä palvelimen voi määrittää verkko-Ohjelmointirajapinnan. Valtuutetun käyttäjätyyppi tunnistetietojen käytettäessä sovelluksen on valita haluamasi "Oikeudet ja muut sovellukset" avattavasta valikosta Azure hallinta-portaalissa. Tämä vaihe ei tarvita, jos sovellus identity-tyyppi on käytössä.


- Usean vuokraaja: Ensin daemon tai palvelimen sovellus on määritetty osoittamassa, että se vaatii toimivan käyttöoikeudet. Tarvittavat käyttöoikeudet luettelo näkyy valintaikkuna, kun käyttäjä tai kohdekansion järjestelmänvalvoja antaa suostumus-sovellukseen, mikä helpottaa niiden organisaation käytettäväksi. Jotkin sovellukset edellyttävät vain käyttäjätason käyttöoikeudet, joten kaikki organisaation käyttäjät voivat suostuu. Muut sovellukset edellyttävät järjestelmänvalvojan oikeudet, jossa organisaation käyttäjä et voi suostuu. Vain kansion järjestelmänvalvoja antaa suostumus sovellukset edellyttävät tämä käyttöoikeustaso. Kun käyttäjän tai järjestelmänvalvojan suostuu, sekä verkko-ohjelmointirajapinnan rekisteröity niiden hakemistossa.

#### <a name="token-expiration"></a>Tunnuksen vanhentumisen

Kun ensimmäisen sovelluksen käyttää luvan sen koodin hankkiminen JWT access tunnuksen, se saa JWT Päivitä-tunnuksen. Kun access-tunnuksen vanhenee, Päivitä-tunnuksen voidaan todennetaan uudelleen kysymättä tunnistetiedot. Päivitä-tunnuksen käytetään todennetaan, joka johtaa uusi käyttöoikeustietue ja Päivitä tunnuksen.

## <a name="see-also"></a>Katso myös

[Azure Active Directory-Sovelluskehittäjän opas](active-directory-developers-guide.md)

[Azure Active Directory-MALLIKOODEJA](active-directory-code-samples.md)

[Tärkeitä tietoja kirjautumisessa Azure AD avaimen palauttaminen](active-directory-signing-key-rollover.md)

[Azure AD-OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)
