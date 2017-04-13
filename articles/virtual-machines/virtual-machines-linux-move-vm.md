<properties
    pageTitle="Siirrä Linux AM | Microsoft Azure"
    description="Siirtää Linux AM toiseen Azure-tilauksen tai resurssin ryhmään Resurssienhallinta käyttöönoton mallin."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Linux AM siirtäminen toiseen tilaukseen tai resurssin ryhmään

Tässä artikkelissa käydään läpi siirtäminen Linux AM resurssiryhmät tai tilausten välillä. Siirtyminen AM välillä tilaukset voidaan kätevä, jos olet luonut AM Omat tilauksen ja haluat nyt siirtyä yrityksen tilaus.

> [AZURE.NOTE] Siirrä osana luodaan uusi resurssitunnukset. AM on siirretty, kun haluat päivittää työkalut ja komentosarjojen käyttää uusi resurssi tunnukset. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>AM Siirry Azure-CLI käyttämällä 

Jos haluat siirtää onnistuneesti AM, siirtää AM ja kaikki sen tukitiedostot resurssit. Luettele kaikki resursseja resurssiryhmän ja niiden tunnukset **azure ryhmän Näytä** -komennon avulla. Sen avulla voit tulostaa tämän komennon tulosteen tiedostoon, jotta voit kopioida ja liittää tunnukset myöhemmin komennot.

    azure group show <resourceGroupName>

Voit siirtyä toiseen resurssiryhmä AM ja resursseja, käyttämällä **Siirry azure resurssin** CLI-komentoa. Seuraavassa esimerkissä AM ja se vaatii yleisimmät resurssit. Emme Käytä **-i** -parametrin ja välittää CSV-luettelon (ilman välilyöntejä) tunnuksista resurssien Siirrä.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Jos haluat siirtää toiseen tilaukseen AM ja resursseja, lisätä **--kohde subscriptionId & #60; destinationSubscriptionID & #62;** Määritä kohdetilauksen parametri.

Jos käsittelet komentokehotteen Windows-tietokoneessa, sinun on lisättävä **$** muuttujien nimissä, kun ne määritellä eteen. Tämä ei ole tarpeen Linux.

Sinua pyydetään vahvistamaan, että haluat siirtää määritettyä resurssia. Kirjoita **Y** vahvistamaan, että haluat siirtää resurssit.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Voit siirtää useita erityyppisiä resursseja resurssiryhmät ja tilaukset. Lisätietoja on artikkelissa [resurssien uusi resurssiryhmä tai-Tilaustani siirtäminen](../resource-group-move-resources.md).    