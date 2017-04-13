<properties
   pageTitle="Mallien luominen Windowsin AM tunniste on | Microsoft Azure"
   description="Lisätietoja authoring Azure Resurssienhallinta malleja on Windowsin VMs varten"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Authoring on AM Windows Azure Resurssienhallinta mallit

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Suorita PowerShellin Azure seuraavan Azure PowerShell cmdlet-komennon:

      Get-AzureVMAvailableExtension


Tämä cmdlet-komento palauttaa julkaisijan nimi, tunnisteen nimi ja versio seuraavasti:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Nämä kolme ominaisuudet Yhdistä "publisher", "tyyppi" ja "typeHandlerVersion" yllä mallia koodikatkelman vuosina.

>[AZURE.NOTE]On aina suositeltavaa viimeksi päivitetty toimintojen uusimman tunniste avulla.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Rakenteen tunniste määritysten parametrien tunnistaminen

Seuraava vaihe kanssa yhtä aikaa tunniste-malli on tunnistaa tarjoamiseksi määrityksiä. Kunkin laajennuksen tukee omassa joukko parametreja.

Tarkista otoksen määritykset Windowsin osassa, on artikkelissa [Windows tunnisteet näytteiden](virtual-machines-windows-extensions-configuration-samples.md).


Tutustu Hae täysin valmis malli on AM seuraavasti.

[Mukautettu komentosarja-tunniste Windows AM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Jälkeen authoring mallia, voit ottaa sen PowerShellin Azure.
