<properties
 pageTitle="Virtuaalikoneen tunnisteet ja toiminnot | Microsoft Azure"
 description="Katso, mitä laajennukset ovat käytettävissä Azuren näennäiskoneiden, mitä ne tarjota tai parantaa Ryhmittelyperuste."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="nepeters"/>

# <a name="about-virtual-machine-extensions-and-features"></a>Tietoja virtuaalikoneen tunnisteet ja toiminnot

## <a name="azure-vm-extensions"></a>Azure AM tunnisteet

Azure virtuaalikoneen laajennukset ovat pieniä sovelluksia, joiden avulla kirjaa käyttöönoton määritys- ja automaatiofunktiot tehtävän Azuren näennäiskoneiden käyttöön. Esimerkiksi jos Virtual Machine edellyttää, että ohjelmisto on asennettu, Anti-Virus suojaus tai Docker määritys-tunniste on AM voidaan näiden tehtävien suorittamiseen. Azure AM laajennusten suorittamista Azure-CLI PowerShell-mallien resurssien hallinta ja Azure-portaalin. Tunnisteet voidaan sisältävien uusi näennäiskoneiden käyttöönottoon tai järjestelmä suorittaa.

Tässä tiedostossa on edellytyksistä Azure virtuaalikoneen tunniste ja ohjeita siitä, miten voit tunnistaa saatavilla AM tunnisteet. 

## <a name="azure-vm-agent"></a>Azure AM agentti

Azure AM agentti hallitsee Azure-virtuaalikoneen ja Azure kangasta ohjauskoneen välistä vuorovaikutusta. AM-agentti on vastuussa toimintojen monia käyttöönotto ja Azuren näennäiskoneiden, mukaan lukien käynnissä AM-laajennusten hallinta. Azure AM agentti Azure valikoiman kuvia valmiiksi asennettuina ja voidaan asentaa tuetut käyttöjärjestelmät. 

Lisätietoja tuetut käyttöjärjestelmät ja ohjeita on artikkelissa [Azure Linux agentti käyttöoppaassa](./virtual-machines-linux-agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Tutustu AM tunnisteet

Useita eri AM laajennukset ovat käytettävissä Azuren näennäiskoneiden kanssa. Saat näkyviin täydellisen luettelon, suorita seuraava komento ja Azure-CLI tilalle valinta sijainti sijainti.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Yleisiä AM-laajennukset

|Tunnisteen nimi   |Kuvaus   |Lisätietoja   |
|---|---|---|
|Mukautettu komentosarja-laajennus Linux  | Komentosarjojen suorittamisen vastaan Azure Virtual Machine  |[Mukautettu komentosarja-laajennus Linux](./virtual-machines-linux-extensions-customscript.md)   |
|Docker tunniste |Asentaa Docker daemon tukemaan Docker peruskomennot.  | [Docker AM tunniste](./virtual-machines-linux-dockerextension.md)  |
|AM Access-tunniste | Alkaa taas käyttää Azure Virtual Machine  |[AM Access-tunniste](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
|Azure diagnostiikka-tunniste |Azure diagnostiikka hallinta |[Azure diagnostiikka-tunniste](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |

