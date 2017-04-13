<properties
   pageTitle="Windows PowerShell-komentosarjojen avulla voit julkaista keskihajonta ja testaa ympäristöissä | Microsoft Azure"
   description="Opi käyttämään Windows PowerShell-komentosarjojen Visual Studio kehittäminen julkaiseminen ja testaa ympäristöissä."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Keskihajonta julkaiseminen ja testaa ympäristöissä Windows PowerShell-komentosarjojen avulla

Kun luot web-sovelluksen Visual Studiossa, voit luoda Windows PowerShell-komentosarjaa, jonka avulla voit automatisoida Azure sivuston julkaisemiseen verkkosovellukseen Azure App palvelun tai virtual machine myöhemmin. Voit muokata ja laajentaa tarpeitasi Visual Studio-editorissa Windows PowerShell-komentosarjaa tai komentosarjan integroida aiemmin muodosta, Testaa ja julkaisemista komentosarjoja.

Komentosarjat voit valmistella mukautettuja versioita (tunnetaan myös nimellä keskihajonta ja testaa ympäristöissä) sivuston tilapäinen käyttöä varten. Voit esimerkiksi määrittää tietyn version sivuston Azure virtuaalikoneen tai Web-sivuston Suorita testi tuotepakettia, toistaa ohjelmavirhe, Testaa ohjelmavirhe korjaa-kokeiluversio ehdotettu muuttaminen tai määrittää mukautetun ympäristön esittely tai esityksen väliaikaisen pidikkeessä. Sen jälkeen, kun olet luonut komentosarjan, joka julkaisee projektin, Luo samanlainen ympäristöissä suorittamalla komentosarja uudelleen tarvittaessa tai suorittaa komentosarjan oman web-sovelluksen haluat luoda mukautetun ympäristön testikäyttöön muodosta.

## <a name="what-you-need"></a>Tarvitset

- Azure SDK 2.3 tai uudempi. Lisätietoja on kohdassa [Visual Studio Lataa](http://go.microsoft.com/fwlink/?LinkID=624384) .

Luo komentosarjat web projektien Azure-SDK ei tarvitse. Tämä toiminto on tarkoitettu web-projektien ei web-roolien pilvipalveluihin.

- Azure PowerShell 0.7.4 tai uudempi versio. Lisätietoja on kohdassa [asentaminen ja määrittäminen PowerShellin Azure](powershell-install-configure.md) .

- [Windows PowerShellin 3.0](http://go.microsoft.com/?linkid=9811175) tai uudempi versio.

## <a name="additional-tools"></a>Lisätyökalut

Muita työkaluja ja resursseja käsittelyyn Visual Studiossa PowerShellin Azure kehittämiseen ovat käytettävissä. Katso [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>Julkaise-komentosarjoja luotaessa

Voit luoda Julkaise-komentosarjoja virtual tietokoneelle, joka isännöi sivuston, kun luot uuden projektin seuraavien [ohjeiden](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md)mukaan. Voit myös [Luo julkaista komentosarjojen verkkosovelluksissa Azure App palvelun](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Komentosarjoja tähän tarkoitukseen Visual Studio Luo

Visual Studio Luo ratkaisun tason kansio nimeltä **PublishScripts** , joka sisältää kaksi Windows PowerShell-tiedostoja, Julkaise komentosarjan virtuaalikoneen tai sivuston ja funktioita, joita voit käyttää komentosarjat sisältävä moduuli. Visual Studio luo tiedoston myös JSON-muotoon, joka määrittää otat projektin tiedot.

### <a name="windows-powershell-publish-script"></a>Windows PowerShellin julkaista komentosarja

Julkaise-komentosarja on tietyn Julkaise vaiheet, sivustoon tai virtuaalikoneen käyttöönotto. Visual Studio on värillisten Windows PowerShellin kehittämiseen syntaksia. Ohje funktiot on käytettävissä ja voit muokata vapaasti muuttaminen tarpeitasi komentosarja-funktiot.

### <a name="windows-powershell-module"></a>Windows PowerShell-moduulin

Windows PowerShell-moduulin, joka luo Visual Studio on toimintoja, Julkaise-komentosarja käyttää. Nämä ovat PowerShellin Azure-Funktiot ja ei ole tarkoitus voi muokata. Lisätietoja on kohdassa [asentaminen ja määrittäminen PowerShellin Azure](powershell-install-configure.md) .

### <a name="json-configuration-file"></a>JSON-kokoonpanotiedosto

JSON-tiedosto luodaan **määritykset** -kansioon, ja se sisältää kokoonpanotietoja, joka määrittää täsmälleen Azure käyttöön resursseja. Tiedosto, jonka Visual Studio Luo on project-nimi-WAWS-dev.json Jos olet luonut sivusto tai projektin nimi-AM-dev.json, jos olet luonut virtual machine. Tässä on esimerkki JSON määritystiedoston, joka on luotu, kun luot sivuston. Useimmat arvot ovat selkeitä. Sivuston nimi luodaan Azure, jotta ne eivät vastaa projektinimi.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Kun luot virtual machine, JSON-kokoonpanotiedosto näyttää seuraavankaltaiselta. Huomaa, että pilvipalveluun luodaan virtuaalikoneen säilö. Virtuaalikoneen on tavallista päätepisteet web Accessin kautta HTTP- ja HTTPS sekä päätepisteet Web käyttöön, joiden avulla voit julkaista verkkosivuston Paikallinen kone-Etätyöpöytä ja Windows PowerShell.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

Voit muokata muuttamiseen, mitä tapahtuu, kun suoritat Julkaise-komentosarjoja JSON-määritys. `cloudService` Ja `virtualMachine` osat ovat pakollisia, mutta voit poistaa `databases` osan, jos et tarvitse sitä. Ominaisuudet, jotka ovat tyhjiä, joka luo Visual Studio oletusarvon määritystiedostossa on valinnainen; ne, jotka on oletusarvo-kokoonpanotiedosto arvot ovat pakollisia.

Jos sinulla on sivuston, jossa on useita käyttöönotto-ympäristössä (eli paikkojen) sijaan yksi tuotannon toimipaikka Azure-tietokannassa, voit lisätä paikan nimi Kirjoita sivuston nimi JSON määritystiedostossa. Esimerkiksi jos sivusto on, nimi on **oman sivuston** ja sen nimeltä **Testaa** sitten URI paikka on oman sivuston test.cloudapp.net, mutta oikea nimi, jota käytetään määritystiedostossa on mysite(test). Voit tehdä vain tämä, jos sivusto ja paikkojen jo olemassa-tilaukseesi. Jos ne eivät ole luominen sivuston suorittamalla komentosarja määrittämättä paikka, sitten luominen [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)paikka ja suorittaminen sen jälkeen komentosarja muokattu sivuston nimi. Saat lisätietoja käyttöönoton paikkojen web Apps-sovellukset [väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen](./app-service-web/web-sites-staged-publishing.md).

## <a name="how-to-run-the-publish-scripts"></a>Julkaise-komentosarjojen suorittaminen

Jos suoritat on aina ennen Windows PowerShell-komentosarja, sinun on määritettävä suorittamisen käytännön käyttöön komentosarjojen suorittamisen. Tämä on toiminto, estä käyttäjiä suorittamasta Windows PowerShell-komentosarjojen, jos ne ovat koske haittaohjelmilta tai viruksilta, joihin sisältyy suoritetaan komentosarjoja.

### <a name="run-the-script"></a>Suorita komentosarja

1. Luo projektin Web käyttöönotto-paketti. Web-käyttöön paketti on pakattu arkisto (.zip-tiedosto), joka sisältää tiedostot, jotka haluat kopioida sivustoon tai virtuaalikoneen. Voit luoda pakettien Web käyttöönotto Visual Studiossa minkä tahansa web-sovelluksen.

![Luo sivusto paketin käyttöönotto](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Lisätietoja on artikkelissa [Toimintaohje: Web käyttöönottopaketti luominen Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Voit myös automatisoida käyttöön Web-pakkauksen luomisen osan **mukauttaminen ja julkaise-komentosarjoja laajentaminen** tämän artikkelin ohjeiden mukaisesti.

1. **Ratkaisunhallinnassa**Avaa pikavalikko komentosarja ja valitse sitten **Avaa PowerShell ise:**.

1. Jos olet suorittanut tämän tietokoneen Windows PowerShell-komentosarjojen ensimmäistä kertaa, Avaa komentokehoteikkuna järjestelmänvalvojana ja kirjoita seuraava komento:

`Set-ExecutionPolicy RemoteSigned`

1. Kirjaudu Azure seuraavat-komennon avulla.

`Add-AzureAccount`

Kun sinulta kysytään, Anna käyttäjänimi ja salasana.

Huomaa, että voit automatisoida komentosarja, kun tätä menetelmää käytetäänkin Azure tunnistetietoja ei toimi. Sen sijaan olisi tunnistetietoja .publishsettings-tiedoston avulla. Yhden kerran vain käytetään komentoa **Get-AzurePublishSettingsFile** tiedosto ladataan Azure, ja sen jälkeen **Tuo AzurePublishSettingsFile** tuotava tiedosto. Lisätietoja on artikkelissa [miten voit asentaa ja määrittää PowerShellin Azure](powershell-install-configure.md).

1. (Valinnainen) Jos haluat luoda Azure resurssit, kuten virtuaalikoneen, tietokannan ja sivuston ilman julkaiseminen web-sovelluksen, käytä **Julkaise WebApplication.ps1** -komento, jossa on **-määritys** -argumentin arvoksi määritetty JSON-kokoonpanotiedosto. Tämä komentorivi käyttää JSON-kokoonpanotiedosto Luo resursseja. Koska se käyttää oletusasetuksia muiden komentorivin argumentit, se luo resursseja, mutta ei julkaista web-sovelluksen. – Yksityiskohtainen vaihtoehto Saat lisätietoja siitä, mitä tapahtuu.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. **Julkaise WebApplication.ps1** -komennon avulla jokin seuraavien esimerkkien mukaisesti käynnistää komentosarjan ja julkaista web-sovelluksen. Jos haluat korvata oletusasetukset minkään muiden argumentteja, kuten tilauksen nimeä, julkaista pakettinimi, virtuaalikoneen tunnistetiedot tai tietokannan server-tunnistetiedot, voit määrittää nämä parametrit. Käytä **– yksityiskohtainen** vaihtoehto, jos haluat nähdä lisätietoja julkaisuprosessin edistymisen.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Jos olet luomassa virtual machine-komento näyttää seuraavalta. Tässä esimerkissä näytetään myös useiden tietokantojen tunnistetietojen määrittäminen. Komentosarjat luominen näennäiskoneiden SSL-varmennetta ei ole luotetun varmenteiden myöntäjältä. Tämän vuoksi sinun on käytettävä **– AllowUntrusted** -vaihtoehto.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Komentosarjan voit luoda tietokantoja, mutta ei luoda tietokannan palvelimiin. Jos haluat luoda tietokantapalvelimeen, voit käyttää **Uutta AzureSqlDatabaseServer** -funktio Azure-moduulin.

## <a name="customizing-and-extending-the-publish-scripts"></a>Mukauttaminen ja julkaise-komentosarjoja laajentaminen

Voit mukauttaa JSON kokoonpanotiedosto ja julkaise komentosarja. Windows PowerShell-moduulin **AzureWebAppPublishModule.psm1** funktioita ei ole tarkoitus voi muokata. Jos haluat määrittää toisen tietokannan tai muuttaa joitakin määritettävissä olevista ominaisuuksista virtuaalikoneen, muokata JSON-kokoonpanotiedosto. Jos haluat automatisoida luomiseen ja testaus projektin komentosarja toimintoja voidaan laajentaa, voit toteuttaa funktion talonki: **Julkaise WebApplication.ps1**.

Voit automatisoida luominen projektiin, Lisää tunnus, jolla MSBuild voit soittaa `New-WebDeployPackage` koodin tämän esimerkin mukaisesti. Polku MSBuild-komento on erilaiset riippuen siitä, olet asentanut Visual Studio versio. Saat oikea polku-käyttää funktiota **Get-MSBuildCmd**tämän esimerkin mukaisesti.

### <a name="to-automate-building-your-project"></a>Voit automatisoida projektin luominen

1. Lisää `$ProjectFile` parametrin yleinen parametri-osassa.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopioi funktion `Get-MSBuildCmd` komentosarja-tiedostoon.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Korvaa `New-WebDeployPackage` kanssa seuraava koodi ja korvaa rivi määrittämisessä paikkamerkit `$msbuildCmd`. Koodi on Visual Studio 2015. Jos käytät Visual Studio 2013, voit alla **VisualStudioVersion** -ominaisuuden muuttaminen `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Soita `New-WebDeployPackage` ennen tämän rivin funktio: `$Config = Read-ConfigFile $Configuration` verkkosovelluksissa tai `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` näennäiskoneiden.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Käynnistää komentoriviltä käyttämällä kulkee mukautettu komentosarja `$Project` argumentti, kuten seuraavassa esimerkissä komentoriviltä.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Automatisointiin testaaminen sovelluksesi lisätä koodin `Test-WebApplication`. Muista kommentointi **Julkaise WebApplication.ps1** , jossa kutsutaan näiden funktioiden rivit. Jos et anna toteutus, voit luoda manuaalisesti projektin Visual Studiossa ja suorita Julkaise komentosarja Azure julkaiseminen.

## <a name="publishing-function-summary"></a>Julkaisun funktioiden yhteenveto

Jos tarvitset apua toimintojen, voit käyttää Windows PowerShellin komentokehotteen, komennolla `Get-Help function-name`. Ohje sisältää parametrin ohjeita ja esimerkkejä. Saman ohjeteksti on myös komentosarja-lähdetiedostot, **AzureWebAppPublishModule.psm1** ja **Julkaise WebApplication.ps1**. Ohje ja komentosarja on lokalisoitu Visual Studio kielellä.

**AzureWebAppPublishModule**

|Funktionimi|Kuvaus|
|---|---|
|Lisää AzureSQLDatabase|Luo uusi Azure SQL-tietokantaan.|
|Lisää AzureSQLDatabases|Luo Azure SQL-tietokantoja JSON configuation tiedosto, joka luo Visual Studio arvoista.|
|Lisää AzureVM|Luo Azure virtual machine ja palauttaa käyttöön AM URL-Osoitteen. Funktio määrittää edellytykset ja kutsumalla sitten **Uusi AzureVM** -funktiota (Azure moduuli) voit luoda uuden virtual koneen.|
|Lisää AzureVMEndpoints|Lisää uusi syötteen päätepisteet virtual machine ja palauttaa uuden päätepisteissä virtuaalikoneen.|
|Lisää AzureVMStorage|Luo uuden Azure-tallennustilan tilin voimassa oleva tilaus. Tilin nimen alussa on "devtest" perään yksilöllinen aakkosnumeerinen merkkijono. Funktio palauttaa uusi tallennustilan tilin nimi. Määritettävä sijainti tai affiniteetti-ryhmästä Uusi tallennustilan tilin.|
|Lisää AzureWebsite|Luo sivusto, jossa on määritetty nimi ja sijainti. Tämä funktio kutsuu Azure-moduulissa **Uusi AzureWebsite** -funktiota. Jos tilaus ei ole jo sivustosta, jossa on määritetty nimi-funktio luo sivuston ja palauttaa sivuston objektin. Muussa tapauksessa palauttaa `$null`.|
|Varmuuskopiointi-tilaus|Tallentaa nykyisen Azure-tilaus `$Script:originalSubscription` muuttujan komentosarja-alueessa. Toiminto tallentaa Azure voimassa oleva tilaus (kuin saadaan `Get-AzureSubscription -Current`) ja tallennustilaa sen tilin ja tilauksesta, joka on muuttanut tätä komentosarjaa (tallennettu muuttujan `$UserSpecifiedSubscription`) ja sen tallennustilan-tili komentosarjan laajuus. Tallentamalla arvot, voit käyttää funktiota, kuten `Restore-Subscription`, voit palauttaa alkuperäisen nykyisen tilauksen ja tallennustilaa tilille nykyisen tilan, jos nykyinen tila on muuttunut.|
|Etsi AzureVM|Hakee määritettyyn Azure virtuaalikoneen.|
|Muotoile DevTestMessageWithTime|Eteen päivämäärän ja kellonajan lisääminen viestiin. Tämä toiminto on suunniteltu virhe ja yksityiskohtainen virtaa kirjoittamiisi viesteihin.|
|Hae AzureSQLDatabaseConnectionString|Kokoaa yhteysmerkkijonon muodostaminen Azure SQL-tietokantaan.|
|Hae AzureVMStorage|Palauttaa nimen ensimmäinen tallennustilan tilin nimi kuvion "devtest*" (kirjainkokoa) määritetyssä sijainnissa tai affiniteetti ryhmän. Jos "devtest*-tallennustilan tilin ei vastaa, sijainnin tai affiniteetti ryhmän, funktio ohittaa sen. Määritettävä sijainti tai affiniteetti-ryhmä.|
|Hae MSDeployCmd|Palauttaa komennon MsDeploy.exe-työkalua.|
|Uusi AzureVMEnvironment|Etsii tai luo virtual machine tilauksen, joka vastaa JSON-kokoonpanotiedosto arvot.|
|Julkaise WebPackage|Käyttää MsDeploy.exe ja sivuston Julkaise paketti. Zip-tiedoston resurssien käyttöön sivustossa. Tämä funktio ei luoda tuloksen. Jos MSDeploy.exe kutsu epäonnistuu, funktio ilmoittaa poikkeuksen. Jos haluat tarkempia tulosteen, käytä **-yksityiskohtainen** vaihtoehto.|
|Julkaise WebPackageToVM|Tarkistaa parametriarvot ja kutsumalla sitten **Julkaise WebPackage** -funktiota.|
|Lue ConfigFile|JSON-kokoonpanotiedosto tarkistaa ja palauttaa valitun hash arvotaulukko.|
|Palauta-tilaus|Palauttaa nykyisen tilauksesi alkuperäinen tilaus.|
|Testaa AzureModule|Palauttaa `$true` jos asennettu Azure moduuli-versio on 0.7.4 tai uudempi versio. Palauttaa `$false` Jos moduulia ei ole asennettu tai on aiempi versio. Tämä toiminto ei ole parametreja.|
|Testaa AzureModuleVersion|Palauttaa `$true` onko Azure moduulin versio 0.7.4 tai uudempi versio. Palauttaa `$false` Jos moduulia ei ole asennettu tai on aiempi versio. Tämä toiminto ei ole parametreja.|
|Testaa HttpsUrl|Muuntaa System.Uri objektin syötteen URL-osoite. Palauttaa `$True` Jos URL-osoite on suora ja sen malli on https. Palauttaa `$false` Jos URL-osoite on suhteellinen, sen värimallin ole HTTPS tai syötteen merkkijonoa ei voi muuntaa URL-osoitteeseen.|
|Testaa jäsen|Palauttaa `$true` Jos ominaisuus tai menetelmä kuuluu objektin. Muussa tapauksessa palauttaa `$false`.|
|Kirjoita ErrorWithTime|Kirjoittaa virhesanoma etuliite nykyisen kellonajan. Tämä funktio kutsuu koskevan aikaa, ennen kuin kirjoittaa viestin virhe virtaa **Muoto DevTestMessageWithTime** -funktiota.|
|Kirjoita HostWithTime|Kirjoittaa viestin etuliite nykyisen kellonajan Host (isäntä)-ohjelma (**Kirjoita isäntä**). Host (isäntä)-ohjelman kirjoittaminen vaikutus vaihtelee. Useimmat ohjelmat, jotka isännöivät Windows PowerShellin kirjoittaa viestit vakio tulos.|
|Kirjoita VerboseWithTime|Kirjoittaa yksityiskohtainen viestin etuliite nykyisen kellonajan. Koska kutsuu **Kirjoittaminen yksityiskohtainen**, näyttöön tulee sanoma vain, kun komentosarja suoritetaan **yksityiskohtainen** -parametrin tai **VerbosePreference** -asetus on määritetty **Jatka**.|

**Julkaise verkkosovellus**

|Funktionimi|Kuvaus|
|---|---|
|Uusi AzureWebApplicationEnvironment|Luo Azure resurssit, kuten sivustoon tai virtuaalikoneen.|
|Uusi WebDeployPackage|Tämä toiminto ei ole otettu käyttöön. Voit lisätä tämän funktion voit luoda projektin komentoja.|
|Julkaise AzureWebApplication|Julkaisee Azure WWW-sovellus.|
|Julkaise verkkosovellus|Luo ja ottaa käyttöön Web Apps, näennäiskoneiden, SQL-tietokannat ja tallennustilaa tilit Visual Studio web projektin.|
|Testi-verkkosovellus|Tämä toiminto ei ole otettu käyttöön. Voit lisätä tämän funktion Testaa sovelluksen komennot.|

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja PowerShell-komentosarjoja lukemalla [komentosarjan Windows PowerShellin avulla](https://technet.microsoft.com/library/bb978526.aspx) ja tarkastella muiden Azure PowerShell-komentosarjojen [Komentosarjat](https://azure.microsoft.com/documentation/scripts/).
