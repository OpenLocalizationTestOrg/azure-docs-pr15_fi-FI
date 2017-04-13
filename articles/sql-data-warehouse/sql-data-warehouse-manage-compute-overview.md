<properties
   pageTitle="Hallitse Laske virtaa Azure SQL-tietovarasto (yleiskatsaus) | Microsoft Azure"
   description="Suorituskyvyn mittakaava Azure SQL-tietovarasto-ominaisuuksien ulos. Skaalaa ulos säätämällä DWUs tai keskeyttää ja jatkaa Laske resurssien kustannusten tallentamiseen."
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
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Laske power Azure SQL-tietovarasto (yleiskatsaus)-hallinta

> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShellin](sql-data-warehouse-manage-compute-powershell.md)
- [MUILLE KÄYTTÄJILLE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

SQL-tietovarasto arkkitehtuuri erottaa säilytys- ja Laske-salliminen kunkin skaalata erikseen. Tuloksena voi skaalata ulos suorituskyvyn tallennettaessa kustannusten mukaan vain maksaa suorituskyvyn, kun tarvitse sitä. 

Tämä yleiskatsaus kuvataan SQL-tietovarasto seuraavia suorituskyky-asteikko-kohtaa-ominaisuuksia ja antaa suosituksia ja käyttää niitä. 

- Skaalaa Laske power säätämällä [tietojen varasto yksiköt (DWUs)][]
- Laske resurssien keskeyttäminen ja jatkaminen

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Skaalaa suorituskyky

SQL-tietovarasto voit nopeasti skaalata suorituskyvyn ulos tai takaisin Laske resurssien suorittimen ja muistin i/o-kaistanleveyden asetteluissa. Skaalata suorituskyvyn, sinun tarvitsee on [tietoja varasto yksiköt (DWUs)][] , joka SQL-tietovarasto varaa tietokantaasi määrän. SQL-tietovarasto tekee tarvittavat muutokset ja kaikki pohjana olevat muutokset laitteiston ja ohjelmiston käsittelee nopeasti.

Ovat tarvittaessa tutkiminen tyypin suorittimen, muistin tai tyypin tallennustilaa, sinun täytyy erinomainen suorituskyky on tietoja varastossa. Sijoittamalla pilveen Data Warehouse ei enää sinulla käsitellä alatason laitteisto-ongelmat. Sen sijaan SQL-tietovarasto kysyy kysymys: kuinka nopeasti haluat analysoida tietoja? 

### <a name="how-do-i-scale-performance"></a>Miten suorituskyvyn skaalata?

Elastically suurentaa tai pienentää Laske power, voit muuttaa tietokannan [tietoja varasto yksiköt (DWUs)][] -asetus. Suorituskyvyn kasvattaa lineaarisesti, kun lisäät useita DWU.  Suurempi DWU tasolla sinun on lisättävä yli 100 DWUs huomaat parantaa suorituskykyä merkittävästi. Avulla voit valita kuvaava viivanylitys DWUs Tarjoamme avulla parhaiten DWU haluamasi tasot.
 
Voit säätää DWUs, voit käyttää yksittäisiä seuraavasti.

- [Skaalaa Laske power Azure-portaalissa][]
- [Skaalaa Laske power PowerShellin avulla][]
- [Skaalaa Laske power REST API kanssa][]
- [Skaalaa Laske power TSQL kanssa][]

### <a name="how-many-dwus-should-i-use"></a>Kuinka monta DWUs kannattaa käyttää?
 
SQL-tietovarasto suorituskyvyn Skaalaa lineaarisesti ja yhdessä Laske asteikon muuttaminen toiseen (Yammer-100 DWUs, 2000 DWUs) tapahtuu sekunnin kuluttua. Tämä lisää joustavuutta kokeilemaan eri DWU asetuksia, kunnes olet selvittänyt käyttämässäsi skenaariossa Sovita.

Saat lisätietoja ihanteellinen DWU-arvo on, yritä skaalaus ylös ja alas sekä käynnissä muutama kysely tietojen lataamisen jälkeen. Skaalaus on nopea, voit kokeilla suorituskyvyn tunnin tai vähemmän eri tasojen määrä. Ota huomioon, että SQL-tietovarasto on suunniteltu käsittelemään suuria tietomääriä ja nähdäksesi sen tosi ominaisuuksia skaalauksen, erityisesti Tarjoamme suurempi Skaalaa, milloin kannattaa käyttää suuren tietojoukon joka lähestyy tai suurempi kuin 1 TT.

Suosituksia löytäminen parhaat DWU havainnollistamiseen:

1. Tietoja varastossa kehitystä Aloita valitsemalla DWUs pieni määrä.  Hyvä aloitusarvo on DW400 tai DW200.
2. Sovelluksen suorituskyvyn seuranta, noudattaen DWUs määrän valittuna verrattuna huomaat suorituskykyä.
3. Määrittää, kuinka paljon aiempaa nopeammin tai hitaammin suorituskyky olisi puolestasi saavuttaa parhaan mahdollisen suorituskyvyn tason tarpeidesi mukaan olettaen lineaarisen asteikon.
4. Suurenna tai Pienennä DWUs määrä suhteessa, miten paljon nopeammin tai hitaammin haluat suorittaa havainnollistamiseen. Palvelun vastaa nopeasti ja muuttaa uuden DWU vaatimukset Laske-resursseja.
5. Jatka muutosten tekeminen, kunnes pääset parhaan mahdollisen suorituskyvyn tason for business-järjestelmävaatimukset.

### <a name="when-should-i-scale-dwus"></a>Kun DWUs olisi skaalata?

Tarvittaessa voit nopeuttaa tulosten Suurenna oman DWUs ja maksaa suurempi suorituskykyä.  Kun on vähemmän Laske power, pienennä oman DWUs ja maksaa vain etsimääsi tietoa. 

Suositukset, milloin haluat skaalata DWUs:

1. Jos sovellus on vaihtelevia työmäärää asteikko DWU Tasaa ylös- tai alaspäin mahtuu päät ja pienen pistettä. Esimerkiksi havainnollistamiseen yleensä päät kuukauden lopussa, jos aiot lisätä Lisää DWUs näiden huippu-päivän aikana ja valitse skaalata, kun piikin kuluessa.
2. Ennen kuin teet paljon tietojen lataaminen tai muunnos toimen, skaalata DWUs määrittäminen niin, että tiedot ovat nopeasti käytettävissä.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Keskeytä suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Voit pysäyttää tietokannan käyttää yksittäisiä seuraavista tavoista.

- [Keskeytä suorittaminen Azure-portaalissa][]
- [Keskeytä suorittaminen PowerShellin avulla][]
- [Keskeytä suorittaminen REST API kanssa][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Jatka suorittaminen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Jatka tietokannan käyttää yksittäisiä seuraavista tavoista.

- [Jatka Laske Azure-portaalissa][]
- [Jatka Laske PowerShellin avulla][]
- [Jatka Laske REST API kanssa][]

## <a name="permissions"></a>Käyttöoikeudet

Tietokannan skaalaus edellyttävät käyttöoikeuksien [Muuta TIETOKANTAA][]kuvattu.  Keskeyttäminen ja jatkaminen edellyttävät [SQL DB osallistuja][] -oikeudet, erityisesti Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Seuraavat vaiheet
Lue lisätietoja voi auttaa sinua ymmärtämään joitakin muita suorituskyvyn käsitteitä on seuraavissa artikkeleissa:

- [Työmäärän ja samanaikainen hallinta][]
- [Taulukko rakenne yleiskatsaus][]
- [Taulun jakelu][]
- [Taulukon indeksointi][]
- [Taulukon jakaminen][]
- [Taulukon tilastotiedot][]
- [Parhaat käytännöt][]

<!--Image reference-->

<!--Article references-->
[varaston yksiköiden (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Skaalaa Laske power Azure-portaalissa]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Skaalaa Laske power PowerShellin avulla]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Skaalaa Laske power REST API kanssa]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Skaalaa Laske power TSQL kanssa]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Keskeytä suorittaminen Azure-portaalissa]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Keskeytä suorittaminen PowerShellin avulla]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Keskeytä suorittaminen REST API kanssa]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Jatka Laske Azure-portaalissa]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Jatka Laske PowerShellin avulla]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Jatka Laske REST API kanssa]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Työmäärän ja samanaikainen hallinta]: ./sql-data-warehouse-develop-concurrency.md
[Taulukko rakenne yleiskatsaus]: ./sql-data-warehouse-tables-overview.md
[Taulun jakelu]: ./sql-data-warehouse-tables-distribute.md
[Taulukon indeksointi]: ./sql-data-warehouse-tables-index.md
[Taulukon jakaminen]: ./sql-data-warehouse-tables-partition.md
[Taulukon tilastotiedot]: ./sql-data-warehouse-tables-statistics.md
[Parhaat käytännöt]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB avustaja]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[MUUTTAA TIETOKANTAA]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
