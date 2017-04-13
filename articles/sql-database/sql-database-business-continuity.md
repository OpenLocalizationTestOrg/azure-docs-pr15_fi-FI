<properties
   pageTitle="Liiketoiminnan jatkuvuus cloud - tietokannan palauttaminen - SQL-tietokanta | Microsoft Azure"
   description="Lisätietoja Azure SQL-tietokanta tukee cloud Liiketoiminnan jatkuvuus ja tietokannan palauttaminen ja auttaa säilyttää kriittisten cloud sovellukset."
   keywords="Liiketoiminnan jatkuvuus, cloud Liiketoiminnan jatkuvuus, tietokannan palauttaminen tietokannan palauttaminen"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Yleistä liiketoiminnan jatkuvuus Azure SQL-tietokanta

Tämä yleiskatsaus kuvataan ominaisuuksista, joita Azure SQL-tietokanta on Liiketoiminnan jatkuvuus ja palauttaminen. Se sisältää asetuksia, suositukset ja opetusohjelmat palauttaminen häiritsevä tapahtumista, jotka voivat aiheuttaa tietojen menettämisen tai saada tietokannan ja sovelluksen eivät ole käytettävissä. Keskustelun sisältää, mitä tehdään, kun käyttäjä tai sovelluksen virhe vaikuttaa tietojen eheys, Azure alue on käyttökatkosta tai sovellus edellyttää ylläpito. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL-tietokanta-ominaisuuksia, joiden avulla voit antaa Liiketoiminnan jatkuvuus

SQL-tietokanta on useita liiketoiminnan jatkuvuuden ominaisuuksista, kuten automaattisen varmuuskopioinnin ja valinnainen tietokannan replikoiminen. Arvioitu palautus ajat (Lisää) ja viimeisimmät tapahtumien mahdollisesta tietojen menettämisen uhkasta on erilaisia ominaisuuksia. Kun tiedät, että nämä asetukset, voit valita niihin - ja -useimmissa skenaarioissa käytetään yhdessä eri skenaarioissa. Kun kehität jatkuvuuden liiketoimintasuunnitelma, sinun täytyy ymmärtää hyväksyttävä enimmäisajan ennen sovelluksen palauttaa täysin häiritsevä tapahtuman jälkeen – tämä on palautus aika tavoitteen (RTO). Sinun on myös ymmärtää uusimmat tiedot-päivitykset (aikaväli) sovellusta hyväksyt enimmäismäärän menetetä, kun palauttaminen jälkeen häiritsevä tapahtuma - palautus-kohdan tavoite (RPO). 

Seuraavassa taulukossa on Vertailtu NNA ja RPO yleisimmät kolmessa tilanteessa.

| Ominaisuus |  Tavallinen taso | Vakio taso  | Premium taso |
|---|---|---|---|
| Valitse aika palauttaminen varmuuskopiosta | Mikä tahansa palauttaa pisteen seitsemän päivän ajalta   | Mikä tahansa palauttaa pisteen 35 päivän aikana  | Mikä tahansa palauttaa pisteen 35 päivän aikana |
Geo replikoida varmuuskopioista GEO-Palauta | NNA < 12h, RPO < 1h   | NNA < 12h, RPO < 1h   | NNA < 12h, RPO < 1h |
|Aktiivinen Geo-replikointi | NNA < 30s, RPO < 5s   | NNA < 30s, RPO < 5s | NNA < 30s, RPO < 5s |


### <a name="use-database-backups-to-recover-a-database"></a>Palauttaa tietokannan käyttämällä tietokannan varmuuskopiointi

SQL-tietokantaan suorittaa automaattisesti koko tietokannan varmuuskopioinnin yhdistelmää viikoittain eroavuus tietokannan suojaaminen yrityksesi tietojen menettämistä jatkossa kerran tunnissa varmuuskopiot ja tapahtuman Lokin varmuuskopioiden viiden minuutin välein. Nämä varmuuskopiot tallennetaan paikallisesti ylimääräinen 35 päivää tietokantojen vakio- ja Premium-palvelutasot ja seitsemän päivän Basic palvelutaso-tietokannoissa – Katso lisätietoja [palvelutasot](sql-database-service-tiers.md) palvelutasot. Jos säilytysaika service-tason eivät vastaa tarpeitasi business, voit suurentaa säilytysaika muuttamalla [palvelutaso](sql-database-scale-up.md). Koko- ja erilliskohtelu tietokannan varmuuskopioita, myös replikoida [pisteparin tietokeskuksen](../best-practices-availability-paired-regions.md) suojautumista vastaan data Centerissä käyttökatkosta - kohdassa [Automaattinen tietokannan varmuuskopioita](sql-database-automated-backups.md) lisätietoja.

Nämä tietokantojen automaattinen varmuuskopiot avulla voit palauttaa tietokannan eri häiritsevä tapahtumat sekä data Centerissä toiseen tietojen käyttöoikeudet. Käyttämällä automaattinen tietokannan varmuuskopioita palautus arvioitu aika riippuu eri tekijöistä, kuten tietokantoja palauttaminen samalla alueella on samaan aikaan, tietokannan koon, tapahtumalokin koon ja kaistanleveys kokonaismäärän. Useimmissa tapauksissa palauttamisaika on pienempi kuin 12 tuntia. Kun palauttaminen toiseen tietojen alueeseen, mahdollisesta tietojen menettämisen uhkasta on rajoitettu tunnin välein eroavuus tietokannan varmuuskopioita geo tarpeettomat tallennustilan 1 tunti. 

> [AZURE.IMPORTANT] Palauttaa automaattisen varmuuskopioinnin avulla, sinun on oltava jäsen SQL Server osallistujan rooli tai tilauksen omistaja - kohdassa [RBAC: valmiin roolien](../active-directory/role-based-access-built-in-roles.md). Voit palauttaa Azure portaalin, PowerShell tai REST-Ohjelmointirajapinnalla avulla. Et voi käyttää Transact-SQL.

Käyttää automaattisen varmuuskopioinnin liiketoiminnan jatkuvuuden ja palautus järjestelmä, jos sovelluksen:

- Ei pidetä toiminta kriittinen.
- Tämän vuoksi käyttökatkot tai 24 tunnin ei tuota taloudellisten sidonta-SLA ei ole.
- On pieni korkokannan tietojen muuttaminen (pieni tapahtumat tunnissa) ja menettämättä enintään tunnin muutos on hyväksyttävä tietojen menetyksen. 
- Kustannus on merkitsevä. 

On nopeampaa palautus käyttää [Active Geo-replikoinnin](sql-database-geo-replication-overview.md) (käsitellyt seuraava). Jos tarvitset voivan tietojen palauttaminen 35 päivää vanhemmat ajan, harkitse arkistointi tietokannan säännöllisesti BACPAC-tiedostoon (pakattu tietokannan rakenteen ja niihin liittyvät tiedot sisältävän tiedoston) tallennettu Azure-blob-säiliö tai toiseen sijaintiin. Lisätietoja muulla tavoin yhdenmukaisia tietokannan arkisto luomisesta on artikkelissa [tietokannan kopion luominen](sql-database-copy.md) ja [vieminen tietokantaan kopion](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Pienennä Palautumisaika ja rajoittaa tietojen menettämistä palautuksen liittyvä aktiivinen Geo-replikoinnin avulla

Voit lisäksi käytät tietokannan varmuuskopioita tietokannan palauttaminen business häiriöt siinä tapauksessa, on enintään neljä luettavissa toissijainen tietokantojen valittua alueilla tietokannan asetusten määrittäminen [Aktiivinen Geo-replikoinnin](sql-database-geo-replication-overview.md) . Toissijainen tietokannat säilytetään synkronoidun käyttämällä asynkroninen replikoinnin ensisijaisen tietokannan kanssa. Tätä toimintoa käytetään suorittaa business häiriöt tietojen center-käyttökatkosta tai sovelluksen päivityksen aikana. Aktiivinen Geo-replikoinnin avulla voidaan myös antaa vain luku-muotoisten kyselyjen kyselyn tehostaviksi maantieteellisesti hajautettuja käyttäjille.

Jos ensisijainen tietokannan siirtyy odottamatta offline-tilassa tai haluat siirtää verkkosivuston offline-tilassa, ylläpitotoimet, voit muuntaa nopeasti toissijainen perus (kutsutaan myös automaattisesti) ja muodostaa juuri ylemmän tason perus-sovellusten määrittäminen. Suunnitellun automaattisesti, jossa ei ole tietojen menettämisen. Suunnittelematon automaattisesti, jossa voi olla joidenkin tietojen menettämisen hyvin viimeisimmät tapahtumien vuoksi laatu asynkroninen replikoinnin vähän. Jälkeen automaattisesti voit myöhemmin tuntisesta - suunnitelman mukaisesti tai tietokeskuksen tullessa online-tilaan. Kaikissa tapauksissa käyttäjät ilmenee pieni käyttökatkot ja haluat yhdistää. 

> [AZURE.IMPORTANT] Käyttämään Active Geo-replikoinnin on tilauksen omistaja tai SQL Server on järjestelmänvalvojan oikeudet. Voit määrittää ja Azure portaalin sekä PowerShell ja REST-Ohjelmointirajapinnalla käyttämällä tilauksen käyttöoikeuksien tai käyttämällä Transact-SQL automaattisesti käyttöoikeuksien sisällä SQL Serverin avulla.

Käytä aktiivinen Geo-replikoinnin, jos sovellus täyttää jotakin näistä ehdoista:

- On ehdottoman toiminta.
- On tason palvelusopimus (SLA), joka ei salli vähintään 24 tuntia käyttökatkot.
- Käyttökatkot johtaa taloudellisten.
- On hyvin korvaus tietojen muutos on suuri ja menettämättä tietoja tunnin ei ole oikein.
- Aktiivinen geo-replikoinnin lisäkustannuksia on pienempi kuin liittyvän menetys ja mahdolliset taloudellisen tilanteen vastuuseen.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Palauttaa tietokannan käyttäjä tai sovellus virheen jälkeen

* Kukaan ei sopivaa! Käyttäjä voi poistaa vahingossa tietoja, pudota vahingossa tärkeitä taulukon tai koko tietokannan vaikka poistaminen käytöstä. Tai sovelluksen saattaa vahingossa korvaa hyvä tiedot sovelluksen virheellisten vuoksi virheelliset tiedoilla. 

Tässä skenaariossa nämä ovat palauttamista asetukset.

### <a name="perform-a-point-in-time-restore"></a>Suorita ajankohta palauttaminen

Voit käyttää automaattisen varmuuskopioinnin palauttaa tietokannan tunnetut hyvä pisteeseen kopio aika-, aika on tietokannan säilytysajan kuluessa. Kun tietokanta on palautettu, voit korvata alkuperäisen tietokannan palautettu tietokanta tai kopioi tarvittavat tiedot alkuperäisen tietokannan palautetut tiedot. Jos tietokanta käyttää Active Geo-replikoinnin, on suositeltavaa tarvittavat tiedot kopioidaan palautettu kopion alkuperäiseen tietokantaan. Jos korvaat alkuperäisen tietokannan palautettu tietokanta, sinun on määritettävä uudelleen ja synkronoida Active Geo-replikoinnin (joka voi kestää kauan suuri tietokantaan). 

Lisätietoja ja yksityiskohtaisia ohjeita tietokannan palauttaminen pisteeseen senhetkinen Azure-portaalissa tai käyttämällä PowerShell-kohdassa [ajankohta palauttaminen](sql-database-recovery-using-backups.md#point-in-time-restore). Et voi palauttaa käyttämällä Transact-SQL.

### <a name="restore-a-deleted-database"></a>Poistetun tietokannan palauttaminen

Jos tietokanta on poistettu mutta looginen palvelin ei ole poistettu, voit palauttaa poistetun tietokannan kohtaan, johon se on poistettu. Tietokannan varmuuskopiointi palauttaa loogisen SQL-palvelimeen, josta se on poistettu. Voit palauttaa sen alkuperäinen nimi tai Anna uusi nimi tai palautettu tietokanta.

Lisätietoja ja yksityiskohtaisia ohjeita poistetun tietokannan palauttaminen Azure-portaalissa tai käyttämällä PowerShell-kohdassa [poistetun tietokannan palauttaminen](sql-database-recovery-using-backups.md#deleted-database-restore). Et voi palauttaa käyttämällä Transact-SQL.

> [AZURE.IMPORTANT] Jos looginen palvelin on poistettu, et voi palauttaa poistetun tietokannan. 

### <a name="import-from-a-database-archive"></a>Tuo tietokannan arkistosta

Jos sinun on ollut arkistointi tietokannan tietojen menettämisen tapahtui nykyisen säilytysaika automaattinen varmuuskopioiden hakeminen ulkopuolella, voit [BACPAC arkistotiedoston tuominen](sql-database-import.md) uuteen tietokantaan. Tässä vaiheessa voit korvata alkuperäisen tietokannan tuodun tietokannan tai kopioi tarvittavat tiedot alkuperäisen tietokannan tuodut tiedot. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Toisen alueen tietokannan palauttaminen Azure aluetiedot-center käyttökatkosta

<!-- Explain this scenario -->

Vaikka harvinaisissa, Azure tietokeskuksen voi olla käyttökatkosta. Käyttökatkosta toteutuessa valitaan business-häiriöitä, jotka saattavat viimeksi vain muutaman minuutin kuluttua tai saattaa kestää tuntia. 

- Yksi vaihtoehto on odotettava, tietokannan palautettavaksi käyttöön, kun data Centerissä käyttökatkosta on päättynyt. Tämä koskee sovelluksia, jotka varaa tietokanta on offline-tilassa. Esimerkiksi kehittäminen projektin tai maksuttoman kokeiluversion ei tarvitse käyttää jatkuvasti. Kun data Centerissä käyttökatkosta, ei tiedät, kuinka kauan kestää käyttökatkosta, jotta tämä asetus toimii vain, jos et tarvitse tietokannan jonkin aikaa.
- Toinen vaihtoehto on joko toinen tietoalueen automaattisesti, jos käytössäsi on aktiivinen Geo-replikoinnin tai palauta geo tarpeettomat tietokannan varmuuskopioita (Palauta Geo) avulla. Automaattisesti kestää vain muutaman sekunnin, kun varmuuskopioista palauttaminen kestää tuntia.

Kun voit toimia, kuinka kauan rekisteröijän voit palauttaa ja kuinka paljon tietojen menettämisen, jos tietojen center-käyttökatkosta maksamaan riippuu siitä, miten päätät käyttää sovelluksen edellä kuvatut liiketoiminnan jatkuvuuden ominaisuuksia. Voit myös käyttämällä sekä tietokannan varmuuskopioita ja aktiivinen Geo-replikoinnin sovelluksen tarpeen mukaan. Katso sovelluksen tyyliseikat erillinen tietokantojen ja joustavasti jakavat liiketoiminnan jatkuvuuden näiden ominaisuuksien esittely, [rakenne cloud palauttaminen-sovelluksen](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Seuraavissa osissa on palauttaa tietokannan varmuuskopioita tai Active Geo-replikoinnin vaiheiden yleiskuvaus. Yksityiskohtaiset ohjeet, mukaan lukien vaatimukset, Kirjaa palautus ohjeita ja tietoja siitä, miten voit simuloida käyttökatkosta suorittamaan tietojen palauttaminen-alirakenne suunnittelemisesta on artikkelissa [palauttaa käyttökatkosta-SQL-tietokantaan](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Käyttökatkosta valmisteleminen

Riippumatta siitä, käytettävä liiketoiminnan jatkuvuuden toiminto toimi seuraavasti:

- Etsi ja Valmistele kohde-palvelimeen, mukaan lukien palvelintason palomuurisäännöt ja kirjautumiset tietokannasta tason käyttöoikeudet.
- Määrittää uudelleenohjaaminen asiakkaat ja uuteen palvelimeen asiakassovellukset
- Asiakirjan muiden riippuvuudet, kuten valvonta-asetukset ja ilmoitukset 
 
Jos et ole suunnitteleminen ja valmisteleminen oikein, joka tuo sovellustesi online-tilassa, kun automaattisesti tai palautuksen kestää ylimääräisen kerran ja todennäköisesti edellyttävät lisäksi vianmääritys kuormitus – Virheellinen yhdistelmä kerrallaan.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Siirtyy toissijaisen geo replikoida tietokanta automaattisesti 

Jos käytät Active Geo-replikoinnin palautus järjestelmä, [pakottaa geo replikoida toissijainen siirtyy automaattisesti](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Muutamassa toissijaisen muuttuvat uuden ensisijainen ylennyksen ja voidaan tallentaa uudet tapahtumat ja vastata kyselyihin - vain muutaman sekunnin, tietojen menettämisen, joka ei ollut vielä replikoida tietojen kanssa. Lisätietoja automatisointi automaattisesti prosessi on artikkelissa [rakenne cloud palauttaminen-sovelluksen](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Tietokeskuksen tullessa online-tilaan voit tuntisesta alkuperäisen ensisijainen (tai ei).

### <a name="perform-a-geo-restore"></a>Suorittaa Geo-Palauta 

Jos käytössäsi on automaattinen palautus järjestelmää, [aloittaa tietokannan palautuksen käyttämällä Geo palauttaminen](sql-database-disaster-recovery.md#recover-using-geo-restore)geo ylimääräinen toistot varmuuskopiot. Palautus yleensä tapahtuu 12 tunnin kuluessa - ja tietojen menetyksen tunnin milloin määräytyy edellisen tunnin välein eroavuuskopiointi otettavat ja replikoitua. Ennen kuin palautus on valmis, tietokanta ei voi tallentaa kaikki tapahtumat tai vastata kyselyihin. 

> [AZURE.NOTE] Jos tietokeskuksen on online-tilaan, ennen kuin voit siirtää sovelluksen palautetun tietokantaan, voit peruuttaa palauttamiseen.  

### <a name="perform-post-failover--recovery-tasks"></a>Suorita Kirjaa automaattisesti / palautus tehtävät 

Kun palauttamisesta joko palautus-järjestelmä on suoritettava seuraavat tehtävät ennen käyttäjille ja sovellukset ovat takaisin käytön:

- Ohjaa asiakkaat ja uusi palvelimelle ja palautetun tietokannan asiakassovellukset
- Varmista tarvittavat palvelintason palomuurisäännöt ovat paikoillaan, käyttäjät voivat yhdistää (tai [tietokannan tason palomuurit](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Varmista tarvittavat kirjautumiset ja tietokannasta tason käyttöoikeudet ovat paikallaan (Voit myös käyttää [sisältyvät käyttäjien](https://msdn.microsoft.com/library/ff929188.aspx))
- Määritä valvonta, tilannekohtainen tieto
- Määritä ilmoitukset, tarpeen mukaan

## <a name="upgrade-an-application-with-minimal-downtime"></a>Sovelluksen mahdollisimman vähän käyttökatkot ja päivittäminen

Joskus sovelluksen on otettava offline-tilassa esimerkiksi sovelluksen päivityksen suunnitellun ylläpidon vuoksi. [Hallitse sovelluksen päivityksiä](sql-database-manage-application-rolling-upgrade.md) käsitellään aktiivinen Geo-replikoinnin avulla voit ottaa käyttöön cloud sovelluksen minimoi käyttökatkot päivitysten aikana ja antaa palautus-polku, tapahtuma jotain vikaan juokseva päivitykset. Tässä artikkelissa tarkastelee orchestrating päivitysprosessin kahdella tavalla ja käsitellään edut ja kunkin asetuksen valinnat.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja sovelluksen tyyliseikat erillinen tietokantojen ja joustavasti jakavat on artikkelissa [rakenne cloud palauttaminen-sovelluksen](sql-database-designing-cloud-solutions-for-disaster-recovery.md) ja [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






