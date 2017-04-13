<properties
    pageTitle="Opi käyttämään Blob-säiliö (objektin tallennus) Ruby | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Opi käyttämään Ruby-Blob-objektien tallennustilaan

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Azure-Blob-säiliö on palvelu, joka tallentaa tiedot rakenteeton pilveen objektit-BLOB-objektit. Blob-objektien tallennustilaan voit tallentaa minkä tyyppisessä tekstiä tai binaarinen tietoja, kuten asiakirjan, mediatiedosto tai sovelluksen asennusohjelma. Blob-objektien tallennustilaan kutsutaan myös objektin säilön.

Tässä oppaassa kerrotaan, kuinka voit suorittaa yleisiä tilanteita, joissa käyttämällä Blob-objektien tallennustilaan. Mallit kirjoitetaan Ruby-Ohjelmointirajapinnan käyttäminen. Koskee skenaariot ovat **lataaminen, luettelon, ladata,** ja **poistamalla** BLOB-objektit.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-sovelluksen luominen

Luo Ruby-sovellus. Ohjeita on artikkelissa [Azure-AM kulkevia verkkosovelluksen Ruby](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Määritä sovelluksesi voi käyttää tallennustilan

Käyttämään Azure-tallennustilan, sinun on ladattava ja Ruby azure pakettia, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-rubygems-to-obtain-the-package"></a>Hanki paketin RubyGems avulla

1. Käytä käyttöliittymä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai **Bash** (Unix).

2. Kirjoita "gem Asenna azure" Asenna gem ja riippuvuuksien komentoikkunassa.

### <a name="import-the-package"></a>Tuo pakkaaminen

Ruby tiedoston, jos aiot käyttää tallennustilan alkuun käyttämällä tuttuja tekstieditorissa, Lisää seuraava:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Azure-tallennustilan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat **AZURE\_TALLENNUSTILAN\_tilin** ja **AZURE\_TALLENNUSTILAN\_ACCESS_KEY** , voit muodostaa yhteyden tiliisi Azure tallennustilan tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, sinun on määritettävä tilitiedot ennen **Azure::Blob::BlobService** käyttäminen seuraava koodi:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Voit hankkia nämä arvot perinteinen tai Resurssienhallinta-tallennustilan tilin Azure-portaalissa:

1. Kirjaudu sisään [Azure portal](https://portal.azure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse oikealla asetukset-sivu **Pikanäppäimet**.
4. Accessin näppäimet sivu, joka tulee näkyviin näet pikanäppäin 1 ja 2 pikanäppäin. Voit käyttää jommankumman nimenä. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta. 

Voit hankkia nämä arvot perinteinen tallennustilan tililtä perinteinen Azure-portaalissa:

1. Kirjaudu sisään [perinteinen Azure portal](https://manage.windowsazure.com).
2. Siirry tallennustilan-tili, jota haluat käyttää.
3. Valitse siirtymisruudun alareunassa **Hallinta PIKANÄPPÄIMET** .
4. Pop-valintaikkunan näet tallennustilan tilin nimi, pikanäppäin ensisijainen ja toissijainen pikanäppäin. Pikanäppäin voit käyttää joko ensisijainen tai toissijainen tunnus. 
5. Kopioi Leikepöydälle avaimen kopio-kuvaketta.

## <a name="create-a-container"></a>Luoda säilön

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

**Azure::Blob::BlobService** objektin voit käsitteleminen säiliöiden ja BLOB-objektit. Voit luoda säilön **luominen\_container()** menetelmää.

Koodin seuraavassa esimerkissä luodaan säilö tai Tulosta virheen, jos ei mitään.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Voit halutessasi julkistaminen säilö tiedostot voit määrittää säilön käyttöoikeutta.

Voit muokata vain <strong>luominen\_container()</strong> kutsu hyväksytty **: julkisen\_access\_tason** vaihtoehto:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Kelvollisia arvoja **: julkisen\_access\_tason** asetus, jos:

* **blob:** Määrittää tietojen säilö ja Blob-objektien koko julkisen lukuoikeudet. Asiakkaiden voi luetteloida BLOB säilöön kautta anonyymi pyynnön, mutta ei voi luetteloida säilöjen tallennustilan tilin.

* **säilö:** Määrittää julkisen BLOB lukuoikeudet. Tämän säilön BLOB-tietoja voi lukea kautta anonyymi pyynnön, mutta säilö tiedot eivät ole käytettävissä. Asiakkaiden ei luetteloi BLOB säilöön anonyymi pyynnön kautta.

Vaihtoehtoisesti voit muokata säilön yleisön taso käyttämällä **määrittäminen\_säilö\_acl()** menetelmä Määritä julkinen käyttöoikeustaso.

Seuraava koodiesimerkki muuttaa julkisen käyttöoikeustaso **säilöön**:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Voit ladata sisältöä blob- **luominen\_estä\_blob()** menetelmä Blob-objektien luomiseen käyttää tiedostoa tai merkkijonoa blob sisällöstä.

Seuraava koodi Lataa tiedosto **test.png** uusi Blob-objektien nimeltä "kuva-blob-säilö-muodossa.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö

Luettelon säilöt **list_containers()** tavalla.
Luettelon BLOB säilöön, käytä **luettelon\_blobs()** menetelmää.

Tämä siirtää kaikki tilin säilöt BLOB URL-osoitteet.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Lataa BLOB-objektit

Voit ladata BLOB **Hae\_blob()** sisältö-menetelmää.

Koodin seuraavassa esimerkissä kuvataan **Hae\_blob()** "kuva-blob-sisällön lataaminen ja kirjoita se paikallisen tiedoston.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Poista Blob
Lopuksi voit poistaa blob-sovelluksella **poistaminen\_blob()** menetelmää. Seuraava koodiesimerkki kerrotaan, miten voit poistaa blob.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Seuraavat vaiheet

Näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä:

- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
- Valitse GitHub [Azure SDK Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) säilöön
- [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
