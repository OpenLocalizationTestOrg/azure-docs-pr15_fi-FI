<properties
   pageTitle="Ei tue Azure SQL-tietokannan T-SQL | Microsoft Azure"
   description="Transact-SQL-lauseita, jotka ovat vähemmän kuin täysin tueta Azure SQL-tietokanta"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-tietokannan Transact-SQL-erot


Useimpia Transact-SQL-ominaisuuksia, jotka riippuvat sovelluksia tuetaan sekä Microsoft SQL Server Azure SQL-tietokantaan. Osittainen luettelo sovellusten tuetut ominaisuudet seuraavasti:

- Tietotyypit.
- Operaattorit.
- Merkkijono, aritmeettinen looginen, ja kohdistin toiminnot.

Kuitenkin Azure SQL-tietokanta on suunniteltu eristämään **perusmuodon** tietokannan kaikki riippuvuus-toimintoja. Näin monta palvelintason toiminnot eivät sovellu SQL-tietokantaan ja ei tueta. Ominaisuudet, jotka on poistettu SQL Server yleensä ei tueta SQL-tietokantaan.

> [AZURE.NOTE]
> Tässä ohjeaiheessa kerrotaan SQL-tietokantaan, kun päivitetään uusimpaan versioon, käytettävissä olevat ominaisuudet SQL-tietokannan Vipuventtiili V12. Saat lisätietoja Vipuventtiili V12 [SQL tietokanta Vipuventtiili V12 mitä 's New](sql-database-v12-whats-new.md).

Seuraavissa osissa on lueteltu ominaisuudet, jotka ovat osittain tuettuja ja ominaisuudet, joita ei täysin tueta.


## <a name="features-partially-supported-in-sql-database-v12"></a>SQL-tietokannan Vipuventtiili V12 osittain tuetut ominaisuudet

SQL-tietokannan Vipuventtiili V12 tukee vastaavat SQL Server 2016 Transact-SQL-lauseet joihinkin mutta ei kaikkiin argumentit, jotka ovat olemassa. CREATE PROCEDURE-lause on esimerkiksi käytettävissä, mutta kaikki CREATE PROCEDURE asetukset eivät ole käytettävissä. Katso lisätietoja tuetut alueet kunkin linkitetyn syntaksi-aiheisiin.

- Tietokannat: [Luo](https://msdn.microsoft.com/library/dn268335.aspx )/[muuttaa](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs on yleisesti saatavilla olevia ominaisuuksia.
- Funktiot: [Luo](https://msdn.microsoft.com/library/ms186755.aspx)/[FUNKTIO muuttaa](https://msdn.microsoft.com/library/ms186967.aspx)
- [POISTA](https://msdn.microsoft.com/library/ms173730.aspx) 
- Kirjautumiset: [Luo](https://msdn.microsoft.com/library/ms189751.aspx)/[muuttaa kirjautuminen](https://msdn.microsoft.com/library/ms189828.aspx)
- Tallennettujen toimintosarjojen: [Luo](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Taulukot: [Luo](https://msdn.microsoft.com/library/dn305849.aspx)/[muuttaa](https://msdn.microsoft.com/library/ms190273.aspx)
- Tyypit (mukautettu): [Luo tyyppi](https://msdn.microsoft.com/library/ms175007.aspx)
- Käyttäjät: [Luo](https://msdn.microsoft.com/library/ms173463.aspx)/[muuttaa käyttäjä](https://msdn.microsoft.com/library/ms176060.aspx)
- Näkymät: [Luo](https://msdn.microsoft.com/library/ms187956.aspx)/[Muuta NÄKYMÄ](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Ominaisuuksia ei tueta SQL-tietokantaan

- Järjestelmän objektien lajittelu
- Yhteyden liittyvät: päätepisteen lauseet ORIGINAL_DB_NAME. SQL-tietokanta ei tue todennusta, mutta tuetaan samalla Azure Active Directory-todentamista. Jotkin todennustyypit edellyttävät SSMS uusimman version. Lisätietoja on artikkelissa [yhteyden muodostaminen SQL-tietokanta tai SQL tietojen varaston mukaan käyttämällä Azure Active Directory-todennusta](sql-database-aad-authentication.md).
- Toimintojen tietokantakyselyt kolme tai neljä osaa nimien käyttäminen. (Vain luku-rajat-tietokantakyselyt tuetaan [joustavasti tietokantakyselyn](sql-database-elastic-query-overview.md)avulla.)
- Cross tietokannan omistajuus ketjutus, LUOTETTAVA asetus
- Tietojen kerääminen
- Tietokannan kaaviot
- Tietokannan sähköposti
- DATABASEPROPERTY (Käytä DATABASEPROPERTYEX sijaan)
- Suorita AS kirjautumiset
- Salaus: extensible hallintaa
- Eventing: tapahtumat, tapahtumailmoituksia, kysely-ilmoitukset
- Tietokannan tiedostojen sijaintia, kokoa ja tietokannoista, joka hallitsee automaattisesti Microsoft Azure liittyviä ominaisuuksia.
- Ominaisuudet, jotka liittyvät suuren käytettävyyden, jossa hallitaan Microsoft Azure-tilisi kautta: varmuuskopioiminen, palauttaminen, AlwaysOn, tietokantapeilausta, loki toimitus palautus-tilassa. Lisätietoja, katso Azure SQL-tietokannan varmuuskopiointi ja palauttaminen.
- Ominaisuudet, joita SQL-tietokanta tietokoneessa log lukija luottaa: Push-replikoinnin, muuta tietojen Tallenna.
- Ominaisuudet, jotka ovat riippuvaisia SQL Server Agent tai msdb: Tietokanta: työt, ilmoitukset, operaattoreita, käytäntö johdolle, tietokannan posti, keskitetyn hallinnan palvelimiin.
- FILESTREAM
- Funktiot: fn_get_sql, fn_virtualfilestats, fn_virtualservernodes
- Yleinen väliaikaisia taulukoita
- Laitteiston liittyvät palvelinasetukset: muistin, työntekijä viestiketjuissa siirtyminen, Suorittimen affiniteetti, Jäljitä merkinnät jne. Käytä palvelutasot.
- HAS_DBACCESS
- LOPETTAA TILASTO TYÖ
- Linkitettyjä palvelimia, AVAAKYSELY, OPENROWSET, OPENDATASOURCE, JOUKKONA Lisää ja neliapila nimet
- Perustyyli/kohde-palvelimet
- .NET framework [CLR SQL Server-integrointi](http://msdn.microsoft.com/library/ms254963.aspx)
- Resurssin säädin
- Semanttinen haku
- Server-tunnistetiedot. Käytä tietokantaa määritetty tunnistetietojen sijaan.
- Palvelimen tason kohteiden: Server rooleja, IS_SRVROLEMEMBER, sys.login_token. Palvelimen tason käyttöoikeudet eivät ole käytettävissä, mutta jotkin korvataan tietokannan tason käyttöoikeudet. Jotkin palvelintason DMVs eivät ole käytettävissä, mutta jotkin korvataan tietokannan tason DMVs.
- Serverless express: localdb, käyttäjän esiintymiä
- Service Brokeria
- MÄÄRITÄ REMOTE_PROC_TRANSACTIONS
- SULKEMINEN
- sp_addmessage
- sp_configure asetukset ja määritä uudelleen. Jotkin asetukset ovat käytettävissä [Muuta TIETOKANNAN SUODATETUT määrityksen](https://msdn.microsoft.com/library/mt629158.aspx)avulla.
- sp_helpuser
- sp_migrate_user_to_contained
- SQL Server-valvonta. Käytä SQL-tietokantaan tarkistaminen sijaan.
- SQL Server Profilerin
- SQL Server-jäljitys
- Jäljitä merkinnät. Jäljitä merkintä kohteita on siirretty Yhteensopivuus-tilassa.
- Transact-SQL-virheenkorjaus
- Käynnistimien: Palvelimen kohdistettu tai kirjautuminen Käynnistimet
- Käytä lausetta: Jos haluat muuttaa tietokannan kontekstia toisen tietokannan, sinun on tehtävä uuden yhteyden uuteen tietokantaan.


## <a name="full-transact-sql-reference"></a>Koko Transact-SQL-viittaus

Lisätietoja Transact-SQL-tarkistus, käyttö ja esimerkkejä Hae SQL Server Books Online- [Transact-SQL-viittaus (tietokantaohjelma)](https://msdn.microsoft.com/library/bb510741.aspx) . 

### <a name="about-the-applies-to-tags"></a>Tietoja "Koskee"-tunnisteet

Transact-SQL-viittaus sisältää päiväksi versiot SQL Server 2008: n liittyvien aiheiden. On ohjeaiheessa otsikon alapuolella on kuvakkeen palkki-, luettelo neljä SQL Server-ympäristöissä ja osoittaa puolivälissä. Esimerkiksi käytettävyys ryhmät Macin SQL Server 2012. [Luo AVAILABILTY ryhmä](https://msdn.microsoft.com/library/ff878399.aspx) aiheen ilmaisee, että lause käyttää ** SQL Server (alkaa numerosta 2012). Laskelma ei koske SQL Server 2008, SQL Server 2008 R2, SQL Azure-tietokannan, Azure SQL-tietovarasto tai Parallel Data Warehouse.

Joissakin tapauksissa Yleiset aihe aihe voi käyttää tuotetta, mutta on tuotteiden vähäisiä eroja. Erot on merkitty osoitteessa keskipisteitä artikkelissa tarpeen mukaan.

