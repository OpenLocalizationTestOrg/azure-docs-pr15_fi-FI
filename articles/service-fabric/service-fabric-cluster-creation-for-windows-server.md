<properties
   pageTitle="Voit luoda ja hallita erillinen Azure palvelun kangasta klusterin | Microsoft Azure"
   description="Voit luoda ja hallita Azure palvelun kangasta-klusterin koneessa (fyysinen tai virtual) käytössä Windows Server, olipa paikallisen tai minkä tahansa pilveen."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Voit luoda ja hallita klusterin Windows Serverissä

Azure-palvelu kangasta voit luoda palvelun kangasta klustereiden näennäiskoneiden tai Windows Server-tietokoneissa. Tämä tarkoittaa sitä, voit ottaa käyttöön ja suorittaa palvelun kangasta sovelluksia ympäristössä, joka sisältää yhdistetyt Windows Server-tietokoneiden, se on paikallinen tai minkä tahansa cloud-palvelussa. Palvelun kangasta on asennuspaketin luominen palvelun kangasta klustereiden kutsutaan erillisversion Windows Server-paketti.

Tässä artikkelissa käydään läpi luomisen klusterin käyttämällä erillisversion paketti palvelun kangasta paikallisesti, mutta sen voi helposti mukautettava muussa ympäristössä, kuten toimittajia cloud vaiheet.

>[AZURE.NOTE] Tämä erillinen Windows Server-paketti sisältää ominaisuuksia, jotka ovat tällä hetkellä esikatselu ja kaupallista käyttöä ei tueta. Luettelo toiminnoista, jotka ovat esikatselu on ohjeartikkelissa "esikatselutoiminnot sisällyttää tämän paketin." Voit myös [käyttöoikeussopimuksen kopion lataaminen](http://go.microsoft.com/fwlink/?LinkID=733084) .


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Ottaa yhteyttä tukeen palvelun kangasta erillisversion paketti

- Kysy yhteisön palvelun kangasta erillisversion paketti Windows Server [Azure-palvelu kangasta keskustelupalstalle](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Avaa lippu [Professional](http://support.microsoft.com/oas/default.aspx?prid=16146 )tuki-palvelun kangasta.  Lisätietoja [Professional tukea Microsoftilta](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Lataa palvelun kangasta erillisversion paketti


[Lataa erillisversion paketti palvelun kangasta for Windows Server 2012 R2 ja uudempi versio](http://go.microsoft.com/fwlink/?LinkId=730690), jonka nimi on Microsoft.Azure.ServiceFabric.WindowsServer. &lt;versio&gt;.zip.


Etsii paketti, seuraavat tiedostot:

|**Tiedostonimi**|**Lyhyt kuvaus**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|.Cab-tiedosto, joka sisältää binaaritiedostoja, jotka on otettu klusterin kunkin tietokoneeseen.|
|ClusterConfig.Unsecure.DevCluster.json|Klusterin määritykset mallitiedosto, joka sisältää luodun suojaamaton, kolme-solmu, yksittäisen tietokoneen (tai virtuaalikoneen) kehittäminen klusterin, mukaan lukien kunkin solmun tiedot klusterin asetukset. |
|ClusterConfig.Unsecure.MultiMachine.json|Klusterin määritykset mallitiedosto, joka sisältää luodun suojaamaton, usean tietokoneen (tai virtuaalikoneen) klusterin, mukaan lukien kunkin tietokoneen tiedot klusterin asetukset.|
|ClusterConfig.Windows.DevCluster.json|Klusterin määritykset mallitiedosto, joka sisältää kaikki suojatun, kolme-solmu, yksittäisen tietokoneen (tai virtuaalikoneen) kehittäminen klusterin, mukaan lukien kunkin solmun, joka on klusterin tiedot asetukset. Klusterin on suojattu käyttämällä [Windows-tunnistetiedot](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.json|Klusterin määritykset mallitiedosto, joka sisältää Windows-suojauksen, mukaan lukien tiedot Jokaisessa koneessa, joka on suojattu klusterin turvallinen, usean tietokoneen (tai virtuaalikoneen) klusterin asetukset. Klusterin on suojattu käyttämällä [Windows-tunnistetiedot](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.x509.DevCluster.json|Klusterin määritykset mallitiedosto, joka sisältää kaikki suojatun, kolme-solmu, yksittäisen tietokoneen (tai virtuaalikoneen) kehittäminen klusterin, mukaan lukien kunkin solmun tiedot klusterin asetukset. Klusterin on suojattu x509 varmenteet.|
|ClusterConfig.x509.MultiMachine.json|Klusterin määritykset mallitiedosto, joka sisältää suojatun, usean tietokoneen (tai virtuaalikoneen) klusterin, mukaan lukien kunkin solmun tiedot suojatun klusterin asetukset. Klusterin on suojattu x509 varmenteet.|
|EULA_ENU.txt|Käyttöoikeussopimuksen ehdot Microsoft Azure palvelun kangasta erillisversion Windows Server-paketti, käyttö. Voit nyt [ladata käyttöoikeussopimuksen kopion](http://go.microsoft.com/fwlink/?LinkID=733084) .|
|Lueminut.txt|Linkki julkaisutiedot ja basic asennusohjeita. Ohjeita tämän asiakirjan alijoukkoa on.|
|CreateServiceFabricCluster.ps1|PowerShell-komentosarja, joka luo klusterin ClusterConfig.json-asetusten avulla.|
|RemoveServiceFabricCluster.ps1|PowerShell-komentosarja, joka poistaa klusterin ClusterConfig.json-asetusten avulla.|
|ThirdPartyNotice.rtf |Kolmannen osapuolen ohjelmiston pakkauksen on ilmoitus.|
|AddNode.ps1|Aiemmin luodun solmun lisääminen PowerShell-komentosarja käyttöön klusterin.|
|RemoveNode.ps1|PowerShell-komentosarjaa poistaminen solmu luodusta käyttöön klusterin.|
|CleanFabric.ps1|PowerShell-komentosarjaa varten puhdistus yksittäisen palvelun kangasta asennuksen käytöstä nykyiseen tietokoneeseen. Aiemmat MSI-asennukset voi poistaa käyttämällä omia liittyvät uninstallers.|
|TestConfiguration.ps1|PowerShell-komentosarjaa analysointiin infrastruktuuri Cluster.json mukaisesti.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Suunnittelu ja klusterikäyttöönoton valmisteleminen
Suorita seuraavat vaiheet, ennen kuin luot yhteyttä klusterin.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Vaihe 1: Klusterin-infrastruktuurin suunnitteleminen
Aiot luoda palvelun kangasta klusterin tietokoneissa omistat, niin voit päättää virheiden luotavat klusterin uutta käyttöä varten. Esimerkiksi on erotat power viivojen tai Internet-yhteydet nämä koneet yhdistettävän? Lisäksi huomioon nämä koneet fyysinen turvallisuus. Missä koneet sijaitsevat ja käyttää niitä sen? Kun olet tehnyt nämä päätökset, voit yhdistää koneet loogisesti eri vika toimialueet (Katso vaiheessa 4). Tuotannon klustereiden suunnitteleminen infrastruktuuri on enemmän aikaa kuin testi klustereiden varten.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Vaihe 2: Valmisteleminen koneet täyttävät tietyt edellytykset
Jokainen tietokone, johon haluat lisätä klusterin edellytykset:

- Vähintään 16 gt RAM-muistia suositellaan.
- Vähintään 40 Gigatavua vapaata kiintolevytilaa on suositeltavaa.
- 4 core tai suurempi suorittimen suositellaan.
- Yhteys suojattuun verkkoon tai verkkojen kaikissa tietokoneissa.
- Windows Server 2012 R2 tai Windows Server 2012 (sinun on oltava asennettuna [KB2858668](https://support.microsoft.com/kb/2858668) ).
- [.NET framework 4.5.1 vähintään](https://www.microsoft.com/download/details.aspx?id=40773), täydellinen asennus.
- [Windows PowerShellin 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- [RemoteRegistry palvelun](https://technet.microsoft.com/library/cc754820) pitäisi olla käynnissä kaikissa tietokoneissa.

Klusterin järjestelmänvalvojaan ottaminen käyttöön ja määrittäminen klusterin on oltava kunkin koneet [järjestelmänvalvojan oikeudet](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) . Et voi asentaa palvelun kangasta toimialueen ohjauskoneen.

### <a name="step-3-determine-the-initial-cluster-size"></a>Vaihe 3: Alkuperäinen klusterin koon määrittäminen
Kukin solmu erillisen palvelun kangasta klusterissa on palvelun kangasta runtime käyttöön ja klusterin kuuluu. Tyypillinen tuotantoympäristössä on yksi solmu OS esiintymän kohden (fyysinen tai virtual). Klusteri määräytyy yrityksesi tarpeisiin. Kuitenkin on oltava vähintään klusterikoko on kolme solmujen (koneet tai näennäiskoneiden).
Tarkoitettu voi olla useita solmu tiettyyn tietokoneeseen. Tuotantoympäristössä-palvelun kangasta tukee vain yksi solmu fyysisiä tai virtuaalikoneen kohden.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Vaihe 4: Määrittää vian toimialuemäärän ja toimialueiden päivittäminen
*Vian toimialueen* (Suodatuspalvelu) on virhe fyysinen yksikkö ja liittyy suoraan fyysinen infrastruktuurin tietojen keskikohdan mukaan. Virhe toimialueen koostuu laitteistosta (tietokoneissa, valitsimet verkkojen tai enemmän), jotka jakavat virheen yhden pisteen. Vaikka ei ole 1:1 yhdistämistä vika toimialueet ja tavara välillä, löyhästi puhumisen, kunkin Teline voidaan pitää vika toimialue. Kun-yhteyttä klusterin solmut, on suositeltavaa solmut voi jakaa vähintään kolme vika toimialueet.

Kun määrität FDs ClusterConfig.json, voit valita kunkin Suodatuspalvelu nimi. Palvelun kangasta tukee hierarkkisia FDs, joten voit osoittaa infrastruktuuri-topologian niitä.  Esimerkiksi seuraavat FDs ovat kelvollisia:

- "faultDomain": "Suodatuspalvelu: / Room1/Rack1/KONE1"
- "faultDomain": "Suodatuspalvelu: / FD1"
- "faultDomain": "Suodatuspalvelu: / Room1/Rack1/PDU1/M1"


*Päivitä toimialueen* (UD) on looginen yksikkö solmujen. Aikana palvelun kangasta koordinoitu päivityksiä (sovelluksen päivitys tai klusterin-päivitys) kaikki solmut UD on otettava päivittämisestä, kun taas muut UDs solmujen ovat kuitenkin käytettävissä Käsittele pyyntöjä. Voit suorittaa tietokoneesi laitteisto-päivityksiä ei tukee UDs, niin tee ne jokin kuormitusryhmälle kerrallaan.

Helpoin tapa ottaa huomioon käsitteistä on huomioitavia FDs suunnitellun ylläpidon yksikkönä suunnittelematon järjestelmävirheiden ja UDs yksiköksi.

Kun määrität UDs ClusterConfig.json, voit valita kunkin UD nimi. Esimerkiksi seuraavat nimet ovat kelvollisia:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Sininen"

Katso tarkempia tietoja päivityksen toimialueet ja vika toimialueiden [kuvaava palvelun kangasta klusterin](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Vaihe 5: Lataa palvelun kangasta erillisversion paketti Windows Server
[Lataa Windows Server Service kangasta erillisversion paketti](http://go.microsoft.com/fwlink/?LinkId=730690) ja Pura paketti, joka ei kuulu klusterin käyttöönoton tietokoneeseen tai johonkin koneet, joka on osa yhteyttä klusterin. Wsp kansion saattaa nimen `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Yhteyttä klusterin luominen

Suunnittelu ja valmistelu vaiheet ovat kadonneet, kun olet valmis luomaan yhteyttä klusterin.

### <a name="step-1-modify-cluster-configuration"></a>Vaihe 1: Muokkaa klusterimääritys
Klusterin on kuvattu ClusterConfig.json. Lisätietoja tämän tiedoston osissa on artikkelissa [erillinen Windows-klusterin asetukset](service-fabric-cluster-manifest.md).
Avaa jokin ClusterConfig.json lataamaasi paketista ja muuttaa seuraavia asetuksia:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Hakumäärityksen asetus**|**Kuvaus**|
|-----------------------|--------------------------|
|**NodeTypes**|Solmutyypit mahdollistavat klusterisolmujen erottaa eri ryhmiin. Klusterin on oltava vähintään yksi NodeType. Kaikki solmut ryhmän on seuraavat yhteiset ominaisuudet: <br> **Nimi** - tämä on solmu tyyppinimi. <br>**Päätepisteen portit** - nämä ovat eri nimetty päätepisteestä (portit), jotka liittyvät solmu tällaista. Voit käyttää mitä tahansa porttinumero, jonka haluat, että, kunhan ne eivät ole ristiriidassa mihinkään muuhun tämän luettelon kanssa ja joita ei ole jo muita tietokoneen/AM käytössä sovelluksen. <br> **Asettelun ominaisuudet** - ne kuvaavat solmu tällaista, jota käyttää sijaintia rajoitukset järjestelmän palveluita tai palvelujen ominaisuuksia. Nämä ominaisuudet ovat käyttäjän määrittämä avain/arvo-parit, jotka sisältävät tietyn solmu ylimääräisiä metatiedot. Esimerkkejä solmujen ominaisuudet olisi onko solmu kiintolevyn tai näyttösovittimen, hammaspyöriä sen kiintolevylle, Sydämiä ja muiden fyysisten ominaisuuksien määrä. <br> **Valmiuksia** - solmu kapasiteetin määrittäminen nimi ja määrä tietyn resurssin, erityisesti solmu on käytettävissä oleva. Solmun voi esimerkiksi määrittää on arvo, jota kutsutaan "MemoryInMb" kapasiteetin ja siinä on 2 048 Megatavua oletusarvoisesti. Nämä valmiuksia käytetään varmistaa, että palvelut, jotka tarvitsevat tietyn määriä resurssit ovat käytettävissä solmuissa tarvittavat summat nämä resurssit sisältävät suorituksen aikana.<br>**IsPrimary** – Jos sinulla on useampi kuin yksi NodeType määritetty varmistaa, että vain yksi on määritetty, ensisijainen ja-arvon *Tosi*, joka on, jossa järjestelmän palveluita suorittaa. Kaikki muut solmu on asetettava-arvon *Epätosi*|
|**Solmujen**|Nämä ovat eri solmujen klusterin (solmutyyppi, Solmunimi, IP-osoite, vika toimialueen ja Päivitä toimialueen solmun) osana olevat tiedot. Koneet haluat klusterin mainittu tässä niiden IP-osoitteita ei tarvitse luoda. <br> Jos käytät kaikkien solmujen sama IP-osoite, valitse yhden klusterin on luotu, jonka avulla voit testausta varten. Älä käytä yhden klustereiden käyttöönoton tuotannon toiminnoista.|


### <a name="step-2-run-the-testconfiguration-script"></a>Vaihe 2: TestConfiguration-komentosarjan suorittaminen

TestConfiguration komentosarjan Testaa infrastruktuurin määritysten mukaisesti ja varmista, että tarvittavat oikeudet määritetään, koneet on yhdistetty toisiinsa ja määritteet määritetään niin, että käyttöönotto voi onnistua cluster.json. Se on lähinnä tarkasteleminen parhaiden käytäntöjen analysointityö. Lisää tämä työkalu lisää vahvistukset ajan, minkä ansiosta tehokkaamman jatketaan.

Tämä komentosarja voidaan suorittaa tietokoneessa, jolla on järjestelmänvalvojan käyttöoikeudet kaikissa tietokoneissa, jotka on lueteltu solmujen klusterin määritystiedostossa nimellä. Tämä komentosarja on käynnissä olevat tietokoneeseen ei ole klusterin osa.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Vaihe 3: Luo klusterin-komentosarjan suorittaminen
Kun olet muokannut klusterin määrityksissä JSON-asiakirjaan ja lisätä kaikki solmu-tiedot, suorita klusterin luominen *CreateServiceFabricCluster.ps1* PowerShell-komentosarjaa paketti-kansiosta ja välittää polussa JSON-kokoonpanotiedosto. Kun tämä on valmis, Hyväksy käyttöoikeussopimus.

Tämä komentosarja voidaan suorittaa tietokoneessa, jolla on järjestelmänvalvojan käyttöoikeudet kaikissa tietokoneissa, jotka on lueteltu solmujen klusterin määritystiedostossa nimellä. Tämä komentosarja on käynnissä olevat tietokoneeseen ei ole klusterin osa.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Käyttöönotto-lokit ovat käytettävissä paikallisesti sisältävään suoritit CreateServiceFabricCluster PowerShell-AM/tietokoneeseen. He ovat alikansiossa DeploymentTraces-kansiossa, jossa suoritit PowerShell-komentoa. Jos palvelun kangasta otettiin oikein tietokoneeseen näkyviin löytyvät asennustiedostot C:\ProgramData hakemiston ja FabricHost.exe ja Fabric.exe prosessit näkevät käynnissä Tehtävienhallinnassa.

### <a name="step-4-connect-to-the-cluster"></a>Vaihe 4: Muodostaa yhteyttä klusterin

Muodostaa suojatun klusterin artikkelissa [palvelun kangasta suojatun klusterin yhdistäminen](service-fabric-connect-to-secure-cluster.md).

Muodostaa yhteyden suojaamaton klusterin suorittaa seuraavan PowerShell-komennon:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Vaihe 5: Noutaa palvelun kangasta Explorer

Nyt voit muodostaa yhteyden palvelun kangasta Resurssienhallinnan joko suoraan jostakin http://localhost:19080/Explorer/index.html tai etäyhteyden http://< koneet klusteri*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Voit lisätä ja poistaa solmujen

Voit lisätä tai poistaa solmujen erillisen palvelun kangasta-klusterin yrityksesi tarpeet muuttuvat. [Lisää tai poista palvelun kangasta erillinen klusterin solmut](service-fabric-cluster-windows-server-add-remove-nodes.md) saat yksityiskohtaiset ohjeet.


## <a name="remove-a-cluster"></a>Poista klusterin

Jos haluat poistaa klusterin, suorita *RemoveServiceFabricCluster.ps1* PowerShell-komentosarjaa paketti-kansiosta ja välittää polussa JSON-kokoonpanotiedosto. Voit myös määrittää lokiin sijainti poiston.

Tämä komentosarja voidaan suorittaa tietokoneessa, jolla on järjestelmänvalvojan käyttöoikeudet kaikissa tietokoneissa, jotka on lueteltu solmujen klusterin määritystiedostossa nimellä. Tämä komentosarja on käynnissä olevat tietokoneeseen ei ole klusterin osa.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemetriatietojen tietojen kerääminen ja miten voit osallistua siitä pois

Tuotteen kerää telemetriatietojen oletuksena, voit parantaa tuotteen Service kangasta-käyttö. Parhaiden käytäntöjen analysointitoiminto, joka suoritetaan osana connectivity asennus etsii [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Jos se ei ole tavoitettavissa, asennus epäonnistuu, ellei osallistua telemetriatietojen ulos.

  1. Telemetriatietojen putkijohto yrittää ladata seuraavat tiedot [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) kerran päivittäin. Se on paras työmäärään Lataa ja ei ole vaikutusta klusterin toimintoja. Solmun, joka suoritetaan vikasietotilaa vain lähettäjä telemetriatietojen hallinnan ensisijainen. Muiden solmujen lähettää telemetriatietojen.

  2. Telemetriatietojen koostuu seuraavasti:

-            Palvelujen määrä
-            ServiceTypes määrä
-            Sovellusten määrä
-            ApplicationUpgrades määrä
-            FailoverUnits määrä
-            InBuildFailoverUnits määrä
-            UnhealthyFailoverUnits määrä
-            Määrä on
-            InBuildReplicas määrä
-            StandByReplicas määrä
-            OfflineReplicas määrä
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Solmujen määrän
-            IsContextComplete: TOSI tai EPÄTOSI
-            ClusterId: Tämä on luotu satunnaisesti kunkin klusterin GUID-tunnus
-            ServiceFabricVersion
-             Virtuaalikoneen tai tietokoneen, josta telemetriatietojen ladataan IP-osoite


Käytöstä telemetriatietojen, Lisää seuraavat *Ominaisuudet* klusterin config: *enableTelemetry: Epätosi*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>Tämän paketin esikatselu ominaisuuksista

Ei mitään.

>[AZURE.NOTE] Uuden [GA erillinen klusterin Windows Server-version (versio 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), voit päivittää yhteyttä klusterin tulevista versioista manuaalisesti tai automaattisesti. Tämä ominaisuus ei ole käytettävissä preview-versio, sinun on klusterin luominen käyttämällä GA-versio ja Siirrä tietoja ja sovelluksia esikatselu-klusterin. Lisätietoja tämän toiminnon tulossa.


## <a name="next-steps"></a>Seuraavat vaiheet
- [Erillinen Windows-klusterin asetukset](service-fabric-cluster-manifest.md)
- [Lisää tai poista solmujen erillisen palvelun kangasta klusteriin](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Luo erillisen palvelun kangasta klusterin Windows Azure virtuaalilaitteiksi](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Suojattu erillinen klusterin Windows käyttämällä Windowsin suojaus](service-fabric-windows-cluster-windows-security.md)
- [Suojattu erillinen klusterin X509 avulla Windowsiin varmenteet](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
