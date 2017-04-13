<properties
 pageTitle="Azure IoT valmiiksi ratkaisuja | Microsoft Azure"
 description="Azure-IoT kuvauksen valmiiksi ratkaisuihin ja niiden arkkitehtuuri linkkejä lisäresursseihin."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Mitkä ovat valmiiksi Azure IoT Suite ratkaisuja?

Azure IoT Suite valmiiksi-ratkaisuja yleisiin IoT ratkaisu kuviot, voit ottaa käyttöön tilauksen käyttämällä Azure käyttöotot. Voit käyttää ennalta määritettyjä ratkaisuja:

- Oman IoT ratkaisuja pohjana.
- Lisätietoja IoT ratkaisun rakenne-ja Yleiset mallit.

Kunkin esimääritettyjä ratkaisu on määritetty, lopusta loppuun toteutus, joka käyttää Simuloitu laitteiden telemetriatietojen luomiseen.

Käyttöönotto ja ratkaisut käytössä Azure lisäksi voit ladata valmis lähdekoodin ja sitten Mukauta ja laajentaa ratkaisun IoT erityistarpeita.

> [AZURE.NOTE] Ottaa käyttöön jonkin esimääritetyt ratkaisut, käy [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Artikkelin [aloittaminen IoT valmiiksi ratkaisuja] [ lnk-getstarted-preconfigured] on lisätietoja asentamisesta ja suorita jompikumpi ratkaisuja.

Seuraavassa taulukossa on esitetty, miten ratkaisuja yhdistäminen IoT lisäominaisuuksia:

| Ratkaisu | Tietoja nieltynä | Laitteen tunnistetiedot | Komento- ja hallinta | Säännöt ja toiminnot | Ennakoivan Analytics |
|------------------------|-----|-----|-----|-----|-----|
| [Remote seuranta][lnk-getstarted-preconfigured] | Kyllä | Kyllä | Kyllä | Kyllä | -   |
| [Ennakoivan ylläpito][lnk-predictive-maintenance] | Kyllä | Kyllä | Kyllä | Kyllä | Kyllä |

- *Tietoja nieltynä*: tunkeutumisen tietojen pilveen tasolla.
- *Laitteen tunnistetiedot*: jokaisen yhdistetyn laitteen yksilöllinen jäsenyyksiä hallitaan.
- *Komento- ja hallinta*: lähettämiseen laitteeseen pilvestä johtuu laitteen joitakin toimia.
- *Säännöt ja toiminnot*: ratkaisun takaisin loppuun käyttää sääntöjä toimimaan tietyt pilvipalveluun laitteen tiedot.
- *Ennakoivan analytics*: ratkaisun takaisin loppuun koskee analysoi ennustaa, kun toimenpiteitä olisi asetetaan pilvipalveluun laitteen tiedot. Esimerkiksi analysoiminen ilma ohjelma telemetriatietojen voit selvittää, milloin ohjelma ylläpito erääntyy.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Remote valmiiksi seuranta-ratkaisun yleiskatsaus

Microsoft on valinnut Keskustele remote seurantaa esimääritettyjä ratkaisu tämän artikkelin, koska se on kuvattu useita yleisiä suunnitteluelementtejä, joilla on muita ratkaisuja.

Seuraavassa kaaviossa on kuvattu tärkeimmät elementit ja remote seurantaa ratkaisu. Seuraavissa osissa on lisätietoja näitä elementtejä.

![Remote seuranta valmiiksi ratkaisuarkkitehtuuri][img-remote-monitoring-arch]

## <a name="devices"></a>Laitteet

Kun otat käyttöön remote seurantaa esimääritettyjä ratkaisun, neljä Simuloitu laitteet ovat valmiiksi valmistellun ratkaisu, joka saadaan aikaan jäähdyttimen. Simuloitu laitteet on valmiita lämpötilan ja kosteuden mallin, joka tietokoneesta kuuluu äänimerkki telemetriatietojen. Simuloitu seuraaviin laitteisiin sisältyvät Arvovirtakarttojen lopusta loppuun ratkaisun tietojen ja saat helposti lähteen telemetriatietojen ja kohde komentoja käyttämällä lähtökohtana ratkaisun mukautettu toteutus taustatietokantaan kehittäjä on.

Kun laite muodostaa IoT keskittimeen ensin remote seurantaa esimääritettyjä ratkaisun, lähettää IoT-toiminnossa laitteen tiedot viestiä Luetteloi komentoja, joita laite voi vastata luettelo. Remote seurantaa esimääritettyjä ratkaisussa komennot ovat: 

- *Ping-laitteen*: laite vastaa Tämä komento kuittausta kanssa. Tästä on hyötyä, kun laite on edelleen aktiivinen ja kuunteleminen tarkistamiseen.
- *Käynnistä Telemetriatietojen*: Ohjaa laite, kun haluat lisätä telemetriatietojen.
- *Lopeta Telemetriatietojen*: Ohjaa enää halua lähettää telemetriatietojen laite.
- *Muuta määrittäminen-kohdan lämpötila*: Ohjaa laite lähettää Simuloitu lämpötilan telemetriatietojen arvot. Tästä on hyötyä testikäyttöön taustatietokantaan logiikan.
- *Diagnostiikan Telemetriatietojen*: määrittää, jos laitteen lähetettävä ulkoisen lämpötilan telemetriatietojen.
- *Muuta laitteen tilan*.: tilan metatiedot-ominaisuus, joka laitteen raportit. Tästä on hyötyä testikäyttöön taustatietokantaan logiikan.

Voit lisätä Lisää Simuloitu laitteiden ratkaisu, joka lähettää sama telemetriatietojen ja vastata samoja komentoja. 

## <a name="iot-hub"></a>IoT-toiminnossa

Tämä esimääritettyjä ratkaisu IoT keskittimeen esiintymä vastaa tyypillinen [IoT ratkaisuarkkitehtuuri] *Cloud yhdyskäytävän* [lnk-what-is-azure-iot].

IoT-toiminnossa saa telemetriatietojen osoitteessa yhden loppupisteen laitteet. IoT-toiminnossa ylläpitää myös laitteen tietyn päätepisteet missä kunkin laitteet voit hakea komentoja, jotka lähetetään siihen.

IoT-toiminnossa on vastaanotettu telemetriatietojen lukea päätepisteen palvelun Include telemetriatietojen kautta.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Valmiiksi määritetyt ratkaisu käyttää kolme [Azure Stream Analytics] [ lnk-asa] (ASA) työt suodattamaan telemetriatietojen virta laitteilla:


- *DeviceInfo työ* - tulostaa tiedot tapahtuma-keskittimeen, jotka ohjaavat laitteen rekisteröinti tietyn, lähetetyt viestit, kun laite on ensin muodostaa tai **Muuta laitteen tila** -komennon suorittaminen ratkaisu laitteen rekisteri (DocumentDB-tietokanta). 
- *Telemetriatietojen työ* - lähettää kaikki raaka telemetriatietojen kiinnostunut tallennustilan Azure-blob-säiliö ja laskee, jotka näyttävät ratkaisu Raporttinäkymät-ikkunan telemetriatietojen koosteet.
- *Sääntöjen työ* - suodattaa telemetriatietojen profiilivirta arvot, jotka ylittävät mitä tahansa sääntöä raja-arvot ja tulostaa tapahtumaa-toiminnossa käytettävät tiedot. Säännön käynnistyy, kun ratkaisu portaalin dashboard-näkymä näyttää tapahtuman hälytys historiataulukossa uusi rivi ja käynnistää toiminnon säännöt ja toiminnot näkymien ratkaisu-portaalissa määritettyjen perusteella.

Tässä esimääritettyjä ratkaisussa ASA työt lomakkeen osa **IoT ratkaisu takaisin Lopeta** tyypilliset [IoT ratkaisuarkkitehtuuri][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Tapahtuman suoritin

Tässä esimääritettyjä ratkaisussa tapahtuman käsittelijä lomakkeiden osa **IoT ratkaisu takaisin loppu** tyypillinen [IoT ratkaisuarkkitehtuuri][lnk-what-is-azure-iot].

**DeviceInfo** ja **säännöt** ASA työt Lähetä niiden tapahtuman porttiin toimittamista uudelleen muihin palveluihin. Ratkaisu käyttää [EventPocessorHost] [ lnk-event-processor] esiintymä käynnissä [WebJob][lnk-web-job], viestien lukeminen nämä tapahtuman keskittimet. **EventProcessorHost** **DeviceInfo** tietoja käyttäen laitteen DocumentDB-tietokannan tietojen päivittäminen ja käyttää **sääntöjä** tietoja käynnistää logiikan sovelluksen ja Päivitä ilmoituksia näyttää ratkaisu-portaalissa.

## <a name="device-identity-registry-and-documentdb"></a>Laitteen tunnistetietojen rekisterin ja DocumentDB

Jokaisen IoT-toiminnossa sisältää [laitteen tunnistetietojen rekisterin] [ lnk-identity-registry] , joka tallentaa laitteen avaimet. IoT keskittimeen käyttää näitä tietoja todentaa laitteita - laite on rekisteröity ja on kelvollinen näppäintä, ennen kuin voit muodostaa valitsemalla.

Tämä ratkaisu tallentaa Lisätietoja laitteita, kuten niiden tilaan, niiden tukemista komentoja ja muita metatietoja. Ratkaisu käyttää DocumentDB tietokannan tämä ratkaisu kielikohtaiset laitteen tietojen tallentamiseen ja ratkaisu-portaalin noutaa tietoja DocumentDB tietokannan näyttöä ja muokkausta varten.

Ratkaisu on säilytettävä tiedot myös synkronoida DocumentDB tietokannan sisällön laitteen tunnistetietojen rekisterin. **EventProcessorHost** käyttää **DeviceInfo** stream analytics projektin tiedot synkronoinnin hallinta.

## <a name="solution-portal"></a>Ratkaisu-portaalissa

![Ratkaisu Raporttinäkymät-ikkunan][img-dashboard]

Ratkaisu-portaali on verkkopohjainen Käyttöliittymä, joka otetaan käyttöön pilveen esimääritettyjä ratkaisun osana. Sen avulla voit:

- Näytä telemetriatietojen ja hälytys historia raporttinäkymät-ikkunassa.
- Valmistele uusissa laitteissa.
- Hallinta ja valvonta laitteet.
- Lähettää komentoja tiettyjä laitteita.
- Sääntöjen ja toimintojen hallinta.

Tässä esimääritettyjä ratkaisussa ratkaisu-portaalin lomakkeiden osa **IoT ratkaisu takaisin loppu** ja osan **käsittelyn ja Yritystietojen yhdistämispalvelun** tyypillinen [IoT ratkaisuarkkitehtuuri][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja IoT ratkaisu arkkitehtuureihin [Microsoft Azure IoT services: viittaus arkkitehtuuri][lnk-refarch].

Kun tiedät esimääritettyjä ratkaisu on, voit aloittaa mukaan *remote seuranta* valmiiksi-ratkaisun käyttöönotto: [Aloita esimääritetyt ratkaisut][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md