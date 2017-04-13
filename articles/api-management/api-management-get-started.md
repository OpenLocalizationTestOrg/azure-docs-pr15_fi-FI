<properties
    pageTitle="Hallita oman ensimmäisen Ohjelmointirajapinnan Azure API Management | Microsoft Azure"
    description="Lue, miten voit luoda ohjelmointirajapinnan ja lisää toimintoja API hallinnan käytön aloittaminen."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Hallitse oman ensimmäisen Ohjelmointirajapinnan Azure API hallinta

## <a name="overview"> </a>Yleiskatsaus

Tämän oppaan avulla voit aloittaa käytön Azure-Ohjelmointirajapinnan hallinnasta ja ensimmäisen API-soittaminen nopeasti.

## <a name="concepts"> </a>Azure API hallinnan ominaisuudet?

Azure API hallinnan avulla voit ottaa minkä tahansa Taustajärjestelmä ja Käynnistä täydellisiksi API-ohjelman sen pohjalta.

Yleisiä tilanteita, joissa ovat seuraavat:

* **Securing mobile infrastruktuuriin** mukaan hallitseva access ja API näppäimet estää DOS hyökkäyksille rajoittaminen tai käyttämällä Lisäasetukset suojauskäytäntöjä, kuten JWT suojaustunnuksen kelpoisuuden.
* **Ottaminen käyttöön ISV kumppanin ekosysteemien** tarjoaa nopean kumppanin onboarding palvelun developer-portaalissa ja muodostetaan Ohjelmointirajapinta ulkoseinä-sisäinen käyttöotot, jotka eivät ole kypsiä kumppanin kulutus irrottamisen.
* **Sisäinen API-ohjelmaa** organisaation keskitetystä sijainnista tarjoamalla vaihtamaan tietoja käytettävyyttä ja uusimmat muutokset API hallitseva organisaation tili perusteella perustuvat suojatun kanavan Ohjelmointirajapinta yhdyskäytävän ja taustaan välillä.


Järjestelmä koostuu seuraavat osat:

* **API yhdyskäytävä** on päätepiste, joka:
  * Hyväksyy API soittaa ja reitittää oman backends.
  * Tarkistaa API näppäimet, JWT tunnuksia, varmenteet ja muut tunnistetiedot.
  * Pakottaa käyttökiintiön korvauksen rajoitukset.
  * Muuntaa oman API suoraan selaimessa ilman koodin muutokset.
  * Välimuistiin Taustajärjestelmä vastaukset, jossa määrittäminen.
  * Lokit Soita metatietojen analytics tarkoituksiin.

* **Publisher-portaalissa** on hallintaliittymän, jossa määrityksen API-ohjelmaa. Käytä sen:
    * Määritä tai tuo API rakenne.
    * Paketin ohjelmointirajapinnan tuotteisiin.
    * Määritä käytäntöjä, kuten kiintiön tai API-muunnoksia.
    * Pyydä analytics tiedot.
    * Käyttäjien hallinta.

* **Developer-portaalissa** on ensisijainen web-sivuston kehittäjille, jossa he voivat:
    * API luku-ohjeista.
    * Kokeile API-Liittymän kautta vuorovaikutteinen konsolin.
    * Luo tili ja tilaaminen Hae API-näppäimiä.
    * Access-analytics omia käyttöraportteja.


## <a name="create-service-instance"> </a>API hallinta luominen

>[AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion][].

Ensimmäinen vaihe API hallinnan käyttäminen on luotava palvelun esiintymän. Kirjautuminen [Azure perinteinen Portal][] ja valitse **Uusi**, **Sovelluksen Services**, **API hallinta**, **Luo**.

![Uuden esiintymän API hallinta][api-management-create-instance-menu]

Määritä **URL-osoite**, yksilöllinen aliraportti toimialuenimi käytettävä URL-osoite.

Valitse haluamasi **tilaus** - ja **alueasetusten** palveluesiintymä. Kun olet tehnyt valinnat, valitse **Seuraava** .

![Uusi Ohjelmointirajapinta hallintapalvelun][api-management-create-instance-step1]

Kirjoita **Contoso Ltd.** **Organisaationimi**ja kirjoita sähköpostiosoitteesi **Järjestelmänvalvojan sähköposti** -kenttään.

>[AZURE.NOTE] Tätä sähköpostiosoitetta käytetään API hallintajärjestelmän ilmoituksia. Lisätietoja on artikkelissa [ilmoitukset ja sähköpostimalleja Azure Ohjelmointirajapinta hallinnan määrittäminen][].

![Uusi API hallintapalvelun][api-management-create-instance-step2]

API Management-palvelun esiintymät ovat käytettävissä kolme tasoa: kehittäjä, Vakio ja Premium. Uusi API Management-palvelun esiintymät luodaan oletusarvoisesti Developer taso. Valitse vakio- tai Premium-taso, **Lisäasetukset** -valintaruutu ja valitse seuraavassa näytössä haluamasi taso.

>[AZURE.NOTE] Kehitystyökalut-taso on kehitystä, testaus ja toteutettu API-sovellus, jossa suuren käytettävyyden ei ole merkitystä. Vakio- ja Premium-tasoa voi skaalata varattu yksikkö-Laske käsittelemään enemmän liikennettä. Vakio- ja Premium tasoa on eniten käsittely power ja suorituskyvyn Ohjelmointirajapinta hallinta-palvelussa. Tässä opetusohjelmassa voivat suorittaa käyttämällä minkä tahansa tason. Saat lisätietoja API hallinta tasoa [API hallinta hinnoittelua][].

Valitse Luo palveluesiintymä-valintaruutu.

![Uusi API hallintapalvelun][api-management-instance-created]

Kun palveluesiintymä on luotu, seuraava vaihe on luoda tai tuoda Ohjelmointirajapinnan.

## <a name="create-api"> </a>Tuo Ohjelmointirajapinta

Ohjelmointirajapinnan koostuu joukko toiminnoista, joita voidaan kutsua asiakas-sovelluksesta. API-toiminnot ovat välityspalvelimen aiemmin web-palveluihin.

API voidaan luoda (ja toimintoja voidaan lisätä) manuaalisesti tai ne on tuotu. Tässä opetusohjelmassa tuodaan otoksen Laskimen verkkopalvelun Microsoftin ja Azure isännöimät Ohjelmointirajapinnan.

>[AZURE.NOTE] Ohjeita Ohjelmointirajapinnan luominen ja lisääminen manuaalisesti toiminnot on artikkelissa [luomisesta ohjelmointirajapinnan](api-management-howto-create-apis.md) sekä [lisätä toimintoja API](api-management-howto-add-operations.md).

Ohjelmointirajapinnan on määritetty publisher-portaalista, jossa Azure perinteinen portaalin kautta. Publisher-portaalin saavuttamiseksi Valitse Azure perinteinen portaalissa Ohjelmointirajapinta Management-palvelun **hallinta** .

![Publisher-portaalissa][api-management-management-console]

API-Laskin tuotava valitsemalla **ohjelmointirajapinnan** **API hallinta** vasemmalla puolella ja valitse sitten **Tuo API**.

![Tuo API-painike][api-management-import-api]

Seuraavien toimien Laskimen API määrittäminen:

1. Valitse **URL-**, kirjoita **http://calcapi.cloudapp.net/calcapi.json** **määrityksen tiedoston URL-osoite** -tekstiruutuun ja valitse **Swagger** -valintanappi.
2. Kirjoita **Laskenta** **Ohjelmointirajapinta WWW-URL-osoite jälkiliite** teksti-ruutuun.
3. **Tuotteet (valinnainen)** -ruutuun ja valitse **Starter**.
4. Valitse Tuo Ohjelmointirajapinnan **Tallenna** .

![Lisää uusi Ohjelmointirajapinta][api-management-import-new-api]

>[AZURE.NOTE] **API hallinta** tukee tällä hetkellä Swagger tiedoston 1.2 sekä 2.0-version tuomista varten. Varmista, että, vaikka [Swagger 2.0-määrityksen](http://swagger.io/specification) ilmoittaa, että `host`, `basePath`, ja `schemes` ominaisuudet eivät ole pakollisia, Swagger 2.0 asiakirjan **täytyy** sisältää nämä ominaisuudet; muuten se ei tuoda. 

Kun Ohjelmointirajapinnan on tuotu, Ohjelmointirajapinnan Yhteenveto-sivulla näkyy publisher-portaalissa.

![API yhteenveto][api-management-imported-api-summary]

Ohjelmointirajapinta-osio on useita välilehtiä. **Yhteenveto** -välilehdessä näkyy basic arvot ja Ohjelmointirajapinnan tietoja. [Asetukset](api-management-howto-create-apis.md#configure-api-settings) -välilehteä käytetään tarkasteleminen ja muokkaaminen Ohjelmointirajapinnan määrityskohde. Hallinta Ohjelmointirajapinnan toimintoja käytetään [Toiminnot](api-management-howto-add-operations.md) -välilehti. Voit määrittää yhdyskäytävän todennus palvelimeen käyttämällä perustodentamista tai [molemminpuolinen todennus](api-management-howto-mutual-certificates.md)ja määrittää [käyttäjän käyttöoikeuksien avulla OAuth 2.0](api-management-howto-oauth2.md)voidaan **Suojaus** -välilehti.  Ilmoittaa sovelluskehittäjät, jotka käyttävät oman ohjelmointirajapinnan seurantakohteiden tarkasteleminen käytetään **Seurantakohteet** -välilehti. Voit määrittää tämän API sisältävät tuotteet käytetään **tuotteet** -välilehti.

Oletusarvon mukaan API hallinta jokaiselle esiintymälle sisältää kahden otoksen tuotteita:

-   **Starter**
-   **Rajoittamaton tallennus**

Tässä opetusohjelmassa Basic Laskimen Ohjelmointirajapinnan on lisätty Starter-tuotteen Ohjelmointirajapinnan tuotaessa.

Siirrä puhelut Ohjelmointirajapinnan kehittäjät on ensin tilattava tuote, joka antaa ne access siihen. Kehittäjät tilata developer-portaalissa-tuotteita tai Järjestelmänvalvojat voit tilata kehittäjät tuotteet publisher-portaalissa. Olet järjestelmänvalvoja, luomisen jälkeen API hallinta-esiintymän aiemmissa vaiheissa opetusohjelmassa, jotta ole vielä tilannut jokaisen tuotteen oletusarvoisesti.

## <a name="call-operation"> </a>Toiminnon kutsua developer-portaalissa

Toimintoja voidaan kutsua suoraan developer-portaalissa, joiden avulla voit tarkastella ja testaa Ohjelmointirajapinnan toimintojen kätevästi. Tämän opetusohjelman vaiheessa soitat Basic laskuri-Ohjelmointirajapinnan **Lisää kahden kokonaisluvun** toiminto. Valitse valikosta yläreunassa **Developer-portaalissa** oikeassa yläkulmassa publisher-portaalissa.

![Developer-portaalissa][api-management-developer-portal-menu]

Valitse **ohjelmointirajapinnan** yläreunan valikosta ja valitse sitten **Basic Laskimen** Nähdäksesi käytettävissä olevat toiminnot.

![Developer-portaalissa][api-management-developer-portal-calc-api]

Huomautus otoksen kuvaukset ja parametrit, jotka on tuotu sekä API ja toimintoja, jotka tarjoavat dokumentaatio sovelluskehittäjille, joka käyttää tätä toimintoa. Voit lisätä nämä kuvaukset myös, kun toiminnot on lisätty manuaalisesti.

Voit kutsua **Lisää kaksi kokonaisluvut** -toiminto valitsemalla **Kokeile**.

![Kokeile][api-management-developer-portal-calc-api-console]

Voit kirjoittaa joitakin arvoja oleville parametreille tai säilyttää oletusarvot ja valitse sitten **Lähetä**.

![HTTP Get][api-management-invoke-get]

Kun toiminto käynnistetään, developer-portaalissa näyttää **vastauksen tila** **vastauksen ylä**-ja **vastaus sisältöä**.

![Vastaus][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Analytics tarkasteleminen

Analytics Basic Laskimen katselemista palaa publisher-portaali valitsemalla **Hallitse** yläreunassa olevaa valikkoa oikeassa yläkulmassa developer-portaalissa.

![Hallinta][api-management-manage-menu]

Publisher-portaalin oletusnäkymä on **raporttinäkymät-ikkunan**, jossa esitetään yleiskatsaus API hallinta-esiintymä.

![Raporttinäkymät-ikkunan][api-management-dashboard]

Hiiren osoitinta kaavion **Basic Laskimen** haluamasi Ohjelmointirajapinnan käyttö tietyn mittaukset tiettynä aikajaksona.

>[AZURE.NOTE] Jos et näe kaikki rivit kaaviossa, palaa developer-portaalissa ja jotkin soittaa Ohjelmointirajapinnan kyselyjä, odota hetki ja sitten palaat koontinäyttö.

Valitse **Näytä tiedot** , joista näet Yhteenveto-sivulla API, kuten suurempi versiosta näytetyt arvot.

![Analytics][api-management-mouse-over]

![Yhteenveto][api-management-api-summary-metrics]

Yksityiskohtaiset arvot ja raporttien Valitse **Analytics** vasemmalla **API hallinta** -valikosta.

![Yleiskatsaus][api-management-analytics-overview]

**Analytics** -osassa on seuraavat neljä välilehteä:

-   **Yhdellä silmäyksellä** on yleinen käyttö- ja kuntotietojen arvot sekä yläreunan kehittäjät, parhaat tuotteet, yläreunan ohjelmointirajapinnan ja yläreunan toimintoja.
-   **Käyttö** on perusteellisempaa katsaus API puhelut ja kaistanleveys, mukaan lukien maantieteellinen esitys.
-   **Kunto** kohdistukset tila koodeja välimuistin success palkat, vastausajat ja API ja palvelun vastaus kertaa.
-   **Tehtävän** tarjoaa raportteja, jotka alirakenteeseen tehtävän developer-tuotteen, API ja toiminto.

## <a name="next-steps"> </a>Seuraavat vaiheet

- Lue, miten [Suojaa oman Ohjelmointirajapinnan kanssa korko rajoitukset](api-management-howto-product-with-rules.md).

[Azure ilmainen kokeiluversio]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Ilmoitukset ja sähköpostimalleja määrittämisestä Azure API hallinta]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Hinnat API hallinta]: http://azure.microsoft.com/pricing/details/api-management/

[Azure perinteinen Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
