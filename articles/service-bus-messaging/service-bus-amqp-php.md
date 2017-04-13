<properties 
    pageTitle="Palvelun Bus ja PHP kanssa AMQP 1.0 | Microsoft Azure"
    description="Palvelun Bus-PHP käyttäminen AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Palvelun Bus-PHP käyttäminen AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton PHP on PHP kielen sitominen Proton-C; Proton PHP on toteutettu paketti ympärille ydin toteutettu C.

## <a name="downloading-the-proton-client-library"></a>Proton asiakkaan kirjaston lataaminen

Voi ladata [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton C ja sen liittyvät sidontojen (mukaan lukien PHP). Lataaminen on lähdekoodin muodossa. Muodostaa koodin ladatut paketti sisälsi ohjeiden.

> [AZURE.IMPORTANT] Milloin tämä kirjoittaminen Proton C SSL-tuki on käytettävissä vain Linux-käyttöjärjestelmissä. Koska Bus Azure-palvelu edellyttää SSL-Proton C (ja kieli-sidontojen) vain voidaan käyttää palvelun Bus Linux tällä hetkellä. Työn käyttöön Proton-C SSL Windows on käynnissä, joten Tarkista Edellinen usein päivitysten.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Palvelun Bus olevien, aiheet ja tilaukset-PHP käyttäminen

Seuraava koodi näytetään, miten voit lähettää ja vastaanottaa viestejä palvelun Bus messaging kohteen.

### <a name="sending-messages-using-proton-php"></a>Viestien Proton PHP lähettämisestä

Seuraava koodi näytetään, miten voit lähettää viestin palvelun Bus messaging kohteen.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Viestien Proton PHP vastaanottaminen

Seuraava koodi esitetään, kuinka voit tuoda näyttöön virhesanoman palvelun Bus messaging kohteen.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>.NET ja Proton PHP välinen viestiliikenne

### <a name="application-properties"></a>Sovelluksen ominaisuudet

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP palvelun Bus .NET-ohjelmointirajapinnan

Proton PHP viestien tukea sovelluksen seuraavia ominaisuuksia: **kokonaisluku**, **Kaksinkertainen** **totuusarvo**, **merkkijono**tai **objektia**. Seuraava koodi PHP esitetään, kuinka voit määrittää ominaisuudet viestin käyttämällä eri ominaisuus.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Palvelun Bus .NET-API viestin sovelluksen ominaisuuksien suoritetaan [BrokeredMessage][] **Ominaisuudet** -kokoelmaan. Seuraava koodi esitetään, kuinka voit lukea PHP asiakas vastaanotti viestin sovelluksen ominaisuuksia.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

Seuraavassa taulukossa yhdistää PHP ominaisuuden tyypit .NET-ominaisuuden käyttämiseen.

| PHP ominaisuuden tyyppi | .NET-ominaisuuden tyyppi |
|-------------------|--------------------|
| kokonaisluku           | kokonaisluku                |
| Kaksinkertainen            | Kaksinkertainen             |
| Totuusarvo           | bool               |
| merkkijono            | merkkijono             |
| objektin            | Objektin             |

#### <a name="service-bus-net-apis-to-php"></a>Palvelun Bus .NET-ohjelmointirajapinnan PHP avulla

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
```

PHP seuraava koodi esitetään, kuinka voit lukea palvelun Bus .NET-asiakas vastaanotti viestin sovelluksen ominaisuuksia.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

Seuraavassa taulukossa yhdistää .NET-ominaisuuden tyypit PHP ominaisuuden tyyppejä.

| .NET-ominaisuuden tyyppi | PHP ominaisuuden tyyppi | Huomautuksia                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tavu               | kokonaisluku           | -                                                                                                                                                                     |
| sbyte              | kokonaisluku           | -                                                                                                                                                                     |
| merkki               | Merkki              | Proton PHP luokan                                                                                                                                                    |
| lyhyt              | kokonaisluku           | -                                                                                                                                                                     |
| ushort             | kokonaisluku           | -                                                                                                                                                                     |
| kokonaisluku                | kokonaisluku           | -                                                                                                                                                                     |
| uint               | Kokonaisluku           | -                                                                                                                                                                     |
| pitkä               | kokonaisluku           | -                                                                                                                                                                     |
| ulong              | kokonaisluku           | -                                                                                                                                                                     |
| liukuluku              | Kaksinkertainen            | -                                                                                                                                                                     |
| Kaksinkertainen             | Kaksinkertainen            | -                                                                                                                                                                     |
| desimaaliluku            | merkkijono            | Desimaaliluku on ei tue Proton.                                                                                                                     |
| bool               | Totuusarvo           | -                                                                                                                                                                     |
| GUID-tunnus               | UUID              | Proton PHP luokan                                                                                                                                                    |
| merkkijono             | merkkijono            | -                                                                                                                                                                     |
| Päivämäärä ja aika           | kokonaisluku           | -                                                                                                                                                                     |
| DateTimeOffset-arvoa     | DescribedType     | DateTimeOffset.UtcTicks yhdistetty AMQP tyyppi:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Aikajakson           | DescribedType     | Timespan.Ticks yhdistetty AMQP tyyppi:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri yhdistetty AMQP tyyppi:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Vakio-ominaisuudet

Seuraavissa taulukoissa on Proton PHP viestin-ominaisuuksien ja [BrokeredMessage][] viestin ominaisuuksien yhdistämisen.

| Proton PHP           | Palvelun Bus .NET         | Huomautuksia                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Kestävät              | puuttuu                      | Palvelun Bus tukee vain kestävät viestejä.          |
| Priority (prioriteetti)             | puuttuu                      | Palvelun Bus tukee vain yhden viestin prioriteetti. |
| TTL (elinaika)                  | Message.TimeToLive       | Muunnoksen Proton PHP TTL on määritetty millisekunteina.   |
| ensimmäinen\_hankkijaosapuoli      | -                          | -                                                          |
| toimituksen\_määrä      | -                          | -                                                          |
| Tunnus                   | Message.Id               | -                                                          |
| käyttäjän\_tunnus             | -                          | -                                                          |
| Osoite              | Message.To               | -                                                          |
| Aihe              | Message.Label            | -                                                          |
| vastaa\_avulla            | Message.ReplyTo          | -                                                          |
| korrelaatio\_tunnus      | Message.CorrelationId    | -                                                          |
| sisällön\_tyyppi        | Message.ContentType      | -                                                          |
| sisällön\_koodaus    | puuttuu                      | -                                                          |
| määräajan\_aika         | Message.ExpiresAtUTC     | -                                                          |
| luomisen\_aika       | puuttuu                      | -                                                          |
| ryhmän\_tunnus            | Message.SessionId        | -                                                          |
| ryhmän\_järjestys      | -                          | -                                                          |
| vastaa\_,\_ryhmän\_tunnus | Message.ReplyToSessionId | -                                                          |
| Muotoile               | puuttuu                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Palvelun Bus .NET-ohjelmointirajapinnan Proton PHP avulla

| Palvelun Bus .NET        | Proton PHP                                             | Huomautuksia                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Viesti -\>sisällön\_tyyppi                                | -                                                        |
| CorrelationId           | Viesti -\>korrelaatio\_tunnus                              | -                                                        |
| EnqueuedTimeUtc         | Viesti -\>huomautukset [x-osallistua-enqueued-time]             | -                                                        |
| Otsikko                   | Viesti -\>aihe                                      | -                                                        |
| MessageId               | Viesti -\>tunnus                                           | -                                                        |
| ReplyTo                 | Viesti -\>vastaa\_avulla                                    | -                                                        |
| ReplyToSessionId        | Viesti -\>vastaa\_,\_ryhmän\_tunnus                         | -                                                        |
| ScheduledEnqueueTimeUtc | Viesti -\>huomautukset ["x-osallistua-ajoitettu-enqueue-time"] | -                                                        |
| Istunto               | Viesti -\>ryhmän\_tunnus                                    | -                                                        |
| TimeToLive              | Viesti -\>ttl                                          | Muunnoksen Proton PHP TTL on määritetty millisekunteina. |
| Jos haluat                      | Viesti -\>osoite                                      | -                                                        |

## <a name="next-steps"></a>Seuraavat vaiheet

Haluatko lisätietoja? Seuraavassa on seuraavissa linkeissä:

- [Palvelun Bus AMQP yleiskatsaus]
- [Windows Server Service Bus AMQP]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[Windows Server Service Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[Palvelun Bus AMQP yleiskatsaus]: service-bus-amqp-overview.md
