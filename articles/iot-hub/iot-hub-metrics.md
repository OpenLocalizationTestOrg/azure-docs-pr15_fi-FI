<properties
 pageTitle="IoT keskittimeen diagnostiikan arvot"
 description="Azure IoT keskittimeen mittarit, käyttäjät voivat arvioida niiden resurssi yleinen kunto yleiskatsaus"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Johdanto diagnostiikan arvot

Diagnostiikan arvot antavat paremman Azure resurssien tilan tietoja Azure-tilaukseesi. Arvot, joiden avulla voit arvioida palvelun ja siihen liitettyjen laitteiden yleinen kunto. Käyttäjän osoittava tilastotiedot ovat tärkeitä, sillä ne avulla voit tarkastella, mistä on kyse joiden IoT keskittimeen ja ohjeen syiden seurantakohteet tarvitsematta Azure tukipalvelun yhteystiedot.

Voit ottaa diagnostiikan arvot Azure-portaalista.

## <a name="how-to-enable-diagnostic-metrics"></a>Diagnostiikan arvot ottaminen käyttöön

1. Luo IoT-toiminnossa. Löydät ohjeita luomisesta IoT-toiminnossa [Aloita] [ lnk-get-started] opas.

2. Avaa IoT-toiminnossa sivu. Sieltä Valitse **Diagnostiikka**.

    ![][1]

3. Määritä oman diagnostiikka tilan asettaminen **,** ja valitsemalla Azure-tallennustilan tilin diagnostiikka tietojen tallennusta varten. Tarkista **arvot**ja paina sitten **Tallenna**. Huomaa, että Azure-tallennustilan tilin on luotava etuajassa ja että perittävän erikseen tallennustilan. Voit valita myös diagnostiikka-tietojen lähettäminen tapahtuman keskittimet päätepisteen.

    ![][2]

4. Kun olet määrittänyt diagnostiikkaa, palaa **Yleiskatsaus** IoT keskittimeen-sivu. Arvot tiedot on täytetty **Seuranta** -osa sivu. Arvot-ruudussa kohtaa, johon voi tarkastella arvot yhteenvetotietoja IoT-toiminnosta ja muokata kaavion arvot valinta napsauttamalla kaaviota avautuu. Voit myös määrittää ilmoituksia metrisillä arvojen perusteella.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Arvot ja niiden käyttämisestä

IoT-toiminnossa on useita mittarit antavat yleiskuvan oman keskittimeen ja siihen liitettyjen laitteiden kokonaismäärän kunto. Voit yhdistää useita mittarit paint IoT-toiminnossa valtion koko kuvan tiedot. Seuraavassa taulukossa on kuvattu kunkin IoT-toiminnossa seuraa arvot ja miten kukin metrijärjestelmä liittyvät IoT-toiminnossa yleistä tilaa.

| Arvo | Metrijärjestelmän kuvaus | Lisätiedot käyttötarkoitus |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Kaikkien laitteiden välillä lähetettyjen viestien määrä | Yleistä tietojen viestin lähettäminen |
| d2c.telemetry.ingress.Success | Onnistuneiden viestien määrä keskittimeen | Onnistuneiden viestin tunkeutumisen keskittimeen yleiskatsaus |
| c2d.Commands.egress.Complete.Success | Valmis vastaanottamisessa laitteen eri laitteille komento viestien määrä | Abandon ja hylkää arvot ja antaa yhteenvedon yleinen C2D komento success korko |
| c2d.Commands.egress.Abandon.Success | Laske kaikkien viestien onnistuneesti ilman ylläpitäjää vastaanottamisessa laitteen kaikkien laitteiden välillä | Korostaa mahdollisen ongelman, jos viestit ovat käytön ilman ylläpitäjää useammin kuin oikein |
| c2d.Commands.egress.Reject.Success | Kaikkien viestien onnistuneesti hylännyt vastaanottava laite eri laitteille määrä | Korostaa viestit ovat käytön hylännyt useammin odotettua mahdolliset ongelmat |
| devices.totalDevices | Keskiarvo, min ja Maks luvun rekisteröity IoT-toiminnossa. | Rekisteröity-keskittimeen määrä |
| devices.connectedDevices.allProtocol | Keskiarvo, min ja Maks luvun samanaikaisesti liitettyjen laitteiden välillä | Yleistä keskittimeen liitettyjen laitteiden määrä |

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun nähnyt yleiskatsaus diagnostiikan mittarit, lisätietoja hallinta Azure IoT keskittimeen työskentelemisestä:

- [Toimintojen seuranta][lnk-monitor]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
