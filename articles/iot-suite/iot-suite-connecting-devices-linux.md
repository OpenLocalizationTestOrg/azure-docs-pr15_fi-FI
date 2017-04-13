<properties
   pageTitle="Käyttämällä C Linux laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään laitteen liittäminen Azure IoT Suite valmiiksi remote seurantaa ratkaisun kirjoitettu C käynnissä Linux-sovelluksesta."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisun (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Luoda ja suorittaa näyte C asiakkaan Linux

Seuraavasti, kuinka voit luoda asiakassovellus, kirjoitettu C ja sisäisten ja suorita Ubuntu Linux, joka välittää remote seurantaa esimääritettyjä ratkaisuun. Näiden vaiheiden suorittamiseen tarvitset Ubuntu versio 15.04 tai 15.10-laitteessa. Ennen kuin jatkat, asenna valmistelevat pakettien Ubuntu laite käyttämällä seuraava komento:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Asiakas-kirjastojen asennettu laitteeseesi

Azure IoT keskittimeen asiakas-kirjastot ovat käytettävissä pakettina, voit asentaa Ubuntu laitteen **piharakennus get** -komennon avulla. Tehtävä Asenna paketti IoT keskittimeen asiakkaan kirjasto ja otsikon tiedostot sisältävä Ubuntu tietokoneessa seuraavat toimet:

1. Lisää AzureIoT säilö tietokoneeseen:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Asenna azure iot-sdk-c-keskihajonta-paketti

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Voit määrittää laitteen toiminta-koodin lisääminen

Ubuntu käyttämääsi laitteeseen, Luo kansio nimeltä **remote\_seuranta**. : **Remote\_seuranta** kansion luominen **main.c**neljä tiedostoa, **remote\_monitoring.c**, **remote\_monitoring.h**, ja **CMakeLists.txt**.

IoT keskittimeen Sarjatoiminto asiakkaan kirjastojen mallin avulla voit määrittää laite lähettää IoT keskittimeen ja se saa IoT-toiminnosta komennot viestin muodon muuttaminen.

1. Avaa tekstieditorissa, **remote\_monitoring.c** tiedosto. Lisää seuraava `#include` lauseet:

    ```
    #include "iothubtransportamqp.h"
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
    static const char* hubName = "IoTHub Name]";
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

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Laitteen toiminnan toteuttamisesta-koodin lisääminen

Lisää Funktiot, kun laite vastaanottaa komennon keskittimeen ja lähettää Simuloitu telemetriatietojen keskittimeen koodin suorittaminen.

1. Lisää, kun laite vastaanottaa **SetTemperature** ja **SetHumidity** komennot, jotka perustuvat malliin, suorita seuraavat toiminnot:

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
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
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
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
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
        platform_deinit();
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

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Lisää koodi käynnistää remote_monitoring_run-funktio

Avaa tekstieditorissa, **remote_monitoring.h** -tiedosto. Lisää seuraava koodi:

```
void remote_monitoring_run(void);
```

Avaa tekstieditorissa, **main.c** -tiedosto. Lisää seuraava koodi:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Luo CMake asiakassovellus avulla

Seuraavassa kuvataan, miten asiakassovellus luonnissa käytettävien *CMake* .

1. Avaa tekstieditorissa, **CMakeLists.txt** -tiedosto tallennetaan **remote_monitoring** -kansioon.

2. Lisää Määritä, miten voit luoda asiakassovellus seuraavia ohjeita:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. **Remote_monitoring** -kansioon Luo kansio tallentamiseen *tehdä* tiedostoista, jotka CMake Luo ja suorita **cmake** ja **Tee** komentoja seuraavalla tavalla:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Suorita asiakassovellus ja lähettää telemetriatietojen IoT keskittimeen seuraavasti:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

