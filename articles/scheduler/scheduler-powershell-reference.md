<properties
 pageTitle="Ajoituksen PowerShell cmdlet-viittaus"
 description="Ajoituksen PowerShell cmdlet-viittaus"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Ajoituksen PowerShell cmdlet-viittaus

Seuraavassa taulukossa kuvataan, sekä linkkejä kunkin pää cmdlet-komennot Azure ajoituksen viittaus-sivulle.

Voit asentaa PowerShellin Azure ja liittää sen Azure-tilaus, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). 

Saat lisätietoja [Azure resurssien hallinnan cmdlet-komennot](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx) [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).

|Cmdlet-komento|Cmdlet-komennon kuvaus|
|---|---|
[Poista käytöstä AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Estää työn sivustokokoelman. 
[Ota käyttöön AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Ottaa käyttöön projektin sivustokokoelman.
[Hae AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Saa ajoituksen työt.
[Hae AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Hakee projektin sivustokokoelmat.
[Hae AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Hakee Työhistoria.
[Uusi AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Luo HTTP-työ.
[Uusi AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Luo projektin sivustokokoelman.
[Uusi AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Luo palvelun bus jonon työn.
[Uusi AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Luo palvelun bus aiheen työn.
[Uusi AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Luo tallennustilan jonon työn. 
[Poista AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Poistaa ajoituksen työn.  
[Poista AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Poistaa työ-sivustokokoelman. 
[Määritä AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Muokkaa ajoituksen HTTP-työ.
[Määritä AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Muokkaa työn sivustokokoelman. 
[Määritä AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Muokkaa palvelun bus jonon työn.  
[Määritä AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Muokkaa palvelun bus aiheen työn. 
[Määritä AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Muokkaa tallennustilan jonon työn.   

Lisätietoja voit avata jonkin seuraavat cmdlet-komennot: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Katso myös


 [Ajoituksen ominaisuudet](scheduler-intro.md)

 [Azure ajoitus-käsitteistä, terminologiaan ja kohteen hierarkia](scheduler-concepts-terms.md)

 [Aloita käyttö ajoituksen Azure-portaalissa](scheduler-get-started-portal.md)

 [Suunnitelmien ja Azure ajoituksen Laskutus](scheduler-plans-billing.md)

 [Azure ajoituksen REST API-viittaus](https://msdn.microsoft.com/library/mt629143)

 [Azure ajoituksen suuren käytettävyyden ja luotettavuutta](scheduler-high-availability-reliability.md)

 [Azure ajoituksen rajoitukset, oletusarvot ja virhekoodit](scheduler-limits-defaults-errors.md)

 [Azure ajoituksen lähtevien yhteyksien todennusta](scheduler-outbound-authentication.md)
