<properties
    pageTitle="Lisää Office 365: n käyttäjät yhdistimen logiikan sovelluksissa | Microsoft Azure"
    description="Office 365: n käyttäjät yhdistimen REST API parametreilla yleiskatsaus"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Office 365: n käyttäjät yhdistimen käytön aloittaminen

Yhdistä Office 365-käyttäjät voivat hakea profiilit ja etsi käyttäjät. 

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.

Office 365-käyttäjille voit tehdä seuraavia toimia:

- Muodostaa yhteyttä business työnkulku, sinulla on Office 365-käyttäjille tietojen perusteella. 
- Käytä Toiminnot, jotka saat alaiset, pyydä esimiehen käyttäjäprofiilin ja paljon muuta. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Esimerkiksi Hae henkilön alaiset, tämä tieto ja SQL Azure-tietokannan päivittäminen. 

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Office 365: n käyttäjät yhdistin on käytettävissä seuraavat toimet. Ei ole Käynnistimet.

| Käynnistimien | Toiminnot|
| --- | --- |
|Ei mitään | <ul><li>Hae hallinta</li><li>Hae Oma profiili</li><li>Hae alaiset</li><li>Hae käyttäjäprofiilin</li><li>Käyttäjien etsiminen</li></ul>|

Yhdistimien tukevat tietojen JSON ja XML-muodossa. 


## <a name="create-a-connection-to-office-365-users"></a>Yhteyden muodostaminen Office 365-käyttäjille

Kun lisäät Tämä yhdistin logiikan sovelluksia, on kirjautuminen Office 365: n käyttäjät-tilisi ja sallia logiikan sovellusten voit muodostaa yhteyden tiliisi.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Kun olet luonut yhteyden, kirjoita Office 365: n käyttäjät ominaisuuksia, kuten käyttäjätunnus. **REST API viittaus** tässä ohjeaiheessa kuvataan nämä ominaisuudet.

>[AZURE.TIP] Voit käyttää samaa Office 365: n käyttäjät yhteyden logiikan muista sovelluksista.


## <a name="office-365-users-rest-api-reference"></a>Office 365: n käyttäjät REST API-viittaus
Koskee versiota: 1.0.

### <a name="get-my-profile"></a>Hae Oma profiili 
Hakee nykyisen käyttäjän profiiliin.  
```GET: /users/me``` 

Ei ole parametreja puhelun.

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


### <a name="get-user-profile"></a>Hae käyttäjäprofiilin 
Hakee tiettyyn käyttäjäprofiiliin.  
```GET: /users/{userId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Käyttäjätunnus|merkkijono|Kyllä|polku|ei mitään|Lainan nimi tai käyttäjätunnus|

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


### <a name="get-manager"></a>Hae hallinta 
Hakee käyttäjäprofiilin määritetyn käyttäjän esimies.  
```GET: /users/{userId}/manager``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Käyttäjätunnus|merkkijono|Kyllä|polku|ei mitään|Lainan nimi tai käyttäjätunnus|

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



### <a name="get-direct-reports"></a>Hae alaiset 
Pyydä alaiset.  
```GET: /users/{userId}/directReports``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Käyttäjätunnus|merkkijono|Kyllä|polku|ei mitään|Lainan nimi tai käyttäjätunnus|

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



### <a name="search-for-users"></a>Käyttäjien etsiminen 
Hakee hakutulokset käyttäjäprofiilit.  
```GET: /users``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|searchTerm|merkkijono|Ei|kyselyn|ei mitään|Hakumerkkijono (koskee: Näyttää nimi, Etunimi, Sukunimi, sähköposti, sähköpostin lempinimien ja käyttäjän käyttäjätunnuksen)|

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



## <a name="object-definitions"></a>Objektimääritykset

#### <a name="user-user-model-class"></a>Käyttäjä: Käyttäjän mallin luokka

|Ominaisuuden nimi | Tietotyyppi |Pakollinen
|---|---|---|
|Näyttönimi|merkkijono|Ei|
|GivenName|merkkijono|Ei|
|Sukunimi|merkkijono|Ei|
|Sähköposti|merkkijono|Ei|
|MailNickname|merkkijono|Ei|
|TelephoneNumber|merkkijono|Ei|
|AccountEnabled|Totuusarvo|Ei|
|Tunnus|merkkijono|Kyllä
|UserPrincipalName|merkkijono|Ei|
|Osasto|merkkijono|Ei|
|Asema|merkkijono|Ei|
|mobilePhone|merkkijono|Ei|


## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

Siirry [API-luettelosta](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
