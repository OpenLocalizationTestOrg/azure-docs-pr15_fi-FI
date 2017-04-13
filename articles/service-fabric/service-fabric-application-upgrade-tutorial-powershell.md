<properties
   pageTitle="Palvelun kangasta sovelluksen päivitys PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi kokemukset palvelun kangasta sovelluksen käyttöönotto, muuttamalla koodi ja toteuttaa päivityksen PowerShellin avulla."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Palvelun kangasta sovelluksen päivitys PowerShellin avulla

> [AZURE.SELECTOR]
- [PowerShellin](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Useimmin käytetyt ja suositellut päivityksen vaihtoehto on valvottu vaiheittaisen päivityksen.  Azure palvelun kangasta valvoo kuntoa päivitettävän sovelluksen joukko kunto käytäntöjä perusteella. Päivitä-toimialue (UD) päivitetään, kun palvelua kangasta arvioi sovelluksen kuntotietojen ja siirtyy seuraavan päivityksen toimialueen tai kunto käytäntöjen mukaan päivitys epäonnistuu.

Valvotun sovelluksen päivitys voi käyttää hallitun tai alkuperäisen API, PowerShell-tai muille käyttäjille. Katso ohjeet suorittamisesta Visual Studiossa päivityksen [Visual Studiossa sovelluksen päivittäminen](service-fabric-application-upgrade-tutorial.md).

Palvelun kangasta seurattava juokseva päivityksiä sovelluksen järjestelmänvalvojan voit määrittää, onko sovellus kunnossa käyttävän palvelun kangasta kunto arvioinnin käytännön. Lisäksi järjestelmänvalvoja määrittää toiminto, joka suoritetaan, kun kunto laskennan epäonnistuu (esimerkiksi tekevät automaattista palauttamista.) Tässä osassa käydään läpi valvottu päivityksen johonkin, joka käyttää PowerShell SDK mallit.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Vaihe 1: Muodosta ja ota käyttöön Visual objektit-Esimerkki


Luoda ja julkaista sovellus napsauttamalla sovelluksen projektissa, **VisualObjectsApplication,** ja valitsemalla **Julkaise** -komennon.  Lisätietoja on artikkelissa [palvelun kangasta sovelluksen päivittäminen opetusohjelma](service-fabric-application-upgrade-tutorial.md).  Vaihtoehtoisesti voit ottaa käyttöön sovelluksen PowerShell.

> [AZURE.NOTE] Ennen palvelun kangasta komentoja voi käyttää powershellissä, sinun on ensin muodostaa yhteyttä klusterin avulla `Connect-ServiceFabricCluster` cmdlet-komento. Vastaavasti oletetaan, että klusterin on jo määritetty paikallisessa tietokoneessa. On artikkelissa- [palvelun kangasta kehitysympäristö määrittämisestä](service-fabric-get-started.md).

Jälkeen perustaminen projektia Visual Studiossa, voit kopioida sovelluspaketin ImageStore PowerShell-komennolla **Kopioi ServiceFabricApplicationPackage** . Seuraavaksi rekisteröidä sovelluksen palvelun kangasta suorituksen **Rekisteröi ServiceFabricApplicationPackage** -cmdlet-komennolla. Viimeinen vaihe on erillisen sovelluksen Aloita **Uusi ServiceFabricApplication** cmdlet-komennolla.  Nämä vaiheet on paperimuotoon käyttäminen Visual Studio **käyttöönotto** -valikkovaihtoehto.

Voit nyt [Palvelun kangasta Explorer klusterin ja sovelluksen tarkastelemiseen](service-fabric-visualizing-your-cluster.md). Sovellus on verkkopalveluun, joka voi olla siirtynyt Internet Explorerissa kirjoittamalla [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) osoiteriville.  Raportissa pitäisi näkyä joitakin irrallisten visual objektien näytön osasta toiseen siirtyminen.  Lisäksi voit tarkistaa sovellusten tilan **Get-ServiceFabricApplication** .

## <a name="step-2-update-the-visual-objects-sample"></a>Vaihe 2: Päivitä Visual objektit-Esimerkki

Saatat huomata, että versiolla, joka on otettu käyttöön vaiheessa 1 visuaalisia objekteja eivät kierry. Oletetaan, että Päivitä tämän sovelluksen sellaiseksi, jossa visuaalisia objekteja myös Kierrä.

Valitse VisualObjects-ratkaisussa VisualObjects.ActorService-projekti ja avaa StatefulVisualObjectActor.cs-tiedosto. Menetelmä Siirry, että tiedostossa `MoveObject`, kommentoi pois `this.State.Move()`, ja kommentointi `this.State.Move(true)`. Tämä muutos kiertää objektit, kun palvelu on päivitetty.

Myös annettava projektin **VisualObjects.ActorService**(kohdassa PackageRoot) *ServiceManifest.xml* -tiedoston päivittämistä. Päivitä *CodePackage* ja palvelun versio 2.0 ja vastaavat rivit *ServiceManifest.xml* -tiedostossa.
Voit Visual Studio *Muokkaa luettelon tiedostot* -asetus, kun napsautat hiiren kakkospainikkeella luettelon tiedoston muokkaamiseen-ratkaisussa.


Kun muutokset on tehty, luettelossa pitäisi näyttää seuraavalta (muutokset näkyvät korostettuna osia):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nyt **VisualObjects.ActorService** projektin versiosta 2.0 päivitetään *ApplicationManifest.xml* -tiedostoon (löytyy kohdasta Valitse **VisualObjects** -ratkaisun **VisualObjects** projektin). Lisäksi sovelluksen versio päivitetään, 2.0.0.0 1.0.0.0. *ApplicationManifest.xml* pitäisi näyttää samalta kuin seuraavassa koodikatkelman:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Luo projektin nyt valitsemalla vain **ActorService** projekti, napsauttamalla hiiren kakkospainikkeella ja valitsemalla Visual Studiossa **luominen** . Jos olet valinnut **kaikki uudelleen**, Päivitä versiot kaikissa projekteissa, koska koodin ovat muuttuneet. Seuraavaksi pakata japanin ***VisualObjectsApplication***hiiren kakkospainikkeella, valitsemalla palvelun kangasta-valikko ja valitsemalla **paketin**päivitetty sovellus. Tämä toiminto luo sovelluksen-paketti, joka voidaan ottaa käyttöön.  Päivitetty sovellus on valmis käyttöön.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Vaihe 3: Päättää kuntokäytännöt ja Päivitä parametrit

Valintanauhaan tutustuminen- [sovelluksen päivittäminen parametrit](service-fabric-application-upgrade-parameters.md) ja on [päivitysprosessi](service-fabric-application-upgrade.md) saat ymmärtää eri päivityksen parametrit, aikakatkaisu ja käytetty kunto-ehtoa. Näiden vaiheiden palvelun kunto arvioinnin ehdon on oletusarvo (ja suositeltava) arvoja, mikä tarkoittaa, että kaikki palvelut ja esiintymät pitäisi olla _kunnossa_ päivityksen jälkeen.  

Kuitenkin japanin suurentaa *HealthCheckStableDuration* 60 sekuntia (niin, että palvelut ovat kunnossa vähintään 20 sekunnin ajan ennen päivityksen siirtyy seuraavaan Päivitä toimialue).  Oletetaan, että myös määrittää *UpgradeDomainTimeout* on 1200 sekuntia ja *UpgradeTimeout* on 3000 sekuntia.

Lopuksi japanin set *UpgradeFailureAction* palauttaminen. Tämä vaihtoehto edellyttää, että palvelun kangasta voit koota takaisin sovelluksen edellistä versiota, jos se havaitsee ongelmat päivityksen aikana. Näin ollen päivitys (valitse vaiheessa 4) käynnistettäessä seuraavat parametrit on määritetty:

FailureAction = peruuttaminen

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Vaihe 4: Valmisteleminen sovelluksen päivitystä varten

Sovellus on nyt valmiita ja valmis päivitettäväksi. Jos avaat PowerShell-ikkunan järjestelmänvalvojana ja kirjoita **Hae ServiceFabricApplication**, se kannattaa avulla voit tietää, että se on sovelluksen tyyppi 1.0.0.0 **VisualObjects** , jotka on otettu käyttöön.  

Sovelluspaketin tallennetaan seuraavasta suhteellinen polusta, missä voit puretaan palvelun kangasta SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Löydät tulisi "Package" kansion, kansion, johon sovelluspaketin on tallennettu. Tarkista aikaleimat varmistaa, että se uusimmassa versiossa (joudut ehkä muokata polut asianmukaisesti sekä).

Nyt päivitetty sovelluspaketin kopioiminen oletetaan, että palvelun kangasta-ImageStore (sovelluksen pakettien tallennuspaikkaa palvelun kangasta mukaan). Parametrin *ApplicationPackagePathInImageStore* ilmoittaa palvelun kangasta, mistä ne löytyvät sovelluspaketin. Emme ole sijoittaa päivitetty sovellus "VisualObjects\_V2" seuraavalla komennolla (joudut ehkä muuttamaan polkuja uudelleen oikein).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Seuraava vaihe on tämän sovelluksen rekisteröityä palveluun kangasta, jota voidaan suorittaa seuraava komento:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Jos edeltävä komento ei onnistu, on todennäköisesti tarvitse muodosta kaikki palvelujen. Kuten edellä vaiheessa 2, saatat joutua päivittämään käyttämäsi verkkopalvelu-versio.

## <a name="step-5-start-the-application-upgrade"></a>Vaihe 5: Sovelluksen päivityksen käynnistäminen

Nyt on valmis sovelluksen päivityksen käynnistäminen avulla seuraava komento:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Sovelluksen nimi on sama kuin se on kuvattu *ApplicationManifest.xml* -tiedostossa. Palvelun kangasta käyttää tätä nimeä tunnistamiseen, missä sovelluksessa käytön päivitetään. Jos määrität aikakatkaisu on liian lyhyt, voit kohdata virhesanoma, joka ilmoittaa ongelma. Vianmääritys-osassa tai suurenna aikakatkaisu.

Nyt-sovelluksen päivityksen tuotot kuin voit valvoa sitä palvelun kangasta Resurssienhallinnan avulla tai käyttämällä seuraavia PowerShell-komentoa: **Get-ServiceFabricApplicationUpgrade kangasta: / VisualObjects**.

Muutaman minuutin kuluttua tila, johon olet saanut edellisen PowerShell-komennon avulla olisi ilmoitettava, että kaikki päivityksen toimialueet on päivitetty (valmis). Ja löydät tulisi selainikkunassa visuaalisia objekteja on aloitettu kiertäminen!

Voit kokeilla versio 3 2-versiota tai versiosta 2 versioon hioa 1 on päivitetty. 2-versiosta siirtäminen versio 1 pidetään myös päivityksen. Toistaa aikakatkaisu ja terveyteen käytännöt voit parantaa tuttuja heidän kanssaan. Kun otat käyttöön Azure klusterin-parametrit on määritetty oikein. Kannattaa määrittää aikakatkaisu käytetään.


## <a name="next-steps"></a>Seuraavat vaiheet

[Päivittäminen Visual Studiossa sovellus](service-fabric-application-upgrade-tutorial.md) opastaa sinua Visual Studiossa sovelluksen päivitys.

Määrittää, miten sovelluksesi päivittää käyttämällä [parametrien päivitystä](service-fabric-application-upgrade-parameters.md).

Varmista sovelluksen päivitykset yhteensopivat opettelemalla käyttämään [tietoja Sarjatoiminto](service-fabric-application-upgrade-data-serialization.md).

Opettele käyttämään kehittyneet toiminnot siirtyessäsi sovelluksesi viittaamalla [lisäohjeita](service-fabric-application-upgrade-advanced.md).

Sovelluksen päivitykset yleisten ongelmien korjaaminen viittaamalla [vianmääritys sovelluksen päivitykset](service-fabric-application-upgrade-troubleshooting.md)ohjeita.
