<properties
   pageTitle="'Big datasta' tietolähteiden käsittelemisestä | Microsoft Azure"
   description="Toimintaohjeet artikkelissa korostaminen kuvioiden määrittäminen Azure tietoluettelon avulla 'big datasta' tietolähteitä, kuten Azure-Blob-säiliö ja Azure tietojen Lake Hadoop HDFS."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Azure tietoluettelon suuri tietolähteiden käyttäminen

## <a name="introduction"></a>Johdanto
**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen **Azure tietoluettelon** on kaikki suojaamisessa henkilöiden löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden aiemmin tietolähteistä, mukaan lukien big datasta.

**Azure tietoluettelon** tukee rekisteröinnin Azure blogin tallennustilan BLOB-objektit ja kansioiden sekä Hadoop HDFS tiedostoja ja kansioita. Näiden tietolähteiden puolirakenteisia laatu tarjoaa joustavasti, mutta se myös tarkoittaa, että käyttäjät Ota huomioon, miten tietolähteet on järjestetty jotta rekisteröimistä **Azure tietoluettelon**useimmat arvon hakeminen.

## <a name="directories-as-logical-data-sets"></a>Kansioiden looginen tiedot joukkona

Yleistä mallia järjestäminen suuri tietolähteet on Käsittele kansioiden looginen tietojoukot. Ylimmän tason kansioiden käytetään määrittämiseen tietojoukon, kun alikansiot määrittäminen osiot ja ne sisältävät tiedostot tallentaa tiedot itse.

Esimerkki tätä mallia voi olla:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Tässä esimerkissä vehicle_maintenance_events ja location_tracking_events edustavat looginen tietojoukot. Jokainen nämä kansiot sisältää tietoja-tiedostoja, jotka on järjestetty alikansioihin vuoden ja kuukauden mukaan. Jokaisen näissä kansioissa voi sisältää mahdollisesti satoja tai tuhansia tiedostoja.

Tässä mallissa yksittäisiä tiedostoja rekisteröimistä **Azure tietoluettelon** luultavasti ole tulkita. Sen sijaan voit rekisteröidä kansiot, jotka edustavat tietojoukot, joka on merkityksellinen tietojen käsitteleminen käyttäjille.

## <a name="reference-data-files"></a>Viite-datatiedostojen sijainti

Täydentäviä kuvio on tallentamiseen viitata tietojoukot yksittäisinä tiedostoina. Nämä tietojoukot voi ajatella big datasta "pieni" reunaan ja ovat usein mitat analytical tietomallissa. Viittaus datatiedostot sisältävät tietueet, joiden avulla asiayhteyden big datasta säilöön tallennettuja muualla datatiedostojen usean.

Esimerkki tätä mallia voi olla:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Kun henkilön tai tietojen Tiedemies toimii suurempi directory-rakenteita sisältämät tiedot, viittaus tiedostot tiedot voidaan kohteisiin, joihin viitataan yksinomaan nimi tai tunnus suurempi tietojoukon tarkempia tietoja.

Tätä mallia se tekee kannattaa rekisteröidä yksittäisiä viittaus-datatiedostoja **Azure tietoluettelon**. Kunkin tiedoston edustaa tietojoukon ja kullekin paperiliitinkuvake- ja havaitsi yksitellen.

## <a name="alternate-patterns"></a>Vaihtoehtoinen kuviot

Yllä olevien ohjeiden kuviot ovat suuri tietosäilö voi järjestää vain kaksi mahdolliset näyttötavat, mutta kussakin on eri. Riippumatta siitä, miten tietolähteet selkeä rakenne, kun suuri tietolähteiden rekisteröimistä **Azure tietoluettelon**Ota huomioon rekisteröiminen tiedostoja ja kansioita, jotka edustavat tietojoukkojen arvo muille organisaation sisällä on. Kaikki tiedostot ja kansiot rekisteröiminen voit tarpeettomat luettelon, jolloin käyttäjät löytävät tarvitsemansa näyttävyyttä.

## <a name="summary"></a>Yhteenveto
Tietolähteiden rekisteröimistä **Azure tietoluettelon** helpottaa niiden löytämistä löydät ja ymmärtää. Rekisteröiminen ja big datasta tiedostoja ja kansioita, jotka edustavat loogisia tietojoukot lisäät huomautuksia, voit auttaa käyttäjiä etsiä ja käyttää tarvitsemiaan suuri tietolähteitä.
