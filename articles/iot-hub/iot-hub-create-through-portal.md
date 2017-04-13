<properties
     pageTitle="Azure portaalin avulla voit luoda IoT-toiminnossa | Microsoft Azure"
     description="Yleisiä tietoja siitä, miten voit luoda ja hallita Azure IoT keskittimet palvelun Azure-portaalissa"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Luo Azure-portaalissa IoT-toiminnossa

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Johdanto

Tässä artikkelissa kuvataan etsiminen IoT keskittimeen palvelun Azure-portaalissa ja miten voit luoda ja hallita IoT keskittimet.

## <a name="where-to-find-iot-hubs"></a>Löydät IoT keskittimet

Useilla eri paikoissa, josta löydät IoT keskittimet.

1. **+ Uusi**: **Azure IoT keskittimeen** IoT-palvelu ja löytyvät luokan **Asioita Internet**, valitse **+ Uusi**, samalla tavalla kuin muiden.

2. IoT keskittimet voi käyttää myös kautta on Marketplace pääkuva palvelun **Asioita Internet**-kohdassa.

## <a name="create-an-iot-hub"></a>Luo IoT-toiminnossa

Voit luoda IoT-keskittimeen, seuraavilla tavoilla:

- Sivu-koodin seuraavassa näytössä näkyvät johtaa luominen IoT-toiminnossa **+ Uusi** -vaihtoehdon avulla. Tällä menetelmällä ja – on marketplace IoT-toiminnossa luomiseen vaiheet ovat samanlaiset.

- Luominen IoT-toiminnossa on Marketplace kautta: **Luo** napsauttaminen avaa sivu, joka on sama kuin edellinen sivu **+ Uusi** takaamiseksi. Seuraavien osien luettelon useita vaiheita luomisessa IoT-toiminnossa.

### <a name="choose-the-name-of-the-iot-hub"></a>Valitse nimi IoT-toiminnossa

Luo IoT-toiminnossa nimeä valitsemalla. Tämä nimi on oltava yksilöllinen keskittimet. Ei ole keskittimet monistaminen ei sallittu taustatietokantaan, joten on suositeltavaa, että tämä toiminto nimetään mahdollisimman yksilöllisesti.

### <a name="choose-the-pricing-tier"></a>Valitse hinnoittelu taso

Voit valita neljä tasoa: **vapaa**, **Standard 1** ja **Standard 2**ja **Vakio S3**. Vapaa taso sallii vain 500 laitteiden edellyttää yhteyttä IoT keskittimeen ja enintään 8 000 viestien päivässä.

**Vakio S1**: IoT keskittimet S1 edition on suunniteltu IoT ratkaisut, joissa on suhteellisen vähän kohti laitteen tiedot luodaan laitteet suuri määrä. Yksittäisen S1-version avulla enintään 400,000 viestien päivässä kaikkien yhdistettyjen laitteiden välillä.

**Vakio S2**: IoT keskittimeen S2 edition on suunniteltu IoT ratkaisuja, jossa laitteet Luo suuria tietomääriä. Yksittäisen S2-version avulla 6 miljoonaa viestien päivässä kaikkien yhdistettyjen laitteiden välillä.

**Vakio S3**: IoT keskittimeen S3 edition on suunniteltu IoT ratkaisuja, jotka luovat suurista tietomääristä. Yksittäisen S3-version avulla enintään 300 miljoonaa viestien päivässä kaikkien yhdistettyjen laitteiden välillä.

![][4]

> [AZURE.NOTE] IoT keskittimeen sallii vain yhden vapaa keskittimeen Azure tilauskohtaisten.

### <a name="iot-hub-units"></a>IoT keskittimeen yksiköt

IoT-toiminnossa yksikkö on tietty määrä viestejä päivässä. Viestien tueta tässä toiminnossa kokonaismäärä on päivässä, taso-viestien määrä kerrottuna yksiköiden määrän. Esimerkiksi halutessasi tukemaan tunkeutumisen 700,000 viestien IoT-toiminnossa voit kaksi S1 taso yksikköä.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Laitteen cloud osiot ja resurssiryhmä

Voit muuttaa IoT-toiminnossa osioiden määrää. Oletus-osiot on määritetty 4; Voit kuitenkin valita osioiden eri määrän avattavasta luettelosta.

Resurssien ryhmä ei tarvitse erikseen luoda tyhjän resurssiryhmä. Luodessasi resurssin, voit luoda uusi resurssiryhmä tai resurssi ryhmän.

![][5]

### <a name="choose-subscriptions"></a>Valitse tilaukset

Azure IoT toiminnossa näkyy automaattisesti Azure tilaukset, joihin käyttäjätili on linkitetty luettelo. Valitse jokin vaihtoehdoista tähän IoT-toiminnossa liitettävä kyseisen Azure tilauksen.

### <a name="choose-the-location"></a>Valitse sijainti

Sijainti-vaihtoehto on luettelo alueista, jossa IoT keskittimeen tarjotaan. IoT-toiminto on käytettävissä ottamaan johonkin seuraavista sijainneista: Australia Itä, Australian varaaja, Aasian Itä, Aasian varaaja, Eurooppa, Europe Länsi, japani Itä japani Länsi ja US Itä, US Länsi.

### <a name="create-the-iot-hub"></a>Luo IoT-toiminnossa

Kun kaikki edelliset vaiheet ovat valmiit, IoT-toiminto on valmis luodaan. Valitse **Luo** aloittaa luomalla IoT-toiminnossa asetukset taustatietokantaa ja käyttöönotto määritettyyn sijaintiin.

Voi kestää muutaman minuutin IoT-toiminnossa voit luoda, koska se vie aikaa oikeaan paikkaan palvelimet esiintyvän taustatietokantaan-käyttöönottoa varten.

## <a name="change-the-settings-of-the-iot-hub"></a>IoT-toiminnossa asetusten muuttaminen

Voit muuttaa olemassa olevan IoT-toiminnossa asetukset sen jälkeen, kun se on luotu IoT keskittimeen-sivu.

![][8]

**Jaettu käytäntöjen**: kyseiset käytännöt Määritä laitteet ja palveluiden käyttöoikeudet muodostaa IoT toiminnossa. Voit käyttää käytännöt napsauttamalla **Jaettu käytäntöjen** **Yleiset**-kohdassa. Tämä sivu-voit muokata aiemmin luotuja käytäntöjä tai lisätä uuden käytännön.

### <a name="create-a-policy"></a>Käytännön luominen

- Valitse **Lisää** Avaa sivu. Tässä voit kirjoittaa uuden käytännön nimen ja käyttöoikeudet, jotka haluat yhdistää tämän käytännön seuraavassa kuvassa esitetyllä tavalla.

    On useita käyttöoikeudet, jotka voivat liittyä jaettuun käytännöt. Kaksi ensimmäistä käytännöt, **rekisterin lukea** ja **rekisterin kirjoittaminen**, lue myöntäminen ja kirjoitusoikeudet oikeudet laitteen tunnistetietojen säilön tai identity-rekisterin. Kirjoita-vaihtoehdon valitseminen automaattisesti valitsee sekä luku vaihtoehdon.

    **Palvelun yhteyden** käytäntö antaa cloud Include päätepisteet kuten palvelut yhteyden muodostaminen IoT-toiminnossa kuluttaja-ryhmän käyttöoikeus. **Laitteen yhteyden** käytännön myöntää käyttöoikeuksia IoT-toiminnossa laitteen Include päätepisteet-viestien lähettäminen ja vastaanottaminen.

- Valitse **Luo** , jos haluat lisätä tämän vastaluotu käytännön olemassa olevaan luetteloon.

![][10]

## <a name="messaging"></a>Viestintä

Valitse Näytä luettelo messaging ominaisuuksia, jotka muokataan IoT-toiminnossa **viestit** . On kaksi päätyyppiä ominaisuudet, joita voit muokata ja kopioida: **Cloud laitteeseen** ja **laitteen pilveen**.

- **Cloud laitteen** asetukset: Tämä asetus on kaksi subsettings: **pilvestä, joka laitteen TTL** (time-to-live) ja viestit **säilytys aika** . Kun IoT-toiminnossa on luotu, nämä asetukset luodaan oletusarvon tunnin. Voit muuttaa näitä arvoja liukusäätimillä tai kirjoita haluamasi arvot.

- **Laitteen pilveen** asetukset: Tämä asetus on useita subsettings, joista osa nimeltä/varataan, kun IoT-toiminnossa luodaan ja voidaan kopioida vain muiden subsettings, joita voi mukauttaa. Nämä asetukset on lueteltu seuraavan osion.

**Osioiden**: arvo on määritetty, kun IoT-toiminnossa luodaan, ja niitä voi muuttaa tämän asetuksen avulla.

**Tapahtuman keskittimeen yhteensopivan nimi ja päätepisteen**: kun IoT toiminnosta on luotu, tapahtuma-toiminnossa luodaan sisäisesti, joudut ehkä käyttää kaikissa tilanteissa. Tätä tapahtumaa-toiminnossa yhteensopivan nimi ja päätepistettä ei voi mukauttaa, mutta ei käytettävissä kautta **Kopioi** -painike.

**Säilytys-aika**: määrittää oletusarvon mukaan yksi päivä, mutta voi mukauttaa muita arvoja, valitse avattavasta luettelosta. Tämä arvo on päivissä laitteen pilveen, mutta ei tuntia, on samanlainen laitteeseen Cloud-asetusta.

**Ryhmiä**: ryhmiä ovat samalla tavalla kuin muiden tekstiviesti järjestelmien, jonka avulla voidaan hakea tietoja tietyllä tavalla muodostaa muita sovelluksia ja palveluja IoT keskittimeen asetus. Jokaisen IoT-toiminnossa on luotu kuluttaja oletusryhmän. Voit lisätä tai poistaa ryhmiä IoT porttiin avulla.

> [AZURE.NOTE] Kuluttaja oletusryhmän ei voi muokata tai poistaa.

![][11]

## <a name="pricing-and-scale"></a>Hinnat ja asteikko

Olemassa olevan IoT-toiminnossa hinnat voi muuttaa kautta **hinnoittelu** asetuksia, mutta seuraavin poikkeuksin:

- Käyttöönottoon, IoT-toiminnosta ja vapaa-tuote ei voi muuttaa tasoa johonkin maksettu tuotteissa tai päinvastoin.
- -Azure tilauksen voi olla vain yksi vapaa taso IoT toiminnossa.

![][12]

Siirtyvät korkeampi taso (S2 tai S3) alemmalle tasolle taso (S1 tai S2) on sallittu vain silloin, kun päivän viestien lukumäärä eivät ole ristiriitaisia. Esimerkiksi jos päivässä viestien määrä ylittää 400,000, valitse IoT-toiminnossa tason voi muuttaa. Kuitenkin jos S1 ylätasolla muutat sitten keskittimeen on rajoitettu päivän.

## <a name="delete-the-iot-hub"></a>Poista IoT-toiminnossa

Voit selata poistettavien valitsemalla **Selaa**ja valitsemalla sitten haluamasi toiminto poistaa IoT keskittimeen. Napsauta **Poista** -painiketta, valitsemalla poistettava keskittimeen nimen alapuolella.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja hallinnasta Azure IoT keskittimeen seuraavista linkeistä:

- [Bulk IoT laitteiden hallinta][lnk-bulk]
- [Käyttö-arvot][lnk-metrics]
- [Toimintojen seuranta][lnk-monitor]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]
- [Suojattu IoT ratkaisu alkava määrittäminen][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md