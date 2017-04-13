<properties
pageTitle="Opettele käyttämään FTP-yhdistin logiikan sovelluksissa | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Muodosta yhteys FTP-palvelimen tiedostojen hallinta. Voit tehdä eri toimintoja, kuten lataamisen, päivittää, Hae ja poistaa palvelimen tiedostoja."
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
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Aloita FTP-yhdistin

FTP-yhdistin avulla voit seurata, hallita ja luoda tiedostoja FTP-palvelimessa. 

Jos haluat käyttää [mitä tahansa yhdysviivaa](./apis-list.md), sinun on logiikan sovelluksen luominen. Voit aloittaa luomalla [logiikan sovellus nyt](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Yhteyden muodostaminen FTP

Ennen kuin logiikan sovelluksen voit käyttää mihinkään palveluun, sinun on palvelun *yhteyden* luominen. [Yhteys](./connectors-overview.md) on logiikan-sovellus ja toisen palvelun välinen yhteys.  

### <a name="create-a-connection-to-ftp"></a>Yhteyden muodostaminen FTP

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Käytä FTP käynnistin

Käynnistimen tapahtuma, jonka avulla voidaan aloittaa työnkulun määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]FTP-yhdistin edellyttää FTP-palvelinta, jossa on käytettävissä Internetistä ja on määritetty toimimaan Passiivinen tila. Myös FTP yhdistin ei **ole yhteensopiva implisiittinen FTPS (FTP SSL-yhteyden kautta)**. FTP-yhdistin tukee vain eksplisiittinen FTPS (FTP SSL-yhteyden kautta).  

Tässä esimerkissä voin esitellään, miten **FTP - tiedoston lisännyt tai muokannut** käynnistimen avulla voit aloittaa logiikan app työnkulun, kun tiedosto lisätään tai muokattu, FTP-palvelimeen. Enterprise-esimerkissä käynnistin avulla voi seurata uusia tiedostoja, jotka edustavat asiakkaiden tilaukset FTP-kansio.  Voit käyttää FTP yhdistimen toiminnon esimerkiksi **tiedoston sisällön saaminen** sitten saat tilauksen lisäkäsittelyä ja tallennustilaa sisällön tilaukset-tietokannan.

1. Kirjoittaa hakukenttään *ftp* logiikan sovellusten suunnittelu ja valitse sitten **FTP - tiedoston lisännyt tai muokannut** käynnistin   
![FTP käynnistimen kuva 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
**Kun tiedosto lisätään tai muokata** ohjausobjektin avautuu  
![FTP käynnistimen kuva 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Valitse **...** sijaitsevat ohjausobjektin oikeassa reunassa. Tämä avaa kansion valitsin-ohjausobjekti  
![FTP käynnistimen kuva 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Valitse **>** (oikea nuoli) ja etsi haluamasi kansio, jota haluat seurata uusi tai muokattu tiedostot. Valitse kansio ja huomaat, kansio näkyy nyt **kansion** -ohjausobjekti.  
![FTP käynnistimen kuva 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


Tässä vaiheessa logiikan-sovellus on määritetty käynnistimen, joka alkaa Suorita käynnistimien ja työnkulku, kun tiedosto on muokattu tai luotu FTP haluamaasi kansioon. 

>[AZURE.NOTE]Logiikan sovelluksen toimivan sen on oltava vähintään yksi käynnistin ja yksi toiminto. Noudattamalla voit lisätä toiminnon seuraavan osion ohjeita.  



## <a name="use-a-ftp-action"></a>FTP-toiminnolla

Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto. [Lisätietoja toiminnot](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Nyt kun olet lisännyt käynnistimen, voit lisätä toiminnon, jotka saavat käynnistin löytämä uusi tai muokattu-tiedoston sisällön seuraavasti.    

1. Valitse **+ Uusi vaihe** Lisää saat FTP-palvelimella tiedoston sisällön toiminto  
- Valitse **Lisää toiminnon** linkki.  
![FTP-toiminnon kuva 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Kirjoita Etsi kaikki toiminnot, jotka liittyvät FTP *FTP* .
- Valitse **FTP - tiedoston sisällön saaminen** toiminto, joka suoritetaan, kun uusi tai muokattu tiedosto löytyy FTP-kansio.      
![FTP-toiminnon kuva 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
**Lataa tiedosto-sisältö** -ohjausobjekti avautuu. **Huomautus**: sinua pyydetään vahvistamaan logiikan sovelluksen käyttämään FTP server-tili, jos et ole vielä tehnyt sitä aiemmin.  
![FTP-toiminnon kuva 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Valitse **Tiedosto** -ohjausobjekti (välilyönnit alapuolella **tiedoston***). Tässä kohdassa voit käyttää eri ominaisuuksia löydy FTP-palvelimella uusi tai muokattu tiedostosta.  
- Valitse **tiedoston sisältöä** .  
![FTP-toiminnon kuva 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  Ohjausobjektin päivitetään, joka ilmaisee, että **FTP - tiedoston sisällön saaminen** toiminnon saavat *tiedoston sisällön* uusi tai muokattu tiedoston FTP-palvelimella.      
![FTP-toiminnon kuva 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Tallenna työsi sitten tiedoston lisääminen Testaa työnkulun FTP-kansioon.    

Tässä vaiheessa logiikan-sovellus on määritetty käynnistimen FTP-palvelimessa olevaan kansioon ja aloittaa työnkulun, kun se löytää uuden tiedoston tai muokatun tiedoston FTP-palvelimen kanssa. 

Logiikan sovelluksen myös on määritetty toiminnon saat uusi tai muokattu-tiedoston sisällön.

Voit nyt lisätä toisen toiminnon, kuten [SQL Server - Lisää rivi](./connectors-create-api-sqlazure.md#insert-row) -toiminto, joka uusi tai muokattu-tiedoston sisällön lisääminen SQL-tietokannan taulukkoon.  

## <a name="technical-details"></a>Teknisiä tietoja

Seuraavassa on tietoja käynnistimet, toiminnot ja vastaukset, joka tukee tätä yhteyttä:

## <a name="ftp-triggers"></a>FTP Käynnistimet

FTP on seuraavassa trigger(s):  

|Käynnistin | Kuvaus|
|--- | ---|
|[Kun tiedosto on lisätty tai muutettu](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|Tämä toiminto tuottaa vuo, kun tiedosto on lisätty tai joita olet muokannut kansioon.|


## <a name="ftp-actions"></a>FTP-toiminnot

FTP on seuraavia toimia:


|Toiminto|Kuvaus|
|--- | ---|
|[Hae tiedoston metatiedot](connectors-create-api-ftp.md#get-file-metadata)|Tämä toiminto saa tiedoston metatiedot.|
|[Tiedoston päivittämistä](connectors-create-api-ftp.md#update-file)|Tämä toiminto päivittää tiedoston.|
|[Tiedoston poistaminen](connectors-create-api-ftp.md#delete-file)|Tämä toiminto poistaa tiedoston.|
|[Hae tiedosto metatietojen polkutiedot](connectors-create-api-ftp.md#get-file-metadata-using-path)|Tämä toiminto saa tiedoston polku metatiedot.|
|[Hae tiedosto-sisällön liittämiseen polku](connectors-create-api-ftp.md#get-file-content-using-path)|Tämä toiminto saa polkutiedot tiedoston sisällön.|
|[Lataa tiedosto-sisältö](connectors-create-api-ftp.md#get-file-content)|Tämä toiminto saa tiedoston sisällön.|
|[Tiedoston luominen](connectors-create-api-ftp.md#create-file)|Tämä toiminto luo tiedoston.|
|[Tiedoston kopioiminen](connectors-create-api-ftp.md#copy-file)|Tämä toiminto kopioi tiedoston FTP-palvelimeen.|
|[Luettele tiedostot-kansiossa](connectors-create-api-ftp.md#list-files-in-folder)|Tämä toiminto saa tiedostoja ja kansion alikansioiden.|
|[Pääkansio tiedostojen luettelo](connectors-create-api-ftp.md#list-files-in-root-folder)|Tämä toiminto saa tiedostot ja alikansiot pääkansioon.|
|[Pura kansio](connectors-create-api-ftp.md#extract-folder)|Tämä toiminto hakee arkistoidut tiedostot-kansioon (Esimerkki: .zip).|
### <a name="action-details"></a>Toiminnon tiedot

Seuraavassa on tietoja toiminnot ja yhdistimen, sekä niiden vastaukset käynnistimet:



### <a name="get-file-metadata"></a>Hae tiedoston metatiedot
Tämä toiminto saa tiedoston metatiedot. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|

* Ilmaisee, että ominaisuus on tehty

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="update-file"></a>Tiedoston päivittämistä
Tämä toiminto päivittää tiedoston. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|
|leipäteksti *|Tiedoston sisältö|Tiedoston sisällön|

* Ilmaisee, että ominaisuus on tehty

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="delete-file"></a>Tiedoston poistaminen
Tämä toiminto poistaa tiedoston. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|

* Ilmaisee, että ominaisuus on tehty




### <a name="get-file-metadata-using-path"></a>Hae tiedosto metatietojen polkutiedot
Tämä toiminto saa tiedoston polku metatiedot. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|polun *|Tiedostopolku|Valitse tiedosto|

* Ilmaisee, että ominaisuus on tehty

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="get-file-content-using-path"></a>Hae tiedosto-sisällön liittämiseen polku
Tämä toiminto saa polkutiedot tiedoston sisällön. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|polun *|Tiedostopolku|Valitse tiedosto|

* Ilmaisee, että ominaisuus on tehty




### <a name="get-file-content"></a>Lataa tiedosto-sisältö
Tämä toiminto saa tiedoston sisällön. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Tiedoston|Valitse tiedosto|

* Ilmaisee, että ominaisuus on tehty




### <a name="create-file"></a>Tiedoston luominen
Tämä toiminto luo tiedoston. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|Kansiopolku *|Kansiopolku|Valitse kansio|
|nimi *|Tiedostonimi|Tiedoston nimi|
|leipäteksti *|Tiedoston sisältö|Tiedoston sisällön|

* Ilmaisee, että ominaisuus on tehty

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="copy-file"></a>Tiedoston kopioiminen
Tämä toiminto kopioi tiedoston FTP-palvelimeen. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tietolähteen *|Lähteen URL-osoite|Lähdetiedosto URL-osoite|
|kohde *|Kohteen polku|Kohde-tiedostopolku, mukaan lukien kohteen tiedostonimi|
|Korvaa|Korvaa?|Korvaa kohdetiedosto, jos arvo on "true"|

* Ilmaisee, että ominaisuus on tehty

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="when-a-file-is-added-or-modified"></a>Kun tiedosto on lisätty tai muutettu
Tämä toiminto tuottaa vuo, kun tiedosto on lisätty tai joita olet muokannut kansioon. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|kansiotun. *|Kansion|Valitse kansio|

* Ilmaisee, että ominaisuus on tehty




### <a name="list-files-in-folder"></a>Luettele tiedostot-kansiossa
Tämä toiminto saa tiedostoja ja kansion alikansioiden. 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tunnus *|Kansion|Valitse kansio|

* Ilmaisee, että ominaisuus on tehty



#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="list-files-in-root-folder"></a>Pääkansio tiedostojen luettelo
Tämä toiminto saa tiedostot ja alikansiot pääkansioon. 


Ei ole parametreja puhelun

#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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




### <a name="extract-folder"></a>Pura kansio
Tämä toiminto hakee arkistoidut tiedostot-kansioon (Esimerkki: .zip). 


|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|tietolähteen *|Lähde-arkisto tiedostopolku|Arkistotiedoston polku|
|kohde *|Kohdekansion polku|Kohdekansion polku|
|Korvaa|Korvaa?|Korvaa kohde-tiedostoja, jos arvo on "true"|

* Ilmaisee, että ominaisuus on tehty



#### <a name="output-details"></a>Tulostustiedot

BlobMetadata


| Ominaisuuden nimi | Tietotyyppi |
|---|---|---|
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

Toiminnoista ja käynnistimien yllä voi palauttaa yhden tai useamman HTTP tila-koodeista: 

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe.|
|Oletusarvo|Epäonnistui.|







## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)