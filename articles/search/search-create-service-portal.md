<properties
    pageTitle="Luo Azure-portaalissa Azure Search-palvelun | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Opettele valmistella Azure-portaalissa Azure Search-palvelun."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Azure-portaalissa Azure Search-palvelun luonti

Tämän oppaan edetään ohjatusti luominen (tai valmistelu) [Azure Portal](https://portal.azure.com/)Azure Search-palvelun.

Tämän oppaan olettaa jo ole Azure-tilausta ja voit kirjautua Azure-portaaliin.

## <a name="find-azure-search-in-the-azure-portal"></a>Etsi Azure haun Azure-portaalissa
1. Siirry [Azure-portaalin](https://portal.azure.com/) ja kirjaudu sisään.
1. Valitse Valitse plus-merkkiä ("+") vasemman yläkulman.
2. Valitse **tietojen + tallennustilan**.
3. Valitse **Azure Etsi**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Valitse palvelunimi ja URL-Osoitteen päätepisteen palvelun
1. Palvelunimi olla osa Azure Search-palvelun päätepisteen URL-osoite, jos vastaan, jotka teet API-kutsujen hakupalvelun käyttämisestä ja hallinta.
2. Kirjoita nimesi palvelun **URL-osoite** -kenttään. Palvelunimi:
  * saa sisältää vain pieniä kirjaimia, numeroita tai katkoviivat ("-")
  * ei voi käyttää yhdysmerkin ("-") ensin 2 merkkiä eli viimeinen merkki
  * ei voi olla peräkkäisiä katkoviivat ("--")
  * on rajoitettu 2 ja 60 merkkiä välillä


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Valitse tilaus, jossa säilytät palvelussa
Jos sinulla on useita tilauksia, voit valita, kumpaa sisällytetään Azure-hakupalvelun.

## <a name="select-a-resource-group-for-your-service"></a>Valitse palvelun resurssiryhmä
Luo uusi resurssiryhmä tai valitse olemassa. Resurssiryhmä on kokoelma Azure palvelut ja resurssit, joita käytetään yhdessä. Esimerkiksi jos käytät Azure haun indeksoida SQL-tietokantaan, sitten molempia näistä palveluista pitäisi olla osa samaan resurssiryhmä.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Valitse sijainti, jossa palvelun isännöidään
Azure palveluna Azure haku on käytettävissä isännöidään palvelinkeskusten eri puolilla maailmaa. Huomaa, että [hinnat voivat olla erilaiset](https://azure.microsoft.com/pricing/details/search/) geography.

## <a name="select-your-pricing-tier"></a>Valitse hinnoittelutiedot taso
[Azure haku on tällä hetkellä tarjoaa useita hinnoittelu tasoa](https://azure.microsoft.com/pricing/details/search/): vapaa, Basic tai vakio. Kunkin tason on oma [kapasiteetin ja rajoitukset](search-limits-quotas-capacity.md). Artikkelissa [hinnoittelu taso tai SKU](search-sku-tier.md) ohjeita.

Tässä tapauksessa Microsoft on valinnut meidän service Standard taso.

## <a name="select-the-create-button-to-provision-your-service"></a>Valitse palvelun valmisteleminen "Luo"-painike

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Skaalaa-palvelussa

Kun palvelussa on valmisteltu, voit skaalata vastaamaan omia tarpeita. Jos olet valinnut vakio taso Azure-hakupalvelun voi skaalata palvelun kaksi mitat: replikoita ja osiot. Jos olet valinnut Basic taso, voit lisätä vain replikoita.

*__Osioiden__* Salli tallentamiseen ja etsiä useampia tiedostoja palveluun.

*__Replikoiden__* Salli käsittelemään suurempi kuormitus, hakukyselyt - [palvelu edellyttää 2 replikoiden saavuttamiseksi vain luku - SLA ja edellyttää 3 replikoiden saavuttamiseksi luku-/ kirjoitusoikeudet SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)palvelun.

1. Siirry Azure Search-palvelun hallinta-sivu Azure-portaalissa.
2. Valitse **asetukset** -sivu **asteikko**.
3. Palvelun skaalata lisäämällä replikoiden tai osiot.
  * Palvelun aiempia 36 haun yksiköt et voi skaalata. Etsi yksiköiden kokonaismäärän tuotetta replikoita ja osiot (replikoiden * osiot = yhteensä haun yksiköt).
  * Jos olet valinnut Basic taso, voit vain skaalata 3 replikoihin. Basic-palveluja on sidottu osio.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Seuraava
Valmistelun Azure-hakupalvelun jälkeen voi aloittaa [Määritä Azure-hakuindeksin](search-what-is-an-index.md) , jotta voit ladata ja tietojen hakua.

Saat nopean opetusohjelma [Azure haku-portaalin käytön aloittaminen](search-get-started-portal.md) .
