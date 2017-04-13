<properties
    pageTitle="Replikoida Hyper-V näennäiskoneiden-VMM paveikslėlis toissijainen VMM sivuston PowerShellin (Resurssienhallinta) | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure palauttaminen, orchestrate replikoinnin, automaattisesti ja toissijainen VMM sivustoon PowerShellin (resurssien hallinta)-VMM paveikslėlis VMs Hyper-V palauttamisen käyttöönotto"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Rinnakkaiset Hyper-V näennäiskoneiden VMM paveikslėlis toissijainen VMM-sivustoon (Resurssienhallinta) PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-vmm.md)
- [Perinteinen Portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Tervetuloa käyttämään Azure palauttaminen! Tässä artikkelissa, jos haluat toistaa paikallisen Hyper-V näennäiskoneiden hallitaan System Center Virtual Machine Manager (VMM) paveikslėlis toissijaisen sivuston käyttö 

Tämän artikkelin avulla voit automatisoida yleisiä tehtäviä, sinun täytyy suorittaa, kun määrität Azure palauttaminen replikointia Hyper-V näennäiskoneiden-System Center VMM paveikslėlis System Center VMM paveikslėlis toissijaisen sivuston PowerShellin avulla.

Artikkelin sisältää skenaarion edellytyksistä ja näytetään 

- Voit määrittää palautus palvelut-säilö
- Azure sivuston palauttaminen Providerin asentaminen VMM lähdepalvelin ja kohde VMM palvelimessa
- Rekisteröi säilö VMM-palvelimesta
- Määrittäminen VMM pilven replikointikäytännön. Käytännön Replikointiasetukset käytetään kaikkien suojatun näennäiskoneiden 
- Ota suojaus näennäiskoneiden käyttöön. 
- Testaa VMs vikasietotilaa erikseen tai osana palautus suunnitelma, varmista, että kaikki toimii odotetulla tavalla.
- Suorittaa suunnitellun tai suunnittelematon automaattisesti, VMs erikseen tai osana palautus suunnitelma, varmista, että kaikki toimii odotetulla tavalla.

Jos kohtaat ongelmia määrittämisestä artikkelista, lähettää kysymyksiä [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure on kaksi eri [käyttöönoton mallien](../resource-manager-deployment-model.md) luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönoton mallia ja Azure-portaalin tukeen käyttöönoton sekä malleille. Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli.



## <a name="on-premises-prerequisites"></a>Paikallisen edellytykset

Näin käytön edellytykset ensisijainen ja toissijainen paikallinen sivustoissa käyttöön tässä skenaariossa:

**Edellytykset** | **Tiedot** 
--- | ---
**VMM** | Suosittelemme, että otat VMM-palvelimen ensisijainen sivuston ja VMM toissijainen-sivustossa.<br/><br/> Voit myös [replikoida paveikslėlis yhteen VMM palvelimeen välillä](site-recovery-single-vmm.md). Tällöin sinun on vähintään kaksi paveikslėlis määritettynä VMM-palvelimessa.<br/><br/> VMM palvelimet on oltava käynnissä vähintään System Center 2012 SP1 ja viimeisimmät päivitykset.<br/><br/> Kunkin VMM palvelimen on oltava yhdellä tai Lisää paveikslėlis määritetty ja kaikki paveikslėlis on oltava Määritä Hyper-V kapasiteetin profiili. <br/><br/>Paveikslėlis on sisällettävä vähintään yksi VMM host ryhmät.<br/><br/>Lisätietoja VMM paveikslėlis [määrittäminen VMM cloud kangasta](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), määrittäminen ja [vaiheittaisissa ohjeissa: luominen yksityinen paveikslėlis System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM palvelimet on internet-yhteyttä. 
**Hyper-V** | Hyper-V-palvelimet on oltava vähintään Windows Server 2012 Hyper-V-roolin kanssa, muttet asentanut uusimmat päivitykset.<br/><br/> Hyper-V server on sisällettävä vähintään yksi VMs.<br/><br/>  Host (isäntä)-ryhmien ensisijainen ja toissijainen VMM paveikslėlis pitäisi sijaita Hyper-V host-palvelimissa.<br/><br/> Jos käytössäsi on Hyper-V Windows Server 2012 R2-klusterin asentamista, [Päivitä 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Jos käytössäsi on Hyper-V-klusterin Windows Server 2012 muistiinpanon kyseisen klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun on määrittää klusterin broker manuaalisesti. [Lue lisää](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Toimittaja** | Sivuston palauttaminen käyttöönoton aikana Azure sivuston palauttaminen tarjoajan asentaminen VMM-palvelimiin. Palvelun yhteydessä palauttaminen HTTPS 443, orchestrate replikoinnin päälle. Tietoja Replikointi tapahtuu Lähiverkon tai VPN-yhteyden kautta ensisijainen ja toissijainen Hyper-V-palvelinten välillä.<br/><br/> VMM palvelimessa tarjoajan tarvitseville näistä URL-osoitteista: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Lisäksi Salli palomuurin tietoliikenne palvelimiin VMM [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja Salli HTTPS (443)-protokollaa.

### <a name="network-mapping-prerequisites"></a>Verkon määritys edellytykset
Verkon määritys kartat VMM AM verkkojen ensisijainen ja toissijainen VMM palvelimissa välillä:

- Aseta replikan VMs optimaalisesti toissijainen Hyper-V isännät jälkeen automaattisesti.
- Yhdistä replikan VMs tarvittavat AM verkkoihin.
- Jos et määritä verkon määritys replikan VMs ei liitettävä minkä tahansa verkkoon jälkeen automaattisesti.
- Jos haluat määrittää verkon yhdistäminen sivuston palauttamisen aikana käyttöönoton tähän on käytön edellytykset:

    - Varmista, että VMM AM verkon yhteydessä VMs lähde Hyper-V Host (isäntä)-palvelimeen. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
    - Varmista, että toissijainen pilveen aiot käyttää palauttamista varten on määritetty vastaavan AM verkkoon. Että AM verkon linkitetty looginen verkko, joka liittyy toissijainen pilveen.


Lisätietoja määrittäminen VMM verkot artikkeleissa alapuolella

- [Miten määritetään loogisen verkkojen VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Miten määritetään AM verkot ja yhdyskäytävät VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Lisätietoja](site-recovery-network-mapping.md) verkon määritys toiminnasta.

###<a name="powershell-prerequisites"></a>PowerShellin edellytykset
Varmista, että sinulla on valmis lähetettäväksi PowerShellin Azure. Jos käytössäsi on jo PowerShell, tarvitset päivittäminen versioksi 0.8.10 tai uudempi versio. Lisätietoja PowerShell on [asennetaan ja määritetään PowerShellin Azure opas](../powershell-install-configure.md). Kun olet määrittänyt ja määritetty PowerShell, voit tarkastella kaikkia käytettävissä cmdlet-komennot-palvelun [tähän](https://msdn.microsoft.com/library/dn850420.aspx). 

Lisätietoja vihjeitä, joiden avulla voit käyttää Cmdlet-komentoja, kuten miten parametriarvot syötteiden ja tulostaa yleensä käsitellään PowerShellin Azure on [opas Azure cmdlet-komennot, joiden avulla pääset alkuun](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Vaihe 1: Määritä tilaus 

1. FROM Azure PowerShellin Azure-tiliisi kirjautuminen: käyttämällä seuraavat cmdlet-komennot
 
        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred 
    

2. Saat luettelon käytettävissä tilauksistasi. Tämä on myös luettelo subscriptionIDs kunkin tilauksista. Huomautus alaspäin tilaus, johon haluat luoda palautus subscriptionID services säilöön    

        Get-AzureRmSubscription 

3. Määritä tilauksen, joiden palautus services säilö on luomat mainitseminen tilauksen tunnus

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Vaihe 2: Luo palautus palvelut-säilö 

1. Luo Azure Resurssienhallinta-resurssiryhmä, jos sitä ei vielä ole

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Luo uusi palautus palvelut-säilö ja Tallenna luotu ASR säilöön-objekti muuttujaan (käytetään jäljempänä). Voit myös hakea ASR säilö objektin viestin luominen Get-AzureRMRecoveryServicesVault cmdlet-komennolla:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Vaihe 3: Määritä palautus Services säilö yhteydessä

1.  Jos sinulla jo luotu säilöön, suorita komento, josta haluat säilö alapuolella.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Määritä säilö yhteydessä suorittamalla-komennon alla.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## <a name="step-4-install-the-azure-site-recovery-provider"></a>Vaihe 4: Azure sivuston palauttaminen Providerin asentaminen

1.  VMM tietokoneessa Luo kansio suorittamalla seuraavan komennon:
    
        New-Item c:\ASR -type directory
        
2.  Pura ladattu-palvelun avulla suorittamalla seuraavan komennon tiedostot
    
        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q

    
3.  Asenna palveluntarjoajaan käyttämällä seuraavia komentoja:
    
        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Odota, viimeistele asennus.
    
4.  Rekisteröi palvelimen säilöön käyttämällä seuraava komento:
    
        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-and-associate-a-replication-policy"></a>Vaihe 5: Luo ja liitä replikoinnin-käytännön luominen

1.  Luo Hyper-V 2012 R2 replikoinnin käytäntö suorittamalla seuraavan komennon:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] VMM pilveen voi sisältää Hyper-V isännät eri versioiden Windows Server (kuten mainitaan edellytykset Hyper-V), mutta replikoinnin käytäntö on tiettyjä käyttöjärjestelmäversio. Jos sinulla on käytössä eri käyttöjärjestelmien eri isännät, luo erilliset replikoinnin käytännöt mistäkin käyttöjärjestelmäversio. Esimerkiksi: Jos sinulla on käynnissä Windows palvelinten 2012: ssa ja Windows Server 2012 R2 kolme viisi isännät, luo kaksi replikoinnin käytännöt – yksi mistäkin käyttöjärjestelmässä versioita.

2.  Hanki säilö ensisijainen protection (ensisijainen VMM Cloud) ja palautus suojaus säilö (palautus VMM Cloud) suorittamalla seuraavat komennot:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Hakea käytäntöä, jonka loit vaiheessa 1 kutsumanimi käytännön avulla

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Aloittaa suojaus säilö (VMM Cloud) suhteen replikoinnin käytäntö:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Odota käytännön liitos työn suorittamiseen. Voit tarkistaa, jos työ on valmis käyttämällä seuraavia PowerShell-koodikatkelman.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Kun työ on suoritettu käsittely, suorita seuraava komento:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.

## <a name="step-5-configure-network-mapping"></a>Vaihe 5: Määritä verkon määritys

1. Ensimmäisen komennon noutaa palvelinten nykyisen Azure palauttaminen säilö. Komento tallentaa $Servers matriisin muuttujan Microsoft Azure sivuston palauttaminen-palvelimiin.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Komentojen saat sivuston palauttaminen verkon VMM lähdepalvelin ja VMM palvelimesta.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] VMM lähdepalvelimeen voi olla ensimmäinen tai toinen palvelinten matriisista. Tarkista VMM-palvelimien nimet ja tutustu verkkojen oikein


4. Lopullinen cmdlet-komento luo yhdistämismäärityksen ensisijaisen verkon ja palautus verkon välillä. Cmdlet määrittää ensisijaisen verkon $PrimaryNetworks ja $RecoveryNetworks ensimmäisenä elementtinä palautus-verkon ensimmäinen elementti.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Vaihe 6: Määritä tallennustilan yhdistäminen

1. -Komennon alla saa tallennustilan luokitusten tuominen $storageclassifications muuttujaa luettelo.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Komentojen Hae yhdeksi $SourceClassificaion muuttujan ja kohteen luokittelu yhdeksi $TargetClassification muuttujan lähde-luokitus. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Lähde- ja luokitusten voi olla mikä tahansa matriisin osa. Tulosteen viitata komennon kuva lähde- ja luokitukset $storageclassifications matriisista indeksin alapuolella. 
    
    > Hae AzureRmSiteRecoveryStorageClassification | Valitse-objekti - ominaisuuden FriendlyName, tunnus | Taulukon muotoileminen


3. Cmdlet-komennon alla Luo yhdistämismäärityksen lähde-luokittelu ja kohteen luokittelu välillä. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Vaihe 7: Ota suojaus näennäiskoneiden

Kun palvelimissa, paveikslėlis ja verkkoja on määritetty oikein, voit ottaa suojauksen näennäiskoneiden pilveen. 


  1. Ota suojaus käyttöön suorittamalla seuraavan komennon saat suojaus-säilö:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Hae suojaus-kohteen (AM) suorittamalla seuraava komento:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Ota käyttöön AM replikointi suorittamalla seuraavan komennon:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testata käyttöönoton

Jos haluat testata käyttöönoton voit suorittaa testi-automaattisesti virtual yhdelle tietokoneelle, tai useita näennäiskoneiden koostuva palautus suunnitelman luominen ja Suorita testi-automaattisesti palvelupaketin. Testaa automaattisesti varmistaminen automaattisesti ja palautus-järjestelmä eristetyssä verkossa. 

> [AZURE.NOTE] Voit luoda palautus suunnitelma sovelluksen Azure-portaalissa.

Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.


### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

1.  Suorita cmdlet-komennot Hae AM verkkoon, johon haluat testata automaattisesti, että VMs alapuolella.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Suorita testi-automaattisesti AM, toimimalla seuraavasti:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Suorita testi-automaattisesti palautus-palvelupaketin toimimalla seuraavasti:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Suorita suunnitellun automaattisesti

1. Suorita suunnitellun automaattisesti AM, toimimalla seuraavasti:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Suorita suunnitellun automaattisesti palautus-palvelupaketin toimimalla seuraavasti:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Suorita suunnittelematon automaattisesti

1. Suorita suunnittelematon automaattisesti AM, toimimalla seuraavasti:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. suorittaa suunnittelematon automaattisesti palautus-palvelupaketin toimimalla seuraavasti:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

[Lue lisää](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure sivuston hyödyntämistä Azure Resurssienhallinta PowerShellin cmdlet-komennot.

