<properties
    pageTitle="Tietojen tuominen koneen Learning Studio online tietolähteistä | Microsoft Azure"
    description="Voit tuoda koulutus tietojen Azure koneen Learning Studio eri verkkolähteistä."
    keywords="tuominen tietojen, tietojen muoto, tietotyyppejä, tietolähteitä, koulutus tiedot"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Tietojen tuominen Azure koneen Learning Studio eri tietojen tuominen-moduulia online tietolähteistä

Tässä artikkelissa kuvataan tuki online-tietojen tuominen eri lähteistä ja tietojen siirtäminen Azure koneen Learning kokeen lähteitä tarvittavat tiedot.

> [AZURE.NOTE] Tässä artikkelissa on yleisiä tietoja [Tietojen] [ import-data] moduuli. Yksityiskohtaisempia tietoja voit käyttää tietotyyppiä, muotoja, parametreja ja vastauksia yleisiin kysymyksiin, aiheessa moduulin viittaus [Tietojen] [ import-data] moduuli.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Johdanto

Voit käyttää tietojen Azure koneen Learning Studio olevista online useista tietolähteistä oman kokeen on käynnissä käyttämällä [Tietojen tuominen] [ import-data] moduuli:

- Http:n URL-osoite
- Hadoop HiveQL käyttäminen
- Azure-blob-säiliö
- Azure-taulukosta
- Azure SQL-tietokanta tai SQL Server Azure AM
- Paikallisen SQL Server-tietokantaan
- Tarjoaja – tällä hetkellä OData-tietosyötteestä
 
Työnkulun sähköä kokeissa Azure koneen Learning Studiossa koostuu vetämällä-ja-poistetaan osia sivulle alusta. Lisää online tietolähteiden [Tietojen tuominen] [ import-data] oman kokeen moduulin **tietolähteen**ja anna tietojen tarvittavat parametrit. Online-palveluissa tuetut tietolähteet on eritelty alla olevassa taulukossa. Tässä taulukossa on yhteenveto myös tiedostomuodot, joita tuetaan ja tietojen käytettävät parametrit.

Huomaa, että näitä koulutustietoja käytetään, että koe on käynnissä, koska se on käytettävissä vain kokeen. Vertailun tiedot, jotka on tallennettu tietojoukko-moduulissa ovat käytettävissä kokeen työtilassa.

> [AZURE.IMPORTANT] Tällä hetkellä [Tietojen] [ import-data] ja [Vie tiedot] [ export-data] moduulit voivat lukea ja kirjoittaa tietoja vain Azure tallennustilan luotu perinteinen käyttöönotto-mallin avulla. Toisin sanoen uusi Azure-Blob-säiliö tilin tyyppi, joka tarjoaa kuuma tallennustilan access taso tai hienoja tallennustilan access taso ei vielä tueta. 

> Tavallisesti kaikki Azure tallennustilan tilit, että olet luonut ennen service-asetus on ollut käytettävissä ei vaikuta. Jos haluat luoda uuden tilin, valitse **perinteinen** käyttöönotto-malli tai resurssi hallinnalla ja valitse **tilin tyyppi**, **yleisiä tarkoitus** **Blob-objektien tallennustilaan**sijaan. 

> Lisätietoja on artikkelissa [Azure-Blob-säiliö: kuuma ja tallennustilaa tasoa](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Tuetut online tietolähteet
Azure koneen Learning **Tietojen tuominen** -moduulin tukee seuraaviin tietolähteisiin:

Tietolähde | Kuvaus | Parametrit |
---|---|---|
HTTP sivuston URL-osoite |Pilkuilla erotetut arvot (CSV), sarkaineroteltuun arvot (TSV), määrite suhde-tiedostomuoto (ARFF) ja tukea Vector koneet (SVM – Vaalea) muotoilut-tietojen lukee kaikki sivuston URL-osoite, joka käyttää HTTP | <b>URL-osoite</b>: määrittää tiedoston, mukaan lukien sivuston URL-osoite ja tiedostonimen tunnistetta, koko nimi. <br/><br/><b>Tietojen muoto</b>: määrittää jokin tuettu tiedot tiedostomuodot: CSV, TSV, ARFF tai SVM light. Jos tiedoissa on otsikkorivi, sillä voidaan määrittää sarakkeiden nimet.|
Hadoop/HDFS|Lukee tiedot Hadoop distributed tallennustilan. Voit määrittää haluamasi tiedot HiveQL, SQL kaltaisessa kyselykieltä käyttäen. HiveQL myös voidaan koota tietoja ja suorittaa tietojen suodattaminen, ennen kuin lisäät tiedot tietokoneen Learning Studio.|<b>Kyselyn rakenne</b>: määrittää tietojen muodostaminen rakenteen kyselyä.<br/><br/><b>HCatalog palvelimen URI</b> : määritetty yhteyttä klusterin muotoiluja *nimen &lt;klusterinimi&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop käyttäjätilisi nimi</b>: määrittää Hadoop käyttäjänimi käytetään klusteri valmisteleminen.<br/><br/><b>Hadoop käyttäjätilin salasanan</b> : määrittää käytetään, kun valmistelu klusterin tunnistetiedot. Lisätietoja on artikkelissa [Luo Hadoop varausyksiköt Hdinsightista](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Siirtää tietoja sijainti</b>: määrittää, onko tiedot on tallennettu Hadoop distributed tiedostojärjestelmässä (HDFS) tai Azure-tietokannassa. <br/><ul>Jos tallennat siirtää tietoja HDFS, Määritä HDFS palvelin URI. (Muista käyttää HDInsight-klusterin nimi ilman HTTPS:// etuliitettä). <br/><br/>Jos tallennat tulosteen tietojen Azure, Määritä Azure-tallennustilan tilin nimi ja tallennustilaa pikanäppäin tallennustilan säilö nimi.</ul>|
SQL-tietokantaan |Lukee tiedot, jotka on tallennettu Azure SQL-tietokantaan tai Azure virtuaalikoneen käytössä SQL Server-tietokantaan. | <b>Tietokantapalvelimen nimi</b>: määrittää nimen, johon tietokanta on käynnissä palvelimeen.<br/><ul>Jos Azure SQL-tietokanta Kirjoita palvelimen nimi, joka on luotu. On yleensä lomakkeen * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Jos kyseessä on SQL server Azure Virtual machine isännöimät Kirjoita *tcp:&lt;virtuaalikoneen DNS-nimen&gt;, 1433*</ul><br/><b>Tietokannan nimi </b>: määrittää tietokannan nimen palvelimessa. <br/><br/><b>Palvelimen käyttäjänimi</b>: määrittää tilille, jolla on käyttöoikeudet tietokannan käyttäjänimi. <br/><br/><b>Palvelimen käyttäjätilin salasana</b>: määrittää käyttäjätilin salasanan.<br/><br/><b>Hyväksy kaikki palvelinvarmennetta</b>: Käytä asetusta (heikompi suojaus), jos haluat ohittaa tarkistaminen sivuston sertifikaatin, ennen kuin voit lukea tietoja.<br/><br/><b>Tietokantakyselyn</b>: Kirjoita SQL-lause, joka kuvaa haluat lukea.|
Paikallisen SQL-tietokantaan |Lukee tiedot, jotka on tallennettu paikallisen SQL-tietokantaan. | <b>Tietoja yhdyskäytävän</b>: määrittää Data Management Gateway asennettu tietokoneeseen, jossa se voi käyttää SQL Server-tietokannan nimen. Lisätietoja yhdyskäytävän määrittämisestä on artikkelissa [Tee advanced analytics Azure koneen Learning tietojen paikallisen SQL Server-palvelimen kanssa](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Tietokantapalvelimen nimi</b>: määrittää nimen, johon tietokanta on käynnissä palvelimeen.<br/><br/><b>Tietokannan nimi </b>: määrittää tietokannan nimen palvelimessa. <br/><br/><b>Palvelimen käyttäjänimi</b>: määrittää tilille, jolla on käyttöoikeudet tietokannan käyttäjänimi. <br/><br/><b>Käyttäjänimi ja salasana</b>: Valitse <b>Enter arvoja</b> ja kirjoittaa tietokannan tunnistetiedot. Voit käyttää Windows-todennusta tai SQL Server-todennus sen mukaan, miten paikallisen SQL-palvelin on määritetty.<br/><br/><b>Tietokantakyselyn</b>: Kirjoita SQL-lause, joka kuvaa haluat lukea.|
Azure-taulukosta|Lukee tietoja Azuren tallennustilaan taulukko-palvelusta.<br/><br/>Jos luet suuria tietomääriä harvoin, käytä Azure-taulukosta palvelu. Luo joustavan relaatio (NoSQL), erittäin skaalattava, edullinen ja erittäin käytettävissä tallennustilan ratkaisu.| **Tietojen tuominen** vaihtoehdot muuttuvat sen mukaan, onko käytät käyttöoikeuspalvelinta julkisia tietoja tai yksityisen-tili, joka edellyttää, että kirjautumistietosi. Tämä määräytyy <b>Todennustyyppi</b> joka voi olla arvo "PublicOrSAS" tai "Asiakas", joista jokaisella on oma joukko parametreja. <br/><br/><b>Julkinen tai jaettu Access allekirjoitus (SAS) URI</b>: parametrit ovat:<br/><br/><ul><b>Taulukon URI</b>: määrittää taulukon julkisen tai SAS URL-osoite.<br/><br/><b>Määrittää rivien määrä ominaisuuksien nimiä</b>: arvot ovat <i>TopN</i> tarkistamaan määritetty rivimäärän tai <i>ScanAll</i> saat kaikki rivit-taulukossa. <br/><br/>Jos tiedot ovat yhtenäisinä ja ennakoitavissa, on suositeltavaa, että valitset *TopN* ja Anna numero N. Suurten taulukoiden Tämä saattaa johtaa nopeammin lukeminen kertaa.<br/><br/>Jos tiedot rakenne on ominaisuuksia, jotka vaihtelevat syvyyttä ja taulukon sijainnin perusteella, valitse *ScanAll* -vaihtoehto, jos haluat tarkistaa kaikki rivit. Tällä varmistetaan, että tuloksena oleva ominaisuus ja metatietojen muuntaminen eheyden.<br/><br/></ul><b>Yksityinen-tallennustilan tilin</b>: parametrit ovat: <br/><br/><ul><b>Tilin nimi</b>: määrittää tilin, joka sisältää lukemaan taulukon nimi.<br/><br/><b>Tili-näppäintä</b>: määrittää näkyy tiliin liitetty tallennustilan-näppäintä.<br/><br/><b>Taulukkonimi</b> : määrittää lukemaan tiedot sisältävän taulukon nimi.<br/><br/><b>Rivien määrä ominaisuuksien nimiä</b>: arvot ovat <i>TopN</i> tarkistamaan määritetty rivimäärän tai <i>ScanAll</i> saat kaikki rivit-taulukossa.<br/><br/>Jos tiedot ovat yhtenäisinä ja ennakoitavissa, on suositeltavaa, että valitset *TopN* ja Anna numero N. Suurten taulukoiden Tämä saattaa johtaa nopeammin lukeminen kertaa.<br/><br/>Jos tiedot rakenne on ominaisuuksia, jotka vaihtelevat syvyyttä ja taulukon sijainnin perusteella, valitse *ScanAll* -vaihtoehto, jos haluat tarkistaa kaikki rivit. Tällä varmistetaan, että tuloksena oleva ominaisuus ja metatietojen muuntaminen eheyden.<br/><br/>|
Azure-Blob-säiliö | Lukee Azuren tallennustilaan, kuten kuvia, tai rakenteeton teksti tai binaaritietoja Blob-palveluun tallennettuja tietoja.<br/><br/>Voit käyttää Blob-palvelun voit näyttää tietoja julkisesti tai sovelluksen tietojen tallentamiseen yksityisesti. Voit käyttää tietoja mistä tahansa käyttämällä HTTP tai HTTPS-yhteyksiä. | **Tietojen tuominen** -moduulissa vaihtoehdot muuttuvat sen mukaan, onko käytät käyttöoikeuspalvelinta julkisia tietoja tai yksityisen-tili, joka edellyttää, että kirjautumistietosi. Tämä määräytyy <b>Todennustyyppi</b> , jonka voi olla arvo, "PublicOrSAS" tai "Tilin".<br/><br/><b>Julkinen tai jaettu Access allekirjoitus (SAS) URI</b>: parametrit ovat:<br/><br/><ul><b>URI</b>: määrittää storage blob julkinen vai SAS URL-osoite.<br/><br/><b>Tiedostomuoto</b>: määrittää muodon tiedot-Blob-palvelussa. Tuetut muodot ovat CSV TSV ja ARFF.<br/><br/></ul><b>Yksityinen-tallennustilan tilin</b>: parametrit ovat: <br/><br/><ul><b>Tilin nimi</b>: määrittää, joka sisältää blob, jota haluat lukea tilin nimi.<br/><br/><b>Tili-näppäintä</b>: määrittää näkyy tiliin liitetty tallennustilan-näppäintä.<br/><br/><b>Säilön, kansion tai blob polku</b> : määrittää nimen, joka sisältää tiedot, jos haluat lukea Blob-objektien.<br/><br/><b>BLOB-tiedostomuoto</b>: määrittää muodon tiedot-blob-palvelussa. Tuetut tietomuotojen on CSV, TSV, ARFF, CSV määritetty koodaus ja Excel. <br/><br/><ul>Jos muodossa on CSV- tai TSV, muista osoittavat, onko tiedostossa on otsikkorivi.<br/><br/>Excel-vaihtoehdon avulla voit lukea tietoja Excel-työkirjoissa. <i>Excel-tietojen muoto</i> -asetuksen osoittavat, onko tiedot Excel-laskentataulukon alueen tai Excel-taulukon. Määritä nimi, jonka haluat lukea taulukon <i>Excel-laskentataulukko tai upotettu taulukko </i>-vaihtoehto.</ul><br/>|
Tietosyötepalvelutoiminto | Lukee tietoja tuetuista syötteen tarjoajalta. Tällä hetkellä vain Open Data Protocol (OData)-muotoa tuetaan. | <b>Tietoja sisältötyyppi</b>: määrittää OData-muodossa.<br/><br/><b>Lähteen URL-osoite</b>: määrittää tietosyötteen koko URL-osoite. <br/>Esimerkiksi seuraava URL-osoite lukee Northwind-esimerkkitietokannan: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
