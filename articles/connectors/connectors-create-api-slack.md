<properties
pageTitle=" Käytä liukuma yhdistimen logiikan-sovelluksissa | Microsoft Azure"
description="Käyttäminen: Microsoft Azure App palvelun logiikan sovelluksia liukuma-yhdistin"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-slack-connector"></a>Liukuman yhdistimen käytön aloittaminen

Liukuma on ryhmän viestintä työkalua, joka yhdistää asioiden kaikki ryhmän viestintää yhdessä sijoittaa, välittömästi etsittävän ja käytössä, minne.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.

Liukuman Connectorilla voit:

* Sen avulla logiikan sovellusten luominen

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Tietoja Käynnistimet ja toiminnot

Liukuman yhdistimen voidaan käyttää toiminnon; ei ole Käynnistimet. Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

 Liukuman yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="slack-actions"></a>Liukuman toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|PostMessagen|Viestin määritettyyn kanavaan.|
## <a name="create-a-connection-to-slack"></a>Yhteyden muodostaminen liukuma
Käyttämään liukuma yhdistimen olet ensin muodostaa **yhteyden** sitten lisätiedot näiden ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Liukuman tunnistetietoja|

Kirjaudu sisään liukuma ja Viimeistele määritys liukuma **yhteyden** logiikan-sovelluksessa seuraavasti:

1. Valitse **Toistuminen**
2. Valitse **korkojakso** ja kirjoita **väli**
3. Valitse **Lisää toiminnon**  
![Määritä liukuma][1]  
4. Kirjoita hakuruutuun liukuma ja odota, haku palauttaa kaikki tapahtumat, joissa liukuma nimi
5. Valitse **liukuma - kirjaa sanoman**
6. Valitse **Kirjaudu sisään aloituksista**:  
![Määritä liukuma][2]
7. Anna kirjautuminen sallia sovelluksen liukuma tunnistetiedot    
![Määritä liukuma][3]  
8. Sinut ohjataan organisaation Kirjaudu sisään-sivulla. **Määritä** Liukuman vuorovaikutuksessa logiikan sovelluksen:      
![Määritä liukuma][5] 
9. Luvan päätyttyä, sinut ohjataan logiikan sovelluksen suorittamiseen määrittämällä **aloituksista – Hae kaikki viestit** -osiossa. Lisää muita Käynnistimet ja toiminnot, jotka tarvitset.  
![Määritä liukuma][6]
10. Tallenna tekemäsi muutokset valitsemalla **Tallenna** yllä valikkorivillä.


>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="slack-rest-api-reference"></a>Liukuman REST API-viittaus
#### <a name="this-documentation-is-for-version-10"></a>Tämä dokumentaatio on tarkoitettu versio: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Viestin määritettyyn kanavaan.
**```POST: /chat.postMessage```** 



| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|kanavan|merkkijono|Kyllä|kyselyn|ei mitään|Kanavan, yksityisen ryhmän tai Pikaviesti kanavan Lähetä viesti. Nimi voi olla (ex: #general) tai koodattu.|
|teksti|merkkijono|Kyllä|kyselyn|ei mitään|Voit lähettää viestin tekstiosassa. Muotoilun asetukset-kohdassa https://api.slack.com/docs/formatting.|
|käyttäjänimi|merkkijono|Ei|kyselyn|ei mitään|Nimi robotti|
|as_user|Totuusarvo|Ei|kyselyn|ei mitään|Välittää Tosi voit kirjata todennetun käyttäjän viestin robotti sijaan|
|Jäsennä|merkkijono|Ei|kyselyn|ei mitään|Voit muuttaa, miten viestit käsitellään. Lisätietoja on artikkelissa https://api.slack.com/docs/formatting.|
|link_names|kokonaisluku|Ei|kyselyn|ei mitään|Etsi ja linkittää kanavan nimet ja käyttäjänimet.|
|unfurl_links|Totuusarvo|Ei|kyselyn|ei mitään|Välittää arvon TRUE arvoksi Ota unfurling ensisijaisesti tekstipohjainen sisältöä.|
|unfurl_media|Totuusarvo|Ei|kyselyn|ei mitään|Välittää käytöstä unfurling mediasisällön on EPÄTOSI.|
|icon_url|merkkijono|Ei|kyselyn|ei mitään|Käytettävä viestistä kuvakkeen kuvan URL|
|icon_emoji|merkkijono|Ei|kyselyn|ei mitään|Hymiöiden käytettävä viestistä kuvake|


### <a name="here-are-the-possible-responses"></a>Seuraavassa on mahdollista vastaukset:

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|408|Pyynnön aikakatkaisu|
|429|Liian monta pyynnöt|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|503|Liukuman palvelu ei ole käytettävissä|
|504|Yhdyskäytävän aikakatkaisu|
|Oletusarvo|Epäonnistui.|
------



## <a name="object-definitions"></a>Objektin definition(s): 

 **Sanoma**: Yammer-viesti

Viestin tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|tunnus|kokonaisluku|
|content_excerpt|merkkijono|
|sender_id|kokonaisluku|
|replied_to_id|kokonaisluku|
|created_at|merkkijono|
|network_id|kokonaisluku|
|message_type|merkkijono|
|sender_type|merkkijono|
|URL-osoite|merkkijono|
|web_url|merkkijono|
|group_id|kokonaisluku|
|tekstissä|ei ole määritetty|
|thread_id|kokonaisluku|
|direct_message|Totuusarvo|
|client_type|merkkijono|
|client_url|merkkijono|
|kieli|merkkijono|
|notified_user_ids|matriisi|
|tietosuoja|merkkijono|
|liked_by|ei ole määritetty|
|system_message|Totuusarvo|



 **PostOperationRequest**: vastaa viestiin pyyntö Yammer yhdistimen kirjaa yammer

PostOperationRequest tarvittavat ominaisuudet:

tekstissä

**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|tekstissä|merkkijono|
|group_id|kokonaisluku|
|replied_to_id|kokonaisluku|
|direct_to_id|kokonaisluku|
|Lähetä|Totuusarvo|
|aihe1|merkkijono|
|aihe2|merkkijono|
|topic3|merkkijono|
|topic4|merkkijono|
|topic5|merkkijono|
|topic6|merkkijono|
|topic7|merkkijono|
|topic8|merkkijono|
|topic9|merkkijono|
|topic10|merkkijono|
|topic11|merkkijono|
|topic12|merkkijono|
|topic13|merkkijono|
|topic14|merkkijono|
|topic15|merkkijono|
|topic16|merkkijono|
|topic17|merkkijono|
|topic18|merkkijono|
|topic19|merkkijono|
|topic20|merkkijono|



 **MessageList**: viestiluetteloa

MessageList tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|viestit|matriisi|



 **MessageBody**: viestin tekstiosa

MessageBody tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|jäsentää|merkkijono|
|vain|merkkijono|
|RTF|merkkijono|



 **LikedBy**: tykännyt mukaan

LikedBy tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Laske|kokonaisluku|
|nimet|matriisi|



 **YammmerEntity**: tykännyt mukaan

YammmerEntity tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|tyyppi|merkkijono|
|tunnus|kokonaisluku|
|full_name|merkkijono|


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Objektin definition(s): 

 **WebResultModel**: Bing web etsinnän tulokset

WebResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Otsikko|merkkijono|
|Kuvaus|merkkijono|
|DisplayUrl|merkkijono|
|Tunnus|merkkijono|
|FullUrl|merkkijono|



 **VideoResultModel**: Bing kuvan etsinnän tulokset

VideoResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Otsikko|merkkijono|
|DisplayUrl|merkkijono|
|Tunnus|merkkijono|
|MediaUrl|merkkijono|
|Runtime|kokonaisluku|
|Pikkukuva|ei ole määritetty|



 **ThumbnailModel**: multimedia elementin ominaisuuksia pikkukuva

ThumbnailModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|MediaUrl|merkkijono|
|ContentType|merkkijono|
|Leveys|kokonaisluku|
|Korkeus|kokonaisluku|
|FileSize|kokonaisluku|



 **ImageResultModel**: Bing kuvan etsinnän tulokset

ImageResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Otsikko|merkkijono|
|DisplayUrl|merkkijono|
|Tunnus|merkkijono|
|MediaUrl|merkkijono|
|SourceUrl|merkkijono|
|Pikkukuva|ei ole määritetty|



 **NewsResultModel**: Bing uutisia etsinnän tulokset

NewsResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Otsikko|merkkijono|
|Kuvaus|merkkijono|
|DisplayUrl|merkkijono|
|Tunnus|merkkijono|
|Lähde|merkkijono|
|Päivämäärä|merkkijono|



 **SpellResultModel**: Bing oikeinkirjoituksen ehdotuksia tulokset

SpellResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Tunnus|merkkijono|
|Arvo|merkkijono|



 **RelatedSearchResultModel**: Bing liittyvät hakutulokset

RelatedSearchResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Otsikko|merkkijono|
|Tunnus|merkkijono|
|BingUrl|merkkijono|



 **CompositeSearchResultModel**: Bing koosteen hakutulokset

CompositeSearchResultModel tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|WebResultsTotal|kokonaisluku|
|ImageResultsTotal|kokonaisluku|
|VideoResultsTotal|kokonaisluku|
|NewsResultsTotal|kokonaisluku|
|SpellSuggestionsTotal|kokonaisluku|
|WebResults|matriisi|
|ImageResults|matriisi|
|VideoResults|matriisi|
|NewsResults|matriisi|
|SpellSuggestionResults|matriisi|
|RelatedSearchResults|matriisi|


## <a name="object-definitions"></a>Objektin definition(s): 

 **PostOperationResponse**: edustaa liukuma yhdistimen toiminnon viestin vastauksen liukuma kirjaaminen

PostOperationResponse tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|Okei|Totuusarvo|
|kanavan|merkkijono|
|ts|merkkijono|
|viestin|ei ole määritetty|
|Virhe|merkkijono|



 **MessageItem**: kanavan viestin.

MessageItem tarvittavat ominaisuudet:


Kaikki ominaisuudet ovat pakollinen. 


**Kaikki ominaisuudet**: 


| Nimi | Tietotyyppi |
|---|---|
|teksti|merkkijono|
|tunnus|merkkijono|
|käyttäjän|merkkijono|
|luotu|kokonaisluku|
|is_user poistettu|Totuusarvo|


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
