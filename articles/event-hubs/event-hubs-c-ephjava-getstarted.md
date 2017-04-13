<properties
    pageTitle="Tapahtuman keskittimet c aloittaminen | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen lähetetään tapahtumien C ja vastaanottaa Java käyttämällä tapahtuman suoritin isäntä."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="c"
    ms.devlang="csharp"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, voit saanti miljoonia sekunnissa tapahtumista, ottaminen käyttöön ja analysoida tietoja tallennuspaikkana sovelluksen valmistettu yhdistetyn laitteita ja sovelluksia. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin.

Lisätietoja on artikkelissa [tapahtuman keskittimet yleiskatsaus][].

Tässä opetusohjelmassa kerrotaan ingest viestien console-sovelluksen käyttäminen C tapahtuma-keskittimeen ja tarkastella niitä rinnakkain käyttämällä C# [Tapahtuman suoritin Host (isäntä)][] -kirjasto.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraava:

+ C kehitysympäristö. Tässä opetusohjelmassa oletetaan [Azure Linux AM](../virtual-machines/virtual-machines-linux-quick-create-cli.md) kanssa Ubuntu 14.04 gcc-pino. Muiden ympäristöjen ohjeet toimitetaan ulkoisia linkkejä.

+ Microsoft Visual Studio Express for Windowsissa

+ Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure maksuttoman kokeiluversion</a>.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-c](../../includes/service-bus-event-hubs-get-started-send-c.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Suorita **vastaanotin** projekti.

    ![][21]

2.  **Lähettäjä** -ohjelman, niin huomaat vastaanottaja-ikkunassa näkyvät tapahtumat.

    ![][24]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut toimimasta-sovellus, joka luo tapahtuma-toiminnosta ja lähettää ja vastaanottaa tietoja, voit siirtää seuraavissa tilanteissa:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.
- [Tapahtuman keskittimet yleiskatsaus][]

<!-- Images. -->
[21]: ./media/event-hubs-c-ephjava-getstarted/ephjava.png
[24]: ./media/event-hubs-c-ephjava-getstarted/receive-eph-c.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Tapahtuman suoritin Host (isäntä)]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
