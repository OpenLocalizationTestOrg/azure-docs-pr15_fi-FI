<properties
 pageTitle="Luo Azure funktion sovelluksen ja Azure-tallennustilan tilin | Microsoft Azure"
 description="Azure funktion sovelluksen seuraa Azure IoT pääsivuston tapahtumia, käsittelee saapuvia viestejä ja kirjoittaa ne Azure-taulukkotallennus."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 luominen Azure funktion sovelluksen ja Azure-tallennustilan tilin

[Azure-Funktiot](../../articles/azure-functions/functions-overview.md) on käynnissä helposti pystysuuntaisena koodi, eli "toiminnot" pilven ratkaisu. Azure funktion sovelluksen isännöi oman Azure-funktioiden suorittaminen.

## <a name="311-what-will-you-do"></a>3.1.1 mitä tehdä

Azure Resurssienhallinta-mallin avulla voit luoda sovelluksen Azure-funktio ja Azure-tallennustilan tilin. Azure funktion sovelluksen seuraa Azure IoT pääsivuston tapahtumia, käsittelee saapuvia viestejä ja kirjoittaa ne Azure-taulukkotallennus. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="312-what-will-you-learn"></a>3.1.2 Mitä on tietoja

- Opi käyttämään [Azure Resurssienhallinta](../../articles/azure-resource-manager/resource-group-overview.md) ottamaan Azure resurssit.
- Vaiheittaiset ohjeet sovelluksen Azure-funktion avulla voit käsitellä IoT keskittimeen viestit ja kirjoittaa ne Azure-taulukkotallennus taulukkoon.

## <a name="313-what-do-you-need"></a>3.1.3 mitä on hyvä

- On onnistui edellisen opitut: [Aloita vadelma pii-3](iot-hub-raspberry-pi-kit-node-get-started.md) ja [Luo Azure IoT-toiminnossa](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 Avaa malli-sovellus

Esimerkki projektin avaaminen Visual Studio Code suorittamalla seuraavat komennot:

```bash
cd Lesson3
code .
```

![Repo rakenne](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- `app.js` -Tiedoston `app` alikansion on avaimen lähdetiedosto. Tämä lähdetiedosto on vilkkuvat kustakin viestistä, se lähettää LED ja Lähetä viesti 20 kertaa IoT-toiminnossa.
- `arm-template.json` Tiedosto on Azure Resurssienhallinta-malli, joka sisältää sovelluksen Azure-funktio ja Azure-tallennustilan tilin.
- `arm-template-param.json` Tiedosto on Azure Resurssienhallinta-mallin määritystiedosto.
- `ReceiveDeviceMessages` Alikansio sisältää Azure-funktion Node.js-koodi.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 määrittäminen Azure Resurssienhallinta mallit ja resurssien luominen Azure

Update `arm-template-param.json` tiedoston Visual Studio-koodin.

![Azure Resurssienhallinta Malliparametrit](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Korvaa **{keskittimeen nimeni}** , jonka olet määrittänyt [Harjoitus 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md) **[IoT pääsivuston nimi]** .
- Korvaa kaikki haluamasi etuliite **[etuliite merkkijono uusi resurssien]** . Etuliite varmistaa, että resurssinimi on yksilöivä ristiriidan välttämiseksi. Älä käytä viiva tai numeron alkuperäinen etuliite.

> [AZURE.NOTE] Sinun ei tarvitse `azure_storage_connection_string` sisältö. Pidä ennalleen.

Kun olet päivittänyt `arm-template-param.json` tiedosto, resurssien käyttöön Azure suorittamalla seuraavan komennon:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Voit luoda nämä resurssit kestää noin viisi minuuttia. Kun resurssi luominen on käynnissä, voit siirtää seuraavaan osaan.

## <a name="316-summary"></a>3.1.6 yhteenveto

Olet luonut Azure funktion sovelluksen IoT keskittimeen viestit ja Azure-tallennustilan tilin näiden viestien tallentamiseen. Voit siirtää ottaminen käyttöön ja suorita otosten seuraavan osion oman piillä laitteen cloud viestien lähettämiseen.

## <a name="next-steps"></a>Seuraavat vaiheet

[3.2 Suorita laitteen cloud viestien lähettämiseen vadelma pii-3-sovelluksen malli](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

