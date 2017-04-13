<properties
    pageTitle="Erän palvelun kiintiön ja rajoitukset | Microsoft Azure"
    description="Lisätietoja oletusarvon Azure erä kiintiön, rajat ja rajoitukset ja pyytämisestä kiintiön kasvattaa"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kiintiöiden ja Azure erä palvelun rajoitukset

Kun muiden Azure-palvelujen kanssa jakautuvat tiettyjen erä-palveluun liitetyn resurssien rajoitukset. Monet nämä raja-arvot ovat oletusarvon kiintiön käyttää Azure tilaus tai tilin tasolla. Tässä artikkelissa käsitellään näitä oletusasetuksia ja kuinka voit pyytää kiintiön kasvaa.

Jos haluat suorittaa Siirtoerän tuotannon työmääriä, joudut ehkä yhden tai useamman edellä oletusarvo kiintiön Suurenna. Jos haluat korottaa kiintiön, voit avata online- [asiakastuki pyynnön](#increase-a-quota) maksutta.

>[AZURE.NOTE] Kiintiö on luottotietojen raja ei kapasiteetin takaa. Jos sinulla on suurissa kapasiteetin tarpeita, ota yhteyttä tukeen Azure.

## <a name="subscription-quotas"></a>Tilauksen kiintiön
**Resurssi**|**Oletusarvoinen enimmäismäärä**|**Enimmäismäärä**
---|---|---
Erän tilit tilauskohtaisten alueittain | 1 | 50

## <a name="batch-account-quotas"></a>Erän tilin kiintiön
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Muut rajoitukset
**Resurssi**|**Enimmäismäärä**
---|---
Laske-solmu kohti [samanaikaisia tehtäviä](batch-parallel-node-tasks.md) | solmun sydämiä 4 x määrä
[Sovellusten](batch-application-packages.md) erä tiliä kohti        | 20
Sovelluksen pakettien sovellusta kohden  | 40
Sovelluksen paketin koon (kaikki)       | (Arvio) 195 gt<sup>1</sup>

Suurin lohkon Blob-objektien koon <sup>1</sup> azure-tallennustila

## <a name="view-batch-quotas"></a>Näytä erä kiintiön

Erän tili-kiintiön tarkasteleminen [Azure portal][portal].

1. Valitse portaalissa **erä tilit** ja valitse sitten erä-tiliä käyttämällä.

2. Valitse **Ominaisuudet** erä asiakkaan valikon sivu

3. Ominaisuudet-sivu näyttää käytössä erä tilin **kiintiön**

    ![Erän tilin kiintiön][account_quotas]

## <a name="increase-a-quota"></a>Kiintiön Suurenna

Noudata seuraavia ohjeita voit pyytää kiintiön Suurenna [Azure portal][portal].

1. Valitse **Ohje- + tuki** ruutu portaalin Raporttinäkymät-ikkunan tai kysymysmerkkiä (****?) portaalin oikeassa alakulmassa.

2. Valitse **Uusi tue pyyntöä** > **perusteet**.

3. Valitse **Perustiedot** -sivu:

    a. **Ongelmatyyppi** > **Kiintiö**

    b. Valitse tilauksen.

    c-näppäinyhdistelmää. **Kiintiön tyyppi** > **erä**

    d. **Suunnitelman tukevat** > **kiintiön tuki - levy**

    Valitse **Seuraava**.

4. Valitse **ongelma** -sivu:

    a. Valitse **vakavuus** mukaan oman [yrityksen vaikutus][support_sev].

    b. **Tiedot**Määritä kunkin kiintiön, jota haluat muuttaa, erän tilin nimi ja uusi rajoitus.

    Valitse **Seuraava**.

5. Valitse **Yhteystiedot** -sivu:

    a. Valitse **ensisijainen yhteydenottotapa**.

    b. Tarkista ja anna tarvittavat yhteyshenkilön tiedot.

    Valitse **Luo** , jos haluat lähettää tukipyynnön.

Kun olet lähettänyt tukipyynnön, Azure tuki sinuun. Huomaa, että olet suorittanut pyynnön voi kestää jopa 2 työpäivän.

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

* [Azure-portaalissa Azure erä tilin luominen](batch-account-create-portal.md)

* [Azure erän ominaisuuden yhteenveto](batch-api-basics.md)

* [Azure tilaus ja palvelun rajoitukset, kiintiön ja rajoitukset](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
