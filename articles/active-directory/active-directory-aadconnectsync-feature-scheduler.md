<properties
   pageTitle="Azure AD Connect synkronointi: ajoituksen | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan Azure AD Connect synkronointi valmiin ajoitus-toiminnon."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect synkronointi: ajoitus
Tässä aiheessa kerrotaan valmiin ajoituksen Azure AD Connect synkronoinnissa (tietovälinettä Sync engine).

Tämä ominaisuus otettiin muodosta 1.1.105.0 (julkaistu helmikuussa 2016) kanssa.

## <a name="overview"></a>Yleiskatsaus
Azure AD Connect synkronointi Synkronoi paikallisen hakemistossa ajoituksen tapahtuu muutoksia. Objektien ja määritteiden synkronointi ja ylläpito tehtävät ovat kaksi ajoitus-prosesseja, yksi salasanan synkronointi ja toiseen. Tässä artikkelissa neuvotaan viimeksi.

Aikaisempien versioiden objektien ja määritteiden ajoitus ei ulkoisen sync engine ja Windows-ajoitetut tai erillisen Windows-palvelun käytettiin käynnistettävän synkronoinnin aikana. Ajoitustoiminnon liittyy 1.1 versioiden sisäisiä synkronoitava moduuliin ja Salli joidenkin mukauttaminen. Uusi oletusarvo-synkronoinnin korkojakso on 30 minuuttia.

Ajoitustoiminnon on vastuussa kaksi tehtävää:

- **Synkronoinnin käy**. Prosessi, jos haluat tuoda, synkronoi ja vie muutokset.
- **Ylläpitotehtäviä**. Uusia näppäimet ja salasanan ja laitteen rekisteröinti Service (DRS) varmenteet. Tyhjennä toimintojen lokin vanhoja tapahtumia.

Ajoitustoiminnon itse suoritetaan aina, mutta se voidaan määrittää toimimaan vain yhden tai ei mitään näistä tehtävistä. Esimerkiksi jos tarvitset oman jakson synkronointiprosessia, voit päivitysyrityksen tämän tehtävän käytöstä, mutta silti suorittaa ylläpito-tehtävän.

## <a name="scheduler-configuration"></a>Ajoituksen määrittäminen
Voit tarkastella nykyisiä määritysten asetuksia valitsemalla PowerShell ja suorita `Get-ADSyncScheduler`. Se näyttää tältä:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Jos näet **synkronointi-komento tai cmdlet-komento ei ole käytettävissä** , kun suoritat cmdlet-PowerShell-moduulia ei ole ladattu. Näin voi käydä, jos suoritat Azure AD Connect toimialueen ohjauskoneen tai palvelimen suuremmat PowerShell määritetyt rajoituksen kuin oletusasetukset. Jos näet tämän virheen, suorita `Import-Module ADSync` saataville-cmdlet-komennolla.

- **AllowedSyncCycleInterval**. Useimmin Azure AD sallii synkronoinnin suoritetaan. Ei voi synkronoida useammin ja edelleen tuetaan.
- **CurrentlyEffectiveSyncCycleInterval**. Tällä hetkellä käytössä aikataulu. Se on saman arvon kuin CustomizedSyncInterval (Jos määritetty), jos se ei ole useammin kuin AllowedSyncInterval. Jos muutat CustomizedSyncCycleInterval, muutokset otetaan käyttöön seuraavan synkronoinnin jakson jälkeen.
- **CustomizedSyncCycleInterval**. Ajoitus suoritetaan muita kuin oletusarvoista korkojakso puolessa halutessasi määritetään asetus. Ajoitustoiminnon yllä olevassa kuvassa on asetettu suorittamaan tunnissa. Jos määrität tämän arvon pienempi kuin AllowedSyncInterval, jälkimmäinen käytetään.
- **NextSyncCyclePolicyType**. Joko Delta tai nimikirjaimet. Määrittää, jos seuraava Suorita olisi vain prosessin delta muutokset, tai jos suoritetaan seuraavan kerran muuta täydellinen tuominen ja synkronoiminen, jotka myös Käsittele uudelleen uusia tai muutettuja sääntöjä.
- **NextSyncCycleStartTimeInUTC**. Kun seuraavan kerran päivitysyrityksen alkaa seuraavan synkronoinnin kehä.
- **PurgeRunHistoryInterval**. Toimintojen lokit tulisi säilyttää aika. Nämä tarkistaa synkronoinnin palvelun hallintaan. Oletusarvo on säilyttää nämä 7 päivää.
- **SyncCycleEnabled**. Ilmaisee, jos päivitysyrityksen on käynnissä tuominen, synkronointi ja vie-prosessien toimintansa osana.
- **MaintenanceEnabled**. Näyttää, onko ylläpito-prosessi on käytössä. Se päivittää varmenteet ja avaimet ja poista toiminnot-loki.
- **IsStagingModeEnabled**. Näyttää, jos [väliaikaisen tila](active-directory-aadconnectsync-operations.md#staging-mode) on käytössä.

Voit muuttaa joitakin näistä asetuksista, `Set-ADSyncScheduler`. Seuraavat parametrit voidaan muokata:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Azure AD tallennetaan ajoitus-määritys. Jos sinulla on väliaikainen palvelin, ensisijaisen palvelimen muutoksia myös vaikuttaa väliaikaisen server (lukuun ottamatta IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaksi:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - päivän HH - tuntia ja mm - minuuttia, ss - sekunnin ajan

Esimerkki:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Muuttaa ajoitus suorittaa 3 tunnin välein.

Esimerkki:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Muuttaa ajoitus suoritetaan päivittäin.

## <a name="start-the-scheduler"></a>Käynnistä ajoitus
Ajoitustoiminnon ovat oletusarvoisesti käytössä 30 minuutin välein. Joissakin tilanteissa haluat ehkä suorittaa synkronoinnin jakson ajoitetun jaksot välissä tai sinun on suoritettava tyypin.

**Sama.arvo synkronointi kehä**  
Sama.arvo synkronointi jakson sisältää seuraavat vaiheet:

- Sama.arvo tuo kaikki yhdistimien
- Yhdistimien Synkronoi delta
- Vie kaikki yhdistimien

Ehkä että muuttaminen, jossa on synkronoitava heti mistä syystä, sinun on suoritettava jakson-kiireellinen. Jos haluat suorittaa manuaalisesti jakson, valitse Suorita PowerShell- `Start-ADSyncSyncCycle -PolicyType Delta`.

**Koko synkronointi kehä**  
Jos olet tehnyt jompikumpi seuraavista määritysmuutoksia, haluat suorittaa koko synkronointi jakson (tietovälinettä Nimikirjain):

- Lisätä useita objekteja tai määritteet, joita voi tuoda lähteen hakemistosta
- Tehdyt muutokset synkronoinnin säännöt
- Muuttaa [suodattaminen](active-directory-aadconnectsync-configure-filtering.md) , joten on eri määrä objektien pitäisi olla mukana

Jos olet tehnyt nämä muutokset, sinun on suoritettava koko synkronointi jakson siten, että sync engine on mahdollisuus koota uudelleen yhdistimen välilyöntejä. Koko synkronointi jakson sisältää seuraavat vaiheet:

- Kaikki yhdistimien täysi tuominen
- Yhdistimien koko Synkronoi
- Vie kaikki yhdistimien

Aloittaa koko synkronointi jakson, suorita `Start-ADSyncSyncCycle -PolicyType Initial` -PowerShell-kehotteessa. Tämä käynnistää koko synkronointi jakson.

## <a name="stop-the-scheduler"></a>Lopeta ajoitus
Jos päivitysyrityksen on parhaillaan käynnissä synkronoinnin jakson voit joutua niin, ettei se. Esimerkki: Jos käynnistät ohjatun asennuksen ja näyttöön seuraava virhesanoma:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Kun synkronointi jakso on käynnissä, et voi tehdä määritysmuutoksia. Onnistunut Odota, kunnes päivitysyrityksen prosessi on valmis, mutta voit lopettaa sisällysluettelon niin olet tehnyt haluamasi muutokset heti. Nykyisen jakson pysäyttäminen ei ole haitalliset ja edelleen ei käsitellä muutoksia käsitellään seuraavan asennuksen kanssa.

1. Käynnistä ilmoittamalla ajoituksen lopettavat sen nykyisen aikana PowerShell cmdlet-komennon avulla `Stop-ADSyncSyncCycle`.
2. Pysäyttää päivitysyrityksen ne eivät enää nykyisen yhdistin sen nykyisen tehtävästä. Pakota lopettavat yhdistimen tehdä seuraavat toimet: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Käynnistä **Sychronization palvelu** Käynnistä-valikosta. Siirry **yhdistimiä**, korostaminen ja siinä **on** yhdistimen ja valitse **Lopeta** toiminnot.

Ajoitustoiminnon on yhä käytössä ja käynnistetään uudelleen Seuraava mahdollisuuden.

## <a name="custom-scheduler"></a>Mukautetun ajoitus
Tässä osassa cmdlet-komennot ovat vain käytettävissä muodosta [1.1.130.0](active-directory-aadconnect-version-history.md#111300) ja uudempi versio.

Jos valmiin scheduler ei täytä tarpeitasi, voit ajoittaa yhdistimet PowerShellin avulla.

### <a name="invoke-adsyncrunprofile"></a>Käynnistää ADSyncRunProfile
Voit aloittaa profiilin yhdistimen tällä tavalla:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

[Yhdistimen](active-directory-aadconnectsync-service-manager-ui-connectors.md) ja [Suorita profiilin nimet](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) käytettäviä nimet löytyvät [Synkronoinnin palvelun hallinta-Käyttöliittymä](active-directory-aadconnectsync-service-manager-ui.md).

![Käynnistää suorituksen profiilia](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Cmdlet-komento on synkronoitu, eli se ei palauta ohjausobjektin ennen kuin yhdistin on valmis toiminto onnistui tai virheen.

Kun ajoitat yhdistimet, Suosituksena on ajoittavan seuraavassa järjestyksessä:

1. (Täysi/Delta) Paikallisen tiedostot, kuten Active Directory tuominen
2. (Täysi/Delta) Tietojen tuominen Azure AD
3. (Täysi/Delta) Paikallisen tiedostot, kuten Active Directory-synkronointi
4. (Täysi/Delta) Azure AD-synkronointi
5. Azure AD vieminen
6. Paikallisen tiedostot, kuten Active Directory vieminen

Jos tarkastelet valmiin ajoitus, tämä on yhdistimet suoritetaan järjestyksessä.

### <a name="get-adsyncconnectorrunstatus"></a>Hae ADSyncConnectorRunStatus
Voit valvoa nähdäksesi, jos se on varattu tai käyttämättömänä synkronointi-ohjelma. Cmdlet palauttavat tyhjän tuloksen, jos Synkronoi-ohjelma ei käytetä ja yhdistin ei ole käynnissä. Jos yhdistin on käynnissä, se palauttaa yhdistimen nimi.

```
Get-ADSyncConnectorRunStatus
```

![Yhdistimen Suorita tila](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Yllä olevassa kuvassa ensimmäinen rivi on tilasta, jossa synkronointi-ohjelma ei käytetä. From Azure AD-yhdistin suoritettaessa toiselta riviltä.

## <a name="scheduler-and-installation-wizard"></a>Ohjattu ajoitus ja asennus
Jos aloitat ohjatun asennuksen, scheduler keskeytetään väliaikaisesti. Tämä johtuu siitä, ohjelma olettaa sen teet määritysmuutoksia ja näitä ei voi käyttää, jos Synkronoi-ohjelma toimii aktiivisesti. Tästä syystä älä jätä ohjattu asennus Avaa jälkeen se lopettaa synkronoinnin toimintoja tekemästä synkronointi-ohjelma.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
