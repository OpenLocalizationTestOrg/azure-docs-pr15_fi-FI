<properties 
    pageTitle="Johdanto Azure Redis välimuistin Premium tason | Microsoft Azure" 
    description="Katso, miten Luo ja hallitse Redis pysyvyys, Redis klusterointi ja Premium taso Azure Redis välimuistin kopioita VNET tuki" 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Johdanto Azure Redis välimuistin Premium-taso
Azure Redis välimuisti on jaettu, hallitun välimuistin, jonka avulla voit luoda erittäin skaalattava ja vastaa sovelluksia antamalla erittäin nopea access-tietoihin. 

Uusi Premium-taso on yrityksen valmis taso, joka sisältää kaikki Standard tason ominaisuudet ja enemmän, kuten parantaa suorituskykyä, suurentaa työmääriä, palauttaminen, Tuo/Vie ja parannettu suojaus. Jatka lukemista lisätietoja Premium välimuistin tason lisäominaisuuksia.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Vakio- tai Basic taso verrattuna suorituskyvyn parantamiseksi
**Parempi suorituskyky Standard tai Basic taso.** Premium-tason tallentaa on otettu käyttöön laitteen, joka on nopeampaa suorittimien ja antaa parempi suorituskyky verrattuna Basic- tai vakio taso. Premium taso tallentaa on suurempi siirtonopeuden ja pienempi viiveitä. 

**Sama samankokoista välimuisti siirtonopeuden on suurempi Premiumin verrattuna vakio taso.** Esimerkiksi 53 gt P4 siirtonopeuden (Premium) välimuisti on 250K pyynnöt sekunnissa verrattuna 150 K C6 (vakio).

Saat lisätietoja kokoa, liikenteen ja kaistanleveyden kanssa premium tallentaa [Azure Redis välimuistin usein kysytyt kysymykset](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Tietojen pysyvyyttä Redis
Premium-tason voit jatkuvat Azure-tallennustilan tilin välimuistin tiedot. Basic/Standard välimuistiin kaikki tiedot on tallennettu vain muistissa. Jos pohjana oleva infrastruktuuri ongelmien voi olla tietoja ei menetetä. On suositeltavaa Redis.txt tietojen pysyvyyttä-toiminnon käyttäminen Premium tason niin, että vikasietoisuudelle tietojen katoamiselta. Azure Redis välimuistin tarjoaa RDB ja AOF (tulossa) [Redis pysyvyys](http://redis.io/topics/persistence)asetukset. 

Ohjeita pysyvyyttä määrittämisestä on artikkelissa [Premium Azure Redis välimuistin pysyvyyden määrittämisestä](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Klusterin Redis
Jos haluat luoda tallentaa 53 megatavun tai et halua shard tietojen useita Redis.txt solmujen välillä, voit käyttää Redis.txt klusterointi, joka on käytettävissä Premium taso. Kukin solmu koostuu hallitsee Azure suuren ensisijainen/replikan välimuistin pari. 

**Redis.txt klusterointi avulla voit suurin asteikko ja siirtonopeuden.** Siirtonopeuden kasvaa lineaarisesti, kun lisäät shards (solmujen) klusterin määrä. . Jos luot 10 shards P4 klusterin ja valitse käytettävissä olevat on 250K * 10 = 2,5 miljoonaa pyynnöt sekunnissa. Katso lisätietoja kokoa, liikenteen ja kaistanleveyden kanssa premium tallentaa [Azure Redis välimuistin usein kysytyt kysymykset](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) .

Aloita asentaminen-kohdasta [käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittämisestä](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Parannettu suojaus ja eristyksen

Tallentaa luotu Basic tai Standard tason voi käyttää julkisen Internetissä. Välimuistin käyttöä on rajoitettu pikanäppäin perusteella. Premium-taso ja voit edelleen varmistaa vain määritetyn verkossa oleville asiakkaat voivat käyttää välimuistin. Voit ottaa käyttöön Redis välimuistin [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). Voit käyttää kaikkia VNet aliverkosta, access hallita käytäntöjä, kuten toimintoja ja muita ominaisuuksia, jotka edelleen rajoittamaan Redis.txt.

Lisätietoja on artikkelissa [Premium Azure Redis välimuistin VPN-tuki määrittämisestä](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Tuo tai Vie

Tuo tai Vie on Azure Redis välimuistin data management toimintoa, joka mahdollistaa Azure Redis välimuistin tietojen tuominen tai vieminen Azure Redis välimuistin tietojen tuomisesta ja viemisestä Redis välimuistin tietokannan (RDB) tilannevedoksen premium välimuistista sivun Blob-objektien Azure-tallennustilan tilin. Näin voit siirtää Azure Redis välimuistin eri esiintymien välillä tai täyttää välimuistin tietoja ennen käyttöä.

Tuo avulla voidaan tuoda Redis.txt yhteensopiva RDB tiedostot minkä tahansa Redis.txt palvelimeen cloud tai ympäristössä, mukaan lukien Redis.txt Linux tai minkä tahansa cloud-palvelun Amazon Web Services ja muiden Windows-käyttöjärjestelmässä. Tietojen tuonti on helppo tapa luoda välimuistin täyttää tiedoilla. Tuonnin aikana Azure Redis välimuistin RDB tiedostoja ladataan Azure tallennustilan muistiin ja lisää sitten välimuistiin näppäimet.

Vie voit viedä tiedot tallennetaan Azure Redis välimuistin Redis yhteensopiva RDB tiedostot. Voit käyttää tätä toimintoa voit siirtää tietoja Azure Redis välimuistin esiintymästä toiseen tai johonkin muuhun Redis.txt palvelimeen. Vie aikana tilapäinen tiedosto luodaan, joka isännöi Azure Redis välimuistin server-esiintymän AM ja tiedosto ladataan nimettyjen tallennustilan-tilille. Kun vienti on valmis joko tila onnistumisen tai virhe, tilapäinen tiedosto poistetaan.

Katso lisätietoja, [miten voit tuoda tiedot ja vie tiedot Azure Redis välimuistista](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Käynnistä tietokone uudelleen

Premium-taso mahdollistaa yhden tai usean käyttäjän välimuistin tarvittaessa solmut uudelleen. Voit testata vikasietoisuudelle, jos sovellus. Voit käynnistää seuraavat solmut uudelleen.

-   Välimuistin perusmuodon solmu
-   Välimuistin toissijaisen solmu
-   Pää- ja toissijaisen välimuistin solmut
-   Käytettäessä premium-välimuistin asentaminen Voit käynnistää uudelleen perusmuodon, toissijaisen tai molemmat solmut välimuistin yksittäiset shards

Lisätietoja on artikkelissa [Käynnistä uudelleen](cache-administration.md#reboot) ja [Käynnistä uudelleen usein kysytyt kysymykset](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Päivitysten ajoittaminen

Ajoitetut päivitykset-ominaisuuden avulla voit määrittää ylläpito-ikkuna, jossa välimuistin. Kun ylläpito-ikkuna on määritetty, Redis.txt palvelimen päivityksiä aikaiset tässä ikkunassa. Määrittää ylläpitovalintaikkunassa, valitse haluamasi päivän ja määritä ylläpidon ikkunan Aloita tunnin raja päivää kohden. Huomaa, että ylläpito ikkunan aika on UTC-muodossa. 

Lisätietoja on artikkelissa [päivitysten ajoittaminen](cache-administration.md#schedule-updates) ja [päivitysten ajoittaminen usein kysytyt kysymykset](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Vain Redis palvelimen päivitykset tehdään aikana suunniteltu ylläpito-ikkuna. Ylläpito-ikkuna ei koske Azure päivitykset tai päivitykset AM-käyttöjärjestelmään.

## <a name="to-scale-to-the-premium-tier"></a>Jos haluat skaalata premium-taso

Laajentaa premium taso, riittää, että Valitse premium tasoissa **Muuta hinnat taso** -sivu. Voit myös skaalata välimuisti, jotta PowerShell ja CLI premium taso. Vaiheittaiset ohjeet ovat kohdassa [asteikko Azure Redis välimuistin poistamisesta](cache-how-to-scale.md) ja [automatisoida skaalauksen toiminto](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Seuraavat vaiheet

Luoda välimuistin ja uusia premium taso-ominaisuuksia.

-   [Pysyvyyttä Premium Azure Redis välimuistin määrittäminen](cache-how-to-premium-persistence.md)
-   [Määrittäminen Premium Azure Redis välimuistin VPN-tuki](cache-how-to-premium-vnet.md)
-   [Käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittäminen](cache-how-to-premium-clustering.md)
-   [Tietojen tuominen ja vieminen tiedot voit Azure Redis välimuistista](cache-how-to-import-export-data.md)
-   [Azure Redis välimuistin hallinnasta](cache-administration.md)
  

