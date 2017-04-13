<properties
   pageTitle="SQL-tietovarasto-tietokannan | Microsoft Azure"
   description="Azure SQL-tietovarasto tietokannan suojaaminen, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>SQL-tietovarasto-tietokannan suojaaminen

> [AZURE.SELECTOR]
- [Yleistä suojauksesta](sql-data-warehouse-overview-manage-security.md)
- [Todennus](sql-data-warehouse-authentication.md)
- [Salauksen (portaalin)](sql-data-warehouse-encryption-tde.md)
- [Salauksen (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Tässä artikkelissa käydään läpi Azure SQL-tietovarasto tietokannan suojaaminen perusteet. Erityisesti tässä artikkelissa pääset alkuun resurssit rajoittaminen access-tietojen suojaaminen ja valvonta toimintoja tietokantaa.

## <a name="connection-security"></a>Yhteyden suojaus

Yhteyden suojaus viittaa siitä, miten voit rajata ja palomuurisäännöt ja yhteyden salaus tietokannan suojattuja yhteyksiä.

Palvelin ja tietokanta käytetään palomuurisäännöt IP-osoitteista, joita ei ole nimenomaisesti whitelisted yhteyden yritykset Hylkää. Voivat muodostaa yhteyden sovelluksen tai asiakkaan tietokoneen julkiseen IP-osoite, sinun on luotava palvelintason palomuurisäännön Azure-portaalissa, REST API tai PowerShellin avulla. Paras käytäntö tulee rajoita sallittu server-palomuurin läpi mahdollisimman paljon IP-osoitealueet.  Voit käyttää SQL Azure-tietovarasto paikallisesta tietokoneesta, varmista verkon ja paikallisen tietokoneen palomuuri antaa lähtevän tietoliikenteen TCP-porttia 1433.  Lisätietoja on artikkelissa [Azure SQL-tietokantaan palomuurin][], [sp_set_firewall_rule][]ja [sp_set_database_firewall_rule][].

SQL-tietovarasto yhteydet salataan oletusarvoisesti.  Muokkaaminen yhteysasetukset salauksen poistaminen käytöstä oteta huomioon.

## <a name="authentication"></a>Todennus

Todennus viittaa siitä, miten voit todistaa henkilöllisyytesi, kun tietokantayhteyden muodostamisessa. SQL-tietovarasto tukee tällä hetkellä SQL Server-todennus käyttäjänimi ja salasana sekä Azure Active Directory. 

Luodessasi looginen palvelimen tietokannan määrittämäsi "palvelimen järjestelmänvalvojan" kirjautumisen käyttäjänimellä ja salasanalla. Käytä näitä tunnistetietoja, voi todentaa minkä tahansa tietokannan palvelimen tietokannan omistaja tai "dbo" SQL Server-todennuksen avulla.

Kuitenkin paras käytäntö on organisaation käyttäjien tulisi käyttää eri tilillä tarkistamiseen. Tällä tavalla voit rajoittaa sovelluksen käyttöoikeudet ja vähentää haitallista tehtävän riskit, jos sovelluksen koodin ei koske SQL-lisäämisen hyökkäyksen. 

Voit luoda SQL Server-todennettu käyttäjän palvelimeen ja palvelimen järjestelmänvalvojan kirjautumisen **master** -tietokantayhteyden muodostamisessa ja luo uusi server-kirjautuminen.  Lisäksi on hyvä käyttäjän luominen perusmuodon tietokannan Azure SQL-tietovarasto käyttäjille. Käyttäjän luominen perustyyli avulla käyttäjä voi kirjautua sisään työkaluilla, kuten SSMS määrittämättä tietokannan nimi.  Se myös antaa niiden objektin Resurssienhallinnan avulla voit tarkastella kaikkien tietokantojen SQL Server.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Sitten muodostaa yhteys **SQL-tietovarasto tietokanta** palvelimen järjestelmänvalvojan kirjautumisen kanssa ja luo tietokannan käyttäjä juuri luomasi server-kirjautuminen perusteella.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Jos käyttäjä suorittamalla muita toimintoja, kuten kirjautumiset tai uusien tietokantojen, tiedot myös on määritettävä `Loginmanager` ja `dbmanager` perusmuodon tietokannan roolit. Katso lisätietoja ohjesivustojen muokkaamisesta rooli ja todennustapa SQL-tietokantaan, [tietokantojen hallinta ja kirjautumiset Azure SQL-tietokantaan][].  Lisätietoja SQL-tietovarasto Azure Active Directory on artikkelissa [yhteyden muodostaminen SQL tietojen varaston mukaan käyttämällä Azure Active Directory-todennusta][].


## <a name="authorization"></a>Todennus

Luvan viittaa miten Azure SQL-tietovarasto-tietokantaan ja tämä määräytyy käyttäjätilisi roolien jäsenyys ja käyttöoikeudet. Paras käytäntö tulee Myönnä käyttäjille vähintään tarvittavat oikeudet. Azure SQL-tietovarasto on tämä T-SQL-roolien hallinta on helppoa:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Palvelimen järjestelmänvalvoja-tili, johon muodostat yhteyden kuuluu db_owner, jolla on oikeus tehdä mitään tietokannasta. Tallenna rakenne päivitykset ja muut hallintatoiminnot käyttöönoton tähän tiliin. Sovelluksen tarvitsemia vähimmäiskäyttöoikeuksin yhteydessä tietokantaan sovelluksestasi Lisää rajoitetuilla käyttöoikeuksilla "ApplicationUser"-tilin avulla.

Usealla tavalla voit rajata mitä käyttäjä voi tehdä Azure SQL-tietokantaan:

- Hajautetun [käyttöoikeuksien][] avulla voit määrittää, mitkä toiminnot Tee yksittäiset sarakkeet, taulukoiden, näkymien, menettelyt ja muihin tietokannan objekteihin. Hajautetun käyttöoikeuksien avulla voit hallinta ja antaa vähintään tarvittavat oikeudet. Hajautetun käyttöoikeus-järjestelmä on hieman monimutkainen ja edellyttävät joitakin tutkimuksen tehokas käyttö.
- [Tietokannan rooleja][] kuin db_datareader ja db_datawriter voidaan luoda tehokkaampia sovelluksen käyttäjätilit tai vähemmän tehokas hallinta-tilit. Valmiin pysyvät tietokannan roolit avulla on helppo Myönnä käyttöoikeuksia, mutta voi johtaa myöntää Lisää oikeuksia kuin on tarpeen.
- [Tallennettu toimintosarja][] voidaan rajoittaa toimintoja, jotka voidaan ottaa käyttöön tietokantaa.

Tietokantojen ja looginen palvelinten hallinta perinteinen Azure-portaalista tai Azure Resurssienhallinta-Ohjelmointirajapinnan käyttäminen ohjataan portaalin käyttäjätilin roolimäärityksiä. Lisätietoja tämän artikkelin kohdassa [Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa][].

## <a name="encryption"></a>Salaus

Azure SQL tietojen varasto läpinäkyvä tietojen salaus (TDE) auttaa suojaamaan tietokonetta haittaohjelmien toiminnan suorittamalla reaaliaikainen salausta ja salauksen tietojen, loput.  Voit salata tietokannan, kun liittyvä varmuuskopiot ja tapahtuman lokitiedostojen salataan tarvitsematta sovelluksiin muutoksia. TDE salaa koko tietokannan tallennustilaan kutsutaan tietokannan salausavaimen symmetrisen avaimen avulla. SQL-tietokantaan tietokannan salausavain on suojattu valmiin palvelinvarmennetta. Valmiin palvelinvarmennetta on yksilöllinen kunkin SQL-tietokantapalvelimen. Microsoft kiertää nämä varmenteet automaattisesti vähintään kerran 90 päivää. SQL-tietovarasto salausalgoritmi on AES-256. Katso TDE yleiskuvaus, [Läpinäkyvä salauksen][].

Voit salata tietokannan [Azure Portal] [ Encryption with Portal] tai [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Seuraavat vaiheet

Tiedot ja esimerkit SQL-tietovarasto käyttämällä eri protokollia muodostamisesta on artikkelissa [yhteyden muodostaminen SQL-tietovarasto][].

<!--Image references-->

<!--Article references-->
[Yhteyden muodostaminen SQL-tietovarasto]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Yhteyden muodostaminen SQL-tietovarasto Azure Active Directory-todennuksen avulla]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL-tietokantaan palomuurin]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Tietokannan roolit]: https://msdn.microsoft.com/library/ms189121.aspx
[Tietokantojen ja Azure SQL-tietokantaan kirjautumiset hallinta]: https://msdn.microsoft.com/library/ee336235.aspx
[Käyttöoikeudet]: https://msdn.microsoft.com/library/ms191291.aspx
[Tallennettujen toimintosarjojen]: https://msdn.microsoft.com/library/ms190782.aspx
[Läpinäkyvä tiedon salaus]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
