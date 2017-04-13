<properties 
    pageTitle="Osioitu olevien ja aiheet | Microsoft Azure"
    description="Tässä artikkelissa käsitellään osio käyttämällä useita viestin vakuutuksenvälittäjän palvelun Bus olevien ja aiheet."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Osioitu olevien ja aiheet

Azure palvelun Bus käytetään useita viestin vakuutuksenvälittäjän käsittelemään sanomia ja useita tekstiviesti stores viestien tallentamiseen. Tavanomaisen jonossa tai aiheen on yhden viestin broker käsittelemien ja yksi tekstiviesti säilöön tallennettuja. Palvelun Bus mahdollistaa myös olevien tai niin osioitu eri viestin vakuutuksenvälittäjän ja tekstiviesti stores välillä. Tämä tarkoittaa yleistä siirtonopeuden osioitua jonossa tai aiheen on rajoitettu, sekä yhden viestin broker tai tekstiviesti-kauppaan. Lisäksi väliaikaisen käyttökatkosta tekstiviesti-säilön hahmontaminen ei onnistu osioitua jonossa tai aiheen ei ole käytettävissä. Osioitu olevien ja aiheet voivat sisältää kaikki kehittyneet Service Bus-ominaisuudet, kuten tapahtumia ja istuntojen tuki.

Lisätietoja palvelun Bus sisäiset [palvelun Bus arkkitehtuuri][] on aiheessa.

## <a name="how-it-works"></a>Toiminta

Kunkin osioitua jonossa tai aiheen koostuu useita osia. Jokaisen osan eri tekstiviesti säilöön tallennettuja ja toista viestiä broker käsittelemien. Kun viesti on lähetetty osioitua jonossa tai aihe-palvelun Bus määrittää viestin johonkin osat. Valinnan tehdään satunnaisesti palvelun Bus tai käyttämällä osio-näppäintä, joka määrittää lähettäjä.

Jos asiakas haluaa vastaanottaa viestin osioitua jonossa tai tilaukseen osioitua aiheen palvelun Bus kyselyjen viestien kaikki osat, palauttaa ensimmäisen viestin, joka saadaan mistä tahansa tekstiviesti stores vastaanottaja. Palvelun Bus tallentaa toisen viestit ja palauttaa ne kun se saa lisää saa pyynnöt. Vastaanottava asiakas ei ole tietoinen jakaminen; asiakkaan osoittava toimintaa osioitua jonossa tai aiheen (esimerkiksi luku, suorittaminen, myöhemmäksi perille toimittamattomien sanomien, prefetching) on sama kuin tavallinen kohteen toimintaa.

Ei ilman lisäkustannuksia, kun viestin lähettäminen tai vastaanottaminen viestin, osioitu jonossa tai aiheen.

## <a name="enable-partitioning"></a>Ota jakaminen käyttöön

Voit käyttää osioitua olevien ja aiheet Azure palvelun Bus, Azure SDK 2.2 tai uudempi versio tai määritä `api-version=2013-10` että HTTP-pyynnöt.

Voit luoda palvelun Bus olevien ja kohdissa 1, 2, 3, 4 ja 5 gt koot (oletusarvo on 1 gt). Palvelun Bus Luo 16 osiot ja jakaminen käytössä, kunkin gt, voit määrittää. Sellaisenaan, jos olet luonut jono, joka on 5 gt kokoisia, 16 osiot ja jonon enimmäiskoko ei ole (5 \* 16) = 80 Gigatavua. Voit tarkastella osioitua jonossa tai aiheen enimmäiskokoa katsoo sen [Azure portal][].

Osioitu jonossa tai aiheen luominen useilla tavoilla. Kun luot jonossa tai aiheen sovelluksen, voit ottaa jakaminen jonossa tai aiheen määrittämällä vastaavasti [QueueDescription.EnablePartitioning][] tai [TopicDescription.EnablePartitioning][] -ominaisuuden arvoksi **true**. Nämä ominaisuudet on määritettävä milloin jonossa tai aiheen luodaan. Ei voida muuttaa olemassa olevan jonossa tai aiheen näitä ominaisuuksia. Esimerkki:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Vaihtoehtoisesti voit luoda osioitua jonossa tai aiheen Visual Studiossa tai [Azure portal][]. Kun luot uuden jonon tai aiheen portaalissa, Määritä **Käyttöön jakaminen** -kohdan jonossa tai aiheen **Tosi** **asetukset** -ikkunassa **Yleiset asetukset** -sivu. Valitse Visual Studion **Uuden jonon** tai **Uusi aihe** -valintaikkunassa **Jakaminen käyttöön** -valintaruutu.

## <a name="use-of-partition-keys"></a>Osion avaimen

Kun viesti on enqueued osioitua jonossa tai aiheen, palvelun Bus tarkistaa osion avaimen. Jos jokin löytyy, valitsee fragmentti avaimen perusteella. Jos se ei löydä osio-näppäintä, valitsee fragmentti sisäisen algoritmin perusteella.

### <a name="using-a-partition-key"></a>Osion avaimen avulla

Joissakin tilanteissa istuntojen tai tapahtumia, kuten edellyttävät viestit tallennetaan tietyn osan. Kaikki näissä tilanteissa on käytettävä osio-näppäintä. Kaikki viestit, jotka käyttävät samaa osion avainta määritetään samaan osa. Jos osa on tilapäisesti poissa käytöstä, palvelun Bus palauttaa virheen.

Toimintamallin-osion avaimeksi voi käyttää eri viestin ominaisuudet:

**Istunto**: Jos viestissä on [BrokeredMessage.SessionId][] -ominaisuuden arvoksi ja sitten palvelun Bus käyttää tätä ominaisuutta osio-näppäintä. Tällä tavalla kaikki viestit, jotka kuuluvat saman istunnon käsitellään saman viestin broker mukaan. Näin palvelun Bus taata viestin sekä istunnon hyötyä yhtenäisyyden järjestys.

**PartitionKey**: Jos viestissä on [BrokeredMessage.PartitionKey][] -ominaisuuden, mutta ei [BrokeredMessage.SessionId][] -ominaisuuden arvoksi ja sitten palvelun Bus käyttää [PartitionKey][] -ominaisuuden osio-näppäintä. Jos viesti on [istunto][] ja [PartitionKey][] ominaisuuksien määritykset, molempia ominaisuuksia on oltava sama. Jos [PartitionKey][] -ominaisuudeksi on määritetty arvon kuin [istunto][] -ominaisuus-palvelun Bus palauttaa **InvalidOperationException** poikkeuksen. [PartitionKey][] -ominaisuus on käytettävä, jos lähettäjän lähettää istunnon aware viestit. Osio-näppäintä varmistaa, että kaikki viestit, jotka lähetetään tapahtumasta käsittelemien saman tekstiviesti broker.

**MessageId**: Jos jonossa tai aiheen [QueueDescription.RequiresDuplicateDetection][] -ominaisuuden arvo on **Tosi** ja [BrokeredMessage.SessionId][] tai [BrokeredMessage.PartitionKey][] ominaisuuksia ei ole määritetty ja valitse [BrokeredMessage.MessageId][] -ominaisuus toimii osio-näppäintä. (Huomaa, että Microsoft .NET ja AMQP kirjastoissa määritellä viestin tunnuksen automaattisesti Jos lähettävä sovellus ei ole). Tässä tapauksessa kaikki kopiot saman viestin käsittelee samaa viestin broker. Näin palvelun Bus havaitsemaan ja poistamaan viesteistä. Jos [QueueDescription.RequiresDuplicateDetection][] -ominaisuutta ei ole määritetty **Tosi**, palvelun Bus kannattaa [MessageId][] -ominaisuutta ei osion avaimeksi.

### <a name="not-using-a-partition-key"></a>Ei osion avaimen avulla

Osion näppäintä puuttuessa palvelun Bus jakaa viestit pyöreän pöydän ajoin, kaikki osioitua jonossa tai aiheen osat. Jos valitsemasi osa ei ole käytettävissä, palvelun Bus määrittää viestin eri osa. Tällä tavalla lähetystoiminto onnistuu tekstiviesti kaupan tilapäinen siitä huolimatta.

Anna palvelun Bus riittävästi aikaa niiden enqueue viestin siirtäminen eri osa-asiakas, joka lähettää viestin määritetyn [MessagingFactorySettings.OperationTimeout][] arvon on oltava suurempi kuin 15 sekunnin kuluttua. On suositeltavaa [OperationTimeout][] -ominaisuuden arvoksi 60 sekuntia oletusarvon.

Huomaa, että osion näppäintä "kiinnittää" tietyn osan viestin. Tekstiviesti kauppaan, jossa on tämän osan ei ole käytettävissä, jos palvelun Bus palauttaa virheen. Osion näppäintä puuttuessa palvelun Bus voit valita eri osa ja toiminto onnistuu. Tämän vuoksi on suositeltavaa, että et anna osio-näppäintä, ellei se ole pakollinen.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Laajennettu aiheita: tapahtumat käyttäminen osioitua yksiköt

Tapahtuman osana lähetetyt viestit on määritettävä osio-näppäintä. Tämä voi olla jokin seuraavista ominaisuuksista: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]tai [BrokeredMessage.MessageId][]. Kaikki viestit, jotka lähetetään osana samaa tapahtumaa on määritettävä saman osio-näppäintä. Jos yrität lähettää viestin ilman osion näppäintä tapahtumasta-palvelun Bus palauttaa poikkeuksen **InvalidOperationException** . Jos yrität lähettää sama tapahtumasta useita viestejä, joissa on eri osioon näppäimet-palvelun Bus palauttaa poikkeuksen **InvalidOperationException** . Esimerkki:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Jos ominaisuuksia, jotka vastaavat avaimeksi osio on määritetty, palvelun Bus kiinnittää tietyn osan viestin. Tämä ongelma ilmenee, onko tapahtuman käytetään. On suositeltavaa, että et määritä osio-näppäintä, jos sitä ei tarvita.

## <a name="using-sessions-with-partitioned-entities"></a>Istunnot käyttäminen osioitua yksiköt

Tapahtumien viestin lähettäminen istunnon tukeva aiheessa tai jonossa, viesti on oltava [BrokeredMessage.SessionId][] -ominaisuuden arvoksi. Jos [BrokeredMessage.PartitionKey][] -ominaisuus on määritetty myös, sen on oltava sama kuin [tunnus][] -ominaisuutta. Jos ne eroavat palvelun Bus palauttaa poikkeuksen **InvalidOperationException** .

Toisin kuin tavallinen (osittamattoman) olevien tai aiheet ei ole mahdollista useiden viestien lähettäminen eri istunnoissa yksittäisen tapahtuman avulla. Jos yritit-palvelun Bus palauttaa poikkeuksen **InvalidOperationException **. Esimerkki:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Viestien automaattinen edelleenlähetys osioitua kohteisiin

Azure palvelun Bus tukee viestien automaattinen edelleenlähetys-, tai osioitua kohteiden välillä. Viestien automaattinen välittäminen käyttöön ominaisuuden [QueueDescription.ForwardTo][] jonossa lähde-tai tilaus. Jos viestin määrittää osion näppäintä ([istunto][], [PartitionKey][]tai [MessageId][])-osion avaimen käytetään kohde.

## <a name="considerations-and-guidelines"></a>Ohjeita ja huomioon otettavia seikkoja

- **Suuri yhdenmukaisuuden ominaisuudet**: Jos kohteen käyttää ominaisuuksia, kuten istunnot, kaksoiskappaleiden tai eksplisiittinen hallinnan jakaminen avain ja valitse viestinnän toimintoja reititetään aina tietyn osat. Jos jokin osat kohdata suuri tietoliikenne tai pohjana säilö on perusasemassa, toimintoihin epäonnistua ja käytettävyys. Yleinen-yhdenmukaisuuden on edelleen paljon suurempi kuin osittamattoman yksiköt liikenne alijoukko on ongelmia, kaikki liikenne sijaan.
- **Hallinta**: toimintoja, kuten luonti-, päivitys- ja poista on suoritettava kaikki kohteen osat. Jos mikä tahansa osa on perusasemassa se voi aiheuttaa virheitä seuraavia toimintoja varten. Get-toiminnon tietoja, kuten viestin laskee on koottava-kaikki osat. Jos jokin osa on perusasemassa, rajoitettu ilmoitetaan yksikön käytettävyys-tila.
- **Vähäinen viestin skenaariot**: näiden tilanteessa, erityisesti silloin, kun käytät HTTP-protokolla, saatat joutua suorittaa useita vastaanottaa toimintojen saadakseen kaikki viestit. Vastaanota pyynnöt edustatietokanta suorittaa vastaanota kaikki osat ja välimuistiin kaikki vastaukset, jotka on vastaanotettu. Seuraavien vastaanota pyyntö samassa yhteydessä hyötyä tämän välimuistin ja vastaanottaa viiveitä heikompi. Jos käytössäsi on useita yhteyksiä tai käyttää HTTP-, joka vahvistetaan kuitenkin sivupyynnön uusi yhteys. Näin ollen ei ole takaa, että se Power View saman solmun. Jos kaikki olemassa olevat viestit estetty eikä välimuistiin toiseen edusta-vastaanota-toimintoa palauttaa **null**. Viestien tekstin voimassa ja näyttöön tulee. HTTP-yhteyden säilyttämisen suositellaan.
- **Selaa/Pikanäkymä viestien**: PeekBatch ei aina Palauta [MessageCount ominaisuuden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)määritetyn viestien määrä. On kaksi yleistä syytä. Yksi syy on viestijoukkoa koostetun koko ylittää 256 kt enimmäiskoon. Voivat johtua siitä, että jos jonossa tai aiheen on [EnablePartitioning-ominaisuuden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) arvoksi **true**-osion ei välttämättä ole tarpeeksi viestien suorittamiseen tarvittavat viestien määrä. Yleensä sovelluksen haluaa viestien tietystä numerosta, se pitäisi soittaa [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) toistuvasti, kunnes se saa viestien määrä tai on voit vilkaista enempää viestejä. Katso lisätietoja, kuten MALLIKOODEJA, [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) tai [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Uusimmat ominaisuuksista

- Lisää tai poista sääntö tuetaan osioitua kohteisiin. Eroaa osittamattoman kohteiden näistä toiminnoista ei tue tapahtumat-kohdassa. 
- AMQP tukee nyt lähettää ja vastaanottaa viestejä ja osioitua kohteesta.
- AMQP tukee nyt seuraavat toiminnot: [Erä Lähetä](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Erä vastaanottaa](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [Vastaanota järjestysnumero mukaan](https://msdn.microsoft.com/library/azure/hh330765.aspx), [vilkaista](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Uusia Lukitse](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Aikataulun viestin](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Peruuta viesti ajoitettu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Lisää sääntö](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Poista sääntö](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Istunnon uusia Lukitse](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Määrittäminen istunnon tila](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get istunnon tila](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Vilkaista istunnon viestit](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)ja [Luetteloi istunnot](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Osioitu kohteiden rajoitukset

Tällä hetkellä palvelun Bus joutuu osioitua olevien ja aiheet seuraavat rajoitukset:

-   Osioitu olevien ja aiheet eivät tue lähettämisen viestit, jotka kuuluvat eri istunnoissa yhdessä tapahtumassa.
-   Palvelun Bus avulla tällä hetkellä enintään 100 osioitua olevien tai aiheet nimitilan kohden. Kunkin osioitua jonossa tai aiheen laskee kohti kiintiön 10 000 kohteiden kohti nimitilan (ei koske Premium taso).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [palvelun Bus AMQP 1.0 tuki osioitu olevien ja ohjeaiheissa][] lisätietoja tekstiviesti kohteiden jakaminen. 

  [Palvelun Bus-arkkitehtuuri]: service-bus-architecture.md
  [Azure portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Istunto]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [Palvelun Bus osioitu olevien ja aiheet AMQP 1.0 tuki]: service-bus-partitioned-queues-and-topics-amqp-overview.md
