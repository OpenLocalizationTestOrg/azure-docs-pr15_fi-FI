<properties
   pageTitle="Tietoja varaston kuormituksen"
   description="SQL tietovarasto joustavuus voit kasvaa, Pienennä tai keskeyttää Laske power Punnitse data warehouse yksiköiden (DWUs) avulla. Tässä artikkelissa kerrotaan data warehouse arvot ja miten ne liittyvät DWUs. "
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
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Tietoja varaston kuormituksen
Tiedot-varaston kuormituksen viittaa kaikista toiminnoista, joita transpire vastaan tietovarasto. Tietoja varaston kuormituksen kattaa koko prosessin tietojen lataaminen varastoon, analysointia ja raportoinut tietovarasto, tietovarasto tietojen hallinta ja tietojen vieminen tietovarasto. Syvyyttä ja kaikki nämä osat ovat usein soveltuva tietovarasto erääntymispvm taso.


## <a name="new-to-data-warehousing"></a>Oletko uusi Tietovarastointi?
Tietovarasto on tietoja, jotka on ladattu vähintään yksi tietolähteistä ja voidaan suorittaa liiketoimintatehtäviä raportointi ja tietojen analysointi esimerkiksi kokoelma.

Tietoja varastoja varaavat kyselyitä, jotka Skannaa suurempi määrä rivejä, suuri tietoalueiden ja voi palauttaa suhteellisen suuri tulokset tai analysointia ja raportointia varten. Tietoja varastoja varaavat myös suhteellisen suurien tietomäärien latautuu ja pieni tapahtuman tason Lisää/päivitykset tai poistaa.

- Tietovarasto toimii parhaiten, kun tiedot on tallennettu niin, että optimoi kyselyitä, jotka on paljon rivejä tai suuri tietoalueiden etsimistä varten. Tämän tyyppistä tarkistus toimii parhaiten, kun tiedot on tallennettu ja etsinnän sarakkeittain, rivien sijaan.

>[AZURE.NOTE] Ladatun columnstore indeksin, joka käyttää sarakkeen tallennustilan on perinteinen binaarinen puut päälle enintään 10 x pakkaamisen voittoja ja 100 x kyselyn suorituskykyä voitot raportoinnin ja analysoinnin kyselyjen. Olemme harkitse columnstore indeksit tallentamiseen ja scanning suurien tietomäärien tietojen varastossa vakioasetuksena.

- Tietovarasto on erilaisia vaatimuksia kuin järjestelmä, joka optimoi online tapahtuman käsittely (OLTP). OLTP järjestelmässä on monia lisätä, päivittää ja poistaa toimintoja. Näitä toimintoja Etsi tietyn taulukon rivit. Taulukon pyritään suorittaa parhaiten, kun tiedot on tallennettu riveittäin tavalla. Tiedot voidaan lajitella nopeasti etsitään kanssa jakaa ja nyt nimeltään binaarinen puun tai btree haun lähestymistapa jäsentää.


## <a name="data-loading"></a>Tietojen lataaminen
Tietojen lataaminen on suuri osa data warehouse työmäärää. Yritysten on yleensä varattu OLTP-järjestelmään, joka seuraa muutoksia koko päivän, kun asiakkaat tuottavat business tapahtumat. Määräajoin usein yöllä aikoina, tapahtumat ovat siirrettyjen tai kopioitujen tietojen varastoon. Kun tiedot on tietovarasto, analyytikot voit analysoida ja tehdä liiketoimintapäätösten tiedot.

- Prosessi, jossa ladataan kutsutaan perinteisesti ETL Pura, muunna ja lataa. Tiedot on yleensä voidaan muuntaa yhdenmukaisesti tietovarasto muilla tiedoilla. Aiemmin yritysten suorittamiseen muunnokset käytetty erillinen ETL-palvelimiin. Nyt kanssa nopeasti erittäin rinnakkain käsittely voit ladata tietoja SQL-tietovarasto ensin ja suorita sitten muunnokset. Tätä prosessia kutsutaan Pura, lataa ja muunna (ELT) ja on jatkossa uuden standardin tietojen varaston kuormituksen.

> [AZURE.NOTE] SQL Server 2016: ssa voidaan nyt suorittaa analytics-reaaliaikainen OLTP-taulukkoon. Tämä ei korvaa tietovarasto tallentamiseen ja analysoida tietoja ei tarvita, mutta siinä voi analysoida reaaliajassa.

### <a name="reporting-and-analysis-queries"></a>Raportointi ja analysointi-kyselyt
Raportointi ja analysointi kyselyt ovat usein luokitella pieni, Normaali ja suuri perusteella useita ehtoja, mutta perustuu aika. Useimmat tietojen varastoja on erilaiset työmäärää, fast-käynnissä ja kyselyissä. Kaikissa tapauksissa on tärkeää selvittää erilaiset ja määrittää sen taajuus (hourly, päivä, kuukausi-loppu, vuosineljänneksen loppu ja niin edelleen). On tärkeää ymmärtää, että erilaiset kyselyn, työmäärää sekä samanaikainen, liidin ERISNIMI kapasiteettiin data warehouse suunnitteleminen.

- Kapasiteetin suunnittelu voi olla erilaiset kysely-kuormituksen monimutkaista, varsinkaan jos tarvitset pitkä limitysaika kapasiteetin lisääminen tietovarasto. SQL-tietovarasto poistaa kapasiteetin suunnittelemisesta kiireellinen jälkeen voit suurennus ja pienennys Laske kapasiteettia, milloin tahansa ja tallennustilaa jälkeen ja Laske kapasiteetin itsenäisesti esitystapa.

### <a name="data-management"></a>Tietojen hallinta
Tietojen hallinta on tärkeää, erityisesti silloin, kun tiedät saattaa toimia levytila on lähitulevaisuudessa. Tietoja varastoja jakaa tietoja yleensä kuvaava alueet, jotka on tallennettu nimellä osioiden taulukon. Kaikkien SQL Server-pohjaisten tuotteiden avulla voit siirtää osiot ja siitä uloskirjautuminen taulukon. Siirrä vanhat tiedot vähemmän kallista tallennustilaa ja jätä käytettävissä viimeisimmät tiedot online-tallennustila tämän osion vaihtaminen avulla.

- Columnstore indeksit tue osioitua taulukoita. Indeksien columnstore osioitua taulukkoja käytetään tietojenhallintaa ja arkistointia. Osiot ovat suurempia roolin taulut tallennettu rivi riviltä-, kyselyn suorituskykyä.  

- PolyBase toistaa on tärkeä rooli tietojen hallinta. Käytä PolyBase, sinulla voi arkistoida vanhat tiedot Hadoop tai Azure-Blob-objektien tallennustilaan.  Tämä on paljon asetukset, koska tiedot ovat silti online.  Saattaa kestää kauemmin tietojen hakemiseen Hadoop, mutta noutaminen aika tarjoa ehkä merkittävämpiä kuin aiheutuvat tallennustilan kustannukset.

### <a name="exporting-data"></a>Tietojen vieminen
Voit tarkastella tietoja offline-raportit ja analyysi on lähettää tietoja tietovarasto palvelinten erillinen raportit ja analyysi suorittamista varten. Seuraavissa palvelimissa kutsutaan tietojen marts. Voit esimerkiksi esikäsittely raporttitiedot ja viedä sen tietovarasto useita eri puolilla maailmaa, jotta se laajasti käytettävissä asiakkaille sekä analyytikot-palvelimiin.

- Raporttien luomista varten jokaisen vuorokauden voit voi lisätä vain luku-raportoinnin palvelinten tilannevedoksen päivittäiset tiedot. Näin suuremman kaistanleveyden asiakkaiden Pienennä-tietovarasto Laske resurssin tarpeisiin. Valitse Suojaus-osa tietojen marts avulla voit vähentää tietovarasto käyttävien käyttäjien lukumäärä.
- Analysoinnissa voit joko luominen tietovarasto analysis-kuutioon ja suorittaa analyysia tietovarasto, tai esikäsittely tiedot ja vie edelleen analytics analysis-palvelimeen.

## <a name="next-steps"></a>Seuraavat vaiheet
Kun tiedät hieman SQL-tietovarasto, tutustu nopeasti [luoda SQL-tietovarasto][] ja [Lataa mallitiedot][].

<!--Image references-->

<!--Article references-->
[Lataa mallitiedot]: ./sql-data-warehouse-load-sample-databases.md
[SQL-tietovarasto luominen]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
