<properties
   pageTitle="Azure Resurssienhallinta pyynnön rajoitukset | Microsoft Azure"
   description="Tässä artikkelissa käsitellään käyttää rajoittimen pyyntöjen Azure Resurssienhallinta, kun Tilausrajoitukset on saavutettu."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Resurssienhallinta pyynnöt rajoittaminen

Kunkin tilauksen ja vuokraajan Resurssienhallinta rajoitukset lukea 15 000 tunnissa pyynnöt ja kirjoittaa pyynnöt arvon 1 200 tunnissa. Jos sovelluksen tai komentosarjan saavuttaa nämä raja-arvot, joudut rajoita pyyntöjen. Tässä aiheessa kerrotaan selvittäminen ennen enimmäismäärä on saavutettu jäljellä olevat pyynnöt ja niiden vastaa, kun raja on saavutettu.

Kun pääset rajoitus, näyttöön tulee HTTP-tilakoodin **429 liian monta pyynnöt**.

Pyyntöjen määrä on määritetty tilauksen tai alihallintaan. Jos sinulla on useita, samanaikainen sovellukset että pyynnöt tilaukselle näiden sovellusten pyyntöihin lisätään yhteen, kun haluat määrittää jäljellä oleva määrä.

Tilauksen kohdistetuissa pyynnöt ovat kulkeva Tilaustunnus, kuten haetaan resurssin involve ryhmät-tilaukseesi. Vuokraajan suodatetut pyynnöt eivät sisällä Tilaustunnus, kuten haetaan kelvollisia Azure sijainteja.

## <a name="remaining-requests"></a>Jäljellä olevat pyynnöt

Voit määrittää jäljellä oleva määrä tarkastelemalla vastausten otsikot. Sivupyynnön sisältää arvot jäljellä oleva luku ja kirjoita pyynnöt. Seuraavassa taulukossa on kuvattu vastausten otsikot, voit tutkia näistä arvoista:

| Vastauksen otsikon | Kuvaus |
| --------------- | ----------- |
| x-MS-ratelimit-Remaining-Subscription-reads | Tilauksen suodatetut lukee jäljellä |
| x-MS-ratelimit-Remaining-Subscription-writes | Tilauksen suodatetut kirjoittaa jäljellä |
| x-MS-ratelimit-Remaining-tenant-reads | Vuokraajan suodatetut lukee jäljellä |
| x-MS-ratelimit-Remaining-tenant-writes | Vuokraajan suodatetut kirjoittaa jäljellä |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests | Tilauksen määritetty resurssin laji pyyntöjä jäljellä.<br /><br />Ylä-arvoksi palautetaan vain, jos palvelu on ohitettu oletusrajoitus. Resurssienhallinta Lisää tämän arvon tilauksen lukuja tai kirjoituksia sijaan. |
| x-MS-ratelimit-Remaining-Subscription-Resource-ENTITIES-Read | Tilauksen määritetty resurssin laji sivustokokoelman pyynnöt jäljellä.<br /><br />Ylä-arvoksi palautetaan vain, jos palvelu on ohitettu oletusrajoitus. Tämä arvo on jäljellä olevan sivustokokoelman pyyntöjen (resurssit) määrä. |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests | Vuokraajan määritetty resurssin laji pyyntöjä jäljellä.<br /><br />Tämä ylätunniste lisätään vain pyyntöjen vuokraajan tasolla ja vain, jos palvelu on ohitettu oletusrajoitus. Resurssienhallinta Lisää tämän arvon vuokraajan lukuja tai kirjoituksia sijaan. |
| x-MS-ratelimit-Remaining-tenant-Resource-ENTITIES-Read | Vuokraajan määritetty resurssin laji sivustokokoelman pyynnöt jäljellä.<br /><br />Tämä ylätunniste lisätään vain pyyntöjen vuokraajan tasolla ja vain, jos palvelu on ohitettu oletusrajoitus. |

## <a name="retrieving-the-header-values"></a>Otsikko-arvot haetaan

Koodia tai komentosarjaa kyseiset Otsikkoarvot haetaan on sama asia kuin minkä tahansa otsikon arvon hakeminen. 

Esimerkiksi **C#**-noudat otsikon arvo **HttpWebResponse** objektista nimetyt **vastaus** on seuraava koodi:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

**PowerShell**-otsikon arvo noutaa Käynnistä WebRequest-toimintoa.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Tai jos haluat nähdä jäljellä tarjouspyyntöjen virheenkorjaus, voit antaa **-Virheenkorjaus** **PowerShell** -cmdlet-parametrin.

    Get-AzureRmResourceGroup -Debug
    
Joka palauttaa tietoja, mukaan lukien vastauksen seuraava arvo:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

**Azure CLI**voit hakea otsikon arvo tarkempia asennustoiminnolla.

    azure group list -vv --json

Joka palauttaa tietoja, kuten seuraavan objektin:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Odottaa ennen lähettämistä seuraava pyyntö

Kun pääset pyynnön rajoitus, Resurssienhallinta palauttaa **429** HTTP-tilakoodin ja **Yritä jälkeen** arvon otsikko. **Jälkeen uudelleen** arvo määrittää sekuntia sovelluksesi odottaa (tai nukkumaanmenoaikojen) ennen lähettämistä seuraavan pyynnön. Jos lähetät pyynnön, ennen kuin Uudelleenyritysarvo on kulunut, pyyntö ei käsitellä, ja yritä uusi arvo palautetaan.
