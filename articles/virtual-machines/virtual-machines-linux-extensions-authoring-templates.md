<properties
   pageTitle="Authoring malleja on Linux AM | Microsoft Azure"
   description="Lisätietoja authoring Azure Resurssienhallinta malleja on Linux VMs varten"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Azure Resurssienhallinta malleja Linux AM tunniste on yhtä aikaa

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Suorita seuraavat yhdistämiskomentoon Azure CLI:

      Azure VM extension list

Tämä komento palauttaa julkaisijan nimi, tunnisteen nimi ja versio seuraavalla tavalla:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Nämä kolme ominaisuudet Yhdistä "publisher", "tyyppi" ja "typeHandlerVersion" yllä mallia koodikatkelman vuosina.

>[AZURE.NOTE]On aina suositeltavaa viimeksi päivitetty toimintojen uusimman tunniste avulla.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Rakenteen tunniste määritysten parametrien tunnistaminen

Seuraava vaihe kanssa yhtä aikaa tunniste-malli on tunnistaa tarjoamiseksi määrityksiä. Kunkin laajennuksen tukee omassa joukko parametreja.

Katsomalla otoksen määritykset Linux tunnisteet valitsemalla Katso [Linux eExtensions näytteiden](virtual-machines-linux-extensions-configuration-samples.md)ohjeissa.

Tutustu Hae täysin valmis malli on AM seuraavasti.

[Mukautettu komentosarja tunniste Linux AM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Jälkeen authoring mallia, voit ottaa sen Azure-CLI.
