<properties
    pageTitle="Azure-tiedostojen tallennus-Python käyttämisestä | Microsoft Azure"
    description="Lue, miten Azure tiedostojen tallentamisesta-Python lataaminen, luettelon, lataa ja poista tiedostot."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-azure-file-storage-from-python"></a>Opi käyttämään Python tallennustilaa Azure-tiedosto

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit suorittaa yleisiä tilanteita, joissa käyttäminen tiedostojen tallentamisesta. Mallit on kirjoitettu Python ja käytä [Microsoft Azure tallennustilan SDK Python]. Tilanteita, joissa kattaa Sisällytä lataaminen, luettelon, lataamista ja tiedostojen poistaminen.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-share"></a>Luo jaettu kansio

**FileService** objektin voit osakkeet, kansiot ja tiedostot. Seuraava koodi luo **FileService** objektin. Lisää minkä tahansa Python tiedoston, jossa haluat käyttää ohjelmallisesti Azuren tallennustilaan yläosassa Seuraava.

    from azure.storage.file import FileService

Seuraava koodi luo **FileService** objektin tallennustilan tilin nimi ja avaimen avulla.  Korvaa 'myaccount' ja 'OmaAvain' tilin nimi ja avaimen avulla.

    file_service = **FileService** (account_name='myaccount', account_key='mykey')

Koodin seuraavassa esimerkissä **FileService** objektin avulla voidaan luoda Jaa, jos se ei ole.

    file_service.create_share('myshare')

## <a name="upload-a-file-into-a-share"></a>Tiedoston lataaminen jaettuun tuominen

Azure tiedostoresurssin tallennustilan sisältää vähintään, johon tiedostot voivat sijaita pääkansio. Tässä osassa opit paikallisesta tallennussijainnista sivulle pääkansiossa jaetun tiedoston lataaminen.

Voit luoda ja ladata tiedostoja, käytä **luominen\_tiedoston\_-\_polku**, **luominen\_tiedoston\_-\_stream**, **luominen\_tiedoston\_-\_tavua** tai **luominen\_tiedoston\_-\_tekstin** menetelmiä. He ovat korkean tason menetelmiä, jotka suorittavat tarvittavat paloittelun, kun tietojen koko ylittää 64 Megatavua.

**Luo\_tiedoston\_-\_polku** latauksia polku-tiedoston sisällön ja **luominen\_tiedoston\_-\_stream** latauksia jo avattujen tiedostojen/virta-sisältöä. **Luo\_tiedoston\_-\_tavua** matriisin tavua, lataa ja **luominen\_tiedoston\_-\_tekstin** latauksia määritetyn tekstiarvon käyttämällä määritettyä koodausta (oletus on UTF-8).

Seuraavassa esimerkissä Lataa **sunset.png** -tiedoston sisällön **OmaTiedosto** -tiedostoon.

    from azure.storage.file import ContentSettings
    file_service.create_file_from_path(
        'myshare',
        None, # We want to create this blob in the root directory, so we specify None for the directory_name
        'myfile',
        'sunset.png',
        content_settings=ContentSettings(content_type='image/png'))

## <a name="how-to-create-a-directory"></a>Toimintaohje: Hakemiston luominen

Voit myös järjestää tallennustilan sijoittamalla alikansioiden sen sijaan, että ne kaikki pääkansiossa sisällä tiedostoja. Azure tiedoston tallennuspalvelu avulla voit luoda niin monta kansioita, kuten tilisi avulla. Seuraava koodi luo alikansiot hakemiston **sampledir** pääkansioon.

    file_service.create_directory('myshare', 'sampledir')

## <a name="how-to-list-files-and-directories-in-a-share"></a>Toimintaohje: tiedostojen ja kansioiden jaettuun-luettelo

Avulla tiedostoja ja kansioita jaettuun kohteessa-luettelosta **luettelon\_kansioiden\_ja\_tiedostojen** menetelmää. Tämä menetelmä palauttaa luontitoiminto. Seuraava koodi tulostaa jokaisen tiedoston ja konsoliin jaetun kansion **nimi** .

    generator = file_service.list_directories_and_files('myshare')
    for file_or_dir in generator:
        print(file_or_dir.name)

## <a name="download-files"></a>Tiedostojen lataaminen

Voit ladata tiedoston tietoja, **Hae\_tiedoston\_,\_polku**, **Hae\_tiedoston\_,\_stream**, **Hae\_tiedoston\_,\_tavua**, tai **Hae\_tiedoston\_,\_tekstin**. He ovat korkean tason menetelmiä, jotka suorittavat tarvittavat paloittelun, kun tietojen koko ylittää 64 Megatavua.

Seuraavassa esimerkissä kuvataan **Hae\_tiedoston\_,\_polku** **OmaTiedosto** -tiedoston sisällön lataaminen ja tallentaa sen **ulos sunset.png** -tiedostoon.

    file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')

## <a name="delete-a-file"></a>Tiedoston poistaminen

Lopuksi voit poistaa tiedoston, soita **delete_file**.

    file_service.delete_file('myshare', None, 'myfile')

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut tiedostosäilön perusteet, noudata näitä linkkejä lisätietoja.

- [Python Developer Center](/develop/python/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure-tallennustilan tiimin blogia]
- [Microsoft Azuren tallennustilaan Python SDK]

[Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azuren tallennustilaan Python SDK]: https://github.com/Azure/azure-storage-python
