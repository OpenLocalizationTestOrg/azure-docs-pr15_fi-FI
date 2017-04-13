<properties
    pageTitle="SQL-virhekoodit - tietokannan yhteysvirhe | Microsoft Azure"
    description="Lisätietoja SQL-virhekoodit SQL-tietokanta-asiakassovellusten, kuten yleisiä tietokannan yhteys virheitä, tietokannan kopion ongelmat ja yleisten virheiden varten. "
    keywords="SQL-virhekoodi, access sql-tietokannan yhteysvirhe sql-virhekoodit"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muut ongelmat


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Tämä artikkeli sisältää SQL-virhekoodit, SQL-tietokanta-asiakassovellusten, mukaan lukien tietokannan yhteys virheitä, lyhytkestoisia virheitä (kutsutaan myös lyhytkestoisia virheitä), resurssien hallinnon virheet, tietokannan kopion ongelmat, joustavasti resurssivarantoon ja muut virheet. Useimmat luokat liittyvät Azure SQL-tietokanta ja eivät vaikuta Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Tietokannan yhteyden virheet, lyhytkestoisia virheet ja muut tilapäinen virheet

Seuraavassa taulukossa kerrotaan SQL-virhekoodit yhteyden tappio virheet ja muut lyhytkestoisia virheitä saattaa ilmetä, kun sovellus yrittää käyttää SQL-tietokantaan. Käytön aloittaminen opetusohjelmat siitä, miten voit muodostaa yhteyden Azure SQL-tietokanta-kohdassa [yhteyden muodostaminen Azure SQL-tietokantaan](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Yleisimmät tietokannan yhteysvirheet ja lyhytkestoisia vika virheet

Azure infrastruktuurin pystyy määrittämään palvelinten dynaamisesti, kun paksu työmääriä syntyä SQL-tietokanta-palvelussa.  Dynaaminen ongelman voi aiheuttaa asiakas-ohjelmaa, menetät yhteyden muodostaminen SQL-tietokantaan. Virhetilassa tällaista kutsutaan *lyhytkestoisia vika*.

Jos asiakasohjelma on uudelleen logiikan, voit yrittää muodostaa yhteyden jälkeen antamalla lyhytkestoisia vika aika, joka korjaa itse.  On suositeltavaa, että olet viivyttää 5 sekunnin ajan ennen ensimmäisen uudelleen. Yritetään lyhyempi 5 sekuntia riskien overwhelming pilvipalvelussa sijaitsevaan viiveen jälkeen. Kunkin myöhemmin uudelleen viive olisi eksponentiaalisesti enintään 60 sekuntia.

Yhtenä seuraavat virheilmoitukset asiakas-ohjelmista luettelon yleensä lyhytkestoisia vika virheitä:

- Palvelimen < Azure_instance > < db_name > tietokanta ei ole tällä hetkellä käytettävissä. Yritä yhteyttä myöhemmin uudelleen. Jos ongelma jatkuu, ota yhteyttä asiakastukeen ja anna ne < session_id > istunnon jäljitys tunnus

- Palvelimen < Azure_instance > < db_name > tietokanta ei ole tällä hetkellä käytettävissä. Yritä yhteyttä myöhemmin uudelleen. Jos ongelma jatkuu, ota yhteyttä asiakastukeen ja anna ne < session_id > istunnon jäljitys tunnus. (Microsoft SQL Server-virhe: 40613)

- Aiemmin luodun yhteyden on pakotetaan sulkeutumaan siirretään.

- System.Data.Entity.Core.EntityCommandExecutionException: Virhe suoritettaessa komento-määritys. Katso lisätietoja sisempi poikkeus. ---> System.Data.SqlClient.SqlException: transport-tason virhe on tapahtunut vastaanotettaessa tuloksia palvelimesta. (provider: istunnon toimittaja-virhe: 19 - fyysistä yhteyttä ei ole käytettävissä)

Koodiesimerkkejä uudelleen logiikan on seuraavissa artikkeleissa:

- [SQL-tietokantaan ja SQL Serverin kirjastot](sql-database-libraries.md) 
- [Voit korjata yhteyden virheet ja lyhytkestoisia virheet SQL-tietokantaan toiminnot](sql-database-connectivity-issues.md)

Keskustelun *estäminen kauden* asiakkaiden, jotka käyttävät ADO.NET sisältyy [SQL Server-yhteyden jakavan (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Lyhytkestoisia vika virhekoodit

Seuraavia virheitä ovat lyhytkestoisia ja on yritettävä sovelluksen logiikka 

| Virhekoodi | Vakavuus | Kuvaus |
| ---: | ---: | :--- |
| 4060 | 16 | Ei voi avata tietokantaa "% & #x2a; ls" pyytäjä kirjautuminen. Kirjautuminen epäonnistui. |
|40197|17|Palvelu on havainnut virheen käsittely puhelinnumeroon. Yritä uudelleen. Virhekoodi %d.<br/><br/>Näyttöön tulee tämä virhe, kun palvelua ei ole käytettävissä, ohjelmiston tai laitteistopäivitykset, laitteisto tai automaattisesti-ongelmien vuoksi. Virhe 40197 viestin upotettu virhekoodi (%d) on lisätietoja virheen tai on tapahtunut automaattisesti tyyppisiä. Esimerkkejä koodit upotettujen virhe 40197 viestin virhe on 40020, 40143, 40166 ja 40540.<br/><br/>SQL-tietokantapalvelinta palvelinyhteyden automaattisesti muodostaa yhteyden tietokantaan kunnossa kopion. Sovellus on todellisen 40197, virheloki upotetun virhekoodi (%d) viestin vianmäärityksen ja yritä muodostamista SQL-tietokantaan, kunnes resurssit ovat käytettävissä ja yhteys muodostetaan uudelleen.|
|40501|20|Palvelu on varattu. Yritä pyynnön 10 sekunnin kuluttua. Tapauksen tunnus: %ls. Koodi: %d.<br/><br/>*Huomautus:* Lisätietoja on artikkelissa:<br/>• [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md).
|40613|17|Tietokannan "% & #x2a; ls" palvelimessa "% & #x2a; ls" ei ole tällä hetkellä käytettävissä. Yritä yhteyttä myöhemmin uudelleen. Jos ongelma jatkuu, ota yhteyttä asiakastukeen ja anna ne istunnon jäljitys tunnus "% & #x2a; ls".|
|49918|16|Pyyntö ei voi käsitellä. Resurssit eivät riitä pyynnön.<br/><br/>Palvelu on varattu. Yritä pyyntöä myöhemmin. |
|49919|16|Ei käsittele luominen tai päivittäminen pyynnön. Liian monta luominen tai päivittäminen toiminnot tilauksen "% ld".<br/><br/>Palvelu on varattu käsittelyn useita luominen tai päivittäminen tilaus tai palvelimen pyynnöt. Resurssin optimointi tällä hetkellä estetyt pyynnöt. Kyselyn [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) varten odottavia toimintoja. Odota, kunnes Luo tai päivitä odottavat pyynnöt ovat valmiit tai poistaa jonkin odottavia pyyntöjä ja lähettää pyynnön myöhemmin uudelleen. |
|49920|16|Pyyntö ei voi käsitellä. Liian monta toiminnot tilauksen "% ld".<br/><br/>Palvelu on varattu useita käsittely tätä tilausta varten. Resurssin optimointi tällä hetkellä estetyt pyynnöt. Kyselyn [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) toiminnon tilan. Odota, kunnes pyyntöihin ovat valmiita tai Poista yksi odottavia pyyntöjä ja lähettää pyynnön myöhemmin uudelleen. |

## <a name="database-copy-errors"></a>Tietokannan kopion virheet

Seuraavia virheitä on ilmennyt kopioitaessa tietokannan Azure SQL-tietokantaan. Lisätietoja on artikkelissa [kopioida Azure SQL-tietokantaan](sql-database-copy.md).


|Virhekoodi|Vakavuus|Kuvaus|
|---:|---:|:---|
|40635|16|Asiakkaan IP-osoite "% & #x2a; ls" on tilapäisesti poissa käytöstä.|
|40637|16|Luo tietokannan kopio on poistettu käytöstä.|
|40561|16|Tietokannan kopiointi epäonnistui. Lähde- tai kohdeluettelon tietokanta ei ole.|
|40562|16|Tietokannan kopiointi epäonnistui. Lähdetietokannan on hylätty.|
|40563|16|Tietokannan kopiointi epäonnistui. Kohdetietokannan on hylätty.|
|40564|16|Tietokannan kopiointi epäonnistui sisäisen virheen vuoksi. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen.|
|40565|16|Tietokannan kopiointi epäonnistui. Enintään 1 samanaikainen tietokannan kopiota samasta lähteestä on sallittu. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen myöhemmin.|
|40566|16|Tietokannan kopiointi epäonnistui sisäisen virheen vuoksi. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen.|
|40567|16|Tietokannan kopiointi epäonnistui sisäisen virheen vuoksi. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen.|
|40568|16|Tietokannan kopiointi epäonnistui. Lähdetietokannan on käytettävissä. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen.|
|40569|16|Tietokannan kopiointi epäonnistui. Kohdetietokannan on käytettävissä. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen.|
|40570|16|Tietokannan kopiointi epäonnistui sisäisen virheen vuoksi. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen myöhemmin.|
|40571|16|Tietokannan kopiointi epäonnistui sisäisen virheen vuoksi. Kohdetietokannan poistaminen käytöstä ja yritä uudelleen myöhemmin.|

## <a name="resource-governance-errors"></a>Resurssien hallinnon virheet

Seuraavat virheet johtuvat liiallinen käyttö resurssien käsitellessäsi Azure SQL-tietokantaan. Esimerkki:

- Tapahtumaan on jo avoinna liian kauan.
- Tapahtuman pitää liikaa lukitukset.
- Sovellus on käyttö liikaa muistia.
- Sovellus on liikaa muissa `TempDb` tilaa.

Aiheeseen liittyviä ohjeita:

* Tarkempia tietoja on saatavilla täällä: [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md)

|Virhekoodi|Vakavuus|Kuvaus|
|---:|---:|:---|
|10928|20|Resurssitunnus: %d. Tietokannan %s rajoitus on, ja se on saavutettu. Lisätietoja on artikkelissa [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Resurssitunnus ilmoittaa resurssi, joka enimmäismäärä on saavutettu. Saat työntekijän viestiketjuissa siirtyminen, Resurssitunnus = 1. Istuntojen Resurssitunnus = 2.<br/><br/>*Huomautus:* Saat lisätietoja tämän virheen ja miten voit ratkaista ongelman:<br/>• [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md). |
|10929|20|Resurssitunnus: %d. %S pienin takuu on %d, enimmäismäärä on ja nykyisen tietokannan käytön on %d. Palvelin on kuitenkin tällä hetkellä varattu ja suurempi kuin %d pyynnöt tuki tätä tietokantaa. Lisätietoja on artikkelissa [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Muussa tapauksessa yritä uudelleen myöhemmin.<br/><br/>Resurssitunnus ilmoittaa resurssi, joka enimmäismäärä on saavutettu. Saat työntekijän viestiketjuissa siirtyminen, Resurssitunnus = 1. Istuntojen Resurssitunnus = 2.<br/><br/>*Huomautus:* Saat lisätietoja tämän virheen ja miten voit ratkaista ongelman:<br/>• [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md).|
|40544|20|Tietokanta on saapunut koon kiintiötä. Osion tai poista tiedot, pudota indeksejä tai käyttöohjeista ratkaisuja.|
|40549|16|Istunnon lopetetaan, koska sinulla pitkään suoritettavien tapahtuma. Kokeile peruuttamisessa sekä tapahtumasta.|
|40550|16|Istunnon on lopetettu, koska se on hankittu liikaa lukitukset. Kokeile lukeminen tai muokkaaminen yhdessä tapahtumassa vähemmän rivejä.|
|40551|16|Istunnon on lopetettu vuoksi liiallinen `TEMPDB` käyttö. Kokeile muuttamalla kyselyä voit vähentää väliaikaisen taulukossa tilaa.<br/><br/>*Vihje:* Jos käytössäsi on väliaikaisia objekteja, säästä tilaa `TEMPDB` tietokannan pudottamalla väliaikaisia objekteja, kun niitä ei enää tarvita istunto.|
|40552|16|Vuoksi liiallinen tapahtuman lokin käyttö on lopetettu istunto. Kokeile muokkaaminen yhdessä tapahtumassa vähemmän rivejä.<br/><br/>*Vihje:* Jos teet joukkona Lisää avulla `bcp.exe` apuohjelma tai `System.Data.SqlClient.SqlBulkCopy` luokan, kokeile `-b batchsize` tai `BatchSize` asetukset rivien määrän rajoittaminen kopioida kuhunkin tapahtumaan palvelimeen. Jos indeksi on ovat muodostetaan uudelleen `ALTER INDEX` ilmoitus, kokeile `REBUILD WITH ONLINE = ON` vaihtoehto.|
|40553|16|Istunnon on lopetettu liiallinen muistinkäytön vuoksi. Kokeile muuttamalla kyselyä käsittelemään vähemmän rivejä.<br/><br/>*Vihje:* Vähentää palautettujen `ORDER BY` ja `GROUP BY` Transact-SQL-koodi toimintojen pienentää kyselyn muistivaatimuksia.|

## <a name="elastic-pool-errors"></a>Joustavasti resurssivarantoon virheet

Seuraavia virheitä liittyvät luomisesta ja käyttämisestä Elastics jakavat.

| Virhenumero | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Tallennustilan enimmäismäärä on saavutettu joustavasti resurssivarantoon. Joustavasti resurssivarantoon tallennustilan käyttö voi olla enintään (%d) MBs. | Joustavasti tilan raja-MBs. | Yritetään kirjoittaa tiedot tietokantaan, kun joustavasti resurssivarantoon tallennustilan enimmäismäärä on saavutettu. | Harkitse lisääntyvien Suurenna tallennusrajoitus, vähentää joustavasti resurssivarantoon yksittäisiä tietokannoista käyttämä tallennustila mahdollisuuksien joustavasti altaan DTUs tai poista tietokannat joustavasti varannon. |
| 10929 | EX_USER | %S pienin takuu on %d, enimmäismäärä on ja nykyisen tietokannan käytön on %d. Palvelin on kuitenkin tällä hetkellä varattu ja suurempi kuin %d pyynnöt tuki tätä tietokantaa. Katso lisäohjeita [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) . Muussa tapauksessa yritä uudelleen myöhemmin. | DTU min kohti tietokantaan. DTU Maks tietokantaa kohden | Samanaikainen työntekijöiden (pyyntöjen) kaikki tietokannat joustavasti varannon yli kokonaismäärän yritti ylittää resurssivarantoon. | Harkitse lisääntyvien mahdollisuuksien joustavasti resurssivarantoon DTUs lisäämiseksi työntekijä raja tai poista tietokannat joustavasti varannon. |
| 40844 | EX_USER | Tietokannan "%ls" palvelimessa "%ls" on "%ls" edition tietokanta joustavasti varannon ja ei voi olla jatkuva kopioi yhteyden. | tietokannan nimen, tietokannan edition, palvelimen nimi | StartDatabaseCopy-komento on annettu-premium-tietokannan joustavasti sarjassa. | Tulossa pian |
| 40857 | EX_USER | Palvelin ei löydy joustavasti resurssivarantoon: "%ls", joustavasti sovellussarjan nimi: "%ls". | nimi-palvelimeen. joustavasti sovellussarjan nimi | Määritetty palvelin ei ole määritetty joustavasti resurssivarantoon. | Anna kelvollinen joustavasti ryhmän nimi. |
| 40858 | EX_USER | Joustavasti resurssivarantoon "%ls" on jo palvelin: "%ls" | joustavasti sovellussarjan nimi-palvelimen nimi | Määritetyn joustavasti varanto on jo määritetty looginen palvelin. | Anna uusi joustavasti ryhmän nimi. |
| 40859 | EX_USER | Joustavasti resurssivarantoon ei tue palvelutaso "%ls". | joustavasti resurssivarantoon palvelutaso | Määritetyn palvelutaso joustavasti resurssivarantoon valmistelu ei tueta. | Anna oikean version tai jätä palvelutaso tyhjäksi ja käytä oletusarvo-palvelutaso. |
| 40860 | EX_USER | Joustavasti resurssivarantoon "%ls"- ja palveluluettelon tavoitteen "%ls" yhdistelmä on virheellinen. | joustavasti ryhmän nimeä. palvelun tason objektiivisten nimi | Joustavasti resurssivaranto ja palvelun tavoitteen voi määrittää yhdessä vain, jos palvelun tavoitteena on määritetty "ElasticPool". | Määritä oikea yhdistelmä joustavasti resurssivaranto ja palvelun tavoitteen. |
| 40861 | EX_USER | Tietokannan versio "%. *ls' ei voi olla eri kuin joustavasti resurssivarantoon palvelun tason eli "%.* ls'. | tietokannan edition-joustavasti resurssivarantoon palvelutaso | Tietokannan versio eroaa joustavasti resurssivarantoon palvelutaso. | Määritä tietokannan-version, joka on erilainen kuin joustavasti resurssivarantoon palvelutaso.  Huomaa, että tietokanta edition ei tarvitse erikseen. |
| 40862 | EX_USER | Joustavasti ryhmän nimen on oltava Jos joustavasti resurssivarantoon palvelun tavoitteena on määritettävä. | Ei mitään | Joustavasti resurssivarantoon palvelun tavoitteena ei tunnista joustavasti resurssivarantoon yksilöllisesti. | Määritä joustavasti sovellussarjan nimi, jos joustavasti resurssivarantoon palvelun tavoitteen. |
| 40864 | EX_USER | DTUs joustavasti resurssivarantoon varten on oltava vähintään (%d) DTUs palvelun tason ' %. * ls'. | Joustavasti resurssivarantoon; DTUs joustavasti resurssivarantoon palvelutaso. | Yritetään määrittää DTUs joustavasti resurssivarantoon alla alaraja varten. | Yritä asetus DTUs Lisää joustavasti altaan vähintään alaraja. |
| 40865 | EX_USER | DTUs joustavasti resurssivarantoon, voi olla enintään (%d) DTUs palvelun tason ' %. * ls'. | Joustavasti resurssivarantoon; DTUs joustavasti resurssivarantoon palvelutaso. | Yritetään määrittää DTUs joustavasti resurssivarantoon yläpuolella enimmäisrajan varten. | Yritä varten joustavasti resurssivarantoon DTUs asettaminen ei ole suurempi kuin enimmäisrajan. |
| 40867 | EX_USER | Max tietokantaa kohden DTU on oltava vähintään (%d) palvelun tason ' %. * ls'. | DTU Maks kohti tietokantaan. joustavasti resurssivarantoon palvelutaso | Yritetään määrittää DTU Enimmäiskäyttöaste alla tuetun enimmäismäärän tietokantaa kohden. | Harkitse joustavasti resurssivarantoon palvelutaso, joka tukee haluamasi asetus. |
| 40868 | EX_USER | Max tietokantaa kohden DTU ei voi ylittää (%d) palvelun tason ' %. * ls'. | DTU Maks kohti tietokantaan. joustavasti resurssivarantoon palvelutaso. | Yritetään määrittää DTU Enimmäiskäyttöaste tuetun enimmäismäärän ulkopuolella tietokantaa kohden. | Harkitse joustavasti resurssivarantoon palvelutaso, joka tukee haluamasi asetus. |
| 40870 | EX_USER | DTU min tietokantaa kohden ei voi ylittää (%d) palvelun tason ' %. * ls'. | DTU min kohti tietokantaan. joustavasti resurssivarantoon palvelutaso. | Yritetään määrittää DTU min tuetun enimmäismäärän ulkopuolella tietokantaa kohden. | Harkitse joustavasti resurssivarantoon palvelutaso, joka tukee haluamasi asetus. |
| 40873 | EX_USER | Tietokantojen (%d) ja DTU min (%d) tietokantaa kohden voi olla enintään DTUs joustavasti altaan (%d). | Luvun tietokantojen joustavasti varannon; DTU min kohti tietokantaan. Joustavasti resurssivarantoon DTUs. | Yritetään määrittäminen DTU min tietokantoja, jotka ylittävät joustavasti resurssivarantoon DTUs joustavasti resurssivarantoon. | Harkitse lisääntyvien joustavasti resurssivarantoon DTUs, pienennä DTU min tietokantaa kohden tai vähennä joustavasti resurssivarantoon ominaisuudet. |
| 40877 | EX_USER | Joustavasti ryhmää ei voi poistaa, jos se ei sisällä tietokantoja. | Ei mitään | Joustavasti varanto on yksi tai useampi tietokanta ja näin ollen ei voi poistaa. | Poista tietokannat joustavasti resurssivarantoon jotta voit poistaa sen. |
| 40881 | EX_USER | Joustavasti resurssivarantoon ' %. * ls' tietokannan Laske enimmäismäärä on saavutettu.  Tietokannan Laske rajoitus joustavasti resurssivarantoon eivät saa ylittää (%d) joustavasti resurssivarantoon, jossa (%d) DTUs. | Joustavasti resurssivarantoon; nimi tietokannan enimmäismäärä on joustavasti resurssivarantoon; e DTUs resurssivarannon. | Yritetään luominen tai lisääminen joustavasti resurssivarantoon tietokantaan, kun joustavasti altaan tietokanta-enimmäismäärä on saavutettu. | Harkitse lisääntyvien DTUs joustavasti altaan Jos mahdollista, jos haluat suurentaa sen tietokannan raja tai poista tietokannat joustavasti varannon. |
| 40889 | EX_USER | DTUs tai joustavasti resurssivarantoon tallennusrajoitus ' %. * ls' ei voi pienentää, koska, jotka tarjoa sen tietokantojen riittävästi levytilaa. | Joustavasti ryhmän nimi. | Yritetään pienentää tallennustilarajan joustavasti altaan sen tallennustilan käyttöäsi alapuolella. | Harkitse yksittäiset tietokannat joustavasti varannon tallennustilan pienentäminen tai poista tietokannat olevilla sen DTUs tai tallennustilarajan vähentämiseksi. |
| 40891 | EX_USER | DTU min (%d) tietokantaa kohden voi olla enintään DTU Enimmäiskäyttöaste (%d) tietokantaa kohden. | DTU min kohti tietokantaan. DTU max tietokantaa kohden. | Yritetään määrittää DTU min suurempi kuin DTU Enimmäiskäyttöaste tietokantaa kohden tietokantaa kohden. | Varmista, että DTU min kohti tietokantoja ei ole suurempi kuin DTU Enimmäiskäyttöaste tietokantaa kohden. |
| TBD | EX_USER | Tallennustilan koko yksittäisiä tietokannan joustavasti varannon eivät saa ylittää salliman enimmäiskoon ' %. * ls' palvelun taso joustavasti resurssivarantoon. | joustavasti resurssivarantoon palvelutaso | Tietokannan enimmäiskoko ylittää joustavasti resurssivarantoon palvelutaso salliman enimmäiskoon. | Määritä enimmäiskoko tietokannan joustavasti resurssivarantoon palvelutaso salliman enimmäiskoon rajoissa. |

Aiheeseen liittyviä ohjeita:

* [Luo joustavasti tietokanta-ryhmän (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Hallitse joustavasti tietokanta-ryhmän (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Luo joustavasti tietokanta-ryhmän (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Näyttö ja hallita joustavasti tietokanta-ryhmän (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Yleiset virheet

Seuraavat virheet eivät kuulu minkä tahansa edelliseen luokkaan.

|Virhekoodi|Vakavuus|Kuvaus|
|---:|---:|:---|
|15006|16|<AdministratorLogin>ei ole kelvollinen, koska se sisältää virheellisiä merkkejä.|
|18452|14|Kirjautuminen epäonnistui. Kirjautuminen on luotettu toimialueen eikä sitä voi käyttää Windows authentication.%. & #x2a; ls (kirjautumiset ei tueta tässä versiossa SQL Server.)|
|18456|14|Käyttäjän kirjautuminen epäonnistui "%. & #x2a;ls'.%. & #x2a ls %. & #x2a; ls (käyttäjän kirjautuminen epäonnistui"% & #x2a; ls". Salasanan muuttaminen epäonnistui. Salasanan vaihtaminen kirjautumisen yhteydessä ei tueta tässä versiossa SQL Server.)|
|18470|14|Käyttäjän kirjautuminen epäonnistui "% & #x2a; ls". Syy: Tili on disabled.% & #x2a; ls|
|40014|16|Useiden tietokantojen ei voi käyttää samaa tapahtumaa.|
|40054|16|Ryhmitellyn indeksin taulukkoja ei tueta tässä versiossa SQL Server. Liitetty indeksin luominen ja yritä uudelleen.|
|40133|15|Tätä toimintoa ei tueta tässä versiossa SQL Server.|
|40506|16|Määritetyn SUOJAUSTUNNUS ei kelpaa tämän SQL Serverin versio.|
|40507|16|"%. & #x2a; ls' ei voi käynnistää parametrien tässä versiossa SQL Serverin kanssa.|
|40508|16|Käytä lause ei voi vaihtaa tietokantojen. Uusi yhteys avulla voit muodostaa yhteyden eri tietokantaan.|
|40510|16|Lause "% & #x2a; ls" ei tueta tässä SQL Serverin versio|
|40511|16|Valmiin funktion "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40512|16|Vanhentuneen "%ls"-ominaisuutta ei tueta tässä versiossa SQL Server.|
|40513|16|Palvelimen muuttujan "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40514|16|"%ls" ei tueta tässä versiossa SQL Server.|
|40515|16|Tietokannan ja/tai palvelimen nimen viittaus tekstimuodossa "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40516|16|Yleinen temp objekteja ei tueta tässä versiossa SQL Server.|
|40517|16|Avainsana tai lause "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40518|16|DBCC komento "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40520|16|Suojattavan luokan '% S_MSG' ei tueta tässä versiossa SQL Server.|
|40521|16|Suojattavan luokan '% S_MSG' ei tueta tässä versiossa SQL Server server-alue.|
|40522|16|Tietokannan pääasiallista "% & #x2a; ls" lajin ei tueta tässä versiossa SQL Server.|
|40523|16|Implisiittinen käyttäjän "% & #x2a; ls" luominen ei tueta tässä versiossa SQL Server. Luo käyttäjän erikseen ennen sen käyttämistä.|
|40524|16|Tietotyyppi "% & #x2a; ls" ei tueta tässä versiossa SQL Server.|
|40525|16|'%.Ls' kanssa ei tueta tässä SQL Serverin versio.|
|40526|16|"%. & #x2a; ls' Rivijoukko tarjoaja ei tueta tässä versiossa SQL Server.|
|40527|16|Linkitettyjä palvelimia ei tueta tässä versiossa SQL Server.|
|40528|16|Käyttäjä ei voi yhdistää varmenteet, julkiseen avaimet tai kirjautumiset tässä versiossa SQL Server.|
|40529|16|Valmiin funktion "% & #x2a; ls"-tekeytyminen kontekstissa ei tueta tässä versiossa SQL Server.|
|40532|11|Palvelin ei voi avata "% & #x2a; ls" pyytäjä kirjautuminen. Kirjautuminen epäonnistui.|
|40553|16|Istunnon on lopetettu liiallinen muistinkäytön vuoksi. Kokeile muuttamalla kyselyä käsittelemään vähemmän rivejä.<br/><br/>*Huomautus:* Vähentää palautettujen `ORDER BY` ja `GROUP BY` Transact-SQL-koodi toimintojen auttaa vähentämään kyselyn muistivaatimuksia.|
|40604|16|Ei ole Luo ja muuta TIETOKANNAN, koska se ylittää palvelimen kiintiön.|
|40606|16|Tietokantojen liittäminen ei tueta tässä versiossa SQL Server.|
|40607|16|Kirjautumiset ei tueta tässä versiossa SQL Server.|
|40611|16|Palvelinten voi olla enintään 128 määritetty palomuurisääntöjä.|
|40614|16|Käynnistä IP-osoite palomuurisääntö voi olla enintään Lopeta IP-osoite.|
|40615|16|Palvelimen {0}, joka"pyytäjä kirjautuminen ei voi avata. Asiakkaan IP-osoite ({1}) ei sallita yhteyden palvelimeen.  Ota käyttöön, SQL-tietokanta-portaalin käyttäminen tai suorittaa sp_set_firewall_rule perusmuodon tietokantaan IP-osoite tai osoitealueita palomuurisäännön luominen.  Voi kestää jopa viisi minuuttia, jotta muutos tulee voimaan.|
|40617|16|Palomuurin säännön nimi, joka alkaa <rule name> on liian pitkä. Enimmäispituus on 128.|
|40618|16|Palomuurin säännön nimi ei voi olla tyhjä.|
|40620|16|Käyttäjän kirjautuminen epäonnistui "% & #x2a; ls". Salasanan muuttaminen epäonnistui. Salasanan vaihtaminen kirjautumisen yhteydessä ei tueta tässä versiossa SQL Server.|
|40627|20|{0}-palvelin ja tietokanta {1}-toiminto on käynnissä. Odota muutama minuutti ennen kuin yrität uudelleen.|
|40630|16|Salasanan tarkistus epäonnistui. Salasana ei vastaa käytännön vaatimuksia, koska se on liian lyhyt.|
|40631|16|Salasana, jonka olet määrittänyt on liian pitkä. Salasana on enintään 128: aa merkkiä.|
|40632|16|Salasanan tarkistus epäonnistui. Salasana ei vastaa käytännön vaatimuksia, koska se ei ole tarpeeksi monimutkaisia.|
|40636|16|Varattu tietokannan nimi ei voi käyttää "% & #x2a; ls"-toimintoa.|
|40638|16|Virheellinen Tilaustunnus < Tilaustunnus >. Tilaus ei ole.|
|40639|16|Pyyntö ei ole rakenteen mukainen: <schema error>.|
|40640|20|Palvelimen tapahtui odottamaton poikkeus.|
|40641|16|Määritetyssä sijainnissa on virheellinen.|
|40642|17|Palvelin on tällä hetkellä varattu. Yritä uudelleen myöhemmin.|
|40643|16|Määritettyä x-ms-versio otsikko-arvo on virheellinen.|
|40644|14|Määritä käyttämään määritettyä tilausta epäonnistui.|
|40645|16|Palvelimen nimi <servername> ei voi olla tyhjä tai null. Se voidaan tehdä vain, pienet kirjaimet "a"-"z", numerot 0-9 ja yhdysmerkin. Tavuviivan saa johtaa tai jälkimmäinen nimessä.|
|40646|16|Tilauksen tunnus et voi olla tyhjä.|
|40647|16|Tilauksen < Tilaustunnus ei ole server palvelimen nimi.|
|40648|17|Liian monta pyynnöt on suoritettu. Yritä uudelleen myöhemmin.|
|40649|16|Virheellinen sisältötyyppi on määritetty. Tuetaan vain sovelluksen/xml.|
|40650|16|Tilauksen < Tilaustunnus > ei ole tai ei ole valmis toimintoa.|
|40651|16|Ei voi luoda palvelimen, koska tilaus < Tilaustunnus > on poistettu käytöstä.|
|40652|16|Ei voi siirtää tai luoda palvelimeen. Tilauksen < Tilaustunnus > ylittää palvelimen käyttökiintiö.|
|40671|17|Yhteysvirhe yhdyskäytävän ja management-palvelun välillä. Yritä uudelleen myöhemmin.|
|40852|16|Ei voi avata tietokantaa "%. *ls' palvelimessa ' %.* ls' pyytää kirjautuminen. Access-tietokantaan on sallittu vain suojatun yhteysmerkkijonon avulla. Voit käyttää tätä tietokantaa, muokata yhteyden-merkkijonot sisältämään 'suojatun' Serverin täydellinen toimialuenimi - 'palvelimen nimi'.database.windows .net olisi muutettava 'palvelimen nimi'.database. `secure`. windows.net.|
|45168|16|SQL Azure-järjestelmä on kohdassa Lataa ja sijoittaa yläraja yksittäisen palvelimen rinnakkaiset DB CRUD-toiminnot (esimerkiksi tietokannan luominen). Määritetty virheilmoituksen palvelin on ylittänyt enimmäismäärä samanaikaisia yhteyksiä. Yritä uudelleen myöhemmin.|
|45169|16|SQL azure-järjestelmä on kohdassa Lataa ja sijoittaa yläraja samanaikainen palvelimen CRUD toimintojen yksittäisen tilauksen määrä (esimerkiksi luominen server). Määritetty virheilmoituksen tilaus on ylittänyt enimmäismäärä samanaikaisia yhteyksiä ja hylättiin. Yritä uudelleen myöhemmin.|

## <a name="related-links"></a>Aiheeseen liittyvät linkit

- [Azure SQL-tietokannan Yleiset rajoitukset ja ohjeita](sql-database-general-limitations.md)
- [Azure SQL-tietokantaan resurssien rajoitukset](sql-database-resource-limits.md)
