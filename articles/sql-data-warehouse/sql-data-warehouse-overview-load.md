   <properties
   pageTitle="Ladata tietoja SQL Azure-tietovarasto | Microsoft Azure"
   description="Lue Yleisiä tilanteita, joissa tietojen SQL-tietovarasto ladataan. Näitä ovat PolyBase, Azure-blob-säiliö, tasainen tiedostoja ja levyllä-toimitus. Voit käyttää myös kolmannen osapuolen työkaluja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Ladata tietoja SQL Azure-tietovarasto

Skenaario asetukset ja tiedot ladataan SQL-tietovarasto koskevia suosituksia yhteenveto.

Tietojen lataaminen vaikein osa yleensä valmistelee lataa tiedot. Azure yksinkertaistaa ladataan käyttämällä Azure-blob-säiliö kuin yleisiä tietosäilö monien palvelut ja orchestrate viestintä- ja tietojen siirtämistä Azure-palveluiden välillä Azure Data Factory avulla. Nämä prosessit on integroitu PolyBase teknologia, joka perustuu erittäin rinnakkain käsittely (MPP) lataamaan tietojen rinnakkain Azure-blob-säiliö SQL-tietovarasto. 

Katso opetusohjelmia, jotka lataavat otoksen tietokantojen, [Lataa Mallitietokannat][].

## <a name="load-from-azure-blob-storage"></a>Lataa Azure-blob-säiliö
Tietojen tuominen SQL-tietovarasto nopein tapa on tietoja ladataan Azure-blob-säiliö PolyBase avulla. PolyBase käyttää SQL Tietovarasto erittäin rinnakkain käsittely (MPP) rakenne lataa tiedot rinnakkain Azure-blob-säiliö. Käyttää PolyBase, voit käyttää T-SQL-komentoja tai Azure Data Factory-myyntijakso.

### <a name="1-use-polybase-and-t-sql"></a>1. Käytä PolyBase ja T-SQL

Yhteenveto ladataan prosessi:

2. Tietojen muotoileminen UTF-8, koska PolyBase ei tällä hetkellä tue UTF-16.
2. Tietojen siirtäminen Azure-blob-säiliö ja tallentaa sen tekstitiedostoista.
3. Ulkoiset objektit paikallaan SQL-tietovarasto sijainti ja tietojen määrittäminen
4. Suorita lataamaan rinnakkain tiedot uuteen tietokannan taulukkoon T-SQL-komento.

<!-- 5. Schedule and run a loading job. --> 

Katso opetusohjelma, [Lataa tiedot Azure-blob-säiliö ja SQL-tietovarasto (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. Käytä Azure Data Factory

Yksinkertaisin tapa käyttää PolyBase voit luoda Azure Data Factory-myyntijakso, joka käyttää PolyBase lataamaan tietojen Azure-blob-säiliö SQL-tietovarasto. Tämä on nopea määrittäminen, sillä sinun ei tarvitse määrittää T-SQL-objekteja. Jos haluat kyselyn ulkoisia tietoja tuomatta, käytä T-SQL. 

Yhteenveto ladataan prosessi:

2. Tietojen muotoileminen UTF-8, koska PolyBase ei tällä hetkellä tue UTF-16.
2. Tietojen siirtäminen Azure-blob-säiliö ja tallentaa sen tekstitiedostoista.
3. Luo Azure Data Factory-putkijohto ja ingest tiedot. Käytä PolyBase-vaihtoehtoa.
4. Ajoittaminen ja pitäminen putkijohto.

Katso opetusohjelma, [Lataa tiedot Azure-blob-säiliö ja SQL-tietovarasto (Azure Data Factory)][].


## <a name="load-from-sql-server"></a>Lataa SQL-palvelimesta
Lataa tiedot SQL Serveristä SQL-tietovarasto voit Integration Services (SSIS), siirrä tasainen tiedostot tai Toimitus levyjen Microsoftille. Katso yhteenveto eri voin luku ladataan prosesseja ja linkit opetusohjelmiin.

Täydellinen tietojen siirtäminen SQL Serveristä SQL-tietovarasto luomaan artikkelissa [siirron yleiskuvaus][]. 

### <a name="use-integration-services-ssis"></a>Käytä Integration Services (SSIS)
Jos käytät jo Integration Services (SSIS)-paketit lataamaan SQL Serveriin, voit päivittää oman pakettien SQL Server käyttää lähde- ja SQL-tietovarasto kohteeksi. Tämä on nopea ja helppo tehdä, ja se on hyvä vaihtoehto, jos yrität ei siirtää ladataan prosessin käyttämään tietoja jo pilveen. Tarjoa on kuormituksen on heikompi kuin käyttäminen PolyBase, koska tämä SSIS Suorita kuormituksen rinnakkain.

Yhteenveto ladataan prosessi:

1. Tarkista Integration Services-paketin osoittamaan lähteen SQL Server-esiintymän ja SQL-tietovarasto tietokannan kohde.
2. Siirtää rakenteen SQL-tietovarasto, jos sitä ei ole jo.
3. Muuta oman pakkauksissa määritystä käyttää tietotyyppejä, jotka tukevat SQL-tietovarasto.
3. Ajoittaminen ja pitäminen paketin.

Katso opetusohjelma, [Lataa tiedot SQL Server Azure SQL Data Warehouse (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Käytä AZCopy (suositellaan < 10 TT tiedot)
Jos arvopisteiden koko on < 10 TT, voit tietojen vieminen tasainen tiedostot SQL Server, kopioi tiedostot Azure-blob-säiliö ja lataa tiedot SQL-tietovarasto PolyBase avulla

Yhteenveto ladataan prosessi:

1. Komentorivin bcp-apuohjelman avulla voit viedä tietoja SQL Serverin tasainen tiedostot.
2. Komentorivin AZCopy-apuohjelman avulla voit kopioida tietoja tasainen tiedostot Azure-blob-säiliö.
3. Lataa SQL-tietovarasto PolyBase avulla.

Katso opetusohjelma, [Lataa tiedot Azure-blob-säiliö ja SQL-tietovarasto (PolyBase)][].

### <a name="use-bcp"></a>Käytä bcp
Jos sinulla on small tietomäärän voit ladata suoraan Azure SQL-tietovarasto bcp.

Yhteenveto ladataan prosessi:
1. Komentorivin bcp-apuohjelman avulla voit viedä tietoja SQL Serverin tasainen tiedostot.
2. Tietojen lataamista tasainen tiedostoja suoraan SQL-tietovarasto bcp avulla.

Katso opetusohjelma, [Lataa tiedot SQL Server Azure SQL-tietovarasto (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Käytä Tuo/Vie (suositellaan > 10 TT tiedot)
Jos haluat sen Siirry Azure arvopisteiden koko on > 10 Teratavua, on suositeltavaa, että käytät Microsoftin levyn toimitus palvelun [Tuo/Vie][]. 

Yhteenveto ladataan prosessi
2. Tietojen vieminen tasainen tiedostot siirrettävissä levyille SQL Server komentorivin bcp-apuohjelman avulla.
3. Toimita levyjen Microsoftille.
4. Microsoft SQL-tietovarasto lataa tiedot

## <a name="load-from-hdinsight"></a>Lataa Hdinsightiin
SQL-tietovarasto tukee ladataan tiedot HDInsight PolyBase kautta. Prosessi on sama kuin tietojen lataaminen Azure-Blob-säiliö - yhteyden muodostaminen Hdinsightiin lataa tiedot PolyBase avulla. 

### <a name="1-use-polybase-and-t-sql"></a>1. Käytä PolyBase ja T-SQL

Yhteenveto ladataan prosessi:

2. Tietojen muotoileminen UTF-8, koska PolyBase ei tällä hetkellä tue UTF-16.
2. Tietojen siirtäminen HDInsight ja tallentaa sen tekstitiedostoista, ORC tai tilat-muodossa.
3. Määrittää ulkoiset objektit SQL-tietovarasto sijainti ja tietojen määrittämiseen.
4. Suorita lataamaan rinnakkain tiedot uuteen tietokannan taulukkoon T-SQL-komento.

Katso opetusohjelma, [Lataa tiedot Azure-blob-säiliö ja SQL-tietovarasto (PolyBase)][].

## <a name="recommendations"></a>Suosituksia

Monissa kumppaneita on ladataan ratkaisuja. On artikkelissa Microsoftin [ratkaisun kumppaneille][]. 

Jos tiedoissa on pian saatavilla relaatio lähteestä ja haluat ladata sen SQL-tietovarasto haluat muuntaa riveihin ja sarakkeisiin, ennen kuin voit ladata sen. Muunnettua havaintoaineistoa ei tarvitse tallentaa tietokannan, se voidaan tallentaa tiedostot.

Luo tilastotiedot ladatut tiedot. Azure SQL-tietovarasto ei vielä tuki automaattinen luominen tai automaattisen päivityksen tilastotiedot.  Jotta saat parhaan suorituskyvyn-kyselyissä, on tärkeää tilastotiedot luomisesta kaikkien taulukoiden kaikkien sarakkeiden ensimmäinen kuormituksen tai merkittäviä muutoksia esiintyvät tiedot.  Lisätietoja on artikkelissa [tilastotiedot][].


## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Tietojen lataaminen Azure-blob-säiliö SQL-tietovarasto (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Lataa tiedot SQL Server Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Tietojen lataaminen SQL Server Azure SQL-tietovarasto (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Lataa Mallitietokannat]: ./sql-data-warehouse-load-sample-databases.md
[Siirron yleiskuvaus]: ./sql-data-warehouse-overview-migrate.md
[ratkaisu-kumppanit]: ./sql-data-warehouse-partner-business-intelligence.md
[kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md
[Tilastotiedot]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Tuo tai Vie]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
