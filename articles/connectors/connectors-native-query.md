<properties
    pageTitle="Kysely-toiminnon lisääminen logiikan sovelluksissa | Microsoft Azure"
    description="Yleistä kysely-toiminnon suodattimen matriisi kuten toimintojen tekemistä varten."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Kysely-toiminnon käytön aloittaminen

Kysely-toiminnon avulla voit käsitellä erissä ja matriisien käyttöä työnkulkuja, joilla voit tehdä:

- Luo tehtävän kaikki tärkeäksi tietueita tietokannasta.
- Tallenna kaikki PDF-liitteet Azure-blob sähköpostiviestejä.

Aloita kysely-toiminnon avulla logiikan-sovelluksessa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Kysely-toiminnolla

Toiminto on toiminto, joka suoritetaan työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja toiminnot](connectors-overview.md).  

Kysely-toiminto on tällä hetkellä yhdellä kertaa, kutsutaan suodatin-taulukko, joka on määritetty suunnittelussa. Näin voit matriisin kyselyn ja palauttaa joukon suodatettuihin tuloksiin.

Näin voit lisätä sen logiikan-sovelluksessa:

1. Valitse **Uusi vaihe** -painike.
2. Valitse **Lisää toiminnon**.
3. Kirjoita **Suodata** luettelo **suodattimen matriisi** -toiminto toiminto-hakuruutuun.

    ![Valitse kysely-toiminto](./media/connectors-native-query/using-action-1.png)

4. Valitse matriisin suodattamiseen. (Seuraavassa näyttökuvassa näkyy Twitter-haun tulokset matriisi.)
5. Luo arvioidaan kullekin kohteelle ehto. (Seuraavista näyttökuvan suodattaa tweets-käyttäjät, joilla on yli 100 seuraajat.)

    ![Suorita kysely-toiminto](./media/connectors-native-query/using-action-2.png)

    Toiminto siirtää uusi taulukko, joka sisältää vain tuloksia, jotka suodattimen vaatimukset täyttyvät.
6. Valitse vasemmassa yläkulmassa työkalurivin tallentamaan ja logiikka sovelluksen sekä Tallenna ja julkaise (Aktivoi).

## <a name="query-action"></a>Kysely-toiminto

Seuraavassa on tietoja toiminnon, joka tukee Tämä yhdistin. Yhdistimen on yksi mahdollista toiminto.

|Toiminto|Kuvaus|
|---|---|
|Taulukon suodattaminen|Laskee matriisin kunkin kohteen ehto ja palauttaa tulokset|

## <a name="action-details"></a>Toiminnon tiedot

Kysely-toiminto on mahdollista toiminnolla. Seuraavassa taulukossa kuvataan pakolliset ja valinnaiset syötteen kentät toiminnon ja vastaavat tulostustiedot, jotka liittyvät-toiminnolla.

### <a name="filter-array"></a>Taulukon suodattaminen
Seuraavassa on toiminto, joka tekee lähtevän HTTP-pyynnön syötteen kentät.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Valitse *|Valitse|Taulukon suodattaminen|
|Ehto *|Jos|Arvioi kutakin ehdon|
<br>

### <a name="output-details"></a>Tulostustiedot

Seuraavassa taulukossa on tulostustiedot HTTP-vastauksen.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Suodatettu matriisi|matriisi|Taulukko, joka sisältää objektin kunkin suodatettu tulos|

## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile nyt-ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [API-luettelosta](apis-list.md).
