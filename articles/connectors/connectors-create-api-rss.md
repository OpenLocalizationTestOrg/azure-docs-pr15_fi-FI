<properties
pageTitle="RSS | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. RSS-Connectorin avulla käyttäjät voivat julkaista ja noutaa syötteen kohteet. Sen avulla käyttäjät voivat aiheuttaa toimintoja, kun uusi kohde on julkaistu syötteen myös."
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

# <a name="get-started-with-the-rss-connector"></a>RSS-yhdistin käytön aloittaminen
RSS on Suositut web syndikointi tiedostomuoto usein päivitettyä sisältöä – kuten blogimerkintöjen ja uutisotsikot julkaisua varten.  Monta sisällön julkaisijat säätää, jotta käyttäjät voivat tilata RSS-syötteen.  RSS-yhdistin avulla voit hakea syötteen tietoja ja liipaisin työnkulut, kun uusia kohteita on julkaistu RSS-syötteen.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

RSS-yhdistin voidaan käyttää toiminnon; siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 RSS-yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="rss-actions"></a>RSS-toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Saat kaikki RSS-syötteen kohteet.|
### <a name="rss-triggers"></a>RSS-Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun uusi syötteen kohde julkaistu|Käynnistää työnkulun, kun uusi syöte on julkaistu|


## <a name="create-a-connection-to-rss"></a>Yhteyden muodostaminen RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-rss"></a>RSS-viittaus
Koskee versiota: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Kun uusi syötteen kohde julkaistu: käynnistää työnkulun, kun uusi syöte on julkaistu 

```GET: /OnNewFeed``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|feedUrl|merkkijono|Kyllä|kyselyn|ei mitään|Syötteen URL-osoite|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="listfeeditems"></a>ListFeedItems
Luettele kaikki RSS-syötteen kohteet: Hae kaikki RSS-syötteen kohteet. 

```GET: /ListFeedItems``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|feedUrl|merkkijono|Kyllä|kyselyn|ei mitään|Syötteen URL-osoite|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|202|Hyväksytty|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="feeditem"></a>FeedItem


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Kyllä |
|otsikko|merkkijono|Kyllä |
|sisältö|merkkijono|Kyllä |
|linkit|matriisi|Ei |
|updatedOn|merkkijono|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)