<properties 
    pageTitle="Avaimen käsitteitä API hallinta" 
    description="Lisätietoja API, tuotteiden, roolit, ryhmät ja muut API hallinta avaimen käsitteitä." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Tuominen Ohjelmointirajapinnan työvaiheita Azure API hallinnan määritys

Uusi API voidaan luoda API hallinta- ja toiminnot, jotka on lisätty manuaalisesti tai Ohjelmointirajapinnan voidaan tuoda ja toimintojen yhdessä vaiheessa.

API ja niiden toimintoja voidaan tuoda seuraavissa muodoissa avulla.

-   WADL
-   Swagger

Tämän oppaan näyttää kuinka luoda uuden API ja viedä toimintojaan yhdessä vaiheessa. Lisätietoja luomista manuaalisesti Ohjelmointirajapinnan ja toimintojen lisääminen on artikkelissa [luomisesta ohjelmointirajapinnan][] sekä [lisätä toimintoja API][].

## <a name="import-api"> </a>Tuo Ohjelmointirajapinta

Ohjelmointirajapinnan on luotu ja määritetty publisher-portaalissa. Voit käyttää publisher-portaali valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

![Publisher-portaalissa][api-management-management-console]

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta ja valitse sitten **Tuo API**.

![Tuo Ohjelmointirajapinta][api-management-import-apis]

**Tuo API** -ikkunassa on kolme välilehteä, jotka vastaavat antamaan API määrityksen kolmella tavalla.

-   **Leikepöydän** avulla voit Liitä API määrityksen nimettyjen tekstiruutuun.
-   **Tiedostosta** antaa selaamalla ja valitse tiedosto, joka sisältää API-määritys.
-   **URL-** voit Ohjelmointirajapinnan määrityksen URL-osoitteen.

![Tuontimuoto Ohjelmointirajapinta][api-management-import-api-clipboard]

Kun olet antanut API määrityksen, käytä valintanappien oikealla osoittamaan määritys-muodossa. Seuraavat muodot ovat tuettuja.

-   WADL
-   Swagger

Kirjoita **API WWW-URL-osoite liite**. Tämä lisätään API hallintapalvelun perus URL-osoite. Kantaluku URL-osoite on yhteinen kaikki ohjelmointirajapinnan isännöimät API hallintapalvelun esiintymissä. API hallinta erottaa ohjelmointirajapinnan niiden jälkiliite mukaan ja näin ollen liitteen on oltava yksilöllinen jokaisen Ohjelmointirajapinnan tietyn API hallinta palvelun esiintymän.

Kun kaikki arvot lisätään, valitse Luo Ohjelmointirajapinnan ja liittyvät toiminnot **Tallenna** . 

>[AZURE.NOTE] Opetusohjelmaan tuonnin basic Laskimen API Swagger muodossa-kohdassa [Manage oman ensimmäisen Ohjelmointirajapinnan Azure API hallinta](api-management-get-started.md).

## <a name="export-api"></a> Vie Ohjelmointirajapinta

Lisäksi tuominen uuteen API, voit viedä oman ohjelmointirajapinnan määritelmät publisher-portaalista. Valitse **Vie API** oman **API** **Yhteenveto-välilehti** .

![Export-Ohjelmointirajapinta][api-management-export-api]

API voidaan viedä WADL tai Swagger avulla. Valitse haluamasi muoto, valitse **Tallenna**ja valitse sijainti, johon haluat tallentaa tiedoston.

![API vientimuoto][api-management-export-api-format]

## <a name="next-steps"> </a>Seuraavat vaiheet

Kun Ohjelmointirajapinnan luomisen ja tuoda toimintoja-linkistä, voit tarkistaa ja määrittää muita asetuksia, Lisää Ohjelmointirajapinnan tuotteen ja julkaista sen niin, että se on käytettävissä kehittäjille. Lisätietoja on artikkelissa seuraavat apuviivat.

-   [API asetusten määrittäminen][]
-   [Voit luoda ja julkaista tuote][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance

[Toimintojen lisääminen Ohjelmointirajapinta]: api-management-howto-add-operations.md
[Voit luoda ja julkaista tuote]: api-management-howto-add-products.md
[API luominen]: api-management-howto-create-apis.md
[API asetusten määrittäminen]: api-management-howto-create-apis.md#configure-api-settings
