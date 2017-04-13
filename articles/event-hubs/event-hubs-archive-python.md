<properties
    pageTitle="Azure tapahtuman keskittimet arkisto ongelmatilanteita | Microsoft Azure"
    description="Malli, joka käyttää Azure Python SDK osoittamaan tapahtuman keskittimet arkisto-ominaisuudella."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Tapahtuman keskittimet arkisto ongelmatilanteita: Python

Tapahtuman keskittimet arkisto on tapahtuma, jonka avulla voit pitää automaattisesti tapahtumaa-toiminnossa Azure-Blob-säiliö tiliin valittua stream tiedot keskittimet uusi ominaisuus. Tämä on helppo suorittaa eräkäsittely streaming reaaliaikaisia tietoja. Tässä artikkelissa käsitellään tapahtuman keskittimet arkisto käyttäminen Python. Lisätietoja tapahtuman keskittimet arkisto on [artikkelissa yleistä](event-hubs-archive-overview.md).

Tässä esimerkissä käytetään Azure Python SDK osoittamaan arkisto-ominaisuudella. Sender.py lähettää Simuloitu ympäristön telemetriatietojen tapahtuman porttiin JSON-muodossa. Tapahtuma-toiminnossa on määritetty käyttämään arkistointia kirjoittaa tämä tieto blob storage erissä. Archivereader.py sitten lukee nämä BLOB-objektit ja luo Liitä tiedoston laite ja kirjoittaa tiedot .csv-tiedostoon.

Mitä tehdä

1.  Luo tili Azure-Blob-säiliö ja Blob-objektien säilön sisällä Azure-portaalissa

2.  Luo tapahtumaa-toiminnossa-nimitilan, Azure-portaalissa

3.  Luo tapahtumaa-toiminnossa arkisto-toiminnon käyttöön, Azure-portaalissa

4.  Tietojen lähettäminen Python komentosarjan tapahtumaa-toiminnossa

5.  Lukea tiedostoja arkistoon ja käsitellä niitä toiseen Python komentosarja

Edellytykset

1.  Python 2.7.x

2.  Azure tilauksen

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Azure-tallennustilan tilin luominen

1.  Kirjaudu [Azure portal][].

2.  Portaalin vasemmanpuoleisessa siirtymisruudussa uusi ja valitse sitten tietojen + tallennustilan ja valitse sitten tallennustilan tilin.

3.  Täytä kentät tallennustilan tili-sivu ja valitse **Luo**.

    ![][1]

4.  Kun näet **Ominaisuuksissa onnistui** -sanoma, valitse uusi tallennustilan-tili ja **Essentials** -sivu valitsemalla **BLOB-objektit**. Kun **Blob service** -sivu avautuu, valitse **+ säilö** yläreunassa. Säilön **arkisto**nimi ja valitse sitten Sulje **Blob-palvelu** -sivu.

5.  Valitse **näppäimiä** vasen sivu ja kopioi tallennustilan tilin nimi ja **Avain1**arvo. Tallenna nämä arvot Muistioon tai muuhun tilapäinen paikkaan.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Voit lähettää tapahtumien tapahtumaa-toiminnossa Python komentosarjan luominen

1.  Avaa tuttuja Python editorin, kuten [Visual Studio koodi][].

2.  Luoda komentosarjan, jota kutsutaan **sender.py**. Tämä komentosarja lähettää 200 tapahtumien tapahtumaa-toiminnossa. He ovat yksinkertainen ympäristön lukemat JSON lähetetyt.

3.  Liitä seuraava koodi sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Päivitä yllä olevaa koodia nimitilan nimi ja-arvoja, jotka olet hankkinut tapahtuman keskittimet nimitilan luotaessa.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Arkisto-tiedostojen lukemiseen Python komentosarjan luominen

1.  Täytä sivu ja valitse **Luo**.

2.  Luoda komentosarjan, jota kutsutaan **archivereader.py**. Tämä komentosarja lukee tiedostojen arkistointi ja luoda tiedoston kohti laite, kun haluat kirjoittaa vain kyseisen laitteen tiedot.

3.  Liitä seuraava koodi archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Liitä todellisilla tallennustilan tilin nimi ja avain puhelu, että `startProcessing`.

## <a name="run-the-scripts"></a>Suorittaa komentosarjat

1.  Avaa komentorivi-ikkuna, jossa on Python polkunsa ja Suorita nämä komennot Python valmistelevat pakettien asentamisen:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Jos käytössäsi on joko azure-tallennustilan aiempi versio tai azure joudut ehkä käyttää **--päivittäminen** vaihtoehto

    Joudut ehkä myös Suorita seuraava (ei tarvita useimmissa järjestelmiin):

    ```
    pip install cryptography
    ```

2.  Muuta hakemistossa aina, kun olet tallentanut sender.py ja archivereader.py ja suorittaa tämän komennon:

    ```
    start python sender.py
    ```
    
    Uuden Python loppuun suorittamiseen lähettäjän käynnistyy.

3. Odota muutama minuutti, jos haluat suorittaa arkistoa varten nyt. Kirjoita seuraava komento alkuperäisen komentorivi-ikkuna:

    ```
    python archivereader.py
    ```

Arkisto-suoritin käyttää paikallisen kansion Lataa kaikki Blob-objektien tallennustilan tilin/säilö. Se käsittelee mitään, jotka eivät ole tyhjiä ja kirjoittaa tulokset paikallisen kansion .csv-tiedostoina.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää tapahtuman keskittimet ohjesisältöä on seuraavissa linkeissä:

- [Yleistä tapahtuman keskittimet arkistoiminen][]
- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.
- [Tapahtuman keskittimet yleiskatsaus][]
 

[Azure portal]: https://portal.azure.com/
[Yleistä tapahtuman keskittimet arkistoiminen]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio-koodin.]: https://code.visualstudio.com/
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
