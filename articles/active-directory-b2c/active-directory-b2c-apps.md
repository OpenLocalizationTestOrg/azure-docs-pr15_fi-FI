<properties
    pageTitle="Azure AD-B2C | Microsoft Azure"
    description="Voit luoda Azure Active Directory-B2C sovellusten tyypit."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory-B2C: Tyyppisiä sovelluksia

Azure Active Directory (Azure AD) B2C tukee todennusta nykyaikaisen app arkkitehtuureihin erilaisille. Kaikki tavat perustuvat alan vakio protokollat [OAuth 2.0](active-directory-b2c-reference-protocols.md) tai [OpenID muodosta](active-directory-b2c-reference-protocols.md). Tässä asiakirjassa kuvataan lyhyesti tyyppisiä sovelluksia, jotka voit luoda, riippumatta kielen tai ympäristö laite. Se myös auttaa sinua ymmärtämään, ennen kuin voit [aloittaa sovellusten](active-directory-b2c-overview.md#getting-started)korkean tason skenaarioita.

## <a name="the-basics"></a>Perusteet
Jokaisen sovellusta, joka käyttää Azure AD B2C on rekisteröitävä [B2C directory](active-directory-b2c-get-started.md) [Azure-portaalin](https://portal.azure.com/)kautta. Sovelluksen rekisteröitymisen kerää ja määrittää muutaman arvon sovelluksen:

- **Tunnus** , joka yksilöi sovelluksen.
- **Uudelleenohjata URI** , jonka avulla voidaan ohjata takaisin sovelluksen vastaukset.
- Muita skenaarion kielikohtaiset-arvoja. Lisätietoja, lue, miten voit [rekisteröidä sovelluksen](active-directory-b2c-app-registration.md).

Kun sovellus rekisteröidään, se kommunikoi Azure AD kanssa lähettämällä pyynnöt Azure AD-v2.0 päätepisteen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Kunkin pyynnön, joka lähetetään Azure AD B2C määrittää **käytännön**. Käytännön ohjaa Azure AD toimintaa. Voit myös luoda mukauttaa käyttäjän kokemukset nämä päätepisteet. Yleiset käytännöt ovat kirjautuminen, kirjaudu sisään ja profiili-Muokkaa käytännöt. Jos et ole tottunut käytäntöjä, lue lisätietoja Azure AD B2C [extensible toimintaperiaatteet](active-directory-b2c-reference-policies.md) ennen kuin jatkat.

Jokaisen sovelluksen käsittelemisen v2.0 päätepisteen tapahtuu samalla korkean tason kuvio:

1. Sovelluksen ohjaa v2.0 päätepisteelle käyttäjälle oikeuden suorittaa [käytännön](active-directory-b2c-reference-policies.md).
2. Käyttäjän suorittaa käytännön käytännön määrityksen mukaan.
4. Sovelluksen saa jokin lisätoimi suojaustunnus v2.0 päätepiste.
5. Sovellus käyttää suojattuja tietoja tai suojatun resurssin suojauksen salasana.
6. Resurssin palvelin vahvistaa suojaustunnus voit varmistaa, että access myönnetty.
7. Sovellus päivitetään säännöllisesti suojauksen salasana.

<!-- TODO: Need a page for libraries to link to -->
Näin voi erota hieman perusteella luot sovelluksen tyyppi. Avaa lähde kirjastojen kohteeksi tiedot puolestasi.

## <a name="web-apps"></a>Web Apps-sovelluksista
Verkkosovelluksissa (mukaan lukien .NET, PHP, Java, Ruby, Python ja Node.js), jotka ovat palvelimessa ja käyttää selaimen kautta Azure AD B2C tukee [OpenID yhteyden](active-directory-b2c-reference-protocols.md) kaikki käyttäjän kokemukset. Tämä sisältää kirjautumisnimi, kirjautuminen, ja Profiilin hallinta. OpenID yhteyden Azure AD B2C käyttöönoton-web-sovelluksen käynnistää käyttäjä on nyt lähettämällä käyttöoikeuksien Azure AD. Pyynnön tulos on `id_token`. Tämä suojaustunnus edustaa käyttäjän tunnistetiedot. Se on myös tietoja käyttäjän lomakkeessa vaateita koskevat rajoitukset:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Lisätietoja tunnusten ja saatavat käytettävissä [B2C tunnuksen viittaus](active-directory-b2c-reference-tokens.md)-sovelluksen avulla.

Online-sovelluksessa kunkin [käytännön](active-directory-b2c-reference-policies.md) suorittaminen kestää korkean tason seuraavasti:

![Web App kaistat kuva](./media/active-directory-b2c-apps/webapp.png)

Vahvistaminen `id_token` käyttämällä julkista allekirjoitetun avainta, joka on saatu Azure AD riittää käyttäjälle jäsenyyden vahvistamiseksi. Tämä asetus määrittää myös istunnon eväste, joilla voidaan tunnistaa käyttäjän sivulle pyynnöt.

Tässä skenaariossa käytössä näkyviin Kokeile web app-kirjautuminen koodin mallit Microsoftin [Aloitus-osassa](active-directory-b2c-overview.md#getting-started).

Lisäksi helpottaminen yksinkertainen kirjautuminen, palvelimen verkkosovellukseen joutua myös käyttää taustatietokantaa verkkopalvelun. Tässä tapauksessa web Appissa voit suorittaa hieman eri [OpenID Yhdistä tiedonkulun](active-directory-b2c-reference-oidc.md) ja hankkia tunnusten luvan merkkikoodeja käyttämällä ja Päivitä tunnukset. Tässä skenaariossa on edellisessä [Verkko-ohjelmointirajapinnan osaan](#web-apis)seuraavasti.

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Verkko-ohjelmointirajapinnan
Voit käyttää Azure AD B2C suojaamiseen sinua sovelluksen RESTful verkko-Ohjelmointirajapinnan WWW-palveluihin. Verkko-ohjelmointirajapinnan suojatun niiden tiedot käyttämällä tunnusten HTTP pyynnöt lisäpalveluita OAuth 2.0 avulla. Verkko-Ohjelmointirajapinnan soittajan liittää tunnuksen luvan HTTP-pyynnön otsikossa:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Verkko-Ohjelmointirajapinnan voi käyttää tunnuksen API soittajan jäsenyyden vahvistamiseksi ja soittajan tiedot noutamiseen saatavat, jotka on koodattu tunnuksen. Lisätietoja tunnusten ja saatavat käytettävissä [Azure AD B2C tunnuksen viittaus](active-directory-b2c-reference-tokens.md)-sovelluksen avulla.

> [AZURE.NOTE]
    Azure AD-B2C tukee tällä hetkellä vain verkko-ohjelmointirajapinnan, joita voi käyttää omia tunnetun asiakkaat. Valmis sovelluksen voi olla esimerkiksi iOS-sovelluksen Android-sovelluksen ja taustatietokantaan verkko-Ohjelmointirajapinnan. Tämä arkkitehtuuri tuetaan täysin. Salliminen kumppanin asiakasohjelman toisessa iOS-sovelluksessa, kuten käyttämään API ei tällä hetkellä tueta samassa verkossa. Kaikkien osien koko sovelluksesi jakavat on yhden sovelluksen.

Verkko-Ohjelmointirajapinnan saavat tunnusten asiakasohjelmilla, kuten web Apps-sovelluksista, työpöydän ja mobile-sovellukset, yksittäiseltä sivulta sovellukset, palvelinpuolen daemonit ja muiden verkko-ohjelmointirajapinnan useita erityyppisiä. Tässä on esimerkki valmiiksi työnkulun, joka kutsuu verkko-Ohjelmointirajapinnan web-sovelluksen:

![Web App-verkko-Ohjelmointirajapinnan kaistat kuva](./media/active-directory-b2c-apps/webapi.png)

Lue lisätietoja luvan koodit, Päivitä tunnusten ja tunnusten vaiheet, [OAuth 2.0-protokollaa](active-directory-b2c-reference-oauth-code.md).

Verkko-Ohjelmointirajapinnan suojaamiseen Azure AD B2C avulla opit, katso web API opetusohjelmiin Microsoftin [Aloitus-osassa](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Matkapuhelin ja alkuperäisen sovellukset
Sovellukset, jotka on asennettu laitteita, kuten sähköistä tai työpöydän sovellukset on usein taustatietokantaan services tai web ohjelmointirajapinnan käyttäjien puolesta. Voit lisätä mukautetun käyttäjätietojen hallinta kokemukset alkuperäisen sovellukset ja suojatusti puhelun taustatietokantaan services Azure AD B2C ja [OAuth 2.0 luvan koodin työnkulun](active-directory-b2c-reference-oauth-code.md)avulla.  

Tässä työnkulussa sovellus suorittaa [käytännöt](active-directory-b2c-reference-policies.md) ja vastaanottaa `authorization_code` from Azure AD jälkeen käytännön. `authorization_code` Edustaa sovelluksen käyttöoikeuksia Soita taustatietokantaan services käyttäjälle, joka on tällä hetkellä kirjautunut puolesta. Sovelluksen sitten vaihtoon `authorization_code` taustalla `id_token` ja `refresh_token`.  Sovelluksen voit käyttää `id_token` -pyyntöjen taustatietokantaan sivustoon API tarkistamiseen. Voit myös käyttää `refresh_token` voit hankkia uuden `id_token` kun vanhoja yksi vanhenee.

> [AZURE.NOTE]
    Azure AD-B2C tukee tällä hetkellä vain tunnuksia, joita käytetään access sovelluksen oma taustatietokantaan verkkopalvelun. Valmis sovelluksen voi olla esimerkiksi iOS-sovelluksen Android-sovelluksen ja taustatietokantaan verkko-Ohjelmointirajapinnan. Tämä arkkitehtuuri tuetaan täysin. IOS-sovelluksen pääsyn kumppanin verkko-Ohjelmointirajapinnan käyttäen OAuth 2.0 access tunnusten salliminen ei ole tuettu tällä hetkellä. Kaikkien osien koko sovelluksesi jakavat on yhden sovelluksen.

![Alkuperäisen sovelluksen kaistat kuva](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Nykyinen rajoitukset
Azure AD-B2C ei tue tällä hetkellä seuraavanlaisten sovellukset, mutta ne ovat roadmapy. Muut rajoitukset ja Azure AD B2C rajoituksia on artikkelissa [rajoitukset ja rajoitukset](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Yksittäisen sivun apps (JavaScript)
Monta Moderni sovellukset on yhden sovelluksen edusta, kirjoitettu ensisijaisesti JavaScript. Niitä käytetään usein framework, kuten AngularJS, Ember.js tai Durandal. Yleisesti saatavilla Azure AD-palvelu tukee nämä sovellukset implisiittinen OAuth 2.0-työnkulku käyttämällä. Kuitenkin tämä työnkulku ei ole vielä käytettävissä Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Daemonit-ja palvelinpuolen sovellukset
Sovellukset, jotka sisältävät pitkään käynnissä olevien prosessien tai, jotka toimivat ilman käyttäjän tavoitettavuustiedot on myös voi käyttää suojattuun resurssit, kuten verkko-ohjelmointirajapinnan. Nämä sovellukset voi todentaa ja poistaa tunnusten käyttää sovelluksen tunnistetietojen (käyttäjän valtuutetun tunnistetietojen sijaan) ja käyttämällä OAuth 2.0-asiakas tunnistetiedot työnkulku.

Tämä työnkulku ei tällä hetkellä tue Azure AD B2C. Nämä sovellukset pääsevät tunnuksia vain, kun vuorovaikutteinen käyttäjä-työnkulku on tapahtunut.

### <a name="web-api-chains-on-behalf-of-flow"></a>Verkko-Ohjelmointirajapinnan ketjuttaa (Valitse-puolesta-ja työnkulku)
Monta arkkitehtuureihin Lisää verkko-Ohjelmointirajapinnan on Soita toiseen edeltävät verkko-Ohjelmointirajapinnan, jossa molemmat suojattu Azure AD B2C. Tässä skenaariossa yhteistä alkuperäisen asiakkaat, joilla verkko-Ohjelmointirajapinnan taustatietokannan. Tämä sitten tallenteita Microsoft-verkkopalveluun, kuten Azure AD-kaavio-Ohjelmointirajapinnan.

Ketjutetut verkko-Ohjelmointirajapinnan tässä skenaariossa on tuettu OAuth 2.0 JWT haltijan tunnistetiedon myöntäminen, eli--puolesta-ja työnkulku.  Kuitenkin--puolesta-ja työnkulku ei ole tällä hetkellä käytössä Azure AD-B2C.
