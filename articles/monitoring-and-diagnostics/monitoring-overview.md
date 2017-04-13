<properties
    pageTitle="Yleistä Microsoft Azure valvontaa | Microsoft Azure"
    description="Ylimmän tason yhteenveto seuranta-ja Microsoft Azure esimerkiksi ilmoitukset webhooks, Automaattinen skaalaus ja vianmääritys."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>Yleistä Microsoft Azure seuranta

Tässä artikkelissa Azure resurssien valvominen Käsitteellinen yleiskatsaus. Se sisältää tietoja osoittimet tietyntyyppiset resurssit.  Ylätason tietoja seuranta sovelluksesi-Azure näkökulmasta on lisätietoja [Seuranta- ja diagnostiikka ohjeissa](../best-practices-monitoring.md).

Cloud sovellukset ovat monimutkaisia monta liikkuvat osat kanssa. Seuranta on tietoja varmistaa, että sovelluksesi pysymisen rajoitusten määrittäminen ja suorittaminen kunnossa-tilaan. Se myös avulla voit stave käytöstä mahdollisia ongelmia tai vianmääritys aiempia lähimpään kokonaislukuun. Voit lisäksi saada laaja tiedot sovelluksen tietoja seurantatiedot. Kyseisen knowledge voit avulla voit parantaa sovelluksen suorituskykyä tai kunnossapidettävyys tai automatisoida toiminnot, jotka muuten edellyttäisi manuaalisia toimia.

Seuraavassa kaaviossa on esitetty käsitteellinen kuva Azure seuranta voit kerätä lokit ja mitä voit tehdä tietojen tyypin mukaan lukien.   

![Loogisten mallien seuranta-ja diagnostiikka Laske resurssit](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Kuva 1: Käsitteellinen malli seuranta-ja diagnostiikka Laske resurssit

<br/>

![Loogisten mallien seurantaa ja Laske resurssien diagnostiikka](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Kuva 2: Käsitteellinen malli seurantaa ja Laske resurssien diagnostiikka


## <a name="monitoring-sources"></a>Lähteiden seuranta
### <a name="activity-logs"></a>Lokeja
Tehtävän lokiin (aiemmalta nimeltään toiminnassa tai valvontalokien) voit etsiä tietoja Azure infrastruktuurin tarkastelu resurssin tietoja. Kirjaudu sisältää tietoja, kuten ajat, kun resursseja luodaan ja tuhotaan.  

### <a name="host-vm"></a>Host AM
**Laske vain**


Jotkin Laske pilvipalveluihin, näennäiskoneiden, esimerkiksi ja palvelun kangasta on erillinen Host AM, käsitellä niitä. Host (isäntä)-AM on pääkansio AM vastikkeen Hyper-V hypervisor mallin. Tässä tapauksessa voit kerätä vain Host AM lisäksi Vieras OS-arvot.  

Azure muista palveluista ei ole välttämättä 1:1, resurssi ja erityisesti Host AM yhdistämisen niin host AM arvot eivät ole käytettävissä.


### <a name="resource---metrics-and-diagnostics-logs"></a>Resurssi - arvot ja diagnostiikka-lokit
Resurssin laji määräytyvät collectable arvot. Esimerkiksi näennäiskoneiden on tilastotietoja levyn IO ja prosenttia suorittimen. Mutta näiden tilasto ei ole palvelun Bus jono, joka sisältää arvot, kuten jonon koko ja viestin siirtonopeuden sijaan.

Laske resurssien voit hankkia arvot, kuten Azure diagnostiikka Vieras OS ja diagnostiikka-moduuleissa. Azure vianmääritys auttaa keräämään ja reitittää kerää diagnostiikkatiedot muihin sijainteihin, kuten Azure-tallennustilan.

Tällä hetkellä collectable arvot luettelo on saatavilla kohdassa [Tuetut arvot](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Sovellus – diagnostiikka lokit, sovelluksen lokit ja arvot
**Laske vain**

Sovellusten suorittamisen Laske mallin vieras-käyttöjärjestelmän päälle. Ne lähettävät omia joukko lokit ja arvot.

Arvot ovat

- Suorituskyvyn laskureita
- Sovelluksen lokit
- Windowsin tapahtumalokien
- .NET Tapahtumalähde
- IIS-lokit
- Luettelo perusteella tapahtumien seuranta
- Kaatumisvedokset
- Asiakkaan virhelokeja


## <a name="uses-for-monitoring-data"></a>Käyttötarkoituksia-tietojen valvominen

### <a name="visualize"></a>Visualisointi
Kesäolympialaisten visualisointi valvontatietojen kuvat ja kaaviot auttaa löytämään trendejä huomattavasti nopeammin kuin esittää tiedot itse kautta.  

Visualisoinnin joitakin menettelytapoja ovat seuraavat:

- Azure-portaalin käyttäminen
- Azure-sovelluksen havainnollistamisen reitin tiedot
- Microsoft PowerBI reitin tiedot
- Reitittää tiedot kolmannen osapuolen visualisoinnin työkalun joko live streaming tai että työkalu lukea arkistosta Azuren tallennustilaan

### <a name="archive"></a>Arkisto
Tietojen valvominen on yleensä kirjoitettu Azure tallennustilan ja säilyttää siellä, kunnes poistat sen.

Jos haluat käyttää näitä tietoja muutamalla tavalla:

- Kun kirjoitettu, voit määrittää muita työkaluja sisällä tai ulkopuolella Azure lukea ja käsitellä niitä.
- Lataa paikallisesti paikallisen arkiston tiedot tai muuttaa oman säilytyskäytännön tiedot pysyvät ajan pitkän pilveen.  
- Jätät tiedot Azure tallennustilan toistaiseksi, mutta sinun tarvitse maksaa Azure tallennustilan pidät tietomäärän perusteella.
-

### <a name="query"></a>Kyselyn
Azure näytön REST-Ohjelmointirajapinta cross platform käyttöliittymä (CLI) komentoja, PowerShellin cmdlet-komennot tai .NET SDK avulla voit käyttää tietoja järjestelmän tai Azure-tallennustilan

Esimerkkejä:

-  Olet kirjoittanut mukautetun seurantaa sovelluksen tietojen hakeminen
-  Mukautettujen kyselyjen luominen ja tietojen lähettäminen kolmannen osapuolen sovelluksen.

### <a name="route"></a>Reititys
Voit käyttää seurantatiedot muihin sijainteihin reaaliajassa.

Esimerkkejä:

- Lähetä sovelluksen havainnollistamisen, jotta voit käyttää visualisoinnin työkaluja siellä.
- Lähetä tapahtuman porttiin, jotta voit ohjata reaaliaikaisia analysoinnissa kolmannen osapuolen työkaluja.

### <a name="automate"></a>Automatisointi
Voit käyttää seurantatiedot käynnistimen tapahtumia tai jopa koko prosessit Esimerkkejä:

- Käytä tietojen Automaattinen skaalaus Laske esiintymät ylös tai alas sovelluksen lataaminen perusteella.
- Lähettää sähköpostiviestejä, kun mittarin ylittää ennalta raja-arvon.
- Soita URL-osoite (webhook) suorittaa toiminnon ulkopuolella Azure-järjestelmässä
- Käynnistä runbookin Azure automaatio suorittaa erilaisia tehtäviä minkä tahansa

## <a name="methods-of-use"></a>Käytä menetelmät
Yleensä voit käsitellä tietoja seuranta, reititys ja noutaminen jollakin seuraavista tavoista. Kaikki menetelmät ovat käytettävissä kaikki toiminnot ja tietotyyppejä.

- [Azure portal](https://portal.azure.com)
- [PowerShellin](insights-powershell-samples.md)  
- [Office kaikissa ympäristöissä rivin komentoliittymä (CLI)](insights-cli-samples.md)
- [REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK-PAKETISSA](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Azure's seuranta-versioissa
Azure on tarjouksia käytettävissä palveluidesi paljas infrastruktuuri-sovelluksen telemetriatietojen seurantaa varten. Seurannan strategia parhaiten yhdistää kaikki kolme, hanki kattava, yksityiskohtaisia tietoja kuntotietojen palvelujen käyttöä.

- [Azure näytön](http://aka.ms/azmondocs) – tarjouksia visualisoinnin, kyselyn, reititys, ilmoitat, Automaattinen skaalaus ja automaatio tiedot sekä Azure infrastruktuuri (tehtävän Log) ja kunkin yksittäisen Azure resurssin (vianmäärityslokit). Tämä artikkeli on osa kokonaisuutta Azure näytön-ohjeista. Azure näytön nimi julkaistiin syyskuu 25 Ignite 2016-palvelussa.  Edellisen nimi on "Azure oivalluksia."  
- [Hakemuksen tiedot](https://azure.microsoft.com/documentation/services/application-insights/) – sisältää monipuolisia tunnistaminen ja diagnostiikka palvelusi hyvin integroitu päälle tietojen Azure seuranta sovellustason ongelmia. Sovelluksen palvelun verkkosovelluksissa oletusarvon diagnostiikka-ympäristö on.  Voit ohjata muiden palveluiden siihen tietoja.  
- [Toimintojen hallinta Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) osassa on kokonaisvaltaiseksi IT-hallintaratkaisu paikallisen ja kolmannen osapuolen pilvipohjainen infrastruktuuri (kuten sisältyy AWS) lisäksi Azure resurssit.  Azure-näytön tiedot voidaan reitittää suoraan Log Analytics niin, että arvot ja lokit koko ympäristössä yhdessä paikassa.     


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja

- [Azure näytön video Ignite 2016](https://myignite.microsoft.com/videos/4977)
- [Azure näytön käytön aloittaminen](monitoring-get-started.md)
- Jos yrität suorittaa pilvipalvelussa, virtuaalikoneen tai palvelun kangasta sovelluksen ongelmien vianmääritystä [Azure diagnostiikka](../azure-diagnostics.md) .
- Jos yrität diagnostiikan ongelmia App Service-sovellukseen [Hakemuksen tiedot](https://azure.microsoft.com/documentation/services/application-insights/) .
- [Azure-tallennustilan vianmääritys](../storage/storage-e2e-troubleshooting.md) käytettäessä BLOB Storage, taulukoita tai olevien
- [Log-analyysin](https://azure.microsoft.com/documentation/services/log-analytics/) ja [Toimintojen hallinta Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)
