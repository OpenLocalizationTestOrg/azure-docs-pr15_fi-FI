<properties
    pageTitle="Viiveen lisääminen logiikan sovellusten | Microsoft Azure"
    description="Yleisiä tietoja viive ja viive-toiminnot ja niiden käyttämisestä Azure logiikan-sovelluksen asti."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Aloita viive ja viive-kunnes toiminnot

Käyttämällä viive ja "viive-kunnes" toiminnot, voit suorittaa työnkulun skenaarioita.

Voit tehdä esimerkiksi seuraavia toimia:

- Odota, kunnes viikonpäivä tilapäivitys lähettäminen sähköpostin välityksellä.
- Viive työnkulun, kunnes HTTP-kutsu on aikaa loppuun, ennen kuin jatkaminen ja hakeminen tulos.

Aloita viive-toiminnon avulla logiikan-sovelluksessa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Käytä viive-toiminnot

Toiminto on toiminto, joka suoritetaan työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja toiminnot](connectors-overview.md).

Tässä on esimerkki-sarjan käytöstä viive vaiheen logiikan-sovelluksessa:

1. Kun olet lisännyt käynnistin, valitse **Uusi vaihe** voit lisätä toiminnon.
2. Etsi **viive** ja tuo näkyviin viive-toiminnot. Tässä esimerkissä on valita **viive**.

    ![Viive-toiminnot](./media/connectors-native-delay/using-action-1.png)

3. Suorita Määritä viive toiminto-ominaisuuksia.

    ![Viive config](./media/connectors-native-delay/using-action-2.png)

4. Valitse Julkaise ja aktivoida logiikan-sovelluksen **Tallenna** .


## <a name="action-details"></a>Toiminnon tiedot

Toistuminen käynnistin on seuraavat ominaisuudet, jotka on määritetty.

### <a name="delay-action"></a>Viive-toiminto

Tämä toiminto viivyttää tietyllä aikavälillä Suorita.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Laske *|Laske|Aikayksiköiden luetelmakohtien määrä|
|Yksikön *|yksikkö|Aikayksikkö: `Second`, `Minute`, `Hour`, tai`Day`|
<br>

### <a name="delay-until-action"></a>Viive-toiminnon asti

Tämä toiminto viivyttää Suorita ennen määritettyä päivämäärä ja kellonaika.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Vuoden *|aikaleima|Vuoden luetelmakohtien (GMT) asti|
|Kuukauden *|aikaleima|Kuukauden viiveen lisääminen ennen (GMT)|
|Päivä *|aikaleima|Viiveen lisääminen ennen (GMT) päivä|
<br>


## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile nyt-ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [API-luettelosta](apis-list.md).
