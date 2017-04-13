<properties
   pageTitle="Azure Active Directory-Sovelluskehittäjän opas | Microsoft Azure"
   description="Tässä artikkelissa on täydellinen opas developer aloittaminen resurssien Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="mbaldwin"/>


# <a name="azure-active-directory-developers-guide"></a>Azure Active Directory-Sovelluskehittäjän opas

## <a name="overview"></a>Yleiskatsaus
Käyttäjätietojen hallinta kuin service (IDMaaS)-ympäristö Azure Active Directory (AD) tarjoaa kehittäjille tehokas tapa jäsenyyksien hallinta integroida verkkosovelluksistaan. Seuraavissa artikkeleissa on yleiskatsaus toteutuksen ja Azure AD ominaisuuksia. Suosittelemme lukea niitä järjestyksessä tai siirry [Aloitus](#getting-started) , jos haluat tarkastella.


1. [Azure AD-integroinnin edut](./develop/active-directory-how-to-integrate.md): tutustu miksi Azure AD-integrointi on suojattu kirjautuminen ja luvan paras mahdollinen ratkaisu.

1. [Azure AD-todennus skenaariot](active-directory-authentication-scenarios.md): voit hyödyntää yksinkertaistettu todennus Azure AD antamaan Sign-sovellukseen.

1. [Paikallisen ympäristön integroinnissa Azure AD-sovelluksiin](active-directory-integrating-applications.md): Lue, miten voit lisätä, päivittää ja poistaa sovellusten Azure AD ja tietoja integroitujen sovellusten liittyviä ohjeita.

1. [Azure AD Graph API](active-directory-graph-api.md): Azure AD käyttäminen ohjelmallisesti REST API päätepisteet Azure AD-kaavio-Ohjelmointirajapinnan avulla. Azure AD-kaavio-Ohjelmointirajapinnan on myös [Microsoft Graph](https://graph.microsoft.io/)kautta. Microsoft Graph on yhdistetty API, joka mahdollistaa useita Microsoft-pilvipalvelussa API yksittäisen REST API-päätepisteen avulla ja yksittäisen käyttöoikeustietue käytön.

1. [Azure AD-todennus kirjastojen](active-directory-authentication-libraries.md): todennetaan helposti käyttäjät voivat hankkia Accessin tunnusten Azure AD-todennus kirjastojen käyttämällä .NET-JavaScript-tavoite-C- ja Android.


## <a name="getting-started"></a>Käytön aloittaminen

Näissä Opetusohjelmissa useita alustojen on räätälöity ja avulla voit aloittaa nopeasti kehittäminen Azure Active Directory-hakemistosta. Edellytyksenä sinun täytyy [Hae Azure Active Directory-vuokraajan](active-directory-howto-tenant.md).

### <a name="mobile-and-pc-application-quick-start-guides"></a>Matkapuhelin ja tietokoneen sovelluksen quick start apuviivat

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android-laitteeseen](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windowsin Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android-laitteeseen](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windowsin Universal](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[Integroida OAuth 2.0](active-directory-protocols-oauth-code.md)|

### <a name="web-application-quick-start-guides"></a>Web-sovelluksen quick start apuviivat

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![JavaScript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID yhdistäminen](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[Integroida OpenID yhdistäminen](active-directory-protocols-openid-connect-code.md)|

### <a name="web-api-quick-start-guides"></a>Verkko-Ohjelmointirajapinnan pika-aloitusoppaiden

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### <a name="querying-the-directory-quickstart-guide"></a>Kyselyt directory pika-aloitusopas

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Graph-Ohjelmointirajapinta](active-directory-graph-api-quickstart.md)|

## <a name="how-tos"></a>How-TOS

Seuraavissa artikkeleissa kerrotaan, kuinka tiettyjen tehtävien suorittamiseen Azure Active Directoryn avulla:

- [Hae Azure AD-vuokraajan](active-directory-howto-tenant.md)
- [Usean vuokraajan sovelluksen kuvion minkä tahansa Azure AD-käyttäjän kirjautuminen](active-directory-devhowto-multi-tenant-overview.md)
- Ota käyttöön toimintojen sovelluksen SSO käyttämällä ADAL-, [Android](active-directory-sso-android.md) - ja [iOS](active-directory-sso-ios.md) -laitteissa
- [Tehdä sovelluksesta AppSource sertifioitu Azure AD varten](active-directory-devhowto-appsource-certified.md)
- [Sovelluksen Azure AD-sovelluksen valikoiman luettelo](active-directory-app-gallery-listing.md)
- [Lähetä web apps Office 365: n myyjä-koontinäyttö](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Azure Active Directory-sovellusluettelo ymmärtäminen](active-directory-application-manifest.md)
- [Tietoja Kirjaudu sisään- ja sovelluksen hankintaa painikkeiden-asiakassovellus liittyviä ohjeita](active-directory-branding-guidelines.md)
- [Esikatselu: Miten voit luoda sovellukset, jotka Allekirjoita-käyttäjät sekä henkilökohtainen ja työpaikan tai oppilaitoksen tilit](active-directory-appmodel-v2-overview.md)
- [Esikatselu: Sovellukset, jotka Rekisteröi ja kirjaudu sisään kuluttajille muodostaminen](../active-directory-b2c/active-directory-b2c-overview.md)
- [Esikatselu: määrittäminen suojaustunnuksen käyttöajan Azure AD](active-directory-configurable-token-lifetimes.md) PowerShellin avulla. Katso [käytännön toiminnot](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ja tiedot [käytännön kohteen](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) määrittäminen Azure AD-kaavio-Ohjelmointirajapinnan kautta.

## <a name="reference"></a>Viittaus

Seuraavissa artikkeleissa antaa muille käyttäjille ja kirjaston käyttöoikeuksien API, protokollat, virheitä, MALLIKOODEJA ja päätepisteet foundation viittaus.  

###  <a name="support"></a>Tuki
- [Merkityt kysymykset](http://stackoverflow.com/questions/tagged/azure-active-directory): Etsi Azure Active Directory ratkaisujen Pinon ylivuoto hakemalla tunnisteet [azure active-kansio](http://stackoverflow.com/questions/tagged/azure-active-directory) ja [adal](http://stackoverflow.com/questions/tagged/adal).
- Katso [Azure AD developer sanasto](active-directory-dev-glossary.md) joistain usein käytettyjä termejä sovellusten kehittämisen ja integrointi liittyviä määrityksiä.

### <a name="code"></a>Koodi

- [Avaa lähde Azure Active Directory-kirjastot](http://github.com/AzureAD): Helpoin tapa löytää kirjaston lähde on Microsoftin [tiedostokirjaston luettelon](active-directory-authentication-libraries.md)avulla.

- [Azure Active Directory-mallit](https://github.com/azure-samples?query=active-directory): käyttämällä [MALLIKOODEJA indeksi](active-directory-code-samples.md)on helpointa siirtyminen näytteiden luettelossa.

- [Active Directory käyttöoikeuksien kirjaston (ADAL), .NET](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) - oppaat on käytettävissä [uusimmat pääversion](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory) ja [edellisen pääversion](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory).

### <a name="graph-api"></a>Graph-Ohjelmointirajapinta

- [Kaavion API viittaus](https://msdn.microsoft.com/library/azure/hh974476.aspx): Azure Active Directory Graph-Ohjelmointirajapinnan muiden viittauksen. [Näytä vuorovaikutteinen Graph API viittaus-toiminto](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Kaavion API käyttöoikeuksien käyttöalueet](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): OAuth 2.0 käyttöoikeuksia alueet, joiden avulla sovelluksen on kansio palvelutili tietojen käyttöoikeuksien hallinta.

### <a name="authentication-and-authorization-protocols"></a>Todennus- ja protokollat

- [Kirjautuminen avaimen palauttaminen Azure AD](active-directory-signing-key-rollover.md): Lisätietoja Azure AD allekirjoitus avaimen palauttaminen cadence ja päivittämisestä sovelluksen tilanteissa näppäintä.

- [OAuth 2.0-protokolla: luvan koodin myönnä](active-directory-protocols-oauth-code.md): OAuth 2.0-protokollan luvan koodin myöntäminen avulla voit sallia access Web-sovellusten ja verkko-ohjelmointirajapinnan Azure Active Directoryssa vuokraaja.

- [OAuth 2.0-protokolla: tietoja implisiittinen myöntäminen](active-directory-dev-understanding-oauth2-implicit-grant.md): Lisätietoja implisiittinen lupa, myöntäminen ja onko sen oikeassa sovelluksen.

- [OAuth 2.0-protokolla: palvelun puhelut käyttämällä asiakkaan käyttäjätiedot palvelun](active-directory-protocols-oauth-service-to-service.md): OAuth 2.0 asiakkaan käyttäjätiedot myöntäminen sallii sijaan käyttäjäksi tekeytyminen verkkopalvelun (luottamuksellisia asiakas) voi todentaa soitettaessa toiseen WWW-palvelun omassa tunnistetietojen avulla. Tässä skenaariossa asiakas on yleensä keskimmäisen web-palvelu, daemon palveluun tai sivuston.

- [OpenID yhteyden 1.0-protokolla: Kirjaudu sisään- ja todennusta](active-directory-protocols-openid-connect-code.md): OpenID yhteyden 1.0-protokollan laajentaa OAuth 2.0 käyttää todennusprotokollaa. Asiakassovellus voi vastaanottaa id_token hallitsemaan Kirjaudu sisään tai verkkotunnistetietojen luvan koodin kulun sekä id_token ja luvan vastaanottamaan.

- [SAML 2.0-protokollan viittaus](active-directory-saml-protocol-reference.md): SAML 2.0-protokollan avulla sovellukset ja tarjota yksittäisen Sign-käyttökokemuksen käyttäjilleen.

- [WS Federation 1.2 protokolla](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory tukee WS Federation 1.2 Web Services Federation versio 1.2 määrityksen mukaan. Saat lisätietoja federation metatietojen asiakirja [Federation metatiedot](active-directory-federation-metadata.md).

- [Tuetut tunnuksen ja varaa tyypit](active-directory-token-and-claims.md): tämän oppaan avulla voit ymmärrä ja arvioiminen SAML 2.0 ja JSON Web tunnusten (JWT) tunnusten saatavat.

## <a name="videos"></a>Videot

### <a name="build"></a>Muodosta

Nämä yleiskatsaus esityksissä sovellusten kehittämisestä Azure Active Directoryn avulla ominaisuuksien kaiuttimet, jotka suoraan suunnitteluryhmät ryhmän työskentelevät. Esitysten käsittelevät keskeisiä, mukaan lukien IDMaaS, todennus, tunnistetietojen yhdistämisessä ja kertakirjautuminen.

- [Microsoft Identity: Tilan unionin ja tulevien suunta](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Jäsenyyksien hallinta palveluna Moderni sovellusten](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Nykyaikainen web-sovellusten Azure Active Directory-hakemistosta](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Nykyaikainen alkuperäisen sovellusten Azure Active Directory-hakemistosta](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### <a name="azure-friday"></a>Azure perjantai
[Azure perjantai](https://azure.microsoft.com/documentation/videos/azure-friday/) on toistuva perjantai asiantuntijoiden Azure aiheet erilaisilla haastatellaan 1:1 videosarja, joka on varattu tarjota sinulle lyhyt (10 – 15 minuuttia).  Suodatin-toimintoa käytetään sivun kaikki Azure Active Directory-videoita.

- [Azure tunnistetietojen 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure tunnistetietojen 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure tunnistetietojen 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## <a name="social"></a>Sosiaaliset

- [Active Directory-ryhmän blogi](http://blogs.technet.com/b/ad/): uusin kehitys Azure Active Directory-maailmassa.

- [Azure Active Directory Graph-ryhmän blogi](http://blogs.msdn.com/b/aadgraphteam): Azure Active Directory-tiedot, jotka liittyvät Graph-Ohjelmointirajapinnan.

- [Cloud tunnistetietojen](http://www.cloudidentity.net): ajatuksia-palvelu,-pääasiallista Azure Active Directory PM tunnistetietojen hallinta.  

- [Azure Active Directory Twitter](https://twitter.com/azuread): Azure Active Directory-ilmoitukset-140 merkkiä.

## <a name="windows-server-on-premises-development"></a>Windows Server paikallisen kehittäminen
Ohjeita käyttämällä Windows Server ja Active Directory Federation Services (ADFS) on artikkelissa:

- [AD FS tilanteita, joissa kehittäjät](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-scenarios-for-developers): yleiskatsaus AD FS-osat ja kuinka se toimii, tuettu todennus ja luvan käyttötavoista tiedot.
- [AD FS vaiheittaisissa ohjeissa](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/ad-fs-development): hallintapaketteihin artikkeleita, jossa on vaiheittaiset ohjeet yksityiskohtaisista liittyvät todennus/luvan kulkee luettelo.
