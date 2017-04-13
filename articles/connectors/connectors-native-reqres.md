<properties
    pageTitle="Käytä pyynnön ja vastauksen toiminnot | Microsoft Azure"
    description="Yleisiä tietoja pyynnön ja vastauksen käynnistin ja Azure logiikan sovelluksen-toiminto"
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

# <a name="get-started-with-the-request-and-response-components"></a>Pyynnön ja vastauksen osien käytön aloittaminen

Rakenneosien pyynnön ja vastauksen logiikan-sovelluksessa voit vastata reaaliajassa tapahtumat.

Voit tehdä esimerkiksi seuraavia toimia:

- Vastaa HTTP-pyyntö, jonka tietoja paikallisen tietokannan logiikan-sovelluksella.
- Käynnistää logiikan-sovelluksen ulkoisen webhook tapahtumasta käsin.
- Soita logiikan-sovelluksessa, jossa pyynnön ja vastauksen toiminto logiikkaa toisessa sovelluksessa.

Aloita pyynnön ja vastauksen-toimintojen käyttäminen logiikan-sovelluksessa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>HTTP-pyyntö käynnistimen käyttäminen

Käynnistimen tapahtuma, jonka avulla voidaan käynnistää työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](connectors-overview.md).

Tässä on esimerkki-järjestys, voit määrittää HTTP-pyyntö logiikan sovelluksen suunnittelussa.

1. Lisää logiikan sovelluksen käynnistimen **pyyntö – kun HTTP-pyyntö on vastaanotettu** . Voit halutessasi kirjoittaa JSON rakenteen (käyttämällä työkalua, kuten [JSONSchema.net](http://jsonschema.net)) pyyntö body. Näin luomiseen HTTP-pyyntö tunnusten ominaisuuksien suunnittelu.
2. Lisää toisen toiminnon, jotta voit tallentaa sovelluksen logiikan.
3. Kun olet tallentanut logiikan sovellus, saat HTTP-pyyntö URL-Osoitteen pyynnön kortista.
4. HTTP POST (kuten [Postman](https://www.getpostman.com/)työkalu käyttää) URL-osoitteeseen käynnistää logiikan-sovellus.

>[AZURE.NOTE] Jos et määritä vastaus-toiminto `202 ACCEPTED` vastaus heti palauttaa kutsuja. Vastauksen-toiminnolla voit mukauttaa vastausta.

![Vastauksen käynnistin](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>HTTP-vastaus-toiminnolla

HTTP-vastaus-toiminto on käytössä vain, kun käytät sitä työnkulun, joka on saatu esiin HTTP-pyynnön. Jos et määritä vastaus-toiminto `202 ACCEPTED` vastaus heti palauttaa kutsuja.  Voit lisätä vastausta toiminnon milloin tahansa työnkulun vaiheen. Logiikan sovellus vain pitää saapuvan pyynnön Avaa minuutin ajan vastauksen odottaminen.  Minuutin kuluttua, jos työnkulku on lähettänyt vastausta (ja määrityksessä on vastaus-toiminto) `504 GATEWAY TIMEOUT` palautetaan soittajalle.

Näin voit lisätä toiminnon HTTP-vastaus:

1. Valitse **Uusi vaihe** -painike.
2. Valitse **Lisää toiminnon**.
3. Kirjoita **vastaus** luettelon vastaus-toiminto toiminto-hakuruutuun.

    ![Valitse vastauksen-toiminto](./media/connectors-native-reqres/using-action-1.png)

4. Lisää HTTP-vastausviesti varten tarvittavat parametrit.

    ![Vastauksen-toiminto suoritetaan](./media/connectors-native-reqres/using-action-2.png)

5. Valitse vasemmassa yläkulmassa työkalurivin tallentamaan ja logiikka sovelluksen sekä Tallenna ja julkaise (Aktivoi).

## <a name="request-trigger"></a>Pyydä käynnistin

Seuraavassa on tietoja käynnistimen, joka tukee Tämä yhdistin. On yksittäinen pyyntö käynnistimen.

|Käynnistin|Kuvaus|
|---|---|
|Pyyntö|Tapahtuu, kun HTTP-pyyntö on vastaanotettu|

## <a name="response-action"></a>Vastauksen-toiminto

Seuraavassa on tietoja toiminnon, joka tukee Tämä yhdistin. On yksittäinen vastaus-toiminto, joka voi käyttää vain, kun se on liitettävä pyynnön käynnistimen.

|Toiminto|Kuvaus|
|---|---|
|Vastaus|Palauttaa Korreloidun HTTP-pyynnön vastaus|

### <a name="trigger-and-action-details"></a>Käynnistin ja toimintojen tiedot

Seuraavissa taulukoissa kuvataan käynnistin ja toiminto syötteen kentät ja sitä vastaava Näytä tiedot.

#### <a name="request-trigger"></a>Pyydä käynnistin
Seuraavassa on syöttökenttä käynnistimen saapuvan HTTP-pyynnöstä.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|JSON-rakenne|rakenne|HTTP-pyyntö jotakin JSON-rakenne|
<br>

**Tulostustiedot**

Seuraavassa taulukossa on tulostustiedot pyynnön.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Otsikot|objektin|Pyydä otsikot|
|Tekstissä|objektin|Pyydä objekti|

#### <a name="response-action"></a>Vastauksen-toiminto

Seuraavassa on syötteen kentät HTTP-vastaus-toiminnon. A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Tilan koodi *|statusCode|HTTP-tilakoodi|
|Otsikot|otsikot|JSON-objektia, jonka sisältämään kaikki vastausten otsikot|
|Tekstissä|tekstissä|Vastauksen teksti|

## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile nyt-ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [API-luettelosta](apis-list.md).
