<properties 
    pageTitle="Ota Windowsin näennäiskoneiden uudelleen | Microsoft Azure" 
    description="Tässä artikkelissa käsitellään Ota Windowsin näennäiskoneiden pienentämään RDP yhteysongelmien uudelleen." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ota uusi Azure solmu virtual machine uudelleen

Jos sinulla on jo vastakkaisten vaikeuksia vianmääritys Remote Desktop (RDP) auttavat yhteyden tai sovelluksen käyttämään Windows-pohjaisesta Azure virtuaalikoneen (AM) mallirakenteeseen AM. AM Ota uudelleen, kun se siirtää AM uusi solmu Azure infrastruktuurin ja tehostaa sen uudelleen käyttöön, säilyttämiseen asetukset ja niihin liittyvät resurssit. Tässä artikkelissa kerrotaan ottamisesta käyttöön AM, PowerShellin Azure tai Azure portaalin.

> [AZURE.NOTE] Kun AM Ota uudelleen, tilapäinen levyn menetetään ja dynaamisia IP-osoitteita, jotka liittyvät VPN-liittymän päivitetään. 

## <a name="using-azure-powershell"></a>Azure PowerShellin avulla

Varmista, että sinulla on uusin PowerShellin Azure 1.x asennettu tietokoneeseesi. Katso lisätietoja, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

Ota uudelleen virtuaalikoneen PowerShellin Azure tällä komennolla:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Seuraavat vaiheet
Jos yhteyden muodostaminen oman AM on vaikeuksia, löydät [vianmääritys RDP yhteydet](virtual-machines-windows-troubleshoot-rdp-connection.md) tai [yksityiskohtaiset vianmääritysohjeet RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md)ohjeita. Jos et voi käyttää sovelluksen oman AM käytössä, voit myös lukea [sovelluksen vianmääritystä](virtual-machines-windows-troubleshoot-app-connection.md).