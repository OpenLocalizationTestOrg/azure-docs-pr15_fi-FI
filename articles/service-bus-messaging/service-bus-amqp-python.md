<properties 
    pageTitle="Palvelun Bus ja Python kanssa AMQP 1.0 | Microsoft Azure"
    description="Palvelun Bus-Python käyttäminen AMQP."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Palvelun Bus-Python käyttäminen AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton Python on Python kielen sitominen Proton-C; Proton Python on toteutettu paketti ympärille ydin toteutettu C.

## <a name="download-the-proton-client-library"></a>Lataa Proton asiakas-kirjasto

Voi ladata [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton C ja sen liittyvät sidontojen (mukaan lukien Python). Lataaminen on lähdekoodin muodossa. Muodostaa koodin sisältämät ladatut paketin ohjeiden.

Huomaa, että tämä kirjoittaminen milloin Proton C SSL-tuki on käytettävissä vain Linux-käyttöjärjestelmissä. Koska Bus Azure-palvelu edellyttää SSL-Proton C (ja kieli-sidontojen) vain voidaan käyttää palvelun Bus Linux tällä hetkellä. Käyttöön Proton-C SSL Windows on käynnissä niin valintaruudun takaisin usein päivitykset.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Palvelun Bus olevien, aiheet ja tilaukset-Python käyttäminen

Seuraava koodi näytetään, miten voit lähettää ja vastaanottaa viestejä palvelun Bus messaging kohteen.

### <a name="send-messages-using-proton-python"></a>Lähettää viestiä, joissa Proton Python

Seuraava koodi näytetään, miten voit lähettää viestin palvelun Bus messaging kohteen.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Vastaanottaa viestiä, joissa Proton Python

Seuraava koodi esitetään, kuinka voit tuoda näyttöön virhesanoman palvelun Bus messaging kohteen.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>.NET ja Proton Python välinen viestiliikenne

### <a name="application-properties"></a>Sovelluksen ominaisuudet

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python vastaamisessa Bus .NET-ohjelmointirajapinnan

Proton Python viestien tukea sovelluksen seuraavia ominaisuuksia: **int** **pitkä** **liukuluku**, **uuid**, **bool**, **merkkijono**. Seuraava koodi Python esitetään, kuinka voit määrittää ominaisuudet viestin käyttämällä eri ominaisuus.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Palvelun Bus .NET-Ohjelmointirajapinnan viestin sovelluksen ominaisuuksien suoritetaan [BrokeredMessage][] **Ominaisuudet** -kokoelmaan. Seuraava koodi esitetään, kuinka voit lukea Python asiakas vastaanotti viestin sovelluksen ominaisuuksia.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Seuraavassa taulukossa yhdistää Python ominaisuuden tyypit .NET-ominaisuuden käyttämiseen.

| Python ominaisuuden tyyppi | .NET-ominaisuuden tyyppi |
|----------------------|--------------------|
| kokonaisluku                  | kokonaisluku                |
| liukuluku                | Kaksinkertainen             |
| pitkä                 | Int64              |
| UUID                 | GUID-tunnus               |
| bool                 | bool               |
| merkkijono               | merkkijono             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Palvelun Proton Python, .NET-ohjelmointirajapinnan Bus

[BrokeredMessage][] tyyppi tukee seuraavia sovelluksen ominaisuuksien: **tavu**, **sbyte**, **merkki**, **lyhyt**, **ushort**, **int**, **uint**, **pitkä**, **ulong**, **liukuluku**, **double**, **desimaaleja**, **bool**, **GUID-tunnus**, **merkkijonon**, **Uri**, **päivämäärä ja aika**, **DateTimeOffset-arvoa**, ja **aikajakson**. .NET seuraava koodi näytetään, miten voit määrittää ominaisuudet käyttämällä eri ominaisuuden [BrokeredMessage][] aluetta.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Python seuraava koodi esitetään, kuinka voit lukea palvelun Bus .NET-asiakas vastaanotti viestin sovelluksen ominaisuuksia.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

Seuraavassa taulukossa yhdistää .NET-ominaisuuden tyypit Python ominaisuuden tyyppejä.

| .NET-ominaisuuden tyyppi | Python ominaisuuden tyyppi | Huomautuksia                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tavu               | kokonaisluku                  | -                                                                                                                                                                     |
| sbyte              | kokonaisluku                  | -                                                                                                                                                                     |
| merkki               | merkki                 | Proton Python luokan                                                                                                                                                 |
| lyhyt              | kokonaisluku                  | -                                                                                                                                                                     |
| ushort             | kokonaisluku                  | -                                                                                                                                                                     |
| kokonaisluku                | kokonaisluku                  | -                                                                                                                                                                     |
| uint               | kokonaisluku                  | -                                                                                                                                                                     |
| pitkä               | kokonaisluku                  | -                                                                                                                                                                     |
| ulong              | pitkä                 | Proton Python luokan                                                                                                                                                 |
| liukuluku              | liukuluku                | -                                                                                                                                                                     |
| Kaksinkertainen             | liukuluku                | -                                                                                                                                                                     |
| desimaaliluku            | Merkkijono               | Desimaaliluku on ei tue Proton.                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| GUID-tunnus               | UUID                 | Proton Python luokan                                                                                                                                                 |
| merkkijono             | merkkijono               | -                                                                                                                                                                     |
| Päivämäärä ja aika           | aikaleima            | Proton Python luokan                                                                                                                                                 |
| DateTimeOffset-arvoa     | DescribedType        | DateTimeOffset.UtcTicks yhdistetty AMQP tyyppi:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Aikajakson           | DescribedType        | Timespan.Ticks yhdistetty AMQP tyyppi:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Uri.AbsoluteUri yhdistetty AMQP tyyppi:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Vakio-ominaisuudet

Seuraavissa taulukoissa on Proton Python viestin-ominaisuuksien ja [BrokeredMessage][] viestin ominaisuuksien yhdistämisen.

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python vastaamisessa Bus .NET-ohjelmointirajapinnan

| Proton Python        | Palvelun Bus .NET         | Huomautuksia                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| kestävät              | puuttuu                      | Palvelun Bus tukee vain kestävät viestejä.               |
| Priority (prioriteetti)             | puuttuu                      | Palvelun Bus tukee vain yhden viestin prioriteetti.      |
| TTL (elinaika)                  | Message.TimeToLive       | Muunnoksen Proton Python TTL on määritetty millisekunteina. |
| ensimmäinen\_hankkijaosapuoli      | puuttuu                      | -                                                           |
| toimituksen\_määrä      | puuttuu                      | -                                                           |
| Tunnus                   | Message.MessageID        | -                                                           |
| käyttäjän\_tunnus             | puuttuu                      | -                                                           |
| osoite              | Message.To               | -                                                           |
| aihe              | Message.Label            | -                                                           |
| vastaa\_avulla            | Message.ReplyTo          | -                                                           |
| korrelaatio\_tunnus      | Message.CorrelationID    | -                                                           |
| sisällön\_tyyppi        | Message.ContentType      | -                                                           |
| sisällön\_koodaus    | puuttuu                      | -                                                           |
| määräajan\_aika         | puuttuu                      | -                                                           |
| luomisen\_aika       | puuttuu                      | -                                                           |
| ryhmän\_tunnus            | Message.SessionId        | -                                                           |
| ryhmän\_järjestys      | puuttuu                      | -                                                           |
| vastaa\_,\_ryhmän\_tunnus | Message.ReplyToSessionId | -                                                           |
| Muotoile               | puuttuu                      | -                                                           |

| Palvelun Bus .NET        | Proton                       | Huomautuksia                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_tyyppi        | -                                                           |
| CorrelationId           | Message.Correlation\_tunnus      | -                                                           |
| EnqueuedTimeUtc         | puuttuu                          | -                                                           |
| Otsikko                   | Message.Subject              | -                                                           |
| MessageId               | Message.ID                   | -                                                           |
| ReplyTo                 | Message.Reply\_avulla            | -                                                           |
| ReplyToSessionId        | Message.Reply\_,\_ryhmän\_tunnus | -                                                           |
| ScheduledEnqueueTimeUtc | puuttuu                          | -                                                           |
| Istunto               | Message.Group\_tunnus            | -                                                           |
| TimeToLive              | Message.TTL                  | Muunnoksen Proton Python TTL on määritetty millisekunteina. |
| Jos haluat                      | Message.address              | -                                                           |

## <a name="next-steps"></a>Seuraavat vaiheet

Haluatko lisätietoja? Seuraavassa on seuraavissa linkeissä:

- [Palvelun Bus AMQP yleiskatsaus]
- [Windows Server Service Bus AMQP]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[Windows Server Service Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx

[Palvelun Bus AMQP yleiskatsaus]: service-bus-amqp-overview.md
