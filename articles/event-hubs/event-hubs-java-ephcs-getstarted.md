<properties
    pageTitle="Tapahtuman keskittimet Java-aloittaminen | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen Java tapahtumat lähettäminen ja vastaanottaminen ne C# EventProcessorHost avulla."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="core"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, voit saanti miljoonia sekunnissa tapahtumista, ottaminen käyttöön ja analysoida tietoja tallennuspaikkana sovelluksen valmistettu yhdistetyn laitteita ja sovelluksia. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin.

Lisätietoja on artikkelissa [tapahtuman keskittimet yleiskatsaus][].

Tässä opetusohjelmassa kerrotaan, miten ingest viestien console-sovelluksesta Java-tapahtuma-keskittimeen ja tarkastella niitä rinnakkain käyttämällä C# [Tapahtuman suoritin Host (isäntä)][] -kirjasto.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ Java kehitysympäristö. Tässä opetusohjelmassa oletetaan [Pimennys](https://www.eclipse.org/).

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure maksuttoman kokeiluversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-java](../../includes/service-bus-event-hubs-get-started-send-java.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Suorita **vastaanotin** projekti Visual Studio ja valitse Käynnistä, kaikki osiot vastaanottajia odottamaan.

    ![][21]

2.  Suorita **lähettäjän** projekti.

    ![][22]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut toimimasta-sovellus, joka luo tapahtuma-toiminnosta ja lähettää ja vastaanottaa tietoja, voit siirtää seuraavissa tilanteissa:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.

Lisätietoja on artikkelissa [Java Developer Center](/develop/java/).

<!-- Images. -->
[21]: ./media/event-hubs-java-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-java-ephcs-getstarted/java-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Tapahtuman suoritin Host (isäntä)]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
 