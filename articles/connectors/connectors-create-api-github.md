<properties
pageTitle="GitHub | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. GitHub on verkkopohjainen Git säilö isännöintipalvelu. Siinä on kaikki hajautettu version ohjausobjektin ja tietolähteen koodin Ohjelmistomääritysten hallinta ominaisuudet Git sekä lisäämällä omia ominaisuuksia."
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

# <a name="get-started-with-the-github-connector"></a>GitHub yhdistimen käytön aloittaminen

GitHub on verkkopohjainen Git säilö isännöintipalvelu. Siinä on kaikki hajautettu version ohjausobjektin ja tietolähteen koodin Ohjelmistomääritysten hallinta ominaisuudet Git sekä lisäämällä omia ominaisuuksia.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. 

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Toiminnon; voidaan käyttää GitHub-yhdistin siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 GitHub yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="github-actions"></a>GitHub toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Luo ongelma|
### <a name="github-triggers"></a>GitHub Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun ongelma on auki|Ongelma on auki|
|Kun ongelma on suljettu|Ongelma on suljettu|
|Kun ongelma on varattu|Ongelma on varattu|


## <a name="create-a-connection-to-github"></a>Yhteyden muodostaminen GitHub
Luomiseen logiikan sovellusten GitHub, sinun on ensin muodostaa **yhteyden** sitten lisätiedot seuraavien ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Anna GitHub tunnistetiedot|
Kun olet luonut yhteyden, voit suorittaa toimintoja ja kuuntele käynnistimien, joka on kuvattu tämän artikkelin. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-github"></a>GitHub viittaus
Koskee versiota: 1.0

## <a name="createissue"></a>CreateIssue
Luo ongelma: Luo ongelma 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|repositoryOwner|merkkijono|Kyllä|polku|ei mitään|Tietovaraston omistaja|
|repositoryName|merkkijono|Kyllä|polku|ei mitään|Säilön nimi|
|issueBasicDetails| |Kyllä|tekstissä|ei mitään|Seurantakohteen tiedot|

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


## <a name="issueopened"></a>IssueOpened
Kun ongelma avataan: ongelma on auki 

```GET: /trigger/issueOpened``` 

Ei ole parametreja puhelun
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


## <a name="issueclosed"></a>IssueClosed
Kun ongelma on suljettu: ongelma on suljettu 

```GET: /trigger/issueClosed``` 

Ei ole parametreja puhelun
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


## <a name="issueassigned"></a>IssueAssigned
Kun ongelma on määritetty: ongelma on varattu 

```GET: /trigger/issueAssigned``` 

Ei ole parametreja puhelun
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|otsikko|merkkijono|Kyllä |
|tekstissä|merkkijono|Kyllä |
|osallistuja|merkkijono|Kyllä |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|otsikko|merkkijono|Kyllä |
|tekstissä|merkkijono|Kyllä |
|osallistuja|merkkijono|Kyllä |
|numero|merkkijono|Ei |
|tila|merkkijono|Ei |
|created_at|merkkijono|Ei |
|repository_url|merkkijono|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)