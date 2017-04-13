<properties 
   pageTitle="Azure SQL-tietokanta usein kysytyt kysymykset" 
   description="Vastauksia yleisiin kysymyksiin asiakkaille Kysy cloud tietokannat ja Azure SQL-tietokanta, Microsoftin relaatiotietokannasta hallintajärjestelmän (RDBMS) ja tietokannan palveluna pilvipalvelussa." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>SQL-tietokantaan usein kysytyt kysymykset

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Miten SQL-tietokannan käyttö näy laskun? 
SQL-tietokantaan laskut-ennakoitavissa tuntihinta perusteella service-tason + yksittäisen tietokannat tai eDTUs kohti joustavasti tietokannan resurssivarantoon suorituskyvyn tason. Todellinen käyttö on laskettu ja jaettu kerran tunnissa, jotta laskun saattavat näyttää tunnin murtolukuja. Jos tietokanta on olemassa 12 tunnin kuukaudessa, laskun näyttää käyttö 0,5 päivää. Lisäksi palvelutasot + suorituskyky ja eDTUs kohti resurssivarantoon jakaudu laskussa sitä on helpompi käytit yksi kuukausi-tietokannan päivien määrän.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Entä jos yhteen tietokantaan on aktiivinen alle tunnin tai käyttää suurempi palvelutaso alle tunnin?
Ovat laskuttaa kunkin tietokannan on luotu käyttämällä suurin palvelutaso tunti + suorituskyvyn taso, tunti, riippumatta siitä, käyttö- tai onko tietokannan oli aktiivinen alle tunnin aikana. Jos luot yhden tietokannan ja poistaa sen myöhemmin viisi minuuttia laskun vastaa yhtä tietokantaa tunti maksu. 

Esimerkkejä
    
- Jos Basic tietokannan luominen ja päivittäminen se heti vakio S1: een, perittävän vakio S1 korko ensimmäisen tunnin.

- Jos olet päivittänyt tietokannan Basic Premium kello 10:00 ja päivitys on valmis kello 1:35 seuraavat päivänä perittävän käynnistäminen kello 1 Premium korko 

- Jos tietokannan Premium Basic alentaa kello 11 ja se on valmis kello 2:15 ja valitse tietokanta laskutetaan Premium korko asti 3-17:00 minkä jälkeen se on ladattu Basic tahdissa.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Joustavasti tietokannan resurssivarantoon käyttö näkyminen laskun ja mitä tapahtuu, kun muutan eDTUs resurssivarantoon kohden?
Joustavasti tietokannan resurssivarantoon kulujen näyttää sisältökorttina laskun joustavasti DTUs (eDTUs) kohdassa eDTUs kohti resurssivarantoon sivun [hinnoittelutiedot](https://azure.microsoft.com/pricing/details/sql-database/)kerrallaan. Ei joustavasti tietokannan jakavat maksutonta tietokantaa kohden. Sinun on laskuttaa varanto on suurin eDTU riippumatta käyttö- tai onko altaan oli aktiivinen alle tuntia tunnin välein. 

Esimerkkejä

- Jos luot joustavasti tietokannan resurssivarantoon 200 eDTUs kello 11:18, viisi tietokantojen lisääminen resurssivarantoon, perittävän 200 eDTUs koko tunnin kello 11 alku – loput päivän.
- Päivä-2, kello 5:05 tietokannan 1 alkaa muissa 50 eDTUs ja pitää tasaisen päivän kautta. Tietokantojen 2 – 5 lisävaluutta väliltä 0 – 80 eDTUs. Voit lisätä viisi muita tietokantoja, jotka käyttävät erilaisten eDTUs koko päivän päivän kuluessa. Päivä 2 on koko päivän osoitteessa 200 eDTU laskutettu. 
- 5 kello 3 päivänä Voit lisätä toisen 15 tietokannat. Tietokannan käyttö kasvaa koko päivän kohtaan, johon päätät niin, että 400-200 resurssivarantoon eDTUs kello 8:05 Kulujen 200 eDTU tasolla on voimassa kunnes 8 pm ja kasvaa 400 eDTUs jäljellä olevat neljä tuntia. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Miten Käytä on aktiivinen Geo-replikoinnin joustavasti tietokannan varannon näy laskun?
Toisin kuin yhden tietokantoja käyttämällä [Active Geo-replikoinnin](sql-database-geo-replication-overview.md) joustavasti tietokantoja ei ole suoraan laskutuksen vaikuttaa.  Saat kaikki jakavat (varanto on ensisijainen ja toissijainen resurssivarantoon) eDTUs vain perittävän

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Miten valvonta-toiminnon käyttö vaikuttaa laskun? 
Seurannan muodostanut SQL-tietokanta-palveluun osoitteessa ei ole ylimääräisiä kustannus- ja on saatavana Basic, Vakio ja Premium-tietokannat. Kuitenkin tallentamiseen valvontalokien valvonta ominaisuus käyttää Azure-tallennustilan tilin ja korvaukset taulukot ja olevien Azuren tallennustilaan perusteella oman valvontaloki kokoa.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Mistä löydän oikean tason ja suorituskyvyn tason yksittäisen tietokantojen ja joustavasti tietokannan jakavat? 
Käytettävissäsi on joitakin työkaluja. 

- Paikallisen tietokantojen suositeltavaa tietokantojen ja DTUs pakollinen [DTU koonmuuttokahvan Advisorin](http://dtucalculator.azurewebsites.net/) avulla ja arvioi joustavasti tietokannan jakavat useiden tietokantojen.
- Jos yhden tietokannan hyötyä resurssivarantoon se, Azure's älykäs ohjelma suosittelee joustavasti tietokannan resurssivarantoon, jos se havaitsee historiallisten käyttö kaavan, joka takaa, että se. Katso [näyttö ja hallita joustavasti tietokanta-ryhmän Azure-portaalissa](sql-database-elastic-pool-manage-portal.md). Lisätietoja siitä, kuinka voit tehdä matemaattiset on artikkelissa [joustavasti tietokannan resurssivarantoon hinnan ja suorituskyvyn Huomioitavaa](sql-database-elastic-pool-guidance.md)
- Onko sinun on soitettava yhteen tietokantaan, ylös tai alas on ohjeartikkelissa [suorituskyvyn ohjeet yksittäisen tietokannat](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Kuinka usein yksittäinen tietokannan palvelun taso tai suorituskyvyn tason muuttaminen? 
Vipuventtiili V12 tietokantoja voit muuttaa palvelun tason (välillä Basic, Vakio ja Premium) tai (esimerkiksi S1 S2) palvelutaso suorituskyvyn sitten Tasaa niin usein kuin haluat. Aiempien versioiden tietokannat voit muuttaa service taso tai suorituskyvyn tason neljä kertaa 24 tunnin aikana yhteensä.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Kuinka usein kohti resurssivarantoon eDTUs säädetään? 
Niin usein kuin haluat.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Kuinka paljon kuluu aikaa yhteen tietokantaan palvelun taso tai suorituskyvyn tason muuttaminen tai siirtäminen tietokannan ja siitä uloskirjautuminen joustavasti tietokannan resurssivarantoon? 
Tietokannan service-tason muuttaminen ja siirtäminen ja siitä uloskirjautuminen resurssivarantoon edellyttää tietokannan kopioidaan ympäristössä kuin taustatoimintoa. Palvelun tason muuttaminen voi kestää muutaman minuutin kuluttua useita tunteja tietokantojen koon mukaan. Kummassakin tapauksessa tietokannat ovat verkkoyhteydessä ja tavoitettavissa siirron aikana. Lisätietoja yksittäisen tietokantojen vaihtamisesta on artikkelissa [Muuta tietokannan service-taso](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kun yhteen tietokantaan ja joustavasti tietokantojen kannattaa käyttää? 
Yleensä joustavasti tietokannan jakavat on suunniteltu tyypillinen [ohjelmiston muodossa--palvelun (SaaS) sovelluksen kuviota](sql-database-design-patterns-multi-tenancy-saas-applications.md), jos tietokoneessa on yksi tietokanta asiakas- tai vuokraajan kohden. Ostamalla yksittäiset tietokannat ja overprovisioning muuttujan ja piikin tietokantojen tarve ei yleensä on kustannus tehokkaasti. Jakavat, jossa voit hallita ryhmän lajittelemisesta suorituskykyä ja tietokantojen Skaalaa ylös ja alas automaattisesti. 

Azure's älykäs ohjelma suosittelee resurssivarantoon tietokantoja, kun käyttö kuvion takaa, se. Lisätietoja on artikkelissa [SQL-tietokantaan hinnat taso suosituksia](sql-database-service-tier-advisor.md). Yksityiskohtaiset ohjeet saat valita yksittäisen ja joustavasti tietokannan Katso [hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan jakavat](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Mitä on enintään 200 % varmuuskopion tallennustilan enimmäismäärä valmisteltu tietokanta-tallennustilan tarkoittaa? 
Varmuuskopiointi-tallennustila on automaattinen tietokannan varmuuskopiot, joita käytetään varten [piste-kohdassa-aika-Palauta liittyvät tallennustilan](sql-database-recovery-using-backups.md#-point-in-time-restore) ja [Geo palauttaminen](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL-tietokanta on enintään 200 % valmistellun tietokannan tallennustilan varmuuskopion tallennustilaa ilman lisäkustannuksia. Jos sinulla on valmisteltu DB kokoisena vakio DB esiintymän 250 gigatavun (gt), voit esimerkiksi on tarkoitettu 500 gt: n varmuuskopion tallennustilan muita maksutta kanssa. Jos tietokannassa on suurempi kuin annettu varmuuskopion tallennustilan, voit pienentää säilytysaika ottamalla yhteyttä tukipalveluun Azure tai ylimääräisiä varmuuskopion tallennustilan laskuttaa Normaalikorvaus lukuoikeudet maantieteellisesti tarpeettomat Storage (RA GRS)-palvelussa maksaa. Lisätietoja RA GRS Laskutus-kohdassa tallennustilan hinnat tiedot.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>I 'M siirtyvät Web/Business uusi palvelutasot mitä minun täytyy tietää?
Azure SQL-Web- ja Business tietokantoja on nyt poistettu käytöstä. Basic, Vakio, Premium ja Lisää joustavasti tasoa korvaa retiring Web- ja Business-tietokannat. Olemme olet muita FAQ, joka auttaa siirtymän tänä aikana. [Web- ja Business-version Auringonlasku usein kysytyt kysymykset](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Mitä on odotettavissa replikoinnin-viive, kun geo replikoiminen tietokannan samaan Azure alueen sisällä kaksi alueiden välillä?  
On tällä hetkellä tukevat viisi sekuntia RPO ja replikoinnin viive on pienempi kuin, kun geo perusasteen isännöidään Azure suositellut pisteparin alue ja palvelun samalla tasolla.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Mikä on arvioidut replikoinnin viive luotaessa geo toissijainen samalla alueella kuin ensisijaisen tietokannan?  
Empiirinen tietojen perusteella, ei ole liikaa sisäisessä alueen ja välisten alueen replikoinnin viive erotuksen kun Azure suositellut pisteparin alueen käytetään. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Jos kaksi alueiden välillä on verkon virheen, yritä logiikan toiminta kun Geo replikoinnin määrittämisen jälkeen  
Jos on Katkaise yhteys, emme uudelleen 10 sekunnin välein yhteyksien palauttamiseksi.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Mitä voin tehdä voit varmistaa, että tärkeät muuta ensisijainen tietokannan replikoida?
Geo perusasteen on asynkroninen ja yritämme ei koota ensisijainen koko synkronointiin. Mutta annamme tapa synkronointia, jotta replikoinnin kriittinen muuttuu (esimerkiksi salasanan päivitykset). Pakotetun synkronoinnin vaikuttaa suorituskykyyn, koska se estää puheluja viestiketjun, kunnes kaikki vahvistettu tapahtumat, replikoida. Lisätietoja on artikkelissa [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Mitä työkalut ovat käytettävissä seurannassa replikoinnin-viive ensisijainen tietokannan ja geo toissijainen välillä?
Olemme Näytä reaaliaikainen replikoinnin-viive geo perusasteen kautta DMV ja ensisijaisen tietokannan välillä. Lisätietoja on artikkelissa [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
