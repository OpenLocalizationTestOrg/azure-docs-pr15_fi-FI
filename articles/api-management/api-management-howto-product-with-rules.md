<properties
    pageTitle="Suojaa oman API Azure API hallinnan | Microsoft Azure"
    description="Opettele suojaa oman API kiintiön ja rajoitusten (korko rajoittaminen) käytännöt."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Oman API suojaaminen korko rajoitukset Azure API hallinnan avulla

Tässä oppaassa kerrotaan, miten helppoa on oman Taustajärjestelmä API suojauksen lisääminen määrittämällä korko raja- ja kiintiötiedot käytännöt Azure API hallinnan.

Tässä opetusohjelmassa luot "Maksuttoman kokeiluversion" Ohjelmointirajapinta tuote, joka tällä ohjelmalla ohjelmistokehittäjät voivat soittaa enintään 10 minuutissa ja enintään 200 puhelut viikossa oman API [raja puhelun korvauksen tilaus](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja [Määritä käyttökiintiö tilauskohtaisten](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) käytännöt. Sitten Julkaise Ohjelmointirajapinnan ja testaa korko raja-käytäntö.

Monimutkaisemman rajoittava tilanteessa [korko-raja-mukaan-avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kiintiön avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) käytäntöjen avulla on [rajoitin Azure API hallinnan lisäasetukset pyynnön](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Tuotteen luominen

Tässä vaiheessa luodaan maksuttoman kokeiluversion tuote, joka ei edellytä tilauksen hyväksyntää.

>[AZURE.NOTE] Jos määritetty tuote on jo ja käytettävät Tässä opetusohjelmassa, voit siirtyä suoraan [määrittäminen][] Soita korko raja-ja kiintiötiedot ja noudata opetusohjelman käyttämällä tuotteesi tekstin näyttäminen maksuttoman kokeiluversion sieltä.

Aloita valitsemalla **hallinta** Azure Classic-ohjelmassa API Management-palvelun. Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Hallitse oman ensimmäisen Ohjelmointirajapinta Azure Ohjelmointirajapinta hallinnan][] opetusohjelman [Luo erillisen service API hallinta][] .

Valitse **tuotteet** - **Ohjelmointirajapinnan hallinta** -valikossa vasemmalle, jos haluat näyttää **tuotteet** -sivulle.

![Lisää tuote][api-management-add-product]

Valitse **Lisää tuote** , saat **Lisää uusi tuote** -valintaikkunan.

![Lisää uusi tuote][api-management-new-product-window]

Kirjoita **otsikko** -ruutuun **Maksuttoman kokeiluversion käyttäjäksi**.

Kirjoita **kuvaus** -ruutuun seuraava teksti:  **tilaajat voivat suorittaa 10 puhelut/minuutti enintään 200 puhelut/viikon kuluttua käyttö on estetty.**

Tuotteet-Ohjelmointirajapinnan hallinnan voidaan suojata tai avaa. Suojatun tuotteet on tilannut, ennen kuin niitä voidaan käyttää. Avaa tuotteet voidaan ilman tilauksen. Varmista, että **edellyttää tilausta** on valittuna Luo suojatun tuote, joka edellyttää tilauksen. Tämä on oletusarvo.

Jos haluat tarkistaa ja hyväksyä tai hylätä tämän tuotteen tilauksen yrittää järjestelmänvalvoja, valitse **tilauksen hyväksynnän vaatiminen**. Jos valintaruutu ei ole valittuna, tilauksen yritykset on automaattinen hyväksytty. Tässä esimerkissä tilaukset hyväksytään automaattisesti, jotta Älä valitse ruutuun.

Jos Kehitystyökalut-tilien useita kertoja tilaaminen uusi tuote, valitse **Salli samanaikaisesti useita tilauksia** -valintaruutu. Tässä opetusohjelmassa ei käyttämiseen samanaikaisesti useita tilauksia, joten se jätä valitsematta.

Kirjoitetut kaikki arvot valitsemalla Luo tuotteen **Tallenna** .

![Lisätyn tuotteen][api-management-product-added]

Uusien tuotteiden ovat oletusarvoisesti **Järjestelmänvalvojat** -ryhmän käyttäjien nähtävissä. Tarkastellaan Lisää **kehittäjät** -ryhmälle. Valitse **Maksuttoman kokeiluversion**ja valitse **näkyvyys** -välilehti.

>API hallinta-ryhmiä käytetään tuotteet kehittäjille näkyvyyden hallinta. Tuotteiden myöntäminen näkyvyyden ryhmiä ja kehittäjät voivat tarkastella ja tilata tuotteita, jotka ovat näkyvissä ryhmiin, johon ne kuuluvat. Katso lisätietoja, [miten voit luoda ja käyttää ryhmiä Azure API hallinnassa][].

![Lisää kehittäjät-ryhmä][api-management-add-developers-group]

Valitse **kehittäjät** -valintaruutu ja valitse sitten **Tallenna**.

## <a name="add-api"> </a>Tuote Ohjelmointirajapinnan lisääminen

Tässä vaiheessa opetusohjelman on Lisää kaiku Ohjelmointirajapinnan uusi maksuttoman kokeiluversion.

>Kunkin Ohjelmointirajapinta Management-palvelun esiintymän tulee esimääritettyjä Kaiku-ohjelmointirajapinnalla, jonka avulla voidaan kokeilla ja tutustu Ohjelmointirajapinta hallinta. Lisätietoja on kohdassa [Manage oman ensimmäisen Ohjelmointirajapinnan Azure API hallinta][].

Valitse **tuotteet** **API hallinta** -valikosta vasemmalla puolella ja valitse sitten **Maksuttoman kokeiluversion** määrittäminen tuotteen.

![Määritä tuote][api-management-configure-product]

Valitse **Lisää API tuotteeseen**.

![Tuotteen Ohjelmointirajapinnan lisääminen][api-management-add-api]

Valitse **Päivitä API**ja valitse sitten **Tallenna**.

![Lisää kaiku Ohjelmointirajapinta][api-management-add-echo-api]

## <a name="policies"> </a>Puhelun korko raja- ja kiintiötiedot käytäntöjen määrittäminen

Korko rajoitukset ja kiintiön on määritetty käytäntö-editorissa. Valitse **käytännöt** vasemmalla **API hallinta** -valikossa. Valitse **tuoteluettelosta** **Maksuttoman kokeiluversion käyttäjäksi**.

![Tuotteen käytäntö][api-management-product-policy]

Valitse **Lisää käytännön** käytännön mallia ja aloittaa luonnin nopeus raja-ja kiintiötiedot.

![Lisää käytäntö][api-management-add-policy]

Jos haluat lisätä käytäntöjä, Vie kohdistin kohtaan käytännön mallin joko **saapuvat** tai **Lähtevät** -osaan. Korko raja- ja kiintiötiedot käytännöt saapuvien käytäntöjä, sijoita kohdistin siten saapuvien osaan.

![Käytännön-editori][api-management-policy-editor-inbound]

Tässä opetusohjelmassa lisätään kaksi käytännöt on [raja puhelun korvauksen tilaus](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja [Määritä käyttökiintiö tilauskohtaisten](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) käytäntöjä.

![Käytännön lauseet][api-management-limit-policies]

Kun kohdistin sijaitsee **saapuvien** policy-elementin, valitse **raja puhelun korvauksen tilauksen** voit lisätä sen käytännön mallia vieressä olevaa nuolta.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Raja puhelun korvauksen tilauksen** voidaan tuotteen tasolla ja voidaan myös API ja yksittäisiä toiminnon nimi tasoilla. Tässä opetusohjelmassa käytetään vain tuotteen tason käytännöt, poistaa niin **api** ja **toiminta** -elementtien **korko raja** -elementin, vain ulompi **korko raja** elementti jää, kuten seuraavassa esimerkissä.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

Maksuttoman kokeiluversion-suurimman sallitun kutsu on 10 puhelut minuutissa, joten kirjoita **10** **puhelut** -määritteen arvon ja **60** **uusimisen piste** -määritteen.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

**Määritä käyttökiintiö tilauskohtaisten** -käytännön määrittäminen Sijoita kohdistin **saapuvien** elementissä lisätyn **korko raja** -osan alapuolella ja valitse sitten vasemmalla puolella **määrittäminen käyttökiintiö tilauskohtaisten**olevaa nuolta.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Koska käytäntö on myös tarkoitus tuotteen tasolla, poista **api** ja **toiminnon** nimi-osat-seuraavan esimerkin mukaisesti.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kiintiön voi perustua kutsuja aikaväli ja kaistanleveyden kohden. Tässä opetusohjelmassa on ovat ei mukaan Kaistanleveyden rajoittaminen, poista niin **kaistanleveyden** -määrite.

    <quota calls="number" renewal-period="seconds">
    </quota>

Maksuttoman kokeiluversion kiintiö on 200 puhelut viikossa. Määritä **200** **puhelut** -määritteen arvon ja määritä **604800** kuin **uusimisen piste** -määritteen arvon.

    <quota calls="200" renewal-period="604800">
    </quota>

>Käytännön väliajoin määritetään sekunteina. Laskemiseen välin viikon päivien (7) voit kertoa verran tuntia päivässä (24) minuutteina tunti (60) minuutti (60) sekuntien määrän mukaan: 7 *24* 60 * 60 = 604800.

Kun olet tehnyt käytännön määrittäminen, se on vastattava seuraavassa esimerkissä.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Kun haluamasi asetukset on määritetty, valitse **Tallenna**.

![Tallenna käytäntö][api-management-policy-save]

## <a name="publish-product"></a> Julkaista tuotteen

Nyt kun-API lisätään ja käytännöt on määritetty, tuote on julkaistava niin, että se voidaan käyttää kehittäjät. Valitse **tuotteet** **API hallinta** -valikosta vasemmalla puolella ja valitse sitten **Maksuttoman kokeiluversion** määrittäminen tuotteen.

![Määritä tuote][api-management-configure-product]

Valitse **Julkaise**ja valitse sitten Vahvista **Kyllä, julkaisemaan** .

![Julkaise tuote][api-management-publish-product]

## <a name="subscribe-account"> </a>Tilata tuotteen Kehitystyökalut-tili

Nyt kun tuote on julkaistu, se on käytettävissä tilannut ja sovelluskehittäjille.

>Jokaisen tuotteen automaattisesti tilannut järjestelmänvalvojat API hallinta-esiintymän. Tämän opetusohjelman vaiheessa olemme tilaa, maksuttoman kokeiluversion järjestelmänvalvoja Kehitystyökalut-tunnuksilla. Jos Kehitystyökalut-tiliisi kuuluu Järjestelmänvalvojat-roolin, sitten voit noudattaa sekä tämä vaihe, vaikka olet jo tilannut.

Valitse **käyttäjiä** **Ohjelmointirajapinta hallinta** -valikon vasemmalla puolella ja valitse sitten Kehitystyökalut-tilisi nimi. Tässä esimerkissä on käytössä **Clayton Gragg** developer-tili.

![Määritä developer][api-management-configure-developer]

Valitse **Lisää tilaus**.

![Lisää tilaus][api-management-add-subscription-menu]

Valitse **Maksuttoman kokeiluversion**ja valitse sitten **tilaa**.

![Lisää tilaus][api-management-add-subscription]

>[AZURE.NOTE] Tässä opetusohjelmassa samanaikaisesti useita tilauksia eivät ole käytössä maksuttoman kokeiluversion. Siirretyt, jos pyytää sinua nimi tilauksen-seuraavan esimerkin mukaisesti.

![Lisää tilaus][api-management-add-subscription-multiple]

Kun olet napsauttanut **tilaa**, tuotteen näkyy käyttäjän **tilauksen** -luettelossa.

![Tilauksen lisätty][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Voit soittaa toiminnon ja testaa raja kerroin

Maksuttoman kokeiluversion on määritetty ja julkaista, emme kutsu joitakin toimintoja ja testaa korko raja-käytäntö.
Siirry developer-portaalissa valitsemalla **Developer-portaalissa** oikeassa-valikossa.

![Developer-portaalissa][api-management-developer-portal-menu]

Valitse **ohjelmointirajapinnan** yläreunan-valikossa ja valitse sitten **Päivitä API**.

![Developer-portaalissa][api-management-developer-portal-api-menu]

Valitse **HAE resurssi**ja valitse sitten **Kokeile**.

![Avaa konsoli][api-management-open-console]

Oletusasetus parametriarvot ja valitse sitten tilauksen key-tuotetunnuksen maksuttoman kokeiluversion tuotteen.

![Tilauksen avain][api-management-select-key]

>[AZURE.NOTE] Jos sinulla on useita tilauksia, valitse avain **Maksuttoman**kokeiluversion, tai muuten käytäntöjä, jotka on määritetty edellä kuvatut vaiheet eivät ole käytössä.

Valitse **Lähetä**ja tarkastele vastaus. Huomaa **vastauksen tila** **200 OK**.

![Toiminnon tulokset][api-management-http-get-results]

Valitse **Lähetä** suurempi kuin 10 puhelujen minuutissa korko raja-käytäntö korvaus. Sen jälkeen, kun korko raja-käytäntö ylittyy, vastauksen tilaa **429 liian monta** pyyntöjen palautetaan.

![Toiminnon tulokset][api-management-http-get-429]

**Vastauksen sisällön** ilmaisee jäljellä olevan aikavälin, ennen kuin uudelleenyritykset onnistuu.

Kun 10 puhelujen minuutissa korko raja-käytäntö on käytössä, seuraavat suosikkiryhmän puhelut epäonnistuvat, kunnes 60 sekuntia on kulunut ensimmäisestä 10 onnistuneen puhelujen tuotteen ennen raja kerroin ylitettiin. Tässä esimerkissä jäljellä olevan aikavälin on 54 sekuntia.

## <a name="next-steps"> </a>Seuraavat vaiheet

-   Katso, miten korko rajoitukset ja kiintiöiden seuraavassa videossa esittely.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Hallitse oman ensimmäisen Ohjelmointirajapinnan Azure API hallinta]: api-management-get-started.md
[Voit luoda ja käyttää ryhmiä Azure API hallinta]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Puhelun korko raja- ja kiintiötiedot käytäntöjen määrittäminen]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
