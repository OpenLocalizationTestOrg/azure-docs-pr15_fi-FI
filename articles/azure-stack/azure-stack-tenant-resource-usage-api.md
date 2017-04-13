<properties
    pageTitle="Vuokraaja resurssien käyttö API | Microsoft Azure"
    description="Viittauksen resurssien käyttö API, joka noutaa Azure pinon käyttötiedot."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Vuokraaja resurssien käyttö Ohjelmointirajapinta

Palvelutili vuokraajan omien resurssien käyttötietojen tarkasteleminen vuokraajan Ohjelmointirajapinnan avulla. Tämä API ovat yhdenmukaisia Azure käyttö Ohjelmointirajapinnan kanssa (käytössä olevien yksityinen esikatselu).

Windows PowerShell-cmdlet-komennon **Get-UsageAggregates** avulla saat käyttötiedot, kuten Azure-tietokannassa.

## <a name="api-call"></a>API-kutsu

### <a name="request"></a>Pyyntö

Pyynnön saa kulutus tiedot pyydetty tilaukset ja pyydetty aikaväli. Ei pyynnön tekstiosaa.

| **Menetelmä**  | **Pyynnön URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HAE         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumentit

| **Argumentti**             | **Kuvaus** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure-pino ympäristön Azure Resurssienhallinta päätepiste. |
| *subId*                   | Tilauksen tunnus käyttäjälle, joka on soittaa. Voit käyttää tätä Ohjelmointirajapinnan vain kyselyn yhden tilauksen käytön. Toimittajien käyttää kyselyn käyttö tarjoajan resurssien käyttö-Ohjelmointirajapinnan kaikki alihallinnat. |
| *reportedStartTime*       | Kyselyn aloitusaika. *DateTime* -arvo on oltava UTC- ja tunti, esimerkiksi 13:00 alussa. Aseta arvoksi päivittäinen kooste keskiyöhön UTC-aika. Muoto on *ohitettuja* ISO 8601, esimerkiksi 2015-06-16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, jossa kaksoispiste on ohitettuja 3 % a ja plus on ohitettuja % 2 b siten, että se on URI helpossa muodossa. |
| *reportedEndTime*         | Kyselyn päättymisaika. Rajoitukset, jotka koskevat *reportedStartTime* koskevat myös tätä argumenttia. *ReportedEndTime* arvo ei voi olla tulevaisuudessa. |
| *aggregationGranularity*  | Valinnainen parametri, joka on erillinen mahdolliset arvoista: päivittäin ja kerran tunnissa. Arvot Ehdota jokin palauttaa tiedot päivittäinen rakeisuuden ja toinen on tunnin välein tarkkuus. Päivittäinen-vaihtoehto on oletusarvo. |
| *subscriberId*            | Tilauksen tunnus. Saat suodatetut tiedot suoraan vuokraajan tarjoajan Tilaustunnus tarvitaan. Jos ei tilauksen tunnus-parametria ei määritetä, puhelun palauttaa kaikki toimittajat suoraan alihallinnat käyttötiedot. |
| *API-versio*             | Protokolla, jota käytetään liittoutumispalvelimien pyyntö versio. Sinun on käytettävä 2015-06-01 – esikatselu. |
| *continuationToken*       | Tunnuksen haetaan viimeksi puhelun käyttö API-palvelulle. Tämä on tarpeen, kun vastauksen on suurempi kuin 1 000 rivit. Tämä on käynnissä kirjanmerkin. Jos niitä ei ole tietoja haetaan päivän alusta tai välitetty tunti, rakeisuuden perusteella. |

### <a name="response"></a>Vastaus

HAE /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00 % 3a00 % 3a00 % 2b00 % 3a00 & aggregationGranularity = päivittäin & api-version = 1.0

{

"arvo":\[

{

"tunnus": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"nimi": "sub1-meterID1"

"tyyppi": "Microsoft.Commerce/UsageAggregate"

"ominaisuudet": {}

"subscriptionId": "sub1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" sijainnin\\":\\" Alaskan\\",\\" tunnisteet\\": null-\\" additionalInfo\\": null}}",

"määrä":2.4000000000

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Vastauksen tiedot

| **Argumentti**      | **Kuvaus** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *tunnus*              | Käyttö koosteen yksilöivä tunnus |
| *Nimi*            | Käyttö koosteen nimi |
| *tyyppi*            | Resurssin määritys |
| *subscriptionId*  | Tilauksen tunnus Azure käyttäjä |
| *usageStartTime*  | UTC-aika aloitusaika, johon tämä käyttö kooste kuuluu käyttö aikajakson |
| *usageEndTime*    | Käyttö aikajakson, johon tämä käyttö kooste kuuluu päättymisaika UTC-aika |
| *instanceData*    | Avain-arvo-pareina esiintymän tiedot (toisessa muodossa):<br>  *resourceUri*: täydellinen Resurssitunnus, mukaan lukien resurssiryhmiä ja esiintymänimi <br>  *sijainti*: alue, johon tämä palvelu on suoritettu <br>  *tunnisteet*: resurssien tunnisteita, joka määrittää käyttäjän <br>  *additionalInfo*: Lisää tiedot resurssista kulutettu, esimerkiksi käyttöjärjestelmän versio tai kuvan tyyppi |
| *määrä*        | Resurssin kulutus tähän aikaväli on tapahtunut määrä |
| *meterId*         | Yksilöllinen tunnus, joka on resurssin kulutettu (myös nimellä, *ResourceID*) |

## <a name="next-steps"></a>Seuraavat vaiheet

[Käyttö liittyvät usein kysytyt kysymykset](azure-stack-usage-related-faq.md)
