<properties
    pageTitle="Tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokanta | Microsoft Azure"
    description="Tässä opetusohjelmassa näytetään kopioi toimintojen käyttämisestä Azure Data Factory-myyntijakso tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokantaan."
    Keywords="Blob-objektien sql-Blob-objektien tallennustilaan tietojen kopioiminen"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Tietojen kopioiminen SQL-tietokannan käyttäminen tietojen Factory Blob-objektien tallennustilaan 
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Resurssienhallinta-malli](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tässä opetusohjelmassa luominen tietojen factory kanssa putkijohto tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokantaan.

Kopioi tehtävän suorittaa tietojen siirto Azure Data Factory. Se on kytketty yleisesti saatavilla-palvelu, voit kopioida tiedot eri tavalla turvallinen, luotettava ja skaalattava tietojen stores välillä. Katso [Tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa lisätietoja kopio-tehtävän.  

> [AZURE.NOTE] Yksityiskohtainen yleiskatsaus Data Factory-palvelun [esittely Azure Data Factory](data-factory-introduction.md) on artikkelissa.

##<a name="prerequisites-for-the-tutorial"></a>Opetusohjelman edellytyksistä
Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure tilauksen**.  Jos sinulla ei ole tilaus, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja artikkelista [Maksuton](http://azure.microsoft.com/pricing/free-trial/) .
- **Azure-tallennustilan tilin**. **Tietolähteen** tietojen säilö blob-säiliö käytetään tässä opetusohjelmassa. Jos sinulla ei ole Azure-tallennustilan tilin, on artikkelissa [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) , kun haluat luoda.
- **Azure SQL-tietokanta**. Voit käyttää **kohde** tietosäilö Azure SQL-tietokantaan Tässä opetusohjelmassa. Jos sinulla ei ole Azure SQL-tietokantaan, voit käyttää opetusohjelman, katso [luominen ja määrittäminen Azure SQL-tietokannan](../sql-database/sql-database-get-started.md) luominen.
- **SQL Server 2012/2014 tai Visual Studio 2013**. Voit käyttää SQL Server Management Studiossa tai Visual Studio, voit luoda mallitietokannan ja näyttää tietokannan tiedot.  

## <a name="collect-blob-storage-account-name-and-key"></a>Palautteen kerääminen-blob-tallennustilan tilin nimi ja avaimen avulla 
Tarvitset tilin nimi ja Azure-tallennustilan tilin opetusohjelman tili-näppäintä. Huomautus **tilin nimi** ja Azure-tallennustilan tilin **tili-näppäintä** .

1. Kirjaudu sisään [Azure portal](https://portal.azure.com/).
2. Valitse vasemmalla olevasta valikosta **Lisää palveluja** ja valitse **Tallennustilan**.

    ![Selaa - tallennustilan tilit](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Valitse **Azure-tallennustilan tilin** , jota haluat käyttää tässä opetusohjelmassa **Tallennustilan tilit** -sivu.
4. Valitse **asetukset**-kohdassa linkki **pikanäppäimet** .
5.  **Kopioi** (kuva)-painiketta, vieressä **tallennustilan tilin nimi** -tekstiruutuun ja Tallenna ja liitä se toiseen sijaintiin (esimerkiksi: tekstitiedostona).
6. Toista edellinen vaihe kopio- tai alaspäin **Avain1**muistiinpanoja.
    
    ![Tallennustilan pikanäppäin](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Sulje kaikki näiden valitsemalla **X**.

## <a name="collect-sql-server-database-user-names"></a>Kerää SQL server-tietokantaan ja käyttäjänimet
Tarvitset Azure SQL-palvelin, tietokanta ja käyttäjän opetusohjelman nimet. Huomautus nimet **palvelimen**, **tietokannan**ja **käyttäjän** Azure SQL-tietokantaan alaspäin.

1. Valitse **Azure portal**vasemman reunan **Lisää palveluja** ja valitse **SQL-tietokannat**.
2. Valitse **tietokanta** , jota haluat käyttää tässä opetusohjelmassa **SQL-tietokantoja sivu**. Huomautus **tietokannan nimi**muistiin.  
3. Valitse **SQL-tietokanta** -sivu valitsemalla **asetukset**-kohdassa **Ominaisuudet** .
4. Huomautus alaspäin arvot **Palvelimen nimi** ja **Palvelimen JÄRJESTELMÄNVALVOJAN kirjautuminen**.
5. Sulje kaikki näiden valitsemalla **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Salli käyttämään SQL server Azure palvelut 
Varmista **, että **Salli käyttöoikeus Azure** -asetus käytössä Azure SQL server niin, että Data Factory-palvelua voi käyttää Azure SQL server** . Voit vahvistaa ja ota tämä asetus käyttöön, toimi seuraavasti:

1. **Lisää palveluja** keskittimeen vasemmalla puolella ja valitse sitten **SQL-palvelimet**.
2. Valitse palvelin ja valitse **palomuurin** **asetukset**-kohdassa. 
4. Valitse **Salli käyttöoikeus Azure** **käytössä** **palomuuri** -sivu.
5. Sulje kaikki näiden valitsemalla **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Valmistele-Blob-säiliö ja SQL-tietokantaan 
Nyt valmistella Azure-blob-säiliö ja Azure SQL-tietokantaan opetusohjelman varten seuraavasti:  

1. Muistion käynnistäminen, liitä seuraava teksti ja tallenna se kiintolevyn **emp.txt** **C:\ADFGetStarted** -kansioon.

        John, Doe
        Jane, Doe

2. Käyttää työkaluilla, kuten [Azure-tallennustilan Explorer](https://azurestorageexplorer.codeplex.com/) **adftutorial** säilö luomiseen ja **emp.txt** -tiedoston lataaminen säilö.

    ![Azure-tallennustilan hallinta. Tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokantaan](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. SQL-komentosarja avulla voit luoda taulukon **emp** Azure SQL-tietokantaan.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Jos sinulla on SQL Server 2012/2014 tietokoneessasi:** noudattamalla ohjeita [Vaihe 2: yhteyden muodostaminen SQL-tietokantaan hallinta Azure SQL-tietokannan SQL Server Management Studiossa](../sql-database/sql-database-manage-azure-ssms.md#Step2) yhdistää Azure SQL-palvelimeen ja SQL-komentosarjan suorittaminen. Tässä artikkelissa käyttää [perinteinen Azure portal](http://manage.windowsazure.com)-ei [uudessa Azure-portaalissa](https://portal.azure.com), voit määrittää palomuurin Azure SQL Server.

    Asiakkaan ei sallita Azure SQL-palvelinta, jos haluat määrittää palomuurin Azure SQL-palvelimen käyttää tietokoneesta (IP-osoite). Katso [tämän artikkelin](../sql-database/sql-database-configure-firewall-settings.md) ohjeita voit määrittää palomuurin Azure SQL Server.

Edellytykset täyttyvät. Voit luoda tietojen-factory jollakin seuraavista tavoista. Valitse ylä-tai suorittaa opetusohjelman linkkejä välilehteä.     

- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
