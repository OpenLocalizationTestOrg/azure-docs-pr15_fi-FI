<properties
 pageTitle="Hallitse HPC Pack klusterin Laske solmujen | Microsoft Azure"
 description="Lisätietoja PowerShell-komentosarjaa työkaluja, joilla lisääminen, poistaminen, aloittaa ja lopettaa HPC Pack klusterin Laske solmujen Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Numero ja Laske solmujen HPC Pack-klusterin Azure-tietokannassa käytettävyyden hallinta

Jos olet luonut HPC Pack-klusterin Azure VMs, haluat ehkä tapoja helposti lisätä, poistaa, Käynnistä (valmisteleminen) tai lopettaa klusterin solmu (deprovision) Laske useita VMs. Voit tehdä seuraavat tehtävät, jotka on asennettu pään solmu AM Azure PowerShell-komentosarjojen suorittaminen Nämä komentosarjat avulla voit hallita numero ja HPC Pack-klusteriresurssien käytettävyyden kustannuksia ja hallita.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Edellytykset

* **Azure VMs klusterin HPC Pack** - HPC Pack-klusterin luominen perinteinen käyttöönotto-mallin avulla vähintään HPC Pack 2012 R2: n päivitys 1. Voit esimerkiksi automatisoida käyttöönoton käyttämällä nykyistä HPC Pack AM kuvan Azure Marketplacesta ja Azure PowerShell-komentosarjaa. Ja edellytykset on artikkelissa [HPC-klusterin HPC Pack IaaS käyttöönoton komentosarjan luominen](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

    Käyttöönoton jälkeen Etsi solmu hallinta komentosarjojen prosenttia CCP\_ALOITUS % pään solmun bin-kansio. Sinun on suoritettava kunkin komentosarjat järjestelmänvalvojana.

* **Azure julkaista asetukset tiedoston tai hallinta-varmenteen** - sinun on suoritettava pään solmun seuraavasti:

    * **Tuo Azure julkaista asetustiedosto**. Voit tehdä tämän Suorita pään solmu seuraavat Azure PowerShell cmdlet-komennot:

    ```
    Get-AzurePublishSettingsFile

    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```

    * **Määritä pään solmu Azure hallinta-varmenteen**. Jos sinulla on .cer-tiedosto Tuo CurrentUser\My varmenteen kaupan ja suorita sitten Azure-ympäristössä (AzureCloud tai AzureChinaCloud) seuraavan Azure PowerShell cmdlet-komennon:

    ```
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Laske solmun VMs lisääminen

Lisää **Lisää HpcIaaSNode.ps1** -komentosarjan suorittaminen solmujen.

### <a name="syntax"></a>Syntaksi
```
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametrit

* **Palvelun nimi** - pilveen nimi palvelun VMs lisätään uusi Laske solmun.

* **ImageName** - Azure AM kuvan nimi, joka saadaan Azure perinteinen portal tai Azure PowerShell cmdlet-komennon **Get-AzureVMImage**kautta. Kuva on täytettävä seuraavat vaatimukset:

    1. Windows-käyttöjärjestelmän on oltava asennettuna.

    2. Laske-solmu roolissa HPC Pack on oltava asennettuna.

    3. Kuva on oltava yksityinen kuva käyttäjä-luokassa Azure AM julkisen kuva.

* **Määrä** - solmu Laske VMs lisätään.

* **InstanceSize** - Laske-solmu VMs kokoa.

* **DomainUserName** - toimialueen käyttäjänimi, jota käytetään uusien VMs liittäminen toimialueen.

* **DomainUserPassword** - toimialuekäyttäjän salasana.

* **NodeNameSeries** (valinnainen) - nimeäminen solmujen Laske kaava. Muoto on oltava &lt; *pääkansion\_nimi*&gt;&lt;*Käynnistä\_numero*&gt;%. Esimerkiksi MyCN % 10 % tarkoittaa sarjan Laske solmu nimistä MyCN11 lähtien. Jos ei määritetä, komentosarja käyttää määritetyn solmu nimeäminen sarjan HPC-klusterin.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä lasketaan yhteen 20 kokoa suuri Laske solmu VMs-pilvi palvelun *hpcservice1*, AM kuva *hpccnimage1*perusteella.

```
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Poista Laske solmu VMs

Poista **Poista HpcIaaSNode.ps1** -komentosarjan suorittaminen solmujen.

### <a name="syntax"></a>Syntaksi

```
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametrit

* **Nimi** - klusterin nimet poistetaan. Yleismerkkejä tuetaan. Parametrin määrittäminen nimi on. Et voi määrittää **nimen** ja **solmu** parametrit.

* **Solmu** - HpcNode objektin solmujen poistetaan, joka saadaan HPC PowerShell-cmdlet-komennon [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kautta. Parametrin joukon nimi on solmu. Et voi määrittää **nimen** ja **solmu** parametrit.

* **DeleteVHD** (valinnainen) - asettaminen poistaa liittyvät levyjen VMs, poistetaan.

* **Voimassa** (valinnainen) - asetus Pakota HPC solmujen offline-tilassa, ennen kuin poistat ne.

* **Vahvista** (valinnainen) - Kysy vahvistus ennen komennon suorittamista.

* **WhatIf** - määrittäminen kuvataan, mitä käydä, jos komento suoritetaan ilman todella komennon suorittamista.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä pakottaa offline-tilassa solmujen kanssa nimet, jotka alkavat *HPCNode-CN -* ja ne poistetaan solmut ja niihin liittyvät levyjä.

```
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Käynnistä Laske solmu VMs

Aloita **Käynnistä HpcIaaSNode.ps1** -komentosarjan suorittaminen solmujen.

### <a name="syntax"></a>Syntaksi

```
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametrit

* **Nimi** - klusterisolmut alkamaan nimet. Yleismerkkejä tuetaan. Parametrin määrittäminen nimi on. Et voi määrittää **nimen** ja **solmu** parametrit.

* **Solmu**- HpcNode objektin solmujen alkamaan, joka saadaan HPC PowerShell-cmdlet-komennon [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kautta. Parametrin joukon nimi on solmu. Et voi määrittää **nimen** ja **solmu** parametrit.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä alkaa solmujen nimet, jotka alkavat *HPCNode-CN -*.

```
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Laske-solmu VMs lopettaminen

Lopeta **Stop HpcIaaSNode.ps1** -komentosarjan suorittaminen solmujen.

### <a name="syntax"></a>Syntaksi

```
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametrit


* **Nimi**- klusterisolmut pysäyttäminen nimet. Yleismerkkejä tuetaan. Parametrin määrittäminen nimi on. Et voi määrittää **nimen** ja **solmu** parametrit.

* **Solmu** - HpcNode objektin solmujen lopetetaan, joka saadaan HPC PowerShell-cmdlet-komennon [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx)kautta. Parametrin joukon nimi on solmu. Et voi määrittää **nimen** ja **solmu** parametrit.

* **Voimassa** (valinnainen) - asetus Pakota tulet ne HPC solmujen offline-tilassa.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä pakottaa offline-tilassa solmujen kanssa nimet, jotka alkavat *HPCNode-CN -* ja lopettaa solmut.

Lopeta-HPCIaaSNode.ps1 – nimi HPCNodeCN-*-voimassa

## <a name="next-steps"></a>Seuraavat vaiheet

* Jos haluat vielä automaattisesti Suurenna tai Pienennä klusterisolmut mukaan projektien ja tehtävien klusterin nykyisen työmäärää, on [automaattisesti suurennus ja pienennys HPC Pack klusterin resursseista Azure klusterin työmäärää mukaan](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).
