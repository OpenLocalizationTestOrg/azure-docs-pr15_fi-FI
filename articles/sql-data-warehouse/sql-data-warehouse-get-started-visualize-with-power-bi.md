<properties
   pageTitle="Power BI Microsoft Azure SQL-tietovarasto tietojen visualisointi"
   description="Power BI SQL-tietovarasto tietojen visualisointi"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Power BI tietojen visualisointi

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure koneen oppiminen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tässä opetusohjelmassa näytetään, miten Power BI avulla voit muodostaa yhteyden SQL-tietovarasto ja luoda perusvisualisointeja.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Edellytykset

Jos haluat askel Tässä opetusohjelmassa on:

- SQL-tietovarasto ladata etukäteen AdventureWorksDW tietokannassa. Valmistelu tämä [Luo SQL-tietovarasto][] ja valitse Lataa mallitiedot. Jos sinulla on jo tietovarasto, mutta ei ole mallitiedot, voit [ladata mallitiedot manuaalisesti][].


## <a name="1-connect-to-your-database"></a>1. Muodosta yhteys tietokantaan

Voit avata Power BI ja AdventureWorksDW-tietokantayhteyden muodostamisessa seuraavasti:

1. Kirjaudu sisään [Azure portal][].
2. **SQL-tietokannat** ja valitse AdventureWorks SQL-tietovarasto tietokannan.

    ![Etsi tietokanta][1]

3. Napsauta "Avaa Power BI-painiketta.

    ![Power BI-painike][2]

4. Pitäisi tulla näkyviin SQL-tietovarasto yhteys-sivun näyttäminen tietokannan verkko-osoite. Valitse Seuraava.

    ![Power BI-yhteys][3]

6. Kirjoita Azure SQL server-käyttäjänimi ja salasana ja täysin yhdistetään tietovarasto SQL-tietokantaan.

    ![Power BI-kirjautuminen][4]

7. Kun olet kirjautunut sisään Power BI-Valitse vasen sivu AdventureWorksDW-tietojoukko. Tämä avaa tietokanta.

    ![Power BI Avaa AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2. raportin luominen

Olet nyt valmiina Power BI avulla voit analysoida AdventureWorksDW mallitiedot. Analyysin suorittamiseen AdventureWorksDW on AggregateSales-näkymä. Tämä näkymä sisältää muutaman yrityksen myynti analysointiin avaimen arvot.

1. Voit luoda myyntisumman mukaan postinumero-kartan oikeassa kentät-ruudussa valitsemalla AggregateSales-näkymässä sen laajentamiseksi. Valitse postinumero ja Myyntisumma saraketta napsauttamalla niitä.

    ![Power BI Valitse AggregateSales][6]

    Power BI tunnistaa automaattisesti tämä on maantieteellisten tietojen ja sijoittaa sen kartan puolestasi.

    ![Power BI-kartan][7]

2. Tämä vaihe luo pylväskaavio, jossa näkyy myynti kohden asiakkaan tulojen määrä. Voit luoda tämän Siirry laajennettu AggregateSales-näkymä. Napsauta kenttää, SalesAmount. Vedä asiakkaan tulo-kentän vasemmalla puolella ja pudottaa akseli.

    ![Valitse Power BI-akseli][8]

    Olemme siirtää palkkikaavion vasemmalle päälle.

    ![Power BI-palkissa][9]

3. Tämä vaihe luo viivakaavio, jossa näkyy Myyntisumma Tilauspäivämäärä kohden. Voit luoda tämän Siirry laajennettu AggregateSales-näkymä. Valitse Myyntisumma- ja tilauspäivämäärä. Valitse visualisointeja-sarakkeen viivakaavio; kuvake Tämä on visualisoinnit toisella rivillä ensimmäistä kuvaketta.

    ![Power BI Valitse viivakaavio][10]

    Raportti, joka sisältää kolme erilaisia visualisointeja tiedot on nyt.

    ![Power BI-rivi][11]

Voit tallentaa edistymistä milloin tahansa valitsemalla **Tiedosto** ja sitten **Tallenna**.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun Microsoft on myöntänyt sinulle jonkin aikaa kiinnostunut mallitiedoilla, katso [kehittää][], [ladata][]tai [siirtää][]. Tai tutustu [Power BI-sivustossa][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[siirtää]: sql-data-warehouse-overview-migrate.md
[kehittäminen]: sql-data-warehouse-overview-develop.md
[lataaminen]: sql-data-warehouse-overview-load.md
[Lataa mallitiedot manuaalisesti]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[SQL-tietovarasto luominen]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI-sivustossa]: http://www.powerbi.com/
