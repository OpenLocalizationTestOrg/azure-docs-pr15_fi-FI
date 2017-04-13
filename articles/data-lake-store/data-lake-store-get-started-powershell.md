<properties
   pageTitle="Aloittaminen järvi tietovaraston | Azure"
   description="Tietosäilö järvi-tilin luominen ja suorittaa perustoimintoja Azure PowerShellin avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Azure Lake Tietosäilölle PowerShellin Azure käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Opettele Azure PowerShellin avulla Azure järvi tietosäilö-tilin luominen ja suorittaa perustoimintoja, kuten luoda kansiot-tiedostojen lataaminen onedriveen ja tietoja, poista tili, jne. Lisätietoja järvi tietovaraston ohjeaiheessa [Tietojen järvi kaupan yleiskatsaus](data-lake-store-overview.md).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

* **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 tai suurempi**. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

## <a name="authentication"></a>Todennus

Tässä artikkelissa käytetään yksinkertaisempi todennus-menetelmän järvi tietovaraston Jos sinua kehotetaan kirjoittamalla tunnistetietosi Azure-tili. Tietoja järvi Store tili ja tiedostojärjestelmän sitten noudatettava kirjautuneena sisään käyttäjän käyttöoikeustaso käyttöoikeustaso. On kuitenkin muiden tavoista sekä tarkistamiseen tietojen järvi kaupan, jotka ovat **käyttäjän todennusta** tai **palvelu todennusta**. Ohjeita ja lisätietoja tarkistamiseen on artikkelissa [tietojen järvi kaupan Tarkista Azure Active Directoryn avulla](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Luo tili Azure järvi tietosäilö

1. Työpöydältä Avaa uusi Windows PowerShell-ikkuna ja kirjoita seuraava koodikatkelman Azure tiliisi kirjautuminen, Määritä tilaus ja rekisteröi tietojen järvi säilöpalvelu. Kirjaudu sisään pyydettäessä Varmista, että kirjaudut yhtenä tilauksen admininistrators/omistaja:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Azure järvi tietovaraston tili on liitetty Azure-resurssiryhmä. Aloita luomalla Azure-resurssiryhmä.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure resurssi-ryhmän luominen] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure resurssi-ryhmän luominen")

2. Luo tili Azure Lake Tietosäilölle. Määritetyn nimen on oltava vain pieniä kirjaimia ja numeroita.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Luo tili Azure järvi tietosäilö] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Luo tili Azure järvi tietosäilö")

3. Varmista, että tili on luotu.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Tämä tulos on oltava **Tosi**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Luo kansion rakenteet tallennuspaikan Azure järvi

Voit luoda kansioita Azure järvi tietovaraston tilissä hallintaa ja tallentaa tiedot.

1. Määritä pääkansio.

        $myrootdir = "/"

2. Luo uusi kansio nimeltä **mynewdirectory** määritetyn pääkansio-kohdassa.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Varmista, että uusi kansio on luotu.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Se näyttää seuraavalta tulos:

    ![Tarkista hakemisto] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Tarkista hakemisto")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Tietojen lataaminen tallennuspaikan Azure järvi

Voit ladata tietojen järvi tietovaraston suoraan päätasolle tai kansio, jonka olet luonut tilin. Alla olevassa katkelmat kuvaavat lataaminen mallitietoja hakemiston (**mynewdirectory**), jonka loit edellisessä osassa.

Jos etsit joitakin mallitietoja lataaminen, saat **Ambulanssi Data** -kansion [Azure tietojen Lake Git säilöön](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Lataa tiedosto ja tallenna se tietokoneeseen, kuten C:\sampledata paikallisessa kansiossa\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Nimeäminen uudelleen, lataa ja poistaa tietoja järvi tietosäilö

Jos haluat nimetä tiedoston uudelleen, käytä seuraavaa komentoa:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Jos haluat ladata tiedoston, käytä seuraavaa komentoa:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Voit poistaa tiedoston, käytä seuraavaa komentoa:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Kirjoita pyydettäessä **Y** poistaa kohteen. Jos sinulla on useita tiedostoja, jos haluat poistaa, voit kirjoittaa kaikki polut, jotka on erotettu toisistaan pilkulla.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Poista tilisi Azure järvi tietosäilö

Seuraavalla komennolla järvi tietovaraston tilin poistaminen.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Kirjoita pyydettäessä **Y** , voit poistaa tilin.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
