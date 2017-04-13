<properties 
   pageTitle="Virtuaalinen verkon muodostaa VPN-yhdyskäytävän ja PowerShellin käyttäminen useissa sivustoissa | Microsoft Azure"
   description="Tässä artikkelissa edetään ohjatusti paikallisen paikallisen sivustoissa muodostaa VPN-yhdyskäytävän käyttäminen perinteinen käyttöönoton mallin virtual verkkoon."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Sivusto yhteyden lisääminen VNet aiemmin luodun VPN-yhdyskäytävä-yhteyden avulla

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Perinteinen - PowerShell](vpn-gateway-multi-site.md)

Tässä artikkelissa käydään läpi sivusto (S2S)-yhteyksien lisääminen VPN-yhdyskäytävän, jossa on aiemmin luotu yhteys PowerShellin avulla. Tämä yhteystyyppi kutsutaan usein "usean sivuston"-määritys. 

Tämä artikkeli koskee virtual verkkoihin luotu perinteinen käyttöönottomalli (tunnetaan myös nimellä Service Management). Nämä vaiheet eivät vaikuta ExpressRoute /-,-sivusto samanaikaisesti olemassa olevien yhteyden määrityksiä. Saat lisätietoja samanaikaisesti olemassa olevien yhteyksien [ExpressRoute/S2S samanaikaisesti olemassa olevien yhteydet](../expressroute/expressroute-howto-coexist-classic.md) .

### <a name="deployment-models-and-methods"></a>Käyttöönotto-malleja ja menetelmät

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Päivitämme tämän taulukon kuin uusia artikkeleita ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Lisätietoja yhteyden

Voit muodostaa yhteyden yhden virtual verkoston paikallisen sivustoissa. Tämä on erityisen hankkimaan hybrid cloud ratkaisujen. Usean sivuston yhteyden Azure virtual yhdyskäytävien luominen on hyvin samankaltaisia kuin muiden sivusto yhteyksien luominen. Itse asiassa voit käyttää aiemmin Azure VPN-yhdyskäytävän, kunhan yhdyskäytävä on dynaaminen (reitti-pohjainen).

Jos sinulla on jo staattinen yhdyskäytävän virtual verkkoon, voit muuttaa yhdyskäytävän tyyppi dynaamiseksi tarvitsematta uudelleen virtual verkossa, jotta mahtuu usean sivuston. Ennen kuin muutat reitityksen tyyppi, varmista, että paikallisen VPN-yhdyskäytävän tukee reitti-pohjainen VPN-määrityksiä. 

![usean sivuston kaavio] (./media/vpn-gateway-multi-site/multisite.png "usean sivuston")

## <a name="points-to-consider"></a>Seikkoja huomioitavaksi

**Et voi tehdä muutoksia virtual verkoston Azure perinteinen portaalin avulla.** Tässä versiossa on halutaan muuttaa verkko-kokoonpanotiedosto sijaan perinteinen Azure-portaalissa. Jos teet muutoksia perinteinen Azure-portaalissa, ne korvaa virtual verkoston viittaus usean sivuston asetukset. 

Tuntuu melko perehtynyt verkon määritystiedoston käyttäminen ajan prosenttiosuuden usean sivuston ohjeiden mukaan. Jos sinulla on useita verkon määritysten työskentelevien henkilöiden, tarvitset varmistaaksesi, että kaikki tietävät tietoja tästä rajoituksesta. Tämä ei tarkoita Azure perinteinen portaalin ei voi käyttää lainkaan. Voit käyttää sitä, kaikki muut paitsi määritysmuutoksia tähän tietyn virtual verkkoon.

## <a name="before-you-begin"></a>Ennen aloittamista

Ennen kuin aloitat määritykset, varmista, että käytössäsi ovat seuraavat:

- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).

- Yhteensopivat VPN-laitteet kunkin paikalliset sijainti. Tarkista [Tietoja VPN laitteiden Virtual verkkoyhteydessä](vpn-gateway-about-vpn-devices.md) voit tarkistaa, onko laite, jota haluat käyttää jotain, joka on ehkä ole yhteensopiva.

- Ulkoisesti aukeaman julkisen IPv4 IP-osoite VPN-pienoisohjelman. IP-osoitetta ei löydy, takana tulkintatoiminnon Tämä on tarpeen.

- Tarvitset Azure PowerShellin cmdlet-komennot uusimman version asentaminen. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

- Henkilölle, joka ei parantamaan osoitteessa määrittäminen VPN-laitteistoa. Et voi määrittää VPN-laitteita automaattisesti luodut VPN-komentosarjoja perinteinen Azure-portaalista avulla. Tämä tarkoittaa sitä, sinun on hyvä tietoa siitä, miten VPN-laitteen määrittäminen ja käyttäminen joku, jolla ole.

- IP-osoitealueet, jota haluat käyttää virtual verkon (Jos et ole luonut). 

- IP-osoitealueet kunkin, sinun yhteyden muodostaminen paikalliseen verkkoon kirjauduttaessa sivustot. Tarvitset, varmista, että IP-osoite alueet kunkin paikallisen verkkosivustot, johon haluat muodostaa yhteyden päällekkäin. Muussa tapauksessa Azure perinteinen portaalin tai REST-Ohjelmointirajapinnalla hylkää ladattavan määritykset. 

    Esimerkiksi jos sinulla on kaksi paikalliseen verkkoon kirjauduttaessa sivustot, että molemmat sisältävät IP-osoite alueen 10.2.3.0/24 ja sinulla on paketti, jolla kohdeosoite 10.2.3.3, Azure ei tiedä mitä sivustoa haluat lähettää paketin, koska osoitealueita päällekkäin. Azure ei salli Lataa määritystiedoston, jossa on päällekkäisiä alueita estämiseksi reititys ongelmat.



## <a name="1-create-a-site-to-site-vpn"></a>1. luoda sivusto sivusto VPN-yhteyttä

Jos sinulla on jo sivusto sivusto VPN-yhteyttä dynaaminen reititys yhdyskäytävän erinomaisesti! Voit siirtyä [VPN-määritysasetusten vieminen](#export). Jos et, toimi seuraavasti:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Jos sinulla on jo sivusto sivusto virtual verkkoon, mutta se on staattinen (käytännön-pohjainen) reititys yhdyskäytävän:

1. Yhdyskäytävä-tyypin muuttaminen dynaaminen reititys. Usean sivuston VPN edellyttää dynaaminen (tunnetaan myös nimellä reitti-pohjainen) reititys yhdyskäytävä. Jos haluat muuttaa yhdyskäytävän tyyppi, sinun on ensin aiemmin yhdyskäytävän poisto ja sitten Luo uusi. Katso ohjeet [muuttamisesta käyttämäsi yhdyskäytävän VPN reititys tyyppi](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Määritä uusi yhdyskäytävä ja luoda VPN-tunnelin. Katso ohjeet [määrittäminen VPN-yhdyskäytävän perinteinen Azure-portaalissa](vpn-gateway-configure-vpn-gateway-mp.md). Ensin, että yhdyskäytävän tyypin muuttaminen dynaaminen reititys. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Jos sinulla ei ole sivuston sivuston virtual verkon:

1. Luo sivusto sivusto virtual verkon seuraavia ohjeita: [Luo Virtual Network Azure perinteinen portaalissa sivusto sivusto VPN-yhteyttä](vpn-gateway-site-to-site-create.md).  

2. Määritä dynaaminen reititys gateway seuraavia ohjeita: [Määritä VPN-yhdyskäytävä](vpn-gateway-configure-vpn-gateway-mp.md). Muista valita **dynaaminen reititys** yhdyskäytävän tyyppi.

## <a name="export"></a>2. verkon määritys-tiedoston vieminen 

Vie verkon määritys-tiedosto. Tiedosto, jonka voit viedä käytetään uusia usean sivuston-asetusten määrittämiseen. Jos tarvitset ohjeita siitä, miten voit viedä tiedoston, kohdassa artikkelin: [luomisesta VNet, verkon määritysten tiedoston Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. Avaa Verkko-kokoonpanotiedosto

Avaa lataamasi verkon määritysten tiedoston viimeisessä vaiheessa. Käytä pidät xml-editorin. Tiedoston pitäisi näyttää seuraavankaltaiselta:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. Lisää usean sivuston viittaukset

Kun lisäät tai poistat sivuston tietoja, määritysmuutoksia tehdä ConnectionsToLocalNetwork/LocalNetworkSiteRef. Lisääminen uusi paikallisen sivuston viittaus käynnistimien Azure, jos haluat luoda uuden tunnelin. Alla olevassa esimerkissä verkon määritys on yhden toimipaikan yhteyden. Tallenna tiedosto, kun olet tehnyt haluamasi muutokset.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. verkon määritys-tiedoston tuominen

Tuo verkko-kokoonpanotiedosto. Kun tuot tämän tiedoston muutokset, uuden tunneleita lisätään. Akselitunnelien käyttämällä dynaaminen yhdyskäytävä aiemmin luomasi. Jos tarvitset ohjeita tuotava tiedosto-kohdassa on artikkelissa: [luomisesta VNet, verkon määritysten tiedoston Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. Lataa näppäimet

Kun uusi tunneleita on lisätty, käytä PowerShell cmdlet-komennon `Get-AzureVNetGatewayKey` saat kullekin tunnelille IP/IKE ennalta jaetut avaimet.

Esimerkki:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Halutessasi voit käyttää myös *Hae Virtual verkon yhdyskäytävän jaettu avaimen* REST-Ohjelmointirajapinnalla saat valmiiksi jaetut avaimet.

## <a name="7-verify-your-connections"></a>7. Tarkista yhteydet

Tarkista usean sivuston tunnelin. Lataa kullekin tunnelille näppäimet, kannattaa tarkistaa yhteydet. Käytä `Get-AzureVnetConnection` saat VPN tunneleita luettelon, kuten alla olevassa esimerkissä. VNet1 on VNet nimi.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja VPN-yhdyskäytäviä, katso [VPN yhdyskäytävät](../vpn-gateway/vpn-gateway-about-vpngateways.md).

