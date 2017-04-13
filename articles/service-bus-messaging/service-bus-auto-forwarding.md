<properties 
    pageTitle="Automaattinen edelleenlähetystä palvelun Bus messaging kohteiden | Microsoft Azure"
    description="Vaiheittaiset ohjeet ketju jonossa tai tilauksen toiseen jonossa tai aihe."
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

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Ketjutus palvelun Bus kohteiden automaattinen välittäminen kanssa

*Automaattinen edelleenlähetys* -ominaisuuden avulla voit ketjut jonossa ja tilauksen toiseen jonossa tai artikkelista, johon kuuluu samaa nimitilaa. Kun automaattinen välittäminen on käytössä, palvelun Bus poistaa viestit, jotka ovat käytettävissä ensimmäisen jonon tai tilaus (lähde) automaattisesti ja siirtää ne toisen jonossa tai aiheen (kohde). Huomaa, että se on vielä mahdollista, voit lähettää viestin kohde suoraan. Lisäksi ei ole mahdollista ketjut alijonoon, kuten perille toimittamattomien sanomien jono, toiseen jonossa tai aihe.

## <a name="using-auto-forwarding"></a>Käyttämällä automaattinen välittäminen

Voit ottaa automaattisen edelleenlähetyksen määrittämällä [QueueDescription][] tai [SubscriptionDescription][] objektien lähteen, kuten seuraavassa esimerkissä [QueueDescription.ForwardTo][] tai [SubscriptionDescription.ForwardTo][] ominaisuudet.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Kohde on oltava samalla kun lähteen kohde luodaan. Jos kohde ei ole-palvelun Bus palauttaa poikkeuksen, kun ohjelma pyytää luomaan lähdekohteen.

Voit käyttää automaattinen välittäminen skaalata ulos yksittäinen aihe. Palvelun Bus rajoittaa 2 000 [tiettyyn aiheeseen tilausten määrä](service-bus-quotas.md) . Mahtuu lisäpalveluita luomalla toisen tason aiheet. Huomaa, että vaikka ovat ei sido palvelun Bus rajoitus tilaukset määrän, lisäämällä toisen tason aiheista parantaa yleistä siirtonopeuden aihe.

![Automaattinen edelleenlähetys-skenaario][0]

Voit käyttää myös automaattinen välittäminen irrottamisen viestien lähettäjät-vastaanottajia. Harkitse esimerkiksi ERP-järjestelmän, joka koostuu kolmesta moduulit: Tilauksen käsitteleminen, Varastonhallinta ja asiakkaan suhteiden hallinta. Kaikki nämä moduulit luo viestejä, jotka ovat enqueued kyselyjä vastaavan ohjeaiheen. Anneli ja Teemu ovat myyntiedustajia, jotka ovat kiinnostuneita kaikki viestit, jotka liittyvät asiakkaidensa. Saat viestit, Anneli ja Teemu kunkin luoda Omat jonossa ja tilauksen kunkin ERP-aiheisiin, joissa jonon kaikkien viestien välittäminen automaattisesti.

![Automaattinen edelleenlähetys-skenaario][1]

Jos Anneli, loma, hänen Omat jonossa sijaan ERP-ohjeaiheen, täyttää. Tässä skenaariossa Myyntiedustaja ei ole saanut viestejä, koska ei mitään ERP-aiheista koskaan saavuttaa kiintiön.

## <a name="auto-forwarding-considerations"></a>Automaattinen välittäminen huomioon otettavia seikkoja

Jos kohde on kertynyt viestien määrän ja ylittää kiintiön tai kohde on poistettu käytöstä, lähdekohteen Lisää viestit sen [Perille toimittamattomat jonossa](service-bus-dead-letter-queues.md) , kunnes kohde on tilaa (tai kohde on uudelleen käyttöön). Viestit edelleen live Perille toimittamattomat jonossa, joten on erikseen vastaanottaa ja käsitellä niitä Perille toimittamattomat jonossa.

Kun ketjutus yhdessä yksittäisiin aiheisiin hankkiminen koosteen aihe ja monta tilausta, on suositeltavaa, että sinulla on Normaali määrä ensimmäisen tason aiheeseen tilaukset ja monta tilausta toisen tason aiheista. Esimerkiksi ensimmäisen tason aiheen on 20 tilausta, jokainen niistä ketjutettu toisen tason aiheen on 200 tilausta sallii suurempi kuin ensimmäisen tason aiheen on 200 tilausta siirtonopeuden kunkin ketjutettu toisen tason aiheen on 20 tilausta.

Palvelun Bus laskuttaa asiakasta ensimmäisen kerran kullekin välitetyn viestin kerralla. Esimerkiksi viestin lähettäminen on 20 tilausta, jokainen niistä määritetty automaattinen eteenpäin viestit toiseen jonossa tai aiheen Aihe laskutetaan 21 toimintoina Jos kaikki ensimmäisen tason tilaukset viestin kopio.

Tilauksessa, joka on ketjutettu toiseen jonossa tai aiheen luomiseen tilauksen luontisovelluksen on oltava käyttöoikeuksien **hallinta** lähde-ja kohde. Vain viestien lähettäminen lähde-aiheen edellyttää **Lähetä** oikeuksia lähde-aiheesta.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja automaattinen välittäminen on seuraavissa artikkeleissa viittaus:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Lisätietoja palvelun Bus suorituskykyparannukset on artikkelissa [Partitioned messaging kohteiden][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Osioitu tekstiviesti yksiköt]: service-bus-partitioning.md