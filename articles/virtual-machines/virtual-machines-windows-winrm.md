<properties
    pageTitle="WinRM access määrittäminen näennäiskoneiden Azure Resurssienhallinta | Microsoft Azure"
    description="WinRM access Azure Resurssienhallinta-virtuaalikoneen käytettäväksi: n määrittäminen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Näennäiskoneiden Azure Resurssienhallinta WinRM käytön määrittäminen

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>Azure hallinnan ja Azure Resurssienhallinta WinRM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli

* Yleisiä tietoja Azure Resurssienhallinta Lue [artikkeli](../azure-resource-manager/resource-group-overview.md)
* Azure hallinnan ja Azure Resurssienhallinta eroista Lue [artikkeli](../resource-manager-deployment-model.md)

Avaimen ero WinRM määritysten välillä kaksi eri pinoihin määrittämisestä on miten varmenteen saa asennettuihin AM. Azure Resurssienhallinta-Pinotut varmenteet mallintaa resursseina hallitsee avaimen säilö resurssin toimittaja. Tämän vuoksi käyttäjä on antaa omia varmenteen ja lataa se ennen kuin käytät sitä AM avaimen säilö.

Seuraavat vaiheet, sinun on suoritettava, jotta voit määrittää AM WinRM yhteys

1. Luo avaimen säilö
2. Itse allekirjoitetun varmenteen luominen
3. Lataa itse allekirjoitetun varmenteen avaimen säilö
4. Oman itse allekirjoitetun varmenteen avaimen säilöön URL-Osoitteen hankkiminen
5. Viittaus itse allekirjoitettua varmenteet URL-osoite AM luomisen aikana

## <a name="step-1-create-a-key-vault"></a>Vaihe 1: Luo avaimen säilö

Voit käyttää Luo avain säilö-komennon alla

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Vaihe 2: Itse allekirjoitetun varmenteen luominen
Voit luoda itse allekirjoitettua varmennetta käyttämällä PowerShell-komentosarja

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Vaihe 3: Lataa itse allekirjoitetun varmenteen avaimen säilö

Ennen lataamista varmenteen avaimen säilö vaiheessa 1 luotu, se on muunnettu Microsoft.Compute resurssin tarjoajaan ymmärtävät muodossa. Alapuolella PowerShell komentosarjan ansiosta voit tehdä

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Vaihe 4: Itse allekirjoitettua varmennetta avain säilöön URL-Osoitteen hankkiminen

Microsoft.Compute resurssi-palvelu on sisällä avaimen säilö salaisuus URL-Osoitetta AM varattaessa. Näin voit ladata toiminta ja vastaavat varmenteen luomisesta AM Microsoft.Compute resurssi-palvelu.

>[AZURE.NOTE]Toiminta URL-osoite on myös versio. Esimerkki URL-osoite näyttää https://contosovault.vault.azure.net:443/tietoja/contososecret/01h9db0df2cd4300a20ence585a6s7ve alapuolella


#### <a name="templates"></a>Mallit

Voit avata linkin URL-mallin käyttäminen koodin alapuolella

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShellin

Voit käyttää tätä URL-Osoitteen avulla PowerShell-komennon alla

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Vaihe 5: Viitata itse allekirjoitettua varmenteet URL-osoite AM luomisen aikana

#### <a name="azure-resource-manager-templates"></a>Azure Resurssienhallinta-mallit

Luotaessa AM mallien varmenteen saa Viitattu tietoja-osan ja winRM-osan kuin alla:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Esimerkki mallin edellä löytyy tähän [201 AM – winrm-keyvault -](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows) ikkunoita

Tämän mallin lähdekoodin löytyy [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>PowerShellin

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Vaihe 6: Yhteyden muodostaminen AM
Ennen kuin voit muodostaa yhteyden AM, sinun on Varmista, että tietokone on määritetty WinRM etähallinnan. Käynnistäminen PowerShell järjestelmänvalvojana ja suorita, varmista, että-komennon alla määritys on valmis.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Voit joutua muuttamaan Varmista WinRM-palvelu on käytössä, jos edellä mainituista ei toimi. Tee, käyttäminen`Get-Service WinRM`

Kun asennus on valmis, voit muodostaa yhteyden käyttäminen AM-komennon alla

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate