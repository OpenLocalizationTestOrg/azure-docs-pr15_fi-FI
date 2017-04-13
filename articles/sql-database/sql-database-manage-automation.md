<properties
    pageTitle="Hallitse Azure SQL-tietokantoja Azure automaatio | Microsoft Azure"
    description="Lue lisätietoja siitä, miten Azure automaatio-palvelun avulla voidaan hallita Azure SQL-tietokantoja tasolla."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL-tietokantoja Azure automaatio hallinta

Opas esittelee voit Azure automaatio-palveluun ja kuinka sen avulla voidaan yksinkertaistaa Azure SQL-tietokantoja hallintaa.


## <a name="what-is-azure-automation"></a>Azure automaatio-ominaisuudet

[Azure automaatio](https://azure.microsoft.com/services/automation/) on Azure-palvelu, yksinkertaistaminen automatisointi cloud hallinta. Käytä Azure automaatio, pitkään suoritettavien, Manuaalinen, virhe voi enää ja usein toistuvia tehtäviä voidaan automatisoida niin, että luotettavasti, tehokkuuden ja aika-arvo omalle organisaatiollesi.

Azure automaatio on erittäin luotettavaa ja erittäin käytettävissä työnkulun suorittamisen-ohjelma, joka skaalaa vastaamaan omia tarpeita, kun organisaatiosi kasvaa. Azure automaatio-prosesseja voi olla käynnistynyt manuaalisesti 3rd osapuolen järjestelmien tai tietyin väliajoin niin, että tehtävien tapahtua täsmälleen tarvittaessa.

Pienennä toiminnallisia katseltavan ja vapauta se / DevOps henkilökunnan työmäärän, joka lisää business keskittyminen arvo siirtämällä cloud hallinta tehtävät voidaan suorittaa automaattisesti Azure automaatio.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Miten Azure automaatio avulla hallita Azure SQL-tietokantoja?

Azure SQL-tietokanta voidaan hallita Azure automaatio [Azure SQL-tietokannan PowerShellin cmdlet-komennot](https://msdn.microsoft.com/library/dn546723.aspx) , jotka ovat käytettävissä [PowerShellin Azure-työkalujen](https://msdn.microsoft.com/library/azure/jj156055.aspx)avulla. Azure automaatio on käytettävissä PowerShellin Azure SQL-tietokannan näiden cmdlet ulos-ruutuun, joten voit suorittaa kaikki SQL DB palvelun hallinta tehtävät. Voit myös pariliitos näiden cmdlet-komennoista Azure automaatio Cmdlet-komentoja Azure muista palveluista, voit automatisoida monimutkaiset toimet Azure services ja 3 osapuolen järjestelmien kanssa.

Azure automaatio on myös mahdollisuuden pitää yhteyttä SQL-palvelimet suoraan lähettämällä SQL-komentoja PowerShellin avulla.

[Azure automaatio runbookin valikoima](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) on erilaisia tuotteen ryhmän ja yhteisön runbooks Aloita automatisointi hallinnointi Azure SQL-tietokantoja, muut Azure services ja 3 osapuolen kaaviota. Valikoima runbooks ovat seuraavat:

 * [Suorittaa SQL-kyselyjä SQL Server-tietokantaan](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Skaalaa pystysuunnassa (ylös tai alas) toistu Azure SQL-tietokanta](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Katkaise SQL-taulukko, jos sen tietokannan lähestyy enimmäiskoko](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Indeksi Azure SQL-tietokannan, jos ne ovat erittäin pirstoutuneet](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut Azure automaatio ja kuinka sen avulla voidaan hallita Azure SQL-tietokantoja, noudata näitä linkkejä lisätietoja Azure automaatio.

- [Azure automaatio yleiskatsaus](../automation/automation-intro.md)
- [Oma ensimmäisen runbookin](../automation/automation-first-runbook-graphical.md)
- [Azure automaatio learning kartta](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automaatio: Oman SQL-agentti pilveen](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
