<properties
    pageTitle="Tapahtuman lihavoituna C ja Apache myrsky aloittaminen | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen tapahtumien lähettäminen c ja vastaanottaa Apache myrsky-klusterin."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="java"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, voit saanti miljoonia sekunnissa tapahtumista, ottaminen käyttöön ja analysoida tietoja tallennuspaikkana sovelluksen valmistettu yhdistetyn laitteita ja sovelluksia. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin.

Lisätietoja on artikkelissa [tapahtuman keskittimet yleiskatsaus].

Tässä opetusohjelmassa kerrotaan ingest viestien console-sovelluksen käyttäminen C tapahtuma-keskittimeen ja tarkastella niitä käyttämällä Apache myrsky rinnakkain.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ C kehitysympäristö. Tässä opetusohjelmassa oletetaan [Azure Linux AM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) kanssa Ubuntu 14.04 gcc-pino. Muiden ympäristöjen ohjeet toimitetaan ulkoisia linkkejä.

+ Java-kehitysympäristö määritetty suorittamaan [maven-testi](http://maven.apache.org/). Tässä opetusohjelmassa oletetaan [Pimennys](https://www.eclipse.org/).

+ Azure active tili. Jos sinulla ei ole tiliä, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Pitäminen Pimennys **LogTopology** -luokka ja valitse Käynnistä, kaikki osiot vastaanottajia odottamaan.

2.  **Lähettäjä** -ohjelman ja tuo näkyviin vastaanottaja-ikkunassa näkyvät tapahtumat.

    ![][23]

> [AZURE.NOTE] Vain tässä opetusohjelmassa käyttäville myrsky paikallisen-tilassa. Lisätietoja [HDInsight myrsky yleiskatsaus] ja virallinen [Apache myrsky] -ohjeista lisätietoja myrsky ominaisuuksissa ja kuviot.

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavat resurssit ovat käytettävissä kehityssovellusten integrointi tapahtuman keskittimet ja myrsky.

- [Myrsky ja HDInsight tunnistimen tietojen analysoiminen][] on koko skenaarion opetusohjelman ingest tunnistimen tietojen Hadoop-klusterin tapahtuman keskittimet myrsky ja HBase avulla.
- Opetusohjelma siitä, miten voit kirjoittaa myrsky putkistot käyttämällä C# [kehittäminen streaming tietojen käsittely sovellusten SCP.NET ja C# myrsky ja Hdinsightista][] on.

<!-- Images. -->
[23]: ./media/event-hubs-c-storm-getstarted/receive-storm3.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md

[Apache myrsky]: https://storm.incubator.apache.org
[HDInsight myrsky yleiskatsaus]: ../hdinsight/hdinsight-storm-overview.md/
[Myrsky ja HDInsight tunnistimen tietojen analysoiminen]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md
[Kehittää streaming tietojen käsittely sovellusten SCP.NET ja C# myrsky ja Hdinsightiin]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
