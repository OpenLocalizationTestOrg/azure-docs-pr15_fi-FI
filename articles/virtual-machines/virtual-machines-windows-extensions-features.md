<properties
 pageTitle="Virtuaalikoneen tunnisteet ja toiminnot | Microsoft Azure"
 description="Katso, mitä laajennukset ovat käytettävissä Azuren näennäiskoneiden, mitä ne tarjota tai parantaa Ryhmittelyperuste."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Tietoja virtuaalikoneen tunnisteet ja toiminnot

## <a name="azure-vm-extensions"></a>Azure AM tunnisteet

Azure virtuaalikoneen laajennukset ovat pieniä sovelluksia, joiden avulla kirjaa käyttöönoton määritys- ja automaatiofunktiot tehtävän Azuren näennäiskoneiden käyttöön. Esimerkiksi jos Virtual Machine edellyttää, että ohjelmisto on asennettu, Anti-Virus suojaus tai Docker määritys-tunniste on AM voidaan näiden tehtävien suorittamiseen. Azure AM laajennusten suorittamista Azure-CLI PowerShell-mallien resurssien hallinta ja Azure-portaalin. Tunnisteet voidaan sisältävien uusi näennäiskoneiden käyttöönottoon tai järjestelmä suorittaa.

Tässä tiedostossa on edellytyksistä Azure virtuaalikoneen tunniste ja ohjeita siitä, miten voit tunnistaa saatavilla AM tunnisteet. 

## <a name="azure-vm-agent"></a>Azure AM agentti

Azure AM agentti hallitsee Azure-virtuaalikoneen ja Azure kangasta ohjauskoneen välistä vuorovaikutusta. AM-agentti on vastuussa toimintojen monia käyttöönotto ja Azuren näennäiskoneiden, mukaan lukien käynnissä AM-laajennusten hallinta. Azure AM agentti Azure valikoiman kuvia valmiiksi asennettuina ja voidaan asentaa tuetut käyttöjärjestelmät. 

Lisätietoja tuetut käyttöjärjestelmät ja ohjeita on artikkelissa [Azure virtuaalikoneen agentti](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Tutustu AM tunnisteet

Useita eri AM laajennukset ovat käytettävissä Azuren näennäiskoneiden kanssa. Saat näkyviin täydellisen luettelon, suorita seuraava komento ja Azure-CLI tilalle valinta sijainti sijainti.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Yleisiä AM-laajennukset

|Tunnisteen nimi   |Kuvaus   |Lisätietoja   |
|---|---|---|
|Windowsin mukautettu komentosarja-laajennus  | Komentosarjojen suorittamisen vastaan Azure Virtual Machine  |[Windowsin mukautettu komentosarja-laajennus](./virtual-machines-windows-extensions-customscript.md)   |
|Windowsin DSC-laajennus | PowerShellin DSC (haluamasi tilan määritys)-tunniste.  | [Docker AM tunniste](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Azure diagnostiikka-tunniste | Azure diagnostiikka hallinta |[Azure diagnostiikka-tunniste](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
