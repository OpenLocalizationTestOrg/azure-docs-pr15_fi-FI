<properties 
    pageTitle="Siirry Azure SQL-tietokantaan tietojen Azure koneen Learning | Azure" 
    description="Luo SQL-taulukon ja SQL-taulukon tietojen lataaminen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Siirry Azure SQL-tietokantaan tietojen Azure koneen oppiminen

Tässä ohjeaiheessa kuvataan asetukset tietojen siirtämistä tasainen-tiedostoista (CSV- tai TSV muotoilut) tai paikallinen SQL Serveriä Azure SQL-tietokantaan tallennetuista tiedoista. Näiden tietojen siirtämistä pilveen tehtävät ovat ryhmän tietojen tiede yhteydessä.

Katso artikkelista, johon ympärille asetukset tietojen siirtäminen tietokoneeseen Learning paikallinen SQL Server-palvelimeen, [Siirrä tiedot SQL Server Azure virtual-laitteeseen](machine-learning-data-science-move-sql-server-virtual-machine.md).

**Valikon** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit ingest tietojen tuominen kohdeluettelo-ympäristössä, jossa tiedot voidaan tallentaa ja käsitellä aikana-työryhmän tiedot tiede prosessi (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Seuraavassa taulukossa on yhteenveto asetukset tietojen siirtämistä Azure SQL-tietokantaan.

<b>LÄHDE</b> |<b>KOHDE: Azure SQL-tietokanta</b> |
-------------- |--------------------------------|
<b>Tietuetiedostoon (CSV- tai muotoiltu TSV)</b> |<a href="#bulk-insert-sql-query">Bulk Lisää SQL-kysely |
<b>SQL-palvelimeen on luotuna</b> | 1. <a href="#export-flat-file">Tietuetiedostoon vieminen<br> 2. <a href="#insert-tables-bcp">siirron ohjattu SQL-tietokanta<br> 3. <a href="#db-migration">tietokannan takaisin ylös ja palauttaminen<br> 4. <a href="#adf">azure Data Factory |


## <a name="prereqs"></a>Edellytykset
Tässä kuvatut toimet edellyttävät, että sinulla on:

* On **Azure tilauksen**. Jos sinulla ei ole tilaus, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
* **Azure-tallennustilan tilin**. Voit käyttää Azure-tallennustilan tilin Tässä opetusohjelmassa tietojen tallentamista varten. Jos sinulla ei ole Azure-tallennustilan tilin, on artikkelissa [Luo tallennustilan tili](storage-create-storage-account.md#create-a-storage-account) . Luotuasi tallennustilan tilin, sinun täytyy käyttää tallennustilaan tilin Key-tunnuksen. Katso [näkymässä, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* **Azure SQL-tietokanta**Access. Jos käytössä on Azure SQL-tietokantaan, [Käytön aloittaminen Microsoft Azure SQL-tietokanta](../sql-database/sql-database-get-started.md) sisältää tietoja siitä, miten voit valmistella uuden esiintymän Azure SQL-tietokantaan.
* Asennettu ja määritetty **PowerShellin Azure** paikallisesti. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

**Tietoja**: siirron prosessit ovat osoittaa [Kokousesitelmän taksin tietojoukko](http://chriswhong.com/open-data/foil_nyc_taxi/)käyttäen. Kokousesitelmän taksin tietojoukko sisältää työmatkan tiedot ja messujen ja on käytettävissä merkille kyseisen viestin, valitse Azure-blob-säiliö: [Kokousesitelmän taksin tiedot](http://www.andresmh.com/nyctaxitrips/). Esimerkki ja näiden tiedostojen kuvaus toimitetaan [Kokousesitelmän taksin Trips tietojoukko kuvaus](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Voit mukauttaa omiin tietoihisi joukkoon Tässä kuvattuja ohjeita tai noudattamalla ohjeiden avulla Kokousesitelmän taksin tietojoukko. Lataa Kokousesitelmän taksin tietojoukko paikallinen SQL Server-tietokantaan, noudata kuvatut [Joukkona tietojen tuominen SQL Server-tietokantaan](machine-learning-data-science-process-sql-walkthrough.md#dbload). Nämä ohjeet koskevat SQL Server Azure Virtual Machine, mutta ohjeet lataaminen paikallinen SQL Server on sama.


## <a name="file-to-azure-sql-database"></a>Tietojen siirtämistä tietuetiedostoon lähteestä Azure SQL-tietokantaan

Tasainen tiedostot (CSV- tai TSV muotoiltu) tietoja voi siirtää joukkona Lisää SQL-kyselyn avulla Azure SQL-tietokantaan.

### <a name="bulk-insert-sql-query"></a>Bulk Lisää SQL-kysely

Bulk Lisää SQL-kyselyn avulla toimissa tehdään kaltaisia osat tietojen siirtämistä tietuetiedostoon lähteestä SQL Server Azure-AM-kohdassa. Lisätietoja on artikkelissa [Joukkona Lisää SQL-kysely](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Tietojen siirtämistä paikallinen SQL Server Azure SQL-tietokantaan

Jos lähdetietojen on tallennettu paikallinen SQL Serveriä, eri vaihtoehtoja siirtymisen tiedot Azure SQL-tietokanta on luotu:

1. [Vie Tietuetiedostoon](#export-flat-file) 
2. [Siirron ohjattu SQL-tietokanta](#insert-tables-bcp)
3. [Tietokannan takaisin ylös ja palauttaminen](#db-migration)
4. [Azure Data Factory](#adf)

Ensimmäiset kolme vaiheet ovat hyvin samankaltaisia kuin tietojen [siirtäminen SQL Server Azure virtuaalikoneen](machine-learning-data-science-move-sql-server-virtual-machine.md) osat, jotka kattavat saman näitä toimintosarjoja. Linkkejä ohjeaiheen asianmukainen osia toimitetaan seuraavien ohjeiden mukaisesti.

###<a name="export-flat-file"></a>Vie Tietuetiedostoon

Viedä muodostaminen tietuetiedostoon tehdään kaltaisia kohdassa [Vie Tietuetiedostoon](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>Siirron ohjattu SQL-tietokanta

Ohjatun SQL-tietokannan siirron luomisen vaiheet ovat kaltaisia kohdassa [Siirron ohjattu SQL-tietokantaan](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

###<a name="db-migration"></a>Tietokannan takaisin ylös ja palauttaminen

[Tietokannan varmuuskopioiminen ja palauttaminen](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup)tarkoitettujen muistuttavat käyttämällä tietokannan varmuuskopiointi ja palauttaminen.

###<a name="adf"></a>Azure Data Factory

[Siirtää tietoja SQL Azure Azure Data Factory kanssa, paikallinen SQL-palvelimesta](machine-learning-data-science-move-sql-azure-adf.md)artikkelissa annetaan ohjeet tietojen siirtämistä Azure SQL-tietokantaan Azure tietojen Factory (SYÖTTÖ). Tässä ohjeaiheessa esitellään siirtäminen tietojen paikallinen SQL Server-tietokantaan Azure SQL-tietokantaan Azure-Blob-säiliö kautta käyttämällä SYÖTTÖ. 

Kannattaa käyttää SYÖTTÖ, kun tiedot on siirrettävä jatkuvasti hybrid tilanne, jossa käyttää sekä paikallisia että cloud resurssit ja tiedot on tapahtumallinen tai tarvitsee voi muokata tai lisätä siihen, kun jätettävien liiketoimintalogiikan. SYÖTTÖ mahdollistaa ajoitus ja käyttämällä yksinkertaisia JSON-komentosarjoja, joilla hallitaan tietojen säännöllisin väliajoin liikkuvuuteen töiden seuranta. SYÖTTÖ on myös muita ominaisuuksia, kuten tuki monimutkaisia toimintoja.




