<properties
   pageTitle="Useita IP-osoitteita näennäiskoneiden - PowerShell | Microsoft Azure"
   description="Lue, miten voit määrittää useita IP-osoitteita virtual tietokoneeseen käyttämällä PowerShellin Azure."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Määrittää useita IP-osoitteita näennäiskoneiden

Azure virtuaalikoneen (AM) voi olla yksi tai useampi verkkoliittymät (NIC) liitetään. Minkä tahansa NIC: ssä voi olla vähintään yksi julkinen tai yksityinen IP-osoitteisiin siihen. Jos et ole tottunut Azure IP-osoitteet, lue lisätietoja niitä [IP-osoitteiden Azure](virtual-network-ip-addresses-overview-arm.md) -artikkelista. Tässä artikkelissa kerrotaan, miten voit määrittää useita IP-osoitteita AM Azure Resurssienhallinta käyttöönoton mallin Azure PowerShellin avulla.

Useita IP-osoitteita määritteleminen AM mahdollistaa seuraavia ominaisuuksia:

- Isännöidä useita sivustoja tai palvelun eri IP-osoitteet ja SSL-varmenteita yhteen palvelimeen.
- Yhteyshenkilönä, esimerkiksi palomuuri verkon virtual laitteen tai kuormituksen.
- Mahdollista lisätä jokin yksityisten IP-osoitteiden minkään NIC Azure kuormituksen-taustatietokannan ryhmään. Aikaisemmin vain ensisijainen IP-osoite ensisijainen NIC voitu lisätä taustatietokantaan resurssivarantoon.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Rekisteröitymään esikatseluun lähettää sähköpostia tilauksen tunnus [Useita IP -osoitteet](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) ja käyttötarkoitus.

## <a name="scenario"></a>Skenaario

Tässä artikkelissa liität kolme verkkoliittymän IP määrityksiä.
Seuraavassa esimerkissä käyttömahdollisuudet luodaan ja myönnetyt, joka on kolme yksityisten IP-osoitteiden ja yksi julkiseen IP-osoite sille NIC:

- IPConfig-1: Dynaaminen yksityisen IP-osoitteen (oletus) ja julkisen IP sähköpostiosoitteen resurssista julkiseen IP address nimeltä PIP1.
- IPConfig-2: Staattinen yksityinen IP-osoite ja julkiseen IP-osoitetta.
- IPConfig-3: Dynaaminen yksityinen IP-osoite ja julkiseen IP-osoitetta.

    ![Kuvan vaihtoehtoinen teksti](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Tässä tilanteessa oletetaan, että kutsutaan *RG1* , jonka sisällä on VNet, jota kutsutaan *VNet1* ja kutsua *Subnet1*aliverkon resurssiryhmä. Lisäksi se oletetaan, sinulla on AM, jota kutsutaan *VM1*, siihen liittyvä *VM1 NIC1* nimeltä verkon käyttöliittymä ja julkinen IP-osoite, jota kutsutaan *PIP1*.

[Tässä artikkelissa](./virtual-machines/virtual-machines-windows-ps-create.md ) käydään läpi luominen siltä varalta, että et ole luonut ne ennen edellä mainituista resurssit.

## <a name = "create"></a>Luo AM useita IP-osoitteet

1. Avaa PowerShellin komentorivi-ikkuna ja jäljellä olevat vaiheet tämän osan yhden PowerShell-istunnossa. Jos sinulla ei ole vielä asentanut ja määrittänyt PowerShell, suorita [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) -artikkelin ohjeiden mukaisesti.

2. Muuta seuraavat $Variables "arvoja" Azure [sijainti](https://azure.microsoft.com/regions) , virtual verkkoyhteys on kohdassa nimi [resurssiryhmä](../azure-resource-manager/resource-group-overview.md#resource-groups)-VNet resurssiryhmän, yhdistettävää, NIC aliverkon ja nimeä NIC-sivustosta. Vaiheiden avulla voit lisätä useita IP-osoitteita minkä tahansa NIC liitetty AM, kuin on tarpeen.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Jos et tiedä Azure paikassa tai resurssiryhmä nimen, kirjoita seuraavat komennot:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>Verkkokortti on oltava yhdistettynä aliverkon aiemmin luotuun Azure Virtual verkossa oleville (VNet). Kolme osaa: NIC, aliverkon ja VNet, kaikki on oltava sama alue-ja [tilauksen](../azure-glossary-cloud-terminology.md#subscription).  Jos et ole aiemmin käyttänyt VNets, lue lisätietoja niitä tai [luoda VNet](virtual-networks-create-vnet-arm-ps.md) -artikkelissa kerrotaan luonti [Virtual yleistä](virtual-networks-overview.md) -artikkelista.

    Jos et tiedä aiemmin VNet nimeä, kirjoita seuraava komento ja *VNet1* korvaa edellisen muuttujaan VNet nimi:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Palautettu luettelo on tyhjä, jos haluat luoda VNet. Lisätietoja siitä, miten, [Luo virtuaalisia verkko](virtual-networks-create-vnet-arm-ps.md) -artikkeliin.

    Kirjoita seuraavat komennot VNet sisällä aiemmin aliverkosta nimen ja korvata *Subnet1* yläpuolella aliverkon nimi:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Kirjoita seuraava komento noutaa aliverkon ja määrittää sen muuttuja.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Määritä IP-määrityksiä haluat määrittää NIC-sivustosta. Kunkin määritys voi olla yksi staattinen tai dynaaminen yksityinen IP-osoite ja julkiseen IP-osoite resurssi liitetty jokin staattinen tai dynaaminen osoite.

    Lisää tai poista haluamasi määrän, jotka noudattavat sen mukaan, kuinka monta IP-osoitteet, jotka haluat liittää Verkkokortti ja asetukset haluat määrittää määrityksiä.

    **IPConfig-1**

    Muuta arvo *PIP1* aiemmin luodun julkisen IP osoite resurssin nimi, joka on sijainnissa, jonka luot NIC- ja, joka ei ole liitetty toiseen NIC-sivustosta. Resurssiryhmä julkiseen IP-osoite resurssin nimen muuttaminen *RG1* sijaitsee. Muuta *IPConfig-1* nimi, jonka haluat myöntää ensimmäisen IP-määritys. Kirjoita seuraavat komennot:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Huomautus *-ensisijainen* vaihtaminen. Kun määrität useita IP-määrityksiä NIC: ssä, yksi määritys täytyy olla vastaanotto *ensisijainen*. Jos et tiedä aiemman julkiseen IP address resurssin nimen, kirjoita seuraava komento: Get-AzureRMPublicIPAddress | Taulukon muotoileminen nimi, sijainti, IP-osoite, IpConfiguration

    Jos **IPConfiguration** -sarakkeen arvoa ei ole palautettu tulosteesta julkiseen IP-osoite resurssin ei ole liitetty aiemmin NIC ja voidaan käyttää. Jos luettelo on tyhjä tai ei ole käytettävissä julkiseen IP address resursseja, voit luoda yksi **Uusi AzureRmPublicIPAddress** -komennon avulla.

    >[AZURE.NOTE] Julkisten IP-osoitteiden on korko.vuosi maksu. Lisätietoja IP-osoite hinnat, lue [IP-osoite hinnoittelu](https://azure.microsoft.com/pricing/details/ip-addresses) -sivu.

    **IPConfig-2**

    Muuta nimi, jonka haluat antaa toisen IP-määritys ja muuttaa *10.0.0.5* käyttämättömät kelvollinen IP-osoitteen määrittäminen NIC, aliverkon *IPConfig-2* . Kirjoita seuraavat komennot:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Kirjoita seuraava komento, jos et tiedä IP osoite alueen aliverkon myönnetyt:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Muuta nimi, jonka haluat myöntää kolmas IP-määritys ja anna seuraavat komennot *IPConfig-3* :

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Voit määrittää enintään 250 yksityinen IP-osoite NIC-sivustosta. Ei rajoitettu julkiseen IP-osoitteet, joita voidaan käyttää tilauksen määrän. Jos haluat lisätietoja, tutustu artikkeliin [Azure rajoituksen](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Luo NIC käyttämällä edellisessä vaiheessa määritetty IP-määrityksiä.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. Liitä Verkkokortti luotaessa AM [Create AM](../virtual-machines/virtual-machines-windows-ps-create.md) -artikkelin ohjeiden mukaisesti. Vaikka on artikkelissa Luo AM, joissa on käytössä Windows Server, vaiheet ovat samat Linux AM, kuin valitseminen eri käyttöjärjestelmä. Suorita vaiheet 1 – 3 artikkelin. Ohita vaiheet 4 ja 5 ja sitten suoritat vaiheen 6 luominen AM-artikkelissa.

    >[AZURE.WARNING] Vaihe 6 luominen AM artikkelissa epäonnistuu, jos olet muuttanut nimeltä $nic joksikin muuksi vaiheessa 6 on tämän artikkelin muuttujan tai ole suorittanut tämän artikkelin edelliset vaiheet.

8. Näytä yksityinen IP käsittelee kyseisen Azure DHCP Verkkokortti myönnetyt ja julkiseen IP address varatun resurssin Verkkokortti kirjoittamalla seuraava komento:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Lisää kaikki yksityinen IP-osoitteet (mukaan lukien ensisijainen) manuaalisesti käyttöjärjestelmän TCP/IP-määritys. 

**Windows**

1. Kirjoita komentokehotteeseen *ipconfig/kaikki*.  Näet vain *ensisijainen* yksityinen IP-osoite (kautta DHCP).
2. Kirjoita seuraavaksi *ncpa.cpl* komentorivi-ikkuna. Tämä Avaa uudessa ikkunassa.
3. Avaa **yhteyden**ominaisuudet.
4. Kaksoisnapsauta Internet-protokollan version 4 (IPv4)
5. Valitse **Käytä IP-osoite** ja seuraavat arvot:
    - **IP-osoite**: Kirjoita *ensisijainen* yksityinen IP-osoite
    - **Aliverkkopeite**: määrittäminen perustuu käytettävissä myös aliverkon ulkopuolella. Jos aliverkon on /24 esimerkiksi aliverkon sitten aliverkkopeite on 255.255.255.0.
    - **Oletusarvoinen yhdyskäytävä**: aliverkon ensimmäisen IP-osoite. Jos aliverkon ulkopuolella 10.0.0.0/24, yhdyskäytävän IP-osoite on 10.0.0.1.
    - Valitse **DNS server-osoitteiden** ja seuraavat arvot:
        - **Ensisijainen DNS-palvelin:** Kirjoita 168.63.129.16, jos et käytä DNS-palvelimeen.  Jos olet, kirjoita DNS-palvelimen IP-osoite.
    - Napsauta **Lisäasetukset** -painiketta ja Lisää muut IP-osoite. Lisää kunkin toissijaisen yksityinen IP-osoitteet samaan aliverkon määritetty ensisijainen IP-osoite ja Verkkokortti vaiheessa 8 lueteltu.
    - Valitse **OK** ja sulje TCP/IP-asetukset ja valitse **OK** , sulje sovittimen asetukset. Tämä muodostaa RDP-yhteys.

6. Kirjoita komentokehotteeseen *ipconfig/kaikki*. Kaikki IP-osoitteet, jotka olet lisännyt näkyvät ja DHCP on poistettu käytöstä.


**Linux (Ubuntu)**

1. Avaa pääte-ikkuna.
2. Varmista, että pääkansio-käyttäjänä. Jos et ole, voit tehdä tämän käyttämällä seuraava komento:

            sudo -i
3. Päivitä määritystiedosto verkko-liittymän (olettaen että 'eth0').
    - Pidä dhcp kohteen rivi. Tämä on aiempi kuin määrittää ensisijainen IP-osoite.
    - Lisää muita staattinen IP-osoite ja seuraavat komennot määritykset:

                cd /etc/network/interfaces.d/
                ls

        Raportissa pitäisi näkyä kaikki tiedoston.
4. Avaa tiedosto: vi *Tiedostonimi*.

    Pitäisi näkyä seuraavat rivit se lopussa:

            auto eth0
            iface eth0 inet dhcp
5. Lisää seuraavat rivit, jotka ovat tämän tiedoston rivien jälkeen:

            iface eth0 inet static
            address <your private IP address here>
6. Tallenna tiedosto käyttämällä seuraava komento:

            :wq
7.  Palauta verkon-liittymän seuraavalla komennolla:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Suorita ifdown ja ifup samalla rivillä, jos käytät etäyhteyden.
8. Tarkista IP-osoite lisätään verkon-liittymän seuraavalla komennolla:

            ip addr list eth0

    Raportissa pitäisi näkyä IP-osoite, voit lisätä luetteloon osana.

**Linux (Redhat, CentOS ja muut)**

1. Avaa pääte-ikkuna.
2. Varmista, että pääkansio-käyttäjänä. Jos et ole, voit tehdä tämän käyttämällä seuraava komento:

            sudo -i
3. Kirjoita salasanasi ja noudata ohjeita, kun ohjelma pyytää. Kun olet pääkansio-käyttäjä, siirry komentosarjojen verkkokansioon seuraavalla komennolla:

            cd /etc/sysconfig/network-scripts
4. Luettelon ifcfg Aiheeseen liittyvät tiedostot seuraava komento:

            ls ifcfg-*

    Raportissa pitäisi näkyä *ifcfg eth0* yhtenä tiedostot.
5. Kopioi *ifcfg eth0* -tiedosto ja anna sille nimi *ifcfg-eth0:0* seuraavalla komennolla:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Muokkaa *ifcfg-eth0:0* tiedoston seuraavalla komennolla:

            vi ifcfg-eth1
7. Muuta laitteen sopiva nimi-tiedostoa. *eth0:0* tässä tapauksessa seuraavalla komennolla:

            DEVICE=eth0:0
8. Muuta *IP-osoite = YourPrivateIPAddress* rivin vastaamaan IP-osoite.
9. Tallenna tiedosto seuraavalla komennolla:

            :wq
10. Käynnistä verkkopalveluita ja varmista, että muutokset onnistuvat suorittamalla seuraavat komennot:

            /etc/init.d/network restart
            Ipconfig

    Raportissa pitäisi näkyä lisäsit, IP-osoite *eth0:0*palautettu luettelo.

## <a name="add"></a>IP-osoitteiden lisääminen aiemmin luotuun AM

Seuraavien vaiheiden Ylimääräisten IP-osoitteiden lisääminen aiemmin luotuun NIC:

1. Avaa PowerShellin komentorivi-ikkuna ja jäljellä olevat vaiheet tämän osan yhden PowerShell-istunnossa. Jos sinulla ei ole vielä asentanut ja määrittänyt PowerShell, suorita [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) -artikkelin ohjeiden mukaisesti.

2. Muuta seuraavat $Variables "arvojen" nimi NIC, johon haluat lisätä IP-osoitteiden resurssiryhmä ja Verkkokortti on sijainti:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Jos et tiedä haluat muuttaa, kirjoita seuraavat komennot NIC nimi, muuta edellisen muuttujan arvot:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Luo muuttujan ja määritä se aiemmin NIC kirjoittamalla seuraava komento:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Noutaa Verkkokortti on yhdistetty täyttämällä [Vaihe 3](#subnet) / Create AM useita IP-osoitteiden osa on tämän artikkelin aliverkon tunnus.

5. Luo IP-määrityksiä, johon haluat lisätä verkkoon Luo vaiheen [5](#ipconfigs) ohjeita noudattamalla AM useita IP-osoitteiden osa on tämän artikkelin.

6. Muuta *$IPConfigName4* edellisessä vaiheessa luomasi IP-määritys nimi. Jos haluat lisätä määritystä, kirjoita seuraava komento:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Määritä IP-määritysten NIC kirjoittamalla seuraava komento:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Lisää IP-osoite lisätään AM-käyttöjärjestelmään NIC vaiheessa [9](#os) Luo AM useita IP-osoitteiden osa on tämän artikkelin ohjeita noudattamalla.
