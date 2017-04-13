<properties
 pageTitle="Hae Azure työkalut (10.10 Mac OS) | Microsoft Azure"
 description="Asentaminen Mac OS Python ja Azure käyttöliittymä (Azure CLI)."
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

# <a name="21-get-azure-tools-macos-1010"></a>2.1 saada Azure työkaluja (Mac OS 10.10)

> [AZURE.SELECTOR]
- [Windows 7: n +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 mitä hyötyä

Asenna Azure käyttöliittymä (Azure CLI). Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="212-what-you-will-learn"></a>2.1.2 mitä opit

- Voit asentaa Azure CLI.
- Voit lisätä Azure CLI IoT-kohteen osaryhmä.

## <a name="213-what-you-need"></a>2.1.3 mitä tarvitset

- Mac-tietokoneeseen, jossa Internet-yhteys
- Aktiivinen Azure-tilaus. Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin.

## <a name="214-install-python"></a>2.1.4 asentaa Python

Vaikka Mac OS mukana Python 2.7 ruutuun ulos, on suositeltavaa asentaa Python Homebrew kautta. Katso [Asentaminen Python Mac OS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Asenna Python ja pip suorittamalla seuraavan komennon:

```bash
brew install python
```

## <a name="215-install-the-azure-cli"></a>2.1.5 asentaa Azure CLI

Azure-CLI tarjoaa useita komentorivin kokemus Azure, voit työskennellä suoraan komentorivi valmistelu ja resurssien hallinta. 

Voit asentaa uusimman Azure CLI, toimimalla seuraavasti:

1. Suorita seuraavat komennot pääte-ikkunassa. Asenna Azure CLI viisi minuuttia voi kestää.

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```

2. Vahvista asennuksen suorittamalla seuraavan komennon:

    ```bash
    az iot -h
    ```
  
Raportissa pitäisi näkyä seuraava tulos, jos asennus ei onnistu.

![Suomi iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="215-summary"></a>2.1.5 yhteenveto

Olet asentanut Azure CLI. Jatka seuraavaan osaan luominen käyttämällä Azure-CLI Azure IoT keskittimeen ja laitteen jäsenyyden.

## <a name="next-steps"></a>Seuraavat vaiheet

[2.2 Luo IoT-toiminnosta ja rekisteröi vadelma pii-3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
