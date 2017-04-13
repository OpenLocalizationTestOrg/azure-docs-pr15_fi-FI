<properties
    pageTitle="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Python | Microsoft Azure"
    description="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Python"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Python

Tässä ohjeaiheessa kerrotaan, miten luettelon, lataa ja lataa BLOB Python Ohjelmointirajapinnan käyttäminen. Python-Ohjelmointirajapinnan on annettu Azure SDK: ssa voit:

- Luoda säilön
- Lataa blob säilöön
- Lataa BLOB-objektit
- Luettelon BLOB-säilö
- Poista blob

Lisätietoja Python Ohjelmointirajapinnan käyttäminen Katso, [miten Python-Blob-objektien tallennustila-palvelun käyttöä varten](../storage/storage-python-how-to-use-blob-storage.md).

Tietojen siirtäminen tai Azure-Blob-säiliö tekniikoista ohjeita on linkitetty tähän:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Jos käytössäsi on AM, joka on määritetty tarjoamia [tietoja tiede Virtual koneet Azure-](machine-learning-data-science-virtual-machines.md)komentosarjat, valitse AzCopy on jo asennettu AM.

> [AZURE.NOTE] Johdatus Azure-blob-säiliö löydät lisätietoja [Azure](../storage/storage-dotnet-how-to-use-blobs.md) -Blob-objektien perustoimintojen ja [Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx)-Blob-palveluun.


## <a name="prerequisites"></a>Edellytykset

Tämän asiakirjan oletetaan, että Azure tilauksen tallennustilan tilin ja vastaavan tallennustilan näppäimen tilin. Ennen kuin lataaminen ja latauksen, sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin avainta.

- Voit määrittää Azure tilauksen-kohdassa [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).

- Lisätietoja tallennustilan tilin luominen ja tilin ja avaintiedoista on artikkelissa [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Tietojen lataaminen Blob

Lisää seuraavat koodikatkelman lähellä sivun, jossa Python koodin, jota haluat käyttää ohjelmallisesti Azuren tallennustilaan:

    from azure.storage.blob import BlobService

**BlobService** objektin voit käsitteleminen säiliöiden ja BLOB-objektit. Seuraava koodi luo BlobService objektin tallennustilan tilin nimi ja avaimen avulla. Korvaa tilin nimi ja tilin avaimen reaali tili ja avaimen avulla.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Tietojen lataaminen blob tekemällä seuraavat toimet:

1. Sijoita\_estäminen\_Blob-objektien\_-\_polku (Lataa polku tiedoston sisällön)
2. Sijoita\_block_blob\_-\_tiedosto (Lataa jo avattujen tiedostojen/virta-sisältö)
3. Sijoita\_estä\_Blob-objektien\_-\_tavua (lataukset jotakin matriisin tavua)
4. Sijoita\_estä\_Blob-objektien\_-\_teksti (Lataa määritetyn tekstiarvon käyttämällä määritettyä koodausta)

Seuraava esimerkki koodi latauksia paikallinen tiedosto säilöön:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Seuraava esimerkki koodi latauksia kaikki tiedostot (lukuun ottamatta kansioita) muodostaminen Blob-objektien tallennustilaan paikallisessa kansiossa:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Tietojen lataaminen Blob

Tietojen lataaminen blob käyttää seuraavilla tavoilla:
1. Hae\_Blob-objektien\_,\_polku
2. Hae\_Blob-objektien\_,\_tiedosto
3. Hae\_Blob-objektien\_,\_tavua
4. Hae\_Blob-objektien\_,\_teksti

Näistä tavoista, jotka suorittavat tarvittavat paloittelun, kun tietojen koko ylittää 64 Megatavua.

Seuraava esimerkki koodi Lataa blob-säilö sisällön paikallisen tiedoston:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

Seuraava esimerkki koodi Lataa kaikki Blob-objektien säilön. Tiedostossa käytetään luettelon\_BLOB-objektit, saat luettelon käytettävissä BLOB säilössä ja lataa ne paikallisessa kansiossa.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
