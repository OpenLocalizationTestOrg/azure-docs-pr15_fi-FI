<properties
    pageTitle="Yhteyden muodostaminen SQL-tietokantaan Exceliin | Microsoft Azure"
    description="Opettele Microsoft Excelin yhdistäminen pilveen Azure SQL-tietokantaan. Voit tuoda tietoja Exceliin, raportointi ja tietojen tarkasteluun."
    services="sql-database"
    keywords="Yhdistä excel-SQL, tuoda tietoja Exceliin"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>SQL-tietokanta-opas: Excel muodostaa Azure SQL-tietokantaan ja raportin luominen

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Opettele muodostamaan Excel SQL-tietokantaan pilveen, jotta voit tuoda tietoja ja luoda taulukoita ja kaavioita tietokannan arvojen perusteella. Tässä opetusohjelmassa, voit määrittää Excel- ja tietokannan taulukon välinen yhteys tallentaa tiedoston, joka tallentaa tiedot ja yhteystiedot, Excel ja luo pivot-kaavion tietokannan arvoista.

Tarvitset SQL Azure-tietokannassa, ennen kuin aloitat. Jos sinulla ei ole, katso [ensimmäisen SQL-tietokannan luominen](sql-database-get-started.md) aloittaa tietokannan mallitiedot ylös-ja käynnissä muutaman minuutin kuluttua. Tässä artikkelissa tuoda mallitiedot Exceliin kyseisen artikkelista, mutta voit tehdä vastaavat vaiheet omilla tiedoillasi.

Sinun on myös Excel-ikkunan. Tässä artikkelissa käytetään [Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Excelin muodostaa yhteyden SQL-tietokantaan ja odc-tiedoston luominen

1.  Muodostaa yhteyden SQL-tietokantaan Excel avaa Excel ja luo uusi työkirja tai avaa aiemmin luotu Excel-työkirja.

2.  Sivun yläreunassa valikkorivillä **tiedot**ja **muista**lähteistä **SQL Serveristä**.

    ![Valitse tietolähde: Excel muodostaa yhteyden SQL-tietokantaan.](./media/sql-database-connect-excel/excel_data_source.png)

    Avaa ohjatun tietoyhteyden muodostamisesta.

3.  **Yhteyden muodostaminen tietokantapalvelimeen** -valintaikkunassa Kirjoita SQL-tietokanta **palvelimen nimi** , jonka haluat muodostaa yhteyden lomakkeen <*palvelimen nimi*>**. database.windows.net**. Esimerkiksi **adworkserver.database.windows.net**.

4.  Valitse **kirjautumisen tunnistetiedot**-kohdassa **seuraavat käyttäjänimen ja salasanan**, **Käyttäjänimi** ja **salasana** SQL-tietokantapalvelimen määrittäminen, kun olet luonut sen, ja valitse sitten **Seuraava**.

    ![Kirjoita palvelimen nimi ja kirjaudu sisään tunnistetiedot](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Verkko-käyttöympäristön mukaan ei ehkä pysty muodostamaan yhteyttä tai yhteys saatetaan menettää, jos SQL-tietokantapalvelinta ei salli tietoliikenteen asiakkaan IP-osoite. Siirry [Azure portal](https://portal.azure.com/), napsauta SQL-palvelimet, valitse palvelimellesi, valitse palomuurin asetukset-kohdassa ja lisää asiakkaan IP-osoite. Katso lisätietoja [palomuurin asetusten määrittämisestä](sql-database-configure-firewall-settings.md) .

5. **Valitse tietokanta ja taulukko** -valintaikkunassa valitse toimimaan luettelosta haluamasi tietokanta ja valitse sitten taulukoiden tai näkymien haluat käsitellä (Microsoft valitsi **vGetAllCategories**), ja valitse sitten **Seuraava**.

    ![Valitse tietokanta ja taulukko.](./media/sql-database-connect-excel/select-database-and-table.png)

    **Tallenna tietoyhteystiedosto ja Lopeta** -valintaikkuna avautuu, johon lisätä Office-tietokannan yhteys (*.odc) tiedoston, jota Excel käyttää tietoja. Voit jättää oletusasetukset tai Mukauta valintoja.

6. Voit jättää oletusarvot, mutta **Tiedostonimi** muistiin erityisesti. **Kuvaus**, **Kutsumanimi**ja **Hakusanojen** auttaa ja muiden käyttäjien muista, mitä olet muodostamassa yhteyttä ja Etsi yhteys. Valitse **aina yrittää käyttää tätä tiedostoa, jos haluat päivittää tiedot** , jos haluat yhteyden tietoja tallennetaan odc-tiedosto, jotta se päivittyy, kun muodostaa yhteyden, ja valitse sitten **Valmis**.

    ![Odc-tiedoston tallentaminen](./media/sql-database-connect-excel/save-odc-file.png)

    **Tuontitiedot** -valintaikkuna.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Tuoda tietoja Exceliin ja pivot-kaavion luominen
Nyt kun olet muodostanut yhteyden ja luotu tiedoston tietoja ja yhteystiedot, jota olet luet tietojen tuominen.

1. **Tietojen tuominen** -valintaikkunassa valitse haluamasi vaihtoehto laskentataulukon tietojen esittämiseen ja valitse sitten **OK**. Microsoft on valinnut **pivot-kaavio**. Voit myös luoda **uuden laskentataulukon** tai **Lisää tämä tieto tietomalliin**. Lisätietoja tietomallien on artikkelissa [Excelin tietomallin luominen](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Valitse **Ominaisuudet** tutkia tietoja edellisessä vaiheessa luomasi odc-tiedosto ja valitse tiedon päivittämistä koskevia asetuksia.

    ![Excelin tietojen muodon valitsemiseen](./media/sql-database-connect-excel/import-data.png)

    Laskentataulukko on nyt tyhjä pivot-taulukon ja kaavion.

8. Valitse **Pivot-taulukon kentissä**kaikki-valintaruudut kentät, jota haluat tarkastella.

    ![Määritä tietokannan raportti.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Jos haluat muodostaa yhteyden tietokantaan muiden Excel-työkirjojen ja laskentataulukoiden, valitse **tiedot**, valitse **yhteydet**, valitsemalla **Lisää**, luettelosta luotu yhteys ja valitse sitten **Avaa**.
> ![Avaa yhteyden toiseen työkirjaan](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten [yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa](sql-database-connect-query-ssms.md) Lisäasetukset kyselyt ja analysointia varten.
- Tässä artikkelissa kerrotaan [joustavasti jakavat](sql-database-elastic-pool.md)etuja.
- Lue [, joka muodostaa yhteyden SQL-tietokantaan taustatietokantaan verkkosovelluksen](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)luomiseen.
