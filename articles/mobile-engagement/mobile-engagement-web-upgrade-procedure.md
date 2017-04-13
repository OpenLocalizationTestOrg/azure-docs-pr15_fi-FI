<properties
    pageTitle="Azure Mobile välitys Web SDK päivityksen menettelyt | Microsoft Azure"
    description="Uusimmat päivitykset ja Azure Mobile välitys Web SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile välitys Web SDK päivityksen ohjeita

Jos olet jo integroitu web-sovelluksen Azure Mobile välitys Web SDK aiemmassa versiossa, tarvitset seuraavat seikat huomioon, kun päivität SDK.

Jos olet ohittanut Mobile välitys Web SDK useita versioita, voit joutua useita toimien tekeminen päivityksen aikana. Esimerkiksi jos siirtää toisesta 1.4.0 1.6.0, ensin ohjeiden päivittämään 1.4.0 1.5.0. Noudata sitten ohjeita päivittämään 1.5.0 1.6.0.

Päivität, kumpi versio korvaa azure-engagement.js-tiedoston aiemman version tiedoston uusimman version.

## <a name="upgrade-from-121-to-200"></a>Päivittää 2.0.0 1.2.1

Tässä osassa käsitellään siirtää Mobile välitys Web SDK-integroinnin Capptain palvelusta tarjoamat Capptain SAS Azure Mobile välitys-sovellukseen. Jos olet siirtymässä aiemmasta versiosta, katso siirtämään ensin 1.2.1 Capptain-sivuston ja käytä sitten seuraavasti.

Tämä Mobile välitys Web SDK-versio ei tue Samsung Smart TV, Opera TV, webOS tai Reach-ominaisuus.

>[AZURE.IMPORTANT] Capptain ja Azure Mobile välitys eivät ole samalla tavalla. Seuraavassa korostaa vain kuinka voit siirtää asiakas-sovellus. Siirtyminen mobiili välitys Web SDK-sovelluksessa ei siirretä tietojen Capptain palvelimesta Mobile välitys-palvelimeen.

### <a name="javascript-files"></a>JavaScript-tiedostot

Korvaa tiedoston capptain-sdk.js tiedoston azure-engagement.js ja Päivitä komentosarjan tuonnin.

### <a name="remove-capptain-reach"></a>Poista Capptain saatavilla

Tämä Mobile välitys Web SDK-versio ei tue Reach-ominaisuus. Capptain Reach Integroitu sovellus, jos haluat poistaa sen.

Poista yhteys CSS-Tuo sivun ja poista liittyvät .css-tiedosto (capptain-reach.css, oletusarvoisesti).

Poista Reach seuraavissa resursseissa: Sulje kuva (capptain-close.png, oletusarvon mukaan) ja merkki-kuvaketta (capptain-ilmoitus-kuvake, oletusarvoisesti).

Poista yhteys Käyttöliittymän-sovellusten ilmoitukset. Oletusarvoisesti näyttää seuraavanlaiselta:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Poista yhteys Käyttöliittymän teksti ja web-ilmoitukset ja kyselyjä. Oletusarvoisesti näyttää seuraavanlaiselta:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Poista `reach` objekti kokoonpanosi, jos se on olemassa. Se näyttää tältä:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Poista muut Reach mukauttaminen, kuten luokat.

### <a name="remove-deprecated-apis"></a>Poista poistettu API

Jotkin API-Capptain on poistettu Mobile välitys Web SDK: ssa.

Poista seuraavat API puhelujen: `agent.connect`, `agent.disconnect`, `agent.pause`, ja `agent.sendMessageToDevice`.

Poistaa kaikki esiintymät seuraavat takaisinkutsuja Capptain määritys: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, ja `onPushMessageReceived`.

### <a name="configuration"></a>Määritys

Mobile välitys käyttää yhteysmerkkijonon SDK tunnuksia, esimerkiksi sovellustunnus määrittämiseen.

Korvaa Sovellustunnus yhteysmerkkijono. Huomaa, että SDK määrityksen global-objekti muuttuu `capptain` , `azureEngagement`.

Ennen siirtoa:

    window.capptain = {
      appId: ...,
      [...]
    };

Kun siirto:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Sovelluksen yhteysmerkkijonon näkyy Azure-portaalissa.

### <a name="javascript-apis"></a>JavaScript-ohjelmointirajapinnan

Yleinen JavaScript object `window.capptain` on nimetty uudelleen `window.azureEngagement` , mutta voit käyttää `window.engagement` API puheluihin alias. Et voi käyttää alias määrittämään SDK-määritys.

Esimerkiksi `capptain.deviceId` tulee `engagement.deviceId`, `capptain.agent.startActivity` tulee `engagement.agent.startActivity`ja niin edelleen.
