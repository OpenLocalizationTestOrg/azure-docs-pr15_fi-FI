<properties
    pageTitle="Microsoft Azure-tilaus ja palvelun rajoitukset, kiintiön ja rajoitukset"
    description="Sisältää luettelon yleisiä Azure tilaus ja palvelun rajoitukset, kiintiön ja rajoitukset. Tämä vaihtoehto sisältää lisätietoja siitä, miten niin, että rajoitukset sekä suurimmat arvot."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure tilaus ja palvelun rajoitukset, kiintiön ja rajoitukset

## <a name="overview"></a>Yleiskatsaus

Tämän asiakirjan määrittää joitakin yleisimmät Microsoft Azure-rajoitukset. Tämä ei tällä hetkellä käsitellä kaikki Azure-palvelut. Ajan myötä nämä raja-arvot laajennettu ja päivittää koskea ympäristön.

Siirry [Azure hinnat yleiskuvaus](https://azure.microsoft.com/pricing/) lisätietoja Azure hinnat. Siellä voit arvioida [Hinnat Laskimen](https://azure.microsoft.com/pricing/calculator/) käyttäminen kustannukset tai ohjesisältöä hinnoittelu tiedot-sivulla palvelun (esimerkiksi [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Jos haluat korottaa rajan **Oletusarvoinen enimmäismäärä**, voit [avata asiakkaan online-tukipyynnön maksutta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Rajoituksia ei voi korottaa edellä **Enimmäismäärä** -arvo alla olevissa taulukoissa. Jos ei ole **Enimmäismäärä** -saraketta, valitse määritettyä resurssia ei ole säädettävät rajoitukset.

## <a name="limits-and-the-azure-resource-manager"></a>Rajoitukset ja Azure Resurssienhallinta

On nyt mahdollista yhdistää useita Azure resursseja, valitse yksittäinen Azure resurssin ryhmään. Kun Resurssiryhmien käyttäminen rajoitukset, jotka olivat kerran yleinen muuttuvat hallitaan aluekohtaiset tasolla kanssa Azure Resurssienhallinta. Saat lisätietoja Azure resurssiryhmät [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md).

Alla rajoituksia uusi taulukko on lisätty vastaamaan mahdolliset rajoitukset erot käytettäessä Azure Resurssienhallinta. Esimerkiksi ei **Tilauksen rajoitukset** ja **Tilauksen rajat - Azure Resurssienhallinta** -taulukon. Jos rajoitus koskee molempia, se näkyy vain ensimmäisen taulukon. Ellei toisin ilmoiteta rajoitukset ovat Yleinen kaikkien alueiden välillä.

> [AZURE.NOTE] On tärkeää korostamaan, kiintiön resurssien Azure resurssin ryhmien ovat kohden-alueen tilauksen jäsenten käytettävissä ja eivät ole kohden-tilaus, service management kiintiöt ovat. Esimerkkinä käytetään core kiintiön. Sinun on pyydettävä kiintiön Suurenna, sydämiä tukeen, jos haluat päättää, kuinka monta sydämiä haluat käyttää mitä alueilla ja tee summat ja alueet, jotka haluat Azure resurssiryhmä core kiintiön erityinen pyyntö. Tämän vuoksi, jos sinun on 30 sydämiä Länsi Euroopan avulla voit suorittaa sovelluksen siellä, Jos erityisesti pyytää 30 sydämiä Länsi Euroopan. Mutta et voi valita core kiintiön Suurenna muiden alueella – Länsi Europe on 30 core kiintiön.
<!-- -->
Tuloksena voi Etsi kannattaa harkita päätät, mitä Azure resurssiryhmä-kiintiön täytyy olla havainnollistamiseen yhteen-alueelle ja pyydä kunkin alueen, johon harkitset käyttöönoton summa. Artikkelissa lisäohjeita, tutustu oman nykyisen kiintiön tiettyjen alueiden [käyttöönoton vianmääritys](./resource-manager-common-deployment-errors.md) .

## <a name="service-specific-limits"></a>Palvelukohtaisia rajoitukset

- [Active Directory](#active-directory-limits)
- [API hallinta](#api-management-limits)
- [Sovelluksen-palvelu](#app-service-limits)
- [Sovelluksen yhdyskäytävän](#application-gateway-limits)
- [Hakemuksen tiedot](#application-insights-limits)
- [Automaatio](#automation-limits)
- [Azure Redis.txt välimuisti](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Varmuuskopiointi](#backup-limits)
- [Erän](#batch-limits)
- [BizTalk palvelut](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Pilvipalveluihin](#cloud-services-limits)
- [Tietoja Factory](#data-factory-limits)
- [Tietoja järvi Analytics](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Tapahtuman keskittimet](#event-hubs-limits)
- [IoT-toiminnossa](#iot-hub-limits)
- [Avaimen säilö](#key-vault-limits)
- [Media-palveluita](#media-services-limits)
- [Mobiili välitys](#mobile-engagement-limits)
- [Mobiili-palvelut](#mobile-services-limits)
- [Seuranta](#monitoring-limits)
- [Monimenetelmäisen todentamisen](#multi-factor-authentication)
- [Verkko](#networking-limits)
- [Pääsivuston ilmoituspalvelu](#notification-hub-service-limits)
- [Toiminnalliset tiedot](#operational-insights-limits)
- [Resurssiryhmä](#resource-group-limits)
- [Ajoitus](#scheduler-limits)
- [Haku](#search-limits)
- [Palvelun Bus](#service-bus-limits)
- [Sivuston palauttaminen](#site-recovery-limits)
- [SQL-tietokantaan](#sql-database-limits)
- [Tallennustilan](#storage-limits)
- [StorSimple järjestelmän](#storsimple-system-limits)
- [Virta Analytics](#stream-analytics-limits)
- [Tilauksen](#subscription-limits)
- [Liikenteen hallinta](#traffic-manager-limits)
- [Näennäiskoneiden](#virtual-machines-limits)
- [Virtuaalikoneen asteikko joukot](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Tilauksen rajoitukset
#### <a name="subscription-limits"></a>Tilauksen rajoitukset
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Tilauksen rajat - Azure resurssien hallinta

Seuraavat rajoitukset koskevat käytettäessä Azure Resurssienhallinta ja Azure resurssiryhmät. Rajoitukset, joka ei ole muutettu kanssa Azure Resurssienhallinta eivät näy alla. Lisätietoja edellä olevassa taulukossa on rajoitukset.

Katso tietoja käsittelyä koskevat rajoitukset Resurssienhallinta pyynnöt [rajoittimen Resurssienhallinta pyynnöt](resource-manager-request-limits.md).

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Resurssiryhmä-rajoitukset

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Näennäiskoneiden rajoitukset
#### <a name="virtual-machine-limits"></a>Virtuaalikoneen rajoitukset
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Näennäiskoneiden rajat - Azure resurssien hallinta

Seuraavat rajoitukset koskevat käytettäessä Azure Resurssienhallinta ja Azure resurssiryhmät. Rajoitukset, joka ei ole muutettu kanssa Azure Resurssienhallinta eivät näy alla. Lisätietoja edellä olevassa taulukossa on rajoitukset.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Virtuaalikoneen asteikko joukot rajoitukset

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Verkko-rajoitukset

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Verkko-rajoitukset
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Sovelluksen yhdyskäytävän rajoittaa

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Liikenteen hallinta rajoitukset

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-rajoitukset

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Tallennustilan rajoitusten

Lisätietoja tilin tallennustilarajojen on artikkelissa [Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Palvelun tallennustilarajojen

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Virtuaalikoneen levyn rajoitukset

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Saat lisätietoja [virtuaalikoneen koot](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Vakio tallennustilan tilit**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium-tallennustilan tilin**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Tallennustilarajojen resurssin tarjoajaan

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Cloud Services-rajoitukset

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>Sovelluksen palvelun rajoitukset
Seuraavat sovelluksen palvelun rajat sisältävät rajoitukset Web Apps-sovellusten, Mobile-sovellusten, API-sovellukset ja logiikka sovellukset.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Ajoituksen rajoitukset

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Erän rajoitukset

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalk Services rajoitukset
Seuraavassa taulukossa rajojen Azure Biztalk Services.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB rajoitukset

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kiintiön tähteä (*)- [voidaan säätää ottamalla yhteyttä tukipalveluun Azure](./documentdb/documentdb-increase-limits.md)luettelossa.

### <a name="mobile-engagement-limits"></a>Mobile välitys rajoitukset

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Etsi rajoitukset

Hinnoittelu tasoa määrittävät kapasiteetin ja search-palvelun rajoissa. Tasoa ovat seuraavat:

- *Vapaa* usean vuokraajan palvelu, muut Azure tilaajille tarkoitettu arviointi ja pieni järjestämis jaettu.
- *Tavallinen* on erillinen tietojenkäsittely resurssien tuotannon työmääriä pienempi mittakaava, on erittäin käytettävissä kyselyn työmääriä, enintään kolme replikoiden kanssa.
- *Vakio (S1, S2, S3, S3 HD)* on suurempi tuotannon toiminnoista. Tasoja ole sisällä vakio taso niin, että voit valita tietyissä skenaarioissa resurssin konfiguroinnin.

**Tilauskohtaisten rajoitukset**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Etsi-palvelua kohden rajoitukset**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Muut rajoitukset, mukaan lukien asiakirjan koko kyselyiden kohden toisen, avaimet, pyynnöt ja vastaukset eritellympiä lisätietoja artikkelissa [Azure hakutoiminnossa palvelun rajoitukset](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media-palveluiden rajoitukset

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN rajoitukset

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobile-palveluihin rajoitukset

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Seurannan rajoitukset

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Pääsivuston ilmoituspalvelu rajoittaa

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Tapahtuman keskittimet rajoitukset

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Palvelun Bus rajoitukset

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT keskittimeen rajoitukset

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Tietoja Factory rajoitukset

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Tietoja järvi Analytics rajoitukset
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Virta Analytics rajoitukset
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-rajoitukset

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp rajoitukset

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple järjestelmän rajoitukset

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Toiminnallisia havainnollistamisen rajoitukset

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Varmuuskopion rajoitukset

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Sivuston palauttaminen rajoitukset

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Hakemuksen tiedot rajoitukset

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API hallinta rajoitukset

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis välimuistin rajoitukset

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Avaimen säilö rajoitukset

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Monimenetelmäisen todentamisen
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automaatio-rajoitukset
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL-tietokanta-rajoitukset

SQL-tietokantaan rajoituksista on artikkelissa [SQL-tietokannan resurssien rajoitukset](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Katso myös

[Tietoja Azure rajoitukset ja kasvaa](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuaalikoneen ja Azure Cloud palvelun koot](virtual-machines/virtual-machines-linux-sizes.md)

[Koot pilvipalveluihin](cloud-services/cloud-services-sizes-specs.md)
