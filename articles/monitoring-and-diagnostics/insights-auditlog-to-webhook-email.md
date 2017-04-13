<properties
    pageTitle="Webhook määrittämistä Azure tehtävän Log ilmoitusten | Microsoft Azure"
    description="Katso, miten voit soittaa webhooks tehtävän Log ilmoitusten avulla. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Määrittää webhook Azure tehtävän Log-ilmoitukset

Webhooks avulla voit reitittää Azure ilmoitusviesti muista järjestelmistä jälkeinen käsittely tai mukautettuja toimintoja. Voit käyttää webhook ilmoituksen reitittämiseen palvelut, jotka tekstiviestien, kirjaudu virheet, Ilmoita ryhmän keskustelu/messaging palveluiden kautta tai toimia muita toimintoja. Tässä artikkelissa webhook määrittämisestä Azure tehtävän Log-ilmoituksen ja kun paketti HTTP-KIRJOITUKSEN webhook varten. Saat lisätietoja asetukset ja rakenteen Azure metrisillä ilmoituksen, [Katso tämän sivun sijaan](insights-webhooks-alerts.md). Voit myös määrittää tehtävän Log-ilmoituksen sähköpostin käynnistyessä.

>[AZURE.NOTE] Tämä toiminto ei esikatselussa, odottaa niin muuttujan laatua ja suorituskykyä.

Voit määrittää tehtävän Log-ilmoituksen [Azure PowerShellin cmdlet-komennot](insights-powershell-samples.md#create-alert-rules), [Office kaikissa ympäristöissä CLI](insights-cli-samples.md#work-with-alerts)tai [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Todennustapa webhook
Ryhmätoimintojen webhook voi todentaa jommallakummalla seuraavista tavoista:

1. **Tunnus-pohjainen todennus** - webhook URI tallennetaan esimerkiksi suojaustunnuksen tunnuksella. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Tavallinen todennus** - webhook URI on tallennettu käyttäjänimellä ja salasanalla, esimerkiksi. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Tietojen rakenne
POST-toiminto sisältää seuraavat JSON-paketti ja kaikki tehtävän Log Hakupohjaisten ilmoitusten rakennetta. Tämän mallin muistuttaa metrijärjestelmä Hakupohjaisten ilmoitusten käyttämä.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Rakenteen nimeä       |Kuvaus|
|---                |---|
|tila             |Käyttää metrisillä ilmoitukset. Määrittää "aktivoitu" aina tehtävän Log ilmoitukset.|
|konteksti            |Tapahtuman kontekstissa.|
|resourceProviderName|Resurssin tarjoaja sellaisiin resurssin.|
|conditionType      |Aina "tapahtuma."|
|Nimi               |Ilmoitusten säännön nimi.|
|tunnus                 |Resurssitunnus ilmoituksen.|
|kuvaus        |Ilmoituksen kuvaus joukkona ilmoituksen luonnin aikana.|
|subscriptionId     |Azure tilauksen tunnus.|
|aikaleima          |Aika, johon tapahtuma on luotu Azure-palvelu, jota käsitellä pyynnön.|
|resourceId         |Resurssitunnus sellaisiin resurssin.|
|resourceGroupName  |Resurssiryhmä sellaisiin resurssin nimi|
|Ominaisuudet:         |Useat `<Key, Value>` paria (eli `Dictionary<String, String>`), joka sisältää tapahtuman tiedot.|
|tapahtuman              |Metatietojen tapahtumasta sisältävä osa.|
|todennus      |Tapahtuman RBAC-ominaisuuksia. Näitä ovat yleensä "toiminto", "roolin" ja "alue".|
|luokka           |Tapahtuman luokka. Tuetut arvot ovat: järjestelmänvalvojan, ilmoitus-suojauksen, ServiceHealth, suositus.|
|soittajan             |Käyttäjä, joka suorittaa toiminnon, UPN varaaminen tai SPN varaa käytettävyyden perusteella sähköpostiosoite. Voi olla tiettyjä Järjestelmäkutsuja null.|
|correlationId      |Yleensä GUID-tunnus merkkijono-muodossa. Tapahtumien kanssa correlationId jäsen saman suurempi toiminnon, ja jaa yleensä correlationId.|
|eventDescription   |Tapahtuman kuvaus staattiseksi tekstiksi.|
|eventDataId        |Tapahtuman yksilöllinen.|
|eventSource        |Azure-palvelun tai tapahtuman luoneen infrastruktuurin nimi.|
|httpRequest        |Sisältää yleensä "clientRequestId", "clientIpAddress" ja "menetelmä" (HTTP-menetelmä esimerkiksi SIJOITTAA).|
|taso              |Jokin seuraavista arvoista: "Kriittinen", "Virhe", "Varoitus", "Tiedottavat" ja "Yksityiskohtainen."|
|toimintotunnukseksi        |Yleensä GUID-tunnus jakaa tapahtumat vastaavat yhdellä kertaa.|
|operationName      |Toiminnon nimi.|
|Ominaisuudet:         |Tapahtuman ominaisuudet.|
|tila             |Merkkijono. Toiminnon tilan. Yleiset arvot ovat: "Käynnistetty", "Keskeneräinen", "Onnistui", "Epäonnistui", "Aktiivinen", "Poistunut".|
|alitila          |Sisältää yleensä HTTP-tilakoodin vastaavan REST-puhelun. Voi sisältää myös muut merkkijonot kuvaava alitila. Yleisiä alitila arvot ovat: OK (HTTP Status Code: 200), luotuja (HTTP Status Code: 201), hyväksytyssä (HTTP Status Code: 202), sisältö ei (HTTP Status Code: 204), virheelliset pyytää (HTTP Status Code: 400), ei löydy (HTTP-tilakoodi: 404), ristiriita (HTTP-tilakoodin: 409), Sisäinen palvelinvirhe (HTTP Status Code: 500), palvelu ei ole käytettävissä (HTTP-tilakoodin: 503), yhdyskäytävän aikakatkaisu (HTTP Status Code: 504)|

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja tehtävän lokiin](monitoring-overview-activity-logs.md)
- [Suorittaa Azure automaatio-komentosarjoja (Runbooks) Azure ilmoituksista](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Logiikan sovelluksen tekstiviestien Twilio from Azure ilmoituksen kautta](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Tässä esimerkissä on metrisillä ilmoitusten, mutta se voidaan muokata tehtävän Log-ilmoituksen käsitteleminen.
- [Logiikan sovelluksen Azure ilmoituksen liukuma viesti lähetetään](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Tässä esimerkissä on metrisillä ilmoitusten, mutta se voidaan muokata tehtävän Log-ilmoituksen käsitteleminen.
- [Voit lähettää viestin Azure-jonossa from Azure ilmoituksen logiikan sovelluksen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Tässä esimerkissä on metrisillä ilmoitusten, mutta se voidaan muokata tehtävän Log-ilmoituksen käsitteleminen.
