<properties 
    pageTitle="Azure Redis välimuistin määrittämisestä | Microsoft Azure"
    description="Opettele määrittämään Azure Redis välimuistin esiintymät ja osaat Azure Redis välimuistin oletusarvon mukainen Redis.txt määritys"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Azure Redis välimuistin määrittäminen

Tässä ohjeaiheessa kerrotaan, miten voit tarkistaa ja päivittää Azure Redis välimuistin esiintymien määritykset ja Azure Redis välimuistin esiintymät palvelinkokoonpanon Redis.txt käsitellään.

>[AZURE.NOTE] Katso lisätietoja määrittämisestä ja käyttämisestä välimuistin erikoisominaisuuksia, [Premium Azure Redis välimuistin pysyvyyden määrittämisestä](cache-how-to-premium-persistence.md), [käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittämisestä](cache-how-to-premium-clustering.md)ja [Premium Azure Redis välimuistin VPN-tuki määrittämisestä](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Redis.txt tulostusvälimuistiasetusten määrittäminen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis välimuistin tarjoaa seuraavia asetuksia **asetukset** -sivu.

![Redis välimuistiasetukset](./media/cache-configure/redis-cache-settings.png)



-   [Tuen ja asetusten vianmääritys](#support-amp-troubleshooting-settings)
-   [Yleiset asetukset](#general-settings)
    -   [Ominaisuudet:](#properties)
    -   [Pikanäppäimet](#access-keys)
    -   [Lisäasetukset](#advanced-settings)
    -   [Redis välimuistin tukipalvelu](#redis-cache-advisor)
-   [Skaalausasetukset](#scale-settings)
    -   [Hinnat taso](#pricing-tier)
    -   [Redis klusteri](#cluster-size)
-   [Tietojen hallinta-asetukset](#data-management-settings)
    -   [Tietojen pysyvyyttä Redis](#redis-data-persistence)
    -   [Tuo tai Vie](#importexport)
-   [Hallinta-asetukset](#administration-settings)
    -   [Käynnistä tietokone uudelleen](#reboot)
    -   [Päivitysten ajoittaminen](#schedule-updates)
-   [Diagnostiikan asetusten](#diagnostics-settings)
-   [Verkkoasetukset](#network-settings)
-   [Resurssien hallinnan asetukset](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Tuen ja asetusten vianmääritys

- **Tuki + vianmääritys** -osan asetuksia antaa sinulle välimuistin ongelmat ratkaisuvaihtoehdot.

![Tuki + vianmääritys](./media/cache-configure/redis-cache-support-troubleshooting.png)

Valitse **diagnosointi ja ratkaiseminen** on annettava yleisiä ongelmia ja strategioita ratkaisemista varten.

Valitse **tehtävän log** tarkastelemaan toiminnot suoritetaan välimuistin. Voit käyttää suodatusta Laajenna tämä näkymä sisältää muita resursseja. Lisätietoja valvontalokien käsittelemisestä on artikkelissa [tapahtumien tarkasteleminen ja valvontalokit](../monitoring-and-diagnostics/insights-debugging-with-events.md) ja [valvonta toimintojen resurssien hallinta](../resource-group-audit.md). Lisätietoja Azure Redis välimuistin tapahtumien seuranta-kohdassa [Toiminnot ja ilmoitukset](cache-how-to-monitor.md#operations-and-alerts).

**Resurssin kunto** seuraa resurssi ja ilmoittaa, jos se toimii odotetulla tavalla. Saat lisätietoja Azure resurssin kunto-palvelun [Azure-kunto Resurssikatsaus](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] Resurssin kuntoa ei voi raportin Azure Redis välimuistin esiintymät ylläpidettävä virtual verkon toimintakunnosta. Lisätietoja on artikkelissa [tehdä kaikki välimuistin ominaisuudet toimivat kun isännöivän VNET-välimuistin?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Valitse Avaa välimuistin tukipyynnön **Uusi tukevat pyynnön** .

## <a name="general-settings"></a>Yleiset asetukset

**Yleiset** -osan asetusten avulla voit käyttää ja määrittää välimuistin seuraavia asetuksia.

![Yleiset asetukset](./media/cache-configure/redis-cache-general-settings.png)

-   [Ominaisuudet:](#properties)
-   [Pikanäppäimet](#access-keys)
-   [Lisäasetukset](#advanced-settings)
-   [Redis välimuistin tukipalvelu](#redis-cache-advisor)

### <a name="properties"></a>Ominaisuudet:

Valitse **Ominaisuudet** liittyviä tietoja välimuistin, kuten välimuistin päätepiste ja portit.

![Redis välimuistin ominaisuudet](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Pikanäppäimet

Valitse **pikanäppäinten** tarkastelemaan tai luo välimuistin pikanäppäimet. Painettavat näppäimet käytetään välimuistin muodostaminen asiakkaille sekä isäntänimi ja portit- **Ominaisuudet** -sivu.

![Redis välimuistin pikanäppäimet](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Lisäasetukset

Seuraavat asetukset ovat määrittämistä **Lisäasetukset** -sivu.

-   [Access-portit](#access-ports)
-   [Maxmemory käytännön ja maxmemory varattu](#maxmemory-policy-and-maxmemory-reserved)
-   [Keyspace ilmoitukset (Lisäasetukset)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Access-portit

Oletusarvon mukaan-SSL access ei ole käytettävissä uusi tallentaa. SSL portin käyttöön valitsemalla **ei** **Salli käyttöoikeus vain kautta SSL** - **Asetukset-sivu** ja valitse sitten **Tallenna**.

![Redis.txt välimuistin Access-portit](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Maxmemory käytännön ja maxmemory varattu

**Lisäasetukset** -sivu **Maxmemory käytäntö** ja **maxmemory varattu** asetuksia määrittää välimuistin muistin-käytännöt. **Maxmemory -** asetus määrittää eviction käytännön välimuistin ja **maxmemory varattu** määrittää välimuisti prosessien varattu muistia.

![Redis välimuistin Maxmemory käytäntö](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory käytännön** avulla seuraavat eviction käytännöt sopivat.

-   volatile-lru – tämä on oletusarvo.
-   allkeys lru
-   volatile satunnaisia
-   allkeys satunnaisia
-   volatile ttl
-   noeviction

Saat lisätietoja maxmemory käytännöt [Eviction käytännöt](http://redis.io/topics/lru-cache#eviction-policies).

**Maxmemory varattu** -asetus määrittää muistin määrän megatavuina on varattu välimuisti toimintoja, kuten replikoinnin aikana automaattisesti. Se voidaan myös, kun käytössä on suuren pirstoutuminen suhde. Jos tämä arvo antaa on entistä yhdenmukaisemmat Redis.txt-palvelimen versio, kun että latautuvat vaihtelee. Tämä arvo on asetettava suurempi toiminnoista, jotka ovat kirjoittaa näkyvä. Kun muistia on varattu toimintoja ei ole käytettävissä tallentaminen välimuistiin tallennetut tiedot.

>[AZURE.IMPORTANT] **Maxmemory varattu** -asetus on käytettävissä Standard vain ja Premium välimuistiin.

### <a name="keyspace-notifications-advanced-settings"></a>Keyspace ilmoitukset (Lisäasetukset)

Redis keyspace ilmoitukset on määritetty **Lisäasetukset** -sivu. Keyspace ilmoitukset Salli asiakkaat voivat saada ilmoituksen, kun tiettyjen tapahtumien yhteydessä.

![Redis välimuistin Lisäasetukset](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace ilmoitukset ja **Ilmoita-keyspace-tapahtumat** -asetukset ovat käytettävissä Standard vain ja Premium välimuistiin.

Lisätietoja on artikkelissa [Redis Keyspace ilmoitukset](http://redis.io/topics/notifications). Lisätietoja käyttävistä [Hei](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) otoksessa [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) -tiedosto.

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis välimuistin tukipalvelu

**Suositukset** -sivu näyttää välimuistin suosituksia. Aikana tavanomainen toiminta suosituksia ei ole saatavilla ovat näkyvissä. 

![Suosituksia](./media/cache-configure/redis-cache-no-recommendations.png)

Jos toimintoja, kuten suuri muistinkäytön, kaistanleveys tai palvelimen kuormituksen välimuistin aikana ilmenee ehdot-ilmoituksen näkyy **Redis välimuisti** -sivu.

![Suosituksia](./media/cache-configure/redis-cache-recommendations-alert.png)

Lisätietoja löytyy **suositukset** -sivu.

![Suosituksia](./media/cache-configure/redis-cache-recommendations.png)

Voit valvoa nämä arvot [Seuranta-kaaviot](cache-how-to-monitor.md#monitoring-charts) ja [kaaviot käyttö](cache-how-to-monitor.md#usage-charts) osissa **Redis välimuisti** -sivu.

Kunkin hinnoittelu taso on eri rajoitukset asiakasyhteydet, muistin ja kaistanleveys. Jos välimuistin lähestyy nämä arvot suurin kapasiteetin kestävä ajan kuluessa, suositus luodaan. Saat lisätietoja arvot ja läpi **suositukset** -työkalun rajoitukset seuraavasta taulukosta.

| Redis välimuistin metrijärjestelmä      | Lisätietoja on artikkelissa                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Verkon kaistanleveyden käytön | [Välimuistin suorituskyky – kaistanleveyttä](cache-faq.md#cache-performance) |
| Asiakkaiden       | [Oletusarvoinen Redis.txt palvelinkokoonpanon - maxclients](#maxclients)            |
| Lataa palvelimeen             | [Käyttö kaaviot - Redis kuormitus](cache-how-to-monitor.md#usage-charts)  |
| Muistinkäyttö            | [Välimuistin suorituskyky – kokoa](cache-faq.md#cache-performance)                |

Päivitä välimuistin valitsemalla [hinnat taso](#pricing-tier) ja skaalata välimuistin **Päivitä heti** . Katso lisätietoja valitsemalla hinnoittelu taso [mitä Redis.txt välimuistin tarjoaa ja koko kannattaa käyttää?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Skaalausasetukset

**Skaalaus** -osan asetusten avulla voit käyttää ja määrittää välimuistin seuraavat asetukset.

![Verkon](./media/cache-configure/redis-cache-scale.png)

-   [Hinnat taso](#pricing-tier)
-   [Redis klusteri](#cluster-size)

### <a name="pricing-tier"></a>Hinnat taso

Valitse **hinnoittelu taso** tarkastelua tai muuttamista varten välimuistin hinnoittelu taso. Lisätietoja Skaalaus-kohdassa [asteikko Azure Redis välimuistin poistamisesta](cache-how-to-scale.md).

![Redis välimuistin hinnat taso](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Redis klusteri

Valitse muutettava käynnissä klusteri **(Ennakkoversio) Redis klusteri** premium välimuistin asentaminen käytössä.

>[AZURE.NOTE] Huomaa, että kun Azure Redis välimuistin Premium taso on julkaistu yleiseen käyttöön Redis klusteri-ominaisuus ei tällä hetkellä esikatselussa.

![Redis klusteri](./media/cache-configure/redis-cache-redis-cluster-size.png)

Klusterin koon muuttaminen liukusäätimellä tai kirjoittamalla luvun väliltä 1 – 10 **Shard määrä** -tekstiruutuun ja valitse **OK** , jos haluat tallentaa.

>[AZURE.IMPORTANT] Redis.txt klusterointi on käytettävissä vain Premium tallentaa. Lisätietoja on artikkelissa [käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittämisestä](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Tietojen hallinta-asetukset

**Tietojen hallinta** -osassa asetusten avulla voit käyttää ja määrittää välimuistin seuraavia asetuksia.

![Tietojen hallinta](./media/cache-configure/redis-cache-data-management.png)

-   [Tietojen pysyvyyttä Redis](#redis-data-persistence)
-   [Tuo tai Vie](#importexport)

### <a name="redis-data-persistence"></a>Tietojen pysyvyyttä Redis

Valitse käyttöön, poistaminen käytöstä tai määrittäminen premium välimuistin tiedot pysyvyyden **Redis tietojen pysyvyyttä** .

![Tietojen pysyvyyttä Redis](./media/cache-configure/redis-cache-persistence-settings.png)

Redis.txt pysyvyyttä valitsemalla käyttöön RDB (Redis.txt tietokannan) varmuuskopion **käytössä** . Poista käytöstä Redis.txt pysyvyyttä valitsemalla **ei käytössä**.

Varmuuskopion välin määrittämiseen avattavasta luettelosta **Varmuuskopion korkojakso** . Vaihtoehtoja ovat **15 minuuttia**, **30 minuuttia**, **60 minuuttia**, **6 tuntia**, **12 tuntia**ja **24 tuntia**. Tällä aikavälillä käynnistyy laskien, kun edellisen varmuuskopioinnin onnistuu ja kun se kuluu uuden varmuuskopion on aloitettu.

Valitsemalla **Tallennustilan tilin** tallennustilan tili, jota haluat käyttää, ja valitse joko **perusavain** tai **toissijaisen avaimen** käyttää **Tallennustilan avain** avattavasta luettelosta. Sinun on valittava tallennustilan tilin samalla alueella kuin välimuistin ja **Premium-tallennustilan** tilin on suositeltavaa, sillä premium tallennustila on suurempi siirtonopeuden. Milloin tahansa pysyvyyttä tilin tallennustilan-avain luodaan uudelleen, sinun on valittava haluamaasi näppäintä uudelleen **Tallennustilan avain** avattavasta luettelosta.

Valitse **OK** , jos haluat tallentaa pysyvyyttä kokoonpanon.

>[AZURE.IMPORTANT] Redis.txt tietojen pysyvyyttä on käytettävissä vain Premium tallentaa. Lisätietoja on artikkelissa [Premium Azure Redis välimuistin pysyvyyden määrittämisestä](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Tuo tai Vie

Tuo tai Vie on Azure Redis välimuistin data management toimintoa, joka mahdollistaa Azure Redis välimuistin tietojen tuominen tai vieminen Azure Redis välimuistin tietojen tuomisesta ja viemisestä Redis välimuistin tietokannan (RDB) tilannevedoksen premium välimuistista sivun Blob-objektien Azure-tallennustilan tilin. Näin voit siirtää Azure Redis välimuistin eri esiintymien välillä tai täyttää välimuistin tietoja ennen käyttöä.

Tuo avulla voidaan tuoda Redis.txt yhteensopiva RDB tiedostot minkä tahansa Redis.txt palvelimeen cloud tai ympäristössä, mukaan lukien Redis.txt Linux tai minkä tahansa cloud-palvelun Amazon Web Services ja muiden Windows-käyttöjärjestelmässä. Tietojen tuonti on helppo tapa luoda välimuistin täyttää tiedoilla. Tuonnin aikana Azure Redis välimuistin RDB tiedostoja ladataan Azure tallennustilan muistiin ja lisää sitten välimuistiin näppäimet.

Vie voit viedä tiedot tallennetaan Azure Redis välimuistin Redis yhteensopiva RDB tiedostot. Voit käyttää tätä toimintoa voit siirtää tietoja Azure Redis välimuistin esiintymästä toiseen tai johonkin muuhun Redis.txt palvelimeen. Tilapäinen tiedosto luodaan AM viemisen aikana, joka isännöi Azure Redis välimuistin server-esiintymän ja tiedosto ladataan nimettyjen tallennustilan-tilille. Kun vienti on valmis joko tila onnistumisen tai virhe, tilapäinen tiedosto poistetaan.

>[AZURE.IMPORTANT] Tuo tai Vie on käytettävissä vain Premium taso tallentaa. Saat lisätietoja ja ohjeita [Azure Redis välimuistin tietojen tuominen ja vieminen](cache-how-to-import-export-data.md).


## <a name="administration-settings"></a>Hallinta-asetukset

Valitse **hallinta** -osasta asetusten avulla voit suorittaa seuraavat hallintatehtävät premium välimuistin. 

![Hallinta](./media/cache-configure/redis-cache-administration.png)

-   [Käynnistä tietokone uudelleen](#reboot)
-   [Päivitysten ajoittaminen](#schedule-updates)

>[AZURE.IMPORTANT] Tämän osion asetukset ovat käytettävissä vain Premium taso tallentaa.

### <a name="reboot"></a>Käynnistä tietokone uudelleen

**Käynnistä uudelleen** -sivu antaa vähintään yksi solmut välimuistin uudelleen. Voit testata vikasietoisuudelle, jos sovellus.

![Käynnistä tietokone uudelleen](./media/cache-configure/redis-cache-reboot.png)

Jos sinulla on premium-välimuistin asentaminen käytössä, voit valita mitkä shards välimuistin käynnistää uudelleen.

![Käynnistä tietokone uudelleen](./media/cache-configure/redis-cache-reboot-cluster.png)

Uudelleen yhden tai useamman solmut välimuistin, valitse haluamasi solmut ja valitse **Käynnistä tietokone uudelleen**. Jos sinulla on premium-välimuistin asentaminen otettu käyttöön, valitse shard(s) Käynnistä uudelleen ja valitse sitten **uudelleen**. Muutaman minuutin kuluttua valitun solmut uudelleen ja ovat takaisin online-muutama minuutti myöhemmin.

>[AZURE.IMPORTANT] Käynnistä on käytettävissä vain Premium taso tallentaa. Saat lisätietoja ja ohjeita [Azure Redis välimuistin hallinnan - uudelleen](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Päivitysten ajoittaminen

**Päivitysten ajoittaminen** -sivu avulla voit määrittää ylläpito-ikkuna, jossa välimuistin Redis.txt server-päivitykset. 

>[AZURE.IMPORTANT] Huomaa, että ylläpitovalintaikkunassa koskee vain Redis palvelimen päivitykset- ja minkä tahansa Azure ei voi päivittää tai päivitykset, jotka isännöivät välimuistin VMs käyttöjärjestelmä.

![Päivitysten ajoittaminen](./media/cache-configure/redis-schedule-updates.png)

Voit määrittää ylläpito-ikkuna, tarkista haluamasi päivän ja määritä kunkin ylläpito ikkunan aloituspäivämäärä ja valitse **OK**. Huomaa, että ylläpito ikkunan aika on UTC-muodossa. 

>[AZURE.IMPORTANT] Ajoita päivitykset on käytettävissä vain Premium taso tallentaa. Saat lisätietoja ja ohjeita [Azure Redis välimuistin hallinnan - päivitysten ajoittaminen](cache-administration.md#schedule-updates).

## <a name="diagnostics-settings"></a>Diagnostiikan asetusten

**Diagnostiikka** -osan avulla voit määrittää diagnostiikka Redis välimuistin.

![Vianmääritys](./media/cache-configure/redis-cache-diagnostics.png)

Valitse **Diagnostiikka** [määrittäminen tallennustilan](cache-how-to-monitor.md#enable-cache-diagnostics) välimuistin diagnostiikka tallentamiseen.

![Redis välimuistin diagnostiikka](./media/cache-configure/redis-cache-diagnostics-settings.png)

[Näytä arvot](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) välimuistin ja **ilmoitusten säännöt** [ilmoitusten sääntöjen](cache-how-to-monitor.md#operations-and-alerts)määrittäminen valitsemalla **Redis arvot** .

Lisätietoja Azure Redis välimuistin diagnostiikka Katso, [miten voit valvoa Azure Redis välimuistin](cache-how-to-monitor.md).


## <a name="network-settings"></a>Verkkoasetukset

**Verkko** -osan asetusten avulla voit käyttää ja määrittää välimuistin seuraavia asetuksia.

![Verkon](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] VPN-asetukset ovat käytettävissä, joka on määritetty premium tallentaa vain VNET tukeen välimuistin luonnin aikana. Lisätietoja luomalla premium-välimuistin VNET tukevat ja päivitetään sen asetuksia, katso, [miten voit määrittää Virtual verkkotuen Premium Azure Redis välimuistin](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Resurssien hallinnan asetukset

![Resurssienhallinta](./media/cache-configure/redis-cache-resource-management.png)

**Tunnisteet** -osan avulla voit järjestää resurssit. Lisätietoja [käyttäminen tunnisteen, kun haluat järjestää Azure resurssit](../resource-group-using-tags.md).

**Lukitsee** -osan avulla voit tilaus, resurssiryhmä tai resurssi estää muita käyttäjiä organisaation poistetaan vahingossa tai muokkaamasta kriittinen resurssit. Lisätietoja on artikkelissa [Lukitse resurssien Azure resurssien hallinta](../resource-group-lock-resources.md).

**Käyttäjät** -osiossa tukee Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure portaalin auttaa organisaatioita täyttävät niiden access vaatimuksia yksinkertaisesti ja tarkasti. Lisätietoja on artikkelissa [Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa](../active-directory/role-based-access-control-configure.md).

Valitse **vietävä malli** voivat laatia ja tulevien versioiden käyttöön resurssien mallin vieminen. Saat lisätietoja mallien käyttämisestä [käyttöönotto resurssien Azure Resurssienhallinta malleja](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Oletusarvoinen Redis.txt server-määritys

Azure Redis välimuistin uusia esiintymiä on määritetty seuraavat Redis.txt määritysten oletusarvot.

>[AZURE.NOTE] Tässä osassa asetuksia ei voi muuttaa käyttämällä `StackExchange.Redis.IServer.ConfigSet` menetelmää. Jos tämä menetelmä kutsutaan jompikumpi tämän osion komentoja, virhe ilmenee poikkeuksen seuraavankaltaiselta:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Määritettävät **max-muistia-käytäntö**, kuten olevat arvot ovat määritettävissä Azure portal tai komentorivin hallintatyökalut, kuten Azure CLI tai PowerShellin kautta.

|Asetus|Oletusarvo|Kuvaus|
|---|---|---|
|tietokannat|16|Tietokantojen oletusmäärän on 16, mutta voit määrittää eri numeron perusteella hinnoittelu taso. <sup>1</sup> oletusarvo-tietokanta on DB 0, voit valita jonkin muun yhteyden peruste käyttämisestä `connection.GetDatabase(dbid)` missä DBID-arvo on luku `0` ja `databases - 1`.|
|maxclients|Määräytyy hinnoittelu tason<sup>2</sup>|Tämä on asiakkaiden samanaikaisesti sallittu enimmäismäärä. Kun on saavutettu Redis.txt Sulje uudet yhteydet lähettäminen virhe "enimmäismäärä on saavutettu asiakkaiden".|
|maxmemory käytäntö|volatile lru|Maxmemory käytäntö on asetus, miten Redis.txt valitsee mitä voit poistaa kun maxmemory (ojentamassa luotaessa välimuistin valitussa välimuistin kokoa) on saavutettu. Azure Redis välimuistin oletusarvo on volatile lru, joka poistaa näppäimet vanhentumispäivämäärä määrittäminen LRU-algoritmin avulla. Tämä asetus on määritetty Azure-portaalissa. Lisätietoja on artikkelissa [Maxmemory käytännön ja maxmemory varattu](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-objektit|3|LRU ja mahdollisimman vähän TTL algoritmit eivät ole täsmällinen algoritmit mutta arvioida algoritmit (Jos haluat tallentaa muistin), joten voit valita myös Tarkista otoksen suuruus. Esimerkiksi oletusarvon Redis.txt Tarkista näppäimistä ja valitse haluamasi, jota käytettiin vähemmän viimeksi.|
|lua-määräajan|5 000|Maks suoritusaika Lua komentosarjan millisekunteina. Jos suurin suoritusaika saavutetaan Redis.txt kirjautua, komentosarja on yhä suorittamisen jälkeen suurin sallittu aika ja vastata kyselyihin ja järjestelmä antaa virheilmoituksen käynnistyy.|
|lua-tapahtuma-rajoita|500|Tämä on komentosarjan tapahtumajonon enimmäiskoon.|
|Asiakas-tulostus-puskurin-limit normalclient-tulostus-puskurin-limit pubsub|0 0 032 Mt 8 Mt 60|Asiakkaan tulosteen puskurin raja-arvot voidaan pakottaa asiakkaat, jotka ovat ei tietojen lukeminen tarpeeksi nopeasti palvelimesta jostain syystä (yleinen syy on kirja ja Sub-asiakas ei voi tarjoaman julkaisijan tuottamaan niitä nopeasti viestit) on katkennut. Lisätietoja on artikkelissa [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a>
<sup>1</sup> Rajoitus `databases` on erilainen jokaiselle Azure Redis välimuistin hinnat taso ja voidaan asettaa välimuistin luominen. Jos `databases` asetus on määritetty välimuistin luonnin aikana oletusarvo on 16.

-   Perus- ja Standard tallentaa
    -   C0 (250 Mt) välimuisti - 16 tietokantojen ylöspäin
    -   C1 (1 gt) välimuisti - 16 tietokantojen ylöspäin
    -   C2 (2,5 Gigatavua) välimuisti - 16 tietokantojen ylöspäin
    -   C3 (6 gt) välimuisti - 16 tietokantojen ylöspäin
    -   C4 (13 gt) välimuisti - enintään 32 tietokannat
    -   C5 (26 gt) välimuisti - 48 tietokantojen ylöspäin
    -   C6 (53 gt) välimuisti - enintään 64 tietokannat
-   Premium välimuistiin
    -   16 tietokantojen ylöspäin (6 Gigatavua - 60 gt) - P1
    -   P2 (13 gt - 130 gt) – 32 tietokantojen ylöspäin
    -   P3 (26 gt - 260 gt) - 48 tietokantojen ylöspäin
    -   P4 (53 gt - 530 gt) - enintään 64 tietokannat
    -   Kaikkien kanssa Redis.txt klusterin käytössä - premium tallentaa Redis.txt klusterin tukee vain tietokannan 0 käyttäminen jotta `databases` rajoittaa minkä tahansa premium välimuistin kanssa Redis.txt klusterin käytössä on 1 ja [Select](http://redis.io/commands/select) -komento ei ole sallittu. Lisätietoja on artikkelissa [minun täytyy mahdolliset muutokset Omat asiakassovellukseen käyttämään klusterointi?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] `databases` Asetus voi olla määritetty vain välimuistin luonnin aikana ja vain PowerShell, CLI tai muiden hallinta asiakkaiden avulla. Esimerkki määrittäminen `databases` välimuistin luotaessa PowerShellin avulla on [Uusi AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` on erilainen jokaiselle Azure Redis välimuistin hinnat taso.

-   Perus- ja Standard tallentaa
    -   C0 (250 Mt) välimuisti - enintään 256 yhteydet
    -   C1 (1 gt) välimuisti - enintään 1 000 yhteydet
    -   C2 (2,5 Gigatavua) välimuisti - enintään 2 000 yhteydet
    -   C3 (6 gt) välimuisti - enintään 5 000 yhteydet
    -   C4 (13 gt) välimuisti - enintään 10 000 yhteydet
    -   C5 (26 gt) välimuisti - korkeintaan 15 000 yhteydet
    -   C6 (53 gt) välimuisti - enintään 20 000 yhteydet
-   Premium välimuistiin
    -   7,500 yhteydet ylöspäin (6 Gigatavua - 60 gt) - P1
    -   P2 (13 gt - 130 gt) - korkeintaan 15 000 yhteydet
    -   P3 (26 gt - 260 gt) - enintään 30 000 yhteydet
    -   P4 (53 gt - 530 gt) – 40 000 yhteydet ylöspäin

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis komennot Azure Redis välimuistia ei tueta

>[AZURE.IMPORTANT] Koska Microsoft hallitsee määrittäminen ja hallinta Azure Redis välimuistin esiintymien seuraavat komennot eivät ole käytettävissä. Jos yrität käynnistää ne näyttöön tulee virhesanoma, joka muistuttaa `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  CONFIG
>-  KORJAAMINEN
>-  SIIRTÄÄ
>-  TALLENNA
>-  SULKEMINEN
>-  SLAVEOF
>-  KLUSTERIN - klusterin kirjoittaminen komennot eivät ole käytettävissä, mutta vain luku-klusterin komennot on sallittua.

Saat lisätietoja Redis.txt komennot [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis konsoli

Voit turvallisesti myöntää komentoja käyttämällä **Redis konsolin**, joka on käytettävissä Standard Azure Redis välimuistin kopioita ja Premium välimuistiin.

>[AZURE.IMPORTANT] Redis-konsolin ei toimi VNET, klusterointi, ja muut kuin 0-tietokannat. 
>
>-  [VNET](cache-how-to-premium-vnet.md) – kun välimuistin on osa VNET vain VNET asiakkaat voivat käyttää välimuistin. Koska Redis-konsolin käyttää isännöimät VMs, jotka eivät kuulu oman VNET Redis.txt cli.exe asiakas, sitä ei voi muodostaa välimuistin.
>-  [Clustering](cache-how-to-premium-clustering.md) - Redis konsoli käyttää Redis.txt cli.exe-asiakas, joka ei tue klusterointi tällä hetkellä. [Epästabiilien](http://redis.io/download) haaran Redis.txt tietovaraston osoitteessa GitHub Redis.txt cli-apuohjelman toteuttaa basic tuki, kun aloittaminen `-c` vaihtaminen. Katso lisätietoja [http://redis.io](http://redis.io) - [klusterin opetusohjelma Redis](http://redis.io/topics/cluster-tutorial)- [klusterin ja toistaminen](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) .
>-  Redis-konsolin on uusi yhteys tietokantaan 0 aina, kun lähetät komennon. Et voi käyttää `SELECT` komennolla voit valita toisen tietokannan, koska tietokannan palautetaan 0 jokaisen komennon. Lisätietoja Redis.txt komentoja, kuten eri tietokantaan, jossa on [miten Redis.txt komentoja voi suorittaa?](cache-faq.md#how-can-i-run-redis-commands)

Valitse käyttämään Redis-konsolin **konsolin** **Redis välimuisti** -sivu.

![Redis konsoli](./media/cache-configure/redis-console-menu.png)

Antamaan komennot vastaan välimuistin esiintymän kirjoittaa haluamasi komennon konsoliin.

![Redis konsoli](./media/cache-configure/redis-console.png)

Redis.txt komennot, jotka eivät ole käytettävissä Azure Redis välimuistin luetteloon Tutustu [Redis komentoja, ei tueta Azure Redis välimuistin](#redis-commands-not-supported-in-azure-redis-cache) edellisessä osassa. Saat lisätietoja Redis.txt komennot [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Siirrä välimuistin uuteen tilaukseen.

Voit siirtää välimuistin uusi tilaus valitsemalla **Siirrä**.

![Siirrä Redis.txt välimuisti](./media/cache-configure/redis-cache-move.png)

Artikkelissa tietojen siirtäminen resurssit ryhmästä yksi resurssi toiseen ja yksi tilaus toiseen [siirtää uusi resurssiryhmä tai-Tilaustani resursseja](../resource-group-move-resources.md).

## <a name="next-steps"></a>Seuraavat vaiheet
-   Lisätietoja Redis.txt komentojen käyttämisestä on artikkelissa [miten Redis.txt komentoja voi suorittaa?](cache-faq.md#how-can-i-run-redis-commands).
