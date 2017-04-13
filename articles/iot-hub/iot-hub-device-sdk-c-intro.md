<properties
    pageTitle="Azure IoT laitteen SDK käyttäminen c | Microsoft Azure"
    description="Tietoja ja Azure IoT laitteen SDK mallikoodi, C. käytön aloittaminen"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Esittely Azure IoT laitteen SDK c

**Azure IoT laitteen SDK** on suunniteltu voidaan yksinkertaistaa lähettämisen tapahtuma-ja **Azure IoT keskittimeen** palvelun vastaanottamisessa sähköpostiviestejä kirjastotyypit. On eri muunnoksia SDK-paketissa, kohdistamisen tietyn alustan, mutta tässä artikkelissa kuvataan **Azure IoT laitteen SDK c**.

C Azure IoT laitteen SDK kirjoitetaan ANSI C (C99), voit suurentaa siirrettävyys. Tämä on hyvin kyseiseen toimimaan määrä alustojen ja -laitteet, erityisesti jos pienentäminen levyn ja muistin on prioriteetti.  

On laajan valikoiman ympäristöjen, joina SDK on testattu (katso [Azure sertifioitu IoT laitteen luettelon](https://catalog.azureiotsuite.com/) lisätietoja). Vaikka tämä artikkeli sisältää vaiheittaiset ohjeet otoksen koodin Windows-ympäristössä, ota huomioon, että koodi, joka on kuvattu tämän artikkelin on täysin sama tuetut käyttöympäristöt alueen yli.

Tässä artikkelissa sinun otettava käyttöön Azure IoT laitteen SDK arkkitehtuuri, c-näppäinyhdistelmää. Seuraavassa esimerkissä kuvattu voit alustaa laitteen-kirjastoa, Lähetä tapahtumien IoT keskittimeen sekä vastaanottaa viestejä. Tämän artikkelin tiedot on oltava riittävän käyttäminen SDK, mutta on myös eräänlainen kirjastoja koskeviin lisätietoihin.

## <a name="sdk-architecture"></a>SDK-arkkitehtuuri

Voit etsiä **Azure IoT laitteen SDK C** [Microsoft Azure IoT SDK: T](https://github.com/Azure/azure-iot-sdks) GitHub säilössä ja tarkastella tietoja API [C API viittaus](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Kirjastot uusimman version löytyy tätä säilöä **perusmuodon** haaran:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Tämä säilö sisältää koko perheen Azure IoT laitteen SDK: T. Tässä artikkelissa on kuitenkin Azure IoT laitteen SDK *c* joka löytyy **c** -kansio.

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* SDK core käyttöönoton löytyy **iothub\_asiakkaan** kansio, joka sisältää SDK alin API taso soveltaminen: **IoTHubClient** -kirjasto. **IoTHubClient** -kirjasto sisältää API käyttöönoton raaka messaging viestien lähettäminen IoT keskittimeen sekä vastaanottaa viestejä siitä. Tämä kirjasto käytettäessä olet toteuttamisesta viestin Sarjatoiminto (joko tekstin Sarjatoiminto otosten jäljempänä), mutta muita tietoja IoT keskittimeen yhteydenpito käsitellään puolestasi.
* **Sarjatoiminto** -kansio sisältää funktioita avustaja ja näytteiden esittävä onnistu tietoja ennen kuin lähetät Azure IoT keskittimeen asiakas-kirjasto. Huomaa, että Sarjatoiminto käyttö ei ole pakollinen ja vain käytön helpottamiseksi. Jos käytät **Sarjatoiminto** kirjastossa, voit aloittaa määrittämällä mallin, joka määrittää tapahtumat, joista haluat lähettää IoT keskittimeen sekä odotat sitä vastaanottaa viestejä. Kun mallin on määritetty, SDK sisältää API-pinta, jonka avulla voit käsitellä helposti tapahtumien ja viestien eikä sinun tarvitse huolehtia Sarjatoiminto tiedot.
Kirjaston määräytyy muita Avaa lähde-kirjastoja, jotka toteuttavat useita protokollia (MQTT, AMQP) käyttämällä transport.
* **IoTHubClient** -kirjaston määräytyy muun Avaa lähde-kirjaston:
   * [Azure C jaettu apuohjelman](https://github.com/Azure/azure-c-shared-utility) kirjaston joka tarjoaa yleisiä toimintoja perustoiminnot (merkkijono, luettelon käytöstä IO, kuten jne...) tarvitaan yli useita Azure liittyvät C SDK: T
   * [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) kirjastoon, joka on optimoitu resurssin rajoituksen laitteiden AMQP asiakkaan puoli toteutus.
   * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) kirjastoon, joka on yleinen kirjaston käyttöönoton MQTT-protokollan ja optimoitu resurssin rajoituksen laitteet.

Nämä on helpompi hahmottaa katsomalla esimerkkikoodi. Seuraavissa osissa käy läpi muutama Esimerkki sovellukset, jotka sisältyvät SDK: ssa. Tämä tulisi ilmoittaa sinulle hyvä ilmeen SDK arkkitehtuuri kerrokset sekä esittely API käyttämisestä eri ominaisuuksia varten.

## <a name="before-running-the-samples"></a>Ennen kuin suoritat mallit

Ennen kuin voit suorittaa mallit Azure IoT laitteen SDK c on luotava erillisen palvelun Azure-tilauksen, jos et jo on yksi ja 2 tehtävien suorittamiseen:
* Paikallinen ympäristö valmistellaan kehittäminen
* Hanki laitteen tunnistetiedot.

Jos haluat luoda Azure IoT keskittimeen esiintymän Azure-tilauksen, noudata [seuraavassa](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

Sisältyvä SDK: N [Lueminut-tiedosto](https://github.com/Azure/azure-iot-sdks/tree/master/c) sisältää ohjeet kehittäminen ympäristön valmisteleminen ja hanki laitteen tunnistetiedot.
Seuraavissa osissa sisältää joitakin muita kommentteja näiden ohjeiden mukaisesti.

### <a name="preparing-your-development-environment"></a>Kehittäminen ympäristön valmisteleminen

Kun pakettien annetaan joitakin ympäristöjen (esimerkiksi NuGet for Windows tai apt_get Debian ja Ubuntu) ja mallit käyttää näitä paketteja, kun niitä on saatavilla, jäljempänä annettuja ohjeita yksityiskohtainen muodostaminen kirjasto ja mallit lomakkeen suoraan koodi.

Sinun on ensin hankkimaan SDK: n GitHub ja luo sitten lähde. Hanki [GitHub säilöön](https://github.com/Azure/azure-iot-sdks) **perusmuodon** päätasolta kopion lähde.

Kun olet ladannut kopion lähteeseen, sinun on suoritettava SDK-artikkelin ["Kehittäminen ympäristön valmisteleminen"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md)kuvatut vaiheet.


Seuraavassa on muutama vihje, joiden avulla voit suorittaa kuvatulla tavalla valmistelu-opas:

-   Kun asennat **CMake** -apuohjelma, valitse lisätä **CMake** järjestelmään POLKU ( **nykyisen käyttäjän** toimii myös lisääminen) **kaikille käyttäjille** :

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Ennen kuin avaat **VS2015 Developer komentorivi-ikkuna**, asenna Git komentorivin työkalut. Asenna nämä työkalut, toimimalla seuraavasti:

    1. Käynnistä **Visual Studio 2015** asennusohjelma (tai valitse **Microsoft Visual Studio 2015** Ohjauspaneelin **Ohjelmat ja toiminnot** ja valitse **Muuta**).
    
    2. Varmista, että **Git for Windows** -ominaisuus on valittuna asennusohjelman, mutta voit myös tarkistaa **Visual Studio GitHub-laajennus** -vaihtoehto, jos haluat säätää IDE integrointi:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Asenna työkalut ohjatun määritystoiminnon suorittaminen

    4. Lisää järjestelmän **PATH** -ympäristömuuttuja Git Työkalut **bin** -kansio. Windows-Tämä näyttää seuraavalta:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Kun olet suorittanut kaikki ["Kehittäminen ympäristön valmisteleminen"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) -sivulla kuvatut vaiheet, voit ryhtyä Käännä Mallisovellukset.

### <a name="obtaining-device-credentials"></a>Laitteen tunnistetiedot hankkiminen

Kun kehittäminen-ympäristösi on määritetty, voit tehdä seuraava asia on saat laitteen tunnistetietojoukko.  Laitteen, voit käyttää IoT-toiminnossa Lisää ensin laitteen IoT keskittimeen laitteen rekisteriin. Kun lisäät laitteen saat joukon laitteen tunnistetiedot, jotka sinun on myös laite on pystyttävä muodostamaan yhteys IoT-toiminnossa. Esimerkki sovellukset, jotka seuraavan osion tarkastellaan odottavat näitä tunnistetietoja **laitteen yhteysmerkkijono**-lomakkeessa.

On muutama työkalujen SDK Avaa lähde säilöön, jotta hallinta IoT-toiminnossa. Yksi on Windows-nimisen laitteen Resurssienhallinnassa toinen sovelluksen perustuu node.js cross platform CLI työkalun iothub explorer. Voit lukea lisää työkaluja [tähän](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Kun tarkastellaan kautta mallit käytössä tämän artikkelin Windows-laitteen Explorer-työkalu on käytössä. Mutta voit myös käyttää iothub explorer halutessasi CLI työkalut.

[Laitteen Explorer](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) -työkalu käyttää Azure IoT palvelun kirjastojen suorittamaan erilaisia toimintoja lisäämällä laitteiden IoT-toiminnossa. Jos käytät laitetta Explorer laitteen lisääminen, saat vastaavan yhteysmerkkijonon. Sinun on tämän yhteysmerkkijonon tehdä otosten sovellukset suoritetaan.

Siltä varalta, että et ole vielä tutustunut prosessi, seuraavat toimet käsitellään laitteen lisääminen ja hanki laitteen yhteysmerkkijonon laitteen Resurssienhallinnan avulla.

Löydät Windows installer- [SDK Vapauta sivun](https://github.com/Azure/azure-iot-sdks/releases)laitteen Explorer-työkalun. Mutta voit myös suorittamalla työkalu suoraan sen koodin ratkaisun kehittämistä sekä **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** avaaminen **Visual Studio 2015** .

Kun käynnistät ohjelman näet tämän liittymän:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Kirjoita **IoT keskittimeen yhteysmerkkijonon** ensimmäiseen kenttään ja valitse sitten **Päivitä**. Työkalu määrittää niin, että se voi olla yhteydessä IoT toiminnossa.

Kun IoT keskittimeen yhteysmerkkijonon on määritetty Valitse **hallinta** -välilehti:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Tämä on kohtaa, johon voit hallita rekisteröity IoT-toiminnossa laitteet.

Voit luoda laitteen napsauttamalla **Luo** -painiketta. Valintaikkuna tulee näkyviin niin, että täyttää näppäinyhdistelmien (ensisijainen ja toissijainen). Sinun on tehtävä on **Laitteen tunnus** ja valitse sitten **Luo**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Kun laite on luotu, rekisteröity laitteilla, myös juuri luomasi päivittyy laitteet-luettelosta. Jos uusi laitteesi hiiren kakkospainikkeella, näkyviin tulee valikossa:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Jos valitset **kopioida yhteysmerkkijonon valitun laitteen** -vaihtoehdon, yhteysmerkkijonon laitteeseesi Kopioi Leikepöydälle. Säilyttää kopion yhteysmerkkijono. Tarvitset sitä suoritettaessa seuraavat osiot on kuvattu otoksen sovellukset.

Kun olet suorittanut edellä kuvatut vaiheet, olet valmis aloittamaan joitakin koodin suorittaminen. Sekä näytteiden on vakio, jonka avulla voit antaa yhteysmerkkijonon tärkeimmät lähdetiedosto yläreunassa. Esimerkiksi vastaavan rivin kohteesta **iothub\_asiakkaan\_otoksen\_amqp** sovellus näkyy seuraavasti.

```
static const char* connectionString = "[device connection string]";
```

Jos haluat seurata, kirjoita laitteen yhteysmerkkijono tähän ja käännä ratkaisun ja pystyt mallin suorittamiseen.

## <a name="iothubclient"></a>IoTHubClient

Sisällä **iothub\_asiakkaan** kansion azure-iot-SDK: t-säilössä on **Mallit** -kansio, joka sisältää sovelluksen nimeltä **iothub\_asiakkaan\_otoksen\_amqp**.

Windows-version **iothub\_asiakkaan\_otoksen\_ampq** sovelluksen sisältää Visual Studio-ratkaisuun:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Tämä ratkaisu sisältää yhden projektin. On otettava huomioon, että on neljä NuGet paketteja, jotka on asennettu tätä ratkaisua:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Tarvitset **Microsoft.Azure.C.SharedUtility** paketin aina, kun käsittelet SDK. Koska tässä esimerkissä on riippuvainen AMQP, sinun on lisättävä myös **Microsoft.Azure.uamqp** ja **Microsoft.Azure.IoTHub.AmqpTransport** -pakettien (ei vastaa pakettien MQTT ja HTTP). Koska otosten käyttää **IoTHubClient** -kirjastoon, ratkaisu täytyy kuulua **Microsoft.Azure.IoTHub.IoTHubClient** -paketti.

Voit etsiä otoksen sovelluksen käyttöönotto **iothub\_asiakkaan\_otoksen\_amqp.c** lähdetiedosto.

Käytämme tämän sovelluksen malli opastusta mikä on pakollinen **IoTHubClient** -kirjasto.

### <a name="initializing-the-library"></a>Kirjaston alustaminen

> [AZURE.NOTE] Ennen kuin alat kirjastojen kanssa, joudut ehkä suorittamaan joitakin ympäristö tietyn alustus. Jos aiot käyttää AMQP Linux sinun on esimerkiksi alusta OpenSSL kirjaston. Mallit [GitHub säilöön](https://github.com/Azure/azure-iot-sdks) Soita apuohjelman funktio **platform_init** asiakkaan käynnistyessä ja kutsua ennen kuin suljet **platform_deinit** -funktiota. Näiden funktioiden ilmoitetaan "platform.h" otsikko-tiedostossa. Näiden funktioiden määritelmät olisi tarkasteltava kohde-ympäristön [säilöön](https://github.com/Azure/azure-iot-sdks) , voit selvittää haluat sisällyttää asiakkaan ympäristö alustus koodia.

Aloita käyttäminen kirjastoissa on ensin jaettava IoT keskittimeen-asiakasohjelman kahvaa:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Huomaa, että olemme olet kulkeva kopion Tutustu laitteen yhteysmerkkijonon toiminto (se on saatu laitteen Explorer). On myös nimettävä protokolla, jota haluat käyttää. Tässä esimerkissä käytetään AMQP, mutta MQTT ja HTTP ovat myös haluamasi vaihtoehto.

Jos sinulla on kelvollinen **IOTHUB\_asiakkaan\_käsitellä**, voit aloittaa kutsumista ohjelmointirajapinnan tapahtumien lähettää ja vastaanottaa viestejä IoT-toiminnosta. Tarkastellaan, että seuraava.

### <a name="sending-events"></a>Tapahtumien lähettäminen

Lähettäessään IoT pääsivuston tapahtumia on tehtävä seuraavat toimet:

Luo viestin:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Seuraavaksi Lähetä viesti:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Aina, kun lähetät viestin, voit määrittää takaisinsoitto-funktio, joka käynnistyy, kun tiedot lähetetään viittaus:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Huomaa, että puhelu **IoTHubMessage\_Destroy** toimi, kun enää tarvitse viestin. Sinun on tehtävä tämä kutsu vapauttaminen resurssit varataan, kun loit viesti.

### <a name="receiving-messages"></a>Viestien vastaanottaminen

Viestien on asynkroninen toiminto. Ensin sinun rekisteröitävä takaisinkutsun, kun laite vastaanottaa viestin:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Viimeinen parametri on tyhjä osoitin haluat sijoittaa. Otoksen se on kokonaisluku osoitin mutta ehkä monimutkaisia tietorakenne osoitin. Näin takaisinkutsutoiminto toimimaan jaettua tilaa tämä funktio soittajan kanssa.

Kun laite vastaanottaa viestin, rekisteröity takaisinkutsutoiminto käynnistetään:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Huomaa, että käytät **IoTHubMessage\_GetByteArray** noutaa sanoman, joka tässä esimerkissä on merkkijono-funktiota.

### <a name="uninitializing-the-library"></a>Kirjaston uninitializing

Kun olet valmis lähettämisen tapahtuma- ja vastaanottamisessa viestejä, voit uninitialize IoT kirjastoon. Voit tehdä annettava seuraavat funktiokutsun:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Tämä vapauttaa aiemmin Varaa resurssit **IoTHubClient\_CreateFromConnectionString** funktio.

Kuten näet, on helppo tapahtumien lähettää ja vastaanottaa viestejä **IoTHubClient** -kirjaston kanssa. Kirjaston käsittelee tietoja IoT keskittimeen, mukaan lukien protokollan, jota käytetään yhteydenpito (kehittäjän kannalta tämä on yksinkertainen määritysvaihtoehto).

**IoTHubClient** kirjasto sisältää myös tarkka päättää, miten voit muuntaa sarjaksi laitteen lähettää IoT pääsivuston tapahtumia. Joissakin tapauksissa tästä on hyötyä, mutta joskus tämä on käyttöönotto-tiedot, joiden et halua asianomaisille. Jos näin on, harkitse **Sarjatoiminto** -kirjastoon, joka kuvattu teksti seuraavan osion avulla.

## <a name="serializer"></a>Sarjatoiminto

**Sarjatoiminto** kirjasto sijaitsee käsitteellisesti SDK **IoTHubClient** kirjastossa päälle. Tiedostossa käytetään **IoTHubClient** kirjaston IoT keskittimeen pohjana tietoliikenteen, mutta se Lisää mallinnus ominaisuuksia, joka esittää viestin Sarjatoiminto myyminen poistaminen kehittäjä. Esimerkki siitä, miten tämä kirjasto toimii parhaiten osoituksena.

**Sarjatoiminto** azure-iot-SDK: t-säilössä kansio on **Mallit** -kansio, joka sisältää sovelluksen nimeltä **simplesample\_amqp**. Tässä esimerkissä Windows-versio sisältää Visual Studio-ratkaisuun:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Samalla tavalla kuin edellisessä esimerkissä tämän saman sisältää useita NuGet paketteja:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Olemme katsomista Useimmat näistä edellisessä esimerkissä, mutta **Microsoft.Azure.IoTHub.Serializer** uudet ominaisuudet. Tämä on pakollinen Käytämme **Sarjatoiminto** kirjastoon.

Voit etsiä otoksen sovelluksen käyttöönotto **simplesample\_amqp.c** tiedosto.

Seuraavissa osissa käy läpi tässä esimerkissä tärkeisiin osiin.

### <a name="initializing-the-library"></a>Kirjaston alustaminen

Aloita **Sarjatoiminto** kirjastoa, sinun on kutsuttava alustus API:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Kutsu **Sarjatoiminto\_alusta** -funktio on erikseen puhelu ja käytetään alustaa pohjana kirjastoa. Valitse sitten Soita **IoTHubClient\_CreateFromConnectionString** -funktiota, joka on saman Ohjelmointirajapinnan kuin **IoTHubClient** otosten. Puhelun asettaa laitteen yhteysmerkkijonon (tämä on myös josta voit valita protokolla haluat käyttää). Huomaa, että tässä esimerkissä käytetään AMQP kuljetus, mutta voitu käyttää HTTP.

Lopuksi Soita **Luo\_mallin\_ESIINTYMÄN** funktio. Huomaa, että **WeatherStation** on mallin nimitilan **ContosoAnemometer** on mallin nimi. Kun mallin esiintymää on luotu, voit sen lähettämään tapahtumien ja vastaanottaa viestejä. On kuitenkin tärkeää ymmärtää, mitä malli on.

### <a name="defining-the-model"></a>Mallin määrittäminen

**Sarjatoiminto** kirjastoon mallin määrittää laitteen lähettää sen IoT keskittimeen ja sanomat, eli *Toiminnot* mallinnus kieltä, joka voi vastaanottaa tapahtumat. Voit määrittää käyttämällä C makrot, kuten mallin **simplesample\_amqp** Esimerkki sovelluksen:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

**ALOITTAA\_NIMITILAN** ja **END\_NIMITILAN** makrot sekä ottaa mallin argumenttina nimitilan. Oletetaan, että mitään makrot välillä on määritelmä oman mallit ja rakenteet, jotka käyttävät mallit.

Tässä esimerkissä on yksi objektimalli **ContosoAnemometer**. Tämän mallin määrittää kaksi tapahtumaa laitteen lähettää sen IoT-toiminnossa: **DeviceId** ja **tuulen nopeus**. Se määrittää kolme toiminnot (viestit), joka voi vastaanottaa laitteen: **TurnFanOn**, **TurnFanOff**ja **SetAirResistance**. Kuhunkin tapahtumaan on jo tyyppi ja kutakin on nimi (ja halutessasi joukko parametreja).

Tapahtuma- ja toiminnot, jotka perustuvat malliin, Määritä API-pinta, joiden avulla voit lähettää tapahtumat IoT keskittimeen sekä vastata viestit on lähetetty laitteeseen. Esimerkki – parhaiten selkeän.

### <a name="sending-events"></a>Tapahtumien lähettäminen

Mallin määrittää tapahtumat, jotka voit lähettää IoT toiminnossa. Tässä esimerkissä siis yksi kaksi tapahtumaa määritetty **WITH_DATA** makron avulla. Esimerkiksi jos haluat lähettää **tuulen nopeus** -tapahtuman IoT-keskittimeen, sinun on muutama vaihe liittyvät tämä tapahtuu. Ensimmäinen on käytettävä Lähetä tiedot:

```
myWeather->WindSpeed = 15;
```

Mallin määritimme aiemmin pystyy toiminto määrittämällä **struct**jäsen. Seuraavaksi on onnistu tapahtuma on haluat lähettää:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Koodin serializes tapahtuman puskurin (viittaa **kohteeseen**). Lopuksi lähetämme IoT keskittimeen tämän koodin tapahtuman:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Tämä on avustaja-funktio, joka lähettää Microsoftin sarjoitettu tapahtuman IoT keskittimeen malli-sovelluksessa:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Koodi on hyvin samantyyppinen olemme tuli **iothub\_asiakkaan\_otoksen\_amqp** sovellus, joka on luotu viestin DBCS-matriisi ja käyttää sitä sitten **IoTHubClient\_SendEventAsync** lähettäminen IoT toiminnossa. Tämän jälkeen on vain on vapauttaminen viestin kahvaa ja esittää tietoja puskurin on kohdistettu aiemmin sarjana.

Viimeinen parametri toiseksi **IoTHubClient\_SendEventAsync** on takaisinsoitto-funktio, jota kutsutaan, kun tiedot on lähetetty viittaus. Tässä on esimerkki takaisinkutsufunktion:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Toinen parametri on käyttäjäkonteksti; osoitin saman osoitin on välitetty **IoTHubClient\_SendEventAsync**. Tässä tapauksessa yhteydessä on yksinkertainen Laskin, mutta se voi olla mitä tahansa.

Tämä on kaikki tapahtumat lähettämiseen on. Vasen peittää riittää ohjeilla voi vastaanottaa viestejä.

### <a name="receiving-messages"></a>Viestien vastaanottaminen

Toimii samalla tavalla viesteihin käyttäminen **IoTHubClient** kirjaston viesti vastaanotetaan. Ensin sinun rekisteröitävä viestin takaisinkutsutoiminto:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Sitten voit kirjoittaa takaisinkutsutoiminto, joka suoritetaan, kun viesti on vastaanotettu:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Tämä koodi asiakirjan osien uudelleen käyttäminen – se on sama minkä tahansa ratkaisu. Tämä funktio saa viestin ja kestää hoitamaan reitittämisestä sopivan funktion puhelun kautta **suoritus\_komento**. Funktion nimi määräytyy tässä vaiheessa Microsoftin mallin toiminnot määritys.

Kun määrität toiminnon mallin, sinua pyydetään toteuttamisesta funktioon, jota kutsutaan, kun laite vastaanottaa vastaavan viestin. Esimerkki: mallin määrittää toiminto:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Sinun on määritettävä allekirjoituksessa funktiota:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Huomaa, että funktion nimi vastaa toiminnon mallin nimi ja funktion parametrit vastaavat määritettyä toiminnon parametrit. Ensimmäinen parametri on aina pakollinen ja sisältää osoittimen Microsoftin mallin esiintymään.

Kun laite vastaanottaa viesti, joka vastaa allekirjoitusta, kutsutaan vastaavan toiminnon. Lisäksi on sisällytettävien **IoTHubMessage**asiakirjan osien uudelleen käyttäminen koodin, vastaanottaa viestejä siis vain ohjeisiin ja määrittämällä yksinkertaisia käänteisfunktio kunkin mallin määritetty toiminto.

### <a name="uninitializing-the-library"></a>Kirjaston uninitializing

Kun olet valmis tietojen lähettämisen ja vastaanottamisen viestien, voit uninitialize IoT-kirjastoon:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Nämä kolme Funktiot Tasaa aiemmin on kuvattu kolme alustus-funktiota. Soittavan nämä API varmistaa vapaa aiemmin varatut resurssit.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa kattaa **Azure IoT laitteen SDK C**-kirjastojen käytön perusteet. Se määrittämiä tarpeeksi tietoa selvittääksesi, mitkä tuotteet sisältyvät SDK-paketissa, sen arkkitehtuuri ja pikaviestien käyttäminen Windows-mallit. Seuraava artikkeli säilyy SDK kuvaus kuvausten [Lisätietoja IoTHubClient-kirjasto](iot-hub-device-sdk-c-iothubclient.md).

Lisätietoja kehittäminen IoT toiminnosta on artikkelissa [IoT keskittimeen SDK: T][lnk-sdks].

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
