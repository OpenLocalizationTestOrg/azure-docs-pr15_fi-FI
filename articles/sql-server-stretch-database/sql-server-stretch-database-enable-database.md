<properties
    pageTitle="Venytä tietokannan käyttöön tietokannan | Microsoft Azure"
    description="Opettele määrittämään tietokannan Venytys tietokantaan."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Ota käyttöön Venytys tietokannan tietokantaan

Aiemmin luodun tietokannan määrittämiseksi Venytys tietokannan, valitse **tehtävät | Venytä | Ota käyttöön** SQL Server Management Studiossa **Venytys käyttöön tietokannan** Ohjattu tietokannan. Voit myös käyttää Transact\-SQL käyttöön Venytys tietokannan tietokantaan.

Jos valitset **tehtävien | Venytä | Ota käyttöön** yksittäisen taulukon ja ei ole vielä ottanut tietokannan Venytys tietokantaan, ohjattu toiminto määrittää tietokannan Venytys tietokannan ja voit valita taulukot yhteydessä. Noudattamalla tämän artikkelin sijaan [Käyttöön Venytys tietokannan taulukon](sql-server-stretch-database-enable-database.md)ohjeita.

Venytä tietokannan ottaminen käyttöön tietokantaan tai johonkin taulukon edellyttää db\_omistajan käyttöoikeudet. Venytä tietokannan käyttöönotto tietokannan edellyttää myös OHJAUSOBJEKTIN TIETOKANNAN käyttöoikeudet.

 >   [AZURE.NOTE] Myöhemmin, jos poistat Venytys tietokannan, muista, että käytöstä Venytys tietokannan lisääminen taulukkoon tai tietokantaan ei poista remote objekti. Jos haluat poistaa remote taulukon tai etätietokantaan, sinun on pudota se käyttämällä Azure hallinta-portaalin. Remote-objekteille Siirry maksamaan Azure kustannukset, kunnes poistat ne manuaalisesti.

## <a name="before-you-get-started"></a>Ennen aloittamista

-   Ennen kuin määrität tietokannan Venytys, on suositeltavaa, että suoritat Venytys tietokannan Advisor tunnistavan tietokannat ja taulukot, jotka soveltuvat Venytys. Venytä tietokannan Advisor etuliitteenä estävät ongelmat. Saat lisätietoja, [Anna tietokantojen ja Venytys tietokannan taulukoiden](sql-server-stretch-database-identify-databases.md).

-   Tarkista [koskevat rajoitukset venyttää tietokannan](sql-server-stretch-database-limitations.md).

-   Venytä tietokannan siirtää tietoja Azure. Tämän vuoksi sinulla on Azure-tili ja laskutus tilauksen. Saat Azure tiliä, [Napsauta tätä](http://azure.microsoft.com/pricing/free-trial/).

-   Ole yhteyden ja kirjaudu sisään tietoja, voit luoda uuden Azure palvelimen tai valitse olemassa oleva Azure-palvelin.

## <a name="EnableTSQLServer"></a>Edellytyksenä: Käyttöön Venytys tietokantaa palvelimessa
Ennen kuin voit ottaa Venytys tietokannan tietokantaan tai johonkin taulukon, sinun on otettava paikallisessa palvelimessa. Tämä toiminto edellyttää sysadmin tai serveradmin käyttöoikeuksia.

-   Jos sinulla on tarvittavat käyttöoikeudet, **Venytys käyttöön tietokannan** ohjattu toiminto määrittää palvelimen Venytys.

-   Jos sinulla ei ole tarvittavia oikeuksia, järjestelmänvalvojan on ottaminen käyttöön manuaalisesti suorittamalla **sp\_määrittäminen** ennen kuin käynnistät ohjatun toiminnon tai järjestelmänvalvoja on ohjattu.

Voit ottaa Venytys tietokantaa palvelimessa manuaalisesti, suorittamalla **sp\_määrittäminen** ja ota käyttöön **remote data-arkisto** -vaihtoehto. Seuraavassa esimerkissä mahdollistaa **remote data-arkisto** -vaihtoehto määrittämällä sen arvoksi 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Saat lisätietoja, [Määritä remote data arkistointi palvelimen määritysvaihtoehto](https://msdn.microsoft.com/library/mt143175.aspx) ja [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Venytä tietokannan käyttöön tietokannan ohjatun toiminnon avulla
Lisätietoja Venytys ohjatun toiminnon käyttöön tietokanta on mukaan lukien tiedot, jotka on annettava ja valinnat, jotka sinun tarvitse tehdä on artikkelissa [Aloita suorittamalla Venytys Ohjattu tietokannan käyttöön](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Käytä Transact\-SQL Venytys tietokannan ottaminen käyttöön tietokantaa
Ennen kuin voit ottaa käyttöön yksittäisiä taulukkoja Venytys tietokannan, sinun on otettava tietokanta.

Venytä tietokannan ottaminen käyttöön tietokantaan tai johonkin taulukon edellyttää db\_omistajan käyttöoikeudet. Venytä tietokannan käyttöönotto tietokannan edellyttää myös OHJAUSOBJEKTIN TIETOKANNAN käyttöoikeudet.

1.  Ennen kuin aloitat, valitse olemassa olevan Azure palvelimen, joka siirtää Venytys tietokannan tietojen tai Luo uusi Azure palvelin.

2.  Azure-palvelimeen ja IP-osoitealueita, jonka avulla SQL Serverin yhteyden etäpalvelimeen SQL Serverin palomuurisäännön luominen.

    Löydät helposti arvot on ja palomuurisäännön luominen yrittämällä muodostaa yhteyden Azure palvelimeen objektin Explorerista SQL Server Management Studiossa (SSMS). SSMS avulla voit luoda säännön avaamalla seuraavat valintaikkuna, jossa on jo IP-osoite pakolliset arvot.

    ![SSMS palomuurisäännön luominen][FirewallRule]

3.  SQL Server-tietokantaan määrittämiseksi Venytys tietokannan, tietokannan saa olla avaimeksi perustyyli. Tietokannan perustyyli avain suojaa tunnistetiedot, jotka Venytys tietokanta käyttää Muodosta yhteys etätietokantaan. Tässä on esimerkki, joka luo uuden tietokannan perustyyli tunnuksen.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Saat lisätietoja tietokannan perustyyli avain [luominen MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) ja [Luo avaimeksi perustyyli](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Kun määrität tietokannan Venytys tietokantaan, on annettava Venytys tietokannan käytössä paikallisen SQL Server- ja Azure etäpalvelimeen väliseen viestintään tunnistetiedot. Sinulla on kaksi vaihtoehtoa.

    -   Voit antaa järjestelmänvalvojan tunnistetiedot.

        -   Jos otat käyttöön ohjatun Venytys tietokannan, voit luoda tunnistetieto aikalohkon.

        -   Jos aiot käyttöön Venytys tietokannan suorittamalla **Muuta TIETOKANTAA**, sinun on luotava manuaalisesti ennen kuin suoritat **Muuta TIETOKANTAA** käyttöön Venytys tietokannan tunnistetieto.

        Tässä on esimerkki, joka luo uuden tunnistetiedon.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Saat lisätietoja tunnistetieto [Luominen TIETOKANNAN SUODATETUT TUNNISTETIEDON (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Tunnistetieto luominen edellyttää muuttaa minkä tahansa TUNNISTETIEDON käyttöoikeudet.

    -   Liitetyt palvelutilin SQL Serverin avulla voit viestiä Azure etäpalvelimeen kaikki seuraavat ehdot täyttyvät.

        -   Palvelutili, jossa SQL Server-esiintymän suoritetaan on toimialuetili.

        -   Toimialuetilin kuuluu toimialueeseen, jonka Active Directory on liitetty Azure Active Directory-hakemistosta.

        -   Azure etäpalvelimeen on määritetty tukemaan Azure Active Directory-todentamista.

        -   Palvelutili, jossa SQL Server-esiintymän suoritetaan on oltava määritettynä dbmanager tai sysadmin tiliksi remote Azure-palvelimeen.

5.  Muuta TIETOKANTAA-komennon suorittaminen määrittämiseksi Venytys tietokannan tietokannan.

    1.  PALVELIN-argumentin nimetä olemassa olevan Azure palvelimen, mukaan lukien `.database.windows.net` osa nimestä \- esimerkiksi `MyStretchDatabaseServer.database.windows.net`.

    2.  Antaa aiemmin järjestelmänvalvoja-tunnistetiedon TUNNISTETIEDON-argumentin tai määrittää organisaation ulkopuolisten\_palvelun\_tilin = ON. Seuraavassa esimerkissä on olemassa olevan tunnistetiedon.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Seuraavat vaiheet
-   [Ota käyttöön Venytys tietokannan taulukkoon](sql-server-stretch-database-enable-table.md) käyttöön taulukoita.

-   [Näytön Venytys tietokannan](sql-server-stretch-database-monitor.md) tietojen siirtäminen tilan tarkasteleminen.

-   [Keskeyttäminen ja jatkaminen Venytys tietokanta](sql-server-stretch-database-pause.md)

-   [Hallinta ja Venytys tietokannan vianmääritys](sql-server-stretch-database-manage.md)

-   [Varmuuskopion Venytys käyttävä tietokannat](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Katso myös

[Määritä tietokannat ja taulukot Venytys tietokantaan](sql-server-stretch-database-identify-databases.md)

[Muuta TIETOKANNAN asetusten määrittäminen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
