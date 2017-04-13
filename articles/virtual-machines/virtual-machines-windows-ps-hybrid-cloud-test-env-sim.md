<properties 
    pageTitle="Simuloitu hybrid cloud testiympäristössä | Microsoft Azure" 
    description="Luo Simuloitu cloud yhdistelmäympäristön, IT pro tai testausta kehitystä kaksi Azure virtual verkkojen ja VNet VNet yhteyden." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Simuloitu cloud yhdistelmäympäristön testikäyttöön määrittäminen

Tässä artikkelissa opastaa Simuloitu cloud yhdistelmäympäristön luominen Microsoft Azure käyttämällä kahta Azure virtual verkkoja. Seuraavassa on tuloksena määritykset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Tämä varmistaminen cloud tuotannon yhdistelmäympäristön ja koostuu:

- Simuloitu ja yksinkertaistettu paikallinen verkko ylläpidettävä Azure virtual verkon (TestLab virtual verkon).
- Simuloitu rajat paikallisen virtual verkko ylläpidettävä Azure (TestVNET).
- VNet VNet yhteys kahden virtual verkon välillä.
- Toissijainen toimialueen ohjauskoneen TestVNET virtual verkossa.

Tämä on perusta ja yleisiä käynnistäminen arvopisteiden voit:

- Kehittää ja testaa sovellusten Simuloitu cloud yhdistelmäympäristössä.
- Luo Testaa määritykset tietokoneiden, jotkin TestLab virtual verkoston ja jotkin TestVNET virtual verkossa, jotta saadaan aikaan hybrid pilvipohjainen IT-toiminnoista.

Neljä tärkeimmät vaiheet hybrid tämän cloud testiympäristössä määrittämisessä on:

1.  Määritä TestLab virtual verkkoon.
2.  Luoda paikallisen-virtual verkkoon.
3.  VNet VNet VPN-yhteyden luominen
4.  Määritä DC2. 

Tämä kokoonpano edellyttää Azure tilauksen. Jos sinulla on MSDN- tai Visual Studio-tilaus, katso [kuukausittain Azure luottotietojen Visual Studio-tilaajille](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Näennäiskoneiden ja VPN-yhdyskäytäviä, Azure maksamaan jatkuvaa raha kustannukset, kun ne ovat käytössä. Kustannus on vastaan oman MSDN laskutettu tai maksetun tilauksen. Azure VPN-yhdyskäytävän on toteutettu kaksi Azuren näennäiskoneiden sarjana. Pienennä kustannukset, Luo testiympäristössä ja suorittaa tarvittavat testaus ja esittelyn mahdollisimman pian.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Vaihe 1: Määritä TestLab virtual verkko

Määrittää DC1, APP1 ja CLIENT1 tietokoneet Azure virtual verkon nimeltä TestLab [Base määritysten testiympäristössä](https://technet.microsoft.com/library/mt771177.aspx) artikkelissa ohjeita avulla. 

Aloita seuraavaksi Azure PowerShell-kehotteessa.

> [AZURE.NOTE] Seuraava komento määrittää Käytä Azure PowerShell 1.0 ja uudempi versio.

Kirjaudu tiliisi.

    Login-AzureRMAccount

Pyydä käyttämällä seuraavaa komentoa tilauksen nimi.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Määritä Azure tilauksen. Käytä samaa tilaus, jota käytit luonnissa perus kokoonpanosi vaiheen 1. Korvaa kaiken tarjoukset, mukaan lukien < ja > merkkejä, oikea nimi.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Lisää seuraavaksi yhdyskäytävän aliverkon perus kokoonpanosi, jota käytetään isännöimiseen Azure yhdyskäytävän TestLab virtual verkostoon.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Pyydä TestLab virtual verkon yhdyskäytävän liittäminen julkinen IP-osoite.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Luo seuraavaksi, että yhdyskäytävän.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Pidä mielessä, että uusia yhdyskäytäviä voi kestää 20 minuutin ajan tai Lisää luomiseen.

Paikallisessa tietokoneessa Azure portaalista muodostaa DC1 CORP\User1 tunnuksilla. Määrittäminen CORP toimialueen siten, että tietokoneet ja käyttäjät käyttävät niiden paikallisen toimialueen ohjauskoneen todennusta varten, Suorita nämä komennot järjestelmänvalvojan Windows PowerShellin komentoriviltä DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Vaihe 2: Luo TestVNET virtual verkko

Luo TestVNET virtual verkon ja suojaaminen verkon käyttöoikeusryhmän.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Pyydä kohdentaa yhdyskäytävän TestVNET virtual verkossa ja luoda oman yhdyskäytävän julkiseen IP-osoite.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Vaihe 3: VNet VNet-yhteyden luominen

Hanki ensin satunnaisia, salaustavalla vahva, 32-merkkinen valmiiksi jaetun avaimen verkko- tai järjestelmänvalvoja. Vaihtoehtoisesti käyttää tietoja, [Luo jaettu IP-näppäimen satunnainen merkkijono](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) hankkiminen jaettua avainta.

Seuraavaksi Luo VNet VNet VPN-yhteys, jota voi kestää jonkin aikaa, voit suorittaa nämä komennot avulla.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Muutaman minuutin kuluttua on muodostettava yhteys.

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Vaihe 4: Määritä DC2

Luo ensin virtual machine DC2. Suorita nämä komennot PowerShellin Azure komentokehotteen paikalliseen tietokoneeseen.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Seuraavaksi muodostaa uusi DC2 virtuaalikoneen Azure-portaalista.

Määritä seuraavaksi Windowsin palomuuri-sääntö, joka sallii liikenteen yhteys testikäyttöön. Järjestelmänvalvojan Windows PowerShellin komentoriviltä-DC2 Suorita seuraavat komennot.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Ping-komento on johtaa neljä IP-osoite 10.0.0.4 onnistuneen vastaukset. Tämä on testi liikenteen VNet VNet-yhteyden kautta.

Lisää lisätiedot-levyn seuraavaksi DC2-kirjain f uusi merkkiin.

1.  Valitse vasemmanpuoleisessa ruudussa palvelimen hallinnan valitsemalla **tiedosto ja tallennustilaa palvelut**ja valitse sitten **levyjen**.
2.  Valitse sisällysruudun **levyjen** -ryhmästä **levyn 2** (ja **osion** arvoksi **Tuntematon**).
3.  Valitse **tehtävät**ja valitse sitten **Uusi asema**.
4.  Valitse ennen aloittaa uuden aseman ohjatun toiminnon sivulla, valitse **Seuraava**.
5.  Valitse Valitse palvelin ja levy-sivulla **levyn 2**ja valitse sitten **Seuraava**. Kun sinulta kysytään, valitse **OK**.
6.  Valitse Määritä aseman sivun koon Valitse **Seuraava**.
7.  Tee määrittäminen aseman kirjain tai kansion sivulle Valitse **Seuraava**.
8.  Valitse Valitse file system asetukset-sivulla **Seuraava**.
9.  Valitse Vahvista asetukset-sivun **luominen**.
10. Kun olet valmis, valitse **Sulje**.

Määritä seuraavaksi DC2 corp.contoso.com toimialueen ohjauskoneen replikan. Suorita nämä komennot Windows PowerShell-komentokehotteen DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Huomaa, että sinua kehotetaan toimittamaan CORP\User1 salasana ja Directory Services Palauta tila (DSRM) salasanan ja käynnistämään DC2.

Nyt kun TestVNET virtual verkko on omassa DNS server (DC2) helpottaa, sinun on määritettävä TestVNET virtual verkon käyttämään DNS-palvelin.

1.  Azure-portaalin vasemmanpuoleisessa ruudussa virtual verkot-kuvaketta ja valitse sitten **TestVNET**.
2.  **Asetukset** -välilehdessä **DNS-palvelimet**.
3.  Kirjoita **ensisijainen DNS-palvelin**, voit korvata 10.0.0.4 **192.168.0.4** .
4.  Valitse **Tallenna**.

Tämä on nykyiset. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Simuloitu hybrid cloud-ympäristössä on nyt valmis testikäyttöön.

## <a name="next-step"></a>Seuraava vaihe

- Määritä tässä ympäristössä [liiketoimintasovelluksessa verkkopohjaisia-riville](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) .
