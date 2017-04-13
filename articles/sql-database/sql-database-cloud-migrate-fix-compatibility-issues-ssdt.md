<properties
   pageTitle="Korjaa SQL Server-tietokannan yhteensopivuusongelmat ennen siirtoa SQL-tietokantaan | Microsoft Azure"
   description="Microsoft Azure SQL-tietokanta-tietokannan siirto, yhteensopivuuden, SQL Azure siirron ohjattu, SSDT"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>SQL Server-tietokantaan siirtäminen SQL Server Data Tools for Visual Studio avulla Azure SQL-tietokanta 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Päivityksen tukipalvelu](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Tässä artikkelissa kerrotaan, voit etsiä ja korjata SQL Serverin tietokantaan yhteensopivuusongelmat käyttäen SQL Server Data Tools for Visual Studio ennen siirtoa Azure SQL-tietokantaan.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>SQL Server Data Tools for Visual Studio avulla

Visual Studio ("SSDT") SQL Server Data Tools avulla voit tuoda tietokantarakenteen Visual Studio tietokannan projektin analysointia varten. Haluat analysoida, Määritä projektin kohdeympäristö SQL-tietokannan Vipuventtiili V12 ja luo sitten projekti. Luo on tarkistettu, jos tietokanta on yhteensopiva. Jos Luo epäonnistuu, voit ratkaista virheet SSDT (tai jokin Työkalut, tässä aiheessa kuvatut). Kun projektin muodostaa onnistuneesti, voit julkaista sen takaisin lähdetietokannan kopio. Voit käyttää sitten tietojen compare-ominaisuus-SSDT haluat kopioida tiedot lähdetietokannan Azure SQL-Vipuventtiili V12 yhteensopiva tietokantaan. Voit siirtää sitten päivitetty tietokanta. Voit käyttää tätä asetusta, Lataa [uusin versio SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![VSSSDT siirron kaavio](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Jos tarvitset vain rakenne-siirron, rakenteen julkaista suoraan Visual Studio suoraan Azure SQL-tietokantaan. Käytä tätä tapaa, kun tietokannan rakennetta edellyttää lisämuutoksia kuin voi ratkaista siirron ohjatussa yksinään.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>SQL Server Data Tools for Visual Studio avulla yhteensopivuusongelmia tunnistaminen
   
1.  Avaa **SQL Server Object Explorerissa** Visual Studiossa. **Lisää SQL Serverin** avulla voit muodostaa yhteyden SQL Server-esiintymän jätettävien tietokannan sisältävän. Etsi tietokanta objektin Resurssienhallinnassa, tietokannan hiiren kakkospainikkeella ja valitse **Luo uusi projekti...**     
    
    ![Uusi projekti](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Tuo **sovelluksen kohdistettu objektit vain**tuontiasetusten määrittäminen. Poista seuraavat vaihtoehdot: Viitattu kirjautumiset, käyttöoikeudet ja tietokanta-asetukset.    

    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Valitse Tuo tietokannan ja Luo projekti, joka sisältää T-SQL-komentosarjatiedosto, tietokannan kunkin objektin **Käynnistä-painiketta** . Komentosarjatiedostot ovat ylemmän tason kansioissa projektissa.    

    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  Napsauta Visual Studio ratkaisunhallinnassa tietokannan projektin hiiren kakkospainikkeella ja valitse Ominaisuudet. Valitse **Projektin asetukset** -sivun Määritä kohdeympäristö, Microsoft Azure SQL-tietokannan V12.    
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Projektin hiiren kakkospainikkeella ja valitse projektin luonnissa **luominen** .    
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  **Virhe-luettelossa** näkyy kunkin eivät ole yhteensopivia. Tässä tapauksessa käyttäjänimi NT-HALLINTA\VERKKOPALVELU ei ole yhteensopiva. Koska ei ole yhteensopiva, voit kommentti sen tai poistaa sen (ja osoite vaikutukset tämän kirjautuminen ja roolin poistaminen tietokanta-ratkaisun).     
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>SQL Server Data Tools for Visual Studio avulla yhteensopivuuden ongelmien korjaaminen

1.  Kaksoisnapsauta ensimmäisen komentosarjan, joka avaa komentosarja kyselyikkunassa ja kommentin komentosarja, ja Suorita komentosarja.     
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Toista nämä vaiheet jokaiselle komentosarjan sisältävä yhteensopivuusongelmia, kunnes virhettä.    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Kun tietokanta on ilmainen virheitä, projektin hiiren kakkospainikkeella ja valitse **Julkaise**. Kopion lähdetietokannan sisäisten ja julkaista (on erittäin suositeltavaa käyttää kopion vähintään aluksi).     
 - Ennen kuin julkaiset, sen mukaan lähteen SQL Server (vanhempi versio SQL Server 2014), voit joutua vaihtamaan projektin kohdeympäristö käyttöön käyttöönotto.     
 - Jos olet siirtymässä vanhempia SQL Server-tietokantaan, sisältämiin ominaisuuksia, joita ei tueta lähde SQL Server-Projectiin asti siirtää tietokannan SQL Server uudemmalla versiolla.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  SQL Server objektin Resurssienhallinnassa lähdetietokannan hiiren kakkospainikkeella ja valitse **Tietoja vertailu**. Vertaaminen alkuperäiseen tietokantaan project auttaa sinua ymmärtämään, muutokset on tehty ohjatussa. Valitse Azure SQL-Vipuventtiili V12-versiosi tietokanta ja valitse sitten **Valmis**.    
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Tarkista havaita erot ja valitse sitten **Päivitä kohde** siirretään tietoja lähdetietokannan Azure SQL-Vipuventtiili V12-tietokantaan.     
    
    ![Vaihtoehtoinen teksti](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Valitse käyttöönottomenetelmä. Katso [yhteensopiva SQL Server-tietokannan siirtäminen SQL-tietokantaan.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Seuraavat vaiheet

- [SSDT uusimmassa versiossa](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studiossa uusimmassa versiossa](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan Vipuventtiili V12](sql-database-v12-whats-new.md)
- [Transact-SQL osittain tai funktioita, joita ei tueta](sql-database-transact-sql-information.md)
- [Siirtää SQL Server-tietokantoja SQL Server siirron hallinnan käyttäminen](http://blogs.msdn.com/b/ssma/)
