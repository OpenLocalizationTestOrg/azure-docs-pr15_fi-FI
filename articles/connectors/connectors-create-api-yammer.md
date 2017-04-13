<properties
pageTitle="Lisää Yammer-yhdistin logiikan-sovelluksissa | Microsoft Azure"
description="Yleistä REST API parametreilla Yammer-yhdistin"
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

# <a name="get-started-with-the-yammer-connector"></a>Yammer-yhdistin käytön aloittaminen

Yhdistä Yammer access keskustelut yrityksen verkkoon.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.

Yammerin, avulla voit:

- Muodostaa yhteyttä business työnkulku, sinulla on Yammer tietojen perusteella. 
- Käytä käynnistää varten, kun kyseessä on uusi viesti ryhmän tai syötteen seuraaminen.
- Toimintojen avulla voit lähettää viestiä, kaikki viestit ja muita toimintoja. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Kun uusi viesti näkyy, voit lähettää sähköpostia Office 365: n avulla.

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot
Yammerin sisältää seuraavat Käynnistimet ja toiminnot. 

Käynnistin | Toiminnot
--- | ---
<ul><li>Kun kyseessä on uusi viesti-ryhmä</li><li>Kun kyseessä on oma seuraavat uuden viestin syötteen</li></ul>| <ul><li>Kaikkien viestien noutaminen</li><li>Viestien saa ryhmälle</li><li>Saa oman seuraavat viestit syötteen</li><li>Julkaise viesti</li><li>Kun kyseessä on uusi viesti-ryhmä</li><li>Kun kyseessä on oma seuraavat uuden viestin syötteen</li></ul>

Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

## <a name="create-a-connection-to-yammer"></a>Yhteyden muodostaminen Yammer
Yammer-yhdistin käyttämään voit ensin muodostaa **yhteyden** sitten lisätiedot näiden ominaisuuksien: 

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Anna Yammer-tunnistetiedoilla.|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="yammer-rest-api-reference"></a>Yammer-REST API-viittaus
Tämä dokumentaatio on tarkoitettu versio: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Saat kaikki julkisen kirjautuneena sisään käyttäjän Yammer-verkoston
Vastaa "Kaikki" keskustelut Yammer-verkko-käyttöliittymän.  
```GET: /messages.json```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|older_then|kokonaisluku|Ei|kyselyn|ei mitään|Palauttaa viestejä, jotka ovat vanhempia kuin määritetty numeerinen merkkijonona Viestitunnus. Tästä on hyötyä paginating viestejä. Jos tarkastelet parhaillaan 20 viestit ja vanhimmasta on luku esimerkiksi 2912, ettet voi liittää "? older_than = 2912″ kuin niihin 20 viestit, näet pyynnön.|
|newer_then|kokonaisluku|Ei|kyselyn|ei mitään|Palauttaa viestejä uudempi kuin määritetty numeerinen merkkijonona Viestitunnus. Tämä käytetään, kun uusia viestejä kyselyt. Jos haluat viestiä ja uusimman viestin palautetaan on 3516, voit tehdä pyyntö, jonka parametrin "? newer_than = 3516″ varmistaa, että et saa viestien kopioiden jo sivulla.|
|raja|kokonaisluku|Ei|kyselyn|ei mitään|Palauttaa vain viestien annettuun määrään.|
|sivun|kokonaisluku|Ei|kyselyn|ei mitään|Pyydä sivulle. Jos tiedot on suurempi kuin edellä, tämän kentän avulla voit käyttää seuraavien sivujen|


### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Okei|
|400|Pyyntö ei kelpaa|
|408|Pyynnön aikakatkaisu|
|429|Liian monta pyynnöt|
|500|Sisäinen palvelinvirhe. Tuntematon virhe|
|503|Yammer-palvelu ei ole käytettävissä|
|504|Yhdyskäytävän aikakatkaisu|
|Oletusarvo|Epäonnistui.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Lähettää viestin ryhmälle tai kaikki yrityksen syötteen
Jos Ryhmätunnus annetaan, viestin kirjataan määritetyn ryhmän muita se kirjataan kaikki yrityksen syötteen.    
```POST: /messages.json``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|syöte| |Kyllä|tekstissä|ei mitään|Kirjaa viestipyyntöä|


### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|201|Luotu|



## <a name="object-definitions"></a>Objektimääritykset

#### <a name="message-yammer-message"></a>Sanoma: Yammerin viesti

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|tunnus|kokonaisluku|Ei|
|content_excerpt|merkkijono|Ei|
|sender_id|kokonaisluku|Ei|
|replied_to_id|kokonaisluku|Ei|
|created_at|merkkijono|Ei|
|network_id|kokonaisluku|Ei|
|message_type|merkkijono|Ei|
|sender_type|merkkijono|Ei|
|URL-osoite|merkkijono|Ei|
|web_url|merkkijono|Ei|
|group_id|kokonaisluku|Ei|
|tekstissä|ei ole määritetty|Ei|
|thread_id|kokonaisluku|Ei|
|direct_message|Totuusarvo|Ei|
|client_type|merkkijono|Ei|
|client_url|merkkijono|Ei|
|kieli|merkkijono|Ei|
|notified_user_ids|matriisi|Ei|
|tietosuoja|merkkijono|Ei|
|liked_by|ei ole määritetty|Ei|
|system_message|Totuusarvo|Ei|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Vastaa viestiin pyyntö Yammer yhdistimen julkaisemista yammer

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|tekstissä|merkkijono|Kyllä|
|group_id|kokonaisluku|Ei|
|replied_to_id|kokonaisluku|Ei|
|direct_to_id|kokonaisluku|Ei|
|Lähetä|Totuusarvo|Ei|
|aihe1|merkkijono|Ei|
|aihe2|merkkijono|Ei|
|topic3|merkkijono|Ei|
|topic4|merkkijono|Ei|
|topic5|merkkijono|Ei|
|topic6|merkkijono|Ei|
|topic7|merkkijono|Ei|
|topic8|merkkijono|Ei|
|topic9|merkkijono|Ei|
|topic10|merkkijono|Ei|
|topic11|merkkijono|Ei|
|topic12|merkkijono|Ei|
|topic13|merkkijono|Ei|
|topic14|merkkijono|Ei|
|topic15|merkkijono|Ei|
|topic16|merkkijono|Ei|
|topic17|merkkijono|Ei|
|topic18|merkkijono|Ei|
|topic19|merkkijono|Ei|
|topic20|merkkijono|Ei|

#### <a name="messagelist-list-of-messages"></a>MessageList: Viestiluettelon

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|viestit|matriisi|Ei|


#### <a name="messagebody-message-body"></a>MessageBody: Viestin tekstissä

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|jäsentää|merkkijono|Ei|
|vain|merkkijono|Ei|
|RTF|merkkijono|Ei|

#### <a name="likedby-liked-by"></a>LikedBy: Tykännyt mukaan

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|Laske|kokonaisluku|Ei|
|nimet|matriisi|Ei|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Tykännyt mukaan

| Nimi | Tietotyyppi | Pakollinen |
|---|---| --- | 
|tyyppi|merkkijono|Ei|
|tunnus|kokonaisluku|Ei|
|full_name|merkkijono|Ei|


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
