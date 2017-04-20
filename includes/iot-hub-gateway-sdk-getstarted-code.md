## <a name="typical-output"></a>Tyypillinen tulos

Alla on esimerkki tulosteen lokitiedostoon kirjoittama Hello World Esimerkki. Luettavuus olisi lisätty merkkiä rivinvaihtomerkkiä ja -välilehti:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Koodikatkelmien

Tässä osassa käsitellään tärkeimpiä osia Hello World näytteen koodi.

### <a name="gateway-creation"></a>Yhdyskäytävän luonti

Kehittäjän täytyy kirjoittaa *gateway prosessi*. Tämä ohjelma luo sisäinen infrastruktuurin (välittäjä), lataa moduulit ja asettaa kaiken oikein. SDK sisältää **Gateway_Create_From_JSON** -funktion avulla voit käynnistää JSON-tiedostosta yhdyskäytävän. Voit käyttää **Gateway_Create_From_JSON** -toimintoa, sinun on välitettävä se polku JSON-tiedostoon, joka määrittää ladata moduulit. 

Hello World- [main.c] näytteessä gateway-prosessin koodi löytyy[ lnk-main-c] tiedoston. Luettavuus gateway koodi lyhennetty versio näkyy alla pätkään. Tämä ohjelma luo yhdyskäytävä ja sitten odottaa, että käyttäjä paina **ENTER** -näppäintä, ennen kuin se alaspäin Yhdyskäytävä purkaa. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

JSON-asetustiedosto sisältää luettelon moduuleista, jotka voit ladata. Kukin moduuli on määritettävä a:

- **module_name**: moduuli yksilöivä nimi.
- **module_path**: kirjasto, joka sisältää moduulin polku. Linux, tämä on .so tiedosto, Windows on .dll-tiedosto.
- **argumentit**: moduuli tarvitsee kokoonpanotietoja.

JSON-tiedosto sisältää myös moduulit, jotka siirretään välittäjän väliset linkit. Linkki on kaksi ominaisuutta:
- **lähde**: moduulin nimi `modules` osan, tai "\*".
- **allas**: moduulin nimi `modules` osa

Kunkin linkin määritellään sanoman reitin ja suunta. Viestit-moduulista `source` -moduulin toimitetaan `sink`. `source` Voi "\*", joka osoittaa, että mikä tahansa moduuli viestit vastaanotetaan mukaan `sink`.

Seuraava esimerkki näyttää JSON-asetustiedoston avulla määritetään Hello World Esimerkki Linux. Jokainen viesti tuottama moduuli `hello_world` moduuli kuluttama `logger`. Onko moduuli tarvitsee moduulin rakenne määräytyy argumentti. Tässä esimerkissä logger-moduuli kestää argumentti, joka on kohdetiedoston polku ja Hello World-moduuli ei hyväksy argumentteja:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World moduuli viestin julkaiseminen

Koodi, jota käytetään moduulissa "Hei" julkaistavat viestit ["hello_world.c"] löytyy[ lnk-helloworld-c] tiedoston. Lisäkommentit ja joitakin koodin luettavuutta poistetaan Virheenkäsittely on muutettu versio näkyy alla pätkään:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Hello World moduuli käsittely

Hello World-moduuli tarvitsee koskaan käsitellä viestejä, jotka välittäjän julkaista muissa moduuleissa. Näin viesti takaisinkutsu täytäntöönpanon Hello World-moduuli ei-op-funktiota.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Lokin moduuli viestin julkaiseminen ja käsittely

Lokin moduuli vastaanottaa sanomia välittäjän ja kirjoittaa tiedostoon. Se julkaisee koskaan viestejä. Siksi lokin moduulin koodi kutsuu koskaan **Broker_Publish** -funktiota.

[Logger.c] - **Logger_Recieve** -toimintoa[ lnk-logger-c] tiedosto on välittäjän kutsuu viestit toimitetaan logger-moduulin takaisinsoittoa. Lisäkommentit ja joitakin koodin luettavuutta poistetaan Virheenkäsittely on muutettu versio näkyy alla pätkään:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Saat IoT Gateway SDK: N käyttämisestä on seuraavissa ohjeaiheissa:

- [IoT Gateway SDK – lähettää viestejä laitetta pilven kanssa Simuloitu laitteen avulla Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK: N] [ lnk-gateway-sdk] -GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md