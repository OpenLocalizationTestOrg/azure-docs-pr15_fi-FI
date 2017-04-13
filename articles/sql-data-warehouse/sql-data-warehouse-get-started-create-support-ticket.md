<properties
   pageTitle="Luomisesta tuki lippu SQL-tietovarasto | Microsoft Azure"
   description="Miten tuki lippu luodaan Azure SQL-tietovarasto."
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
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>SQL-tietovarasto tuki lippu luominen
 
Jos sinun tarvitsee SQL-tietovarasto ongelmia Luo tuki lippujen niin, että suunnitteluryhmät tiimimme auttaa sinua.

## <a name="create-a-support-ticket"></a>Luo tuki-lippu

1. Avaa [Azure portal][].

2. Napsauta aloitus-näytössä **Ohjeen + tuki** -ruutua.

    ![Ohjeen + tuki](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Auta + tuki-sivu, valitse **Luo tukipyyntö**.

    ![Uusi tukipyyntö](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Valitse **pyytäminen tyypin**.

    ![Pyynnön tyyppi](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Kunkin SQL server (kuten myserver.database.windows.net) on oletusarvoisesti 45,000 **DTU kiintiön** . Tämä kiintiö on yksinkertaisesti turvallisuutta rajoitukset. Voit suurentaa kiintiön luomalla tuki lippu ja valitsemalla *kiintiön* pyynnön tyyppiä. Oman DTU laskemiseen tarvitsee, kerro 7.5 summan [DWU][] tarpeen mukaan. Esimerkiksi haluat isännöidä kaksi DW6000s SQL-palvelimessa ja sitten olisi pyytää 90,000 DTU kiintiön.  Voit tarkastella oman nykyisen DTU kulutus SQL server-sivu-portaalissa. Keskeytetty- ja poistamalla keskeytetty tietokantojen laskea DTU kiintiön kohti. 

5. Valitse **tilaus** , joka isännöi tietokantaa, jossa ongelma.

    ![Tilauksen](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Valitse **SQL-tietovarasto** resurssi.

    ![Resurssi](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Valitse oman [Azure tukevat suunnitelma][].

    - **Laskutus-kiintiön ja tilauksen** tuki on saatavilla kohdassa kaikki tukitasot.
    - **Korjauksista** tuki annetaan [Developer][], [Standard][], [Professional suoraan][] tai [Premier][] -tukeen. Sivunvaihto Ratkaise ongelmat on ongelmia käyttäessäsi Azure asiakkaiden on suositeltavaa kerralla oletuksena, että ongelma johtuu Microsoft.
    - **Kehittäjän neuvonnan** ja **neuvoa palvelut** ovat käytettävissä [Professional suora][] ja [Premier][] -tuki tasolla. 
    
    Jos sinulla on Premier tukevat palvelupaketti, voit lähettää raportin myös SQL-tietovarasto liittyvät ongelmat [Microsoft Premier online-portaalia][].  Katso lisätietoja eri tuki-palvelupakettien, mukaan lukien laajuus, vastausajat, hinnat ja niin edelleen [Azure tukevat suunnitelmien][Azure tukevat suunnitelma] .  Usein kysyttyjä kysymyksiä Azure tuesta on kohdassa [usein kysyttyjä kysymyksiä tukevat Azure][].  

    ![Tuen suunnitelma](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Valitse **ongelmatyyppi** ja **luokka**.

    ![Ongelman tyyppi luokka](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Ongelman kuvaus ja valitse business vaikutus käyttöoikeustaso.

    ![Ongelman kuvaus](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. **Yhteystiedot** tuki-lippujen olla valmiiksi täytettyjä. Päivitä tarvittaessa.

    ![Yhteystiedot](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Valitse **Luo** , jos haluat lähettää tukipyynnön.


## <a name="monitor-a-support-ticket"></a>Näytön tuki-lippu

Kun olet lähettänyt tukipyyntö, Azure tukiryhmän sinuun. Voit tarkistaa pyynnön tila- ja tiedot valitsemalla **hallinnoi tukipyyntöjä** koontinäytössä.

![Tilan tarkistaminen](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Muut resurssit

Lisäksi voit muodostaa SQL-tietovarasto yhteisön [Pinon ylivuoto][] tai [Azure SQL Data Warehouse MSDN-keskustelupalsta][]kanssa.

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure laatiminen]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Vakio]: https://azure.microsoft.com/support/plans/standard/  
[Ammattimaisten suoraan]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure tuki usein kysyttyjä kysymyksiä]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online-portaalia]: https://premier.microsoft.com/
[Pinon ylivuoto]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN-keskustelupalsta]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

