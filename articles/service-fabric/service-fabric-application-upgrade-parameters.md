
<properties
   pageTitle="Sovelluksen päivitys: parametrien päivitystä | Microsoft Azure"
   description="Tässä artikkelissa kuvataan päivittämiseen palvelun kangasta-sovelluksessa, mukaan lukien terveystarkastukset suorittamiseen ja Kumoa automaattinen päivitys käytännöt liittyviä parametrit."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Sovelluksen päivitys parametrit

Tässä artikkelissa kuvataan eri parametrit, jotka koskevat Azure palvelun kangasta sovelluksen päivityksen aikana. Parametrien sisältää nimen ja sovelluksen versio. Ne ovat nupit, jotka ohjaavat aikakatkaisu ja kuntotietojen tarkistus, joita käytetään päivityksen aikana, ja ne määrittävät käytäntöjä, jotka on otettava käyttöön, kun päivitys epäonnistuu.


<br>

| Parametri | Kuvaus |
| --- | --- |
| ApplicationName | Sovellus, jossa on päivitettävän nimi. Esimerkkejä: kangasta: / VisualObjects kangasta: / ClusterMonitor  |
| TargetApplicationTypeVersion | Sovelluksen versio, kirjoita päivityksen kohteet. |
| FailureAction | Toiminto, tekemä palvelun kangasta, kun päivitys epäonnistuu. Sovellus voi koota takaisin päivitystä edeltävässä versiossa (peruutus) tai päivitys voidaan pysäyttää nykyisen päivityksen domain-palvelussa. Jälkimmäisessä päivityksen tila muuttuu myös manuaalisesti. Sallitut arvot ovat palauttamista ja Manuaalinen. |
| HealthCheckWaitDurationSec | Odotusaikaa (sekunteina), kun päivitys on valmis ennen palvelun kangasta päivityksen toimialueella arvioi sovelluksen kunto. Tämä kesto on pidetään myös sovelluksen on oltava käynnissä, ennen kuin sitä voidaan pitää kunnossa aika. Jos kuntotietojen tarkistuksessa, päivitysprosessi etenee seuraavan päivityksen toimialueeseen.  Kuntotietojen tarkistus epäonnistuu, jos palvelun kangasta odottaa ennen yritetään kuntotietojen tarkistuksen uudelleen, kunnes HealthCheckRetryTimeout saavutetaan aikavälin (UpgradeHealthCheckInterval). Oletus- tai Suositeltu arvo on 0 sekuntia. |
| HealthCheckRetryTimeoutSec | Kesto (sekunteina) kyseisen palvelun kangasta säilyy suorittaa kunto arviointi ennen määritteleminen päivitys epäonnistui, kun. Oletusarvo on 600 sekuntia. Tämä kesto käynnistyy, kun HealthCheckWaitDuration on saavutettu. Tämän HealthCheckRetryTimeout sisällä palvelun kangasta voi suorittaa useita sovelluksen terveyden kuntotietojen tarkistus. Oletusarvo on 10 minuuttia ja olisi voi mukauttaa asianmukaisesti sovelluksen. |
| HealthCheckStableDurationSec | Kesto (sekunteina) Tarkista, että sovellus on vakaata ennen seuraavan päivityksen toimialueen siirtäminen tai täyttämällä päivitys. Odota tätä kesto käytetään estää tunnistamattoman muutokset terveyden oikealle, kun kuntotietojen tarkistus suoritetaan. Oletusarvo on 120 sekuntia ja olisi voi mukauttaa asianmukaisesti sovelluksen. |
| UpgradeDomainTimeoutSec | Suurin aika (sekunteina) päivittämiseen yksittäisen päivityksen toimialueeseen. Jos tämä aikakatkaisun saavutetaan, päivitys pysäyttää ja etenee mukaan UpgradeFailureAction-asetusta. Oletusarvo on ei koskaan (ääretön) ja olisi mukautettu asianmukaisesti sovelluksen. |
| UpgradeTimeout | Aikakatkaisu (sekunteina), joka koskee koko päivitykseen. Jos tämä aikakatkaisun saavutetaan, päivityksen pysähtyy ja UpgradeFailureAction käynnistyy. Oletusarvo on ei koskaan (ääretön) ja olisi mukautettu asianmukaisesti sovelluksen. |
| UpgradeHealthCheckInterval | Taajuus kunnon tila on valittuna. Tämä parametri on määritetty ClusterManager-osassa _klusterin_ _luettelon_ja osana päivityksen cmdlet-komento ei ole määritetty. Oletusarvo on 60 sekuntia.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Palvelun kunto-arviointi sovelluksen päivityksen aikana

<br>
Kuntotietojen arvioinnin ehdot ovat valinnaisia. Jos kunto arvioinnin ehto ei ole määritetty, on päivitettävä käynnistyessä, palvelun kangasta käyttää sovelluksen esiintymää ApplicationManifest.xml määritetyn sovelluksen kuntokäytännöt.


<br>

| Parametri | Kuvaus |
| --- | --- |
| ConsiderWarningAsError | Oletusarvo on EPÄTOSI. Käsittele varoitus kunto tapahtumat sovelluksen virheinä laskettaessa kuntotietojen sovelluksen päivityksen aikana. Oletusarvon mukaan palvelun kangasta ei arvioi varoitus kunto tapahtumien on virheitä (virheitä), joten päivitys voi jatkaa, vaikka on varoitustapahtumat.   |
| MaxPercentUnhealthyDeployedApplications | Oletusarvoiset ja Suositeltu arvo on 0. Määritä enimmäismäärä on otettu käyttöön (katso [kunto-osassa](service-fabric-health-introduction.md)) sovelluksia, jotka voivat virheelliseksi, ennen kuin sovellus pidetään perusasemassa ja päivitys epäonnistuu. Tämä parametri määrittää sovelluksen kuntotietojen solmun ja auttaa tunnistamaan ongelmia päivityksen aikana. Yleensä sovelluksen replikoiden Hae kuormituksen saapuva muiden solmun, joka sallii sovelluksen näkyvän kunnossa, jolloin päivityksen Jatka. Tarkka MaxPercentUnhealthyDeployedApplications kunto määrittämällä palvelun kangasta voit tunnistaa sovelluspaketin ongelma nopeasti ja tuottaa nopeasti päivityksen epäonnistumisen Ohje. |
| MaxPercentUnhealthyServices | Oletusarvoiset ja Suositeltu arvo on 0. Määritä palvelujen enimmäismäärä sovelluksen esiintymän, voit virheelliseksi, ennen kuin sovellus pidetään perusasemassa ja päivitys epäonnistuu. |
| MaxPercentUnhealthyPartitionsPerService | Oletusarvoiset ja Suositeltu arvo on 0. Määritä palvelu, jota voit virheelliseksi, ennen kuin palvelun katsotaan perusasemassa osioiden enimmäismäärä. |
| MaxPercentUnhealthyReplicasPerPartition | Oletusarvoiset ja Suositeltu arvo on 0. Määritä osio, joka voi virheelliseksi, ennen kuin osio katsotaan perusasemassa replikoiden enimmäismäärä. |
| UpgradeReplicaSetCheckTimeout | **Tilattomien palvelun**--yksittäisen päivityksen toimialueen Service kangasta yrittää varmistaa, että muut esiintymät palvelun ovat käytettävissä. Jos kohde esiintymän määrä on enemmän kuin yksi, palvelun kangasta odottaa useamman kuin yhden esiintymän on käytettävissä enintään aikakatkaisuarvoksi ylöspäin. Tämä aikakatkaisun on määritetty UpgradeReplicaSetCheckTimeout-ominaisuuden avulla. Jos aikakatkaisun vanhenee, palvelun kangasta jatketaan päivitys-palvelun kerrat riippumatta. Jos kohde esiintymän määrä on yksi, palvelun kangasta odottaa ja jatketaan päivitys heti. **Tilallisten palvelun**--yksittäisen päivityksen toimialueen Service kangasta yrittää varmistaa, että replikajoukon on asetettu vaatimus. Palvelun kangasta odottaa asetettu vaatimus on käytettävissä enintään suurin aikakatkaisuarvoksi (UpgradeReplicaSetCheckTimeout-ominaisuudessa määritetyn). Jos aikakatkaisun vanhenee, palvelun kangasta jatketaan päivityksen quorum riippumatta. Tämä asetus on joukko kuin koskaan (jatkuva), kun juoksevan eteenpäin ja 900 sekuntia kun peruutetaan. |
| ForceRestart | Jos päivität määritysten tai tietojen pakkaus ilman päivittämistä palvelukoodi, palvelun käynnistetään vain, jos norestart-ominaisuudeksi on määritetty tosi. Kun päivitys on valmis, palvelun kangasta ilmoittaa palvelun määritysten paketin tai tietojen pakkaus on käytettävissä. Palvelu on vastuussa muutokset otetaan käyttöön. Valitse tarvittaessa palvelun voit aloittaa itse. |



<br>
<br>
MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService ja MaxPercentUnhealthyReplicasPerPartition ehdot voidaan määrittää erillisen sovelluksen palvelutyyppi kohden. Nämä parametrit kohden-palvelun määrittäminen mahdollistaa sovelluksen sisältävät erilaisia palveluita tyypit eri arvioinnin käytäntöjen kanssa. Tilattomien yhdyskäytävän palvelutyyppi voi olla esimerkiksi MaxPercentUnhealthyPartitionsPerService, joka poikkeaa tilallisten engine-palvelutyyppi sovelluksen esiintymän.

## <a name="next-steps"></a>Seuraavat vaiheet

[Päivittäminen oman sovellus käyttää Visual Studio](service-fabric-application-upgrade-tutorial.md) esitellään sovelluksen päivityksen Visual Studiossa.

[Päivittäminen sovelluksen käyttämällä Powershell](service-fabric-application-upgrade-tutorial-powershell.md) esitellään sovelluksen päivityksen PowerShellin avulla.

Varmista sovelluksen päivitykset yhteensopivat opettelemalla käyttämään [Tietoja Sarjatoiminto](service-fabric-application-upgrade-data-serialization.md).

Opettele käyttämään kehittyneitä toimintoja siirtyessäsi sovelluksen viittaamalla [Lisäohjeita](service-fabric-application-upgrade-advanced.md).

Sovelluksen päivitykset yleisten ongelmien korjaaminen viittaamalla [Vianmääritys sovelluksen päivitykset](service-fabric-application-upgrade-troubleshooting.md)ohjeita.
 
