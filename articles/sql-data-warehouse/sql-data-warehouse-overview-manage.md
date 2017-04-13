<properties
   pageTitle="Azure SQL-tietovarasto tietokantojen hallinta | Microsoft Azure"
   description="Yleiskatsaus SQL-tietovarasto tietokantojen hallintaa. Sisältää hallintatyökalut, DWUs ja skaalaus-kohtaa suorituskyvyn vianmääritys kyselyn suorituskykyä, muodostamisesta hyvä suojauskäytäntöjä ja tietokannan palauttaminen tietovirheitä tai alueellisen käyttökatkosta."
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
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Azure SQL-tietovarasto tietokantojen hallinta

SQL-tietovarasto automatisoi monia tietokantojen hallinta. Esimerkiksi skaalata suorituskyvyn vain haluat säätää ja Laske resurssien oikean suojaustason maksaa ja anna sitten SQL-tietovarasto tehdä tarvittavat työt laajentaminen ja skaalausta takaisin. 

Haluat varmasti havainnollistamiseen tunnistaa suorituskyvyn tarpeitasi sekä vianmääritys kyselyissä seurannassa. Sinun on myös muutamia hallittavan käyttöoikeudet käyttäjille ja kirjautumiset suojauksen tehtävien suorittamiseen.

Tämä yleiskatsaus käsitellään seuraavia ominaisuuksia SQL-tietovarasto hallinta.

- Hallintatyökalut
- Skaalaa suorittaminen
- Keskeyttäminen ja jatkaminen
- Suorituskyvyn parhaat käytännöt
- Kyselyn seuranta
- Tietoturva
- Varmuuskopiointi ja palauttaminen

## <a name="management-tools"></a>Hallintatyökalut

Voit käyttää erilaisia työkaluja SQL-tietovarasto tietokantojen hallitsemiseen. Tietokantojen Hallitessasi kehität kullakin virhelajilla on tehtävä, sinun on suoritettava työkalun asetukset.

### <a name="azure-portal"></a>Azure portal
[Azure portaalissa][] on verkkopohjainen portal, jossa voit luoda, päivittää, ja poista tietokannat ja seurata tietokannan resurssit. Tämä työkalu on hyvä on, jos olet vasta Azure-pieneen data warehouse tietokantojen hallinta kanssa tai sinun tehtävä joitakin nopeasti.

Aloita Azure-portaalissa-kohdasta [Luo SQL-tietovarasto (Azure portaalin)][].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools Visual Studiossa
[SQL Server Data Tools][] (SSDT) Visual Studiossa avulla voit muodostaa yhteyden, hallita ja kehittää tietokantoja. Jos olet tutustunut Visual Studio tai integroitua muiden ympäristöjen (IDEs)-sovelluksen developer, kokeile SSDT Visual Studiossa.

SSDT sisältää SQL Server Object Explorerissa, joiden avulla voit visualisoida, yhdistää ja suorittaa komentosarjoja SQL-tietovarasto tietokannoissa. Voit pitää yhteyttä SQL-tietovarasto, riittää, että napsauttamalla komentopalkin **Avaa Visual Studiossa** -painikkeen tarkasteltaessa tietokannan tietoja perinteinen Azure-portaalissa.  

Aloita SSDT Visual Studiossa, on artikkelissa [Kyselyn Azure SQL-tietovarasto Visual Studiossa][].

### <a name="command-line-tools"></a>Komentorivin Työkalut
Komentorivityökalujen soveltuvat erinomaisesti automatisointi oman toiminnoista.  PowerShellin ja sqlcmd ovat automatisoida prosesseja hyvien kahdella.  On suositeltavaa kyseisten työkalujen hallinta on runsaasti loogista palvelinta ja käyttöönotto Resurssimuutokset tuotantoympäristössä, kuten tehtäviä voit komentosarjatoimintojen ja sitten automaattinen.

### <a name="dynamic-management-views"></a>Dynaaminen hallinta näkymät 

DMVs ovat leipä ja voin SQL-tietovarasto hallintaa. Lähes kaikki tiedot, jotka pintoja portaalissa on riippuvainen DMVs. Lisätietoja SQL Data Warehouse DMVs luettelo on [SQL-tietovarasto järjestelmänäkymiä][].

Aloita-kohdasta [Yhdistä- ja kysely, joka sisältää sqlcmd][]ja [Luo tietokanta (PowerShell)][].

## <a name="scale-compute"></a>Skaalaa suorittaminen

SQL-tietovarasto voit nopeasti skaalata suorituskyvyn ulos tai takaisin Laske resurssien suorittimen ja muistin i/o-kaistanleveyden asetteluissa. Skaalata suorituskyvyn, sinun tarvitsee on Säädä yksiköiden määrän tietojen varasto (DWUs), joka SQL-tietovarasto varaa tietokantaan. SQL-tietovarasto tekee tarvittavat muutokset ja kaikki pohjana olevat muutokset laitteiston ja ohjelmiston käsittelee nopeasti.

Lisätietoja DWUs Skaalaus-kohdassa [asteikko suorituskykyä][].

##  <a name="pause-and-resume"></a>Keskeyttäminen ja jatkaminen

Tallenna kustannukset, keskeyttäminen ja jatkaminen Laske resurssien pyydettäessä. Esimerkiksi jos ei voi tietokannan aikana yöllä ja viikonloppuisin, voit keskeyttää näiden aikoina, ja jatka päivän aikana. DWUs ei voidaan veloittaa tietokannan ollessa keskeytettynä.

Lisätietoja on artikkeleissa [Laske Keskeytä][]ja [ansioluettelon laskemiseen][].

## <a name="performance-best-practices"></a>Suorituskyvyn parhaat käytännöt

Kun uusi tekniikka käytön aloittaminen-vihjeitä ja vinkkejä, jotka toimivat parhaiten oikealle alusta etsiminen säästät paljon aikaa.  Etsii parhaita käytäntöjä koko monia Microsoftin ohjeita.

Monta tärkeimmät seikat yhteenveto kun kehittäminen havainnollistamiseen on ohjeartikkelissa [SQL Data Warehouse parhaita käytäntöjä][].

## <a name="query-monitoring"></a>Kyselyn seuranta

Joskus kyselyn toimii hitaasti, mutta et ole varma, mikä niistä on virheen aiheuttaja. SQL-tietovarasto on dynaaminen hallinta-näkymää (DMVs), joiden avulla voit selvittää, mitkä kyselyn kestää liian kauan. 

Kyselyissä on artikkelissa [valvoa havainnollistamiseen DMVs avulla][].

## <a name="security"></a>Tietoturva

Suojattu järjestelmä pitämään olla ilmoituksen ja kaikenlaisia luvattoman käytön estämiseksi. Järjestelmä on Varmista, että palomuurisäännöt ovat paikoillaan, joten vain valtuutetut IP-osoitteiden voit muodostaa yhteyden. Se on oikea todennusta käyttäjän tunnistetietoja. Kun käyttäjä on yhteydessä tietokantaan, käyttäjän tulee olla vain vähän toimintojen suorittamiseen. Suojaa tiedot, voit käyttää salausta. On tärkeää on tarkistaminen ja seuranta, jotta voit retrace tapahtumat, onko epäilyttävistä tapahtumista.

Lisätietoja suojauksen, pää hallinta päälle [Suojauksen yleiskatsaus][].

## <a name="backup-and-restore"></a>Varmuuskopiointi ja palauttaminen

On luotettava backps koskevista tiedoista on olennainen osa tuotannon minkä tahansa tietokannan. SQL-tietovarasto pitää tietojen turvallisten mukaan automaattisesti varmuuskopioiminen active tietokantoja säännöllisin väliajoin. Nämä varmuuskopiot mahdollistavat skenaarioita, joissa on vioittunut tietojen tai poistetaan vahingossa tietojen tai tietokannan palauttaminen.  Tietojen varmuuskopioinnin ajoituksen säilytyskäytäntö ja palauttaminen tietokantaan, saat [Tilannevedoksen palauttaminen][].

## <a name="next-steps"></a>Seuraavat vaiheet
Hyvä tietokannan rakenne periaatteet helpompi hallita tietovarasto SQL-tietokantoja käytettäessä Jos haluat lisätietoja, pää päälle [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto (Azure portaalin) luominen]: sql-data-warehouse-get-started-provision.md
[Luo tietokanta (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Kyselyn Azure SQL-tietovarasto Visual Studiossa]: sql-data-warehouse-query-visual-studio.md
[Yhdistä ja kysely, jossa on sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Kehittäminen yleiskatsaus]: sql-data-warehouse-overview-develop.md
[Valvoa havainnollistamiseen DMVs käyttäminen]: sql-data-warehouse-manage-monitor.md
[Keskeytä suorittaminen]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Tilannevedoksen palauttaminen]: sql-data-warehouse-restore-database-overview.md
[Jatka suorittaminen]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Skaalaa suorituskyky]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Yleistä suojauksesta]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse parhaat käytännöt]: sql-data-warehouse-best-practices.md
[SQL-tietovarasto järjestelmänäkymiä]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
