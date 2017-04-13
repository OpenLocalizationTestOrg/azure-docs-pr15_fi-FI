<properties 
    pageTitle="Logiikan-sovellusten Azure App palvelussa valvonta | Microsoft Azure" 
    description="Logiikan sovelluksia on tehty tarkastelu" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="monitor-your-logic-apps"></a>Logiikan sovellusten valvonta

[Logiikan sovelluksen luominen](app-service-logic-create-a-logic-app.md), kun näet sen Azure Portalissa suorittamisen historia.  Voit myös määrittää palvelujen, kuten Azure diagnostiikka- ja Azure ilmoitusten valvottavat tapahtumat reaaliaikainen ja ilmoitusten tapahtumien pidät "Kun enemmän kuin 5 suoritetaan hylätty tunnin kuluessa."

## <a name="monitor-in-the-azure-portal"></a>Näytön Azure-portaalissa

Näytä historia, valitse **Selaa**ja valitse **Logiikan sovellukset**. Tilauksen kaikkien logiikan sovellusten luettelo näkyy.  Valitse logiikan-sovellus, joita haluat seurata.  Näet kaikki toiminnot ja asennuksen aikana tapahtuneista virheistä logiikan sovelluksen käynnistimien luettelo.

![Yleiskatsaus](./media/app-service-logic-monitor-your-logic-apps/overview.png)

Tämä sivu joitakin osia, jotka ovat hyödyllisiä on:

- **Yhteenveto** on lueteltu **kaikki suoritetaan** ja **Käynnistimen historia**
    - **Kaikki suoritetaan** luettelon uusimman logiikan sovelluksen suoritetaan.  Valitse minkä tahansa rivin lisätietoja Suorita, tai luettelon Lisää suoritetaan-ruutu.
    - **Käynnistimen versiohistoria** näyttää kaikki logiikan sovelluksen käynnistimen-tehtävän.  Käynnistimen tehtävän voi olla "Ohitetut" rasti uusissa tiedoissa (esimerkiksi jakaa nähdäksesi, jos uusi tiedosto on lisätty FTP), "Onnistui" eli Kyselysäännön logiikan app palautti tiedot tai "Epäonnistui" vastaavan määrityksessä virheen.
- **Diagnostiikan** avulla voit runtime tietojen ja tapahtumien ja [Azure ilmoitusten](#adding-azure-alerts) tilaaminen

>[AZURE.NOTE] Kaikki runtime tiedot ja tapahtumien salataan logiikan App palvelun rest-palvelussa. Ne vain purkaa näkymän käyttäjän pyynnöstä. Käyttää näitä tapahtumia voidaan ohjata myös Azure Role-Based Access ohjausobjektin (RBAC).

### <a name="view-the-run-details"></a>Suorita tarkastella

Suorittaa luettelo näkyy **tila**, **Aloitusaika**ja **kesto** erityisesti Suorita. Valitse minkä tahansa rivin näet tiedot, jotka suoritetaan.

Seurannan näkymässä näkyy kunkin vaiheen Suorita, syötteiden ja tulostaa ja virhesanomia, jotka voivat sisältää occurre.

![Suorita ja toiminnot](./media/app-service-logic-monitor-your-logic-apps/monitor-view.png)

Jos tarvitset lisää kaikki tiedot, kuten suorituksen **Korrelaatiotunnus** (joka voi käyttää REST API), voit valita **Tiedot Suorita** -painiketta.  Tämä vaihtoehto sisältää kaikki vaiheet, tila ja syötteiden ja tulostaa, suorita.

## <a name="azure-diagnostics-and-alerts"></a>Azure diagnostiikka- ja ilmoitukset

Lisäksi myöntämä Azure-portaalin ja REST API yllä tietoja voit määrittää logiikan sovelluksen käyttämään Azure diagnostiikka monipuolisia tiedot ja virheenkorjaus.

1. Valitse logiikan app sivu **Diagnostiikka** -osa
1. Valitse **Diagnostiikka-asetusten muuttaminen**
1. Määritä tapahtumaa-toiminnossa tai tallennustilan tilin tietojen tulostaminen

    ![Azure diagnostiikan asetusten](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

### <a name="adding-azure-alerts"></a>Azure hälytyksiä lisäämällä

Kun kansio on määritetty, voit lisätä Azure-ilmoituksia, kun tietyt raja-arvot ovat pystyyn Kyselysäännön.  Valitse **ilmoitukset** -ruutua ja **Lisää ilmoitusten** **Diagnostiikka** -sivu.  Tämä edetään ohjatusti määrittäminen ilmoituksen, montako raja-arvot ja arvot.

![Azure ilmoitusten arvot](./media/app-service-logic-monitor-your-logic-apps/alerts.png)

Voit määrittää **ehto**, **raja-arvon**ja **jakson** haluamallasi tavalla.  Voit lopuksi Lähetä ilmoitus sähköpostiosoitteen määrittäminen tai määrittäminen webhook.  Voit logiikan app [pyynnön käynnistin](../connectors/connectors-native-reqres.md) voidaan suorittaa ilmoituksen sekä (Voit tehdä esimerkiksi [liukuma julkaiseminen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app), [Lähetä tekstiä](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)tai [Lisää viesti, jossa jono](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)).

### <a name="azure-diagnostics-settings"></a>Azure diagnostiikan asetusten

Jokainen näitä tapahtumia sisältää tietoja logiikan sovelluksen ja tapahtuman tila.  Tässä on esimerkki *ActionCompleted* nopeasti:

```javascript
{
            "time": "2016-07-09T17:09:54.4773148Z",
            "workflowId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
            "resourceId": "/SUBSCRIPTIONS/80D4FE69-ABCD-EFGH-A938-9250F1C8AB03/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-06-01",
                "startTime": "2016-07-09T17:09:53.4336305Z",
                "endTime": "2016-07-09T17:09:53.5430281Z",
                "status": "Succeeded",
                "code": "OK",
                "resource": {
                    "subscriptionId": "80d4fe69-ABCD-EFGH-a938-9250f1c8ab03",
                    "resourceGroupName": "MyResourceGroup",
                    "workflowId": "cff00d5458f944d5a766f2f9ad142553",
                    "workflowName": "MyLogicApp",
                    "runId": "08587361146922712057",
                    "location": "eastus",
                    "actionName": "Http"
                },
                "correlation": {
                    "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
                    "clientTrackingId": "my-custom-tracking-id"
                },
                "trackedProperties": {
                    "myProperty": "<value>"
                }
            }
        }
```

Kaksi ominaisuudet, jotka ovat erityisen hyödyllisiä seuranta ja valvominen on *clientTrackingId* ja *trackedProperties*.  

#### <a name="client-tracking-id"></a>Ostajantunnus seuranta

Asiakkaan seuranta tunnus on arvo, joka yhdistää tapahtumat välillä logiikan app suorittaa, mukaan lukien sisäkkäisiä työnkulkuja kutsutaan osana logiikan-sovellusta.  Tunnus olla automaattisesti luodut Jos näin ei ole annettu, mutta voit määrittää manuaalisesti seuranta tunnus käynnistimestä välittämällä asiakas `x-ms-client-tracking-id` otsikon käynnistimen pyynnön (pyynnön käynnistin, HTTP käynnistimen tai webhook käynnistimen) tunnus-arvolla.

#### <a name="tracked-properties"></a>Seurattujen ominaisuudet

Seurattujen ominaisuudet voidaan lisätä sivulle työnkulun määritys voi seurata syötteiden toimintoa tai tulostaa diagnostiikka tiedot.  Tämä voi olla hyödyllinen, jos haluat seurata tietoja, kuten "Tilaustunnuksen"-kohdassa telemetriatietojen.  Voit lisätä Jäljitettyjen ominaisuuden, Sisällytä `trackedProperties` toiminnon-ominaisuutta.  Seurattujen ominaisuudet voivat vain seuraa yhden toiminnot-syötteiden ja tulostaa, mutta voit käyttää `correlation` tapahtumista, voit yhdistää usean Suorita toiminnot ominaisuudet.

```javascript
{
    "myAction": {
        "type": "http",
        "inputs": {
            "uri": "http://uri",
            "headers": {
                "Content-Type": "application/json"
            },
            "body": "@triggerBody()"
        },
        "trackedProperties":{
            "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
            "myActionHTTPValue": "@action()['outputs']['body']['foo']",
            "transactionId": "@action()['inputs']['body']['bar']"
        }
    }
}
```

### <a name="extending-your-solutions"></a>Ratkaisujen laajentaminen

Tämä telemetriatietojen tapahtumaa-toiminnossa tai tallennustilan kyselyjä muiden palvelujen, kuten [Toimintojen hallinta Suite](https://www.microsoft.com/cloud-platform/operations-management-suite), [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)ja [Power BI](https://powerbi.com) on reaaliaikainen seuranta työnkulkuja integroinnin avulla voidaan hyödyntää.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Yleisiä esimerkkejä ja tilanteita, joissa logiikan sovellukset](app-service-logic-examples-and-scenarios.md)
- [Logiikan sovelluksen käyttöönotto mallin luominen](app-service-logic-create-deploy-template.md)
- [Yrityksen asiakasintegraatio-ominaisuuksia](app-service-logic-enterprise-integration-overview.md)
