<properties
pageTitle="MailChimp | Microsoft Azure"
description="Luo logiikan sovelluksia Azure-sovelluksen-palvelun kanssa. MailChimp on SaaS-palvelu, jonka avulla yritysten hallintaa ja automatisoida sähköpostin markkinoinnin aktiviteetteja, kuten lähettää markkinoinnin sähköpostiviestit, automaattinen viestit ja kohdennettujen kampanjat."
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

# <a name="get-started-with-the-mailchimp-connector"></a>MailChimp yhdistimen käytön aloittaminen

MailChimp on SaaS-palvelu, jonka avulla yritysten hallintaa ja automatisoida sähköpostin markkinoinnin aktiviteetteja, kuten lähettää markkinoinnin sähköpostiviestit, automaattinen viestit ja kohdennettujen kampanjat.


>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio.

Voit aloittaa luomalla nyt logiikan-sovellus on artikkelissa [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Toiminnon; voidaan käyttää MailChimp-yhdistin siinä on trigger(s). Yhdistimien tukevat tietojen JSON ja XML-muodossa.

 MailChimp yhdistin on seuraavat toimet ja/tai trigger(s) käytettävissä:

### <a name="mailchimp-actions"></a>MailChimp toiminnot
Voit tehdä seuraavat toimet:

|Toiminto|Kuvaus|
|--- | ---|
|[newcampaign](connectors-create-api-mailchimp.md#newcampaign)|Luo uusi markkinointikampanja, markkinointikampanjan lajin vastaanottajien luettelo ja markkinointikampanjan asetukset (aiherivi ja otsikko, from_name ja reply_to)|
|[newlist](connectors-create-api-mailchimp.md#newlist)|Luo uusi luettelo MailChimp tilisi|
|[fwlink](connectors-create-api-mailchimp.md#addmember)|Lisää tai Päivitä luettelo jäsen|
|[removemember](connectors-create-api-mailchimp.md#removemember)|Jäsenen poistaminen luettelosta.|
|[updatemember](connectors-create-api-mailchimp.md#updatemember)|Tiettyyn luetteloon jäsenen tietojen päivittäminen|
### <a name="mailchimp-triggers"></a>MailChimp Käynnistimet
Voit kuunnella nämä tapahtuma(t) varten:

|Käynnistin | Kuvaus|
|--- | ---|
|Kun luettelo on lisätty jäsen|Käynnistää työnkulun, kun luettelo on lisätty uusi jäsen|
|Kun luodaan uusi luettelo|Käynnistää työnkulun, kun luodaan uusi luettelo|


## <a name="create-a-connection-to-mailchimp"></a>Yhteyden muodostaminen MailChimp
Luomiseen logiikan sovellusten MailChimp, sinun on ensin **yhteyden** luominen sitten lisätiedot seuraavien ominaisuuksien:

|Ominaisuus| Pakollinen|Kuvaus|
| ---|---|---|
|Suojaustunnuksen|Kyllä|Anna MailChimp tunnistetiedot|

>[AZURE.INCLUDE [Steps to create a connection to MailChimp](../../includes/connectors-create-api-mailchimp.md)]

>[AZURE.TIP] Voit käyttää tätä yhteyttä logiikan muista sovelluksista.

## <a name="reference-for-mailchimp"></a>MailChimp viittaus
Koskee versiota: 1.0

## <a name="newcampaign"></a>newcampaign
Uuden markkinointikampanjan: Luo uusi markkinointikampanja, markkinointikampanjan lajin vastaanottajien luettelo ja markkinointikampanjan asetukset (aiherivi ja otsikko, from_name ja reply_to)

```POST: /campaigns```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|newCampaignRequest| |Kyllä|tekstissä|ei mitään|JSON objektin tekstissä uuden markkinointikampanjan pyynnön parametreilla lähettäminen|

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


## <a name="newlist"></a>newlist
Uusi luettelo: Luo uusi luettelo MailChimp tilisi

```POST: /lists```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|newListRequest| |Kyllä|tekstissä|ei mitään|JSON objektin tekstissä uuden markkinointikampanjan pyynnön parametreilla lähettäminen|

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


## <a name="addmember"></a>fwlink
Jäsenen lisääminen luetteloon: Lisää tai Päivitä luettelo jäsen

```POST: /lists/{list_id}/members```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|merkkijono|Kyllä|polku|ei mitään|Luettelon yksilöllinen tunnus|
|newMemberInList| |Kyllä|tekstissä|ei mitään|JSON objektin tekstissä jäsenen tiedot lähetetään|

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


## <a name="removemember"></a>removemember
Jäsenen poistaminen luettelosta: jäsenen poistaminen luettelosta.

```DELETE: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|merkkijono|Kyllä|polku|ei mitään|Luettelon yksilöllinen tunnus|
|member_email|merkkijono|Kyllä|polku|ei mitään|Jos haluat poistaa jäsenen sähköpostiosoite|

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


## <a name="updatemember"></a>updatemember
Jäsenen tietojen päivittäminen: tiettyyn luetteloon jäsenen tietojen päivittäminen

```PATCH: /lists/replacemailwithhash/{list_id}/members/{member_email}```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|merkkijono|Kyllä|polku|ei mitään|Luettelon yksilöllinen tunnus|
|member_email|merkkijono|Kyllä|polku|ei mitään|Päivitä jäsenen yksilöllinen sähköpostiosoite|
|updateMemberInListRequest| |Kyllä|tekstissä|ei mitään|JSON objektin Lähetä päivitetyt jäsenen tiedot tekstissä|

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


## <a name="onmembersubscribed"></a>OnMemberSubscribed
Kun luettelo on lisätty jäsen: käynnistää työnkulun, kun luettelo on lisätty uusi jäsen

```GET: /trigger/lists/{list_id}/members```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|list_id|merkkijono|Kyllä|polku|ei mitään|Luettelon yksilöllinen tunnus|

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


## <a name="oncreatelist"></a>OnCreateList
Kun luodaan uusi luettelo: käynnistää työnkulun, kun luodaan uusi luettelo

```GET: /trigger/lists```

Ei ole parametreja puhelun
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

### <a name="newcampaignrequest"></a>NewCampaignRequest


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tyyppi|merkkijono|Kyllä |
|vastaanottajat|ei ole määritetty|Kyllä |
|asetukset|ei ole määritetty|Kyllä |
|variate_settings|ei ole määritetty|Ei |
|seuranta|ei ole määritetty|Ei |
|rss_opts|ei ole määritetty|Ei |
|social_card|ei ole määritetty|Ei |



### <a name="recipient"></a>Vastaanottaja


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|list_id|merkkijono|Kyllä |
|segment_opts|ei ole määritetty|Ei |



### <a name="settings"></a>Asetukset


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|subject_line|merkkijono|Kyllä |
|otsikko|merkkijono|Ei |
|from_name|merkkijono|Kyllä |
|reply_to|merkkijono|Kyllä |
|use_conversation|Totuusarvo|Ei |
|to_name|merkkijono|Ei |
|folder_id|kokonaisluku|Ei |
|todennus|Totuusarvo|Ei |
|auto_footer|Totuusarvo|Ei |
|inline_css|Totuusarvo|Ei |
|auto_tweet|Totuusarvo|Ei |
|auto_fb_post|matriisi|Ei |
|fb_comments|Totuusarvo|Ei |



### <a name="variatesettings"></a>Variate_Settings


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|winner_criteria|merkkijono|Ei |
|Odotusaika|kokonaisluku|Ei |
|test_size|kokonaisluku|Ei |
|subject_lines|matriisi|Ei |
|send_times|matriisi|Ei |
|from_names|matriisi|Ei |
|reply_to_addresses|matriisi|Ei |



### <a name="tracking"></a>Seuranta


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Avaa|Totuusarvo|Ei |
|html_clicks|Totuusarvo|Ei |
|text_clicks|Totuusarvo|Ei |
|goal_tracking|Totuusarvo|Ei |
|ecomm360|Totuusarvo|Ei |
|google_analytics|merkkijono|Ei |
|clicktale|merkkijono|Ei |
|Salesforce|ei ole määritetty|Ei |
|highrise|ei ole määritetty|Ei |
|Kapseli|ei ole määritetty|Ei |



### <a name="rssopts"></a>RSS_Opts


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|feed_url|merkkijono|Ei |
|taajuus|merkkijono|Ei |
|constrain_rss_img|merkkijono|Ei |
|aikataulu|ei ole määritetty|Ei |



### <a name="socialcard"></a>Social_Card


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|image_url|merkkijono|Ei |
|kuvaus|merkkijono|Ei |
|otsikko|merkkijono|Ei |



### <a name="segmentopts"></a>Segment_Opts


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|saved_segment_id|kokonaisluku|Ei |
|vastaa|merkkijono|Ei |



### <a name="salesforce"></a>Salesforce


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|markkinointikampanja|Totuusarvo|Ei |
|Huomautuksia|Totuusarvo|Ei |



### <a name="highrise"></a>Highrise


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|markkinointikampanja|Totuusarvo|Ei |
|Huomautuksia|Totuusarvo|Ei |



### <a name="capsule"></a>Kapseli


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Huomautuksia|Totuusarvo|Ei |



### <a name="schedule"></a>Aikataulu


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Tunti|kokonaisluku|Ei |
|daily_send|ei ole määritetty|Ei |
|weekly_send_day|merkkijono|Ei |
|monthly_send_date|numero|Ei |



### <a name="dailysend"></a>Daily_Send


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|sunnuntai|Totuusarvo|Ei |
|maanantai|Totuusarvo|Ei |
|Tiistai|Totuusarvo|Ei |
|Keskiviikko|Totuusarvo|Ei |
|Torstai|Totuusarvo|Ei |
|Perjantai|Totuusarvo|Ei |
|lauantai|Totuusarvo|Ei |



### <a name="campaignresponsemodel"></a>CampaignResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Ei |
|tyyppi|merkkijono|Ei |
|create_time|merkkijono|Ei |
|archive_url|merkkijono|Ei |
|tila|merkkijono|Ei |
|emails_sent|kokonaisluku|Ei |
|send_time|merkkijono|Ei |
|CONTENT_TYPE|merkkijono|Ei |
|Vastaanottaja|matriisi|Ei |
|asetukset|ei ole määritetty|Ei |
|variate_settings|ei ole määritetty|Ei |
|seuranta|ei ole määritetty|Ei |
|rss_opts|ei ole määritetty|Ei |
|ab_split_opts|ei ole määritetty|Ei |
|social_card|ei ole määritetty|Ei |
|report_summary|ei ole määritetty|Ei |
|delivery_status|ei ole määritetty|Ei |
|_links|matriisi|Ei |



### <a name="absplitopts"></a>AB_Split_Opts


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|split_test|merkkijono|Ei |
|pick_winner|merkkijono|Ei |
|wait_units|merkkijono|Ei |
|Odotusaika|kokonaisluku|Ei |
|split_size|kokonaisluku|Ei |
|from_name_a|merkkijono|Ei |
|from_name_b|merkkijono|Ei |
|reply_email_a|merkkijono|Ei |
|reply_email_b|merkkijono|Ei |
|subject_a|merkkijono|Ei |
|subject_b|merkkijono|Ei |
|send_time_a|merkkijono|Ei |
|send_time_b|merkkijono|Ei |
|send_time_winner|merkkijono|Ei |



### <a name="reportsummary"></a>Report_Summary


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Avaa|kokonaisluku|Ei |
|unique_opens|kokonaisluku|Ei |
|open_rate|numero|Ei |
|Valitsee|kokonaisluku|Ei |
|subscriber_clicks|numero|Ei |
|click_rate|numero|Ei |



### <a name="deliverystatus"></a>Delivery_Status


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|käytössä|Totuusarvo|Ei |
|can_cancel|Totuusarvo|Ei |
|tila|merkkijono|Ei |
|emails_sent|kokonaisluku|Ei |
|emails_canceled|kokonaisluku|Ei |



### <a name="link"></a>Linkki


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|rel|merkkijono|Ei |
|Hyperlinkkiviittaus|merkkijono|Ei |
|menetelmä|merkkijono|Ei |
|targetSchema|merkkijono|Ei |
|rakenne|merkkijono|Ei |



### <a name="newlistrequest"></a>NewListRequest


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Nimi|merkkijono|Kyllä |
|Ota yhteyttä|ei ole määritetty|Kyllä |
|permission_reminder|merkkijono|Kyllä |
|use_archive_bar|Totuusarvo|Ei |
|campaign_defaults|ei ole määritetty|Kyllä |
|notify_on_subscribe|merkkijono|Ei |
|notify_on_unsubscribe|merkkijono|Ei |
|email_type_option|Totuusarvo|Kyllä |
|näkyvyys|merkkijono|Ei |



### <a name="contact"></a>Yhteyshenkilö


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|yrityksen|merkkijono|Kyllä |
|Osoite1|merkkijono|Kyllä |
|Osoite2|merkkijono|Ei |
|Kaupunki|merkkijono|Kyllä |
|tila|merkkijono|Kyllä |
|Postinumero|merkkijono|Kyllä |
|maa|merkkijono|Kyllä |
|puhelinnumero|merkkijono|Kyllä |



### <a name="campaigndefaults"></a>Campaign_Defaults


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|from_name|merkkijono|Kyllä |
|from_email|merkkijono|Kyllä |
|aihe|merkkijono|Ei |
|kieli|merkkijono|Kyllä |



### <a name="createnewlistresponsemodel"></a>CreateNewListResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Kyllä |
|Nimi|merkkijono|Kyllä |
|Ota yhteyttä|ei ole määritetty|Kyllä |
|permission_reminder|merkkijono|Kyllä |
|use_archive_bar|Totuusarvo|Ei |
|campaign_defaults|ei ole määritetty|Kyllä |
|notify_on_subscribe|merkkijono|Ei |
|notify_on_unsubscribe|merkkijono|Ei |
|date_created|merkkijono|Ei |
|list_rating|kokonaisluku|Ei |
|email_type_option|Totuusarvo|Kyllä |
|subscribe_url_short|merkkijono|Ei |
|subscribe_url_long|merkkijono|Ei |
|beamer_address|merkkijono|Ei |
|näkyvyys|merkkijono|Ei |
|Moduulit|matriisi|Ei |
|Tilasto|ei ole määritetty|Ei |
|_links|matriisi|Ei |



### <a name="stats"></a>Tilasto


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|member_count|kokonaisluku|Ei |
|unsubscribe_count|kokonaisluku|Ei |
|cleaned_count|kokonaisluku|Ei |
|member_count_since_send|kokonaisluku|Ei |
|unsubscribe_count_since_send|kokonaisluku|Ei |
|cleaned_count_since_send|kokonaisluku|Ei |
|campaign_count|kokonaisluku|Ei |
|campaign_last_sent|kokonaisluku|Ei |
|merge_field_count|kokonaisluku|Ei |
|avg_sub_rate|numero|Ei |
|avg_unsub_rate|numero|Ei |
|target_sub_rate|numero|Ei |
|open_rate|numero|Ei |
|click_rate|numero|Ei |
|last_sub_date|merkkijono|Ei |
|last_unsub_date|merkkijono|Ei |



### <a name="getlistsresponsemodel"></a>GetListsResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|luettelot|matriisi|Ei |
|total_items|kokonaisluku|Ei |



### <a name="newmemberinlistrequest"></a>NewMemberInListRequest


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|email_type|merkkijono|Ei |
|tila|merkkijono|Kyllä |
|merge_fields|ei ole määritetty|Ei |
|kiinnostuksen kohteet|merkkijono|Ei |
|kieli|merkkijono|Ei |
|VIP|Totuusarvo|Ei |
|sijainti|ei ole määritetty|Ei |
|Email_Address|merkkijono|Kyllä |



### <a name="firstandlastname"></a>FirstAndLastName


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|FNAME|merkkijono|Ei |
|LNAME|merkkijono|Ei |



### <a name="location"></a>Sijainti


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|leveyttä|numero|Ei |
|pituutta|numero|Ei |



### <a name="memberresponsemodel"></a>MemberResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Ei |
|Email_Address|merkkijono|Ei |
|unique_email_id|merkkijono|Ei |
|email_type|merkkijono|Ei |
|tila|merkkijono|Ei |
|merge_fields|ei ole määritetty|Ei |
|kiinnostuksen kohteet|merkkijono|Ei |
|Tilasto|ei ole määritetty|Ei |
|ip_signup|merkkijono|Ei |
|timestamp_signup|merkkijono|Ei |
|ip_opt|merkkijono|Ei |
|timestamp_opt|merkkijono|Ei |
|member_rating|kokonaisluku|Ei |
|last_changed|merkkijono|Ei |
|kieli|merkkijono|Ei |
|VIP|Totuusarvo|Ei |
|email_client|merkkijono|Ei |
|sijainti|ei ole määritetty|Ei |
|last_note|ei ole määritetty|Ei |
|list_id|merkkijono|Ei |
|_links|matriisi|Ei |



### <a name="lastnote"></a>Last_Note


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|note_id|kokonaisluku|Ei |
|created_at|merkkijono|Ei |
|created_by|merkkijono|Ei |
|Huomautus|merkkijono|Ei |



### <a name="getallmembersresponsemodel"></a>GetAllMembersResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|jäsenet|matriisi|Ei |
|list_id|merkkijono|Ei |
|total_items|kokonaisluku|Ei |



### <a name="object"></a>Objektin






### <a name="updatememberinlistrequest"></a>UpdateMemberInListRequest


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|Email_Address|merkkijono|Ei |
|email_type|merkkijono|Ei |
|tila|merkkijono|Kyllä |
|merge_fields|ei ole määritetty|Ei |
|kiinnostuksen kohteet|merkkijono|Ei |
|kieli|merkkijono|Ei |
|VIP|Totuusarvo|Ei |
|sijainti|ei ole määritetty|Ei |



### <a name="getmembersresponsemodel"></a>GetMembersResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|jäsenet|matriisi|Ei |
|list_id|merkkijono|Ei |
|total_items|kokonaisluku|Ei |



### <a name="adduserresponsemodel"></a>AddUserResponseModel


| Ominaisuuden nimi | Tietotyyppi | Pakollinen |
|---|---|---|
|tunnus|merkkijono|Kyllä |
|Email_Address|merkkijono|Kyllä |
|unique_email_id|merkkijono|Ei |
|email_type|merkkijono|Ei |
|tila|merkkijono|Ei |
|merge_fields|ei ole määritetty|Kyllä |
|kiinnostuksen kohteet|merkkijono|Ei |
|Tilasto|ei ole määritetty|Ei |
|ip_signup|merkkijono|Ei |
|timestamp_signup|merkkijono|Ei |
|ip_opt|merkkijono|Ei |
|timestamp_opt|merkkijono|Ei |
|member_rating|kokonaisluku|Ei |
|last_changed|merkkijono|Ei |
|kieli|merkkijono|Ei |
|VIP|Totuusarvo|Ei |
|email_client|merkkijono|Ei |
|sijainti|ei ole määritetty|Ei |
|last_note|ei ole määritetty|Ei |
|list_id|merkkijono|Ei |
|_links|matriisi|Ei |


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
