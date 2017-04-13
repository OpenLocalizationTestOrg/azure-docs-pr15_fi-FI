<properties
    pageTitle="Python Internetin kautta tai työntekijä roolit Visual Studiossa | Microsoft Azure"
    description="Yleistä luominen Azure pilvipalveluihin, mukaan lukien web roolit ja työntekijä roolit Python Tools for Visual Studio avulla."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python Internetin kautta tai työntekijä roolit Python Tools for Visual Studio

Tässä artikkelissa on yleiskatsaus Python Internetin kautta tai työntekijä roolien [Python][]työkaluilla Visual Studio avulla. Opit miten Visual Studio avulla voit luoda ja ottaa käyttöön basic pilvipalvelussa, joka käyttää Python.

## <a name="prerequisites"></a>Edellytykset

 - Visual Studio 2013: n tai 2015
 - [Python Tools for Visual Studio][] (PTVS)
 - [Azure SDK ja 2013: n][] tai [Azure SDK-työkalut ja 2015 julkaistut][]
 - [Python 2.7 32-bittinen][] tai [Python 3.5 32-bittinen][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Mitkä ovat Python Internetin kautta tai työntekijä roolit?

Azure tarjoaa kolme Laske sovellusten mallit: [Azure App palvelun Web Apps-toiminnon][execution model-web sites], [Azuren näennäiskoneiden][execution model-vms], ja [Azure pilvipalveluihin][execution model-cloud services]. Kaikkien kolmen mallit tukevat Python. Cloud Services-palvelut, jotka sisältävät Internetin kautta tai työntekijä roolit ovat *alusta palveluna (PaaS)*. Sisällä pilvipalveluun web-rooli on erillinen Internet Information Services (IIS) verkkopalvelimen host edusta-WWW-sovellukset, kun Työntekijä-roolin suorittamisen asynkroninen, pitkään käynnissä olevien tai pysyvät tehtävät riippumatta vuorovaikutteisuuden tai syöte.

Lisätietoja on artikkelissa [pilvipalveluun ominaisuudet?].

> [AZURE.NOTE]*Looking voit luoda yksinkertaisen sivuston?*
Jos käyttämässäsi skenaariossa vain yksinkertaisia sivuston edusta, kannattaa käyttää kevyt Web Apps-ominaisuus Azure sovelluksen-palvelussa. Voit helposti päivittää pilvipalveluun sivuston kasvaa ja tarpeen muuttaa. Katso <a href="/develop/python/">Python Developer Center</a> artikkeleita, jotka kattavat kehittäminen Azure App palvelun Web Apps-toiminnon.
<br />


## <a name="project-creation"></a>Projektin luominen

Valitse Visual Studiossa, valitse **Uusi projekti** -valintaikkunan **Python** **Azure pilvipalvelussa** .

![Uusi projekti-valintaikkuna](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Ohjatun Azure pilvipalvelussa voit luoda uusi web- ja työntekijä roolit.

![Azure Cloud palvelun valintaikkuna](./media/cloud-services-python-ptvs/new-service-wizard.png)

Työntekijän rooli malliin sisältyy asiakirjan osien uudelleen käyttäminen koodin Azure-tallennustilan tilin tai Azure palvelun Bus.

![Cloud Service-ratkaisua](./media/cloud-services-python-ptvs/worker.png)

Voit lisätä sivuston tai työntekijä roolit aiemmin pilvipalveluun milloin tahansa.  Voit lisätä aiemmin projekteja ratkaisu tai luoda uusia.

![Lisää rooli-komento](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Cloud-palvelussa voi olla käytössä eri kielillä roolit.  Voit määrittää esimerkiksi käyttämällä Django, Python tai C# työntekijä roolit Python web-rooli.  Voit helposti viestiä yhteyttä palvelun Bus olevien tai tallennustilan olevien roolien välillä.

## <a name="install-python-on-the-cloud-service"></a>Asenna Python pilvipalvelussa

>[AZURE.WARNING] Asetukset-komentosarjoja, jotka on asennettu Visual Studiossa (Tässä artikkelissa on viimeksi päivitetty milloin) eivät toimi. Tässä osassa kuvataan seuraavaa vaihtoehtoista menetelmää.

Asetukset-komentosarjoja tärkeimmät ongelma on, että he eivät asenna python. Määritä ensin [Käynnistys tehtävien](cloud-services-startup-tasks.md) [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) -tiedosto. Ensimmäinen tehtävä (**PrepPython.ps1**) Lataa ja asentaa Python runtime. Toinen tehtävä (**PipInstaller.ps1**) suorittaa pip riippuvuuksia, sinun on ehkä asennettava.

Komentosarjojen alla on kirjoitettu kohdistamisen Python 3.5. Jos haluat käyttää python, 2.x määrittäminen **PYTHON2** muuttujan tiedoston, **Valitse** käynnistys tehtävät ja runtime-tehtävän: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

**PYTHON2** ja **PYPATH** muuttujat on lisättävä työntekijän käynnistys-tehtävä. **PYPATH** muuttujaa käytetään vain, jos **PYTHON2** muuttuja on määritetty **Kyllä**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Esimerkki ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Seuraavaksi luodaan **PrepPython.ps1** ja **PipInstaller.ps1** tiedostot **. / palkin** tehtäväsi-kansioon.

#### <a name="preppythonps1"></a>PrepPython.ps1

Tämä komentosarja asentaa python. Jos **PYTHON2** enviornment muuttuja on määritetty **,** Valitse Python 2.7 asennetaan, muuten Python 3.5 asennetaan.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Tämä komentosarja kutsuu määrittäminen pip ja asentaa kaikki riippuvuudet **requirements.txt** -tiedostossa. Jos **PYTHON2** enviornment muuttuja on määritetty **,** Valitse Python 2.7 käytetään, muuten Python 3.5 käytetään.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Muokkaa LaunchWorker.ps1

>[AZURE.NOTE] **Työntekijän rooli** , projektin kyseessä **LauncherWorker.ps1** tiedoston tarvitaan Suorita käynnistystiedostoa. **Web-rooli** , projektin käynnistys-tiedoston sen sijaan määritetään projektin ominaisuuksien.

**Bin\LaunchWorker.ps1** on alun perin luotu tehtävä on paljon prep, mutta se ei toimi todella. Korvaa tiedoston sisältöä seuraavaa komentosarjaa.

Tämä komentosarja kutsuu **worker.py** tiedoston python projektin. Jos **PYTHON2** enviornment muuttuja on määritetty **,** Valitse Python 2.7 käytetään, muuten Python 3.5 käytetään.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Visual Studio mallit olisi luonut **ps.cmd** tiedoston **. / palkin** kansio. Tämän liittymän komentosarja soittaa ulos yllä PowerShell paketti-komentosarjojen ja tarjoaa kirjaaminen PowerShell-paketti, jota kutsutaan nimen perusteella. Jos tiedosto ei ole luotu, näin mitä pitäisi olla ei. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Suorita paikallisesti

Jos määrität cloud palvelun projektin käynnistys-projektin ja painamalla F5-näppäintä, pilvipalvelussa sijaitsevaan suoritetaan paikallisen Azure emulaattorin.

Vaikka PTVS tukee avaamista emulaattori, virheenkorjaus (esimerkiksi keskeytyskohdat) ei toimi.

Korjaamisessa Internetin kautta tai työntekijä roolit, voit määrittää roolin projektin käynnistys-projektin ja korjaaminen, sen sijaan.  Voit myös määrittää useita käynnistys projekteja.  Ratkaisu hiiren kakkospainikkeella ja valitse sitten **Aseta käynnistys projektit**.

![Ratkaisu projektin käynnistysasetukset](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Azure julkaiseminen

Jos haluat julkaista, cloud palvelun projektin ratkaisun hiiren kakkospainikkeella ja valitse sitten **Julkaise**.

![Microsoft Azure julkaista kirjautuminen](./media/cloud-services-python-ptvs/publish-sign-in.png)

Noudata ohjatun toiminnon. Jos haluat, ota käyttöön etätyöpöytä. Etätyöpöytä on hyötyä, kun haluat korjata jotain.

Kun olet valmis määrittäminen asetukset, valitse **Julkaise**.

Jotkin edistyminen näkyy kohde-ikkunassa ja valitse näyttöön tulee Microsoft Azure tehtävän loki-ikkunaan.

![Microsoft Azure tehtävän loki-ikkuna](./media/cloud-services-python-ptvs/publish-activity-log.png)

Käyttöönotto voi kestää useita minuutteja ja valitse verkko ja/tai työntekijä roolit toimii Azure!

### <a name="investigate-logs"></a>Tutkia lokit

Kun cloud palvelun virtuaalikoneen käynnistyy ja asentaa Python, voit etsiä viestejä virheen lokit voi tarkastella. Lokitiedostot sijaitsevat **C:\Resources\Directory\\{roolin} \LogFiles** kansio. **PrepPython.err.txt** on vähintään yksi virhe ei, kun komentosarja yrittää tunnistaa, jos Python on asennettu ja **PipInstaller.err.txt** saattaa hitaista pip vanhentunut versio.

## <a name="next-steps"></a>Seuraavat vaiheet

Tarkempia tietoja Internetin kautta tai työntekijä roolit Python työkalujen käsitteleminen Visual Studio PTVS ohjeissa:

- [Cloud palvelun projektit][]

Lisätietoja Azure Internetin kautta tai työntekijä roolit, kuten Azuren tallennustilaan tai Service Bus-palveluiden avulla on seuraavissa artikkeleissa.

- [BLOB-palvelu][]
- [Taulukko-palvelu][]
- [Jonopalvelu][]
- [Palvelun Bus olevien][]
- [Palvelun Bus aiheita][]


<!--Link references-->

[Mikä on pilvipalveluun?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[BLOB-palvelu]: ../storage/storage-python-how-to-use-blob-storage.md
[Jonopalvelu]: ../storage/storage-python-how-to-use-queue-storage.md
[Taulukko-palvelu]: ../storage/storage-python-how-to-use-table-storage.md
[Palvelun Bus olevien]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Palvelun Bus aiheita]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud palvelun projektit]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK-työkalut ja 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK työkaluja ja 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bittinen]: https://www.python.org/downloads/
[Python 3.5 32-bittinen]: https://www.python.org/downloads/
