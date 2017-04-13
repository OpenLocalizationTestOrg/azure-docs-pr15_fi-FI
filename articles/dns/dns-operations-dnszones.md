<properties
   pageTitle="Hallitse DNS-vyöhykkeet PowerShellin avulla | Microsoft Azure"
   description="Voit hallita DNS-vyöhykkeet Azure PowerShellin avulla. Voit päivittää, poistaa ja luo DNS-vyöhykkeet Azure DNS"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>Voit hallita DNS-vyöhykkeet PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShellin](dns-operations-dnszones.md)



Tässä artikkelissa kerrotaan, miten voit hallita DNS-vyöhyke PowerShell-toiminnolla. Tarvitset näiden ohjeiden avulla, jotta voit asentaa uusimman version Azure Resurssienhallinta PowerShellin cmdlet-komennot (1.0 tai uudempi). Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.


## <a name="create-a-new-dns-zone"></a>Luo uusi DNS-vyöhyke

DNS-vyöhyke-kohdassa [luominen DNS-vyöhyke PowerShell-toiminnolla](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Hae DNS-vyöhyke

Voit hakea DNS-vyöhyke `Get-AzureRmDnsZone` cmdlet-komento. Tämä toiminto palauttaa vastaavan aiemmin Azure DNS-vyöhykkeelle DNS zone objektin. Objekti sisältää tietoja (kuten tietueen joukot määrä) vyöhykkeen, mutta se ei sisällä tietueen joukot itsestään.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Luettelon DNS-vyöhykkeet

Alueen nimen poistamalla `Get-AzureRmDnsZone`, voit Luetteloi kaikki vyöhykkeet resurssi-ryhmässä. Tämä toiminto palauttaa alueen objekteja.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Päivitä DNS-vyöhyke

Voit tehdä muutoksia DNS zone resurssille käyttämällä `Set-AzureRmDnsZone`. Tämä ei päivitä mitään DNS-tietueen määrittää vyöhykkeen sisällä (Katso, [miten voit hallita DNS-tietueita](dns-operations-recordsets.md)). Sitä käytetään vain itse vyöhykkeen resurssin ominaisuuksien päivittäminen. Tämä on tällä hetkellä rajoitettu Azure resurssien hallinta tunnisteita, jos zone resurssin avulla. Lisätietoja on kohdassa [Etags ja tunnisteet](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Käytä jompikumpi seuraavista update DNS-vyöhyke kahdella tavalla:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Määrittää vyöhykkeen käyttämällä alueen nimi ja resurssi-ryhmä

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Määrittää vyöhykkeen $zone objektin käyttäminen

Määrittää vyöhykkeen käyttämällä $zone objektin `Get-AzureRmDnsZone`. Kun käytät `Set-AzureRmDnsZone` $zone objektissa Etag tarkistukset käytetään varmistamiseksi samanaikaiset muutokset ei korvata. Voit käyttää valinnaiset *-Korvaa* valitsin estää tarkistukset. Lisätietoja on kohdassa [Etags ja tunnisteet](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Poista DNS-vyöhyke

DNS-vyöhykkeet voi poistaa Poista AzureRmDnsZone-cmdlet-komennolla.

Ennen kuin poistat DNS-vyöhyke Azure DNS-palvelimeen, sinun on poistettava kaikki tietueet joukot lukuun ottamatta NS ja SOA tietueet vyöhykkeen ylimmällä tasolla, jotka on luotu automaattisesti, kun vyöhyke on luotu.

Käytä jotakin seuraavista tavoista, voit poistaa DNS-vyöhyke:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Määrittää vyöhykkeen alueen nimi ja resurssiryhmän nimi

Tämä toiminto on valinnainen *-voimassa* valitsin, joka estää ohjelma pyytää vahvistamaan, että haluat poistaa DNS-vyöhyke.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Määrittää vyöhykkeen $zone objektin käyttäminen

Määrittää vyöhykkeen käyttämällä $zone objektin `Get-AzureRmDnsZone`. Tämä toiminto on valinnainen *-voimassa* valitsin, joka estää ohjelma pyytää vahvistamaan, että haluat poistaa DNS-vyöhyke. Kuten `Set-AzureRmDnsZone`-vyöhykkeen $zone-objektin määrittäminen mahdollistaa Etag tarkistukset, jotta samanaikaiset muutokset ei poisteta. <BR>
Valinnainen *-Korvaa* merkinnän vaimentaa tarkistukset. Lisätietoja on kohdassa [Etags ja tunnisteet](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Vyöhykkeen objektin voi myös piped sijaan Parametrina välitetty:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut DNS-vyöhyke, luo [tietue joukot ja tietueiden](dns-getstarted-create-recordset.md) voit käynnistää Internet-toimialueen nimet selvitetään.