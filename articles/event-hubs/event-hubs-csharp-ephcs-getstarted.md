<properties
    pageTitle="Aloita tapahtuman keskittimet C# | Microsoft Azure"
    description="Katso tämä opetusohjelma, pääset alkuun käyttämällä Azure tapahtuman keskittimet ja C# ja EventProcessorHost."
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
    ms.date="09/02/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-event-hubs"></a>Tapahtuman keskittimet käytön aloittaminen

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Johdanto

Tapahtuman keskittimet on palvelu, joka käsittelee paljon yhdistetyn laitteita ja sovelluksia tapahtuman tiedot (telemetriatietojen). Jälkeen voit koota tietoja tapahtuman keskittimet, voit tallentaa tiedot käyttämällä tallennustila-klusterin tai muuntamiseen reaaliaikainen analytics-palvelun avulla. Tämä suurissa tapahtuman kerääminen ja käsittely-ominaisuus on nykyaikaisen sovelluksen arkkitehtuureihin, mukaan lukien asiat Internet (IoT) avaimen osa.

Tässä opetusohjelmassa näytetään, miten Azure perinteinen portaalin avulla voit luoda tapahtumaa-toiminnossa. Se myös näyttää kerääminen viestien tapahtumaan-keskittimeen, kirjoitettu C# console-sovelluksesta ja noutamisesta ne rinnakkain käyttämällä C# [Tapahtuman suoritin Host (isäntä)][] -kirjasto.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on seuraavasti:

+ [Microsoft Visual Studio](http://visualstudio.com)

+ Azure active tili. Jos sinulla ei ole, voit luoda ilmaisen tilin muutamaan minuuttiin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/free/).

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan sovellukset.

1. Visual Studio avata aiemmin luomasi **vastaanotin** projektin.

2. **Vastaanottaja** -ratkaisun hiiren kakkospainikkeella ja valitse **Lisää**ja valitse sitten **Olemassa olevaan projektiin**.
 
3. Etsi Sender.csproj aiemmin luotu tiedosto ja valitse Lisää ratkaisuun kaksoisnapsauttamalla.
 
4. Uudelleen **Vastaanottaja** -ratkaisun hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**. **Vastaanottaja** -ominaisuus-sivu tulee näkyviin.

5. Valitse **Käynnistys-projekti**ja valitse sitten **useita käynnistys projektit** -painiketta. Määritä **Action** -ruutuun **vastaanottajan** ja **lähettäjän** projektien **alkavan**.

    ![][19]

6. Valitse **Projektiriippuvuudet**. **Projektit** -ruudussa Valitse **Lähettäjä**. **Riippuvuus** -ruutuun Varmista, että **Vastaanottaja** on valittuna.

    ![][20]

7. Valitse Hylkää **Ominaisuudet** -valintaikkunassa **OK** .

1.  Voit suorittaa **vastaanotin** projektin Visual Studion ja valitse Käynnistä, kaikki osiot vastaanottajia odottamaan painamalla F5.

    ![][21]

2.  **Lähettäjä** -projekti suoritetaan automaattisesti. Konsoli-ikkunan **Enter** -näppäintä ja tuo näkyviin vastaanottaja-ikkunassa näkyvät tapahtumat.

    ![][22]

Paina **Ctrl + C** **Lähettäjä** -ikkunassa, Lopeta lähettäjä-sovellus ja paina **Enter** vastaanottaja-ikkunassa sovellus suljetaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut toimimasta-sovellus, joka luo tapahtuma-toiminnosta ja lähettää ja vastaanottaa tietoja, voit siirtää seuraavissa tilanteissa:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.
- [Tapahtuman keskittimet yleiskatsaus][]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Azure classic portal]: https://manage.windowsazure.com/
[Tapahtuman suoritin Host (isäntä)]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
