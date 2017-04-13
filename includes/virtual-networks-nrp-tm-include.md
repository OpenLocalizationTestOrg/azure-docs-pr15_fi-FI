## <a name="traffic-manager-profile"></a>Profiilin liikenteen hallinta

Liikenteen hallinta ja sen ali päätepisteen resurssin tukevat päätepisteiden Azure-tietokannassa ja sen ulkopuolisten Azure DNS-reititystä. Tällainen liikenne jakauman säätelevät reititys käytännön tavoista. Liikenteen hallinta mahdollistaa myös päätepisteen kunto seurattava ja liikenne vientitarkoituksessa asianmukaisesti perusteella päätepisteen kunto. 

| Ominaisuus | Kuvaus |
|---|---|
|**trafficRoutingMethod**| mahdolliset arvot ovat *suorituskykyä*ja *painotettu* *prioriteetti* | 
| **dnsConfig** | Profiilin FQDN | 
| **Protokolla** | seuranta-protokolla, mahdolliset arvot ovat *HTTP* ja *HTTPS*|
| **Port (portti)** | valvonta |  
| **Polku** | seurannan polku |
| **Päätepisteet** |  päätepisteen resurssit-säilö | 

### <a name="endpoint"></a>Päätepisteen 

Päätepisteen on Aliobjektin resurssi liikenteen hallinta profiilin. Palvelu on tai web päätepisteen, jolla liikenne jaetaan perusteella määritetty käytäntö liikenteen hallinta profiilin resurssista. 

| Ominaisuus | Kuvaus | 
|---|---| 
| **Tyyppi** |  tyyppi päätepisteen, mahdolliset arvot ovat *Azure loppuun*, *Ulkoinen päätepiste*ja *Ylemmän tason päätepiste* | 
| **targetResourceId** |  julkiseen IP-osoite tai web-palvelu päätepisteen. Tämä voi olla Azure tai ulkoinen päätepiste. | 
| **Weight (paino)** | päätepisteen paino käyttää liikenteen hallinta. | 
| **Priority (prioriteetti)** | prioriteetti päätepisteen avulla määritetään automaattisesti-toiminto |

Otoksen liikenne hallinta Json-muodossa: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Lisäresursseja

Lue lisätietoja [REST API-ohjeissa liikenteen hallinta](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
