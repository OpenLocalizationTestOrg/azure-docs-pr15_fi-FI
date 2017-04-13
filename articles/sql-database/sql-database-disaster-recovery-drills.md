<properties 
   pageTitle="SQL-tietokannan tietojen palauttaminen työkaluja | Microsoft Azure" 
   description="Lue ohjeet ja parhaita käytäntöjä, jotka auttavat tietojen palauttaminen työkaluja Azure SQL-tietokannan avulla säilyttää oman toiminta kriittinen yrityssovellusten joustavat virheet ja katkokset." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Tietojen palauttaminen Poraudu suorittamiseen

On suositeltavaa sovelluksen valmiuden palautus työnkulun vahvistaminen suoritetaan säännöllisin väliajoin. Sovelluksen toimintaa ja vaikutukset tietojen menettämisen ja/tai häiriöitä, automaattisesti liittyy on hyvä suunnitteluryhmät. Kannattaa myös tarpeen mukaan useimmat yleisesti käytettyjen liiketoiminnan jatkuvuuden sertifikaatin osana.

Tietojen palauttaminen tietojen noutamista sisältää seuraavat tiedot:

- Jotka jäljittelevät tietojen taso käyttökatkosta
- Palauttaminen 
- Vahvista sovellusten eheys kirjaa palauttaminen

Sen mukaan, miten voit [suunniteltu sovelluksesi liiketoiminnan jatkuvuuden](sql-database-business-continuity.md), työnkulun suorittaminen porautuminen voivat vaihdella. Alla on kuvataan parhaat käytännöt toteuttaa tietojen palauttaminen-alirakenne, Azure SQL-tietokanta kontekstissa. 

##<a name="geo-restore"></a>GEO palauttaminen

Estää mahdollisesta tietojen menettämisen uhkasta, kun tietojen palauttaminen-alirakenne, on suositeltavaa suorittamiseen käyttämällä testiympäristössä kopion tuotantoympäristössä luomalla ja käyttämällä sen vahvistamiseksi sovelluksen automaattisesti työnkulun sääntö.
 
####<a name="outage-simulation"></a>Käyttökatkosta simuloinnissa

Voit simuloida käyttökatkosta voit poistaa tai uudelleennimetä lähdetietokannan. Tämä aiheuttaa sovelluksen yhteys epäonnistui. 

####<a name="recovery"></a>Palautus

- Toimiminen Geo palauttaa tietokannan eri Serveriin kuvattu [seuraavassa](sql-database-disaster-recovery.md). 
- Muuta sovelluksen määritystä muodostaa yhteyden palautetut tietokannat ja viimeistele palautus [määrittäminen tietokannan palautus](sql-database-disaster-recovery.md) -oppaassa.

####<a name="validation"></a>Kelpoisuustarkistus

- Suorita porautuminen vahvistamalla sovellusten eheys kirjaa palauttaminen (eli yhteyden merkkijonoja, kirjautumiset, perustoiminnot testaaminen tai muita vahvistukset osan vakio-sovellusta signoffs toimenpiteet).

##<a name="geo-replication"></a>GEO sallittuja

Porautuminen Harjoitus edellyttää tietokantaan, joka on suojattu Geo replikoinnin suunnitellun automaattisesti toissijaisen tietokanta. Suunniteltu vikasietotilaa varmistaa, että ensisijainen ja toissijainen tietokantojen säilyvät synkronoinnissa Jos roolit ovat. Toisin kuin suunnittelematon keskeneräiset tätä toimintoa ei tuota tietojen menetyksiltä, joten porautuminen voi suorittaa tuotantoympäristössä. 

####<a name="outage-simulation"></a>Käyttökatkosta simuloinnissa

Voit simuloida käyttökatkosta, voit poistaa käytöstä web-sovelluksen tai virtuaalikoneen yhteydessä tietokantaan. Tämä aiheuttaa yhteys epäonnistuu web-asiakkaille.

####<a name="recovery"></a>Palautus

- Varmista, että sovelluksen määritys DR alueen osoittaa entisen toissijainen, josta tulee käytettävissä uusi ensisijainen. 
- Suorittaa, jotta toissijaisen tietokannan uusi ensisijainen [suunniteltu automaattisesti](sql-database-geo-replication-powershell.md#initiate-a-planned-failover)
- Noudata suorittamiseen palautus [määrittäminen tietokannan palautuksen jälkeen](sql-database-disaster-recovery.md) -opas.

####<a name="validation"></a>Kelpoisuustarkistus

- Suorita porautuminen vahvistamalla sovellusten eheys kirjaa palauttaminen (eli yhteyden merkkijonoja, kirjautumiset, perustoiminnot testaaminen tai muita vahvistukset osan vakio-sovellusta signoffs toimenpiteet).


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja liiketoiminnan jatkuvuuden tilanteita, joissa on artikkelissa [jatkuvuuden skenaariot](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampi palautusasetukset on artikkelissa [Aktiivinen-Geo-replikointi](sql-database-geo-replication-overview.md)  
