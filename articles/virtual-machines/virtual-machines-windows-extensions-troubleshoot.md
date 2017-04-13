<properties
   pageTitle="Windowsin AM tunniste virheiden vianmääritys | Microsoft Azure"
   description="Lisätietoja Azure Windows AM tunniste virheiden vianmääritys"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Windows Azure-AM tunniste virheiden vianmääritys

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Tunniste-tilan tarkasteleminen
Azure Resurssienhallinta malleja voidaan suorittaa PowerShellin Azure. Mallin suoritetaan, kun laajennuksen tila voi tarkastella Azure resurssin Resurssienhallinnasta tai komentorivi-työkalut.

Tässä on esimerkki:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Tässä on esimerkki tulos:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Tunniste virheiden vianmääritys

### <a name="re-running-the-extension-on-the-vm"></a>Tunniste on käytössä AM uudelleen

Jos käytössäsi on komentosarjat AM käyttämällä mukautettu komentosarja-tunniste, joita saattaa tulla joskus virhesanoman, jossa AM luonti onnistui, mutta komentosarja on epäonnistunut. Valitse näiden yksityiskohtaisten sääntöjen suositeltu tapaa palauttaminen tämä virhe on Suorita malli uudelleen ja Poista tunniste.
Huomautus: tulevaisuudessa toiminto voidaan parantaa Poista laajennuksen asennuksen poistaminen edellyttää.


#### <a name="remove-the-extension-from-azure-powershell"></a>Tunnisteen poistaminen PowerShellin Azure

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Kun tiedostotunniste on poistettu, malli voidaan suorittaa uudelleen voidaan suorittaa komentosarjat AM.
