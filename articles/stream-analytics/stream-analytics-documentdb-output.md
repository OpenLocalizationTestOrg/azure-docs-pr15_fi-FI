<properties
    pageTitle="JSON tulosteen Stream analysoinnissa | Microsoft Azure"
    description="Katso, miten Stream Analytics kohdistaa Azure DocumentDB JSON tulostukseen tietojen arkistointi ja pieni viive kyselyjä rakenteeton JSON tiedot."
    keywords="JSON-tulostus"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Kohteen Azure DocumentDB JSON tulosteen Stream Analytics varten

Virta Analytics kohdistaa [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) JSON tulosteen, ottaminen käyttöön arkistointia ja pieni viive tietokyselyjä rakenteeton JSON-tietoihin. Opi toteuttamaan integrointi parhaiten.

Käyttäjille, jotka eivät tunne DocumentDB Tutustu [DocumentDB's oppimispolku](https://azure.microsoft.com/documentation/learning-paths/documentdb/) avulla pääset alkuun.

## <a name="basics-of-documentdb-as-an-output-target"></a>DocumentDB tulostus-kohteeksi perusteet
Azure DocumentDB tulosteen Stream Analytics mahdollistaa kirjoittaminen oman stream käsittelyn tulosten kuin JSON tulosteen DocumentDB collection(s) kyselyjä. Virta Analytics ei luoda sivustokokoelmat tietokannassa edellyttävän sen sijaan voit luoda toimiston ulkopuolella. Tämä on niin, että DocumentDB kokoelmien laskutuksen kustannukset eivät näy sinulle ja siten, että voit hienosäätää suorituskyvyn, yhdenmukaisuutta ja käyttämällä suoraan [DocumentDB API](https://msdn.microsoft.com/library/azure/dn781481.aspx)kokoelmien kapasiteetti. On suositeltavaa käyttää yhtä DocumentDB tietokantaa kohden streaming työn erottaa loogisesti streaming työn oman sivustokokoelmat.

Jotkin DocumentDB sivustokokoelman asetukset on kuvattu alla.

## <a name="tune-consistency-availability-and-latency"></a>Paranna yhdenmukaisuuden, käytettävyys ja viive

Vastaamaan sovelluksen tarpeitasi DocumentDB avulla voit tarkentaa paranna tietokanta ja sivustokokoelmat ja tee valinnat yhdenmukaisuuden, käytettävyys ja viive välillä. Sen mukaan, mitä tasot luku yhtenäisyyden skenaarion tarpeitasi vastaan lukeminen ja kirjoittaminen viive, voit valita yhdenmukaisuuden tason tietokanta-tililläsi. Oletusarvon mukaan DocumentDB ottaa myös synkronisen indeksoinnin kokoelmaan kunkin CRUD-toimintoa. Tämä on toisen hyödyllinen vaihtoehto, jos haluat hallita DocumentDB kirjoittaminen/luettu-suorituskykyä. Lisätietoja aiheesta Tarkista [tietokannan ja kyselyn yhdenmukaisuuden tasojen muuttaminen](../documentdb/documentdb-consistency-levels.md) -artikkelissa.

## <a name="choose-a-performance-level"></a>Valitse suorituskyky-käyttöoikeustaso

3 suorituskyvyn eri tasoilla (S1, S2 tai S3), joka määrittää nopeus käytettävissä CRUDs kyseisen kokoelmaan voi luoda DocumentDB sivustokokoelmat. Lisäksi suorituskyky vaikuttaa kokoelmaa indeksoinnin/yhdenmukaisuuden-tasot. Lue lisätietoja [tämän artikkelin](../documentdb/documentdb-performance-levels.md) ymmärtämään, nämä suorituskyvyn tasot yksityiskohtaiset tiedot.

## <a name="upserts-from-stream-analytics"></a>Virta Analytics-Upserts

Virta Analytics integrointi DocumentDB avulla voit lisätä tai päivittää tietueita oman DocumentDB-kokoelmaan annetun Tiedostotunnisteen sarakkeen mukaan. Tätä kutsutaan myös *Upsert*.

Virta Analytics käyttämällä Optimistinen Upsert toimintatavan, jossa päivitykset tehdään vain, kun Lisää epäonnistuu Tiedostotunnisteen ristiriitoja vuoksi. Tämä päivitys hoitaa Stream Analytics kuin KORJAUSTIEDOSTON ja siten sen avulla asiakirjaan, eli lisäämällä uusia ominaisuuksia tai korvaa aiemmin luodun ominaisuus suoritetaan vaiheittainen osittaisen päivitykset. Huomaa, että taulukko-ominaisuudet, JSON asiakirjan tuloksissa eli matriisin käytön korvata koko matriisin arvojen muutokset ei yhdistetä.

## <a name="data-partitioning-in-documentdb"></a>Tietojen jakaminen DocumentDB

DocumentDB sivustokokoelmat mahdollistavat osion tietojen kyselyn kuvioiden ja sovelluksen suorituskyvyn tarpeiden perusteella. Kunkin sivustokokoelman voi olla enintään 10 gt tietoja (enintään) ja tällä hetkellä tällä ei voi skaalata (tai ylivuoto) kokoelma. Skaalauksen ulos, Stream analyysin avulla voit kirjoittaa useita sivustokokoelmat annetun etuliite (Katso alla käyttötiedot). Virta Analytics käyttää yhdenmukaista [Hash-osion tulkintatoiminnon](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategia käyttäjän antamaa PartitionKey sarakkeen perusteella osion tulosteen tietueet. Kokoelmien annetun etuliite käynnistyksen streaming projektin aikana määrä käytetään tulosteen osion määrä, joihin resurssit kirjoittaa rinnakkain (DocumentDB sivustokokoelmat = tulosteen osiot). Yksittäisen S3, jossa kuitata indeksoinnin harjoittelu vain Lisää-0,4 ilmoitettuna olevien Mt/s kirjoittaminen siirtonopeuden tule olettaa tietoja. Useita Kokoelmien käyttäminen voit avulla voit tehostaa suurempi siirtonopeuden ja kapasiteetin.

Jos haluat suurentaa osion Laske myöhemmin, voit joutua lopettaa työtäsi, uudelleenosioiminen oman aiemmin sivustokokoelmat uusi ryhmiin tiedot ja Käynnistä Stream Analytics-työ. Lisätietoja käyttämällä PartitionResolver ja jakaminen uudelleen ja otoksen koodin sisällytetään seuranta viestiin. Tiedot myös artikkelissa [DocumentDB jakaminen](../articles/documentdb-partition-data.md#developing-a-partitioned-application) on tästä.

## <a name="documentdb-settings-for-json-output"></a>DocumentDB asetukset JSON tulostusta varten

Tietojen tarkastelu alla Kysy luominen DocumentDB tulos Stream Analytics Luo. Tässä osassa on selvitys ominaisuudet-määritys.

![documentdb stream analytics tulosteen näytössä](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Tulosteen Alias** – sähköpostitunnus viittaa tätä ASA kyselyn tulos  
-   **Tilin nimi** – nimi tai päätepisteen URI DocumentDB tilin.  
-   **Tilin avain** – DocumentDB tilin jaetun access-näppäintä.  
-   **Tietokannan** – DocumentDB tietokannan nimi.  
-   **Sivustokokoelman nimi kuvion** – sivustokokoelman nimi kuviota, jota käytetään loppu. Sivustokokoelman nimen muoto on rakennettava käyttämällä valinnainen {osion}-tunnuksen, mistä osioiden aloittaa 0. Seuraavassa on esimerkki kelvollinen syötteiden:  
   1\) MyCollection – yksi sivustokokoelman nimeltä "MyCollection" on olemassa.  
   2\) – esimerkiksi sivustokokoelmat on olemassa – MyCollection {osion} "MyCollection0", "MyCollection1", "MyCollection2" ja niin edelleen.  
-   **Osion avaimen** – tulosteen tapahtumat määritetään näppäintä jakaminen tulosteen yli sivustokokoelmat kentän nimi. Yksittäisen sivustokokoelman tulostukseen minkä tahansa haluamaansa esitystapa-sarakkeen voi käyttää esimerkiksi PartitionId.  
-   **Tiedostotunnisteen** – valinnainen. Määrittää perusavaimen, Lisää tai päivitä toimintojen perustuvat tulosteen tapahtumien kentän nimi.  
