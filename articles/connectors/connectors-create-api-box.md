<properties
    pageTitle="Lisää ruutuun yhdistimen logiikan sovelluksia | Microsoft Azure"
    description="Yleistä REST API parametreilla ruutuun-yhdistin"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Ruutuun yhdistimen käytön aloittaminen
Yhteyden muodostaminen ruutuun ja luo tiedostot ja poista tiedostot. 

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.

-Ruudussa voit tehdä seuraavia toimia:

- Muodostaa yhteyttä business työnkulku, sinulla on ruutuun tietojen perusteella. 
- Käytä käynnistimien, kun tiedosto luodaan tai päivitetään.
- Käytä Toiminnot, jotka tiedoston kopioiminen, poista tiedoston ja paljon muuta. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Esimerkiksi kun tiedosto on muuttunut-ruutu, voit viedä tiedoston ja lähettää sen Office 365: n avulla.

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
Ruutu sisältää seuraavat käynnistin ja toiminnot.

| Käynnistimien | Toiminnot|
| --- | --- |
|<ul><li>Kun tiedosto on luotu</li><li>Tiedoston muokkaamisen</li></ul> | <ul><li>Tiedoston luominen</li><li>Kun tiedosto on luotu</li><li>Tiedoston kopioiminen</li><li>Tiedoston poistaminen</li><li>Pura arkisto-kansioon</li><li>Hae tiedosto-sisällön liittämiseen tunnus</li><li>Hae tiedosto-sisällön liittämiseen polku</li><li>Hae tiedosto metatietojen tunnuksen avulla</li><li>Hae tiedosto metatietojen polkutiedot</li><li>Tiedoston päivittämistä</li><li>Tiedoston muokkaamisen</li></ul>

Yhdistimien tukevat tietojen JSON ja XML-muodossa.

## <a name="create-a-connection-to-box"></a>Yhteyden muodostaminen valinta
Kun lisäät Tämä yhdistin logiikan sovelluksia, on sallittava logiikan sovellusten muodostaa yhteyttä-ruutuun.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Kun olet luonut yhteyden, voit kirjoittaa ominaisuudet. **REST API viittaus** tässä ohjeaiheessa kuvataan nämä ominaisuudet.

>[AZURE.TIP] Voit käyttää samaa ruutuun yhteyden logiikan muista sovelluksista.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-viittaus
Koskee versiota: 1.0.

### <a name="create-file"></a>Tiedoston luominen
Lataa tiedosto-ruutuun.  
```POST: /datasets/default/files```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kansiopolku|merkkijono|Kyllä|kyselyn|Ei mitään |Lataa tiedosto-ruutuun kansiopolku|
|Nimi|merkkijono|Kyllä|kyselyn|Ei mitään |Luo-ruutuun tiedoston nimi|
|tekstissä|String(Binary) |Kyllä|tekstissä|Ei mitään |Tiedoston lataaminen-ruutuun sisältö|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="when-a-file-is-created"></a>Kun tiedosto on luotu
Käynnistää vuo, kun uusi tiedosto luodaan ruutuun-kansioon.  
```GET: /datasets/default/triggers/onnewfile```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kansiotun.|merkkijono|Kyllä|kyselyn|Ei mitään |Yksilöllinen tunnus ruutuun kansion|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="copy-file"></a>Tiedoston kopioiminen
Kopioi tiedosto-ruutuun.  
```POST: /datasets/default/copyFile```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|lähde|merkkijono|Kyllä|kyselyn|Ei mitään |Lähdetiedosto URL-osoite|
|kohde|merkkijono|Kyllä|kyselyn| Ei mitään|Kohteen polku-ruutuun, kuten kohteen tiedostonimi|
|Korvaa|Totuusarvo|Ei|kyselyn| Ei mitään|Korvaa kohdetiedosto, jos arvo on "true"|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="delete-file"></a>Tiedoston poistaminen
Voit poistaa tiedoston-ruudusta.  
```DELETE: /datasets/default/files/{id}```


| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|Ei mitään |Yksilöllinen tunnus-ruudusta poistettavan tiedoston|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="extract-archive-to-folder"></a>Pura arkisto-kansioon
Hakee arkistoidut tiedostot-kansioon-ruutuun (Esimerkki: .zip).  
```POST: /datasets/default/extractFolderV2```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|lähde|merkkijono|Kyllä|kyselyn| |Arkistotiedoston polku|
|kohde|merkkijono|Kyllä|kyselyn| |Pura arkisto-sisältö-ruudussa polku|
|Korvaa|Totuusarvo|Ei|kyselyn| |Korvaa kohde-tiedostoja, jos arvo on "true"|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-file-content-using-id"></a>Hae tiedosto-sisällön liittämiseen tunnus
Hakee tiedoston sisällön tunnus-ruudussa.  
```GET: /datasets/default/files/{id}/content```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|Ei mitään |Yksilöllinen tunnus-ruutuun tiedoston|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-file-content-using-path"></a>Hae tiedosto-sisällön liittämiseen polku
Hakee tiedoston sisällön polku-ruudussa.  
```GET: /datasets/default/GetFileContentByPath```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|polku|merkkijono|Kyllä|kyselyn|Ei mitään |Yksilöivä-ruutuun tiedoston polku|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-file-metadata-using-id"></a>Hae tiedosto metatietojen tunnuksen avulla
Hakee tiedoston metatiedot tunnus-ruudussa.  
```GET: /datasets/default/files/{id}```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku| Ei mitään|Yksilöllinen tunnus-ruutuun tiedoston|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="get-file-metadata-using-path"></a>Hae tiedosto metatietojen polkutiedot
Hakee tiedoston metatiedot polku-ruudussa.  
```GET: /datasets/default/GetFileByPath```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|polku|merkkijono|Kyllä|kyselyn|Ei mitään |Yksilöivä-ruutuun tiedoston polku|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="update-file"></a>Tiedoston päivittämistä
Päivittää tiedosto-ruutuun.  
```PUT: /datasets/default/files/{id}```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku| Ei mitään|Päivitä-ruutuun tiedoston yksilöivä tunnus|
|tekstissä|String(Binary) |Kyllä|tekstissä|Ei mitään |Päivitä-ruutuun tiedoston sisällön|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


### <a name="when-a-file-is-modified"></a>Tiedoston muokkaamisen
Käynnistää vuo, kun tiedosto on muokattu ruutuun-kansiossa.  
```GET: /datasets/default/triggers/onupdatedfile```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kansiotun.|merkkijono|Kyllä|kyselyn|Ei mitään |Yksilöllinen tunnus ruutuun kansion|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Ominaisuuden nimi | Tietotyyppi | Pakollinen|
|---|---|---|
|taulukkomuotoinen|ei ole määritetty|Ei|
|BLOB|ei ole määritetty|Ei|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|lähde|merkkijono|Ei|
|Näyttönimi|merkkijono|Ei|
|urlEncoding|merkkijono|Ei|
|tableDisplayName|merkkijono|Ei|
|tablePluralName|merkkijono|Ei|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|lähde|merkkijono|Ei|
|Näyttönimi|merkkijono|Ei|
|urlEncoding|merkkijono|Ei|

#### <a name="blobmetadata"></a>BlobMetadata

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Tunnus|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|Näyttönimi|merkkijono|Ei|
|Polku|merkkijono|Ei|
|LastModified|merkkijono|Ei|
|Kokoa|kokonaisluku|Ei|
|MediaType|merkkijono|Ei|
|IsFolder|Totuusarvo|Ei|
|ETag|merkkijono|Ei|
|FileLocator|merkkijono|Ei|

## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).
