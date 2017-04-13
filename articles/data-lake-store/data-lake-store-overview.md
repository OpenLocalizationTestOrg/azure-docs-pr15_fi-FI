<properties
   pageTitle="Yleistä Azure järvi tietovaraston | Microsoft Azure"
   description="Tietoja Azure järvi tietovaraston ja se antaa tietoja muiden stores päälle arvo"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Yleistä Azure järvi tietosäilö

Azure järvi tietosäilö on yritystason hyper-akseli säilö big datasta analyyttisten toiminnoista. Azure tietojen järvi avulla voit tallentaa minkä tahansa kokoa, tyyppi ja nieltynä nopeus yhden paikassa toiminnalliset ja kokeilevaa analysoinnissa tietojen.

> [AZURE.TIP] Tutustu sen toimintoihin Azure tietojen järvi säilöpalvelun [järvi tietovaraston oppimispolku](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) avulla.

Azure järvi tietovaraston voidaan käyttää Hadoop (käytettävissä HDInsight-klusterin) käyttämällä WebHDFS-yhteensopiva REST API. Se on suunniteltu erityisesti käyttöön analytics tallennettuja tietoja ja suorituskyvyn tietojen analytics skenaarioissa optimoitu. Ulos-ruutuun se sisältää kaikki yrityksen arvosanojen ominaisuudet – suojaus, hallittavuuden, skaalattavuus, luotettavuutta ja käytettävyyden – olennaiset tosielämän yrityksen Käytä tapauksissa.


![Azure tietojen järvi](./media/data-lake-store-overview/data-lake-store-concept.png)

Jotkin Azure tietojen järvi keskeisiä ominaisuuksia ovat seuraavat.

### <a name="built-for-hadoop"></a>Laadittuihin ratkaisuihin Hadoop

Azure tietojen järvi säilö on yhteensopiva Hadoop Distributed (HDFS)-järjestelmän ja se toimii Hadoop ekosysteemissä Apache Hadoop-tiedostojärjestelmän.  Aiemmin HDInsight-sovellusten tai palveluihin, jotka käyttävät WebHDFS Ohjelmointirajapinnan helposti integroida tiedot järvi kaupan. Tietosäilö järvi paljastaa myös WebHDFS-yhteensopiva muiden sovellusten käyttöliittymä

Tietosäilö järvi tallennettuja tietoja voidaan helposti analysoida käyttämällä Hadoop analyyttisten puitteissa, kuten MapReduce tai rakenne. Microsoft Azure HDInsight klustereiden voit valmisteltu ja määritetty käyttämään suoraan järvi tietovaraston tallennettuja tietoja.

### <a name="unlimited-storage-petabyte-files"></a>Rajoittamaton tallennus-petabyte tiedostot

Azure järvi tietovaraston rajoittamaton tallennustila ja sopii erilaisia analysoinnissa tietojen tallentamista varten. Se ei asettaa rajoitusten tilin koot, tiedostoille tai tiedot, jotka voidaan tallentaa tiedot järvi määrää. Yksittäisiä tiedostoja voi vaihdella kilotavun kokoinen, jolloin hyvä vaihtoehto tallentaa minkä tyyppisiä tietoja petabytes. Tiedot tallennetaan kuuluminen tekemällä useita kopioita ja ei ole rajoitettu siksi ajaksi, jonka tiedot voidaan tallentaa tiedot järvi, valitse.

### <a name="performance-tuned-for-big-data-analytics"></a>Suorituskyvyn virittää suuri tietojen analysoinnissa

Azure järvi tietosäilö on luotu käytössä suuren mittakaavan analyyttisten järjestelmät, jotka edellyttävät valtaviin siirtonopeuden kysely ja analysoi suuria määriä tietoja. Tietoja järvi näkymät tiedoston osien päälle yksittäisiä tallennustilan palvelimien määrä. Lue siirtonopeuden parantaa luettaessa tiedoston rinnakkain tietojen analytics tekemistä varten.


### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise-sivuille: Erittäin käytettävissä ja turvallisesti

Azure järvi tietosäilö on alan standardiksi käytettävyyden ja luotettavuuden. Tietoja resurssien tallennetaan kuuluminen tekemällä tarpeettomat kopiota tahansa odottamattomia virheitä estämiseksi. Yrityksille käyttää Azure tietojen järvi ehdotetaan niihin ratkaisuja niiden olemassa olevat tiedot-ympäristön tärkeä osa.

Tietosäilö järvi sisältää myös yrityksen arvosanojen suojauksen tallennetut tiedot. Lisätietoja on artikkelissa [Azure järvi tietovaraston tietojen Securing](#DataLakeStoreSecurity).


### <a name="all-data"></a>Kaikki tiedot

Azure järvi tietovaraston voidaan tallentaa kaikki tiedot alkuperäisessä muodossaan sellaisenaan tarvitsematta mahdollisesti edellisen muunnoksia. Järvi tietosäilö ei tarvita rakenne määritellään, ennen kuin tiedot on ladattu, jätä yksittäisiä analyyttisten framework tulkita tiedot ja voit määrittää rakenteen analyysin milloin ylöspäin. Ei voi tallentaa tiedostoja haluamaansa koot ja muotoilut ansiosta järvi tietovaraston käsittelemään, puolirakenteinen, erimuotoisia tietoja.

Azure järvi tietovaraston säilöjä tiedot ovat olennaisesti kansiot ja tiedostot. Voit toimia käyttämällä SDK: T ja Azure Portal PowerShellin Azure tallennetut tiedot. Kun lisäät tietoa säilöön Nämä liityntäkohdat ja sopiva säilöjen avulla, voit tallentaa minkä tyyppisiä tietoja. Tietosäilö järvi Suorita minkä tahansa käsitellä tietoja, se tallentaa tietojen tyypin mukaan.


## <a name="DataLakeStoreSecurity"></a>Azure järvi tietovaraston tietojen suojaaminen

Azure järvi tietovaraston käyttää Azure Active Directory käyttöoikeuksien ja käyttöoikeusluettelot (käyttöoikeusluettelot) voi käyttää tietojasi hallintaan.

| Toiminto                                 | Kuvaus                              |
|-----------------------------------------|------------------------------------------|
| Todennus | Azure järvi tietovaraston integroi ja Azure Active Directory (AAD) tunnistetietojen ja käytön hallinnan kaikki Azure järvi tietovaraston tallennetut tiedot. Tuloksena integrointi Azure tietojen järvi eduista kaikki AAD ominaisuudet, mukaan lukien monimenetelmäisen todentamisen, ehdollisen käyttöoikeuden, Roolipohjainen käyttöoikeuksien valvonta, sovelluksen käytön valvonta, suojauksen valvonnan ja ilmoitat, jne. Azure järvi tietovaraston tukee OAuth 2.0-protokollan todentaminen REST-käyttöliittymän. |
| Käyttöoikeuksien hallinta                          | Azure järvi tietosäilö on käyttöoikeuksien valvonta tukemalla POSIX-tyyppiseksi käyttöoikeudet näyttämiä WebHDFS-protokollaa. Nykyisessä versiossa käyttöoikeusluettelot voidaan ottaa käyttöön pääkansio, alikansioita sekä yksittäisiä tiedostoja. Haluat käyttää pääkansion käyttöoikeusluettelot tulee koskevat myös kaikki lapsen kansiot ja tiedostot sekä.|

Jos haluat lisätietoja järvi tietovaraston tietojen suojaaminen. Noudata alla olevia linkkejä.

* Ohjeita siitä, miten voit tietojen suojaaminen järvi säilössä on artikkelissa [Azure järvi tietosäilö tietojen Securing](data-lake-store-secure-data.md).
* Mieluummin videoita? [Tässä videossa](https://mix.office.com/watch/1q2mgzh9nn5lx) suojauksesta järvi tietovaraston tallennettuja tietoja.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Azure järvi säilön kanssa yhteensopivista sovelluksista

Azure järvi tietosäilö on yhteensopiva Hadoop ekosysteemissä eniten Avaa lähde-osat. Se myös integroitu hyvin Azure muiden. Näin järvi tietovaraston sopivaa vaihtoehtoa tietojen tallennustilan tarpeisiin. Alla olevista linkeistä saat lisätietoja siitä, miten järvi tietovaraston voidaan sekä Avaa lähde osat sekä Azure muiden palveluiden kanssa.

* Katso [sovellusten ja palveluiden yhteensopiva Azure järvi tietovaraston](data-lake-store-compatible-oss-other-applications.md) yhteensopiva kanssa järvi tietovaraston Avaa lähde-sovellusten luettelo.
* Katso selvittääksesi, miten järvi tietovaraston voidaan Azure muiden palvelujen kanssa voit ottaa käyttöön skenaarioiden monenlaisille [integrointi Azure muiden palvelujen kanssa](data-lake-store-integrate-with-other-services.md) .
* Katso [järvi tietovaraston käytön skenaariot](data-lake-store-data-scenarios.md) opit käyttämään järvi tietovaraston tilanteissa, kuten ingesting tietoja, tietojen käsittelemistä, latauksen ja kesäolympialaisten visualisointi tiedot.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Mitä Azure järvi tietovaraston tiedostojärjestelmässä (adl: / /)?

Tietosäilö järvi käyttää uuden tiedostojärjestelmän AzureDataLakeFilesystem kautta (adl: / /), Hadoop-ympäristöissä (käytettävissä HDInsight-klusterin). Sovellusten ja palveluiden adl: / / voivat hyödyntää edelleen suorituskyvyn optimointi, jotka eivät ole tällä hetkellä käytettävissä WebHDFS. Tuloksena järvi tietovaraston lisää joustavuutta käyttää joko käytön adl suositella parhaan suorituskyvyn: / / tai aiemmin luotua koodia ylläpitää jatkuvaa WebHDFS Ohjelmointirajapinnan käyttäminen suoraan. Azure Hdinsightiin hyödyntää täysin antaa parhaan suorituskyvyn järvi tietovaraston AzureDataLakeFilesystem.

Voit käyttää tietojen järvi tietovaraston käyttämällä `adl://<data_lake_store_name>.azuredatalakestore.net`. Katso lisätietoja siitä, miten voit käyttää tietoja järvi tietovaraston [tallennetut tiedot ominaisuuksien tarkasteleminen](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Miten Azure järvi tietovaraston käyttö aloitetaan?

Katso, miten valmistelu Azure-portaalissa tietovaraston järvi [järvi tietovaraston aloittaminen käyttämällä Azure-portaalissa](data-lake-store-get-started-portal.md). Kun on valmisteltu Azure tietojen järvi, big datasta palveluja, kuten Azure tietojen järvi Analytics tai Azure Hdinsightiin käytettäväksi järvi tietovaraston Opi. Voit myös luoda .NET-sovelluksen Azure järvi tietosäilö-tilin luominen ja suorittaa toimintoja, kuten Lataa tietoja, lataa tiedot jne.

- [Azure tietojen järvi Analytics käytön aloittaminen](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Azure Lake Tietosäilölle käyttämällä .NET SDK: N käytön aloittaminen](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Tietosäilö järvi videot

Jos käytät mieluummin katsominen videoiden avulla opit, järvi tietosäilö sisältää videoita alueen ominaisuuksia.

* [Luo tili Azure järvi tietosäilö](https://mix.office.com/watch/1k1cycy4l4gen)
* [Data Explorer avulla voit hallita tietojen Azure järvi tietosäilö](https://mix.office.com/watch/icletrxrh6pc)
* [Azure tietojen järvi Analytics yhdistäminen Azure järvi tietosäilö](https://mix.office.com/watch/qwji0dc9rx9k)
* [Accessin Azure järvi tietovaraston tietojen järvi Analytics kautta](https://mix.office.com/watch/1n0s45up381a8)
* [Yhteyden muodostaminen Azure järvi tietovaraston Azure Hdinsightiin](https://mix.office.com/watch/l93xri2yhtp2)
* [Accessin Azure järvi tietovaraston rakenne ja Possu kautta](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Tietojen kopioiminen ja sieltä pois Azure järvi tietovaraston DistCp (Hadoop Distributed kopio) avulla](https://mix.office.com/watch/1liuojvdx6sie)
* [Tietojen siirtäminen relaatio lähteiden ja Azure järvi tietovaraston Apache Sqoop avulla](https://mix.office.com/watch/1butcdjxmu114)
* [Tietoja tiedonsiirron Azure Data Factory käytön Azure järvi tietosäilö](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Azure tietovaraston järvi tietojen suojaaminen](https://mix.office.com/watch/1q2mgzh9nn5lx)



