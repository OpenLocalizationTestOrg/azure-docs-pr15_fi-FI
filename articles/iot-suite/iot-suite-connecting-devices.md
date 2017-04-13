<properties
   pageTitle="C käyttäminen Windows laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään laitteen liittäminen Azure IoT Suite valmiiksi remote seurantaa ratkaisun kirjoitettu Windows-käyttöjärjestelmässä C-sovelluksesta."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisun (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>C otoksen ratkaisun luominen Windows

Seuraavia ohjeita noudattamalla voit Visual Studio avulla voit luoda asiakassovellus, C, joka on valmiiksi Remote seuranta-ratkaisun yhteydessä kirjoitetut.

Starter projektin luominen Visual Studio 2015 ja IoT keskittimeen laitteen asiakkaan NuGet pakettien lisääminen:

1. Luo Visual Studio 2015 C console-sovellus, joka käyttää Visual C++ **Konsolin Win32-sovellus** -malli. Projektin **RMDevice**nimi.

2. Varmista **Ohjatun Win32-sovellus** **Sovellukset asetukset** -sivun **Console-sovellus** on valittuna, ja poista **Precompiled ylä** - ja **tarkistaa Security Development Lifecycle (SDL)**.

3. Poista tiedostot stdafx.h, targetver.h ja stdafx.cpp **Ratkaisunhallinnassa**.

4. **Ratkaisunhallinnassa**nimeä tiedosto RMDevice.cpp RMDevice.c.

5. **Ratkaisunhallinnassa** **RMDevice** projektin hiiren kakkospainikkeella ja valitse sitten **Hallitse NuGet paketit**. Valitse **Selaa**, valitse Etsi ja asenna seuraavat NuGet paketit projektiin:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. **Ratkaisunhallinnassa** **RMDevice** projektin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet** , jos haluat avata projektin **Ominaisuusikkunat** -valintaikkunan. Lisätietoja on artikkelissa [Asetus Visual C++ projektin ominaisuuksien][lnk-c-project-properties]. 

7. Valitse **linkitin** -kansio ja valitse sitten **syötteen** ominaisuutta-sivulla.

8. Lisää **crypt32.lib** **Muita riippuvuuksia** -ominaisuutta. Valitse **OK** ja valitse **OK** uudelleen Voit tallentaa projektin ominaisuusarvoihin.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Määritä IoT keskittimeen laitteen toiminta

IoT keskittimeen asiakkaan kirjastojen mallin avulla voit määrittää laite lähettää IoT keskittimeen ja se saa IoT-toiminnosta komennot viestin muodon muuttaminen.

1. Avaa Visual Studion RMDevice.c-tiedosto. Korvaa olemassa olevat `#include` lauseet, joiden seuraava koodi:

    ```
    #include "iothubtransporthttp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Lisää seuraavat muuttujan määrittelyjä jälkeen `#include` lauseita. Korvaa paikkamerkin arvot [laitteen tunnus] ja [laitteen avainta] arvot laitteeseesi remote seurantaa ratkaisu-koontinäytössä. IoT keskittimeen isäntänimi koontinäytöstä avulla voit korvata [IoTHub nimi]. Esimerkiksi jos IoT-toiminnossa Hostname **contoso.azure devices.net**, **contoso**korvaa [IoTHub nimi]:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Lisää seuraava koodi malli, jonka avulla pitää yhteyttä IoT keskittimeen laitteen määrittämiseen. Tämä malli määrittää, että laitteen lähettää telemetriatietojen lämpötilan, ulkoiset lämpötila, kosteus ja laitteen tunnus. Laitteen lähettää laitteen metatietoa myös sekä luettelo komennot, jotka laite tukee IoT-toiminnossa. Tämä laite vastaa **SetTemperature** ja **SetHumidity**-komennot:

    ```
    // Define the Model
    BEGIN_NAMESPACE(Contoso);

    DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
    );

    DECLARE_STRUCT(DeviceProperties,
    ascii_char_ptr, DeviceID,
    _Bool, HubEnabledState
    );

    DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
    );

    END_NAMESPACE(Contoso);
    ```

## <a name="implement-the-behavior-of-the-device"></a>Toteuta laitteen toimintaa

Nyt voit lisätä koodin, jotka perustuvat malliin toiminnan toteuttaa.

1. Lisää seuraavat toiminnot, jotka suoritetaan, kun laite vastaanottaa IoT-toiminnosta **SetTemperature** ja **SetHumidity** -komennot:

    ```
    EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
    {
      (void)printf("Received temperature %d\r\n", temperature);
      thermostat->Temperature = temperature;
      return EXECUTE_COMMAND_SUCCESS;
    }

    EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
    {
      (void)printf("Received humidity %d\r\n", humidity);
      thermostat->Humidity = humidity;
      return EXECUTE_COMMAND_SUCCESS;
    }
    ```

2. Lisää seuraavan funktion, joka lähettää viestin IoT-toiminnossa:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
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
    }
    ```

3. Lisää seuraavan funktion, joka yhdistää Sarjatoiminto kirjaston SDK: n määrittäminen:

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

4. Lisää seuraavan funktion Yhdistä IoT keskittimeen, Lähetä ja vastaanota viestit ja Katkaise yhteys-toiminnosta. Huomaa, kuinka laite lähettää itse liittyviä metatietoja, kuten se tukee IoT-keskittimeen, kun se muodostaa komentoja. Metatiedot mahdollistaa ratkaisun laitteen tilan päivittäminen koontinäytössä **käynnissä** :

    ```
    void remote_monitoring_run(void)
    {
      if (serializer_init(NULL) != SERIALIZER_OK)
      {
        printf("Failed on serializer_init\r\n");
      }
      else
      {
        IOTHUB_CLIENT_CONFIG config;
        IOTHUB_CLIENT_HANDLE iotHubClientHandle;

        config.deviceId = deviceId;
        config.deviceKey = deviceKey;
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
        iotHubClientHandle = IoTHubClient_Create(&config);
        if (iotHubClientHandle == NULL)
        {
          (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
        }
        else
        {
          Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
          if (thermostat == NULL)
          {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
          }
          else
          {
            STRING_HANDLE commandsMetadata;

            if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
            {
              printf("unable to IoTHubClient_SetMessageCallback\r\n");
            }
            else
            {

              /* send the device info upon startup so that the cloud app knows
              what commands are available and the fact that the device is up */
              thermostat->ObjectType = "DeviceInfo";
              thermostat->IsSimulatedDevice = false;
              thermostat->Version = "1.0";
              thermostat->DeviceProperties.HubEnabledState = true;
              thermostat->DeviceProperties.DeviceID = (char*)deviceId;

              commandsMetadata = STRING_new();
              if (commandsMetadata == NULL)
              {
                (void)printf("Failed on creating string for commands metadata\r\n");
              }
              else
              {
                /* Serialize the commands metadata as a JSON string before sending */
                if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                {
                  (void)printf("Failed serializing commands metadata\r\n");
                }
                else
                {
                  unsigned char* buffer;
                  size_t bufferSize;
                  thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                  /* Here is the actual send of the Device Info */
                  if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed serializing\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                }

                STRING_delete(commandsMetadata);
              }

              thermostat->Temperature = 50;
              thermostat->ExternalTemperature = 55;
              thermostat->Humidity = 50;
              thermostat->DeviceId = (char*)deviceId;

              while (1)
              {
                unsigned char*buffer;
                size_t bufferSize;

                (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                {
                  (void)printf("Failed sending sensor value\r\n");
                }
                else
                {
                  sendMessage(iotHubClientHandle, buffer, bufferSize);
                }

                ThreadAPI_Sleep(1000);
              }
            }

            DESTROY_MODEL_INSTANCE(thermostat);
          }
          IoTHubClient_Destroy(iotHubClientHandle);
        }
        serializer_deinit();
      }
    }
    ```
    
    Käyttöä seuraavassa on IoT keskittimeen käynnistyksen lähetetty esimerkkiviesti **DeviceInfo** :

    ```
    {
      "ObjectType":"DeviceInfo",
      "Version":"1.0",
      "IsSimulatedDevice":false,
      "DeviceProperties":
      {
        "DeviceID":"mydevice01", "HubEnabledState":true
      }, 
      "Commands":
      [
        {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
        { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
      ]
    }
    ```
    
    Viitteen tässä on lähetetty IoT keskittimeen **Telemetriatietojen** esimerkkiviesti:

    ```
    {"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
    ```
    
    Viitteen tässä on esimerkki **komento** vastaanotettu IoT-toiminnosta:
    
    ```
    {
      "Name":"SetHumidity",
      "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
      "CreatedTime":"2016-03-11T15:09:44.2231295Z",
      "Parameters":{"humidity":23}
    }
    ```

5. Korvaa **tärkeimmät** funktio käynnistää **remote_monitoring_run** -funktion seuraava koodi:

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Valitse **Muodosta** ja **Luoda ratkaisun** luonnissa laite-sovelluksen.

7. **Ratkaisunhallinnassa** **RMDevice** projektin hiiren kakkospainikkeella, valitse **Virheenkorjaus**ja valitse sitten mallin suorittamiseen **aloittaa uuden esiintymän** . Konsolin näyttää viestit sovelluksen lähettää otoksen telemetriatietojen IoT keskittimeen ja vastaanottaa komentoja.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx