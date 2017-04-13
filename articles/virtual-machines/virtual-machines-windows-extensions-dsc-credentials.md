<properties
   pageTitle="Kulkeva tunnistetietojen avulla DSC Azure | Microsoft Azure"
   description="Yleiskatsaus siitä, että lähettämistä suojatusti tunnistetiedot Azuren näennäiskoneiden käyttämällä PowerShell toivottuja tilan määrittäminen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Tunnistetietojen välittämistä Azure DSC tunniste käsittely #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Tässä artikkelissa käsitellään toivottuja tilan määritys-tunniste Azure varten. Yleiskatsaus DSC tunniste käsittelijä tukikäytännöistä [Azure toivottuja tilan määritys tunniste käsittelijä esittely](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Kulkevat tunnistetiedot
Määritysprosessi osana voit on määrittämään käyttäjätilejä, käyttää palveluita tai asentaa ohjelman käyttäjän kontekstissa. Voit tehdä seuraavia asioita, tarvitset Anna tunnistetiedot. 

DSC sallii Parametroitu määritykset, jossa tunnistetiedot välitetty määritykset ja MOF-tiedostoina tallennettuja suojatusti. Azure-tunniste käsittelijä yksinkertaistaa tunnistetietojen hallinta tarjoamalla varmenteiden automaattinen hallinta. 

Ota huomioon seuraavat DSC määritysten komentosarja, joka luo paikallisen käyttäjätilin määritetyn salasanalla:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

On tärkeää jättää *solmu localhost* määrityksen. Jos tässä asiakirjassa ei ole näkyvissä, seuraavat vaiheet eivät toimi kuin tunniste käsittelijä etsitään erityisesti solmu localhost-lause. Kannattaa myös sisältämään typecast *[PsCredential]*, kun tämän tietyntyyppinen käynnistää tunniste tunnistetietojen salaamiseen. 

Julkaise tämä komentosarja Blob-objektien tallennustilaan:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Aseta Azure DSC-laajennus ja anna tunnistetiedot:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Miten tunnistetiedot suojataan
Tämän koodin suorittaminen pyytää olevan tunnistetiedon. Kun se on käytettävissä, se tallennetaan muistin lyhyesti. Kun se on julkaistu kanssa `Set-AzureVmDscExtension` cmdlet-komento, se siirretään HTTPS AM päälle, jos Azure tallentaa sen salattu levyllä paikallisen AM sertifikaatin avulla. Valitse se lyhyesti salauksen memory ja siirtää DSC uudelleen salattu.

Toiminta on erilainen kuin [käyttäminen ilman tunniste käsittelijä suojatut määritykset](https://msdn.microsoft.com/powershell/dsc/securemof). Azure-ympäristön avulla voit välittää määritysten tietoja suojatusti varmenteet kautta helposti. DSC tunniste käsittelijä käytettäessä ei ole tarpeen $CertificatePath tai $CertificateID / ConfigurationData $Thumbprint tapahtuma.


## <a name="next-steps"></a>Seuraavat vaiheet ##

Lisätietoja Azure DSC tunniste käsittelijä on artikkelissa [Johdanto Azure toivottuja tilan määritys tunniste käsittelyn](virtual-machines-windows-extensions-dsc-overview.md). 

Tarkastele [Azure Resurssienhallinta mallia DSC-tunniste](virtual-machines-windows-extensions-dsc-template.md).

Lisätietoja PowerShell-DSC [PowerShell dokumentaatio Centeristä](https://msdn.microsoft.com/powershell/dsc/overview). 

Voit etsiä lisätoimintoja voit hallita PowerShell DSC, [Selaa PowerShell-valikoiman](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) DSC lisämateriaalia kanssa.
