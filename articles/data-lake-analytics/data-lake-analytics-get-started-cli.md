<properties 
   pageTitle="Pääset alkuun käyttämällä Azure käyttöliittymä Azure tietojen järvi Analytics | Microsoft Azure" 
   description="Opi käyttämään Luo järvi tietosäilö-tili, tietoja järvi Analytics työn U SQL Azure käyttöliittymä ja Lähetä työ. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Opetusohjelma: käytön aloittaminen Azure tietojen järvi Analytics käyttämällä Azure käyttöliittymä (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Opettele Azure CLI avulla voit luoda Azure tietojen järvi Analytics-tilit ja määritä tietojen järvi Analytics työt [U](data-lake-analytics-u-sql-get-started.md)SQL lähettää työt tietojen järvi Analytics-tilit. Lisätietoja tietojen järvi Analytics artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).

Tässä opetusohjelmassa kehität työn, joka lukee välilehti erotetut arvot (TSV)-tiedosto ja muuntaa sen Luetteloerottimella erotetut arvot (CSV)-tiedostoon. Siirry saman opetusohjelman käyttämällä muita tuetuilla työkaluilla kautta valitsemalla ylälaidassa tämän osion välilehtiä.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Katso [asentaminen ja määrittäminen Azure CLI](../xplat-cli-install.md).
    - Lataa ja asenna **ennakkoversion** [Azure CLI Työkalut](https://github.com/MicrosoftBigData/AzureDataLake/releases) suorittaminen edellyttää tätä esittely.
- **Todennus**seuraava komento:

        azure login
    Lisätietoja todennustapa työpaikan tai oppilaitoksen tilillä on artikkelissa [etäyhteyden muodostaminen Azure tilauksen Azure-CLI](../xplat-cli-connect.md).
- **Siirry Azure Resurssienhallinta-tilassa**käyttämällä seuraava komento:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Tietoja järvi Analytics-tilin luominen

Sinulla on oltava tietojen järvi Analytics-tili, ennen kuin voit suorittaa töitä. Tietoja järvi Analytics-tilin luominen, sinun on määritettävä seuraavasti:

- **Azure resurssiryhmä**: A tietojen järvi Analytics-tili on luotava Azure resurssiryhmä kuluessa. [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) mahdollistaa sovelluksen ryhmänä resurssien käsitteleminen. Voit ottaa käyttöön, Päivitä tai poista kaikki resurssit sovelluksen yhteen, koordinoidun toiminnassa.  

    Luettelointi tilauksen resurssiryhmät:
    
        azure group list 
    
    Luo uusi resurssiryhmä seuraavasti:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Tietoja järvi Analytics tilin nimi**
- **Sijainti**: jokin Azure tietojen keskikohdan mukaan, joka tukee tietojen järvi Analytics.
- **Oletusarvoinen tietojen järvi tilin**: tietojen järvi Analytics-tileille on tietoja järvi oletustilin.

    Luettelon tietojen järvi käyttäjätilille:
    
        azure datalake store account list

    Luo uusi tietojen järvi seuraavasti:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Tietoja järvi tilin nimen on oltava vain pieniä kirjaimia ja numeroita.



**Tietoja järvi Analytics-tilin luominen**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Tietoja järvi Analytics Näytä tili](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Tietoja järvi Analytics tilin nimen on oltava vain pieniä kirjaimia ja numeroita.


## <a name="upload-data-to-data-lake-store"></a>Tietojen lataaminen järvi tietosäilö

Tässä opetusohjelmassa käsittelee joitakin haun lokitiedot.  Etsi lokin voidaan tallentaa tiedot järvi kaupan tai Azure-Blob-säiliö. 

Azure-portaalissa on käyttöliittymän kopioimiseen joitakin mallidatatiedostot tietojen järvi oletustilin, jotka sisältävät haun lokitiedostoon. Kohdassa [valmisteleminen lähdetietojen](data-lake-analytics-get-started-portal.md#prepare-source-data) tietojen lataaminen järvi tietovaraston oletustilin.

Lataa tiedostot cli, käytä seuraavaa komentoa:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Tietoja järvi Analytics käyttää Azure-Blob-säiliö.  Tietojen lataaminen Azure-Blob-säiliö, artikkelissa [Azure CLI Azuren tallennustilaan kanssa](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Lähettää tietoja järvi Analytics työt

Tietoja järvi Analytics työt kirjoitetaan U SQL-kielen. Lisätietoja U-SQL-kohdassa [U SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md) - ja [U-SQL-kieliohje](http://go.microsoft.com/fwlink/?LinkId=691348).

**Tietoja järvi Analytics työn komentosarjan luominen**

- Luo tekstitiedosto seuraavat U-SQL-komentosarjan ja Tallenna tekstitiedosto työasemaan:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Tämä U-SQL-komentosarja lukee käyttämällä **Extractors.Tsv()**lähdetiedosto tiedot ja luo csv-tiedoston **Outputters.Csv()**. 
    
    Älä muokkaa kaksi polkua, paitsi jos lähdetiedosto kopioiminen toiseen paikkaan.  Tietojen järvi Analytics Luo tulostus-kansioon, jos se ei ole.
    
    On helpompaa käyttämään suhteelliset polut oletusarvon tietojen järvi tilit tallennettuja tiedostoja. Voit käyttää myös suoria polkuja.  Esimerkki 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Sinun on käytettävä absoluuttiset polut linkitetyt tallennustilan asiakkaat tiedostoja.  Liitetyn Azure-tallennustilan tilin tallennettuja tiedostoja syntaksi on seuraava:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob-säilö julkisen BLOB tai julkinen säilöjen käyttöoikeudet eivät ole tuettuja.      

    
**Voit lähettää työ**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Työt-luettelosta projektin tietoja ja Peruuta työt avulla voidaan seuraavia komentoja:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Kun työ on valmis, voit käyttämällä seuraavat Cmdlet-komentoja tiedosto ja lataa tiedosto:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Katso myös

- Saat saman opetusohjelman muiden työkaluilla valitsemalla välilehden valitsimet-sivulla.
- Monimutkaisen kyselyn on ohjeartikkelissa [Analysoi sivuston lokit Azure tietojen järvi Analytics avulla](data-lake-analytics-analyze-weblogs.md).
- Aloita U-SQL-sovellusten kehittäminen-kohdasta [kehittää U-SQL-komentosarjat käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL-kohdassa [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).
- Katso hallintatehtäviä, [Hallita Azure tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-manage-use-portal.md).
- Saat yleiskatsauksen tietojen järvi Analytics-artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).

