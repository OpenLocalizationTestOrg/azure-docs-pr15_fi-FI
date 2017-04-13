<properties
    pageTitle="Automaattinen skaalaus-toimintojen avulla voit lähettää sähköpostiviestejä ja webhook ilmoitukset. | Microsoft Azure"
    description="Katso, miten voit käyttää Automaattinen skaalaus toimintoja web URL-osoitteet tai Lähetä sähköposti-ilmoitusten Azure näyttö. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Automaattinen skaalaus-toimintojen avulla voit lähettää sähköpostiviestejä ja webhook ilmoitusten Azure valvonta

Tässä artikkelissa kerrotaan, miten määrität käynnistimien niin, että voit soittaa tietyn web URL-osoitteet tai lähettää sähköpostiviestejä Azure toimintoa Automaattinen skaalaus perusteella.  

## <a name="webhooks"></a>Webhooks
Webhooks avulla voit reitittää Azure ilmoitusten muista järjestelmistä jälkeinen käsittely tai mukautettuja ilmoituksia. Esimerkiksi reititys ilmoituksen, joka pystyy käsittelemään saapuvat WWW-pyyntö lähetetään SMS-loki virheet ilmoittaa työryhmän käyttämällä keskustelun tai messaging services-palveluiden jne. Webhook URI on oltava kelvollinen HTTP tai HTTPS-päätepiste.

## <a name="email"></a>Sähköpostin
Mikä tahansa kelvollinen sähköpostiosoite voidaan lähettää sähköpostia. Järjestelmänvalvojat ja apuyhteyshenkilöiden Tilauksen, jossa sääntö on käynnissä myös ilmoituksen.


## <a name="cloud-services-and-web-apps"></a>Pilvipalveluiden ja Web Apps-sovelluksista
Voit voit sisältyy Azure-portaalista pilvipalveluihin ja palvelimen klustereihin (online).

- Valitse **mukaan asteikko** -arvo.

![skaalata](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuaalikoneen asteikko joukot
Näennäiskoneiden uudempaan luotiin resurssien hallinnan (virtuaalikoneen asteikko sääntöjoukot) voit määrittää tämän REST API, Resurssienhallinta-mallit, PowerShell ja CLI. Portaalin liittymää ei ole vielä käytettävissä.
Kun REST API tai Resurssienhallinta-mallin avulla lisätä ilmoituksia elementti, jonka seuraavista vaihtoehdoista.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Kenttä                              |Pakollinen? |Kuvaus|
|---                                |---        |---|
|toiminto                          |Kyllä        |arvon on oltava "Asteikko"|
|sendToSubscriptionAdministrator    |Kyllä        |arvon on oltava "true" tai "false"|
|sendToSubscriptionCoAdministrators |Kyllä        |arvon on oltava "true" tai "false"|
|customEmails                       |Kyllä        |arvo voi olla null [] tai merkkijonon matriisin sähköpostit.|
|webhooks                           |Kyllä        |arvo voi olla null tai kelvollinen Uri|
|serviceUri                         |Kyllä        |kelvollinen https Uri|
|Ominaisuudet:                         |Kyllä        |arvon on oltava tyhjä {} tai sisältää avain-arvo-pareina|


## <a name="authentication-in-webhooks"></a>Webhooks todennus
On kaksi todennus URI:

1. Tunnuksen base tunnistamisen, johon tallennat webhook URI parametrina suojaustunnuksen tunnuksella. Esimerkiksi https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Perustodentamista, jossa käytät Käyttäjätunnus ja salasana. Esimerkiksihttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automaattinen skaalaus ilmoituksen webhook paketti rakenne
Kun Automaattinen skaalaus-ilmoitus luodaan, seuraavat metatiedot sisältyy webhook tiedot:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Kenttä  |Pakollinen?|    Kuvaus|
|---|---|---|
|tila |Kyllä    |Tila, joka ilmaisee, että Automaattinen skaalaus-toiminto on luotu|
|toiminto| Kyllä |Se on "Asteikko ulos" esiintymien lisäys- ja varten vähentyä esiintymät, se on "Asteikko"|
|konteksti|   Kyllä |Automaattinen skaalaus-toiminnon yhteydessä|
|aikaleima| Kyllä |Kun Automaattinen skaalaus-toiminto on aiheutunut aikaleima|
|tunnus |Kyllä|   Resurssienhallinta tunnus Automaattinen skaalaus-asetus|
|Nimi   |Kyllä|   Nimen Automaattinen skaalaus-asetus|
|tiedot|   Kyllä |SELITYS toiminnon, joka Automaattinen skaalaus-palvelun noudatit ja esiintymän Laske muutos|
|subscriptionId|    Kyllä |Tilauksen tunnus, joka on skaalattu kohde resurssin|
|resourceGroupName| Kyllä|    Resurssiryhmä, joka on skaalattu kohde resurssin nimi|
|resourceName   |Kyllä|   Kohde, joka on skaalattu resurssin nimi|
|resourceType   |Kyllä|   Kolme tuettuja arvoja: "microsoft.classiccompute/domainnames/slots/roles" - pilvipalvelussa roolit, "microsoft.compute/virtualmachinescalesets" - virtuaalikoneen asteikko joukot- ja "Microsoft.Web/serverfarms" - Web Appissa|
|resourceId |Kyllä|Resurssienhallinta, joka on skaalattu kohde resurssin tunnus|
|portalLink |Kyllä    |Azure portaalin linkin kohde resurssin yhteenveto-sivu|
|oldCapacity|   Kyllä |(Vanha) esiintymän määrän kun Automaattinen skaalaus noudatit asteikko-toiminto|
|newCapacity|   Kyllä |Uuden esiintymän määrä, jotka Automaattinen skaalaus skaalattu resurssin|
|Ominaisuudet:|    Ei| Valinnainen. Määritä < avaimen arvo > paria (esimerkiksi sanaston < merkkijonon, merkkijonon >). Ominaisuudet-kenttä on valinnainen. Mukautettu käyttöliittymä tai logiikan sovelluksen mukaan työnkulun voit kirjoittaa avaimet ja arvot, jotka on välitetty paketti. Vaihtoehtoinen tapa välittää mukautettuja ominaisuuksia lähtevän webhook kutsu on käyttää webhook URI itse (kuten kyselyparametrit)|
