<properties 
    pageTitle="Toimintojen lisääminen Azure API hallinta Ohjelmointirajapinnan | Microsoft Azure" 
    description="Opi lisää toimintoja Ohjelmointirajapinnan Azure API hallinta." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Toimintojen lisääminen Ohjelmointirajapinnan Azure API hallinta

Ennen kuin API hallinta Ohjelmointirajapinnan voidaan käyttää toiminnot on lisättävä. Tässä oppaassa kerrotaan, miten lisättävä ja määritettävä erilaisiin toimintoihin Ohjelmointirajapinnan API hallinta.

## <a name="add-operation"> </a>Lisää toiminto

Toimintojen lisätään ja määritetään API-publisher-portaalissa. Voit käyttää publisher-portaali valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** .

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Valitse haluamasi Ohjelmointirajapinnan publisher-portaalissa, ja valitse sitten **Toiminnot** -välilehti. 

![Toiminnot][api-management-operations]

Valitse Lisää uusi toiminto **Lisää toiminto** . **Uuden toiminnon** näkyvät ja **allekirjoitus** -välilehti valittuna oletusarvoisesti.

![Lisää toiminto][api-management-add-operation]

Määritä **HTTP verbin** valitsemalla avattavasta luettelosta.

![HTTP-menetelmä][api-management-http-method]

<a name="url-template"></a>

Määritä mallin URL-Osoitteen kirjoittamalla URL-osa, joka koostuu vähintään yksi URL-polku osia ja nolla tai useampia kyselyparametri. URL-osoite-mallin Ohjelmointirajapinta, Perusosoitteen liitetään tunnistaa yhtä HTTP-toimintoa. Siinä voi olla yksi tai useita nimetystä muuttujan osat, jotka on merkitty aaltosulkeet. Seuraavat muuttujan osat kutsutaan Malliparametrit ja dynaamisesti määritetyt arvot poimittujen pyyntö URL-osoite, kun pyyntö käsitellään API hallinta-ympäristössä.

![Mallin URL-osoite][api-management-url-template]

<a name="rewrite-url-template"></a>

Halutessasi voit määrittää **uuden esimerkkitekstin URL-malli**. Voit käyttää vakio URL-osoite-mallia käsittelyyn pyynnöt-edusta-ja kutsumista taustatietokantaan suorittamaan mallin mukaan muunnetun URL-Osoitteen kautta. Malliparametrit URL-Osoitteen mallia käytetään suorittamaan mallia. Seuraavassa esimerkissä esitetään, miten sisältötyypin koodattu polku segmenttiä edellisessä esimerkissä käytetty mallitietokanta web-palvelu voidaan suorittaa kyselyn parametrina Ohjelmointirajapinnan julkaistu API hallinta-ympäristö käyttämällä URL-mallien kautta.

![URL-Osoitteen mallin suorittamaan][api-management-url-template-rewrite]

Soittajat toimintoa käytetään muotoa `/customers?customerid=ALFKI` ja tämä yhdistetään `/Customers('ALFKI')` kun taustatietokantaan-palvelu käynnistyy.


**Näyttönimi** ja **kuvaus** sisältää toiminnon kuvaus ja niitä käytetään dokumentaatio tarjoamaan tämän Ohjelmointirajapinnan käyttäminen developer-portaalissa kehittäjät.

![Kuvaus][api-management-description]

Toiminnon kuvaus voidaan määrittää vain teksti- tai HTML- **kuvaus** -ruutuun.

## <a name="operation-caching"> </a>Toiminnon välimuistiin tallentaminen

Viive, jonka API kuluttajille, alentaa kaistanleveyden käyttöä ja lataa HTTP-web-palvelun käyttöönoton Ohjelmointirajapinnan nousuja vastauksen välimuistiin vähentää. 

Helposti ja nopeasti käyttöön välimuistiasetukset toimintoa, valitse **välimuisti** -välilehti ja valitse **Ota käyttöön** -valintaruutu.

![Välimuistiin tallentaminen][api-management-caching-tab]

**Kesto** määrittää ajanjakson aikana toiminnon vastaus säilyy välimuistin. Oletusarvo on 3 600 sekuntia tai 1 tunti.

Välimuistin näppäimet käytetään erottaa vastaukset niin, että vastaavat eri välimuistin avainta vastauksen saavat omassa erillisessä välimuistissa oleva arvo. Voit myös kirjoittaa tietyn kyselyparametri ja/tai HTTP-otsikoiden käytettäviksi tietojenkäsittely välimuistin avainarvot **Muuta kyselyparametri mukaan** ja **Muuta otsikoiden mukaan** -ruutuihin tarpeen mukaan. Kun ei ole määritetty, koko pyyntö URL-osoite ja HTTP otsikko-arvoja käytetään välimuistin avaimen luominen: **Hyväksy** ja **Hyväksy merkistö**.

>Lisätietoja välimuistin ja tallentamisesta välimuistiin käytäntöjä Katso, [miten välimuistin toiminnon tuloksiin Azure API hallinnassa][].


## <a name="request-parameters"> </a>Pyynnön parametrit

Toiminnon parametrit hallitaan Parametrit-välilehti. Parametrit on määritetty **allekirjoitus** -välilehden **Mallin URL-osoite** lisätään automaattisesti, ja se voi muuttaa vain muokkaamalla URL-malli. Lisää parametrit voidaan kirjoittaa manuaalisesti.

Lisää uusi kyselyparametri, valitse **Lisää kyselyn parametri** ja seuraavat tiedot:

-   **Nimi** - parametrin nimi.
-   **Kuvaus** - lyhyt kuvaus (valinnainen)-parametrin.
-   **Tyyppi** - parametrityyppi avattavassa alaspäin.
-   **Arvot** - arvoja, jotka voidaan varata parametriä. Yksi arvot voidaan merkitä oletukseksi (valinnainen).
-   **Pakollinen** - parametrin tehdä pakollisen valitsemalla valintaruudun valinta. 

![Pyynnön parametrit][api-management-request-parameters]

## <a name="request-body"> </a>Pyytää tekstissä

Jos toiminnon avulla (kuten HYLLYTETTY, Kirjaa) ja edellyttää tekstissä, voit antaa Esimerkki sen kaikki tuetut esitys muotoilut (kuten json, XML). 

>Pyyntö tekstissä käytetään vain dokumentointia varten ja ei ole vahvistettu.

Kirjoita pyyntö tekstissä, siirtymällä **runko** -välilehti.

Valitse **Lisää esitys**, ala kirjoittaa haluamasi sisältötyypin nimi (esimerkiksi sovelluksen/json), valitsemalla sen vieressä olevaa avattavan luettelon ja liitä haluamasi pyynnön leipätekstin Esimerkki valitussa muodossa-tekstiruutuun. 

![Pyydä tekstissä][api-management-request-body]

Valitse esitykset lisäksi voit myös määrittää valinnainen kuvauksen **kuvaus** -tekstiruutuun.

## <a name="responses"> </a>Vastaukset

Se on hyvä on esimerkkejä vastaukset kaikkien tila-koodien, joka voi tuottaa toiminnon. Kunkin tilakoodin voi olla useita vastauksen leipätekstin esimerkissä yksi kullekin tuetut sisältötyypit. 

Jos haluat lisätä vastausta, valitsemalla **Lisää** ja kirjoittamalla haluamasi tilakoodi. Tässä esimerkissä tilakoodi on **200 OK**. Kun koodi on näkyvissä olevaa avattavaa luetteloa, valitsemalla sen ja vastauskoodi on luotu ja lisätään toiminto.

![Vastauksen koodi][api-management-response-code]

Valitse **Lisää esitys**, kirjoita haluamasi sisältötyypin nimi (esimerkiksi sovelluksen/json) ja valitse se avattavassa alaspäin.

![Sisällön perusrakenne][api-management-response-body-content-type]

Liitä vastauksen leipätekstin Esimerkki valitussa muodossa-tekstiruutuun. 

![Vastauksen tekstissä][api-management-response-body]

Jos haluat, Lisää halutessasi kuvaus **kuvaus** -tekstiruutuun.

Kun toiminto on määritetty, valitse **Tallenna**.


## <a name="next-steps"> </a>Seuraavat vaiheet

Kun toiminnot lisätään Ohjelmointirajapinta, seuraava vaihe on Ohjelmointirajapinnan liittäminen tuotteen ja julkaista sen niin, että kehittäjät voivat soittaa toimintansa.

-   [Voit luoda ja julkaista tuote][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Voit luoda ja julkaista tuote]: api-management-howto-add-products.md
[Miten välimuistin toiminnon tulokset Azure API hallinta]: api-management-howto-cache.md