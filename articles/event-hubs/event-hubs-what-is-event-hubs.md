<properties
    pageTitle="Mikä on Azure tapahtuman keskittimet? | Microsoft Azure"
    description="Yleiskatsaus ja Azure tapahtuman keskittimet kuvaus"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="sethm"/>

# <a name="what-is-azure-event-hubs"></a>Mikä on Azure tapahtuman keskittimet?

Azure tapahtuman keskittimet on erittäin skaalattava tietojen tunkeutumisen palvelu, jota voit ingest tapahtumat sekunnissa miljoonia niin, että voit käsitellä ja analysoida yhdistetyn laitteita ja sovelluksia tuottamat tiedot mahdutettavia. Tapahtuman keskittimet toimii "eteen oven" tapahtuman myyntijakso varten, ja kun tiedot on kerätty tapahtuma-keskittimeen, voidaan muuntaa ja tallentaa minkä tahansa reaaliaikainen analytics-palveluntarjoajan tai jonottaminen ja tallennustilaa sovittimet. Tapahtuman keskittimet decouples stream näitä tapahtumia kulutus tapahtumia tuotannon niin, että tapahtuman kuluttajille käyttää omia aikataulussa tapahtumat. Lisätietoja ja teknisistä tiedoista on artikkelissa [tapahtuman keskittimet yleiskatsaus](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Tapahtuman keskittimet ominaisuudet

Tapahtuman keskittimet on käsittelyn palvelun, joka tarjoaa tapahtuma- ja telemetriatietojen käsittelyn valtaviin skaalaus-pieni viive ja suuri luotettavuutta tapahtuma. Tämä palvelu on hyötyä erityisesti:

- Sovelluksen instrumentation
- Käyttäjäkokemuksen tai työnkulun käsittely
- Asioita Internet (IoT) skenaarioita

Joitakin muita avaimen tapahtuman keskittimet ominaisuuksia ovat toiminnan seuraaminen mobiilisovellukset, tietojen yhdistäminen web klustereihin Ottelu-tapahtuman sieppaus konsolin visualisointi: n tai telemetriatietojen kerääminen teollisuuden koneet tai yhdistetyn ajoneuvot liikennettä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja tapahtuman keskittimet on seuraavissa artikkeleissa.

- [Tapahtuman keskittimet yleiskatsaus](event-hubs-overview.md)
- [Tapahtuman keskittimet ohjelmointi opas](event-hubs-programming-guide.md)
- [Tapahtuman keskittimet saatavuudesta ja tuesta usein kysytyt kysymykset](event-hubs-availability-and-support-faq.md)
- [Tapahtuman keskittimet opetusohjelman][] käytön aloittaminen
- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][]

[Tapahtuman keskittimet opetusohjelma]: event-hubs-csharp-ephcs-getstarted.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
