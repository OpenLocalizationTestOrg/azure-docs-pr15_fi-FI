<properties
   pageTitle="Luo AM useita NIC"
   description="Lue, miten voit luoda ja määrittää vms useita NIC kanssa"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Luo AM useita NIC

Voit luoda näennäiskoneiden (VMs) Azure ja liitä kunkin käyttäjän VMs useita verkkoliittymät (NIC). Usean NIC vaaditaan monta verkon virtual laitteita, kuten sovelluksen toimitus- ja WAN optimointimahdollisuudet. Usean NIC sisältää myös useita verkon liikenteen hallinta-toiminnon, mukaan lukien liikenne välillä eteen eristystaso lopettaa NIC ja takaisin end verkkokorttia tai tasossa tietoliikennettä erottaminen tasossa liikenteen hallinta.

![Usean NIC AM varten](./media/virtual-networks-multiple-nics/IC757773.png)

Yllä kuvassa on kolme NIC kanssa AM kunkin yhteydessä eri aliverkon.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Internetiin yhteydessä oleva VIP (perinteinen ominaisuuksissa) tuetaan vain "oletus" NIC-sivustosta. On vain yksi VIP, IP, oletusarvo NIC-sivustosta.
- Tällä hetkellä esiintymän tason julkiseen IP (LPIP)-osoitteet (perinteinen ominaisuuksissa) ei tueta usean NIC VMs.
- NIC sisällä AM järjestyksen satunnaisesti ja voi muuttua myös Azure infrastruktuurin päivitysten. Kuitenkin IP-osoitteet ja vastaavan ethernet MAC-osoitteet on säilyvät ennallaan. Oletetaan, että **Eth1** on IP-osoite 10.1.0.100 ja MAC-osoite 00-0D-3A-B0-39-0D; jälkeen Azure infrastruktuurin update ja Käynnistä tietokone uudelleen se saattaa muuttua **Eth2**, mutta IP ja MAC laiteparin ne ovat samat. Jos uudelleenkäynnistys on asiakkaan käynnistämä, NIC-tilauksen pysyvät ennallaan.
- Kunkin NIC kunkin AM-osoite on sijaittava aliverkon, valitse yksittäinen AM useita NIC kunkin voi määrittää osoitteet, jotka ovat samassa aliverkon.
- AM koko määrää, voit luoda AM NIC määrä. Viittaus [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) ja [Linux](../virtual-machines/virtual-machines-linux-sizes.md) AM koot artikkeleissa voit määrittää, kuinka monta NIC jokaisen AM koon tukee. 

## <a name="network-security-groups-nsgs"></a>Verkon käyttöoikeusryhmät (NSGs)
Resurssien hallinnan käyttöönotto minkä tahansa NIC-AM voi olla liittyvät kanssa verkon suojauksen ryhmän (NSG), mukaan lukien kaikki NIC-AM, jossa on käytössä useita NIC. Jos NIC: ssä on määritetty sisällä Jos aliverkon liittyy NSG aliverkon osoite, valitse säännöt aliverkon NSG sovelletaan myös kyseisen NIC-sivustosta. Lisäksi liittäminen NSGs aliverkosta, voit myös yhdistää NIC NSG.

Jos aliverkon liittyy NSG ja NIC kyseisen aliverkon sisällä on liitetty yksitellen NSG, liittyvät NSG sääntöjä käytetään **työnkulku tilauksen** välitettävä sisään tai ulos Verkkokortti liikenteen suunnan mukaan:

- **Saapuvan liikenteen** , jonka kohde on kyseisen NIC kulkee ensin aliverkon, käynnistävä aliverkon NSG säännöt ennen kulkeva Verkkokortti kyselyjä ja valitse käynnistävä NIC NSG säännöt.
- **Lähtevän tietoliikenteen** , jonka tietolähde on kyseisen NIC jatkuu ensin ulos NIC käynnistävä NIC-NSG ennen aliverkon läpi ja valitse käynnistävä aliverkon NSG säännöt säännöt.

Lisätietoja [Verkko-käyttöoikeusryhmät](virtual-networks-nsg.md) ja miten niitä käytetään perustuvat suhteet aliverkosta, VMs ja NIC...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Usean NIC AM määrittämisestä perinteinen käyttöönotto

Alla olevien ohjeiden avulla voit luoda usean NIC AM, joka sisältää 3 NIC: oletusallekirjoitusta NIC ja kaksi muita NIC. Määritysvaiheet Luo AM, joka on määritetty mukaan-palvelun määrittäminen tiedoston osa alla:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Tarvitset seuraavat edellytykset ennen kuin yrität suorittaa esimerkissä PowerShell-komennoilla.

- Azure tilaus.
- Määritetty virtual verkkoon. Saat lisätietoja VNets [Virtual yleistä](virtual-networks-overview.md) .
- PowerShellin Azure uusimman version ladataan ja asennetaan. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

Luomiseen AM useita NIC, noudata seuraavia ohjeita:

1. Valitse Azure AM kuvavalikoima AM kuva. Huomaa, että kuvat muuttuvat usein, ja ovat käytettävissä alueittain. Määritetty alla olevassa esimerkissä kuva voi muuttaa tai voi ei voidaan alueellasi, joten varmista, että määrität kuva, sinun on.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Luo AM asetukset.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Luo oletusarvoisen järjestelmänvalvojan käyttäjätunnus.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Lisää muita NIC työnkulkuun AM.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Määritä IP-osoite ja aliverkon oletusarvon NIC-sivustosta.

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Luo AM virtual verkossa.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] Tähän määrittämäsi VNet on oltava olemassa (kuten mainitaan edellytykset). Alla olevassa esimerkissä määrittää virtual verkon nimeltä **MultiNIC VNet**.

## <a name="limitations"></a>Rajoitukset

Seuraavat rajoitukset ovat käytettävissä, kun usean NIC-toiminnon avulla:

- Usean NIC VMs on luotava Azure virtual verkkojen (VNets). Muut VNet VMs ei voi määrittää usean NIC kanssa.
- Kaikki VMs käytettävyys joukossa on käytettävä joko usean NIC tai yksittäisen NIC-sivustosta. Ei voi olla usean NIC VMs ja yksittäisen NIC VMs käytettävyys asettaa. Samoja sääntöjä hakea VMs pilvipalvelussa.
- Yksittäisen NIC kanssa AM ei voi määrittää usean NIC (ja päinvastoin), kun se otetaan käyttöön, poistaminen ja sen luominen uudelleen.


## <a name="secondary-nics-access-to-other-subnets"></a>Toissijainen NIC pääsy muiden aliverkosta

Oletusarvon mukaan toissijainen NIC ei ole määritetty oletusarvon Gatewayn, joiden vuoksi toissijainen NIC liikenne-työnkulku on rajoitettu saman aliverkon rajoissa. Jos käyttäjien käyttöön toissijainen NIC keskustelemaan omia aliverkon ulkopuolella, tiedot on lisättävä reititys taulukossa voit määrittää yhdyskäytävän seuraavalla tavalla.

>[AZURE.NOTE] VMs ennen heinäkuussa 2015 voi olla kaikki NIC määritetty oletusarvoinen yhdyskäytävä. Toissijainen NIC oletusarvon Gatewayn ei poistetaan, kunnes nämä VMs on käynnistettävä. Internet-yhteys voit katkaista, jos tunkeutumisen ja ulospääsy-liikenne eri NIC, jotka käyttävät heikko host reititys mallin, kuten Linux-käyttöjärjestelmissä.

### <a name="configure-windows-vms"></a>Windowsin VMs määrittäminen

Oletetaan, että sinulla on Windows-AM, jossa on kaksi NIC seuraavasti:

- Ensisijainen NIC IP-osoite: 192.168.1.4
- Toissijainen NIC IP-osoite: 192.168.2.5

Tämä AM IPv4-reitin taulukko näyttää tältä:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Huomaa, että oletusarvoinen reitin (0.0.0.0) on käytettävissä ainoastaan ensisijainen NIC-sivustosta. Ei voi käyttää resurssien aliverkon ulkopuolella, toissijainen NIC tarkastelu alla:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Voit lisätä oletusarvon reitin toissijainen NIC-noudattamalla seuraavia ohjeita:

1. Komentokehote Suorita-komento alla, voi tunnistaa järjestysnumero toissijainen NIC:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Huomaa toinen merkintä indeksin 27 (Kirjoita tässä esimerkissä)-taulukkoon.
3. Suorita komentokehote- **reitin lisääminen** -komennon alla kuvatulla tavalla. Tässä esimerkissä on määrittäminen 192.168.2.1 kuin oletusarvoinen yhdyskäytävän toissijainen NIC varten:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Testaa yhteys, palaa komentokehote ja yritä ping eri aliverkon toissijainen NIC-muodossa näkyy int eh alla olevassa esimerkissä:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Voit myös tarkistaa reitin taulukossa Tarkista lisätyn reititys alla kuvatulla tavalla:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Määritä Linux VMs

Linux VMs koska oletustoiminnan heikko host reititys, suosittelemme, että toissijainen NIC on rajoitettu liikenne kulkee vain saman aliverkon sisällä. Kuitenkin jos aliverkon ulkopuolella connectivity tietyissä tilanteissa, käyttäjien olisi käytäntö on otettu käyttöön mukaan reititys varmistaa, että tunkeutumisen ja ulospääsy liikenne käyttää samaa NIC-sivustosta.

## <a name="next-steps"></a>Seuraavat vaiheet

- Ota käyttöön [MultiNIC VMs 2 tason sovelluksen tilanne Resurssienhallinta-yhdistelmäympäristössä](virtual-network-deploy-multinic-arm-template.md).
- Ota käyttöön [Perinteinen käyttöönoton 2 tason sovelluksen tilanne VMs MultiNIC](virtual-network-deploy-multinic-classic-ps.md).
