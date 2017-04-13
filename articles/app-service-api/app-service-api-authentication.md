<properties
    pageTitle="Todennus- ja Azure App Service API-sovellusten | Microsoft Azure"
    description="Tietoja, joka sisältää Azure App Service API-sovellusten todennus- ja -palvelut."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Todennus- ja Azure App Service API varten

## <a name="overview"></a>Yleiskatsaus 

> [AZURE.NOTE] Tässä ohjeaiheessa siirretään, kootut [sovelluksen todentaminen / luvan](../app-service/app-service-authentication-overview.md) ohjeaiheen, joka kattaa Web, Mobile ja API sovellusten.

Azure sovelluksen-palvelussa valmiit todennus- ja -palvelut, jotka toteuttavat [OAuth 2.0](#oauth) ja [OpenID yhteyden](#oauth). Tässä artikkelissa kuvataan palvelut ja Azure App Service API varten käytettävissä olevista vaihtoehdoista.

Seuraavassa kaaviossa on kuvattu joitakin sovelluksen palvelun todennusta tärkeimmät ominaisuudet:

* Se esikäsittelee API pyynnöt on mikä tarkoittaa, että se toimii minkä tahansa kielen ja sovelluksen palvelun tukemat framework.
* Tällä tavalla saat erilaisia vaihtoehtoja paljonko todennus toimisi haluat tehdä oman koodin.
* Se toimii sekä peruskäyttäjän ja palvelun tili todennusta varten. 
* Se tukee viisi tunnistetietojen toimittajat: Azure Active Directory, Facebook, Google, Twitter ja Microsoft Account.
* Se toimii samalla API-sovellukset, Web Apps-sovellusten ja Mobile-sovellukset.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Kielen ympäristöstä riippumattomalla tavalla

Sovelluksen palvelun todennus käsittely tapahtuu ennen pyynnöt saavuttaa API-sovellus, joka tarkoittaa, että authentication-ominaisuudet toimivat framework tai kielellä kirjoitettu API-sovellukset.  Oman API voi perustua ASP.NET, Java, Node.js tai framework, joka palvelun sovellus tukee.

Sovelluksen palvelun siirtää JSON web tunnuksen (JWT) HTTP-pyynnön Authorization-otsikko- ja minkä tahansa kielen tai framework kirjoitettua koodia saa tarvitsemiaan tietoja tunnuksen. Lisäksi sovelluksen-palvelun avulla voit helpottaa yleisimmin käytettyjä saatavat määrittämällä määräten otsikot, kuten jokin seuraavista:

* X-MS-ASIAKAS-LYHENNYS-NIMI
* X-MS-ASIAKAS-LYHENNYS-TUNNUS
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
Voit käyttää .NET-Ohjelmointirajapinta `Authorize` määrite ja tarkasti rajattuja lupaa, voit helposti kirjoittaa koodin mukaan saatavat, koska .NET luokkia on täytetty tietoja puolestasi.

## <a name="multiple-protection-options"></a>Useita suojausasetukset

Sovelluksen palvelun estää anonyymi pyyntöjen ottamasta API-sovelluksen, se välittämään pyynnöt ja vahvista tunnusten pyyntöihin, jotka sisältävät niitä, tai se antaa läpi kaikki palvelupyynnöt tekemättä mitään toimia niitä:

1. Salli vain todennetut pyynnöt API sovelluksen saavuttamiseksi.

    Jos anonyymi pyyntö on vastaanotettu selaimella, sovelluksen palvelun ohjaa tulee kirjautumissivu todennus-palveluntarjoajasi (Azure AD, Google, Twitter jne.), voit valita. 

    Kun tämä asetus, sinun ei tarvitse kirjoittaa todennus koodia lainkaan sovelluksen ja luvan koodi on yksinkertaistettu, koska tärkeimmät saatavat toimitetaan HTTP-otsikot.

2. Salli kaikki palvelupyynnöt saavuttaa API-sovelluksen, mutta Vahvista todennetut pyynnöt ja välittää todennustiedot HTTP-otsikoiden pitkin.

    Tämän asetuksen avulla voit joustavammin käsittely anonyymit pyynnöt, mutta sinun tarvitse kirjoittaa koodia, jos haluat estää anonyymit käyttäjät oman Ohjelmointirajapinnan käyttäminen. Suosituimpien saatavat siirretään pyyntöjen otsikot, luvan koodi on melko yksinkertainen.
    
3. Salli kaikki pyynnöt käytettävissä oman API, ei käsittele pyyntöjä todennustiedot.

    Tämä asetus ei täysin sovelluksen koodin ylöspäin todennus- ja tehtävät.

[Azure-portaalissa](https://portal.azure.com/), valitse vaihtoehto, jota haluat käyttää **todennus / luvan** sivu.

![](./media/app-service-api-authentication/authblade.png)

Ottaa käyttöön **Sovelluksen todentaminen**asetukset 1 ja 2 ja valitse **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** avattavasta luettelosta **Kirjaudu sisään** tai **Salli-pyyntö (mitään toimia)**.  Jos valitset **Kirjaudu sisään**, sinulla käyttöoikeustarkistuspalvelun ja määrittää, että palvelun.

![](./media/app-service-api-authentication/actiontotake.png)

Lisätietoja käyttöoikeuksien määrittämisestä on artikkelissa [Azure Active Directory-kirjautuminen käyttämään App palvelusovelluksen määrittämisestä](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Artikkelin koskee API sovellusten sekä mobiilisovellusten ja sen linkit toisiin ohjeaiheisiin, muut käyttöoikeustarkistuspalvelun.
 
## <a id="internal"></a>Palvelun tilin todennus

Sovelluksen todentaminen toimii sisäinen tilanteita, joissa esimerkiksi soitat API-sovelluksesta toiseen API-sovellukseen. Tässä skenaariossa voit avata tunnusta tunnistetiedoilla palvelutilin käyttäjän tunnistetietojen sijaan. Palvelutili käytetään myös nimitystä Azure Active Directory- *palvelun pääasiallista* ja tällaisen tilin käyttäminen todennusta käytetään myös nimitystä palvelun skenaario. 

Palvelun skenaarioille suojata kutsuttu API-sovelluksen Azure Active Directoryn avulla ja antaa AAD palvelun pääasiallista todennus-tunnuksen, kun soitat API-sovellus. Saat tunnusta antamalla asiakkaan tunnuksen ja asiakkaan salainen AAD-sovelluksesta. Vain Azure-koodia ei ole erityistä tarvitaan, kuten käytettävä täytyttävä käsittelyyn Mobile Services Zumo-tunnuksen. Tässä skenaariossa ASP.NET API-sovelluksilla Esimerkki piiriin opetusohjelman [Service API-sovellusten pääasiallista tunnistus](app-service-api-dotnet-service-principal-auth.md).

Jos haluat käsitellä palvelun skenaario käyttämättä App todentaminen, voit käyttää asiakkaan varmenteet tai perustodentamista. Lisätietoja Azure asiakkaan varmenteet on artikkelissa [Siitä, miten voit määrittää TLS molemminpuoleista verkkosovelluksissa](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Lisätietoja perustodentamista ASP.NET-kohdassa [ASP.NET-verkko-Ohjelmointirajapinnan 2 todennus suodattimet](http://www.asp.net/web-api/overview/security/authentication-filters).

Palvelun tilin todennus App palvelun logiikan sovelluksesta API-sovellukseen on luettava tapaus, joka on selitetty [mukautetun API isännöimät logiikan sovellukset App-palvelun avulla](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobiilisovelluksen todennus

Lisätietoja käsittelemisestä mobiilisovelluksista todennus on [ohjeet mobile-sovellusten tunnistus](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Sovelluksen todentaminen toimii samalla tavalla mobiilisovellukset ja API-sovellukset.
  
## <a name="more-information"></a>Lisätietoja

Saat lisätietoja todennus- ja Azure sovelluksen-palvelussa on seuraavissa resursseissa:

* [Laajennettavassa App palvelun todennus ja todennus](/blog/announcing-app-service-authentication-authorization/)
* [Sovelluksen palvelusovelluksen Azure Active Directory-kirjautuminen käyttämään määrittäminen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Sisältää linkkejä muihin käyttöoikeustarkistuspalvelun sivun yläreunassa.) 

Lisätietoja OAuth 2.0 OpenID yhteyden ja JSON Web tunnusten (JWT) on seuraavissa resursseissa.

* [OAuth 2.0 käytön aloittaminen] (http://shop.oreilly.com/product/0636920021810.do "OAuth 2.0 käytön aloittaminen") 
* [Johdanto OAuth2, OpenID yhteyden ja JSON Web tunnukset (JWT) - PluralSight kurssi](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Luomiseen ja suojaaminen RESTful-Ohjelmointirajapinta useita Client-ASP.NET - PluralSight kurssi](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Saat lisätietoja Azure Active Directory on seuraavissa resursseissa.

* [Azure AD-skenaariot](http://aka.ms/aadscenarios)
* [Azure AD kehittäjien opas](http://aka.ms/aaddev)
* [Azure AD-objektit](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on tietoja todennus- ja sovelluksen-palvelu, jota voit käyttää API-sovellusten ominaisuuksia. Hakeminen aloittaminen sarjan seuraavaan opetusohjelmaan näyttää ottamisesta käyttöön [käyttöoikeuksien App Service API-sovelluksissa](app-service-api-dotnet-user-principal-auth.md).
