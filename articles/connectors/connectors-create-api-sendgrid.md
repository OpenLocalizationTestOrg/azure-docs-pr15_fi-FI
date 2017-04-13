<properties
pageTitle="SendGrid | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. SendGrid yhteyden toimittaja avulla voit lähettää sähköpostia ja vastaanottajien luetteloiden hallinta."
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

# <a name="get-started-with-the-sendgrid-connector"></a>SendGrid yhdistimen käytön aloittaminen

SendGrid yhteyden toimittaja avulla voit lähettää sähköpostia ja vastaanottajien luetteloiden hallinta.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Toiminnon; voidaan käyttää SendGrid-yhdistin siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 SendGrid yhdistin on käytettävissä seuraavat toimet: Ei ole Käynnistimet.

### <a name="sendgrid-actions"></a>SendGrid toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[LähetäSähköposti](connectors-create-api-sendgrid.md#sendemail)|Lähettää sähköpostin SendGrid API (rajoitettu 10 000 vastaanottajille)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Yksittäisen vastaanottajan lisääminen Vastaanottajaluettelon muokkaaminen|


## <a name="create-a-connection-to-sendgrid"></a>Yhteyden muodostaminen SendGrid
Luomiseen logiikan sovellusten SendGrid, sinun on ensin **yhteyden** luominen sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|ApiKey|Kyllä|SendGrid-ohjelmointirajapinnan avain|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

Kun olet luonut yhteyden, voit suorittaa toimintoja ja kuuntele käynnistimien, joka on kuvattu tämän artikkelin.

## <a name="reference-for-sendgrid"></a>SendGrid viittaus
Koskee versiota: 1.0

## <a name="sendemail"></a>LähetäSähköposti
Sähköpostin lähettäminen: lähettää sähköpostin SendGrid API (10 000 vastaanottajat rajoitettu) 

```POST: /api/mail.send.json``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|pyyntö| |Kyllä|tekstissä|ei mitään|Sähköpostiviestin lähettäminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|429|Liian monta pyyntö|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Vastaanottajan lisääminen luetteloon: Lisää yksittäisiä vastaanottaja Vastaanottajaluettelon muokkaaminen 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|listId|merkkijono|Kyllä|polku|ei mitään|Vastaanottajaluettelo yksilöivä tunnus|
|recipientId|merkkijono|Kyllä|polku|ei mitään|Vastaanottajan yksilöllinen tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="emailrequest"></a>EmailRequest


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Valitse|merkkijono|Kyllä |
|fromName|merkkijono|Ei |
|Jos haluat|merkkijono|Kyllä |
|toname|merkkijono|Ei |
|aihe|merkkijono|Kyllä |
|tekstissä|merkkijono|Kyllä |
|ishtml|Totuusarvo|Ei |
|kopio|merkkijono|Ei |
|kopionimi|merkkijono|Ei |
|Piilokopio|merkkijono|Ei |
|piilokopionimi|merkkijono|Ei |
|ReplyTo|merkkijono|Ei |
|päivämäärä|merkkijono|Ei |
|otsikot|merkkijono|Ei |
|tiedostot|matriisi|Ei |
|tiedostonimet|matriisi|Ei |



### <a name="emailresponse"></a>EmailResponse


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|viestin|merkkijono|Ei |



### <a name="recipientlists"></a>RecipientLists


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|luettelot|matriisi|Ei |



### <a name="recipientlist"></a>RecipientList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|Nimi|merkkijono|Ei |
|recipient_count|kokonaisluku|Ei |



### <a name="recipients"></a>Vastaanottajat


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|vastaanottajat|matriisi|Ei |



### <a name="recipient"></a>Vastaanottaja


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|sähköpostin|merkkijono|Ei |
|Sukunimi|merkkijono|Ei |
|Etunimi|merkkijono|Ei |
|tunnus|merkkijono|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)