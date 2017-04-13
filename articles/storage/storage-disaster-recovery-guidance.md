<properties
    pageTitle="Mitä tehdä, jos Azuren tallennustilaan käyttökatkosta | Microsoft Azure"
    description="Mitä tehdä, jos Azuren tallennustilaan käyttökatkosta"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Jos ilmenee Azuren tallennustilaan käyttökatkosta

Microsoftilla on toimivat kiintolevyn ja varmista, että palveluiden ovat aina käytettävissä. Joskus pakottaa lisäksi Microsoftin ohjausobjektin vaikutus us tavoilla, jotka aiheuttavat suunnittelematon palvelukatkoksia yhteen tai useampaan. Avulla voit käsitellä nämä harvinaisissa esiintymien annamme Azuren tallennustilaan services seuraavat korkean tason lisäohjeita.

## <a name="how-to-prepare"></a>Valmistautuminen 

On ehdottoman jokaisen asiakkaan valmisteleminen omien tietojen palauttaminen suunnitelma. Palauttaa yleensä tallennustilan käyttökatkosta työmäärään liittyy toimintojen henkilöstön ja automaattinen menettelyt Aktivoi sovellustesi toimivassa tilassa, jotta. Lue alla voit luoda omat tietojen palauttaminen suunnitelman Azure ohjeissa:

-   [Tietojen palauttaminen ja Azure sovellusten suuri käytettävyys](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Azure vikasietoisuudelle tekniset ohjeet](../resiliency/resiliency-technical-guidance.md)

-   [Azure palauttaminen-palvelu](https://azure.microsoft.com/services/site-recovery/)

-   [Azure tallennustilan sallittuja](storage-redundancy.md)

-   [Azure varmuuskopiointi-palvelu](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Tunnistaminen 

Azure-palvelun tilan myös tilata [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/)avulla.

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Jos tallennustilaa käyttökatkosta tapahtuu

Jos vähintään yksi liittyviä palveluja on tilapäisesti poissa käytöstä yhteen tai useampaan, sinun on kaksi vaihtoehtoa: Voit ottaa huomioon. Jos heti käyttöösi haluaa tietojen, harkitse vaihtoehto 2.

### <a name="option-1-wait-for-recovery"></a>Vaihtoehto 1: Odota palauttaminen

Tässä tapauksessa osaltasi mitään toimia ei tarvita. Yritämme apuväline luotaessa joukkopostituksia palauttaa Azure palvelun saatavuus. Voit valvoa [Azure palvelun kunnon koontinäyttö](https://azure.microsoft.com/status/)palvelutila.

### <a name="option-2-copy-data-from-secondary"></a>Vaihtoehto 2: Tietojen kopioiminen toissijainen

Jos valitsit [lukuoikeudet geo tarpeettomat storage (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (suositus) tallennustilan-tileissä, sinun on lukuoikeudet tietojen toissijainen alueelta. Voit käyttää työkaluja, kuten [AzCopy](storage-use-azcopy.md), [PowerShellin Azure](storage-powershell-guide-full.md)ja [Azure tietojen siirto kirjaston](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) tietojen kopioiminen toisen alueen toisen tallennustilan tilin unimpacted alueella ja valitse sitten sovellusten tallennustilan tiliin sekä lukeminen ja kirjoittaminen käytettävyys.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Toiminta, jos tallennustila-vikasietotila ilmenee

Jos valitsit [Geo tarpeettomat tallennus (GRS)](storage-redundancy.md#geo-redundant-storage) - tai [lukuoikeudet geo tarpeettomat storage (RA GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (suositus), Azuren tallennustilaan säilyttää tietosi kestävät kummallakin alueella (ensisijainen ja toissijainen). Molemmat alueet Azuren tallennustilaan ylläpitää jatkuvasti useita replikoita koskevista tiedoista.

Kun alueellisen huono vaikuttaa ensisijainen alue, on ensin yrittää palauttaa alueen palvelun. Riippuvainen laatu tietojen ja sen vaikutukset harvinaisissa tästä huolimatta: emme ei ehkä voi palauttaa ensisijainen alue. Olemme senhetkinen, suorittaa geo-automaattisesti. Usean alueen tiedot replikointi on asynkroninen prosessi, joka voi liittyä viiveen, joten se on mahdollista, että muutokset, joita ei vielä ole replikoida toissijainen alueen saatetaan menettää. Voit tehdä kyselyn ["Edellisen synkronoinnin aika-tallennustilan tilin](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) saat yksityiskohtaiset tiedot replikoinnin tila.

Muutama koskeva tallennustilan geo automaattisesti kokemus asioista:

-   Tallennustilan geo-automaattisesti käynnistyy vain Azuren tallennustilaan työryhmän – ei asiakkaan toimia ei tarvita.

-   Oman olemassa olevan tallennustilan Palvelupäätepisteet BLOB-objektit, taulukoita tai olevien tiedostojen pysyvät ennallaan vikasietotilaa; jälkeen DNS-merkintä on päivitettävä, jotta siirtyminen ensisijainen alueen toissijainen alue.

-   Ennen ja aikana geo-vikasietotilaa kirjoitusoikeudet ei tarvitse tallennustilan tilin vuoksi tietojen vaikutukset, mutta voit kuitenkin lukea toissijaisen Jos tallennustilan-tilisi on määritetty RA GRS.

-   Kun DNS-muutokset välitetty geo-vikasietotilaa on suoritettu loppuun, luku- ja kirjoitusoikeudet tallennustilan tiliisi jatketaan. Voit tehdä kyselyn ["Geo automaattisesti viimeksi-tallennustilan tilin](https://msdn.microsoft.com/library/azure/ee460802.aspx) tarkempia.

-   Jälkeen vikasietotilaa-tallennustilan tilin oltava toimiva, mutta "eivät toimi oikein" tila, sellaisena kuin se on todella ylläpidettävä erillinen-alue, jossa geo replikointia ei ole mahdollista. Riskin pienentämään olemme Palauta alkuperäinen ensisijainen alue ja tee sitten geo-tuntisesta Palauta alkuperäiseen tilaan. Jos alkuperäisen ensisijainen alue on vakava, emme jakaa toisen toissijainen alueen.
Lue lisätietoja Azure-tallennustilan geo replikoinnin infrastruktuurin lisätietoja artikkelista-tallennustilan tiimin blogia [Redundancy asetukset](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)ja RA GRS.

##<a name="best-practices-for-protecting-your-data"></a>Tietojen suojaaminen parhaat käytännöt

On joitakin suositellut tavoista tallennustilan tietojen varmuuskopioida säännöllisesti.

-   AM levyjen – Azuren näennäiskoneiden käyttämä AM levyjen varmuuskopiointi [Azure varmuuskopiointi-palvelun](https://azure.microsoft.com/services/backup/) avulla.

-   Estä BLOB – Create [tilannevedoksen](https://msdn.microsoft.com/library/azure/hh488361.aspx) kunkin estä Blob-objektien tai kopioiminen toiseen tallennustilaan BLOB tilin toisen alueen [AzCopy](storage-use-azcopy.md)sekä [PowerShellin Azure](storage-powershell-guide-full.md)ja [Azure tietojen siirto-kirjasto](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).

-   Taulukoiden – [AzCopy](storage-use-azcopy.md) avulla voit viedä taulukkotiedot toisen alueen toisen tallennustilan tililtä.

-   Tiedostojen – Käytä [AzCopy](storage-use-azcopy.md) tai [PowerShellin Azure](storage-powershell-guide-full.md) tiedostojen kopioiminen toisen alueen toisen tallennustilan tilin.
