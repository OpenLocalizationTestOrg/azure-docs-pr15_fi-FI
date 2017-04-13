<properties 
    pageTitle="Voit käyttää palvelun Bus olevien Python | Microsoft Azure" 
    description="Opettele käyttämään Python peräisin olevien Azure palvelun Bus." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Opi käyttämään palvelua Bus olevien

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tässä artikkelissa kerrotaan, miten käyttämään palvelua Bus olevien. Mallit on kirjoitettu Python ja [Python Azure palvelun Bus paketti][]. Tilanteita, joissa kattaa ovat **luominen olevien, lähettää ja vastaanottaa viestejä**ja **olevien poistaminen**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Asenna Python tai [Python Azure palvelun Bus-paketti][], on [Python oppaaseen](../python-how-to-install.md).

## <a name="create-a-queue"></a>Luo jono

**ServiceBusService** objektin avulla voit käsitellä olevien. Lisää lähellä sivun, jossa Python tiedostojen, jota haluat käyttää ohjelmallisesti palvelun Bus seuraava koodi:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Seuraava koodi luo **ServiceBusService** objektin. Korvaa `mynamespace`, `sharedaccesskeyname`, ja `sharedaccesskey` nimitilan, jaettu käyttö allekirjoitus (SAS) avaimen nimi ja arvo.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Suojaussidosten avaimen nimeksi arvot ja arvo löytyy [Azure perinteinen portal][] -yhteystietoja tai Visual Studio **Ominaisuudet** -ruudussa valitaan palvelun Bus nimitilan palvelimen Explorer (kuten edellisessä osassa).

```
bus_service.create_queue('taskqueue')
```

**create_queue** tukee myös lisäasetuksia, joiden avulla voit korvata oletusasetukset jonon kuten viestin, kun haluat live (TTL) tai jonon enimmäiskoko. Seuraavassa esimerkissä jonon enimmäiskoko 5 gt ja TTL-arvoa 1 minuutti:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Viestien lähettäminen jonossa

Viestin lähettäminen palvelun Bus jono-sovelluksen puhelut **lähettää\_jonossa\_viestin** menetelmä **ServiceBusService** objektin.

Seuraavassa esimerkissä kerrotaan, miten testi viestin lähettäminen nimeltä *taskqueue käyttämällä* jonossa **Lähetä\_jonossa\_viestin**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Palvelun Bus olevien tue viestin koko on [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu pidetään jonossa viestien määrän, mutta ole pää kokonaiskoko saamisesta jonon viestit. Tämä jono koko on määritetty luominen aikaa, yläraja 5 gigatavua. Saat lisätietoja kiintiöiden [palvelun Bus kiintiön][].

## <a name="receive-messages-from-a-queue"></a>Vastaanottaa viestejä jonossa

Viestejä vastaanotetaan jonon käyttämällä **vastaanottaa\_jonon\_viestin** **ServiceBusService** objektin menetelmää:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Viestit poistetaan jonossa, kun ne ovat lukea parametrin **pikanäkymä\_Lukitse** arvo on **Epätosi**. Voit lukea (pikanäkymä) ja Lukitse viestin poistamatta jonossa, parametrin määrittämällä **pikanäkymä\_Lukitse** **Tosi**.

Toimintaa lukeminen ja vastaanota-toimintoa osana viestin poistaminen on helpoin mallin, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

Jos **pikanäkymä\_Lukitse** parametrin arvo on **Tosi**, vastaanotto tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen soittamalla **viestin** objektin **delete** -menetelmällä. **Delete** -menetelmällä Merkitse viesti on kulutettu-tilassa ja poista se jonossa.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se kutsua **lukituksen** menetelmää **viestin** objektin. Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu), valitse palvelun Bus viestin lukituksen automaattisesti ja lisätään uudelleen Vastaanotettava.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen kuin **delete** -menetelmällä kutsutaan, valitse viestin olla toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **MessageId** -ominaisuuden avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut voit palvelun Bus olevien perusteet, noudata näitä linkkejä lisätietoja.

-   Katso [olevien, aiheet, ja tilaukset][].

[Azure perinteinen portal]: https://manage.windowsazure.com
[Python Azure palvelun Bus paketti]: https://pypi.python.org/pypi/azure-servicebus  
[Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
[Palvelun Bus kiintiön]: service-bus-quotas.md
 
