<properties
   pageTitle="Poista Azure klusterin ja resursseja | Microsoft Azure"
   description="Katso, miten poistaa kokonaan palvelun kangasta klusterin joko poistaminen resurssiryhmä, joka sisältää klusterin tai poistamalla valikoivasti resurssit."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>

# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a>Poista palvelun kangasta klusterin Azure ja se käyttää resursseihin

Palvelun kangasta klusterin koostuu muiden monta Azure resurssien lisäksi klusteriresurssin itse. Niin, jos haluat poistaa palvelun kangasta-klusterin, sinun on poistettava kaikki resurssit, se on tehty, myös kokonaan.
Sinulla on kaksi vaihtoehtoa: joko Poista resurssiryhmä, johon klusterin (joka poistaa klusteriresurssi ja muita resursseja resurssiryhmän) tai poista erityisesti klusteriresurssi ja siihen liittyy resurssit (mutta ei muita resursseja resurssiryhmän).

>[AZURE.NOTE] Poistaminen klusterin resurssin **ei** Poista muita resursseja, joka koostuu palvelun kangasta-klusterin.

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a>Koko resurssiryhmän (RG), joka on palvelun kangasta klusterin poistaminen

Tämä on helpoin tapa varmistaa, että poistat kaikki yhteyttä klusterin, mukaan lukien resurssiryhmän liittyvät resurssit. Voit poistaa PowerShellin resurssiryhmä tai palvelun Azure-portaalissa. Jos resurssiryhmä on resursseja, jotka eivät ole palvelun kangasta klusterin, voit poistaa tietyn resurssit.

### <a name="delete-the-resource-group-using-azure-powershell"></a>Poista resurssiryhmän Azure PowerShellin avulla

Voit myös poistaa resurssiryhmän suorittamalla seuraavat Azure PowerShell cmdlet-komennot. Varmista, että Azure PowerShell 1.0 tai uudempi on asennettu tietokoneeseen. Jos et ole tehnyt tämän ennen, noudata artikkelin [asentaminen ja määrittäminen PowerShellin Azure.](../powershell-install-configure.md)

Avaa PowerShell-ikkuna ja suorita seuraavat PS cmdlet-komennot:

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

Näyttöön tulee kehote, joka pyytää vahvistamaan poistamisen, jos et ole käyttänyt *-voimaan* vaihtoehto. Vahvistuksen RG ja kaikki resurssit, se sisältää poistetaan.

### <a name="delete-a-resource-group-in-the-azure-portal"></a>Poista resurssiryhmä Azure-portaalissa  

1. Kirjaudu sisään [Azure portal](https://portal.azure.com).
2. Siirry palvelun kangasta klusterin haluat poistaa.
3. Napsauta klusterin essentials-sivulla resurssiryhmä-nimeä.
4. Tämä näyttää **Resurssin ryhmän Essentials** -sivu.
5. Valitse **Poista**.
6. Viimeistele poistaminen resurssiryhmän sivun ohjeiden mukaisesti.

![Resurssiryhmä poistaminen][ResourceGroupDelete]


## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a>Klusteriresurssi- ja se käyttää resursseja, mutta ei muita resursseja resurssiryhmän poistaminen

Jos resurssiryhmä on vain resurssit, jotka liittyvät palvelun kangasta klusterin haluat poistaa, on helpompaa poistaa koko resurssiryhmä. Jos haluat poistaa valikoivasti resurssiryhmän resursseista yksi kerrallaan, noudata seuraavia ohjeita.

Jos olet asentanut yhteyttä klusterin portaalissa tai käytä jotakin seuraavista palvelun kangasta Resurssienhallinta-mallien mallivalikoimasta, kaikki resurssit, joiden klusteri käyttää merkityt seuraavat kaksi tunnisteet kanssa. Niiden avulla voit päättää, mitä haluat poistaa resursseja.

***Merkitseminen #1:*** Avain = clusterName, arvo = "klusterin nimi"

***Merkitseminen #2:*** Avain = resourceName, arvo = ServiceFabric

### <a name="delete-specific-resources-in-the-azure-portal"></a>Poista tietyt resurssit Azure-portaalissa

1. Kirjaudu sisään [Azure portal](https://portal.azure.com).
2. Siirry palvelun kangasta klusterin haluat poistaa.
3. Siirry **kaikkia asetuksia** essentials-sivu.
4. Napsauta **tunnisteet** **Resurssienhallinta** -kohdassa asetukset-sivu.
5. Valitse jokin **tunnisteet** tunnisteet-sivu, saat luettelon käytettävissä tunnistetta resurssit.

    ![Resurssin tunnisteet][ResourceTags]

6. Kun merkittynä resurssien luetteloa, valitse kunkin resurssit ja Poista sivut.

    ![Merkitty resurssit][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a>Poista resursseja Azure PowerShellin avulla

Voit poistaa resurssit yksi kerrallaan suorittamalla seuraavat Azure PowerShell cmdlet-komennot. Varmista, että Azure PowerShell 1.0 tai uudempi on asennettu tietokoneeseen. Jos et ole tehnyt tämän ennen, noudata artikkelin [asentaminen ja määrittäminen PowerShellin Azure.](../powershell-install-configure.md)

Avaa PowerShell-ikkuna ja suorita seuraavat PS cmdlet-komennot:

```powershell
Login-AzureRmAccount
```
Kunkin resurssit haluat poistaa, suorita seuraava:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

Voit poistaa klusteriresurssin suorittamalla seuraavasti:

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a>Seuraavat vaiheet
Lue tietoja myös klusterin päivittäminen ja jakaminen services seuraavasti:

- [Lisätietoja klusterin päivitykset](service-fabric-cluster-upgrade.md)
- [Lisätietoja jakaminen tilallisten palvelut suurin asteikko](service-fabric-concepts-partitioning.md)


<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
