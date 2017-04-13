<properties
   pageTitle="Päivitä erillisen palvelun kangasta-klusterin Windows Server | Microsoft Azure"
   description="Päivittää palvelun kangasta koodi ja/tai määritys, joka suoritetaan erillisen palvelun kangasta klusterin, mukaan lukien klusterin päivityksen tila"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Erillisen palvelun kangasta-klusterin Windows Server-päivitys

> [AZURE.SELECTOR]
- [Azure klusterin](service-fabric-cluster-upgrade.md)
- [Erillisestä klusterin](service-fabric-cluster-upgrade-windows-server.md)

Nykyaikainen järjestelmän upgradability suunnittelemisesta on saavuttamiseksi pitkään success tuotteen-näppäintä. Palvelun kangasta klusteriin on omistat resurssi. Tässä artikkelissa kuvataan, miten voit varmistaa, että klusterin suoritetaan palvelun kangasta tuettujen versioiden aina koodi ja määritykset.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kangasta-versio, joka suoritetaan yhteyttä klusterin hallinta

Voit määrittää yhteyttä klusterin lataaminen kangasta palvelupäivityksistä, kun Microsoft julkaisee uuden version ja valita haluamasi yhteyttä klusterin on tuettu kangasta-version. 

Voit tehdä tämän määrittämällä "fabricClusterAutoupgradeEnabled" klusterin määrityksissä TOSI tai EPÄTOSI.


>[AZURE.NOTE] Varmista, että voit pitää yhteyttä klusterin palvelun kangasta tuettuun aina käynnissä. Kun ja kerromme palvelun kangasta uuden version julkaisemisen, aiemmasta versiosta on merkitty tuki loppuun aikaisintaan 60 päivän päivämäärän. Uudet ovat ilmoitettu [-palvelun kangasta tiimin blogia](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Uudessa versiossa on käytettävissä ja valitse sitten. 


Voit päivittää yhteyttä klusterin uuteen versioon vain, jos käytät tuotannon tyylin solmun kokoonpanossa, jossa kunkin palvelun kangasta solmun on varattu erillinen fyysisiä tai virtuaalikoneen. Jos sinulla on kehittäminen-klusterin on useampi kuin yksi palvelun kangasta solmujen yksittäisen fyysisiä tai virtuaalikoneen, tear yhteyttä klusterin alaspäin ja sen uudelleen uudella versiolla.


On kaksi eri työnkulut päivittäminen yhteyttä klusterin uusimmat tai tuettujen service kangasta versio. Yksi klustereiden, joka on yhteys, voit ladata automaattisesti uusimpaan versioon ja klustereiden, jotka ovat ilman yhteyttä palvelun kangasta uusimman lataaminen toinen.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Päivitä varausyksiköiden yhteyttä Lataa uusin koodi ja määritykset 

Näiden vaiheiden avulla voit päivittää yhteyttä klusterin tuettu versio, jos klusterin solmut on internet-yhteys [http://download.microsoft.com](http://download.microsoft.com) 

Klustereiden, joka on yhteys [http://download.microsoft.com](http://download.microsoft.com)emme säännöllisesti Tarkista uusi palvelu kangasta versiot käytettävyyden.


Uusi palvelu kangasta versio on käytettävissä, kun paketti ladataan paikallisesti klusteriin ja saat päivitys. Lisäksi ilmoittamaan tämä uusi versio asiakkaan järjestelmä sijoittaa eksplisiittinen klusterin kunto varoitus seuraavankaltaiselta:

"Nykyinen klusterin versio [versio #] tuki päättyy [päivämäärä].", kun klusterin on käynnissä uusimpaan versioon, Varoitus häviää näkyvistä.


#### <a name="cluster-upgrade-workflow"></a>Klusterin päivityksen työnkulun.
 
Kun näet klusterin kunto varoitus, sinun on suoritettava seuraavat:

1. Yhteyden muodostaminen klusterin tietokoneesta, jolla on järjestelmänvalvojan käyttöoikeudet kaikissa tietokoneissa, jotka on lueteltu klusterin solmut nimellä. Tämä komentosarja on käynnissä olevat koneen ei tarvitse olla klusterin osa

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Versioita, voit päivittää palvelun kangasta luettelon noutaminen

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    tulos pitäisi saada tällainen:

    ![Hae kangasta versiot][getfabversions]

3. Projektin klusterin-päivityksen johonkin versiot, joka on käytettävissä [Aloitus ServiceFabricClusterUpgrade PowerShell-komennon](https://msdn.microsoft.com/library/mt125872.aspx) avulla

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Voit valvoa edistymistä päivitys palvelun kangasta explorer tai power shell-komennon suorittaminen

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Kun olet korjannut ongelmat, jotka ovat palauttamista, sinun täytyy Aloita päivitys uudelleen noudattamalla samoja vaiheita kuin ennen.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Päivitä kanssa klustereiden <U>ilman yhteyttä</u> voi ladata uusimman koodi ja määrittäminen

Näiden vaiheiden avulla voit päivittää yhteyttä klusterin tuettu versio, jos klusterin solmujen **ei ole** internet yhteys [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Jos käytössäsi on klusteriin, joka ei ole Internetiin yhteydessä, sinun on valvoa palvelua kangasta tiimin blogia, saat ilmoituksen uuden version. Klusterin kunto varoitusta varoituksen sen järjestelmän **ei** Vie.  

1. Muokkaa klusterin kokoonpanosi seuraavan ominaisuuden arvoksi epätosi.

        "fabricClusterAutoupgradeEnabled": false,

ja projektin määritys-päivitys. Lisätietoja [Käynnistä ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) n käyttötiedot. Klusterin tiedostojen versio on versiota, joka on clusterConfig.JSON. Varmista, että päivittää sen ennen kuin voit poisto määritysten päivitys käytöstä.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Klusterin päivityksen työnkulun.
 


1. Ladata uusimman version paketin [luominen palvelun kangasta klusterin windows Server](service-fabric-cluster-creation-for-windows-server.md) -asiakirjasta 


1. Yhteyden muodostaminen klusterin tietokoneesta, jolla on järjestelmänvalvojan käyttöoikeudet kaikissa tietokoneissa, jotka on lueteltu klusterin solmut nimellä. Tämä komentosarja on käynnissä olevat koneen ei tarvitse olla klusterin osa 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopioi ladattu paketin klusterin kuva säilöön.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Kopioidussa paketin rekisteröiminen 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Projektin klusterin-päivityksen johonkin versiot, joka on käytettävissä. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Voit valvoa edistymistä päivitys palvelun kangasta explorer tai power shell-komennon suorittaminen

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Kun olet korjannut ongelmat, jotka ovat palauttamista, sinun täytyy Aloita päivitys uudelleen noudattamalla samoja vaiheita kuin ennen.



## <a name="next-steps"></a>Seuraavat vaiheet
- Opi mukauttamaan osa [kangasta klusterin kangasta Palveluasetukset](service-fabric-cluster-fabric-settings.md)
- Lue, miten [yhteyttä klusterin sisään ja ulos](service-fabric-cluster-scale-up-down.md)
- Lue lisää [sovellus-päivitykset](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
