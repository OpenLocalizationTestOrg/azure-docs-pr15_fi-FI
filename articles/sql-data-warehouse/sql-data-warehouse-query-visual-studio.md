<properties
   pageTitle="Kyselyn Azure SQL-tietovarasto (Visual Studiossa) | Microsoft Azure"
   description="Kyselyn SQL-tietovarasto Visual Studiossa."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Kyselyn Azure SQL-tietovarasto (Visual Studiossa)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure koneen oppiminen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Visual Studio avulla kyselyn SQL Azure-tietovarasto vain muutaman minuutin kuluttua. Tässä menetelmässä SQL Server Data Tools (SSDT)-tunniste Visual Studiossa. 

## <a name="prerequisites"></a>Edellytykset

Jos haluat käyttää tässä opetusohjelmassa, sinun on:

+ SQL tietovarasto. Voit luoda luettelon artikkelissa [luominen SQL-tietovarasto][].
+ Visual Studio SSDT. Jos sinulla on Visual Studiossa, todennäköisesti on jo tämä. Katso asennusohjeet ja asetukset- [asennuksen Visual Studio ja SSDT][].
+ Täydellinen SQL-palvelimen nimi. Tämä on artikkelissa [yhteyden muodostaminen SQL-tietovarasto][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. yhteyden muodostaminen SQL-tietovarasto

1. Avaa Visual Studio 2013 tai 2015.
2. Avaa SQL Server objektin Resurssienhallinta. **Näkymän tällöin** > **SQL Server Object Explorerissa**.

    ![SQL Server Object Explorerissa][1]

3. Napsauta **Lisää SQL Server** -kuvaketta.

    ![Lisää SQL Server][2]

4. Täytä kentät, valitse palvelin-ikkunaan Yhdistä.

    ![Muodostaa yhteyttä palvelimeen][3]

    - **Palvelimen nimi**. Kirjoita **palvelimen nimi** aiemmin määritetty.
    - **Todennusta**. Valitse **SQL Server-todennuksen** tai **Active Directory-todennusta**.
    - **Käyttäjänimi** ja **salasana**. Kirjoita käyttäjänimi ja salasana, jos SQL Server-todennus on valittu.
    - Valitse **Muodosta yhteys**.

5. Tutustu, laajenna Azure SQL-palvelimeen. Voit tarkastella palvelimeen liittyviä tietokantoja. Laajenna AdventureWorksDW näet otoksen tietokannan taulukot.

    ![Tutustu AdventureWorksDW][4]

## <a name="2-run-a-sample-query"></a>2. esimerkkikyselyn suorittaminen

Nyt kun yhteys on muodostettu tietokantaan, kirjoittaa oletetaan, että kyselyn.

1. Napsauta hiiren kakkospainikkeella tietokannan SQL Server objektin Resurssienhallinnassa.

2. Valitse **Uusi kysely**. Näyttöön avautuu uusi kyselyikkuna.

    ![Uusi kysely][5]

3. Kopioi kyselyikkunan TSQL kysely.

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Suorita kysely. Voit tehdä tämän napsauttamalla vihreää nuolta tai seuraavat pikanäppäin: `CTRL` + `SHIFT` + `E`.

    ![Suorita kysely][6]

5. Tarkista kyselyn tuloksiin. Tässä esimerkissä FactInternetSales taulukossa on 60398 rivit.

    ![Kyselytulokset][7]

## <a name="next-steps"></a>Seuraavat vaiheet

Voit yhdistää ja kysely, yritä [kesäolympialaisten visualisointi PowerBI tiedot][].

Ympäristön Azure Active Directory käyttöoikeuksien määrittämiseen on ohjeaiheessa [Tarkista SQL-tietovarasto][].

<!--Arcticles-->
[Yhteyden muodostaminen SQL-tietovarasto]: sql-data-warehouse-connect-overview.md
[SQL-tietovarasto luominen]: sql-data-warehouse-get-started-provision.md
[Visual Studio ja SSDT asentaminen]: sql-data-warehouse-install-visual-studio.md
[SQL-tietovarasto todentaminen]: sql-data-warehouse-authentication.md
[kesäolympialaisten visualisointi PowerBI tiedot]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
