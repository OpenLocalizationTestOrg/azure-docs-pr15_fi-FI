<properties
    pageTitle="Azure IoT laitteen SDK C - Sarjatoiminto | Microsoft Azure"
    description="Sarjatoiminto kirjaston Azure IoT laitteen SDK käyttämisestä c"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT laitteen SDK c – lisätietoja Sarjatoiminto

[Ensimmäinen artikkelissa](iot-hub-device-sdk-c-intro.md) sarjassa käyttöön **Azure IoT laitteen SDK C-näppäinyhdistelmää**. Seuraava artikkeli annettu [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md)tarkempi kuvaus. Tässä artikkelissa on valmis SDK soveltamisala antamalla yksityiskohtainen kuvaus jäljellä olevan osan: **Sarjatoiminto** kirjastoon.

Johdanto artikkelin ohjeiden käyttämisestä **Sarjatoiminto** kirjaston tapahtumien lähettää ja vastaanottaa viestejä IoT-toiminnosta. Tässä artikkelissa on laajentaa keskustelun antamalla lisätietoja selvitys siitä, miten mallitietoihin **Sarjatoiminto** makro-kielellä. Artikkelin myös on lisätietoja siitä, miten kirjasto serializes viestit (ja joissakin tapauksissa voit hallita Sarjatoiminto toiminnan). Kerromme myös joitakin parametreja muokkaamista, voit luoda malleja koon määrittäminen.

Lopuksi on artikkelissa revisits joitakin aiheita edellisen artikkeleita, kuten viestin ja ominaisuuden käsittely. Kun olemme tietää, nämä ominaisuudet toimivat **Sarjatoiminto** -kirjaston käytön tavalla kuin **IoTHubClient** -kirjaston kanssa samalla tavalla.

Kaikki tässä artikkelissa kuvattuja perustuu **Sarjatoiminto** SDK mallit. Jos haluat seurata, katso **simplesample\_amqp** ja **simplesample\_http** sovellukset sisältyvät Azure IoT laitteen SDK C.

Voit etsiä **Azure IoT laitteen SDK C** [Microsoft Azure IoT SDK: T](https://github.com/Azure/azure-iot-sdks) GitHub säilössä ja tarkastella tietoja API [C API viittaus](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>Kielen mallintaminen

[Johdanto artikkelissa](iot-hub-device-sdk-c-intro.md) samasta käyttöön säädetty Esimerkki kielen mallintaminen **Azure IoT laitteen SDK C** **simplesample\_amqp** sovelluksen:

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

Kuten näet, kielen mallintaminen perustuu C makrot. Aina ensin määrityksen kanssa **Aloita\_NIMITILAN** ja aina pääty **loppu\_NIMITILAN**. On yhteinen nimi nimitilan yrityksesi tai, kuten tässä esimerkissä projektista, jossa työskentelet.

Mitä sisältävät sisällä nimitilan ovat määritykset. Tässä tapauksessa on anemometer yhden malli. Uudelleen malli voidaan nimetä mitään, mutta yleensä tämä nimeltä laitetta tai tietotyypin haluat vaihtaa IoT keskittimeen kanssa.  

Mallien sisällä määritystä tilanteista sekä viestejä, näyttöön tulee IoT-toiminnosta ( *Toiminnot*) voit tunkeutumisen IoT keskittimeen ( *tiedot*). Kuten näet esimerkistä lajissa on tyyppi ja nimi. toiminnot on nimi ja valinnaisten parametrien (kunkin tyyppi).

Mitä ei esitellään tässä esimerkissä on muita tietotyyppejä, jotka tukevat SDK. Käsittelemme, että seuraava.

> [AZURE.NOTE] IoT keskittimeen viittaa laitteen lähettää siihen *tapahtumat*, kun kielen mallintaminen viittaa siihen *tietoja* (määritetty käyttämällä **WITH_DATA**) tiedot. Vastaavasti IoT keskittimeen viittaa tiedot lähetät laitteiden *viestejä*, kun kielen mallintaminen viittaa siihen *Toiminnot* (määritetty käyttämällä **WITH_ACTION**). Huomaa, että sisältö voidaan tarkoittavat näitä ehtoja.

### <a name="supported-data-types"></a>Tuetut tietotyypit

Mallit luodaan **Sarjatoiminto** -kirjaston kanssa tuetaan seuraavia tietotyyppejä:

| Tyyppi                    | Kuvaus                            |
|-------------------------|----------------------------------------|
| Kaksinkertainen                  | kaksinkertainen tarkkuus liukuluku |
| kokonaisluku                     | 32-bittisen kokonaisluvun                         |
| liukuluku                   | yksittäisen tarkkuuden liukuluku |
| pitkä                    | Pitkä kokonaisluku                           |
| int8\_t                 | 8-bittisen kokonaisluvun                          |
| Int16\_t                | 16-bittisen kokonaisluvun                         |
| Int32\_t                | 32-bittisen kokonaisluvun                         |
| Int64\_t                | 64-bittisen kokonaisluvun                         |
| bool                    | Totuusarvo                                |
| ASCII\_merkki\_ptr        | ASCII-merkkijono                           |
| EDM\_PÄIVÄMÄÄRÄN\_ajan\_siirtymä | päivämäärän ajan siirtymän                       |
| EDM\_GUID-tunnus               | GUID-TUNNUS                                   |
| EDM\_binaarinen             | binaaritiedosto                                 |
| MÄÄRITELLÄ\_STRUCT         | Monimutkaisten tietojen tyyppi                      |

Aloitetaan viimeisen tietotyyppi. **DECLARE\_STRUCT** avulla voit määrittää monimutkaisten tietotyyppien, jotka ovat muiden primitiivityyppejä ryhmiteltyjä. Nämä ryhmittelyt Salli määrittäminen mallin, joka näyttää tältä:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Tutustu mallissa on yhtä tapahtuma, jonka laji **TestType**. **TestType** on monimutkainen, joka sisältää useita jäseniä, jotka kuvaavat muunnokset kielen mallintaminen **Sarjatoiminto** tukemat primitiivityyppejä.

Mallin tältä emme voi kirjoittaa koodin tietojen lähettäminen IoT keskittimeen, joka tulee näkyviin seuraavasti:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Olemme olet lähinnä, arvo määritteleminen jokaisen jäsenen **testi** -rakenne ja soittaminen **SendAsync** **testi** tietojen tapahtuman lähettäminen pilveen. **SendAsync** on avustaja-funktio, joka lähettää yhtä tapahtuman IoT-toiminnossa:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Tämä funktio serializes annettu data tapahtuma ja lähettää sen IoT keskittimeen **IoTHubClient\_SendEventAsync**. Tämä on sama koodi käsitellyt edellisen artikkeleissa (**SendAsync** kapseloi logiikan kyselyjä helposti funktioon).

Yksi muiden käyttää edellisen koodissa avustaja-funktio on **GetDateTimeOffset**. Tämä funktio muuntaa tietyn ajan arvo, jonka tyyppi **EDM\_PÄIVÄMÄÄRÄN\_ajan\_siirtymä**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Jos suoritat koodi, IoT keskittimeen lähetetään seuraavan sanoman:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Huomaa, että Sarjatoiminto ei JSON, eli **Sarjatoiminto** kirjaston luoma muoto. Huomaa, että kunkin jäsenen sarjoitettu JSON objektin vastaa **TestType** , jonka määritimme Microsoftin mallin jäsenet. Arvot vastaavat täsmälleen myös käyttää koodissa. Huomaa kuitenkin, että binaaritietoja on base64-koodattu: "AQID" on base64 koodausta {0x01, 0x02, 0x03}.

Tässä esimerkissä näytetään **Sarjatoiminto** kirjaston käytön etuna--us JSON lähettäminen pilvipalveluun, eikä sinun tarvitse erikseen käsitellä Sarjatoiminto tämän sovelluksen avulla. Meidän huolehtia on päivämääräkentän tietojen tapahtumat arvot Microsoftin mallin ja soittaminen yksinkertainen ohjelmointirajapinnan lähettämään tapahtumat pilveen.

Nämä ohjeet emme voi määrittää mallit, jotka sisältävät solualue, mukaan lukien monimutkaisia (emme voi myös sisältää monimutkaisia tyyppejä monimutkaisia muuntyyppisten sisällä) tuetut tietotyypit. Kuitenkin hän esittää sarjana JSON luoma edellä olevassa esimerkissä näyttää tärkeää. *Miten* **Sarjatoiminto** kirjaston tietojen lähettämistä määrittää tarkasti, kuinka JSON on muodostettu. Kyseisellä on mitä käsittelemme Seuraava.

## <a name="more-about-serialization"></a>Lisätietoja Sarjatoiminto

Edellisen osan korostaa esimerkki **Sarjatoiminto** kirjaston tuottamat tulosteet. Tässä osassa kerrotaan kuinka kirjasto serializes tiedot ja kuinka voidaan hallita sitä, toiminta Sarjatoiminto ohjelmointirajapinnan käyttäminen.

Jotta voit vaihtaa Sarjatoiminto keskusteluun, emme käsitellä termostaatin perustuvan uuden mallin. Ensin antaa oletetaan, että jotkin tausta on yrittää osoite skenaario.

Haluat malli, joka mittaa lämpötilan ja kosteuden termostaatin. Jokainen tieto suorittaminen voidaan lähettää IoT keskittimeen eri tavalla. Oletusarvoisesti termostaatin ingresses lämpötilan tapahtumasta kerran 2 minuuttia. kosteus tapahtuma on ingressed 15 minuutin välein. Kun joko tapahtuma on ingressed, se täytyy sisältää aikaleiman, joka näyttää ajan, että vastaavan lämpötilan tai kosteus mitattuna.

Valita tässä tilanteessa, on kuvattu tiedot voidaan mallintaa kahdella eri tavalla ja tehosteen kerrotaan, että mallinnus on sarjoitettu tulos.

### <a name="model-1"></a>Malli 1

Näin ensimmäinen versio, malli, joka tukee edellisen skenaariota:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Huomaa, että mallin tiedot kaksi tapahtumaa: **lämpötilan** ja **kosteuden**. Toisin kuin edellisessä esimerkissä jokaisen tapahtuman tyyppi on määritetty rakenne **DECLARE\_STRUCT**. **TemperatureEvent** sisältää lämpötilan mitta ja aikaleiman; **HumidityEvent** sisältää kosteus mitta ja aikaleiman. Tämän mallin kautta us luonnollinen kaikki mallitietoihin kuvattuun skenaarion. Kun lähetämme tapahtuman pilveen, joko lähetämme lämpötilan/aikaleima tai kosteus/aikaleima pari.

Lämpötila tapahtuman voidaan lähettää pilvipalveluun käyttämällä esimerkiksi seuraavasti:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

On nyt käyttää koodattu arvona lämpötilan ja kosteuden otoksen koodissa, mutta Kuvitellaan, että olemme on todella haetaan nämä arvot ottamalla käyttöön termostaatin vastaavan anturit.

Yllä olevan koodin käyttää **GetDateTimeOffset** apuohjelma, joka otettiin aiemmin. Syistä, joka tulee Poista myöhemmin koodi erottaa erikseen tehtävän sarjoitettaessa ja lähettämisestä tapahtuma. Edellinen koodi serializes lämpötilan tapahtuman puskurin kyselyjä. Valitse **sendMessage** on avustaja-funktio (sisältyy **simplesample\_amqp**), joka lähettää tapahtuman IoT-toiminnossa:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Koodi on kuvattu edellisessä osassa, niin että eivät mene päälle uudelleen **SendAsync** helper alijoukkoa.

Edellinen koodi lähettää lämpötilan tapahtuman suoritettaessa on tämän sarjoitettu tapahtuman lähetetään IoT-toiminnossa:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Olemme haluat lähettää lämpötilan tyypin **TemperatureEvent** eli ja kyseisen struct sisältää **lämpötilan** ja **ajan** jäsen. Tämä näkyy suoraan sarjoitettu tiedot.

Emme voi lähettää kosteus-tapahtumaa, jonka koodi:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sarjoitettu lomake, joka lähetetään IoT keskittimeen tulee näkyviin seuraavasti:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Tämä on kuitenkin odotetulla tavalla.

Tämän mallin avulla voit kuvitella miten muita tapahtumia helposti voidaan lisätä. Voit määrittää lisää rakenteet käyttämällä **DECLARE\_STRUCT**, ja Sisällytä vastaavan tapahtuman mallin avulla **WITH\_tietojen**.

Nyt muokata mallia nyt, niin, että se sisältää samat tiedot mutta erilaisia rakenne.

### <a name="model-2"></a>Mallin 2

Tutustu tämän vaihtoehtoinen mallin edellä kuvattua:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Tässä tapauksessa emme olet poistanut **DECLARE\_STRUCT** makrot ja määritetään vain Microsoftin skenaarion käyttämällä yksinkertaista tiedostotyyppien kielen mallintaminen tieto-osat.

Vain tällä hetkellä, Ohita japanin **aika** tapahtuma. Kanssa, kesantoala näin tunkeutumisen **lämpötilan**koodi:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tämä koodi lähettää seuraava sarjoitettu tapahtuma IoT-toiminnossa:

```
{"Temperature":75}
```

Ja lähettäminen kosteus tapahtuman koodi näkyy seuraavasti:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tämä koodi lähettää tämä IoT-toiminnossa:

```
{"Humidity":45}
```

Tähän mennessä ei vielä ole paljon. Nyt muuttaa japanin kuinka Käytämme SERIALIZE makro.

**SERIALIZE** makron voi kestää useita tietojen tapahtumien argumentteina. Ottaa käyttöön us onnistu **lämpötilan** ja **kosteuden** tapahtuman yhdessä ja lähetä ne IoT keskittimeen yhden käynnissä:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Voi arvata, että koodi tulos on kaksi tapahtumat lähetetään IoT-toiminnossa:

[

{"Lämpötilan": 75},

{"Kosteus": 45}

]

Toisin sanoen voisi olettaa, että koodi on sama kuin lähettäminen **lämpötilan** ja **kosteuden** erikseen. Molempien tapahtumien siirtää **SERIALIZE** samaan kutsuun helppokäyttöisyys on. Joka ei kuitenkaan kirjainkokoa. Yllä olevan koodin lähettää sen sijaan yhtä tapahtuman IoT-toiminnossa:

{"Lämpötilan": 75, "kosteus": 45}

Tämä saattaa näyttää outo, koska Microsoftin mallin määrittää **lämpötilan** ja **kosteuden** kahta *erillistä* tapahtumia:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Lisää pisteeseen, emme ei ole malli näitä tapahtumia, on sama rakenne **lämpötilan** ja **kosteuden** sijainti:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Jos on käytetty tätä mallia, se olisi helpompi tarkastella, kuinka **lämpötilan** ja **kosteuden** lähetettäviä saman sarjoitettu viestissä. Mutta se ei ehkä Tyhjennä miksi se toimii tällä tavalla voit välittää tietoja sekä tapahtumien **SERIALIZE** käyttämällä mallin 2.

Tämä on helpompi hahmottaa, jos tiedät, **Sarjatoiminto** kirjaston tekeminen perustiedot. Tämä selventämään mennään takaisin tämän mallin:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Tämän mallin ajatella objektipohjaisten ehdot. Tässä tapauksessa emme ole mallinnus fyysinen laite (termostaatin) ja laitteen sisältää määritteitä, kuten **lämpötilan** ja **kosteuden**.

Emme voi lähettää koko tilan Microsoftin mallin koodilla, kuten jokin seuraavista:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Jos lämpötila arvoja, kosteus ja kellonaika on asetettu, näemme tapahtuman tältä lähetetään IoT-toiminnossa:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Joskus voi vain haluat lähettää *joitakin* ominaisuuksia mallin (tämä on erityisen TOSI, jos mallissa on paljon tietoja tapahtumien) pilveen. Se on hyödyllinen Lähetä vain tapahtumien tietojen alijoukkoa aiemman tässä esimerkissä:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Luo täsmälleen saman sarjoitettu tapahtuman aivan kuin määritimme oli **TemperatureEvent** **lämpötilan** ja **ajan** , jäsenten kanssa samalla tavalla kuin on ollut mallia 1. Tässä tapauksessa emme voi luoda täsmälleen saman sarjoitettu tapahtuman käyttämällä eri mallia (malli 2), koska on nimeltään **SERIALIZE** eri tavalla.

Tärkeitä piste on, jos useita tietojen tapahtumien välittää **SERIALIZE,** ja valitse se olettaa kuhunkin tapahtumaan on ominaisuuden JSON yhtenä objektina.

Paras tapa määräytyy sen mukaan, ja miten suunnittelet mallin. Jos haluat lähettää "tapahtumat" pilvipalveluun ja kuhunkin tapahtumaan sisältää joukosta ominaisuuksia, ensimmäisestä tavasta on järkevää paljon. Tässä tapauksessa käytetään **DECLARE\_STRUCT** määrittäminen kuhunkin tapahtumaan rakenteen ja lisätä ne sitten mallin **WITH\_tietojen** makron. Valitse Lähetä kuhunkin tapahtumaan on samoin ensimmäisen edellä olevassa esimerkissä. Tämän menetelmän vain välittää yhtä tapahtuman **SARJATOIMINTO**.

Jos suunnittelet mallin objektipohjaisten tavalla, sitten toinen vaihtoehto saattaa tarpeidesi mukaan. Tässä tapauksessa elementit määritetty käyttämällä **WITH\_tietojen** objektin "ominaisuudet". Siirtää jostakin tapahtumien alijoukkoa **SERIALIZE** , jotka haluat sen mukaan, kuinka paljon haluat lähettää pilvipalveluun "objektin 's"-tilassa.

Nether on oikealle tai virhe. Vain huomioon **Sarjatoiminto** kirjaston toiminta ja valitse mallinnus lähestymistapa, joka vastaa parhaiten tarpeitasi.

## <a name="message-handling"></a>Viestin käsittely

Tähän mennessä tässä artikkelissa on vain käsitellyt lähettämisen tapahtumien IoT keskittimeen ja ei ole osoitettu vastaanottaa viestejä. Syy on, että Haluamme tietää viestien vastaanottamiseen suurelta tasaamisessa [aiemmissa artikkelissa](iot-hub-device-sdk-c-intro.md). Peruuttaa kyseisen artikkelista Käsittele viestejä rekisteröidään viestin takaisinkutsutoiminto:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Kirjoita takaisinkutsutoiminto, joka suoritetaan, kun viesti on vastaanotettu:

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

Tämä toteutus **IoTHubMessage** kutsuu tietyn funktiota kunkin toiminnon mallin. Esimerkki: mallin määrittää toiminto:

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

**SetAirResistance** kutsutaan sitten, kun viestin lisääminen laitteeseen.

Emme ole vielä lisäämistapaa ei vielä viestin sarjoitettu versio näyttää tältä. Toisin sanoen, jos haluat lähettää viestin **SetAirResistance** laitteeseen, miltä, joka näyttää?

Jos haluat lähettää viestin laitteeseen, sinun kiellä Azure IoT SDK-palvelun kautta. Haluat tietää, mitä merkkijonon lähettäminen tietyn toiminnon käynnistäminen. Viestin lähettäminen Yleinen-muotoilu näkyy seuraavasti:

```
{"Name" : "", "Parameters" : "" }
```

Haluat lähettää sarjoitettu JSON-objekti, jossa on kaksi ominaisuudet: **nimi** on toiminnon (viesti) ja **Parametrit** sisältää toiminnon parametrit.

Käynnistää **SetAirResistance** voit esimerkiksi lähettää viestiä laitteeseen:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Toiminnon nimen on oltava täsmälleen samat mallin määritetty toiminto. Parametrinimet on vastattava paikan päällä. Huomaa myös kirjainkoko. **Nimi** ja **Parametrit** ovat aina isoja. Varmista, että sama kirjainkoko toiminnon nimi ja mallin parametrit. Tässä esimerkissä toiminnon nimi on "SetAirResistance" ja ei-setairresistance".

Tässä osassa kuvataan kaikki on tiedettävä, kun tapahtumien lähettäminen ja vastaanottaminen viestit **Sarjatoiminto** -kirjaston kanssa. Ennen siirtymistä, kansilehden oletetaan, että voit määrittää joitakin parametreja, jotka määrittävät, kuinka suuri on mallin.

## <a name="macro-configuration"></a>Makron määrittäminen

Jos käytät **Sarjatoiminto** kirjaston huomioon SDK tärkeä osa löytyy azure-c-jaettu-kirjastoa.
Jos on kopioitu Azure-iot-SDK: t-säilöön-GitHub--rekursiivinen-vaihtoehdon, saatat huomata jaetun apuohjelman kirjaston tähän:

```
.\\c\\azure-c-shared-utility
```

Jos olet ei kopioitu kirjastoon, löydät sen [tähän](https://github.com/Azure/azure-c-shared-utility).

Jaettu apuohjelman kirjastosta etsii seuraavassa kansiossa:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Tämä kansio sisältää Visual Studio-ratkaisu **makron\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Ratkaisu: Ohjelma luo **makron\_utils.h** tiedosto. On oletusarvoinen makron\_utils.h tiedoston SDK mukana. Tämä ratkaisu avulla voit muokata joitakin parametreja ja luo sitten nämä parametrit-otsikko-tiedosto.

Kaksi avaimen parametrit kiinnostaa ovat **nArithmetic** ja **nMacroParameters** , jotka on määritelty nämä kaksi riviä makro kohteessa\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Nämä arvot ovat mukana SDK oletusarvon parametrit. Kunkin parametrin on kelvollisia:

-   nMacroParameters – määrittää, kuinka monta parametrit voi olla yksi DECLARE\_mallin makroon määrityksen.

-   nArithmetic – ohjaa sallittu mallin jäsenten määrä.

Nämä kuukausiparametrit ovat tärkeitä syy on, sillä he kuinka suuri mallin voi olla. Esimerkkinä tämän mallin määritys:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Kuten edellä mainittiin, **DECLARE\_mallin** on vain C makro. Mallin nimet ja **kanssa\_tietojen** lauseen (vielä toisen makron) ovat parametrit **DECLARE\_mallin**. **nMacroParameters** määrittää, kuinka monta parametrit voidaan lisätä **DECLARE\_mallin**. Tämä määrittää tehokkaasti, kuinka monta tietoja tapahtuma- ja ilmoitusten voi olla. Näin ollen 124 oletusrajoitus kanssa tämä tarkoittaa, että voit määrittää malli, joka sisältää yhdistelmän noin 60 toiminnot ja tietojen tapahtumat. Jos yrität enintään, näyttöön tulee kääntäjän virheet, joka näyttää seuraavanlaiselta:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

**NArithmetic** -parametri on lisätietoja makron kielen sisäisiä toimintoja kuin sovelluksen.  Ne ohjaavat jäsenet, voit määrittää mallin, kuten **DECLARE_STRUCT** makrot kokonaismäärän. Jos aloitat kääntäjän virheet esimerkiksi, että se toimii, valitse Kokeile lisääntyvien **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Jos haluat muuttaa näitä parametreja, muuta makron arvot\_utils.tt tiedoston ja käännä makron\_utils\_h\_generator.sln ratkaisu ja suorita käännetty ohjelma. Kun teet näin, uusi makro\_utils.h tiedosto on luotu ja sijoittaa. \\yleisiä\\Inc. hakemisto.

Jotta voit käyttää uutta versiota, makron\_utils.h, **Sarjatoiminto** NuGet paketin poistaminen ratkaisu ja Sisällytä sijaintia **Sarjatoiminto** Visual Studio-projekti. Näin kääntää vastaan lähdekoodin Sarjatoiminto kirjaston koodi. Tämä vaihtoehto sisältää päivitettyjen makron\_utils.h. Jos haluat tehdä tämän **simplesample\_amqp**, aloita poistamalla Sarjatoiminto kirjaston NuGet pakkaaminen ratkaisusta:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Lisää tämä projekti Visual Studio-ratkaisuun:

> . \\c\\Sarjatoiminto\\luominen\\windows\\serializer.vcxproj

Kun olet valmis, ratkaisu pitäisi näyttää tältä:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nyt kun ratkaisu, päivitetyn makron kääntää\_utils.h sisältyy oman binaariluvuksi.

Huomaa, että kasvavilla arvoilla nämä riittävän voi ylittää kääntäjän rajoitukset. Tähän mennessä **nMacroParameters** on ensisijainen parametri, jonka asianomaisille. C99 määritys määrittää, että 127 parametrien pienin sallittu makroon määrityksen. Microsoft-kääntäjän seuraa määritys täsmälleen (ja 127 enimmäismäärä on), joten et voi suurentaa **nMacroParameters** lisäksi oletusarvo. Muut kääntäjät ehkä avulla voit tehdä niin (esimerkiksi GNU kääntäjä tukee korkeamman raja-arvon).

Tähän mennessä on olet kattaa lähes kaikki tarvitset lisätietoja viestin kirjoittaminen koodin **Sarjatoiminto** -kirjaston kanssa. Ennen tekemistä, japanin palata tähän määritykseen jotkin aiheet edellisen artikkeleita, mutta tietoja.

## <a name="the-lower-level-apis"></a>Alemman tason API

Esimerkkisovellus, tämän artikkelin koskivat on **simplesample\_amqp**. Tässä esimerkissä käytetään korkeamman tason (ei-"kaikki") API tapahtumien lähettää ja vastaanottaa viestejä. Jos nämä API, tausta viestiketjun suoritetaan, joka kestää varoen sekä tapahtumien lähettää ja vastaanottaa viestejä. Voit poistaa taustan säikeen ja eksplisiittisiä päättää, milloin tapahtumat lähetät tai vastaanotat viestejä pilvestä alemman tason (kaikki)-ohjelmointirajapinnan.

[Edellinen artikkeli](iot-hub-device-sdk-c-iothubclient.md)kuvattua on joukko toimintoja, joka koostuu ylemmän tason API:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_tuhoa

Nämä API ovat esitellään **simplesample\_amqp**.

On myös paperimuotoon alemman tason API joukko.

-   IoTHubClient\_li\_CreateFromConnectionString

-   IoTHubClient\_li\_SendEventAsync

-   IoTHubClient\_li\_SetMessageCallback

-   IoTHubClient\_li\_tuhoa

Huomaa, että alemman tason API toimi samalla tavalla kuin edellinen artikkeleissa. Voit käyttää ohjelmointirajapinnan ensimmäisiin halutessasi taustan säikeen käsittelemään lähettämisen tapahtuma- ja vastaanottamisessa viestejä. Voit käyttää toisen joukon API halutessasi eksplisiittisiä päättää, milloin lähettää ja vastaanottaa tietoja IoT-toiminnosta. Määritetty API työn tasaisesti sekä **Sarjatoiminto** -kirjaston kanssa.

Esimerkki alemman tason-ohjelmointirajapinnan käyttäminen **Sarjatoiminto** -kirjaston kanssa on artikkelissa **simplesample\_http** sovelluksen.

## <a name="additional-topics"></a>Muita ohjeita

Muutamia muita nykyarvo mainitseminen ohjeaiheet ovat uudelleen ominaisuuden käsittely-, Vaihtoehtoinen laitteen tunnistetiedot ja asetukset. Nämä ovat kaikki [edellisen artikkelin](iot-hub-device-sdk-c-iothubclient.md)aiheita. Pääkohdasta on, että kaikki näistä toiminnoista toimii samalla tavalla kuin **Sarjatoiminto** kirjastoa tavalla kuin **IoTHubClient** -kirjaston kanssa. Esimerkiksi jos haluat liittää tapahtuman ominaisuudet mallista, voit käyttää **IoTHubMessage\_ominaisuudet** ja **kartan**\_**AddorUpdate**, samalla tavalla kuin edellä kuvatun:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Onko tapahtuma luotu **Sarjatoiminto** kirjastosta tai luotu manuaalisesti **IoTHubClient** kirjasto ei ole merkitystä.

Vaihtoehtoinen laitteen tunnistetiedot-käyttämällä **IoTHubClient\_li\_Luo** toimii vain sekä kuin **IoTHubClient\_CreateFromConnectionString** jakamisesta **IOTHUB\_asiakkaan\_käsitellä**.

Jos käytät **Sarjatoiminto** kirjastossa, voit lopuksi määrittää asetukset ja **IoTHubClient\_li\_SetOption** samaan tapaan kuin teit käytettäessä **IoTHubClient** -kirjasto.

Ominaisuus, joka on yksilöllinen **Sarjatoiminto** kirjastoon, joka on alustus API. Ennen kuin voit aloittaa kirjastoa, sinun on kutsuttava **Sarjatoiminto\_alusta**:

```
serializer_init(NULL);
```

Tämä on valmis, ennen kuin otat yhteyttä **IoTHubClient\_CreateFromConnectionString**.

Samalla, kun olet valmis käsittelet kirjastoa, voitte viimeisen puhelun on **Sarjatoiminto\_deinit**:

```
serializer_deinit();
```

Muussa tapauksessa kaikki edellä mainitut ominaisuudet toimivat samalla tavalla **Sarjatoiminto** kirjaston tavalla kuin **IoTHubClient** -kirjastossa. Saat lisätietoja näistä aiheista, [Edellinen artikkeli](iot-hub-device-sdk-c-iothubclient.md) sarjassa.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa kerrotaan yksityiskohtaisesti **Azure IoT laitteen SDK c**sisältämien **Sarjatoiminto** kirjaston yksilöllisiä ominaisuuksia. Tietoja annettu on oltava ymmärtää mallien avulla tapahtumien lähettää ja vastaanottaa viestejä IoT-toiminnosta.

On päättynyt myös siitä, miten voit kehittää sovelluksia **Azure IoT laitteen SDK c**kolmiosaisen sarjan. Tämä olisi riittävästi tietoja paitsi pääset alkuun, mutta avulla voit perusteellisesti tietoja API toiminnasta. Lisätietoja on muutamia esimerkkejä, joiden eivät sisälly tähän SDK: ssa. Muussa tapauksessa [SDK dokumentaatio](https://github.com/Azure/azure-iot-sdks) on on hyvä resurssi lisätiedot.


Lisätietoja kehittäminen IoT toiminnosta on artikkelissa [IoT keskittimeen SDK: T][lnk-sdks].

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
