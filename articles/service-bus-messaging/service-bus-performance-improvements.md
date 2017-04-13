<properties 
    pageTitle="Parhaat käytännöt käyttäen palvelua Bus suorituskyvyn parantaminen | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure palvelun Bus avulla voit optimoida suorituskyvyn, kun se viestien vaihtaminen."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Suorituskykyä on parannettu käyttäen palvelua Bus Messaging parhaat käytännöt

Tässä ohjeaiheessa kerrotaan, miten suorituskyvyn parantamiseksi, kun se viestien vaihtaminen Azure Bus Messaging avulla. Ensimmäinen osa on tässä ohjeaiheessa kuvataan erilaisia menetelmiä, jotka tarjotaan suorituskyvyn parantamiseksi. Toinen osa on ohjeita käyttämisestä palvelun Bus niin, että voit tarjota parhaan suorituskyvyn annetun skenaariossa.

Tässä ohjeaiheessa koko termi "asiakas" viittaa kohteeseen, joka käyttää palvelun Bus. Asiakas voi kestää lähettäjän tai vastaanottajan rooli. Termin "lähettäjä" käytetään palvelun Bus jonossa tai aiheen asiakas, joka lähettää viestejä palvelun Bus jonossa tai aihe. Termin "vastaanottaja" viittaa palvelun Bus jonossa ja tilauksen asiakas, joka vastaanottaa viestejä palvelun Bus jonossa ja tilauksen.

Nämä osat esitellä useita käsitteestä, jotka palvelun Bus käyttää Tehosta suorituskykyä.

## <a name="protocols"></a>Protokollat

Palvelun Bus avulla voit lähettää ja vastaanottaa viestejä kolme protokollat kautta asiakkaat

1. Lisäasetukset sanomajonojärjestelmä Protocol (AMQP)

2. Palvelun Bus Messaging Protocol (SBMP)

3. HTTP

AMQP ja SBMP ovat tehokkaampaa, koska ne ylläpitävät palvelun Bus yhteys, kunhan tekstiviesti factory on olemassa. Se ottaa käyttöön myös jonottaminen ja prefetching. Ellet nimenomaisesti edellä, kaikki tämän ohjeaiheen sisältö olettaa AMQP tai SBMP.

## <a name="reusing-factories-and-clients"></a>Tehtaan ja asiakkaat

Palvelun Bus asiakkaan objektit, kuten [QueueClient][] tai [MessageSender][]luodaan [MessagingFactory][] -objekti, joka sisältää myös sisäisten yhteyksien kautta. Olisi ei sulje tekstiviesti tehtaan tai jonossa, aihe ja tilauksen asiakkaiden jälkeen voit lähettää viestin, ja luo uudelleen ne lähettäessäsi seuraavan viestin. Tekstiviesti factory sulkeminen poistaa yhteyden palvelun Bus-palveluun ja uusi yhteys on muodostettu, kun luotaessa valmiiksi. Yhteyden muodostamisesta on kallista toiminnon, jota voit välttää käyttämällä samaa factory ja useita toimintoja asiakkaan objektit uudelleen. Voit käyttää turvallisesti [QueueClient][] objektin samanaikainen asynkronisten toimintojen ja useita viestiketjuissa siirtyminen viestien lähettämistä varten. 

## <a name="concurrent-operations"></a>Rinnakkaiset toiminnot

Suorittaa (lähetetään, saavat, poistaa, jne.) kestää jonkin aikaa. Tällä hetkellä sisältää toiminnon käsittelyn palvelun Bus-palvelun viive pyynnön ja vastauksen lisäksi. Kun haluat lisätä toimintoja, ajan, toiminnot on suoritettava samanaikaisesti. Voit tehdä tämän usealla eri tavalla:

-   **Asynkronisten toimintojen**: asiakkaan ajoittaa toimintojen suorittamalla asynkroninen toimintoja. Seuraava pyyntö käynnistetään, ennen kuin edelliseen pyyntö on valmis. Seuraavassa on esimerkki asynkroninen lähetystoiminto:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Tämä on esimerkki asynkroninen vastaanotto toiminto:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Useita tehtaan**: kaikki asiakkaat (lähettäjien lisäksi vastaanottajia), jotka on luotu saman factory jakaa yhden TCP-yhteyden. Viestin siirtonopeuden on rajoitettu verran toiminnoista, joita voit siirtyä tämän TCP-yhteyden kautta. Siirtonopeuden, joka saadaan yksi factory vaihtelee huomattavasti TCP vastauksen työajat ja viestin koko. Saadakseen suurempi siirtonopeuden käytettävä useita tekstiviesti tehtaan.

## <a name="receive-mode"></a>Saat tila

Luodessasi jonossa ja tilauksen asiakkaan voit määrittää vastaanotto-tilassa: *Pikanäkymä Lukitse* tai *vastaanottaminen ja poista*. Oletusarvo vastaanottaa tila on [PeekLock][]. Kun tässä tilassa, asiakastietokone lähettää pyynnön sanoma palvelun Bus. Kun asiakas on vastaanottanut viestin, se lähettää pyynnön suorittamiseen viestin.

Määrittäessään receive-tilassa [ReceiveAndDelete][]molemmat vaiheet yhdistetään yksittäisen pyynnön. Tämä vähentää yleistä toiminnoista ja parantaa yleistä viestin siirtonopeuden. Tämä suorituskyvyn voitto tulee sen vastuulla menettämättä viestejä.

Palvelun Bus eivät tue tapahtumat vastaanottaa-ja-delete-toimintoja. Lisäksi pikanäkymä Lukitse semantiikkaan liittyvien tarvitaan haluamasi skenaariot, jotka asiakas haluaa myöhemmäksi tai [Perille toimittamattomat](service-bus-dead-letter-queues.md) viestiin.

## <a name="client-side-batching"></a>Asiakkaan jonottaminen

Asiakkaan jonottaminen mahdollistaa jonossa tai aiheen asiakkaan viivyttää viestin lähettämistä varten tietyn ajan kuluessa. Jos asiakas lähettää uusien viestien tänä aikana, se lähettää yhden erän viestit. Asiakkaan jonottaminen aiheuttaa myös jonon/tilauksen asiakkaan erän useita **valmiina** pyyntöjä yksittäisen pyynnön kyselyjä. Jonottaminen on käytettävissä vain asynkroninen **Lähetä** ja **kokonais** operations. Synkronoitujen toimintojen lähetetään välittömästi palvelun Bus-palveluun. Jonottaminen ei toteutuvat pikanäkymä tai vastaanottaa toimintojen eikä jonottaminen ilmeneekö asiakkaiden yli.

Jos erän ylittää viestin enimmäiskoko, erä poistetaan viimeisen viestin ja asiakas lähettää erän heti. Viimeisen viestin tulee seuraava erä ensimmäisen viestin. Asiakas käyttää oletusarvon mukaan 20ms erä ajanjakson. Voit muuttaa erän väli määrittämällä [BatchFlushInterval][] -ominaisuuden, ennen kuin luot tekstiviesti factory. Tämä asetus koskee kaikkia asiakkaita, jotka on luotu Tämä tehdas.

Voit poistaa käytöstä jonottaminen, aseta [BatchFlushInterval][] -ominaisuuden arvoksi **TimeSpan.Zero**. Esimerkki:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Jonottaminen ei vaikuta laskutettavan viestinnän toimintoja ja on käytettävissä vain palvelun Bus client-protokolla. HTTP-protokolla ei tue jonottaminen.

## <a name="batching-store-access"></a>Jonottaminen kaupan access

Lisäämiseksi jonon/aiheen/tilauksen siirtonopeuden palvelun Bus erien useiden viestien kirjoittaessaan sen sisäinen säilöön. Jos käytössä jonossa tai aiheen, viestien kirjoittaminen säilöön erämuotoinen. Jos käytössä jonossa ja tilauksen, viestien poistaminen kaupasta erämuotoinen. Jos kohteen on käytössä oletusmäärä kaupan access-palvelun Bus viivyttää store-kirjoitus-toimintoa koskevat kyseisen kohteen, enintään 20ms mukaan. Lisää säilö toiminnoista, joita aikana aikaväliä lisätään erän. Oletusmäärä kaupan access koskee vain **Lähetä** ja **kokonais** operations; Saat toiminnot eivät vaikuta. Oletusmäärä kaupan access on kohteen ominaisuuden. Jonottaminen ilmenee kaikki kohteet, jotka mahdollistavat oletusmäärä kaupan access yli.

Kun luot uuden jonon, aiheen tai tilauksen, oletusmäärä kaupan access on käytössä oletusarvoisesti. Käytöstä oletusmäärä kaupan access, aseta [EnableBatchedOperations][] -ominaisuuden arvoksi **Epätosi** , ennen kuin luot kohteen. Esimerkki:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Oletusmäärä kaupan access ei vaikuta laskutettavan viestinnän toimintoja ja on jonossa, aiheen tai tilaus-ominaisuus. On erillinen vastaanottotila- ja protokolla, jota käytetään asiakkaan ja palvelun Bus-palvelun välillä.

## <a name="prefetching"></a>Prefetching

Prefetching mahdollistaa jonossa ja tilauksen-asiakasohjelman lataaminen uusien viestien-palvelusta, kun se on suorittanut vastaanota-toimintoa. Asiakkaan tallentaa viestit paikallisesta välimuistista. Välimuistin koko määräytyy [QueueClient.PrefetchCount][] tai [SubscriptionClient.PrefetchCount][] ominaisuudet. Kukin asiakas, joka mahdollistaa prefetching ylläpitää omassa välimuistin. Välimuistia ei jaetaan asiakkaille. Jos asiakas aloittaa vastaanota-toimintoa ja sen välimuistin on tyhjä, palvelu lähettää viestejä erän. Erän kokoa on välimuistin tai 256 kt koon, kumpi on pienempi. Jos asiakas aloittaa vastaanota-toimintoa ja välimuistin sisältää viestin, viestin otetaan välimuistista.

Kun viesti on prefetched-palvelun lukitsee prefetched viestin. Näin eri vastaanottaja ei voi vastaanottaa prefetched viestin. Jos vastaanottaja saa suoritettua viesti, ennen kuin lukitus vanhenee, viesti ei ole käytettävissä muita vastaanottajia. Prefetched kopio, viesti säilyy välimuistin. Vastaanottajan, joka käyttää vanhentuneet välimuistissa saavat poikkeuksen, kun se yrittää viestin loppuun. Oletusarvon mukaan viestin lukitus vanhenee 60 sekunnin kuluttua. Tämän arvon laajennettavissa 5 minuuttia. Voit estää vanhentuneet sähköpostiviestit kulutus välimuistin kokoa on aina oltava pienempi kuin asiakas voidaan kulutettu Lukitse aikakatkaisuajan kuluessa viestien määrän.

Oletusarvoinen Lukitse vanhenemista 60 sekuntia käytettäessä [SubscriptionClient.PrefetchCount][] hyvä arvo on 20 kertaa suurin käsittely myönnettävän tehdas kaikki vastaanottajia. Esimerkiksi factory Luo 3 vastaanottajia ja kunkin vastaanottaja voi käsitellä sekunnissa enintään 10 viestejä. Esihaun Laske enintään 20\*3\*10 = 600. Oletusarvon mukaan [QueueClient.PrefetchCount][] asetetaan 0, mikä tarkoittaa, että uusia viestejä ei haetut-palvelusta.

Yleistä siirtonopeuden jonossa tai tilauksen prefetching viestien kasvaa, koska se vähentää viestin toimintojen tai PYÖRISTÄ-funktiota trips kokonaismäärästä. Haetaan ensimmäisen viestin kuitenkin kestää kauemmin (vuoksi parantavat viestin koko). Prefetched viestien vastaanottamiseen ovat nopeampi, koska nämä viestit on jo ladattu asiakas.

Time-to-live (TTL)-ominaisuuden viestin on kuitannut palvelimen palvelin lähettää viestin asiakkaan aikaa. Asiakas ei tarkista viestin TTL-ominaisuuden viestin saapuessa. Viestin voi vastaanottaa, vaikka viestin Elinaika on ohitettu, kun viesti on välimuistissa asiakas.

Prefetching ei vaikuta laskutettavan viestinnän toimintoja ja on käytettävissä vain palvelun Bus client-protokolla. HTTP-protokolla ei tue prefetching. Prefetching on käytettävissä sekä synkronoidusti ja asynkroninen vastaanotto toimintoja.

## <a name="express-queues-and-topics"></a>Express olevien ja aiheet

Expressin kohteiden käyttöön suuri siirtonopeuden ja vähentää viive skenaarioita. Pika-objektien kanssa Jos viesti on lähetetty jonossa tai aiheen viestiä ei ole heti tallennettu tekstiviesti-kaupan. Sen sijaan se välimuistissa. Jos viesti jää jonossa yli joitakin sekunteja, se automaattisesti kirjoitetaan vakaa tallennus-suojaaminen näin katoamiselta käyttökatkosta vuoksi. Viestin kirjoittaminen muistin välimuistiin kasvattaa siirtonopeuden ja vähentää viive, koska ei ei voi käyttää tallennustilan vakaa viesti lähetetään aikaan. Viestit, joissa on käytetty muutamassa ei ole kirjoitettu tekstiviesti-kaupasta. Seuraavassa esimerkissä luodaan express aihe.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Jos viestiin, joka sisältää tärkeitä tietoja, jotka eivät saa olla menettää lähetetään pika-kohteeseen, lähettäjän voit pakottaa palvelun Bus säilyttää vakaa tallennustilan määrittämällä [ForcePersistence][] -ominaisuuden arvoksi **true**viestin heti.

## <a name="use-of-partitioned-queues-or-topics"></a>Osioitu olevien tai aiheet

Sisäisesti-palvelun Bus käyttää samaa solmu ja messaging tallentaa käsittelemään ja tallentaa kaikki viestit tekstiviesti kohteen (jonossa tai aihe). Osioitu jonossa tai aiheen toisaalta, jaetaan useiden solmujen ja tekstiviesti stores. Osioitu olevien ja aiheet ei vain tuotto suurempi siirtonopeuden kuin tavallinen olevien ja ohjeita, ne toimia päätason käytettävyys. Luomaan osioitua kohteen ominaisuuden [EnablePartitioning][] **Tosi**, kuten seuraavassa esimerkissä. Saat lisätietoja osioitua kohteiden [osioitu tekstiviesti kohteita][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Useita olevien käyttäminen

Jos ei ole mahdollista osioitua jonossa tai aiheen tai odotettu kuormituksen ei voi käsitellä yksittäisen osioitua jonossa tai ohjeaiheen, sinun on käytettävä useiden tekstiviesti kohteiden. Useiden kohteiden käytettäessä luomalla kullekin objektille on sen sijaan, että kaikki kohteet samaan käyttävä erillinen asiakkaan.

## <a name="development--testing-features"></a>Kehittämisen ja testauksen toiminnot

Palvelun Bus on yksi ominaisuus, joka on käytetty kehitystä varten minkä **ei koskaan voi käyttää tuotannon määrityksiä**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Kun uusi sääntöjä tai suodattimia lisätään ohjeaiheen, EnableFilteringMessagesBeforePublishing voidaan tarkistamaan, että uusi suodatin-lausekkeen toimii odotetulla tavalla.

## <a name="scenarios"></a>Skenaariot

Seuraavissa osissa kuvaillaan tavallista tekstiviesti mahdollisuutta, sekä jäsentäminen ensisijaiset palvelun Bus-asetukset. Siirtonopeuden on luokiteltu mahdollisimman pieni (alle 1 viestin/toisen), valvominen (1 viestin/toisen tai suurempi, mutta pienempi kuin 100 viestien/toisen) ja suuri (100 viestien/toisen tai uudempi). Asiakkaiden määrä luokitellaan pieni (5 tai vähemmän), keskitaso (enemmän kuin 5 mutta 20), ja suurten (yli 20).

### <a name="high-throughput-queue"></a>Suuri siirtonopeuden jonossa

Tavoite: Suurenna yhden jonon siirtonopeuden. Lähettäjien ja vastaanottajien määrä on pieni.

-   Käytä osioitua jonon parannettu suorituskyky ja käytettävyyden.

-   Yleistä Lähetä nopeutta kyselyjä jonossa parantamiseksi Luo lähettäjien useita viestin tehtaan avulla. Käytä asynkroninen toimintoja tai useita viestiketjuissa siirtyminen kullekin lähettäjälle.

-   Yleistä vastaanota nopeutta, jonossa parantamiseksi Luo vastaanottajia useita viestin tehtaan avulla.

-   Asynkronisten toimintojen avulla voit hyödyntää asiakkaan jonottaminen.

-   Määritä batching välin 50ms palvelun Bus asiakkaan protokolla lähetysten määrää. Jos useita lähettäjät ovat käytössä, suurentaa sen batching välin 100 millisekunnin yksikköinä.

-   Jätä oletusmäärä kaupan access käytössä. Tämä suurentaa yleistä nopeutta, jolla voidaan kirjoittaa viestejä jonossa kyselyjä.

-   Määritä esihaun Laske 20 kertaa suurin käsittely myönnettävän factory kaikki vastaanottajia. Tämä pienentää palvelun Bus asiakkaan protokolla lähetysten määrä.

### <a name="multiple-high-throughput-queues"></a>Useita suuren siirtonopeuden olevien

Tavoite: Suurenna useita olevien yleinen siirtonopeuden. Yksittäisten jonon on Normaali tai suuri.

Saadakseen suurin nopeus yli useita olevien Suurenna yhden jonon siirtonopeuden kuvattujen asetusten avulla. Lisäksi luoda asiakkaiden, joka lähettää tai vastaanottaa eri olevien eri tehtaan avulla.

### <a name="low-latency-queue"></a>Pieni viive jonossa

Tavoite: Pienennä lopusta loppuun viive jonossa tai aihe. Lähettäjien ja vastaanottajien määrä on pieni. Jonon on small Business- tai Normaali.

-   Käytä osioitua jonon parannettu käytettävyys.

-   Poista käytöstä asiakkaan jonottaminen. Asiakastietokone lähettää viestin heti.

-   Poista oletusmäärä store-tila käytöstä. Palvelun kirjoittaa viestin heti kauppa.

-   Jos yhteen asiakas-arvoksi 20 kertaa vastaanotin käsittely diskonttokorko esihaun määrä. Useiden viestien vuoroon jonossa samaan aikaan, jos palvelun Bus client-protokolla välittää niitä yhtä aikaa. Asiakkaan saa seuraavan sanoman, kun viesti on jo paikallisen välimuistin. Välimuistin pitäisi olla pieni.

-   Jos käytät useita asiakkaita, arvoksi 0 esihaun määrä. Näin toisen asiakkaan saavat toinen virhesanoma, kun ensimmäinen asiakas käsittelee yhä ensimmäisen viestin.

### <a name="queue-with-a-large-number-of-senders"></a>Jonon lähettäjien suuri määrä

Tavoite: Suurenna siirtonopeuden jonossa tai aiheen lähettäjien suuri määrä. Kullekin lähettäjälle lähettää viestit, joissa on Normaali korvaus. Vastaanottajia määrä on pieni.

Palvelun Bus mahdollistaa enintään 1 000 samanaikaisia yhteyksiä tekstiviesti kohteeseen (tai 5000 käyttämällä AMQP). Tämä rajoitus on voimassa nimitilan tasolla ja olevien/aiheet/tilaukset ovat aikana samanaikaista yhteyttä kohti nimitilan raja mukaan. Olevien tämä luku jaetaan lähettäjien ja vastaanottajien välillä. Jos kaikki 1000 yhteydet tarvitaan lähettäjät, korvattava jonossa aiheen ja yksi tilaus. Aiheen hyväksyy enintään 1 000 samanaikaisia yhteyksiä lähettäjien, olisi tilaus hyväksyy muita 1000 samanaikainen yhteydet vastaanottajia. Jos yli 1 000 samanaikainen lähettäjät ovat pakollisia, lähettäjien pitäisi lähettää viestejä HTTP palvelun Bus-protokollaa.

Voit suurentaa siirtonopeuden, toimi seuraavasti:

-   Käytä osioitua jonon parannettu suorituskyky ja käytettävyyden.

-   Jos kullekin lähettäjälle sijaitsee toinen prosessi, käytä vain yhden factory prosessia kohti.

-   Asynkronisten toimintojen avulla voit hyödyntää asiakkaan jonottaminen.

-   Vähennä palvelun Bus asiakkaan protokolla lähetysten jonottaminen 20ms aikaväli oletusarvo avulla.

-   Jätä oletusmäärä kaupan access käytössä. Tämä suurentaa yleistä nopeutta, jolla voidaan kirjoittaa viestejä jonossa tai aihe.

-   Määritä esihaun Laske 20 kertaa suurin käsittely myönnettävän factory kaikki vastaanottajia. Tämä pienentää palvelun Bus asiakkaan protokolla lähetysten määrä.

### <a name="queue-with-a-large-number-of-receivers"></a>Jonon vastaanottajia suuri määrä

Tavoite: Suurenna vastaanota korko jonossa tai tilaus on paljon vastaanottajia. Kunkin vastaanottajan vastaanottaa viestit Normaali korvaus-palvelussa. Lähettäjien määrä on pieni.

Palvelun Bus mahdollistaa enintään 1 000 samanaikaisia yhteyksiä kohteeseen. Jos jonon vaatii yli 1 000 vastaanottajia, aiheen ja useita tilauksia korvattava jonossa. Jokaisen tilauksen tukee enintään 1 000 samanaikaisia yhteyksiä. Voit myös vastaanottajia voi käyttää jonossa HTTP-protokollan kautta.

Voit suurentaa siirtonopeuden, toimi seuraavasti:

-   Käytä osioitua jonon parannettu suorituskyky ja käytettävyyden.

-   Jos kunkin vastaanottajan sijaitsee toinen prosessi, käytä vain yhden factory prosessia kohti.

-   Vastaanottajia voi käyttää painikkeen tai asynkroninen varten. Yksittäisen vastaanottajan Keskitaso vastaanota korkokannan ottaen asiakkaan jonottaminen valmis pyynnön ei vaikuta vastaanotin siirtonopeuden.

-   Jätä oletusmäärä kaupan access käytössä. Tämä pienentää yleinen kuormituksen kohteen. Se myös pienentää yleistä nopeutta, jolla voidaan kirjoittaa viestejä jonossa tai aihe.

-   Aseta esihaun Laske pieni arvo (esimerkiksi PrefetchCount = 10). Tämä estää vastaanottajia ei tapahdu, kun muita vastaanottajia on paljon välimuistiin tallennetut viestit.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Aihe on pieni useita tilauksia

Tavoite: Suurenna siirtonopeuden on vähän tilauksista aihe. Viesti on vastaanottanut monta tilausta, mikä tarkoittaa, että kaikkien tilausten päälle yhdistetyn vastaanotto-korko on suurempi kuin Lähetä korko. Lähettäjien määrä on pieni. Tilauskohtaisten vastaanottajia määrä on pieni.

Voit suurentaa siirtonopeuden, toimi seuraavasti:

-   Käytä osioitua ohjeaiheessa parannettu suorituskyky ja käytettävyyden.

-   Yleistä Lähetä nopeutta aiheeseen parantamiseksi Luo lähettäjien useita viestin tehtaan avulla. Käytä asynkroninen toimintoja tai useita viestiketjuissa siirtyminen kullekin lähettäjälle.

-   Kun haluat lisätä yleistä vastaanota nopeutta, tilaukseen, Luo vastaanottajia useita viestin tehtaan avulla. Käytä kunkin vastaanottajan asynkronisten toimintojen tai useita viestiketjuissa siirtyminen.

-   Asynkronisten toimintojen avulla voit hyödyntää asiakkaan jonottaminen.

-   Vähennä palvelun Bus asiakkaan protokolla lähetysten jonottaminen 20ms aikaväli oletusarvo avulla.

-   Jätä oletusmäärä kaupan access käytössä. Tämä suurentaa yleistä nopeutta, jolla voidaan kirjoittaa viestejä aiheen kyselyjä.

-   Määritä esihaun Laske 20 kertaa suurin käsittely myönnettävän factory kaikki vastaanottajia. Tämä pienentää palvelun Bus asiakkaan protokolla lähetysten määrä.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Aiheen tilaukset suuri määrä

Tavoite: Suurenna siirtonopeuden aiheen tilaukset suuri määrä. Viestin on vastaanottanut monta tilausta, mikä tarkoittaa sitä kautta kaikkien tilausten yhdistetyn vastaanotto-korko on paljon suurempi kuin Lähetä korko. Lähettäjien määrä on pieni. Tilauskohtaisten vastaanottajia määrä on pieni.

Aiheita, joissa on runsaasti tilaukset Näytä pienen yleinen siirtonopeuden yleensä, jos kaikki viestit reititetään kaikki tilaukset. Syynä on se, että kukin viesti vastaanotetaan monta kertaa, ja kaikki viestit, jotka sisältyvät aiheen ja kaikki sen tilaukset tallennetaan samassa säilössä. Oletetaan, että lähettäjien määrän ja vastaanottajia tilauskohtaisten määrä on pieni. Palvelun Bus tukee enintään 2 000 tilauksista aihe kohden.

Voit suurentaa siirtonopeuden, toimi seuraavasti:

-   Käytä osioitua ohjeaiheessa parannettu suorituskyky ja käytettävyyden.

-   Asynkronisten toimintojen avulla voit hyödyntää asiakkaan jonottaminen.

-   Vähennä palvelun Bus asiakkaan protokolla lähetysten jonottaminen 20ms aikaväli oletusarvo avulla.

-   Jätä oletusmäärä kaupan access käytössä. Tämä suurentaa yleistä nopeutta, jolla voidaan kirjoittaa viestejä aiheen kyselyjä.

-   Määrittää esihaun Laske 20 kertaa odotettu vastaanota luokitus sekuntia. Tämä pienentää palvelun Bus asiakkaan protokolla lähetysten määrä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun Bus optimoinnista on artikkelissa [Partitioned messaging kohteita][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Osioitu tekstiviesti yksiköt]: service-bus-partitioning.md
  