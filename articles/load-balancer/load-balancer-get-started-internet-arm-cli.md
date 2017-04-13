<properties
   pageTitle="Luo vastakkaisten kuormituksen resurssien hallinta käyttämällä Azure-CLI Internet | Microsoft Azure"
   description="Opettele luomaan vastakkaisten kuormituksen resurssien hallinta käyttämällä Azure-CLI Internet"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internal-load-balancer-using-the-azure-cli"></a>Azure-CLI käyttämällä sisäinen kuormituksen luominen

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [vastakkaisten kuormituksen käyttämällä perinteinen käyttöönotto Internet luominen](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Käyttämällä Azure-CLI-ratkaisun käyttöönotto

Seuraavissa vaiheissa kuvataan luomisesta vastakkaisten Azure Resurssienhallinta käyttäminen CLI kuormituksen Internet. Azure resurssien hallinnan kullekin resurssille on luotu ja määritetty yksitellen, valitse hyllytetty yhteen, kun haluat luoda resurssin.

Luo ja seuraaville objekteille ottamaan kuormituksen tasauspalvelun asetusten määrittäminen:

- IP-määritys edusta - sisältää julkisten IP-osoitteiden saapuvaa verkkoliikennettä.
- Taustatietokannan osoiteryhmä - sisältää verkkoliittymät (NIC) vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden.
- Kuormituksen tasaamisen säännöt - sisältää sääntöjä yhdistäminen julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon-porttiin.
- Saapuvan liikenteen säännöt NAT - sisältää yhdistäminen julkisen porttiin kuormituksen tiettyä virtual konetta taustatietokantaan osoite varannon portin säännöt.
- Probes - sisältää kunto keräysputkien käytettävä näennäiskoneiden esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen.

Katso lisätietoja [Azure Resurssienhallinta kuormituksen tuki](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Määritä CLI käyttämään resurssien hallinta

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Odotettu tulos:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Luo virtuaalisia verkko- ja edusta IP-ryhmän julkiseen IP-osoite

1. Luo nimetty *NRPVnet* käyttämällä nimeltä *NRPRG*resurssiryhmä Yhdysvaltojen Itä sijainnissa virtual verkko (VNet).

        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16

    Luo nimetty *NRPVnetSubnet* CIDR palkilla, 10.0.0.0/24 *NRPVnet*aliverkon.

        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24

2. Luo nimetty *NRPPublicIP* käytettävän edusta IP-ryhmän kanssa DNS nimi *loadbalancernrp.eastus.cloudapp.azure.com*julkinen IP-osoite. Alla oleva komento käyttää staattinen kohdistustyyppi ja aikakatkaisun 4 minuuttia.

        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4

    >[AZURE.IMPORTANT]Kuormituksen käyttää toimialueen nimi, julkiseen IP sen FQDN. Tämä perinteinen käyttöönotosta, joka käyttää pilveen siirtyminen palveluun kuin kuormituksen täysin täydellinen toimialuenimi (FQDN).
    >Tässä esimerkissä FQDN on *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Luo kuormituksen tasaus

Seuraava komento luo kuormituksen, *NRPlb* *NRPRG* resurssiryhmä *Yhdysvaltojen Itä* -nimisen Azure sijainti.

    azure network lb create NRPRG NRPlb eastus

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Luo edusta IP-ryhmän ja Taustajärjestelmä osoite resurssivarantoon

Tässä esimerkissä näytetään, joka vastaanottaa saapuvia verkkoliikenteen kuormituksen edusta IP-ryhmän ja jossa edusta resurssivarantoon lähettää kuormitus tasataan verkkoliikennettä Taustajärjestelmä IP-ryhmän luomisesta.

1. Avautuvassa luotu edellisessä vaiheessa ja kuormituksen julkiseen IP edusta IP-ryhmän luominen

        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip

2. Määritä saapuvan liikenteen vastaanottaa edusta IP-varannon taustatietokantaan osoite resurssivarantoon.

        azure network lb address-pool create NRPRG NRPlb NRPbackendpool

## <a name="create-lb-rules-nat-rules-and-probe"></a>KG sääntöjä, NAT säännöt ja näytteenottimen luominen

Tässä esimerkissä luodaan seuraavat kohteet.

- NAT sääntö, joka kääntää kaikki saapuvan liikenteen porttiin 21 portin 22<sup>1</sup>
- NAT sääntö, joka kääntää kaikki saapuvan liikenteen porttiin 23 porttiin 22
- kuormituksen tasauspalvelun sääntö, joka täsmää kaikki saapuvan liikenteen porttiin 80 taustatietokantaan resurssivarantoon osoitteet 80 porttiin.
- kuntotietojen tarkistaminen sivulla näytteenottimen säännön nimi *HealthProbe.aspx*.

<sup>1</sup> NAT säännöt on liitetty tiettyyn virtuaalikoneen esiintymään kuormituksen takana. Tiettyä virtual konetta porttiin 22 NAT sääntöön liitetyn lähetetään saapuvat portti 21 verkkoliikennettä. Määritä protocol (UDP tai TCP) NAT-säännön. Molemmat ei voi määrittää samaa porttia.

1. Luo NAT-sääntöjä.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22

2. Kuormituksen tasauspalvelun säännön luominen

        azure network lb rule create nrprg nrplb lbrule -p tcp -f 80 -b 80 -t NRPfrontendpool -o NRPbackendpool

3. Luo kunto näytteenottimen.

        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4

4. Tarkista asetukset.

        azure network lb show nrprg nrplb

    Odotettu tulos:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Luo NIC

On luotava NIC (tai muokata aiemmin luotuja) ja liitä ne NAT sääntöjä, kuormituksen tasauspalvelun säännöt ja näytteenottimet.

1. Luo NIC, jonka nimi on *nic1 kg on*, ja liittää sen *rdp1* NAT-sääntö ja *NRPbackendpool* taustatietokantaan osoite resurssivarantoon.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus

    Odotettu tulos:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Luo NIC, jonka nimi on *nic2 kg on*, ja liittää sen *rdp2* NAT-sääntö ja *NRPbackendpool* taustatietokantaan osoite resurssivarantoon.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus

3. Luo virtual machine (AM), joka on nimetty *web1*ja liittää sen nimeltä NIC *nic1 kg voidaan*. Kutsua *web1nrp* tallennustilan-tili on luotu ennen-komennon alla.

        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Kuormituksen tasauspalvelun VMs on oltava sama käytettävyys määrittäminen. Käytä `azure availset create` luominen saatavuus.

    Tulosteen pitäisi näyttää seuraavankaltaiselta:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    >[AZURE.NOTE] **Tämä on määritetty publicIP ilman NIC** sanoman odotetaan luoda yhteyden Internetiin, kuormituksen tasauspalvelun julkiseen IP-osoitetta kuormituksen NIC jälkeen.

    Koska *nic1 kg voidaan* NIC liittyy *rdp1* NAT sääntöä, voit muodostaa yhteyden *web1* käyttämällä RDP kuormituksen 3441 porttiin kautta.

4. Luo virtual machine (AM), joka on nimetty *web2*ja liittää sen nimeltä NIC *nic2 kg voidaan*. Kutsua *web1nrp* tallennustilan-tili on luotu ennen-komennon alla.

        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="update-an-existing-load-balancer"></a>Päivitä aiemmin kuormituksen

Voit lisätä aiemmin kuormituksen viittaava säännöt. Seuraavassa esimerkissä uusi kuormituksen tasauspalvelun sääntö on lisätty aiemmin kuormituksen **NRPlb**

    azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool

## <a name="delete-a-load-balancer"></a>Poista kuormituksen tasaus

Seuraavalla komennolla voit poistaa kuormituksen:

    azure network lb delete --resource-group nrprg --name nrplb

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-cli.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
