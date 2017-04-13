<properties
   pageTitle="Tietosäilö järvi käyttäminen Office kaikissa ympäristöissä komentoliittymä rivin aloittaminen | Microsoft Azure"
   description="Tietosäilö järvi-tilin luominen ja suorittaa perustoimintoja Azure Office kaikissa ympäristöissä komentorivin avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Azure Lake Tietosäilölle Azure komentoriviltä käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Opettele Azure järvi tietosäilö-tilin luominen ja suorittaa perustoimintoja, kuten luoda kansiot-tiedostojen lataaminen onedriveen ja tietojen Azure komentoliittymä rivin avulla, poista tili, jne. Lisätietoja järvi tietovaraston ohjeaiheessa [Tietojen järvi kaupan yleiskatsaus](data-lake-store-overview.md).

Azure-CLI on toteutettu Node.js. Sen avulla voidaan ympäristössä, joka tukee Node.js, kuten Windows, Mac tai Linux. Azure-CLI on Avaa lähde. Lähdekoodin hallitaan GitHub osoitteessa <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Tässä artikkelissa käsitellään Azure-CLI käyttäminen vain järvi tietosäilö. Yleistä ohjetta Azure CLI käyttämisestä, tutustu [käyttämään Azure-CLI] [azure-command-line-tools].


##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** – Katso [asentaminen ja määrittäminen Azure-CLI](../xplat-cli-install.md) asennus-ja määritystietoja. Varmista, että käynnistät tietokoneesi CLI asentamisen jälkeen.

## <a name="authentication"></a>Todennus

Tässä artikkelissa käytetään järvi tietovaraston kohtaa, johon kirjaudut sisään käyttäjänä loppukäyttäjien yksinkertaisempi todennusta-vaihtoehto. Tietoja järvi Store tili ja tiedostojärjestelmän sitten noudatettava kirjautuneena sisään käyttäjän käyttöoikeustaso käyttöoikeustaso. On kuitenkin muiden tavoista sekä tarkistamiseen tietojen järvi kaupan, jotka ovat **käyttäjän todennusta** tai **palvelu todennusta**. Ohjeita ja lisätietoja tarkistamiseen on artikkelissa [tietojen järvi kaupan Tarkista Azure Active Directoryn avulla](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Azure-tilaukseen kirjautuminen

1. [Yhdistä Azure tilaukseen Azure komentorivivalitsimet Interface (Azure CLI)-](../xplat-cli-connect.md) kuvattu ohjeiden mukaisesti ja muodostaa tilauksen käyttämällä `azure login` menetelmää.

2. Luettelo tilaukset, jotka liittyvät tilin käyttäminen `azure account list` komento.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Yllä tuloste **Azure-sub-1** on käytössä ja muu tilaus on **Azure-sub-2**. 

3. Valitse tilaus haluat työskennellä. Jos haluat työskennellä Azure-sub-2-tilaus, voit käyttää `azure account set` komento.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Luo tili Azure järvi tietosäilö

Avaa komentokehote, runko tai pääte istunnon ja suorittamalla seuraavat komennot.

2. Siirry Azure Resurssienhallinta tilassa seuraava komento:

        azure config mode arm


5. Luo uusi resurssiryhmä. Kirjoita seuraava komento ongelman parametriarvoja, jota haluat käyttää.

        azure group create <resourceGroup> <location>

    Jos sijainnin nimi sisältää välilyöntejä, laita se lainausmerkkeihin. Esimerkiksi "Itä US 2".

5. Luo järvi tietosäilö-tili.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Kansioiden luominen järvi tietosäilö

Voit luoda kansioita Azure järvi tietovaraston tilissä hallintaa ja tallentaa tiedot. Seuraavalla komennolla Luo kansio nimeltä "mynewfolder" järvi tietovaraston ylimmällä tasolla.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Esimerkki:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Tietojen lataaminen järvi tietosäilö

Voit ladata tietojen järvi tietovaraston suoraan päätasolle tai kansioon, jonka olet luonut tilin. Alla olevassa katkelmat kuvaavat lataaminen mallitietoja kansio (**mynewfolder**), jonka loit edellisessä osassa.

Jos etsit joitakin mallitietoja lataaminen, saat **Ambulanssi Data** -kansion [Azure tietojen Lake Git säilöön](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Lataa tiedosto ja tallenna se tietokoneeseen, kuten C:\sampledata paikallisessa kansiossa\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Esimerkki:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Järvi tietovaraston tiedostojen luettelo

Käytä seuraavaa komentoa järvi tietovaraston tilillä tiedostojen luetteloa.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Esimerkki:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Tämä tulos pitäisi näyttää seuraavankaltaiselta:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Nimeäminen uudelleen, lataa ja poistaa tietoja järvi tietosäilö

* **Jos haluat nimetä tiedoston uudelleen**, kirjoita seuraava komento:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Esimerkki:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Voit ladata tiedoston**, käytä seuraavaa komentoa. Varmista, että määrität jo kohteen polku on olemassa.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Esimerkki:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Voit poistaa tiedoston**, kirjoita seuraava komento:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Esimerkki:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Kirjoita pyydettäessä **Y** poistaa kohteen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Tietosäilö järvi käyttöoikeusluettelon kansion tarkasteleminen

Seuraava komento avulla voit tarkastella käyttöoikeusluettelot järvi tietosäilö-kansiota. Nykyisessä versiossa käyttöoikeusluettelot voidaan määrittää vain tietojen järvi säilön. Tällöin path-parametri on aina pääkansio (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Esimerkki:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Poista tilisi järvi tietosäilö

Seuraavalla komennolla Poista järvi tietosäilö-tili.

    azure datalake store account delete <dataLakeStoreAccountName>

Esimerkki:

    azure datalake store account delete mynewdatalakestore

Kirjoita pyydettäessä **Y** , voit poistaa tilin.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
