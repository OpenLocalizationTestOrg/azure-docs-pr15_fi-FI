<properties
    pageTitle="Aloita IoT keskittimeen yhdyskäytävän SDK | Microsoft Azure"
    description="Azure IoT yhdyskäytävän SDK tätä vaiheittaista käyttää Linux esitä avaimen käsitteitä pitäisi ymmärtää käytettäessä Azure IoT yhdyskäytävän SDK-paketissa."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT yhdyskäytävän SDK (beta) - Linux käytön aloittaminen

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Otosten muodostaminen

Ennen kuin aloitat, sinun täytyy [määrittää oman kehitysympäristö] [ lnk-setupdevbox] Linux SDK käsittelyyn.

1. Avaa on.
2. Siirry **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan pääkansio.
3. **Tools/build.sh** -komentosarjan suorittaminen Tämä komentosarja käyttää **cmake** -apuohjelma luo kansio nimeltä **azure-iot-yhdyskäytävä-sdk** -tietovarasto paikallisen kopion pääkansioon **luominen** ja luo koontitiedostoa. Valitse komentosarja muodostaa ratkaisun ja suorittaa testejä.

> [AZURE.NOTE]  Aina, kun suoritat **build.sh** komentosarja, poistaa ja luo sitten pääkansio **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan **luominen** -kansio.

## <a name="how-to-run-the-sample"></a>Otosten suorittaminen

1. **Build.sh** komentosarja luo tulos **azure-iot-yhdyskäytävä-sdk** -säilön paikallisen tietokannan **luominen** -kansiossa. Tämä sisältää tässä esimerkissä käytetään kahta moduulit.

    Muodosta komentosarja sijoittaa **liblogger_hl.so** - **Muodosta/moduulit/lokin/** kansio ja valitse **libhello_world_hl.so** **Muodosta/moduulit/hello_world/** kansio. Käyttää näitä polkuja **moduulin path** -arvo alla JSON asetustiedosto esitetyllä tavalla.

2. Tiedosto- **hello_world_lin.json** **näytteiden/hello_world/src** -kansiossa on esimerkki JSON asetukset tiedosto Linux, joiden avulla voit suorittaa otosten varten. Alla olevassa esimerkissä JSON asetukset oletetaan, voit suorittaa otosten **azure-iot-yhdyskäytävä-sdk** -tietovarasto paikallisen kopion ylimmällä.

3. Määritä **tiedoston nimi** -arvo **logger_hl** -moduulin **argumentit** -osassa, joka sisältää lokitiedot tiedoston polku ja nimi.

    Tämä on esimerkki Linux, kirjoittaa **log.txt** kansioon, jossa suoritat otosten JSON asetukset tiedostoon.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
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

3. Oman käyttöliittymän Siirry **azure-iot-yhdyskäytävä-sdk** -kansioon.
4. Suorita seuraava komento:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
