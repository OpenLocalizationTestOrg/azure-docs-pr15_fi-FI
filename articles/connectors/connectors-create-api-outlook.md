<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Outlook.com Connectorin mahdollistaa sähköpostin, kalenterin ja yhteystietojen hallinta. Voit tehdä eri toimintoja, kuten Lähetä viesti, kokousten ajoittaminen, Lisää yhteystieto-, jne."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Outlook.com-yhdistin käytön aloittaminen

Outlook.com Connectorin mahdollistaa sähköpostin, kalenterin ja yhteystietojen hallinta. Voit tehdä eri toimintoja, kuten Lähetä viesti, kokousten ajoittaminen, Lisää yhteystieto-, jne.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Toiminnon; voi käyttää Outlook.com-yhdistin siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 Outlook.com-yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="outlookcom-actions"></a>Outlook.com-toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Hakee sähköpostit kansio|
|[LähetäSähköposti](connectors-create-api-outlook.md#SendEmail)|Lähettää sähköpostia|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Poistaa sähköpostiviestin tunnuksen mukaan|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Merkitsee sähköpostiviestin luettu|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Sähköpostin vastaukset|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Hakee sähköpostin liitteen tunnuksen mukaan|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Lähetä sähköpostiviesti, jossa on useita vaihtoehtoja ja odota, että vastaanottaja voi vastaa takaisin jokin vaihtoehdoista|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Lähetä sähköpostiviesti hyväksyntä ja odota, kunnes vastaanottaja vastausta|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Hakee kalenterit|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Hakee kalenterin kohteet|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Luo uusi tapahtuma|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Tietyn kohteen hakee kalenteri|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Poistaa kalenterikohteen|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Päivittää osittain kalenterikohteen|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Hakee yhteystietokansioihin|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Hakee yhteystietoja Yhteystiedot-kansiosta|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Luo uusi yhteyshenkilö|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Hakee tietyn yhteyshenkilön Yhteystiedot-kansiosta|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Poistaa yhteyshenkilön|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Osittain päivittää yhteystiedon|
### <a name="outlookcom-triggers"></a>Outlook.com-Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Valitse tapahtuma alkaa pian|Käynnistää vuo tulevien kalenteritapahtuman käynnistettäessä|
|Valitse uusi sähköpostiviesti.|Käynnistää vuo, kun uusi sähköposti saapuu|
|Valitse uudet kohteet|Kun luot uuden kalenterikohteen saatu|
|Päivitetty kohteet|Käynnistyy, kun kalenterikohteen on muokattu|


## <a name="create-a-connection-to-outlookcom"></a>Yhteyden muodostaminen Outlook.com
Luo logiikan sovellusten Outlook.comiin, sinun on ensin muodostaa **yhteyden** sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Outlook.com-tunnistetietoja|
Kun olet luonut yhteyden, voit suorittaa toimintoja ja kuuntele käynnistimien, joka on kuvattu tämän artikkelin.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.  

## <a name="reference-for-outlookcom"></a>Outlook.com-viittaus
Koskee versiota: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Valitse tapahtuma alkaa pian: käynnistää vuo tulevien kalenteritapahtuman käynnistettäessä 

```GET: /Events/OnUpcomingEvents``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|kyselyn|ei mitään|Kalenterin yksilöivä tunnus|
|lookAheadTimeInMinutes|kokonaisluku|Ei|kyselyn|15|Aika (minuutteina) voit etsiä tulevista tapahtumista eteenpäin|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|202|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="getemails"></a>GetEmails
Hae sähköpostit: hakee sähköpostit kansio 

```GET: /Mail``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kansiopolku|merkkijono|Ei|kyselyn|Saapuneet-kansioon|Hakea sähköpostiviestejä sen kansion polku, (oletus: "Saapuneet")|
|alkuun|kokonaisluku|Ei|kyselyn|10|Hakea sähköpostiviestejä määrä (oletus: 10)|
|fetchOnlyUnread|Totuusarvo|Ei|kyselyn|TOSI|Noutaa vain lukemattomien sähköpostiviestien?|
|includeAttachments|Totuusarvo|Ei|kyselyn|EPÄTOSI|Jos arvo on TOSI, liitteet myös noudetaan yhdessä sähköposti|
|searchQuery|merkkijono|Ei|kyselyn|ei mitään|Voit suodattaa haun kyselyn sähköpostit|
|Ohita|kokonaisluku|Ei|kyselyn|0|Voit ohittaa sähköpostit määrä (oletusarvo: 0)|
|skipToken|merkkijono|Ei|kyselyn|ei mitään|Siirry tunnuksen Nouda uusi sivu|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="sendemail"></a>LähetäSähköposti
Sähköpostin lähettäminen: lähettää sähköpostia 

```POST: /Mail``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|emailMessage| |Kyllä|tekstissä|ei mitään|Sähköpostin|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="deleteemail"></a>DeleteEmail
Poista sähköposti: poistaa sähköpostiviestin tunnuksen mukaan 

```DELETE: /Mail/{messageId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|messageId|merkkijono|Kyllä|polku|ei mitään|Voit poistaa sähköpostin tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="markasread"></a>MarkAsRead
Merkitse luetuksi: merkitsee sähköpostiviestin luettu 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|messageId|merkkijono|Kyllä|polku|ei mitään|Lue sähköpostiviestissä merkitään tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="replyto"></a>ReplyTo
Vastaa sähköpostiin: sähköpostin vastaukset 

```POST: /Mail/ReplyTo/{messageId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|messageId|merkkijono|Kyllä|polku|ei mitään|Voit vastata sähköpostin tunnus|
|kommentti|merkkijono|Kyllä|kyselyn|ei mitään|Vastaa kommenttiin|
|replyAll|Totuusarvo|Ei|kyselyn|EPÄTOSI|Vastaa kaikille vastaanottajille|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="getattachment"></a>GetAttachment
Hae liitteen: noutaa sähköpostin liitteen tunnuksen mukaan 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|messageId|merkkijono|Kyllä|polku|ei mitään|Sähköpostin tunnus|
|attachmentId|merkkijono|Kyllä|polku|ei mitään|Lataa liite tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="onnewemail"></a>OnNewEmail
Valitse uusi sähköposti: tuottaa vuo, kun uusi sähköposti saapuu 

```GET: /Mail/OnNewEmail``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kansiopolku|merkkijono|Ei|kyselyn|Saapuneet-kansioon|Noutaa sähköpostikansion (oletus: Saapuneet-kansiossa)|
|Jos haluat|merkkijono|Ei|kyselyn|ei mitään|Vastaanottajien sähköpostiosoitteet|
|Valitse|merkkijono|Ei|kyselyn|ei mitään|Osoitteesta|
|merkitys|merkkijono|Ei|kyselyn|Normaali|Merkitys sähköposti (suuri, Normaali, pieni) (oletus: Normaali)|
|fetchOnlyWithAttachment|Totuusarvo|Ei|kyselyn|EPÄTOSI|Hae vain liitetiedoston sähköpostit|
|includeAttachments|Totuusarvo|Ei|kyselyn|EPÄTOSI|Sisällytä liitteet|
|subjectFilter|merkkijono|Ei|kyselyn|ei mitään|Tarkista, aiheessa merkkijono|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|202|Hyväksytty|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Lähetä sähköpostia-vaihtoehtojen avulla: Lähetä sähköpostiviesti, jossa on useita vaihtoehtoja ja odota, että vastaanottaja voi vastaa takaisin jokin vaihtoehdoista 

```POST: /mailwithoptions/$subscriptions``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Kyllä|tekstissä|ei mitään|Tilauksen pyynnön asetukset sähköpostia varten|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|201|Tilauksen, joka on luotu|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Hyväksynnän sähköpostin lähettäminen: Lähetä sähköpostiviesti hyväksyntä ja odota, kunnes vastaanottaja vastausta 

```POST: /approvalmail/$subscriptions``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Kyllä|tekstissä|ei mitään|Tilauksen pyynnön hyväksymisen sähköpostia varten|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|201|Tilauksen, joka on luotu|
|400|BadRequest|
|401|Ei valtuuksia|
|403|Käyttö estetty|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="calendargettables"></a>CalendarGetTables
Hae kalentereita: hakee kalenterit 

```GET: /datasets/calendars/tables``` 

Ei ole parametreja puhelun
#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendargetitems"></a>CalendarGetItems
Hae tapahtumat: hakee kalenterin kohteet 

```GET: /datasets/calendars/tables/{table}/items``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus kalenterin noutaminen|
|$filter|merkkijono|Ei|kyselyn|ei mitään|ODATA suodatinkyselyn, joka rajoittaa tapahtumien määrä|
|$orderby|merkkijono|Ei|kyselyn|ei mitään|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|kokonaisluku|Ei|kyselyn|ei mitään|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|kokonaisluku|Ei|kyselyn|ei mitään|Noutaa kohteiden enimmäismäärä (oletus = 256)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendarpostitem"></a>CalendarPostItem
Tapahtuman luominen: Luo uusi tapahtuma 

```POST: /datasets/calendars/tables/{table}/items``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|kohteen| |Kyllä|tekstissä|ei mitään|Kalenterikohteen luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendargetitem"></a>CalendarGetItem
Hae tapahtuman: hakee tietyn kohteen kalenterista 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus kalenterikohteen noutaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Poista tapahtuma: poistaa kalenterikohteen 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus kalenterikohteen poistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Päivitä tapahtuman: päivittää osittain kalenterikohteen 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus kalenterikohteen päivittäminen|
|kohteen| |Kyllä|tekstissä|ei mitään|Kalenterikohteen päivittäminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Valitse uudet kohteet: käynnistyy, kun uusi kalenterikohde luodaan 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|$filter|merkkijono|Ei|kyselyn|ei mitään|ODATA suodatinkyselyn, joka rajoittaa tapahtumien määrä|
|$orderby|merkkijono|Ei|kyselyn|ei mitään|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|kokonaisluku|Ei|kyselyn|ei mitään|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|kokonaisluku|Ei|kyselyn|ei mitään|Noutaa kohteiden enimmäismäärä (oletus = 256)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Kohteiden päivitetty: käynnistyy, kun kalenterikohteen on muokattu 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Kalenterin yksilöivä tunnus|
|$filter|merkkijono|Ei|kyselyn|ei mitään|ODATA suodatinkyselyn, joka rajoittaa tapahtumien määrä|
|$orderby|merkkijono|Ei|kyselyn|ei mitään|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|kokonaisluku|Ei|kyselyn|ei mitään|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|kokonaisluku|Ei|kyselyn|ei mitään|Noutaa kohteiden enimmäismäärä (oletus = 256)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactgettables"></a>ContactGetTables
Hae yhteystiedot-kansiot: noutaa yhteystietokansiot 

```GET: /datasets/contacts/tables``` 

Ei ole parametreja puhelun
#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactgetitems"></a>ContactGetItems
Hae yhteystiedot: hakee yhteystietoja Yhteystiedot-kansiosta 

```GET: /datasets/contacts/tables/{table}/items``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus hakemiseen Yhteystiedot-kansio|
|$filter|merkkijono|Ei|kyselyn|ei mitään|ODATA suodatinkyselyn, joka rajoittaa tapahtumien määrä|
|$orderby|merkkijono|Ei|kyselyn|ei mitään|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|kokonaisluku|Ei|kyselyn|ei mitään|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|kokonaisluku|Ei|kyselyn|ei mitään|Noutaa kohteiden enimmäismäärä (oletus = 256)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactpostitem"></a>ContactPostItem
Luo yhteyshenkilö: Luo uusi yhteyshenkilö 

```POST: /datasets/contacts/tables/{table}/items``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus Yhteystiedot-kansio|
|kohteen| |Kyllä|tekstissä|ei mitään|Yhteyshenkilön luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactgetitem"></a>ContactGetItem
Hae yhteyshenkilö: hakee tietyn yhteyshenkilön Yhteystiedot-kansiosta 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus Yhteystiedot-kansio|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Yhteyshenkilön hakemiseen yksilöllinen tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Poista yhteyshenkilö: poistaa yhteyshenkilön 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus Yhteystiedot-kansio|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Jos haluat poistaa yhteyshenkilön yksilöllinen tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="contactpatchitem"></a>ContactPatchItem
Päivitä yhteyshenkilö: päivittää osittain yhteystiedon 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|taulukko|merkkijono|Kyllä|polku|ei mitään|Yksilöllinen tunnus Yhteystiedot-kansio|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Päivitä yhteyshenkilön yksilöllinen tunnus|
|kohteen| |Kyllä|tekstissä|ei mitään|Jos haluat päivittää yhteystieto|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [merkkijono, objekti]]


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="object"></a>Objektin


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Liitteet|matriisi|Ei |
|Valitse|merkkijono|Ei |
|Kopio|merkkijono|Ei |
|Piilokopio|merkkijono|Ei |
|Aihe|merkkijono|Kyllä |
|Tekstissä|merkkijono|Kyllä |
|Merkitys|merkkijono|Ei |
|IsHtml|Totuusarvo|Ei |
|Jos haluat|merkkijono|Kyllä |



### <a name="sendattachment"></a>SendAttachment


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|@odata.type|merkkijono|Ei |
|Nimi|merkkijono|Kyllä |
|ContentBytes|merkkijono|Kyllä |



### <a name="receivemessage"></a>ReceiveMessage


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Tunnus|merkkijono|Ei |
|IsRead|Totuusarvo|Ei |
|Liite|Totuusarvo|Ei |
|DateTimeReceived|merkkijono|Ei |
|Liitteet|matriisi|Ei |
|Valitse|merkkijono|Ei |
|Kopio|merkkijono|Ei |
|Piilokopio|merkkijono|Ei |
|Aihe|merkkijono|Kyllä |
|Tekstissä|merkkijono|Kyllä |
|Merkitys|merkkijono|Ei |
|IsHtml|Totuusarvo|Ei |
|Jos haluat|merkkijono|Kyllä |



### <a name="receiveattachment"></a>ReceiveAttachment


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Tunnus|merkkijono|Kyllä |
|ContentType|merkkijono|Kyllä |
|@odata.type|merkkijono|Ei |
|Nimi|merkkijono|Kyllä |
|ContentBytes|merkkijono|Kyllä |



### <a name="digestmessage"></a>DigestMessage


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Aihe|merkkijono|Kyllä |
|Tekstissä|merkkijono|Ei |
|Merkitys|merkkijono|Ei |
|Käsittely|matriisi|Kyllä |
|Liitteet|matriisi|Ei |
|Jos haluat|merkkijono|Kyllä |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



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



### <a name="tablemetadata"></a>TableMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Ei |
|otsikko|merkkijono|Ei |
|x-ms-käyttöoikeudet|merkkijono|Ei |
|x-ms-ominaisuudet|ei ole määritetty|Ei |
|rakenne|ei ole määritetty|Ei |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|sortRestrictions|ei ole määritetty|Ei |
|filterRestrictions|ei ole määritetty|Ei |
|filterFunctions|matriisi|Ei |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|lajiteltava|Totuusarvo|Ei |
|unsortableProperties|matriisi|Ei |
|ascendingOnlyProperties|matriisi|Ei |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|suodatettavia|Totuusarvo|Ei |
|nonFilterableProperties|matriisi|Ei |
|RequiredProperties-arvo|matriisi|Ei |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|NotificationUrl|merkkijono|Ei |
|Viestin|ei ole määritetty|Ei |



### <a name="messagewithoptions"></a>MessageWithOptions


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Aihe|merkkijono|Kyllä |
|Asetukset|merkkijono|Kyllä |
|Tekstissä|merkkijono|Ei |
|Merkitys|merkkijono|Ei |
|Liitteet|matriisi|Ei |
|Jos haluat|merkkijono|Kyllä |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Ei |
|resurssi|merkkijono|Ei |
|notificationType|merkkijono|Ei |
|notificationUrl|merkkijono|Ei |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|NotificationUrl|merkkijono|Ei |
|Viestin|ei ole määritetty|Ei |



### <a name="approvalmessage"></a>ApprovalMessage


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Aihe|merkkijono|Kyllä |
|Asetukset|merkkijono|Kyllä |
|Tekstissä|merkkijono|Ei |
|Merkitys|merkkijono|Ei |
|Liitteet|matriisi|Ei |
|Jos haluat|merkkijono|Kyllä |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|SelectedOption|merkkijono|Ei |



### <a name="tableslist"></a>TablesList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="table"></a>Taulukko


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Ei |
|Näyttönimi|merkkijono|Ei |



### <a name="item"></a>Kohteen


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|ItemInternalId|merkkijono|Ei |



### <a name="calendaritemslist"></a>CalendarItemsList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="calendaritem"></a>CalendarItem


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|ItemInternalId|merkkijono|Ei |



### <a name="contactitemslist"></a>ContactItemsList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="contactitem"></a>ContactItem


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|ItemInternalId|merkkijono|Ei |



### <a name="datasetslist"></a>DataSetsList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="dataset"></a>Tietojoukko


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Ei |
|Näyttönimi|merkkijono|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)