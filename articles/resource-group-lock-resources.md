<properties 
    pageTitle="Lukitse resurssien hallinnan resurssit | Microsoft Azure" 
    description="Estä käyttäjiä päivittäminen tai poistaminen lisäämällä rajoitus kaikkien käyttäjien ja roolien tietyt resurssit." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Lukitse Azure resurssien hallinnan resurssit

Järjestelmänvalvoja voit joutua tilauksen, resurssiryhmä tai resurssi estää muita käyttäjiä organisaation poistetaan vahingossa tai muokkaamasta kriittinen resurssien lukitseminen. Voit määrittää Lukitse tason **CanNotDelete** tai **vain luku-tilassa**. 

- **CanNotDelete** tarkoittaa valtuutetut käyttäjät voivat lukea ja muokata resurssin edelleen, mutta niitä ei voi poistaa. 
- **Vain luku** tarkoittaa valtuutetut käyttäjät voivat lukea resurssin, mutta he eivät voi poistaa tai tehdä siihen mitään toimia. Resurssin käyttöoikeudet on rajoitettu **lukija** . 

**Vain luku-tilassa** käyttämällä voi aiheuttaa odottamattomia tuloksia koska joitakin toimintoja, jotka näyttävät, kuten luku-toiminnoissa on todella muita toimintoja. Esimerkiksi **vain luku** -lukko markkinoille tallennustilan tilin estää kaikki luettelon näppäimet. Näppäimet-toiminto käsitellään POST-pyynnössä kautta, koska palautetut pikanäppäimet ovat käytettävissä luettelon kirjoitus-toimintoja. Toinen esimerkki sijoittaminen **vain luku** -Lukitse-sovelluksen palvelun resurssin estää Visual Studio palvelimen Explorer näyttäminen resurssin tiedostoja, koska edellyttää, että vuorovaikutus kirjoitusoikeudet.

Toisin kuin Roolipohjainen käyttöoikeuksien valvonta Käytä lukitusten hallinta välittyminen kaikkiin kaikkien käyttäjien ja roolien rajoitus. Lisätietoja käyttäjien ja roolien käyttöoikeuksien määrittämisestä on artikkelissa [Azure Roolipohjainen käyttöoikeuksien valvonta](./active-directory/role-based-access-control-configure.md).

Kun otat käyttöön lukko pääkohde-alueessa, kaikki lapsen resurssit perivät saman lukitus. Lisätään myöhemmin jopa resurssien Peri lukituksen ylemmän tason. Useimmat rajoittava lukituksen periytymisen ohittaa.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Kuka voi luoda tai poistaa lukitukset organisaation

Voit luoda tai poistaa lukitusten hallinta, on pääsy **Microsoft.Authorization/\* ** tai **Microsoft.Authorization/locks/\* ** toiminnot. Valmiiden roolien **omistaja** ja **Käyttäjän käyttöoikeus järjestelmänvalvoja** myönnetään nämä toiminnot.

## <a name="creating-a-lock-through-the-portal"></a>Lukitse – portaalin luominen

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Lukko luominen mallista

Seuraavassa esimerkissä on malli, joka luo lukko tallennustilan tilille. Tallennustilan tili, jos haluat käyttää lukitus on annettu parametrina. Lukitse nimi luodaan ketjuttamalla **/Microsoft.Authorization/** resurssinimi ja lukitusta tämän palvelupyynnön **myLock**nimi.

Resurssin laji on annettu tyyppi. Määritä tallennuspaikkaa, kirjoita "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Lukko luominen REST-ohjelmointirajapinnalla

Voit lukita käyttöön resurssien [REST-Ohjelmointirajapinnalla lukitusten hallinta](https://msdn.microsoft.com/library/azure/mt204563.aspx). REST API mahdollistaa luominen ja poistaminen lukitukset ja hakea tietoja aiemmin lukitukset.

Voit luoda lukko suorittamalla:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Vaikutusalueen voi olla tilauksen, resurssiryhmä tai resurssi. Lukitse-nimi on riippumatta siitä, mitä, johon haluat soittaa lukitus. Käytä **2015-01-01**api-versio.

Sisältää pyynnön JSON-objekti, joka määrittää lukituksen ominaisuudet.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Katso esimerkkejä [REST-Ohjelmointirajapinnalla lukitusten hallinta](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Luomalla PowerShellin Azure lukko

Voit lukita PowerShellin Azure käyttöön resursseja käyttämällä **Uutta AzureRmResourceLock** , kuten seuraavassa esimerkissä.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell tarjoaa muut komennot teki lukitukset, kuten **Määrittäminen AzureRmResourceLock** päivittämään lukko ja **Poista AzureRmResourceLock** lukituksen poistaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso lisätietoja käyttämisestä resurssien lukitukset [Lukitse alaspäin Your Azure resurssit](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Lisätietoja tietojen järjestäminen loogisesti resurssien on artikkelissa [käyttäminen tunnisteen, kun haluat järjestää resurssit](resource-group-using-tags.md)
- Resurssiryhmä, johon resurssi sijaitsee muuttamisesta on ohjeaiheessa [resurssien uusi resurssiryhmä siirtäminen](resource-group-move-resources.md)
- Voit käyttää rajoitukset ja nimeämiskäytännön yli ‑tilauksen mukautettuja käytäntöjä. Lisätietoja on artikkelissa [Käyttöä koskevasta käytännöstä resurssien hallintaa ja hallita](resource-manager-policy.md).
