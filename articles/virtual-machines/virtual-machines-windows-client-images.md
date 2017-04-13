<properties
   pageTitle="Käyttämällä Windows asiakkaan kuvia keskihajonta/testi skenaarioissa | Microsoft Azure"
   description="Opit käyttämään Visual Studio tilauksen edut käyttöön Windows 7/8/10 Azure keskihajonta/testi skenaarioissa"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Windows-asiakasohjelman käyttäminen Azure keskihajonta/testi skenaarioissa

Voit käyttää Windows 7, Windows 8: n tai Windows Azure keskihajonta/testi skenaarioissa 10 Jos sinulla on tarvittavat Visual Studio (aiemmalta nimeltään MSDN)-tilaus. Tässä artikkelissa käsitellään käynnissä olevan Windows-tietokone Azure ja Azure-valikoiman kuvia käyttäminen kelpoisuusvaatimukset.


## <a name="subscription-eligibility"></a>Tilauksen kelpoisuus
Aktiivinen Visual Studio tilaajille (henkilöt, jotka ovat hankkineet Visual Studio tilauskäyttöoikeus) voit käyttää Windows-asiakasohjelman kehittämisen ja testauksen. Windows-asiakas voidaan käyttää omia laitteiston ja Azuren näennäiskoneiden, minkä tyyppisessä Azure tilauksen. Windows-asiakasohjelman saa voidaan ottaa käyttöön tai käyttää tavallista käytettäväksi Azure tai käyttävät henkilöt, jotka eivät ole aktiivinen Visual Studio-tilaajille.

Ladattavana on tehnyt tiettyjä Windows 10-kuvia käytettävissä alueella [olevalla keskihajonta/testi tarjoaa](#eligible-offers)Azure-valikoimasta. Visual Studio tilaajille sisällä kaikenlaisia tarjouksen voi myös [riittävästi valmisteleminen ja luo](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bittinen Windows 7, Windows 8: n tai Windows 10-kuva ja valitse [Lataa Azure](virtual-machines-windows-upload-image.md). Käytä pysyy rajoitettu keskihajonta/testaaminen mukaan aktiivinen Visual Studio-tilaajille.


## <a name="eligible-offers"></a>Olevalla tarjouksia
Seuraavassa taulukossa kuvataan tarjous tunnukset, jotka on voivat ottaa käyttöön Windows 10 – Azure-valikoima. Windows 10-kuvat näkyvät vain seuraavat tarjouksia. Visual Studio-tilaajille, jotka on suoritettava Windows-asiakasohjelman eri tarjouksen tyyppi edellyttävät [riittävästi valmisteleminen](virtual-machines-windows-prepare-for-upload-vhd-image.md) ja luoda on 64-bittinen Windows 7: ssä, Windows 8: ssa tai Windows 10-kuva ja [sitten Lataa Azure](virtual-machines-windows-upload-image.md).

| Tarjouksen nimi | Tarjouksen numero | Käytettävissä olevat asiakkaan kuvat |
|:-----------|:------------:|:-----------------------:|
| [Ryhmävakuutussopimukset keskihajonta ja testi](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10: ssä |
| [Visual Studio Enterprise (MPN)-tilaajille](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10: ssä |
| [Visual Studio Professional-tilaajille](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10: ssä |
| [Visual Studio testi Professional-tilaajille](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10: ssä |
| [Visual Studio Premium MSDN (etu) kanssa](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10: ssä |
| [Visual Studio Enterprise-tilaajille](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10: ssä |
| [Visual Studio Enterprise (BizSpark)-tilaajille](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10: ssä |
| [Yrityksen keskihajonta ja testi](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10: ssä |


## <a name="check-your-azure-subscription"></a>Tarkista Azure tilauksen
Jos et tiedä tarjouksen tunnus, voit hankkia sen Azure portaalin tai portaalin tilin kautta.

Tarjouksen Tilaustunnus on merkitty Azure portaalin "Tilaukset"-sivu:

![Tarjouksen tiedot Azure-portaalista](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Voit myös tarkastella tarjouksen tunnus ["Tilaukset-välilehdestä](http://account.windowsazure.com/Subscriptions) Azure-tili-portaalin:

![Tarjouksen tiedot Azure-tili-portaalista](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt ottaa yhteyttä VMs [PowerShell](virtual-machines-windows-ps-create.md)sekä [Resurssienhallinta mallit](virtual-machines-windows-ps-template.md)ja [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
