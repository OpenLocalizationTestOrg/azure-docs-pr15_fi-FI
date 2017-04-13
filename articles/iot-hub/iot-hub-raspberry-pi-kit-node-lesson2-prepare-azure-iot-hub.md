<properties
 pageTitle="Luo IoT-toiminnosta ja rekisteröi vadelma pii-3 | Microsoft Azure"
 description="Luoda resurssiryhmän, Luo Azure IoT-toiminnosta ja rekisteröi oman pii käyttämällä Azure-CLI Azure IoT-toiminnossa."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 Luo IoT-toiminnosta ja rekisteröi vadelma pii-3

## <a name="221-what-you-will-do"></a>2.2.1 mitä hyötyä

- Luo resurssiryhmä.
- Luo resurssiryhmän Azure IoT-toiminnossa.
- Lisää Azure IoT keskittimeen Azure-CLI vadelma pii-3.

Kun Azure-CLI avulla voit lisätä oman pii IoT-keskittimeen, palvelun Luo avain oman pii palvelun todentamismenetelmä. Jos täytät ilmenee ongelmia, Etsi ratkaisuja [sivun vianmääritys](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2 mitä opit

- Opi käyttämään Azure-CLI luominen Azure-IoT toiminnossa.
- Vaiheittaiset ohjeet laitteen käyttäjätietojen luominen oman pii Azure IoT-toiminnossa.

## <a name="223-what-you-need"></a>2.2.3 mitä tarvitset

- Azure-tili
- Mac-tietokoneesta tai Windows-tietokone, jossa on asennettu Azure CLI

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 luominen Azure IoT-toiminnossa

Azure IoT keskittimeen avulla voit muodostaa, seurata ja hallita miljoonia IoT kalusto. Luo Azure IoT-keskittimeen, toimi seuraavasti:

1. Kirjaudu sisään Azure-tili suorittamalla seuraavan komennon:

    ```bash
    az login
    ```

    Kaikki käytettävissä olevat Azure tilausten näkyvät onnistumisen kirjautumisen jälkeen.

2. Asettaminen Azure tilauksen, jota haluat käyttää suorittamalla seuraavan komennon:

    ```bash
    az account set -n {subscription id or name}
    ```

    Tilauksen tunnus tai nimi löytyy tulosteen `az login`.

3. Rekisteröi palveluntarjoajan suorittamalla seuraavan komennon:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Sinun on rekisteröitävä palveluntarjoajan ennen niiden käyttöönottoa Azure resurssi, joka sisältää palvelun.

    > [AZURE.NOTE] Useimmat palveluntarjoajat on rekisteröity automaattisesti Azure portaalin tai käytät Azure CLI mukaan, mutta ei kaikkiin. Lisätietoja palvelu on kohdassa [vianmääritys yleisiä Azure käyttöönoton Azure resurssien hallinta](../resource-manager-common-deployment-errors.md)

4. Luo resurssiryhmän nimeltä iot otoksen Länsi US alueen suorittamalla seuraavan komennon:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Luo IoT-toiminnossa iot otoksen resurssiryhmä suorittamalla seuraavan komennon:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Voit luoda IoT-toiminnossa oletusarvo-versio on **F1**, joka on **vapaa**. Lisätietoja on artikkelissa [Azure IoT keskittimeen hinnat](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] IoT-toiminnossa nimen on oltava yksilöivä.
>
> Voit luoda vain yhden **F1** version Azure IoT keskittimeen Azure tilauksen mukaisesti.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 rekisteröidä oman pii IoT-toiminnossa

Kunkin laitteella, joka lähettää ja vastaanottaa sähköpostiviestejä ja Azure IoT-toiminnossa rekisteröitävä yksilöllinen tunnus.

Rekisteröityä oman pii Azure IoT-toiminnossa käynnissä seuraava komento:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 yhteenveto

Olet luonut Azure IoT-toiminnosta ja että pii rekisteröityjä laitteen tunnistetietojen Azure IoT-toiminnossa. Olet valmis, voit siirtää seuraavaan Harjoitus Lisätietoja viestien lähettäminen IoT-toiminnossa oman pii.

## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt [luoda Azure funktio-sovelluksen ja Azure-tallennustilan tilin käsittelemään ja IoT keskittimeen viestien tallentaminen](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)Harjoitus 3, joka alkaa aloittaminen.