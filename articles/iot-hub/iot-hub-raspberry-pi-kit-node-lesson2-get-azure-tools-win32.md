<properties
 pageTitle="Hae Azure työkalut (Windows 7: n +) | Microsoft Azure"
 description="Asenna Python ja Azure käyttöliittymä (Azure CLI) Windows 7: ssä ja sitä uudemmissa versioissa."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 saada Azure työkaluja (Windows 7: n +)

> [AZURE.SELECTOR]
- [Windows 7: n +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 mitä hyötyä

Asenna Python ja komentorivin Azure Interface (Azure CLI). Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 mitä opit

- Voit asentaa Python.
- Voit asentaa Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 mitä tarvitset

- Windows-tietokone, jossa on Internet-yhteys
- Aktiivinen Azure-tilaus. Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin.

## <a name="214-install-python"></a>2.1.4 asentaa Python

Python asentaminen Windows-tietokoneessa. Voit asentaa Python 2.7, 3.4 tai 3.5. Tässä opetusohjelmassa perustuu Python 2.7. Jos olet jo asentanut Python, siirry kohtaan 2.1.5.

[Hae Python for Windowsissa](https://www.python.org/downloads/)

Sinun on myös lisättävä kansiot, jossa python.exe ja pip.exe asennetaan järjestelmään polun `PATH` ympäristömuuttuja. Python.exe asennetaan oletusarvoisesti `C:\Python27` ja pip.exe on asennettu `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 asentaa Azure CLI

Azure-CLI tarjoaa useita komentorivin kokemus Azure, voit työskennellä suoraan komentorivi valmistelu ja resurssien hallinta.

Voit asentaa Azure CLI, toimimalla seuraavasti:

1. Avaa komentokehoteikkuna järjestelmänvalvojana.
2. Suorita seuraavat komennot:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Vahvista asennuksen suorittamalla seuraavan komennon:

    ```bash
    az iot -h
    ```

Näyttöön tulee seuraava tulos, jos asennus ei onnistu.

![Suomi iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 yhteenveto

Olet asentanut Azure CLI. Jatka seuraavaan osaan luominen käyttämällä Azure-CLI Azure IoT keskittimeen ja laitteen jäsenyyden.

## <a name="next-steps"></a>Seuraavat vaiheet

[2.2 Luo IoT-toiminnosta ja rekisteröi vadelma pii-3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
