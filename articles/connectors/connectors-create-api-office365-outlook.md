<properties
    pageTitle="Lisää Office 365: n Outlook connector logiikan-sovelluksissa | Microsoft Azure"
    description="Luo logiikan sovelluksia Office 365: n Connectorilla voit ottaa käyttöön Office 365: n vuorovaikutusta. Esimerkki: luominen, muokkaaminen ja päivittäminen yhteystietojen ja kalenterin."
    services=""    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="anneta"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="10/18/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-office-365-outlook-connector"></a>Office 365: n Outlook connector käytön aloittaminen 

Office 365: n Outlook Connectorin mahdollistaa Outlook Office 365: ssä. Tämä yhdistin avulla voit luoda, muokata, ja Päivitä yhteystiedot ja kalenterikohteet, ja myös, Lähetä ja vastaa viestiin.

Office 365: n Outlookissa voit:

- Luoda työnkulun sisällä Office 365: n sähköposti ja kalenteri-ominaisuuksilla. 
- Käynnistimien avulla voit käynnistää työnkulun, kun kyseessä on uusi sähköpostiviesti, kun kalenterikohteen päivitetään ja paljon muuta.
- Toimintojen avulla voit lähettää sähköpostia, Luo uusi Kalenteritapahtuma ja paljon muuta. Esimerkiksi kun kyseessä on uusi objekti Salesforce (käynnistimen), lähettää sähköpostia Office 365: n Outlookin (toiminto). 

Tämän artikkelin avulla voit käyttää Office 365: n Outlook connector logiikan-sovelluksessa ja siinä myös Käynnistimet ja toiminnot.

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten yleiseen käyttöön (GA).

Lisätietoja logiikan sovellukset on artikkelissa [mitä logiikan sovellukset](../app-service-logic/app-service-logic-what-are-logic-apps.md) ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Yhteyden muodostaminen Office 365: n

Ennen kuin logiikan sovelluksen voit käyttää mihinkään palveluun, sinun on luotava *yhteyden* palveluun. Yhteyden sisältää logiikan-sovellus ja toisen palvelun välinen yhteys. Esimerkiksi muodostaa yhteyden Office 365: n Outlook tarvitset Office 365- *yhteyden*. Voit luoda yhteyden, kirjoita tavallisesti avulla voit käyttää haluat muodostaa yhteyden palvelun tunnistetiedot. Niin Office 365: n Outlookin kanssa Anna yhteyden muodostaminen Office 365-tilin tunnistetiedot.


## <a name="create-the-connection"></a>Yhteyden muodostaminen

>[AZURE.INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]

## <a name="use-a-trigger"></a>Käytä käynnistin

Käynnistimen tapahtuma, jonka avulla voidaan aloittaa työnkulun määritetty logiikan-sovelluksessa. Käynnistimien "äänestyksen järjestäminen" palvelun aikaväli ja usein haluat. [Lisätietoja käynnistimien](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Logiikan-sovellukseen Kirjoita "office 365: n" käynnistimet luettelo:  

    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)

2. Valitse **Office 365: n Outlook - tapahtuma on pian käynnistyksen yhteydessä**. Jos on jo yhteys, valitse kalenterin avattavasta luettelosta.

    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)

    Jos sinua kehotetaan kirjautumaan, kirjoita merkki yhteyden muodostaminen tiedot. [Luo yhteys](connectors-create-api-office365-outlook.md#create-the-connection) sisältö on esitetty vaiheet. 

    > [AZURE.NOTE] Tässä esimerkissä logiikan sovellus käynnistyy, kun kalenteritapahtuman päivitetään. Lisää toisen toiminnon, joka lähettää tekstiviestin käynnistin tulokset näkyviin. Lisää esimerkiksi Twilio *Lähetä viesti* -toiminto, tekstit, kun Kalenteritapahtuma käynnistetään 15 minuutin. 

3. Valitse **Muokkaa** -painiketta ja määritä **korkojakso** ja **aikaväli** . Esimerkiksi halutessasi lisääminen käynnistin 15 minuutin välein sitten **korkojakso** asettaminen **minuutin**ja määritä **aikaväli** - **15**. 

    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)

4. **Tallenna** tekemäsi muutokset (vasemmassa yläkulmassa työkalurivin). Logiikan sovelluksen on tallennettu, ja se saattaa olla käytössä automaattisesti.


## <a name="use-an-action"></a>Toiminnon käyttäminen

Toiminto on määritetty logiikan-sovelluksessa työnkulun suorittamiin toiminto. [Lisätietoja toiminnot](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Valitse plus-merkkiä. Näet useita vaihtoehtoja: **Lisää toiminnon**, **Lisää ehto**tai jokin **Lisää** asetuksia.

    ![](./media/connectors-create-api-office365-outlook/add-action.png)

2. Valitse **Lisää toiminnon**.

3. Kirjoita tekstiruutuun "office 365: n" kaikki käytettävissä olevat toiminnot luettelo.

    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 

4. Valitse tämän esimerkin **Office 365: n Outlook - yhteystiedon**. Jos on jo yhteys, valitse **Kansio-tunnus**, **Etunimi**ja muita ominaisuuksia:  

    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)

    Jos ohjelma pyytää yhteystiedot, kirjoita tiedot yhteyden luominen. [Luo yhteys](connectors-create-api-office365-outlook.md#create-the-connection) tässä ohjeaiheessa kuvataan nämä ominaisuudet. 

    > [AZURE.NOTE] Tässä esimerkissä on Office 365: n Outlookin uuden yhteyshenkilön luominen. Tulos toisen käynnistimestä avulla voit luoda yhteystiedon. Lisää esimerkiksi SalesForce- *objektin luomisen* käynnistäminen. Valitse Lisää Office 365: n Outlook *Luo yhteyden ottaminen* toiminto, joka käyttää SalesForce-kenttien uudet uuden yhteyshenkilön luominen Office 365: ssä. 

5. **Tallenna** tekemäsi muutokset (vasemmassa yläkulmassa työkalurivin). Logiikan sovelluksen on tallennettu, ja se saattaa olla käytössä automaattisesti.


## <a name="technical-details"></a>Teknisiä tietoja

Seuraavassa on tietoja käynnistimet, toiminnot ja vastaukset, joka tukee tätä yhteyttä:

## <a name="office-365-triggers"></a>Office 365: n Käynnistimet

|Käynnistin | Kuvaus|
|--- | ---|
|[Kun tapahtuma käynnistetään pian](connectors-create-api-office365-outlook.md#when-an-upcoming-event-is-starting-soon)|Tämä toiminto käynnistää vuo tulevien kalenteritapahtuman käynnistettäessä.|
|[Kun uusi sähköposti saapuu](connectors-create-api-office365-outlook.md#when-a-new-email-arrives)|Tämä toiminto käynnistää vuo, kun uusi sähköposti saapuu|
|[Uuden tapahtuman luotaessa](connectors-create-api-office365-outlook.md#when-a-new-event-is-created)|Tämä toiminto tuottaa vuo, kun kalenterin luodaan uusi tapahtuma.|
|[Tapahtuman muokkaamisen](connectors-create-api-office365-outlook.md#when-an-event-is-modified)|Tämä toiminto tuottaa vuo, kun tapahtuman on muokattu kalenteri.|


## <a name="office-365-actions"></a>Office 365-toiminnot

|Toiminto|Kuvaus|
|--- | ---|
|[Hae sähköpostit](connectors-create-api-office365-outlook.md#get-emails)|Tämä toiminto saa sähköpostit kansion.|
|[Lähetä sähköpostiviesti](connectors-create-api-office365-outlook.md#send-an-email)|Tämä toiminto lähettää sähköpostiviestin.|
|[Poistaa sähköpostiviestin](connectors-create-api-office365-outlook.md#delete-email)|Tämä toiminto poistaa sähköpostiviestin tunnuksen mukaan.|
|[Merkitse luetuksi](connectors-create-api-office365-outlook.md#mark-as-read)|Tämä toiminto merkitsee sähköpostiviestin luettu.|
|[Vastaa sähköpostiin](connectors-create-api-office365-outlook.md#reply-to-email)|Tämä toiminto vastauksia sähköpostin.|
|[Hae liite](connectors-create-api-office365-outlook.md#get-attachment)|Tämä toiminto saa sähköpostin liitteenä tunnuksen mukaan.|
|[Lähetä sähköpostia ja asetukset](connectors-create-api-office365-outlook.md#send-email-with-options)|Tämä toiminto lähettää sähköpostiviestin, jossa on useita vaihtoehtoja ja odottaa, että vastaanottaja voi vastaa takaisin jokin vaihtoehdoista.|
|[Hyväksynnän sähköpostin lähettäminen](connectors-create-api-office365-outlook.md#send-approval-email)|Tämä toiminto lähettää sähköpostiviestin hyväksyntä ja odottaa vastausta vastaanottajalta.|
|[Hae kalenterit](connectors-create-api-office365-outlook.md#get-calendars)|Tämä toiminto on lueteltu käytettävissä kalenterit.|
|[Hae tapahtumat](connectors-create-api-office365-outlook.md#get-events)|Tämä toiminto saa tapahtumien lisääminen kalenteriin.|
|[Tapahtuman luominen](connectors-create-api-office365-outlook.md#create-event)|Tämä toiminto luo uuden tapahtuman lisääminen kalenteriin.|
|[Hae tapahtuma](connectors-create-api-office365-outlook.md#get-event)|Tämä toiminto saa tietyn tapahtuman kalenteri.|
|[Tapahtuman poistaminen](connectors-create-api-office365-outlook.md#delete-event)|Tämä toiminto poistaa tapahtuman kalenterin.|
|[Päivittää tapahtuman](connectors-create-api-office365-outlook.md#update-event)|Tämä toiminto päivittää kalenterin tapahtuman.|
|[Hae yhteystiedot-kansiot](connectors-create-api-office365-outlook.md#get-contact-folders)|Tämä toiminto on lueteltu käytettävissä yhteystietokansioihin.|
|[Hae yhteystiedot](connectors-create-api-office365-outlook.md#get-contacts)|Tämä toiminto saa yhteystietoja Yhteystiedot-kansioon.|
|[Yhteyshenkilön luominen](connectors-create-api-office365-outlook.md#create-contact)|Tämä toiminto luo uuden yhteystiedon Yhteystiedot-kansioon.|
|[Hae yhteyshenkilö](connectors-create-api-office365-outlook.md#get-contact)|Tämä toiminto saa tietyn yhteyshenkilön Yhteystiedot-kansioon.|
|[Yhteystiedon poistaminen](connectors-create-api-office365-outlook.md#delete-contact)|Tämä toiminto poistaa yhteystiedon Yhteystiedot-kansioon.|
|[Päivitä yhteyshenkilö](connectors-create-api-office365-outlook.md#update-contact)|Tämä toiminto päivittää yhteystiedon Yhteystiedot-kansioon.|

### <a name="trigger-and-action-details"></a>Käynnistin ja toimintojen tiedot

Tässä osassa on tarkkoja tietoja kunkin käynnistin ja toiminnon, mukaan lukien kaikki pakolliset ja valinnaiset syötteen ominaisuudet ja yhdistimen liittyvät vastaavan tuloksen.

#### <a name="when-an-upcoming-event-is-starting-soon"></a>Kun tapahtuma käynnistetään pian
Tämä toiminto käynnistää vuo tulevien kalenteritapahtuman käynnistettäessä. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Kalenterin yksilöivä tunnus|
|lookAheadTimeInMinutes|Etsi eteenpäin aika|Aika (minuutteina) voit etsiä tulevista tapahtumista eteenpäin|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarItemsList: Kalenterikohteiden luettelo

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|arvo|matriisi|Kalenterikohteiden luettelo|


#### <a name="get-emails"></a>Hae sähköpostit
Tämä toiminto saa sähköpostit kansion. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|Kansiopolku|Kansiopolku|Hakea sähköpostiviestejä sen kansion polku, (oletus: "Saapuneet")|
|alkuun|Alkuun|Hakea sähköpostiviestejä määrä (oletus: 10)|
|fetchOnlyUnread|Lukemattomien viestien hakeminen|Noutaa vain lukemattomien sähköpostiviestien?|
|includeAttachments|Sisällytä liitteet|Jos arvo on TOSI, liitteet myös noudetaan yhdessä sähköposti|
|searchQuery|Hakukyselyn|Voit suodattaa haun kyselyn sähköpostit|
|Ohita|Ohita|Voit ohittaa sähköpostit määrä (oletusarvo: 0)|
|skipToken|Ohita tunnus|Siirry tunnuksen Nouda uusi sivu|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
ReceiveMessage: Sähköpostin vastaanottaminen

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Valitse|merkkijono|Valitse|
|Jos haluat|merkkijono|Jos haluat|
|Aihe|merkkijono|Aihe|
|Tekstissä|merkkijono|Tekstissä|
|Merkitys|merkkijono|Merkitys|
|Liite|Totuusarvo|Sisältää liitteen|
|Tunnus|merkkijono|Viestitunnus|
|IsRead|Totuusarvo|On vain luku|
|DateTimeReceived|merkkijono|Päivämäärän vastaanottoajan|
|Liitteet|matriisi|Liitteet|
|Kopio|merkkijono|Määritä sähköpostiosoitteet puolipisteellä eroteltuina,someone@contoso.com|
|Piilokopio|merkkijono|Määritä sähköpostiosoitteet puolipisteellä eroteltuina,someone@contoso.com|
|IsHtml|Totuusarvo|Html on|


#### <a name="send-an-email"></a>Lähetä sähköpostiviesti
Tämä toiminto lähettää sähköpostiviestin. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|emailMessage *|Sähköpostin|Sähköpostin|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.

#### <a name="delete-email"></a>Poistaa sähköpostiviestin
Tämä toiminto poistaa sähköpostiviestin tunnuksen mukaan. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|messageId *|Viestitunnus|Voit poistaa sähköpostin tunnus|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.

#### <a name="mark-as-read"></a>Merkitse luetuksi
Tämä toiminto merkitsee sähköpostiviestin luettu. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|messageId *|Viestitunnus|Lue sähköpostiviestissä merkitään tunnus|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="reply-to-email"></a>Vastaa sähköpostiin
Tämä toiminto vastauksia sähköpostin. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|messageId *|Viestitunnus|Voit vastata sähköpostin tunnus|
|kommentin *|Kommentti|Vastaa kommenttiin|
|replyAll|Vastaa kaikille|Vastaa kaikille vastaanottajille|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="get-attachment"></a>Hae liite
Tämä toiminto saa sähköpostin liitteenä tunnuksen mukaan. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|messageId *|Viestitunnus|Sähköpostin tunnus|
|attachmentId *|Liitteen tunnus|Lataa liite tunnus|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="when-a-new-email-arrives"></a>Kun uusi sähköposti saapuu
Tämä toiminto käynnistää vuo, kun uusi sähköposti saapuu.

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|Kansiopolku|Kansiopolku|Noutaa sähköpostikansion (oletus: Saapuneet-kansiossa)|
|Jos haluat|Jos haluat|Vastaanottajien sähköpostiosoitteet|
|Valitse|Valitse|Osoitteesta|
|merkitys|Merkitys|Merkitys sähköposti (suuri, Normaali, pieni) (oletus: Normaali)|
|fetchOnlyWithAttachment|Liitteet|Hae vain liitetiedoston sähköpostit|
|includeAttachments|Sisällytä liitteet|Sisällytä liitteet|
|subjectFilter|Aihe-suodatin|Tarkista, aiheessa merkkijono|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
TriggerBatchResponse [ReceiveMessage]

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|arvo|matriisi|


#### <a name="send-email-with-options"></a>Lähetä sähköpostia ja asetukset
Tämä toiminto lähettää sähköpostiviestin, jossa on useita vaihtoehtoja ja odottaa, että vastaanottaja voi vastaa takaisin jokin vaihtoehdoista. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|optionsEmailSubscription *|Tilauksen pyynnön asetukset sähköpostia varten|Tilauksen pyynnön asetukset sähköpostia varten|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
SubscriptionResponse: Hyväksynnän Sähköpostitilaus mallin

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|tunnus|merkkijono|Tilauksen tunnus|
|resurssi|merkkijono|Tilauksen pyynnön resurssi|
|notificationType|merkkijono|Ilmoituksen tyyppi|
|notificationUrl|merkkijono|Ilmoituksen URL-osoite|


#### <a name="send-approval-email"></a>Hyväksynnän sähköpostin lähettäminen
Tämä toiminto lähettää sähköpostiviestin hyväksyntä ja odottaa vastausta vastaanottajalta. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|approvalEmailSubscription *|Tilauksen pyynnön hyväksymisen sähköpostia varten|Tilauksen pyynnön hyväksymisen sähköpostia varten|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
SubscriptionResponse: Hyväksynnän Sähköpostitilaus mallin

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|tunnus|merkkijono|Tilauksen tunnus|
|resurssi|merkkijono|Tilauksen pyynnön resurssi|
|notificationType|merkkijono|Ilmoituksen tyyppi|
|notificationUrl|merkkijono|Ilmoituksen URL-osoite|


#### <a name="get-calendars"></a>Hae kalenterit
Tämä toiminto on lueteltu käytettävissä kalenterit. 

Ei ole parametreja puhelun.

##### <a name="output-details"></a>Tulostustiedot
TablesList

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|arvo|matriisi|


#### <a name="get-events"></a>Hae tapahtumat
Tämä toiminto saa tapahtumien lisääminen kalenteriin. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|$filter|Voit suodattaa kyselyn|ODATA suodatinkyselyn, joka palauttaa tietojen rajoittaminen|
|$orderby|Lajittelu|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|Ohita määrä|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|Suurin Get määrä|Noutaa kohteiden enimmäismäärä (oletus = 256)|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarEventList: Kalenterikohteiden luettelo

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|arvo|matriisi|Kalenterikohteiden luettelo|


#### <a name="create-event"></a>Tapahtuman luominen
Tämä toiminto luo uuden tapahtuman lisääminen kalenteriin. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|kohteen *|Kohteen|Tapahtuman luominen|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarEvent: Yhdistimen tiettyyn kalenterin tapahtuman mallin luokka.

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Tapahtuman yksilöllinen.|
|Osallistujat|matriisi|Tapahtuman osallistujien luettelo.|
|Tekstissä|ei ole määritetty|Tapahtumaan liittyvä viestin tekstiosaan.|
|BodyPreview|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|
|Luokat|matriisi|Tapahtumaan liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio. Aina, kun tapahtuma on muutettu, ChangeKey muuttuvat myös.|
|DateTimeCreated|merkkijono|Päivämäärä ja aika, joka on luotu tapahtuma.|
|DateTimeLastModified|merkkijono|Päivämäärä ja kellonaika, tapahtuma on viimeksi muokattu.|
|Lopeta|merkkijono|Tapahtuman päättymisaika.|
|EndTimeZone|merkkijono|Määrittää aikavyöhykkeen kokouksen päättymisaika. Tämän arvon on oltava määritysten mukaisesti Windows (Esimerkki: Tyynenmeren normaaliaika).|
|Liitteitä|Totuusarvo|Määritä TOSI, jos tapahtuma sisältää liitteitä.|
|Merkitys|merkkijono|Tapahtuman tärkeys: pieni, Normaali tai suuri.|
|IsAllDay|Totuusarvo|Määritä TOSI, jos tapahtuma kestää koko päivä.|
|IsCancelled|Totuusarvo|Määritä TOSI, jos tapahtuma on peruutettu.|
|IsOrganizer|Totuusarvo|Määritä TOSI, jos viestin lähettäjä on myös järjestäjä.|
|Sijainti|ei ole määritetty|Tapahtuman sijainti.|
|Järjestäjä|ei ole määritetty|Tapahtuman järjestäjä.|
|Toistuvat tapahtumat|ei ole määritetty|Tapahtuman toistumiskaavan.|
|Muistutus|kokonaisluku|Minuuttia ennen tapahtuman alkua muistuttamaan aika.|
|ResponseRequested|Totuusarvo|Määritä TOSI, jos lähettäjän haluaisit vastausta, kun tapahtuma on hyväksytty tai hylätty.|
|ResponseStatus|ei ole määritetty|Osoittaa ryhmän tyypin vastaus vastauksena tapahtuma-viesti lähetettiin.|
|SeriesMasterId|merkkijono|Sarjan perustyyli tapahtumalaji yksilöllinen.|
|Näytä muodossa|merkkijono|Näyttää vapaa-tai varattu.|
|Aloittaminen|merkkijono|Tapahtuman aloitusaika.|
|StartTimeZone|merkkijono|Määrittää vyöhykkeen kokouksen aloitusaika. Tämän arvon on oltava määritysten mukaisesti Windows (esimerkiksi: "Tyynenmeren normaaliaika").|
|Aihe|merkkijono|Tapahtuman aihe.|
|Tyyppi|merkkijono|Tapahtumatyyppi: yksittäisen esiintymän, esiintymä, poikkeuksen tai sarjan perustyyli.|
|Web-linkki|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|


#### <a name="get-event"></a>Hae tapahtuma
Tämä toiminto saa tietyn tapahtuman kalenteri. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|tunnus *|Kohteen tunnus|Valitse tapahtuma|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarEvent: Yhdistimen tiettyyn kalenterin tapahtuman mallin luokka.

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Tapahtuman yksilöllinen.|
|Osallistujat|matriisi|Tapahtuman osallistujien luettelo.|
|Tekstissä|ei ole määritetty|Tapahtumaan liittyvä viestin tekstiosaan.|
|BodyPreview|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|
|Luokat|matriisi|Tapahtumaan liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio. Aina, kun tapahtuma on muutettu, ChangeKey muuttuvat myös.|
|DateTimeCreated|merkkijono|Päivämäärä ja aika, joka on luotu tapahtuma.|
|DateTimeLastModified|merkkijono|Päivämäärä ja kellonaika, tapahtuma on viimeksi muokattu.|
|Lopeta|merkkijono|Tapahtuman päättymisaika.|
|EndTimeZone|merkkijono|Määrittää aikavyöhykkeen kokouksen päättymisaika. Tämän arvon on oltava määritysten mukaisesti Windows (Esimerkki: Tyynenmeren normaaliaika).|
|Liitteitä|Totuusarvo|Määritä TOSI, jos tapahtuma sisältää liitteitä.|
|Merkitys|merkkijono|Tapahtuman tärkeys: pieni, Normaali tai suuri.|
|IsAllDay|Totuusarvo|Määritä TOSI, jos tapahtuma kestää koko päivä.|
|IsCancelled|Totuusarvo|Määritä TOSI, jos tapahtuma on peruutettu.|
|IsOrganizer|Totuusarvo|Määritä TOSI, jos viestin lähettäjä on myös järjestäjä.|
|Sijainti|ei ole määritetty|Tapahtuman sijainti.|
|Järjestäjä|ei ole määritetty|Tapahtuman järjestäjä.|
|Toistuvat tapahtumat|ei ole määritetty|Tapahtuman toistumiskaavan.|
|Muistutus|kokonaisluku|Minuuttia ennen tapahtuman alkua muistuttamaan aika.|
|ResponseRequested|Totuusarvo|Määritä TOSI, jos lähettäjän haluaisit vastausta, kun tapahtuma on hyväksytty tai hylätty.|
|ResponseStatus|ei ole määritetty|Osoittaa ryhmän tyypin vastaus vastauksena tapahtuma-viesti lähetettiin.|
|SeriesMasterId|merkkijono|Sarjan perustyyli tapahtumalaji yksilöllinen.|
|Näytä muodossa|merkkijono|Näyttää vapaa-tai varattu.|
|Aloittaminen|merkkijono|Tapahtuman aloitusaika.|
|StartTimeZone|merkkijono|Määrittää vyöhykkeen kokouksen aloitusaika. Tämän arvon on oltava määritysten mukaisesti Windows (esimerkiksi: "Tyynenmeren normaaliaika").|
|Aihe|merkkijono|Tapahtuman aihe.|
|Tyyppi|merkkijono|Tapahtumatyyppi: yksittäisen esiintymän, esiintymä, poikkeuksen tai sarjan perustyyli.|
|Web-linkki|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|


#### <a name="delete-event"></a>Tapahtuman poistaminen
Tämä toiminto poistaa tapahtuman kalenterin. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|tunnus *|Tunnus|Valitse tapahtuma|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="update-event"></a>Päivittää tapahtuman
Tämä toiminto päivittää kalenterin tapahtuman. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|tunnus *|Tunnus|Valitse tapahtuma|
|kohteen *|Kohteen|Tapahtuman päivittäminen|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarEvent: Yhdistimen tiettyyn kalenterin tapahtuman mallin luokka.

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Tapahtuman yksilöllinen.|
|Osallistujat|matriisi|Tapahtuman osallistujien luettelo.|
|Tekstissä|ei ole määritetty|Tapahtumaan liittyvä viestin tekstiosaan.|
|BodyPreview|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|
|Luokat|matriisi|Tapahtumaan liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio. Aina, kun tapahtuma on muutettu, ChangeKey muuttuvat myös.|
|DateTimeCreated|merkkijono|Päivämäärä ja aika, joka on luotu tapahtuma.|
|DateTimeLastModified|merkkijono|Päivämäärä ja kellonaika, tapahtuma on viimeksi muokattu.|
|Lopeta|merkkijono|Tapahtuman päättymisaika.|
|EndTimeZone|merkkijono|Määrittää aikavyöhykkeen kokouksen päättymisaika. Tämän arvon on oltava määritysten mukaisesti Windows (Esimerkki: Tyynenmeren normaaliaika).|
|Liitteitä|Totuusarvo|Määritä TOSI, jos tapahtuma sisältää liitteitä.|
|Merkitys|merkkijono|Tapahtuman tärkeys: pieni, Normaali tai suuri.|
|IsAllDay|Totuusarvo|Määritä TOSI, jos tapahtuma kestää koko päivä.|
|IsCancelled|Totuusarvo|Määritä TOSI, jos tapahtuma on peruutettu.|
|IsOrganizer|Totuusarvo|Määritä TOSI, jos viestin lähettäjä on myös järjestäjä.|
|Sijainti|ei ole määritetty|Tapahtuman sijainti.|
|Järjestäjä|ei ole määritetty|Tapahtuman järjestäjä.|
|Toistuvat tapahtumat|ei ole määritetty|Tapahtuman toistumiskaavan.|
|Muistutus|kokonaisluku|Minuuttia ennen tapahtuman alkua muistuttamaan aika.|
|ResponseRequested|Totuusarvo|Määritä TOSI, jos lähettäjän haluaisit vastausta, kun tapahtuma on hyväksytty tai hylätty.|
|ResponseStatus|ei ole määritetty|Osoittaa ryhmän tyypin vastaus vastauksena tapahtuma-viesti lähetettiin.|
|SeriesMasterId|merkkijono|Sarjan perustyyli tapahtumalaji yksilöllinen.|
|Näytä muodossa|merkkijono|Näyttää vapaa-tai varattu.|
|Aloittaminen|merkkijono|Tapahtuman aloitusaika.|
|StartTimeZone|merkkijono|Määrittää vyöhykkeen kokouksen aloitusaika. Tämän arvon on oltava määritysten mukaisesti Windows (esimerkiksi: "Tyynenmeren normaaliaika").|
|Aihe|merkkijono|Tapahtuman aihe.|
|Tyyppi|merkkijono|Tapahtumatyyppi: yksittäisen esiintymän, esiintymä, poikkeuksen tai sarjan perustyyli.|
|Web-linkki|merkkijono|Tapahtumaan liittyvä viestin esikatselu.|


#### <a name="when-a-new-event-is-created"></a>Uuden tapahtuman luotaessa
Tämä toiminto tuottaa vuo, kun kalenterin luodaan uusi tapahtuma. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|$filter|Voit suodattaa kyselyn|ODATA suodatinkyselyn, joka palauttaa tietojen rajoittaminen|
|$orderby|Lajittelu|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|Ohita määrä|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|Suurin Get määrä|Noutaa kohteiden enimmäismäärä (oletus = 256)|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarItemsList: Kalenterikohteiden luettelo

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|arvo|matriisi|Kalenterikohteiden luettelo|


#### <a name="when-an-event-is-modified"></a>Tapahtuman muokkaamisen
Tämä toiminto tuottaa vuo, kun tapahtuman on muokattu kalenteri. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kalenterin tunnus|Valitse kalenteri|
|$filter|Voit suodattaa kyselyn|ODATA suodatinkyselyn, joka palauttaa tietojen rajoittaminen|
|$orderby|Lajittelu|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|Ohita määrä|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|Suurin Get määrä|Noutaa kohteiden enimmäismäärä (oletus = 256)|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
CalendarItemsList: Kalenterikohteiden luettelo


| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|arvo|matriisi|Kalenterikohteiden luettelo|


#### <a name="get-contact-folders"></a>Hae yhteystiedot-kansiot
Tämä toiminto on lueteltu käytettävissä yhteystietokansioihin. 

Ei ole parametreja puhelun.

##### <a name="output-details"></a>Tulostustiedot
TablesList

| Ominaisuuden nimi | Tietotyyppi |
|---|---|
|arvo|matriisi|


#### <a name="get-contacts"></a>Hae yhteystiedot
Tämä toiminto saa yhteystietoja Yhteystiedot-kansioon. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kansion tunnus|Yksilöllinen tunnus hakemiseen Yhteystiedot-kansio|
|$filter|Voit suodattaa kyselyn|ODATA suodatinkyselyn, joka palauttaa tietojen rajoittaminen|
|$orderby|Lajittelu|ODATA lajittelu-kyselyn argumenteille tapahtumien järjestys|
|$skip|Ohita määrä|Voit ohittaa merkintöjen määrä (oletus = 0)|
|$top|Suurin Get määrä|Noutaa kohteiden enimmäismäärä (oletus = 256)|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
ContactList: Yhteystietojen luettelo

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|arvo|matriisi|Yhteystietoluettelo|


#### <a name="create-contact"></a>Yhteyshenkilön luominen
Tämä toiminto luo uuden yhteystiedon Yhteystiedot-kansioon. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kansion tunnus|Valitse Yhteystiedot-kansio|
|kohteen *|Kohteen|Yhteyshenkilön luominen|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Yhteyshenkilö: yhteyshenkilö

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Yhteyshenkilön yksilöllinen.|
|Edellyttää ParentFolderId|merkkijono|Yhteyshenkilön pääkansion tunnus|
|Syntymäpäivä|merkkijono|Yhteyshenkilön syntymä.|
|FileAs|merkkijono|Yhteyshenkilön nimi on tallennettu.|
|Näyttönimi|merkkijono|Yhteyshenkilön nimi.|
|GivenName|merkkijono|Yhteyshenkilön etunimi.|
|Nimikirjaimet|merkkijono|Yhteyshenkilön nimikirjaimet.|
|MiddleName|merkkijono|Yhteyshenkilön toinen nimi.|
|Lempinimien|merkkijono|Yhteyshenkilön kutsumanimi.|
|Sukunimi|merkkijono|Yhteyshenkilön sukunimi.|
|Otsikko|merkkijono|Yhteyshenkilön nimi.|
|Luonti|merkkijono|Yhteyshenkilön luominen.|
|EmailAddresses|matriisi|Yhteyshenkilön sähköpostiosoitteet.|
|ImAddresses|matriisi|Yhteyshenkilön instant messaging (IM)-osoitteet.|
|Asema|merkkijono|Yhteyshenkilön tehtävänimike.|
|YrityksenNimi|merkkijono|Yhteyshenkilön yrityksen nimi.|
|Osasto|merkkijono|Yhteyshenkilön osasto.|
|OfficeLocation|merkkijono|Yhteyshenkilön office sijainti.|
|Ammattia|merkkijono|Yhteyshenkilön ammatti.|
|BusinessHomePage|merkkijono|Yhteyshenkilön business aloitussivulle.|
|AssistantName|merkkijono|Yhteyshenkilön avustajan nimi.|
|Hallinta|merkkijono|Yhteyshenkilön esimiehen nimi.|
|HomePhones|matriisi|Yhteyshenkilön Kotipuhelin numerot.|
|BusinessPhones|matriisi|Yhteyshenkilön business puhelinnumeroita|
|MobilePhone1|merkkijono|Yhteyshenkilön matkapuhelinnumero.|
|Kotiosoite|ei ole määritetty|Yhteyshenkilön Kotiosoite.|
|Työpaikanosoite|ei ole määritetty|Yhteyshenkilön työpaikan osoite.|
|OtherAddress|ei ole määritetty|Muut yhteyshenkilön osoitteet.|
|YomiCompanyName|merkkijono|Yhteyshenkilön foneettinen japaninkielinen yrityksen nimi.|
|YomiGivenName|merkkijono|Foneettinen japaninkielinen annettu nimi (Etunimi) yhteyshenkilön.|
|YomiSurname|merkkijono|Foneettinen japaninkielinen Sukunimi (Sukunimi) yhteystiedon|
|Luokat|matriisi|Yhteyshenkilöön liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio|
|DateTimeCreated|merkkijono|Yhteyshenkilö on luotu aika.|
|DateTimeLastModified|merkkijono|Yhteyshenkilö on muokattu aika.|


#### <a name="get-contact"></a>Hae yhteyshenkilö
Tämä toiminto saa tietyn yhteyshenkilön Yhteystiedot-kansioon. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kansion tunnus|Valitse Yhteystiedot-kansio|
|tunnus *|Kohteen tunnus|Yhteyshenkilön hakemiseen yksilöllinen tunnus|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Yhteyshenkilö: yhteyshenkilö

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Yhteyshenkilön yksilöllinen.|
|Edellyttää ParentFolderId|merkkijono|Yhteyshenkilön pääkansion tunnus|
|Syntymäpäivä|merkkijono|Yhteyshenkilön syntymä.|
|FileAs|merkkijono|Yhteyshenkilön nimi on tallennettu.|
|Näyttönimi|merkkijono|Yhteyshenkilön nimi.|
|GivenName|merkkijono|Yhteyshenkilön etunimi.|
|Nimikirjaimet|merkkijono|Yhteyshenkilön nimikirjaimet.|
|MiddleName|merkkijono|Yhteyshenkilön toinen nimi.|
|Lempinimien|merkkijono|Yhteyshenkilön kutsumanimi.|
|Sukunimi|merkkijono|Yhteyshenkilön sukunimi.|
|Otsikko|merkkijono|Yhteyshenkilön nimi.|
|Luonti|merkkijono|Yhteyshenkilön luominen.|
|EmailAddresses|matriisi|Yhteyshenkilön sähköpostiosoitteet.|
|ImAddresses|matriisi|Yhteyshenkilön instant messaging (IM)-osoitteet.|
|Asema|merkkijono|Yhteyshenkilön tehtävänimike.|
|YrityksenNimi|merkkijono|Yhteyshenkilön yrityksen nimi.|
|Osasto|merkkijono|Yhteyshenkilön osasto.|
|OfficeLocation|merkkijono|Yhteyshenkilön office sijainti.|
|Ammattia|merkkijono|Yhteyshenkilön ammatti.|
|BusinessHomePage|merkkijono|Yhteyshenkilön business aloitussivulle.|
|AssistantName|merkkijono|Yhteyshenkilön avustajan nimi.|
|Hallinta|merkkijono|Yhteyshenkilön esimiehen nimi.|
|HomePhones|matriisi|Yhteyshenkilön Kotipuhelin numerot.|
|BusinessPhones|matriisi|Yhteyshenkilön business puhelinnumeroita|
|MobilePhone1|merkkijono|Yhteyshenkilön matkapuhelinnumero.|
|Kotiosoite|ei ole määritetty|Yhteyshenkilön Kotiosoite.|
|Työpaikanosoite|ei ole määritetty|Yhteyshenkilön työpaikan osoite.|
|OtherAddress|ei ole määritetty|Muut yhteyshenkilön osoitteet.|
|YomiCompanyName|merkkijono|Yhteyshenkilön foneettinen japaninkielinen yrityksen nimi.|
|YomiGivenName|merkkijono|Foneettinen japaninkielinen annettu nimi (Etunimi) yhteyshenkilön.|
|YomiSurname|merkkijono|Foneettinen japaninkielinen Sukunimi (Sukunimi) yhteystiedon|
|Luokat|matriisi|Yhteyshenkilöön liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio|
|DateTimeCreated|merkkijono|Yhteyshenkilö on luotu aika.|
|DateTimeLastModified|merkkijono|Yhteyshenkilö on muokattu aika.|


#### <a name="delete-contact"></a>Yhteystiedon poistaminen
Tämä toiminto poistaa yhteystiedon Yhteystiedot-kansioon. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kansion tunnus|Valitse Yhteystiedot-kansio|
|tunnus *|Tunnus|Jos haluat poistaa yhteyshenkilön yksilöllinen tunnus|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Ei mitään.


#### <a name="update-contact"></a>Päivitä yhteyshenkilö
Tämä toiminto päivittää yhteystiedon Yhteystiedot-kansioon. 

|Ominaisuuden nimi| Näyttönimi|Kuvaus|
| ---|---|---|
|taulukon *|Kansion tunnus|Valitse Yhteystiedot-kansio|
|tunnus *|Tunnus|Päivitä yhteyshenkilön yksilöllinen tunnus|
|kohteen *|Kohteen|Jos haluat päivittää yhteystieto|

Tähtimerkkiä (*) tarkoittaa, että ominaisuus on pakollinen.

##### <a name="output-details"></a>Tulostustiedot
Yhteyshenkilö: yhteyshenkilö

| Ominaisuuden nimi | Tietotyyppi | Kuvaus |
|---|---|---|
|Tunnus|merkkijono|Yhteyshenkilön yksilöllinen.|
|Edellyttää ParentFolderId|merkkijono|Yhteyshenkilön pääkansion tunnus|
|Syntymäpäivä|merkkijono|Yhteyshenkilön syntymä.|
|FileAs|merkkijono|Yhteyshenkilön nimi on tallennettu.|
|Näyttönimi|merkkijono|Yhteyshenkilön nimi.|
|GivenName|merkkijono|Yhteyshenkilön etunimi.|
|Nimikirjaimet|merkkijono|Yhteyshenkilön nimikirjaimet.|
|MiddleName|merkkijono|Yhteyshenkilön toinen nimi.|
|Lempinimien|merkkijono|Yhteyshenkilön kutsumanimi.|
|Sukunimi|merkkijono|Yhteyshenkilön sukunimi.|
|Otsikko|merkkijono|Yhteyshenkilön nimi.|
|Luonti|merkkijono|Yhteyshenkilön luominen.|
|EmailAddresses|matriisi|Yhteyshenkilön sähköpostiosoitteet.|
|ImAddresses|matriisi|Yhteyshenkilön instant messaging (IM)-osoitteet.|
|Asema|merkkijono|Yhteyshenkilön tehtävänimike.|
|YrityksenNimi|merkkijono|Yhteyshenkilön yrityksen nimi.|
|Osasto|merkkijono|Yhteyshenkilön osasto.|
|OfficeLocation|merkkijono|Yhteyshenkilön office sijainti.|
|Ammattia|merkkijono|Yhteyshenkilön ammatti.|
|BusinessHomePage|merkkijono|Yhteyshenkilön business aloitussivulle.|
|AssistantName|merkkijono|Yhteyshenkilön avustajan nimi.|
|Hallinta|merkkijono|Yhteyshenkilön esimiehen nimi.|
|HomePhones|matriisi|Yhteyshenkilön Kotipuhelin numerot.|
|BusinessPhones|matriisi|Yhteyshenkilön business puhelinnumeroita|
|MobilePhone1|merkkijono|Yhteyshenkilön matkapuhelinnumero.|
|Kotiosoite|ei ole määritetty|Yhteyshenkilön Kotiosoite.|
|Työpaikanosoite|ei ole määritetty|Yhteyshenkilön työpaikan osoite.|
|OtherAddress|ei ole määritetty|Muut yhteyshenkilön osoitteet.|
|YomiCompanyName|merkkijono|Yhteyshenkilön foneettinen japaninkielinen yrityksen nimi.|
|YomiGivenName|merkkijono|Foneettinen japaninkielinen annettu nimi (Etunimi) yhteyshenkilön.|
|YomiSurname|merkkijono|Foneettinen japaninkielinen Sukunimi (Sukunimi) yhteystiedon|
|Luokat|matriisi|Yhteyshenkilöön liittyvä luokat.|
|ChangeKey|merkkijono|Tunnistaa tapahtumaobjektin versio|
|DateTimeCreated|merkkijono|Yhteyshenkilö on luotu aika.|
|DateTimeLastModified|merkkijono|Yhteyshenkilö on muokattu aika.|



## <a name="http-responses"></a>HTTP-vastaukset

Toiminnoista ja käynnistimien yllä voi palauttaa yhden tai useamman HTTP tila-koodeista: 

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


## <a name="next-steps"></a>Seuraavat vaiheet

[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Tutustu muiden käytettävissä yhdistimet logiikan sovellusten etsiminen [API-luettelosta](apis-list.md).