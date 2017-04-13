<properties 
    pageTitle="Voit käyttää palvelun Bus aiheet Python | Microsoft Azure" 
    description="Opettele käyttämään Azure palvelun Bus aiheet ja tilaukset-Python." 
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
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Palvelun Bus aiheet ja tilaukset käyttäminen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä artikkelissa käsitellään palvelun Bus aiheet ja tilaukset. Mallit on kirjoitettu Python ja [Python Azure-paketti][]. Tilanteita, joissa kattaa ovat **luominen aiheet ja tilaukset**, **tilauksen suodattimien luomisesta**, **aiheen viestien lähettäminen**, **vastaanottaa viestejä tilauksesta**ja **poistaminen aiheet ja tilaukset**. Lisätietoja aiheet ja tilaukset-kohdassa [Seuraavat vaiheet](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**Huomautus:** Jos haluat asentaa Python tai [Python Azure-paketti][], katso [Python oppaaseen](../python-how-to-install.md).

## <a name="create-a-topic"></a>Aiheen luominen

**ServiceBusService** objektin avulla voit käsitellä aiheet. Lisää minkä tahansa Python tiedoston, jossa haluat käyttää ohjelmallisesti palvelun Bus yläosassa seuraavasti:

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Seuraava koodi luo **ServiceBusService** objektin. Korvaa `mynamespace`, `sharedaccesskeyname`, ja `sharedaccesskey` oman todellinen nimitila jaettu Access allekirjoitus (SAS) avaimen nimi ja avaimen arvo.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Voit hankkia Suojaussidosten avaimen nimeksi arvot ja arvon [Azure portal][].

```
bus_service.create_topic('mytopic')
```

**luominen\_aiheen** tukee myös lisäasetuksia, joiden avulla voit korvata oletusasetukset aiheen kuten viestin, kun haluat live tai suurin aiheen kokoa. Seuraavassa esimerkissä määrittää suurin aiheen koon 5 gt ja kerrallaan live (TTL) arvon 1 minuutti:

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Luo tilaukset

Tilaukset aiheisiin luodaan myös **ServiceBusService** objekti. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa viestit toimitetaan tilauksen virtual jonossa olevat.

> [AZURE.NOTE] Tilaukset on jatkuva ja edelleen olemassa, kunnes ne tai ohjeaiheen, johon ne on tilattu, poistetaan.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

**MatchAll** suodatin on oletussuodatin, jota käytetään, jos ei suodatusta määritetään, kun luodaan uusi tilaus. Kun **MatchAll** -suodatin on käytössä, kaikki viestit, jotka on julkaistu aiheeseen sijoitetaan tilauksen virtual jonon. Seuraava esimerkki luo tilausta nimeltä 'AllMessages' ja käyttää **MatchAll** oletussuodatin.

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös määrittää suodattimia, joiden avulla voit määrittää, mitkä aiheen lähetetyt viestit näytetään tiettyä aihetta-tilauksen piiriin kuuluvien.

Tuettu tilauksissa suodattimen eniten joustavia tyyppi on **SqlFilter**, joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Saat lisätietoja lausekkeita, jotka voidaan käyttää SQL-suodattimella [SqlFilter.SqlExpression][] syntaksi.

Voit lisätä suodattimia tilauksen avulla **luominen\_säännön** **ServiceBusService** objektin menetelmää. Tätä menetelmää voit lisätä uusia suodattimia olemassa olevaan tilaukseen.

> [AZURE.NOTE] Koska oletussuodatin käytetään automaattisesti kaikissa uusissa tilauksissa, sinun on ensin poistettava oletussuodatin tai **MatchAll** ohittaa muut voivat määritellä suodattimet. Voit poistaa oletusarvon säännön avulla **poistaminen\_säännön** **ServiceBusService** objektin menetelmää.

Seuraavassa esimerkissä luodaan tilausta nimeltä `HighMessages` **SqlFilter** , joka valitsee vain viestit, jotka on suurempi kuin 3 mukautetun **messagenumber** ominaisuus kanssa:

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä `LowMessages` **SqlFilter** , joka valitsee vain viestit, joiden **messagenumber** ominaisuus pienempi tai yhtä 3 kanssa:

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nyt, kun viesti on lähetetty `mytopic` se toimitetaan aina vastaanottajia **AllMessages** aihe-tilaus on tilattu ja valikoivasti toimitettu vastaanottajia tilannut (sen mukaan, viestisisältö) **HighMessages** ja **LowMessages** aihe-tilauksia.

## <a name="send-messages-to-a-topic"></a>Viestien lähettäminen aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen on käytettävä **Lähetä\_aiheen\_viestin** **ServiceBusService** objektin menetelmää.

Seuraavassa esimerkissä viisi testata viestien lähettäminen `mytopic`. Huomaa, että kustakin viestistä **messagenumber** ominaisuuden arvo vaihtelee-silmukan iteraatio (paina ALT + w määrittää, mitkä tilaukset vastaanottaa sen):

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa kokoa on määritetty luominen aikaa, yläraja on 5 gt. Saat lisätietoja kiintiöiden [palvelun Bus kiintiön][].

## <a name="receive-messages-from-a-subscription"></a>Vastaanottaa viestejä tilaukseen

Viestejä vastaanotetaan tilauksen käyttämisen **vastaanottaa\_tilauksen\_viestin** **ServiceBusService** objektin menetelmää:

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Viestit poistetaan tilauksesta, kun ne ovat lukea parametrin **pikanäkymä\_Lukitse** arvo on **Epätosi**. Voit lukea (pikanäkymä) ja Lukitse viestin poistamatta jonossa, parametrin määrittämällä **pikanäkymä\_Lukitse** **Tosi**.

Toimintaa lukeminen ja vastaanota-toimintoa osana viestin poistaminen on helpoin mallin, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

Jos **pikanäkymä\_Lukitse** parametrin arvo on **Tosi**, vastaanotto tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen soittamalla **viestin** objektin **delete** -menetelmällä. **Poista** -menetelmä merkitsee viestin kulutettu käytössä-tilassa ja poistaa sen tilauksen.

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se kutsua **lukituksen** menetelmää **viestin** objektin. Tämä aiheuttaa palvelun Bus tilauksen piiriin kuuluvien viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu tilauksen piiriin kuuluvien viestin liittyvät aikakatkaisu, ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu)-palvelun Bus lukitus viestin automaattisesti ja mahdollistaa sen käytön vastaanotettu uudelleen.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen kuin **delete** -menetelmällä kutsutaan, valitse viestin olla toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **MessageId** -ominaisuuden avulla.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Ohjeita ja tilaukset on jatkuva ja täytyy erikseen poistaa [Azure portal][] kautta tai sen ohjelmallisesti. Seuraavassa esimerkissä nimeltä aiheen poistaminen `mytopic`:

```
bus_service.delete_topic('mytopic')
```

Aiheen poistaminen poistaa myös tilaukset, jotka on rekisteröity aihetta. Tilauksia voi poistaa myös erikseen. Seuraava koodi esitetään, kuinka voit poistaa tilausta nimeltä `HighMessages` kohteesta `mytopic` aihe:

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus aiheet perusteet, noudata näitä linkkejä lisätietoja.

-   Katso [olevien, aiheet, ja tilaukset][].
-   [SqlFilter.SqlExpression][]viittaus.

[Azure portal]: https://portal.azure.com
[Python Azure-paketti]: https://pypi.python.org/pypi/azure  
[Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Palvelun Bus kiintiön]: service-bus-quotas.md 
