<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Wunderlist on todo luettelon ja tehtävien hallinta voit helpommin helpottava niiden valmis.  Onko jaat Ostoslista loved luotuun projektiin tai suunnittelet loma-Wunderlist on helppo tallentaa, jakaa ja suorita oman to¬dos. Wunderlist Synkronoi välittömästi puhelin-, Tablet PC- ja tietokoneen välillä, jotta voit käyttää kaikkien tehtävien missä tahansa."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Wunderlist yhdistimen käytön aloittaminen

Wunderlist on todo luettelon ja tehtävien hallinta voit helpommin helpottava niiden valmis.  Onko jaat Ostoslista loved luotuun projektiin tai suunnittelet loma-Wunderlist on helppo tallentaa, jakaa ja suorita oman to¬dos. Wunderlist Synkronoi välittömästi puhelin-, Tablet PC- ja tietokoneen välillä, jotta voit käyttää kaikkien tehtävien missä tahansa.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Toiminnon; voidaan käyttää Wunderlist-yhdistin siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 Wunderlist yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="wunderlist-actions"></a>Wunderlist toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Noutaa tilisi luettelot.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Voit luoda luettelon.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Noutaa tehtävät tiettyyn luetteloon.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Tehtävän luominen|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Noutaa alitehtävät tiettyyn luetteloon tai tiettyyn tehtävään.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Tietyn tehtävän kuluessa alitehtävien luominen|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Muistiinpanojen tiettyyn luetteloon tai tietyn tehtävän hakeminen.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Lisää huomautus tiettyyn tehtävään|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Noutaa tiettyyn luetteloon tai tiettyyn tehtävään tehtävän huomautuksia.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Lisää kommentti tiettyyn tehtävään|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Muistutusten asettaminen tiettyyn luetteloon tai tietyn tehtävän hakeminen.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Muistutuksen asettaminen.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Palauttaa tiedostoja tiettyyn luetteloon tai tiettyyn tehtävään.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Hakee tiettyyn luetteloon|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Poistaa luettelon|
|[PäivitäLuettelo](connectors-create-api-wunderlist.md#updatelist)|Päivitä tiettyyn luetteloon|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Hakee tiettyyn tehtävään|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Päivittää tiettyyn tehtävään|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Poistaa tietyn tehtävän|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Hakee tietyn alitehtävien|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Päivittää tietyn alitehtävien|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Poistaa tietyn alitehtävien|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Erillisen huomautuksen hakeminen|
|[PäivitysHuomautus](connectors-create-api-wunderlist.md#updatenote)|Päivitä erillisen huomautuksen|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Erillisen huomautuksen poistaminen|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Kommentin tietyn tehtävän hakeminen|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Päivitä tietyn muistutus|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Tietyn muistutuksen poistaminen|
### <a name="wunderlist-triggers"></a>Wunderlist Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun tehtävä on määräpäivä|Käynnistää uusi työnkulku tehtäväluettelossa napsauttamalla määräaikana|
|Kun uusi tehtävä luodaan|Käynnistää uusi työnkulku, kun uusi tehtävä luodaan luettelossa|
|Muistutuksen ajankohdan|Käynnistää uusi työnkulku muistutuksen yhteydessä|


## <a name="create-a-connection-to-wunderlist"></a>Yhteyden muodostaminen Wunderlist
Luomiseen logiikan sovellusten Wunderlist, sinun on ensin **yhteyden** luominen sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Anna Wunderlist tunnistetiedot|
Kun olet luonut yhteyden, voit suorittaa toimintoja ja kuuntele käynnistimien, joka on kuvattu tämän artikkelin. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-wunderlist"></a>Wunderlist viittaus
Koskee versiota: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Kun tehtävä on asianmukaisesti: käynnistää uusi työnkulku tehtäväluettelossa napsauttamalla määräaikana 

```GET: /trigger/tasksdue``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|


## <a name="triggertasknew"></a>TriggerTaskNew
Kun uusi tehtävä luodaan: käynnistää uusi työnkulku, kun uusi tehtävä luodaan luettelossa 

```GET: /trigger/tasksnew``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|


## <a name="triggerreminder"></a>TriggerReminder
Muistutuksen ajankohdan: käynnistää uusi työnkulku muistutuksen yhteydessä 

```GET: /trigger/reminders``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|


## <a name="retrievelists"></a>RetrieveLists
Hae luettelot: noutaa tilisi luettelot. 

```GET: /lists``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createlist"></a>CreateList
Luettelon luominen: luettelon luominen. 

```POST: /lists``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Luodaan uusi luettelo|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|Oletusarvo|Epäonnistui.|


## <a name="listtasks"></a>ListTasks
Hae tehtävät: noutaa tehtävät tiettyyn luetteloon. 

```GET: /tasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|Valmis|Totuusarvo|Ei|kyselyn|ei mitään|Valmis|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createtask"></a>CreateTask
Tehtävän luominen: tehtävän luominen 

```POST: /tasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Uuden tehtävän luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|201|Luotu|


## <a name="listsubtasks"></a>ListSubTasks
Hae alitehtävät: noutaa alitehtävät tiettyyn luetteloon tai tiettyyn tehtävään. 

```GET: /subtasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|
|Valmis|Totuusarvo|Ei|kyselyn|ei mitään|Valmis|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createsubtask"></a>CreateSubTask
Luo alitehtävän: tietyn tehtävän kuluessa alitehtävien luominen 

```POST: /subtasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Uusi alitehtävien luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|201|Luotu|


## <a name="listnotes"></a>ListNotes
Hae muistiinpanoja: noutaa tiettyyn luetteloon tai tietyn tehtävän huomautukset. 

```GET: /notes``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createnote"></a>CreateNote
Luo muistilapun: huomautuksen lisääminen tiettyyn tehtävään 

```POST: /notes``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Luoda uuden muistiinpanon|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|201|Luotu|


## <a name="listcomments"></a>ListComments
Hae tehtävän kommentit: noutaa tiettyyn luetteloon tai tiettyyn tehtävään tehtävän huomautuksia. 

```GET: /task_comments``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createcomment"></a>CreateComment
Kommentin lisääminen tehtävään: Lisää kommentti tiettyyn tehtävään 

```POST: /task_comments``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Uuden tehtävän kommentti luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|201|Luotu|


## <a name="retrievereminders"></a>RetrieveReminders
Muistuttaminen: muistutuksia tiettyyn luetteloon tai tietyn tehtävän hakeminen. 

```GET: /reminders``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createreminder"></a>CreateReminder
Muistutuksen asettaminen: Määritä muistutus. 

```POST: /reminders``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Luoda uuden muistutuksen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|Oletusarvo|Epäonnistui.|


## <a name="retrievefiles"></a>RetrieveFiles
Siirtää tiedostojasi: palauttaa tiedostoja tiettyyn luetteloon tai tiettyyn tehtävään. 

```GET: /files``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|TASK_ID|kokonaisluku|Ei|kyselyn|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="getlist"></a>GetList
Hae luettelo: hakee tiettyyn luetteloon 

```GET: /lists/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Luettelotunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="deletelist"></a>DeleteList
Poista luettelo: poistaa luettelon 

```DELETE: /lists/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Luettelotunnus|
|tarkistaminen|kokonaisluku|Kyllä|kyselyn|ei mitään|Tarkistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|204|Sisältöä ei ole|


## <a name="updatelist"></a>PäivitäLuettelo
Päivitä luettelo: Päivitä tiettyyn luetteloon 

```PATCH: /lists/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Luettelotunnus|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Luettelotiedot|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="gettask"></a>GetTask
Hae tehtävän: hakee tiettyyn tehtävään 

```GET: /tasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Tehtävätunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="updatetask"></a>UpdateTask
Tehtävän päivittäminen: päivittää tiettyyn tehtävään 

```PATCH: /tasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Tehtävätunnus|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Tehtävän tiedot|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="deletetask"></a>DeleteTask
Poista tehtävä: poistaa tietyn tehtävän 

```DELETE: /tasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|kokonaisluku|Kyllä|kyselyn|ei mitään|Luettelotunnus|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Tehtävätunnus|
|tarkistaminen|kokonaisluku|Kyllä|kyselyn|ei mitään|Tarkistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|204|Sisältöä ei ole|


## <a name="getsubtask"></a>GetSubTask
Hae alitehtävien: hakee tietyn alitehtävien 

```GET: /subtasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Alitehtävien tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="updatesubtask"></a>UpdateSubTask
Päivitä alitehtävän: päivittää tietyn alitehtävien 

```PATCH: /subtasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Alitehtävien tunnus|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Alitehtävien tiedot|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="deletesubtask"></a>DeleteSubTask
Poista alitehtävän: poistaa tietyn alitehtävien 

```DELETE: /subtasks/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Alitehtävien tunnus|
|tarkistaminen|kokonaisluku|Kyllä|kyselyn|ei mitään|Tarkistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|204|Sisältöä ei ole|


## <a name="getnote"></a>GetNote
Hae Huomautus: noutaa erillisen huomautuksen 

```GET: /notes/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Huomautus tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="updatenote"></a>PäivitysHuomautus
Päivitä Huomautus: Päivitä erillisen huomautuksen 

```PATCH: /notes/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Huomautus tunnus|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Huomautus tiedot|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="deletenote"></a>DeleteNote
Viitteen poistaminen: erillisen huomautuksen poistaminen 

```DELETE: /notes/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Huomautus tunnus|
|tarkistaminen|kokonaisluku|Kyllä|kyselyn|ei mitään|Tarkistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|204|Sisältöä ei ole|


## <a name="getcomment"></a>GetComment
Hae tehtävän kommentti: kommentin tietyn tehtävän hakeminen 

```GET: /task_comments/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|merkkijono|Kyllä|polku|ei mitään|Kommentin tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="updatereminder"></a>UpdateReminder
Päivitä muistutus: Päivitä tietyn muistutus 

```PATCH: /reminders/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Muistutus-tunnus|
|Kirjaa| |Kyllä|tekstissä|ei mitään|Muistutus-tiedot|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|


## <a name="deletereminder"></a>DeleteReminder
Poista muistutus: tietyn muistutuksen poistaminen 

```DELETE: /reminders/{id}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|tunnus|kokonaisluku|Kyllä|polku|ei mitään|Muistutuksen tunnus.|
|tarkistaminen|kokonaisluku|Kyllä|kyselyn|ei mitään|Tarkistaminen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|204|Sisältöä ei ole|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="list"></a>Luettelo


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|otsikko|merkkijono|Ei |
|list_type|merkkijono|Ei |
|tyyppi|merkkijono|Ei |
|tarkistaminen|kokonaisluku|Ei |



### <a name="createdlist"></a>CreatedList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|otsikko|merkkijono|Ei |
|tarkistaminen|kokonaisluku|Ei |
|tyyppi|merkkijono|Ei |



### <a name="task"></a>Tehtävä


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|assignee_id|kokonaisluku|Ei |
|assigner_id|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|created_by_id|kokonaisluku|Ei |
|due_date|merkkijono|Ei |
|list_id|kokonaisluku|Ei |
|tarkistaminen|kokonaisluku|Ei |
|starred|Totuusarvo|Ei |
|otsikko|merkkijono|Ei |



### <a name="subtask"></a>Alitehtävien


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|TASK_ID|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|created_by_id|kokonaisluku|Ei |
|tarkistaminen|merkkijono|Ei |
|otsikko|merkkijono|Ei |



### <a name="note"></a>Huomautus


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|TASK_ID|kokonaisluku|Ei |
|sisältö|merkkijono|Ei |
|created_at|merkkijono|Ei |
|updated_at|merkkijono|Ei |
|tarkistaminen|kokonaisluku|Ei |



### <a name="comment"></a>Kommentti


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|TASK_ID|kokonaisluku|Ei |
|tarkistaminen|kokonaisluku|Ei |
|teksti|merkkijono|Ei |
|tyyppi|merkkijono|Ei |
|created_at|merkkijono|Ei |



### <a name="reminder"></a>Muistutus


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|päivämäärä|merkkijono|Ei |
|TASK_ID|kokonaisluku|Ei |
|tarkistaminen|kokonaisluku|Ei |
|tyyppi|merkkijono|Ei |
|created_at|merkkijono|Ei |
|updated_at|merkkijono|Ei |



### <a name="createdreminder"></a>CreatedReminder


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|päivämäärä|merkkijono|Ei |
|TASK_ID|kokonaisluku|Ei |
|tarkistaminen|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|updated_at|merkkijono|Ei |



### <a name="file"></a>Tiedoston


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|kokonaisluku|Ei |
|URL-osoite|merkkijono|Ei |
|TASK_ID|kokonaisluku|Ei |
|list_id|kokonaisluku|Ei |
|user_id|kokonaisluku|Ei |
|Tiedostonimi|merkkijono|Ei |
|CONTENT_TYPE|merkkijono|Ei |
|file_size|kokonaisluku|Ei |
|local_created_at|merkkijono|Ei |
|created_at|merkkijono|Ei |
|updated_at|merkkijono|Ei |
|tyyppi|merkkijono|Ei |
|tarkistaminen|kokonaisluku|Ei |



### <a name="newtask"></a>NewTask


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|kokonaisluku|Kyllä |
|otsikko|merkkijono|Kyllä |
|assignee_id|kokonaisluku|Ei |
|Valmis|Totuusarvo|Ei |
|recurrence_type|merkkijono|Ei |
|recurrence_count|kokonaisluku|Ei |
|due_date|merkkijono|Ei |
|starred|Totuusarvo|Ei |



### <a name="newlist"></a>NewList


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|otsikko|merkkijono|Kyllä |



### <a name="newsubtask"></a>NewSubtask


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|kokonaisluku|Kyllä |
|TASK_ID|kokonaisluku|Kyllä |
|otsikko|merkkijono|Kyllä |
|Valmis|Totuusarvo|Ei |



### <a name="newnote"></a>NewNote


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|kokonaisluku|Kyllä |
|TASK_ID|kokonaisluku|Kyllä |
|sisältö|merkkijono|Kyllä |



### <a name="newcomment"></a>Uusi_huomautus


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|kokonaisluku|Kyllä |
|TASK_ID|kokonaisluku|Kyllä |
|teksti|merkkijono|Kyllä |



### <a name="newreminder"></a>NewReminder


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|kokonaisluku|Kyllä |
|TASK_ID|kokonaisluku|Kyllä |
|päivämäärä|merkkijono|Kyllä |



### <a name="updatetask"></a>UpdateTask


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tarkistaminen|kokonaisluku|Ei |
|otsikko|merkkijono|Ei |
|assignee_id|kokonaisluku|Ei |
|Valmis|Totuusarvo|Ei |
|recurrence_type|merkkijono|Ei |
|recurrence_count|kokonaisluku|Ei |
|due_date|merkkijono|Ei |
|starred|Totuusarvo|Ei |



### <a name="updatelist"></a>PäivitäLuettelo


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tarkistaminen|kokonaisluku|Ei |
|otsikko|merkkijono|Ei |



### <a name="updatesubtask"></a>UpdateSubtask


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tarkistaminen|kokonaisluku|Ei |
|otsikko|merkkijono|Ei |
|Valmis|Totuusarvo|Ei |



### <a name="updatenote"></a>PäivitysHuomautus


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tarkistaminen|kokonaisluku|Ei |
|sisältö|merkkijono|Ei |



### <a name="updatereminder"></a>UpdateReminder


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|päivämäärä|merkkijono|Ei |
|tarkistaminen|kokonaisluku|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)