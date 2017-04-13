<properties
   pageTitle="Jakaa tietoja ja jakaa taulukon asetukset SQL-tietovarasto ja Parallel Data Warehouse erittäin rinnakkain käsittelyn MPP-Systemsin | Microsoft Azure"
   description="Katso, kuinka tiedot jaetaan erittäin rinnakkain käsittelyn MPP ja jakamiseksi taulukoita SQL Azure-tietovarasto ja Parallel Data Warehouse asetukset."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Hajautettu tietojen ja hajautettu taulukoiden varten erittäin rinnakkain käsittelyn MPP

Lue, miten käyttäjätiedot jaetaan Azure SQL-tietovarasto ja Parallel Data Warehouse, jotka ovat Microsoftin erittäin rinnakkain käsittelyn MPP-järjestelmiä. Suunnitteleminen tietojen varastossa tehokas käyttö tietojen avulla voit käsittelyn MPP-arkkitehtuuri edut kyselyn saavuttamiseksi. Joitakin tietokannan rakenteen vaihtoehtoja voi olla merkittävästi parantaa kyselyn suorituskykyä vaikutus.  

>[AZURE.NOTE] Azure SQL-tietovarasto ja Parallel Data Warehouse käyttää samaa erittäin rinnakkain käsittelyn MPP-rakenne, mutta niissä on muutamia pohjana ympäristö vuoksi. SQL-tietovarasto on palveluna (PaaS), joka suoritetaan Azure. Parallel Data Warehouse suorittaa-Analytics Platform järjestelmän (‑sovellukset), joka suoritetaan, Windows Server paikallisen laitteen eli.

## <a name="what-is-distributed-data"></a>Jaettujen tietojen ominaisuudet

SQL-tietovarasto ja Parallel Data Warehouse jaettujen tietojen viittaa käyttäjätiedot, jotka on tallennettu eri sijainneissa MPP-järjestelmän välillä. Kunkin sijaintien toimii riippumaton tallennuskiintiön ja käsittely-yksikkö, joka suoritetaan sen joidenkin tietojen kyselyt. Jaettujen tietojen on käynnissä kyselyt rinnakkain saavuttamiseksi hyvin kyselyn suorituskykyä.

Voit jakaa tietoja tietovarasto määrittää käyttäjän taulukon kunkin rivin hajautettu yhdessä paikassa.  Voit jakaa taulukoita, joissa hash-jakauman menetelmän tai pyöreän pöydän-menetelmää. CREATE TABLE-lause on määritetty jakauma. 

## <a name="hash-distributed-tables"></a>Hash distributed taulukot
  
Seuraavassa kaaviossa on kuvattu, miten täydellinen (ei jaettu taulukko) tallennetaan hash distributed taulukoksi. Deterministic funktioon määrittää kullekin riville yhden jakauman kuuluu. Taulukko-määrityksessä yhden sarakkeen on määritetty jakauman-sarake. Hash-funktio käyttää arvoa jakauman sarakkeen kunkin rivin liittäminen jakauman.

Jakautuvat suorituskykyyn liittyviä tietoja jakauman sarakkeen valinnan, kuten erotettavuuden, tietojen jakauman.vinous-funktio ja kyselytyypeistä suorittaa järjestelmässä.
  
![Jaettu-taulukossa] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Jaettu-taulukossa")  
  
-   Kullakin rivillä kuuluu yksi jakauman.  
  
-   Deterministic hash-algoritmin määrittää kullekin riville yhden jakauman.  
  
-   Jakauman kohti taulukon rivien määrä vaihtelee taulukoiden erikokoisia osoittamalla tavalla.

## <a name="round-robin-distributed-tables"></a>Pyöreän pöydän hajautettu taulukot

Pyöreän pöydän hajautettu taulukon jakaa rivit määrittämällä kunkin rivin jakauman peräkkäisiä tavalla. On nopea lataa tiedot taulukkoon pyöreän pöydän, mutta kyselyjen suorituskykyä saattaa kestää kauemmin.  Liittyminen pyöreän pöydän taulukon yleensä edellyttää reshuffling rivit käyttöön kyselyn, joka tuottaa tarkkoja tulos, joka kestää jonkin aikaa.

## <a name="distributed-storage-locations-are-called-distributions"></a>Hajautettu tallennuspaikkojen kutsutaan jakelu

Hajautettu paikkoihin kutsutaan jakauman. Kun kysely suoritetaan rinnakkain, kunkin jakauman suorittaa SQL-kyselyn sen osa tiedoista. SQL-tietovarasto käyttää SQL-tietokantaan Suorita kysely. Parallel Data Warehouse käyttää SQL Server Suorita kysely. Saavuttamiseksi asteikko-kohtaa rinnakkain tietojenkäsittely on jaettu mitään arkkitehtuuri-rakenne.

### <a name="can-i-view-the-distributions"></a>Voit tarkastella jaot?

Kunkin jakauman on jakauman tunnus, ja se näkyy järjestelmän näkymissä, jotka koskevat SQL-tietovarasto ja Parallel Data Warehouse. Voit käyttää jakauman tunnus kyselyn suorituskykyä ja muiden ongelmien vianmääritys. Järjestelmänäkymät luettelo on artikkelissa [MPP järjestelmänäkymä](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Jakauman ja Laske-solmu välinen ero

Jakauman on jaettu tietojen tallentamiseen ja käsittelee rinnakkain kyselyt. Laske solmujen on ryhmitelty jaot. Laske jokaisen solmu seuraa vähintään yksi jaot.  

-   Analytics Platform järjestelmä käyttää Laske solmujen keskitetyn osana laitteisto-arkkitehtuuri ja skaalaus-kohtaa ominaisuudet. Se käyttää kahdeksan jaot kohti Laske solmu aina hash distributed taulukoiden kaavio esitetyllä tavalla. Laske solmujen määrän ja näin ollen jaot määrä määräytyy voit ostaa laitteen Laske solmujen määrän. Esimerkiksi jos ostat kahdeksan Laske solmujen, saat 64 jaot (8 Laske solmujen x 8 jaot/solmu). 

-   SQL-tietovarasto on on kiinteä määrä 60 jakelu ja Laske solmujen joustava määrä. Laske-solmut otettu käyttöön Azure laskemista ja tallennustilaa tiedoilla. Laske solmujen määrän muuttaa Taustajärjestelmä palvelun työmäärää ja määrität tietovarasto tietojenkäsittely kapasiteetti (DWUs) mukaan. Laske solmujen määrän muuttuessa jaot kohti Laske solmu määrä muuttuu. 

### <a name="can-i-view-the-compute-nodes"></a>Laske-solmut tarkasteleminen

Laske jokaisen solmu Solmutunnus on, ja se näkyy järjestelmän näkymissä, jotka koskevat SQL-tietovarasto ja Parallel Data Warehouse.  Näet Laske-solmu hakemalla järjestelmänäkymiä, joiden nimet alkavat sys.pdw_nodes node_id-sarakkeessa. Järjestelmänäkymät luettelo on artikkelissa [MPP järjestelmänäkymä](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Taulukoiden replikoida Parallel-tietovarasto varten 
  
Koskee: rinnakkainen tietovarasto

Parallel Data Warehouse on lisäksi hajautettu taulukoiden avulla voi replikoida taulukot. *Replikoida taulukko* on taulukko, joka on tallennettu Laske jokaisen solmun kokonaisuudessaan. Siirtää sen taulukkorivit Laske solmujen ennen kuin käytät taulukon liitoksen tai koostamista ei tarvitse poistaa replikoiminen taulukon. Replikoitua taulukot ovat vain mahdollista pieni taulukoita tallentamiseen koko taulukossa Laske kullekin solmun enemmän tallennustilaa vuoksi.  
  
Seuraavassa kaaviossa on esitetty replikoitua taulukon, joka on tallennettu kunkin Laske-solmun. Replikoitua taulukko on tallennettu kaikkien varattujen Laske-solmu levyille. Tämä levyn strategia on toteutettu FileGroup SQL Server-ryhmien avulla.  
  
![Replikoitu taulukko] (media/sql-data-warehouse-distributed-data/replicated-table.png "Replikoitu taulukko") 
  
## <a name="next-steps"></a>Seuraavat vaiheet
  
Hajautettu taulukoiden tehokkaasti on artikkelissa [Distributing taulukot SQL-tietovarasto](sql-data-warehouse-tables-distribute.md)  
  



  
