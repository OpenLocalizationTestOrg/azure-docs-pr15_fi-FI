<properties
   pageTitle="Luo cloud palvelusäilön PowerShellin | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit luoda cloud palvelusäilön PowerShellin avulla. Säilön isännöi Internetin kautta tai työntekijä roolit."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>PowerShellin Azure-komennon avulla voit luoda tyhjän cloud-palvelusäilö
Tässä artikkelissa kerrotaan, miten voit luoda nopeasti käyttämällä Azure PowerShellin cmdlet-komennot pilvipalveluihin säilön. Noudata seuraavia ohjeita:

1. Asenna Microsoft Azure PowerShell-cmdlet [PowerShellin Azure](http://aka.ms/webpi-azps) -lataussivulta.
2. Avaa PowerShell-komentokehote.
3. [Lisää AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) avulla voit kirjautua sisään.

    > [AZURE.NOTE] Viittaavat lisäohjeita Azure PowerShell cmdlet-komennon asentaminen ja yhteyden Azure-tilaukseen, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

4. **Uusi AzureService** cmdlet-komennon avulla voit luoda tyhjän Azure cloud-palvelusäilö.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Noudata tässä esimerkissä käynnistää-cmdlet-komennolla:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Lisätietoja Azure pilvipalvelussa suorittaa:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Seuraavat vaiheet

 * Hallittavan cloud palvelun käyttöönoton viitata [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Poista AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)ja [Määritä AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) -komennot. Voivat myös katsoa [määrittäminen pilvipalveluihin](cloud-services-how-to-configure.md) lisätietoja.

 * Cloud palvelun projektin julkaiseminen Azure-viitata [Jatkuva toimitukseen pilvipalvelussa Azure-tietokannassa](cloud-services-dotnet-continuous-delivery.md) **PublishCloudService.ps1** koodi-malli.
