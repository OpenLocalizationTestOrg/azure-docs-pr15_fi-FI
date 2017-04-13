<properties
   pageTitle="Määritä aina käytettävyys ryhmän kuuntelijoita – Microsoft Azure"
   description="Määritä käytettävyys ryhmän kuuntelijoita Azure Resurssienhallinta-mallin sisäinen kuormituksen käyttäminen yksi tai useampi IP-osoite."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Määritä vähintään yksi aina käyttöön käytettävyys ryhmän kuuntelijoita - resurssien hallinta 

Tässä ohjeaiheessa esitellään, miten tehtävä kaksi asiaa:

- Luo sisäinen kuormituksen SQL Serverin käytettävyys ryhmien käyttämällä PowerShell cmdlet-komennot.

- Lisää muita IP-osoitteiden kuormituksen tukemaan useita SQL Server-käytettävyys ryhmän. 

SQL Server käytettävyys-ryhmän listener on virtual verkkonimi, jossa asiakastietokoneet muodostavat yhteyden, jotta voit käyttää tietokannan ensisijaisen tai toissijaisen se. Valitse Azure-virtuaalikoneissa kuormituksen tasauspalvelun pitää kuuntelua IP-osoite. Kuormituksen reitittää liikenteen, joka seuraa näytteenottimen porttiin SQL Server-esiintymän. Useimmissa tapauksissa käytettävyys-ryhmä käyttää sisäistä kuormituksen. Azure sisäinen kuormituksen ylläpitää yhden tai usean IP-osoitteet. Kunkin IP-osoite käyttää tiettyä näytteenottimen porttia. Tämä asiakirja näyttää, miten PowerShellin avulla voit luoda uuden kuormituksen tai IP-osoitteiden lisääminen aiemmin kuormituksen, SQL Server käytettävyys ryhmien. 

Mahdollisuus määrittää useita IP-osoitteita sisäinen kuormituksen uudet Azure ja on vain resurssien hallinnan malli. Tämän tehtävän suorittamiseen tarvitset on otettu käyttöön Resurssienhallinta-mallissa Azuren näennäiskoneiden SQL Server käytettävyys ryhmän. SQL Server sekä näennäiskoneiden on kuuluttava käytettävyys samat. Voit käyttää [Microsoft mallin](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) käytettävyys-ryhmän luominen automaattisesti Azure Resurssienhallinta. Tämä malli luo automaattisesti käytettävyys-ryhmässä, mukaan lukien sisäinen kuormituksen puolestasi. Halutessasi voit [manuaalisesta aiemmin AlwaysOn käytettävyys-ryhmään](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Tässä ohjeaiheessa edellyttää käytettävyys ryhmien on jo määritetty.  

Aiheeseen liittyvät artikkelit ovat seuraavat:

- [Määritä AlwaysOn käytettävyys ryhmät Azure AM (Graafisen)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [VNet VNet yhteysasetusten Azure Resurssienhallinta ja PowerShellin avulla](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windowsin palomuurin

Määritä SQL Server käyttää Windowsin palomuuri. Haluat määrittää palomuurin sallimaan TCP-portit käytössä SQL Server-esiintymän sekä listener näytteenottimen käyttämä portti yhteydet. Lisätietoja on artikkelissa [Configure tietokantoja ohjelma Windowsin palomuurin](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Luo saapuvan liikenteen sääntö SQL Server-portin ja näytteenottimen portin.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Esimerkkikomentosarja: Luo sisäinen kuormituksen PowerShellin avulla

> [AZURE.NOTE] Jos olet luonut käytettävyys-ryhmä [Microsoft mallin](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kuormituksen sinun ei tarvitse suorittaa tämän vaiheen. 

Seuraavaa PowerShell-komentosarjaa Luo sisäinen kuormituksen, määrittää kuormituksen säännöt ja määrittää kuormituksen IP-osoite. Suorittamalla komentosarja, Avaa Windows PowerShell ise: ja liitä komentosarja-ruudussa. Käytä `Login-AzureRMAccount` PowerShell Kirjaudu. Jos sinulla on useita tilauksia Azure- `Select-AzureRmSubscription ` tilauksen määrittämiseen. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Esimerkkikomentosarja: IP-osoitteen lisääminen aiemmin luotuun kuormituksen PowerShellin avulla

Käyttämään useita käytettävyys-ryhmän Lisää IP-osoitteen lisääminen aiemmin luotuun kuormituksen PowerShellin avulla. Kunkin IP-osoite tarvitaan oma kuormituksen säännön, näytteenottimen portti ja eteen portin.

Edusta-portti on portti, sovellusten avulla voit muodostaa yhteyden SQL Server-esiintymän. IP-osoitteiden eri käytettävyys ryhmien käyttää samaa edusta-porttiin.

>[AZURE.NOTE] SQL Server käytettävyys ryhmät jokaisen IP-osoitteen edellyttää tietyn näytteenottimen portin. Esimerkiksi jos yksi IP-osoitetta kuormituksen käyttää näytteenottimen porttia 59999, ei muiden kyseisen kuormituksen IP-osoitteiden käyttää näytteenottimen portin 59999. 

- Lisätietoja kuormituksen rajoitukset artikkelissa [Verkko rajat - Azure Resurssienhallinta](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits)-kohdassa **Yksityinen eteen lopettaa IP kuormituksen kohden** .

- Lisätietoja käytettävyys ryhmän rajoitukset artikkelissa [rajoitukset (käytettävyys ryhmät)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Seuraavaa komentosarjaa Lisää aiemmin kuormituksen uusi IP-osoite. Päivitä muuttujat-ympäristössä. ILB käyttää kuuntelevan portin kuormituksen Vastatilin edusta-porttiin. Tämä portti voi olla käytettävä Kuuntele SQL Serverin portti. Tämä on oletusarvo esiintymät SQL Server-porttia 1433. Säännön käytettävyys-ryhmän kuormituksen vaatii irrallisen IP (Palauta suoraan server), joten uudelleen portti on sama kuin edusta-porttiin.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Määritä klusteri käyttämään kuormituksen tasauspalvelun IP-osoite 

Seuraavaksi kuuntelua klusterin ja tuoda kuuntelua verkossa. Voit tehdä tämän seuraavasti: 

1. Luo käytettävyys ryhmän kuuntelua automaattisesti klusterin  

2. Tuo kuuntelua verkossa

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. käytettävyys ryhmän kuuntelijaa luoda automaattisesti klusterin

Tässä vaiheessa asiakkaan tukiasemaa lisääminen vikasietoklusteriin automaattisesti klusterin hallinta ja klusterin resurssien kuuntelemaan näytteenottimen porttia PowerShellin avulla. 

1. Azure virtuaalikoneen, joka isännöi ensisijainen se muodostaa RDP avulla. 

2. Avaa automaattisesti klusterin hallinta.

3. Valitse **verkot** -solmu ja klusterin verkon taulukon nimi. Käytä tätä nimen `$ClusterNetworkName` muuttujan PowerShell-komentosarjaa.

4. Laajenna klusterinimeä ja valitse sitten **roolit**.

5. Valitse **roolit** -ruudussa käytettävyys ryhmänimeä hiiren kakkospainikkeella ja valitse sitten **Lisää resurssin** > **Asiakkaan tukiasemaa**.

6. Kirjoita **nimi** -ruutuun nimen antaminen tämän uusi kuuntelu ja valitse sitten **seuraavan** kahdesti ja valitse sitten **Valmis**. Ei tuoda listener tai resurssi online tässä vaiheessa.
 
    Uusi kuuntelu nimi on verkkonimi, jonka avulla sovellukset yhteyden muodostaminen SQL Server käytettävyys-ryhmän ominaisuudet.

7. **Resurssit** -välilehti ja valitse juuri luomasi Client Access-kohdassa Laajenna. IP-resurssia hiiren kakkospainikkeella ja valitse Ominaisuudet. IP-osoitteen nimi muistiin. Voit käyttää tätä nimeä `$IPResourceName` muuttujan PowerShell-komentosarjaa.

8. Valitse **IP-osoite** **Staattinen IP-osoite** ja määritä staattinen IP-osoite on sama osoite, jota käytit, kun määrität kuormituksen tasauspalvelun IP-osoite Azure-portaalissa. 

9. Poista käytöstä NetBIOS tätä osoitetta ja valitse **OK**. Toista tämä vaihe kunkin IP-resurssin, jos ratkaisu ulottuu useita Azure VNets. 

10. Tee SQL Server käytettävyys Group resurssi määräytyy IP-osoite. Oikea napsautus resurssin klusterin hallinnassa, tämä on **resurssit** -välilehden **Muut resurssit**. 

11. Klusterisolmu, joka isännöi tällä hetkellä ensisijainen se, valitse Avaa laajennettuja PowerShell ise: ja liitä seuraavat komennot uusi komentosarja. **Riippuvuudet** -välilehdessä kuuntelutoiminnon nimi.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Päivitä muuttujat ja suorita IP-osoite ja portin määrittäminen uusi kuuntelu PowerShell-komentosarjaa.

 >[AZURE.NOTE] SQL-palvelimet ovat eri alueilla, jos haluat suorittaa PowerShell-komentosarjaa kahdesti. Ensimmäisen kerran käyttämällä klusterin IP resurssinimi-klusterin verkkonimi ja lataa ensimmäisen resurssiryhmä tasaustoiminto IP-osoite. Toisen kerran käyttämällä klusterin IP resurssinimi-klusterin verkkonimi ja lataa toisen resurssiryhmä tasaustoiminto IP-osoite.

Klusterin on nyt yritysresurssi listener käytettävyys ryhmän.

## <a name="2-bring-the-listener-online"></a>2. Tuo kuuntelua verkossa

Käytettävyys ryhmä-listener resurssille määritetty voit tuoda kuuntelua verkossa niin, että sovellusten muodostaa käytettävyys-ryhmä ja kuuntelua tietokannoissa.

1. Siirry takaisin käyttämään automaattisesti klusterin Manager. Laajenna **roolit** ja Korosta käytettävyys-ryhmä. **Resurssit** -välilehden listener nimeä hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. Valitse **riippuvuudet** -välilehti. Jos luettelossa on lueteltu useita resursseja, varmista, että IP-osoitteiden on tai ei ja riippuvuudet. Valitse **OK**.

1. Listener nimeä hiiren kakkospainikkeella ja valitse **Ota käyttöön**.

1. Kun listener on online- **resurssit** -välilehti, napsauta käytettävyys-ryhmää hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. Luo riippuvuus listener nimi resurssi (ei IP address resurssien nimi). Valitse **OK**.

1. Käynnistä SQL Server Management Studiossa ja yhdistä se ensisijainen.

1. Siirry **AlwaysOn suuri käytettävyys** | **käytettävyys ryhmien** | **käytettävyys ryhmän kuuntelijoita**. 

1. Pitäisi tulla näkyviin listener nimi, jonka loit automaattisesti klusterin hallinta. Listener nimeä hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

1. **Port (portti)** -ruutuun Määritä porttinumero käytettävyys ryhmän kuuntelua avulla voit käyttää aiemmin $EndpointPort (1433 oli oletusarvo), valitse **OK**.

Nyt on SQL Server käytettävyys ryhmän Azuren näennäiskoneiden Resurssienhallinta-tilassa. 

## <a name="test-the-connection-to-the-listener"></a>Testaa yhteys kuuntelutoiminto

Testaa yhteys:

1. SQL Server-palvelimeen, joka on samassa virtual verkossa, mutta se ei ole RDP. Tämä on muita SQL Server-klusterin.

2. Testaa yhteys **sqlcmd** apuohjelmalla. Esimerkiksi seuraava komentosarja muodostaa **sqlcmd** yhteyden ensisijainen se kuuntelua Windows-todennuksen avulla:

    ```
    sqlmd -S <listenerName> -E
    ```

    Jos kuuntelua käyttää kuin oletussijaintiin portin portin (1433), Määritä portti yhteysmerkkijonon. Esimerkiksi seuraava sqlcmd komento muodostaa yhteyden portin 1435 kuuntelutoimintoa: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQLCMD yhteys muodostetaan automaattisesti sen mukaan, kumpi SQL Server-esiintymän isännöi ensisijainen se. 

>[AZURE.NOTE] Varmista, että määrität portti on auki sekä SQL-palvelimien palomuurin. Molempien palvelinten vaadi saapuvan liikenteen sääntö, jota käytetään porttinumeroa. Lisätietoja on kohdassa [Lisää tai Muokkaa palomuurisääntöä](http://technet.microsoft.com/library/cc753558.aspx) . 

## <a name="guidelines-and-limitations"></a>Ohjeita ja rajoitukset

Huomautus kuormituksen käytettävyys ryhmän listener käyttämällä sisäinen Azure-tietokannassa, noudata seuraavia ohjeita:

- Sisäinen kuormituksen kanssa voit käyttää vain saman virtual verkoston-kuuntelua.

## <a name="for-more-information"></a>Lisätietoja

Lisätietoja on artikkelissa [Azure AM ryhmitellä manuaalisesti määrittäminen aina käyttöön käytettävyyttä](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

### <a name="powershell-cmdlets"></a>PowerShellin cmdlet-komennot

Luo sisäinen kuormituksen Azuren näennäiskoneiden PowerShellin cmdlet-komennot avulla.

- `New-AzureRmLoadBalancer`Luo kuormituksen tasauspalvelun. Lisätietoja on kohdassa [Uusi AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) . 

- `New-AzureRMLoadBalancerFrontendIpConfig`Luo kuormituksen tasauspalvelun edusta IP-määrityksen. Lisätietoja on kohdassa [Uusi AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) .

- `New-AzureRmLoadBalancerRuleConfig`Luo sääntö konfiguroinnin kuormituksen tasauspalvelun. Lisätietoja on kohdassa [Uusi AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) . 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`Luo Taustajärjestelmä osoite resurssivarantoon konfiguroinnin kuormituksen tasauspalvelun. Lisätietoja on kohdassa [Uusi AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) . 

- `New-AzureRmLoadBalancerProbeConfig`Luo kuormituksen tasauspalvelun näytteenottimen konfiguroinnin. Lisätietoja on kohdassa [Uusi AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) .

Jos haluat poistaa kuormituksen tasauspalvelun Azure resurssiryhmä, käytä `Remove-AzureRmLoadBalancer`. Lisätietoja on artikkelissa [Poista AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx).
