<properties 
   pageTitle="Perinteinen virtual verkkojen yhdistämisestä Resurssienhallinta virtual verkkojen PowerShellin avulla | Microsoft Azure"
   description="Perinteinen VNets ja Resurssienhallinta VNets VPN-yhdyskäytävän ja PowerShell VPN-yhteyden luominen"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Virtual verkostojen yhdistäminen eri käyttöönoton mallien PowerShellin avulla

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShellin](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure on kaksi hallinta-mallit: perinteinen ja resurssien hallinta (RM). Jos olet käyttänyt Azure jonkin aikaa, sinulla on luultavasti Azure VMs ja esiintymän roolit perinteinen VNet käynnissä. Uudempaan VMs ja roolin esiintymät saattaa olla käynnissä VNet, joka on luotu resurssien hallinta. Tässä artikkelissa käydään läpi perinteinen VNets liittämisestä Resurssienhallinta VNets, jotta resurssien sijaitsee erillisessä käyttöönoton mallit keskenään yhdyskäytävä-yhteyden kautta.

Voit luoda yhteyden VNets, jotka ovat eri tilaukset ja eri alueiden välillä. Voit muodostaa VNets, joka on jo yhteyden paikallisen verkkoihin, kunhan yhdyskäytävään, joka on määritetty kanssa on dynaaminen tai reitityksen. Saat lisätietoja VNet VNet yhteydet [VNet VNet usein kysytyt kysymykset](#faq) on tämän artikkelin lopussa.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Käyttöönotto-malleja ja viestintämenetelmien yhteyden VNets eri käyttöönotto-malleissa

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Päivitämme seuraavan taulukon kuin uusia artikkeleita ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan taulukosta.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Ennen kuin aloitat

Seuraavat vaiheet opastusta tarvittavat dynaaminen tai reitin yhdyskäytävän määrittäminen kunkin VNet ja luo välillä yhdyskäytävien VPN-yhteyden asetukset. Tämä määritys ei tue staattinen tai käytännön yhdyskäytävät.

### <a name="prerequisites"></a>Edellytykset

 - Molemmat VNets on jo luotu.
 - VNets osoitealueet Älä päällekkäin keskenään tai päällekkäin muiden yhteydet, jotka yhdyskäytävien voidaan yhdistää tulostusalueet avulla.
 - Olet asentanut PowerShellin cmdlet-komennot (1.0.2 tai uudempi). Lisätietoja on kohdassa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) . Varmista, että olet asentanut Service Management (k) ja resurssin Manager (RM) Cmdlet-komentoja. 

### <a name="exampleref"></a>Esimerkki asetukset

Voit käyttää viitteenä Esimerkki-asetukset.

**Perinteinen VNet asetukset**

VNet nimi = ClassicVNet <br>
Sijainti = Länsi US <br>
VPN osoite välilyöntejä = 10.0.0.0/24 <br>
Aliverkon 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Paikallinen verkkonimi = RMVNetLocal <br>
GatewayType = DynamicRouting

**Resurssienhallinta VNet asetukset**

VNet nimi = RMVNet <br>
Resurssiryhmä = RG1 <br>
VPN IP-osoite välilyöntejä = 192.168.0.0/16 <br>
Aliverkon 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Sijainti = Itä US <br>
Yhdyskäytävän julkiseen IP-nimi = gwpip <br>
Paikallinen yhdyskäytävien = ClassicVNetLocal <br>
Virtuaalinen verkon yhdyskäytävän nimi = RMGateway <br>
Yhdyskäytävän IP-osoitteiden määritys = gwipconfig


## <a name="createsmgw"></a>Osa 1 – perinteinen VNet määrittäminen

### <a name="part-1---download-your-network-configuration-file"></a>Osa 1 – lataa verkko-kokoonpanotiedosto

1. Kirjaudu sisään järjestelmänvalvojan oikeuksilla PowerShell-konsolissa Azure-tiliisi. Seuraavan cmdlet-komennon kysyy kirjautumistietosi Azure-tilillesi. Kirjautuminen, kun se lataa tilin asetukset niin, että ne ovat käytettävissä PowerShellin Azure. Käytät k PowerShell cmdlet-komentojen suorittamiseen tämän osan määritykset.

        Add-AzureAccount

2. Vie Azure verkon kokoonpanotiedosto suorittamalla seuraavan komennon. Voit muuttaa eri sijaintiin tarvittaessa vientitiedoston sijainti. Voit muokata tiedostoa ja tuo ne Azure.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Voit muokata sitä lataamasi .xml-tiedoston avaaminen Esimerkki verkon määritys-tiedosto on artikkelissa [Verkon määritysten rakenne](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Osa 2 – Tarkista yhdyskäytävän aliverkon

Lisätä yhdyskäytävän aliverkon **VirtualNetworkSites** -osaan tekemäsi VNet, jos jokin ei ole jo luotu. Kun käsittelet verkon määritystiedoston yhdyskäytävän aliverkon on nimeltä "GatewaySubnet" tai Azure ei tunnista ja käytä sitä yhdyskäytävän aliverkon.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Esimerkki:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Osa 3 - Lisää paikallinen verkko-sivusto

Paikallinen verkko-sivusto, voit lisätä kuvaa, johon haluat muodostaa RM VNet. Saatat joutua lisäämään **LocalNetworkSites** elementti, jos kyseinen tiedosto ei ole vielä käytössä. Tässä vaiheessa määrityksessä, VPNGatewayAddress voi olla mikä tahansa kelvollinen julkiseen IP-osoite koska emme ole vielä luonut yhdyskäytävän Resurssienhallinta VNet varten. Yhdyskäytävän luodaan, kun olemme korvaa oikeaa julkiseen IP-osoitetta, joka on määritetty RM yhdyskäytävän tämä paikkamerkki IP-osoite.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Osa 4 - Liitä VNet lähiverkossa-sivuston kanssa

Tässä osassa määritetään, jonka haluat yhdistää VNet, lähiverkossa-sivustossa. Tässä tapauksessa on Resurssienhallinta VNet, joka viittaa aiemmin. Varmista, että nimet vastaavat. Tämä vaihe ei Gatewayn luominen. Määrittää paikallisen verkon, joka muodostaa yhdyskäytävän.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Tallenna tiedosto ja lataa 5 - osa

Tallenna tiedosto ja valitse tuodaan Azure suorittamalla seuraavan komennon. Varmista, että muutat tiedostopolku ympäristössä tarpeen mukaan.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Raportissa pitäisi näkyä kaltaisen tämä tulos, jossa tuonti onnistui.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Osa 6 – yhdyskäytävän luominen 

Voit luoda VNet yhdyskäytävän käyttämällä perinteinen portaalin tai PowerShell-toiminnolla.

Seuraavassa esimerkissä avulla voit luoda dynaamisia reititys yhdyskäytävän:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Voit tarkistaa yhdyskäytävän tila avulla `Get-AzureVNetGateway` cmdlet-komento.


## <a name="creatermgw"></a>Osa 2: Määritä RM VNet gateway

Voit luoda VPN-yhdyskäytävän RM VNet varten toimimalla seuraavien ohjeiden mukaisesti. Vaiheet, kunnes ei alkaa, kun perinteinen VNet yhdyskäytävän julkiseen IP-osoite on noudettu. 

1. **Kirjaudu sisään Azure-tiliisi** PowerShell-konsolissa. Seuraavan cmdlet-komennon kysyy kirjautumistietosi Azure-tilillesi. Kun lokiin kirjaaminen, tilin asetukset ladataan niin, että ne ovat käytettävissä PowerShellin Azure.

        Login-AzureRmAccount 

    Saat luettelon käytettävissä Azure tilauksistasi, jos sinulla on useita tilauksia.

        Get-AzureRmSubscription

    Määritä tilaus, jota haluat käyttää. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä**. Virtual verkon paikalliseen verkkoon kirjauduttaessa yhdyskäytävän tarkoittaa yleensä paikallisen sijainti. Tässä tapauksessa paikalliseen verkkoon kirjauduttaessa yhdyskäytävän viittaa perinteinen VNet. Anna sille nimi, jolla Azure voivat siihen viitataan ja määritä myös osoitteen tilaa etuliite. Azure käyttää IP-osoitteiden etuliitettä määrittämäsi tunnistavan liikenne lähettää paikallisen sijainti. Jos haluat muuttaa tietoja tähän myöhemmin, ennen kuin voit luoda oman yhdyskäytävän voit muokata arvot ja suorita otosten uudelleen.<br><br>
    - `-Name`on haluat määrittää viittaamaan paikalliseen verkkoon kirjauduttaessa yhdyskäytävän nimi.<br>
    - `-AddressPrefix`koskee perinteinen VNet osoitetilaa varten.<br>
    - `-GatewayIpAddress`on perinteinen VNet yhdyskäytävän julkiseen IP-osoite. Muista vaihtaa seuraava näyte vastaamaan oikea IP-osoite.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. Kohdistetaan VPN-yhdyskäytävän Resurssienhallinta VNet **pyytää julkiseen IP-osoite** . Et voi määrittää IP-osoite, jota haluat käyttää. IP-osoite on kohdistettu dynaamisesti VPN-yhdyskäytävä. Tämä ei kuitenkaan tarkoita IP-osoite muuttuu. Vain VPN yhdyskäytävän IP-osoitteen muuttumisen on kun yhdyskäytävän poistetaan ja luoda uudelleen. Et voi muuttaa koon, palauttaminen tai muut sisäiset ylläpito/päivitykset yhdyskäytävän yli.<br>Tässä vaiheessa on määritetty myös muuttuja, jota käytetään myöhemmin.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Tarkista virtual verkossa on yhdyskäytävän aliverkon**. Jos on olemassa ei Gatewayn aliverkon, voit lisätä sellaisen. Varmista, että yhdyskäytävän aliverkon nimeltä *GatewaySubnet*.

5. **Noutaa aliverkon** käytettäviä yhdyskäytävän suorittamalla seuraavan komennon. Tässä vaiheessa on määritetty myös muuttuja, jota käytetään seuraavaan vaiheeseen.
    - `-Name`on Resurssienhallinta VNet nimi.
    - `-ResourceGroupName`on, VNet on liitetty resurssiryhmä. Yhdyskäytävän aliverkon on jo tämä VNet ja nimen on oltava *GatewaySubnet* toimii oikein.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Luo yhdyskäytävän IP-osoitteiden määritys**. Yhdyskäytävän määrittäminen määrittää aliverkon ja käyttämään julkiseen IP-osoite. Seuraavassa esimerkissä avulla voit luoda yhdyskäytävän määritykset.<br>Tässä vaiheessa `-SubnetId` ja `-PublicIpAddressId` parametrit on siirrettävä id-ominaisuutta aliverkon ja IP-osoite objektien vastaavasti. Et voi käyttää yksinkertaisen merkkijonon. Muuttujia on määritetty vaiheessa pyytää julkiseen IP ja vaiheen aliverkon hakemiseen.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Luo Resurssienhallinta VPN-yhdyskäytävän** suorittamalla seuraavan komennon. `-VpnType` On oltava *RouteBased*. Voi kestää vähintään 45 minuuttia suorittaminen.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Julkiseen IP-osoitteen kopioiminen** kerran VPN-yhdyskäytävän on luotu. Sitä käytetään, kun määrität perinteinen VNet lähiverkon asetukset. Seuraavan cmdlet-komennon avulla voit hakea julkisia IP-osoite. Julkinen IP-osoite on merkitty palata *IP-osoite*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Osa 3: Muokata lähiverkossa perinteinen VNet

Voit tehdä tämän joko vieminen verkko-kokoonpanotiedosto, muokkaamaan sitä, ja tallentamalla ja tuominen takaisin Azure vaiheen. Vaihtoehtoisesti voit muokata tätä asetusta perinteinen-portaalissa. 

### <a name="to-modify-in-the-portal"></a>Jos haluat muokata portaalissa

1. Avaa [perinteinen portal](https://manage.windowsazure.com).

2. Perinteinen-portaalissa vasemmassa reunassa alaspäin ja valitse **verkot**. Valitse **verkot** -sivulla **Paikallisen verkkojen** sivun yläreunassa. 

3. Valitse **Paikallinen verkot** -sivulla lähiverkossa, että olet osa 1, määrittänyt ja siirry sivun alareunaan ja valitse **Muokkaa**.

4. **Määritä lähiverkossa-tiedot** -sivulla Korvaa paikkamerkin IP-osoite, jonka loit edellisessä osassa Resurssienhallinta yhdyskäytävän julkiseen IP-osoite. Napsauta nuolta, siirry seuraavaan osioon. Tarkista, että **Osoitetilaa** ovat oikein ja valitse sitten Hyväksy muutokset valintamerkki.

### <a name="to-modify-using-the-network-configuration-file"></a>Jos haluat muokata, verkon määritysten käyttäminen

1. Vie verkon määritys-tiedosto.
2. Muokkaa tekstieditorissa, VPN-yhdyskäytävän IP-osoite.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Tallenna muutokset ja tuoda muokatun tiedoston Azure.


## <a name="connect"></a>Osa 4: Yhdyskäytävien välisen yhteyden luominen

Yhdyskäytävien välisen yhteyden luominen edellyttää PowerShell. Voit joutua lisäämään Azure-tilisi käyttämään perinteinen PowerShellin cmdlet-komennot. Voit tehdä seuraavan cmdlet-komennon: 
        
    Add-AzureAccount

1. **Määritä jaetun avaimen** suorittamalla seuraavan komennon. Tässä esimerkissä `-VNetName` on perinteinen VNet nimi ja `-LocalNetworkSiteName` on määrittämäsi lähiverkossa kun perinteinen portaalissa määritetty nimi. `-SharedKey` On arvo, joka voi luoda ja määrittää. Tässä määrittämäsi arvon on oltava sama arvo, joka määritetään seuraavassa vaiheessa luodessasi yhteys. Palaa tässä esimerkissä näytetään **tila: onnistuneen**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Luo VPN-yhteyden suorittamalla seuraavat komennot.

    **Määritä muuttujat**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Yhteyden muodostaminen**<br> Huomaa, että `-ConnectionType` on IP-ei Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Voit tarkastella yhteyden joko portaalissa tai käyttämällä `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet-komento.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet VNet usein kysytyt kysymykset

Saat lisätietoja VNet VNet yhteydet tarkastella usein kysytyt kysymykset.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


