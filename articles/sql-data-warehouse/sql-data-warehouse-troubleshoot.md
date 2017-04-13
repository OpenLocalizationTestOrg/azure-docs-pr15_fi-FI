<properties
   pageTitle="Azure SQL-tietovarasto vianmääritys | Microsoft Azure"
   description="Vianmääritys Azure SQL-tietovarasto."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Azure SQL-tietovarasto vianmääritys

Tässä artikkelissa on lueteltu joitakin olemme kuulla asiakkaidemme tavallisimmista vianmäärityksen kysymyksiin.

## <a name="connecting"></a>Yhteyden muodostaminen

| Ongelma                              | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| Kirjautuminen epäonnistui käyttäjän NT-hallinta\Anonyymi kirjautuminen. (Microsoft SQL Server-virhe: 18456) | Tämä virhe ilmenee, kun AAD-käyttäjä yrittää master-tietokantayhteyden muodostamisessa, mutta ei ole käyttäjän perustyyli.  Voit korjata ongelman joko määrittää SQL-tietovarasto haluat muodostaa yhteyttä muodostettaessa tai Lisää käyttäjä tietokannasta.  Lisätietoja on artikkelissa [Yleistä suojauksesta][] artikkelista.|
|Palvelin pääasiallista "Tämä" ei ole voivat käyttää tietokantaa "perustyyli" suojauksen nykyisessä kontekstissa. Käyttäjän oletusarvon tietokantaa ei voi avata. Kirjautuminen epäonnistui. Kirjaudu sisään käyttäjän 'Tämä' epäonnistui. (Microsoft SQL Server-virhe: 916) | Tämä virhe ilmenee, kun AAD-käyttäjä yrittää master-tietokantayhteyden muodostamisessa, mutta ei ole käyttäjän perustyyli.  Voit korjata ongelman joko määrittää SQL-tietovarasto haluat muodostaa yhteyttä muodostettaessa tai Lisää käyttäjä tietokannasta.  Lisätietoja on artikkelissa [Yleistä suojauksesta][] artikkelista.|
| CTAIP virhe                        | Tämä virhe voi ilmetä, kun kirjautumisen on luotu perusmuodon SQL server-tietokantaan, mutta ei ole SQL-tietovarasto tietokannassa.  Jos tämä virhe ilmenee, katso [Suojauksen yleiskatsaus][] on artikkelissa.  Tässä artikkelissa kerrotaan, miten voit luoda kirjautuminen ja käyttäjän luominen perustyyli ja sitten käyttäjän luominen tietovarasto SQL-tietokantaan.|
| Palomuuri estää                |Azure SQL-tietokantoja on suojattu palvelin ja tietokanta tason palomuurit, jotta ainoastaan tunnetut IP-osoitteiden on pääsy tietokantaan. Palomuurit ovat suojattuja oletusarvon, mikä tarkoittaa, että sinun on otettava erikseen ja IP-osoitteeseen tai osoitteiden alueen, ennen kuin voit muodostaa yhteyden.  Määrittää palomuurin käytön, [Määritä palvelimen palomuurin käyttö asiakkaan IP-osoite][] [Provisioning ohjeita][]noudattamalla.|
| Pysty muodostamaan yhteyttä työkalua tai ohjain | SQL-tietovarasto suosittelee kyselyn tietojen [SSMS][]ja [Visual Studio 2015 SSDT][] [sqlcmd][] avulla. Lisätietoja ohjaimet ja yhteyden muodostaminen SQL-tietovarasto on [Azure SQL-tietovarasto ohjaimet][] ja [Muodosta yhteys SQL Azure-tietovarasto][] artikkeleissa.|


## <a name="tools"></a>Työkalut

| Ongelma                              | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio object Explorerissa puuttuu AAD käyttäjille | Tämä on tunnettu ongelma.  Voit kiertää tämän ongelman tarkastella käyttäjät [sys.database_principals][].  Katso lisätietoja Azure Active Directory käyttäminen SQL-tietovarasto [Azure SQL-tietovarasto todennusta][] .|

## <a name="performance"></a>Suorituskyky

|  Ongelma                             | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| Kyselyn suorituskyvyn vianmääritys  | Jos yrität vianmääritys tietyn kyselyn, aloita [opettelemalla kyselyissä seurannassa][].|
| Heikko kyselyn suorituskykyä ja suunnitelmat usein on saatu puuttuu tilastotiedot   | Yleisimmät huonosti aiheuttaa puutteen taulukoiden tilastoja.  Katso [Maintaining taulukon tilastotiedot] [ Statistics] lisätietoja tilastotiedot luomisesta ja miksi ne ovat tärkeitä suorituskyvyn.|
| Pieni samanaikainen / kyselyt jonossa   | Tietoja [kuormituksen hallinta][] on tärkeää, kuinka saldo muistin varaamista samanaikainen kanssa.|
| Ottamisesta käyttöön parhaat käytännöt    | Parhaan Aseta Aloita ja tutustu tapoja parantaa kyselyn suorituskykyä on artikkelissa [SQL-tietovarasto parhaita käytäntöjä][] .|
| Voit parantaa suorituskykyä ja skaalaus  | Joskus ratkaisu suorituskyvyn parantaminen on Lisää Lisää Laske power kyselyissä mukaan [Skaalaus SQL-tietovarasto][].|
| Tuloksena heikko indeksi laatu heikko kyselyn suorituskykyä | Toisinaan kyselyt voi Hidastamista [indeksi laatu heikko columnstore][]vuoksi.  Katso lisätietoja tästä artikkelista ja [muodostaa indeksejä parantaa segmentin laatua][].|

## <a name="system-management"></a>Järjestelmänhallinta

|  Ongelma                             | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| Virhesanoma 40847: Ei voi suorittaa toiminnon, koska palvelimen ylittää sallitun tietokannan tapahtuman yksikön kiintiö 45000. | Pienennä [DWU][] tietokanta, jota yrität luoda tai [Pyydä kiintiön Suurenna][].|
| Käsittelevälle tilan käyttö    | Katso tietoja järjestelmän tilan käyttö [taulukon kokoa][] .|
| Taulukoiden hallinnan ohje          | Katso [taulukon yleistä] [ Overview] artikkelin ohjeita taulukoiden hallinta.  Tässä artikkelissa on linkkejä myös yksityiskohtaisempia aiheisiin, kuten [taulukon tietotyypit][Data types], [jakaminen taulukon][Distribute], [indeksointi taulukon][Index], [jakaminen taulukon][Partition], [Maintaining taulukon tilastotiedot] [ Statistics] ja [väliaikaisten taulukoiden][Temporary].|

## <a name="polybase"></a>Polybase

|  Ongelma                             | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8-virhe                        |  Tukee tällä hetkellä PolyBase vain ladattaessa tiedostoja, jotka on koodattu UTF-8.  Katso ohjeita siitä, miten voit kiertää tämän rajoituksen [toimi PolyBase UTF-8 vaatimus ympärille][] .|
| Lataaminen epäonnistuu vuoksi suuri rivit   | Tällä hetkellä suuri rivin tuki ei ole käytettävissä Polybase.  Tämä tarkoittaa, että jos taulukko sisältää VARCHAR(MAX) ja VARBINARY(MAX) NVARCHAR(MAX), ulkoiset taulukot ei voi käyttää tietojen lataaminen.  Lataa suuri rivien ei tällä hetkellä tuetaan vain Azure Data Factory (kanssa BCP), Azure Stream Analytics, SSIS, BCP tai .NET SQLBulkCopy-luokka. Suuri rivien PolyBase tuki lisätään uuteen versioon.|
| epäonnistui tuntemattomasta BCP kuormituksen taulukko, jossa on MAX-tietotyyppi | On tunnettu ongelma, joka edellyttää, että VARCHAR(MAX) ja VARBINARY(MAX) NVARCHAR(MAX) sijoitetaan joissakin tilanteissa taulukon lopussa.  Siirrä MAX-sarakkeet taulukon loppuun.|

## <a name="differences-from-sql-database"></a>SQL-tietokantaan erot

|  Ongelma                             | Tarkkuus                                      |
| :----------------------------------| :---------------------------------------------- |
| SQL-tietokannan ominaisuuksia  | Katso [taulukkotoimintoja ei tueta][].|
| Ei tueta SQL-tietokanta-tietotyypit  | Katso [ei-tuetut tietotyypit][].|
| Poista- ja UPDATE rajoituksia      | Katso [päivityksen menetelmää][], [Poista vaihtoehtoisista menetelmistä][] ja [CTAS avulla voit kiertää ei tueta Päivitä ja poista syntaksia][].  |
| Yhdistä-lauseessa ei tueta   | Katso [Yhdistäminen menetelmää][].|
| Tallennetun toimintosarjan rajoitukset       | Katso [tallennetut toimintosarjan rajoitukset][] selvittääksesi, tallennettujen toimintosarjojen liittyviä rajoituksia.|
| UDF eivät tue SELECT-lauseet | Tämä on Microsoftin UDF nykyinen rajoitus.  Katso [Luominen FUNKTION][] syntaksi sovellus tukee.   |

## <a name="next-steps"></a>Seuraavat vaiheet

Jos ole ei löydy ratkaisun ongelmaasi edellä, seuraavassa on joitakin muita resursseja, voit kokeilla.

- [Blogeja]
- [Ehdottaa ominaisuuksia]
- [Videot]
- [KISSA ryhmän blogeihin]
- [Luo tuki lippu]
- [MSDN-keskustelupalsta]
- [Pinon ylivuoto-keskustelupalsta]
- [Twitter-]

<!--Image references-->

<!--Article references-->
[Yleistä suojauksesta]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[Visual Studio 2015 SSDT]: ./sql-data-warehouse-install-visual-studio.md
[Azure SQL-tietovarasto ohjaimet]: ./sql-data-warehouse-connection-strings.md
[Yhteyden muodostaminen Azure SQL-tietovarasto]: ./sql-data-warehouse-connect-overview.md
[Luo tuki lippu]: ./sql-data-warehouse-get-started-create-support-ticket.md
[SQL-tietovarasto skaalaus]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Pyydä kiintiön Suurenna]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Miten seurannassa kyselyissä]: ./sql-data-warehouse-manage-monitor.md
[Ohjeita valmistelu]: ./sql-data-warehouse-get-started-provision.md
[Asiakkaan IP-osoite palvelimen palomuuri käytön määrittäminen]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[SQL-tietovarasto parhaat käytännöt]: ./sql-data-warehouse-best-practices.md
[Taulukon kokoa]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Taulukkotoimintoja ei tueta]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Ei-tuetut tietotyypit]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Heikko columnstore indeksi laatu]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Muodostaa indeksejä segmentin laadun parantamiseksi]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Kuormituksen hallinta]: ./sql-data-warehouse-develop-concurrency.md
[CTAS avulla voit kiertää ei tueta Päivitä ja poista syntaksi]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Päivitä menetelmää]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Poista menetelmää]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Yhdistä menetelmää]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Tallennetun toimintosarjan rajoitukset]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Azure SQL-tietovarasto-todennus]: ./sql-data-warehouse-authentication.md
[Toimi PolyBase UTF-8 vaatimus ympärille]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[FUNKTION LUOMINEN]: https://msdn.microsoft.com/library/mt203952.aspx
[SQLCMD]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogit]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[KISSA ryhmän blogit]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Ehdottaa ominaisuuksia]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-keskustelupalsta]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Pinon ylivuoto-keskustelupalsta]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter-]: https://twitter.com/hashtag/SQLDW
[Videot]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

