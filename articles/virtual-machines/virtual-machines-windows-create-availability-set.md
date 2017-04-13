<properties
    pageTitle="AM-käytettävyys joukon luominen | Microsoft Azure"
    description="Opettele luomaan saatavuus määrittäminen oman näennäiskoneiden Azure kautta tai Resurssienhallinta käyttöönoton mallin PowerShellin avulla."
    keywords="käytettävyys määrittäminen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Käytettävyys-joukon luominen 

Kun käyttämällä portaaliin, jos haluat, että AM saatavuus osaksi määritetty, sinun täytyy luoda käytettävyys, määritä ensin.

Lisätietoja luominen ja käyttäminen käytettävyyden joukot-kohdassa [Manage näennäiskoneiden käytettävyyttä](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Portaalin avulla voit luoda määrittäminen ennen kuin voit luoda oman VMs saatavuus

1. Valitse toiminto-valikossa Valitse **Selaa** ja valitse **käytettävyys-vaihtoehdon avulla**.

2. Valitse **käytettävyys määrittää sivu**valitsemalla **Lisää**.

    ![Näyttökuva, jossa näkyy luotaessa uusia käytettävyys Lisää-painikkeen Määritä.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Täytä tiedot joukon **luominen saatavuus** -sivu.

    ![Näyttökuva, jossa näkyy tietoja sinun on annettava Luo joukko käytettävyys.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Nimi** - nimen on oltava numeroita, kirjaimia, jaksot, alaviivoja ja katkoviivat koostuu 1 80 merkkiä. Ensimmäinen merkki on oltava kirjain tai numero. Viimeisen merkin on oltava kirjain, numero tai alaviivoja.
    - **Vian toimialueet** - vika toimialueet määrittävät, joilla on yhteiset power lähde- ja vaihda näennäiskoneiden ryhmän. Oletusarvon mukaan VMs erotetaan enintään kolme vika toimialueilla ja voidaan muuttaa väliltä 1 – 3.
    - **Päivitä toimialueet** - toimialueita on määritetty oletusarvoinen ja tämä viisi päivitys voidaan määrittää väliltä 1 – 20. Päivitä toimialueen osoittavat näennäiskoneiden ja pohjana fyysinen laite, voit käynnistettävä samanaikaisesti. Esimerkiksi jos olemme Määritä viisi päivittää toimialueet, kun useampaan kuin viiteen näennäiskoneiden on määritetty sisällä yhden käytettävyys määrittäminen, kuudennesta virtuaalikoneen sijoitetaan ensimmäisen virtuaalikoneen saman UD kuin toinen virtuaalikoneen kuudennesta päivityksen toimialueelle kyselyjä ja niin edelleen. Järjestyksen käynnistyy ei ehkä ole peräkkäisiä, mutta vain yksi toimialue käynnistettävä kerrallaan.
    - **Tilauksen** – Jos sinulla on useampi kuin yksi tilaus.
    - **Resurssiryhmä** - Valitse aiemmin luotu resurssiryhmä napsauttamalla nuolta ja valitsemalla avattavasta resurssiryhmä alaspäin. Voit myös luoda uusi resurssiryhmä kirjoittamalla nimen. Nimessä voi olla seuraavia merkkejä: kirjaimia, numeroita, jaksot, katkoviivat, alaviivoja ja avaaminen tai sulkeva Sulje. Nimeä ei voi loppua pisteeseen. Kaikki VMs käytettävyys-ryhmässä täytyy luoda saman resurssiryhmän saatavuus joukkona.
    - **Sijainti** - kohdassa sijainti avattavasta luettelosta.

4. Kun olet tehnyt lisännyt tiedot, valitse **Luo**. Kun käytettävyys-ryhmä on luotu, näet sen luettelosta sen jälkeen, kun päivität portaalin.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Portaalin avulla voit luoda virtual machine ja määritä samanaikaisesti saatavuus

Jos luot uuden AM, portaalissa, voit myös luoda uuden käytettävyys, määrittää AM samalla, kun luot ensimmäisen AM joukon.

![Näyttökuva, jossa näkyy uusi saatavuus, kun luot AM luomiseen.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Lisää uusi AM luotua tietojoukkoa käytettävyys

Kunkin muita AM, jonka luot, joka olisi joukkoon kuuluvat, varmista, että luominen samaan **resurssiryhmä** ja valitse sitten Määritä vaiheessa 3 aiemmin käytettävyys. 

![Näyttökuva valitseminen aiemmin määritetty käyttämään omaa AM saatavuus.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>PowerShellin käytettävyys-joukon luominen

Tässä esimerkissä luodaan saatavuus määrittäminen **RMResGroup** resurssiryhmä **Länsi USA** sijainnissa. Tämä on tehtävä, ennen kuin voit luoda ensimmäisen joukon tulevat AM.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Lisätietoja on artikkelissa [Uusi AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Vianmääritys

- Kun luot AM, jos avattavasta luettelosta portaalissa ei ole haluamasi käytettävyys voi olet luonut sen eri resurssiryhmä. Jos et ole varma, milloin olet käytettävissä resurssiryhmän määrittäminen, toiminto-valikosta ja sitten Selaa > käytettävyys määrittää luettelo käytettävyys joukot ja johon ne kuuluvat ryhmissä.


## <a name="next-steps"></a>Seuraavat vaiheet

Lisää tallennustilaa oman AM lisäämällä muita [tietoja levy](virtual-machines-windows-attach-disk-portal.md).
