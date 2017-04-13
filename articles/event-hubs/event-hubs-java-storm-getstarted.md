<properties
    pageTitle="Tapahtuman keskittimet kanssa Apache myrsky Java-aloittaminen | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen Java tapahtumat lähettämiseen ja vastaanottamiseen ne Apache myrsky-klusterin."
    services="event-hubs"
    documentationCenter=""
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, voit saanti miljoonia sekunnissa tapahtumista, ottaminen käyttöön ja analysoida tietoja tallennuspaikkana sovelluksen valmistettu yhdistetyn laitteita ja sovelluksia. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin.

Lisätietoja on artikkelissa [Tapahtuman keskittimet yleiskatsaus][].

Tässä opetusohjelmassa kerrotaan, miten kerätä viestejä console-sovelluksesta Java-tapahtuma-keskittimeen ja tarkastella niitä käyttämällä Apache myrsky rinnakkain.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ Java-kehitysympäristö määritetty suorittamaan [maven-testi](http://maven.apache.org/). Tässä opetusohjelmassa oletetaan [Pimennys](https://www.eclipse.org/).

+ Azure active tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Pitäminen Pimennys **LogTopology** -luokka ja valitse Käynnistä, kaikki osiot vastaanottajia odottamaan.

2.  Suorita **lähettäjän** projekti konsoli-ikkunan **Enter** -näppäintä ja katso vastaanottaja-ikkunassa näkyvät tapahtumat.

    ![][22]

> [AZURE.NOTE] Vain tässä opetusohjelmassa käyttäville myrsky paikallisen-tilassa. On [HDInsight myrsky yleiskatsaus][] ja virallinen [Apache myrsky][] -ohjeissa lisätietoja myrsky ominaisuuksissa ja kuviot.

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavat resurssit ovat käytettävissä kehityssovellusten integrointi tapahtuman keskittimet ja myrsky.

- [Myrsky ja HDInsight tunnistimen tietojen analysoiminen] on koko skenaarion opetusohjelman ingest tunnistimen tietojen Hadoop-klusterin tapahtuman keskittimet myrsky ja HBase avulla.
- Opetusohjelma siitä, miten voit kirjoittaa myrsky putkistot käyttämällä C# [kehittäminen streaming tietojen käsittely sovellusten SCP.NET ja C# myrsky ja Hdinsightista][] on.

<!-- Images. -->
[22]: ./media/event-hubs-java-storm-getstarted/receive-storm2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md

[Apache myrsky]: https://storm.incubator.apache.org
[HDInsight myrsky yleiskatsaus]: ../hdinsight/hdinsight-storm-overview.md
[Myrsky ja HDInsight tunnistimen tietojen analysoiminen]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Kehittää streaming tietojen käsittely sovellusten SCP.NET ja C# myrsky ja Hdinsightiin]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
 