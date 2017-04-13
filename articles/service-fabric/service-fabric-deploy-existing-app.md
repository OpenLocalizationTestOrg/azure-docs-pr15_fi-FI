<properties
   pageTitle="Ota käyttöön aiemmin suoritettavan Azure palvelun kangasta | Microsoft Azure"
   description="Vaiheittainen kuvaus siitä, miten voit pakata sovellukseen vieraana suoritettavan, jotta se voidaan ottaa käyttöön palvelun kangasta klusterin"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Suoritettava Vieras käyttöön palvelun kangasta

Voit suorittaa minkä tyyppisessä sovellus, esimerkiksi node.js, Java tai alkuperäisen sovellusten Azure palvelun kangasta. Palvelun kangasta viittaa seuraavanlaisiin sovellusten Vieras suoritettavat mukaisesti.
Vieras suoritettavat pidetään palvelun kangasta, kuten tilattomien palvelut. Siksi ne ovat käytettävissä solmuissa klusterin, käytettävyys ja muita tietoja perusteella. Tässä artikkelissa käsitellään pakkaaminen ja ota käyttöön suoritettavan Vieras palvelun kangasta klusterin Visual Studio tai komentorivin apuohjelman avulla.

Tässä artikkelissa kerrotaan ohjeita pakata suoritettavan Vieras ja ota se käyttöön palvelun kangasta.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Suoritettava palvelun kangasta Vieras edut

On monia etuja, jotka on suoritettava palvelun kangasta klusterin Vieras mukana:

- Suuren käytettävyyden. Sovellukset, jotka suoritetaan palvelun kangasta ovat saatavilla erittäin. Palvelun kangasta varmistaa, että sovelluksen esiintymää on käynnissä.
- Kunnon seuranta. Jos sovellus on käynnissä ja tietoja, jos on ilmennyt virhe diagnostiikka kangasta palvelun kunnon valvonta havaitsee.   
- Sovelluksen elinkaaren hallinta. Lisäksi toimittaa päivityksiä ei ole käyttökatkot, palvelun kangasta on automaattista palauttamista edellisen version, jos on virheellinen kunto-tapahtuman raportoitu päivittämisen aikana.    
- Kaavan avulla. Voit suorittaa useita sovelluksia klusterin, joka ei tarvitse suorittaa omassa laitteiston kunkin sovelluksen.


## <a name="overview-of-application-and-service-manifest-files"></a>Yleisiä tietoja sovelluksen ja tiedostojen service-tiedostot

Osana käyttöönotto suoritettavan Vieras kannattaa antaa tietoja palvelun kangasta pakkaus ja käyttöönoton mallin kuvattu [sovelluksen malli](service-fabric-application-model.md). Palvelun kangasta pakkaus malli on riippuvainen kaksi XML-tiedostoja: sovellus- ja -luettelot. ApplicationManifest.xml ja ServiceManifest.xml tiedostojen rakenteen määritys on asennettu palvelun kangasta SDK *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*kyselyjä.

* **Sovelluksen luettelon** Sovelluksen luettelo käytetään kuvaamaan sovellus. Se näyttää palvelut, jotka voit kirjoittaa sen ja muut parametrit, joiden avulla voit määrittää, miten yhden tai useamman palvelut käyttöön, kuten esiintymien määrän.

  Palvelun kangasta sovellus on yksikkö käyttöönottoa ja päivitys. Sovelluksen voi päivittää yhtenä yksikkönä, jossa mahdolliset virheet ja mahdolliset palautukset hallitaan. Palvelun kangasta takaa, että päivitysprosessin on joko onnistuu, tai jos päivitys epäonnistuu, jätä sovelluksen Tuntematon epävakaaseen tilaan.

* **Palvelun luettelon** Palvelun luettelo kerrotaan palvelun osat. Se sisältää tietoja, kuten nimi ja tyyppi-palveluun, ja sen koodin, määritys ja tiedot. Service-luettelo sisältää myös joitakin muita parametreja, jonka avulla voidaan määrittää palvelua, kun se on otettu käyttöön.


## <a name="application-package-file-structure"></a>Sovelluksen paketin tiedostorakenne
Ottaa käyttöön sovelluksen palvelun kangasta sovellus on seurattava ennalta määritettyjä kansiorakenne. Seuraavassa esimerkissä rakenteessa.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

ApplicationPackageRoot sisältää ApplicationManifest.xml-tiedosto, joka määrittää sovelluksen. Jokaisen palvelun hakemukseen alikansio käytetään sisällä kaikkia tietoja, jotka palvelu edellyttää--ServiceManifest.xml ja yleensä seuraavat kolme kansioita:

- *Koodi*. Tämä kansio on koodi.
- *Config*. Tämä kansio sisältää Settings.xml-tiedosto (ja muita tiedostoja, jos se on tarpeen) palvelu voi käyttää suorituksen aikana, voit hakea tiettyjä asetuksia.
- *Tietoja*. Tämä on Lisää hakemisto, paikalliset tiedot, jotka on ehkä palvelun tallentamiseen. Huomautus: Tietoja käytetään vain tilapäiset tietojen tallentamiseen. Palvelun kangasta ei kopioi/toisinto muutokset tiedot hakemiston Jos palvelu on sijoitetaan uudelleen – esimerkiksi aikana automaattisesti.

Huomautus: Sinun ei tarvitse luoda `config` ja `data` kansioita, jos et tarvitse niitä.

## <a name="packaging-an-existing-executable"></a>Olemassa olevan suoritettavan pakkaamista

Kun pakkaamista Vieras suoritettavat, voit joko käyttää Visual Studio projektin mallia tai [luoda sovelluspaketin manuaalisesti](#manually). Visual Studiossa, sovelluksen package rakenne ja tiedostojen tiedostot luodaan uusi projekti ohjatulla puolestasi.

>[AZURE.NOTE] Helpoin tapa suoritettava lisääminen Windowsin pakkaaminen on Visual Studiolla.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Pakkaa aiemmin suoritettavan Visual Studion avulla

Visual Studio sisältää palvelun kangasta palvelun mallin käyttöönoton ajaksi suoritettavan Vieras palvelun kangasta klusteriin.

Siirry seuraavien vaiheiden suorittamiseen julkaisu:

1. Valitse Tiedosto -> uusi projekti ja luo kangasta palvelusovellus.
2. Valitse Vieraskäytön suoritettavat palvelun mallina.
3. Valitse Selaa ja valitse kansio, jossa oman suoritettavat ja Täytä loput palvelun luominen parametrit.
    - *Koodin Package ongelma* voidaan määrittää kopioi kansion koko sisällön Visual Studio-projektiin, joka on hyödyllinen, jos suoritettavan tiedoston eivät muutu. Jos arvelet ja haluat jatkaa uusiin dynaamisesti mahdollisuus suoritettavan tiedoston, voit linkittää kansion sijaan. Huomaa, että voit käyttää linkitetyn kansiot sovelluksen projektia Visual Studiossa luotaessa. Tämä Lähdesijainnin Projectissa, jolloin päivittämistä lähteen perillepääsy suoritettavan vieraskäytön linkkejä ottaa osaksi sovelluspaketin muodosta-päivitykset.
    - *Ohjelma* – Valitse suoritettava tiedosto, joka on ehkä suoritettava-palvelun käynnistäminen.
    - *Argumentit* - argumentit, jotka suoritettava välitetään. Parametrien argumentit luettelo voi olla.
    - *WorkingFolder* - määrittää prosessi, jossa suorittaminen aloitettavaksi työpäivän kansion. Voit määrittää kolme arvoa:
        - `CodeBase`määrittää käytön hakemiston aiotaan voi määrittää sovelluspaketin koodi-kansio (`Code` hakemistoon tiedostorakenne näkyy edeltävän).
        - `CodePackage`määrittää käytön hakemiston aiotaan voi määrittää sovelluspaketin ylimmällä (`GuestService1Pkg` -tiedostorakenne näkyy edeltävän).
        - `Work`määrittää, että tiedostot sijoitetaan alikansio nimeltä työmäärä
4. Anna palvelun nimi ja valitse OK.
5. Jos palvelun tarvitsee päätepisteen viestintään, voit nyt lisätä protokolla porteista ja kirjoita ServiceManifest.xml-tiedostoon. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Voit nyt pakkaaminen ja julkaise vastaan paikallisen klusterin toiminnon virheenkorjaus ratkaisun käyttöön Visual Studiossa. Kun olet valmis voit julkaista remote klusterin sovelluksen tai sisään-ratkaisun hallintaan.
7. Siirry tämän artikkelin Katso, miten voit tarkastella Vieras suoritettavan palvelun suorittamisen kangasta Explorerissa loppuun.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Pakkaamista manuaalisesti ja käyttöönotto aiemmin suoritettavan
Prosessi, jossa pakkaamista manuaalisesti Vieras suoritettavat perustuu seuraavasti:

1. Luo paketti kansiorakenne.
2. Lisätä sovelluksen koodi- ja tiedostoja.
3. Muokkaa luettelon palvelutiedosto.
4. Muokkaa sovelluksen tiedostojen tiedostoa.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Luo package kansiorakenne
Voit aloittaa luomalla kansion rakenteesta, kuvatulla.

### <a name="add-the-applications-code-and-configuration-files"></a>Sovelluksen koodi- ja tiedostojen lisääminen
Kun olet luonut kansiorakenne, voit lisätä sovelluksen koodi ja asetustiedostot koodi ja config-kansiot-kohdassa. Voit myös luoda uusia kansioita tai alihakemistoista koodi tai config-kansiot-kohdassa.

Palvelun kangasta tekee xcopy sovelluksen-pääkansio sisällön niin ei ole valmiin rakenteen muihin kuin luominen kaksi yläreunan kansioita, koodi ja asetukset. (Voit valita eri nimiä halutessasi. Lisätietoja on seuraavassa osassa.)

>[AZURE.NOTE] Varmista, että Sisällytä kaikki tiedostot ja riippuvuuksien sovelluksen tarvitsemat. Palvelun kangasta kopioi klusterin, jossa sovelluksen services sujuvat käyttöön kaikissa solmuissa sovelluspaketin sisällön. Paketin pitäisi olla kaikki tunnus, jolla sovellus on suoritettava. Emme suosittele oletetaan, että riippuvuudet on jo asennettu.

### <a name="edit-the-service-manifest-file"></a>Muokkaa luettelon palvelutiedosto
Seuraavaksi voit muokata palvelun luettelon tiedosto sisältää seuraavat tiedot:

- Palvelutyypin nimi. Tämä on tunnus, jonka palvelun kangasta palvelu tunnistaa.
- Komento Käynnistä sovellus (ExeHost) avulla.
- Komentosarjan, joka on suoritettava määrittämiseen ylös/sovelluksen kokoonpanon määrittäminen (SetupEntrypoint).

Seuraavassa on esimerkki `ServiceManifest.xml` tiedosto:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Mennään päälle tiedosto, jonka haluat päivittää eri osat:

#### <a name="updating-the-servicetypes"></a>Päivitys ServiceTypes

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Voit valita minkä tahansa nimen, jota haluat käyttää `ServiceTypeName`. Arvo on käytössä `ApplicationManifest.xml` palvelu tunnistaa tiedosto.
- Sinun on määritettävä `UseImplicitHost="true"`. Tämän määritteen kertoo, että palvelun perustuu itsenäinen-sovelluksessa, joten kaikki palvelun kangasta täytyy tehdä on Käynnistä kuin prosessin ja sen kunnon valvonta palvelun kangasta.

#### <a name="updating-the-codepackage"></a>Päivitys CodePackage
CodePackage-elementti määrittää palvelun koodin sijainti (ja versio).

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

`Name` Elementti voidaan määrittää hakemiston nimen, joka sisältää palvelun koodin sovelluspaketin. `CodePackage`on myös `version` määrite. Sen avulla voidaan määrittää koodin--version ja voidaan myös mahdollisesti päivittää palvelun koodin käyttämällä palvelun kangasta sovelluksen elinkaaren hallinta infrastruktuurin.
#### <a name="optional-updating-the-setupentrypoint"></a>Valinnainen: Päivittäminen SetupEntrypoint

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Määritä suoritettavat tai erä tiedosto, joka on suoritettava, ennen kuin palvelun koodi käynnistetään käytetään SetupEntrypoint elementti. Tämä on valinnainen vaihe, joten se ei tarvitse sisällytetään, onko alustus/asetuksia ei tarvitse. SetupEntryPoint suoritetaan aina, kun palvelu on käynnistettävä uudelleen.

On vain yksi SetupEntrypoint, joten asennus ja määritys-komentosarjoja ryhmitellä yhteen komentojonotiedosto, jos sovelluksen asennus ja määritys edellyttää useita komentosarjoja. SetupEntrypoint voit suorittaa minkä tyyppisiä tiedostoja--ohjelmatiedostot, erän tiedostoja ja PowerShellin cmdlet-komennot. Lisätietoja on artikkelissa [Määrittäminen SetupEntryPoint](service-fabric-application-runas-security.md).

Edellisessä esimerkissä SetupEntrypoint suoritetaan komentojonotiedosto nimeltä `LaunchConfig.cmd` , joka sijaitsee `scripts` (olettaen että WorkingFolder elementti on määritetty CodeBase) koodi-kansion alikansio.

#### <a name="updating-the-entrypoint"></a>Päivittäminen aloituskohta

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

`Entrypoint` Tiedostojen palvelutiedosto osan käytetään sekä määrittää käynnistää palvelu. `ExeHost` Elementti määrittää suoritettavan (ja argumentit), joka olisi käytettävä käynnistää palvelu.

- `Program`määrittää, jotka on suoritettava-palvelun käynnistäminen suoritettavan tiedoston nimi.
- `Arguments`määrittää, että suoritettavan tiedoston välitetään argumentit. Parametrien argumentit luettelo voi olla.
- `WorkingFolder`määrittää prosessi, jossa suorittaminen aloitettavaksi työpäivän kansion. Voit määrittää kolme arvoa:
    - `CodeBase`määrittää käytön hakemiston aiotaan voi määrittää sovelluspaketin koodi-kansio (`Code` hakemistoon edellisen tiedostorakenne).
    - `CodePackage`määrittää käytön hakemiston aiotaan voi määrittää sovelluspaketin ylimmällä (`GuestService1Pkg` edellisen tiedoston rakenteessa).
  - `Work`määrittää, että tiedostot sijoitetaan alikansio nimeltä työmäärä

Voit määrittää oikean käytön hakemiston suhteelliset polut voidaan käyttää sovelluksen tai alustus komentosarjoja kannattaa WorkingFolder.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Päivittäminen päätepisteet ja rekisteröimistä nimeäminen palvelun viestintään

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Edellisessä esimerkissä `Endpoint` elementti määrittää, sovelluksen voit kuunnella päätepisteet. Tässä esimerkissä Node.js sovelluksen seuraa http porttiin 3000.

Lisäksi voit esittää palvelun kangasta tämän päätepisteen julkaiseminen nimeäminen-palvelu, jotta muiden palvelujen löytävät tämän palvelun päätepisteosoite. Näin voit voi olla yhteydessä palvelut, jotka ovat Vieras suoritettavat välillä.
Julkaistun päätepisteosoite on muotoa `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`ja `PathSuffix` on valinnainen määritteet. `IPAddressOrFQDN`on IP-osoite tai täydellinen toimialuenimi solmun tätä suoritettavaa saa markkinoille ja lasketaan puolestasi.

Seuraavassa esimerkissä palvelu on otettu käyttöön, kun palvelua kangasta Explorerissa näet päätepisteen samalla `http://10.1.4.92:3000/myapp/` julkaistu palvelun esiintymän tai jos kyseessä on paikallinen kone, näet `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Voit käyttää näitä osoitteita [käänteisen välityspalvelimen](service-fabric-reverseproxy.md) viestintä-palveluiden välillä.

### <a name="edit-the-application-manifest-file"></a>Sovelluksen tiedostojen tiedoston muokkaaminen

Kun olet määrittänyt `Servicemanifest.xml` tiedostoa, sinun täytyy tehdä muutoksia, `ApplicationManifest.xml` varmistaa, että oikea palvelutyyppi ja nimeä käytetään tiedosto.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

Valitse `ServiceManifestImport` elementti, voit määrittää yhden tai useamman palvelut, jotka haluat sisällyttää-sovelluksessa. Palvelujen viitataan kanssa `ServiceManifestName`, joka määrittää hakemiston nimen kohtaa, johon `ServiceManifest.xml` tiedosto sijaitsee.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Kirjaamisen määrittäminen
Vieras suoritettavat on hyötyä näe konsolin lokit voit selvittää, jos sovellus ja määritys-komentosarjoja Näytä virheitä.
Konsolin uudelleenohjaus voidaan määrittää `ServiceManifest.xml` tiedoston avulla `ConsoleRedirection` elementti.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`voidaan uudelleenohjaaminen console (stdout ja stderr) toimimasta kansioon, jotta niiden avulla voidaan varmistaa, että asennuksen tai palvelun kangasta klusterin sovelluksen suorittamisen aikana ei ole virheitä.

    * `FileRetentionCount`määrittää, kuinka monta tiedostojen tallennuksessa käytön hakemiston. Arvon 5, esimerkiksi tarkoittaa edellisen viisi suorituskerran lokitiedostojen tallennetaan toimimasta hakemistossa.
    * `FileMaxSizeInKb`määrittää lokitiedostojen enimmäiskoon.

Lokitiedostojen tallennetaan jokin palvelun käytön kansioita. Jos haluat selvittää, jossa tiedostot sijaitsevat, palvelun kangasta Resurssienhallinnan avulla voit määrittää, että palvelu on käytössä ja mitkä työkansion käytetään solmun. Tämä toimenpide käsitellään tämän artikkelin.

## <a name="deployment"></a>Käyttöönotto
Viimeinen vaihe on sovelluksen käyttöönotto. Seuraavaa PowerShell-komentosarjaa esitetään, kuinka voit ottaa käyttöön sovelluksen paikallista kehittämistä-klusterin ja aloita uusi palvelu kangasta-palvelu.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Palvelun kangasta palvelu voidaan ottaa käyttöön eri "määrityksiä." Esimerkiksi se voidaan ottaa yksittäisiä tai useita tapauksia tai se on otettu käyttöön siten, että on palvelun kunkin palvelun kangasta klusterin solmun esiintymän.

`InstanceCount` -Parametri `New-ServiceFabricService` cmdlet-komento voidaan määrittää, kuinka monta esiintymää palvelun on käynnistetty palvelun kangasta klusterin. Voit määrittää `InstanceCount` arvo-sovellus, jossa otat tyypin mukaan. Kaksi eniten Yleisiä tilanteita, joissa on:

* `InstanceCount = "1"`. Tässä tapauksessa vain yhden esiintymän palvelu on otettu käyttöön klusterin. Palvelun kangasta ajoitus määrittää solmun palvelun siirtymällä otettu käyttöön.

* `InstanceCount ="-1"`. Tässä tapauksessa esiintymän palvelu on otettu käyttöön jokaisen palvelun kangasta klusterin solmun. Tulos on on yksi (ja vain yksi) palvelun kunkin solmun esiintymän klusterin.

Tämä on palvelinkokoonpanon edusta-sovellusten (esimerkiksi REST-päätepisteen), koska asiakassovelluksissa tarvitse "muodostaa"-klusterin käyttämään päätepisteen solmut. Tässä määrityksessä voidaan myös, kun esimerkiksi kaikki solmut palvelun kangasta klusterin yhdistetään kuormituksen tasauspalvelun niin Asiakkaan liikenne voidaan jaetaan palvelu, joka toimii kaikissa klusterin solmuissa.

## <a name="check-your-running-application"></a>Tarkista käytössä oleva sovellus

Kangasta Resurssienhallinnassa tunnistaa solmu, jossa palvelu on käynnissä. Tässä esimerkissä se suoritetaan Node1:

![Solmun, jossa palvelu on käynnissä](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Jos Siirry solmu ja Etsi sovellus, näet olennaiset solmu-tiedot, mukaan lukien sen sijainnin levyllä.

![Sijainnin levyllä](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Jos siirryt hakemiston palvelimen Resurssienhallinnan avulla, voit etsiä käytön hakemiston ja palvelun log kansion kuvan seuraavasti.

![Lokitiedostojen sijainti](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa on opit Vieras suoritettavat pakkaaminen ja ota se käyttöön palvelun kangasta. Seuraavassa vaiheessa voit tarkistaa tämän ohjeaiheen lisäsisältöä.

- [Esimerkki pakkaus ja käyttöönotto suoritettavan GitHub Vieras](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication)esijulkaisuversion pakkaus-työkalun linkki
- [Ottaa käyttöön useita Vieras suoritettavat](service-fabric-deploy-multiple-apps.md)
- [Luo ensimmäisen sovelluksen palvelun kangasta Visual Studiossa](service-fabric-create-your-first-application-in-visual-studio.md)
