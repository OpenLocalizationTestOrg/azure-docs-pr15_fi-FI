<properties
   pageTitle="Resurssienhallinta mallin tallennustilan | Microsoft Azure"
   description="Näyttää Resurssienhallinta rakenteen käyttöönotto mallin tallennustilan-tilit."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Tallennustilan tilin mallin rakenne

Luo tallennustilan tilin.

## <a name="schema-format"></a>Rakenteen muoto

Tallennustilan tilin luominen lisää seuraavan rakenteen lomakemalliin resurssit-osa.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Arvot

Seuraavissa taulukoissa kuvataan arvot, jotka on määritelty määrittämiseen.

| Nimi | Arvo |
| ---- | ---- |
| tyyppi | Luettelo<br />Pakollinen<br />**Microsoft.Storage/storageAccounts**<br /><br />Resurssin laji luomiseen. |
| apiVersion | Luettelo<br />Pakollinen<br />**2015-06-15** tai **2015-05-01-esikatselu**<br /><br />API-versio, jonka avulla voi luoda resurssin. | 
| Nimi | Merkkijono<br />Pakollinen<br />3-24 merkkiä, numeroita ja pienet kirjaimet.<br /><br />Luo tallennustilan tilin nimi. Nimen on oltava yksilöllinen kaikissa Azure. Harkitse [uniqueString](resource-group-template-functions.md#uniquestring) -funktion käyttäminen nimeämiskäytännön mukaisesti, kuten alla olevassa esimerkissä. |
| sijainti | Merkkijono<br />Pakollinen<br />Alue, joka tukee tallennustilan tilit. Kelvollinen alueet-kohdassa [Tuetut alueet](resource-manager-supported-services.md#supported-regions).<br /><br />Tallennustilan tilin isännöimiseen alue. |
| Ominaisuudet: | Objektin<br />Pakollinen<br />[objektin ominaisuudet](#properties)<br /><br />Objekti, joka määrittää luomaan tallennustilan tilin tyyppi. |

<a id="properties" />
### <a name="properties-object"></a>objektin ominaisuudet

| Nimi | Arvo |
| ---- | ---- | 
| accountType | Merkkijono<br />Pakollinen<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**tai **Premium_LRS**<br /><br />Tallennustilan tilin tyyppi. Sallitut arvot vastaavat Vakio paikallisesti tarpeettomat, vakio vyöhykkeen tarpeettomat, vakio Geo-Redundant, Vakio-ja lukuoikeudet Geo Redundant ja Premium paikallisesti tarpeettomat. Nämä tilityypit tietoja on artikkelissa [Azure-tallennustilan replikoinnin](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Esimerkkejä

Seuraavassa esimerkissä ottaa käyttöön vakio paikallisesti tarpeettomat tallennustilan-tili on yksilöllinen nimi resurssin ryhmätunnuksen perusteella.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Pikaopas-mallit

On monia pikaopas malleilla, jotka sisältävät tallennustilan tilin. Seuraavat mallit kuvaavat havainnollistetaan Yleisiä tilanteita:

- [Vakio-tallennustilan tilin luominen](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Windows-AM yksinkertainen käyttöönotto](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Yksinkertainen Linux AM käyttöönotto](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Luo CDN profiili, CDN-päätepisteen tallennustilan tilillä origin nimellä](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Suuri Availabilty SharePoint-klusterin luominen 9 VMs käyttämällä Powershell DSC-tunniste](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Yksinkertainen käyttöönotto 5 solmu suojatun palvelun kangasta klusteri ja WAD käytössä](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Windows-kuva, jossa 4 tyhjä tietonäkymä-levyjä Virtual Machine luominen](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Seuraavat vaiheet

- Yleisiä tietoja tallennustila on artikkelissa [Microsoft Azuren tallennustilaan esittely](./storage/storage-introduction.md).
- Esimerkiksi malleilla, jotka uuden tallennustilan tilin käyttäminen Virtual Machine-kohdassa [Ota käyttöön yksinkertainen Linux AM](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) tai [Ota käyttöön yksinkertainen Windows-AM](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
