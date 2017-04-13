<properties 
    pageTitle="Linkittäminen Azure resurssien hallinnan resurssit | Microsoft Azure" 
    description="Luo linkki eri resurssin ryhmien Azure Resurssienhallinta liittyvät resurssit välillä." 
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
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Linkittäminen Azure resurssien hallinnan resurssit

Käyttöönoton aikana voit merkitä toiselle resurssille määräytyy resurssin, mutta kyseisen elinkaari päättyy käyttöönotto. Käyttöönoton jälkeen yhteyttä ei ole tunnistettujen riippuvaiset resurssien välillä. Resurssienhallinta sisältää toiminnon resurssin linkittäminen pysyvä resurssien väliset suhteet.

Resurssin linkität, jossa voit asiakirjan yhteydet, jotka ulottuvat resurssiryhmät. Esimerkiksi on yhteinen tietokanta on oltava oma elinkaari sijaitsevat yksi resurssiryhmä ja sovelluksen kanssa eri elinkaari sijaitsevat eri resurssiryhmä. Sovelluksen muodostaa yhteyden tietokantaan, joten haluat merkitä sovellus ja tietokannan välisen linkin. 

Kaikki linkitetyt resurssit on kuuluttava samaan tilaukseen. Kullekin resurssille voidaan linkittää 50 muita resursseja. Ainoa tapa kyselyn liittyvät resurssit on REST API-Liittymän kautta. Jos linkitetty resursseista on poistettu tai siirretty, linkki omistaja on Puhdista jäljellä oleva linkki. Olet varoitetaan **ei** -toiminnon poistaminen resurssi, joka on linkitetty muita resursseja.

## <a name="linking-in-templates"></a>Malleihin linkittäminen

Määritä linkki mallin-sisällytät Resurssityyppi, joka yhdistää resurssin tarjoajan nimitilan ja **/providers/links**resurssin lähteen tyyppi. Nimen on oltava lähde-resurssin nimi. Sinun on määritettävä kohdesovelluksen resurssin resurssitunnus. Seuraavassa esimerkissä muodostaa linkin web-sivuston ja tallennustilaa tilin.

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


Katso täydellinen kuvaus mallimuoto [resurssin linkit - mallin rakenne](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Linkittäminen REST-ohjelmointirajapinnalla

Voit määrittää linkin käyttöön resurssien välillä, suorittamalla:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Korvaa {Tilaustunnus} tilauksen tunnus. Korvaa {resurssi-ryhmä} {tarjoajan nimitilan, {resurssi-tyyppi}, {resurssi- nimi} arvot, jotka tunnistaa linkin ensimmäisen resurssin. Korvaa {linkin nimi} voit luoda linkin nimi. Käytä 2015-01-01 api-version.

Pyynnön Sisällytä objekti, joka määrittää toisen resurssin linkkiä:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Ominaisuudet-osa sisältää toisen resurssin tunnus.

Voit tehdä kyselyn linkit tilaukseesi kanssa:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Lisää esimerkkejä mukaan lukien linkkejä koskevien tietojen hakeminen on [Linkitetty](https://msdn.microsoft.com/library/azure/mt238499.aspx)resursseissa.

## <a name="next-steps"></a>Seuraavat vaiheet

- Voit myös järjestää resurssien ja tunnisteita. Tietoja tunnisteiden resurssit-kohdassa [käyttäminen tunnisteen, kun haluat järjestää resurssit](resource-group-using-tags.md).
- Kuvaus siitä, miten voit luoda malleja ja määrittää resurssien avulla voidaan ottaa käyttöön on kohdassa [julkaisu-mallit](resource-group-authoring-templates.md).
