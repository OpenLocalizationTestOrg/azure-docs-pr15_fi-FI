<properties
    pageTitle="Yleistä Azure diagnostiikka | Microsoft Azure"
    description="Käytä virheenkorjaus mittaaminen suorituskyvyn, seurannan, liikenne analyysin pilvipalveluihin, näennäiskoneiden ja palvelun kangasta Azure diagnostiikka"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Mikä on Microsoft Azure diagnostiikka


Azure Diagnostiikka on sisällä Azure, ottaa käyttöön sovelluksen diagnostiikan tiedot sivustokokoelman ominaisuuksien. Voit käyttää diagnostiikan tunniste useista eri lähteistä. Tällä hetkellä tueta ovat Azure Cloud Service Web ja työntekijä roolit näennäiskoneiden Azure Microsoft Windows ja Service kangasta. Azure muiden on omat erilliset Diagnostiikka.

## <a name="data-you-can-collect"></a>Voit kerätä tietoja

Azure diagnostiikka voi kerätä tietoja seuraavanlaisia:

Tietolähde|Kuvaus
---|---
Suorituskyvyn laskureita | Käyttöjärjestelmän ja mukautetun suorituskyvyn laskureita
Sovelluksen lokit     | Jäljitä sovelluksen kirjoittamat viestit
Windowsin tapahtumalokien   | Windows-tapahtuman kirjaaminen järjestelmän sovellukseen lähetettyjä tietoja
.NET Tapahtumalähde    | Koodin kirjoittamisen tapahtumien.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) -luokan avulla
IIS-lokit             | Tietoja IIS-sivustot
Luettelo perusteella tapahtumien seuranta   | Tapahtuman jäljitys Windowsin tapahtumien luomat prosessit
Kaatumisvedokset          | Jos sovelluksen kaatumisen prosessin tilatietoja
Mukautetun virhelokeja    | Sovelluksen tai palvelun luomat lokit
Azure diagnostiikan infrastruktuurin lokit|Tietoja itse diagnostiikka

Azure diagnostiikka-tunniste voit siirtää nämä tiedot Azure-tallennustilan tilin tai lähettää sen [Sovelluksen tietoja](./application-insights/app-insights-cloudservices.md)palveluita. Voit käyttää tietojen virheiden vianmääritys, mittaaminen suorituskyvyn, seurannan resurssien käyttö, liikenne analysointia ja kapasiteetin suunnittelu ja seuranta.


## <a name="versioning"></a>Versiotiedot
Katso [Azure diagnostiikka versiotietojen historia](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Valitse mikä palvelu, jonka haluat kerätä diagnostiikka ja aloita on seuraavissa artikkeleissa avulla. Yleiset Azure diagnostiikka linkkien avulla Reference for tiettyihin tehtäviin.

## <a name="web-apps"></a>Web Apps-sovelluksista
Huomaa, että verkkosovelluksissa ei käyttää Azure Diagnostiikka. Vastaavat tiedot etsiminen [Web Apps -sovelluksista](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Pilvipalveluihin Azure Diagnostiikan avulla
- Jos Visual Studiossa, katso [Käytä Visual Studio jäljittää Cloud Services-sovelluksen](./vs-azure-tools-debug-cloud-services-virtual-machines.md) avulla pääset alkuun. Muussa tapauksessa tarkastella
- [Voit valvoa pilvipalveluihin Azure Diagnostiikan avulla](./cloud-services/cloud-services-how-to-monitor.md)
- [Azure diagnostiikka Cloud Services-sovelluksen määrittäminen](./cloud-services/cloud-services-dotnet-diagnostics.md)

Katso vaativampia aiheita

- [Hakemuksen tiedot Azure diagnostiikka käyttäminen pilvipalveluihin](./application-insights/app-insights-cloudservices.md)
- [Cloud Services-sovelluksen Azure vianmäärityksen etenemisen seuranta](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [Määritä diagnostiikka Cloud Services-PowerShellin avulla](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Näennäiskoneiden Azure Diagnostiikan avulla
- Jos Visual Studiossa, katso [Käytä Visual Studio jäljittää Azuren näennäiskoneiden](./vs-azure-tools-debug-cloud-services-virtual-machines.md) avulla pääset alkuun. Muussa tapauksessa tarkastella
- [Azure diagnostiikka Azure Virtual Machine määrittäminen](./virtual-machines-dotnet-diagnostics.md)

Katso vaativampia aiheita

- [Määritä diagnostiikka-Azuren näennäiskoneiden PowerShellin avulla](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Luo Windows Virtual machine seuranta- ja diagnostiikka Azure Resurssienhallinta mallin avulla](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Palvelun kangasta Azure Diagnostiikan avulla
Aloita [näytön palvelun kangasta-sovellusta](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)osoitteessa. Monia muita palvelun kangasta diagnostiikka artikkeleita ovat käytettävissä siirtymisrakenteessa vasemmalla, kun saat tässä artikkelissa.

## <a name="general-azure-diagnostics-articles"></a>Yleinen Azure diagnostiikka artikkelit
- [Azure diagnostiikka rakenteen määritys](https://msdn.microsoft.com/library/azure/mt634524.aspx) - rakennetiedoston voit kerätä ja reitittää diagnostiikka tietojen vaihtamiseen. Huomaa, että voit myös käyttää Visual Studio rakennetiedoston muuttamiseen.
- [Miten tiedot tallennetaan Azuren tallennustilaan Azure-diagnostiikka](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - tietää nimiä, taulukoita ja BLOB-objektit, joissa tietoja kirjoitetaan.
- Opi käyttämään [Suorituskyvyn joukon Azure diagnostiikka](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Katso [reitti Azure diagnostiikka tiedot sovelluksen havainnollistamisen](./azure-diagnostics-configure-applicationinsights.md)
- Jos sinulla on vaikeuksia vianmäärityksen käynnistämisestä ja tietojen etsiminen Azuren tallennustilaan taulukoiden, katso [Vianmääritys Azure diagnostiikka](./azure-diagnostics-troubleshooting.md)
