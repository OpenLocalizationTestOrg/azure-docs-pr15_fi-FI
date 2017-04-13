<properties 
    pageTitle="Ota Linux näennäiskoneiden uudelleen | Microsoft Azure" 
    description="Tässä artikkelissa käsitellään Ota Linux näennäiskoneiden pienentämään SSH yhteysongelmien uudelleen." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Ota uusi Azure solmu virtual machine uudelleen

Jos sinulla on vastakkaisten vaikeuksia vianmäärityksen SSH tai sovelluksen pääsyn Azure virtuaalikoneen (AM), mallirakenteeseen AM ohjeita. AM Ota uudelleen, kun se siirtää AM uusi solmu Azure infrastruktuurin ja tehostaa sen uudelleen käyttöön, säilyttämiseen asetukset ja niihin liittyvät resurssit. Tässä artikkelissa kerrotaan ottamisesta käyttöön AM, Azure CLI tai Azure portaalin.

> [AZURE.NOTE] Kun AM Ota uudelleen, tilapäinen levyn menetetään ja dynaamisia IP-osoitteita, jotka liittyvät VPN-liittymän päivitetään. 


## <a name="using-azure-cli"></a>Azure CLI käyttäminen

Varmista, että tietokoneeseen on [Asennettu uusimmat Azure-CLI](../xplat-cli-install.md) ja Resurssienhallinta-tilassa (`azure config mode arm`).

Seuraavat Azure CLI-komennon avulla voit ottaa virtuaalikoneen uudelleen:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Näet tilan AM muutos, kun se kulkee laukaisun vaiheet. `PowerState` AM, siirtyy 'Suoritus' 'Päivittäminen', 'Käynnistetään' ja lopuksi 'on', kun se käy läpi prosessi, jossa uusi isäntä mallirakenteeseen. Tarkista tila VMs resurssin ryhmän kanssa:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Seuraavat vaiheet
Jos yhteyden muodostaminen oman AM on vaikeuksia, löydät [vianmääritys SSH yhteydet](virtual-machines-linux-troubleshoot-ssh-connection.md) tai [yksityiskohtaiset vianmääritysohjeet SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md)ohjeita. Jos et voi käyttää sovelluksen oman AM käytössä, voit myös lukea [sovelluksen vianmääritystä](virtual-machines-linux-troubleshoot-app-connection.md).