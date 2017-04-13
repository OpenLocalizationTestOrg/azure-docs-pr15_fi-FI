<properties
   pageTitle="Ottaa käyttöön Node.js-sovellus, joka käyttää MongoDB | Microsoft Azure"
   description="Vaiheittainen kuvaus siitä, miten voit pakata useita Vieras suoritettavat Azure palvelun kangasta klusteri otettavat"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Ottaa käyttöön useita Vieras suoritettavat

Tässä artikkelissa kerrotaan pakkaaminen ja ottaa käyttöön useita Vieras suoritettavat Azure palvelun kangasta. Ja käyttöönottoon yhden palvelun kangasta paketin lukea [suoritettavan palvelun kangasta Vieras](service-fabric-deploy-existing-app.md)ottamisesta käyttöön.

Kun tätä vaiheittaista näyttää, miten sovelluksen kanssa Node.js edusta, joka käyttää MongoDB tietovaraston ottaminen käyttöön, voit käyttää ohjeita sovellus, jossa on riippuvainen toisesta sovelluksesta.   

Voit käyttää Visual Studio tuottamaan sovelluspaketin, joka sisältää useita Vieras suoritettavat. Katso [Käyttämällä Visual Studio pakkaaminen sovellukseen](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Sen jälkeen, kun olet lisännyt ensimmäisen Vieras suoritettavan tiedoston, napsauta sovelluksen projektin hiiren kakkospainikkeella ja valitse **Lisää uusi palvelu kangasta service ->** Lisää toinen Vieras suoritettavan projekti ratkaisu. Huomautus: Jos haluat linkittää Visual Studio projektin lähde-luominen Visual Studio-ratkaisun varmistaa oman sovelluspaketin on ajan tasalla tietolähteen muutokset. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Pakkaa manuaalisesti useita Vieras suoritettava sovellus
Voit myös pakata suoritettavan Vieras manuaalisesti. Tässä artikkelissa käyttää manuaalista pakkaus työkalun palvelun kangasta pakkaus, joka on saatavilla osoitteessa [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Pakkaamista Node.js-sovellus
Tässä artikkelissa oletetaan, että Node.js ei ole asennettu-palvelun kangasta-klusterin solmut. Seurauksena on lisättävä Node.exe ennen pakkaus solmu sovelluksen pääkansiossa. Kansiorakenteen (joko Express web framework ja Jade mallin moduuli) Node.js-sovelluksen pitäisi muistuttaa alla:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Seuraavassa vaiheessa voit luoda Node.js sovelluksen sovelluspaketin. Seuraava koodi luo palvelun kangasta sovelluspaketin, joka sisältää Node.js-sovellus.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Alla on parametrit, joita käytetään kuvaus:

- **/Source** osoittaa sovellus, joka on pakattu kansio.
- **/ Target** määrittää hakemiston, jossa paketin luotu. Tämä kansio on oltava eri lähde-hakemistosta.
- **/AppName** määrittää olemassa olevan sovelluksen sovelluksen nimi. On tärkeää ymmärtää, että tämä vastaa palvelun nimen luettelossa, mutta ei toiseen palvelun kangasta sovelluksen nimi.
- **/exe** määrittää suoritettavan tiedoston, joka käynnistää tässä tapauksessa on tarkoitettu palvelun kangasta `node.exe`.
- **/ma** määrittää argumentin, joka käynnistää suoritettavan tiedoston käytetään. Kun Node.js ei ole asennettu, palvelun kangasta tarvitsee käynnistää Node.js verkkopalvelin suorittamalla `node.exe bin/www`.  `/ma:'bin/www'`kertoo pakkaus työkalun `bin/ma` node.exe argumenttina.
- **/AppType** määrittää palvelun kangasta sovelluksen nimi.

Selaa kansioon, joka on määritetty/target-parametria, jos näet, että työkalu on luonut toimiva palvelun kangasta-paketti alla kuvatulla tavalla:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Luotu ServiceManifest.xml on nyt osa, joka kuvaa, miten jostain Node.js verkkopalvelimeen, alla koodikatkelman esitetyllä tavalla:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Tässä esimerkissä Node.js verkkopalvelin seuraa porttiin 3000, jotta sinun on päivitettävä päätepisteen tiedot ServiceManifest.xml tiedoston alla kuvatulla tavalla.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Pakkaamista MongoDB-sovellus
Voit nyt, kun on pakattu Node.js-sovelluksen, siirry eteenpäin ja pakata MongoDB. Kuin edellä mainittuja ohjeita, kun siirryt nyt – eivät ole Node.js ja MongoDB. Itse asiassa ohjeet koskevat myös kaikki sovellukset, jotka on tarkoitettu koottu yhteen palvelun kangasta-sovelluksina.  

Haluat pakata MongoDB, varmista, että Mongod.exe ja Mongo.exe pakata. Molemmat binaaritiedostot sijaitsevat `bin` hakemistoon MongoDB asennuksen hakemistossa. Alla kansiorakenne seuraavankaltaiselta.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Palvelun kangasta on käynnistettävä MongoDB samalta kuin komennolla alla, joten sinun on käytettävä `/ma` parametri, kun pakkaamista MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Tiedot on ei säilytetä kyseessä solmun virheen Jos asetat MongoDB datakansiossa solmun paikallisen kansion. On joko kestävät tallennusvälineiden tai toteuta MongoDB replikajoukon jotta et menetä tietoja.  

PowerShellin tai komentoliittymän, emme pakkaus-työkalun suorittaminen seuraavilla parametreilla:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Jotta voit lisätä MongoDB palvelun kangasta sovelluspaketin, haluat varmistaa, että/target parametrin kohdeosoite samaan kansioon, joka sisältää jo sovelluksen luettelon Node.js-sovelluksen kanssa. Haluat myös varmistaa, että käytät ApplicationType nimi.

Oletetaan, että kansio selaamalla ja tutkia työkalu on luotu.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Kuten näet, työkalu lisätään uusi kansio MongoDB-kansio, joka sisältää MongoDB binaaritiedostoja. Jos avaat `ApplicationManifest.xml` tiedoston, voit tarkastella paketti sisältää nyt Node.js sovelluksen ja MongoDB. Seuraava koodi näkyy sovelluksen luettelo sisällöstä.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Sovelluksen julkaiseminen
Viimeinen vaihe on julkaista palvelun kangasta paikallisen klusterin sovelluksen käyttämällä PowerShell-komentosarjojen:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Kun sovellus on julkaistu paikallisen klusterin, voit käyttää Node.js sovelluksen porttiin, joka on antanut Node.js sovelluksen – esimerkiksi http://localhost:3000 service-luettelossa.

Tässä opetusohjelmassa tullut pakkaaminen helposti palvelun kangasta sovelluksena kaksi sovellukset. Opit on myös ottamisesta käyttöön palvelun kangasta niin, että se voi olla hyötyä joistakin palvelun kangasta-ominaisuudet, kuten suuri käytettävyys ja järjestelmän kunto-integrointi.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [palvelun kangasta ja säilöjä](service-fabric-containers-overview.md) yleiskuvauksessa säilöjen käyttöönotto
