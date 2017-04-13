<properties
   pageTitle="Ottaa käyttöön usean NIC VMs Azure-CLI resurssien hallinnan avulla | Microsoft Azure"
   description="Lue, miten voit ottaa usean NIC VMs Azure-CLI resurssien hallinnan avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-the-azure-cli"></a>Usean NIC VMs käyttämällä Azure-CLI käyttöönotto

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](virtual-network-deploy-multinic-classic-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ei voi tällä hetkellä on VMs yksittäisen NIC kanssa ja VMs, jossa on useita NIC saman resurssiryhmä. Tämän vuoksi haluat toteuttavien uudelleen palvelimissa eri kuin muiden osien resurssiryhmä. Seuraavia ohjeita käyttää nimetyt *IaaSStory* tärkeimmät resurssiryhmä ja *IaaSStory Taustajärjestelmä* uudelleen palvelimia resurssiryhmä.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit ottaa käyttöön uudelleen palvelimissa, sinun täytyy käyttöönotto ja kaikki tarvittavat resurssit tämän skenaarion tärkeimmät resurssiryhmä. Ottaa käyttöön näiden resurssien, noudata seuraavia ohjeita.

1. Siirry [mallisivun](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Valitse mallisivun oikealta puolelta **ylätason resurssiryhmä** **käyttöönotto Azure**.
3. Tarvittaessa Muuta kaavarivillä parametriarvot ja noudata ohjeita ottamaan resurssiryhmän Azure esikatselu-portaalissa.

> [AZURE.IMPORTANT] Varmista, että tallennustilan tilin nimet ovat yksilöllisiä. Kaksoiskappaleiden tallennustilan tilin nimiä ei voi olla Azure-tietokannassa.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Edellinen loppuun VMs käyttöönotto

Taustajärjestelmä VMs määräytyvät alla luetellut resurssit luomista.

- **Tallennustilan tilin tiedot levyille**. Parantaa suorituskykyä tietoja levyjen tietokannan palvelimissa, jotka käyttävät tasainen tilan asemaa Suoritettaessa tekniikka, jossa pyydetään premium-tallennustilan tilin. Varmista, että käyttöönottoa premium tallennustilan tukemaan Azure sijainti.
- **NIC**. Kunkin AM on kaksi NIC-tietokannan käyttämiseen ja yksi hallintaa varten.
- **Saatavuus**. Tietokantapalvelin lisätään yksi käytettävyys määrittäminen, jotta vähintään yksi VMs on ylös ja suorittamalla ylläpidon aikana.

### <a name="step-1---start-your-script"></a>Vaihe 1 – komentosarja

Voit ladata käytetty koko bash komentosarja [tähän](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh). Noudata seuraavia ohjeita voit muuttaa toimimaan ympäristössäsi komentosarja.

1. Muuttaa arvoja muuttujien, otettu käyttöön edellä [edellytykset](#Prerequisites)aiemmin resurssiryhmän perusteella.

        existingRGName="IaaSStory"
        location="westus"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"
        remoteAccessNSGName="NSG-RemoteAccess"

2. Muuttaa arvoja muuttujien, jota haluat käyttää Taustajärjestelmä käyttöönottoa varten arvojen perusteella.

        backendRGName="IaaSStory-Backend"
        prmStorageAccountName="wtestvnetstorageprm"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        publisher="Canonical"
        offer="UbuntuServer"
        sku="14.04.2-LTS"
        version="latest"
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskName="datadisk"
        nicNamePrefix="NICDB"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

3. Noutaa tunnus `BackEnd` aliverkon, jossa VMs luodaan. Sinun täytyy tehdä tämän jälkeen voit liittyä tämän aliverkon NIC ovat eri resurssiryhmä.

        subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
                        --vnet-name $vnetName \
                        --name $backendSubnetName|grep Id)"
        subnetId=${subnetId#*/}

    >[AZURE.TIP] Edellä ensimmäisen komennon käyttää [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) ja [merkkijonon käytöstä](http://tldp.org/LDP/abs/html/string-manipulation.html) (tarkemmin sanottuna alimerkkijono poistaminen).

4. Noutaa tunnus `NSG-RemoteAccess` NSG. Sinun täytyy tehdä tämän jälkeen voit liittyä tämän NSG NIC ovat eri resurssiryhmä.

        nsgId="$(azure network nsg show --resource-group $existingRGName \
                        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Vaihe 2: Luo tarvittavat resurssit oman VMs

1. Luo uusi resurssiryhmä kaikkien Taustajärjestelmä resurssien. Huomaa, käyttäminen `$backendRGName` muuttujan resurssin nimi- ja `$location` Azure alue.

        azure group create $backendRGName $location

2. Luo omasi VMs käytettävän OS ja tietojen levyjen premium tallennustilan-tili.

        azure storage account create $prmStorageAccountName \
            --resource-group $backendRGName \
            --location $location \
            --type PLRS

3. Luo VMs määrittäminen saatavuus.

        azure availset create --resource-group $backendRGName \
            --location $location \
            --name $avSetName

### <a name="step-3---create-the-nics-and-backend-vms"></a>Vaihe 3 – NIC- ja Taustajärjestelmä VMs luominen

1. Aloita silmukka, voit luoda useita VMs, perusteella `numberOfVMs` muuttujat.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Luo kullekin AM NIC tietokannan käytön.

            nic1Name=$nicNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x
            azure network nic create --name $nic1Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress1 \
                --subnet-id $subnetId

3. Luo kullekin AM NIC etäkäyttöä varten. Ilmoitus `--network-security-group` parametri, liittää NIC NSG avulla.

            nic2Name=$nicNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x
            azure network nic create --name $nic2Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress2 \
                --subnet-id $subnetId $vnetName \
                --network-security-group-id $nsgId

4. Luo AM.

            azure vm create --resource-group $backendRGName \
                --name $vmNamePrefix$suffixNumber \
                --location $location \
                --vm-size $vmSize \
                --subnet-id $subnetId \
                --availset-name $avSetName \
                --nic-names $nic1Name,$nic2Name \
                --os-type linux \
                --image-urn $publisher:$offer:$sku:$version \
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --os-disk-vhd $osDiskName$suffixNumber.vhd \
                --admin-username $username \
                --admin-password $password

5. Luo kaksi tietojen levyä kunkin AM ja lopettaa kanssa tapahtuu `done` komento.

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-1.vhd \
                --size-in-gb $diskSize \
                --lun 0

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-2.vhd \
                --size-in-gb $diskSize \
                --lun 1
        done


### <a name="step-4---run-the-script"></a>Vaihe 4 - komentosarjan suorittaminen

Ladattu ja muuttaa käyttötarkoitukseen komentosarja, useita NIC tietokannan VMs luominen uudelleen suorittamalla.

1. Tallenna komentosarja ja pitäminen **Bash** päätteen. Alkuperäinen tulos näkyy alla kuvatulla tavalla.

        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up the availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up the network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up the network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB1"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB1-DA"
        info:    Looking up the NIC "NICDB1-RA"
        info:    Creating VM "DB1"

2. Muutaman minuutin kuluttua suorittamisen päättyy ja muiden tulos tulee näkyviin alla kuvatulla tavalla.

        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up the network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up the network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB2"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB2-DA"
        info:    Looking up the NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
