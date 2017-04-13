<properties
    pageTitle="Azure Active Directory-B2C: Rajoitukset ja rajoituksia | Microsoft Azure"
    description="Luettelo rajoitukset ja Azure Active Directory-B2C rajoituksia"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory-B2C: Rajoitukset ja rajoitukset

On useita ominaisuuksia ja Azure Active Directory (Azure AD) B2C toimintoja, joita ei vielä tueta. Monia näistä tunnettuja ongelmia ja rajoituksia osoitetaan Exceliä, mutta sinun olisi otettava huomioon ne, jos kokoat kuluttaja osoittava sovellusten Azure AD B2C.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Ongelmat Azure AD B2C alihallinnat luonnin aikana

Jos [Azure AD B2C vuokraajan luonnin](active-directory-b2c-get-started.md)aikana ilmenee ongelmia, katso ohjeet [luominen Azure AD-vuokraajan tai Azure AD B2C-vuokraajan--ongelmat ja ratkaisut](active-directory-b2c-support-create-directory.md) .

Huomaa, että aiheuttavat tunnetusti ongelmia, kun poistat aiemmin B2C-vuokraajan ja luo se uudelleen sama toimialuenimi. Sinun on luotava B2C palvelutili eri toimialuenimeä.

## <a name="note-about-b2c-tenant-quotas"></a>B2C vuokraajan kiintiön koskeva huomautus

Oletusarvon mukaan B2C-alihallinnan käyttäjien määrä on rajoitettu 50 000 käyttäjille. Jos haluat korottaa B2C vuokraajan kiintiön, ota yhteyttä tuki.

## <a name="branding-issues-on-verification-email"></a>Ulkoasuongelmat, valitse vahvistussähköposti

Oletusarvoinen vahvistussähköposti sisältää Microsoft mukauttaminen. Olemme poistaa sen myöhemmin. Nyt voit poistaa sen [yrityksen yrityksen tunnuksen-toiminnon](../active-directory/active-directory-add-company-branding.md)avulla.

## <a name="restrictions-on-applications"></a>Sovellusten rajoitukset

Sovellusten seuraavanlaisia ei tällä hetkellä tueta Azure AD B2C. Tuetut tiedostotyypit sovellusten kuvauksen, viitata [Azure AD B2C: tyyppisiä sovelluksia](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Sovellusten yksisivuiselle (JavaScript)

Monet Moderni sovellukset yhden sivun sovelluksen (SPA) edusta, joka on kirjoitettu ensisijaisesti JavaScript on, ja käyttää usein SPA-framework esimerkiksi AngularJS Ember.js, Durandal. Tämä työnkulku ei ole vielä käytettävissä Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Daemonit / palvelinpuolen sovellukset

Sovellukset, jotka sisältävät pitkään käynnissä olevien prosessien tai, jotka toimivat ilman käyttäjän tavoitettavuustiedot on myös voi käyttää suojattuun resurssit, kuten verkko-ohjelmointirajapinnan. Nämä sovellukset voi todentaa ja hakea tunnuksia käyttämällä sovelluksen tunnistetietojen (mieluummin kuin kuluttajille valtuutetun tunnistetietojen) [OAuth 2.0 asiakkaan tunnistetiedot työnkulku](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Tämä työnkulku ei ole vielä käytettävissä Azure AD B2C, jotta nyt sovellusten pääsevät käyttämään tunnusten vasta, kun vuorovaikutteinen kuluttaja-kirjautuminen työnkulku on tapahtunut.

### <a name="standalone-web-apis"></a>Erillinen verkko-ohjelmointirajapinnan

Azure AD-B2C sinulla voi [muodostaa verkko-Ohjelmointirajapinnan, joka on suojattu OAuth 2.0 tunnusten avulla](active-directory-b2c-apps.md#web-apis). Kuitenkin, että verkko-Ohjelmointirajapinnan vain voi vastaanottaa tunnusten on jokin muu sähköpostiohjelma, joka jakaa saman sovelluksen tunnus Verkko-Ohjelmointirajapinnan, jota käytetään useita eri asiakkaiden rakentaminen ei tueta.

### <a name="web-api-chains-on-behalf-of"></a>Verkko-Ohjelmointirajapinnan ketjut (Valitse-puolesta-)

Monta arkkitehtuureihin Lisää verkko-Ohjelmointirajapinnan on Soita toiseen edeltävät Web API, molemmat Azure AD B2C suojattu. Tässä skenaariossa yhteistä alkuperäisen asiakkaat, joilla verkko-Ohjelmointirajapinnan taustatietokantaan, joka puolestaan kutsuu Microsoft-verkkopalveluun, kuten Azure AD-kaavio-Ohjelmointirajapinnan.

Ketjutetut verkko-Ohjelmointirajapinnan artikkelista ehkä tueta OAuth 2.0 Jwt haltijan tunnistetiedon myöntäminen, eli käytössä-puolesta-,-työnkulku käyttämällä. Kuitenkin käytössä-puolesta-ja työnkulku ei ole tällä hetkellä käytössä Azure AD-B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Rajoituksen kirjastoihin ja SDK: T

Tuettu Microsoft-kirjastoja, jotka toimivat Azure AD B2C joukko on hyvin rajoitettu tällä hetkellä. .NET-pohjaisen web Apps-sovellusten ja palveluiden sekä NodeJS web-sovellusten ja palveluiden tuki on.  Myös on nimeltään MSAL, joka voidaan käyttää Azure AD-B2C Windows & .NET muista sovelluksista esikatselu .NET asiakkaan kirjastoon.

Microsoft ei ole kirjaston kieliä tai ympäristöjen, kuten iOS- ja Android.  Jos haluat luoda eri kuin edellä mainituista ympäristössä, on suositeltavaa käyttää Avaa lähde SDK, joka viittaa sekä [OAuth 2.0 ja OpenID yhteyden protokolla viittaus](active-directory-b2c-reference-protocols.md) tarpeen mukaan.  Azure AD-B2C toteuttaa OAuth & OpenID yhteyden, joka mahdollistaa sellaisten käyttävät yleinen OAuth tai OpenID muodosta kirjastoja integrointia varten.

Tutustu iOS- ja Android pikaopas opetusohjelmat käyttää Avaa lähde-kirjastoja, joissa on testattu Azure AD B2C yhteensopivuuden.  Kaikki Microsoftin quick start-opetusohjelmat ovat käytettävissä Microsoftin [Aloitus](active-directory-b2c-overview.md#getting-started) -osassa.

## <a name="restriction-on-protocols"></a>Rajoituksen protokollat

Azure AD-B2C tukee OpenID yhteyden ja OAuth 2.0. Mutta ei kaikkia ominaisuuksia ja kunkin protokollan ominaisuudet on toteutettu. Tuetut protokolla-toiminnot Azure AD B2C kuvattuihin sekä [OpenID muodosta ja OAuth 2.0-protokollan viittaus](active-directory-b2c-reference-protocols.md)laajuuden ymmärtäminen entistä paremmin. SAML ja WS Fed protokollatuki ei ole käytettävissä.

## <a name="restriction-on-tokens"></a>Rajoituksen tunnusten

Monia Azure AD B2C myöntämä tunnusten toteutettu JSON Web tunnusten tai JWTs. Kaikki JWTs (eli "saatavat") sisältämät tiedot on kuitenkin aivan kuin se on oltava tai se puuttuu. Esimerkkejä "sub" ja "preferred_username" saatavat.  Arvot, muoto tai saatavat muutos ajan mittaan merkitys tunnusten aiemmin käytäntöjen pysyy ei vaikuta – haluat niiden arvojen tuotannon-sovelluksissa.  Kun arvot muuttuvat, emme avulla voit mahdollisuus määrittää muutokset kunkin käyttäjän käytännöt.  Lisätietoja Azure AD B2C-palvelun tällä hetkellä lähettämän tunnuksia, lukea oma [Suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Rajoituksen sisäkkäiset ryhmät

Azure AD B2C alihallinnat, jotka eivät tue sisäkkäisen ryhmäjäsenyyksiä. Olemme enkä aio käyttää Lisää tätä ominaisuutta.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Rajoituksen käyttöön Azure AD Graph API eroavuus kysely-ominaisuus

[Azure AD Graph API eroavuus kysely-toiminto](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) ei tue Azure AD B2C alihallinnat. Tämä on Microsoftin pitkään ohjeet.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Käyttäjien hallinta Azure perinteinen portaalissa liittyvät ongelmat

B2C ominaisuudet ovat käytettävissä Azure-portaalissa. Voit kuitenkin käyttää muita vuokraaja ominaisuuksista, kuten Käyttäjähallinta Azure perinteinen portaalin. Tällä hetkellä on muutama Käyttäjähallinta ( **käyttäjät** -välilehti) Azure perinteinen portaalissa tunnetuista ongelmista:

- Paikallisen tilin käyttäjän (kuten, kuka sähköpostiosoitteen ja salasanan, tai käyttäjänimellä ja salasanalla Rekisteröi kuluttaja) **Käyttäjätunnus** -kentässä ei vastaa-kirjautuminen tunnus (sähköpostiosoite tai käyttäjänimi), jota käytettiin rekisteröitymisen yhteydessä. Tämä johtuu siitä Azure perinteinen portaalissa näkyvän kentän on todella käyttäjän pääasiallista nimi (UPN), jossa ei käytetä B2C tilanteissa. Kirjaudu sisään tunnus paikallisen tilin katselemista Etsi käyttäjäobjekti [Graph](https://graphexplorer.cloudapp.net/)Resurssienhallinnassa. Saat näkyviin sama ongelma sosiaalisen tilin käyttäjälle (eli kuluttaja kuka Rekisteröi Facebook, Google +, jne.), mutta siinä tapauksessa on puhua ja ei-kirjautuminen-tunniste.

    ![Paikallisen tilin - UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Paikallisen tilin käyttäjän se ei voi muokata kenttiä ja Tallenna muutokset **profiili** -välilehdessä.

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Järjestelmänvalvojan käynnistämä salasanan Azure perinteinen portaalissa liittyvät ongelmat

Jos salasanan, paikallinen asiakaspohjaiset kuluttajien Azure perinteinen-portaalissa ( **käyttäjät** -välilehden **Vaihda salasana** -komento), että kuluttajien eivät pysty vaihtamaan salasanan seuraava kirjautumissivulla, jos käytät merkki tai Kirjaudu sisään-käytäntö ja on lukittuna pois sovellukset. Voit kiertää tämän ongelman Käytä [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) kuluttaja salasanan (ilman salasanan vanhentumisesta) tai käyttää merkin merkki sijaan käytännöstä tai käytännön kirjautuminen.

## <a name="issues-with-creating-a-custom-attribute"></a>Mukautetun määritteen luominen liittyvät ongelmat

[Mukautetun määritteen lisätty Azure-portaalissa](active-directory-b2c-reference-custom-attr.md) ei ole heti luotu B2C vuokraajan. Sinun on B2C vuokraajan luodaan ja muuttuvat Graph Ohjelmointirajapinnan kautta käyttämällä mukautetun määritteen vähintään yhdessä suojauskäytäntöjen määrittäminen sitä.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Azure-perinteinen portaalissa toimialueen vahvistamisongelmien

Tällä hetkellä ei voi vahvistaa toimialueen onnistuneesti [Azure perinteinen portal](https://manage.windowsazure.com/).

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Ongelmia sisäänkirjautumisessa ongelmia MFA käytännön Safari-selaimissa

Kirjaudu sisään-käytäntöjä (MFA käytössä) epäonnistuvat ajoittain Safari-selaimissa HTTP 400 (virheelliset pyynnön) virheitä pyynnöt. Tämä on Safari on pieni eväste kokorajoitukset määräpäivä. Muutamalla ratkaisuehdotuksia ongelman:

- Käytä "kirjautuminen tai Kirjaudu sisään käytäntöä" "-kirjautuminen käytännön" sijaan.
- Vähennä **sovelluksen saatavat** sinulta pyydetään käytännön.
