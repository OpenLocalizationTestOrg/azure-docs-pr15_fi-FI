<properties
    pageTitle="Azure IoT laitteen SDK C - IoTHubClient | Microsoft Azure"
    description="Lisätietoja Azure IoT laitteen SDK IoTHubClient-kirjaston käytön C"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT laitteen SDK c – lisätietoja IoTHubClient

[Ensimmäinen artikkelissa](iot-hub-device-sdk-c-intro.md) sarjassa käyttöön **Microsoft Azure IoT laitteen SDK C-näppäinyhdistelmää**. Tässä artikkelissa selitetään, että SDK: ssa on kaksi arkkitehtuuri kerrosta. On kantaluku **IoTHubClient** -kirjastoon, joka hallitsee suoraan tietoliikenteen IoT toiminnossa. On myös **Sarjatoiminto** kirjasto, joka muodostaa päällä oleva Sarjatoiminto tarjoamiseen. Tässä artikkelissa **IoTHubClient** -kirjastossa annamme muut tiedot.

Edellinen artikkeli on kuvattu käyttämään **IoTHubClient** kirjaston IoT pääsivuston tapahtumia Lähetä ja vastaanota viestit. Tässä artikkelissa kerrotaan tarkemmin hallinta, *Kun* Lähetä ja vastaanota tietoja, voit esittely **alemman tason API**laajentaa keskustelun. Olemme kerrotaan, miten ominaisuudet liittäminen tapahtumien (ja noutaa viestit) käyttämällä käsittely **IoTHubClient** kirjaston ominaisuudet-ominaisuutta. Lopuksi annamme muita selitys IoT-toiminnosta vastaanotetut viestit käsitellään eri tavalla.

Artikkelin esiintyvän mukaan sen päällä pari muut aiheista, mukaan lukien enemmän tietoja laitteen tunnistetiedot ja **IoTHubClient** määritysten asetukseen toiminnan muuttaminen.

On seuraavissa artikkeleissa kerrotaan **IoTHubClient** SDK mallit käytetään. Jos haluat seurata, katso **iothub\_asiakkaan\_otoksen\_http** ja **iothub\_asiakkaan\_otoksen\_amqp** sovellukset, jotka sisältyvät Azure IoT laitteen SDK, C. kaikki on kuvattu seuraavassa esitellään näitä esimerkkejä.

Voit etsiä **Azure IoT laitteen SDK C** [Microsoft Azure IoT SDK: T](https://github.com/Azure/azure-iot-sdks) GitHub säilössä ja tarkastella tietoja API [C API viittaus](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Alemman tason API

Edellinen artikkeli on kuvattu **IotHubClient** puitteissa perustoiminnot **iothub\_asiakkaan\_otoksen\_amqp** sovelluksen. Esimerkiksi kuvaus siitä, miten alustaa koodi kirjastoon.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Se myös kuvattu lähettäminen käyttämällä tämän funktiokutsun tapahtumat.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Artikkelissa on kuvattu myös voit vastaanottaa viestejä rekisteröidään takaisinkutsufunktion.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Artikkelin ilmeni myös siitä, miten vapauttaminen käyttämällä esimerkiksi seuraavia resursseja.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

On kuitenkin kunkin nämä API-funktioita avustaja:

-   IoTHubClient\_li\_CreateFromConnectionString

-   IoTHubClient\_li\_SendEventAsync

-   IoTHubClient\_li\_SetMessageCallback

-   IoTHubClient\_li\_tuhoa


Kaikkien näiden funktioiden Sisällytä "Kaikki" API nimi. Kuin, parametreja nämä funktiot ovat samat niiden li vastineiden. Näiden funktioiden toimintaa on kuitenkin eri tärkeitä tavalla.

Kun soitat **IoTHubClient\_CreateFromConnectionString**, pohjana kirjastojen Luo uuden viestiketjun, joka suorittaa taustalla. Säikeen tapahtumien lähettää ja vastaanottaa viestejä, IoT toiminnossa. Näiden viestiketjun luodaan, kun "Kaikki" ohjelmointirajapinnan käyttäminen. Taustan säikeen luominen on kehittäjän käytön helpottamiseksi. Sinun ei tarvitse huolehtia erikseen tapahtumien viestien lähettäminen ja vastaanottaminen IoT-toiminnosta se tapahtuu automaattisesti taustalla. Sen sijaan "Kaikki" ohjelmointirajapinnan antaa sinulle IoT-toiminnossa tietoliikenteen eksplisiittinen päättää, jos tarvitse sitä.

Tutustu tämän paremmin katsotaan Esimerkki:

Kun soitat **IoTHubClient\_SendEventAsync**, mitä todellisuudessa teet on puskurin sijoittaminen tapahtuma. Taustan säikeen luodaan, kun soitat **IoTHubClient\_CreateFromConnectionString** jatkuvasti valvoo tämän puskurin ja lähettää sen sisältämät tiedot IoT toiminnossa. Tämä tapahtuu taustalla tärkeimmät viestiketjun suorittaman muiden töiden samanaikaisesti.

Vastaavasti, kun olet rekisteröinyt viestiä, joissa takaisinsoitto-funktio **IoTHubClient\_SetMessageCallback**, olet neuvot on ongelma takaisinkutsutoiminto, kun viesti on vastaanotettu, riippumaton tärkeimmät säikeen taustan viestiketjun SDK.

"Kaikki" API ei tarvitse luoda taustan viestiketjun. Sen sijaan uusi Ohjelmointirajapinnan on kutsuttava erikseen lähettää ja vastaanottaa tietoja IoT-toiminnosta. Tämä on selitetty seuraavan esimerkin mukaisesti.

**Iothub\_asiakkaan\_otoksen\_http** sovellus, joka sisältyy SDK esitellään alemman tason API. Kyseisen esimerkissä lähetämme tapahtumien IoT keskittimeen koodilla, kuten jokin seuraavista:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Kolme ensimmäistä riviä Luo viesti ja viimeisen rivin lähettää tapahtuma. Kuten edellä mainittiin, "lähettäminen" tapahtuman tarkoittaa, että tiedot sijoitetaan yksinkertaisesti puskurin. Mitään lähetetään verkon, kun otamme **IoTHubClient\_li\_SendEventAsync**. Järjestyksessä todella tunkeutumisen tiedot IoT keskittimeen, sinun on kutsuttava **IoTHubClient\_li\_DoWork**, valitse tässä esimerkissä:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Koodi (kohteesta **iothub\_asiakkaan\_otoksen\_http** sovelluksen) toistuvasti soittaa **IoTHubClient\_li\_DoWork**. Aina kun **IoTHubClient\_li\_DoWork** on nimeltään, se lähettää joitakin tapahtumien puskuri IoT keskittimeen ja se hakee jonossa viesti on lähetetty laitteeseen. Jälkimmäisessä tapauksessa tarkoittaa, että jos on rekisteröity takaisinsoitto funktion viestien, valitse takaisinkutsua käynnistetään (olettaen että kaikki viestit on jonossa). Olemme rekisteröity takaisinkutsutoiminto koodilla, kuten jokin seuraavista:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Syy, **IoTHubClient\_li\_DoWork** kutsutaan usein silmukassa on aina, kun se on nimeltään, se lähettää *joitakin* ellei tapahtumat IoT keskittimeen ja noutaa *seuraavan* viestin jonossa laitteen. Jokainen kutsu ei ole taata Lähetä kaikki puskuroitua tapahtumia tai voit hakea kaikki jonossa olevat viestit. Jos haluat lähettää kaikki tapahtumat puskurin ja jatka sitten muut käsittely voit korvata tämä silmukka esimerkiksi seuraavasti:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Tämä koodi kutsuu **IoTHubClient\_li\_DoWork** , kunnes kaikki tapahtumat puskuri on lähetetty IoT toiminnossa. Huomautus Tämä myös tarkoita, että kaikki jonossa olevat viestit on vastaanotettu. Tämä osa on "kaikki" viestit tarkistamatta ei deterministic niin toimen. Mitä tapahtuu, jos haet "kaikki", jos viestit, mutta valitse toinen lähetetään laitteen heti, kun? Parempi tapa käsitellä, joka on ohjelmoitua aikakatkaisu. Viestin takaisinkutsutoiminto voi esimerkiksi nollaa ajastin aina, kun se käynnistetään. Voit kirjoittaa logiikan Jatka käsittely, jos esimerkiksi viestejä ei ole vastaanotettu viimeisen *X* sekunnin kuluttua.

Kun olet valmis ingressing tapahtumien ja vastaanottaa viestejä, muista kutsua resurssien tyhjentääksesi vastaavan-funktiota.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Lähinnä on vain yksi joukko API lähettämiseen ja vastaanottamiseen taustan viestiketjun ja eri API, joka tekee samojen ilman taustaa viestiketjun. Kehittäjät paljon halutessasi ei - li API, mutta alemman tason API on hyötyä, kun kehittäjä haluaa verkon tiedonsiirto eksplisiittisten päättää. Esimerkiksi joitakin laitteita kerää tietoja ajan ja vain tunkeutumisen tapahtumien tietyin väliajoin (esimerkiksi kerran tunti tai kerran päivässä). Alemman tason API antaa sinulle mahdollisuuden erikseen ohjausobjektiin lähettää ja vastaanottaa tietoja IoT-toiminnosta. Muiden ainoastaan on yksinkertaista, jotka tarjoavat alemman tason API. Kaikki tapahtuu tärkeimmät viestiketjun taustalla joitakin työ-asetuksen sijaan.

Voit valita, kumpi mallin muista olevan yhdenmukaisia, mitä voit käyttää API. Jos aloitat soittamalla **IoTHubClient\_li\_CreateFromConnectionString**, varmista, että käytät vain vastaavan alemman tason API seuranta työ:

-   IoTHubClient\_li\_SendEventAsync

-   IoTHubClient\_li\_SetMessageCallback

-   IoTHubClient\_li\_tuhoa

-   IoTHubClient\_li\_DoWork

On päinvastainen paikan päällä. Jos aloitat **IoTHubClient\_CreateFromConnectionString**, käytä ei - li API minkä tahansa lisäkäsittelyä varten.

Azure IoT laitteen SDK, C, katso **iothub\_asiakkaan\_otoksen\_http** sovelluksen täydellinen esimerkki alemman tason API. **Iothub\_asiakkaan\_otoksen\_amqp** sovelluksen voidaan viitata koko esimerkiksi on ei - li API.

## <a name="property-handling"></a>Ominaisuuden käsittely

Tähän mennessä kun Microsoft on kuvattu lähettävät tietoja, on olet ollut viittaaminen viestin tekstiosaan. Esimerkkinä koodi:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Tässä esimerkissä lähettää viestin IoT keskittimeen lukee "Hei". IoT keskittimeen sallii kuitenkin myös ominaisuudet liitettävä viesti. Ominaisuudet ovat nimi-arvoa, joka on liitetty viestiin parit. Emme voi muokata ominaisuus liitetään viestin Edellinen koodi:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Aluksi soittamalla **IoTHubMessage\_ominaisuudet** ja kulkeva Microsoftin viestin kahvaa. Mikä on palata on **KARTAN\_käsitellä** viittaus, jonka avulla Microsoft jatkaa lisäämällä ominaisuudet. Tehdä soittamalla **kartan\_AddOrUpdate**, joka kestää KARTAN viittaa\_KAHVA, ominaisuuden nimen ja ominaisuuden arvon. Tämä API lisäämme yhteenkuuluvien on niin monta ominaisuudet.

Tapahtuma on luettu **Tapahtuman keskittimet**, vastaanottaja voi Luetteloi ominaisuudet ja hakea niiden vastaavat arvot. Esimerkiksi .NET-tämä tehdä muodostamalla yhteyden [EventData objektin ominaisuudet-kokoelmaan](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

Edellisessä esimerkissä emme olet liittäminen ominaisuudet tapahtuman, joka IoT keskittimeen lähettämistä. Ominaisuudet voidaan liittää myös IoT-toiminnosta vastaanotetut viestit. Jos haluamme ominaisuudet hakea viestistä, olemme käyttäminen Microsoftin viestin takaisinkutsutoiminto esimerkiksi seuraavat:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Puhelun **IoTHubMessage\_ominaisuudet** palauttaa **KARTAN\_käsitellä** viittaus. Olemme Välitä sitten, että viittaus **kartan\_GetInternals** hankkiminen viittauksen matriisin nimen ja arvon tietoparin (sekä ominaisuuksien määrä). Tässä vaiheessa on yksinkertainen ohjeisiin ja luettelointi pääset haluamme arvot ominaisuudet.

Sinun ei tarvitse käyttää ominaisuudet-sovelluksessa. Jos haluat asettaa tapahtumia tai noutaa viestit **IoTHubClient** kirjasto on helppo.

## <a name="message-handling"></a>Viestin käsittely

Ilmoitetulla aiemmin viestin saapuessa IoT-toiminnosta **IoTHubClient** -kirjaston vastaa kutsumalla rekisteröity takaisinkutsutoiminto. Tämän funktion, joka on hieman muita selitystä ansainnut palauta parametria ei. Näin ote takaisinsoitto-funktion käytöstä **iothub\_asiakkaan\_otoksen\_http** Esimerkki sovelluksen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Huomaa, että palautustyyppi on **IOTHUBMESSAGE\_poistamisen\_tuloksen** ja tässä tapauksessa emme palauttaa **IOTHUBMESSAGE\_hyväksytyt**. Emme voi palauttaa tämän funktion muita arvoja, jotka muuttuvat, miten **IoTHubClient** kirjaston reagoi viestin takaisinkutsu on. Seuraavat vaihtoehdot:

-   **IOTHUBMESSAGE\_hyväksytyt** – viestin käsittely onnistui. **IoTHubClient** -kirjastoa ei kutsuu takaisinkutsutoiminto saman viestin uudelleen kanssa.

-   **IOTHUBMESSAGE\_hylätty** – viestiä ei käsitellä ja ei ole haluavat tehdä myöhemmin. **IoTHubClient** kirjaston ei olisi kutsua takaisinkutsutoiminto saman viestin uudelleen kanssa.

-   **IOTHUBMESSAGE\_ABANDONED** – viesti ei käsiteltiin, mutta **IoTHubClient** kirjaston tulisi käynnistää takaisinkutsutoiminto saman viestin uudelleen kanssa.

Palauttaa kahden ensimmäisen koodien **IoTHubClient** kirjaston lähettää viestin IoT keskittimeen, joka ilmaisee, että viesti poistetaan laitteen jonossa ja toimittaa uudelleen. Nettonykyarvon vaikutus on sama (viesti poistetaan myös laitteen jonossa), mutta onko viesti on hyväksytty tai hylätty edelleen on tallennettu.  Tallennuksen tunnukseen on hyötyä viestin lähettäjiin, kuka voi kuunnella palautetta varten ja selvittää, jos laite on hyväksytty tai hylätty tietty sähköpostiviesti.

Viimeksi tapauksessa viesti lähetetään myös IoT keskittimeen, mutta se osoittaa, että viesti on toimitettu edelleen. Yleensä hylätä viestin, jos jotkin virhe ilmenee, mutta haluat yrittää käsitellä viestiä uudelleen. Sen sijaan hylkäämällä viestin sopii kun kohtaat palauttaa virheen (tai jos riittää, että et halua käsitellä viestiä).

Missään tapauksessa huomioon eri palautuksen koodit niin, että voit merkkirokotteella **IoTHubClient** kirjastosta toimintaa.

## <a name="alternate-device-credentials"></a>Vaihtoehtoinen laitteen tunnistetiedot

Aiemmin kuvatulla Huomaa tehdään, jos **IoTHubClient** kirjastoa ei saada **IOTHUB\_asiakkaan\_käsitellä** puhelun esimerkiksi seuraavasti:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Argumentit **IoTHubClient\_CreateFromConnectionString** on Microsoftin laitteen ja parametri, joka ilmaisee Käytämme pitää yhteyttä IoT keskittimeen protokolla yhteysmerkkijono. Yhteysmerkkijonon on muodossa, joka tulee näkyviin seuraavasti:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

On neljä kappaletta tämän tietojen: IoT keskittimeen nimi, IoT keskittimeen jälkiliite tai laitteen tunnus jaetun pikanäppäin. Täydellinen toimialuenimi (FQDN) hankkia IoT-toiminnossa, kun luot IoT-toiminnossa esiintymän Azure-portaalissa, kyseinen rivimäärä on IoT keskittimeen nimi (FQDN ensimmäinen osa) ja IoT keskittimeen liitteen (asiakirjan muut osat FQDN). Saat laitteen tunnus ja jaettu-pikanäppäin, kun laite rekisteröidä IoT keskittimeen (kuvatulla tavalla [Edellinen artikkeli](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** kautta yhteen kaikki alustaa-kirjastoa. Jos käytät mieluummin, voit luoda uuden **IOTHUB\_asiakkaan\_käsitellä** avulla yksittäiset muuttujat yhteysmerkkijonon sijaan. Tämä on saavutettu seuraava koodi:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Tämä suorittaa sama kuin **IoTHubClient\_CreateFromConnectionString**.

Saattaa näyttää selvää, haluat käyttää **IoTHubClient\_CreateFromConnectionString** sen sijaan, että alustus tarkempia menetelmä. Ota huomioon, kuitenkin, että kun laitteen rekisteröinnin IoT toiminnossa voit saada on laitteen tunnus ja laitteen avainta (ei yhteysmerkkijonon). [Edellinen artikkeli](iot-hub-device-sdk-c-intro.md) käyttöön **Laitehallinta** SDK-työkalu käyttää kirjastojen **Azure IoT palvelu SDK** yhteysmerkkijonon luominen laitteen tunnus, laitteen avainta ja IoT keskittimeen isäntänimi. Soittavan niin **IoTHubClient\_li\_Luo** voi olla parempi, koska se tallentaa voit luoda yhteysmerkkijonon vaihe. Käytä valitsemallasi tavalla on kätevä.

## <a name="configuration-options"></a>Kokoonpanoasetusten määrittäminen

Tähän mennessä kaikki kuvattu siitä, miten **IoTHubClient** -kirjaston toimii kuvastaa sen oletustoiminnan. On kuitenkin muutamalla eri tavalla, voit määrittää kirjaston toimintatavan muuttaminen. Tämä on kirjautumalla hyödyntäminen **IoTHubClient\_li\_SetOption** API. Ota huomioon tässä esimerkissä:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Muutama yleisimmin käytettyjä vaihtoehdoista:

-   **SetBatching** (bool) – Jos **Tosi,**ja valitse IoT keskittimeen lähetetyt tiedot lähetetään erissä. Jos **Epätosi**ja sitten viestit lähetetään yksitellen. Oletusarvo on **Epätosi**. Huomaa, että **SetBatching** -asetus koskee vain HTTP-protokolla eikä MQTT tai AMQP-protokollaa.

-   **Aikakatkaisu** (allekirjoittamaton int) – tämä arvo esitetään millisekunteina. Jos lähettää HTTP-pyynnön tai vastausta kestää kauemmin kuin tällä hetkellä ja valitse haluamasi yhteys aikakatkaistaan.

Batching vaihtoehto on tärkeä. Oletusarvoisesti kirjaston ingresses tapahtumat erikseen (yksi tapahtuma on riippumatta siitä, mitä voit siirtää **IoTHubClient\_li\_SendEventAsync**). Batching asetus on **Tosi**, jos kirjaston kerää niin monta tapahtumien kuin mahdollista-puskuri (ylöspäin, IoT keskittimeen hyväksyy viestin enimmäiskoko).  Tapahtuman erä lähetetään IoT keskittimeen yksittäisen HTTP-puhelua (yksittäiset tapahtumat on koottu JSON-matriisi). Ottaminen käyttöön jonottaminen yleensä johtaa suuri suorituskykyä, koska olet pienentämisestä verkon edestakaista. Se myös merkittävästi vähentää kaistanleveyden jälkeen, kun haluat lähettää HTTP-otsikot tapahtuman erä joukkona sijaan yksittäisiä kuhunkin tapahtumaan otsikoiden joukko. Ellet tietyn halua tehdä muussa, yleensä kannattaa ottaa käyttöön jonottaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa kerrotaan yksityiskohtaisesti löydy **Azure IoT laitteen SDK c** **IoTHubClient** kirjaston toimintaa. Nämä ohjeet pitäisi olla ymmärtää **IoTHubClient** kirjaston ominaisuuksia. [Seuraava artikkeli](iot-hub-device-sdk-c-serializer.md) on samanlainen tiedot **Sarjatoiminto** -kirjastossa.

Lisätietoja kehittäminen IoT toiminnosta on artikkelissa [IoT keskittimeen SDK: T][lnk-sdks].

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
