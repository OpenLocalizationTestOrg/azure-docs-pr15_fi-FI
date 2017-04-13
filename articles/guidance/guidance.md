
<properties
   pageTitle="Azure ohjeet | kuvioiden ja käytännöt | Microsoft Azure"
   description="Parhaita käytäntöjä ja Azure ohjeita"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure ohjeet

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Microsoft kuvioiden ja käytännöt ryhmän kuuluu Azure asiakkaan neuvoa ryhmän. Microsoftin tarkoitus on auttaa kehittäjät architects ja IT-ammattilaisille onnistuisi Microsoft Azure-ympäristössä. Olemme kehittää, jossa näkyy parhaat käytännöt ratkaisujen cloud Azure ohjeita.

## <a name="checklists"></a>Tarkistusluettelot

Nämä ovat pikaopas tarkistaminen saatavuudesta ja skaalattavuus keskeisiä ominaisuuksia. 

- [Käytettävyys tarkistusluettelo][AvailabilityChecklist] 

    Suositeltuja toimintatapoja käytettävyyden varmistaminen yhteenveto.

- [Skaalattavuus tarkistusluettelo][ScalabilityChecklist]

    Suositeltu käytännöt suunnittelu ja toteuttaminen skaalattava services ja käsittelemisen tietojen hallinta yhteenveto.

## <a name="best-practices-articles"></a>Parhaiden käytäntöjen artikkelit

Seuraavissa artikkeleissa on perusteellisempaa keskustelun tärkeitä käsitteitä yleisesti liittyvät cloud tietojenkäsittely. 

- [API rakenne][APIDesign] 

    Keskustelun verkko-Ohjelmointirajapinnan suunnitellessasi rakenne-ongelmista.

- [API käyttöönotto][APIImplementation] 

    Suositeltu käytännöt voidaan toteuttaa ja julkaisemista verkko-Ohjelmointirajapinnan joukko.

- [API ohjeita](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Keskusteluun vastaaminen, todennus- ja koskee (esimerkiksi suojaustunnuksen tyypit, luvan protokollia, luvan työnkulut ja uhkien rajoitus).

- [Automaattisen skaalauksen poistaminen ohjeita][AutoscalingGuidance] 

    Huomioitavaa skaalaus ratkaisuja ilman manuaalisia toimia ei tarvita yhteenveto.

- [Taustan työt ohjeet][BackgroundJobsGuidance] 

    Kuvaus käytettävissä olevat vaihtoehdot ja toteuttaminen tehtävät, jotka suoritetaan taustalla, itsenäisesti mistä tahansa edustalla tai vuorovaikutteisia toimintoja suositeltu käytännöt.

- [Sisällön toimituksen verkon (CDN) ohjeet][CDNGuidance] 

    Yleiset ohjeet ja Suosituksena sovellustesi kuormitus pienentää ja suurentaa käytettävyys ja suorituskyky CDN avulla.

- [Välimuistin ohjeet][CachingGuidance] 

    Välimuistiin tallentamisen käytöstä parantaa suorituskyky ja skaalattavuus järjestelmän yhteenveto.

- [Tietoja osioimisen ohjeet][DataPartitioningGuidance]

    Strategioita, että voit käyttää osion tietoihin parantamiseksi skaalattavuus, pienennä ristiriita ja suorituskyvyn parantamiseksi.

- [Seuranta- ja diagnostiikka-ohjeet][MonitoringandDiagnosticsGuidance] 

    Ohjeita siitä, miten käyttäjien käyttämiseen järjestelmän, jäljittää resurssien käyttö ja valvoa yleensä kunto ja järjestelmän suorituskykyä seuranta.

- [Suositeltu nimeämiskäytännöt][naming-conventions] 

    Suositeltava nimeämiskäytännöt Azure resurssien.

- [Yritä yleisohjeita][RetryGeneralGuidance] 

    Keskustelun Yleiset käsitteitä käsittelyyn lyhytkestoisia virheitä.

- [Yritä palvelukohtaisia ohjeet][RetryServiceSpecificGuidance]

    Monia Azure palveluja, kuten tietoja, joiden avulla voit käyttää, muokata tai laajentaa palvelun uudelleen-järjestelmä uudelleen ominaisuuksien yhteenveto.

## <a name="scenario-guides"></a>Skenaario apuviivat

- [Azure Elasticsearch käytössä][elasticsearch] 
    
    Elasticsearch on erittäin skaalattava Avaa lähde hakukone ja tietokanta. Se sopii tilanteita, jotka edellyttävät nopea analysointi ja etsiminen on paljon tietoja. Nämä ohjeet tarkastelee avaimen tietyiltä Elasticsearch-klusterin suunnitellessasi.

- [Multitenant sovellusten jäsenyyksien hallinta][identity-multitenant] 
    
    Multitenancy on arkkitehtuuri jossa useita alihallinnat jakaa sovelluksen, mutta eristetään toisistaan. Näiden ohjeiden avulla voit hallita käyttäjätietojen multitenant sovelluksessa [Azure Active Directoryn] avulla[ AzureAD] käsittelemään Kirjaudu sisään- ja todennusta.
    
- [Big datasta, ratkaisujen kehittämiseen](https://msdn.microsoft.com/library/dn749874.aspx)

    Tässä oppaassa käsitellään HDInsight käytön skenaariot, kuten iteratiivinen siihen tarkemmin kuin tietovarasto ETL prosessit ja integroida aiemmin BI-järjestelmiä. Se on myös tietoa big datasta, suunnittelu ja suunnittelemisesta big datasta ratkaisuja käsitteitä ja ratkaisujen käyttöönoton ohjeita.
    
## <a name="patterns"></a>Kuviot

- [Cloud rakenne kuvioita: Saat arkkitehtuuri ohjeet Cloud-sovellukset](https://msdn.microsoft.com/library/dn568099.aspx)

    Cloud rakenne kuvioita on valikoiman rakenne kuvioiden ja Aiheeseen liittyvät ohjeaiheet. Se articulates käyttöönottoa kuviot näyttämällä, miten kutakin mahtuvien cloud sovelluksen arkkitehtuureihin etu.
    
- [Cloud sovellusten suorituskyvyn optimoiminen](https://github.com/mspnp/performance-optimization)

    Nämä ohjeet on Yleiset Anti mallit, joka estää kuormitus skaalaus-sovelluksia tarkasteluun. Se sisältää esimerkkejä, joiden osoittaa kahdeksan Anti kuvioiden ja [suorituskyvyn analysointi askeleet](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) ja opas oppilaitosten [arvioidaan suorituskykyä vastaan avaimen arvot](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Arkkitehtuureihin viitteen

Tutustu viittaus arkkitehtuureihin järjestetään skenaarion mukaan.
Kunkin yksittäisiä arkkitehtuuri tarjoaa suositellut käytännöt ja saat ohjeita ja suoritettavan tiedoston osa, joka embodies suositukset.

Arkkitehtuureihin viitteen nykyisen kirjasto on saatavilla kohdassa [http://aka.ms/architecture](http://aka.ms/architecture).

## <a name="resiliency-guidance"></a>Vikasietoisuudelle ohjeet

Seuraavissa artikkeleissa kerrotaan, kuinka voit suunnitella sovelluksia, jotka ovat joustavat virheen hajautettu cloud-ympäristössä.   

- [Vikasietoisuudelle yleiskatsaus][ResiliencyOvervew]

     Voit luoda sovelluksia Azure-ympäristössä, joka palauttaa virheet ja toimivat. Tässä artikkelissa Jäsennettyjen lähestymistapa saavuttamiseksi vikasietoisuudelle rakenteesta toteutusta käyttöönoton ja toiminnot.

- [Vikasietoisuudelle tarkistusluettelo][resiliency-checklist]

    Tarkistusluettelon suosituksia, joiden avulla voit suunnitella virheen tilat, joita saattaa ilmetä eri.

- [Virhe tilassa analyysi][resiliency-fma] 

    Virhe tilassa analysointi (FMA) on prosessin etsimisen vikasietoisuudelle järjestelmään, tunnistamalla mahdollista virheen pistettä. Tässä artikkelissa on mahdollisen virheen tilasta luetteloon ja niiden ongelman pienentämistavat FMA prosessin-kohdan. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

