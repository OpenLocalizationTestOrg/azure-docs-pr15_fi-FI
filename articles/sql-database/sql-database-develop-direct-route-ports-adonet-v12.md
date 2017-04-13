<properties 
    pageTitle="SQL-tietokantaan 1433 lisäksi portit | Microsoft Azure"
    description="Joskus asiakasyhteydet ADO.NET Azure SQL-tietokannan Vipuventtiili V12 ohittaa välityspalvelimen ja käsitellä tietokannan. Portit kuin 1433 ole tärkeä."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12 portit


Tässä ohjeaiheessa kerrotaan muutokset, jotka Azure SQL-tietokannan Vipuventtiili V12 tuo yhteyden toimintaa asiakkaat, jotka käyttävät ADO.NET 4.5 tai sitä uudempi versio.


## <a name="v11-of-sql-database-port-1433"></a>SQL-tietokantaan V11: 1433 portti


Kun asiakasohjelma käyttää ADO.NET 4.5 yhteyden ja kysely, jossa on SQL-tietokannan V11, sisäinen jaksossa on seuraavanlainen:


1. ADO.NET yrittää muodostaa yhteyden SQL-tietokantaan.

2. ADO.NET käyttää porttia 1433 Soita middleware moduuli, ja middleware muodostaa yhteyden SQL-tietokantaan.

3. SQL-tietokantaan lähettää sen takaisin middleware, joka välittää ADO.NET vastauksen 1433-porttiin.


**Termejä:** Olemme kuvaavat edellisessä jaksossa sanomalla, että ADO.NET käyttämällä *välityspalvelimen reitittää*toimii SQL-tietokannan kanssa. Jos ei ole middleware viimeaikaisista, emme sano *suoran reitin* on käytetty.


## <a name="v12-of-sql-database-outside-vs-inside"></a>SQL-tietokannan Vipuventtiili V12: ulkopuolella ja


Yhteyksien Vipuventtiili V12 on pyydämme asiakasohjelman suoritetaanko *sisä* - tai *ulkopuolelle* Azure cloud reunaa. Jaksoissa käsitellään kaksi Yleisiä tilanteita, joissa.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Ulkopuolella:* Asiakkaan suoritetaan paikallisessa tietokoneessa


Porttia 1433 on vain portti, joka on oltava avoinna pöytätietokoneessa, joka isännöi SQL-tietokanta-asiakassovellukseen.


#### <a name="inside-client-runs-on-azure"></a>*Sisällä:* Asiakas toimii Azure


Kun asiakkaan suoritetaan Azure cloud reunan sisällä, se käyttää mitä emme voi soittaa *suoran reitin* SQL-tietokantapalvelinta vuorovaikutuksessa. Kun yhteys on muodostettu, edelleen asiakkaan ja tietokannan vuorovaikutuksesta liittyä middleware välityspalvelinta.


Jaksossa on seuraavanlainen:


1. ADO.NET 4,5 (tai uudempi) aloittaa Azure pilveen lyhyt käsittelemisen ja vastaanottaa dynaamisesti tunnistettujen porttinumero.
 - Dynaamisesti tunnistettujen porttinumero on 11000 11999 tai 14000 14999 solualue.

2. ADO.NET sitten yhdistää SQL-tietokantapalvelimeen suoraan ei middleware välillä.

3. Kyselyt lähetetään suoraan tietokannan ja tulokset palautetaan suoraan asiakkaalle.


Varmista, että alueet 11000 11999 ja Azure asiakaskoneeseen 14000-14999 jäävät käytettävissä ADO.NET 4.5 asiakkaan aktiviteettien SQL-tietokannan Vipuventtiili V12 portti.

- Erityisesti portit alueella on oltava lähtevä sallittu vapaa.

- Azure-AM, valitse **Windowsin laajennettu palomuuri** ohjaa portin asetuksia.
 - [Palomuurin käyttöliittymän](http://msdn.microsoft.com/library/cc646023.aspx) avulla voit lisätä säännön, jonka määrität syntaksin, kuten **11000 11999**Port ()-porttialueen sekä **TCP** -protokollaa.


## <a name="version-clarifications"></a>Version selvennykset


Tässä osassa selkeyttää monikerit, jotka viittaavat versiota. Myös joitakin parit tuotteiden välillä versioiden luettelo.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 tukee TKJ 7.3 protokolla, mutta ei 7.4.
- ADO.NET 4,5 ja uudempi versio tukee TKJ 7.4-protokollaa.


#### <a name="sql-database-v11-and-v12"></a>SQL-tietokannan V11 ja Vipuventtiili V12


Asiakkaan yhteyden SQL-tietokannan V11 ja eroista Vipuventtiili V12 näkyvät korostettuina tämän ohjeaiheen.


*Huomautus:* Transact-SQL-lause `SELECT @@version;` palauttaa arvon, jotka alkavat numeroon, esimerkiksi "11". tai '12.', ja ne vastaavat V11 ja Vipuventtiili V12 versio Microsoftin nimet SQL-tietokantaan.


## <a name="related-links"></a>Aiheeseen liittyvät linkit


- ADO.NET 4.6 julkaistiin 2015 20 heinäkuussa. Blogi-ilmoitus .NET kuulumisia on saatavilla [tähän](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 julkaistiin August 15, 2012. Blogi-ilmoitus .NET kuulumisia on saatavilla [tähän](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Blogimerkintä ADO.NET 4.5.1 tietoja on saatavilla [tähän](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [TKJ protokolla versio-luettelosta](http://www.freetds.org/userguide/tdshistory.htm)


- [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)


- [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md)


- [Toimintaohje: SQL-tietokantaan palomuurin asetusten määrittäminen](sql-database-configure-firewall-settings.md)

