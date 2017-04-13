<properties
   pageTitle="Suojatun yksityinen klusterin yhdistäminen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään suojattu tietoliikenne erillinen tai yksityinen klusterin sekä välillä asiakkaat ja klusterin."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Suojattu erillinen klusterin X.509-varmenteita avulla Windowsiin

Tässä artikkelissa kuvataan suojaamisesta välisen erillinen Windows klusterin eri solmuissa sekä kuinka tarkistamiseen yhteyden tähän klusteriin asiakkaita käyttämällä X.509 varmenteet. Näin varmistat, että vain valtuutettujen käyttäjien on klusterin käyttöön sovellukset ja suorittaa hallintatehtäviä.  Varmenteen suojauksen pitäisi olla käytössä klusterin klusterin luomisen yhteydessä.  

Katso lisätietoja klusterin suojaus, kuten solmu solmu suojaus, asiakkaan solmu suojaus ja Roolipohjainen käyttöoikeuksien valvonta- [klusterin skenaarioita](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Mitkä varmenteet on pystyttävä?

Voit aloittaa, [Lataa erillisversion klusterin paketti](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) johonkin-yhteyttä klusterin solmut. Ladatun paketin etsii **ClusterConfig.X509.MultiMachine.json** tiedoston. Avaa tiedosto ja **Ominaisuudet** -kohdassa **Suojaus** -osiossa:

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Tässä osassa kuvataan sertifikaatteja, joita tarvitset erillisen Windows-klusterin suojaamiseen. Ottaa käyttöön sertifikaattiin perustuva suojauksen arvoksi *X509* **ClusterCredentialType** ja **ServerCredentialType** arvot.

>[AZURE.NOTE] [Allekirjoitus](https://en.wikipedia.org/wiki/Public_key_fingerprint) on ensisijainen henkilöllisyys varmenne. Lue [noutamisesta varmenteen allekirjoitus](https://msdn.microsoft.com/library/ms734695.aspx) voit selvittää sertifikaatteja, joita voit luoda allekirjoitus.

Seuraavassa taulukossa on lueteltu sertifikaatteja, joita tarvitset klusterin määritys:

|**CertificateInformation-asetus**|**Kuvaus**|
|-----------------------|--------------------------|
|ClusterCertificate|Tämän todistuksen tarvitaan suojaamiseen välisen klusterin solmut. Voit käyttää kahta eri varmenteet, ensisijainen ja toissijainen päivitystä varten. Määritä ensisijainen varmenteen allekirjoitus **allekirjoitus** -kohta ja että **ThumbprintSecondary** muuttujat toissijaisen.|
|ServerCertificate|Tämän todistuksen esitetään asiakkaalle yrittäessään tämän klusterin yhdistäminen. Asiasta voit käyttää samaa varmennetta *ClusterCertificate* ja *ServerCertificate*. Voit käyttää kahta eri palvelimen sertifikaatit, ensisijainen ja toissijainen päivitystä varten. Määritä ensisijainen varmenteen allekirjoitus **allekirjoitus** -kohta ja että **ThumbprintSecondary** muuttujat toissijaisen. |
|ClientCertificateThumbprints|Tämä on sertifikaatteja, joita haluat asentaa todennetut asiakassovelluksissa joukko. Voit valita toisen asiakkaan varmenteet asennettu tietokoneissa, joihin haluat sallia käyttöoikeus klusterin määrä. Määritä kunkin varmenteen allekirjoitus **CertificateThumbprint** muuttuja. Jos määrität **IsAdmin** *Tosi*, valitse asiakkaan tällä varmenteella asennettu mahdollisuuksista järjestelmänvalvojan hallintatoimintoihin klusterin. Jos **IsAdmin** on *Epätosi*, tällä varmenteella asiakkaan tehdä vain käyttäjän käyttöoikeuksia, yleensä vain luku-sallitut toiminnot. Lue lisätietoja roolit [Roolipohjainen käyttöoikeuksien valvonta (RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Voit määrittää ensimmäisen Asiakasvarmenne kutsumanimi **CertificateCommonName**. **CertificateIssuerThumbprint** on tämän varmenteen myöntäjä allekirjoitus. Lue [käsittelemisestä varmenteet](https://msdn.microsoft.com/library/ms731899.aspx) Lisätietoja nimien ja myöntäjä.|
|HttpApplicationGatewayCertificate|Tämä on valinnainen varmenne, jota voi olla määritetty, jos haluat suojata Http-sovelluksen yhdyskäytävä. Varmista, että reverseProxyEndpointPort määritetään nodeTypes, jos käytössäsi on tämän todistuksen.|

Tässä on esimerkki klusterin kokoonpano missä klusterin, palvelimen ja asiakkaan varmenteet saanut.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Aquire X.509 varmenteet
Suojaamiseen viestintä sisällä klusterin ensin tarvitset X.509-varmenteita klusterin solmujen hankkiminen. Lisäksi voit rajoittaa tämän klusterin koneet/rajoitettu yhteyden, sinun on hankkiminen ja asentaa asiakaskoneiden varmenteet.

Klustereiden käynnissä olevat tuotannon työmääriä, sinun täytyy käyttää [Varmenteen myöntäjä (CA)](https://en.wikipedia.org/wiki/Certificate_authority) kirjautunut suojaamiseen klusterin X.509-varmenne. Lisätietoja hankkiminen nämä varmenteet, siirry [Toimintaohje: varmenteen hankkiminen](http://msdn.microsoft.com/library/aa702761.aspx).

Klustereiden, jota käytetään testaus voit käyttää itse allekirjoitettua varmennetta.

## <a name="optional-create-a-self-signed-certificate"></a>Valinnainen: Itse allekirjoitetun varmenteen luominen
Voit luoda itse allekirjoitetun varmenteen, joka on suojattu oikein on käytettävä *CertSetup.ps1* komentosarja hakemistossa *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*palvelun kangasta SDK-kansiossa. Muokkaa tiedostoa ja tällä komennolla voit varmenteen luominen sopivat nimellä.

Varmenteen vieminen nyt suojattu salasanalla PFX-tiedostoon. Ensin sinun on hankittava varmenteen allekirjoitus. Certmgr.exe-sovelluksen käyttämiseen. Siirry kansioon, **Paikallinen Computer\Personal** ja Etsi juuri luomasi varmenne. Kaksoisnapsauta varmennetta, avaa se, valitse *tiedot* -välilehti ja Selaa alaspäin kohtaan *allekirjoitus* -kenttään. Kopioi allekirjoitusarvo kyselyjä alla PowerShell-komentoa, välilyönnit poistetaan.  Muuta sopivuus suojattua salasanan suojaaminen sen ja suorittaa PowerShell *$pswd* arvo:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Voit tarkastella sertifikaatin asennettu tietokoneeseen suorittamalla PowerShell-komentoa:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Vaihtoehtoisesti jos sinulla on Azure-tilaus, noudata kohta [Lisää varmenteiden avaimen säilö](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Asenna varmenteet
Kun olet sertifikaatit, voit asentaa ne klusterisolmut. Oman solmujen tarvitse uusimmat Windows PowerShellin 3.x asennettu. Tarvitset, toista nämä vaiheet jokaiselle solmun klusterin ja palvelimen varmenteiden ja minkä tahansa toissijainen.

1. Kopioi solmu .pfx-tiedostot.
2. Avaa PowerShell-ikkunassa järjestelmänvalvojana ja kirjoita seuraavat komennot. Korvaa *$pswd* salasanaa, jolla on luotu sertifikaatti. Korvaa *$PfxFilePath* kopioitu solmu .pfx koko polku.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Seuraavaksi sinun täytyy määrittää tämän varmenteen käyttöoikeuksia niin, että palvelun kangasta prosessi, joka suoritetaan Verkkopalvelu-tilillä, voit käyttää sitä suorittamalla seuraavaa komentosarjaa. Anna varmenne- ja "VERKKOPALVELUN" palvelutilin allekirjoitus. Voit tarkistaa, että sertifikaatti käyttöoikeusluettelot ovat oikein, certmgr.exe-työkalulla ja hallita yksityiset avaimet katsoo varmenne.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Yhdistä ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation-arvon CurrentUser - StoreName Omat
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Yhdistä ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
