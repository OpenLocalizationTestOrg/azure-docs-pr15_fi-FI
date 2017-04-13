<properties 
    pageTitle="Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan PowerShellin | Microsoft Azure" 
    description="Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan PowerShellin avulla" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan PowerShellin avulla



> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShellin](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Tässä artikkelissa kerrotaan aloittaminen suunniteltu tai suunnittelematon automaattisesti SQL-tietokantaan PowerShellin avulla. Määrittää Geo replikoinnin, on artikkelissa [Määrittäminen Geo-replikoinnin Azure SQL-tietokantaan](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Aloita suunnitellun automaattisesti

Edistää toissijainen tietokannan luomisella alemmalle tasolle aiemmin ensisijainen luomisella toissijainen uusi ensisijainen tietokanta **- Automaattisesti** -parametrin **Määrittäminen AzureRmSqlDatabaseSecondary** cmdlet-komennon avulla. Tämä toiminto on suunniteltu suunnitellun automaattisesti, kuten aikana tietojen palauttaminen työkaluja ja edellyttää, että ensisijainen tietokanta on käytettävissä.

Komento suorittaa seuraavat työnkulun:

1. Vaihda väliaikaisesti replikoinnin synkronoitu tila. Tämä aiheuttaa kaikki avoimet tapahtumat on tyhjennetty toissijaisen.

2. Vaihda tietokannoista roolit-Geo replikoinnin partnership.  

Tässä järjestyksessä takaa, että tietokannoista on synkronoitu ennen roolit Vaihda ja näin ollen ei tietojen menetystä tapahtuu. Tällä aikana molemmat tietokannat eivät ole käytettävissä (järjestykseen 0 25 sekuntia) samalla, kun roolit vaihdetaan lyhyen ajan. Koko-toiminto tekee saapuville alle minuutin Normaali tilanteissa. Lisätietoja on kohdassa [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Tämä cmdlet-komento palauttaa kun prosessi, jossa toissijainen tietokannasta siirtymällä ensisijainen on valmis.

Seuraava komento siirtyy nimeltä "mydb" palvelin "srv2" resurssin ryhmän "rg2" ensisijainen, valitse tietokanta roolit. Alkuperäinen ensisijainen, johon on liitetty "db2", siirtyy toissijaisen tietokannoista täysin synkronoinnin jälkeen.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] Vain harvoin on mahdollista, että toiminto ei voi suorittaa ja saattaa näkyä ei vastaa. Tässä tapauksessa käyttäjä voi soittaa voimassa automaattisesti-komento (suunnittelematon automaattisesti) ja hyväksy tietojen menettämisen.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Aloita suunnittelematon automaattisesti ensisijainen tietokannasta toissijaisen tietokanta


Voit käyttää **Määrittäminen AzureRmSqlDatabaseSecondary** cmdlet-komento **– Automaattisesti** ja **- AllowDataLoss** parametreilla edistää toissijainen tietokannan muuttuvat uuden ensisijaisen tietokannan suunnittelematon tavalla pakottaminen alennus aiemmin ensisijaisen luomisella toissijainen kerrallaan, kun ensisijainen tietokanta ei ole enää käytettävissä.

Tämä toiminto on suunniteltu palauttaminen, kun käytettävyys tietokannan palauttaminen on ehdottoman ja kadota tietoja ovat oikein. Pakotetun automaattisesti käynnistyy, kun määritetyn toissijainen tietokannan tulee ensisijaisen tietokannan välittömästi ja aloittaa hyväksymällä kirjoittaminen tapahtumat. Alkuperäinen ensisijainen tietokanta ei voi muodostaa uusi ensisijainen tietokanta pakotettu automaattisesti toiminnon jälkeen, kun lisäävän varmuuskopioinnin otetaan alkuperäisen ensisijainen tietokannan ja vanha ensisijaisen tietokannan tehdään toissijainen tietokantaan uuden ensisijaisen tietokannan; Tämän jälkeen se on pelkästään uuden ensisijaisen.

Mutta koska piste-aika palauttaminen ei tue toissijaisen tietokantoja halutessasi tiedot sitoutunut vanha ensisijainen tietokanta, joka ei ole replikoida ensisijainen uuteen tietokantaan, olisi valtuutettua CSS-Tyylisivun tietokannan palauttaminen tunnetut log varmuuskopion.

> [AZURE.NOTE] Jos komento on annettu, kun ensisijainen ja toissijainen on verkossa vanha ensisijainen tulee heti ilman tietojen synkronoiminen uuden toissijaisen. Vahvistetaan tapahtumat, kun komento on annettu kadota tietoja voi ilmetä, jos ensisijainen.


Jos ensisijainen tietokannalla on useita secondaries komento onnistuu osittain. Toissijainen, jossa komento suoritettiin muuttuu ensisijainen. Vanha ensisijainen kuitenkin säilyvät ensisijainen, eli kaksi primaareista päätyvät epäyhtenäiseen tilaan ja yhdistää keskeytetty replikoinnin linkki. Käyttäjä saa korjaamaan "Poista toissijainen"-Ohjelmointirajapinnan käyttäminen joko ensisijainen tietokannat määritysten manuaalisesti.


Seuraava komento siirtyy roolit tietokannan nimeltä "mydb" ensisijainen, kun ensisijainen ei ole käytettävissä. Alkuperäinen ensisijainen, johon on liitetty "mydb", siirtyy toissijaisen jälkeen online-tilaan. Tässä vaiheessa synkronointi voi aiheuttaa tietojen menettämisen.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Seuraavat vaiheet   

- Automaattisesti, kun varmistat todennusvaatimukset palvelin ja tietokanta on määritetty uusi ensisijaisessa. Lisätietoja on artikkelissa [SQL-tietokannan suojaus palauttaminen jälkeen](sql-database-geo-replication-security-config.md).
- Lisätietoja palauttaminen käyttämällä Active Geo-replikoinnin mukaan lukien pre huono jälkeen ja lähettää palautus vaiheet ja suorittamiseen tietojen palauttaminen-sääntö on ohjeaiheessa [Tietojen palauttaminen työkaluja](sql-database-disaster-recovery.md)
- Sasha Nosov blogimerkinnän tietoja Active Geo-replikoinnin Katso [valokeila Geo replikoinnin uudet ominaisuudet](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Lisätietoja cloud-sovellusten käyttäminen aktiivinen Geo-replikoinnin suunnitteleminen on [suunnittelemisesta cloud sovellusten Liiketoiminnan jatkuvuus käyttämällä Geo toistoa varten](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Saat lisätietoja aktiivinen Geo-replikoinnin käyttämisestä joustavasti tietokannan jakavat [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Saat yleiskatsauksen business continurity [Liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
