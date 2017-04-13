<properties
    pageTitle="Yhteisön Työkalut Azure hallinnan Azure Resurssienhallinta-siirto"
    description="Tässä artikkelissa Luetteloi työkaluja, joilla saanut yhteisö auttaminen siirtoon IaaS resurssien Azure palvelun hallinnasta Azure Resurssienhallinta-kenttään."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Yhteisön Työkalut Azure hallinnan Azure Resurssienhallinta-siirto

Tässä artikkelissa Luetteloi työkaluja, joilla saanut yhteisö auttaminen siirtoon IaaS resurssien Azure palvelun hallinnasta Azure Resurssienhallinta-kenttään.

>[AZURE.NOTE]Työkaluja ei virallisesti tue Microsoft Support. Tämän vuoksi ne ovat avoinna, valitse Github Orpotermit ja olemme on valmis hyväksymään laatuviiniä korjaukset tai Lisää skenaarioita. Voit raportoida ongelman-toiminnolla Github ongelmat.
>
> Siirtyminen näillä työkaluilla aiheuttaa käyttökatkot perinteinen virtuaalikoneen varten. Jos etsit tuetut käyttöympäristö siirron, käy 
>
>- [Tuetut käyttöympäristö siirto Azure Resurssienhallinta pinon perinteinen IaaS resurssit](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Tekniset perinpohjaisesti käsittelevään artikkeliin ympäristössä tuettu siirtymisen perinteinen Azure resurssien hallinta](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Siirtää IaaS resurssien perinteinen, Azure Resurssienhallinta Azure PowerShellin avulla](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Tämä on PowerShellin komentosarja-moduulin siirtämistä varten oman **yksittäisen** virtuaalikoneen (AM) from Azure hallinnan pinon Azure Resurssienhallinta pino. 

[Työkalun ohjeista linkittäminen](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz on lisäasetuksia siirtämään joukon Azure Service Management IaaS resurssien Azure Resurssienhallinta IaaS resursseja. Siirron voi ilmetä saman tilauksen piiriin kuuluvien tai eri tilaukset ja tilaustyypit välillä (ex: CSP tilaukset).

[Työkalun ohjeista linkittäminen](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)