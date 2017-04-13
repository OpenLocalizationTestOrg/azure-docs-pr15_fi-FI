<properties
   pageTitle="Lokitiedoston Analytics ilmoitusten REST-Ohjelmointirajapinnalla"
   description="Log Analytics ilmoitusten REST API avulla voit luoda ja hallita ilmoituksia toimintojen hallinta Suite (OMS).  Tässä artikkelissa on tietoja Ohjelmointirajapinnan ja esimerkkien eri toimintoja varten."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Lokitiedoston Analytics ilmoitusten REST-Ohjelmointirajapinnalla

Log Analytics ilmoitusten REST API avulla voit luoda ja hallita ilmoituksia toimintojen hallinta Suite (OMS).  Tässä artikkelissa on tietoja Ohjelmointirajapinnan ja esimerkkien eri toimintoja varten.

Log Analytics haun REST-Ohjelmointirajapinta on RESTful ja käyttää Azure Resurssienhallinta REST-Ohjelmointirajapinnalla kautta. Tässä asiakirjassa löydät esimerkkejä mistä Ohjelmointirajapinnan käytetään [ARMClient](https://github.com/projectkudu/ARMClient), Avaa lähde komentoriviltä työkalua, joka helpottaa käynnistettäessä Azure Resurssienhallinta-Ohjelmointirajapinnan käyttäminen PowerShell komentoriviltä. ARMClient ja PowerShell on useita vaihtoehtoja, voit käyttää lokiin Analytics haun Ohjelmointirajapinta. Näillä työkaluilla voit hyödyntää RESTful Azure Resurssienhallinta-Ohjelmointirajapinta soittaa OMS työtilojen ja tee haku-komennot, niiden välillä. Ohjelmointirajapinnan siirtää hakutulosten sinulle JSON-muodossa, jolloin voit käyttää hakutulosten ohjelmallisesti monella tavalla.

## <a name="prerequisites"></a>Edellytykset
Tällä hetkellä ilmoituksia vain voidaan luoda lokin Analytics tallennettu haku.  Voit viitata [Log haun REST-Ohjelmointirajapinta](log-analytics-log-search-api.md) lisätietoja.

## <a name="schedules"></a>Aikatauluja
Tallennetun haun voi olla yksi tai useampi aikatauluja. Aikataulun määrittää, kuinka usein haku ei suorita ja päällä, jossa ehdot on määritetty aikaväli.
Aikataulujen on määrittämäsi ominaisuudet seuraavan taulukon.

| Ominaisuus  | Kuvaus |
|:--|:--|
| Väli | Kuinka usein haku tehdään. Minuutteina. |
| QueryTimeSpan | Tulosjoukko, jolle ehdot arvioidaan aikaväli. On oltava yhtä suuri kuin tai suurempi kuin aikaväli. Minuutteina. |
| Versio | API-versio on käytössä.  Tällä hetkellä tämän tulee aina arvoksi 1. |

Esimerkkinä tapahtuma-kysely, jossa on 15 minuutin välein ja aikajakson 30 minuuttia. Tässä tapauksessa kysely suoritetaan 15 minuutin välein ja hälytys aina, jos ehto jatkuu osoittamaan tosi yli 30 minuutin aikaväli.

### <a name="retrieving-schedules"></a>Haetaan aikatauluja
Get-menetelmän avulla voit hakea kaikki aikataulut tallennetun haun.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Aikataulun tunnuksella Get-menetelmän avulla voit noutaa tallennetun haun tiettyyn aikataulu.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Seuraavassa on esimerkki vastauksen sarjoille.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Aikataulun luominen
Ainutkertainen aikataulu-tunnuksella hyllytetty menetelmän avulla voit luoda uuden aikataulun.  Huomaa, kaksi aikatauluja voi olla sama tunnus, vaikka heillä ei olisikaan liitetty eri tallennetut haut.  Kun luot aikataulun OMS konsolissa, GUID-tunnus luodaan aikataulun ID-tunnuksellasi.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Aikataulun muokkaaminen
Aiemmin aikataulu-tunnuksella saman tallennetun haun hyllytetty menetelmän avulla voit muokata, aikataulu.  Pyynnön leipätekstiin täytyy sisältää etag aikataulun.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Aikataulujen poistaminen
Poista aikataulun aikataulun tunnuksella Delete-menetelmä avulla.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Toiminnot
Aikataulun voi olla useita toimintoja. Toiminnon voi määrittää yhden tai useamman prosesseja, kuten sisältävän sähköpostiviestin lähettäminen uudelleen tai käynnistää niitä runbookin suorittavan tai se voi määrittää raja-arvon, joka määrittää, milloin haun tulokset vastaavat muutamia.  Joitakin toimintoja määrittää sekä niin, että prosessit suoritetaan, kun kynnysarvo täyttyy.

Kaikki toiminnot on määrittämäsi ominaisuudet seuraavan taulukon.  Erityyppiset ilmoitukset on eri lisäominaisuuksia, joka on kuvattu seuraavassa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi | Toiminnon tyyppi.  Mahdolliset arvot ovat tällä hetkellä ilmoituksen ja Webhook. |
| Nimi | Ilmoituksen näyttönimi. |
| Versio | API-versio on käytössä.  Tällä hetkellä tämän tulee aina arvoksi 1. |

### <a name="retrieving-actions"></a>Toimintojen hakeminen
Get-menetelmän avulla voit hakea kaikki toiminnot sarjoille.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Toiminto-tunnuksella Get-menetelmän avulla voit hakea tietyn toiminnon sarjoille.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Luomisessa ja muokkaamisessa toiminnot
Voit tehdä-menetelmällä toiminto-tunnus, joka on yksilöivä aikatauluun voit luoda uuden toiminnon.  Kun luot toiminnon OMS konsolissa, GUID-tunnus on toiminto-tunnuksen

Aiemmin toiminto-tunnuksella saman tallennetun haun hyllytetty menetelmän avulla voit muokata, aikataulu.  Pyynnön leipätekstiin täytyy sisältää etag aikataulun.

Uuden toiminnon luomisen pyynnön muoto vaihtelee toiminnon tyyppi niin näitä esimerkkejä annetaan ohjeita.

### <a name="deleting-actions"></a>Poistaminen toiminnot
Toiminto-tunnuksella Delete-menetelmä avulla voit poistaa toiminnon.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Ilmoitusten toiminnot
Aikataulun pitäisi olla vain yksi ilmoitustoiminnon.  Ilmoitusten toiminnot on vähintään yksi seuraavista osat seuraavassa taulukossa.  Lisää tiedot alla on kuvattu kunkin.

| Osa | Kuvaus |
|:--|:--|
| Raja-arvo | Kun toiminto suoritetaan ehdot. |  
| EmailNotification | Sähköpostin lähettäminen useille vastaanottajille. |
| Korjaus | Aloita runbookin Azure automaatio yrittää korjata tunnistettua ongelman. |

#### <a name="thresholds"></a>Raja-arvot
Ilmoitus toiminnon pitäisi olla vain yksi raja-arvon.  Kun tallennetun haun tulokset vastaavat raja-arvon, että haku liittyvän toiminnon, muut prosessit-toiminto suoritetaan.  Toiminnon voi sisältää myös vain kynnysarvo, niin, että se voidaan käyttää muita tyypit, jotka eivät sisällä raja-arvot toiminnot.

Raja-arvot ovat ominaisuudet seuraavan taulukon.

| Ominaisuus | Kuvaus |
|:--|:--|
| Operaattori | Raja-vertailu operaattori. <br> gt = suurempi kuin <br> lt = alle |
| Arvo | Raja-arvon. |

Esimerkkinä tapahtumakyselyn 15 minuuttia ja raja-arvo on suurempi kuin 10 aikajakson 30 minuutin välein. Tässä tapauksessa kysely suoritetaan 15 minuutin välein ja hälytys aina, jos se palauttaa 10 tapahtumia, jotka on luotu tietyn ajanjakson 30 minuuttia.

Seuraavassa on esimerkki vastausta toiminto, jolla vain kynnysarvo.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Luo uusi raja-toiminto sarjoille yksilöivä toimenpiteen tunnuksella hyllytetty menetelmän avulla.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Toiminto-tunnuksella hyllytetty menetelmän avulla voit muokata raja-toiminnon sarjoille.  Pyynnön leipätekstiin täytyy sisältää toiminnon etag.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Sähköposti-ilmoituksen
Sähköposti-ilmoitusten lähettää sähköpostia yhdelle tai usealle vastaanottajalle.  Seuraavassa taulukossa niissä ominaisuudet.

| Ominaisuus | Kuvaus |
|:--|:--|
| Vastaanottajat | Sähköpostiosoitteiden luettelo. |
| Aihe | Viestin aihe. |
| Liite | Liitteitä ei tällä hetkellä tueta, joten tämä on aina arvo on "Ei". |

Seuraavassa on esimerkki vastausta sähköpostin ilmoituksen toiminto, jolla raja-arvon.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Luo uusi sähköpostiviesti-toiminto sarjoille yksilöivä toimenpiteen tunnuksella hyllytetty menetelmän avulla.  Seuraava esimerkki luo sähköposti-ilmoituksen kanssa raja-arvon, jolloin viestit lähetetään, kun tallennetun haun tulokset suurempi kuin kynnysarvo.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Toiminto-tunnuksella hyllytetty menetelmän avulla voit muokata sarjoille sähköposti-toiminnon.  Pyynnön leipätekstiin täytyy sisältää toiminnon etag.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Korjaus-toiminnot
Remediations Käynnistä runbookin Azure automaatio, joka yrittää korjata merkittyä ilmoituksen.  On webhook korjaus-toimintoa käytetään runbookin, Luo ja määritä sitten WebhookUri-ominaisuuden URI.  Kun luot tätä toimintoa käyttämällä OMS-konsolin, uusi webhook luodaan automaattisesti: n runbookin.

Remediations sisällyttää ominaisuudet seuraavan taulukon.

| Ominaisuus | Kuvaus |
|:--|:--|
| RunbookName | Nimi: n runbookin. Tämä on vastattava julkaistun runbookin määritetty OMS työtilassa automaatio-ratkaisussa automaatio-tilille. |
| WebhookUri | Webhook URI.
| Määräajan | Vanhentumispäivä ja aika webhook.  Jos webhook ei ole vanheneminen, tämä voi olla mikä tahansa kelvollinen tulevan päivämäärän. |

Seuraavassa on esimerkki vastausta korjaus-toiminto, jolla raja-arvon.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Yksilöivä toimenpiteen tunnuksella hyllytetty menetelmän avulla voit luoda uuden korjaamisen toiminnon sarjoille.  Seuraavassa esimerkissä luodaan korjaus kanssa raja, jotta: n runbookin alkaa, kun tallennetun haun tulokset suurempi kuin kynnysarvo.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Toiminto-tunnuksella hyllytetty menetelmän avulla voit muokata korjaus-toiminnon sarjoille.  Pyynnön leipätekstiin täytyy sisältää toiminnon etag.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Esimerkki
Seuraavassa on valmis esimerkki luo uusi sähköposti-ilmoituksen.  Tämä luo uuden aikataulun sekä toiminnon, joka sisältää raja- ja sähköpostitoimialueitasi.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook toiminnot
Webhook toiminnot Aloita kutsumalla URL-osoite ja tarjoavat myös tiedot lähetetään.  Niitä samalla tavalla kuin korjaus toiminnot, mutta ne on tarkoitettu webhooks, joka voi käynnistää prosesseja kuin Azure automaatio runbooks.  Ne sisältävät myös toimitetaan remote-prosessi paketti käytetäänkin lisäasetuksia.

Webhook toiminnot raja-arvo ei ole, mutta sen sijaan lisätään aikataulun, joka on ilmoitusten toiminto, jolla raja-arvon.  Voit lisätä useita Webhook toimintoja, jotka kaikki suoritetaan täyttyessä raja-arvon.

Webhook toiminnot sisällyttää ominaisuudet seuraavan taulukon.

| Ominaisuus | Kuvaus |
|:--|:--|
| WebhookUri | Viestin aihe. |
| CustomPayload | Mukautetut tiedot lähetetään webhook.  Muoto riippuu webhook odottaa. |

Seuraavassa on esimerkki vastausta webhook-toiminto ja liittyvän ilmoituksen toiminto, jolla raja-arvon.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Luo tai Muokkaa webhook-toiminto
Yksilöivä toimenpiteen tunnuksella hyllytetty menetelmän avulla voit luoda uuden webhook toiminnon sarjoille.  Seuraavassa esimerkissä luodaan Webhook-toiminto ja ilmoitusten toiminnon kanssa raja webhook käynnistyy, kun tallennetun haun tulokset suurempi kuin kynnysarvo.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Toiminto-tunnuksella hyllytetty menetelmän avulla voit muokata webhook-toiminnon sarjoille.  Pyynnön leipätekstiin täytyy sisältää toiminnon etag.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Seuraavat vaiheet

- Käyttää lokiin Analytics [REST API log hakujen tekemiseen](log-analytics-log-search-api.md) .
