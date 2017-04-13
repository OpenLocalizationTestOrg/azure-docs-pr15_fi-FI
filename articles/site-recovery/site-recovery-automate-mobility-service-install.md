<properties
    pageTitle="Replikoida VMware näennäiskoneiden Azure käyttämällä palauttaminen Azure automaatio DSC | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure automaatio DSC avulla voit ottaa automaattisesti käyttöön Azure Azure sivuston palauttaminen Mobility-palveluun ja virtual/fyysisiä koneet Azure-agentti."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Replikoida VMware näennäiskoneiden Azure käyttämällä Azure automaatio DSC sivuston palauttaminen

Toimintojen hallinta-ohjelmistopaketin emme antaa sinulle täydellinen varmuuskopiointi- ja palauttamisratkaisun, joiden avulla voit jatkuvuuden liiketoimintasuunnitelma osana.

Tämä matkan ja Hyper-V aloittama Hyper-V replikan avulla. Mutta emme ole laajennettu tukemaan erilaisten asetukset, koska asiakkaiden on useita hypervisors ja ympäristöjen niiden paveikslėlis.

Jos käytät VMware toiminnoista ja/tai fyysiset palvelimet tänään, asiakirjanhallinnan palvelimeen suorittaa kaikki Azure palauttaminen osat ympäristössäsi käsittelee viestintää ja tietojen replikointi Azure-kanssa, kun Azure on hyperlinkin kohde.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Sivuston palauttaminen Mobility-palvelun asentavat automaatio DSC
Aloitetaan tekemällä nopeasti hajautuksen tämän hallintapalvelin ei.

Asiakirjanhallinnan palvelimeen suorittaa useita palvelinrooleista. Yksi rooli on *määritys*, joka koordinoi viestintä ja hallitsee tietojen replikoinnin ja hyödyntämistä prosessit.

Lisäksi *prosessi* -roolin toimii replikoinnin yhdyskäytävä. Tämä rooli vastaanottaa replikoinnin tietoja suojatun lähde koneet, optimoi tallentamisesta välimuistiin, pakkaamisen ja salaus ja lähettää sen sitten Azure-tallennustilan tilin. Prosessi-roolin funktioita on myös push asennuksen Mobility-palvelun suojatun koneet ja tee VMware VMs Automaattinen etsiminen.

Jos näkyvissä on tuntisesta azuren, *master kohde* -roolin käsittelee replikoinnin tiedot osana tätä toimintoa.

Suojatun tietokoneissa on luottavat *Mobility-palvelun*. Tämän osan otetaan käyttöön jokaiseen tietokoneeseen (VMware AM tai fyysinen palvelin), jonka haluat toistaa Azure. Se sieppaa tietojen kirjoituksia tietokoneeseen ja välittää niitä management server (prosessin rooli).

Kun olet käsittely Liiketoiminnan jatkuvuus, on tärkeää ymmärtää, että työmääriä, infrastruktuurin ja liittyvät komponentit. Voit sitten täytä edellytyksiä palautus aika tavoitteen (RTO) ja palautus pisteen tavoitteen (RPO). Tässä yhteydessä Mobility-palvelu on varmistaa, että toiminnoista on suojattu samoin kuin-näppäintä.

Mistä, optimoitu tavalla varmistaa, että sinulla on luotettava suojatun asennuksen toimintojen hallinta Suite joitakin osia avulla?

Tässä artikkelissa on esimerkki siitä, miten voit käyttää Azure automaatio toivottuja tilan määrittäminen (DSC), sivuston palauttaminen ja varmistaa, että:

- Mobility-palveluun ja Azure AM agentti otetaan käyttöön Windows-tietokoneissa, jonka haluat suojata.
- Mobility-palveluun ja Azure AM agentti aina käynnissä, kun Azure replikoinnin kohteena.

## <a name="prerequisites"></a>Edellytykset

- Säilön tallentamiseen tarvittavat asetukset
- Säilön tallentamiseen tarvittavat salasana rekisteröidä hallinta-palvelimen kanssa

 > [AZURE.NOTE] Yksilöivä salasana luodaan kunkin management Server. Jos aiot asentaa useita hallinta-palvelimia, sinun on varmistaa, että oikea salasana on tallennettu passphrase.txt-tiedostoon.

- Windows Management Framework (WMF) 5.0 asennettu tietokoneissa, joihin haluat ottaa käyttöön protection (automaatio DSC vaatimus)

 > [AZURE.NOTE] Jos haluat käyttää DSC for Windows-tietokoneissa, joissa on asennettu WMF 4.0-kohdassa [Käytä DSC yhteys katkaistu ympäristöissä](#Use DSC in disconnected environments).

Mobility-palvelun kautta komentorivin voidaan asentaa ja hyväksyy useita argumentteja. Minkä vuoksi on binaaritiedostoja (sen jälkeen, kun ne purkaminen asetukset) ja tallentaa ne paikka, josta voit hakea ne DSC määrityksen avulla.

## <a name="step-1-extract-binaries"></a>Vaihe 1: Pura binaaritiedostot

1. Pura tiedostot, jotka tarvitset nämä asetukset, siirry seuraavaan kansioon hallinta-palvelimeen:

    **\Microsoft azure sivuston Recovery\home\svsystems\pushinstallsvc\repository**

    Tämän kansion pitäisi näkyä MSI-tiedosto, jonka nimi on:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Seuraavalla komennolla voit purkaa asennusohjelma:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Valitse kaikki tiedostot ja lähetä ne pakattu kansio.

Binaaritiedostot, jotka haluat automatisoida Mobility-palvelun asetusten avulla Automation DSC on nyt.

### <a name="passphrase"></a>Salasana

Seuraavaksi sinun täytyy määrittää, johon haluat sijoittaa uuden kansion pakatun. Voit käyttää Azure-tallennustilan tilin esitetyllä myöhemmin, salasana, jonka on asetusten tallentamiseen. Agentti sitten Rekisteröi yhteydessä hallinta-palvelimen kanssa.

Salasana, jotka olet saanut kun asiakirjanhallinnan palvelimeen käyttöön voidaan tallentaa passphrase.txt tekstitiedostoon.

Lisää oma säilön Azure-tallennustilan tilin zip-kansio ja salasana.

![Kansiosijainti](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Jos haluat säilyttää nämä tiedostot jaettuun verkossa, voit tehdä. Haluat varmistaa, että käytät myöhemmin DSC käyttämäsi resurssi on käyttöoikeus ja käyttää asetukset ja salasana.

## <a name="step-2-create-the-dsc-configuration"></a>Vaihe 2: Luo DSC-määritys

Asetuksista riippuen WMF 5.0. Koneen sovelletaan määritys – automaatio DSC onnistuneesti WMF 5.0 on oltava käytössä.

Ympäristön käyttää esimerkki DSC-asetukset ovat seuraavat:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Kokoonpano seuraavasti:

- Muuttujien kertoo määritykset hankkiminen binaaritiedostoja Mobility-palveluiden ja Azure AM-agentti hankkiminen salasana ja tallennuspaikan tulos.
- Määritykset tuo xPSDesiredStateConfiguration DSC resurssin niin, että voit käyttää `xRemoteFile` voit ladata tiedostot säilöstä.
- Kokoonpanon Luo kansio, johon haluat tallentaa binaaritiedostoja.
- Arkisto-resurssin Pura tiedostot zip-kansiosta.
- Paketin asennuksen resurssin asennetaan Mobility-palvelu UNIFIEDAGENT. EXE installer tiettyjen argumenttien mukaisesti. (Muuttujat, joka käyttää argumenttien tarvitse ympäristön muuttuvat.)
- Paketin AzureAgent resurssin asennetaan Azure AM-agentti, mikä on suositeltavaa jokaisen AM, joka suoritetaan Azure-tietokannassa. Azure AM-agentti myös mahdollistaa sellaisten tunnisteet lisääminen AM automaattisesti.
- Palveluresurssi- tai resurssien varmistat, että liittyvät Mobility-palvelujen ja Azure palvelut aina käynnissä.

Voit tallentaa **ASRMobilityService**määritykset.

>[AZURE.NOTE] Muista korvata CSIP kokoonpanoa vastaamaan todellinen asiakirjanhallinnan palvelimeen, jotta agentti kytketty oikein ja käyttää oikea salasana.

## <a name="step-3-upload-to-automation-dsc"></a>Vaihe 3: Lataa automaatio DSC

Koska DSC määrityksistä, jotka on tehty tuo tarvittavat DSC Resurssimoduuli (xPSDesiredStateConfiguration), sinun on tuotava automaatio-moduulin, ennen kuin lataat DSC-määritys.

Kirjautuminen automaatio-tilille, siirry **varat** > **Moduulit**, ja valitse **Selaa-valikoima**.

Tässä voit hakea moduulin ja tuominen tiliin.

![Tuo-moduuli](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Kun olet valmis tämä, siirry käyttämääsi laitteeseen, joihin on asennettu Azure Resurssienhallinta moduulit ja siirry tuo juuri luomasi DSC-määritys.

### <a name="import-cmdlets"></a>Tuo cmdlet-komennot

Azure-tilaukseen kirjautuminen PowerShellin. Muokkaa ympäristön ja siepata automaatio tilitietoihin muuttujaan cmdlet-komennot:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Lataa määritykset automaatio DSC seuraavat cmdlet-komennolla:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Käännä automaatio DSC määrittäminen

Seuraavaksi sinun täytyy kääntää automaatio DSC määrittäminen niin, että voit aloittaa rekisteröidä solmujen siihen. Voit saavuttaa suorittamalla seuraavat cmdlet-komennon:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Tämä voi kestää muutaman minuutin kuluttua, koska olet lähinnä käyttöönotto määritykset isännöityä DSC salaus puretaan-palveluun.

Kun kääntää määritykset, voit hakea työn tiedot käyttämällä PowerShell (Get-AzureRmAutomationDscCompilationJob) tai [Azure portal](https://portal.azure.com/).

![Työn hakeminen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Olet nyt julkaistu ja ladata DSC kokoonpanosi automaatio DSC.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Vaihe 4: Määrän koneet automaatio DSC
>[AZURE.NOTE] Jokin edellytyksistä Viimeistellään tässä skenaariossa on, että Windows-tietokoneissa on päivitetty WMF uusimman version. Voit ladata ja asenna [Download Centeristä](https://www.microsoft.com/download/details.aspx?id=50395)omaa ympäristöäsi varten.

Luo nyt metaconfig DSC, jotka koske oman solmujen varten. Kun tämä onnistuu, sinun täytyy noutaa päätepisteen URL-osoite ja perusavaimen valitun automaatio-tilin Azure-tietokannassa. Voit etsiä nämä arvot kohdassa **näppäimet** automaatio-tilin **kaikki asetukset** -sivu.

![Avainarvot](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Tässä esimerkissä käytössä Windows Server 2012 R2 fyysinen palvelin, jonka haluat suojata sivuston palauttaminen.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Tarkista odottavia nimeä tiedostotoiminnot rekisterin

Ennen kuin aloitat palvelimen liitettävä automaatio DSC päätepisteen, on suositeltavaa, että tarkistat mille tahansa odottavien tiedoston nimeä toimintojen rekisterissä. Ne voivat estä määritykset viimeistely vuoksi odotetaan uudelleen.

Suorita seuraavat cmdlet-komennolla voit varmistaa, että ei ole odotetaan uudelleenkäynnistyksen palvelimessa:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Jos tämä on tyhjä, olet OK, niin siirryt. Muussa tapauksessa kannattaa osoite mukaan palvelin uudelleenkäynnistyksen yhteydessä ylläpito-ikkuna.

Voit käyttää määritykset palvelimessa, Käynnistä PowerShell integroitu komentosarjat ympäristössä (ise:) ja suorita seuraavaa komentosarjaa. Tämä on käytettävä DSC paikallisen kokoonpano, joka opastaa rekisteröidä automaatio DSC-palveluun ja Nouda kokoonpano (ASRMobilityService.localhost) paikallisen määritysten hallinta-ohjelma.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Tässä määrityksessä aiheuttavat rekisteröityä itse automaatio DSC paikallinen määritysten hallinta-ohjelma. Se myös määrittää, kuinka kannattaa toimia moduulin, mitä se pitäisi tehdä, jos määritysten ajoverkoilla (ApplyAndAutoCorrect) ja miten se Jatka määritykset Jos tietokone on käynnistettävä uudelleen.

Kun olet suorittanut tämän komentosarjan, solmu kannattaa aloittaa rekisteröidä automaatio DSC.

![Solmun rekisteröinti käynnissä](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Jos siirryt takaisin Azure-portaaliin, näet, että juuri rekisteröity solmu on nyt näkyi portaalin.

![Rekisteröity solmu-portaalissa](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Palvelimessa voit suorittaa seuraavan PowerShell cmdlet-komennon voit varmistaa, että solmu on rekisteröity oikein:

```powershell
Get-DscLocalConfigurationManager
```

Kun määritykset on otettu mukaan ja käytetty palvelimeen, voit tarkistaa suorittamalla seuraavat cmdlet-komennon:

```powershell
Get-DscConfigurationStatus
```

Tulos näkyy, että palvelin on onnistuneesti vedetään sen määritykset:

![Tulos](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Lisäksi Mobility-palvelun asetukset on oma lokitiedoston, joka on osoitteessa *SystemDrive*\ProgramData\ASRSetupLogs.

Se on. Olet nyt käyttöön ja tietokoneen, jonka haluat suojata sivuston palauttaminen Mobility-palvelu rekisteröidyt. DSC varmistaa, että tarvittavat palvelut ovat aina käytössä.

![Onnistunut käyttöönotto](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Jälkeen asiakirjanhallinnan palvelimeen havaitsee onnistunut käyttöönotto, voit määrittää suojaus ja ottaa replikoinnin tietokoneeseen käyttämällä sivuston palauttaminen.

## <a name="use-dsc-in-disconnected-environments"></a>Käytä DSC yhteys katkaistu ympäristöissä

Jos tietokoneesi ei ole yhteydessä Internetiin, voit ottaa käyttöön ja Mobility-palvelun määrittämistä toiminnoista, jotka haluat suojata DSC luottavat edelleen.

Voit vahvistaa oman DSC salaus puretaan palvelimen ympäristösi on käytettävä samaa ominaisuuksista, joita sinulla on automaatio DSC. Toisin sanoen asiakkaat otetaan DSC päätepisteen määritykset (sen jälkeen, kun se on rekisteröity). Toinen vaihtoehto on kuitenkin push manuaalisesti DSC-määritysten lisääminen koneet paikallisesti tai etäyhteyden välityksellä.

Huomaa, että tässä esimerkissä on lisätty parametri, joka on tietokoneen nimi. Remote tiedostot sijaitsevat nyt jaettu kansio, joka on oltava käytettävissä koneet, jonka haluat suojata mukaan. Komentosarjan enacts määritykset ja sitten alkaa käyttää DSC asetuksia kohde tietokoneeseen.

### <a name="prerequisites"></a>Edellytykset

Varmista, että xPSDesiredStateConfiguration PowerShell-moduuli on asennettu. Windows-tietokoneissa, johon WMF 5.0 on asennettu voit asentaa xPSDesiredStateConfiguration moduulin suorittamalla seuraavat cmdlet-komennon kohde-tietokoneissa:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Voit ladata ja Tallenna moduuli siltä varalta, että haluat jakaa sen Windows-tietokoneissa, joissa on WMF 4.0. Suorittaa cmdlet tietokoneessa, jossa PowerShellGet (WMF 5.0) ei sisällä tietoja:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Myös WMF 4. 0: Varmista, että [Windows 8.1 Päivitä KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) on asennettu tietokoneissa.

Windows-tietokoneissa, joissa on WMF 5.0 / WMF 4.0 voivat sijaita asetukset ovat seuraavat:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Jos haluat vahvistaa oman DSC salaus puretaan palvelimen jäljitteleminen ominaisuuksista, joita voit käyttää Automation DSC yrityksen verkossa, on [erotettu DSC verkkopalvelimen on määritetty](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Valinnainen: Käyttöönotto DSC määritysten Azure Resurssienhallinta-mallin avulla

Tässä artikkelissa on kohdistettu siitä, miten voit luoda oman DSC määritysten Mobility-palvelun ja Azure AM agentti käyttöönotto ja varmistaa automaattisesti ne käynnissä olevat tietokoneissa, joihin haluat suojata. On myös Azure Resurssienhallinta-malli, joka otetaan käyttöön DSC määritysten uuteen tai aiemmin luotuun Azure automaatio-tilille. Mallin luominen automaatio-resurssit, joka sisältää muuttujat ympäristössä käyttämällä syöteparametrit.

Kun otat mallin käyttöön, voit jättää viitata vaiheeseen 4 oppaan määrän tietokoneesi.

Malli seuraavasti:

1. Käytä aiemmin luotua automaatio-tiliä tai luoda uuden
2. Tehdä syöteparametrit:
    - ASRRemoteFile--sijainti, johon on tallennettu Mobility-palvelun asetukset
    - ASRPassphrase--sijainti, johon on tallennettu passphrase.txt-tiedosto
    - ASRCSEndpoint--hallinnan palvelimen IP-osoite
3. Tuo xPSDesiredStateConfiguration PowerShell-moduuli
4. Luo ja kääntää DSC-määritys

Edellisten vaiheiden tapahtuu oikeassa järjestyksessä niin, että voit aloittaa onboarding oman koneet suojautumista.

Mallin käyttöönoton ohjeet sijaitsee [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Ottaa mallin käyttöön PowerShellin avulla:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Seuraavat vaiheet

Kun Mobility-palvelun tekijöiden otetaan käyttöön, voit näennäiskoneiden [käyttöön replikoinnin](site-recovery-vmware-to-azure.md#step-6-replicate-applications) .
