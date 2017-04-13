<properties
pageTitle="Project online | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. Project Online on joustava online ratkaisu portfolion projektinhallinnan (PPM) ja jokapäiväisessä työssä Microsoftilta. Toimitettu Office 365: n kautta, Project Onlinen avulla organisaatiot voivat pääset nopeasti alkuun tehokkaita projektin hallintatoiminnot, priorisoida ja hallita projekteja ja projektiportfoliota – miltei missä tahansa lähes missä tahansa laitteessa."
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

# <a name="get-started-with-the-projectonline-connector"></a>Project online-yhdistin käytön aloittaminen

Project Online on joustava online ratkaisu portfolion projektinhallinnan (PPM) ja jokapäiväisessä työssä Microsoftilta. Toimitettu Office 365: n kautta, Project Onlinen avulla organisaatiot voivat pääset nopeasti alkuun tehokkaita projektin hallintatoiminnot, priorisoida ja hallita projekteja ja projektiportfoliota – miltei missä tahansa lähes missä tahansa laitteessa.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Project online-yhdistin voidaan käyttää toiminnon; siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 Project online-yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="projectonline-actions"></a>Project online-toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Project online-sivuston projektien luettelo|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Luo uuden projektin project online-sivuston|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Luo uuden tehtävän voit Projectissa|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Luo project online-sivuston yrityksen resurssit|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Näyttää projektin julkaistun tehtävät|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Kuittaa ulos project-sivustossa|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Kuittaa sisään ja Julkaise ja aiemmin luotu projekti-sivustossa|
### <a name="projectonline-triggers"></a>Project online-Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun uusi projekti luodaan|Käynnistää vuo aina, kun uusi projekti luodaan|
|Kun uusi resurssi luodaan|Käynnistää uusi työnkulku, kun luodaan uusi resurssi|
|Kun uusi tehtävä luodaan|Käynnistää vuo, kun uusi tehtävä luodaan|


## <a name="create-a-connection-to-projectonline"></a>Yhteyden muodostaminen Project online
Luodaksesi logiikan sovellusten Project online-sinun on ensin muodostaa **yhteyden** sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Project online-tunnistetietoja|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-projectonline"></a>Project online-viittaus
Koskee versiota: 1.0

## <a name="onnewproject"></a>OnNewProject
Kun uusi projekti on luotu: käynnistää vuo aina, kun uusi projekti luodaan 

```GET: /trigger/_api/ProjectData/Projects``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="onnewresource"></a>OnNewResource
Kun uusi resurssi luodaan: käynnistää uusi työnkulku, kun luodaan uusi resurssi 

```GET: /trigger/_api/ProjectData/Resources``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="onnewtask"></a>OnNewTask
Kun uusi tehtävä luodaan: tuottaa vuo, kun uusi tehtävä luodaan 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="listprojects"></a>ListProjects
Luettelo projektien: project online-sivuston projektien luettelo 

```GET: /_api/ProjectServer/Projects``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createproject"></a>CreateProject
Luo uusi projekti: Luo uuden projektin project online-sivuston 

```POST: /_api/ProjectServer/Projects``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Proj| |Kyllä|tekstissä|ei mitään|Uuden projektin luominen|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createtask"></a>CreateTask
Luo uusi tehtävä: Luo uuden tehtävän voit Projectissa 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|merkkijono|Kyllä|polku|ei mitään|Voit lisätä tehtävän projektin yksilöllinen tunnus|
|tehtävä| |Kyllä|tekstissä|ei mitään|Uuden tehtävän lisääminen projektiin|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="createresource"></a>CreateResource
Luo uusi resurssi: Luo project online-sivuston yrityksen resurssit 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|resurssi| |Kyllä|tekstissä|ei mitään|Uuden yrityksen resurssien lisääminen projektiin|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="listtasks"></a>ListTasks
Luettelo tehtävistä: Näyttää projektin julkaistun tehtävät 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|merkkijono|Kyllä|polku|ei mitään|Projektin yksilöivä tunnus hakeaksesi tehtävät|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="checkoutproject"></a>CheckoutProject
Uloskuittauksen projektin: Kuittaa ulos project-sivustossa 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|merkkijono|Kyllä|polku|ei mitään|Voit lisätä tehtävän projektin yksilöllinen tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="publishproject"></a>PublishProject
Sisäänkuittaus ja julkaista projektin: kuittaaminen sisään ja julkaista aiemmin luotu projekti-sivustossa 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|siteUrl|merkkijono|Kyllä|kyselyn|ei mitään|Pääkansio Projektisivuston sivuston URL-osoite (Esimerkki: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|merkkijono|Kyllä|polku|ei mitään|Sisäänkuittaus projektin yksilöivä tunnus|

#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|401|Luvattomia|
|403|Käyttö estetty|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset 

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="triggerproject"></a>TriggerProject


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Projektin alkamispäivä|merkkijono|Ei |
|Projektin valmistumispäivä|merkkijono|Ei |
|ProjectCreatedDate|merkkijono|Ei |
|Projektin tunnus|merkkijono|Ei |
|Projektin muokkauspäivämäärä|merkkijono|Ei |
|Projektin tyyppi|kokonaisluku|Ei |
|Projektin nimi|merkkijono|Ei |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="triggerresource"></a>TriggerResource


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|ResourceId|merkkijono|Ei |
|Resurssin peruskalenteri|merkkijono|Ei |
|Resurssin Varaustyyppi|kokonaisluku|Ei |
|Resurssi on tasattavissa|Totuusarvo|Ei |
|ResourceCostPerUse|numero|Ei |
|Resurssin luontipäivämäärä|merkkijono|Ei |
|Resurssi käytettävissä aikaisintaan alkaen|merkkijono|Ei |
|ResourceEmail|merkkijono|Ei |
|Resurssin alkukirjaimet|merkkijono|Ei |
|Resurssi on aktiivinen|Totuusarvo|Ei |
|Resurssi on yleinen|Totuusarvo|Ei |
|Resurssi käytettävissä enintään asti|merkkijono|Ei |
|Resurssin muokkauspäivämäärä|merkkijono|Ei |
|ResourceName|merkkijono|Ei |
|ResourceStatsuName|merkkijono|Ei |
|ResourceType|kokonaisluku|Ei |
|TypeDescription|merkkijono|Ei |
|TypeName|merkkijono|Ei |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="triggertask"></a>Laukaiseva tehtävä


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Projektin tunnus|merkkijono|Ei |
|TaskId|merkkijono|Ei |
|Projektin nimi|merkkijono|Ei |
|Tehtävän nimi|merkkijono|Ei |
|Tehtävän luomispäivämäärä|merkkijono|Ei |
|TaskModifieddate|merkkijono|Ei |
|Tehtävän alkamispäivä|merkkijono|Ei |
|Tehtävän valmistumispäivä|merkkijono|Ei |
|Tehtävän prioriteetti|kokonaisluku|Ei |
|Tehtävä on aktiivinen|Totuusarvo|Ei |



### <a name="newproject"></a>NewProject


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Kyllä |
|Kuvaus|merkkijono|Ei |
|Aloittaminen|merkkijono|Ei |



### <a name="newreource"></a>NewReource


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Kyllä |
|IsBudget|Totuusarvo|Ei |
|IsGeneric|Totuusarvo|Ei |
|IsInactive|Totuusarvo|Ei |



### <a name="project"></a>Projektin


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|ApprovedStart|merkkijono|Ei |
|ApprovedEnd|merkkijono|Ei |
|CheckedOutDate|merkkijono|Ei |
|CheckOutDescription|merkkijono|Ei |
|CheckOutId|merkkijono|Ei |
|CreatedDate|merkkijono|Ei |
|Tunnus|merkkijono|Ei |
|IsCheckedOut|Totuusarvo|Ei |
|LastPublishedDate|merkkijono|Ei |
|LastSavedDate|merkkijono|Ei |
|OptimizerDecision|kokonaisluku|Ei |
|PlannerDecision|kokonaisluku|Ei |
|Projektin tyyppi|kokonaisluku|Ei |
|Nimi|merkkijono|Ei |
|WinprojVersion|merkkijono|Ei |



### <a name="projectswrapper"></a>ProjectsWrapper


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="newtask"></a>NewTask


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Parametrit|ei ole määritetty|Kyllä |



### <a name="taskparameters"></a>TaskParameters


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Kyllä |
|Huomautuksia|merkkijono|Ei |
|Aloittaminen|merkkijono|Ei |
|Kesto|merkkijono|Ei |



### <a name="enterpriseresource"></a>EnterpriseResource


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|CanLevel|Totuusarvo|Ei |
|Koodi|merkkijono|Ei |
|CostAccrual|kokonaisluku|Ei |
|CostCenter|merkkijono|Ei |
|Luotu|merkkijono|Ei |
|DefaultBookingType|kokonaisluku|Ei |
|Sähköpostin|merkkijono|Ei |
|ExternalId|merkkijono|Ei |
|Ryhmän|merkkijono|Ei |
|HireDate|merkkijono|Ei |
|Tunnus|merkkijono|Ei |
|Nimikirjaimet|merkkijono|Ei |
|IsActive|Totuusarvo|Ei |
|IsBudget|Totuusarvo|Ei |
|IsCheckedOut|Totuusarvo|Ei |
|IsGeneric|Totuusarvo|Ei |
|IsTeam|Totuusarvo|Ei |
|MaterialLabel|merkkijono|Ei |
|Muokattu|merkkijono|Ei |
|Nimi|merkkijono|Ei |
|Fonetiikka|merkkijono|Ei |
|ResourceType|kokonaisluku|Ei |
|TerminationDate|merkkijono|Ei |



### <a name="taskswrapper"></a>TasksWrapper


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|arvo|matriisi|Ei |



### <a name="task"></a>Tehtävä


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Luotu|merkkijono|Ei |
|Muokattu|merkkijono|Ei |
|Aloittaminen|merkkijono|Ei |
|Valmis|merkkijono|Ei |
|Nimi|merkkijono|Ei |
|Tunnus|merkkijono|Ei |
|Priority (prioriteetti)|kokonaisluku|Ei |
|PercentComplete|kokonaisluku|Ei |
|Huomautuksia|merkkijono|Ei |
|Yhteyshenkilö|merkkijono|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)