<properties
    pageTitle="Azure Government-ohjeet | Microsoft Azure"
    description="Tämä on vertailu ominaisuuksista ja ohjeita Azure Government sovellusten kehittämisestä."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Azure Government valvonnan ja hallinnan

Tässä artikkelissa käsitellään seuranta ja hallinta-palveluiden variaatiot ja huomioon otettavia seikkoja Azure Government-ympäristössä.

## <a name="automation"></a>Automaatio

Automaatio on yleisesti saatavilla Azure Government.

### <a name="variations"></a>Variaatiot

Seuraavat automaatio-ominaisuudet eivät ole tällä hetkellä käytettävissä Azure Government.

+ Palvelun tarpeen-credential todennustavaksi luominen

Lisätietoja [Automaattiset julkiset asiakirjat](../automation/automation-intro.md).

## <a name="log-analytics"></a>Lokitiedoston Analytics

Lokitiedoston Analytics ei yleensä käytettävissä Azure Government.

### <a name="variations"></a>Variaatiot

Seuraavat Log Analytics ominaisuudet ja ratkaisut eivät ole tällä hetkellä käytettävissä Azure Government.

+ Ratkaisuja, jotka ovat esikatselussa Microsoft Azure, mukaan lukien:
  - Verkon seuranta ratkaisu
  - Sovelluksen riippuvuus seuranta ratkaisu
  - Office 365-ratkaisuun
  - Windows 10-päivityksen Analytics ratkaisu
  - Hakemuksen tiedot ratkaisu
  - Azure verkko Analytics-ratkaisu
  - Azure automaatio Analytics-ratkaisu
  - Avaimen säilö Analytics ratkaisu
+ Ratkaisujen ja ominaisuudet, jotka edellyttävät päivitykset paikallisen-ohjelmistoa, kuten:
  - Järjestelmän Center Operations Manager 2016 (Operations Manager aiemmissa versioissa tuetaan) integrointi
  - Tietokoneiden ryhmät-System Center määritysten hallinta
  - Surface keskittimeen ratkaisu
+ Ominaisuudet, jotka ovat julkisia Azure esikatselussa mukaan lukien:
  - Vie tiedot Power BI
+ Azure mittarit ja Azure diagnostiikka
+ Toimintojen hallinta Suite mobile-sovellukset

Lokitiedoston Analytics URL-osoitteet ovat erilaisia Azure Government:

| Azure julkisen | Azure Government | Huomautuksia |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Kirjaudu Analytics-portaalissa |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Tietojen kerääminen Ohjelmointirajapinta](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |


Seuraavat Log Analytics-ominaisuudet toimivat eri tavalla Azure Government:

+ Windows-agentti on ladattava [Log Analytics portal](https://oms.microsoft.us) Azure Government.
+ Muodostaa Log Analytics System Center Operations Manager hallinnan palvelimen sinun on ladattava ja tuoda päivitetyt management Pack-paketit.
  1. Lataa ja Tallenna [päivitetty management Pack-paketit](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Pura lataamasi tiedosto.
  3. Tuo Operations Manager management Pack-paketit. Lisätietoja management pack tuominen DVD-levyllä, katso, [miten voit tuoda Operations Manager Management Pack](http://technet.microsoft.com/library/hh212691.aspx) Microsoft TechNet-sivustossa.
  4. Muodostaa Log Analytics Operations Manager ohjeiden [Yhteyden Operations Manager Log Analytics](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

+ Voit voin siirtää tietoja Microsoft Azure-loki Analytics Azure Government?
  - Ei. Ei voi siirtää tietoja tai työtilan Microsoft Azure Azure Government.
+ Voit vaihtaa Microsoft Azure ja Azure Government välillä työtilat toimintojen hallinta Suite Log Analytics-portaalista?
  - Ei. Microsoft Azure ja Azure Government portaaleihin erilliset ja tietoja ei jaeta.

Lisätietoja on [lokin Analytics julkisen ohjeissa](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>
