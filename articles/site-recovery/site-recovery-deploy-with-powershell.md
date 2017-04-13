<properties
    pageTitle="Toistaa Hyper-V näennäiskoneiden VMM paveikslėlis Azure palauttaminen ja PowerShell | Microsoft Azure"
    description="Lue, miten voit automatisoida Hyper-V näennäiskoneiden VMM paveikslėlis palauttaminen ja PowerShell-replikoinnin."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Toistaa Hyper-V näennäiskoneiden-VMM paveikslėlis Azure PowerShellin – perinteinen

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Perinteinen Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – perinteinen](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Yleiskatsaus

Azure palauttaminen osaltaan mukaan orchestrating replikoinnin, automaattisesti ja palauttamisen käyttöönottoskenaariot useilla näennäiskoneiden yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Luettelon kaikista käyttöönoton skenaariot artikkelissa [Azure palauttaminen yleiskatsaus](site-recovery-overview.md).

Tämän artikkelin avulla voit automatisoida yleisiä tehtäviä, sinun täytyy suorittaa, kun määrität Azure palauttaminen replikointia Hyper-V näennäiskoneiden-System Center VMM paveikslėlis Azure tallennustilan PowerShellin avulla.

Artikkelin sisältää skenaarion edellytyksistä ja kerrotaan, miten määrittäminen sivuston palautus-säilö, Azure sivuston palauttaminen tarjoajan asentaminen VMM lähdepalvelimeen, rekisteröidä palvelimen säilö, Azure-tallennustilan tilin lisääminen, asenna Azure palautus palvelut-agentti Hyper-V host-palvelimissa, VMM paveikslėlis, jota käytetään kaikissa suojatun näennäiskoneiden suojauksen asetusten määrittäminen , ja valitse Ota käyttöön kyseiset näennäiskoneiden suojaus. Valmis testaamalla vikasietotilaa varmistaaksesi, että kaikki toimii odotetulla tavalla.

Jos kohtaat ongelmia määrittämisestä artikkelista, lähettää kysymyksiä [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md). Tässä artikkelissa käsitellään perinteinen käyttöönotto-mallin avulla.



## <a name="before-you-start"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset on paikassa:

### <a name="azure-prerequisites"></a>Azure edellytykset

- Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
- Tarvitset Azure-tallennustilan tilin replikoitua tietojen tallentamiseen. Tili on geo replikoinnin käytössä. Se olisi Azure palauttaminen säilö kanssa samalla alueella ja saman tilauksen yhdistetä. [Lisätietoja Azure-tallennustilan](../storage/storage-introduction.md).
- Tarvitset, varmista, että haluat suojata näennäiskoneiden noudattavat [Azure virtuaalikoneen edellytykset](site-recovery-best-practices.md#virtual-machines).

### <a name="vmm-prerequisites"></a>VMM edellytykset
- Tarvitset VMM System Center 2012 R2-palvelimeen.
- Tarvitset vähintään yksi cloud suojattava VMM palvelimessa. Pilveen tulisi sisältää:
    - Yksi tai useampi VMM host ryhmä.
    - Hyper-V host-palvelimissa tai klustereiden host kunkin ryhmän.
    - Yhden tai useamman näennäiskoneiden lähteen Hyper-V-palvelimeen.

### <a name="hyper-v-prerequisites"></a>Hyper-V edellytykset

- Hyper-V host-palvelimissa on oltava vähintään **Windows Server 2012** Hyper-V-roolin tai **Microsoft Hyper-V Server 2012: n** kanssa, muttet asentanut uusimmat päivitykset.
- Jos käytössäsi on Hyper-V-klusterin muistiinpanon kyseisen klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun on määrittää klusterin broker manuaalisesti. Voit tehdä tämän Serverin hallinta > automaattisesti klusterin hallinta muodostaa yhteyttä klusterin, valitse **Määritä rooli** ja valitse **Hyper-V replikan Broker** suuren käytettävyyden ohjatun toiminnon **Valitse rooli** -näytössä.
- Hyper-V host palvelin- tai klusterin, jonka haluat hallita suojaus täytyy sisältyä VMM pilvestä.

### <a name="network-mapping-prerequisites"></a>Verkon määritys edellytykset
Kun suojaat näennäiskoneiden Azure verkon määritys karttoja AM-verkkojen VMM lähdepalvelimeen välillä ja kohdeluettelo Azure verkkojen käyttöön seuraavasti:

- Kaikkien tietokoneissa, joissa epäonnistuvat saman verkon kautta voit muodostaa yhteyden toisistaan riippumatta palautus palvelupaketin ne sijaitsevat.
- Jos yhdyskäytävien on aikataulussa Azure verkon määritys, näennäiskoneiden muodostaa paikallisen muiden näennäiskoneiden.
- Jos et määritä verkon määritys vain näennäiskoneiden, jotka eivät läpäise päälle saman palautus-suunnitelmassa voivat muodostaa yhteyden toistensa jälkeen Azure automaattisesti.

Jos haluat ottaa käyttöön verkon määritys sinun on seuraavasti:

- Tietolähteen VMM palvelimessa suojattava näennäiskoneiden tulee olla yhdistetty AM verkkoon. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
- Azure verkko, johon replikoitua näennäiskoneiden muodostaa automaattisesti jälkeen. Voit valita verkoston automaattisesti aikaan. Verkossa on oltava sama alue kuin Azure palauttaminen tilauksen.
- [Lue lisää](site-recovery-network-mapping.md) siitä, verkon määritys:

###<a name="powershell-prerequisites"></a>PowerShellin edellytykset
Varmista, että sinulla on valmis lähetettäväksi PowerShellin Azure. Jos käytössäsi on jo PowerShell, tarvitset päivittäminen versioksi 0.8.10 tai uudempi versio. Lisätietoja PowerShell Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md). Kun olet määrittänyt ja määritetty PowerShell, voit tarkastella kaikkia käytettävissä cmdlet-komennot-palvelun [tähän](https://msdn.microsoft.com/library/dn850420.aspx).

Lisätietoja vihjeitä, joiden avulla voit käyttää Cmdlet-komentoja, kuten miten parametriarvot syötteiden ja tulostaa yleensä käsitellään PowerShellin Azure-artikkelissa [Azure cmdlet-komentojen käytön aloittaminen](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Vaihe 1: Määritä tilaus

PowerShellin, suorita seuraavat cmdlet-komennot:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Korvaa "< >" elementteihin tietyt tiedot.

## <a name="step-2-create-a-site-recovery-vault"></a>Vaihe 2: Luo sivuston palautus-säilö

Korvaa "< >" elementteihin tarkkoja tietoja PowerShell- ja Suorita nämä komennot:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Vaihe 3: Luo säilöön rekisteröinti avain

Luo säilö rekisteröinti-näppäintä. Kun olet ladannut Azure sivuston palauttaminen tarjoaja ja asenna se VMM-palvelimeen, käytät tätä näppäintä voit rekisteröidä VMM palvelimen säilö.

1.  Hanki säilö asetustiedoston ja määritä yhteydessä:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Määritä säilö yhteydessä suorittamalla seuraavat komennot:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Vaihe 4: Azure sivuston palauttaminen Providerin asentaminen

1.  VMM tietokoneessa Luo kansio suorittamalla seuraavan komennon:

    ```

    pushd C:\ASR\

    ```

2.  Pura ladattu-palvelun avulla suorittamalla seuraavan komennon tiedostot

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Asenna palveluntarjoajaan käyttämällä seuraavia komentoja:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Odota, viimeistele asennus.

4.  Rekisteröi palvelimen säilöön käyttämällä seuraava komento:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Vaihe 5: Azure-tallennustilan tilin luominen

Jos sinulla ei ole Azure-tallennustilan tilin, luoda geo replikoinnin käytössä tilin suorittamalla seuraavan komennon:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Huomaa, että tallennustilan tilin on oltava samalla alueella Azure palauttaminen-palvelu ja samaan tilaukseen.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Vaihe 6: Asenna Azure palautus palvelut-agentti

Azure-portaalista Asenna Azure palautus palvelut-agentti kunkin Hyper-V Host (isäntä)-palvelimeen, VMM paveikslėlis, jonka haluat suojata sijaitsee.

Valitse kaikki VMM isännät varten suorittamalla seuraavan komennon:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Vaihe 7: Määritä cloud suojausasetuksia

1.  Luo Azure cloud suojaus profiilia suorittamalla seuraavan komennon:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Hanki suojauksen säilö suorittamalla seuraavat komennot:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Aloita suojaus-säilö liitos pilven kautta:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Kun työ on suoritettu, suorita seuraava komento:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Kun työ on suoritettu käsittely, suorita seuraava komento:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.

## <a name="step-8-configure-network-mapping"></a>Vaihe 8: Määrittäminen verkon määritys
Ennen kuin aloitat verkon määritys Varmista, että näennäiskoneiden lähteen VMM palvelimessa yhteydessä verkkoon AM. Lisäksi voit luoda yhden tai useamman Azure virtual verkot. Huomaa, että useiden AM verkostojen voidaan määrittää yhden Azure verkoston.

Huomaa, että, jos kohde verkossa on useita aliverkosta ja jokin näistä aliverkosta on sama nimi kuin aliverkon joina lähde virtuaalikoneen sijaitsee ja valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon jälkeen automaattisesti. Jos aliverkon kohde vastaavaa nimeä ei ole, virtuaalikoneen kytketystä ensimmäisen aliverkon verkossa.

Ensimmäisen komennon noutaa palvelinten nykyisen Azure palauttaminen säilö. Komento tallentaa $Servers matriisin muuttujan Microsoft Azure sivuston palauttaminen-palvelimiin.

    $Servers = Get-AzureSiteRecoveryServer


Toinen komento saa sivuston palauttaminen verkon ensimmäisen palvelimen $Servers matriisista. Komento tallentaa verkkojen $Networks muuttuja.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Kolmas komento saa Azure tilauksistasi Get-AzureSubscription cmdlet-komennolla ja tallentaa sitten kyseisen arvon $Subscriptions muuttuja.

    $Subscriptions = Get-AzureSubscription



Neljäs komento saa Azure virtual verkkojen käyttämällä $AzureVmNetworks muuttujan Get-AzureVNetSite cmdlet-komento ja valitse sitten arvo.


    $AzureVmNetworks = Get-AzureVNetSite



Lopullinen cmdlet-komento luo yhdistämismäärityksen ensisijaisen verkon ja Azure virtuaalikoneen verkon välillä. Cmdlet määrittää ensisijaisen verkon $Networks ensimmäinen elementti. Cmdlet määrittää virtuaalikoneen verkon $AzureVmNetworks ensimmäinen elementti käyttämällä sen ID-tunnuksellasi. Komento sisältää Azure tilauksen käyttämällä.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Vaihe 9: Käyttöönotto näennäiskoneiden suojaus

Kun palvelimissa, paveikslėlis ja käyttäminen on määritetty oikein, voit ottaa suojauksen näennäiskoneiden pilveen. Ota seuraavat seikat huomioon:

Näennäiskoneiden on täytettävä [Azure virtuaalikoneen edellytykset](site-recovery-best-practices.md#virtual-machines).

Ottaa käyttöön suojauksen käyttöjärjestelmä ja käyttöjärjestelmän levyn ominaisuudet on määritettävä virtuaalikoneen. Kun luot virtual machine VMM virtuaalikoneen mallin avulla voit määrittää ominaisuus. Voit myös määrittää nämä ominaisuudet aiemmin näennäiskoneiden virtuaalikoneen ominaisuudet **Yleiset** ja **Laitekokoonpanon** -välilehdissä. Jos et määritä nämä ominaisuudet-VMM voit voi määrittää ne Azure sivuston palautus-portaalissa.



1.  Ota suojaus käyttöön suorittamalla seuraavan komennon saat suojaus-säilö:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Hae suojaus-kohteen (AM) suorittamalla seuraava komento:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Ota käyttöön DR AM varten suorittamalla seuraavan komennon:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testata käyttöönoton

Jos haluat testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhdelle tietokoneelle, tai useita näennäiskoneiden koostuva palautus suunnitelman luominen ja Suorita testi-automaattisesti palvelupaketin. Testaa automaattisesti varmistaminen automaattisesti ja palautus-järjestelmä eristetyssä verkossa. Huomaa, että:

- Jos haluat muodostaa yhteyden virtuaalikoneen Azure-tietokannassa etätyöpöydän vikasietotilaa jälkeen, ota käyttöön virtuaalikoneen Etätyöpöytäyhteys, ennen kuin suoritat testin vikasietotilaa.
- Automaattisesti, kun käytät julkiseen IP-osoite muodostaa virtuaalikoneen etätyöpöydän Azure-tietokannassa. Jos haluat tehdä tämän, varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.

Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.

### <a name="create-a-recovery-plan"></a>Suunnittele palauttaminen

1. Luo .xml-tiedostossa mallina palautus-palvelupaketin vaihtaminen ohjatulla alla tiedot ja tallenna se sitten "C:\RPTemplatePath.xml".
2. Muuta RecoveryPlan solmu tunnus, nimi, PrimaryServerId ja SecondaryServerId.
3. Voit muuttaa ProtectionEntity solmu PrimaryProtectionEntityId (vmid-VMM).
4. Voit lisätä useita VMs lisäämällä useita ProtectionEntity solmujen.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Täytä tiedot mallissa:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Luo RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

1.  Hae RecoveryPlan objektin suorittamalla seuraavan komennon:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Aloita testi vikasietotilaa suorittamalla seuraavan komennon:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Näytön toiminta

Seuraavat komennot avulla voit valvoa. Huomaa, että voit joutua odottamaan välissä työt Viimeistele käsittelyä varten.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Seuraavat vaiheet

[Lue lisää](https://msdn.microsoft.com/library/dn850420.aspx) Azure sivuston palauttaminen PowerShell cmdlet-komennot. </a>.
