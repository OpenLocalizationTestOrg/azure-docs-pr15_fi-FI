<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokantaan C#-querylla | Microsoft Azure"
    description="Kirjoita ohjelman C# kyselyyn ja yhteyden muodostaminen SQL-tietokantaan. Voit lukea lisää IP-osoitteet, yhteysmerkkijonon, suojattu kirjautuminen ja vapaa Visual Studio."
    services="sql-database"
    keywords="c# tietokantakyselyn, c# kyselyn yhteyden tietokantaan, SQL-C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Yhteyden muodostaminen Visual Studiossa SQL-tietokantaan

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Opettele muodostamaan Visual Studiossa Azure SQL-tietokantaan. 

## <a name="prerequisites"></a>Edellytykset


Yhteyden muodostaminen SQL-tietokantaan Visual Studiossa edellyttää seuraavia: 


- SQL-tietokanta, johon yhteys muodostetaan. Tässä artikkelissa käytetään **AdventureWorks** mallitietokannassa. AdventureWorks mallitietokannan asentamisesta [Luo esittely-tietokanta](sql-database-get-started.md).


- Visual Studio 2013 Päivitä 4 (tai uudempi). Microsoft toimittaa Visual Studio yhteisön nyt *ilmaiseksi*.
 - [Visual Studio yhteisön ja lataaminen](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Lisää asetuksia vapaa Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Avaa Visual Studio Azure-portaalista


1. Kirjaudu sisään [Azure portal](https://portal.azure.com/).

2. Valitse **Lisää palveluja** > **SQL-tietokannat**
3. Avaa **AdventureWorks** tietokannan sivu etsiminen ja valitsemalla *AdventureWorks* -tietokantaan.

6. Napsauta tietokannan sivu yläreunassa **Työkalut** -painiketta:

    ![Uusi kysely. Yhteyden muodostaminen SQL-tietokanta: SQL Server Management Studiossa](./media/sql-database-connect-query/tools.png)

7. Valitse **Avaa Visual Studiossa** (Jos tarvitset Visual Studiossa, napsauta linkkiä):

    ![Uusi kysely. Yhteyden muodostaminen SQL-tietokanta: SQL Server Management Studiossa](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio avautuu **Muodosta yhteys palvelimeen** -ikkunassa voit muodostaa yhteyden palvelin ja tietokanta-portaalissa on jo määritetty.  (Valitse Vahvista, että yhteys on määritetty oikein tietokannan **asetukset** .) Palvelimen järjestelmänvalvojan salasanasi ja valitse **Yhdistä**.


    ![Uusi kysely. Yhteyden muodostaminen SQL-tietokanta: SQL Server Management Studiossa](./media/sql-database-connect-query/connect.png)


8. Jos sinulla ei ole palomuurin sääntöjoukon sinua käyttäjäksi tietokoneen IP-osoite, saat *muodostamasta* viestin tähän. Palomuurisäännön luomisesta on artikkelissa [Azure SQL-tietokanta-palvelintason palomuurisäännön määrittäminen](sql-database-configure-firewall-settings.md).


9. Kun muodostettu yhteys **SQL Server Object Explorer** -ikkuna avautuu yhteydessä tietokantaan.

    ![Uusi kysely. Yhteyden muodostaminen SQL-tietokanta: SQL Server Management Studiossa](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Esimerkkikyselyn suorittaminen

Nyt, kun Microsoft on yhteys tietokantaan, yksinkertaisen kyselyn suorittaminen Näytä seuraavasti:

2. Tietokannan hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

    ![Uusi kysely. Yhteyden muodostaminen SQL-tietokanta: SQL Server Management Studiossa](./media/sql-database-connect-query/new-query.png)

3. Valitse kyselyikkunassa kopioi ja liitä seuraava koodi.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Suorita kysely valitsemalla **Suorita** :

    ![Onnistui. Yhteyden muodostaminen SQL-tietokanta: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- SQL-tietokantoja avaaminen Visual Studiossa käyttää SQL Server Data Tools. Lisätietoja on artikkelissa [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Muodostaa yhteyden SQL-tietokanta käyttämällä ohjeaiheessa [yhteyden muodostaminen SQL-tietokantaan käyttämällä .NET (C#)](sql-database-develop-dotnet-simple.md).



