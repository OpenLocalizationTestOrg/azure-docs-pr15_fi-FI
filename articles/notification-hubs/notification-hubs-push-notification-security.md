<properties
    pageTitle="Ilmoitus keskittimet suojaus"
    description="Tässä ohjeaiheessa kerrotaan Azure ilmoituksen keskittimet suojaus."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Tietoturva

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa kerrotaan Azure ilmoituksen keskittimet suojausmalli. Koska ilmoituksen keskittimet palvelun Bus kohteen, ne toteuttaa saman suojausmalli palvelun Bus nimellä. Lisätietoja on ohjeaiheissa [Bus todentaminen](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Jaetun Access allekirjoitus-suojauksen (SAS) 

Keskittimet toteuttaa kohteen käyttäjätason suojauksen värimallin korostettuna Suojaussidosten (jaettu Access allekirjoitus). Tämä malli mahdollistaa tekstiviesti kohteet voivat määritellä 12 käyttöoikeuksien myöntämissääntöjä niiden kuvaus, joka antaa kyseisen kohteen käyttöoikeudet.

Niiden sääntöjen sisältää nimen, avainarvon (jaettu salaisuus) ja oikeuksien joukko kuvatulla tavalla osan "Suojauksen saatavat." Ilmoitus-toiminnossa luotaessa kaksi säännöt luodaan automaattisesti: yksi kuunnella oikeuksilla (eli asiakas-sovellus käyttää) ja toinen kaikki oikeuksilla (joka käyttää sovelluksen Taustajärjestelmä).

Suoritettaessa rekisteröinnin hallinta asiakas-sovelluksista, jos tiedot lähetetyissä kutsuissa ilmoituksia ei ole merkitsevä (esimerkiksi säätietoja päivitykset), yleinen tapa käyttää ilmoitus-toiminnossa on antaa avainarvon säännön vain kuunnella asiakas-sovellukseen ja antamaan säännön täydet avainarvon app Taustajärjestelmä.

Ei ole suositeltavaa, että voit upottaa avainarvon Windows-kaupan asiakassovellukset. Tapa välttää upottaminen avainarvon on asiakas-sovellus on noutaa app taustaan käynnistyksen yhteydessä.

On tärkeää ymmärtää, että kuunnella käytön avaimen avulla asiakas-sovelluksen tunnisteen Rekisteröidy. Jos sovellus on rajoittaa merkintöjen tunnisteet tietyn asiakkaille (esimerkiksi silloin, kun tunnisteet vastaavat käyttäjätunnuksiin), sovelluksen Taustajärjestelmä suoritettava merkintöjä. Lisätietoja on artikkelissa rekisteröinnin hallinnan. Huomaa, että tällä tavalla asiakas-sovellus ei ole ilmoituksen porttiin tutustuminen.

##<a name="security-claims"></a>Suojauksen vaateita koskevat rajoitukset

Samalla muihin kohteisiin, ilmoitus-toiminnossa toimintojen sallittu kolme suojauksen saatavat: kuunnella, Lähetä ja hallinta.

| Varaa | Kuvaus | Sallitut toiminnot |
|-------|-------------|--------------------|
| Kuuntele | Luo tai päivitä, lue ja poista yksittäinen | Luo tai päivitä rekisteröinti<br><br>Lue rekisteröinti<br><br>Kaikkien merkintöjen osalta hoitamaan lukeminen<br><br>Poista rekisteröinti |
| Lähetä | Viestit lähetetään ilmoitus-toiminnossa | Viestin lähettäminen |
| Hallinta | CRUDs ilmoituksen keskittimet (mukaan lukien päivittäminen PNS tunnistetiedot ja suojausavaimet) ja lue merkintöjen tunnisteiden perusteella | Ilmoituksen luominen, päivittäminen ja luku/poistaminen keskittimet<br><br>Lue merkintöjen tunnisteen mukaan |


Ilmoitus keskittimet Hyväksy Microsoft Azure käyttöoikeuksien valvonta tunnusten ja luotu ja määritetty suoraan ilmoitus-toiminnossa jaetun näppäimet allekirjoituksen tunnuksia myönnetyt saatavat.