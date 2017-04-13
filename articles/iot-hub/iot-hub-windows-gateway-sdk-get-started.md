<properties
    pageTitle="Aloita IoT keskittimeen yhdyskäytävän SDK | Microsoft Azure"
    description="Havainnollistetaan keskeisiä käsitteitä pitäisi ymmärtää, kun käytät Azure IoT yhdyskäytävän SDK käyttäminen Windows Azure IoT yhdyskäytävän SDK ongelmatilanteita."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Aloita käyttäminen Windows Azure IoT yhdyskäytävän SDK (beta)-

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Otosten muodostaminen

Ennen kuin aloitat, sinun täytyy [määrittää oman kehitysympäristö] [ lnk-setupdevbox] käsittelyyn Windows SDK.

1. Avaa komentokehote **VS2015 Developer komentorivi-ikkuna** .
2. Siirry **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan pääkansio.
3. Suorita **Työkalut\\build.cmd** komentosarjan. Tämä komentosarja luo tiedoston ratkaisun Visual Studiossa, muodostaa ratkaisun ja suorittaa testejä. Löytyvät Visual Studio-ratkaisun **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan **luominen** -kansiossa.

## <a name="how-to-run-the-sample"></a>Otosten suorittaminen

1. **Build.cmd** komentosarja luo kansio nimeltä tietovaraston paikallisen tietokannan **luominen** . Tämä kansio sisältää tässä esimerkissä käytetään kahta moduulit.

    Muodosta komentosarja merkit- **logger_hl.dll** **luominen\\moduulit\\lokin\\virheenkorjaus** kansio ja valitse **hello_world_hl.dll** **luominen\\moduulit\\hello_world\\virheenkorjaus** kansio. Käyttää näitä polkuja **moduulin path** -arvo alla JSON asetustiedosto esitetyllä tavalla.

2. Tiedoston **hello_world_win.json** - **näytteiden\\hello_world\\src** kansio on Windowsin, joiden avulla voit suorittaa otosten Esimerkki JSON asetukset-tiedosto. Alla olevassa esimerkissä JSON asetukset olettaa monistaa **c** -asema pääkansioon IoT yhdyskäytävän SDK-säilöön. Jos latasit toiseen sijaintiin, sinun on muutettava **moduulin polku** arvot **näytteiden\\hello_world\\src\\hello_world_win.json** tiedoston vastaavasti.

3. Määritä **tiedoston nimi** -arvo **logger_hl** -moduulin **argumentit** -osassa, joka sisältää lokitiedot tiedoston polku ja nimi.

    Tämä on esimerkki JSON asetustiedosto Windows, kirjoittaa **C:** aseman **log.txt** -tiedostoon.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Komentokehotteeseen Siirry **azure-iot-yhdyskäytävä-sdk** -tietovarasto paikallisen kopion pääkansioon.
4. Suorita seuraava komento:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md