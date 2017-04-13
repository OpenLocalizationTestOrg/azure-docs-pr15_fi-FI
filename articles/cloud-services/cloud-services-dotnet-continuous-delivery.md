<properties
    pageTitle="Jatkuva toimitukseen cloud Services-palveluiden kanssa TFS Azure-tietokannassa | Microsoft Azure"
    description="Opettele jatkuva toimituksen Azure cloud-sovellusten määrittäminen. MSBuild komentorivin lauseet ja PowerShell-komentosarjojen MALLIKOODEJA."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Jatkuva toimitukseen pilvipalveluihin Azure-tietokannassa

Tässä artikkelissa kuvatut toimet näytetään, miten voit määrittää jatkuva toimituksen Azure cloud sovellusten. Tämän prosessin avulla voit luoda automaattisesti pakettien ja paketin käyttöön Azure, kun jokaisen koodin sisäänkuittauksen. Tässä artikkelissa kuvattuja paketin muodosta prosessi on sama kuin Visual Studiossa **paketti** -komento ja julkaisun vaiheet ovat samat Visual Studiossa **Julkaise** -komennon.
Artikkelissa käsitellään luominen muodosta palvelimen MSBuild komentorivin lauseet ja Windows PowerShell-komentosarjojen vakiomittoja tapaa ja Visual Studio Team Foundation Server - ryhmän luominen määritykset MSBuild komentojen ja PowerShell-komentosarjojen määrittämisestä halutessasi myös esimerkissä. Prosessi on mukautettavissa ympäristön ja Azure kohde-ympäristössä.

Voit käyttää myös Visual Studio ryhmän palvelut-versio, joka isännöi Azure-toiminto on helpompi TFS. Lisätietoja on artikkelissa [Azure käyttämällä Visual Studio ryhmän palveluissa jatkuva toimitus][].

Ennen kuin aloitat, kannattaa julkaista sovelluksesi Visual Studio.
Näin varmistat, että kaikki resurssit ovat käytettävissä ja valmisteltu kun yrität automatisoida julkaisun.

## <a name="1-configure-the-build-server"></a>1: Muodosta-palvelimen määrittäminen

Ennen MSBuild avulla voit luoda Azure paketin, sinun on asennettava tarvittavat ohjelmistot ja työkalut muodosta-palvelimeen.

Visual Studio ei tarvitse on asennettu muodosta-palvelimeen. Jos haluat hallita muodosta palvelinta ryhmän Foundation luominen-palvelun avulla- [Ryhmän Foundation luominen palvelun][] -ohjeet ohjeiden mukaisesti.

1.  Muodosta palvelimessa Asenna [.NET Framework 4.5.2][], joka sisältää MSBuild.
2.  Asenna uusimmat [.NET Azure yhtä aikaa muiden kanssa Työkalut](https://azure.microsoft.com/develop/net/).
3.  Asenna [.NET Azure kirjastoissa](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Kopioi Microsoft.WebApplication.targets tiedosto Visual Studio asennuksen muodosta palvelimeen.

    Visual Studiossa, jotka on asennettu tietokoneeseen, tämä tiedosto sijaitsee hakemistossa C:\\ohjelma Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. Sinun on kopioitava se samaan kansioon muodosta-palvelimeen.
5.  Asenna [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: MSBuild komennoilla paketin luominen

Tässä osassa kuvataan MSBuild-komento, joka muodostaa Azure paketin muodostamisesta. Voit varmistaa, että kaikki tiedot on määritetty oikein ja että MSBuild-komento tekee mitä haluat tehdä muodosta palvelimessa suoritettavat tämän vaiheen. Voit lisätä aiemmin muodosta komentosarjat muodosta palvelimessa Tämä komentorivi tai komentorivin TFS muodosta määrityksessä, voit käyttää seuraavan osion ohjeiden mukaisesti. Saat lisätietoja komentoriviparametreja ja MSBuild [MSBuild komentoriviltä viittaus](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Jos Visual Studio on asennettu muodosta palvelimessa, Etsi ja valitse **Visual Studio komennon kehote** Windows **Visual Studio Tools** -kansiossa.

    Jos Visual Studio ei ole asennettu muodosta palvelimessa, Avaa komentorivi-ikkuna ja varmista, että MSBuild.exe on käytettävissä polulla. MSBuild on asennettu .NET Framework polku % WINDIR %\\Microsoft.NET\\Framework\\*versio*. Esimerkiksi MSBuild.exe lisääminen PATH-ympäristömuuttuja, kun käytössä on asennettu .NET Framework 4, kirjoita komentoriville seuraava komento:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Komentokehotteen Siirry Azure projektitiedoston, johon haluat luoda sisältävä kansio.

3.  Suorita MSBuild / target: Julkaise vaihtoehto, kuten seuraavassa esimerkissä:

        MSBuild /target:Publish

    Tämä asetus on lyhennettä /t: Julkaise. MSBuild /t:Publish-vaihtoehto tulee ei pidä sekoittaa Julkaise komennoista Visual Studiossa ollessasi asennettu Azure SDK-paketissa. /T: asetus vain versiot Azure-pakettien julkaiseminen. Se Ota paketit tavalla Visual Studiossa Julkaise-komentoja.

    Vaihtoehtoisesti voit määrittää MSBuild-parametrin projektin nimeä. Jos ei ole määritetty, käytetään nykyisen hakemiston. Saat lisätietoja MSBuild komentorivivalitsimet [MSBuild komentoriviltä viittaus](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Etsi tulos. Oletusarvoisesti tämä komento luo kansion pääkansion projektin, kuten *ProjectDir*suhteessa\\bin\\*määritysten*\\app.publish\\. Kun luot Azure project-Luo kaksi tiedostoa, itse pakettitiedosto ja mukana määritystiedostoa:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Oletusarvon mukaan kunkin Azure project sisältää yhden palvelun kokoonpanotiedosto (.cscfg-tiedosto) paikallisen (muistin) versiot ja toinen cloud (väliaikainen tai tuotannon)-versiot, mutta voit lisätä tai poistaa palvelun tiedostojen tarpeen mukaan. Kun luot Visual Studion paketin, sinua pyydetään mitä määritysten tiedosto, joka sisältää paketin rinnalla.

5.  Määritä service-kokoonpanotiedosto. Kun luot paketin käyttämällä MSBuild-paikallinen palvelu-kokoonpanotiedosto sisältyy oletusarvoisesti. Sisällytettävien eri kokoonpanotiedosto MSBuild-komento, kuten seuraavassa esimerkissä TargetProfile-ominaisuuden määrittäminen:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Määritä tulosteen sijainti. Määritä polku /p:PublishDir käyttämällä =*Hakemisto* \\ vaihtoehto, kuten lopussa kenoviiva erotin, kuten seuraavassa esimerkissä:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Kun olet rakennettava ja testaa MSBuild komentorivi voivat laatia projektien ja yhdistää ne yhdeksi Azure paketti, voit lisätä Tämä komentorivi muodosta-komentosarjoja. Jos muodosta server käyttää mukautettuja komentosarjoja, tämä toimenpide määräytyvät mukautetun muodostusprosessin yksityiskohtia. Jos käytät TFS muodosta-ympäristössä, voit noudattamalla ohjeita seuraavassa vaiheessa Azure paketin Luo lisääminen muodostusprosessin.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: Muodosta paketin käyttämällä TFS ryhmän luominen

Jos sinulla on TFS muodosta koneen määritetty muodosta ohjauskoneen ja muodosta-palvelimen määrittäminen Team Foundation Server (TFS) ja valitse voit halutessasi määrittää Automaattinen muodosta Azure-paketin. Saat lisätietoja siitä, miten voit määrittää ja käyttää ryhmän Foundation-palvelimeen Muodosta järjestelmän, [Mittakaava, muodosta järjestelmässä][]. Erityisesti Seuraavassa oletetaan, että olet määrittänyt muodosta palvelimellesi kuvatulla tavalla [käyttöönotto ja määrittää muodosta palvelimen][], ja että olet luonut, ryhmäprojektin luonut cloud palvelun projektin työryhmän projektissa.

Voit määrittää TFS luonnissa Azure pakettien toimimalla seuraavasti:

1.  Valitse **Ryhmän Explorer**Visual Studio kehittäminen tietokoneessasi Näytä-valikon tai valitse Ctrl +\\, Ctrl + M. Ryhmän hallinta-ikkunassa Laajenna **muodostaa** solmu tai valitse **muodostaa** -sivu ja valitse **Uusi luominen määritys**.

    ![Uusi luominen määritelmä-vaihtoehto][0]

2.  **Käynnistimen** -välilehti ja määritä haluamasi ehdot, kun haluat paketin rakentaa. Esimerkiksi määrittää **Jatkuva integrointi** luonnissa paketin aina tietolähteen ohjausobjektin sisäänkuittauksen yhteydessä.

3.  **Tietolähteen asetukset** -välilehti ja varmista, että projektikansio näkyy **Tietolähteen ohjausobjektin kansio** -sarakkeessa ja tila on **aktiivinen**.

4.  **Muodosta oletukset** -välilehti ja tarkista muodosta-palvelimen nimi ohjauskoneen muodosta-kohdassa.  Myös vaihtoehto **kopion luominen tulosteen drop-kansioon** ja määritä haluamasi avattavan sijainti.

5.  Valitse **prosessi** -välilehti. Valitse prosessi-välilehden oletusmalli, valitse **Muodosta**, valitse projekti, jos se ei jo ole valittuna, ja laajenna ruudukon **luominen** -kohdassa **Lisäasetukset** -osassa.

6.  Valitse **MSBuild argumentit**, ja määritä haluamasi MSBuild komentoriviargumentit vaiheessa 2 edellä kuvatulla tavalla. Kirjoita esimerkiksi **/t: Julkaise /p:PublishDir =\\\\omapalvelin\\pudottaa\\ ** voivat laatia paketin ja kopioi tiedostojen pakkaaminen sijaintiin \\ \\omapalvelin\\pudottaa\\:

    ![MSBuild argumentit][2]

    **Huomautus:** Tiedostojen kopioiminen resurssiin manuaalisesti käyttöön kehittäminen tietokoneesta paketit on helpompaa.

5.  Testaa muodosta vaihe onnistuu valitsemalla Muuta projektiin tai jono ylöspäin uusi versio. Jonon uusi versio, ryhmän Explorerissa ylös **Kaikki luominen määritykset,** hiiren kakkospainikkeella ja valitse sitten **Jonon uusi luominen**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: Julkaise paketin käyttämällä PowerShell-komentosarja

Tässä osassa kuvataan, miten voit käyttää Windows PowerShell-komentosarja, joka julkaisee Cloud app paketin tulosteen Azure valinnaisten parametrien avulla. Tämä komentosarja voidaan kutsua kun Luo vaiheessa mukautetun muodosta automaatio. Se voidaan kutsua myös prosessin mallin työnkulun toimintojen Visual Studio TFS tiimin rakentaminen.

1.  Asenna [Azure PowerShellin cmdlet-komennot][] (v0.6.1 tai uudempi).
    Cmdlet-komento asetukset vaiheen aikana valitsemalla Asenna laajennus. Huomaa, että tämä julkisesti tuettuihin versio korvaa vanhemman version-CodePlex-sivustoista, vaikka aiempiin versioihin verrattuna on numeroitu 2.x.x.

2.  Käynnistä aloitusvalikkoon PowerShellin Azure tai sivun. Jos aloitat tällä tavalla, Azure PowerShellin cmdlet-komennot ladataan.

3.  PowerShell tulee näkyviin, varmista, että PowerShellin cmdlet-komennot ladataan kirjoittamalla osittaisen komento `Get-Azure` ja painamalla SARKAINTA lauseen käyttöönotto.

    Jos painat SARKAINTA toistuvasti, pitäisi näkyä eri Azure PowerShell-komennoilla.

4.  Varmista, että voit muodostaa yhteyden Azure tilauksen tuomalla .publishsettings tiedostosta tilauksen tiedot.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Kirjoita kehote

    `Get-AzureSubscription`

    Tämä näyttää Tilauksen tiedot. Varmista, että kaikki kohdat ovat oikein.

4.  C: kuin komentosarjat-kansioon tämän artikkelin lopussa komentosarja-mallin tallentaminen\\komentosarjojen\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Tarkista komentosarjan parametrit-osassa. Voit lisätä tai muokata oletusarvot. Nämä arvot voi ohittaa aina eksplisiittinen parametrit kulkee.

6.  Varmista kelpaa cloud palvelu ja tallennustilaa tilit luodaan tilaukseesi, joka voidaan kohdistaa Julkaise-komentosarjalla. Tallennustilan tilin (Blob-objektien tallennustila) käytetään ladataan ja tallentaa käyttöönoton paketin ja config tiedoston tilapäisesti käyttöönoton luonnin aikana.

    -   Voit luoda uuden pilvipalvelussa, voit soittaa tämä komentosarja tai käyttää [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). Cloud palvelun nimeä käytetään täydellinen toimialuenimi on etuliite ja näin ollen on oltava yksilöllinen.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Luo uusi tallennustilan-tili, voit soittaa tämä komentosarja tai käyttää [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). Tallennustilan tilin nimi, jota käytetään etuliitteenä-täydellinen toimialuenimi ja näin ollen on oltava yksilöllinen. Voit kokeilla on sama nimi kuin cloud-palvelussa.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Soita komentosarja suoraan PowerShellin Azure tai kulmien määrittäminen tämä komentosarja host muodosta-automaatio paketin Luo jälkeen.

    >[AZURE.IMPORTANT] Komentosarjan aina Poista tai korvaa aiemmin luotu käyttöönoton oletusarvoisesti, jos niitä havaita. Tämä on tarpeen, jotta automaattiset jatkuva toimituksesta, jossa ei ole käyttäjän kehotteita on mahdollista.

    **Esimerkkitapaus 1:** jatkuva käyttöönoton palvelun väliaikaisen-ympäristöön:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Tämä yleensä seuranta Testaa Suorita vahvistus ja VIP-Vaihda mukaan. VIP swap voidaan toteuttaa [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885) tai Siirrä käyttöönoton cmdlet-komennolla.

    **Esimerkkitapaus 2:** erillinen testi palvelun tuotantoympäristössä jatkuva käyttöönotto

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Etätyöpöytä:**

    Jos sinun on suoritettava erikseen lisätoimia, jotta oikea Cloud palvelun sertifikaatti tuodaan kaikki tämän komentosarjan kohteena pilvipalveluihin Azure projektin Etätyöpöytä on otettu käyttöön.

    Etsi oman roolia oikein varmenteen allekirjoitus arvot. Allekirjoitus-arvot ovat näkyvissä cloud määritystiedosto (eli ServiceConfiguration.Cloud.cscfg) varmenteet-osassa. Se on myös näkyvissä Visual Studiossa Remote Desktop-määritys-valintaikkunassa, kun näytät asetukset ja Näytä valittu varmenne.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Lataa etätyöpöydän varmenteet erikseen määritys vaiheessa seuraavat cmdlet-komentosarjan avulla:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Esimerkki:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Vaihtoehtoisesti voit viedä varmenteen tiedoston PFX yksityinen avain ja varmenteet lataaminen kunkin kohteen pilvipalvelussa [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885). 
    Lisätietoja on seuraavassa artikkelissa: [[http://msdn.microsoft.com/library/windowsazure/gg443832.aspx]].

    **Päivitä käyttöönotto ja poista käyttöönotto -\> uusi käyttöönotto**

    Komentosarja oletusarvoisesti suorittaa päivittäminen käyttöönoton ($enableDeploymentUpgrade = 1) kun ei ole parametri on välitetty tai arvon 1 välitetään erikseen. Yksittäisen esiintymien on vähemmän aikaa kuin koko käyttöönottoa hyödyntää. Jos esiintymät, jotka edellyttävät suuren käytettävyyden, tämä on myös hyödyntää jätä tietyissä tilanteissa, kun muut päivitetään (ominaisuussäilöjen Päivitä toimialue) sekä oman VIP ei poisteta.

    Päivityksen käyttöönoton voidaan poistaa käytöstä komentosarjan ($enableDeploymentUpgrade = 0) tai siirtämällä *- enableDeploymentUpgrade 0* parametrinä, joka muuttaa, poista ensin kaikki olemassa olevat käyttöönotto ja luo sitten uusi käyttöönoton komentosarja-ongelma.

    >[AZURE.IMPORTANT] Komentosarjan aina Poista tai korvaa aiemmin luotu käyttöönoton oletusarvoisesti, jos niitä havaita. Tämä on tarpeen, jotta automaattiset jatkuva toimituksesta, jossa ei ole käyttäjän/ylläpitäjä kehotteita on mahdollista.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: Julkaise paketin käyttämällä TFS ryhmän luominen

Tämä vaihe on valinnainen muodostaa TFS ryhmän muodosta komentosarja loit vaiheessa 4, joka käsittelee julkaisemisen Azure-paketin muodosta. Tämä edellyttää muokkaaminen prosessimalli koontiversion määrityksen niin, että se suorittaa Julkaise tehtävän työnkulun lopussa. Julkaise tehtävää suoritetaan PowerShell kulkeva parametrien koonti-komento. Tulosteen MSBuild kohteena ja julkaista komentosarja piped vakio muodosta tulosteen kyselyjä.

1.  Muokkaa vastuussa luominen-määritystä jatkuva käyttöönotto.

2.  Valitse **prosessi** -välilehti.

3.  Noudattamalla [seuraavia ohjeita](http://msdn.microsoft.com/library/dd647551.aspx) voit lisätä tehtävän-projektin muodosta prosessin mallin, lataa oletusmallia, lisääminen projektiin ja kuittaa sisään. Anna muodosta prosessin mallille uusi nimi, kuten AzureBuildProcessTemplate.

3.  Palaa **prosessi** -välilehden ja **Näytä tiedot** avulla voit näyttää käytettävissä muodosta prosessin mallien luettelo. Valitse **Uusi...** -painiketta ja Siirry projektin juuri lisäämäsi ja kuitata sisään. Etsi malli juuri luonut ja valitse **OK**.

4.  Avaa valitun prosessin mallin muokkausta varten. Voit avata suoraan työnkulun suunnittelussa tai XML-editoriin XAML-käyttöä varten.

5.  Lisää seuraavat uuden argumenttiluettelon erillisen rivin kohteiden työnkulun suunnittelun argumentit-välilehti. Kaikkien argumenttien on oltava suunta = ja kirjoittamalla = merkkijono. Nämä käytetään vuon parametrit muodosta määrityksestä työnkulkuja, joita Hae sitten käyttää Soita Julkaise komentosarja.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Argumenttiluettelosta.][3]

    Vastaavat XAML näyttää seuraavanlaiselta:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Lisää uuden sarjan Suorita agentti lopussa:

    1.  Aloita lisäämällä Jos lauseen tehtävän kelvollinen komentosarjatiedosto tarkistavan. Määritä ehto tämän arvon:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Ja sitten tapauksessa, jos-lauseessa, Lisää uusi tehtävä järjestyksessä. Käynnistä julkaista sen käyttönimen määrittämiseen

    3.  Käynnistä julkaista valittuna järjestys, Lisää uusia muuttujia seuraavassa luettelossa erillisen rivin kohteiden työnkulun suunnittelun muuttujat-välilehti. Kaikki muuttujat pitäisi olla muuttujien = merkkijono ja laajuus = alku Julkaise. Nämä käytetään vuon parametrit muodosta määrityksestä työnkulkuja, joita Hae sitten käyttää Soita Julkaise komentosarja.

        -   SubscriptionDataFilePath tyypin merkkijono

        -   PublishScriptFilePath tyypin merkkijono

            ![Uusi muuttujat][4]

    4.  Jos käytät TFS 2012 tai sitä aiempi versio, Lisää ConvertWorkspaceItem tehtävän uuden järjestyksen alussa. Jos käytät TFS 2013 tai uudempi versio, Lisää GetLocalPath tehtävän uuden järjestyksen alussa. Määritä ConvertWorkspaceItem-ominaisuudet seuraavasti: suunta = ServerToLocal, näyttönimi = "Muunna julkaista komentosarjan tiedostonimi", syötteen = "PublishScriptLocation", tulos = 'PublishScriptFilePath'-työtilan = "Työtilan". GetLocalPath tehtävälle ominaisuuden IncomingPath 'PublishScriptLocation' ja 'PublishScriptFilePath' tuloksen. Tässä aktiviteetti muuntaa polku Julkaise-komentosarjan TFS palvelimen sijainneista (jos saatavilla) Vakio paikalliseen levyasemaan polkua.

    5.  Jos käytössäsi on TFS 2012 tai sitä aiempi versio, Lisää toinen ConvertWorkspaceItem tehtävän uuden järjestyksen lopussa. Suunta = ServerToLocal, näyttönimi = "Muuntaa tilauksen tiedostonimi", syötteen = "SubscriptionDataFileLocation", tulos = 'SubscriptionDataFilePath'-työtilan = "Työtilan". Jos käytössäsi on TFS 2013 tai uudempi versio, Lisää toinen GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation', ja tulos = "SubscriptionDataFilePath."

    6.  Lisää InvokeProcess tehtävän uuden järjestyksen lopussa.
        Tässä aktiviteetti kutsuu PowerShell.exe muodosta määritelmä argumentit.

        1.  Argumentit = String.Format ("-tiedostoa""{0}" "- palvelun nimi {1} - storageAccountName {2} - PackageLocation-ominaisuudessa""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6}-ympäristön""{7}" "", PublishScriptFilePath, palvelun nimi, StorageAccountName, PackageLocation-ominaisuudessa, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, ympäristön)

        2.  Näyttönimi = Execute julkaista komentosarja

        3.  FileName = "PowerShell" (sisältää lainausmerkkejä)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  **Käsittele vakio tulostus** -kohdassa InvokeProcess, tekstiruutu arvo tekstiruutuun "tiedot". Tämä on muuttujan vakio tulosteen tietojen tallennusta varten.

    8.  Lisää WriteBuildMessage tehtävä, **Käsittele vakio tulostus** -osan alapuolella. Tärkeysasteeksi määritetään = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' ja viestin = "tiedot". Tällä varmistetaan, että komentosarja vakio tulos Hae kirjoitettu muodosta tulostukseen.

    9.  **Käsittele virhe tulostus** -osassa InvokeProcess, tekstiruutu Määritä tekstiruudun arvoksi "tiedot". Tämä on muuttujan keskivirhe tietojen tallennusta varten.

    10. Lisää WriteBuildError tehtävä, **Käsittele virhe tulostus** -osan alapuolella. Määrittää viestin = "tiedot". Tällä varmistetaan, että komentosarjan Keskivirheet Hae kirjoitettu muodosta virhe tulostukseen.

    11. Korjaa mahdolliset virheet, merkitty Sininen huutomerkki merkit. Saat lisätietoja virheestä Vihje huutomerkki merkit päälle. Tallenna työnkulku, poista virheet.

    Julkaise työnkulun toiminnot lopullinen tulos näyttää tältä suunnittelussa:

    ![Työnkulun toiminnot][5]

    Julkaise työnkulun toiminnot lopullinen tulos näyttää tältä: XAML:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Tallenna muodosta mallin työnkulun ja kuittaa sisään tämä tiedosto.

8.  Muodosta kuvauksen muokkaaminen (sulkea sen, jos se on jo avoinna), ja valitse **Uusi** -painiketta, jos se ei ole vielä näkyvissä prosessimallit-luettelosta uuden mallin.

9.  Määritä parametrin ominaisuusarvoihin muut-kohdassa seuraavasti:

    1.  CloudConfigLocation ='c:\\pudottaa\\app.publish\\ServiceConfiguration.Cloud.cscfg' *arvoksi johdetaan: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation-ominaisuudessa = ' c:\\pudottaa\\app.publish\\ContactManager.Azure.cspkg' *arvoksi johdetaan: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = ' c:\\komentosarjojen\\WindowsAzure\\PublishCloudService.ps1 "

    4.  Palvelun nimi = "mycloudservicename" *Käytä tätä tarvittavat cloud-palvelunimi*

    5.  Ympäristön = "Väliaikaisen"

    6.  StorageAccountName = "mystorageaccountname" *Käytä tätä tarvittavat tallennustilan-tilin nimi*

    7.  SubscriptionDataFileLocation = ' c:\\komentosarjojen\\WindowsAzure\\Subscription.xml "

    8.  SubscriptionName = "oletus"

    ![Parametrin ominaisuusarvoihin][6]

10. Tallenna muutokset luominen-määritys.

11. Jono muodosta suorittaa paketti-Luo ja Julkaise. Jos sinulla on jatkuva integrointi asettaminen käynnistin, valitse jokaisen sisäänkuittauksen suoritetaan tämän ongelman.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 komentosarja-malli

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Seuraavat vaiheet

Käyttöön remote virheenkorjaus käytettäessä jatkuva toimitus-kohdassa [ottaminen käyttöön remote virheenkorjaus jatkuva toimituksen Azure julkaiseminen käytettäessä](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Jatkuva toimittamista Azure Visual Studio Team Services-palvelujen avulla]: cloud-services-continuous-delivery-use-vso.md  
  [Ryhmän Foundation muodosta-palvelu]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Skaalaa, muodosta-järjestelmässä]: https://msdn.microsoft.com/library/dd793166.aspx
  [Ottaa käyttöön ja muodosta-palvelimen määrittäminen]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShellin cmdlet-komennot]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
