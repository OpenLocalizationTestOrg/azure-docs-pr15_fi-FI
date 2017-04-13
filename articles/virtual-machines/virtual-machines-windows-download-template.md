<properties
    pageTitle="Luoda AM kuva Azure-AM | Microsoft Azure"
    description="Lue, miten voit luoda generalized AM kuva aiemmin Azure-AM luotu resurssien hallinnan käyttöönottomalli"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Lataa malli AM

Kun luot Azure kautta tai PowerShellin avulla AM, Resurssienhallinta mallin luodaan automaattisesti puolestasi. Tämän mallin avulla voit nopeasti kopioida käyttöönoton. Malli sisältää tietoja kaikista resursseja resurssiryhmän. Virtual-tietokoneelle siis mallin säilöjen kaikki tiedot, jotka on luotu, jotka tukevat resurssin ryhmän verkko resursseista AM.

## <a name="download-the-template-using-the-portal"></a>Lataa malli-portaalissa

1. Kirjaudu sisään [Azure portal](https://portal.azure.com/).
2. Jokin toiminto-valikosta Valitse **näennäiskoneiden**.
3. Valitse virtuaalikoneen luettelosta.
5. Valitse **Automaattiset komentosarja**.
6. Valitse **Lataa** ja Tallenna .zip-tiedosto paikalliseen tietokoneeseen.
7. Avaa .zip-tiedosto ja Pura tiedostot-kansioon. .Zip-tiedosto sisältää:
    
    - Deploy.ps1
    - Deploy.SH 
    - deployer.RB
    - DeploymentHelper.cs
    - parameters.JSON
    - Template.JSON

.Json-tiedosto on malli.
    
## <a name="download-the-template-using-powershell"></a>Lataa malli PowerShellin avulla

Voit ladata .json mallitiedoston [Vie AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) cmdlet-komennolla. Voit käyttää `-path` parametrin antamaan .json tiedoston polku ja tiedostonimi. Tässä esimerkissä näytetään nimeltä **myResourceGroup** **C:\users\public\downloads** kansioon paikallisessa tietokoneessa resurssiryhmä mallin lataamisesta.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja käyttöönotto resurssien mallien avulla, katso [Resurssienhallinta mallin ongelmatilanteita](../resource-manager-template-walkthrough.md).