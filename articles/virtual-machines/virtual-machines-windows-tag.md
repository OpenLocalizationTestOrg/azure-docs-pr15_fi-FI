<properties
   pageTitle="Miten tunniste AM | Microsoft Azure"
   description="Lisätietoja tunnisteita Windows virtual machine-luotu Azure käyttämällä resurssien hallinnan käyttöönottomalli"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Miten tunnisteen virtual kone Windows Azure-tietokannassa


Tässä artikkelissa kuvataan tapoja tunnistetta virtual kone Windows Azure kautta resurssien hallinnan käyttöönottomalli. Tunnisteet ovat käyttäjän määrittämä avain/arvo paria, joka voidaan sijoittaa mihin suoraan resurssi tai resurssiryhmä. Azure tukee tällä hetkellä enintään 15 tunnistetta resurssi ja resurssiryhmä kohden. Tunnisteiden saattaa sijoitettu resurssin luomisen yhteydessä tai lisätä aiemmin luotu resurssi. Huomaa, että tunnisteet tuetaan vain Resurssienhallinta-käyttöönoton mallin avulla luodut resurssit. Voit halutessasi tunnisteen Linux virtual koneen Katso, [miten voit merkitä Linux virtual kone Azure-tietokannassa](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Tunnisteita PowerShellin avulla

Jos haluat luoda, lisääminen ja poistaminen tunnisteet powershellissä voit ensimmäisen tarvitse [ympäristön PowerShellin Azure resurssien hallinnan][]määrittäminen. Kun asennus on valmis, voit sijoittaa tunnisteet luomista tai kun resurssi on luotu PowerShellin kautta suorittaminen, verkon ja tallennustilaa resursseja. Tässä artikkelissa keskitytään näennäiskoneiden sijoitettu tarkastella ja muokata tunnisteita.

Siirry ensin Virtual Machine, kautta `Get-AzureRmVM` cmdlet-komento.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Jos virtuaalikoneen on jo tunnisteita, tulee Valitse resurssi näkyviin kaikki tunnisteet:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Jos haluat lisätä tunnisteita PowerShellin kautta, voit käyttää `Set-AzureRmResource` komento. Huomautus päivitettäessä tunnisteet PowerShellin kautta, tunnisteet päivitetään kokonaisuudessaan. Joten jos olet lisäämässä resurssia, joka on jo tunnisteet yhden tunnisteen, sinun on koskemaan kaikkia tunnisteita, johon haluat sijoittaa resurssin. Alla on esimerkki siitä, miten voit lisätä muita tunnisteita resurssin kautta PowerShellin cmdlet-komennot.

Ensimmäinen cmdlet asettaa sijoitettu *MyTestVM* *$tags* muuttujaan, tunnisteiden avulla `Get-AzureRmResource` ja `Tags` ominaisuus.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Toista-komento näyttää tietyn muuttujan tunnisteet.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Kolmas-komento lisää muita tunnisteen *$tags* muuttuja. Huomaa, **+=** liittäminen uusi avain/arvo-pari *$tags* luetteloon.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Neljäs-komento määrittää kaikki tietyn resurssin *$tags* muuttujan määritelty tunnisteet. Tässä tapauksessa on MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Viidennen komento näyttää kaikki tunnisteet resurssin. Kuten näet, *sijainti* on nyt määritetty tunnisteen kanssa *MyLocation* arvona.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Lisätietoja PowerShellin kautta tunnisteita, tutustu [Azure resurssin cmdlet-komennot][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja Azure resurssien merkitsemisestä on artikkelissa [Azure Resurssienhallinta yleiskatsaus][] ja [Käyttämällä tunnisteen, kun haluat järjestää Azure-resurssit][].
* Katso, miten tunnisteiden avulla voit hallita Azure resurssien käyttöä-kohdassa [Azure laskun tietoja][] ja [käyttää tietoja tuominen Microsoft Azure-resurssin kulutus][].

[PowerShell-ympäristön Azure resurssien hallinta]: ../powershell-azure-resource-manager.md
[Azure resurssin cmdlet-komennot]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure resurssin hallinnassa: yleiskatsaus]: ../azure-resource-manager/resource-group-overview.md
[Tunnisteiden avulla voit järjestää Azure-resurssit]: ../resource-group-using-tags.md
[Azure laskun ymmärtäminen]: ../billing/billing-understand-your-bill.md
[Hanki tietoja tuominen Microsoft Azure-resurssin kulutus]: ../billing-usage-rate-card-overview.md
