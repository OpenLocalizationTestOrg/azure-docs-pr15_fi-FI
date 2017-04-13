<properties
   pageTitle="Toimintojen hallinta Suite (OMS) yleiskatsaus | Microsoft Azure"
   description="Microsoft toimintojen hallinta Suite (OMS) on Microsoftin pilvipohjainen IT hallintaratkaisu, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuuria.  Tässä artikkelissa on eri palvelut sisältyvät OMS ja linkkejä yksityiskohtaiset niiden sisältöön."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Mikä on toimintojen hallinta Suite (OMS)?

Microsoft toimintojen hallinta Suite (OMS) on Microsoftin pilvipohjainen IT hallintaratkaisu, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuuria.  Koska OMS on toteutettu pilvipohjaisia palveluja, voit määrittää sen käytön nopeasti infrastruktuuripalvelut mahdollisimman vähän sijoituksen kanssa.  Uudet ominaisuudet toimitetaan automaattisesti tallentaminen jatkuvaa ylläpitoa ja Päivitä kustannukset.

Lisäksi arvokkaita palveluja tarjoavan omalla, OMS integroida System Center-osia, kuten System Center Operations Manager siirtämisestä oman aiemmin hallinta sijoitukset pilveen kyselyjä.  System Center ja OMS voivat käyttää yhdessä antamaan koko hybrid hallintatoiminnot.

Seuraavissa osissa on eri arvon alueet OMS ja palvelut, jotka toteuttavat ne korkean tason kuvaus.  Voit viitata OMS arkkitehtuuri eri OMS-osien yleiskatsaus ennen tarkistaminen yksityiskohtaiset ohjeissa.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Tietoja ja analysoinnin](media/operations-management-suite-overview/icon-insight-analytics.png) Tietoja ja analysoinnin

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) avulla voit kerätä, yhdistää, Etsi ja toimia käyttöjärjestelmien ja sovellusten luomat log ja suorituskyvyn tiedot. Tällä tavalla saat reaaliaikainen toiminnallisia havainnollistamisen integroitu haku- ja mukautettu raporttinäkymien käyttäminen haluat analysoida helposti tietueiden miljoonia kaikista toiminnoista ja palvelinten niiden fyysinen paikasta riippumatta.

Voit helposti lisätä ratkaisuja Log-analyysin, jotka määrittävät kerättävät tiedot ja sen analysointi logiikan.  Ratkaisut voivat lisätä lisätoimintoja, joka lähetetään automaattisesti tekijöiden kanssa mahdollisimman vähän tai ei ole määritys.  Lisäksi yksittäisiä ratkaisuja myöntämä Analyysityökalut, voit suorittaa hakuja mukautettu koko tietojoukko yli jotta voit yhdistää tietojen, järjestelmien ja sovellusten välillä.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automaatio ja hallinta](media/operations-management-suite-overview/icon-automation-control.png) Automaatio ja hallinta

Azure automaatio automatisoi järjestelmänvalvojan prosessien havainnollistaminen [runbooks](../automation/automation-runbook-types.md) , jotka perustuvat PowerShell ja suorita Azure pilveen.  Runbooks käyttää tuotetta tai palvelua, joka voidaan hallita PowerShellin resurssien mukaan lukien muiden paveikslėlis, kuten Amazon Web Services (sisältyy AWS).  Runbooks voi suorittaa myös paikallisen tietokeskuksen hallittavan paikalliset resurssit-palvelimessa.

Azure automaatio on [PowerShell DSC](../automation/automation-dsc-overview.md)määritysten hallinta.  Voit luoda ja hallita DSC resursseja ylläpidettävä Azure ja koske cloud ja paikallisen järjestelmien määrittäminen ja Säilytä automaattisesti niiden kokoonpanon.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Suojaus ja palauttaminen](media/operations-management-suite-overview/icon-protection-recovery.png) Suojaaminen ja palauttaminen

[Azure varmuuskopiointi](http://azure.microsoft.com/documentation/services/backup) suojaa sovelluksen tietoja ja säilyttää vuotta ilman mitään pääoman sijoitus ja mahdollisimman vähän kustannuksia.  Se voi varmuuskopioida tiedot fyysinen ja virtuaalinen Windows-palvelimien lisäksi sovelluksen toiminnoista, kuten SQL Serverin ja SharePoint.  Se voidaan myös mukaan System Center tietojen suojauksen hallinta (DPM) replikointia suojattuja tietoja Azure redundancy ja tallennustilaa pitkällä aikavälillä.

[Azure palauttaminen](http://azure.microsoft.com/documentation/services/site-recovery) prosentuaalista osuutta Liiketoiminnan jatkuvuus ja tietojen palauttaminen (BCDR) strategia mukaan orchestrating replikoinnin automaattisesti ja paikallisen Hyper-V näennäiskoneiden, VMware näennäiskoneiden ja fyysinen Windows-tai Linux-palvelimiin. Voit toistaa koneet toissijainen keskelle tai laajentaminen data Centerissä replikoiminen ne Azure. Sivuston palautus on myös yksinkertainen automaattisesti ja palautus toiminnoista. Tietojen palauttaminen-järjestelmiä, kuten SQL Server AlwaysOn integroituu ja tarjoaa palautus suunnitelmien toiminnoista, jotka ovat tasoisen useita tietokoneissa helposti automaattisesti.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS tietosuojaan ja vaatimustenmukaisuuteen](media/operations-management-suite-overview/icon-security-compliance.png) Turvallisuus ja vaatimustenmukaisuus
Tietosuojaan ja vaatimustenmukaisuuteen avulla voit tunnistaa, arvioida ja hallita suojausriskejä, infrastruktuurin.  Näitä toimintoja OMS otettu käyttöön useita ratkaisuja Log Analytics, analysoida tietoja ja määritysten agentti järjestelmistä, voit helpottaa varmistaa, että ympäristön jatkuva suojaus.

- [Suojaus ja valvonta-ratkaisun](oms-security-getting-started.md ) kerää ja analysoi hallittujen järjestelmien tunnistavan epäilyttävistä tehtävän tapahtumiin suojaus.
- Hallittujen järjestelmien antimalware suojauksen tilan [Antimalware ratkaisu](log-analytics-malware.md ) -raportit.  
- Järjestelmäpäivitykset-ratkaisun suorittaa analyysi päivitykset ja muut päivittää hallittujen järjestelmien niin, että voit helposti tunnistaa järjestelmien edellyttävän päivitetään.


## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja [lokin Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
- Lisätietoja [Azure automaatio](../automation/automation-intro.md).
- Lisätietoja [Azure varmuuskopion](http://azure.microsoft.com/documentation/services/backup).
- Lisätietoja [Azure palauttaminen](http://azure.microsoft.com/documentation/services/site-recovery).
