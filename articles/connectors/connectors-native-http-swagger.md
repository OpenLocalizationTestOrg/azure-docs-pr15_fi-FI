
<properties
    pageTitle="Lisää HTTP + Swagger logiikan sovellukset-toiminto | Microsoft Azure"
    description="Yleistä HTTP + Swagger-toiminto ja toiminnot"
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

# <a name="get-started-with-the-http--swagger-action"></a>Aloita HTTP + Swagger-toiminto

HTTP + Swagger-toimintoa, voit luoda kaikki muut päätepisteen kautta [Swagger asiakirjan](https://swagger.io)pääosin yhdistimen. Voit myös laajentaa logiikan-sovelluksen, kaikki muut päätepisteen pääosin logiikan App Designer miellyttävämpää Soita.

Aloita HTTP + Swagger toiminnon logiikan-sovelluksessa, katso [Uusi logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Käytä HTTP + Swagger käynnistimen tai toiminnon

HTTP + Swagger käynnistin ja toiminto toimi sama kuin [HTTP-toiminnon](connectors-native-http.md) , mutta voi paremmin suunnittelukokemusta näyttämällä Ohjelmointirajapinta ja tulostaa suunnittelija- [Swagger metatiedot](https://swagger.io). Lisäksi voit käyttää HTTP + Swagger käynnistämisen. Jos haluat ottaa kysely käynnistimen, se pitäisi seurata kysely mallia, joka on kuvattu [luominen mukautetun Ohjelmointirajapinnan käyttäminen logiikan sovellukset](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Lisätietoja logiikan app Käynnistimet ja toiminnot.](connectors-overview.md)

Tässä on esimerkki siitä, miten käyttää HTTP + Swagger toiminto toiminnon työnkulun logiikan-sovelluksessa.

1. Valitse **Uusi vaihe** -painike.
2. Valitse **Lisää toiminnon**.
3. Kirjoita toiminto-hakuruutuun luettelon HTTP + Swagger toiminnon **swagger** .

    ![Valitse HTTP + Swagger toiminto](./media/connectors-native-http-swagger/using-action-1.png)

4. Kirjoita Swagger tiedoston URL-osoite:
    - Toimimaan logiikan sovelluksen suunnittelussa URL-osoite on oltava päätepiste HTTPS ja ottanut CORS.
    - Jos Swagger asiakirjan ei täytä tämä vaatimus, voit tallentaa asiakirjan [Azuren tallennustilaan käytössä CORS kanssa](#hosting-swagger-from-storage) .
5. Valitse **Seuraava** ja hahmontaminen Swagger asiakirjasta.
6. Voit lisätä parametreja, joita tarvitaan HTTP-puhelu.

    ![Valmis HTTP-toiminto](./media/connectors-native-http-swagger/using-action-2.png)

1. Valitse vasemmassa yläkulmassa työkalurivin ja että logiikan app säilyttää sekä Tallenna **Tallenna** ja julkaise (Aktivoi).

### <a name="host-swagger-from-azure-storage"></a>Host Swagger Azure säilöstä

Voit viitata Swagger asiakirjan, joka ei ole nykyisessä tai, joka ei täytä suojaus ja suunnittelija Risteävät origin koskevat vaatimukset. Voit ratkaista ongelman, voit tallentaa Swagger asiakirjan Azuren tallennustilaan ja ota käyttöön viittaamaan asiakirjan CORS.  

Näin voit luoda ja määrittää Azuren tallennustilaan Swagger tiedostojen tallentaminen:

1. [Luo Azure-Blob-säiliö Azure-tallennustilan tilin](../storage/storage-create-storage-account.md). (Tämä tapahtuu määritetty käyttöoikeudet **yleisön**.)
2. CORS käyttöön blob. Voit määrittää, että asetusta automaattisesti [PowerShell-komentosarjaa](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) .
3. Lataa Swagger-tiedoston tuominen Blob-objektien. Voit tehdä tämän [Azure portal](https://portal.azure.com) -tai työkaluversiosta, kuten [Azure-tallennustilan Explorer](http://storageexplorer.com/).
1. Viittaus HTTPS-linkki Azure-Blob-säiliö asiakirjaan. (Linkki noudattaa muotoa `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Teknisiä tietoja

Seuraavassa on tietoja Käynnistimet ja toiminnot, jotka tämä HTTP + Swagger connector tukee.

## <a name="http--swagger-triggers"></a>HTTP + Swagger Käynnistimet

Käynnistimen tapahtuma, jonka avulla voidaan käynnistää työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien.](connectors-overview.md) HTTP + Swagger yhdistimellä on yksi käynnistin.

|Käynnistin|Kuvaus|
|---|---|
|HTTP + Swagger|HTTP-soittaminen ja palauttaa vastauksen sisältöä|

## <a name="http--swagger-actions"></a>HTTP + Swagger toiminnot

Toiminto on toiminto, joka suoritetaan työnkulun, joka on määritetty logiikan-sovelluksessa. [Lue lisää toimintoja.](connectors-overview.md) HTTP + Swagger yhdistimellä on yksi mahdollista toiminto.

|Toiminto|Kuvaus|
|---|---|
|HTTP + Swagger|HTTP-soittaminen ja palauttaa vastauksen sisältöä|

### <a name="action-details"></a>Toiminnon tiedot

HTTP + Swagger yhdistimen sisältyy yksi mahdollista toiminto. Seuraavassa on lisätietoja kustakin toiminnot, niiden pakolliset ja valinnaiset syötteen kentät ja vastaavat tulostustiedot, jotka liittyvät niiden käyttö.

#### <a name="http--swagger"></a>HTTP + Swagger

Varmista lähtevän HTTP-pyynnön apua Swagger metatiedot.
Tähdellä (*) tarkoittaa muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Menetelmä *|menetelmä|HTTP-toiminnon käyttäminen.|
|URI *|URI|HTTP-pyyntö URI.|
|Otsikot|otsikot|HTTP-otsikoiden sisältämään JSON objekti.|
|Tekstissä|tekstissä|HTTP-pyyntö tekstissä.|
|Todennus|todennus|Todennus käyttämään pyynnön. [Lisätietoja on kohdassa HTTP](./connectors-native-http.md#authentication).|

**Tulostustiedot**

HTTP-vastaus

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Otsikot|objektin|Vastausten otsikot|
|Tekstissä|objektin|Vastauksen objekti|
|Tilakoodin|kokonaisluku|HTTP-tilakoodi|

### <a name="http-responses"></a>HTTP-vastaukset

Kun kutsut erilaisia toimintoja, voit saada tiettyjä vastaukset. Seuraavassa on taulukko, jossa kuvataan vastaavan vastaukset ja kuvauksia.

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe.|

---

## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md) nyt. Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [ohjelmointirajapinnan luettelo](apis-list.md).
