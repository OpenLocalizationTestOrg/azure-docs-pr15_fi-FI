<properties
    pageTitle="Poista palvelimia ja poistaminen käytöstä suojaus | Microsoft Azure" 
    description="Tässä artikkelissa kerrotaan, miten unregister palvelinten palauttaminen säilöstä ja suojauksen näennäiskoneiden ja fyysiset palvelimet käytöstä." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Poista palvelimia ja suojauksen poistaminen käytöstä

Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan poistamisesta unregister palvelinten palauttaminen säilöstä ja suojattu palauttaminen näennäiskoneiden suojauksen poistaminen käytöstä. 

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-vmm-server"></a>Unregister VMM-palvelin

Poistamalla server Azure palauttaminen portaalissa **palvelimet** -välilehden unregister VMM palvelin säilöstä. Huomaa, että:

-  **Yhdistetty VMM palvelin**: suosittelemme unregister VMM palvelimeen, kun se on yhdistetty Azure. Näin varmistat, että paikallinen VMM palvelimessa ja siihen (VMM palvelimissa, jotka sisältävät paveikslėlis, jotka on yhdistetty paveikslėlis palvelimessa, jonka haluat poistaa) VMM palvelimet asetukset leikkauskohdat Siivotaan oikein. Suosittelemme, että poistat yhteydettömät palvelimen vain, jos pysyvä ongelma yhteyttä.
- **Yhteydettömät VMM palvelin**: Jos VMM palvelin ei ole yhteyttä, kun poistat sen sinun on suorittamalla manuaalisesti tyhjennys suorittavan. Komentosarja on käytettävissä [Microsoft valikoima](http://aka.ms/asr-cleanup-script-vmm). Tarkista VMM palvelimen manuaalinen uudelleenjärjestäminen prosessin suorittamista varten.
- **VMM server-klusterin**: Jos tarvitset unregister VMM palvelin, jossa on otettu käyttöön klusterin seuraavasti:

    - Jos palvelin on yhdistetty, poista yhdistetty VMM palvelin, **palvelimet** -välilehden. Jos haluat poistaa palveluntarjoajan palvelimessa, kirjaudu sisään jokaisen klusterin solmun ja asennuksen poistaminen Ohjauspaneelissa. Viitattu edellisessä osassa poistettava rekisterimerkinnät klusterin kaikissa passiivinen solmuissa uudelleenjärjestäminen komentosarjan suorittaminen
    - Jos palvelin ei ole yhteyttä tarvitset suorittamaan uudelleenjärjestäminen-komentosarjan kaikissa klusterin solmuissa.

### <a name="unregister-an-unconnected-vmm-server"></a>Unregister yhteydettömät VMM-palvelin

VMM palvelimessa, jonka haluat poistaa:

1. Unregister VMM-server Azure-portaalista.
2. Lataa uudelleenjärjestäminen komentosarja VMM-palvelimeen.
3. Avaa PowerShell Suorita järjestelmänvalvojana-vaihtoehdosta, voit muuttaa suorittamisen käytäntö oletusalue (LocalMachine).
4. Noudata komentosarja. 

VMM palvelimissa, joiden paveikslėlis, joka on yhdistetty paveikslėlis on poistettu palvelimen kanssa:

1. Uudelleenjärjestäminen-komentosarjan suorittaminen ja Suorita vaiheet 2-4.
2. Määritä VMM tunnus VMM palvelimessa, jossa on poistettu. 
3. Tämä komentosarja poistaa rekisteröintitietoja VMM palvelimen ja cloud laiteparin tiedot.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Unregister Hyper-V palvelimen Hyper-V-sivustossa

Kun Azure palauttaminen otetaan käyttöön suojaaminen palvelimessa Hyper-V Hyper-V-sivuston (ja-palvelinta ei ole VMM) näennäiskoneiden säilöstä Hyper-V palvelimen voit unregister seuraavasti:

1. Poista suojaus näennäiskoneiden Hyper-V-palvelimessa.
2. Valitse Azure palauttaminen portaalin **palvelimet** -välilehden palvelimen > Poista. Palvelin ei ole yhteyttä Azure, kun teet tämän.
3. Suorita seuraava komentosarja uudelleenjärjestämisen asetukset palvelimessa ja unregister säilöstä. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Lopeta Hyper-V virtual-tietokoneen suojaaminen

Jos haluat estää suojaaminen Hyper-V virtual koneen tarvitset sitä suojauksen poistamista. Sen mukaan, miten voit poistaa suojauksen voit joutua muuttamaan suojausasetuksia manuaalisesti tietokoneessa Tyhjennä. 

### <a name="remove-protection"></a>Suojauksen poistaminen

1. Valitse Tunnistepilven ominaisuudet **näennäiskoneiden** -välilehdessä virtuaalikoneen > **Poista**.
2. **Vahvista virtuaalikoneen poisto** -sivulla on muutama asetukset:

    - **Suojauksen poistaminen käytöstä**, jos otat käyttöön ja Tallenna asetus virtuaalikoneen enää suojataan palauttaminen. Virtuaalikoneen suojausasetuksia oltava siivotaan automaattisesti.
    - **Poista säilö**– Jos valitset tämän vaihtoehdon, virtuaalikoneen poistetaan vain sivuston palauttaminen säilöstä. Paikallisen suojaus Epäsuotuisiin virtuaalikoneen ei vaikuta. Tarvitset tyhjentääksesi asetukset manuaalisesti ja poista suojausasetuksia ja poista virtuaalikoneen Azure tilauksesta, ja poista suojausasetuksia, sinun on tyhjentää niitä manuaalisesti alla olevien ohjeiden avulla.

Jos valitset Poista virtuaalikoneen ja sen kiintolevyillä, ne poistetaan kohdesijainnin.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Puhdista suojausasetuksia manuaalisesti (välillä VMM sivustot)

Jos valitsit **hallitseminen virtuaalikoneen lopetetaan**, puhdistaa asetukset manuaalisesti:

1. Ensisijaisen palvelimen suorittamalla tämän VMM konsolin tyhjentääksesi ensisijainen virtuaalikoneen asetuksia. Valitse VMM-konsolin PowerShell-painikkeen napsauttaminen avaa VMM PowerShell console. Korvaa SQLVM1 virtuaalikoneen nimi.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Toissijainen VMM palvelimeen suorittamalla tämän Puhdista toissijainen virtuaalikoneen asetuksia:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Toissijainen VMM-palvelimeen, kun olet suorittanut komentosarja päivittää näennäiskoneiden Hyper-V Host (isäntä)-palvelimeen niin, että toissijainen virtuaalikoneen saa tunnistaa uudelleen VMM konsolissa.
4. Edellä kuvatut toimet tyhjentää replikoinnin asetukset vain VMM palvelimeen. Jos haluat poistaa virtuaalikoneen toistoa varten virtuaalikoneen. Sinun on suoritettava alla olevia ohjeita on ensisijainen ja toissijainen näennäiskoneiden. Suorita komentosarjan replikoinnin poistaa ja korvata SQLVM1 virtuaalikoneen nimen alapuolella.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Puhdista suojausasetuksia manuaalisesti (välillä paikallisen VMM sivustot ja Azure)

1. Suorita tämä komentosarja ensisijainen virtuaalikoneen asetusten tyhjentääksesi VMM lähdepalvelimeen:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Edellä kuvatut toimet tyhjentää replikoinnin asetukset vain VMM palvelimeen. Kun olet poistanut VMM palvelimesta replikoinnin Varmista Poista virtuaalikoneen tämän komentosarjan Hyper-V Host (isäntä)-palvelimessa replikoinnin. Korvaa SQLVM1 nimi virtuaalikoneen ja host01.contoso.com Hyper-V Host (isäntä)-palvelimen nimi.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Puhdista suojausasetuksia manuaalisesti (välillä Hyper-V sivustot ja Azure)

1. Lähteen Hyper-V Host (isäntä)-palvelimeen Poista virtuaalikoneen replikointi käyttää tätä komentosarjaa. Korvaa SQLVM1 virtuaalikoneen nimi.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Lopeta VMware virtual koneeseen tai fyysinen palvelimen suojaaminen

Jos haluat estää suojaaminen VMware virtual koneeseen tai fyysinen palvelimen tarvitset sitä suojauksen poistamista. Sen mukaan, miten voit poistaa suojauksen voit joutua muuttamaan suojausasetuksia manuaalisesti tietokoneessa Tyhjennä. 

### <a name="remove-protection"></a>Suojauksen poistaminen

1. Valitse Tunnistepilven ominaisuudet **näennäiskoneiden** -välilehdessä virtuaalikoneen > **Poista**.
2. **Poista virtuaalikoneen** valitsemalla jokin vaihtoehdoista:

    - **Käytöstä protection (Käytä palautus sääntö ja voimakkuutta koon)**, näkyviin vain ja ota tämä asetus, jos olet:
        - **Resized Virtuaalikoneen Äänenvoimakkuuden**, kun aseman virtuaalikoneen tiedosto on kriittinen. Valitse tämä asetus, jos näin tapahtuu. Poistaa käytöstä säilyttäen palautus asioista Azure suojaus. Kun olet koneen suojaus uudelleen käyttöön kokoa on muutettu äänenvoimakkuuden tiedot siirretään Azure.
        - **Suorita automaattisesti**, kun olet testannut ympäristön suorittamalla automaattisesti paikallisen VMware näennäiskoneiden tai fyysinen palvelimia Azure, valitse tämä asetus, Aloita suojaaminen paikallisen näennäiskoneiden uudelleen. Tämä asetus poistaa käytöstä kunkin virtuaalikoneen ja sitten sinun on niiden suojaus uudelleen käyttöön. Huomaa, että:
            - Tätä asetusta virtuaalikoneen poistaminen käytöstä ei vaikuta replikan virtuaalikoneen Azure-tietokannassa.
            - Mobility-palvelun eivät Poista virtuaalikoneen.
    
    - **Suojauksen poistaminen käytöstä**, jos otat käyttöön ja Tallenna asetus koneen enää suojataan palauttaminen. Tietokoneen suojausasetuksia oltava siivotaan automaattisesti.
    - **Poista säilö**– Jos valitset tämän vaihtoehdon, tietokoneen poistetaan vain sivuston palauttaminen säilöstä. Paikallisen suojausasetuksia koneen ei vaikuta. Voit poistaa tietokoneen asetukset ja poista virtuaalikoneen Azure tilaus ja on puhdistaa poistamalla Mobility-palvelun asetukset.
    
        ![Poista asetukset](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















