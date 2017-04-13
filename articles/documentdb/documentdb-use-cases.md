<properties 
    pageTitle="Yleisiä DocumentDB käyttötapauksiin | Microsoft Azure" 
    description="Tietoja sivun viisi käyttötapauksiin DocumentDB varten: käyttäjän luoma sisältöä, tapahtumalokiin kirjaaminen, luettelotiedot, asetukset käyttäjätiedot ja asioita Internet (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Yleisiä DocumentDB Käytä tapauksissa
Tässä artikkelissa on yleiskatsaus useita yleisiä Käytä tapauksissa DocumentDB varten.  Tässä artikkelissa suositukset yhteyshenkilönä pohjana kun kehität sovelluksen DocumentDB mukana.   

Luettuasi tämän artikkelin pystyt seuraaviin kysymyksiin: 
 
- Mitkä ovat yleisiä käytön laatikkomäärät DocumentDB?
- Mitkä ovat eduista DocumentDB käyttäjänä luotu sisällön kaupan?
- Mitä eduista DocumentDB kuin luettelotiedot tallennetaan?
- Mitä eduista DocumentDB tapahtumaloki nimellä tallentaa?
- Mitä eduista DocumentDB nimellä asetukset käyttäjätiedot tallennetaan?
- Mitä hyötyä käyttäen DocumentDB tietosäilö asioita Internet (IoT) Systemsin?

## <a name="common-use-cases-for-documentdb"></a>Käytä yleisiä palvelupyynnöt DocumentDB
Azure DocumentDB on yleisiä tarkoitus NoSQL tietokanta, jota käytetään monien sovellukset ja käytä tapauksissa. On sovellus, jossa on pieni millisekunnin tilauksen vastauksen kertaa sekä skaalata nopeasti on hyvä vaihtoehto. Seuraavassa on joitakin DocumentDB määritteitä, jotka helpottavat sovellu tehokas sovellukset.

- DocumentDB osioiden grafiikkatiedostomuotoja hyvän käytettävyyden ja skaalattavuus tiedot.
- DocumentDB's on pieni viive millisekunnin tilauksen vastauksen aikoja tallennustilan palautettua Suoritettaessa.
- Yhdenmukaisuuden tasoja, kuten potentiaalisen-istunnon ja joka staleness DocumentDB käyttäjän tuki mahdollistaa pienen kustannukset, suorituskyky-suhde. 
- DocumentDB on joustavia tietojen soveltuvia hinnoittelu mallin, joka mittareissa säilytys- ja siirtonopeuden erikseen.
- DocumentDB on varattu siirtonopeuden mallin avulla voit ajatella lukee/kirjoituksia asemesta pohjana olevan laitteen suorittimen ja muistin/IOPs määrä.
- DocumentDB's rakenne avulla voit skaalata valtaviin pyynnön tietomääristä päivittäisten pyyntöjen miljardeja järjestyksessä.

Nämä määritteet ovat erityisen hyödyllisiin, kun gaming ja IoT-sovelluksia, jotka on pieni vastauksen kertaa ja sinun pitää käsitellä mahdutettavia lukee ja kirjoittaa mobiili-sivustoon on. 

## <a name="user-generated-content"></a>Luotu käyttäjän sisältö
Yleisiä Käyttötapaus DocumentDB kannattaa tallentaa ja kysely-käyttäjän luoma sisällön (UGC) verkko-ja mobile-sovellukset, erityisesti Yhteisöpalvelut sovellukset.  Esimerkkejä UGC ovat keskustelu-istunnot, tweets, blogimerkintöjen, luokitusten ja kommentit.  Usein UGC Yhteisöpalvelut-sovelluksissa on sekoitus vapaamuotoinen tekstin ominaisuudet, tunnisteet ja suhteita, jotka ovat ei rajoittamalla kiinteä rakenne.   

Sisältö, kuten keskustelut, kommentit ja viestit voidaan tallentaa DocumentDB tarvitsematta muunnoksia tai relaatio yhdistäminen kerrokset monimutkaisia objektia.  Tietojen ominaisuudet voi lisätä tai muokata helposti vastaamaan vaatimukset, kun kehittäjät käytöstä sovelluksen-koodin päälle edistää näin nopeasti.  

Sovellusten etsiminen eri yhteisöpalveluihin integrointi on vastattava muuttaminen rakenteita verkostojen.  Tietoja indeksoidaan automaattisesti DocumentDB oletusarvoisesti, kun tiedot on valmis kyselyitä milloin tahansa.  Näin ollen nämä sovellukset halutessasi noutaa ennusteiden vastaaviin tarpeen mukaan.       

Monia sosiaalisen sovellusten maailmanlaajuisesti suorituksen ja odottamatonta käyttötavat voivat toimia.  Joustavuutta skaalaus tietovaraston on tärkeää kun sovelluskerroksen Skaalaa vastaamaan käyttö tarvittaessa.  Voit skaalata lisäämällä DocumentDB-tilissä osioiden tiedot.  Lisäksi voit myös luoda DocumentDB tilejä useiden alueiden välillä. Katso DocumentDB palvelun alueen saatavuus [Azure-alueita](https://azure.microsoft.com/regions/#services).   

## <a name="catalog-data"></a>Luettelotiedot
Luettelon tietojen käyttöskenaariot liittyä tallentamisesta ja kyselyt joukon kohteiden, kuten henkilöt, paikat ja tuotteiden määritteet.  Esimerkkejä luettelotiedot ovat käyttäjätilit, tuotteen luetteloita, laite paikallisrekisterit IoT ja materiaalien järjestelmien laskun.  Tämä tiedoille määritteet voivat vaihdella ja voivat muuttua ajan kuluessa sovelluksen vaatimukset sopivaksi.  

Harkitse Esimerkki tuoteluettelon auton osat toimittaja. Jokaisen osan voi olla oma määritteet yleisiä määritteet, joilla on eri osien välillä.  Tietyn osan määritteet voit lisäksi muuttaa seuraavan vuoden, kun uusi malli on julkaistu.  JSON asiakirjan myymälän DocumentDB tukee joustavia rakenteita ja mahdollistaa sisäkkäisiä ominaisuuksissa koskevat tiedot ja näin se sopii hyvin hyödyntää tuotteen luettelon tietojen tallentamista varten.       

## <a name="logging-and-time-series-data"></a>Kirjaaminen ja aikasarjalle tiedot
Sovelluksen kirjaaminen on usein kemiallisissa suurista tietomääristä ja saattaa on erilaisten määritteiden perusteella käyttöön sovellusversio tai osan kirjaaminen tapahtumat.  Monimutkaisten suhteiden tai kiinteä rakenteita ei rajoittamalla lokitiedot. Yhä lokitiedot on samanlainen JSON-muodossa, koska JSON on kevyt ja helposti lukea ihmisiin.
   
Ovat yleensä tapahtumaloki liittyviä pää Käytä tapauksista.  Ensimmäinen käyttötapaus on suorittaa ad-hoc kyselyiden osan tiedoista vianmääritystä varten.  Vianmäärityksessä, tietojen alijoukon ensin haetaan lokit, yleensä aikasarjalle mukaan.  Siirtyä alaspäin suoritetaan suodattamalla tietojoukko virhe tasot tai virhesanomia. Tämä on, jos tallentaminen tapahtumalokien DocumentDB on hyötyä. Oletusarvoisesti indeksoidaan automaattisesti DocumentDB tallennettuja tietoja ja näin se on valmis kyselyitä milloin tahansa. Lisäksi lokitiedot voidaan määrittää pysyväksi tietojen osioiden välillä aika-sarjana. Vanhat lokit voit koota kiinnostunut säilöön oman säilytyskäytäntö kohden.          

Toinen Käyttötapaus liittyy pitkään suoritetut tietojen analytics työt suorittaa offline-tilassa päivittäin erittäin paljon tietoja.  Tämä Käyttötapaus esimerkiksi palvelimen käytettävyys analyysi-, sovelluksen virhe analyysi- ja clickstream Tietoanalyysi.  Hadoop käytetään yleensä seuraavanlaisiin analyysien suorittamiseen.  Hadoop Connector for DocumentDB, jossa DocumentDB tietokannat toimivat tietolähteet ja poistumia Possu, rakenne ja kartta ja vähennä projekteille. Lisätietoja Hadoop Connector for DocumentDB on artikkelissa [Hadoop-suoritetaan DocumentDB ja Hdinsightista](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Gaming
Tietokannan taso on tärkeä osa gaming sovellusten. Nykyaikainen visualisointi n suorittaa graafinen käsittely mobile/console-asiakassovelluksissa, mutta riippuvaisia mukautettuja ja mukautettu sisältöä, kuten Ottelu-tilasto Yhteisöpalvelut integrointi ja suuri pistemäärän tulostaulukot pilveen. Visualisointi n vaadi erittäin pienen viiveitä lukee kirjoituksia antamaan kiinnostavissa--Ottelu kokemus ja tietokannan taso on käsitellä highs ja pyyntö korvaukset lows uuden Ottelu käynnistyy ja toimintojen päivitykset aikana.

DocumentDB on eivoi valtaviin asteikko [ominaisuussäilöjen toimimattoman: N mies maa](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) [Seuraava visualisointi n](http://www.nextgames.com/)ja [Halo 5: huoltajiensa tietoja](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Sekä käyttötapauksiin, DocumentDB tärkeimmistä eduista on seuraavasti:

- DocumentDB sallii suorituskyky voi skaalata ylös tai alas elastically. Näin visualisointi n käsittelemään päivittäminen profiilin ja tilasto-kymmeniä miljoonia samanaikaisesti pelaajat, tekemällä yhden API-kutsu.
- DocumentDB tukee millisekunnin lukee ja kirjoittaa ehkäistä minkä tahansa ennusteiden Ottelu aikana.
- DocumentDB's automaattisen indeksoinnin avulla vastaan useita erilaisia ominaisuuksia reaaliajassa suodattamista varten, Etsi esimerkiksi pelaajat niiden sisäinen Playerin tunnukset, tai niiden GameCenter, Facebook, Google tunnukset tai Playerin jäsenyys guild perustuva kysely. Tämä on mahdollista ilman monimutkaisia indeksoinnin tai sharding infrastruktuuri.
- Sosiaalisia ominaisuuksia, kuten Ottelu-keskustelun viestit, Playerin guild jäsenyydet, valmis haasteisiin, suuri pistemäärän tulostaulukot ja sosiaalisen kaavioiden on helpompi toteuttaa joustavia rakenteen käyttäminen.
- Hallitun platform-muodossa--palveluna (PaaS) DocumentDB tarvitaan mahdollisimman vähän määrittäminen ja hallinta toimii nopean iteraation varten, ja vähentää ajan market.


## <a name="user-preferences-data"></a>Käyttäjätiedot-asetukset
Internetissä eniten Moderni web- ja mobile-sovellukset sisältyvät monimutkaisia näkymissä ja kokemukset. Nämä näkymät ja kokemukset ovat yleensä dynaamisia ateriapalvelut Käyttäjäasetukset tai erilaisia mielialoja ja mukautus tarpeisiin.  Näin ollen sovellukset on voi hakea asetuksesi tehokkaasti jotta hahmonnetaan Käyttöliittymän osat ja kokemukset nopeasti. 

JSON on tehokas muoto edustaa Käyttöliittymän asettelun tietojen, se ei ole vain kevyt, mutta myös voit helposti tulkita JavaScript.  DocumentDB on tunable yhdenmukaisuuden tasoa, joka mahdollistaa nopea lukee pieni viive kirjoituksia kanssa. Näin ollen tallentaminen Käyttöliittymän asettelun tietojen mukaan lukien Omat asetukset JSON-tiedostoina DocumentDB on tehokas tapa saat tämän tiedot eri koneisiin.

## <a name="iot-and-device-sensor-data"></a>IoT ja laitteen tunnistimen tiedot
IoT Käytä tapauksissa jakaa yleisesti jotkin kuviot, miten ne ingest, käsitellä ja tallentaa tiedot.  Ensin järjestelmät Salli tietojen saanti, jotka voivat ingest bursts eri kielistä anturit laitteen tiedot.  Seuraavaksi järjestelmät käsitellä ja analysoida streaming saada reaaliaikaisia tietoja. Ja viimeksi mutta ei vähintään useimmat jollei kaikki tiedot myöhemmin Power View tietosäilö tietokoneiden välinen kyseltäessä ja offline-tilassa analysoinnissa.    

Microsoft Azure on monipuolisia palvelut, joita voidaan hyödyntää varten IoT käyttötapauksiin.  Azure IoT-palvelut ovat palvelujen, kuten Azure tapahtuman keskittimet, Azure DocumentDB, Azure Stream Analytics, Azure ilmoituksen keskittimeen, Azure koneen Learning, Azure Hdinsightiin ja PowerBI. 

Tietojen bursts on voidaan ruuansulatukseen Azure tapahtuman keskittimet kuin siinä on hyvin siirtonopeuden tietojen nieltynä pieni viive kanssa. Nautittuina tiedot, jotka käsiteltävä reaaliaikaisia tietoja voidaan funneled Azure Stream Analytics reaaliaikainen analysoinnissa. Tietoja voi ladata yhdeksi DocumentDB tietokoneiden välinen kyselyitä varten. Kun tiedot on ladattu DocumentDB, tiedot on valmis suorittaa kysely.  DocumentDB tiedot voidaan reaaliaikainen analytics osana viittaus tietoina. Lisäksi tietojen edelleen voidaan säätää ja käsitellä yhdistämällä DocumentDB tiedot HDInsight Possu, rakenne ja kartta ja vähennä projekteille.  Hienostunut tiedot on ladattu sitten takaisin DocumentDB raportointia varten.   

Esimerkki IoT ratkaisu, DocumentDB, EventHubs ja myrsky Katso [hdinsight-myrsky-esimerkkejä säilöön-GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Lisätietoja Azure tarjouksia IoT varten on artikkelissa [luominen Internet on Your asioita](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet
 
Aloita DocumentDB [tilin](https://azure.microsoft.com/pricing/free-trial/) luominen ja noudata Microsoftin [oppimispolku](https://azure.microsoft.com/documentation/learning-paths/documentdb/) DocumentDB tietoja ja löytää tarvittavat tiedot. 

Tai jos haluat lisätietoja asiakkaita käyttämällä DocumentDB, seuraavat asiakkaan jutuista ovat käytettävissä:

- [Seuraava visualisointi n](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Kävelykepit perille: N mies on maalla Ottelu soars # 1 tukee Azure DocumentDB.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Toteuttamistapaa Halo 5 sosiaalisen pelaaminen Azure DocumentDB avulla.
- [Cortana Analytics-valikoimassa](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics valikoima - Azure DocumentDB rakennettu skaalattava yhteisösivuston.
- [Postitusosoitteet](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Kansainvälisissä yritysten yleinen tietoja alussa integraattori antaa minuutteina joustavia Cloud-tekniikoiden kanssa.
- [Uutisia tasavalta](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Aikatietojen lisääminen tarkoitus ja antamaan kytketty kansalaisten uutisia. 
- [SGS kansainvälisen](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Yhtenäisen värin maapallon yli pää merkkien ottaminen SGS. Ja SGS muuttuu Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Yleinen Täytemerkki Telenor käyttää pilvipalveluun, siirtää käynnistys nopeuden. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Tulevaisuudessa kaupan suorittaa nopean haun ja tietojen kulun helposti.
