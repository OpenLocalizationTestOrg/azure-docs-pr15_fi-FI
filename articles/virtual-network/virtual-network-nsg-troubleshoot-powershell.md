<properties 
   pageTitle="Vianmääritys verkon käyttöoikeusryhmät - PowerShell | Microsoft Azure"
   description="Opi suorittamaan verkon käyttöoikeusryhmät Azure Resurssienhallinta käyttöönoton mallin Azure PowerShellin avulla."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Vianmääritys verkon käyttöoikeusryhmät Azure PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShellin](virtual-network-nsg-troubleshoot-powershell.md)

Jos määritetty verkon käyttöoikeusryhmät (NSGs) virtuaalikoneen (AM) ja esiintyy AM yhteysongelmat, tässä artikkelissa on yleiskatsaus diagnostiikka ominaisuuksia NSGs ongelmien toimenpiteitä varten.

NSGs avulla voit ohjata liikenne, että työnkulku ja siitä uloskirjautuminen näennäiskoneiden (VMs). NSGs voi suojata aliverkosta Azure Virtual Network (VNet), verkkoliittymät (NIC) tai molempia. Tehokas sääntöjä NIC ovat säännöt, jotka ovat käytetty NIC NSGs ja se on yhdistetty aliverkon yhteenlaskeminen. Nämä NSGs yli säännöt voi joskus ristiriidassa keskenään ja vaikuttaa AM verkkoyhteyden.  

Voit tarkastella kaikkia tehokkaita suojaussäännöt-että NSGs oman AM NIC käytössä. Tässä artikkelissa kerrotaan vianmääritys AM yhteysongelmat sääntöjen käyttäminen Azure resurssien hallinnan käyttöönottomalli. Jos et ole tottunut VNet ja NSG käsitteitä, lue [Virtual verkko](virtual-networks-overview.md) - ja [Verkko-käyttöoikeusryhmät](virtual-networks-nsg.md) yleiskatsaus artikkelit.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Vianmääritys AM liikenteen suunnan tehokkaita suojaussäännöt avulla

Tilanne, jossa seuraa on esimerkki yleisen yhteyden ongelman:

AM, jonka nimi on *VM1* on osa nimeltä *Subnet1* sisällä VNet, nimeltä *WestUS VNet1*aliverkon. Yhteyden muodostaminen käyttämällä RDP TCP-portin 3389 AM epäonnistuu. NSGs käytetään NIC- *VM1 NIC1* ja aliverkon *Subnet1*. TCP-portin 3389 liikenteen on sallittu verkkoliittymän *VM1 NIC1*liittyvät NSG kuitenkin TCP ping VM1's portti 3389 epäonnistuu.

Tässä esimerkissä käytetään TCP-portin 3389, kun seuraavat vaiheet voidaan määrittää saapuvan ja lähtevän epäonnistuneita minkä tahansa portin kautta.

## <a name="detailed-troubleshooting-steps"></a>Yksityiskohtaiset vianmääritysohjeet
Mikä AM NSGs vianmäärityksen seuraavasti:

1. Käynnistä PowerShellin Azure-istunnon ja Azure kirjautuessasi. Jos et ole käyttänyt Azure PowerShelliä, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) -artikkelissa.

2. Kirjoita seuraava komento palauttaa kaikki NIC, nimeltä *VM1 NIC1* resurssiryhmä *RG1*NSG koskevat:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Jos et tiedä NIC nimeä, kirjoita seuraava komento noutaa kaikki NIC-resurssiryhmä nimet: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Seuraava teksti on tehokas säännöt-tulosteen palautettiin *VM1 NIC1* NIC:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Huomautus tulosteen seuraavat tiedot:

    - On kaksi **NetworkSecurityGroup** osaa: liitetty aliverkon (*Subnet1*) ja liitetty NIC (*VM1 NIC1*). Tässä esimerkissä NSG on käytössä kunkin.
    - **Liitoksen** näyttää resurssin (aliverkon tai NIC) annetun NSG on liitetty. Jos NSG resurssi on siirretty/objekteista heti, ennen kuin suoritat tämän komennon, voit joutua odottamaan muutaman sekunnin, jotta muutos vastaamaan komennon tulosteessa. 
    - Säännön nimiä, jotka ovat etuliitteenä on *defaultSecurityRules*: kun NSG on luotu, se luodaan useita oletusarvon suojaussäännöt. Oletusarvoinen sääntöjä ei voi poistaa, mutta niitä voi korvata suurempi prioriteettisääntöjä.
     Lue lisätietoja NSG oletusarvon suojaussäännöt [NSG yleiskatsaus](virtual-networks-nsg.md#default-rules) -artikkelista.
    - **ExpandedAddressPrefix** laajentaa osoitteiden etuliitteiden NSG oletusarvon tunnisteiden. Tunnisteet vastaavat useita osoitteiden etuliitteiden. Tunnisteet laajentamista voi olla hyötyä, kun AM yhteyden ja valitse tietyn sähköpostiosoitteen etuliitteiden vianmääritys. Esimerkiksi kanssa VNET peering VIRTUAL_NETWORK tunnisteen saat näkyviin peered VNet etuliitteiden edellisen tulos.

        >[AZURE.NOTE] Komennon vain näyttää tehokkaita säännöt, jos NSG liittyy aliverkon, NIC tai molempia. AM voi olla useita NIC käytetty eri NSGs kanssa. Kun vianmääritysohjeita on suorittaa komennon kunkin NIC-sivustosta.
        
3. Helpottamiseksi suodattaminen NSG säännöt useita päälle, kirjoita vianmäärittämistä seuraavia komentoja: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Suodattimen RDP tietoliikenteen (porttinumeroa 3389) käytetään ruudukkonäkymässä, seuraavassa kuvassa esitetyllä tavalla:

    ![Sääntöluettelo](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Kuten näet ruudukko-näkymässä, ovat molemmat Salli ja ryhmiltä RDP säännöt. Vaihe 2 tulosteen näkyy, että *DenyRDP* sääntö on käytetty aliverkon NSG. Saapuvan liikenteen säännöt käytetty aliverkon NSGs käsitellään ensin. Jos vastine löytyy, verkkoliittymän käytetty NSG ei käsitellä. Tässä tapauksessa aliverkon *DenyRDP* sääntöä estää RDP AM (**VM1**).

    >[AZURE.NOTE] AM voi olla useita NIC liitetään. Kunkin voi poistaa eri aliverkon. Koska komennot edelliset vaiheet suoritetaan NIC vastaan, on tärkeää varmistaa, että määrität NIC, sinulla on yhteys epäonnistui. Jos et ole varma, voit suorittaa komentoja aina vastaan kunkin NIC AM liitetään.

5. RDP VM1 kyselyjä, voit muuttaa *Salli RDP(3389)* - **Subnet1 NSG** NSG *Estä RDP (3389)* sääntö. Varmista, että TCP-portin 3389 on avoinna avaamalla RDP-yhteyden AM tai PsPing-työkalun avulla. Lue lisää PsPing lukemalla [PsPing lataussivulle](https://technet.microsoft.com/sysinternals/psping.aspx)

    Voit tai poistaa säännöt NSG edellä mainittujen tietojen avulla tuloste seuraava komento:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Huomioon otettavia seikkoja

Ota seuraavat seikat huomioon, kun yhteysongelmien vianmääritys:

- Oletusarvon NSG säännöt Internetistä saapuvien käytön estäminen ja sallia vain VNet saapuva liikenne. Sääntöjen kannattaa lisätä eksplisiittisesti sallimaan saapuvan Internet-halutulla tavalla.
- Jos ei ole NSG suojaussäännöt vuoksi AM verkkoyhteyden epäonnistuu, syy voi olla ongelman:
    - Palomuuriohjelmiston sisällä AM-käyttöjärjestelmä
    - Määritetty virtual laitteiden tai paikallisen liikenne ohjautuu. Internet-liikenne on ohjattu paikalliseen kautta pakotettu tunneling. Kun tämä asetus, sen mukaan, miten paikallisen verkkolaitteiston käsittelee tämän liikenne ei välttämättä toimi on RDP/SSH yhteys Internet-yhteyttä AM. Lue, miten voit selvittää reitin ongelmia, jotka voivat olla estämään etenemisen liikenteen ja siitä uloskirjautuminen AM artikkeliin [Vianmääritys tiet](virtual-network-routes-troubleshoot-powershell.md) . 
- Jos olet peered VNets, oletusarvoisesti, VIRTUAL_NETWORK tunnisteen laajenee automaattisesti sisällytettävät etuliitteiden peered VNets. Voit tarkastella seuraavia etuliitteitä **ExpandedAddressPrefix** -luettelosta vianmääritys VNet peering connectivity liittyvät ongelmat. 
- Tehokas suojaussäännöt näkyvät vain onko NSG liittyvät AM NIC ja aliverkon. 
- Jos argumentissa on ei ole liitetty Verkkokortti NSGs tai aliverkon ja sinulla on julkinen IP-osoite, että AM myönnetyt, kaikki portit on avoin saapuva ja lähtevä käytön. Jos AM on julkinen IP-osoite, NSGs soveltaminen NIC tai aliverkon on erittäin suositeltavaa.  
