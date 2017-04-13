<properties
    pageTitle="Määrittämistä ILB-Listener saat aina käytettävyys ryhmät | Microsoft Azure"
    description="Tässä opetusohjelmassa käytetään resursseja, jotka on luotu perinteinen käyttöönottomalli ja luo aina käyttöön käytettävyys ryhmän Listener Azure sisäinen kuormituksen tasauspalvelun (ILB) avulla."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Määritä ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa

> [AZURE.SELECTOR]
- [Sisäinen Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Ulkoinen Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään aina käyttöön käytettävyys ryhmän kuuntelija määrittämisestä **Sisäinen kuormituksen tasauspalvelun (ILB)**avulla.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Paikallaan ILB-listener aina käytettävyys ryhmän resurssien hallinnan malli-kohdassa [Configure sisäinen kuormituksen aina käytettävyys ryhmän Azure-tietokannassa](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Käytettävyys-ryhmän voivat sisältää replikat, jotka ovat paikallisen vain ainoastaan Azure tai kattavat paikallisen ja Azure yhdistelmäympäristö. Azure replikat voivat sijaita samalla alueella tai useita alueita käyttämällä useita virtual verkkoja (VNets). Seuraavia ohjeita oletetaan on jo [määritetty käytettävyys-ryhmä](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , mutta ei ole määritetty kuuntelija.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Ohjeita ja sisäisen kuuntelijoita rajoituksia
Ota käyttöön käytettävyys ryhmän kuuntelua käyttämällä ILB Azure-tietokannassa seuraavia ohjeita:

- Käytettävyys ryhmän kuuntelua tuetaan Windows Server 2008 R2, Windows Server 2012: ssa ja Windows Server 2012 R2.

- Vain yksi sisäinen käytettävyys ryhmän listener tuetaan kohti pilvipalvelussa, koska kuuntelutoiminto on määritetty ILB ja on vain yksi ILB pilvipalvelussa kohden. On kuitenkin voi luoda useita ulkoisia kuuntelijoita. Lisätietoja on artikkelissa [määrittäminen ulkoisten listener, aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Luo sisäinen listener saman pilvipalvelussa, johon sinulla ulkoisia listener käyttämällä cloud-palvelun julkisen VIP myös on ei tueta.

## <a name="determine-the-accessibility-of-the-listener"></a>Määrittää kuuntelua helppokäyttötoiminnot

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tässä artikkelissa keskitytään luominen listener, joka käyttää **Sisäinen kuormituksen tasauspalvelun (ILB)**. Jos tarvitset julkinen/ulkoinen listener, tutustu tämän artikkelin ohjeaiheiden määrittämisestä [ulkoinen listener](virtual-machines-windows-classic-ps-sql-ext-listener.md) versiota

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Luo kuormituksen AM päätepisteet palautuksen suoraan palvelimen kanssa

Saat ILB sinun on luotava sisäinen kuormituksen. Tämä tapahtuu alla olevaa komentosarjaa.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Jos **ILB**kannattaa lisätä staattinen IP-osoite. Tarkista ensin VNet määrittämisen suorittamalla seuraavan komennon:

        (Get-AzureVNetConfig).XMLConfiguration

1. Huomaa, joka sisältää VMs, jotka isännöivät replikat aliverkon **aliverkon** nimi. Tämä käytetään komentosarja **$SubnetName** -parametrissa.

1. Huomautus sitten **VirtualNetworkSite** nimi ja aloittaa **AddressPrefix** , joka sisältää VMs, jotka isännöivät replikat aliverkon. Etsi käytettävissä IP-osoite sekä siirtämällä **Testi AzureStaticVNetIP** -komento ja käsittelystä **AvailableAddresses**. Esimerkiksi jos VNet nimeltä *MyVNet* - ja oli aliverkon osoite alue, joka alkoi *172.16.0.128*, seuraava komento luettelo käytettävissä olevat osoitteet:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Valitse jokin käytettävissä olevat osoitteet ja käyttää sitä seuraavaa komentosarjaa **$ILBStaticIP** -parametri.

3. Kopioi alla PowerShell-komentosarjaa tekstieditorissa ja määritä ympäristöön (Huomaa, että oletusasetusten saanut joidenkin parametrien) sopivaksi muuttujien arvoihin. Huomaa, että aiemmin asennuksia, jotka käyttävät affiniteetti ryhmiä voi lisätä ILB. Lisätietoja ILB vaatimuksista on artikkelissa [Sisäisen kuormituksen](../load-balancer/load-balancer-internal-overview.md). Myös, jos käytettävyys-ryhmän laajuinen Azure alueet, sinun on suoritettava komentosarja kerran kullekin joten pilvipalvelussa ja solmut, jotka sijaitsevat, joten.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Kun olet määrittänyt muuttujat, kopioi komentosarja tekstieditorissa suorittaa kyselyjä PowerShellin Azure-istunto. Jos kehote näkyy edelleen >>, kirjoita ENTER uudelleen ja varmista, että komentosarjan suoritetaan. Huomautus

>[AZURE.NOTE] Azure perinteinen portaalin ei tue sisäinen kuormituksen tällä hetkellä niin ei näy ILB tai päätepisteet Azure perinteinen-portaalissa. **Get-AzureEndpoint** palauttaa kuitenkin sisäinen IP-osoite, jos se on käynnissä kuormituksen. Muussa tapauksessa palauttaa null.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Varmista, että KB2854082 on asennettu tarvittaessa

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Palomuurin porttien avaaminen käytettävyys ryhmän solmut

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Käytettävyys ryhmän kuuntelua luominen

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. ILB sinun on käytettävä IP-osoite, sisäisen kuormituksen tasauspalvelun (ILB) aiemmin luotu. Käytä seuraavaa komentosarjaa PowerShell tämän IP-osoitteen.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Johonkin VMs kopioi käyttöjärjestelmäkohtaisia PowerShell-komentosarjaa tekstieditorissa ja määritä muuttujat kirjoittamasi arvot.

    Windows Server 2012: n tai sitä uudemmassa versiossa voit käyttää seuraavaa komentosarjaa:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Käytä seuraavaa komentosarjaa Windows Server 2008 R2:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Kerran olet määrittänyt muuttujat, avaa järjestelmänvalvojana Windows PowerShell-ikkuna, ja valitse Kopioi komentosarja tekstieditori ja liitä PowerShellin Azure-istunnon suorittaa. Jos kehote näkyy edelleen >>, kirjoita ENTER uudelleen ja varmista, että komentosarjan suoritetaan.

2. Toista tämä jokaiselle AM. Tämä komentosarja määrittää IP-osoite resurssin cloud-palvelun IP-osoite ja määrittää muut parametrit kuten näytteenottimen portti. Kun IP-osoite-resurssi on online-tilaan, valitse vastata näytteenottimen porttiin kysely päätepisteestä kuormituksen loit aiemmin tässä opetusohjelmassa.

## <a name="bring-the-listener-online"></a>Tuo kuuntelua verkossa

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Kohteiden seuranta

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testaa käytettävyys ryhmän kuuntelua (sisällä saman VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
