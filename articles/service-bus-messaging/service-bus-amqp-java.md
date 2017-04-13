<properties 
    pageTitle="Palvelun Bus ja Java kanssa AMQP 1.0 | Microsoft Azure"
    description="Java-palvelun Bus käyttäminen AMQP"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Java-palvelun Bus käyttäminen AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java viestin Service (JMS) on vakio Ohjelmointirajapinnan käyttäminen viestin välitysohjelmistoksi Java-ympäristössä. Microsoft Azure palvelun Bus testattu kanssa AMQP 1.0 perusteella JMS asiakkaan kirjaston kehittämä Apache Qpid projektin. Tämä kirjasto tukee koko JMS 1.1-Ohjelmointirajapinnan ja voidaan käyttää minkä tahansa AMQP 1.0 yhteensopiva tekstiviesti palvelu. Tässä skenaariossa tuetaan myös [Palvelun Bus Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) (paikallisen palvelun Bus). Lisätietoja on artikkelissa [AMQP palvelun Bus Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Lataa Apache Qpid AMQP 1.0 JMS asiakas-kirjasto

Saat lisätietoja uusimman version Apache Qpid JMS AMQP 1.0 asiakkaan kirjaston lataaminen seuraavasta [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

On lisättävä seuraavat neljä PURKKI tiedostot Apache Qpid JMS AMQP 1.0 jakauman arkistosta Java-SELAIMESI kun sekä JMS-sovellusten käytön palvelun Bus:

-   geronimo jms\_1.1\_.jar määritys-[versio]

-   qpid-amqp-1-0-Client-[version].JAR

-   qpid-amqp-1-0-Client-jms-[version].JAR

-   qpid-amqp-1-0-Common-[version].JAR

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Palvelun Bus olevien, aiheet ja tilaukset-JMS

### <a name="java-naming-and-directory-interface-jndi"></a>Java nimeämisestä ja käyttöliittymän Directory (JNDI)

JMS käytetään luomaan väliä looginen ja fyysinen nimet Java nimeämisestä ja hakemisto-liittymän (JNDI). Kahdentyyppisiä JMS objektien selvitetään käyttämällä JNDI: **ConnectionFactory** ja **kohde**. JNDI käyttää tarjoajan malli, johon voi kiinnittää eri hakemistopalvelujen käsittelemään nimi tarkkuus tehtävät. Apache Qpid JMS AMQP 1.0-kirjasto sisältää yksinkertaisen ominaisuudet tiedostopohjaisia JNDI-palvelu, joka on määritetty tekstitiedosto.

Qpid ominaisuudet tiedoston JNDI-palvelu on määritetty muotoa, ominaisuudet-tiedoston avulla:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Määritä yhteys factory

Voit määrittää **ConnectionFactory** Qpid ominaisuudet tiedoston JNDI palvelussa tapahtuma on seuraava kaava:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Jos `[jndi\_name]` ja `[ConnectionURL]` , on seuraava:

| Nimi            | Merkitys                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Yhteyden factory looginen nimi. Tämä nimi on korjattu Java-sovelluksen avulla JNDI `IntialContext.lookup()` menetelmää. |   |   |   |   |
| `[ConnectionURL]` | URL-osoite, joka tarjoaa JMS kirjaston AMQP broker tarvittavat tiedot.                                                      |   |   |   |   |

Muodon muuttaminen yhteys URL-osoite on seuraavanlainen:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Jos `[namespace]`, `[username]`, ja `[password]` , on seuraava:

| Nimi          | Merkitys                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Palvelun Bus nimitila saatu [Azure portal][].                      |   |   |   |   |
| `[username]`  | Palvelun Bus Suojaussidosten avaimen nimi saadaan [Azure portal][].                    |   |   |   |   |
| `[password]`  | URL-koodatun saatu [Azure portal][]-palvelun Bus Suojaussidosten avaimen lomake. |   |   |   |   |

> [AZURE.NOTE] Sinun täytyy URL-Osoitteen koodata salasanan manuaalisesti. Hyödyllisiä koodauksen URL-osoite-apuohjelma on saatavilla kohdassa [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

Esimerkiksi jos saatuja tietoja portaalissa on seuraavanlainen:

| Namespace:   | Test.servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Myöntäjän nimi: | RootManageSharedAccessKey                                        |
| Myöntäjä-näppäintä:  | abcdefg |

Valitse, jotta voidaan määrittää **ConnectionFactory** objektia `SBCONNECTIONFACTORY`, määritys-merkkijono on seuraavasti:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Määritä kohteisiin

Merkinnän, joka määrittää kohteen Qpid ominaisuudet tiedoston JNDI-palvelussa on seuraavanlainen:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Jos `[jndi\_name]` ja `[physical\_name]` , on seuraava:

| Nimi              | Merkitys                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Kohteen looginen nimi. Tämä on nimi selvitetään Java-sovelluksen avulla JNDI `IntialContext.lookup()` menetelmää. |
| `[physical\name]` | Palvelun Bus kohde, johon sovellus lähettää tai vastaanottaa viestit nimi.                                                  |

Ota seuraavat seikat huomioon:

- `[physical\name]` Arvo voi olla palvelun Bus jonossa tai aihe.
- Palvelun Bus aiheen tilauksesta vastaanotettaessa määritetyn JNDI fyysinen nimen tulisi olla aiheen nimi. Tilauksen nimi on annettu luotaessa kestävät tilauksen JMS sovelluksen koodissa.
- On myös mahdollista käsitellä palvelun Bus aiheen tilauksen JMS jonossa. On monia etuja tämän menetelmän: saman vastaanottaja-koodin avulla voidaan olevien ja aihe-tilaukset ja ominaisuudet-tiedostossa on externalized kaikki osoitetiedot (aihe ja tilauksen nimet).
- Käsitellä palvelun Bus aiheen tilauksen JMS jonossa, tapahtuman ominaisuudet-tiedostossa on oltava lomake: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Määritä nimeltä "AIHEEN", joka yhdistää nimeltä "aihe1" palvelun Bus aiheen looginen JMS kohde-tapahtuman ominaisuudet-tiedostossa on seuraavasti:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Lähettää viestiä, joissa JMS

Seuraava koodi näytetään, miten voit lähettää viestin palvelun Bus aiheen. Oletetaan, että `SBCONNECTIONFACTORY` ja `TOPIC` on määritetty **servicebus.properties** määritystiedostossa edellisessä kohdassa kuvatulla tavalla.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Vastaanottaa viestiä, joissa JMS

Seuraava koodi näkyy `how` haluat vastaanottaa viestin palvelun Bus aiheen tilaukseen. Oletetaan, että `SBCONNECTIONFACTORY` ja aihe on määritetty **servicebus.properties** määritystiedostossa edellisessä kohdassa kuvatulla tavalla. Sen lisäksi oletetaan, että tilauksen nimi on `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Ohjeet tehokkaat sovellusten luominen

JMS määrityksen määrittää, miten API menetelmien ja sovelluksen koodin poikkeus-sopimus on kirjoitettava käsittelemään tällaiset poikkeukset. Seuraavassa on joitakin poikkeuksen käsittelyä koskevat huomioitavia seikkoja:

-   Voit rekisteröidä **ExceptionListener** JMS yhteys **connection.setExceptionListener**. Näin asiakkaan ilmoitettava asynkronisesti ongelma. Tämä ilmoitus on erityisen tärkeää yhteydet, jotka käyttävät vain viestit, ne on ei voi kerrotaan, että niiden yhteys epäonnistui. Jos ongelma pohjana AMQP yhteyden, istuntoon tai linkkiä, nimi on **ExceptionListener** . Tässä tilanteessa sovelluksen tulee Luo **JMS yhteyden**, **istunnon**, **MessageProducer** ja **MessageConsumer** objektien alusta alkaen.

-   Varmista, että viesti on onnistuneesti lähetetty **MessageProducer** palvelun Bus-kohteeseen, varmista, että sovellus on määritetty **qpid.sync\_julkaista** järjestelmän ominaisuusjoukko. Voit tehdä tämän ohjelman ja käynnistämällä **-Dqpid.sync\_julkaista = true** Java AM asetus määrittää komentorivillä, kun sovellus käynnistetään. Tämä asetus määrittää kirjaston tuotto ei lähetä kutsu kunnes vahvistus on vastaanotettu, viesti on hyväksytty palvelun Bus. Jos ongelma ilmenee lähetystoiminto aikana, **JMSException** käynnistyy. On kaksi mahdollisia syitä: 
    1. Jos ongelma ei vuoksi palvelun Bus hylkäämällä tietty viesti on lähetetty, **MessageRejectedException** poikkeuksen korotetaan. Tämä virhe on näyttöominaisuusvaihtoehdon, tai viestin ongelmia vuoksi. Toiminnon suositellut kurssi on tehdä useita yrittää, yritä Edellinen käytöstä logiikkaa kanssa. Jos ongelma jatkuu, valitse viesti pitäisi jäädä ilman ylläpitäjää virheen kirjautunut paikallisesti. Ei ole tarpeen tilanne ** **JMS yhteyden**, tai **MessageProducer** **objektit uudelleen. 
    2. Jos ongelma ei vuoksi palvelun Bus sulkeminen AMQP linkkiä, **InvalidDestinationException** poikkeuksen korotetaan. Tämä voi olla lyhytkestoisia virheen vuoksi tai vuoksi viestin kohde poistetaan. Kummassakin tapauksessa **JMS yhteys** **istunnon**ja **MessageProducer** objektit on luotava uudelleen. Jos virhetilan lyhytkestoisia, valitse tämän toiminnon myöhemmin onnistuu. Jos kohde on poistettu, virheen poistetaan lopullisesti.

## <a name="messaging-between-net-and-jms"></a>.NET ja JMS välinen viestiliikenne

### <a name="message-bodies"></a>Viestien tekstit

JMS määrittää viisi eri viestin tyypit: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**ja **TextMessage**. Palvelun Bus .NET-Ohjelmointirajapinnan on yhden viestin tyyppi, [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS palvelun Bus .NET-Ohjelmointirajapinta

Seuraavissa osissa näyttää, miten tarjoaman kaikkien JMS viestin keskitetystä .NET viestejä. Esimerkki **ObjectMessage** ei ole sisältää, kun **ObjectMessage** leipätekstissä voi sarjoittaa objekti-Java-ohjelmointikielellä, joka ei ole interpretable .NET-sovelluksen.

##### <a name="bytesmessage"></a>BytesMessage

Seuraava koodi näytetään, miten tarjoaman **BytesMessage** objektin leipätekstiin palvelun Bus .NET-ohjelmointirajapinnan avulla.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Seuraava koodi näytetään, miten tarjoaman **MapMessage** objektin leipätekstiin palvelun Bus .NET-ohjelmointirajapinnan avulla. Tämä koodi iteroivaa kartan näyttäminen nimi ja muut elementit arvo osasta toiseen.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Seuraava koodi näytetään, miten tarjoaman **StreamMessage** objektin leipätekstiin palvelun Bus .NET-ohjelmointirajapinnan avulla. Koodi on luettelo kaikista virta ja niiden tiedostotyyppien kohteet.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Seuraava koodi näytetään, miten tarjoaman **TextMessage** objektin leipätekstiin palvelun Bus .NET-ohjelmointirajapinnan avulla. Tämä koodi näkyy viestin tekstiosaan sisältämien merkkijono.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Palvelun Bus .NET-ohjelmointirajapinnan JMS avulla

Seuraavissa osissa näytetään, miten .NET-sovelluksen voit luoda viestin, joka on vastaanotettu JMS kussakin JMS viestin erilaista. Esimerkki **ObjectMessage** ei ole sisältää, kun **ObjectMessage** leipätekstissä voi sarjoittaa objekti-Java-ohjelmointikielellä, joka ei ole interpretable .NET-sovelluksen.

##### <a name="bytesmessage"></a>BytesMessage

Seuraava koodi esitetään, kuinka voit luoda [BrokeredMessage][] objektin .NET, jossa on vastaanottanut JMS asiakkaan **BytesMessage**nimellä.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Seuraava koodi esitetään, kuinka voit luoda [BrokeredMessage][] objektin .NET, jossa on vastaanottanut JMS asiakkaan **StreamMessage**nimellä.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Seuraava koodi näyttää, miten tarjoaman **TextMessage** leipätekstiin palvelun Bus .NET-Ohjelmointirajapinnan käyttäminen. Tämä koodi näkyy viestin tekstiosaan sisältämien merkkijono.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Sovelluksen ominaisuudet

####<a name="jms-to-service-bus-net-apis"></a>JMS palvelun Bus .NET-ohjelmointirajapinnan

JMS viestien tukea sovelluksen seuraavia ominaisuuksia: **totuusarvo**, **tavu** **lyhyt**, **kokonaisluku**, **pitkä**, **liukuluku**, **double**tai **merkkijono**. Java-koodia näytetään, miten voit määrittää viestin ominaisuudet käyttämällä eri ominaisuus.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Palvelun Bus .NET-API viestin sovelluksen ominaisuuksien suoritetaan [BrokeredMessage][] **Ominaisuudet** -kokoelmaan. Seuraava koodi esitetään, kuinka voit lukea JMS asiakas vastaanotti viestin sovelluksen ominaisuuksia.

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

Seuraavassa taulukossa näkyy, kuinka JMS ominaisuuden tietotyypit yhdistetään .NET-ominaisuuden käyttämiseen.

| JMS ominaisuuden tyyppi | .NET-ominaisuuden tyyppi |
|-------------------|--------------------|
| Tavu              | sbyte              |
| Kokonaisluku           | kokonaisluku                |
| Liukuluku             | liukuluku              |
| Kaksinkertainen            | Kaksinkertainen             |
| Totuusarvo           | bool               |
| Merkkijono            | merkkijono             |

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

Java-koodia esitetään, kuinka voit lukea palvelun Bus .NET-asiakas vastaanotti viestin sovelluksen ominaisuuksia.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Seuraavassa taulukossa näkyy, kuinka .NET-ominaisuuden tietotyypit yhdistetään JMS ominaisuuden käyttämiseen.

| .NET-ominaisuuden tyyppi | JMS ominaisuuden tyyppi | Huomautuksia                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tavu               | UnsignedByte      | -                                                                                                                                                                      |
| sbyte              | Tavu              | -                                                                                                                                                                     |
| merkki               | Merkin         | -                                                                                                                                                                     |
| lyhyt              | Lyhyt             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| kokonaisluku                | Kokonaisluku           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| pitkä               | Pitkä              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| liukuluku              | Liukuluku             | -                                                                                                                                                                     |
| Kaksinkertainen             | Kaksinkertainen            | -                                                                                                                                                                     |
| desimaaliluku            | BigDecimal        | -                                                                                                                                                                     |
| bool               | Totuusarvo           | -                                                                                                                                                                     |
| GUID-tunnus               | UUID              | -                                                                                                                                                                     |
| merkkijono             | Merkkijono            | -                                                                                                                                                                     |
| Päivämäärä ja aika           | Päivämäärä              | -                                                                                                                                                                     |
| DateTimeOffset-arvoa     | DescribedType     | DateTimeOffset.UtcTicks yhdistetty AMQP tyyppi:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Aikajakson           | DescribedType     | Timespan.Ticks yhdistetty AMQP tyyppi:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri yhdistetty AMQP tyyppi:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Vakio-otsikot

Seuraavassa taulukossa esitetään, miten JMS vakio ylä- ja [BrokeredMessage][] vakio-ominaisuudet ovat AMQP 1.0 avulla.

#### <a name="jms-to-service-bus-net-apis"></a>JMS palvelun Bus .NET-ohjelmointirajapinnan

| JMS              | Palvelun Bus .NET               | Huomautuksia                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Tällä hetkellä käytettävissä        | Palvelun Bus tukee vain kestävät viestien; esimerkiksi DeliveryMode.PERSISTENT, riippumatta siitä, mitä on määritetty.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Viestin. TimeToLive            | Muuntaminen                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Oletusarvon mukaan JMSMessageID on koodattu binaarimuodossa AMQP viestissä. Saatuaan binaarinen viestin-tunnuksen .NET asiakkaan kirjaston muuntaa merkkijonoesitys tavujen unicode-arvojen perusteella. Voit siirtyä käyttämään merkkijonon viestin tunnukset JMS-kirjastoon, liittäminen "binaarinen messageid = false" JNDI ConnectionURL kyselyparametrit merkkijonon. Esimerkki: “amqps://[username]:[password]@[namespace].servicebus.windows.net? binaarinen messageid = false ". |
| JMSPriority      | Tällä hetkellä käytettävissä        | Palvelun Bus ei tue viestin prioriteetti.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Tällä hetkellä käytettävissä        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Viestin. ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Muuntaminen                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms-tyyppi"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Palvelun Bus .NET-ohjelmointirajapinnan JMS avulla

| Palvelun Bus .NET        | JMS              | Huomautuksia                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Tällä hetkellä käytettävissä |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Muuntaminen              |
| Otsikko                   | puuttuu              | Tällä hetkellä käytettävissä |
| MessageId               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | puuttuu              | Tällä hetkellä käytettävissä |
| ScheduledEnqueueTimeUtc | puuttuu              | Tällä hetkellä käytettävissä |
| Istunto               | puuttuu              | Tällä hetkellä käytettävissä |
| TimeToLive              | JMSExpiration    | Muuntaminen              |
| Jos haluat                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Ei-tuettuja ominaisuuksia ja rajoitukset

Seuraavat rajoitukset olemassa käytettäessä JMS AMQP 1.0 Bus palvelun kautta:

-   Vain yksi **MessageProducer** tai **MessageConsumer** sallitaan istunnossa. Jos haluat luoda sovelluksen **MessageProducer** tai **MessageConsumer** useita objekteja, Luo kullekin niistä oma istuntoja.

-   Muuttuvat aiheen tilaukset eivät ole tuettuja.

-   **MessageSelector** objekteja ei tueta.

-   Tilapäinen kohteisiin; esimerkiksi **TemporaryQueue** tai **TemporaryTopic**, ei tueta, sekä **QueueRequestor** ja **TopicRequestor** API, joka käyttää niitä.

-   Tapahtuma istuntojen ei tueta.

-   Jaetut tapahtumat ei tueta.

## <a name="next-steps"></a>Seuraavat vaiheet

Haluatko lisätietoja? Seuraavassa on seuraavissa linkeissä:

- [Palvelun Bus AMQP yleiskatsaus]
- [Windows Server Service Bus AMQP]

[Windows Server Service Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Palvelun Bus AMQP yleiskatsaus]: service-bus-amqp-overview.md
[Azure portal]: https://portal.azure.com
