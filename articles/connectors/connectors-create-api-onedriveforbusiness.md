<properties
pageTitle="OneDrive for Business | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Muodosta yhteys OneDrive for Businessin tiedostojen hallinta. Voit suorittaa erilaisia toimintoja Lataa, kuten päivittäminen, hakea ja poistaa tiedostoja."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-onedrive-for-business-connector"></a>OneDrive for Business connector käytön aloittaminen

Muodosta yhteys OneDrive for Businessin tiedostojen hallinta. Voit suorittaa erilaisia toimintoja Lataa, kuten päivittäminen, hakea ja poistaa tiedostoja.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

OneDrive for Business connector voidaan käyttää toiminnon; siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 OneDrive for Business connector on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="onedrive-for-business-actions"></a>OneDrive for Business-toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Hakee metatiedot tiedoston hakeminen OneDrive for Business-tunnuksen avulla|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|Päivittää onedrive for Business-tiedosto|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Poistaa tiedoston OneDrive for Business|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Hakee metatiedot tiedoston hakeminen OneDrive for Business-polku|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Hakee sisällön tiedoston hakeminen OneDrive for Business-polku|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Hakee sisällön tiedoston hakeminen OneDrive for Business-tunnuksen avulla|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Lataa tiedosto OneDrive for Business|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Kopioi tiedoston OneDrive for Business|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Luetteloiden tiedostoja OneDrive for Business-kansio|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Näyttää tiedostot onedrive for Business-pääkansio|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Hakee kansio onedrive for Business|
### <a name="onedrive-for-business-triggers"></a>OneDrive for Businessin Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun tiedosto on luotu|Tuottaa vuo, kun uusi tiedosto luodaan onedrive for Business-kansio|
|Tiedoston muokkaamisen|Käynnistää vuo, kun tiedosto on muokattu onedrive for Business-kansio|


## <a name="create-a-connection-to-onedrive-for-business"></a>Yhteyden muodostaminen OneDrive for Business
Luo logiikan sovellusten OneDrive for Businessin kanssa, sinun on ensin muodostaa **yhteyden** sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Antaa OneDrive for Business tunnistetiedot|
Kun olet luonut yhteyden, voit suorittaa toimintoja ja kuuntele käynnistimien, joka on kuvattu tämän artikkelin. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-onedrive-for-business"></a>OneDrive for Business-viittaus
Koskee versiota: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Hae tiedosto metatietojen tunnuksen avulla: noutaa metatiedot tiedoston hakeminen OneDrive for Business-tunnuksen avulla 

```GET: /datasets/default/files/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Määritä tiedosto|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="updatefile"></a>UpdateFile
Päivitystiedosto: päivittää onedrive for Business-tiedosto 

```PUT: /datasets/default/files/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Määritä Päivitä tiedosto|
|tekstissä| |Kyllä|tekstissä|ei mitään|Päivitä onedrive for Business-tiedoston sisällön|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="deletefile"></a>DeleteFile
Tiedoston poistaminen: poistaa tiedoston OneDrive for Business 

```DELETE: /datasets/default/files/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Määritä tiedoston poistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Hae tiedosto metatietojen polkutiedot: noutaa metatiedot tiedoston hakeminen OneDrive for Business-polku 

```GET: /datasets/default/GetFileByPath``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|polku|merkkijono|Kyllä|kyselyn|ei mitään|Tiedoston hakeminen OneDrive for Businessin yksilöllinen polku|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Lataa tiedosto sisältö polkutiedot: hakee sisällön tiedoston hakeminen OneDrive for Business-polku 

```GET: /datasets/default/GetFileContentByPath``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|polku|merkkijono|Kyllä|kyselyn|ei mitään|Tiedoston hakeminen OneDrive for Businessin yksilöllinen polku|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="getfilecontent"></a>GetFileContent
Hae tiedosto-sisällön liittämiseen id: hakee sisällön tiedoston hakeminen OneDrive for Business-tunnuksen avulla 

```GET: /datasets/default/files/{id}/content``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Määritä tiedosto|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="createfile"></a>CreateFile
Tiedoston luominen: Lataa tiedosto OneDrive for Business 

```POST: /datasets/default/files``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kansiopolku|merkkijono|Kyllä|kyselyn|ei mitään|Kansiopolku, voit ladata tiedoston OneDrive for Business|
|Nimi|merkkijono|Kyllä|kyselyn|ei mitään|Tiedoston luominen OneDrive for Business|
|tekstissä| |Kyllä|tekstissä|ei mitään|Lataa OneDrive for Business-tiedoston sisällön|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="copyfile"></a>CopyFile
Kopioi tiedosto: kopioi tiedoston OneDrive for Business 

```POST: /datasets/default/copyFile``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|lähde|merkkijono|Kyllä|kyselyn|ei mitään|Lähdetiedosto URL-osoite|
|kohde|merkkijono|Kyllä|kyselyn|ei mitään|Onedrive for Business-palvelua, mukaan lukien kohteen tiedostonimi kohdepolku|
|Korvaa|Totuusarvo|Ei|kyselyn|EPÄTOSI|Korvaa kohdetiedosto, jos arvo on "true"|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="onnewfile"></a>OnNewFile
Kun tiedosto on luotu: tuottaa vuo, kun uusi tiedosto luodaan onedrive for Business-kansio 

```GET: /datasets/default/triggers/onnewfile``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kansiotun.|merkkijono|Kyllä|kyselyn|ei mitään|Määritä kansio|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Tiedoston muokkaamisen: tuottaa vuo, kun tiedosto on muokattu onedrive for Business-kansio 

```GET: /datasets/default/triggers/onupdatedfile``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kansiotun.|merkkijono|Kyllä|kyselyn|ei mitään|Määritä kansio|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="listfolder"></a>ListFolder
Luettele tiedostot-kansiossa: Näyttää tiedostoja OneDrive for Business-kansio 

```GET: /datasets/default/folders/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Määritä kansio|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="listrootfolder"></a>ListRootFolder
Luettelon pääkansio: Näyttää tiedostot onedrive for Business-pääkansio 

```GET: /datasets/default/folders``` 

Ei ole parametreja puhelun
#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Pura kansio: hakee kansio onedrive for Business 

```POST: /datasets/default/extractFolderV2``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|lähde|merkkijono|Kyllä|kyselyn|ei mitään|Arkistotiedoston polku|
|kohde|merkkijono|Kyllä|kyselyn|ei mitään|Onedrive for Business ja purkaa arkisto sisällön polku|
|Korvaa|Totuusarvo|Ei|kyselyn|EPÄTOSI|Korvaa kohde-tiedostoja, jos arvo on "true"|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="datasetsmetadata"></a>DataSetsMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|taulukkomuotoinen|ei ole määritetty|Ei |
|BLOB|ei ole määritetty|Ei |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|lähde|merkkijono|Ei |
|Näyttönimi|merkkijono|Ei |
|urlEncoding|merkkijono|Ei |
|tableDisplayName|merkkijono|Ei |
|tablePluralName|merkkijono|Ei |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|lähde|merkkijono|Ei |
|Näyttönimi|merkkijono|Ei |
|urlEncoding|merkkijono|Ei |



### <a name="blobmetadata"></a>BlobMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Tunnus|merkkijono|Ei |
|Nimi|merkkijono|Ei |
|Näyttönimi|merkkijono|Ei |
|Polku|merkkijono|Ei |
|LastModified|merkkijono|Ei |
|Kokoa|kokonaisluku|Ei |
|MediaType|merkkijono|Ei |
|IsFolder|Totuusarvo|Ei |
|ETag|merkkijono|Ei |
|FileLocator|merkkijono|Ei |



### <a name="object"></a>Objektin


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)