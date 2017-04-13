## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>VNet, käyttämällä PowerShell verkon config tiedoston luominen

Azure määrittää xml-tiedoston avulla kaikki käytettävissä olevat VNets tilaukseen. Voit ladata tämän tiedoston ja muokata sitä, voit muokata tai poistaa aiemmin VNets ja luoda uusia. Tässä asiakirjassa Lue, miten voit ladata tätä tiedostoa, verkon määritysten (tai netcgf)-tiedostona, tarkoitetun ja Muokkaa Luo uusi VNet. Lisätietoja verkko-kokoonpanotiedosto [Azure virtual verkon määritysten rakenteen](https://msdn.microsoft.com/library/azure/jj157100.aspx) tarkistaminen

Voit luoda VNet, PowerShellin netcfg-tiedoston, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../articles/powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.
2. PowerShellin Azure-konsolin ladata verkko-kokoonpanotiedosto alla oleva komento suorittamalla **Get-AzureVnetConfig** cmdlet-komennon avulla. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Odotettu tulos:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Avaa tiedosto, jonka tallensit vaiheessa 2 XML- tai muokkaaja-sovelluksesta ja Etsi **<VirtualNetworkSites>** elementti. Jos sinulla on jo luotu verkkoja, kunkin verkon näkyvät kuin oma **<VirtualNetworkSite>** elementti.
4. Tässä skenaariossa on kuvattu virtual verkoston luominen, Lisää seuraavat XML alle **<VirtualNetworkSites>** elementti:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Tallenna tiedosto verkon määritys.
10. PowerShellin Azure-konsolin verkon määritys-tiedoston lataaminen-komennon alla **Määrittäminen AzureVnetConfig** cmdlet-komennon avulla. Huomaa, että tulostus komennon alla, näkyviin tulee Valitse **OperationStatus** **onnistui** . Jos ei ole tapauksessa valitse virheet xml-tiedosto.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Näin Odotettu tulos yllä-komennon:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. PowerShellin Azure-konsolin varmistaa, että uutta verkostoa lisättiin alla oleva komento suorittamalla **Get-AzureVnetSite** cmdlet-komennon avulla. 

        Get-AzureVNetSite -VNetName TestVNet

    Näin Odotettu tulos yllä-komennon:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded