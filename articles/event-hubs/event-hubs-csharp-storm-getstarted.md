<properties
    pageTitle="Tapahtuman keskittimet C# kanssa Apache myrsky aloittaminen | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen C#- ja vastaanottaa Apache myrsky-klusterin lähettämisen tapahtumat."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/06/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, voit saanti miljoonia sekunnissa tapahtumista, ottaminen käyttöön ja analysoida tietoja tallennuspaikkana sovelluksen valmistettu yhdistetyn laitteita ja sovelluksia. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin.

Lisätietoja on artikkelissa [tapahtuman keskittimet yleiskatsaus].

Tässä opetusohjelmassa kerrotaan ingest viestien tapahtumaan-keskittimeen, console-sovelluksen avulla C# ja tarkastella niitä käyttämällä Apache myrsky rinnakkain.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraava:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Java-kehitysympäristö määritetty suorittamaan [maven-testi](http://maven.apache.org/). Tässä opetusohjelmassa on olettaa [Pimennys](https://www.eclipse.org/)

+ Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure maksuttoman kokeiluversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]


[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-storm](../../includes/service-bus-event-hubs-get-started-receive-storm.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Pitäminen Pimennys **LogTopology** -luokka ja valitse Käynnistä, kaikki osiot vastaanottajia odottamaan.

2.  Suorita **lähettäjän** projekti konsoli-ikkunan **Enter** -näppäintä ja katso vastaanottaja-ikkunassa näkyvät tapahtumat.

    ![][22]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut toimimasta-sovellus, joka luo tapahtuma-toiminnosta ja lähettää ja vastaanottaa tietoja, voit siirtää seuraavissa tilanteissa:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.

<!-- Images. -->
[22]: ./media/event-hubs-csharp-storm-getstarted/receive-storm1.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 