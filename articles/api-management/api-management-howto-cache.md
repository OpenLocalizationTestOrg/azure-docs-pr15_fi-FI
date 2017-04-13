<properties
    pageTitle="Lisää välimuistiin suorituskyvyssä Azure API hallinnan | Microsoft Azure"
    description="Lue, miten voit parantaa viive-kaistanleveyden käyttöä ja WWW-palvelun ladata API Management-palvelun puheluihin."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Lisää välimuistiin suorituskyvyssä Azure API hallinta

Toimintojen API hallinnan voi määrittää vastauksen välimuistiin tallentaminen. Vastauksen välimuistiin voi merkittävästi vähentää API viive-kaistanleveyden käyttöä ja WWW-palvelun kuormituksen tiedot, jotka eivät muutu usein.

Tämän oppaan avulla voit Lisää vastaus välimuistiasetukset oman API ja määritä otoksen Kaiku API toimille käytännöt. Voit osallistua toiminnon sitten Vahvista välimuistiin tallentamisen toiminnon developer-portaalissa.

>[AZURE.NOTE] Lisätietoja välimuistiin kohteiden avaimella käytännön lausekkeiden avulla on artikkelissa [mukautetun välimuistiin tallentamisen Azure API hallinta](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin noudattamalla tämän oppaan, sinulla on oltava API hallinnan palveluesiintymä Ohjelmointirajapinnan kanssa ja tuotteen määritetty. Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

## <a name="configure-caching"> </a>Määrittäminen välimuisti-toiminto

Tässä vaiheessa tarkastellaan välimuistiasetukset otosten Kaiku API **HAE resurssi (välimuistissa)** -toiminnon.

>[AZURE.NOTE] Kunkin API hallinta esiintymää on ennalta määritetty Kaiku-ohjelmointirajapinnalla, jonka avulla voidaan kokeilla ja tutustu API hallinta. Lisätietoja on artikkelissa [Azure API hallinnan käytön aloittaminen][].

Aloita valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

Valitse **ohjelmointirajapinnan** vasemmalla **API hallinta** -valikosta ja valitse sitten **Päivitä API**.

![Päivitä Ohjelmointirajapinta][api-management-echo-api]

Valitse **Toiminnot** -välilehti ja valitse sitten **Toiminnot** -luettelosta **(Välimuistissa) HAE resurssi** -toiminnon.

![Päivitä API-toiminnot][api-management-echo-api-operations]

Valitse toiminto välimuistiasetukset tarkastelemaan **välimuisti** -välilehti.

![Välimuistiin-välilehti][api-management-caching-tab]

Välimuistiasetukset toiminnon käyttöön valitsemalla **Ota käyttöön** -valintaruutu. Tässä esimerkissä välimuisti on käytössä.

Liittyvät haluttuihin kunkin toiminnon vastauksen, **Muuta kyselyparametri mukaan** ja **Muuta otsikoiden mukaan** -kenttien arvojen perusteella. Jos haluat tallentaa useita vastauksia kyselyparametri tai otsikot, voit määrittää ne nämä kaksi kenttää.

**Kesto** määrittää vanheneminen välimuistissa vastaukset. Tässä esimerkissä välin on **3600** sekuntia, joka vastaa tunnin.

Välimuistiin kokoonpanon käyttämisestä tässä esimerkissä, **HAE resurssi (välimuistissa)** -toiminnon ensimmäinen pyyntö palauttaa vastauksen Taustajärjestelmä-palvelusta. Tämä vastaus tallennetaan välimuistiin, liittyvät haluttuihin määritetyn ylä- ja kyselyparametri mukaan. Seuraavat suosikkiryhmän puhelut toimintoon vastaavia parametreilla on välimuistissa vastauksen palauttaa, kunnes välimuistin kesto väli on vanhentunut.

## <a name="caching-policies"> </a>Tarkista välimuistiin käytännöt

Tässä vaiheessa voit tarkistaa välimuistiasetukset otosten Kaiku API **HAE resurssi (välimuistissa)** -toimintoa.

Kun välimuistiasetukset on määritetty toiminnon **välimuisti** -välilehdessä, välimuistiin käytännöt lisätään toiminnon. Käytännöt voi tarkastella ja muokata käytäntöä-editorissa.

Valitse **käytännöt** **API hallinta** -valikosta vasemmalla puolella ja valitse sitten **Kaiku API / HAE resurssi (välimuistissa)** **toiminnon** avattavasta luettelosta.

![Käytännön laajuus-toiminto][api-management-operation-dropdown]

Tämän toiminnon käytännöt näkyviin käytäntö-editorissa.

![API hallinta käytännön-editori][api-management-policy-editor]

Tämän toiminnon käytännön määritys sisältää käytäntöjä, jotka määrittävät välimuistiin määritys, joka on tarkistanut edellisessä vaiheessa **välimuisti** -välilehdellä.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Käytännön editorissa välimuistiin käytännöt tehdyt muutokset näkyvät myös **välimuisti** -välilehdessä toiminto ja päinvastoin.

## <a name="test-operation"> </a>Soita toiminnon ja testaa välimuistiin

Saat toiminnon välimuistiin, on osallistua toiminnon developer-portaalissa. Valitse yläreunan oikean valikossa **Developer-portaalissa** .

![Developer-portaalissa][api-management-developer-portal-menu]

Valitse **ohjelmointirajapinnan** yläreunan-valikossa ja valitse sitten **Päivitä API**.

![Päivitä Ohjelmointirajapinta][api-management-apis-echo-api]

>Jos sinulla on vain yksi API määritetty tai näkyvissä tiliisi, valitsemalla ohjelmointirajapinnan siirryt suoraan kyseisen API toimintoja.

Valitse **HAE resurssi (välimuistissa)** -toiminto ja valitse sitten **Avaa konsoli**.

![Avaa konsoli][api-management-open-console]

Konsolin voit käynnistää toimintojen suoraan developer-portaalissa.

![Konsolin][api-management-console]

Pidä **param1** ja **param2**oletusarvot.

Valitse haluamasi avain **tilauksen avain** avattavasta luettelosta. Jos tiliisi on vain yksi tilaus, se jo valittuna.

Kirjoita **sampleheader:value1** **pyytää otsikot** teksti-ruutuun.

Valitse **HTTP Get** ja Merkitse muistiin vastauksen otsikon.

Kirjoita **sampleheader:value2** **pyytää otsikot** teksti-ruutuun ja valitse sitten **HTTP Get**.

Huomaa, että **sampleheader** arvo on edelleen **arvo1** vastauksessa. Kokeile joitakin eri arvoja ja Huomaa, että ensimmäinen kutsu välimuistissa vastaus palautetaan.

Kirjoita **25** **param2** -kenttään ja valitse sitten **HTTP Get**.

Huomaa, että **sampleheader** vastauksessa arvo on nyt **arvo2**. Koska toiminnon tulokset ovat liittyvät haluttuihin kyselymerkkijonon mukaan, edellisen välimuistissa vastausta ei palautettu.

## <a name="next-steps"> </a>Seuraavat vaiheet

-   Saat lisätietoja välimuistiin käytännöt [API hallinta käytännön viitata][] [välimuistiin käytännöt][] .
-   Lisätietoja välimuistiin kohteiden avaimella käytännön lausekkeiden avulla on artikkelissa [mukautetun välimuistiin tallentamisen Azure API hallinta](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md

[API hallinta käytännön viitata]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Välimuistin käytännöt]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
