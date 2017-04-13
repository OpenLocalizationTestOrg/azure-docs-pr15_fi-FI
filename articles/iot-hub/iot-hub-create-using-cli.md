<properties
    pageTitle="Luo IoT-keskittimeen Azure CLI | Microsoft Azure"
    description="Noudata tämän artikkelin käyttämisestä Azure IoT-toiminnossa."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Luo käyttämällä Azure CLI IoT-toiminnossa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Johdanto

Voit luoda ja hallita Azure IoT keskittimet ohjelmallisesti Azure käyttöliittymä. Tässä artikkelissa kerrotaan, miten Azure-CLI avulla voit luoda IoT-toiminnossa.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

- Azure active tili. Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -vain muutaman minuutin.
- [Azure CLI 0.10.4] [ lnk-CLI-install] tai uudempi versio. Jos sinulla on jo Azure CLI voit tarkistaa nykyisen version seuraavalla komennolla komentoriville seuraava komento:
```
    azure --version
```

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Azure Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md). Azure-CLI edellyttää, että Azure Resurssienhallinta-tilassa:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Azure-tili ja tilauksen määrittäminen 

1. Komentokehote kirjautumisen kirjoittamalla seuraava komento
```
    azure login
```
Todentaa ehdotettu selain ja koodin avulla.

2. Jos sinulla on useita tilauksia Azure-yhteyden muodostamisesta Azure myöntää käyttöoikeuksia kaikki Azure-tilaukset, jotka liittyvät tunnistetiedot. Voit tarkastella Azure tilaukset sekä kumpi on oletusarvon mukaan-komennon avulla
```
    azure account list 
```

Jos haluat määrittää tilauksen yhteydessä vallitessa, jonka haluat suorittaa loput komentojen käyttäminen

```
    azure account set <subscription name>
```

3. Jos sinulla ei ole resurssiryhmä, voit luoda nimetty **exampleResourceGroup** 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] [Käytä Azure resurssit ja resurssiryhmien hallintaan Azure-CLI] artikkelin[ lnk-CLI-arm] on lisätietoja Azure CLI avulla voit hallita Azure resursseja. 


## <a name="create-an-iot-hub"></a>Luo IoT-toiminnossa

Tarvittavat parametrit:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Jos haluat nähdä kaikki parametrit käytettävissä luontia varten voit käyttää Ohje-komennon komentorivi-ikkuna
```
    azure iothub create -h 
```
Lyhyt Esimerkki:

 Voit luoda IoT-toiminnossa kutsutaan **exampleIoTHubName** , suorita seuraava komento resurssin ryhmän **exampleResourceGroup**
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Azure CLI Tämä komento luo S1 vakio IoT keskittimeen, jossa on laskutettu. Voit poistaa komennon seuraavia käyttämällä IoT toiminnossa **exampleIoTHubName** 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja kehitystyön IoT toiminnosta on seuraavissa artikkeleissa:
- [IoT keskittimeen SDK: T][lnk-sdks]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Azure-portaalin hallinta IoT keskittimeen avulla][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
