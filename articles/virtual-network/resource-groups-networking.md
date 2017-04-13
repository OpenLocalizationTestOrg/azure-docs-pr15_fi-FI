<properties
   pageTitle="Verkko-palveluntarjoajan Resurssikatsaus | Microsoft Azure"
   description="Lue lisää uusi resurssi verkkopalvelun Azure resurssien hallinta"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Resurssin verkkopalvelun
Nykyisen business onnistumista taustasovellus tarvetta on voi luoda ja hallita suuren mittakaavan verkkosovellukset huomioon joustava, joustava, suojattu ja toistettavien tavalla. Azure resurssien hallinta (ARM) avulla voit luoda näiden sovellusten kuin yhden kokoelma resursseja resurssin ryhmissä. Näiden resurssien hallitaan eri resurssin tarjoajat ARM-kohdassa.

Azure Resurssienhallinta on riippuvainen eri resurssin tarjoajat pääsy resurssit. On kolme ensisijainen resurssi tarjoajat: verkko-, tallennus- ja Laske. Tässä asiakirjassa kerrotaan ominaisuudet ja edut resurssin verkkopalvelun, mukaan lukien:

- **Metatietojen** – voit lisätä resursseja tunnisteiden tiedot. Tunnisteet voidaan resurssien käytön seuraat resurssiryhmät ja tilaukset.
- **Ohjata verkoston** - resurssit ovat löyhästi verkoston ja hallita niitä eritellympiä tavalla. Tämä tarkoittaa sitä, että joustavammin verkko resurssien hallinta.
- **Nopeampi määritys** - koska verkkoresursseja ovat löyhästi, voit luoda ja orchestrate verkkoresursseja rinnakkain. Tämä on pienennetty merkittävästi määrityksen aika.
- Oletusroolit tietyn suojauksen laajuutta lisäksi mukautettuja rooleja suojatun hallinnan luomisen sallimisesta tarjoaa **Roolin perusteella käyttöoikeuksien valvonta** - RBAC.
- **Helpompi hallinta ja käyttöönotto** - on helpompi käyttöön ja hallita sovellusten jälkeen voit voit luoda koko sovelluksen pinon kuin yhden kokoelma resursseja resurssiryhmän. Ja nopeampi tapa ottaa käyttöön, koska voit ottaa käyttöön tarjoamalla yksinkertaisesti mallia JSON-paketti.
- **Nopea mukauttaminen** - määritettäviä tyylin mallien avulla voit käyttöön ominaisuuksissa toistettavien ja nopea mukauttaminen.
- **Toistettavien mukauttaminen** - määritettäviä tyylin mallien avulla voit käyttöön ominaisuuksissa toistettavien ja nopea mukauttaminen.
- **Hallinnan liittymät** - jokin seuraavista liittymät avulla voit hallita resursseja:
    - REST API perusteella
    - PowerShellin
    - .NET SDK-PAKETISSA
    - Node.JS SDK-paketissa
    - Java SDK-paketissa
    - Azure CLI
    - Esikatselu-portaalissa
    - ARM-mallin kieli

## <a name="network-resources"></a>Verkkoresursseja
Voivat nyt hallita verkkoresursseja itsenäisesti sen sijaan, että ne kaikki hallitaan Laske yhteen resurssin (virtual machine). Tällä varmistetaan, että suurempi aste joustavuutta ja joustavuutta-sähköpostiviestiä monimutkaisia ja suurten asteikko-infrastruktuuria resurssiryhmä.

Käsitteellinen kuva henkilöihin liittyvät usean tasoisen sovelluksen otoksen käyttöönoton esitetään jäljempänä. Kullekin resurssille, näet, kuten NIC, julkiseen IP-osoitteet ja VMs, voidaan hallita erikseen.

![Verkon resurssin malli](./media/resource-groups-networking/Figure2.png)

Resurssi on yhteiset ominaisuudet ja niiden yksittäiset ominaisuusjoukko. Yhteiset ominaisuudet ovat:

|Ominaisuus|Kuvaus|Otoksen arvot|
|---|---|---|
|**Nimi**|Yksilöivä resurssinimi. Oma nimeämiskäytäntö rajoituksista on kunkin resurssilaji.|PIP01, VM01, NIC01|
|**sijainti**|Azure alue, johon resurssi sijaitsee|westus, eastus|
|**tunnus**|Yksilöivä mukaan URI-tunnus|/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Voit tarkistaa seuraavien resurssien yksittäisiä ominaisuuksien.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Liityntäkohdat hallinta
Voit hallita käyttämällä eri liityntäkohdat Azure verkko resursseja. Tässä asiakirjassa keskitytään saadaan kyseisten rajapintojen: REST API ja malleja.

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA
Kuten edellä mainittiin, verkkoresursseja voi hallita liittymät, mukaan lukien REST API, .NET SDK, Node.JS SDK-paketissa, Java SDK-paketissa, PowerShell, CLI, Azure-portaalin ja malleja eri kautta.

Rest API mukainen HTTP 1.1-protokollaa määritystä. Yleiset URI rakenteen Ohjelmointirajapinnan esitetään alla:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

Ja parametrit aaltosulkeiden sisällä edustavat seuraavat osat:

- **tilauksen tunnus** - Azure Tilaustunnus.
- **resurssi-palvelu-nimitilan** - nimitilan palveluntarjoajasi käytössä. Resurssin verkkopalvelun arvo *Microsoft.Network*.
- **alueen nimi** - Azure alueen nimi

REST API soittamista sovelluksia tuetaan HTTP seuraavista tavoista:

- **SIJOITA** - resurssin tietyntyyppinen, luomiseen käytetyt resurssi-ominaisuuden tai muuttaa resurssien välinen suhde.
- **HAE** - valmistellun resurssin tietojen hakemiseen käytetty.
- **Poista** - käytetään poistamaan aiemmin luotu resurssi.

Pyynnön ja vastauksen mukainen JSON-paketti-muoto. Lisätietoja on artikkelissa [Azure resurssien hallinnan API](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>ARM-mallin kieli
Lisäksi imperatively (joko ohjelmointirajapinnan tai SDK) resurssien hallinta, voit käyttää myös määritettäviä ohjelmoinnin tyylin luominen ja hallinta verkkoresursseihin KÄDESSÄ malli-kielen avulla.

Alla on annettu mallin otoksen esittäminen

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Malli on ensisijaisesti JSON kuvaus resursseja ja esiintymän arvot injektiona parametrit kautta. Alla olevassa esimerkissä voidaan luoda virtuaalisen verkon 2 aliverkosta.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Voit halutessasi käytetäänkin parametriarvot manuaalisesti mallin avulla tai voit käyttää parametri-tiedosto. Alla olevassa esimerkissä parametriarvot yllä mallia käytetään mahdollinen joukko:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Mallien käyttäminen eduista on:

- Voit luoda monimutkaisia infrastruktuuri-resurssiryhmä määritettäviä tyylillä. ARM käsittelee tiedonsiirron resurssit, kuten hallinnan riippuvuuden luomista.
- Infrastruktuuri voi luoda toistettavien tavalla eri alueiden välillä ja alueella yksinkertaisesti vaihtamalla parametrit.
- Määritettäviä tyylin johtaa lyhentää mallien luomisesta ja toteuttaa infrastruktuurin limitysajasta.

Katso esimerkkejä malleista [Azure pikaopas malleja](https://github.com/Azure/azure-quickstart-templates).

Lisätietoja ARM-mallin kieli on artikkelissa [Azure Resurssienhallinta mallin kieli](../resource-group-authoring-templates.md).

Esimerkki mallin yllä käyttää VPN ja aliverkon resurssit. On muita verkkoresursseja, voit käyttää seuraavia ohjeita:

### <a name="using-a-template"></a>Mallin avulla

Voit ottaa käyttöön services Azure mallista käyttämällä PowerShell-AzureCLI, tai voit ottaa GitHub suorittamiseen. Ottamaan services GitHub mallista Suorita seuraavat toimet:

1. Avaa tiedosto template3 GitHub. Esimerkkinä Avaa [Virtual verkossa, jossa on kahden aliverkon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Napsauta **käyttöönotto Azure**ja valitse Kirjaudu sisään Azure-portaaliin ja tunnistetiedot.
3. Tarkista malli ja valitse sitten **Tallenna**.
4. Valitse **Muokkaa parametrit** ja valitse sijainti, kuten *Länsi Yhdysvaltain*vnet ja aliverkosta.
5. Valitse tarvittaessa muuttaa **ADDRESSPREFIX** ja **SUBNETPREFIX** parametrit ja valitse sitten **OK**.
6. Valitse **Valitse resurssiryhmässä** ja valitse sitten resurssin ryhmä, johon haluat lisätä vnet ja aliverkosta. Voit myös luoda uusi resurssiryhmä valitsemalla **tai luoda uusia**.
3. Valitse **Luo**. Huomaa **valmistelu mallin käyttöönottoa**näyttäminen vierekkäin. Kun asennus on valmis, näet jokin näyttöön alla.

![Esimerkki mallia käyttöönotto](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Seuraavat vaiheet

[Azure Resurssienhallinta mallin kieli](../resource-group-authoring-templates.md)

[Azure verkko – käytetyt mallit](https://github.com/Azure/azure-quickstart-templates)

[Laske resurssin tarjoajaan](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)
