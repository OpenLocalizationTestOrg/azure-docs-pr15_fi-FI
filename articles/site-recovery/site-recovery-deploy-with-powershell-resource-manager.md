<properties
    pageTitle="Suojaa Azure-palvelinten Azure PowerShellin Azure resurssien hallinnan avulla | Microsoft Azure"
    description="Automatisoida kanssa Azure palauttaminen Azure-palvelinten suojauksen käyttämällä PowerShell ja Azure Resurssienhallinta."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Toistaa paikallisen Hyper-V näennäiskoneiden ja Azure välillä käyttämällä PowerShell ja Azure resurssien hallinta

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - resurssien hallinta](site-recovery-deploy-with-powershell-resource-manager.md)
- [Perinteinen Portal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Yleiskatsaus

Azure palauttaminen prosentuaalista osuutta yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen määrittäminen mukaan orchestrating replikoinnin ja palauttamisen käyttöönottoskenaariot useilla näennäiskoneiden automaattisesti. Käyttöönottoskenaariot täydellinen luettelo on artikkelissa [Azure palauttaminen yleiskatsaus](site-recovery-overview.md).

Azure PowerShell on moduuli, joka sisältää cmdlet-komentojen Azure Windows PowerShellin kautta. Voit käsitellä kahdenlaisia moduuleja: Azure profiili-moduuli tai Azure Resurssienhallinta-moduuli.

Sivuston palauttaminen PowerShell cmdlet-PowerShellin Azure for Azure resurssien hallinta auttaa suojaamaan ja palauttamaan palvelinten Azure-tietokannassa.

Tässä artikkelissa käsitellään palauttaminen ja orchestrate server Azure suojauksen ottaminen käyttöön ja Azure resurssien hallinta Windows PowerShellin avulla. Tässä artikkelissa käytetyt esimerkissä esitetään, kuinka suojaaminen, epäonnistua päälle ja näennäiskoneiden Hyper-V isännän Azure, voit palauttaa Azure PowerShellin Azure resurssien hallinnan avulla.

> [AZURE.NOTE] Sivuston palauttaminen PowerShell cmdlet-komentojen avulla voit määrittää seuraavat tällä hetkellä: virtuaalikoneen hallinnan sivusto toiseen, Azure virtuaalikoneen hallinnan sivuston ja Azure Hyper-V sivusto.

Sinun ei tarvitse olla asiantuntija PowerShell käyttämään tämän artikkelin, mutta sinun on tiedettävä peruskäsitteet, kuten moduulit, cmdlet-komennot ja istuntoja. Saat lisätietoja Windows PowerShell- [Käytön aloittaminen Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Voit myös lukea lisää tietoja [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Cloud ratkaisu Provider (CSP)-ohjelman Microsoftin yhteistyökumppanien voit määrittäminen ja hallinta suojauksen asiakkaidensa palvelinten asiakkaidensa vastaaviin CSP tilauksia (vuokraajan tilaukset).

## <a name="before-you-start"></a>Ennen aloittamista

Varmista, että seuraavat edellytykset on paikassa:

- [Microsoft Azure](https://azure.microsoft.com/) -tili. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/). Lisäksi on lisätietoja [Azure palautus Sivustonhallinta hinnoittelua](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Lisätietoja tässä versiossa ja asennusohjeet on [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) ja [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduulit. Saat uusimmat versiot nämä moduulit [PowerShell-valikoima](https://www.powershellgallery.com/)

Tässä artikkelissa kuvataan, miten voit käyttää PowerShellin Azure Azure resurssien hallinta voit määrittää ja hallita suojaus-palvelimiin. Tässä artikkelissa käytetyt esimerkissä esitetään, kuinka suojaaminen virtual machine Hyper-V-isännän Azure-käyttöjärjestelmässä. Tässä esimerkissä ovat seuraavat edellytykset. Monipuolisemman joukko eri palauttaminen skenaarioita koskevat vaatimukset lue ohjeet koskevat skenaarion.

- Windows Server 2012 R2-tai Microsoft Hyper-V Server 2012 R2 joka sisältää vähintään yhden näennäiskoneiden Hyper-V isäntä.
- Hyper-V palvelinten yhteydessä Internetiin, joko suoraan tai välityspalvelimen kautta.
- Suojattava näennäiskoneiden on mukaisia [virtuaalikoneen edellytykset](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Vaihe 1: Kirjautuminen sisään Azure-tili


1. Avaa PowerShell-konsolin ja suorita tämä komento kirjautumaan Azure-tili. Cmdlet näyttää verkkosivulle, joka pyytää tilin tunnistetiedot.

        Login-AzureRmAccount

    Vaihtoehtoisesti voit myös lisätä tilin tunnistetiedot parametrina `Login-AzureRmAccount` cmdlet-komennon avulla `-Credential` parametria.

    Jos olet CSP kumppanin toimi palvelutili puolesta, Määritä asiakkaan vuokraajan, käyttämällä niiden tenantID tai vuokraajan toimialuenimi.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Tilin voi olla useita tilauksia, joten tulee Liitä haluat käyttää sen tilin tilaus.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Varmista, että tilauksesi on rekisteröity käyttämään Azure tarjoajan palautus-palveluiden ja sivuston palauttaminen käyttämällä seuraavia komentoja:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    Nämä komennot tulosteeseen, jos **RegistrationState** on asetettu **rekisteröity**, voit siirtyä vaiheeseen 2. Jos et rekisteröi puuttuu toimittaja-tilaukseesi.

    Voit rekisteröidä Azure palveluntarjoajan sivuston palauttaminen ja palautus-palveluja, suorita seuraavat komennot:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Varmista, että tarjoajien rekisteröity onnistuneesti käyttämällä seuraavia komentoja: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` ja `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Vaihe 2: Määritä palautus palvelut-säilö

1. Azure Resurssienhallinta resurssiryhmä, jossa luot säilö tai resurssi ryhmän luominen Voit luoda uusi resurssiryhmä käyttämällä seuraava komento:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    Jos $ResourceGroupName muuttujan sisältää luotavan resurssiryhmän nimi ja $Geo muuttujan Azure alue, johon haluat luoda resurssiryhmä (esimerkiksi "Brasilia Etelä").

    Voit hankkia resurssiryhmien luettelo tilaukseesi avulla `Get-AzureRmResourceGroup` cmdlet-komento.

2. Luo uusi Azure palautus palvelut-säilö seuraavasti:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Voit hakea aiemmin vaults luettelo `Get-AzureRmRecoveryServicesVault` cmdlet-komento.

> [AZURE.NOTE] Jos haluat suorittaa toimintoja palauttaminen vaults luotu perinteinen portaalin tai Azure Service Management PowerShell-moduulin, voit hakea tällaisten vaults luettelo käyttämällä `Get-AzureRmSiteRecoveryVault` cmdlet-komento. Sinun on luotava uusi palautus palvelut-säilö, kaikki uudet toiminnot. Aiemmin luomasi sivuston palauttaminen vaults ovat tuettuja, mutta se ei ole uusimpia ominaisuuksia.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Vaihe 3: Määritä palautus Services säilö yhteydessä

1.  Määritä säilö yhteydessä suorittamalla seuraavan komennon:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Vaihe 4: Hyper-V sivuston luominen ja luo uusi sivuston säilö rekisteröinti avain.

1. Luo uuden Hyper-V sivuston seuraavasti:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Tämä cmdlet-komento aloittaa sivuston palauttaminen työn luotavan sivuston ja palauttaa objektin palauttaminen työn. Odota, suorittaa ja varmista, että projektin työn onnistui.

    Voit hakea työn ja tarkista siten projektin nykyisen tilan Get-AzureRmSiteRecoveryJob cmdlet-komennolla.
2. Luo ja lataa rekisteröinti näppäintä-sivustossa seuraavasti:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopioi ladattu avain Hyper-V isäntään. Tarvitset rekisteröi sivuston isäntää Hyper-V-näppäintä.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Vaihe 5: Asenna Azure palauttaminen tarjoaja ja Azure palautus Services agentti Hyper-V isännän

1. Lataa [Microsoft](https://aka.ms/downloaddra)installer-palvelun uusimman version.

2. Suorita asennusohjelma Hyper-V isännän ja asennuksen lopussa jatka vaiheeseen rekisteröinti.

3. Anna kehotettaessa ladattu sivustosta rekisteröinti-näppäintä painettuna ja valmis rekisteröinti Hyper-V isäntä-sivustoon.

4. Varmista, että Hyper-V isäntä on rekisteröity sivustoon käyttämällä seuraava komento:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Vaihe 6: Replikoinnin-käytännön luominen ja liittää sen suojaus-säilö

1. Replikoinnin-käytännön luominen seuraavasti:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Tarkista palautetut työn varmistaa, että replikoinnin-käytännön luominen onnistuu.

    >[AZURE.IMPORTANT] Määritetyn tallennustilan tilin on oltava sama Azure alue kuin palautus palvelut-säilö ja pitäisi olla geo replikoinnin käytössä.
    >
    > - Jos määritetyn palautus-tallennustilan tilin tyyppi Azuren tallennustilaan (perinteinen), suojattu koneet automaattisesti palauttaa Azure IaaS (perinteinen) tietokone.
    > - Jos tallennustilan tilin tyyppi Azuren tallennustilaan (Azure resurssien hallinta)-suojatun koneet automaattisesti on määritetty palautus Palauta tietokone Azure IaaS (Azure resurssien hallinta).

2. Hae suojaus säilö vastaavat-sivustossa seuraavasti:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Suojaus-säilö liitos aloittaminen replikoinnin-käytännön seuraavasti:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Odota liitos työn suorittamiseen ja varmista, että se täyttää onnistuneesti.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Vaihe 7: Ota suojaus näennäiskoneiden

1. Hae suojaus-kohteen vastaavat AM, jonka haluat suojata seuraavasti:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Aloita suojaaminen virtuaalikoneen, seuraavasti:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Määritetyn tallennustilan tilin on oltava sama Azure alue kuin palautus palvelut-säilö ja pitäisi olla geo replikoinnin käytössä.
    >
    > - Jos määritetyn palautus-tallennustilan tilin tyyppi Azuren tallennustilaan (perinteinen), suojattu koneet automaattisesti palauttaa Azure IaaS (perinteinen) tietokone.
    > - Jos tallennustilan tilin tyyppi Azuren tallennustilaan (Azure resurssien hallinta)-suojatun koneet automaattisesti on määritetty palautus Palauta tietokone Azure IaaS (Azure resurssien hallinta).

    > Jos suojaat AM on liitetty useita levyn, Määritä *OSDiskName* parametrilla käyttöjärjestelmän levy.

3. Odota näennäiskoneiden saavuttamiseksi jälkeen ensimmäisen replikoinnin suojattu tila. Tämä voi kestää jonkin aikaa, eri tekijöistä, kuten tietomäärän replikoida ja Azure, seuraavat kaistanleveyttä. Projektin tilasta ja StateDescription päivitetään yhteydessä kurottavat suojattu tila AM seuraavasti.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Päivitä palautus-ominaisuuksia, kuten AM roolin kokoa ja Azure verkon liitettävä virtual machine verkon käyttöliittymän kortteja yhteydessä automaattisesti.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Vaihe 8: Suorita testi automaattisesti

1. Suorita testi automaattisesti, työn seuraavasti:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Varmista, että testi AM luodaan Azure-tietokannassa. (Testi automaattisesti työn keskeytetään, kun olet luonut testi AM Azure-tietokannassa. Työn suorittaa Siivotaan luodut artefacts yhteydessä jatkaminen työ, kuten seuraavassa vaiheessa.)

3. Suorita testi, vikasietotilaa seuraavasti:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Seuraavat vaiheet

[Lue lisää](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure sivuston hyödyntämistä Azure Resurssienhallinta PowerShellin cmdlet-komennot.
