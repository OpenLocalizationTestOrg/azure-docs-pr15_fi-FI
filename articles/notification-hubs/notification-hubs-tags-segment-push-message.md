<properties
    pageTitle="Reititys ja tunnisteen lausekkeet"
    description="Tässä ohjeaiheessa kerrotaan Azure ilmoituksen keskittimet reititys ja tunnisteen lausekkeita."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Reititys ja tunnisteen lausekkeet

##<a name="overview"></a>Yleiskatsaus

Tunnisteen lausekkeiden käyttöön push-ilmoituksen kautta ilmoituksen keskittimet lähettäminen kohde tietyn tietojoukkojen laitteiden tai tarkemmin merkintöjä.


## <a name="targeting-specific-registrations"></a>Kohdistamisen tietyn merkintöjä

Kohde on ainoa tapa tietyn ilmoituksen merkintöjen on liitettävä niihin tunnisteita kohde sitten tunnisteet. Kuten edellä [Rekisteröinnin](notification-hubs-push-notification-registration-management.md)hallinnassa, push-ilmoitusten vastaanottaminen sovelluksen on Rekisteröi laite hoitamaan-ilmoitus-toiminnossa. Kun rekisteröinti luodaan ilmoitus-keskittimeen, sovelluksen Taustajärjestelmä lähettää siihen push-ilmoitukset.
Sovelluksen Taustajärjestelmä valita merkintöjä kohteeseen tietyn ilmoituksen kanssa seuraavilla tavoilla:

1. **Lähetä**: kaikkien merkintöjen ilmoitus-toiminnossa saat ilmoituksen.
2. **Tunnisteen**: kaikki merkintöjä, jotka sisältävät tietyn tunnisteen saat ilmoituksen.
3. **Merkinnän lauseketta**: kaikkien merkintöjen, jonka joukko tunnisteet vastaavat määritettyä lauseketta saa ilmoituksen.

## <a name="tags"></a>Tunnisteet

Tunnisteen voi olla mikä tahansa merkkijono, enintään 120 merkkiä, joka sisältää aakkosnumeerisia ja seuraavia muita kuin aakkosnumeerisia merkkejä: "_", ‘@’, "#", ". ',':", "-". Seuraavassa esimerkissä esitetään, joista voit saada ilmoitukseen ilmoituksia tietyn musiikkia ryhmät sovellus. Tässä skenaariossa on helppo reitin ilmoitukset on otsikko merkintöjen ja tunnisteita, jotka edustavat eri riveihin, kuten seuraavassa kuvassa.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Tässä kuvassa viestin merkittynä **Beatles** saavuttaa tablet, joka on rekisteröity **Beatles**-tunnisteella.

Lisätietoja merkintöjen tunnisteiden luomisesta on artikkelissa [Rekisteröinti hallinnan](notification-hubs-push-notification-registration-management.md).

Voit lähettää ilmoituksia tunnisteita Lähetä ilmoitukset menetelmät `Microsoft.Azure.NotificationHubs.NotificationHubClient` [Microsoft Azure ilmoituksen keskittimet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK-luokka. Voit käyttää myös Node.js tai Push-ilmoitukset REST API.  Tässä on esimerkki SDK: N avulla.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Tunnisteiden ei tarvitse olla valmiiksi valmisteltu ja voi viitata useisiin app kielikohtaiset käsitteistä. Esimerkiksi tämän esimerkin-sovelluksen käyttäjät voivat kommentoida palkkia ja haluat vastaanottaa niiden ystävien, riippumatta siitä, joina he ovat kommentoinnista rivin toasts, paitsi tuttuja niiden rivien kommentit, mutta myös kaikki kommentit. Seuraavassa kuvassa on esimerkki Tämä skenaario:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Tässä kuvassa Anneli on kiinnostunut Beatles päivitykset ja Teemu on kiinnostunut Wailers päivitykset. Teemu on myös käyttämällä Juhani tekemät kommentit ja Juhani ovat kiinnostuneita Wailers. Ilmoituksen lähettäessään Juhani 's kommentti Beatles Anneli ja Pekka saa sitä.

Voit salata useita koskee tunnisteet (esimerkiksi "band_Beatles" tai "follows_Charlie"), tunnisteet on yksinkertainen merkkijonot ja ei-arvoilla ominaisuuksia. Rekisteröinti vastaa vain-tavoitettavuustietojen tai tietyn tunnisteen puuttuminen.

Katso täysi vaiheittaiset opetusohjelma käyttämisestä tunnisteet ryhmien lähettämistä varten, [Purkaa uutisia](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Tunnisteiden kohde käyttäjille

Toinen tapa käyttää tunnisteita on tunnistavan tietyn käyttäjän kaikkia laitteita. Merkintöjen voi olla merkitty tunnisteen, joka sisältää käyttäjätunnus, kuten seuraavassa kuvassa:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Tässä kuvassa merkitty viesti uid:Alice saavuttaa kaikkien merkintöjen merkittynä uid:Alice; Näin ollen kaikki Anneli 's laitteet.


##<a name="tag-expressions"></a>Tunniste-lausekkeet

On tapauksia, joissa ilmoituksen on kohde, joka on yksittäinen tunniste mukaan, mutta totuusarvolauseke tunnisteiden merkintöjen joukko.

Harkitse urheilu-sovellus, joka lähettää muistutuksen kaikille oppimiasi tietoja Ottelu Cardinals ja punainen Sox välillä. Jos asiakas-sovelluksen Rekisteröi tunnisteet tietoja ryhmiä ja sijainnin, valitse ilmoituksen olisi voidaan kohdistaa kaikille oppimiasi, joka on punainen Sox tai Cardinals käyttämällä. Tämä ehto voidaan esittää totuusarvo seuraava lauseke:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Tunnisteen lausekkeita voi sisältää kaikki loogisia operaattoreita, kuten AND (& &), tai (|), eikä (!). Ne voivat sisältää myös sulkeita. Tunnisteen lauseke on rajoitettu 20 tunnisteita, jos ne sisältävät vain ORs; Muussa tapauksessa ne on rajoitettu 6 tunnisteet.

Tässä on esimerkki lähettämiseen ilmoitukset käyttämällä SDK tunnisteen lausekkeilla.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
