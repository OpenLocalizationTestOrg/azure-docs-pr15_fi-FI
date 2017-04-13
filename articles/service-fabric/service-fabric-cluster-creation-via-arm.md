
<properties
   pageTitle="Luo suojatun palvelun kangasta-klusterin Azure resurssien hallinnan avulla | Microsoft Azure"
   description="Tässä artikkelissa käsitellään suojattu palvelu kangasta-klusterin Azure-tietokannassa käyttämällä Azure Resurssienhallinta ja Azure Active Directory (AAD) Azure avaimen säilö asiakkaan todennuksen määrittäminen."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Palvelun kangasta-klusterin luominen Azure Azure resurssien hallinnan avulla

> [AZURE.SELECTOR]
- [Azure Resurssienhallinta](service-fabric-cluster-creation-via-arm.md)
- [Azure portal](service-fabric-cluster-creation-via-portal.md)

Tämä on vaiheittaiset ohjeet, joka opastaa sinua koskevan ilmoituksen määrittäminen suojatun Azure palvelun kangasta-klusterin Azure-tietokannassa Azure resurssien hallinnan avulla. Tämän oppaan käydään läpi seuraavat vaiheet:

 - Määrittää avaimen säilö näppäimet klusterin ja sovellusten hallinta.
 - Luo suojatun klusterin Azure Azure resurssien hallinta.
 - Todentaa käyttäjät ja Azure Active Directory (AAD) klusterihallintaa varten.

Suojatun klusterin on klusterin, joka estää näin luvattoman pääsyn hallintatoiminnot, joka sisältää käyttöönotosta, päivittäminen ja poistaminen sovellusten, palveluihin ja niiden sisältämät tiedot. Suojaamaton klusterin on klusterin, kuka tahansa voi muodostaa yhteyden milloin tahansa ja suorittaa hallinta. On mahdollista luoda suojaamaton klusterin, mutta se on **erittäin suositeltavaa luoda suojatun klusterin**. Suojaamaton klusteria **ei voi suojata myöhemmin** - uuden klusterin on luotava.

Käsitteitä ovat samat luominen suojatun klustereiden, ovatko varausyksiköiden Linux varausyksiköiden tai Windows-klustereiden. Lisää tiedot ja helper komentosarjojen suojatun Linux klustereiden luomista varten katso [luominen suojatun klustereiden Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Azure kirjautuminen
Tämän oppaan käyttää [PowerShellin Azure][azure-powershell]. Kun aloitat uuden PowerShell-istunnon Azure tiliisi kirjautuminen ja valitse tilauksesi, ennen kuin suoritetaan Azure komentoja.

Kirjautuminen azure-tili:

```powershell
Login-AzureRmAccount
```

Valitse tilaus:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Avaimen säilö määrittäminen

Tässä osassa käydään läpi avaimen säilö Azure-palvelu kangasta klusterin ja palvelun kangasta sovellusten luominen. Lisätietoja ja valmis-avain säilö on viittaavat [avaimen säilö aloitusopas][key-vault-get-started].

Palvelun kangasta käyttää X.509-varmenteita suojatun klusterin ja anna sovelluksen suojaustoiminnot. Azure avaimen säilö käytetään palvelun kangasta varausyksiköt Azure sertifikaattien hallinta. Azure klusteriin on otettu käyttöön, kun palvelua kangasta klustereiden luomisesta vastuussa Azure resurssin tarjoajaan hakee varmenteet avain säilöstä ja asentaa ne klusterin VMs.

Seuraavassa kaaviossa on kuvattu avaimen säilö, palvelun kangasta klusterin ja Azure resurssin palvelu, joka tallennetaan avaimen säilö, kun se luo klusterin sertifikaattien suhteen:

![Sertifikaatin asennus][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Resurssin ryhmän luominen

Ensimmäiseksi on luotava resurssiryhmä avaimen säilö varten. Avaimen säilö liikkeelle oma resurssiryhmä suositellaan. Näin voit poistaa suorittaminen ja tallennustilaa resurssiryhmät, mukaan lukien resurssiryhmä, jossa on Service kangasta klusterin näppäimet ja tietoja menettämättä. Resurssiryhmä, jossa on avain säilö on oltava sama alue kuin klusteriin, joka on käytössä.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Luo avaimen säilö 

Luo uusi resurssiryhmä avaimen säilö. -Avain säilö **on otettava käyttöön käyttöönottoa varten** palvelun kangasta resurssin toimittaja se ja asenna-klusterin varmenteet käyttämistä:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Jos sinulla on aiemmin avain-säilö, voit ottaa sen käyttöön käyttöönottoa Azure CLI:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Varmenteiden lisääminen avaimen säilö

Varmenteita käytetään palvelun kangasta todennus ja salaus suojaamiseen klusterin ja sen sovellusten eri ominaisuuksia. Lisätietoja varmenteiden käyttämisestä palvelun kangasta on artikkelissa [palvelun kangasta klusterin suojaustapoja][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Klusterin ja palvelimen varmenne (pakollinen) 

Tämän todistuksen tarvitaan suojata klusterin ja estää näin luvattoman pääsyn. Se on klusterin suojaus on muutamia esimerkkejä:
 
 - **Klusterin käyttöoikeuden:** Todentaa solmu solmu viestintä klusterin yhdistämiseen. Vain solmut, jotka voivat todista lisätoimenpiteisiin jäsenyyden tällä varmenteella liittyä klusterin.
 - **Palvelimen todennuksen:** Todentaa klusterin hallinta päätepisteet management-asiakas, jotta management-asiakas tietää, se on puhuminen reaali klusterin. Tämän todistuksen myös tarjoaa SSL HTTPS-hallinnan API ja palvelun kangasta Explorer HTTPS-protokollan välityksellä.

Tukemaan näiltä varmenne on täytettävä seuraavat vaatimukset:

 - Varmenne on oltava yksityinen avain.
 - Varmenne on luotava avaimen exchange, voi viedä henkilökohtaisten tietojen vaihtaminen (.pfx)-tiedostoon.
 - Varmenteen aihe on vastattava käyttää palvelun kangasta klusterin toimialueen. Tämä matchng tarvitaan säätää SSL klusterin HTTPS hallinta päätepisteet ja palvelun kangasta Explorer. SSL-varmenteen varmenteiden myöntäjältä (CA) ei voi hankkia varten `.cloudapp.azure.com` toimialueen. Mukautetun toimialuenimen yhteyttä klusterin on hankittava. Kun pyydät varmenteen myöntäjältä, sertifikaatin aihe on vastattava yhteyttä klusterin käyttää mukautettua toimialuenimeä.

### <a name="application-certificates-optional"></a>Sovelluksen varmenteet (valinnainen)

Jokin muu luku antaminen voidaan asentaa sovelluksen tietoturvasyistä klusterissa. Ennen kuin luot yhteyttä klusterin, harkitse sovelluksen suojaustapoja, jotka edellyttävät varmenne on asennettava solmut, kuten:

 - Salaaminen ja salauksen purkaminen sovelluksen kokoonpanon arvoista
 - Salauksen solmujen tietojen replikoinnin aikana 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Muotoilun varmenteet Azure resurssin palvelun käyttöä varten

Yksityisen avaimen tiedostot (.pfx) voidaan lisätä ja käyttää suoraan avaimen säilö. Resurssin Azure-palvelu edellyttää kuitenkin avaimet tallennetaan JSON erikoismuotoilua, joka sisältää .pfx kuin base-64-koodatun merkkijonon ja yksityisen avaimen salasana. Sopimaan näitä vaatimuksia näppäimet on sijoitettu JSON merkkijonon ja sitten tallennettujen *tietoja* avain säilöön.

Voit helpottaa prosessia PowerShell-moduulin on [käytettävissä GitHub][service-fabric-rp-helpers]. Käyttää-moduulia seuraavasti:

  1. Lataa repo koko sisällön paikalliseen kansioon. 
  2. Tuo moduulin PowerShell-ikkunassa seuraavasti:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
`Invoke-AddCertToKeyVault` Komento PowerShell-moduulin automaattisesti muotoilee sertifikaatin yksityinen avain JSON merkkijonoksi ja lataa se avaimen säilö. Käytä klusterin varmenne- ja muita sovelluksen sertifikaatteja lisääminen avaimen säilö. Toista tämä vaihe asentaa yhteyttä klusterin varmenteet.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Edellisen merkkijonot on kaikki avaimen säilö palvelun kangasta klusterin Resurssienhallinta mallina, joka asentaa varmenteet solmu todennusta, hallinnan päätepisteen suojaus ja todennus ja kaikki muut sovelluksen suojausominaisuuksia, jotka X.509-varmenteita käyttävät edellytykset. Tässä vaiheessa pitäisi nyt olla seuraavat asetukset Azure-tietokannassa:

 - Avaimen säilö resurssiryhmä
   - Avaimen säilö
     - Klusterin palvelimen todennusvarmenne
     - Sovelluksen varmenteet

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory asiakkaan todennuksen määrittäminen

AAD avulla organisaatiot (nimeltään alihallinnat) hallittavan käyttöoikeuden sovellukset, jotka on jaettu sovellukset, joilla on verkkopohjainen kirjautumisen Käyttöliittymän ja sovellusten native client käytettävyyttä. Tässä asiakirjassa on oletetaan, että olet jo luonut alihallinnan. Jos näin ei ole, Aloita lukemalla [saaminen Azure Active Directory-vuokraajan][active-directory-howto-tenant].

Palvelun kangasta klusterin tarjoaa useita pikakuvakkeiden hallinta-toimintoja, kuten verkkopohjaisia- [Palvelun kangasta Explorer] [ service-fabric-visualizing-your-cluster] ja [Visual Studio][service-fabric-manage-application-in-visual-studio]. Voit luoda seurauksena kaksi AAD sovellukset voivat valvoa klusterin, yksi web-sovelluksen ja alkuperäisen sovelluksen.

Helpottaa seuraavia asioita yhdistävää määrittäminen AAD palvelun kangasta klusteriin on luonut Windows PowerShell-komentosarjojen joukko.

>[AZURE.NOTE] Sinun on suoritettava nämä vaiheet *ennen* luomista klusterin niin tilanteissa, joissa komentosarjat toiminta klusterin nimet ja päätepisteet, nämä tiedostot on suunniteltu arvot, ei nykyisten, jonka olet jo luonut.

1. [Lataa komentosarjat] [ sf-aad-ps-script-download] tietokoneeseen.

2. Zip-tiedostoa hiiren kakkospainikkeella, valitsemalla **Ominaisuudet**, valitse **Salli** -valintaruutu ja käytä.

3. Pura zip-tiedosto.

4. Suorita `SetupApplications.ps1`, tarjoamalla TenantId, ClusterName ja WebApplicationReplyUrl parametreiksi. Esimerkki:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Voit etsiä oman **TenantId** suoritetaan PowerShell-komennolla "Get-AzureSubscription" ". Tässä näkyvät jokaisen tilauksen **TenantId** .

    **ClusterName** käytetään etuliite komentosarja luomia AAD sovelluksia. Se ei tarvitse vastaamaan todellinen klusterinimi tarkalleen siinä muodossa kuin se on tarkoitettu vain helpottaa AAD palvelutiedot yhdistäminen palvelun kangasta klusterin, ne tottunut kanssa.

    **WebApplicationReplyUrl** on oletusarvoinen päätepiste, joka AAD palauttaa käyttäjille, kun olet suorittanut kirjautumisprosessin. Sinun kannattaa asettaa tämän palvelun kangasta Explorer päätepisteen yhteyttä klusterin, joka on oletusarvoisesti:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Sinua kehotetaan kirjautua tilille, jolla on AAD vuokraajan järjestelmänvalvojan oikeudet. Kun teet, komentosarja etenee web ja alkuperäisen sovellusten edustavan palvelun kangasta klusterin luomiseen. Jos tarkastelet [Azure perinteinen portal]-sovellukset vuokraajan[azure-classic-portal], näkyviin tulee kaksi uusia merkintöjä:

    - *ClusterName*\_klusteri
    - *ClusterName*\_asiakas

    Komentosarjan tulostaa, joten vaatii Azure Resurssienhallinta-mallin, kun luot klusterin seuraavan osion Json PowerShell-ikkuna.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Palvelun kangasta klusterin Resurssienhallinta mallin luominen

Tässä osassa edellisen PowerShell-komennoilla tulosteen käytetään mallissa Service kangasta klusterin Resurssienhallinta.

Esimerkki Resurssienhallinta mallit ovat käytettävissä [Azure quick start-mallit-osa GitHub][azure-quickstart-templates]. Mallit voidaan klusterin mallin pohjana. 

### <a name="create-the-resource-manager-template"></a>Resurssienhallinta-mallin luominen

Tämän oppaan käyttää [5 solmun suojatun klusterin] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] Esimerkki mallista ja Malliparametrit. Lataa `azuredeploy.json` ja `azuredeploy.parameters.json` tietokoneeseen ja avaa molemmat tiedostot Suosikit-tekstieditorissa.

### <a name="add-certificates"></a>Lisää varmenteet

Varmenteiden lisätään klusterin Resurssienhallinta mallin viittaamalla avaimen säilö, joka sisältää varmenne-avaimet. On suositeltavaa avaimen säilö nämä arvot sijoitetaan Säilytä Resurssienhallinta mallin tiedoston Uudelleenkäytettävä ja vapaa käyttöönoton tiettyjen arvojen Resurssienhallinta mallin parametrit-tiedostossa.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Kaikki varmenteiden lisääminen VMSS osProfile

Jokaisen varmenne, jota on asennettava klusterissa on oltava määritettynä VMSS resurssi (Microsoft.Compute/virtualMachineScaleSets) osProfile-osassa. Tämän tiedon varmenteen asentaminen VMs resurssin Internet-palveluntarjoajaan. Tämä vaihtoehto sisältää klusterin varmenteen sekä aiot käyttää sovellusten sovelluksen suojauksen sertifikaatteja:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Palvelun kangasta klusterin varmenteen määrittäminen

Klusterin todennusvarmenne on myös määritettävä palvelun kangasta klusteriresurssi (Microsoft.ServiceFabric/clusters) ja palvelun kangasta tunniste VMSS VMSS resurssista. Näin palvelun kangasta resurssin tarjoaja voi määrittää sen käytettäväksi klusterin ja palvelimen todennusta hallinta päätepisteiden.

##### <a name="vmss-resource"></a>VMSS resurssi:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Palvelun kangasta resurssi:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Lisää AAD config

Aiemmin luotu AAD-määritys voidaan lisätä suoraan Resurssienhallinta-malli, mutta kannattaa pura ensin tiedostoon parametrit pitämään Resurssienhallinta-mallin Uudelleenkäytettävä ja vapaa-arvojen tietyn käyttöympäristöön parametrien arvot.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "määrittäminen-arm" ></a>määrittäminen Resurssienhallinta Malliparametrit

Lopuksi täyttäminen parametrit-tiedoston käyttämällä tulosteen arvot avaimen säilö ja AAD PowerShell-komennot:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
Tässä vaiheessa pitäisi nyt olla seuraavasti:

 - Avaimen säilö resurssiryhmä
    - Avaimen säilö
    - Klusterin palvelimen todennusvarmenne
    - Tietojen salaus varmenne
 - Azure Active Directory-vuokraajan 
    - Verkkopohjaisia hallinta ja palvelun kangasta Explorer AAD hakeminen
    - AAD sovelluksen native client hallintaa varten
    - Käyttäjät, joilla on roolien perusteella 
 - Palvelun kangasta klusterin Resurssienhallinta-malli
    - Varmenteiden säilöön avaimen avulla
    - Azure Active Directory, jotka on määritetty 

Seuraavassa kaaviossa on kuvattu, jossa avaimen säilö ja AAD määritys mahdu Resurssienhallinta-malli.

![Resurssienhallinta riippuvuuden kartta][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Klusterin luominen

Olet nyt valmis käyttämällä [ARM käyttöönoton]klusterin luomaan[resource-group-template-deploy].

#### <a name="test-it"></a>Testaa se

Käytä seuraavaa PowerShell-komentoa Testaa Resurssienhallinta mallin parametrit-tiedoston:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Ottaa raporttinäkymän käyttöön

Jos Resurssienhallinta mallin testi, Resurssienhallinta mallin käyttöön parametrit-tiedoston seuraavan PowerShell-komennon avulla:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Käyttäjien määrittäminen roolit

Kun olet luonut edustavan yhteyttä klusterin sovellukset, sinun täytyy määrittää käyttäjien palvelun kangasta tukemat rooleja: vain luku- ja järjestelmänvalvoja. Voit tehdä tämän [Azure perinteinen portal][azure-classic-portal].

1. Siirry alihallintaan ja valitse sovellukset.
2. Valitse web-sovelluksen, jonka nimi on esimerkiksi `myTestCluster_Cluster`.
3. Valitse käyttäjät-välilehti.
4. Valitse käyttäjä voi määrittää ja näytön alareunassa **Liitä** -painiketta.

    ![Käyttäjien määrittäminen roolit-painike][assign-users-to-roles-button]

5. Valitse rooli, jotta voit määrittää käyttäjälle.

    ![Käyttäjien määrittäminen roolit][assign-users-to-roles-dialog]

>[AZURE.NOTE] Saat lisätietoja palvelun kangasta roolien [Roolipohjainen käyttöoikeuksien valvonta palvelun kangasta asiakkaille](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Luo suojatun klustereiden Linux

Voit helpottaa prosessin helper komentosarja on annettu [tähän](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Helper tämän komentosarjan avulla, sen oletetaan, että sinulla on jo asennettu Azure CLI ja polku on. Varmista, että komentosarja on oikeuksia suorittaa suorittamalla `chmod +x cert_helper.py` jälkeen ladata sen verkosta. Ensimmäiseksi on Kirjaudu sisään käyttämällä kanssa CLI Azure-tiliin `azure login` komento. Azure tilille kirjautumisessa, kun Käytä avustaja CA allekirjoitettua varmennetta, komento seuraavasti:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Tämä komento palauttaa seuraavat kolme merkkijonot tulos: 

1. SourceVaultID, joka on uusi KeyVault ResourceGroup, se luodaan tunnus. 

2. CertificateUrl käyttämiseen varmenne.

3. CertificateThumbprint, jota käytetään todennusta varten.


Seuraavassa esimerkissä esitetään, miten-komennolla:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Edellisen komennon suorittamista on kolme merkkijonot kanssa seuraavasti:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Varmenteen aihe on vastattava käyttää palvelun kangasta klusterin toimialueen. Tämä on määritettävä SSL klusterin HTTPS hallinta päätepisteet ja palvelun kangasta Explorer. SSL-varmenteen varmenteiden myöntäjältä (CA) ei voi hankkia varten `.cloudapp.azure.com` toimialueen. Mukautetun toimialuenimen yhteyttä klusterin on hankittava. Kun pyydät varmenteen myöntäjältä sertifikaatin aihe on vastattava yhteyttä klusterin käyttää mukautettua toimialuenimeä.

Nämä ovat luominen suojatun palvelun kangasta klusterin (ilman AAD), [Määritä Resurssienhallinta Malliparametrit](#configure-arm)-palvelussa kuvatulla tarvittavat merkinnät. Voit muodostaa yhteyden suojatun klusterin kautta [todennustapa client access klusteriin](service-fabric-connect-to-secure-cluster.md)ohjeiden mukaisesti. Linux esikatselu klustereiden eivät tue AAD todennusta. Voit määrittää järjestelmänvalvojan ja asiakkaan roolit [roolien käyttäjille](#assign-roles)kohdassa kuvatulla tavalla. Järjestelmänvalvoja ja asiakkaan roolit Linux esikatselu-klusterin määritettäessä on annettava varmenteen thumbprints todennustavaksi (eikä aihe, koska ei ole ketju vahvistus tai kumottujen varmenteiden suoritetaan tähän preview-versio).


Jos haluat käyttää itse allekirjoitettua varmennetta testikäyttöön, voit käyttää saman komentosarjan lataaminen KeyVault, antamalla merkintä ja luo itse allekirjoitetun varmenteen `ss` sen sijaan, että Sertifikaattipolku ja varmenteen nimi. Esimerkiksi näkyviin luomiseen ja ladataan itse allekirjoitetun varmenteen seuraava komento:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Tämä komento palauttaa saman kolme merkkijonoja, SourceVault, CertificateUrl ja CertificateThumbprint, jolla voidaan luoda suojatun Linux-klusterin, sekä itse allekirjoitetun varmenteen sisältävään sijainti. Sinun on muodostaa yhteyttä klusterin itse allekirjoitettua varmennetta.  Voit muodostaa yhteyden suojatun klusterin kautta [todennustapa client access klusteriin](service-fabric-connect-to-secure-cluster.md)ohjeiden mukaisesti. Varmenteen aihe on vastattava käyttää palvelun kangasta klusterin toimialueen. Tämä on määritettävä SSL klusterin HTTPS hallinta päätepisteet ja palvelun kangasta Explorer. SSL-varmenteen varmenteiden myöntäjältä (CA) ei voi hankkia varten `.cloudapp.azure.com` toimialueen. Mukautetun toimialuenimen yhteyttä klusterin on hankittava. Kun pyydät varmenteen myöntäjältä sertifikaatin aihe on vastattava yhteyttä klusterin käyttää mukautettua toimialuenimeä.

Avustaja-komentosarjan myöntämä parametrit voi täyttää portaalin [Azure-portaalissa klusterin luominen](service-fabric-cluster-creation-via-portal.md#create-cluster-portal)-kohdassa kuvatulla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä vaiheessa sinun on suojattu klusteri ja Azure Active Directory tarjoavat hallinta todennusta. Seuraavaksi [muodostaa yhteyttä klusterin](service-fabric-connect-to-secure-cluster.md) ja opi hallinnasta [sovelluksen tietoja](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Asiakkaan todennuksen Azure Active Directory-asetuksiin liittyviä ongelmia

Jos käytössä ilmenee ongelma Azure Active Directory asiakkaan todennuksen määrittäminen aikana, tarkista seuraavat suggestings mahdollisia ratkaisuja.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Palvelun kangasta Explorer kysyy varmenteen valitseminen

#### <a name="problem"></a>Ongelma

Kirjautuminen onnistui-kirjautumissivu AAD kangasta Explorerissa, jälkeen selaimen palauttaa kotisivulle, mutta pyytää valintaikkunan valitsemiseen varmenne.

![SFX Valitse varmenne-valintaikkuna][sfx-select-certificate-dialog]

#### <a name="reason"></a>Syy

Käyttäjä ei ole määritetty roolia AAD klusterin sovelluksessa. Näin AAD todennus epäonnistuu palvelun kangasta klusterissa. Palvelun kangasta Explorer kuuluu takaisin varmenteen todentamiseen.

#### <a name="solution"></a>Ratkaisu

Noudata ohjeita AAD määrittäminen ja määritä liiketoiminta-roolien. Lisäksi "Käyttäjän VARAUKSEN TARVITAAN, ACCESS-Sovelluksen" suositellaan voi ottaa käyttöön kuin `SetupApplications.ps1` onko.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Yhdistä PowerShell epäonnistuu virheen vuoksi: määritetyn tunnistetiedot ovat virheellisiä

#### <a name="problem"></a>Ongelma

Kun PowerShellin avulla muodostaa yhteyttä klusterin "AzureActiveDirectory" suojaus-tilassa, onnistuneesti AAD kirjautumissivu-kirjautuminen, kun yhteys epäonnistuu virheen vuoksi: määritetyn tunnistetiedot ovat virheellisiä näkyvän.

#### <a name="solution"></a>Ratkaisu

Sama kuin edellä.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Palvelun kangasta Explorer kirjautuminen samalla virhe: AADSTS50011

#### <a name="problem"></a>Ongelma

Kirjautuminen AAD kangasta Explorerissa kirjautumissivulla, kun sivun palauttaa Kirjaudu virhe - AADSTS50011: vastausosoite &lt;URL-osoite&gt; vastaa sovelluksen määritetty vastaa osoitteet: &lt;guid&gt;. 

![Vastaa SFX vastausosoite][sfx-reply-address-not-match]

#### <a name="reason"></a>Syy

Palvelun kangasta Explorer yrittää todennetaan AAD edustava cluster(web) sovelluksen osana pyynnön se sisältää uudelleenohjauksen palauttaa URL-osoite. Mutta se ei ole mainittu AAD sovelluksen 'vastaa URL-luettelosta.

#### <a name="solution"></a>Ratkaisu

"Vastaa URL-palvelun kangasta Explorer URL-osoitteen lisääminen cluster(web)-sovelluksen määrittäminen-välilehti tai korvaa jokin kohde-luettelossa. Valitse Tallenna.

![Verkkosovelluksen vastaa URL-osoite][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Voit käyttää useita klustereiden saman AAD Alihallinta?

#### <a name="answer"></a>Answer (vastaus)

Kyllä. Mutta muista lisätä palvelun URL-Osoitteen kangasta, Explorer cluster(web) sovelluksen muussa palvelun kangasta Explorer ei toimi.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Miksi tarvitsenko edelleen palvelinvarmennetta AAD käytössä?

#### <a name="answer"></a>Answer (vastaus)

FabricClient ja FabricGateway suorittaa kaksisuuntainen käyttöoikeuksien. AAD todennusta, jos AAD integrointi on asiakkaan käyttäjätiedot-palvelimen nimen ja palvelinvarmennetta käytetään palvelimen jäsenyyden vahvistamiseksi. Lisätietoja varmenteen toiminnasta palvelun kangasta Tarkista [X.509-varmenteita ja palvelun kangasta][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png