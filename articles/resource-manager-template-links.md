<properties
   pageTitle="Resurssienhallinta mallin linkittämisen resurssien | Microsoft Azure"
   description="Näyttää Resurssienhallinta rakenteen käyttöönottoon liittyvät resurssit kautta mallin välisten linkkien tarkasteleminen."
   services="azure-resource-manager"
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

# <a name="resource-links-template-schema"></a>Resurssin linkit mallin rakenne

Luo kaksi resurssia välisen linkin. Linkki otetaan käyttöön tietolähteen resurssin nimeltään resurssin. Linkin toisessa resurssin kutsutaan kohde resurssi.

## <a name="schema-format"></a>Rakenteen muoto

Jos haluat luoda linkin, lisää seuraavan rakenteen mallin resurssit-osa.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Arvot

Seuraavissa taulukoissa kuvataan arvot, jotka on määritelty määrittämiseen.

| Nimi | Arvo |
| ---- | ---- |
| tyyppi | Luettelo<br />Pakollinen<br />**{nimitilan} / {tyypin} / tarjoajien/linkit**<br /><br />Resurssin laji luomiseen. {Nimitilan} {} arvot ja lähteen resurssin tarjoajan nimitila ja resurssin tyyppi. |
| apiVersion | Luettelo<br />Pakollinen<br />**2015-01-01**<br /><br />API-versio, jonka avulla voi luoda resurssin. |  
| Nimi | Merkkijono<br />Pakollinen<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> enintään 64 merkkiä, eikä se saa sisältää <>, %, ja -?, tai jokin ohjausmerkkejä.<br /><br />A-arvo, määrittää sekä lähde resurssin nimi ja linkin nimi. |
| dependsOn | Matriisi<br />Valinnainen<br />Pilkuilla erotettu luettelo resurssin nimiä tai resurssin yksilöivää tunnistetta.<br /><br />Resurssien tätä linkkiä määräytyy sivustokokoelman. Jos linkität resursseja on otettu käyttöön samaan malliin, Sisällytä tämä elementti, jotta ne on otettu käyttöön ensin resurssien nimet. | 
| Ominaisuudet: | Objektin<br />Pakollinen<br />[objektin ominaisuudet](#properties)<br /><br />Objekti, joka yksilöi resurssin linkittäminen ja huomautuksia linkkiä. |  

<a id="properties" />
### <a name="properties-object"></a>objektin ominaisuudet

| Nimi | Arvo |
| ------- | ---- |
| targetId | Merkkijono<br />Pakollinen<br />**{resurssitunnus}**<br /><br />Linkitä kohde-resurssin tunnus. |
| Huomautuksia | Merkkijono<br />Valinnainen<br />enintään 512 merkkiä<br /><br />Lukitse kuvaus. |


## <a name="how-to-use-the-link-resource"></a>Linkki resurssien käyttäminen

Voit käyttää kahta resursseja, kun resursseja on riippuvuus, joka jatkuu käyttöönoton jälkeen välisen linkin. Esimerkiksi sovelluksen voivat muodostaa yhteyden eri resurssiryhmä-tietokantaan. Voit määrittää kyseisen riippuvuuden luomalla linkki sovelluksen tietokanta. Linkkien avulla voit asiakirjan kahden resurssin välisen suhteen. Myöhemmin sinun tai jonkun muun organisaation tehdä kyselyn resurssin linkkejä saat tietää, kuinka resurssin toimii muita resursseja.

Kaikki linkitetyt resurssit on kuuluttava samaan tilaukseen. Kullekin resurssille voidaan linkittää 50 muita resursseja. Jos linkitetty resursseista on poistettu tai siirretty, linkki omistaja on Puhdista jäljellä oleva linkki.

Toimimaan linkkien kautta muiden nähdä [Linkitetyn resurssit](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Seuraavat PowerShellin Azure-komennon avulla voit tarkastella kaikkia tilauksen linkkiä. Voit antaa muita parametreja, jos haluat rajoittaa tuloksia.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Esimerkkejä

Seuraava esimerkki koskee vain luku-lukko verkkosovellukseen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Pikaopas-mallit

Seuraavat pikaopas-mallit käyttöön resursseja, jossa on linkki.

- [Ilmoita jonon logiikan-sovelluksessa](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Logiikan sovelluksen liukuma ilmoitus](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [API-sovelluksessa, jossa aiemmin yhdyskäytävän valmistelu](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Valmistele API-sovelluksessa, jossa uusi yhdyskäytävä](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Mallin avulla sekä logiikan sovellus että API sovelluksen luominen](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logiikan-sovellusta, joka lähettää tekstiviestin, kun ilmoituksen käynnistyy](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja mallin rakenne on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
