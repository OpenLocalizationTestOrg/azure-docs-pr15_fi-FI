<properties
    pageTitle="Webhooks määrittämistä Azure metrisillä ilmoitusten | Microsoft Azure"
    description="Määritä yhdysviivan reitti-Azure muista järjestelmistä Azure ilmoituksia."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Määritä webhook Azure metrisillä ilmoituksen

Webhooks avulla voit reitittää Azure ilmoitusviesti muista järjestelmistä jälkeinen käsittely tai mukautettuja toimintoja. Voit käyttää webhook ilmoituksen reitittämiseen palvelut, jotka tekstiviestien, kirjaudu virheet, Ilmoita ryhmän keskustelu/messaging palveluiden kautta tai toimia muita toimintoja. Tässä artikkelissa webhook määrittämisestä Azure metrisillä ilmoituksen ja kun paketti HTTP-KIRJOITUKSEN webhook varten. Lisätietoja asetuksista ja rakenteen Azure-toimintojen lokin Ilmoita (tapahtumien koskeva ilmoitus), [Katso tämän sivun sijaan](./insights-auditlog-to-webhook-email.md).

Azure ilmoitusten HTTP POST JSON-ilmoituksen sisältöä muotoillaan, määritetty alla webhook URI, jonka annat ilmoituksen luotaessa rakenne. Tämä URI on oltava kelvollinen HTTP tai HTTPS-päätepiste. Azure kirjaa yksi tapahtuma pyynnön, kun ilmoituksen on aktivoitu.

## <a name="configuring-webhooks-via-the-portal"></a>Portaalin kautta webhooks määrittäminen

Voit lisätä tai päivittää webhook URI [portal](https://portal.azure.com/)Luo/päivitysilmoitukset-näytössä.

![Ilmoitusten säännön lisääminen](./media/insights-webhooks-alerts/Alertwebhook.png)

Voit myös määrittää ilmoituksen julkaiseminen webhook [Azure PowerShellin cmdlet-komennot](./insights-powershell-samples.md#create-alert-rules), [Office kaikissa ympäristöissä CLI](./insights-cli-samples.md#work-with-alerts)tai [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)URI.

## <a name="authenticating-the-webhook"></a>Todennustapa webhook

Webhook voi todentaa jommallakummalla seuraavista tavoista:

1. **Tunnus-pohjainen todennus** - webhook URI tallennetaan esimerkiksi suojaustunnuksen tunnuksella. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Tavallinen todennus** - webhook URI on tallennettu käyttäjänimellä ja salasanalla, esimerkiksi. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Tietojen rakenne

POST-toiminto sisältää seuraavat JSON-paketti ja kaikki metrijärjestelmä Hakupohjaisten ilmoitusten rakennetta.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Kenttä | Pakollinen | Kiinteiden arvojen määrittäminen | Huomautuksia |
| :-------------| :-------------   | :-------------   | :-------------   |
|tila|Y|"Aktivoitu", "Poistunut"|Ilmoitus perään ehdot on määritetty tila.|
|konteksti| Y | | Ilmoituksen yhteydessä.|
|aikaleima| Y | | Aika, jolloin hälytys on.|
|tunnus | Y | | Jokaisen ilmoitusten säännön on yksilöllinen tunnus.|
|Nimi               |Y                  |                   | Ilmoitusten nimi.|
|kuvaus        |Y                  |                           |Ilmoituksen kuvaus.|
|conditionType      |Y                  |"Metrisillä", "Tapahtuma"          |Kahdentyyppisiä ilmoitukset ovat tuettuja. Toinen metrisillä ehdon perusteella ja toinen tehtävä lokiin tapahtuman perusteella. Tarkista, jos ilmoituksen perustuu metrijärjestelmä tai tapahtuman tätä arvoa käytetään.|
|ehto          |Y                  |                           | Tarkista kentät perusteella conditionType.|
|metricName         |Metrijärjestelmän ilmoitusasetusten määrittäminen  |                           |Arvo, joka määrittää säännön valvoo nimi.|
|metricUnit         |Metrijärjestelmän ilmoitusasetusten määrittäminen  |"Tavua", "BytesPerSecond", "Määrä", "CountPerSecond", "Prosentti", "Sekuntia"|     Yksikkö sallittu lisätiedot. [Sallitut arvot on lueteltu täällä](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |Metrijärjestelmän ilmoitusasetusten määrittäminen  |                           |Arvo, joka aiheuttaa ilmoituksen todellinen arvo.|
|raja-arvo          |Metrijärjestelmän ilmoitusasetusten määrittäminen  |                           |Raja-arvo, jolla ilmoitus on aktivoitu.|
|windowSize         |Metrijärjestelmän ilmoitusasetusten määrittäminen  |                           |Ajanjakso, jota käytetään ilmoitusten valvoa raja-arvon perusteella. On oltava 5 minuuttia-1 päivä. ISO 8601 kesto-muodossa.|
|timeAggregation    |Metrijärjestelmän ilmoitusasetusten määrittäminen  |"Keskiarvo", "Sukunimi", "Suurin", "Pienin", "Ei", "yhteensä" | Miten tiedot, jotka kerätään voi yhdistellä ajan kuluessa. Oletusarvo on keskiarvon. [Sallitut arvot on lueteltu täällä](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|operaattori           |Metrijärjestelmän ilmoitusasetusten määrittäminen  |                           |Operaattorin, jolla voit verrata nykyinen metrisillä tietojen määrittäminen raja-arvon.|
|subscriptionId     |Y                  |                           |Azure tilauksen tunnus.|
|resourceGroupName  |Y                  |                           |Sellaisiin resurssin resurssiryhmän nimi.|
|resourceName       |Y                  |                           |Sellaisiin resurssin resurssinimi.|
|resourceType       |Y                  |                           |Resurssin laji sellaisiin resurssin.|
|resourceId         |Y                  |                           |Resurssitunnus sellaisiin resurssin.|
|resourceRegion     |Y                  |                           |Alueen tai sellaisiin resurssin sijainti.|
|portalLink         |Y                  |                           |Suora linkki portaalin resurssin yhteenveto-sivulle.|
|Ominaisuudet:         |N                  |Valinnainen                   |Useat `<Key, Value>` paria (eli `Dictionary<String, String>`), joka sisältää tapahtuman tiedot. Ominaisuudet-kenttä on valinnainen. Mukautettu Käyttöliittymä tai -funktioiden app perustuva työnkulussa käyttäjät voivat kirjoittaa avaimen/arvot, jotka voi välittää kautta paketti. Vaihtoehtoinen tapa välittää mukautetut ominaisuudet webhook on webhook uri itse (kuten kyselyparametrit)|


>[AZURE.NOTE] Ominaisuudet-kentässä voi määrittää vain [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx)avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure ilmoitukset ja videon [Integroida Azure varoittaa PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080) webhooks
- [Suorittaa Azure automaatio-komentosarjoja (Runbooks) Azure ilmoituksista](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logiikan sovelluksella voit lähettää Tekstiviestin kautta Twilio Azure ilmoituksen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Liukuman sähköpostin lähettäminen Azure ilmoituksen logiikan-sovelluksen avulla](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Azure-jonossa sähköpostin lähettäminen Azure ilmoituksen logiikan-sovelluksen avulla](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
