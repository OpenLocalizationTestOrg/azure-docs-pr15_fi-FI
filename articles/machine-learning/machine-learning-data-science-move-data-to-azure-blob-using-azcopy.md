<properties
    pageTitle="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö AzCopy | Microsoft Azure"
    description="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö AzCopy"
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö AzCopy

AzCopy on suunniteltu lataamisesta, lataamisesta ja kopioiminen tiedot ja Microsoft Azure-blob-tiedoston ja taulukkotallennus komentorivin apuohjelma.

Ohjeita asentamisesta AzCopy ja lisätietoja Azure ympäristö käyttämisestä on artikkelissa [Aloittaminen AzCopy komentorivivalitsimet-apuohjelma](../storage/storage-use-azcopy.md).

Tietojen siirtäminen tai Azure-Blob-säiliö tekniikoista ohjeita on linkitetty tähän:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Jos käytössäsi on AM, joka on määritetty tarjoamia [tietoja tiede Virtual koneet Azure-](machine-learning-data-science-virtual-machines.md)komentosarjat, valitse AzCopy on jo asennettu AM.

> [AZURE.NOTE] Johdatus Azure-blob-säiliö löydät lisätietoja [Azure](../storage/storage-dotnet-how-to-use-blobs.md) -Blob-objektien perustoimintojen ja [Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx)-Blob-palveluun.


## <a name="prerequisites"></a>Edellytykset

Tämän asiakirjan oletetaan, että Azure tilauksen tallennustilan tilin ja vastaavan tallennustilan näppäimen tilin. Ennen kuin lataaminen ja latauksen, sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin avainta.

- Voit määrittää Azure tilauksen-kohdassa [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).

- Lisätietoja tallennustilan tilin luominen ja tilin ja avaintiedoista on artikkelissa [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Suorita AzCopy-komennot

Voit suorittaa AzCopy komentoja, Avaa komentorivi-ikkuna ja siirry AzCopy asennuksen kansio tietokoneessa, jossa suoritettavan AzCopy.exe sijaitsee. 

AzCopy komennot basic syntaksi on seuraava:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Voit lisätä AzCopy asennussijainnista järjestelmäpolussa ja minkä tahansa hakemistosta suorittamalla komentoja. Oletusarvon mukaan AzCopy asennetaan *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* tai *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.

## <a name="upload-files-to-an-azure-blob"></a>Tiedostojen lataaminen Azure-blob

Jos haluat ladata tiedoston, käytä seuraavaa komentoa:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Lataa tiedostot Azure-blob

Voit ladata tiedoston Azure-blob-seuraavalla komennolla:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Siirtää BLOB Azure säilöjen välillä

Siirtää BLOB Azure säilöjen välillä, käytä seuraavaa komentoa:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>AzCopy liittyviä vinkkejä

> [AZURE.TIP]   
> 1. Kun **lataaminen** tiedostot */S* Lataa tiedostot rekursiivisesti. Tämä parametri ilman tiedostojen alihakemistoista ei ladata.  
> 2. Kun **lataaminen** tiedoston */S* hakee säilö rekursiivisesti ennen kuin kaikki tiedostot kohteessa määritetyn hakemistosta ja sen alihakemistoista tai kaikki tiedostot, jotka vastaavat määritettyä mallia annetun kansio ja alikansiot, ladataan.  
> 3.  Et voi määrittää **tietyn blob-tiedosto** ja lataa */Source* -parametrin avulla. Voit ladata tiettyyn tiedostoon, Määritä blob tiedostonimi lataamaan */Pattern* -parametrin avulla. **/S** parametrin avulla voidaan etsiä tiedoston nimen kuvio-rekursiivisesti AzCopy on. Ilman kuvio-parametrin AzCopy Lataa kaikki tiedostot-kansioon.
