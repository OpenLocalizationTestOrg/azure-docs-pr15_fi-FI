<properties 
    pageTitle="API luominen Azure API hallinta" 
    description="Lue, miten voit luoda ja määrittää ohjelmointirajapinnan Azure API hallinta." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-apis-in-azure-api-management"></a>API luominen Azure API hallinta

API hallinta Ohjelmointirajapinnan edustaa toiminnoista, joita voi käynnistää asiakassovelluksia. Publisher-portaalissa luodaan uusi ohjelmointirajapinnan ja valitse haluamasi toiminnot on lisätty. Kun toiminnot on lisätty, Ohjelmointirajapinnan lisätään tuotteen ja julkaista. Kun Ohjelmointirajapinnan on julkaistu, se voidaan tilannut ja sovelluskehittäjille.

Tämän oppaan näyttää ensimmäiseksi prosessi: Voit luoda ja uusi API paikallaan API hallinnan. Katso lisätietoja toimintojen lisääminen ja julkaisemista tuotteen, [Voit lisätä toimintoja API][] ja [miten voit luoda ja julkaista tuote][].

## <a name="create-new-api"> </a>Luo uusi Ohjelmointirajapinta

Ohjelmointirajapinnan on luotu ja määritetty publisher-portaalissa. Voit käyttää publisher-portaali valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** .

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta ja valitse sitten **Lisää API**.

![Luo Ohjelmointirajapinta][api-management-create-api]

**Lisää uusi API** -ikkunan avulla voit määrittää uuden Ohjelmointirajapinnan.

![Lisää uusi Ohjelmointirajapinta][api-management-add-new-api]

Määritä uusi Ohjelmointirajapinnan käytetään seuraavat kentät.

-   **Verkko-Ohjelmointirajapinnan nimi** sisältää Ohjelmointirajapinnan yksilöllinen ja kuvaava nimi. Kehitystyökalut- että publisher-portaaleihin näkyy se.
-   **WWW-palvelun URL** viittaa HTTP-palvelun käyttöönoton Ohjelmointirajapinnan. API hallinta välittää osoitteen pyynnöt.
-   **Verkko-Ohjelmointirajapinnan URL-Osoitteen jälkiliite** lisätään perus API management-palvelun URL-osoite. Kantaluku URL-osoite on yhteinen kaikki ohjelmointirajapinnan ylläpitämä erillisen service API hallinta. API hallinta erottaa ohjelmointirajapinnan niiden jälkiliitteen mukaan ja näin ollen liitteen on oltava yksilöllinen jokaisen API annetun Publisherin.
-   **Verkko-Ohjelmointirajapinnan URL-malli** määrittää protokollat avulla voidaan käyttää Ohjelmointirajapinnan. HTTPs on määritetty oletusarvoisesti.
-   Tämä uusi API lisääminen myös tuote- **tuotteiden (valinnainen)** avattavasta ja valitse tuote. Tässä vaiheessa voit toistaa useita kertoja Ohjelmointirajapinnan lisääminen useita tuotteita.

Kun haluamasi arvot on määritetty, valitse **Tallenna**. Kun uusi Ohjelmointirajapinnan on luotu, Ohjelmointirajapinnan Yhteenveto-sivulla näkyy publisher-portaalissa.

![API yhteenveto][api-management-api-summary]

## <a name="configure-api-settings"> </a>Määrittäminen API asetukset

Voit tarkistaa ja muokata määrityskohde Ohjelmointirajapinnan **asetukset** -välilehti. **Verkko-Ohjelmointirajapinnan nimi**, **WWW-palvelun URL**ja **Ohjelmointirajapinta WWW-URL-osoite jälkiliite** aluksi määritetään Ohjelmointirajapinnan luodaan ja muokata tätä. **Kuvaus** on valinnainen kuvaus, ja **Verkko-Ohjelmointirajapinnan URL-malli** määrittää protokollat avulla voidaan käyttää Ohjelmointirajapinnan.

![API-asetukset][api-management-api-settings]

Voit määrittää yhdyskäytävän todennusta Taustajärjestelmä palvelun käyttöönoton Ohjelmointirajapinnan, valitsemalla **Suojaus** -välilehti. **Tunnuksilla** avattavasta voidaan **HTTP basic** - tai **asiakkaan varmenteet** käyttöoikeuksien määrittämiseen. Käyttämään HTTP perustodentamista, kirjoita haluamasi tunnistetiedot. Lisätietoja Asiakasvarmenteen todennus käyttämisestä on artikkelissa [suojaamisesta taustatietokantaan-palveluita käyttämällä Asiakasvarmenteen todennus Azure API hallinta][].

**Suojaus** -välilehden voidaan myös käyttämällä OAuth 2.0 **käyttäjän käyttöoikeuksien** määrittämiseen. Katso lisätietoja, [miten sallivat developer tilille OAuth 2.0 Azure API hallinta][].

![Perustodentamista asetukset][api-management-api-settings-credentials]

Valitse **Tallenna** Tallenna API-asetuksiin tekemäsi muutokset.

## <a name="next-steps"> </a>Seuraavat vaiheet

Kun Ohjelmointirajapinnan on luotu ja määritetty asetuksia, seuraavat vaiheet ovat toimintojen lisääminen Ohjelmointirajapinnan, lisätä tuotteeseen Ohjelmointirajapinnan ja julkaista sen niin, että se on käytettävissä kehittäjille. Lisätietoja on seuraavissa artikkeleissa.

-   [Toimintojen lisääminen Ohjelmointirajapinta][]
-   [Voit luoda ja julkaista tuote][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Toimintojen lisääminen Ohjelmointirajapinta]: api-management-howto-add-operations.md
[Voit luoda ja julkaista tuote]: api-management-howto-add-products.md

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Suojaamisesta taustatietokantaan-palveluita käyttämällä asiakkaan todennus Azure API hallinta]: api-management-howto-mutual-certificates.md
[Voit sallia developer tilille OAuth 2.0 Azure API hallinta]: api-management-howto-oauth2.md