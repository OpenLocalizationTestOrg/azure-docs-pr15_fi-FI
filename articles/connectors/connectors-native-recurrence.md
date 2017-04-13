<properties
    pageTitle="Lisää toistuminen-käynnistimen logiikan sovelluksissa | Microsoft Azure"
    description="Yleisiä tietoja toistuminen käynnistin ja Azure logiikan-sovelluksen käyttäminen."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Toistuminen-käynnistimen käytön aloittaminen

Toistuminen-käynnistimen avulla voit luoda tehokkaita työnkulut pilveen.

Voit tehdä esimerkiksi seuraavia toimia:

- Ajoita työnkulku suoritetaan SQL-toimintosarjan päivittäin.
- Sähköpostin kaikki tweets yhteenveto tietoja tietyn tunnisteen profiilisivua viikkoon.

Voit aloittaa käytön toistuminen-käynnistimen logiikan-sovelluksessa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Käytä toistuminen käynnistin

Käynnistimen tapahtuma, jonka avulla voidaan käynnistää työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](connectors-overview.md).

Tässä on esimerkki-järjestys, voit määrittää toistuminen käynnistimen logiikan-sovelluksessa:

1. Lisää **Toistuminen** -käynnistimen ensimmäiseksi logiikan-sovelluksessa.
2. Täytä toistovälin parametrit.

Logiikan sovellus käynnistyy nyt suorittamisen jälkeen kunkin aikaväli.

![HTTP-käynnistin](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Käynnistimen tiedot

Toistuminen käynnistin on seuraavat ominaisuudet, joita voit määrittää.

Se käynnistyy logiikan app jälkeen määritetyllä aikavälillä.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Taajuus *|taajuus|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Aikavälin *|väli|Toistuminen annetun korkojakso aikaväli.|
|Aikavyöhyke|Aikavyöhyke|Jos alkamisaika annetaan ilman UTC-poikkeama, tämä aikavyöhyke käytetään.|
|Aloitusaika|Aloitusajan|Alkamisaika [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)-muodossa.|
<br>


## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile nyt-ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [API-luettelosta](apis-list.md).
