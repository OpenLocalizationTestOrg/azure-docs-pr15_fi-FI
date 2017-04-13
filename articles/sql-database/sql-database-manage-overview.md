<properties
    pageTitle="Yleistä: hallintatyökalut SQL-tietokannan | Microsoft Azure"
    description="Vertaa asetukset hallintaan Azure SQL-tietokanta"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Yleistä: hallintatyökalut SQL-tietokantaan

Tässä artikkelissa käsitellään ja vertaa hallintaan Azure SQL-tietokantoja, joista voit valita oikean työkalun työ, yrityksesi ja olet asetukset. Valitsemalla oikea työkalu, riippuu siitä, kuinka monta tietokantojen hallitset, tehtävä ja kuinka usein tehtävä on suoritettu.

## <a name="azure-portal"></a>Azure portal

[Azure portaalissa](https://portal.azure.com) on verkkopohjainen sovellus, jossa voit luoda, päivittää, ja poista tietokannat ja loogista palvelinta ja tietokannan valvoa. Tämä työkalu on erinomainen, jos olet vasta kanssa Azure-muutamia tietokantojen hallintaa tai sinun tehtävä joitakin nopeasti.

Saat lisätietoja portaalissa [SQL-tietokantojen hallinta käyttämällä Azure portaalin](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studiossa ja SQL Server Data Tools Visual Studiossa

SQL Server Management Studiossa (SSMS) ja SQL Server Data Tools (SSDT) ovat asiakastyökalut, jotka suoritetaan tietokoneessa, hallinta ja kehittämiseen tietokantasi pilveen. Jos olet tottunut Visual Studio tai integroitua mobiililaite (IDEs), [Kokeile SSDT Visual Studiossa](https://msdn.microsoft.com/library/mt204009.aspx)sovelluskehittäjän. Monet tietokannan järjestelmänvalvojat ovat tuttuja SSMS, jotka voidaan käyttää Azure SQL-tietokannat. [Lataa uusin SSMS](https://msdn.microsoft.com/library/mt238290) ja käytä uusin versio aina, kun käsittelet Azure SQL-tietokantaan. Lisätietoja Azure SQL-tietokantoja, jossa SSMS hallinnasta on ohjeaiheessa [SQL-tietokantojen hallinta SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Käytä aina uusimman [SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290) ja [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan.


## <a name="powershell"></a>PowerShellin

Voit käyttää PowerShell tietokantojen ja joustavasti tietokannan jakavat hallinta ja automatisoida Azure resurssin ominaisuuksissa. Microsoft suosittelee tämä työkalu on runsaasti tietokantojen hallinta ja automatisointi käyttöönotto- ja Resurssimuutokset tuotantoympäristössä.

Lisätietoja on artikkelissa [SQL-tietokannan hallinta PowerShellin avulla](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Joustavasti Tietokantatyökalut
Joustavasti Tietokantatyökalut avulla voit suorittaa seuraavia toimintoja 

* T-SQL-komentosarja vastaan tietokantoja, joissa [joustavasti projektin](sql-database-elastic-jobs-overview.md) suorittaminen
* Usean vuokraajan mallin tietokantojen siirtäminen yhden vuokraajan mallin [Jaa ja yhdistäminen-työkalu](sql-database-elastic-scale-overview-split-and-merge.md)
* Tietokantojen yhden vuokraajan mallia tai usean vuokraajan mallin avulla [joustavasti asteikko asiakkaan kirjaston](sql-database-elastic-database-client-library.md)hallintaa.
 

## <a name="additional-resources"></a>Lisäresursseja

- [Azure Resurssienhallinta](https://azure.microsoft.com/features/resource-manager/)
- [Azure automaatio](https://azure.microsoft.com/documentation/services/automation/)
- [Azure ajoitus](https://azure.microsoft.com/documentation/services/scheduler/)