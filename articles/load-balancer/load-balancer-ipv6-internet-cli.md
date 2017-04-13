<properties
    pageTitle="Luo vastakkaisten kuormituksen kanssa IPv6-Azure Resurssienhallinta käyttämällä Azure-CLI Internet | Microsoft Azure"
    description="Opettele luomaan selainpohjainen vastakkaisten kuormituksen IPv6-Azure Resurssienhallinta käyttämällä Azure-CLI kanssa"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6: ta, azure kuormituksen, kahden pinon, julkiseen ip, alkuperäisen ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Luo Internet vastakkaisten kuormituksen IPv6-Azure Resurssienhallinta käyttämällä Azure-CLI kanssa

> [AZURE.SELECTOR]
- [PowerShellin](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Malli](./load-balancer-ipv6-internet-template.md)

Azure kuormituksen on kerros-4 (TCP- ja UDP) kuormituksen. Kuormituksen on suuri käytettävyys jakamalla saapuvan liikenteen kunnossa palveluesiintymiä cloud Services-palveluissa tai näennäiskoneiden kuormituksen tasauspalvelun joukon. Azure kuormituksen myös näyttää useita portit, useita IP-osoitteita tai molempia näistä palveluista.

## <a name="example-deployment-scenario"></a>Käyttöönotto-Esimerkkitapaus

Seuraavassa kaaviossa on kuvattu ratkaisu kuormituksen otetaan käyttöön, joka on kuvattu tämän artikkelin Esimerkki mallin avulla.

![Kuormituksen tasauspalvelun-skenaario](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Tässä skenaariossa luot Azure seuraavissa resursseissa:

- kaksi näennäiskoneiden (VMs)
- VPN-liittymän, kunkin AM määritetty IPv4- ja IPv6-osoitteet
- Internetiin yhteydessä oleva-kuormituksen IPv4 ja IPv6 julkiseen IP-osoite
- Käytettävyyden asettaminen, joka sisältää kaksi VMs
- kaksi ladata julkisen VIPs yhdistäminen yksityinen päätepisteet Vastatilin säännöt

## <a name="deploying-the-solution-using-the-azure-cli"></a>Käyttämällä Azure-CLI-ratkaisun käyttöönotto

Seuraavissa vaiheissa kuvataan luomisesta vastakkaisten Azure Resurssienhallinta käyttäminen CLI kuormituksen Internet. Azure resurssien hallinnan avulla kullekin resurssille on luotu ja määritetty erikseen ja ladata yhteen, kun haluat luoda resurssin.

Ottaa käyttöön kuormituksen luominen ja määrittäminen seuraaville objekteille:

- IP-määritys edusta - sisältää julkisten IP-osoitteiden saapuvaa verkkoliikennettä.
- Taustatietokannan osoiteryhmä - sisältää verkkoliittymät (NIC) vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden.
- Kuormituksen tasaamisen säännöt - sisältää sääntöjä yhdistäminen julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon-porttiin.
- Saapuvan liikenteen säännöt NAT - sisältää yhdistäminen julkisen porttiin kuormituksen tiettyä virtual konetta taustatietokantaan osoite varannon portin säännöt.
- Probes - sisältää kunto keräysputkien käytettävä näennäiskoneiden esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen.

Lisätietoja on artikkelissa [Azure Resurssienhallinta kuormituksen tuki](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Määritä CLI ympäristön käyttämään Azure resurssien hallinta

Tässä esimerkissä on on käynnissä CLI Työkalut PowerShell-komentoikkunassa. Emme Käytä Azure PowerShellin cmdlet-komennot, mutta Käytämme käyttäjän PowerShell komentosarjaominaisuuksien parantaa luettavuutta ja uudelleenkäyttöön.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Suorita Resurssienhallinta tilaan siirtyminen **azure config tila** -komento.

        azure config mode arm

    Odotettu tulos:

        info:    New mode is arm

3. Kirjaudu Azure ja tilaukset luettelo.

        azure login

    Kirjoita Azure tunnistetiedot pyydettäessä.

        azure account list

    Valitse tilaus, jota haluat käyttää. Kirjoita muistiin tilauksen tunnus, siirry seuraavaan vaiheeseen.

4. Määritä PowerShell muuttujat CLI komentojen käyttöä varten.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Resurssiryhmä, kuormituksen tasauspalvelun, virtual verkon ja aliverkosta luominen

1. Resurssiryhmän luominen

        azure group create $rgName $location

2. Luo kuormituksen tasaus

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Luo virtuaalisia verkon (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Luo kaksi aliverkosta tämän VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Julkisten IP-osoitteiden edusta resurssivarannon luominen

1. PowerShell-muuttujien määrittäminen

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Luo julkinen IP-osoite edusta IP-ryhmän.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Kuormituksen käyttää toimialueen nimi, julkiseen IP sen FQDN. Tämä perinteinen käyttöönotosta, joka käyttää pilvipalvelussa muuta nimi kuin kuormituksen täydellinen toimialuenimi.
    >Tässä esimerkissä FQDN on *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Luoda edusta- ja

Tässä esimerkissä Luo edusta IP-ryhmän, joka vastaanottaa saapuvia verkkoliikenteen kuormituksen ja jossa edusta resurssivarantoon lähettää kuormitus tasataan verkkoliikennettä taustatietokantaan IP-ryhmän.

1. PowerShell-muuttujien määrittäminen

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Avautuvassa luotu edellisessä vaiheessa ja kuormituksen julkiseen IP edusta IP-ryhmän luominen

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Näytteenottimen, NAT säännöt ja kg sääntöjen luominen

Tässä esimerkissä luo seuraavat kohteet:

- Tarkista yhteys porttinumeroa 80 näytteenottimen säännön
- NAT sääntö, joka kääntää kaikki saapuvan liikenteen porttia 3389 porttiin 3389 RDP<sup>1</sup>
- NAT sääntö, joka kääntää kaikki saapuvan liikenteen porttiin 3391 porttiin 3389 RDP<sup>1</sup>
- kuormituksen tasauspalvelun sääntö, joka täsmää kaikki saapuvan liikenteen porttiin 80 taustatietokantaan resurssivarantoon osoitteet 80 porttiin.

<sup>1</sup> NAT säännöt on liitetty tiettyyn virtuaalikoneen esiintymään kuormituksen takana. Portti 3389 saapuvat verkkoliikennettä lähetetään tietyn virtuaalikoneen ja portin liittyvät NAT-säännön. Määritä protocol (UDP tai TCP) NAT-säännön. Molemmat ei voi määrittää samaa porttia.

1. PowerShell-muuttujat määrittäminen

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Luo näytteenottimen

    Seuraavassa esimerkissä luodaan TCP Räjähdysnopeuden, joka tarkistaa taustatietokantaan porttinumeroa 80 yhteys 15 sekunnin välein. Se merkitsee taustatietokantaan resurssin ei käytettävissä kaksi peräkkäistä virheen jälkeen.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Luo saapuvan NAT säännöt, jotka sallivat RDP taustatietokantaan-resurssit

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Luo kuormituksen tasauspalvelun säännöt, jotka liikenne lähettää eri taustatietokantaan portit sen mukaan, mitä edusta saanut pyynnön

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Asetusten tarkistaminen

        azure network lb show --resource-group $rgName --name $lbName

    Odotettu tulos:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Luo NIC

Luo NIC ja liittää ne NAT sääntöjä, kuormituksen tasauspalvelun säännöt ja näytteenottimet.

1. PowerShell-muuttujien määrittäminen

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Luo NIC kunkin taustatietokannan, ja lisää IPv6-määritys.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Luo taustatietokantaan AM resurssit ja liitä kunkin NIC: ssä

Jos haluat luoda VMs, on oltava tallennustilan tilin. VMs kuormituksen tasaamisen, on käytettävyys määrittäminen jäseniä. Katso lisätietoja luomisesta VMs [luominen Azure-AM PowerShellin avulla](../virtual-machines/virtual-machines-windows-ps-create.md)

1. PowerShell-muuttujien määrittäminen

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] Tässä esimerkissä käytetään tekstimuodossa VMs käyttäjänimi ja salasana. Tarvittava hoito on otettava käyttämällä tunnistetiedot, valitse Poista. Katso turvallisempi keinot käsittely PowerShell tunnistetiedot, [Hae tunnistetiedon](https://technet.microsoft.com/library/hh849815.aspx) cmdlet-komento.

2. Tallennustilan tilin ja käytettävyyden joukon luominen

    Käytössä olevan tallennustilan tilin voi käyttää, kun luot VMs. Seuraava komento luo uusi tallennustilan tili.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Seuraavaksi luodaan käytettävyys määrittäminen.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Luo näennäiskoneiden liitetty NIC

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-cli.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
