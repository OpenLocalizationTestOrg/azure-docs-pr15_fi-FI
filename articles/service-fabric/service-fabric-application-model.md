<properties
   pageTitle="Palvelun kangasta sovelluksen mallin | Microsoft Azure"
   description="Miten malli ja kuvaamiseen sovellusten ja palveluiden palvelun kangasta."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Palvelun kangasta sovelluksen malli

Tässä artikkelissa on yleiskatsaus Azure palvelun kangasta-sovelluksen malli. Se myös kerrotaan, miten voit määrittää sovelluksen ja palvelun kautta luettelon tiedostoja ja hanki sovellus pakattu ja valmis käyttöönottoa varten.

## <a name="understand-the-application-model"></a>Tietoja sovelluksen malli

Sovellus on kokoelma tuotteen palvelut, jotka suorittavat tiettyä toimintoa tai -funktioita. Palvelun suorittaa täydellinen ja erillinen funktion (se voi alkamis- ja muiden palveluiden erillisenä) ja koostuu koodi ja määritykset. Jokaisen palvelun koodissa on suoritettava binaaritiedostot, määritys koostuu Palveluasetukset, jota voi ladata suorituksen aikana ja tietojen koostuu haluamaansa staattinen tietojen kulutettu palvelu. Jokaisen osan hierarkkisia sovelluksen tämän mallin voidaan versiotietoja ja päivitetyn erikseen.

![Palvelun kangasta-sovelluksen malli][appmodel-diagram]


Sovelluksen tyyppi on sovelluksen luokittelu ja koostuu paljon palveluita. Palvelutyyppi on palvelun luokitus. Luokittelu voi olla eri asetukset ja määritykset, mutta perustoimintoihin säilyy ennallaan. Palvelun esiintymät ovat eri määritysten variaatioita palvelun samantyyppisten.  

Luokkien (tai "tyypit"), sovellusten ja palveluiden on kuvattu kautta XML-tiedostot (sovelluksen luettelot ja palvelun luettelot), jotka ovat malleja, joita vastaan sovellukset voidaan luoda klusterin kuva-kaupasta. Rakenteen määritys ServiceManifest.xml ja ApplicationManifest.xml tiedoston on asennettu palvelun kangasta SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*-työkaluja.

Erillisten prosessien myös silloin, kun samaa palvelun kangasta solmu ylläpitämä suoritetaan eri Palvelusovellusten esiintymät koodi. Lisäksi sovelluksen jokaiselle esiintymälle elinkaari hallittavissa (eli päivittää) erikseen. Seuraavassa kaaviossa on esitetty, miten sovelluksen tyypit koostuvat palvelutyypit, joka puolestaan koostuvat koodi, määrittäminen ja paketit. Jos kaaviosta halutaan yksinkertaisempi, vain koodin/config/tiedot paketteja `ServiceType4` näkyvät, vaikka näkyy tällöin kunkin palvelutyyppi jotkin tai kaikki ne pakata tyyppejä.

![Palvelun kangasta sovelluksen ja palvelun tietotyypeistä][cluster-imagestore-apptypes]

Kahden eri luettelon tiedostoja käytetään kuvaus, sovellusten ja palveluiden: service-luettelo ja sovellusluettelo. Näihin kuuluvat niille osien yksityiskohtaiset tiedot.

Yhden tai useamman esiintymät palvelun laji voi olla käytössä klusterin. Esimerkiksi tilallisten palveluesiintymiä tai replikoita, tehostaa hyvin luotettavuutta mukaan replikoiminen tila-klusterin eri solmut sijaitsevat replikoiden välillä. Tämän replikoinnin tarjoaa olennaisesti redundanssin palvelun on käytettävissä, vaikka yhden solmun klusterin epäonnistuu. [Osioitu palvelun](service-fabric-concepts-partitioning.md) edelleen jakaa sen tila (ja access-kuvioiden samaan tilaan)-klusterin solmut yli.

Seuraavassa kaaviossa näkyvät sovellukset ja palvelun esiintymät, osiot ja replikoiden välisen suhteen.

![Osiot ja replikoiden palvelu sisällä][cluster-application-instances]


>[AZURE.TIP] Voit tarkastella käytettävissä palvelun kangasta Explorer-työkalun avulla osoitteessa http:// klusterin sovellusten asettelun&lt;yourclusteraddress&gt;: 19080/Explorer. Lisätietoja on artikkelissa [havainnollistetaan usein yhteyttä klusterin kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Palvelun kuvaus

Palvelun luettelo määrittelee määrittämisen avulla palvelutyyppi ja versio. Määrittää palvelun metatietojen, kuten palvelutyyppi, terveys-ominaisuudet, kuormituksen tasaamisen mittarit, palvelun binaaritiedostot ja tiedostojen.  Toisin sanoen se kuvaa koodi ja määritysten paketteja, jotka muodostavat palvelupakettiin tukemaan yhden tai usean palvelutyypit. Tässä on esimerkki palvelun luettelo:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Version** määritteet ovat rakenteeton merkkijonoja, ja järjestelmä ei jäsentää. Nämä ovat käytetään versio jokaisen osan päivityksiä varten.

**ServiceTypes** ilmoittaa palvelun mitä tiedostotyyppejä tuetaan **CodePackages** mukaan tätä luetteloa. Palvelu on vahvistaa palvelun tällaisia vastaan, kun kaikki tämän luettelon ilmoitettu koodi-paketit aktivoidaan suorittamalla niiden käyttötavat. Tuloksena prosessit odotetaan rekisteröidä palvelun Tuetut tiedostotyypit suorituksen aikana. Huomautus palvelutyypit ilmoitetaan luettelon tason ja ei koodin paketti-tasolla. Niin kun luettelossa on useita koodi-paketteja, ne kaikki aktivoidaan aina, kun järjestelmä etsii jokin ilmoitettu palvelun tyyppejä.

**SetupEntryPoint** on sellaisten aloituskohtaa, joka suoritetaan muita aloituskohtaa ennen kuin palvelun kangasta (yleensä *LocalSystem* -tili) samoilla tunnuksilla. **Aloituskohta** määrittämää suoritettavat on yleensä pitkään suoritettavien isännöinti. Erillisen asennuksen aloituskohtaa esiintyminen ei tarvitse suorittaa palvelun host hyvin oikeuksilla pitkän ajan. **Aloituskohta** määrittämää suoritettavat suoritetaan, kun **SetupEntryPoint** poistuu onnistuneesti. Tuloksena on seurattava ja käynnistetään uudelleen (Aloita uudelleen **SetupEntryPoint**), jos koskaan lopettaa tai kaatuu.

**DataPackage** ilmoittaa kansion nimi **nimi** -määrite, joka sisältää haluamaansa staattinen tietojen kulutettu prosessin suorituksen aikana.

**ConfigPackage** ilmoittaa kansion nimi **nimi** -määrite, joka sisältää *Settings.xml* . Tiedoston nimi sisältää osien käyttäjän määrittämä, avain-arvo pari asetuksia, jotka prosessi voi lukea takaisin suorituksen aikana. Päivityksen aikana vain **ConfigPackage** - **versio** on muuttunut, valitse käynnissä prosessi ei käynnistetään. Sen sijaan takaisinkutsun ilmoittaa prosessi, jossa asetukset ovat muuttuneet, jotta he voivat ladataan dynaamisesti. Tässä on esimerkki *Settings.xml* tiedosto:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Palvelun luettelo voi olla useita koodi, määrittäminen ja tietojen paketit. Kussakin voi olla versiotietoja erikseen.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Sovelluksen kuvaus


Sovelluksen luettelo määrittämisen avulla kuvataan sovelluksen tyyppi ja versio. Määrittää palvelun yhdistelmä metatietoja, kuten vakaata nimiä, jakaminen värimallin, esiintymän Laske/replikoinnin kerroin, suojaus ja eristystaso käytännön, sijaintia rajoitukset, määritys ohittaa ja tuotteen service tyypit. Kuormituksen tasaamisen toimialueet, johon sovelluksen sijoitetaan on myös artikkelissa.

Näin ollen sovellusluettelo on kuvattu sovelluksen tasolla osat ja viittaa vähintään yksi palvelun luettelot Luo sovelluksen tyyppi. Tässä on esimerkki sovellusluettelo:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Palvelun luettelot, kuten **Version** määritteet ovat rakenteeton merkkijonoja, ja järjestelmä ei jäsentää. Nämä ovat myös käyttää versio jokaisen osan päivityksiä varten.

**ServiceManifestImport** sisältää viittauksia palvelun luettelot, jotka muodostavat tämän sovelluksen tyyppi. Tuodut palvelun luettelot määrittävät palvelun mitä tiedostotyyppejä ovat kelvollisia tämän sovelluksen tyyppi.

**DefaultServices** ilmoittaa palvelun esiintymät, jotka luodaan automaattisesti aina, kun sovellus on vahvistaa vastaan tämän sovelluksen tyyppi. Oletus-palvelut vain helppokäyttöisyys ja toimivat kuten Normaali services kaikilta osin, kun ne on luotu. Ne yhdessä muiden palveluiden sovelluksen esiintymän päivittää ja poistaa myös.

> [AZURE.NOTE] Sovellusluettelo voi olla useita service-tiedostojen tuonti ja oletusarvo-palvelut. Jokaisen palvelun tuotu voi olla versiotietoja erikseen.

Lisätietoja säilyttää toiseen sovellukseen ja parametreja yksittäisiä ympäristössä, katso [hallinta sovelluksen parametrit useita ympäristössä](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Sovelluksen pakkaaminen

### <a name="package-layout"></a>Paketti-asettelu

Sovelluksen luettelo, palvelun manifest(s) ja muut tarvittavat pakettitiedostot on järjestettävä palvelun kangasta klusterin käyttöönoton tietyn asettelun. Esimerkki luettelot tässä artikkelissa on järjestettävä seuraava kansiorakenne:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Kansiot nimetään vastaamaan kunkin vastaavan osan **nimi** määritteet. Esimerkiksi jos service-luettelo sisältää kaksi koodi-paketteja, joissa on **MyCodeA** ja **MyCodeB**nimet, valitse kansioilla samannimistä sisältää tarvittavat kunkin koodin paketti binaaritiedostoja.

### <a name="use-setupentrypoint"></a>Käytä SetupEntryPoint

Tyypillinen **SetupEntryPoint** käytön skenaariot ovat, jos sinun on suoritettava suoritettavan ennen palvelun käynnistymistä tai sinun täytyy suorittaa toiminnon oikeuksin. Esimerkki:

- Määrittämisestä ja valmistellaan ympäristömuuttujat service suoritettavan tiedoston korjattava. Ei rajoitettu vain ne tiedostot, jotka on kirjoitettu palvelun kangasta ohjelmoinnin mallien kautta. Esimerkiksi npm.exe on joitakin ympäristömuuttujien käyttöönotto node.js sovelluksen määritetty.

- Käyttöoikeuksien hallinta asentamalla suojauksen varmenteet määrittämisen.

### <a name="build-a-package-by-using-visual-studio"></a>Paketin luominen Visual Studion avulla

Jos Visual Studio 2015 avulla voit luoda sovelluksen, voit paketti-komento, joka vastaa edellä kuvatun asettelun pakkauksen luominen automaattisesti.

Pakkauksen luominen ratkaisunhallinnassa sovelluksen projektin hiiren kakkospainikkeella ja valitse paketti-komennon alla kuvatulla tavalla:

![Sovelluksen Visual Studiossa pakkaamista][vs-package-command]

Kun pakkaaminen on valmis, etsii pakkauksen sijainti **kohde** -ikkunassa. Huomautus pakkaus vaihe tapahtuu automaattisesti, kun ottaminen käyttöön tai Visual Studiossa sovelluksen korjaaminen.

### <a name="test-the-package"></a>Paketin testaaminen

Voit tarkistaa paketin rakenteen paikallisesti PowerShellin kautta **Testi ServiceFabricApplicationPackage** -komennon avulla. Tämä komento luettelon jäsentäminen ongelmat etsiä ja Tarkista kaikki viittaukset. Huomaa, että tämä komento tarkistaa vain kansiot ja tiedostot Pakkaaminen rakenteellisia oikeellisuuden. Se tarkistaa ei mitään koodin tai tiedon paketin sisällön lisäksi tarkistaa, että kaikki tarvittavat tiedostot ovat näkyvissä.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Tämä virhe näkyy, että palvelun luettelo **SetupEntryPoint** Viitattu *MySetup.bat* tiedosto puuttuu koodin paketista. Kun puuttuvan tiedoston on lisätty, sovelluksen vahvistus välittää:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Kun sovellus on pakattu oikein ja välittää vahvistus on valmis käyttöönottoa varten.

## <a name="next-steps"></a>Seuraavat vaiheet

[Ottaa käyttöön ja poista sovellukset][10]

[Sovelluksen parametrit useita ympäristössä hallinta][11]

[RunAs: Palvelun kangasta-sovelluksen käytön eri käyttöoikeuksia][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
