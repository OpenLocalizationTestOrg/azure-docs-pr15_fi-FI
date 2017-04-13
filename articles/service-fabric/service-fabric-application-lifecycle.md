<properties
   pageTitle="Sovelluksen elinkaari-palvelun kangasta | Microsoft Azure"
   description="Tässä artikkelissa kuvataan kehittäminen, käyttöönotosta, testaaminen, päivittäminen, ylläpito ja palvelun kangasta sovellusten poistaminen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Palvelun kangasta sovelluksen elinkaari
Kun muiden ympäristöjen kanssa Azure palvelun kangasta sovelluksen yleensä käy läpi seuraavat vaiheet: rakenne, kehitystä, testaaminen, käyttöönoton, päivittäminen, ylläpito ja poistaminen. Palvelun kangasta tukee pääosin cloud sovellusten kehittämisen käyttöönoton, päivittäiset hallinnan ja ylläpito-potentiaalisen käytöstä koko sovelluksen elinkaari. Palvelun malli mahdollistaa useita eri rooleja osallistumaan itsenäisesti sovelluksen elinkaari. Tässä artikkelissa on yleiskatsaus API ja miten niitä käytetään eri rooleja palvelun kangasta sovelluksen elinkaaren vaiheiden koko.

## <a name="service-model-roles"></a>Palvelun mallin roolit
Palvelun mallin roolit ovat seuraavat:

- **Palvelun kehittäjä**: kehittää moduulit ja Yleinen-palvelut, jotka purposed uudelleen ja käyttää samaa tyyppiä tai eri useita sovelluksia. Esimerkiksi jonon palvelu voidaan luomisen reittiosuuksien sovelluksen (tukipalvelu) tai e commerce sovelluksen (ostoskori).

- **Sovelluksen kehittäjä**: Luo sovelluksia integroimalla sivustokokoelman palvelujen täyttävät tietyt erityiset vaatimukset tai skenaarioita. Esimerkiksi e commerce sivuston voi integroida "JSON tilattomien FEP palvelu," "Kilpailuun tilallisen palveluun" ja "Jonon tilallisen palvelun" Muodosta auctioning ratkaisu.

- **Sovelluksen järjestelmänvalvojan**: tekee päätöksiä sovelluksen määritys (täyttämällä määritys Malliparametrit), käyttöönoton (määrityksiä resursseilla) ja palvelun laatua. Sovelluksen järjestelmänvalvoja päättää esimerkiksi sovelluksen kielen (englanti Yhdysvalloissa) tai japani, esimerkiksi japani. Eri käyttöön sovellus voi olla eri asetukset.

- **Operaattori**: ottaa käyttöön sovelluksen määritys-sovelluksilla ja sovelluksen järjestelmänvalvojan asettamia vaatimuksia. Esimerkiksi operaattori valmistelee ja ottaa käyttöön sovelluksen ja varmistaa, että se toimii Azure-tietokannassa. Operaattorit seurata sovelluksen terveys- ja suorituskyvyn ja säilyttää fyysinen infrastruktuurin tarpeen mukaan.


## <a name="develop"></a>Kehittäminen
1. *Palvelun developer* kehittää erityyppisten palveluiden [Luotettava toimijat](service-fabric-reliable-actors-introduction.md) tai [Luotettavia palveluja](service-fabric-reliable-services-introduction.md) ohjelmoinnin mallin.
2. *Palvelun developer* määrittämisen avulla kuvataan muodostuvalle vähintään yksi koodi ja määritysten palvelun luettelon tiedoston kehittämä palvelun tiedostotyypeistä.
3. *Sovelluksen kehittäjän* muodostaa sitten eri tyypit-sovellus.
4. *Sovelluksen kehittäjän* määrittämisen avulla kuvataan sovellusluettelo-sovelluksen tyyppi viittaamalla tuotteen palvelut ja asianmukaisesti ohittaminen ja parametrointi eri määritys ja käyttöönoton asetukset tuotteen palveluja service-luettelot.

Katso esimerkkejä [luotettava toimijoiden käytön aloittaminen](service-fabric-reliable-actors-get-started.md) ja [luotettavia palveluja käytön aloittaminen](service-fabric-reliable-services-quick-start.md) .

## <a name="deploy"></a>Ottaa käyttöön
1. *Sovelluksen järjestelmänvalvojan* voidaan muuttaa tietyn sovelluksen käyttöön palvelun kangasta klusterin määrittämällä **ApplicationType** elementin parametrit sovellusluettelo sovelluksen tyyppi.

2. *Operaattori* Lataa sovelluspaketin klusterin kuva Store käyttämällä [ **CopyApplicationPackage** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) tai [ **Kopio-ServiceFabricApplicationPackage** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125905.aspx). Sovelluspaketin sisältää sovelluksen luettelo ja palvelupakettien sivustokokoelman. Palvelun kangasta ottaa käyttöön kuva säilöön, joka voi olla Azure-blob-säilö tai palvelun kangasta järjestelmäpalvelulla tallennettuja sovelluspaketin sovelluksista.

3. Valitse *operaattori* valmistelee sovelluksen tyyppi- [ **ProvisionApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Rekisteröi ServiceFabricApplicationType** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125958.aspx)tai [ **säännöstä sovelluksen** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707672.aspx)käyttäminen ladata sovelluspaketin kohde-klusterin.

4. *Operaattori* alkaa sovelluksen valmistelun sovelluksen jälkeen annetut *sovelluksen järjestelmänvalvojan* [ **CreateApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), [ **Uusi ServiceFabricApplication** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125913.aspx)tai [ **Sovelluksen luominen** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707676.aspx)avulla.

5. Sen jälkeen, kun sovellus on otettu käyttöön, *operaattori* käyttää [ **CreateServiceAsync** menetelmää](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), [ **Uusi ServiceFabricService** cmdlet-komento,](https://msdn.microsoft.com/library/azure/mt125874.aspx)tai [ **Luo palvelun** REST-toiminto](https://msdn.microsoft.com/library/azure/dn707657.aspx) Luo uuden sovelluksen perustuvat käytettävissä palvelun palvelun esiintymät.

6. Sovellus on nyt käynnissä palvelun kangasta klusterin.

Katso esimerkkejä [sovelluksen käyttöönotto](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Testi
1. *Palvelun developer* suorittamisen jälkeen käyttöönotossa paikallista kehittämistä klusterin tai testi-klusterin, valmiit automaattisesti testi-skenaario [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) ja [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) luokkien tai [ **Käynnistä ServiceFabricFailoverTestScenario** cmdlet-komennon](https://msdn.microsoft.com/library/azure/mt644783.aspx)avulla. Automaattisesti testi-skenaario suorittaa tietyn palvelun kautta tärkeitä siirtymien ja failovers ovat edelleen käytettävissä ja että mikrofoni toimii.

2. *Palvelun developer* suorittaa valmiin chaos testi-skenaario [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) ja [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) luokkien tai [ **Käynnistä ServiceFabricChaosTestScenario** cmdlet-komennon](https://msdn.microsoft.com/library/azure/mt644774.aspx)avulla. Chaos testi-skenaario aiheuttaa satunnaisesti solmun, koodi paketti ja replikan virheitä klusterin kyselyjä.

3. *Palvelun developer* [testien palvelun viestintä](service-fabric-testability-scenarios-service-communication.md) mukaan authoring testi tilanteita, joissa siirtää ensisijainen replikoiden klusterin ympärille.

Lisätietoja on kohdassa [Johdanto vika Analysis-palveluun](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Päivittäminen
1. *Palvelun developer* käyttöönotettu sovelluksen tuotteen palveluja ja/tai korjaa virheet ja uuden version service-luettelo.

2. *Sovelluksen kehittäjän* ohittaa ja parameterizes yhdenmukaisia palvelujen määrittämistä ja käyttöönottoa asetuksia ja tarjoaa uuden version sovelluksen luettelo. Sovelluksen kehittäjä kattaa palvelun luettelot uusia versioita sovellukseen sitten ja tarjoaa sovelluksen tyyppi on päivitetty sovelluspaketin uuden version.

3. *Sovelluksen järjestelmänvalvojan* kattaa kohde-sovellukseen uudella versiolla sovelluksen tyyppi, päivittämällä tarvittavat parametrit.

5. *Operaattori* lataa päivitetyt sovelluspaketin klusterin kuva store [ **CopyApplicationPackage** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) tai [ **Kopioi ServiceFabricApplicationPackage** cmdlet-komennon](https://msdn.microsoft.com/library/azure/mt125905.aspx)avulla. Sovelluspaketin sisältää sovelluksen luettelo ja palvelupakettien sivustokokoelman.

6. *Operaattori* valmistelee kohde-klusterin sovelluksen uuden version [ **ProvisionApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Rekisteröi ServiceFabricApplicationType** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125958.aspx)tai [ **säännöstä sovelluksen** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707672.aspx)avulla.

7. *Operaattori* päivittää kohdesovelluksen uuteen versioon [ **UpgradeApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), [ **Käynnistä ServiceFabricApplicationUpgrade** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125975.aspx)tai [ **sovelluksen päivittäminen** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707633.aspx)avulla.

8. *Operaattori* tarkistaa [ **GetApplicationUpgradeProgressAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), [ **Get-ServiceFabricApplicationUpgrade** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125988.aspx)tai [ **Hae sovelluksen päivittäminen edistymisen** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707631.aspx)käyttäminen päivityksen etenemisen.

9. Tarvittaessa *operaattori* Muokkaa ja käyttää parametreja nykyisen sovelluksen päivityksen [ **UpdateApplicationUpgradeAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), [ **Päivitys ServiceFabricApplicationUpgrade** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt126030.aspx)tai [ **Update-sovelluksen päivittäminen** REST-toiminnon](https://msdn.microsoft.com/library/azure/mt628489.aspx)avulla.

10. Valitse tarvittaessa *operaattori* peruutetaan nykyisen sovelluksen päivityksen [ **RollbackApplicationUpgradeAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), [ **Käynnistä ServiceFabricApplicationRollback** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125833.aspx)tai [ **Peruutus-sovelluksen päivittäminen** REST-toiminnon](https://msdn.microsoft.com/library/azure/mt628494.aspx)avulla.

11. Palvelun kangasta päivittää käynnissä klusterin menettämättä jokin sen tuotteen palveluista käytettävyyttä kohdesovellus.

Katso esimerkkejä [sovelluksen päivittäminen opetusohjelma](service-fabric-application-upgrade-tutorial.md) .

## <a name="maintain"></a>Ylläpidä
1. Käyttöjärjestelmän päivitykset ja korjaukset-palvelun kangasta liittymät Azure infrastruktuuriin taata kaikki klusterin sovellusten käytettävyyden.

2. Päivitykset ja korjaukset palvelun kangasta-ympäristö palvelun kangasta päivittää itse käytettävissä minkä tahansa klusterin sovellusten menettämättä.

3. *Sovelluksen järjestelmänvalvojan* hyväksyy solmujen klusterista poistamisen jälkeen analysoiminen historiallisten kapasiteetin käyttöasteen tiedot ja arvioidut tulevat tarvittaessa tai lisäys.

4. *Operaattori* Lisää ja poistaa *sovelluksen järjestelmänvalvojan*määrittämää solmujen.

5. Kun uusi solmujen lisätään tai aiemmin solmujen on poistettu klusterista, palvelun kangasta automaattisesti kuormituksen-saldojen käynnissä olevat sovellukset yli kaikki solmut klusterin parhaan mahdollisen suorituskyvyn saavuttamiseksi.

## <a name="remove"></a>Poista
1. *Operaattori* poistaa työnkulun esiintymään käynnissä palvelu klusterin poistamatta koko sovelluksen [ **DeleteServiceAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), [ **Poista ServiceFabricService** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt126033.aspx)tai [ **Poistaa palvelun** REST-toiminnon](https://msdn.microsoft.com/library/azure/dn707687.aspx)avulla.  

2. *Operaattorin* myös poistaa sovelluksen esiintymää ja kaikki sen-palveluita käyttämällä [ **DeleteApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), [ **Poista ServiceFabricApplication** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125914.aspx)tai [ **Poista sovellus** REST-toimintoa](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Kun sähköpostisovelluksen ja -palvelut on pysäytetty- *operaattoria* voidaan laajentamisen poistaminen [ **UnprovisionApplicationAsync** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), [ **Poista ServiceFabricApplicationType** cmdlet-komento](https://msdn.microsoft.com/library/azure/mt125885.aspx)tai [ **Poista sovellus valmistelu** REST-toimintoa](https://msdn.microsoft.com/library/azure/dn707671.aspx)käyttämällä sovelluksen tyyppi. Sovelluksen tyyppi laajentamisen poistaminen ei poista sovelluspaketin ImageStore. Sinun on poistettava sovelluspaketin manuaalisesti.

4. *Operaattori* poistaa sovelluspaketin ImageStore [ **RemoveApplicationPackage** menetelmä](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) tai [ **Poista ServiceFabricApplicationPackage** cmdlet-komennon](https://msdn.microsoft.com/library/azure/mt163532.aspx)avulla.

Katso esimerkkejä [sovelluksen käyttöönotto](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja kehittämisestä testaus ja palvelun kangasta sovellusten ja palveluiden hallinta artikkelissa:

- [Luotettavan toimijat](service-fabric-reliable-actors-introduction.md)
- [Luotettavan palvelut](service-fabric-reliable-services-introduction.md)
- [Sovelluksen ottaminen käyttöön](service-fabric-deploy-remove-applications.md)
- [Sovelluksen päivitys](service-fabric-application-upgrade.md)
- [Testattavuus yleiskatsaus](service-fabric-testability-overview.md)
- [REST-pohjaisesta sovelluksesta elinkaari-Esimerkki](service-fabric-rest-based-application-lifecycle-sample.md)
