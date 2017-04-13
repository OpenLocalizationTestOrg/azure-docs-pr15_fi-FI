<properties
   pageTitle="Etsi ja valitse Windows AM kuvat | Microsoft Azure"
   description="Lue tietoja sen selvittämisestä publisher, tarjous ja SKU kuvien luotaessa Windows-virtual machine resurssien hallinnan käyttöönottomalli."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Siirtyminen ja valitse PowerShell tai CLI Azure Windows virtuaalikoneen kuvat

Tässä ohjeaiheessa kerrotaan, miten löydät AM kuva julkaisijat, tarjoukset, tuotteissa ja versiot kunkin sijainti, johon voi ottaa käyttöön. Jos haluat antaa esimerkki, jotkin tavallisimmat Windows AM kuvat ovat:

## <a name="table-of-commonly-used-windows-images"></a>Windowsin usein käytettyjä kuvia sisältävä taulukko


| PublisherName                        | Tarjouksen                                 | Tuote                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise-optimoitu-varten-DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise-optimoitu-varten-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 R2-Palvelinkeskukseen                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 Palvelinkeskukseen               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2-SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Windows-palvelin-Technical-esikatselu |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
