<properties
   pageTitle="Azure SQL-tietokannan suojausohjeita ja rajoituksia | Microsoft Azure"
   description="Tutustu Microsoft Azure SQL-tietokanta-ohjeita ja suojausta koskevat rajoitukset."
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
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Azure SQL-tietokannan suojausohjeita ja rajoitukset

Tässä artikkelissa kuvataan Microsoft Azure SQL-tietokanta-ohjeita ja suojausta koskevat rajoitukset. Ota seuraavat seikat huomioon, kun Azure SQL-tietokantoja suojauksen hallinta.

## <a name="access-to-the-virtual-master-database"></a>Virtuaalinen perusmuodon tietokannan

Vain järjestelmänvalvoja on yleensä access tietokannasta. Säännöllisestä pääsy kunkin käyttäjän tietokannassa on oltava järjestelmänvalvoja sisältämät tietokannan käyttäjien luotujen tietokantojen kautta. Kun sisältämät tietokannan käyttäjiä, sinun ei tarvitse kirjautumiset perusmuodon tietokannan luominen. Lisätietoja on artikkelissa [Sisälsi tietokannan käyttäjiä - että Your tietokannan kannettavat](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Palomuurin

SQL Server-palomuurin, joka on määritetty koko Azure SQL Server on yleensä määritetty portaalin kautta ja olisi vain Hyväksy järjestelmänvalvojien IP-osoitteet. Voit luoda tietokannan kohdistettu palomuurisäännön, joka avaa jokaiselle tietokannalle edellyttämät IP-osoitteet, käyttäjän-tietokantayhteyden muodostamisessa ja käyttämällä [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-lause.

Azure SQL-tietokanta-palvelu on käytettävissä vain TCP-portin 1433 kautta. Voit käyttää SQL-tietokanta tietokoneessa, varmista, että asiakkaan tietokoneen palomuurin sallii lähtevät TCP tietoliikenteen TCP-porttia 1433. Jos muut sovellukset ei tarvita, estä porttinumeroa 1433 saapuvat yhteydet. 

Osana yhteyden muodostamisprosessi Azuren näennäiskoneiden yhteydet ohjataan eri IP-osoite ja portin yksilöllinen kunkin työntekijän roolin. Porttinumero on alueella 11000 11999. Saat lisätietoja TCP-portit [portit 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12 lisäksi](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Todennus

Käytä Active Directory-todennusta (integroitu suojaus) aina, kun se on mahdollista. Lisätietoja AD-käyttöoikeuksien määrittämisestä on artikkelissa [yhteyden muodostaminen SQL tietokanta mukaan käyttämällä Azure Active Directory-todennusta](sql-database-aad-authentication.md)ja SQL Server Books Online- [todennus-tilan valitseminen](https://msdn.microsoft.com/library/ms144284.aspx) . 

Käytä sisälsi tietokannan käyttäjiä varten skaalattavuus. Lisätietoja on artikkelissa [Sisälsi tietokannan käyttäjiä - että Your tietokannan kannettavaan](https://msdn.microsoft.com/library/ff929188.aspx) [Luo käyttäjä (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)tai [Sisälsi tietokannat](https://technet.microsoft.com/library/ff929071.aspx).

Database Engine sulkee yhteydet, jotka ovat käyttämättömänä yli 30 minuuttia. Yhteys on kirjautuminen uudelleen, ennen kuin sitä voidaan käyttää.

Jatkuvasti aktiivisen yhteyden muodostaminen SQL-tietokanta vaatii reauthorization (maksettavan korvauksen Database Engine) vähintään kerran 10 tuntia. Database Engine yrittää reauthorization alun perin lähetetyn salasanalla ja käyttäjän toimia ei tarvita. Suorituskyvyn parantamiseksi uuden salasanan SQL-tietokantaan, kun yhteyttä ei ole langattomilla, vaikka yhteys palautetaan vuoksi yhteysvarannon. Tämä on erilainen kuin paikallisen SQL Serverin toimintaa. Jos salasana on vaihdettu, koska yhteys on alun perin myönnetty, yhteys on lopetettava ja uusi yhteys tehdään käyttämällä uutta salasanaa. Käyttäjä, jolla lopettaa TIETOKANTAYHTEYS oikeudet erikseen voit lopettaa yhteyden SQL-tietokantaan [Poista](https://msdn.microsoft.com/library/ms173730.aspx) -komennon avulla.

## <a name="logins-and-users"></a>Kirjautumiset ja käyttäjille

Kun kirjautumiset ja SQL-tietokannan käyttäjien hallintaa, ole rajoituksia.


- Sinun on oltava yhdistettynä **perusmuodon** tietokannan suoritettaessa ``CREATE/ALTER/DROP DATABASE`` lauseita. Vastaavat palvelintason pääasiallista kirjautuminen perusmuodon tietokannan tietokanta-käyttäjä ei voi muuttaa tai kohteet poistetaan. 
- Englannin (Yhdysvallat) on palvelintason principal kirjautuminen oletuskielen.
- Vain järjestelmänvalvojat (palvelintason pääasiallista kirjautuminen tai Azure AD-järjestelmänvalvojan) ja **perusmuodon** tietokannan **dbmanager** tietokanta-roolin jäsenet hallintaoikeutta suorittamaan ``CREATE DATABASE`` ja ``DROP DATABASE`` lauseita.
- Sinun on oltava yhdistettynä perusmuodon tietokannan suoritettaessa ``CREATE/ALTER/DROP LOGIN`` lauseita. Kuitenkin käyttämällä kirjautumiset ei suositella. Käytä sisältämät tietokannan käyttäjiä.
- Käyttäjä-tietokantayhteyden muodostamisessa, sinun on määritettävä yhteysmerkkijonon tietokannan nimi.
- Vain palvelintason pääasiallista kirjautuminen ja **perusmuodon** tietokannan **loginmanager** tietokanta-roolin jäsenet on oikeus suorittaa ``CREATE LOGIN``, ``ALTER LOGIN``, ja ``DROP LOGIN`` lauseita.
- Kun suoritat ``CREATE/ALTER/DROP LOGIN`` ja ``CREATE/ALTER/DROP DATABASE`` lauseet ADO.NET-sovelluksessa, Parametroitu komennoilla ei sallita. Lisätietoja on artikkelissa [komentoja ja parametreja](https://msdn.microsoft.com/library/ms254953.aspx).
- Kun suoritat ``CREATE/ALTER/DROP DATABASE`` ja ``CREATE/ALTER/DROP LOGIN`` lauseet, kaikki nämä on oltava vain lause Transact-SQL-osalta. Muussa tapauksessa tapahtuu virhe. Esimerkiksi seuraavat Transact-SQL tarkistaa, onko tietokanta on olemassa. Jos se on olemassa ``DROP DATABASE`` lauseen kutsutaan Poista tietokanta. Koska ``DROP DATABASE`` lause ei ole vain lauseen osalta, suorittamalla seuraavat Transact-SQL-lause aiheuttaa virheen.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Suoritettaessa ``CREATE USER`` lauseen kanssa ``FOR/FROM LOGIN`` vaihtoehto, sen on oltava vain lause Transact-SQL-osalta.
- Suoritettaessa ``ALTER USER`` lauseen kanssa ``WITH LOGIN`` vaihtoehto, sen on oltava vain lause Transact-SQL-osalta.
- Voit ``CREATE/ALTER/DROP`` käyttäjän edellyttää, että ``ALTER ANY USER`` tietokannan käyttöoikeudet.
- Kun tietokantaroolin omistaja yrittää lisätä tai poistaa toisen tietokannan käyttäjän tai tietokannan kyseiseen roolista, seuraava virhe voi ilmetä: **käyttäjä tai rooli "Nimi" ei ole tässä tietokannassa.** Tämä virhe ilmenee, koska käyttäjä ei ole näkyvissä omistajalle. Voit ratkaista ongelman myöntää roolin omistaja ``VIEW DEFINITION`` käyttäjän käyttöoikeus. 

Saat lisätietoja kirjautumiset ja käyttäjien [tietokantojen hallinta ja kirjautumiset Azure SQL-tietokantaan](sql-database-manage-logins.md).


## <a name="see-also"></a>Katso myös

[Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md)

[Toimintaohje: palomuurin asetusten (Azure SQL-tietokanta)](sql-database-configure-firewall-settings.md)

[Tietokantojen ja Azure SQL-tietokantaan kirjautumiset hallinta](sql-database-manage-logins.md)

[Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589)
