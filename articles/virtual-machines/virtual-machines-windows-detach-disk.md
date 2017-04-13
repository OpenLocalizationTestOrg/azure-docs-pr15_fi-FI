<properties
    pageTitle="Irrota Windows AM tietojen levyltä | Microsoft Azure"
    description="Lue irrottaa tietojen DVD-levyllä virtual tietokoneesta, Resurssienhallinta käyttöönoton mallin Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Miten irrottaa tietokoneesta Windowsin virtual tietojen DVD-levyllä


Kun et enää tarvitse tietoja DVD-levyllä, joka on liitetty virtual tietokoneeseen, voit irrottaa sen helposti. Tämä poistaa levyn virtuaalikoneen, mutta ei poista sitä tallennustilan. 

> [AZURE.WARNING] Jos DVD-levyllä irrota sitä ei poisteta automaattisesti. Jos olet tilannut Premium tallennustilan säilyvät maksamaan levyn tallennustilan kulut. Lisätietoja on [hinnoittelu](../storage/storage-premium-storage.md#pricing-and-billing)ja laskutukseen Premium tallennustilan käytettäessä. 

Jos haluat käyttää olemassa olevia tietoja levyn uudelleen, voit voit muodostaa yhteyden uudelleen samaan virtuaalikoneen tai toiseen.  


## <a name="detach-a-data-disk-using-the-portal"></a>Irrota portaalissa tietojen DVD-levyllä

1. Valitse **näennäiskoneiden**portal-toiminnossa.

2. Valitse virtuaalikoneen, jossa on tietoja levyn haluat irrottaa ja valitse sitten **kaikki asetukset**.

3. Valitse **asetukset** -sivu **levyjä**.

4. Valitse tiedot-levy, jota haluat irrottaa **levyjen** -sivu.

5. Valitse tiedot-levyn sivu **Katkaise yhteys**.


    ![Näyttökuva Katkaise yhteys-painiketta.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Levyn tallennustilan jää, mutta ei enää ole liitettynä virtual koneeseen.


## <a name="detach-a-data-disk-using-powershell"></a>Irrota PowerShellin avulla tietojen DVD-levyllä

Tässä esimerkissä ensimmäisen komennon saa virtuaalikoneen nimeltä **MyVM07** **RG11** resurssiryhmän Get-AzureRmVM cmdlet-komennolla. Komento tallentaa virtuaalikoneen **$VirtualMachine** muuttuja. 

Toinen komento poistaa tietoja levyn nimeltä DataDisk3 virtual tietokoneesta. 

Lopulliseksi-komennolla päivittää virtuaalikoneen Viimeistele prosessi, jossa tiedot-levyn poistaminen tila.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Lisätietoja on artikkelissa [Poista AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat käyttää tietojen levy, voit vain [Liitä se toiseen AM](virtual-machines-windows-attach-disk-portal.md)
