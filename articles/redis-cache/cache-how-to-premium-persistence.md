<properties 
    pageTitle="Tietojen pysyvyyttä Premium Azure Redis välimuistin määrittäminen" 
    description="Lue, miten voit määrittää ja hallita tietojen pysyvyyttä Premium taso Azure Redis välimuistin kopioita" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Tietojen pysyvyyttä Premium Azure Redis välimuistin määrittäminen

Azure Redis välimuistin on eri välimuistin tarjouksia, jossa välimuistin kokoa ja ominaisuuksista, kuten uusien Premium tason valinta joustavasti.

Azure Redis välimuistin premium taso sisältää ominaisuuksia, kuten klusterointi, pysyvyyttä ja virtual verkkotuki. Tässä artikkelissa käsitellään määrittäminen pysyvyyttä premium Azure Redis välimuistin esiintymässä.

Lisätietoja muiden välimuistin erikoisominaisuuksia on artikkelissa [Johdatus Azure Redis välimuistin Premium-tason](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Mitä tietoja pysyvyyttä?
Redis.txt pysyvyyttä voit jatkuvat Redis.txt tallennettuja tietoja. Voit myös ottaa tilannevedoksen ja Varmuuskopioi tiedot, joita voit ladata Jos epäonnistui. Tämä on erittäin suuri etu Basic tai Standard taso, jossa kaikki tiedot on tallennettu muistiin ja mahdollisesta tietojen menettämisen uhkasta, jos kyseessä on virhe, jossa välimuistin solmut eivät ole käytettävissä voi olla verrattuna. 

Azure Redis välimuistin on Redis.txt pysyvyys [RDB mallia](http://redis.io/topics/persistence), jossa tiedot on tallennettu Azure-tallennustilan tilin. Kun pysyvyys on määritetty, Azure Redis välimuistin jatkuu tilannevedoksen Redis.txt välimuistin Redis.txt binaarimuodossa määritettäviä varmuuskopion korkojakso levylle. Jos kriittinen tapahtuman ilmetessä, joka estää perus- ja replikan välimuistin välimuistin järjestetään uudelleen viimeisimmän tilannevedoksen avulla.

Pysyvyys voidaan määrittää **Uusi Redis välimuisti** -sivu, välimuistin luonnin aikana ja varten luodun premium tallentaa **asetukset** -sivu.

## <a name="create-a-premium-cache"></a>Luoda premium välimuistin

Voit luoda välimuistin ja määrittää pysyvyys, kirjautuminen [Azure portal](https://portal.azure.com) ja valitse **Uusi**->**tietojen + tallennustilan**>**Redis välimuistin**.

![Luoda Redis.txt välimuistin][redis-cache-new-cache-menu]

Määrittämiseen pysyvyyttä ensin valitse jokin seuraavista **Premium** tallentaa **Valitse hinnoittelu taso** -sivu.

![Valitse hinnoittelu taso][redis-cache-premium-pricing-tier]

Kun premium, hinnoittelua taso on valittuna, valitse **Redis pysyvyys**.

![Redis pysyvyys][redis-cache-persistence]

Seuraavassa osassa kuvataan, miten uusi premium välimuistin Redis.txt pysyvyyttä määrittämistä varten. Kun Redis.txt pysyvyys on määritetty, valitse **Luo** luodaksesi uuden premium välimuistin Redis.txt pysyvyys.

## <a name="configure-redis-persistence"></a>Määritä Redis.txt pysyvyys

Redis pysyvyys on määritetty **Redis tietojen pysyvyyttä** -sivu. Saat uuden tallentaa tämä sivu pääsee välimuistin luomisen aikana, edellisessä kohdassa kuvatulla tavalla. Aiemmin tallentaa **Redis tietojen pysyvyyttä** -sivu avataan varten välimuistin **asetukset** -sivu.

![Redis asetukset][redis-cache-settings]

Redis.txt pysyvyyttä valitsemalla käyttöön RDB (Redis.txt tietokannan) varmuuskopion **käytössä** . Poista Redis.txt pysyvyyttä aiemmin käytössä premium-välimuistin käytöstä valitsemalla **ei käytössä**.

Varmuuskopion välin määrittämiseen avattavasta luettelosta **Varmuuskopion korkojakso** . Vaihtoehdot ovat **15 minuuttia**, **30 minuuttia**, **60 minuuttia**, **6 tuntia**, **12 tuntia**ja **24 tuntia**. Tällä aikavälillä käynnistyy laskien, kun edellisen varmuuskopioinnin onnistuu ja kun se kuluu uuden varmuuskopion on aloitettu.

Valitsemalla **Tallennustilan tilin** tallennustilan tili, jota haluat käyttää, ja valitse joko **perusavain** tai **toissijaisen avaimen** käyttää **Tallennustilan avain** avattavasta luettelosta. Sinun on valittava tallennustilan tilin samalla alueella kuin välimuistin ja **Premium-tallennustilan** tilin on suositeltavaa, sillä premium tallennustila on suurempi siirtonopeuden. 

>[AZURE.IMPORTANT] Jos tilisi pysyvyyttä tallennustilan näppäintä täyttyy, on rechoose haluamaasi näppäintä **Tallennustilan avain** avattavasta luettelosta.

![Redis pysyvyys][redis-cache-persistence-selected]

Valitse **OK** , jos haluat tallentaa pysyvyyttä kokoonpanon.

Seuraava varmuuskopiointi (tai tallentaa uuden ensimmäisen varmuuskopiointi) käynnistetään, kun varmuuskopioinnin korkojakso varoittava.



## <a name="persistence-faq"></a>Pysyvyyttä usein kysytyt kysymykset

Seuraavassa on vastauksia usein kysyttyihin kysymyksiin Azure Redis välimuistin pysyvyys.

-   [Aiemmin luodun välimuistin pysyvyyttä käyttöön](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Voit muuttaa varmuuskopioinnin korkojakso välimuistin luotuasi?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Miksi minun varmuuskopion taajuus 60 minuuttia siellä yli 60 minuuttia varmuuskopioiden välillä?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Mitä tapahtuu vanhojen varmuuskopioiden, kun uusi varmuuskopio tehdään?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Mitä tapahtuu, jos minulla on sovitettu erikokoinen ja varmuuskopion palautetaan, joka on tehty ennen skaalauksen toimintoa?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Aiemmin luodun välimuistin pysyvyyttä käyttöön

Kyllä, Redis.txt pysyvyyttä voi määrittää sekä välimuistin luominen ja aiemmin premium tallentaa.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Voit muuttaa varmuuskopioinnin korkojakso välimuistin luotuasi?

Voit muuttaa varmuuskopioinnin taajuus- **Redis tietojen pysyvyyttä** -sivu. Katso ohjeet [määrittäminen Redis pysyvyys](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Miksi minun varmuuskopion taajuus 60 minuuttia siellä yli 60 minuuttia varmuuskopioiden välillä?

Varmuuskopion toistumisvälin ei käynnisty, kunnes edellinen Varmuuskopiointi onnistui. Jos varmuuskopion korkojakso on 60 minuuttia ja vie varmuuskopion prosessin 15 minuutin loppuun, seuraava varmuuskopiointi ei käynnisty edellisen varmuuskopioinnin alkamispäivää 75 minuutin asti.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Mitä tapahtuu vanhojen varmuuskopioiden, kun uusi varmuuskopio tehdään?

Kaikki paitsi uusin varmuuskopiot poistetaan automaattisesti. Tämän poiston ei ehkä suoriteta välittömästi, mutta vanhoja varmuuskopioita ei säilytetä jatkuvasti.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Mitä tapahtuu, jos minulla on sovitettu erikokoinen ja varmuuskopion palautetaan, joka on tehty ennen skaalauksen toimintoa?

-   Jos olet skaalata suurentaminen, ei ole vaikutusta.
-   Jos on sovitettu pienemmäksi ja sinulla mukautettujen [tietokantojen](cache-configure.md#databases) asetus, joka on suurempi kuin [tietokantojen rajoittaa](cache-configure.md#databases) varten uusi koko kyseiset tietokannat tietoja ei voi palauttaa. Lisätietoja on artikkelissa [on omat mukautetut tietokannat kyseinen asetus aikana skaalaus?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Jos on sovitettu pienemmäksi, kaikki tiedot edellisen varmuuskopioinnin pienemmäksi ei mahdu näppäimet voi poistaa palauttaminen aikana yleensä [allkeys lru](http://redis.io/topics/lru-cache) eviction-käytännön avulla.

## <a name="next-steps"></a>Seuraavat vaiheet
Opettele käyttämään Lisää erikoisominaisuuksia välimuistin.

-   [Johdanto Azure Redis välimuistin Premium-taso](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
