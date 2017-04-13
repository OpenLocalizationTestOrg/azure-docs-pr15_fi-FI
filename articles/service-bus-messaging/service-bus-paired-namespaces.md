<properties 
    pageTitle="Palvelun Bus Parittainen nimitilan | Microsoft Azure"
    description="Pisteparin nimitilan säännöt ja kustannukset"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Parittainen nimitilan käyttöönottotiedot ja niiden vaikutukset

[PairNamespaceAsync][] -menetelmä käyttämällä [SendAvailabilityPairedNamespaceOptions][] -esiintymä suorittaa näkyvissä tehtävät puolestasi. Käytettävissä ovat kustannus huomioon otettavia seikkoja käytettäessä ominaisuus, se on hyötyä ymmärtää sellaiset tehtävät, niin, että odotat toiminnan, kun se tapahtuu. Ohjelmointirajapinnan salattua puolestasi automaattisen seuraavat seikat:

-   Keskeneräisen olevien luominen.
-   [MessageSender][] objektia, jonka puheen olevien tai aiheet luominen.
-   Kun tekstiviesti kohde ei ole käytettävissä, lähettää ping viestit kohteeseen ja yrittää tunnistaa, kun kyseisen yksikön käytettävyys uudelleen.
-   Voit myös Luo joukko "viestin pumput", joka siirtää viestit keskeneräisen olevien ensisijainen olevien.
-   Koordinoi sulkeminen/Virhesovellus ensisijainen ja toissijainen [MessagingFactory][] esiintymät.

Korkean tason ominaisuus toimii seuraavasti: kun ensisijainen kohde on kunnossa, ei ole ongelma muutosten syistä. Kun [FailoverInterval][] keston kuluu ja ensisijaisen kohteen näkee ei onnistunut lähettää jälkeen lyhytaikainen [MessagingException][] tai [TimeoutException][], tapahtuu seuraavaa:

1.  Lähetä toimintojen ensisijainen kohde on poistettu käytöstä, ja järjestelmä Testaa yhteyden ensisijaisen kohteen, kunnes Testaa voit valita toimitettu.

2.  Satunnainen keskeneräisen jonossa on valittuna.

3.  Valittu keskeneräisen jonossa reititetään [BrokeredMessage][] objekteja.

1.  Jos valitsemasi keskeneräisen jonossa, lähetystoiminto ei onnistu, kyseisen jonon on vedetään kiertoa ja uuden jonon on valittuna. Kaikki lähettäjät [MessagingFactory][] -esiintymässä tietoja virheen.

Seuraavat kuvat kuvaavat järjestys. Ensin lähettäjän lähettää viestejä.

![Pisteparin nimitilan][0]

Virheen ensisijainen jonossa lähettäminen yhteydessä lähettäjän aloittaa viestien lähettäminen satunnaisesti valittua keskeneräisen jonossa. Samanaikaisesti se käynnistyy ping-tehtävän.

![Pisteparin nimitilan][1]

Tässä vaiheessa viestit ovat edelleen toissijaisen jonossa ja ei ole toimitettu ensisijainen jonossa. Ensisijainen jonossa on kunnossa uudelleen, kun vähintään yksi prosessi on käynnissä syphon. Syphon toimittaa viestit eri keskeneräisen olevien ERISNIMI kohde kohteisiin (olevien ja aiheet).

![Pisteparin nimitilan][2]

Loput tässä ohjeaiheessa kerrotaan tarkkoja nämä kappaletta toiminnasta.

## <a name="creation-of-backlog-queues"></a>Keskeneräisen olevien luominen

Välitetty [PairNamespaceAsync][] -menetelmän [SendAvailabilityPairedNamespaceOptions][] objekti ilmaisee keskeneräisen olevien, jota haluat käyttää määrän. Keskeneräisen kunkin jonon sitten luodaan sisältää seuraavat ominaisuudet erikseen (kaikki muut arvot määritetään [QueueDescription][] oletusarvot) määrittäminen:

| Polku                                   | [ensisijainen nimitila] / x-servicebus-siirto / [Indeksi] [Indeksi] missä arvon [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | arvo MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| Saannista                           | 1 minuutti                                                                                             |
| EnableDeadLetteringOnMessageExpiration | TOSI                                                                                                 |
| EnableBatchedOperations                | TOSI                                                                                                 |

Esimerkiksi ensimmäisen keskeneräisen jonon luodaan nimitilan **contoso** -niminen `contoso/x-servicebus-transfer/0`.

Luotaessa olevien koodi tarkistaa ensin toiminnan olemassa. Jos jonossa ei ole, jonossa luodaan. Koodin ei Puhdista "ylimääräisen" keskeneräisen olevien. Tarkista etenkin jos ensisijainen nimitilan **contoso** sovellukseen pyytää viisi käsittelemättömät mutta polulla keskeneräisen jonossa olevien `contoso/x-servicebus-transfer/7` olemassa, kyseisen ylimääräisiä keskeneräisen jonossa on edelleen olemassa, mutta ei käytetä. Järjestelmä sallii eksplisiittisesti ylimääräisiä keskeneräisen olemassa olevien, joka ei. Nimitilan omistajana olet vastuussa kaikki käyttämättömät/ei-toivottuja keskeneräisen olevien Siivotaan. Tämä päätös syy on, että palvelun Bus et voi tietää, mitä tarkoitetaan ovat kaikki oman nimitilan olevien. Lisäksi jos jono on olemassa määritettyä nimeä, mutta ei täytä oletettu [QueueDescription][], sitten oman syitä Omat muuttamiseen oletustoiminnan. Ei takuita tehdään muutoksia keskeneräisen olevien koodisi. Varmista, että voit esikatsella muutoksiasi huolellisesti.

## <a name="custom-messagesender"></a>Mukautettu MessageSender

Kun lähetät, kaikki viestit käydä läpi sisäinen [MessageSender][] -objekti, joka toimii yleensä kaikki toimii, jolloin ohjaa keskeneräisen olevien, kun asioita "katkaista." Yhteydessä vastaanottaminen lyhytaikainen virhe, ajastin käynnistyy. [FailoverInterval][] -ominaisuuden arvon ajalta ei onnistunut viestit lähetetään koostuva [aikajakson][] ajan kuluttua vikasietotilaa on kytketty. Tässä vaiheessa kunkin kohteen tapahtuu seuraavat kohdat:

- Ping-tehtävän suorittaa jokaisen [PingPrimaryInterval][] tarkistamaan, jos kohde on käytettävissä. Kun tehtävä on valmis, kohteen heti käyttävä asiakas-koodi käynnistää uusien viestien lähettäminen ensisijainen nimitilan.

- Voit lähettää saman kohteen muiden lähettäjien johtaa [BrokeredMessage][] on lähetetty muokattava istut keskeneräisen jonossa tulevien pyynnöt. Muutoksen poistaa joitakin ominaisuuksia [BrokeredMessage][] -objektista, ja tallentaa ne muualla. Seuraavat ominaisuudet poistetaan ja lisätään uusi alias-palvelun Bus ja SDK käsittelemään sanomia yhdenmukaisesti:

| Vanha ominaisuuden nimi       | Uusi ominaisuuden nimi |
|-------------------------|-------------------|
| Istunto               | x-ms-istunto    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-polku         |

Alkuperäinen kohdepolku tallennetaan myös viestistä x-ms-polku-niminen ominaisuus. Tämä rakenne mahdollistaa viestien päällekkäisen yksittäisen keskeneräisen jonossa useissa kohteissa. Ominaisuuksien muunnetaan takaisin syphon mukaan.

Mukautetun [MessageSender][] objektin ongelmia voi ilmetä, kun viestejä esitellä 256 kt rajan ja automaattisesti on kytketty. Mukautettu [MessageSender][] objekti tallentaa viestit olevien ja aiheet yhdessä keskeneräisen olevien. Tätä objektia seokset monta primaareista yhdessä keskeneräisen olevien sisällä viestejä. Käsitellä välillä monet asiakkaat, jotka eivät tiedä toisiinsa kuormituksen SDK valitsee satunnaisesti yksi keskeneräisen jono kullekin [QueueClient][] tai [TopicClient][] koodin luominen.

## <a name="pings"></a>Testaa

Ping-viesti on sen [ContentType][] -ominaisuuden arvoksi sovelluksen/vnd.ms-servicebus-ping- ja sekunnin [TimeToLive][] arvo tyhjä [BrokeredMessage][] . Tämä ping on yksi määräten ominaisuus palvelun Bus: palvelimen toimittaa ping-pyyntö aina, kun kaikki soittajan pyytää [BrokeredMessage][]. Näin ollen koskaan on lisätietoja vastaanottaminen ja Ohita nämä viestit. Kunkin kohteen (yksilöllinen jonossa tai aihe) kohti [MessagingFactory][] esiintymän asiakasta kohden pinged, kun niitä pidetään ei ole käytettävissä. Oletusarvoisesti tämä tapahtuu kerran minuutissa. Ping-viestien pidetään Normaali palvelun Bus viestit ja voi johtaa ovat kaistanleveys ja viestejä. Kun asiakkaat havaita, että järjestelmä on käytettävissä, viestit lopettaa.

## <a name="the-syphon"></a>Syphon

Vähintään yksi suoritettava ohjelma sovelluksen aktiivisesti suoritetaan syphon. Syphon suorittaa pitkän kyselyn vastaanottaa, joka kestää 15 minuuttia. Kun kaikki kohteet ovat käytettävissä ja sinulla on 10 keskeneräisen olevien, sovellus, joka isännöi syphon kutsuu 40 kertaa tunnissa, 960 kertoja päivässä ja 28800 ajat-30 päivää vastaanota-toimintoa. Kun syphon on aktiivisesti siirtyvät viestejä keskeneräisen ensisijainen jonossa, jokaisen viestin ilmenee seuraavia maksuja (viestin koko ja kaistanleveys vakio ovat voimassa kaikissa vaiheissa):

1.  Lähetä keskeneräisen.

2.  Saat keskeneräisen.

3.  Lähetä ensisijainen.

4.  Saat ensisijainen.

## <a name="closefault-behavior"></a>Sulje/vika toiminta

Sovelluksesta, joka isännöi syphon kerran ensisijaisen tai toissijaisen- [MessagingFactory][] virheitä tai suljetaan ilman sen kumppaneiden käytössä myös virhe/suljetaan ja syphon havaitsee tässä tilassa syphon säädösten. Jos muut [MessagingFactory][] ole sulkenut 5 muutamassa, syphon vika edelleen avoinna [MessagingFactory][].

## <a name="next-steps"></a>Seuraavat vaiheet

Saat yksityiskohtaiset keskustelun palvelun Bus asynkroninen viestien [asynkroninen tekstiviesti toistuvien tulosten ja suuren käytettävyyden][] . 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Aikajakson]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynkroninen tekstiviesti toistuvien tulosten ja suuri käytettävyys]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
