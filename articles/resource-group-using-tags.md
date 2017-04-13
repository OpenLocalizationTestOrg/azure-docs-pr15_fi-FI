<properties
    pageTitle="Tunnisteiden avulla voit järjestää Azure resurssien | Microsoft Azure"
    description="Näyttää, miten tunnisteiden järjestämiseen resurssien laskutus ja hallintaa varten."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Tunnisteiden avulla voit järjestää Azure resurssit

Resurssien hallinnan avulla voit järjestää loogisesti lisäämällä niihin tunnisteita resurssit. Tunnisteet koostuvat, jotka määrittävät resursseja ominaisuudet, jotka määrität avain/arvo-pareina. Voit merkitä resurssit kuin samaan luokkaan kuuluvat käyttää samaa tunnistetta resurssit.

Kun tarkastelet tietyn tunnisteen resursseja, näet resurssien kaikki resurssi-ryhmistä. Et ole rajoitettu vain saman resurssiryhmä, jonka avulla voit järjestää niin, että on erillinen käyttöönoton yhteydet resurssien resurssit. Tunnisteita voi olla hyötyä, kun haluat järjestää resurssien laskutus tai hallinta.

Tilauksen laajuisen luokituksen lisätään automaattisesti kunkin tunnisteen, voit lisätä resurssi tai resurssiryhmä. Voit myös määrittää luokituksen valmiiksi tilauksen tunnisteiden nimet ja haluat käyttää resursseja on merkitty tulevaisuudessa arvot.

Kunkin resurssi tai resurssiryhmä voi olla enintään 15 tunnistetta. Tunnisteen nimi on 512 merkkiä ja tunniste-arvo on enintään 256 merkkiä.

> [AZURE.NOTE] Voit käyttää tunnisteita vain resursseille, jotka tukevat Resurssienhallinta-toimintoja. Jos olet luonut virtuaalikoneen, VPN ja tallennustilaa – perinteinen käyttöönottomalli (kuten – perinteinen portaalin), et voi käyttää tunnisteen resurssin. Tukemaan tunnisteita käyttöön näiden resurssien avulla Resurssienhallinta. Muita resursseja tukevat tunnisteita.

## <a name="templates"></a>Mallit

Merkitse resurssin käyttöönoton aikana, riittää, että otat resurssin **tunnisteet** -osan lisääminen ja tunnistenimi ja arvo. Tunnistenimi ja arvo ei tarvitse valmiiksi olemassa tilauksen. Voit kirjoittaa enintään 15 tunnistetta kullekin resurssille.

Seuraavassa esimerkissä esitetään tallennustilan-tilin kanssa tunnisteen.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Resurssienhallinta tällä hetkellä tue käsittelyn objektin tunnisteiden nimet ja arvot. Välittää sen sijaan objektin tunniste-arvoja, mutta on silti määritettävä tunnisteen nimiä, kuten seuraavassa esimerkissä.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShellin

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Portaalin ja PowerShell sekä käyttää [Resurssienhallinta REST API](https://msdn.microsoft.com/library/azure/dn848368.aspx) taustalla. Jos tarvitset integroiminen toiseen ympäristöön tunnisteita, voit tunnisteet sisältämistä GET resurssitunnus- ja päivittää tunnisteet joukko KORJAUSTIEDOSTON puhelun.


## <a name="tags-and-billing"></a>Tunnisteet ja laskutus

Tuetut palveluiden laskutuksen tietojen ryhmitteleminen tunnisteiden avulla. Esimerkiksi [näennäiskoneiden integroitu Azure resurssien hallinta](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) , joiden avulla voit määrittää ja tunnisteita, voit järjestää näennäiskoneiden laskutuksen käyttö. Jos käytössäsi on useita VMs eri organisaatioissa, voit käyttää tunnisteiden ryhmän käyttö kustannuspaikka mukaan.  
Tunnisteiden avulla voit myös luokittelevat kustannukset runtime-ympäristöä, kuten tuotantoympäristössä virtuaalilaitteiksi laskutuksen käyttö.

Voit noutaa tietoja tunnisteet [Azure resurssien käyttö- ja RateCard API](billing-usage-rate-card-overview.md) - tai käyttö pilkuilla erotetut arvot (CSV)-tiedoston avulla. Käyttö-tiedosto ladataan [Azure tilit portal](https://account.windowsazure.com/) tai [EA portaalin](https://ea.azure.com). Saat lisätietoja laskutustiedot ohjelmoitua pääsyä [Hanki tuominen Microsoft Azure-resurssin kulutus tietoja](billing-usage-rate-card-overview.md). Artikkelissa [Azure Laskutus REST API viittaus](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)REST API-toimintoja.

Kun lataat palvelut, jotka tukevat laskutuksesta tunnisteiden käytön CSV-tunnisteet näkyvät **tunnisteet** -sarakkeessa. Lisätietoja on artikkelissa [Microsoft Azure saat laskun ymmärtäminen](billing/billing-understand-your-bill.md).

![Katso Laskutus-tunnisteet](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Voit käyttää rajoitukset ja nimeämiskäytännön yli ‑tilauksen mukautettuja käytäntöjä. Voit määrittää käytännön saattaa edellyttää, että kaikki resurssit on tietyn tunnisteen arvo. Lisätietoja on artikkelissa [Käyttöä koskevasta käytännöstä resurssien hallintaa ja hallita](resource-manager-policy.md).
- Katso esittely avulla PowerShellin Azure, kun otat resurssit, [Azure PowerShellin Azure resurssien hallinta](./powershell-azure-resource-manager.md).
- Katso esittely avulla Azure CLI, kun otat resurssit, [Azure-CLI Mac, Linux-ja Windows Azure resurssien hallinnan avulla](./xplat-cli-azure-resource-manager.md).
- Katso esittely-portaalissa, [Voit hallita Azure resursseja Azure-portaalissa](./azure-portal/resource-group-portal.md)  
