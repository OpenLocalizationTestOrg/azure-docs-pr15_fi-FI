## <a name="virtual-network"></a>VPN
Virtual verkoissa (VNET) ja aliverkosta resurssien avulla määrittää suojauksen reunaa varten työmääriä Azure käynnissä. VNet jakauma osoite välilyöntejä, lasketaan CIDR lohkot perusteella. 

>[AZURE.NOTE] Verkoston järjestelmänvalvojat ovat tuttuja CIDR merkintätapaa. Jos et ole tottunut CIDR, [Katso lisätietoja sen](http://whatismyipaddress.com/cidr).

![VNet useita aliverkosta kanssa](./media/resource-groups-networking/Figure4.png)

VNets sisältää seuraavat ominaisuudet.

|Ominaisuus|Kuvaus|Otoksen arvot|
|---|---|---|
|**addressSpace**|Osoitteiden etuliitteiden, jotka muodostavat CIDR merkintätavan VNet kokoelma|192.168.0.0/16|
|**aliverkosta**|Muotoiluja sisältäviä aliverkosta, jotka muodostavat VNet|Katso [aliverkosta](#Subnets) alla.|
|**IP-osoite**|Liitetty objekti IP-osoite. Tämä on vain luku-ominaisuus.|104.42.233.77|

### <a name="subnets"></a>Aliverkosta
Aliverkon on Aliobjektin resurssi, VNet ja auttaa Määritä CIDR estäminen käyttämällä IP-osoitteiden etuliitteiden välilyönnit osoitteen osia. NIC voit aliverkosta lisätään ja yhdistetty VMs, tarjoamalla connectivity eri toiminnoista.

Aliverkosta sisältää seuraavat ominaisuudet. 

|Ominaisuus|Kuvaus|Otoksen arvot|
|---|---|---|
|**addressPrefix**|Yksittäisen osoitteen etuliitteen, jotka muodostavat CIDR merkintätavan aliverkon|192.168.1.0/24|
|**networkSecurityGroup**|Käytetty aliverkon NSG|Katso [NSGs](#Network-Security-Group)|
|**routeTable**|Käytetty aliverkon taulussa|Katso [UDR](#Route-table)|
|**ipConfigurations**|IP-configruation objektien NIC aliverkon yhteydessä käyttämien kokoelma|Katso [UDR](#Route-table)|


Esimerkki VNet JSON-muodossa:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Lisäresursseja

- Lisätietoja [VNet](../articles/virtual-network/virtual-networks-overview.md).
- Tässä artikkelissa [REST API-oppaat](https://msdn.microsoft.com/library/azure/mt163650.aspx) VNets.
- Lue aliverkon [REST API-oppaat](https://msdn.microsoft.com/library/azure/mt163618.aspx) .