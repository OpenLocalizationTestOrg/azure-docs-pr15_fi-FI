<properties 
    pageTitle="Kirjoita palvelun Bus olevien käyttävät sovellukset | Microsoft Azure"
    description="Viestin kirjoittaminen yksinkertainen jonon-pohjaisesta sovelluksesta, joka käyttää palvelun Bus."
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="create-applications-that-use-service-bus-queues"></a>Luo palvelun Bus olevien käyttävät sovellukset

Tässä ohjeaiheessa kerrotaan palvelun Bus olevien ja kerrotaan, miten voit kirjoittaa yksinkertaisen jonon-pohjaisesta sovelluksesta, joka käyttää palvelun Bus.

Harkitse jälleenmyynti, jossa on reititetty yksittäisten Point-of-Sale () kassapäätteiden myynnin tiedot varaston-hallintajärjestelmään milloin stock on täydennystä tietoja käyttävän maailman-skenaario. Tämä ratkaisu käyttää palvelun Bus viestintä päätteitä ja varasto-hallintajärjestelmään väliseen viestintään, kuten seuraavassa kuvassa:

![Palvelun Bus olevien kuva 1](./media/service-bus-create-queues/IC657161.gif)

Kunkin kassapääte raportit sen myyntitiedot lähettämällä viestejä **DataCollectionQueue**. Viestit säilyvät Tämä jono, kunnes varaston hallintajärjestelmän hakemisen. Tämä kaava on usein termillä *asynkronisen viestinvälityksen*, koska kassapääte ei ole Jatka käsittely varaston hallinta-järjestelmästä vastausta odotetaan.

## <a name="why-queuing"></a>Miksi queuing?

Ennen kuin Odotamme koodia, joita tarvitaan tämän sovelluksen määrittäminen, ota huomioon tässä skenaariossa sen sijaan, että Kassapäätteet jonon käyttämisen edut keskustella suoraan (synkronoidusti) varaston hallintajärjestelmään.

### <a name="temporal-decoupling"></a>Ajallinen irtautus

Asynkroninen tekstiviesti kuvion tuottajat ja kuluttajille ei tarvitse olla online samanaikaisesti. Tekstiviesti-infrastruktuuria tallentaa viestit luotettavasti, kunnes vievää osapuoli on valmiina vastaanottamaan ne. Tämä tarkoittaa jaetun sovelluksen osia voi irrottaa, joko vapaaehtoisesti; esimerkiksi ylläpito tai osan kaatumisen vuoksi ilman vaikuttavia koko järjestelmään. Lisäksi vievää sovellus voi vain on oltava online aikoina päivän aikana. Esimerkiksi jälleenmyynti tässä skenaariossa varaston hallintajärjestelmän vain ehkä tulee online liiketoiminnan päivän päätyttyä.

### <a name="load-leveling"></a>Kuormituksen tasaus

Monissa sovelluksissa järjestelmän kuormituksen vaihtelee ajan myötä käsittelyaika, kukin työmäärän yksikkö on yleensä vakio. Intermediating viestin tuottajat ja kuluttajille jonon kanssa tarkoittaa, että vievää (Liitos)-sovelluksen vain valmistellaan vastaamisessa keskimääräinen kuormituksen piikin kuormituksen sijaan. Jonossa syvyys kasvaa ja sopimuksen, kuten Saapuvan kuormituksen vaihtelee. Toiminto tallentaa suoraan rahaa infrastruktuurin vastaamisessa sovelluksen lataa tarvittavan määrän osalta.

![Palvelun Bus olevien kuva 2](./media/service-bus-create-queues/IC657162.gif)

### <a name="load-balancing"></a>Kuormituksen

Kun lataus kasvaa, Lisää työntekijä prosessit voidaan lisätä työntekijä jonon lukeminen. Kukin viesti käsitellään vain yksi työntekijä prosesseista. Lisäksi tässä salaus puretaan perustuva kuormituksen sallii optimaalisen käytön työntekijä tietokoneiden vaikka työntekijä tietokoneet eroavat toisistaan osalta käsittely power ne otetaan viestien etsiminen omia enimmäisnopeus. Tämä kaava on usein termillä kilpailevien kuluttaja-kuvion.

![Palvelun Bus olevien kuva 3](./media/service-bus-create-queues/IC657163.gif)

### <a name="loose-coupling"></a>Irtoaa kytkentä

Keskitaso viestin tuottajat ja kuluttajille sanomajonojärjestelmä sisältävät sisäisten irtoaa kytkentä osien välillä. Koska tuottajat ja käyttäjät eivät tiedä toisiinsa, kuluttaja voi päivittää käyttämättä mitään vaikutusta tuottaja. Tekstiviesti topologian voit lisäksi kehityttävä vaikuttamatta aiemmin päätepisteet. Käsittelemme Tämä lisää kun käsittelemme Julkaise ja tilaa.

## <a name="show-me-the-code"></a>Näytä koodi

Seuraavassa osassa kerrotaan, miten palvelun Bus avulla voit luoda tämän sovelluksen.

### <a name="sign-up-for-an-azure-account"></a>Azure tilin rekisteröiminen

Tarvitset Azure-tili, jotta voit aloittaa palvelun Bus. Jos sinulla ei ole yhtä, voit rekisteröityä maksuttoman tilin [tähän](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A85619ABF).

### <a name="create-a-namespace"></a>Luo nimitila

Kun tilaus, voit [luoda uuden nimitilan](service-bus-create-namespace-portal.md). Kunkin nimitilan toimii säilöön palvelun Bus kohteiden määrittäminen. Anna uusi nimitilan yksilöivä nimi, kaikki palvelun Bus tilien välillä. 

### <a name="install-the-nuget-package"></a>Asenna NuGet-paketti

Sovelluksen viitattava käyttämään palvelua Bus nimitilan palvelun Bus kokoonpanossa, erityisesti Microsoft.ServiceBus.dll. Voit etsiä tämän kokoonpanon osana Microsoft Azure SDK ja lataaminen on saatavilla kohdassa [Azure SDK lataussivulle](https://azure.microsoft.com/downloads/). [Palvelun Bus NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus) on kuitenkin helposti saat palvelun Bus Ohjelmointirajapinnan ja määrittää kaikki palvelun Bus riippuvuudet sovelluksen.

### <a name="create-the-queue"></a>Luo jonossa

Palvelun Bus messaging kohteiden (olevien ja Julkaise ja tilaa aiheet) hallintatoiminnot suoritetaan [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -luokan kautta. Palvelun Bus käyttää [Jaettu Access allekirjoitus (SAS)](service-bus-sas-overview.md) perusteella suojausmalli. [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) luokan edustaa suojaustunnuksen toimittaja joidenkin tunnetun suojaustunnuksen tarjoajien palvelua, joka palauttaa valmiin factory menetelmillä. Esitellään pidä SAS tunnistetietojen [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) menetelmän avulla. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) esiintymän muodostetaan sitten perusosoitteen palvelun Bus nimitilan ja suojaustunnuksen tarjoajan kanssa.

[NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) luokan sisältää tapaa luoda, Luetteloi ja poistaa tekstiviesti kohteiden. Tunnus, jolla on mukana näyttää, miten [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) esiintymä luodaan ja **DataCollectionQueue** jonossa luomiseen käytetyt.

```
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", 
                "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";
 
TokenProvider tokenProvider = 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = 
    new NamespaceManager(uri, tokenProvider);
namespaceManager.CreateQueue("DataCollectionQueue");
```

Huomaa, että Osastollasi [CreateQueue](https://msdn.microsoft.com/library/azure/hh322663.aspx) menetelmän, joiden avulla voidaan virittää jonon ominaisuuksia on. Voit esimerkiksi määrittää--elinaika (TTL) oletusarvo jonossa lähetettyihin viesteihin.

### <a name="send-messages-to-the-queue"></a>Viestien lähettäminen jonossa

Suorituksenaikainen toiminnoille palvelun Bus kohteiden; esimerkiksi lähettää ja vastaanottaa viestejä, sovellus on luotava [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) objekti. Kaltaisilta [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) luokan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) esiintymä luodaan perusosoitteen palvelun nimitila ja suojaustunnuksen toimittaja.

```
 BrokeredMessage bm = new BrokeredMessage(salesData);
 bm.Label = "SalesReport";
 bm.Properties["StoreName"] = "Redmond";
 bm.Properties["MachineID"] = "POS_1";
```

Lähetettyjä viestejä ja poistaa palvelun Bus olevien ovat [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) luokan esiintymät. Tähän luokkaan koostuu joukosta vakio-ominaisuuksia (esimerkiksi [otsikon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sanasto, jota käytetään sovelluksen ominaisuuksien ja haluamaansa sovelluksen tietoja tekstiosaan. Sovellus määrittää tekstiin mukaan kulkee (Seuraavassa esimerkissä välittää **SalesData** -objekti, joka vastaa myyntitiedot kassapääte) voi sarjoittaa objektissa, joka käyttää [System.Runtime.Serialization](https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx) onnistu objekti. Vaihtoehtoisesti voit toimittaa [Stream](https://msdn.microsoft.com/library/azure/system.io.stream.aspx) -objektin.

Viestien lähettäminen tietyn jonon, tässä tapauksessa **DataCollectionQueue**helpoin tapa on luoda [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) objektin suoraan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) esiintymän [CreateMessageSender](https://msdn.microsoft.com/library/azure/hh322659.aspx) avulla.

```
MessageSender sender = factory.CreateMessageSender("DataCollectionQueue");
sender.Send(bm);
```

### <a name="receiving-messages-from-the-queue"></a>Viestien vastaanottaminen jonossa

Vastaanottamaan viestejä jonossa, voit käyttää [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objekti, joka luodaan suoraan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx)avulla. Viestin vastaanottajia työskennellä kahta eri tilat: **ReceiveAndDelete** ja **PeekLock**. [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) määritetään, kun viestin vastaanottaja on luotu, parametrina [CreateMessageReceiver](https://msdn.microsoft.com/library/azure/hh322642.aspx) puheluun.


Kun käytät **ReceiveAndDelete** -tilassa, vastaanotto on yksi tuleva; toiminto Kun palvelun Bus saa pyynnön, se merkitsee viestin kulutettu käytössä-tilassa ja palauttaa sen sovelluksen. **ReceiveAndDelete** tila on helpoin malli, ja se toimii parhaiten tilanteissa, joissa sovellusta hyväksyt ei käsittele viesti, jos virhe ilmenee. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkitty viesti on kulutettu, kun sovellus käynnistyy ja aloittaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa vastaanotto on kaksivaiheinen-toimintoa, joka mahdollistaa sellaisten sovellusten, joka ei hyväksy puuttuvat viestit. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen kutsumalla [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) vastaanotetun viestin. Kun palvelun Bus näkee Soita [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) , se merkitsee viestin on käytetty.

Kaksi tulosten on mahdollista. Ensimmäisen kerran, jos sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua [hylätä](https://msdn.microsoft.com/library/azure/hh181837.aspx) viestin (sijaan [Valmis](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Tämä aiheuttaa palvelun Bus viestin lukituksen ja vastaanottaa uudelleen samaan kuluttaja tai toisen Viimeistellään kuluttaja käytettäväksi. Seuraavaksi on liittyvät lukituksen aikakatkaisu ja jos sovellus ei voi käsitellä viestiä lukitus ennen aikakatkaisun vanhentuu (esimerkiksi, jos sovellus kaatuu) ja valitse palvelu Bus tulee viestin lukituksen ja ottaa sen käyttöön uudelleen vastaanotettu (olennaisesti suorittaa [Hylkää](https://msdn.microsoft.com/library/azure/hh181837.aspx) oletusarvoisesti).

Huomaa, että jos sovellus kaatuu, kun se käsittelee viestin, mutta ennen [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) pyyntö lähetettiin, viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein termillä *Vähintään kerran* käsittely. Tämä tarkoittaa, että kustakin viestistä käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, Lisää logiikan tarvitaan sovelluksen kaksoiskappaleita. Tämä onnistuu viestin [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -ominaisuuden perusteella. Tämän ominaisuuden arvoa pysyy vakiona toimitusyritysten yli. Tätä kutsutaan termillä *Täsmälleen kerran* käsittely.

Tunnus, jolla on mukana vastaanottaa ja käsittelee **PeekLock** -tilassa, joka on oletusarvo, jos [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) arvoa ei ole nimenomaisesti viestin.

```
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionQueue");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

### <a name="use-the-queue-client"></a>Jono-asiakas

Esimerkkejä aiemmin tässä osassa luotu [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx) ja [MessageReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx) objektit suoraan [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) , voit lähettää ja vastaanottaa viestejä jonon tarpeen mukaan. Vaihtoehtoinen menetelmä on [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) luokan, mitkä tukee sekä lähettää ja vastaanottaa toimintojen lisäksi kehittyneempiä ominaisuuksia, kuten istuntoja.

```
QueueClient queueClient = factory.CreateQueueClient("DataCollectionQueue");
queueClient.Send(bm);
            
BrokeredMessage message = queueClient.Receive();

try
{
    ProcessMessage(message);
    message.Complete();
}
catch (Exception e)
{
    message.Abandon();
} 
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut olevien perusteet-kohdassa [Luo sovelluksia, jotka käyttävät palvelun Bus aiheet ja tilaukset](service-bus-create-topics-subscriptions.md) Jatka keskustelun käyttämällä palvelun Bus aiheet ja tilaukset Julkaise ja tilaa-ominaisuuksia.