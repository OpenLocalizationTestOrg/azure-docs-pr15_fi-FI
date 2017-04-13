<properties
    pageTitle="Lokitiedoston Analytics yleiskatsaus Azure tallennustilan tietojen keräämisen | Microsoft Azure"
    description="Azure resurssien voit kirjoittaa lokit ja arvot Azure-tallennustilan tilin usein Azure Diagnostiikan avulla. Log Analytics indeksoida tiedot ja tee etsittävän."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Azure-tallennustilan tiedonkeräyksen Log Analytics yleiskatsaus

Monta Azure resurssit ovat kirjoittaa Azure-tallennustilan tilin lokit ja arvot. Log analyysin voit käyttää näitä tietoja ja tiedostojasi on helpompi seurata Azure resurssit.

Kirjoittaminen Azure tallennustilan resurssi voi käyttää Azure diagnostiikka tai on oma tapa kirjoittaa tietoja. Nämä tiedot voidaan kirjoittaa erimuotoisia johonkin seuraavista sijainneista:

+ Azure-taulukosta
+ Azure-blob
+ EventHub

Lokitiedoston Analytics tukee Azure palvelua, joiden avulla [Azure vianmäärityslokit](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)tietojen kirjoittaminen. Lisäksi loki Analytics tukee palvelut, joita tulosteen lokit ja arvot eri muodoissa ja sijainnit.  

>[AZURE.NOTE] Peritään Normaali Azure tietojen korvaukset tallennus-ja tapahtumat, kun lähetät diagnostiikka tallennustilan tilin ja kun lokiin Analytics lukee tiedot tallennustilan-tililtä.

![Azure-tallennustilan kaavio](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Tuetut Azure resurssit

Lokitiedoston Analytics voi kerätä tietoja seuraavat Azure resurssien:

| Resurssilaji | Lokit (diagnostiikan luokat) | Lokitiedoston Analytics ratkaisu |
| --------------------------------------- | -------------------------------- | --------------- |
| Hakemuksen tiedot | Käytettävyys <br> Mukautetut tapahtumat <br> Poikkeukset <br> Palvelupyynnöt <br> | Hakemuksen tiedot (ennakkoversio) |
| API hallinta | | *ei mitään* (Ennakkoversio) |
| Automaatio <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (ennakkoversio) |
| Avaimen säilö <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (ennakkoversio) |
| Sovelluksen yhdyskäytävän <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (ennakkoversio) |
| Verkko-käyttöoikeusryhmän <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (ennakkoversio) |
| Palvelun kangasta                          | ETWEvent <br> Toiminnallisia tapahtuma <br> Luotettavan toimija-tapahtuma <br> Luotettavan palvelun tapahtumaa| ServiceFabric (ennakkoversio) |
| Näennäiskoneiden | Linux Syslog <br> Windowsin tapahtumalokiin <br> IIS-lokiin <br> Windows-ETWEvent | *ei mitään* |
| Web-roolit <br> Työntekijän roolit | Linux Syslog <br> Windowsin tapahtumalokiin <br> IIS-lokiin <br> Windows-ETWEvent | *ei mitään* |

>[AZURE.NOTE] Seurannan Azuren näennäiskoneiden (Linux ja Windows), suosittelemme asentaminen [Log Analytics AM tunniste](log-analytics-azure-vm-extension.md). Agentti on tarkempaa kuin käytettäessä kirjoitettu tallennustilan diagnostiikka virtuaalisten laitteiden tietoja.

Voit Auta meitä priorisoida OMS analysoi äänestäminen usealle [palautesivu](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)muita lokit.


- Katso lisätietoja siitä, miten Log Analytics voivat lukea lokit Azure Services-palveluista, jotka tukevat [Azure vianmäärityslokit](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) [analysoida Azure vianmäärityslokit Log-analyysin avulla](log-analytics-azure-storage-json.md) :
  - Azure avaimen säilö
  - Azure automaatio
  - Sovelluksen yhdyskäytävän
  - Verkon käyttöoikeusryhmät
- [Käytä Blob-objektien tallennustilaan IIS ja tapahtumien taulukkotallennus](log-analytics-azure-storage-iis-table.md) lisätietoja kuinka loki Analytics voivat lukea lokit varten Azure services, kirjoita diagnostiikka taulukkotallennus tai IIS-lokeja kirjoitettu blob storage, mukaan lukien:
  - Palvelun kangasta
  - Web-roolit
  - Työntekijän roolit
  - Näennäiskoneiden


Sovelluksen tiedot on yksityinen esikatselu ja se käyttää blob storage jatkuva Vie. Liittyminen yksityinen esikatselu, ota yhteyttä Microsoft-Account ryhmän tai viitata [palautetta sivuston](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights)tiedot.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Log-analyysin avulla analysoida Azure vianmäärityslokit](log-analytics-azure-storage-json.md) lokit lukea Azure services, kirjoita vianmääritys ja Blob-objektien tallennustilaan JSON-muodossa.
- [Käytä Blob-objektien tallennustilaan IIS ja tapahtumien taulukkotallennus](log-analytics-azure-storage-iis-table.md) Lue lokit Azure services, kirjoita diagnostiikka taulukkotallennus tai kirjoittaa blob storage IIS-lokeja.
- [Ota käyttöön ratkaisuja](log-analytics-add-solutions.md) selventäminen tiedot.
- [Käytä hakukyselyt](log-analytics-log-searches.md) tietojen analysoimista varten.
