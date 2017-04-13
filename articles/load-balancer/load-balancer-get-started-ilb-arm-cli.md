<properties
   pageTitle="Luo sisäinen kuormituksen Azure-CLI resurssien hallinnan avulla | Microsoft Azure"
   description="Opettele luomaan sisäinen kuormituksen Azure-CLI resurssien hallinnan avulla"
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

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Luo sisäinen kuormituksen Azure-CLI avulla

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](load-balancer-get-started-ilb-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Ratkaisun käyttöönotto Azure-CLI avulla

Seuraavissa vaiheissa kuvataan Internetiin yhteydessä oleva-kuormituksen luominen käyttämällä CLI Azure Resurssienhallinta. Azure resurssien hallinnan avulla kullekin resurssille on luotu ja määritetty erikseen ja ladata yhteen, kun haluat luoda resurssin.

Haluat luoda ja määrittää kuormituksen tasauspalvelun ottamaan seuraaville objekteille:

- **FEP IP-asetuksia**: sisältää saapuvaa verkkoliikennettä julkiseen IP-osoitteet
- **Taustatietokannan osoite resurssivarantoon**: verkkoliittymät (NIC), joiden avulla voit vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden on
- **Kuormituksen tasaamisen sääntöjä**: sisältää sääntöjä, jotka vastaavat julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon portti
- **NAT saapuvan liikenteen säännöt**: sisältää sääntöjä, jotka vastaavat yleisen porttiin kuormituksen tiettyä virtual konetta taustatietokantaan osoite varannon portti
- **Probes**: sisältää kunto keräysputkien, joita käytetään näennäiskoneiden esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen

Lisätietoja on artikkelissa [Azure Resurssienhallinta kuormituksen tuki](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Määritä CLI käyttämään resurssien hallinta

1. Jos et ole aikaisemmin käyttänyt Azure CLI, katso lisätietoja artikkelista [asentaminen ja määrittäminen Azure-CLI](../../articles/xplat-cli-install.md). Noudata kohtaan, jossa valitaan Azure-tili ja tilauksen.

2. Suorita **azure config tila** -komento Vaihda Resurssienhallinta tilaan seuraavasti:

        azure config mode arm

    Odotettu tulos:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Luo sisäinen kuormituksen vaihe vaiheelta

1. Azure kirjautuminen.

        azure login

    Kirjoita pyydettäessä Azure tunnistetiedot.

2. Muuta komennon Työkalut Azure Resurssienhallinta-tilassa.

        azure config mode arm

## <a name="create-a-resource-group"></a>Resurssiryhmän luominen

Kaikki resurssit Azure Resurssienhallinta liittyy resurssiryhmä. Jos et ole antanut sitä vielä ole, Luo resurssiryhmä.

    azure group create <resource group name> <location>

## <a name="create-an-internal-load-balancer-set"></a>Luo sisäinen kuormituksen tasauspalvelun määrittää siihen

1. Luo sisäinen kuormituksen

    Seuraavassa tilanteessa nimeltä nrprg resurssiryhmä luodaan Yhdysvaltojen Itä-alueella.

        azure network lb create --name nrprg --location eastus

    >[AZURE.NOTE] Sisäinen kuormituksen tasoitusmääritykset, kuten virtual verkkojen ja VPN-aliverkosta, kaikki resurssit on oltava sama resurssiryhmän ja samalla alueella.

2. Luo sisäinen kuormituksen edusta IP-osoite.

    IP-osoite, jota käytetään on oltava virtual verkon aliverkon-alueella.

        azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet

3. Taustatietokannan osoiteryhmän luominen

        azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb

    Kun olet määrittänyt edusta IP-osoite ja taustatietokantaan osoite resurssivarantoon, voit luoda kuormituksen tasauspalvelun sääntöjä saapuvien NAT säännöt ja mukautettu kunto probes.

4. Sisäinen kuormituksen kuormituksen tasauspalvelun säännön luominen

    Kun olet suorittanut kaikki vaiheet, komento luo porttia 1433 edusta varannon kuunteleminen ja lähettää kuormituksen verkkoliikennettä taustatietokantaan osoite-ryhmään, myös käyttämällä porttia 1433 kuormituksen säännön.

        azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb

5. Luo NAT saapuvan liikenteen säännöt.

    Saapuvan NAT sääntöjä käytetään päätepisteet luominen kuormituksen, siirry tiettyyn virtuaalikoneen esiintymä. Kaksi NAT sääntöjä etätyöpöydän edelliset vaiheet.

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389

6. Luo kunto keräysputkien kuormituksen varten.

    Kuntotietojen näytteenottimen tarkistaa kaikki virtuaalikoneen esiintymät, varmista, että he voivat lähettää verkkoliikennettä. Virtuaalikoneen-esiintymää epäonnistui näytteenottimen tarkistukset poistetaan kuormituksen, kunnes se online-tilaan näytteenottimen valinta määrittää, että se on kunnossa.

        azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4

    >[AZURE.NOTE]Microsoft Azure-ympäristön käyttää järjestelmänvalvojan skenaarioita staattinen julkisesti reititettävä IPv4-osoite. IP-osoite on 168.63.129.16. IP-osoitteen estänyt minkä tahansa palomuurit ei olisi, koska tämä aiheuttaa odottamatonta toimintaa.
    >Azure sisäinen kuormituksen tasaamisen, jotka koskevat IP-osoitteen käytetään seuranta-kuormituksen keräysputkien määrittämään kuntotietojen näennäiskoneiden kuormituksen määrittäminen. Jos verkon käyttöoikeusryhmän rajoitetaan liikenne Azuren näennäiskoneiden sisäisesti kuormituksen saapuva määrittäminen tai käytetään VPN-aliverkon, varmistaa, että verkon suojaussäännön lisätään sallimaan tietoliikenteen 168.63.129.16.

## <a name="create-nics"></a>Luo NIC

On luotava NIC (tai muokata aiemmin luotuja) ja liitä ne NAT sääntöjä, kuormituksen tasauspalvelun säännöt ja näytteenottimet.

1. Luo-niminen NIC *nic1 kg voidaan*, ja liittää sen *rdp1* NAT sääntö ja *beilb* taustatietokantaan osoite resurssivarantoon.

        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus

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

2. Luo-niminen NIC *nic2 kg voidaan*, ja liittää sen *rdp2* NAT sääntö ja *beilb* taustatietokantaan osoite resurssivarantoon.

        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus

3. Luo *MJP1*-nimisen virtual-koneen ja liittää nimeltä NIC *nic1 kg voidaan*. Kutsua *web1nrp* tallennustilan-tili on luotu ennen suoritetaan seuraava komento:

        azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

    >[AZURE.IMPORTANT] Kuormituksen tasauspalvelun VMs on oltava sama käytettävyys määrittäminen. Käytä `azure availset create` luominen saatavuus.

4. Luo virtual machine (AM), joka on nimetty *DB2*ja liittää nimeltä NIC *nic2 kg voidaan*. Kutsua *web1nrp* tallennustilan-tili on luotu ennen kuin suoritat seuraavan komennon.

        azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825

## <a name="delete-a-load-balancer"></a>Poista kuormituksen tasaus

Jos haluat poistaa kuormituksen, käytä seuraavaa komentoa:

    azure network lb delete --resource-group nrprg --name ilbset

## <a name="next-steps"></a>Seuraavat vaiheet

[Määrittää kuormituksen tasauspalvelun jakauman tilan käyttämällä lähde-IP-affiniteetti](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
