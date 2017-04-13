<properties
    pageTitle="Eri tapoja luoda Windows AM | Microsoft Azure"
    description="Eri tapoja luoda Windows-virtual machine resurssien hallinta."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="different-ways-to-create-a-windows-virtual-machine-with-resource-manager"></a>Eri tapoja luoda Windows-virtual machine resurssien hallinta

Azure on erilaisia tapoja luoda virtual machine, koska näennäiskoneiden on parhaiten eri käyttäjille ja varten. Tämä tarkoittaa, että haluat tehdä tehdäänkö virtuaalikoneen ja miten voit luoda sen. Tässä artikkelissa tutustutaan näitä vaihtoehtoja ja linkit ohjeisiin.

## <a name="azure-portal"></a>Azure portal

Azure-portaalissa on helppo tapa kokeilla virtual-koneen, etenkin silloin, kun olet olet vasta alkuvaiheessa ja Azure. 

[Luo virtual koneeseen, jossa käytetään Windows-portaalissa](virtual-machines-windows-hero-tutorial.md)

## <a name="template"></a>Malli

Näennäiskoneiden edellyttävät yhdistelmän resurssit (esimerkiksi käytettävyys joukot ja tallennustilaa tilit). Sen sijaan, että käyttöönotto ja hallinta kullekin resurssille erikseen, voit luoda Azure Resurssienhallinta-malli, joka ottaa käyttöön ja valmistelee kaikkien resurssien yhteen, koordinoidun toiminnossa.

- [Voit luoda Windows-virtual machine Resurssienhallinta-mallin avulla](virtual-machines-windows-ps-template.md)


## <a name="azure-powershell"></a>Azure PowerShell

Jos käytät mieluummin-komento-käyttöliittymän toimi, voit käyttää PowerShellin Azure.

- [Luo Windows-AM PowerShellin avulla](virtual-machines-windows-ps-create.md)


## <a name="visual-studio"></a>Visual Studio

Visual Studio avulla voit luoda ja hallita käyttöönotto Visual Studio ja Azure SDK VMs Azure-työkaluja.

[Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs)

