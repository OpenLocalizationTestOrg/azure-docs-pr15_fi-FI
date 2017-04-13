<properties
    pageTitle="Käyttöönotto AM käyttämällä Azure pinon avaimen säilö sertifikaatilla | Microsoft Azure"
    description="Lue, miten käyttöön AM ja lisää Azure pinon avain säilöstä varmenne"
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
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Luo VMs ja Sisällytä avain säilöstä noutaa varmenteet

Azure Pinotut VMs otetaan käyttöön Azure Resurssienhallinta kautta ja voi nyt tallentaa varmenteet Azure pinon avain säilöön. Valitse Azure pino (Microsoft.Compute resurssin palvelu johonkin tiettyyn) vie ne kyselyjä oman VMs kun VMs on otettu käyttöön. Varmenteiden voi käyttää esimerkiksi SSL, salaus ja pohjainen todennus skenaarioita.

Käyttämällä tätä menetelmää voit pitää varmenteen turvallisten. Nyt kuin AM kuva tai sovelluksen tai muita epäluotettavasta sijainnista. Avaimen säilö käytännön asianmukaiset käyttöoikeudet, voit määrittää voit myös määrittää, kuka pääsee käyttämään varmennetta. Toinen etu on, että voit hallita kaikki varmenteet Azure pinon avain säilöön yhdessä paikassa.

Seuraavassa on prosessin pikaisesti:

-   Tarvitset varmenteen .pfx-muodossa.

-   Luo avaimen säilö, (joko malli tai seuraava näyte komentosarja).

-   Varmista, että olet ottanut käyttöön EnabledForDeployment valitsin.

-   Lataa sertifikaatti salaisuus nimellä.

## <a name="deploying-vms"></a>VMs käyttöönotto

Tämä esimerkki komentosarja luo avaimen säilö ja tallentaa varmenteen avaimen säilö salaisuus kuin haluat paikallisen kansion .pfx-tiedosto tallennetaan.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

Komentosarjan alkuosa lukee .pfx-tiedosto, ja valitse se JSON objekti, jossa on koodattu tiedoston sisällön base64 stores. Valitse JSON-objekti on myös base64-koodattuja.

Seuraavaksi Luo uusi resurssiryhmä ja luo sitten avaimen säilö. Huomaa viimeinen parametri uusi AzureKeyVault-komento "-EnabledForDeployment", joka antaa access, Azure (erityisesti Microsoft.Compute resurssi-palvelu) voit lukea tietoja, Key säilö käyttöönotoissa.

Viimeisen komennon tallentaa vain base64-koodattuja JSON-objektin salaisuus avaimen säilöön.

Tässä on esimerkki tulosteen Edellinen komentosarja:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Microsoft on nyt valmis ottamaan AM mallin. Huomautus tulosteesta (kuten korostettu vihreällä Edellinen tulos) salaisuus URI.

Sinun on malli, joka sijaitsee täällä. Parametrit määräten halutut (lisäksi tavanomaiset AM parametrit) ovat säilö nimi, säilö resurssiryhmä ja salainen URI. Voit ladata GitHub ja muokata tarpeen mukaan.

Kun tämä AM otetaan käyttöön, Azure injects varmenteen tuominen AM.
Windows-varmenteet .pfx-tiedosto on lisätty ei voi viedä yksityinen avain. Varmenteen lisätään LocalMachine varmenteen sijainti-todistus-kaupan, että käyttäjän antama. Linux-sertifikaattitiedosto sijoitetaan /var/lib/waagent-hakemisto ja tiedostonimi-kohdassa &lt;UppercaseThumbprint&gt;X509 .crt sertifikaattitiedosto, ja &lt;UppercaseThumbprint&gt;yksityinen avain .prv.
Molempien tiedostojen on muotoiltu .pem.

Sovellus on yleensä löytää varmennetta käyttämällä allekirjoitus ja ei tarvitse muokkaaminen.

## <a name="retiring-certificates"></a>Poistamisesta varmenteet


Edellisen osion näytimme, voit siirtää olemassa olevat VMs uutta varmennetta. Mutta vanha varmenne on yhä AM ja ei voi poistaa. Suojauksen voit muuttaa-määritteen vanha salaisuus "Käytöstä"-niin, että vaikka vanhan mallin yrittää luoda AM tämän todistuksen vanhaa versiota, se. Näin, miten voit määrittää haluamasi salainen version poistetaan käytöstä:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Tekemistä


Uusi tällöin varmenteen voidaan säilyttää erillään AM kuva tai hakemuksen tiedot. Jotta on poistanut näyttäminen yhden pisteen.

Varmenne on uusia ja ladata eikä sinun tarvitse muodostaa uudelleen AM kuva tai sovelluksen käyttöönottopaketti avaimen säilö. Sovellus on edelleen annettava vaikka versiossa varmenteen uusi URI.

Erottamalla varmennetta AM tai sovelluksen paketti on pienennetty nyt henkilöstöä, jolla on suora pääsy varmenteen määrä.

Kuin lisättyä etu on nyt paikka avain säilöön hallittavan kaikki varmenteet-versiot mukaan lukien kaikki jotka on otettu käyttöön ajan kuluessa.

## <a name="next-steps"></a>Seuraavat vaiheet

[Ottaa käyttöön AM avaimen säilö salasanalla](azure-stack-kv-deploy-vm-with-secret.md)

[Salli sovellus, Key säilöön](azure-stack-kv-sample-app.md)