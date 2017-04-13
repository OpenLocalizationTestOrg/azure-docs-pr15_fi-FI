## <a name="download-and-understand-the-arm-template"></a>Lataa ja ymmärrät ARM-malli

Voit ladata aiemmin luodun ARM mallin luomisesta VNet ja kahden aliverkon github, muutokset saattavat haluat ja käyttää sitä uudelleen. Voit tehdä, noudata seuraavia ohjeita.

1. Siirry [malli malli-sivulta](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Valitse **azuredeploy.json**ja valitse sitten **raaka**.
3. Tallenna tiedosto tietokoneen paikalliseen kansioon.
4. Jos olet tutustunut ARM-mallit, siirry vaiheeseen 7.
5. Avaa tallentamasi tiedoston ja tarkista sisältö rivi 5 **Parametrit** -kohdassa. ARM-Malliparametrien avulla paikkamerkin arvoja, jotka voi täyttää käyttöönoton aikana.

    | Parametri | Kuvaus |
    |---|---|
    | **sijainti** | Azure alue, jossa VNet luodaan |
    | **vnetName** | Uusi VNet nimi |
    | **addressPrefix** | VNet CIDR muodossa osoitetilaa |
    | **subnet1Name** | Ensimmäinen VNet nimi |
    | **subnet1Prefix** | Ensimmäinen aliverkon CIDR estäminen |
    | **subnet2Name** | Toinen VNet nimi |
    | **subnet2Prefix** | Toinen aliverkon CIDR estäminen |

    >[AZURE.IMPORTANT] ARM-mallien ylläpidetään github voivat muuttua ajan myötä. Varmista, että tarkistat ennen sen käyttämistä malli.
    
6. Tarkista **resurssit** -kohdassa sisältö ja huomaat seuraavasti:

    - **tyyppi**. Resurssin luomisen mallin tyyppi. Tässä tapauksessa **Microsoft.Network/virtualNetworks**, jotka edustavat VNet.
    - **nimi**. Resurssin nimi. Huomaa **[parameters('vnetName')]**, mikä tarkoittaa nimi näkyy syötteeksi käyttäjä tai parametrin tiedoston käyttöönoton aikana käyttö.
    - **Ominaisuudet**. Resurssin ominaisuudet luettelo. Tämä malli käyttää osoitteen tilaa ja aliverkon ominaisuudet VNet luonnin aikana.

7. Siirry takaisin [malli malli-sivulta](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Valitse **azuredeploy paremeters.json**ja valitse sitten **raaka**.
9. Tallenna tiedosto tietokoneen paikalliseen kansioon.
10. Avaa tallentamasi tiedoston muokattavaksi parametrien arvot. Tässä skenaariossa on kuvattu VNet käyttöön alla olevista arvoista.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Tallenna tiedosto.
  