<properties
    pageTitle="Azure Active Directory-toimialuepalveluista: Hallinnan oppaan | Microsoft Azure"
    description="Liittää Windowsin virtual koneen hallitun toimialueelle PowerShellin Azure ja perinteinen käyttöönotto-mallin avulla."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Liittää Windows Server-virtual machine hallitun toimialueelle PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure perinteinen portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: [Resurssienhallinta ja perinteinen](../resource-manager-deployment-model.md). Tässä artikkelissa käsitellään perinteinen käyttöönotto-mallin. Azure AD-toimialueen palveluista tällä hetkellä tue Resurssienhallinta-malli.

Nämä vaiheet näyttää, miten voit mukauttaa PowerShellin Azure-komentojen, luoda ja määrittää valmiiksi koneen Azure virtual Windows-pohjaisesta rakenneosan menetelmän avulla. Seuraavasti avulla voit luoda koneen Azure virtual Windows-pohjaisten ja liittäminen hallitun Azure AD-toimialueen palvelut-toimialueen.

Seuraavia ohjeita noudattamalla täytön----tyhjät-menetelmän PowerShellin Azure komentojoukon luomiseen. Tämän menetelmän voi olla hyödyllinen, jos ole aiemmin käyttänyt PowerShell tai haluat tietää, mitä arvoja voit määrittää onnistuneen konfiguroinnin. PowerShellin käyttäjille siirtää komentoja ja korvata oman muuttujien arvoihin (rivit alkaen "$").

Jos et ole jo tehnyt niin, käytä ohjeita [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) PowerShellin Azure asennetaan paikalliseen tietokoneeseen. Avaa Windows PowerShellin komentorivi-ikkuna.

## <a name="step-1-add-your-account"></a>Vaihe 1: Tilin lisääminen

1. Kirjoita **Lisää AzureAccount** PowerShell-kehotteessa ja sitten **ENTER-näppäintä**.
2. Kirjoita sähköpostiosoite ja Azure-tilaus ja valitse **Jatka**.
3. Kirjoita tilin salasana.
4. Valitse **Kirjaudu sisään**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Vaihe 2: Määritä tilauksen ja tallennustilaa tili

Määritä Azure tilauksen ja tallennustilaa tilin suorittamalla Windows PowerShellin komentokehotteeseen seuraavat komennot. Korvaa kaiken tarjoukset, mukaan lukien < ja > merkkejä, jossa oikeat nimet.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Saat oikeat tilauksen nimi **Hae AzureSubscription** -komennon SubscriptionName-ominaisuus. Saat oikeat tallennustilan tilin nimi **Hae AzureStorageAccount** -komennon otsikko-ominaisuuden, **Valitse AzureSubscription** -komennon suorittamisen jälkeen.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Vaihe 3: Vaiheittaiset ongelmatilanteita - valmistella virtuaalikoneen ja liittyä hallitun toimialueeseen
Seuraavassa on määrittäminen luomalla virtual tämän tietokoneen tyhjien rivien välinen kunkin lohkon luettavuuden PowerShellin Azure komennon.

Määritä tietoja Windows virtuaalikoneen valmistellaan.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Katso D-, DS- tai G sarjan näennäiskoneiden InstanceSize, arvot [virtuaalikoneen ja Azure Cloud palvelun koot](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Anna tiedot hallitun toimialueessa.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Määritä cloud-palvelun nimi.

    $svcname="Contoso100-test"

Määritä nimi, johon AM liittyneet virtual verkoston. Varmista, että AAD DS hallitun toimialueen virtual verkoston käytettävissä.

    $vnetname="MyPreviewVnet"

Valitse käytettävä valmistelu AM AM-kuva.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Määritä AM - AM nimen, esiintymän koon ja kuva, jota käytetään.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Hankkia AM paikallisen järjestelmänvalvojan tunnistetietoja. Valitse vahva paikallisen järjestelmänvalvojan salasanan.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Hanki liittymään AM hallitun toimialueen 'AAD Ohjauskoneen Järjestelmänvalvojat-ryhmään kuuluvat käyttäjätilin tunnistetiedot. Määritä sen toimialuenimi - esimerkiksi tämän esimerkin, emme Määritä 'Teemu' kuin käyttäjänimi.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Määritä AM - määrittää toimialueen liity vaatimus & tarvittavilla tunnuksilla.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Voit määrittää aliverkon AM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Valinnainen: Valitse toimialueen IP-osoite. Jos määrität Azure AD-toimialueen palveluista hallitun toimialueen on virtual verkon DNS-palvelimet IP-osoitteet, tämä vaihe ei tarvita.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Valmistele palattuasi AM Windows toimialueeseen liittymistä.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Voit valmistella Windows AM ja automaattisesti liittyä AAD toimialuepalveluita hallitun toimialueeseen komentosarja
Tämä PowerShell-komentoa määrittäminen Luo käyttöympäristön liiketoiminta-palvelimeen, jotka:

- Käyttää Windows Server 2012 R2 palvelinkeskuksen kuvaa.
- On erittäin pienen virtuaalikoneen.
- On nimi contoso-testi.
- Automaattisesti toimialueen liitetään contoso100 hallitun toimialueen.
- Virtual samassa verkossa kuin hallitun toimialue lisätään.

Seuraavassa on koko komentosarjan luominen Windowsin virtuaalikoneen ja automaattisesti liittyä Azure AD-toimialueen palveluista hallitun toimialueen.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
- [Azure AD - toimialueen palvelut-aloitusopas](./active-directory-ds-getting-started.md)

- [Hallitut Azure AD-toimialueen palvelut-toimialueen hallintaan](./active-directory-ds-admin-guide-administer-domain.md)
