<properties
    pageTitle="Käytön aloittaminen Azure pinon avaimen säilö | Microsoft Azure"
    description="Azure pinon avaimen säilö käytön aloittaminen"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Avaimen säilö käytön aloittaminen

Tässä osassa kuvataan, miten säilöön luoda, hallita näppäimet ja tietoja sekä sallivat käyttäjien tai sovellusten liittäminen kutsu Azure Pinotut säilöön toimintoja. Seuraavissa vaiheissa oletetaan vuokraajan tilaus on olemassa ja KeyVault-palvelu on rekisteröity kyseisen tilauksen piiriin kuuluvien. Esimerkki komennoista perustuvat KeyVault Cmdlet-komentoja käytettävissä Azure PowerShell SDK osana.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Vuokraajan tilaus säilö toimintojen ottaminen käyttöön 

Ennen kuin voit myöntää toimintojen vastaan minkä tahansa säilöön, sinun täytyy varmistaa, että tilauksesi on käytössä säilö toimintoja. Voit vahvistaa, lähettämällä PowerShell-komentoa:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Yllä olevan komennon tulosteen Ilmoita "Rekisteröity", "Rekisteröinti" tilan jokaisella rivillä.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Jos tämä ei ole, tulisi käynnistää KeyVault palvelun kuluessa tilauksen seuraava komento:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

Ja seuraavat kirjoitetaan-komennon:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Jos saat virheilmoituksen: "*tilaus ei ole rekisteröity Azure avaimen säilö*" silloin, kun käynnistettäessä KeyVault cmdlet-komennot, Vahvista kohti edellä mainittuja ohjeita KeyVault resurssi-palvelu on otettu käyttöön.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Vahvistettu (säilö) säilön luominen Azure Pinotut voivat tallentaa ja hallita salausavaimet ja tietoja

Jotta voit luoda säilöön, palvelutili pitää ensin luoda resurssiryhmä. Resurssiryhmä ja tallentamisesta säilöön luominen PowerShell-komennot, resurssiryhmä. Esimerkki myös kyseisen cmdlet-komento tyypillinen tuloste.

### <a name="creating-a-resource-group"></a>Resurssiryhmä luominen:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Tulos:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Luodaan säilöön:


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Tulos:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Cmdlet tulos näyttää avaimen säilö juuri luomasi ominaisuudet. Kaksi tärkeimmät ominaisuudet ovat:

-   **Säilö nimi**: Tämä on esimerkki **vault010**. Voit käyttää tätä nimeä muiden avaimen säilö cmdlet-komennot.

-   **Säilö URI**: esimerkissä tämä on https://vault010.vault.azurestack.local. Oman säilö sen REST API-Liittymän kautta käyttävät sovellukset on käytettävä tämän URI.

Suorita kaikki tämän avaimen säilö nyt oikeus Azure-tili. Kukaan muu ei vielä.


## <a name="operating-on-keys-and-secrets"></a>Liiketoiminnan näppäimet ja tietoja

Kun olet luonut säilöön, noudata luomisen vaiheet alla Hallitse näppäimet ja tietoja:

### <a name="creating-a-key"></a>Avaimen luomista

Jotta voit luoda avaimen, käytä **Lisää AzureKeyVaultKey** alla olevasta esimerkistä kohden. Onnistuneiden avaimen luonnin jälkeen cmdlet siirtää juuri luomasi tärkeimmät tiedot.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Seuraavassa on *Lisää AzureKeyVaultKey* cmdlet-komennon tulosteen:

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Voit nyt viitata avaimeen luomasi tai ladata sen URI käyttämällä Azure avaimen säilö. Käytä **https://vault010.vault.azurestack.local:443/näppäimet/keyVaultKeyName001** saat aina nykyisen version; ja tässä tietyn versio **https://vault010.vault.azurestack.local:443/näppäimet/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** avulla.

### <a name="retrieving-a-key"></a>Haetaan avain

**Hae AzureKeyVaultKey** avulla voit hakea avaimen ja sen tiedot kohti seuraavassa esimerkissä:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Seuraavassa on Get-AzureKeyVaultKey tulos

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Asetus salaisuus

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Tulos

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Hakeminen salaisuus

    Get-AzureKeyVaultSecret -VaultName vault010

Tulos

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Nyt avaimen säilö ja avain tai salaisuus on valmis käyttämään sovelluksia.
Sinun on sallittava sovellukset voivat käyttää niitä.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Määritä sovellus käyttää avainta tai salaisuus 

Määritä sovellus käyttää avainta tai säilö salaisuus, käytä Set -**AzureRmKeyVaultAccessPolicy** cmdlet-komento.

Esimerkiksi jos säilöön nimi on *ContosoKeyVault* ja haluat sallia sovelluksen on *Asiakastunnus* *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*, ja haluat sallia sovelluksen salaus ja kirjaudu näppäimet lisääminen säilöön, suorita seuraava:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Jos haluat sallia saman sovelluksesta lukea tietoja oman säilöön, suorita seuraava:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Seuraavat vaiheet
[Ottaa käyttöön AM avaimen säilö salasanalla](azure-stack-kv-deploy-vm-with-secret.md)

[AM avaimen säilö sertifikaatilla käyttöönotto](azure-stack-kv-push-secret-into-vm.md)