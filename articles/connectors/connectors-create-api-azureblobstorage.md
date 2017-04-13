<properties
    pageTitle="Azure-blob-säiliö yhdysviivan lisääminen logiikan sovelluksia | Microsoft Azure"
    description="Azure-blob-säiliö yhdistimen REST API parametreilla yleiskatsaus"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>Azure-blob storage yhdistimen käytön aloittaminen
Azure-Blob-säiliö on palvelu, projektiin rakenteeton suurista tietomääristä. Tehdä eri toimintoja, kuten lataamisen, päivittää, Hae ja poistaa Azure-blob-säiliö BLOB. 

Azure-blob-säiliö, jossa voit:

- Työnkulun luominen lataamalla uusien projektien ja tiedostoja, jotka on päivitetty viimeksi käytön.
- Toimintojen avulla voit saada tiedoston metatietoja, poista tiedoston ja kopioi tiedostot. Esimerkiksi työkalun päivittämisestä Azure web-sivuston (käynnistimen), valitse Päivitä tiedoston Blob-objektien tallennustilaan (toiminto). 

Tämän artikkelin avulla voit käyttää blob storage yhdistimen logiikan-sovelluksessa ja siinä myös toiminnot.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten yleiseen käyttöön (GA). 

Lisätietoja logiikan sovellukset on artikkelissa [mitä logiikan sovellukset](../app-service-logic/app-service-logic-what-are-logic-apps.md) ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Yhteyden muodostaminen Azuren Blob-objektien tallennustilaan

Ennen kuin logiikan sovelluksen voit käyttää mihinkään palveluun, sinun on luotava *yhteyden* palveluun. Yhteyden sisältää logiikan-sovellus ja toisen palvelun välinen yhteys. Esimerkiksi muodostaa tallennustilan tilin luotava blob storage *yhteys*. Jos haluat luoda yhteyden, kirjoitettava tunnistetiedot, voit tavallisesti käyttää palvelua, johon olet muodostamassa yhteyttä. Niin kanssa Azure tallennusta, Syötä yhteyden muodostaminen tallennustilan tilin tunnistetiedot. 

#### <a name="create-the-connection"></a>Yhteyden muodostaminen

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>Käytä käynnistin

Tämä yhdistin ei ole Käynnistimet. Käynnistä logiikan sovellus, esimerkiksi toistuminen käynnistimen HTTP Webhook käynnistin, käynnistimien ja muut yhdysviivat käytettävissä muissa käynnistimien avulla. [Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md) on esimerkki.

## <a name="use-an-action"></a>Toiminnon käyttäminen
    
Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto.

1. Valitse plus-merkkiä. Näet useita vaihtoehtoja: **Lisää toiminnon**, **Lisää ehto**tai jokin **Lisää** asetuksia.

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Valitse **Lisää toiminnon**.

3. Kirjoita tekstiruutuun "blob-saat kaikki käytettävissä olevat toiminnot luettelon.

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. Valitse tämän esimerkin **AzureBlob - tiedoston metatietojen polkutiedot noutaminen**. Jos on jo yhteys, valitse **** ... Valitse tiedoston (Näytä valitsin)-painiketta.

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    Jos ohjelma pyytää yhteystiedot, kirjoita tiedot yhteyden luominen. [Luo yhteys](connectors-create-api-azureblobstorage.md#create-the-connection) tässä ohjeaiheessa kuvataan nämä ominaisuudet. 

    > [AZURE.NOTE] Tässä esimerkissä on tiedoston metatietojen noutaminen. Metatietojen näkyviin lisää toisen toiminnon, joka luo uuden tiedoston toisessa Connectorin avulla. Lisää esimerkiksi OneDrive-toiminto, joka luo uuden "testata" tiedoston metatietojen perusteella. 

5. **Tallenna** tekemäsi muutokset (vasemmassa yläkulmassa työkalurivin). Logiikan sovelluksen on tallennettu, ja se saattaa olla käytössä automaattisesti.

> [AZURE.TIP] [Tallennustilan Explorer](http://storageexplorer.com/) on erinomainen työkalu, voit hallita useita tallennustilan.

## <a name="technical-details"></a>Teknisiä tietoja

## <a name="storage-blob-actions"></a>Tallennustilan Blob-toiminnot

|Toiminto|Kuvaus|
|--- | ---|
|[Hae tiedoston metatiedot](connectors-create-api-azureblobstorage.md#get-file-metadata)|Tämä toiminto saa tiedoston metatiedot käyttämällä tunnus.|
|[Tiedoston päivittämistä](connectors-create-api-azureblobstorage.md#update-file)|Tämä toiminto päivittää tiedoston.|
|[Tiedoston poistaminen](connectors-create-api-azureblobstorage.md#delete-file)|Tämä toiminto poistaa tiedoston.|
|[Hae tiedosto metatietojen polkutiedot](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Tämä toiminto saa tiedoston metatiedot polkutiedot.|
|[Hae tiedosto-sisällön liittämiseen polku](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Tämä toiminto saa tiedoston sisällön polkutiedot.|
|[Lataa tiedosto-sisältö](connectors-create-api-azureblobstorage.md#get-file-content)|Tämä toiminto saa tiedoston sisällön tunnuksen avulla.|
|[Tiedoston luominen](connectors-create-api-azureblobstorage.md#create-file)|Tämä toiminto Lataa tiedosto.|
|[Tiedoston kopioiminen](connectors-create-api-azureblobstorage.md#copy-file)|Tämä toiminto kopioi tiedoston Azure-Blob-objektien tallennustilaan.|
|[Pura arkisto-kansioon](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Tämä toiminto hakee arkistoidut tiedostot-kansioon (Esimerkki: .zip).|

### <a name="action-details"></a>Toiminnon tiedot

Tässä osassa on tarkkoja tietoja kunkin toiminnon, mukaan lukien kaikki pakolliset ja valinnaiset syötteen ominaisuudet ja yhdistimen liittyvät vastaavan tuloksen.

#### <a name="get-file-metadata"></a>Hae tiedoston metatiedot
Tämä toiminto saa tiedoston metatiedot käyttämällä tunnus.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|


#### <a name="update-file"></a>Tiedoston päivittämistä
Tämä toiminto päivittää tiedoston.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|
|leipäteksti *|Tiedoston sisältö|Jos haluat päivittää tiedoston sisällön|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|


#### <a name="delete-file"></a>Tiedoston poistaminen
Tämä toiminto poistaa tiedoston.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="get-file-metadata-using-path"></a>Hae tiedosto metatietojen polkutiedot
Tämä toiminto saa tiedoston metatiedot polkutiedot.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|polun *|Tiedostopolku|Valitse tiedosto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|


#### <a name="get-file-content-using-path"></a>Hae tiedosto-sisällön liittämiseen polku
Tämä toiminto saa tiedoston sisällön polkutiedot.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|polun *|Tiedostopolku|Valitse tiedosto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="get-file-content"></a>Lataa tiedosto-sisältö
Tämä toiminto saa tiedoston sisällön tunnuksen avulla.  

|Ominaisuuden nimi| Tietotyyppi|Kuvaus|
| ---|---|---|
|tunnus *|merkkijono|Valitse tiedosto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="create-file"></a>Tiedoston luominen
Tämä toiminto Lataa tiedosto.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|Kansiopolku *|Kansiopolku|Valitse kansio|
|nimi *|Tiedostonimi|Voit ladata tiedoston nimi|
|leipäteksti *|Tiedoston sisältö|Voit ladata tiedoston sisällön|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi | 
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|


#### <a name="copy-file"></a>Tiedoston kopioiminen
Tämä toiminto kopioi tiedoston Azure-Blob-objektien tallennustilaan.  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tietolähteen *|Lähteen URL-osoite|Määritä URL-osoite lähdetiedosto|
|kohde *|Kohteen polku|Määritä kohdepolku, mukaan lukien kohteen tiedostonimi|
|Korvaa|Korvaa?|On aiemmin luotu kohdetiedosto voi korvata (tosi tai EPÄTOSI)?  |

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|

#### <a name="extract-archive-to-folder"></a>Pura arkisto-kansioon
Tämä toiminto hakee arkistoidut tiedostot-kansioon (Esimerkki: .zip).  

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tietolähteen *|Lähde-arkisto tiedostopolku|Valitse arkistotiedosto|
|kohde *|Kohdekansion polku|Jos haluat poimia sisällön valitseminen|
|Korvaa|Korvaa?|On aiemmin luotu kohdetiedosto voi korvata (tosi tai EPÄTOSI)?|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
BlobMetadata

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Nimi|merkkijono|
|Näyttönimi|merkkijono|
|Polku|merkkijono|
|LastModified|merkkijono|
|Kokoa|kokonaisluku|
|MediaType|merkkijono|
|IsFolder|Totuusarvo|
|ETag|merkkijono|
|FileLocator|merkkijono|


## <a name="http-responses"></a>HTTP-vastaukset

Kun kutsut erilaisia toimintoja, näkyviin voi tulla tiettyjä vastaukset. Seuraavassa taulukossa kuvataan vastaukset ja niiden kuvaukset:  

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|

## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Tutustu muiden käytettävissä yhdistimet logiikan sovellusten etsiminen [API-luettelosta](apis-list.md).



