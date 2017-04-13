<properties
   pageTitle="Käyttöönotto ja päivittäminen paikallisen klusterin-sovellusten käytön aloittaminen | Microsoft Azure"
   description="Määrittää paikallisen palvelun kangasta-klusterin, siihen aiemmin sovelluksen ottaminen käyttöön ja päivitä sitten sovellus."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Käyttöönotto ja päivittäminen paikallisen klusterin sovellusten käytön aloittaminen
Azure-palvelu kangasta SDK sisältää koko paikallisen kehitysympäristö, joiden avulla voit nopeasti Aloita käyttöönotto ja paikallisen klusterin sovellusten hallinta. Tässä artikkelissa paikallisen klusterin luominen, siihen aiemmin sovelluksen ottaminen käyttöön ja päivität uuteen versioon, kaikki Windows PowerShell-sovellus.

> [AZURE.NOTE] Tässä artikkelissa oletetaan, että olet jo [kehittäminen ympäristön määritys](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Paikallisen klusterin luominen
Palvelun kangasta klusterin edustaa laitteistoresurssien avulla voit asentaa sovellukset. Yleensä klusterin koostuu mihin viidestä koneet useita tuhansia. Palvelun kangasta SDK on kuitenkin klusterin määrityksistä, jotka voidaan suorittaa yhteen tietokoneeseen.

On tärkeää ymmärtää, että palvelun kangasta paikallisen klusterin ei ole emulaattorin tai simulator. Se suoritetaan samalla platform-tunnus, jolla usean kuormitusryhmälle klustereiden löytyy. Ainoa ero on, että se suoritetaan ympäristö prosesseja, jotka ovat yleensä leviävät viisi tietokoneissa yhdessä koneessa.

SDK voit määrittää paikallisen klusterin kahdella tavalla: Windows PowerShell-komentosarjaa ja paikallisen klusterin hallinta järjestelmän ilmaisinalueen sovellus. Tässä opetusohjelmassa Käytämme PowerShell-komentosarjaa.

> [AZURE.NOTE] Jos olet jo luonut paikallisen klusterin ottamalla käyttöön Visual Studio sovelluksen, voit ohittaa tämän osion.


1. Käynnistä uuden PowerShell-ikkunan järjestelmänvalvojana.

2. Suorita klusterin asennuksen komentosarjan SDK-kansiosta:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Klusterin asennus kestää jonkin aikaa. Kun asennus on valmis, tulos muistuttaa pitäisi tulla näyttöön:

    ![Klusterin asennuksen tulostus][cluster-setup-success]

    Voit nyt voit kokeilla yhteyttä klusterin sovelluksen käyttöönotto.

## <a name="deploy-an-application"></a>Sovelluksen ottaminen käyttöön
Palvelun kangasta SDK sisältää monenlaisia kehysten ja developer tooling sovellusten luomisen. Jos olet kiinnostunut opettelemalla sovellusten luominen Visual Studiossa, katso [luominen ensimmäisen palvelun kangasta-sovelluksen Visual Studiossa](service-fabric-create-your-first-application-in-visual-studio.md).

Tässä opetusohjelmassa Käytämme (eli WordCount) olemassa olevan otoksen sovelluksen niin, että olemme voit keskittyä platform – esimerkiksi käyttöönoton seuranta ja päivityksen hallinta-ominaisuuksia.


1. Käynnistä uuden PowerShell-ikkunan järjestelmänvalvojana.

2. Tuo palvelun kangasta SDK PowerShell-moduulin.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Luo sovellus, jossa voit ladata ja asentaa, kuten C:\ServiceFabric tallentamiseen hakemisto.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. [Lataa WordCount-sovelluksen](http://aka.ms/servicefabric-wordcountapp) sijainnin loit.  Huomautus: Microsoft Edge-selaimella tallentaa tiedoston tiedostotunniste on *.zip* .  Muuta tiedostopääte *.sfpkg*.

5. Yhdistä paikallisen klusterin:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Luo uusi sovellus on SDK: N käyttöönotto-komento käyttäminen nimi ja polku sovelluspaketin.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Jos kaikki sujuu hyvin, pitäisi näkyä tulosteen, kuten jompikumpi seuraavista:

    ![Paikallisen klusterin-sovelluksen ottaminen käyttöön][deploy-app-to-local-cluster]

7. Jos haluat nähdä sovelluksen käytössä, Käynnistä selain ja siirry [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Näkyviin tulee:

    ![Sovelluksen Käyttöliittymä][deployed-app-ui]

    WordCount-sovellus on kuvattu yksinkertainen. Se sisältää luomiseen satunnainen viiden merkin pituinen "sanat", joka on sitten välittäminen sovelluksen kautta ASP.NET-verkko-Ohjelmointirajapinnan asiakaspuolen JavaScript-koodia. Tilallisten palvelun seuraa käynnistyksestä sanamäärä. He ovat osioitu sanan ensimmäisen merkin perusteella. Löydät lähdekoodin WordCount-sovelluksen [käytön aloittaminen-mallit](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/).

    Sovellus, jossa on otettu käyttöön sisältää neljään osaan. Jotta sanoja kirjaimella A – G tallennetaan ensimmäinen osio, sanat, jotka alkavat H – N tallennetaan toinen osio ja niin edelleen.

## <a name="view-application-details-and-status"></a>Sovellusten tarkasteleminen ja tila
Nyt kun on otettu käyttöön sovellus, katsotaan, kunnes osa PowerShell sovelluksen-tiedot.

1. Kyselyn kaikkien käyttöön sovellusten klusterin:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Oletetaan, että olet ottanut vain WordCount sovellus, näet suurin piirtein seuraavanlaisen:

    ![Kyselyn kaikkien käyttöön PowerShell-sovelluksia][ps-getsfapp]

2. Siirry seuraavalle tasolle napsauttamalla kyselyt palvelut, jotka sisältyvät WordCount-sovelluksen määrittäminen.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Luetteloi palvelut PowerShell-sovellukselle][ps-getsfsvc]

    Sovelluksen koostuu kahden palvelun--web-edusta- ja tilallisten palvelu, joka hallitsee sanat.

3. Voit tarkastella luetteloa osioiden lopuksi WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Näytä palvelun osiot powershellissä][ps-getsfpartitions]

    Komennot, jotka olet käyttänyt, kuten kaikki palvelun kangasta PowerShell-komennot ovat käytettävissä, jotka voivat muodostaa yhteyden, paikallinen tai remote klusterin.

    Lisää visuaalinen tapa ottaa yhteyttä klusterin voit käyttää verkkopohjainen palvelu kangasta Explorer-työkalun siirtymällä [http://localhost:19080/Explorer](http://localhost:19080/Explorer) selaimessa.

    ![Näytä hakemuksen tiedot kangasta Explorerissa][sfx-service-overview]

    > [AZURE.NOTE] Lisätietoja palvelun kangasta Explorer on artikkelissa [havainnollistetaan usein yhteyttä klusterin kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Sovelluksen päivittäminen
Palvelun kangasta tarjoaa ei käyttökatkot päivitykset sovelluksen tilan seurantaan, kun se palauttaa klusterin yli. Oletetaan, että suorittaa yksinkertaisen päivityksen WordCount-sovelluksen.

Nyt sovelluksen uuden version laskee vain sanat, joiden alussa vokaali. Kun päivitys palauttaa, näemme kaksi sovelluksen toiminnan muutokset. Ensin korko, jolla määrä kasvaa olisi hidastaa, koska vähemmän sanoja lasketaan. Se, koska ensimmäinen osio on kaksi vokaaleja (A ja E) ja kaikki osiot sisältävät vain yhden kunkin, sen määrä olisi myöhemmin aloittaa outpace muihin sarakkeisiin.

1. [Lataa WordCount v2 paketti](http://aka.ms/servicefabric-wordcountappv2) samaan sijaintiin, johon olet ladannut v1-paketti.

2. Palaa PowerShell-ikkunaan ja rekisteröi uusi versio klusterin päivityksen on SDK-komennon avulla. Aloita päivittäminen kangasta: / WordCount sovelluksen.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Raportissa pitäisi näkyä tulosteen powershellissä, joka näyttää seuraavankaltaiselta päivittää alkaa.

    ![PowerShellin edistymisen päivittäminen][ps-appupgradeprogress]

3. Kun päivitys on jatkamista, voit saattaa olla helpompi seurata sen tilan kangasta Explorerista. Käynnistä selain ja siirry [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Laajenna puun vasemmalla puolella **sovellukset** ja valitse sitten **WordCount**ja lopuksi **kangasta: / WordCount**. Perustiedot-välilehti tulee näkyviin päivityksen tila sen edetessä klusterin päivityksen toimialueiden kautta.

    ![Kangasta Explorerissa edistymisen päivittäminen][sfx-upgradeprogress]

    Päivityksen edetessä läpi kunkin toimialueen kuntotietojen tarkistus suoritetaan varmistaa, että sovellus on muunnettuna.

4. Jos suoritat aiemmissa kysely kangasta Services-palvelujen määrittäminen: / WordCount sovellus, Huomaa, että WordCountService versio muuttunut mutta WordCountWebService versio ei muutu:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Kyselyn sovelluspalveluja päivittämisen jälkeen][ps-getsfsvc-postupgrade]

    Tämä korostaa miten palvelun kangasta hallitsee sovelluksen päivitykset. Se koskettaa vain joukon palveluita (tai koodi tai määritysten pakettien näistä palveluista), jotka ovat muuttuneet, joka tekee Käsittele päivitetään nopeammin ja luotettava.

5. Palaa selaimen noudata uusi sovellusversio toimintaa lopuksi. Odotetusti määrä etenee hitaasti, ja ensimmäinen osio päättyy hieman enemmän äänenvoimakkuutta.

    ![Sovelluksen uuden version tarkasteleminen selaimessa][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Puhdistaminen

Ennen kuin lopuksi, on tärkeää muistaa, että paikallisen klusterin on reaaliluku. Sovellukset edelleen käynnissä taustalla, kunnes ne poistetaan.  Sen mukaan, sovellus laatu käynnissä sovellus voi kestää merkittäviä resurssien käyttämääsi laitteeseen. Sinulla on useita tapoja hallita sovellusten ja klusterin:

1. Jos haluat poistaa yksittäisen sovelluksen ja kaikki siihen liittyvät tiedot, suorita seuraava:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Tai poista sovellus palvelun kangasta Explorer **Toiminnot** -valikon tai pikavalikon vasemmanpuoleisessa ruudussa sovelluksen luettelonäkymässä.

    ![Poista sovellus on palvelu kangasta Explorer][sfe-delete-application]

2. Kun olet poistanut sovelluksen klusterista, voit unregister versiot 1.0.0 ja 2.0.0 WordCount sovelluksen tyyppi. Poistaminen poistaa sovelluksen-paketteja, mukaan lukien koodi ja määritys klusterin kuva-kaupasta.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Tai valitse kangasta Resurssienhallinnassa **Poista valmistelu tyyppi** -sovelluksen.

3. Klusterin Sammuta, mutta säilyttää tiedot ja jäljittää, valitse **Lopeta paikallisen klusterin** järjestelmän ilmaisinalueen-sovelluksessa.

4. Voit poistaa klusterin kokonaan valitsemalla **Poista paikallisen klusterin** järjestelmän ilmaisinalueen-sovelluksessa. Tämä asetus aiheuttaa toisen hidas käyttöönoton painamalla F5-näppäintä Visual Studiossa seuraavan kerran. Poista paikallisen klusterin vain, jos et aio käyttää sitä jonkin aikaa, tai jos haluat vapauttaa resursseja.

## <a name="1-node-and-5-node-cluster-mode"></a>1-solmu ja 5 solmun klusterin tila

Kun käsittelet paikallisen klusterin sovellusten kehittämiseen, usein löydät itsellesi tekeminen nopeasti Iteraatioita koodin kirjoittamista, virheenkorjaus, muuttaa koodi, muistin jne. Optimoi tämän prosessin avulla paikallisen klusterin suorittaa kaksi tilaa: 1 solmu tai 5 solmu. Klusterin sekä tilat on niiden etuja.
5 solmun klusterin tilassa avulla voit käsitellä reaali klusterin. Voit testata automaattisesti skenaariot, Lisää esiintymät ja palvelujen replikoita.
1 solmu klusterin tila on optimoitu nopeasti käyttöönotto ja rekisteröinti palvelujen auttaa nopeaan vahvistamiseen koodin palvelu kangasta Runtimen avulla.

1 solmu klusterin tila sekä 5 solmun klusterin tila ei ole emulaattorin tai simulator. Se suoritetaan samalla platform-tunnus, jolla usean kuormitusryhmälle klustereiden löytyy.

> [AZURE.NOTE] Tämä ominaisuus on käytettävissä SDK versio 5.2 ja yläpuolella.

Voit muuttaa klusterin tila 1 solmu klusteriin, palvelun kangasta paikallisen klusterin Managerissa tai käyttämällä PowerShell seuraavalla tavalla:

1. Käynnistä uuden PowerShell-ikkunan järjestelmänvalvojana.

2. Suorita klusterin asennuksen komentosarjan SDK-kansiosta:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Klusterin asennus kestää jonkin aikaa. Kun asennus on valmis, tulos muistuttaa pitäisi tulla näyttöön:
    
    ![Klusterin asennuksen tulostus][cluster-setup-success-1-node]

Jos käytössäsi on palvelun kangasta paikallisen klusterin hallinta:

![Vaihda klusterin tila][switch-cluster-mode]

> [AZURE.WARNING] Vaihdettaessa klusterin tilassa nykyiseen klusteria poistetaan järjestelmästä ja uuden klusterin luodaan. On tallentamat klusterin, tiedot poistetaan, kun muutat klusterin tilaa.

## <a name="next-steps"></a>Seuraavat vaiheet
- On otettu käyttöön ja päivittää valmiita sovelluksia, voit [kokeilla rakentaminen Omat Visual Studiossa](service-fabric-create-your-first-application-in-visual-studio.md).
- Kaikki tämän artikkelin paikallisen klusterin toimintoja voidaan suorittaa luodun [Azure klusterin](service-fabric-cluster-creation-via-portal.md) paikan päällä.
- Päivityksen, joka on suorittaa tässä artikkelissa on perustiedot. Artikkelissa on [päivittäminen ohjeista](service-fabric-application-upgrade.md) lisätietoja power ja joustavuutta palvelun kangasta päivitykset.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
