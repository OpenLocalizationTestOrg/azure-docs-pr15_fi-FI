<properties
   pageTitle="PowerShellin Azure (perinteinen) palvelujen käänteinen DNS-tietueiden hallinta | Microsoft Azure"
   description="Voit hallita käänteinen DNS-tietueet tai Azure-palveluita käyttämällä PowerShell perinteinen käyttöönoton mallin PTR-tietueet. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Azure PowerShellin Azure palvelujen (perinteinen) käänteinen DNS-tietueiden hallinta

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Käänteinen DNS-tietueiden vahvistaminen
Jotta kolmannen osapuolen ei voi luoda käänteisen DNS-tietueet DNS-toimialueiden yhdistäminen Azure sallii vain luomisen käänteinen DNS-tietue, jossa jompikumpi seuraavista pitää paikkansa:

- Vastaavasti DNS FQDN on nimi, johon se on määritetty pilvipalvelussa tai minkä tahansa pilvipalvelussa nimen saman tilauksen piiriin kuuluvien esimerkiksi, käänteinen DNS-contosoapp1.cloudapp.net.".
- Käänteinen DNS FQDN eteenpäin korjaa nimeä tai pilvipalvelussa, johon se on määritetty tai minkä tahansa pilvipalvelussa nimen IP- tai IP saman tilauksen piiriin kuuluvien esimerkiksi käänteinen DNS on "app1.contoso.com." Mikä on CName-alias contosoapp1.cloudapp.net varten.

Kelpoisuuden tarkistus suoritetaan vain, kun pilvipalveluun käänteinen DNS-ominaisuuden määrittäminen tai muokata. Säännöllisiä uudelleen vahvistuksen ei suoriteta.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Käänteinen DNS lisääminen aiemmin pilvipalveluihin
Voit lisätä aiemmin pilvipalvelussa, "AzureService määrittäminen"-cmdlet-komennolla käänteinen DNS-tietue:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Luo pilvipalveluun käänteinen DNS
Voit lisätä uuden pilvipalvelussa käänteinen DNS-ominaisuutta, määritetty "AzureService määrittäminen"-cmdlet-komennolla:

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Näytä käänteinen DNS: ää aiemmin pilvipalveluihin
Voit tarkastella määritetty arvo aiemmin pilvipalvelussa, "Get-AzureService"-cmdlet-komennolla:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Käänteinen DNS poistaminen aiemmin pilvipalveluihin
Voit poistaa aiemmin pilvipalvelussa, "AzureService määrittäminen"-cmdlet-komennolla käänteinen DNS-ominaisuutta. Tämä tehdään määrittämällä käänteinen DNS-ominaisuuden arvon tyhjä:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
