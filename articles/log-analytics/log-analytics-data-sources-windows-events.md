<properties 
   pageTitle="Windowsin tapahtumalokiin kirjautuu Log Analytics | Microsoft Azure"
   description="Windowsin tapahtumalokien ovat yksi loki Analytics käyttämiä yleisimmät tietolähteitä.  Tässä artikkelissa käsitellään määrittäminen Windowsin tapahtumalokien sivustokokoelman ja he luovat OMS säilössä tietueiden tietoja."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windowsin tapahtumalokiin tietolähteitä, loki Analytics

Windowsin tapahtumalokien ovat yksi käytettävät Windows agenttien vuoksi, koska tämä on tapa, jolla tiedot ja virheet kirjautua useimpien sovellusten yleisimmät [tietolähteet](log-analytics-data-sources.md) .  Voit kerätä tapahtumat tavallisen lokien järjestelmän ja sovellukset lisäksi kaikki mukautetut lokit haluat valvoa sovellusten luomien määrittäminen.

![Windows-tapahtumat](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Määrittäminen Windows-tapahtumalokit

Määritä Windowsin tapahtumalokien [Lokiasetukset Analytics-tiedot-valikko](log-analytics-data-sources.md#configuring-data-sources).

Log Analytics vain kerää tapahtumat-Windowsin tapahtumalokien, jotka on määritetty asetuksia.  Voit lisätä uuden lokin kirjoittamalla luku Kirjoita lokitiedoston nimi ja valitse **+**.  Kunkin lokin kerätään vain valitun severities tapahtumat.  Tarkista tietyn lokin, jotka haluat kerätä severities.  Et voi määrittää lisää ehtoja tapahtumien suodattaminen.

![Windowsin tapahtumien määrittäminen](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Tietojen kerääminen

Lokitiedoston Analytics kerää kuhunkin tapahtumaan, joka vastaa valitun vakavuus valvottu tapahtumalokista, samalla luodaan tapahtuma.  Agentti tallentaa sijaintia jokaisen tapahtumalokin, se kerää.  Jos agentti menee offline-tilaan tietyn ajanjakson aikana, valitse Log Analytics kerää tapahtumat-kohtaa, johon se viimeksi jäi, vaikka tapahtumat on luotu agentti ollessa offline-tilassa.


## <a name="windows-event-records-properties"></a>Windowsin tapahtumalokiin tietueiden ominaisuudet

Windows tapahtumatiedot **tapahtuman** tyyppi, ja se on ominaisuudet seuraavassa taulukossa.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tietokoneen            | Tietokoneeseen, jossa tapahtuma on kerätty nimi. |
| EventCategory       | Tapahtuman luokka. |
| EventData           | Kaikki tapahtuman tiedot raaka-muodossa. |
| EventID             | Tapahtuman määrä. |
| EventLevel          | Numeerinen lomakkeen tapahtuman vakavuus. |
| EventLevelName      | Tapahtuman tekstimuodossa vakavuus. |
| Tapahtumaloki            | Tapahtumaloki, tapahtuma on kerätty nimi. |
| ParameterXml        | Tapahtuman parametriarvot XML-muodossa. |
| ManagementGroupName | SCOM tekijöiden hallinta-ryhmän nimi.  Muiden tekijöiden tämä on AOI-<workspace ID> |
| RenderedDescription | Tapahtuman kuvaus parametrin arvot |
| Lähde              | Tapahtuman lähde. |
| SourceSystem  | Edustajan tapahtuman tyyppi on kerätty. <br> Yhdistä OpsManager – Windows-agentti, joko suoraan tai SCOM <br> Linux – kaikki Linux agenttien vuoksi  <br> AzureStorage – Azure diagnostiikka |
| TimeGenerated       | Päivämäärä ja kellonaika Windowsissa on luotu tapahtuma. |
| Käyttäjänimi            | Tapahtuman kirjannut tilin käyttäjänimi. |



## <a name="log-searches-with-windows-events"></a>Log hakuja ja Windows-tapahtumat

Seuraavassa taulukossa on eri esimerkkejä log hakuja, jotka hakevat Windowsin tapahtumalokiin tietueita.

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = tapahtuma | Kaikki Windows-tapahtumat. |
| Tyyppi = tapahtuman EventLevelName = virhe | Kaikkien Windowsin tapahtumien virheen vakavuus. |
| Tyyppi = tapahtuman & #124; Mittaa count() lähteen mukaan | Laske Windowsin tapahtumien lähteen mukaan. |
| Tyyppi = tapahtuman EventLevelName = virhe & #124; Mittaa count() lähteen mukaan | Laske Windows virhe tapahtumien lähteen mukaan. |

## <a name="next-steps"></a>Seuraavat vaiheet

- Määritä lokin Analytics kerätä analyysi muista [tietolähteistä](log-analytics-data-sources.md) .
- Lisätietoja [lokin haut](log-analytics-log-searches.md) tietolähteet ja ratkaisujen kerättyjen tietojen analysointia varten.  
- [Mukautettujen kenttien](log-analytics-custom-fields.md) avulla voit tapahtumatietueet jäsentää yksittäisiä kenttiä.
- Määritä [sivustokokoelman suorituskyvyn laskureita](log-analytics-data-sources-performance-counters.md) Windows edustajasi.