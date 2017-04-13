<properties
    pageTitle="Facebook-yhdistin lisääminen logiikan sovelluksia | Microsoft Azure"
    description="Yleistä REST API parametreilla Facebook-yhdistin"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Facebook-yhdistin käytön aloittaminen
Yhteyden muodostaminen Facebookiin ja julkaiseminen aikajanan, syötteen sivun ja muita toimintoja. 

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.


Facebook-avulla voit:

- Muodostaa yhteyttä business työnkulku, saat Facebookista tietojen perusteella. 
- Käytä käynnistimen, kun uusi viesti vastaanotetaan.
- Käytä Toiminnot, jotka kirjaa aikajanassa, pyydä syötteen sivu ja lisää. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Esimerkiksi kun kyseessä on uusi viesti aikajanalla, voit ottaa kyseisen viestin ja push Twitter-syöte. 



Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
Facebook-yhdistin sisältää seuraavat käynnistin ja toiminnot. 

| Käynnistimien | Toiminnot|
| --- | --- |
| <ul><li>Kun kyseessä on uusi viesti Omat aikajanalla</li></ul> |<ul><li>Hae syötteen Omat aikajanalta</li><li>Oma aikajanan julkaiseminen</li><li>Kun kyseessä on uusi viesti Omat aikajanalla</li><li>Syötteen sivulla</li><li>Hae käyttäjän aikajana</li><li>Sivun julkaiseminen</li></ul>

Yhdistimien tukevat tietojen JSON ja XML-muodossa.

## <a name="create-a-connection-to-facebook"></a>Facebook-yhteyden luominen
Kun lisäät Tämä yhdistin logiikan sovelluksia, on sallittava logiikan muodostaa Facebook-sovellukset.

1. Facebook-tiliisi kirjautuminen
2. Valitse **Hyväksy**ja Salli Facebook-yhteyden ja logiikka sovelluksia. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Voit käyttää tätä samaa Facebook-yhteyttä logiikan muista sovelluksista.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-viittaus
Koskee versiota: 1.0.

### <a name="get-feed-from-my-timeline"></a>Hae syötteen Omat aikajanalta
Hakee syötteet kirjautuneena sisään käyttäjän aikajana.  
```GET: /me/feed```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kentät|merkkijono|Ei|kyselyn|ei mitään |Määritä palautettavan kentät. Esimerkki (tunnus, nimi ja kuva).|
|raja|kokonaisluku|Ei|kyselyn| ei mitään|Viestit, jotka noudetaan enimmäismäärä|
|kanssa|merkkijono|Ei|kyselyn| ei mitään|Viestiluettelon rajoittaminen vain ne liitetty sijaintia.|
|Suodatin|merkkijono|Ei|kyselyn| ei mitään|Noutaa vain ne viestit, jotka vastaavat tietyssä muodossa-suodatinta.|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


### <a name="post-to-my-timeline"></a>Oma aikajanan julkaiseminen
Kirjaa tilasanoma kirjautuneena sisään käyttäjän aikajanaan.  
```POST: /me/feed```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Kirjaa|merkkijono |Kyllä|tekstissä|ei mitään |Uusi viesti lähetetään|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Kun kyseessä on uusi viesti Omat aikajanalla
Käynnistää uusi työnkulku, kun kyseessä on uusi viesti kirjautuneena sisään käyttäjän aikajanalla.  
```GET: /trigger/me/feed```

Ei ole parametreja. 

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


### <a name="get-page-feed"></a>Syötteen sivulla
Pyydä viestit tietyn sivun syöte.  
```GET: /{pageId}/feed```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|pageId|merkkijono|Kyllä|polku| ei mitään|Sivu, josta viestit on noudetaan tunnus.|
|raja|kokonaisluku|Ei|kyselyn| ei mitään|Viestit, jotka noudetaan enimmäismäärä|
|include_hidden|Totuusarvo|Ei|kyselyn|ei mitään |Sisältävät viestit, jotka on piilotettu jollakin sivulla vai ei|
|kentät|merkkijono|Ei|kyselyn|ei mitään |Määritä palautettavan kentät. Esimerkki (tunnus, nimi ja kuva).|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


### <a name="get-user-timeline"></a>Hae käyttäjän aikajana
Viestien hakeminen käyttäjän aikajana.  
```GET: /{userId}/feed```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|Käyttäjätunnus|merkkijono|Kyllä|polku|ei mitään |Käyttäjätunnus, jonka aikajana on noudetaan.|
|raja|kokonaisluku|Ei|kyselyn|ei mitään |Viestit, jotka noudetaan enimmäismäärä|
|kanssa|merkkijono|Ei|kyselyn|ei mitään |Viestiluettelon rajoittaminen vain ne liitetty sijaintia.|
|Suodatin|merkkijono|Ei|kyselyn| ei mitään|Noutaa vain ne viestit, jotka vastaavat tietyssä muodossa-suodatinta.|
|kentät|merkkijono|Ei|kyselyn| ei mitään|Määritä palautettavan kentät. Esimerkki (tunnus, nimi ja kuva).|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


### <a name="post-to-page"></a>Sivun julkaiseminen
Lähettää viestin Facebook-sivustasi kirjautuneena sisään käyttäjänä.  
```POST: /{pageId}/feed```

| Nimi|Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|pageId|merkkijono|Kyllä|polku|ei mitään |Kirjaa sivun tunnus.|
|Kirjaa|monta |Kyllä|tekstissä|ei mitään |Uusi viesti lähetetään.|

#### <a name="response"></a>Vastaus
|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset

#### <a name="getfeedresponse"></a>GetFeedResponse

|Ominaisuuden nimi | Tietotyyppi | Pakollinen|
|---|---|---|
|tietoja|matriisi|Ei|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tietoja|matriisi|Ei|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Profiilin yksittäinen on syötteen
Profiilin voi olla käyttäjä, sivun, sovelluksen tai ryhmä. 

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|
|admin_creator|matriisi|Ei|
|kuvateksti|merkkijono|Ei|
|created_time|merkkijono|Ei|
|kuvaus|merkkijono|Ei|
|feed_targeting|ei ole määritetty|Ei|
|Valitse|ei ole määritetty|Ei|
|kuvake|merkkijono|Ei|
|is_hidden|Totuusarvo|Ei|
|is_published|Totuusarvo|Ei|
|linkki|merkkijono|Ei|
|viestin|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|object_id|merkkijono|Ei|
|Kuva|merkkijono|Ei|
|Aseta|ei ole määritetty|Ei|
|tietosuoja|ei ole määritetty|Ei|
|Ominaisuudet:|matriisi|Ei|
|lähde|merkkijono|Ei|
|status_type|merkkijono|Ei|
|Tarinan|merkkijono|Ei|
|kohdistaminen|ei ole määritetty|Ei|
|Jos haluat|matriisi|Ei|
|tyyppi|merkkijono|Ei|
|updated_time|merkkijono|Ei|
|with_tags|ei ole määritetty|Ei|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Profiilin yksittäinen on syötteen
Profiilin voi olla käyttäjä, sivun, sovelluksen tai ryhmä.

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|
|created_time|merkkijono|Ei|
|Valitse|ei ole määritetty|Ei|
|viestin|merkkijono|Ei|
|tyyppi|merkkijono|Ei|

#### <a name="adminitem"></a>AdminItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|
|linkki|merkkijono|Ei|

#### <a name="propertyitem"></a>PropertyItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Nimi|merkkijono|Ei|
|teksti|merkkijono|Ei|
|Hyperlinkkiviittaus|merkkijono|Ei|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|viestin|merkkijono|Kyllä|
|linkki|merkkijono|Ei|
|Kuva|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|kuvateksti|merkkijono|Ei|
|kuvaus|merkkijono|Ei|
|Aseta|merkkijono|Ei|
|tunnisteet|merkkijono|Ei|
|tietosuoja|ei ole määritetty|Ei|
|object_attachment|merkkijono|Ei|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|viestin|merkkijono|Kyllä|
|linkki|merkkijono|Ei|
|Kuva|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|kuvateksti|merkkijono|Ei|
|kuvaus|merkkijono|Ei|
|Toiminnot|matriisi|Ei|
|Aseta|merkkijono|Ei|
|tunnisteet|merkkijono|Ei|
|object_attachment|merkkijono|Ei|
|kohdistaminen|ei ole määritetty|Ei|
|feed_targeting|ei ole määritetty|Ei|
|julkaistu|Totuusarvo|Ei|
|scheduled_publish_time|merkkijono|Ei|
|backdated_time|merkkijono|Ei|
|backdated_time_granularity|merkkijono|Ei|
|child_attachments|matriisi|Ei|
|multi_share_end_card|Totuusarvo|Ei|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|

#### <a name="profilecollection"></a>ProfileCollection

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tietoja|matriisi|Ei|

#### <a name="useritem"></a>UserItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|
|Etunimi|merkkijono|Ei|
|Sukunimi|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|sukupuoli|merkkijono|Ei|
|tietoja|merkkijono|Ei|

#### <a name="actionitem"></a>ActionItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Nimi|merkkijono|Ei|
|linkki|merkkijono|Ei|

#### <a name="targetitem"></a>TargetItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Palauttaa niiden maiden|matriisi|Ei|
|kielet|matriisi|Ei|
|alueiden|matriisi|Ei|
|kaupunkien|matriisi|Ei|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekti, joka ohjaa uutisia syötteen kohdistus tämä viesti
Osallistuneet ryhmiin on todennäköisesti Katso tämä viesti, toiset ovat epätodennäköistä. Koskee ainoastaan sivuja.

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Palauttaa niiden maiden|matriisi|Ei|
|alueiden|matriisi|Ei|
|kaupunkien|matriisi|Ei|
|age_min|kokonaisluku|Ei|
|age_max|kokonaisluku|Ei|
|sukupuolen|matriisi|Ei|
|relationship_statuses|matriisi|Ei|
|interested_in|matriisi|Ei|
|college_years|matriisi|Ei|
|kiinnostuksen kohteet|matriisi|Ei|
|relevant_until|kokonaisluku|Ei|
|education_statuses|matriisi|Ei|
|kielet|matriisi|Ei|

#### <a name="placeitem"></a>PlaceItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|overall_rating|numero|Ei|
|sijainti|ei ole määritetty|Ei|

#### <a name="locationitem"></a>LocationItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Kaupunki|merkkijono|Ei|
|maa|merkkijono|Ei|
|leveyttä|numero|Ei|
|located_in|merkkijono|Ei|
|pituutta|numero|Ei|
|Nimi|merkkijono|Ei|
|alue|merkkijono|Ei|
|tila|merkkijono|Ei|
|Katuosoite|merkkijono|Ei|
|Postinumero|merkkijono|Ei|

#### <a name="privacyitem"></a>PrivacyItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|kuvaus|merkkijono|Ei|
|arvo|merkkijono|Kyllä|
|Salli|merkkijono|Ei|
|Estä|merkkijono|Ei|
|ystävät|merkkijono|Ei|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|linkki|merkkijono|Ei|
|Kuva|merkkijono|Ei|
|image_hash|merkkijono|Ei|
|Nimi|merkkijono|Ei|
|kuvaus|merkkijono|Ei|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|URL-osoite|merkkijono|Kyllä|
|kuvateksti|merkkijono|Ei|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Kyllä|
|post_id|merkkijono|Kyllä|

#### <a name="postvideorequest"></a>PostVideoRequest

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|videoData|merkkijono|Kyllä|
|kuvaus|merkkijono|Kyllä|
|otsikko|merkkijono|Kyllä|
|uploadedVideoName|merkkijono|Ei|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tietoja|ei ole määritetty|Kyllä|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|URL-osoite|merkkijono|Kyllä|
|is_silhouette|Totuusarvo|Kyllä|
|korkeus|merkkijono|Ei|
|leveys|merkkijono|Ei|

#### <a name="geteventresponse"></a>GetEventResponse

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tietoja|matriisi|Kyllä|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Ominaisuuden nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|tunnus|merkkijono|Kyllä|
|Nimi|merkkijono|Kyllä|
|start_time|merkkijono|Ei|
|end_time|merkkijono|Ei|
|aikavyöhyke|merkkijono|Ei|
|sijainti|merkkijono|Ei|
|kuvaus|merkkijono|Ei|
|ticket_uri|merkkijono|Ei|
|rsvp_status|merkkijono|Kyllä|


## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).
