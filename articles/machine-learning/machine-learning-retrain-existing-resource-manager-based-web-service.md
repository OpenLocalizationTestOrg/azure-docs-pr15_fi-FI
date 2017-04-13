<properties
    pageTitle="Aiemmin luodun ennakoivan WWW-palvelun suuntaamista | Microsoft Azure"
    description="Opettele suuntaamista mallin ja Päivitä juuri koulutetun mallin käyttäminen Azure koneen Learning web-palveluun."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="v-donglo"/>


# <a name="retrain-an-existing-predictive-web-service"></a>Suuntaamista nykyinen ennakoivan web-palvelu

Tässä asiakirjassa kerrotaan, uudelleenkoulutuksen seuraava tilanne:

* Sinulla on koulutus kokeessa ja ennakoivan koe, jotka olet ottanut operationalized web-palvelu.
* Uudet tiedot, jotka haluat ennakoivan WWW-palvelun avulla voit tehdä sen näkyvissä pistemäärä.

Aloittaminen aiemmin web-palvelu ja kokeet, tarvitset toimimalla seuraavasti:

1. Päivitä mallin.
    1. Muokkaa koulutus-kokeen WWW-palvelun syötteiden ja tulostaa.
    2. Ota käyttöön koulutusta kokeen uudelleenkoulutuksen WWW-palvelulle.
    3. Mallin suuntaamista koulutus kokeen erä suorittamisen Service (BES) avulla.
2. Päivitä ennakoivan kokeen Azure koneen Learning PowerShellin cmdlet-komennot avulla.
    1.  Kirjaudu tiliisi Azure Resurssienhallinta.
    2.  Saat WWW-palvelun määritys.
    3.  Vie JSON WWW-palvelun määritys.
    4.  Päivitä JSON ilearner-blob viittaus.
    5.  Tuo JSON web-palvelu-määritys.
    6.  Päivitä WWW-palvelun uuden web-palvelu-määrityksen.

## <a name="deploy-the-training-experiment"></a>Koulutus kokeen käyttöönotto

Ottamaan koulutusta kokeen uudelleenkoulutuksen web-palvelu on lisättävä WWW-palvelun syötteiden ja tulostaa tietomalliin. Osallistuminen *Web-palvelu tulosteen* moduuli kokeile * [Junassa mallin] [ train-model] * moduulissa otat koulutus kokeen tuottamaan uuden koulutetun mallin, jota voit käyttää ennakoivan kokeessa. Jos sinulla on *Arvioitava malli* -moduulin, voit liittää web-palvelu tulosteessa saat arvioinnin tuloksen kuin tulos.

Voit päivittää koulutus kokeen seuraavasti:

1. Yhdistäminen *Web-palvelu syötteen* moduuli tietosyöte (esimerkiksi *SIIVOA puuttuvien tietojen* moduuli). Yleensä haluat varmistaa, että syötetietoja käsitellään samalla tavalla kuin alkuperäinen koulutus-tietoja.
2. Yhdistäminen *Web-palvelu tulosteen* moduuli *Junassa malli* -moduulin tulos.
3. Jos *Arvioi malli* -moduulin ja haluat siirtää arvioinnin tulokset, muodostaa *Arvioi malli* -moduulin tulosteen *WWW-palvelun tulosteen* moduuli.

Suorita oman kokeen.

Seuraavaksi on otettava käyttöön, koulutus kokeen kuin verkkopalveluun, joka tuottaa koulutetun mallin ja mallin arvioinnin tuloksen.  

Kokeen alusta alareunassa **Määrittää WWW-palvelun**ja valitse sitten **Ota käyttöön Web-palvelu [uusi]**. Azure koneen Learning Web Services-portaalin tulee **Käyttöön verkkopalvelu** -sivulle. WWW-palvelun nimi, valitse maksusuunnitelma ja valitse sitten **Ota käyttöön**. Voit käyttää vain eräkäsittely menetelmä koulutetun malleja.


## <a name="retrain-the-model-with-new-data-by-using-bes"></a>Mallin uusilla tiedoilla suuntaamista BES avulla

Tässä esimerkissä esimerkissä on käytössä C# uudelleenkoulutuksen-sovelluksen luominen. Voit käyttää myös Python tai R otoksen koodin tämän tehtävän suorittamiseen.

Soita uudelleenkoulutuksen API:

1. C#-konsolisovelluksen luominen Visual Studio (**Uusi** > **projektin** > **Työpöydän** > **Console-sovellus**).
2.  Kirjaudu tietokoneeseen Learning Web Services-portaaliin.
3.  Valitse web-palvelu, joiden kanssa työskentelet.
2.  Valitse **tarjoaman**.
3.  Valitse **Kulutettava** -sivulla **Otoksen** koodiosassa **erä**.
5.  Esimerkki C# koodi eräkäsittely kopioi ja liitä se Program.cs-tiedostoon. Varmista, että nimitilan säilyy muuttumattomana.

NuGet paketin Microsoft.AspNet.WebApi.Client, Lisää kommenttien. Voit lisätä Microsoft.WindowsAzure.Storage.dll viittaus ensin joutua asentaa [Azuren tallennustilaan services client kirjasto](https://www.nuget.org/packages/WindowsAzure.Storage).

Seuraavassa näyttökuvassa näkyy **Kulutettava** sivun Azure koneen Learning Web Services-portaalissa.

![Kuluttavat sivu][1]

### <a name="update-the-apikey-declaration"></a>Päivitä apikey-ilmoitus

Etsi **apikey** ilmoitus:

    const string apiKey = "abc123"; // Replace this with the API key for the web service

**Kulutettava** sivun **Basic kulutus tiedot** -osassa Etsi perusavaimen ja kopioi se **apikey** -ilmoitus.

### <a name="update-the-azure-storage-information"></a>Päivitä Azuren tallennustilaan

BES sample code latauksia Azuren tallennustilaan tiedoston paikalliseen levyasemaan (esimerkiksi "C:\temp\CensusIpnput.csv"), käsittelee sen ja kirjoittaa tulokset takaisin Azure-tallennustilan.  

Azuren tallennustilaan tietojen päivittämistä tulee hakea tallennustilan tilin nimi, avain ja säilötietoja tallennustilan tilin perinteinen Azure-portaalista ja valitse Päivitä koe, tuloksena olevat työnkulun suorittamisen jälkeen correspondi pitäisi olla seuraavankaltaiselta:

![Tuloksena olevat työnkulun suorittamisen jälkeen][4]maa olevat arvot.

1. Kirjaudu Azure perinteinen-portaaliin.
1. Valitse vasemmassa siirtymisruudussa-sarakkeessa **tallennustilan**.
1. Valitse jokin tallentamiseen retrained mallin tallennustilan tilit-luettelosta.
1. Valitse **Hallitse pikanäppäimet**sivun alareunaan.
1. Kopioida ja tallentaa **Access perusavaimen** ja sulje ikkuna.
1. Valitse sivun yläreunassa **säilöjen**.
1. Valitse aiemmin luotu säilö, tai luoda uuden ja Tallenna nimi.

Etsi *StorageAccountName*, *StorageAccountKey*ja *StorageContainerName* ilmoitukset ja Päivitä arvot, jotka olet tallentanut perinteinen-portaalista.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Voit myös on varmistettava, että lähdetiedoston käytettävissä sijainnissa, jotka määrität koodi.

### <a name="specify-the-output-location"></a>Määritä tulostuskohde

Kun määrität tulostuskohde pyytää-paketti, tiedosto, joka on määritetty *RelativeLocation* tunniste on määritettävä muodossa `ilearner`. Esimerkki:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Seuraavassa on esimerkki uudelleenkoulutuksen output: ![uudelleenkoulutusta tulostus][6]

## <a name="evaluate-the-retraining-results"></a>Arvioi uudelleenkoulutuksen tulokset

Kun käynnistät sovelluksen, tulos on URL-Osoitteen ja jaetun allekirjoitukset käyttöoikeustietue, joita voi käyttää arvioinnin tulokset.

Näet retrained mallin suorituskyvyn tulosten yhdistäminen *BaseLocation*, *RelativeLocation*ja *SasBlobToken* *output2* tulos tulokset kohteesta (katso edellä uudelleenkoulutuksen tulosteen kuvassa) ja liittämällä täydellinen URL-osoite selaimen osoiteriville.  

Tarkastele tuloksia määrittääksesi, onko juuri koulutetun mallin suorittaa tarpeeksi sekä korvaa aiemmin luotuun.

Kopioi tulosteen tulokset *BaseLocation*, *RelativeLocation*ja *SasBlobToken* .

## <a name="retrain-the-web-service"></a>Suuntaamista web-palvelu

Kun suuntaamista uusi web-palvelu, voit päivittää viittaamaan uuden koulutetun mallin ennakoivan web service-määritys. Web-palvelu määritelmä on WWW-palvelun koulutetun mallin sisäinen esittäminen ja ei voi muokata suoraan. Varmista, että haet WWW-palvelun määritys ennakoivan kokeessa ja ei koulutus kokeen.

## <a name="sign-in-to-azure-resource-manager"></a>Kirjaudu sisään ja Azure resurssien hallinta

Sinun on kirjauduttava Azure tiliisi PowerShell-ympäristöön [Lisää AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komennolla.

## <a name="get-the-web-service-definition-object"></a>Hae WWW-palvelun määritelmä-objekti

Saat WWW-palvelun määritelmä-objektin seuraavaksi soittamalla [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komento.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Aiemmin luodun WWW-palvelun resurssin ryhmän nimen määrittämiseen suorittamalla Get-AzureRmMlWebService cmdlet-komento ilman WWW-palvelut näkyvät tilauksen parametreja. Etsi web-palvelu ja katso sen WWW-palvelun tunnus. Resurssiryhmän nimi on tunnus, neljännen osan vain *resourceGroups* elementin jälkeen. Seuraavassa esimerkissä resurssin ryhmänimi on oletusarvo-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Vaihtoehtoisesti voit määrittää aiemmin luodun WWW-palvelun resurssiryhmän nimi, kirjaudu Azure koneen Learning Web Services-portaaliin. Valitse web-palvelu. Resurssin ryhmänimi on viidennen osan WWW-palvelun URL-osoitteen vain *resourceGroups* elementin jälkeen. Seuraavassa esimerkissä resurssin ryhmänimi on oletusarvo-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a>Vie JSON WWW-palvelun määritelmä-objekti

Muokattava koulutetun mallin juuri koulutetun mallin käyttämään määrityksen sinun on käytettävä [Vie AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet-komento vieminen JSON-tiedostomuodossa.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a>Päivitä ilearner Blob-objektien viittaus

Kohteiden Etsi [koulutetun malli], Päivitä ilearner Blob-objektien URI *locationInfo* solmun *uri* -arvo. URI luodaan yhdistämällä *BaseLocation* ja *RelativeLocation* uudelleenkoulutusta puhelun BES tulosteesta.

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a>JSON tuominen Web-palvelu määritelmä-objekti

Sinun on käytettävä [Tuo AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet-komento muuntaa takaisin Web-palvelu määritelmä-objektia, jonka avulla voit päivittää predicative kokeen muokattu JSON-tiedosto.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a>Päivitä web-palvelu

Lopuksi Päivitä ennakoivan kokeen [Päivityksen AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet-komennon avulla.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
