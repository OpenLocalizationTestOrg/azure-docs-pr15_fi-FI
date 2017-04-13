<properties
    pageTitle="Toistaa Hyper-V näennäiskoneiden VMM paveikslėlis Azure palauttaminen ja PowerShell (resurssien hallinta) | Microsoft Azure"
    description="Rinnakkaiset Hyper-V näennäiskoneiden VMM paveikslėlis Azure palauttaminen ja PowerShellin avulla"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Toistaa Hyper-V näennäiskoneiden VMM paveikslėlis-Azure PowerShell ja Azure resurssien hallinta

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resurssien hallinta](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Perinteinen Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell – perinteinen](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Yleiskatsaus

Azure palauttaminen osaltaan mukaan orchestrating replikoinnin, automaattisesti ja palauttamisen käyttöönottoskenaariot useilla näennäiskoneiden yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Luettelon kaikista käyttöönoton skenaariot artikkelissa [Azure palauttaminen yleiskatsaus](site-recovery-overview.md).

Tämän artikkelin avulla voit automatisoida yleisiä tehtäviä, sinun täytyy suorittaa, kun määrität Azure palauttaminen replikointia Hyper-V näennäiskoneiden-System Center VMM paveikslėlis Azure tallennustilan PowerShellin avulla.

Artikkelin sisältää skenaarion edellytyksistä ja näytetään

- Voit määrittää palautus palvelut-säilö
- Azure sivuston palauttaminen tarjoajan asentaminen VMM lähdepalvelimeen
- Rekisteröidä palvelimen säilö, Azure-tallennustilan tilin lisääminen
- Asenna Azure palautus palvelut-agentti Hyper-V host-palvelimissa
- Määritä VMM paveikslėlis, jota käytetään kaikissa suojatun näennäiskoneiden suojausasetuksia
- Ota käyttöön kyseiset näennäiskoneiden suojaus.
- Testaa korvaava, varmista, että kaikki toimii odotetulla tavalla.

Jos kohtaat ongelmia määrittämisestä artikkelista, lähettää kysymyksiä [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md). Tässä artikkelissa käsitellään käyttämällä resurssien hallinnan käyttöönottomalli.

## <a name="before-you-start"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset on paikassa:

### <a name="azure-prerequisites"></a>Azure edellytykset

- Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Jos sinulla ei ole, aloita [ilmainen tili](https://azure.microsoft.com/free). Lisäksi on lisätietoja [Azure palautus Sivustonhallinta hinnat](https://azure.microsoft.com/pricing/details/site-recovery/).
- Tarvitset CSP tilauksen, jos haluat kokeilla replikoinnin CSP tilaus-tilanne. Lisätietoja CSP-ohjelmassa [Voit rekisteröidä CSP-ohjelmassa](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Tarvitset Azure v2 (resurssien hallinta)-tallennustilan tilin replikoida Azure tietojen tallentamiseen. Tili on geo replikoinnin käytössä. Se olisi sama alueelle Azure palauttaminen-palvelu ja yhdistetä saman tilauksen tai CSP-tilauksen. Lisätietoja Azure tallennustilan määrittämisestä on artikkelissa [Johdanto Microsoft Azuren tallennustilaan](../storage/storage-introduction.md) käyttöä.
- Tarvitset, varmista, että haluat suojata näennäiskoneiden noudattavat [Azure virtuaalikoneen edellytykset](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] Vain AM toiminnot ovat tällä hetkellä mahdollista PowerShellin kautta. Suunnitelman tason toimiin tuki tehdään pian.  Nyt voit on rajoitettu suorittamiseen fail-objektin päällä-tapahtumat vain "suojattu AM" rakeisuuden ja ei tasolla palautus suunnitteleminen.

### <a name="vmm-prerequisites"></a>VMM edellytykset
- Tarvitset VMM System Center 2012 R2-palvelimeen.
- Mikä tahansa VMM palvelimeen, jossa haluat suojata näennäiskoneiden on oltava käynnissä Azure sivuston palauttaminen tarjoaja. Tämä on asennettu Azure sivuston palautus-käyttöönoton yhteydessä.
- Tarvitset vähintään yksi cloud suojattava VMM palvelimessa. Pilveen tulisi sisältää:
    - Yksi tai useampi VMM host ryhmä.
    - Hyper-V host-palvelimissa tai klustereiden host kunkin ryhmän.
    - Yhden tai useamman näennäiskoneiden lähteen Hyper-V-palvelimeen.
- Lisätietoja VMM paveikslėlis määrittäminen:
    - Lue lisätietoja yksityinen VMM paveikslėlis [uusiin yksityinen-pilvi ja System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) ja [VMM 2012 ja paveikslėlis](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Lisätietoja [VMM cloud kangasta määrittäminen](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Kun cloud kangasta-osat ovat paikallaan perustietoja luomisesta yksityinen paveikslėlis luoda [Yksityinen pilvestä VMM-](http://go.microsoft.com/fwlink/p/?LinkId=324953) ja [vaiheittaisissa ohjeissa: luominen yksityinen paveikslėlis System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Hyper-V edellytykset

- Hyper-V host-palvelimissa on oltava vähintään **Windows Server 2012** Hyper-V-roolin tai **Microsoft Hyper-V Server 2012: n** kanssa, muttet asentanut uusimmat päivitykset.
- Jos käytössäsi on Hyper-V-klusterin muistiinpanon kyseisen klusterin broker ei ole luodaan automaattisesti, jos sinulla on staattinen IP osoite-pohjainen klusterin. Sinun on määrittää klusterin broker manuaalisesti. Varten
- Ohjeet [Määritä Hyper-V replikan Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx)näin.
- Hyper-V host palvelin- tai klusterin, jonka haluat hallita suojaus täytyy sisältyä VMM pilvestä.

### <a name="network-mapping-prerequisites"></a>Verkon määritys edellytykset
Kun suojaat näennäiskoneiden Azure-tietokannassa, verkon määritys kartat virtuaalikoneen verkot lähde VMM palvelimessa ja kohdeluettelo Azure verkkojen käyttöön seuraavasti:

- Kaikkien tietokoneissa, joissa epäonnistuvat saman verkon kautta voit muodostaa yhteyden toisistaan riippumatta palautus palvelupaketin ne sijaitsevat.
- Jos yhdyskäytävien on aikataulussa Azure verkon määritys, näennäiskoneiden muodostaa paikallisen muiden näennäiskoneiden.
- Jos verkon määritys ei määritetä, näennäiskoneiden, jotka eivät läpäise päälle saman palautus-suunnitelmassa voi liittää toisiinsa jälkeen korvaava, Azure.

Jos haluat ottaa käyttöön verkon määritys sinun on seuraavasti:

- Tietolähteen VMM palvelimessa suojattava näennäiskoneiden tulee olla yhdistetty AM verkkoon. Verkon olisi voidaan linkittää looginen verkko, joka on liitetty pilveen.
- Azure verkko, johon replikoitua näennäiskoneiden muodostaa korvaava jälkeen. Voit valita verkoston korvaava aikaan. Verkossa on oltava sama alue kuin Azure palauttaminen tilauksen.

Lisätietoja verkko-määritys

- [Miten määritetään loogisen verkkojen VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Miten määritetään AM verkot ja yhdyskäytävät VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Kuinka voit määrittää ja seurata Azure virtual verkot](https://azure.microsoft.com/documentation/services/virtual-network/)


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

1. Luoda resurssiryhmän Azure Resurssienhallinta, jos sitä ei vielä ole

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Luo uusi palautus palvelut-säilö ja Tallenna luotu ASR säilöön-objekti muuttujaan (käytetään jäljempänä). Voit myös hakea ASR säilö objektin viestin luominen Get-AzureRMRecoveryServicesVault cmdlet-komennolla:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Vaihe 3: Määritä palautus Services säilö yhteydessä

1.  Määritä säilö yhteydessä suorittamalla-komennon alla.

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


## <a name="step-5-create-an-azure-storage-account"></a>Vaihe 5: Azure-tallennustilan tilin luominen

1. Jos sinulla ei ole Azure-tallennustilan tilin, Luo geo replikoinnin käytössä-tiliä saman geo kuin säilö suorittamalla seuraavan komennon:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Huomaa, että tallennustilan tilin on oltava samalla alueella Azure palauttaminen-palvelu ja samaan tilaukseen.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Vaihe 6: Asenna Azure palautus palvelut-agentti

1. Azure palautus palvelut-agentti [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) , lataa ja asenna se sijaitsee VMM paveikslėlis, jonka haluat suojata kunkin Hyper-V host palvelimen.

2. Valitse kaikki VMM isännät varten suorittamalla seuraavan komennon:

    marsagentinstaller.exe/q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Vaihe 7: Määritä cloud suojausasetuksia

1.  Luo Azure replikoinnin käytännön suorittamalla seuraavan komennon:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Hanki suojauksen säilö suorittamalla seuraavat komennot:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Käytännön tiedot muuttujan käyttämällä työ, joka on luotu ja mainitseminen helpossa muodossa käytännön nimi:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Aloittaa suojaus-säilö liitos replikoinnin käytäntö:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Kun työ on suoritettu, suorita seuraava komento:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Kun työ on suoritettu käsittely, suorita seuraava komento:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.

## <a name="step-8-configure-network-mapping"></a>Vaihe 8: Määrittäminen verkon määritys

Ennen kuin aloitat verkon määritys Varmista, että näennäiskoneiden lähteen VMM palvelimessa yhteydessä verkkoon AM. Lisäksi voit luoda yhden tai useamman Azure virtual verkot.

Lisätietoja luomisesta virtual verkon [virtual verkon Azure Resurssienhallinta ja PowerShell sivusto sivusto VPN-yhteyden luominen](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) Azure Resurssienhallinta ja PowerShellin käyttäminen

Huomaa, että useiden virtuaalikoneen verkostojen voidaan määrittää yhden Azure verkoston. Jos jokin näistä aliverkosta on sama nimi kuin aliverkon, jossa lähde virtuaalikoneen on kohde verkossa on useita aliverkosta, valitse replikan virtuaalikoneen yhdistetään kyseisen kohteen aliverkon korvaava jälkeen. Jos ei ole kohteen aliverkon vastaavaa nimeä, virtuaalikoneen yhdistetään ensimmäisen aliverkon verkossa.

1. Ensimmäisen komennon noutaa palvelinten nykyisen Azure palauttaminen säilö. Komento tallentaa $Servers matriisin muuttujan Microsoft Azure sivuston palauttaminen-palvelimiin.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Toinen komento saa sivuston palauttaminen verkon ensimmäisen palvelimen $Servers matriisista. Komento tallentaa verkkojen $Networks muuttuja.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Kolmas komento saa $AzureVmNetworks muuttujan Azure virtual verkkojen ja sitten arvo.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Lopullinen cmdlet-komento luo yhdistämismäärityksen ensisijaisen verkon ja Azure virtuaalikoneen verkon välillä. Cmdlet määrittää ensisijaisen verkon $Networks ensimmäinen elementti. Cmdlet määrittää virtuaalikoneen verkon $AzureVmNetworks ensimmäinen elementti.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Vaihe 9: Käyttöönotto näennäiskoneiden suojaus

Kun palvelimissa, paveikslėlis ja verkkoja on määritetty oikein, voit ottaa suojauksen näennäiskoneiden pilveen.

 Ota seuraavat seikat huomioon:

 - Näennäiskoneiden on täytettävä Azure vaatimukset. Tarkista nämä [edellytykset](site-recovery-best-practices.md) ja tuki suunnitteluoppaan.

 - Ottaa käyttöön suojauksen, käyttöjärjestelmä ja käyttöjärjestelmän levyn ominaisuudet on määritettävä virtuaalikoneen. Kun luot virtual machine VMM virtuaalikoneen mallin avulla voit määrittää ominaisuus. Voit myös määrittää nämä ominaisuudet aiemmin näennäiskoneiden virtuaalikoneen ominaisuudet **Yleiset** ja **Laitekokoonpanon** -välilehdissä. Jos et määritä nämä ominaisuudet-VMM voit voi määrittää ne Azure sivuston palautus-portaalissa.


  1. Ota suojaus käyttöön suorittamalla seuraavan komennon saat suojaus-säilö:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Hae suojaus-kohteen (AM) suorittamalla seuraava komento:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Ota käyttöön DR AM varten suorittamalla seuraavan komennon:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Testata käyttöönoton

Jos haluat testata käyttöönoton voit suorittaa Testaa korvaava virtual yhdelle tietokoneelle, tai useita näennäiskoneiden koostuva palautus suunnitelman luominen ja suorittaminen Testaa korvaava palvelupaketin. Testaa korvaava varmistaminen korvaava ja palautus-järjestelmä eristetyssä verkossa. Huomaa, että:

- Jos haluat muodostaa yhteyden virtuaalikoneen etätyöpöydän jälkeen korvaava Azure-tietokannassa, ottaa käyttöön virtuaalikoneen Etätyöpöytäyhteys, ennen kuin suoritat testin vikasietotilaa.
- Korvaava, kun käytät julkiseen IP-osoite muodostaa virtuaalikoneen etätyöpöydän Azure-tietokannassa. Jos haluat tehdä tämän, varmista toimialuekäytäntöjä, jotka estävät yhteyden julkisen osoitteen virtual tietokoneeseen ei ole.

Voit tarkistaa toiminnon loppuun noudattamalla [Näytön](#monitor)toimintaa.


### <a name="run-a-test-failover"></a>Suorita testi automaattisesti

1.  Aloita testi vikasietotilaa suorittamalla seuraavan komennon:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Suorita suunnitellun automaattisesti

1. Käynnistä suunnitellun vikasietotilaa suorittamalla seuraavan komennon:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Suorita suunnittelematon automaattisesti

1. Käynnistä suunnittelematon vikasietotilaa suorittamalla seuraavan komennon:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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
