<properties
    pageTitle="Voit käyttää palvelun Bus aiheet Java | Microsoft Azure"
    description="Opettele käyttämään palvelua Bus aiheet ja tilaukset Azure-tietokannassa. MALLIKOODEJA kirjoitetaan Java-sovelluksissa."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Palvelun Bus aiheet ja tilaukset käyttäminen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä oppaassa kerrotaan, miten palvelun Bus aiheet ja tilaukset. Mallit on kirjoitettu Java ja käytä [Azure SDK Java][]. Tilanteita, joissa kattaa ovat **luominen aiheet ja tilaukset**, **tilauksen suodattimien luomisesta**, **aiheen viestien lähettäminen**, **vastaanottaa viestejä tilauksesta**ja **poistaminen aiheet ja tilaukset**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Mitä palvelun Bus aiheet ja tilaukset?

Palvelun Bus aiheet ja tilaukset tukevat *Julkaise ja tilaa* messaging viestintä-malli. Kun käytät aiheet ja tilaukset, jaetun sovelluksen osia ei keskenään suoraan; sen sijaan ne vaihtaa viestejä ohjeaiheessa, johon välittämään kautta.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Palvelun Bus olevien, jossa jokaisen viestin käsitellään yksittäisen consumer in Contrast with aiheet ja tilaukset on "yksi-moneen-lomakkeen viestinnän, Julkaise ja tilaa kaavan. On voitu rekisteröidä useita tilauksia aiheeseen. Kun viesti on lähetetty aiheen, se tehdään sitten käytettävissä jokaisen tilauksen kahvaa/prosessin erikseen.

Aiheen tilannut muistuttaa virtual jono, joka vastaanottaa viestit, jotka on lähetetty aiheen kopioita. Voit halutessasi rekisteröidä suodatusehdot aiheen tilausta kohti välein, jonka avulla voit suodattimen tai rajoittaa aiheen viestit on vastaanottanut aiheen päättyneet tilaukset.

Palvelun Bus aiheet ja tilaukset, joiden avulla voit käsitellä välillä on erittäin suuri määrä käyttäjiä ja sovellusten erittäin paljon viestejä skaalata.

## <a name="create-a-service-namespace"></a>Luo palvelun nimitila

Kun haluat käyttää palvelun Bus aiheet ja tilaukset Azure, sinun on luotava palvelun nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa.

Voit luoda nimitilan seuraavasti:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Varmista, että olet asentanut [Azure SDK Java][] ennen tämän mallin luomisesta. Jos käytät Pimennys, voit asentaa [Azure työkalujen Pimennys varten][] , joka sisältää Java Azure SDK-paketissa. Voit lisätä **Microsoft Azure-kirjastojen Java** sitten projektin:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Lisää tuo seuraavista väittämistä Java-tiedoston alkuun:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Java Azure-kirjastojen lisääminen muodosta polku ja Sisällytä se projektin käyttöönotto-kokoonpanossa.

## <a name="create-a-topic"></a>Aiheen luominen

Palvelun Bus aiheet hallintatoiminnot voivat tehdä **ServiceBusContract** -luokan kautta. **ServiceBusContract** objektin muodostetaan sopiva kokoonpanossa, kapseloi SAS tunnuksen ja hallita sitä oikeudet ja **ServiceBusContract** -luokka on ainoa Azure tietoliikenteen.

**ServiceBusService** -luokka on tapoja luoda ja luetteloida poistaminen ohjeita. Seuraavassa esimerkissä esitetään, miten **ServiceBusService** objektin avulla voidaan nimeltä aiheen luominen `TestTopic`, nimitila, jota kutsutaan `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

On **TopicInfo** menetelmiä, jotka mahdollistavat asettaminen aiheen ominaisuuksia (esimerkiksi: Määritä time-to-live (TTL) oletusarvo aiheen lähetettyihin viesteihin käytetään). Seuraavassa esimerkissä luomisesta nimeltä aiheen `TestTopic` koko on enintään 5 gt: N kanssa:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Huomaa, että voit käyttää **listTopics** menetelmä **ServiceBusContract** objektit tarkistamaan, jos aiheen, määritetty nimi on jo palvelun nimitila.

## <a name="create-subscriptions"></a>Luo tilaukset

Tilaukset aiheisiin luodaan myös **ServiceBusService** -luokka. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa tietuejoukon tilauksen virtual jonon välitetään viestejä.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

**MatchAll** suodatin on oletussuodatin, jota käytetään, jos ei suodatusta määritetään, kun luodaan uusi tilaus. Kun **MatchAll** -suodatin on käytössä, kaikki viestit, jotka on julkaistu aiheeseen sijoitetaan tilauksen virtual jonon. Seuraava esimerkki luo tilausta nimeltä "AllMessages" ja käyttää **MatchAll** oletussuodatin.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös luoda suodattimia, joiden avulla voit laajuus, joiden aihe lähetetyt viestit näytetään tiettyä aihetta-tilauksen piiriin kuuluvien.

Tuettu tilauksissa suodattimen eniten joustavia tyyppi on [SqlFilter][], joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Lisätietoja lausekkeista, jotka voidaan käyttää SQL-suodatin Tarkista [SqlFilter.SqlExpression][] syntaksi.

Seuraavassa esimerkissä luodaan tilausta nimeltä `HighMessages` [SqlFilter][] objektia, jonka valitsee vain viestit, joissa on suurempi kuin 3 mukautetun **MessageNumber** ominaisuuden kanssa:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä `LowMessages` [SqlFilter][] -objektia, joka valitsee vain viestit, joiden **MessageNumber** ominaisuus pienempi tai yhtä 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Kun viesti on nyt lähetetty `TestTopic`, se toimitetaan aina tilannut vastaanottajia `AllMessages` -tilaukseen ja valikoivasti toimittaa tilannut vastaanottajia `HighMessages` ja `LowMessages` tilaukset (sen mukaan, viestin sisältöä).

## <a name="send-messages-to-a-topic"></a>Viestien lähettäminen aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen hakee **ServiceBusContract** objekti. Seuraava koodi näytetään, miten voit lähettää viestin `TestTopic` aiheen aiemmin luotu sisällä `HowToSample` nimitilan:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Bus ohjeaiheisiin lähetetyt viestit ovat [BrokeredMessage][] luokan esiintymät. [BrokeredMessage][] *objekteissa on joukko tavallisia menetelmiä (kuten * *setLabel* * ja * *TimeToLive**), joka sanasto, jota käytetään pidä mukautetun sovelluksen kielikohtaiset ominaisuudet ja haluamaansa hakemuksen tiedot. Sovellus määrittää viestin tekstiosaan välittämällä voi sarjoittaa objektin tuominen [BrokeredMessage][]ja haluamasi konstruktoria * *System.Runtime.Serialization* * käytetään onnistu objekti. Voit myös * *java.io.InputStream** voidaan suorittaa.

Seuraavassa esimerkissä viisi testata viestien lähettäminen `TestTopic` **MessageSender** on saatu edellä koodikatkelman.
Huomaa, miten kukin viesti **MessageNumber** ominaisuuden arvo vaihtelee silmukan iteraation (tämä määritetään päättyneet tilaukset perille):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa kokoa on määritetty luominen aikaa, yläraja on 5 gt.

## <a name="how-to-receive-messages-from-a-subscription"></a>Voit vastaanottaa viestejä tilauksesta

Voit vastaanottaa viestejä tilaukseen, käytä **ServiceBusContract** objektia. Vastaanotettujen viestien työskennellä kahta eri tilat: **ReceiveAndDelete** ja **PeekLock**.

Kun **ReceiveAndDelete** -tilassa on yksittäinen kuva - toiminto, kun palvelua Bus vastaanottaa lukukuittauksen viestin, se merkitsee viestin parhaillaan kulutettu ja palauttaa sen sovelluksen. **ReceiveAndDelete** tila on helpoin malli, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa vastaanottaa tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen soittamalla vastaanotetun viestin **poistaminen** . Palvelun Bus näkee **Poista** kutsu, kun se Merkitse viesti on kulutettu-tilassa ja poista se aihetta.

Alla olevassa esimerkissä esitetään, miten viestit voi vastaanottaa ja käsitellä **PeekLock** -tilassa (ei oletustila). Alla olevassa esimerkissä ympyrä tekee ja käsittelee viestien "HighMessages" tilaus ja sitten sulkeutuu, kun ei lisää viestejä (Voit myös sen voitiin odottamaan uudet viestit).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää vastaanotetun viestin (sijaan **deleteMessage** -menetelmä). Tämä aiheuttaa palvelun Bus sisällä aihetta viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu sisällä aihetta viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu), valitse palvelun Bus viestin lukituksen automaattisesti ja lisätään uudelleen Vastaanotettava.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -pyyntö annetaan, valitse viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **getMessageId** -menetelmällä.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Ensisijainen tapa poistaa aiheet ja tilaukset kannattaa käyttää **ServiceBusContract** objektia. Aiheen poistaminen poistaa myös tilaukset, jotka on rekisteröity aihetta. Tilauksia voi poistaa myös erikseen.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus olevien perusteet, katso lisätietoja [palvelun Bus olevien, aiheet, ja tilaukset][] .

  [Java Azure SDK]: http://azure.microsoft.com/develop/java/
  [Azure työkalujen Pimennys varten]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Palvelun Bus olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
