<properties
   pageTitle="SQL-tietokannan palauttaminen | Microsoft Azure"
   description="Opi palauttamaan tietokannan alueellisen palvelinkeskuksen käyttökatkosta tai virhe Azure SQL tietokanta aktiivinen Geo-replikoinnin ja Geo palauttaminen ominaisuuksia."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Palauttaa Azure SQL-tietokanta tai automaattisesti toissijainen

Azure SQL-tietokanta on, siitä käyttökatkosta seuraavia ominaisuuksia:

- [Aktiivinen Geo-replikointi](sql-database-geo-replication-overview.md)
- [GEO palauttaminen](sql-database-recovery-using-backups.md#point-in-time-restore)

Tietoja liiketoiminnan jatkuvuuden skenaariot ja tukeminen tilanteista toiminnot-kohdassa [Liiketoiminnan jatkuvuus](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Käyttökatkosta tapahtuman valmisteleminen

Palauttaminen toiseen tietoalueen aktiivinen Geo-replikoinnin tai geo tarpeettomat varmuuskopioiden onnistuminen, sinun on valmisteleminen palvelimen toiseen data Centerissä käyttökatkosta muuttuvat uuden ensisijainen palvelin on tarpeen syntyä sekä hyvin määrittänyt vaiheet kuvattu ja testaa varmistaa sujuvan palauttaminen. Näitä valmistavia vaiheita ovat seuraavat:

- Määritä looginen palvelin tulla uusi ensisijainen palvelin toisen alueen. Aktiivinen Geo-toistot, tämä on vähintään yksi ja ehkäpä kunkin toissijaisen palvelimiin. Geo-palauttaminen tämä on yleensä palvelimen [pisteparin alueen](../best-practices-availability-paired-regions.md) alue, johon tietokanta sijaitsee.
- Tunnistaa, ja voit myös määrittää tarvitseman käyttäjät voivat käyttää uuden ensisijaisen tietokannan palvelintason palomuurisäännöt.
- Määrittää, miten aiot ohjaamaan käyttäjät uuteen ensisijainen palvelimeen, kuten muuttamalla yhteyden merkkijonoja tai muuttamalla DNS-tapahtumat.
- Tunnistaa, ja voit myös luoda kirjautumiset, joilla on sijaittava perusmuodon tietokanta uudessa ensisijainen palvelimessa ja poista nämä kirjautumiset on tarvittavat käyttöoikeudet perusmuodon tietokannan, jos minkä tahansa. Lisätietoja on kohdassa [SQL-tietokantaan security jälkeen palauttaminen](sql-database-geo-replication-security-config.md)
- Määritä ilmoitusten säännöt, jotka on päivitettävä, jotta uusi ensisijainen tietokanta yhdistäminen.
- Asiakirjan nykyinen ensisijainen tietokannan valvonta määritys
- Suorita [palauttaminen siirtyminen](sql-database-disaster-recovery-drills.md). Voit simuloida käyttökatkosta Geo palautettavaksi, voit poistaa tai uudelleennimetä lähdetietokannan johtuu sovelluksen yhteys epäonnistui. Voit simuloida käyttökatkosta varten aktiivinen Geo-replikoinnin, voit poistaa käytöstä verkkosovelluksen tai yhteys tietokantaan tai automaattisesti tietokannan virtuaalikoneen johtuu connectity sovellusvirheet.

## <a name="when-to-initiate-recovery"></a>Milloin kannattaa aloittaa palauttaminen

Palautustoiminto vaikuttaa sovelluksen. Se vaatii SQL-yhteysmerkkijono ja uudelleenohjauksen DNS muuttamista, mikä saattaa johtaa pysyvä tietojen menettämisen. Vuoksi on valmis, vain, kun käyttökatkosta todennäköisesti kestää kauemmin kuin sovelluksen palautus aika tavoitteen. Kun sovellus otetaan käyttöön tuotannon kannattaa suorittaa sovelluksen kuntotietojen säännöllisesti seuranta ja vahvistamisen, että palautus käyttöön seuraavat arvopisteiden avulla:

1.  Pysyvä yhteys epäonnistui sovelluksen taso-tietokantaan.
2.  Azure portaalin näkyy ilmoitus tapahtuma alue, jossa laajan vaikutus.
3.  Azure SQL-tietokantapalvelinta on merkitty heikentynyt.

Sen mukaan, sovellus-poikkeama käyttökatkot ja mahdollista liiketoiminnan vastuuta harkitse seuraavat palautusasetukset.

Saat uusimman Geo replikoida Palauta-kohdassa [Hae palautettavissa tietokannan](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) avulla.

## <a name="wait-for-service-recovery"></a>Odota palvelun palautus

Azure ryhmiä työn apuväline luotaessa joukkopostituksia palauttamaan palvelun saatavuus sillä aiheuttaa sen nopeasti kuin mahdollista, mutta sen mukaan, pääkansio voi kestää tunnit tai päivät.  Jos sovellusta hyväksyt merkittäviä käyttökatkot voit yksinkertaisesti odottaa suorittamiseen palauttamista varten. Tässä tapauksessa osaltasi mitään toimia ei tarvita. Näet palvelun tilan Microsoftin [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/). Kun palautus on alueen sovelluksen käytettävyys palautetaan.

## <a name="failover-to-geo-replicated-secondary-database"></a>Siirtyy toissijaisen tietokanta geo replikoida automaattisesti

Jos sovelluksen käyttökatkot voi johtaa business vastuu olisi käytät geo replikoida tietokannat-sovelluksessa. Se ottaa käyttöön sovelluksen palauttaa nopeasti käytettävyys, jos kyseessä käyttökatkosta toisella alueella. Lisätietoja [Geo replikoinnin](sql-database-geo-replication-portal.md)määrittämisestä.

Palauttaa tietokannat saatavuuden haluat aloittaa, käytä jotakin seuraavista tuetuista tavoista geo replikoida toissijaisen vikasietotilaa.

Jollakin seuraavista apuviivojen voit siirtää geo replikoida toissijaisen tietokanta:

- [Azure-portaalissa geo replikoida toissijainen automaattisesti](sql-database-geo-replication-portal.md)
- [Siirtyy toissijaisen geo replikoida automaattisesti PowerShellin avulla](sql-database-geo-replication-powershell.md)
- [T-SQL käyttämällä geo replikoida toissijainen automaattisesti](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Palauttaa käyttämällä Geo palauttaminen

Jos sovelluksen käyttökatkot aiheuta business vastuuta voit Geo palauttaminen Method palauttamaan sovelluksen tietokannat. Se luo tietokannan kopion uusimman geo tarpeettomat varmuuskopion.

Käytä jotakin seuraavista ohjaa geo palautettavan tietokannan uuden alueen tuominen:

- [GEO-Palauta tietokannalle uuden alueen Azure-portaalissa](sql-database-geo-restore-portal.md)
- [GEO-Palauta tietokannalle uuden alueen PowerShellin avulla](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Määritä tietokantasi palautuksen jälkeen

Varmista Jos käytät geo replikoinnin automaattisesti tai geo palauttaminen palauttaminen käyttökatkosta, että yhteys uusien tietokantojen on määritetty oikein niin, että Normaali sovelluksen-funktio voi jatkaa. Tämä on palautettu tietokannan tuotannon voit aloittaa tehtävien tarkistusluettelon.

### <a name="update-connection-strings"></a>Päivitä yhteysmerkkijonoja

Palautetun tietokannan liittyville eri palvelimeen, koska haluat päivittää sovelluksen yhteysmerkkijonon osoittamaan tähän palvelimeen.

Lisätietoja yhteysmerkkijonon muuttamisesta on artikkelissa [yhteyskirjaston](sql-database-libraries.md)tarvittavat kehittäminen kieli.

### <a name="configure-firewall-rules"></a>Määritä palomuurisäännöt

Varmista, että määritetty palvelimeen ja tietokantaan palomuurisäännöt vastaavat ne, jotka on määritetty ensisijainen palvelin ja ensisijainen tietokanta on. Lisätietoja on artikkelissa [Toimintaohje: palomuurin asetusten määrittäminen (Azure SQL-tietokanta)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Määritä kirjautumiset ja tietokannan käyttäjät

Varmista, että kaikki sovelluksesi käyttämät kirjautumiset olemassa palvelimessa, joka isännöi palautetun tietokannan on. Lisätietoja on artikkelissa [Geo replikoinnin suojausasetukset](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Olisi määrittäminen ja testaa aikana tietojen palauttaminen-alirakenne server-palomuurisäännöt ja kirjautumiset (ja niiden käyttöoikeudet). Palvelintason objektit ja niiden määritykset eivät välttämättä ole käytettävissä käyttökatkosta aikana.

### <a name="setup-telemetry-alerts"></a>Telemetriatietojen ilmoitusten määrittäminen

Varmista, että hälytyksen asetuksia päivitetään palautetun tietokannan ja eri palvelimen yhdistäminen on.

Saat lisätietoja tietokannan ilmoitusten säännöistä [Saavat ilmoituksen ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ja [Seuraa palvelun kuntoa](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Seurannan ottaminen käyttöön

Jos tarkistaminen tarvitaan tietokanta, sinun on otettava käyttöön valvonta tietokannan palautuksen jälkeen. Hyvä ilmaisin, että tarkistaminen on tehty on käyttävän asiakassovelluksissa rakenteessa suojatun yhteyden merkkijonot *. database.secure.windows.net. Lisätietoja on artikkelissa [valvonta SQL-tietokannan käytön aloittaminen](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja liiketoiminnan jatkuvuuden rakenne- ja skenaariot on artikkelissa [jatkuvuuden skenaariot](sql-database-business-continuity.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
