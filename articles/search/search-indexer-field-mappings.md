<properties
pageTitle="Kenttä-määrityksiä Azure haun Indeksoijilla"
description="Määritä Azure haun indeksointitoiminto kentän yhdistämismääritykset laskemisesta kenttien nimien ja tietojen esittämiseen erot"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Kenttä-määrityksiä Azure haun Indeksoijilla

Azure haun Indeksoijilla käytettäessä toisinaan löydät itsellesi tilanteissa, jossa syötetietoja aivan ei vastaa kohde-hakemiston rakenne. Tällöin voit **kentän yhdistämismäärityksiä** voi muuntaa tietojen haluamasi muoto. 

Joissakin tilanteissa, joissa kentän yhdistämismäärityksiä on hyötyä:
 
- Tietolähde on kentän `_id`, mutta Azure haku ei salli kentän nimen alussa on alaviiva. Kenttien yhdistämismäärityksen voit kentän "nimeä". 
- Haluat täyttää useita kenttiä samaan tiedot lähdetiedot, esimerkiksi, koska haluat käyttää eri analyzers kenttiä. Kentän yhdistämismäärityksiä mahdollistavat "haarautuvat" tietolähteen kenttä.
- Voit Base64 koodata tai purkaa tiedot. Kenttien yhdistämismääritykset tukevat useita **Yhdistäminen Funktiot**, kuten Funktiot Base64 koodaus ja koodauksen.   


> [AZURE.IMPORTANT] Kentän yhdistämismäärityksiä toiminnoista on tällä hetkellä esikatselu. Se on käytettävissä vain REST-Ohjelmointirajapinnalla versio **2015-02-28-esikatselu**. Ota muista, esikatsella ohjelmointirajapinnan on tarkoitettu testaus ja arviointi ja ei voi käyttää tuotantoympäristössä.

## <a name="setting-up-field-mappings"></a>Kentän yhdistämismäärityksiä määrittäminen

Voit lisätä kentän yhdistämismäärityksiä, kun luot uuden indeksointitoiminto, [Luo indeksointitoiminto](search-api-indexers-2015-02-28-preview.md#create-indexer) Ohjelmointirajapinnan käyttäminen. Voit hallita indeksoinnin indeksointitoiminto [Päivityksen indeksointitoiminto](search-api-indexers-2015-02-28-preview.md#update-indexer) Ohjelmointirajapinnan käyttäminen Kenttämääritykset. 

Yhdistämismäärityksen kenttä sisältää 3 osat: 

1. A `sourceFieldName`, joka edustaa tietolähteen kentän. Tämä ominaisuus on pakollinen. 

2. Valinnainen `targetFieldName`, joka edustaa etsinnän indeksiin kentän. Jos jätetään pois, on sama nimi kuin tietolähteen käytetään. 

3. Valinnainen `mappingFunction`, jotka voivat muuttaa yhden useista tietojen ennalta määritettyjä toimintoja. Funktiot täydellinen luettelo on [alle](#mappingFunctions).

Kenttien yhdistäminen lisätään `fieldMappings` indeksointitoiminto määrityksen matriisi. 

Esimerkiksi näin miten voi olla eroja kenttien nimissä: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indeksointitoiminto voi olla useita kentän yhdistämismäärityksiä. Esimerkiksi seuraavalla siitä, miten voit voit "haarautuvat" kentän seuraavasti:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure haku käyttämällä asennustavan vertailu ratkaista kentän yhdistämismäärityksiä kentän ja funktio nimet. Tämä on kätevä (sinun ei tarvitse kaikki kannen oikea), mutta se tarkoittaa sitä, tietolähteen tai indeksi voi olla kenttiä, jotka eroavat toisistaan vain kirjainkoko.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Kentän määritystä Funktiot

Nämä funktiot ovat tällä hetkellä tueta: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Suorittaa *URL-Osoitteen sopivaa* Base64 koodaus syötteen merkkijonon. Oletetaan, että syöte on koodattu UTF-8. 

#### <a name="sample-use-case"></a>Esimerkki Käyttötapaus 

Vain URL-Osoitteen sopivaa merkit näkyvät Azure haku-asiakirjan avain (koska asiakkaat voivat osoitteen haku-Ohjelmointirajapinnan käyttäminen esimerkiksi asiakirja). Jos haluat käyttää etsinnän indeksiin avaimen kentän tiedot on URL-osoite sisältää tietoturvariskin merkkejä, käyttää tätä toimintoa.   

#### <a name="example"></a>Esimerkki 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Suorittaa Base64 koodauksen syötteen merkkijonon. Syöte oletetaan *URL-Osoitteen sopivaa* Base64-koodatun merkkijonon. 

#### <a name="sample-use-case"></a>Esimerkki Käyttötapaus 

Blob-objektien mukautetun metatietojen arvojen on oltava ASCII-koodattu. Voit käyttää Base64 koodaus edustavan haluamaansa Unicode-merkkijonot Blob-objektien mukautetun metatiedot. Kuitenkin tehdä haun kuvaava, voit tehdä tämän funktion muuttuvat tiedoille takaisin "Normaali" merkkijonot kun täyttää Etsi indeksi.  

#### <a name="example"></a>Esimerkki 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Jakaa string ‑kenttään määritetyn erotin ja poimii tunnuksen merkkijonon määritetyssä tuloksena Jaa osiin.

Jos syöte on esimerkiksi `Jane Doe`, `delimiter` on `" "`(välilyönti) ja `position` on 0, tulos on `Jane`; Jos `position` on 1, tulos on `Doe`. Jos sijainti viittaa tunnuksen, joka ei ole, palautetaan virhe.

#### <a name="sample-use-case"></a>Esimerkki Käyttötapaus 

Tietolähde sisältää `PersonName` kentän ja haluat luoda hakemiston kaksi erillisenä `FirstName` ja `LastName` kentät. Tämän funktion avulla voit jakaa välilyönnin erottimena syötettä.

#### <a name="parameters"></a>Parametrit

- `delimiter`: merkkijonon käyttäminen erottimena, kun jaat syötteen merkkijonon.
- `position`: kokonaisluku salasana ja valitse sen jälkeen, kun syötteen merkkijono on jaettu nollasta sijainti.    

#### <a name="example"></a>Esimerkki

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Muuntaa tekstimerkkijonon muotoillut JSON matriisina merkkijonojen merkkijonotaulukko, jonka avulla voit täyttää `Collection(Edm.String)` indeksi-kenttään. 

Jos syötteen merkkijono on esimerkiksi `["red", "white", "blue"]`, valitse kohdekentän tyyppi `Collection(Edm.String)` täytetään kolme arvoa `red`, `white` ja `blue`. Syöttöarvojen, joka ei voida jäsentää kuin JSON merkkijonon matriiseja palautetaan virhe. 

#### <a name="sample-use-case"></a>Esimerkki Käyttötapaus

Azure SQL-tietokanta ei ole valmiita tietotyyppi, joka yhdistää luonnollisesti `Collection(Edm.String)` Azure kentät. Täytä merkkijonon sivustokokoelman kentät Muotoile lähdetiedot JSON merkkijonon matriisina ja käyttää tätä toimintoa. 

#### <a name="example"></a>Esimerkki 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Auta meitä parantamaan Azure haun toimintaa

Jos sinulla on ehdottaa ominaisuuksia tai ideoita tehokkuutta, ota saavuttaminen meille [UserVoice-sivustossa](https://feedback.azure.com/forums/263029-azure-search/).