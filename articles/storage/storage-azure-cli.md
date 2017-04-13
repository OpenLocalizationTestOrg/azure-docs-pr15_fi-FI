<properties
    pageTitle="Azure CLI käyttäminen Azure tallennustilan | Microsoft Azure"
    description="Opettele käyttämään Azure komentorivivalitsimet Interface (Azure CLI) avulla voit luoda ja hallita tallennustilan ja Azure BLOB-objektit ja tiedostojen Azure-tallennustilan. Azure-CLI on Office kaikissa ympäristöissä-työkalu "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Azure CLI käyttäminen Azure-tallennustilan

## <a name="overview"></a>Yleiskatsaus

Azure-CLI on joukko Avaa lähde Office kaikissa ympäristöissä komennot Azure-ympäristön käyttämisen. Se tarjoaa paljon [Azure ‑portaaliin](https://portal.azure.com) sekä monipuolisia tietoja access-toiminnot-kohdan samalla tavalla.

Tämän oppaan tutustutaan [Azure käyttöliittymä (Azure CLI)](../xplat-cli-install.md) avulla voit suorittaa erilaisia kehittämisen ja hallinnan tehtävät, joiden Azure-tallennustilan. On suositeltavaa, lataa ja asentaa tai päivittää uusimmat Azure CLI, ennen kuin tässä oppaassa.

Tässä oppaassa oletetaan, että ymmärrät peruskäsitteet Azure-tallennustilan. Opas sisältää useita komentosarjojen osoittamaan Azure CLI kanssa Azure-tallennustilan käyttö. Muista päivittää kokoonpanosi ennen kuin suoritat kukin komentosarja script muuttujat.

> [AZURE.NOTE] Opas on Azure CLI-komento ja komentosarjan esimerkkejä perinteinen tallennustilan tilit. Katso [käyttämällä Mac, Linux-ja Windows Azure resurssien hallinnan Azure-CLI](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Azure CLI komennot Resurssienhallinta tallennustilan tileissä.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Aloita Azuren tallennustilaan ja Azure-CLI 5 minuuttia

Tässä oppaassa käytetään Ubuntu esimerkkejä, mutta OS muiden ympäristöjen pitäisi tehdä vastaavasti.

**Uusi Azure:** Hanki Microsoft Azure-tilausta ja kyseiseen tilaukseen liitetyllä Microsoft-tiliä. Lisätietoja Azure hankkiminen asetukset saat [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/), [Osta asetukset](https://azure.microsoft.com/pricing/purchase-options/)ja [Jäsenen on](https://azure.microsoft.com/pricing/member-offers/) (MSDN-, Microsoft Partner Network- ja BizSpark, ja muihin Microsoft-ohjelmiin jäsenet).

[Järjestelmänvalvojan roolien määrittäminen Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Katso lisätietoja Azure tilaukset.

**Kun olet luonut Microsoft Azure-tilauksen ja tilin:**

1. Lataa ja asenna Azure-CLI kuvatut [asentaa Azure-CLI](../xplat-cli-install.md)ohjeiden mukaisesti.
2. Azure-CLI on asennettu, kun osaat käyttää Azure CLI komentoja komentorivivalitsimet-liittymästä (Bash, pääte-komentokehote) azure-komennon avulla. Kirjoita `azure` komento ja pitäisi näkyä seuraava tulos.

    ![Azure-komennon tuloste][Image1]

3. Kirjoita rivin komentoliittymä `azure storage` ulos azure tallennustilan komentojen luettelo ja toimintoja ensimmäisen yleiskuvan Azure-CLI on. Voit kirjoittaa **-h** -parametrin komennon nimi (esimerkiksi `azure storage share create -h`) voit tarkastella tietoja komennon syntaksi.
4. Nyt uutiskirjeessä voit yksinkertaisen komentosarjan, joka näyttää Azure CLI peruskomennot käyttämään Azure-tallennustilan. Komentosarja ensin pyytää määrittämään kahden muuttujan tallennustilan tilin ja avain. Tämän jälkeen komentosarja luo uuden säilön uusi tallennustilan asiakas ja lataa aiemmin luotu kuvatiedosto (blob) kyseisen säilön. Kun komentosarja on lueteltu kaikki Blob-objektien kyseisen säilön, se lataa kuvatiedoston kohdekansioon, joka on luotu paikalliseen tietokoneeseen.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Avaa paikalliseen tietokoneeseen ensisijainen tekstieditorissa (esimerkiksi vim). Kirjoita edellä komentosarja teksti-editoriin.

6. Nyt on päivitettävä komentosarjan muuttujat määritysasetukset perusteella.

    - **< storage_account_name >** Määritetyn nimen käyttäminen komentosarja tai kirjoita uusi tallennustilan tilin nimi. **Tärkeä:** Tallennustilan tilin nimi on oltava yksilöllinen Azure-tietokannassa. Se on kirjoitettava pienellä, liian!

    - **< storage_account_key >** Tallennustilan tilin pikanäppäin.

    - **< container_name >** Määritetyn nimen käyttäminen komentosarja tai oman säilö uusi nimi.

    - **< image_to_upload >** Kirjoita polku kuvan paikallisessa tietokoneessa seuraavasti: "~ / images/HelloWorld.png".

    - **< destination_folder >** Kirjoita polku paikallisen hakemiston tallentamiseen Azure-tallennustilan, kuten ladatut tiedostot: "~/downloadImages".

7. Kun olet päivittänyt vim tarvittavat muuttujat, paina näppäinyhdistelmät "Esc,:, wq!" Jos haluat tallentaa komentosarja.

8. Jos haluat suorittaa tämän komentosarjan, kirjoittaa komentosarjan bash konsolissa. Kun tämä komentosarja suoritetaan, taulukossa pitäisi olla paikallisen kohdekansiota, joka sisältää ladatut kuvatiedoston. Seuraavassa näyttökuvassa näkyy esimerkki tulos:

Kun komentosarja on suoritettu, on oltava paikallisen kohdekansiota, joka sisältää ladatut kuvatiedoston.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Azure-CLI ja tallennustilaa tilien hallinta

### <a name="connect-to-your-azure-subscription"></a>Yhteyden muodostaminen Azure tilauksen

Vaikka useimmat tallennustilan komennot toimivat ilman Azure-tilausta, on suositeltavaa voit muodostaa yhteyden tilauksen Azure-CLI. Voit määrittää toimimaan tilauksen Azure-CLI ohjeiden [Azure tilauksen Azure-CLI Yhdistä](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Luo uusi tallennustilan-tili

Azure tallennuskiintiön käyttämään on tallennustilan tilin. Voit luoda uuden Azure-tallennustilan tilin, kun olet määrittänyt tietokone muodostaa tilaukseen.

        azure storage account create <account_name>

Tallennustilan tilin nimi on 3 ja 24 merkkiä välillä ja käytä numerot ja pienet kirjaimet.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ympäristömuuttujat Azure tallennustilan oletustilin määrittäminen

Voit määrittää useita tallennustilan tilejä tilauksen. Voit valita yhden niistä ja määrittää sen ympäristömuuttujat tallennustilan komentojen samassa istunnossa. Näin voit suorittaa Azure CLI tallennustilan komennot määrittämättä tallennustilan tilin ja avainta erikseen.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Toinen tapa tallennustilan oletustilin määrittäminen käyttää yhteysmerkkijonon. Hae ensin yhteysmerkkijono-komennolla:

        azure storage account connectionstring show <account_name>

Valitse Kopioi tulosteen-yhteysmerkkijono ja aseta sen arvoksi ympäristömuuttuja:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Voit luoda ja hallita BLOB-objektit

Azure-Blob-säiliö on paljon erimuotoisia tietoja, kuten binaaritietoja tai tiedot-, jota voi käyttää mitä tahansa HTTP tai HTTPS maailmanlaajuisesti tallentamista varten. Tässä osassa oletetaan, että olet jo aiemmin Azure Blob storage käsitteitä. Lisätietoja on artikkelissa [pääset alkuun käyttämällä .NET Azure-Blob-säiliö](storage-dotnet-how-to-use-blobs.md) ja [Blob-objektien palvelun käsitteitä](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Luoda säilön

Jokaisen Blob-objektien tallennustilaan Azure on oltava säilö. Voit luoda yksityinen säilön avulla `azure storage container create` komento:

        azure storage container create mycontainer

> [AZURE.NOTE] Anonyymi lukuoikeudet kolme tasoa: **käytöstä**, **Blob-objektien**ja **säilö**. Anonyymin käytön estämiseksi BLOB-objektit, määrittää käyttöoikeuksia parametrin arvoksi **Off**. Oletusarvon mukaan uusi säilö on yksityinen, ja niitä voi käyttää vain tilin omistaja. Anna anonyymi julkinen lukuoikeudet blob resursseja, mutta ei oikeutta säilö metatietojen tai BLOB-säilö luetteloon, **Blob-objektien**käyttöoikeudet-parametrin arvoksi. Anna koko julkinen lukuoikeudet blob-resursseja, säilön metatietojen ja BLOB-säilö luettelo, **säilön**käyttöoikeus-parametrin arvoksi. Lisätietoja on artikkelissa [Hallitse anonyymi lukuoikeudet säilöjen ja BLOB-objektit](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

Azure-Blob-säiliö tukee estä BLOB-objektit ja sivun BLOB-objektit. Lisätietoja on artikkelissa [tietoja estä BLOB-objektit, Liitä BLOB ja sivun BLOB-objektit](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Voit ladata BLOB-säilö, voit käyttää `azure storage blob upload`. Oletusarvoisesti tämä komento lataa paikallisina tiedostoina estä Blob-objektien. Voit määrittää blob varten, voit käyttää `--blobtype` parametria.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Lataa BLOB-säilö

Seuraavassa esimerkissä kerrotaan, miten voit ladata BLOB-säilö.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopioi BLOB-objektit

Voit kopioida BLOB sisällä tai tallennustilan tilit ja alueiden välillä asynkronisesti.

Seuraavassa esimerkissä kerrotaan, miten voit kopioida BLOB storage-tililtä toiselle. Tässä esimerkissä luodaan missä BLOB ovat julkisesti, säilön nimettömänä käytettävissä.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

Tässä esimerkissä suorittaa asynkroninen kopio. Voit valvoa kunkin kopiointi tilan suorittamalla `azure storage blob copy show` toiminto.

Huomaa, että kopiointia varten on annettu lähteen URL-osoite on joko on kaikkien käytettävissä, tai lisätä Suojaussidosten (jaettu käyttö allekirjoitus)-tunnuksen.

### <a name="delete-a-blob"></a>Poista blob

Voit poistaa blob-komennon alla:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Voit luoda ja hallita jaettujen tiedostojen

Azure tiedostojen tallentamisesta on jaettu tallennustilan sovellusten vakio SMB-protokollan avulla. Microsoft Azuren näennäiskoneiden ja pilvipalveluihin sekä paikallisen sovellukset, voit jakaa tiedostotietoja liitetyn osakkeet kautta. Voit hallita jaettujen tiedostojen ja tiedostotietoja Azure-CLI kautta. Lisätietoja Azure tiedostojen tallentamisesta on artikkelissa [Windows Azure-tiedostosäilön aloittaminen](storage-dotnet-how-to-use-files.md) tai [kuinka pystyt käyttämään Azure tiedostosäilön Linux kanssa](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Jaetun tiedoston luominen

Azure-tiedostoresurssin on SMB-tiedostoresurssin Azure-tietokannassa. Kaikkien kansioiden ja tiedostojen on luotava olevaa jaettua tiedostoresurssia. Tilin voi sisältää rajattomasti osakkeet ja jaettuun voi tallentaa tiedostoja kapasiteetin rajat-tallennustilan tilin rajoittamaton määrä. Seuraavassa esimerkissä luodaan nimeltä **minun**jaettua tiedostoresurssia.

        azure storage share create myshare

### <a name="create-a-directory"></a>Hakemiston luominen

Kansio on valinnainen hierarkkisessa rakenteessa Azure tiedostoresurssin. Seuraavassa esimerkissä luodaan hakemiston **myDir** -jaettua tiedostoresurssia.

        azure storage directory create myshare myDir

Huomaa, että kansiopolku voi olla useita tasoja, *kuten* **ja b**. Varmista kuitenkin, että kaikki ylemmän tason kansiot on olemassa. Esimerkiksi polku **ja b**, on luoda hakemiston **** ensin ja sitten Luo kansio **b**.

### <a name="upload-a-local-file-to-directory"></a>Paikallisen tiedoston lataaminen-kansioon

Seuraavassa esimerkissä Lataa **~/temp/samplefile.txt** tiedoston **myDir** -kansioon. Muokkaa tiedostopolku niin, että se osoittaa kelvollinen paikallisessa tietokoneessa:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Huomaa, että Jaa tiedosto voi olla enintään 1 TT: n kokoisia.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Jaa pääkansio tai kansion tiedostojen luettelo

Voit lisätä tiedostot ja alikansiot jaa pääkansio tai hakemistossa, käyttämällä seuraavaa komentoa:

        azure storage file list myshare myDir

Huomaa, että kansionimi on valinnainen luettelo-toiminnon. Jos jätetään pois, komento näyttää jaetun kansion pääkansion sisällön.

### <a name="copy-files"></a>Tiedostojen kopioiminen

Alkavat Azure CLI 0.9.8 versio, voit kopioida tiedoston toiseen tiedostoon, blob tai Blob-objektien tiedostoon tiedoston. Alla on näytetään, miten voit suorittaa nämä kopioi toiminnot CLI komennoilla. Tiedoston kopioiminen uuteen hakemistoon:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Voit kopioida blob tiedoston kansion:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavassa on joitakin liittyvät artikkeleissa ja ohjeaiheissa on lisätietoja Azure-tallennustilan.

- [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-tallennustilan REST API-viittaus](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
