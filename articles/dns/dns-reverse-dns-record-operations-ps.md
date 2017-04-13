<properties
   pageTitle="PowerShellin Azure palvelujen käänteinen DNS-tietueiden hallinta | Microsoft Azure"
   description="Voit hallita käänteinen DNS-tietueet tai Azure-palveluita käyttämällä PowerShell-Resurssienhallinta PTR-tietueet"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-azure-powershell"></a>Azure PowerShellin Azure palvelujen käänteinen DNS-tietueiden hallinta

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Käänteinen DNS-tietueiden vahvistaminen
Kolmannen osapuolen ei voi luoda käänteisen DNS-tietueet DNS-toimialueiden yhdistäminen varmistaaksesi Azure sallii vain luomisen käänteinen DNS-tietue, jossa jompikumpi seuraavista pitää paikkansa:

- "ReverseFqdn" on sama kuin julkiseen IP-osoite, johon se on määritetty resurssin "toimialuenimeä" tai "Fqdn" saman tilauksen piiriin kuuluvien minkä tahansa julkiseen IP-osoite esimerkiksi, "ReverseFqdn" on "contosoapp1.northus.cloudapp.azure.com.".

- "ReverseFqdn" eteenpäin korjaa nimeä tai julkiseen IP-osoite, johon se on määritetty tai julkinen IP-osoite "Fqdn-tai IP IP saman tilauksen piiriin kuuluvien esimerkiksi"ReverseFqdn"on"app1.contoso.com." CName-alias "contosoapp1.northus.cloudapp.azure.com.", joka on

Kelpoisuuden tarkistus suoritetaan vain, kun käänteinen DNS-ominaisuuden julkiseen IP-osoite on määritetty tai joita olet muokannut. Säännöllisiä uudelleen vahvistuksen ei suoriteta.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Käänteinen DNS lisääminen olemassa olevan julkisen IP-osoitteet
Voit lisätä käänteinen DNS olemassa olevan julkisen IP-osoite "AzureRmPublicIpAddress määrittäminen"-cmdlet-komennolla:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

Jos haluat lisätä käänteinen DNS olemassa olevan julkisen IP-osoite, joka ei ole vielä DNS-nimen, määritä myös DNS-nimi. Voit lisätä saavuttaa "AzureRmPublicIpAddress määrittäminen"-cmdlet-komennolla:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings = New-Object -TypeName Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings
    PS C:\> $pip.DnsSettings.DomainNameLabel = "contosoapp1"
    PS C:\> $pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Luo julkinen IP-osoite käänteinen DNS
Voit lisätä uuden julkiseen IP-osoitteen käänteinen DNS-ominaisuutta, määritetty "AzureRmPublicIpAddress uusi"-cmdlet-komennolla:

    PS C:\> New-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS -Location WestUS -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Näytä käänteinen DNS olemassa olevan julkisen IP-osoitteet
Voit tarkastella määritetty arvo olemassa olevan julkisen IP-osoitetta "Get-AzureRmPublicIpAddress"-cmdlet-komennolla:

    PS C:\> Get-AzureRmPublicIpAddress -Name PublicIP2 -ResourceGroupName NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Käänteinen DNS poistaminen olemassa olevan julkisen IP-osoitteet
Voit poistaa aiemmin julkiseen IP-osoite "AzureRmPublicIpAddress määrittäminen"-cmdlet-komennolla käänteinen DNS-ominaisuutta. Tämä tapahtuu ReverseFqdn-ominaisuuden arvoksi asetetaan tyhjä:

    PS C:\> $pip = Get-AzureRmPublicIpAddress -Name PublicIP -ResourceGroupName NRP-DemoRG-PS
    PS C:\> $pip.DnsSettings.ReverseFqdn = ""
    PS C:\> Set-AzureRmPublicIpAddress -PublicIpAddress $pip


[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
