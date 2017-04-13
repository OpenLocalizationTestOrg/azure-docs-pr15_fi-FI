<properties
    pageTitle="Azure Government-ohjeet | Microsoft Azure"
    description="Tämä sisältää ominaisuuksia ja ohjeita vertailua Azure Government-sovellusten kehittäminen"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Azure Government hallinta ja suojaus

## <a name="automation"></a>Automaatio

Automaatio on yleisesti saatavilla Azure Government.

### <a name="variations"></a>Variaatiot

Seuraavat automaatio-ominaisuudet eivät ole tällä hetkellä käytettävissä Azure Government.

+ Palvelun tarpeen olevan tunnistetiedon todennustavaksi luominen

Lisätietoja [Automaattiset julkiset asiakirjat](../automation/automation-intro.md).


##  <a name="key-vault"></a>Avaimen säilö
Lisätietoja tämän palvelun ja sen käyttämisestä on artikkelissa <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure avaimen säilö julkisen ohjeissa.</a>
### <a name="data-considerations"></a>Tietojen huomioon otettavia seikkoja
Seuraavassa on esitetty Azure Government rajan Azure avaimen säilö:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kaikki tiedot Azure avaimen säilö avaimella salattujen voivat sisältää Regulated ja hallita tietoja. | Azure avaimen säilö metatietojen ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito avaimen säilö.  Älä kirjoita Regulated ja hallita tiedot seuraaviin kenttiin: resurssien ryhmien nimet, avaimen säilö nimiä, tilauksen nimi |

Avaimen säilö on yleensä käytettävissä Azure Government. Julkinen, kuten ei ilman laajennusta niin avaimen säilö on saatavana PowerShellistä ja CLI vain.
## <a name="log-analytics"></a>Lokitiedoston Analytics
Lokitiedoston Analytics ei yleensä käytettävissä Azure Government. 

### <a name="variations"></a>Variaatiot

Seuraavat Log Analytics ominaisuudet ja ratkaisut eivät ole tällä hetkellä käytettävissä Azure Government. Tämä luettelo päivitetään kun ominaisuudet tilan / ratkaisuja muuttuu.

+ Ratkaisuja, jotka ovat julkisia Azure esikatselussa mukaan lukien:
  - Verkon seuranta ratkaisu
  - Sovelluksen riippuvuus seuranta
  - Office 365-ratkaisuun
  - Windows 10-päivityksen Analytics ratkaisu
  - Hakemuksen tiedot
  - Azure verkko Analytics-ratkaisu
  - Azure automaatio Analytics
  - Avaimen säilö Analytics
+ Ratkaisujen ja ominaisuudet, jotka edellyttävät päivitykset paikallisen-ohjelmistoa, kuten
  - Järjestelmän Center Operations Manager 2016 (Operations Manager aiemmissa versioissa tuetaan) integrointi
  - Tietokoneiden ryhmiä System Center määritysten hallinta
  - Surface keskittimeen ratkaisu
+ Ominaisuudet, jotka ovat julkisia Azure esikatselussa mukaan lukien
  - Vie tiedot PowerBI
+ Azure arvot ja Azure diagnostiikka
+ OMS Mobile-sovellukset

Lokitiedoston Analytics URL-osoitteet ovat erilaisia Azure Government:

| Azure julkisen | Azure Government | Huomautuksia |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Kirjaudu Analytics-portaalissa |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Tietojen kerääminen Ohjelmointirajapinta](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agentti viestintä - [palomuurin asetusten määrittäminen](../log-analytics/log-analytics-proxy-firewall.md) |


Seuraavia Log Analytics-ominaisuuksia on Azure Government eri tavalla:

+ Windows-agentti on ladattava [Log Analytics portal](https://oms.microsoft.us) Azure Government.
+ Muodostaa Log Analytics System Center Operations Manager hallinnan palvelimen sinun on ladattava ja tuoda päivitetyt Management Pack-paketit.
  1. Lataa ja tallenna sitten [päivitetty management Pack-paketit](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Ladatun tiedoston purkaminen
  3. Tuo Operations Manager management Pack-paketit. Lisätietoja siitä, miten voit tuoda hallintapaketin levyltä, on aiheessa [Operations Manager Management Pack tuominen](http://technet.microsoft.com/library/hh212691.aspx) Microsoft TechNet-sivustossa.
  4. Muodostaa Log Analytics Operations Manager ohjeiden [Yhteyden Operations Manager Log Analytics](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

+ Tietojen siirtäminen-loki Analytics-julkinen Azure-Azure Government
  - Ei. Ei ole mahdollista voit siirtää tietoja tai työtilan julkisen Azure-Azure Government.
+ Voit vaihtaa julkisen Azure ja Azure Government työtilat OMS Log Analytics-portaalista välillä?
  - Ei. Julkinen Azure ja Azure Government portaaleihin erilliset ja tietoja ei jaeta. 

Lisätietoja on [lokin Analytics julkisen ohjeissa](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>
 
