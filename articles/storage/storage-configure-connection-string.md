<properties 
    pageTitle="Määritä yhteysmerkkijonoa Azure säilöön | Microsoft Azure"
    description="Määritä yhteysmerkkijonon tallennustilan Azure-tiliin. Yhteysmerkkijonon sisältää tiedot, joita tarvitaan todentaa access tallennustilan tilin sovelluksestasi suorituksen aikana."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Määritä Azure tallennustilan yhteyden merkkijonoja

## <a name="overview"></a>Yleiskatsaus

Yhteysmerkkijonon sisältää todennustiedot tarvitaan käyttämään tietoja Azure-tallennustilan tilin sovelluksestasi suorituksen aikana. Voit määrittää yhteysmerkkijonon avulla:

- Muodosta yhteys Azure tallennustilan emulaattori.
- Käyttää tallennustilan-tiliä Azure-tietokannassa.
- Käyttää määritettyä resursseja Azure jaettua käyttöä allekirjoituksen (SAS) kautta.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Tallentaminen yhteysmerkkijono

Sovellus on käyttää suorituksen yhteysmerkkijonon jotta todennetaan Azuren tallennustilaan tehdyt pyynnöt. Sinulla on useita eri vaihtoehtoja yhteysmerkkijono tallentamista varten:

- Sovelluksen käytössä, työpöydän tai laitteessa, voit tallentaa-yhteysmerkkijono `app.config `tiedosto tai `web.config` tiedosto. Lisää yhteysmerkkijonon **AppSettings** -osaan.
- Sovelluksen käynnissä Azure pilvipalvelussa voit tallentaa yhteysmerkkijono [Azure palvelun kokoonpanotiedosto rakenteen (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). Lisää yhteysmerkkijonon palvelun kokoonpanotiedosto **ConfigurationSettings** -osaan.
- Voit käyttää myös yhteysmerkkijono suoraan koodissa. Useimmat tilanteessa Suosittelemme kuitenkin tallentaa määritys-merkkijono määritystiedostossa.

Yhteysmerkkijono määritys-tiedoston tallentaminen Päivitä yhteysmerkkijonon tallennustilan emulaattori ja Azure-tallennustilan tilin pilveen välillä on helppoa. Tarvitset vain Muokkaa yhteysmerkkijonoa siten, että kohde-ympäristössä.

[Microsoft Azure määritysten hallinta](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) -luokan avulla voit käyttää yhteysmerkkijono suorituksen riippumatta siitä, jossa sovellus on käynnissä.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Yhteysmerkkijonon ja tallennustilaa emulaattori luominen

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Katso lisätietoja tallennustilan emulaattori [käyttäminen Azure-tallennustilan emulaattorin kehittämisen ja Testing](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Azure-tallennustilan tiliin yhteysmerkkijonon luominen

Voit luoda yhteysmerkkijonoa Azure tallennustilan tiliisi käyttämällä alla yhteysmerkkijonon muoto. Määrittää, haluatko yhdistää tallennustilan tilin kautta (suositus) HTTPS- tai HTTP, korvaa `myAccountName` tallennustilan tilin ja korvaa nimellä `myAccountKey` access tiliavain kanssa:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Esimerkiksi yhteysmerkkijono nyt muistuttaa otoksen yhteysmerkkijonon:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure-tallennustilan tukee HTTP- ja HTTPS yhteysmerkkijonon; kuitenkin käyttämällä HTTPS on erittäin suositeltavaa.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Jaettu käyttö allekirjoituksella yhteysmerkkijonon luominen

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Yhteysmerkkijonon eksplisiittisiä tallennustilan päätepisteen luominen

Voit määrittää Palvelupäätepisteet eksplisiittisesti yhteysmerkkijono oletusarvon päätepisteet sijaan. Voit luoda yhteysmerkkijonon, joka määrittää eksplisiittisiä päätepisteen määrittämällä valmis Palvelupäätepisteen jokaisen palvelun, mukaan lukien (HTTPS (suositus) tai HTTP)-protokollaa määrityksen seuraavassa muodossa:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Yksi tilanne, johon haluta määrittää eksplisiittinen päätepisteen on, jos olet yhdistänyt Blob storage päätepiste mukautettua toimialuetta. Siinä tapauksessa voit määrittää oman mukautetun päätepiste Blob-objektien tallennustilaan yhteysmerkkijono ja halutessasi määrittää oletusarvon päätepisteet olevat, jos sovellus käyttää niitä.

Seuraavassa on esimerkkejä yhteysmerkkijonoja, jotka määrittävät eksplisiittinen päätepisteen Blob-palveluun:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Päätepisteen arvo, joka näkyy yhteysmerkkijonon käytetään muodostaa pyynnön URI-Blob-palveluun, ja se määrittää URI, joka palautetaan koodisi lomakkeen.

Huomaa, että jos haluat jättää pois palvelun-päätepisteen yhteysmerkkijonon, valitse ei osaat käyttää kyseisen palvelun tietoja koodista palauttaman yhteysmerkkijonon avulla.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Yhteysmerkkijonon luominen päätepisteen jälkiliite

Voit luoda tallennuspalvelu alueilla tai esiintymät kanssa eri päätepisteen liitteet, yhteysmerkkijono kuten Azure Kiinan tai Azure hallinnon käyttämällä seuraavia yhteysmerkkijonon muoto. Määrittää, haluatko muodostaa tallennustilan tilin kautta HTTP tai HTTPS ja korvaa `myAccountName` tallennustilan tilin nimi, korvaa `myAccountKey` tilin pikanäppäin ja korvaa `mySuffix` URI-jälkiliitteellä:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Esimerkiksi yhteysmerkkijono pitäisi nyt muistuttaa seuraavaa yhteysmerkkijonoa:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Yhteysmerkkijonon jäsentäminen

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

- [Käytä Azure tallennustilan emulaattori kehittäminen ja testaus](storage-use-emulator.md)
- [Azure-tallennustilan hallinnasta](storage-explorers.md)
- [Jaettu käyttö allekirjoitusten (SAS) käyttäminen](storage-dotnet-shared-access-signature-part-1.md)
