<properties
    pageTitle="Aloita tapahtuman keskittimet C# | Microsoft Azure"
    description="Katso tämä opetusohjelma Azure tapahtuman solmukohdat käytön aloittaminen lähetetään tapahtumien C#- ja vastaanottaa Java EventProcessorHost avulla."
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
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on palvelu, joka käsittelee paljon yhdistetyn laitteita ja sovelluksia tapahtuman tiedot (telemetriatietojen). Jälkeen voit koota tietoja tapahtuman keskittimet, voit tallentaa tiedot käyttämällä tallennustila-klusterin tai muuntamiseen reaaliaikainen analytics-palvelun avulla. Tämä suurissa tapahtuman kerääminen ja käsittely-ominaisuus on avaimen osa nykyaikaista sovelluksen arkkitehtuureihin, mukaan lukien asiat Internet (IoT).

Tässä opetusohjelmassa näytetään, miten Azure perinteinen portaalin avulla voit luoda tapahtumaa-toiminnossa. Se myös näyttää kerääminen viestien tapahtumaan-keskittimeen, kirjoitettu C# console-sovelluksesta ja noutamisesta ne rinnakkain käyttämällä Java tapahtuman suoritin isäntäkirjasto.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active tili. <br/>Jos sinulla ei ole, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1.  Suorita **vastaanotin** projekti.

    ![][21]

2.  Suorita **lähettäjän** projekti.

    ![][22]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut toimimasta-sovellus, joka luo tapahtuma-toiminnosta ja lähettää ja vastaanottaa tietoja, voit siirtää seuraavissa tilanteissa:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.
- [Tapahtuman keskittimet yleiskatsaus][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
