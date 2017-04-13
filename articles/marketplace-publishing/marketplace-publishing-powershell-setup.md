<properties
   pageTitle="Määritä PowerShell luomiseen AM Marketplacen | Microsoft Azure"
   description="Ohjeet PowerShellin Azure määrittäminen ja käyttäminen valinnainen prosessin flow AM kuvien käyttöön ja myydä Azure Marketplace-"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Voit luoda tarjouksen Azure Marketplacen PowerShellin Azure määrittäminen
Lisätietoja PowerShellin Azure määrittämisestä on artikkelissa [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). Yksinkertainen vaihtoehto on käytettävä varmenne-menetelmä, jota Lataa ja tuo todennuksessa varmenne. Tarvittavat sertifikaatin hankkiminen **Get-AzurePublishSettingsFile** cmdlet-komennon avulla Tallenna tiedosto, kun järjestelmä pyytää sinua kirjoittamaan. Varmenteen tuominen PowerShell-istunnon, käytä **Tuo AzurePublishSettingsFile** cmdlet-komento.

Määrittäminen ja yleisiä Microsoft Azure-tilausasetusten tallentamiseen PowerShell-istunnon, käytä **Määrittäminen AzureSubscription** ja **Valitse AzureSubscription** cmdlet-komennot:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Ensimmäisen komennon liittää tallennustilan oletustilin tilaus (tarvitaan valmistelu AM joitakin toimintoja varten).  Toinen tekee tilauksen nykyisen rivin (tunnista muiden cmdlet-komennot).

## <a name="see-also"></a>Katso myös
- [Aloittaminen: julkaiseminen tarjouksen Azure Marketplacesta](marketplace-publishing-getting-started.md)
- [Marketplacen virtuaalikoneen näköistiedoston luominen](marketplace-publishing-vm-image-creation.md)
