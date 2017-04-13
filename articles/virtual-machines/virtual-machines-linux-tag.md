<properties
   pageTitle="Miten tunnisteen Linux virtual koneen | Microsoft Azure"
   description="Lisätietoja tunnisteita Linux virtual machine-luotu Azure käyttämällä resurssien hallinnan käyttöönottomalli."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Voit merkitä Linux virtual koneen Azure

Tässä artikkelissa kuvataan tapoja tunnistetta Linux virtual koneen Azure kautta resurssien hallinnan käyttöönottomalli. Tunnisteet ovat käyttäjän määrittämä avain/arvo paria, joka voidaan sijoittaa mihin suoraan resurssi tai resurssiryhmä. Azure tukee tällä hetkellä enintään 15 tunnistetta resurssi ja resurssiryhmä kohden. Tunnisteiden saattaa sijoitettu resurssin luomisen yhteydessä tai lisätä aiemmin luotu resurssi. Huomaa, tunnisteet tuetaan vain Resurssienhallinta-käyttöönoton mallin avulla luodut resurssit.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Tunnisteet ja Azure CLI

Voit aloittaa, [Asenna ja Määritä Azure-CLI](../xplat-cli-azure-resource-manager.md) ja varmista, että Resurssienhallinta-tilassa (`azure config mode arm`).

Voit tarkastella kaikkia ominaisuuksia annetun Virtual tietokoneelle, kuten tunnisteita, tällä komennolla:

        azure vm show -g MyResourceGroup -n MyTestVM

Voit lisätä uuden AM tunnisteen Azure-CLI kautta, voit käyttää `azure vm set` sekä tunnisteen parametri **-t**-komennon:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Jos haluat poistaa kaikki tunnisteet, voit käyttää **– T** -parametri `azure vm set` komento.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Kun on käytetty resurssien Azure CLI ja portaalin tunnisteita, voit katsaus Nähdäksesi laskutuksen portaalissa tunnisteet käyttötiedot.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja Azure resurssien merkitsemisestä on artikkelissa [Azure Resurssienhallinta yleiskatsaus][] ja [Käyttämällä tunnisteen, kun haluat järjestää Azure-resurssit][].
* Katso, miten tunnisteiden avulla voit hallita Azure resurssien käyttöä-kohdassa [Azure laskun tietoja][] ja [käyttää tietoja tuominen Microsoft Azure-resurssin kulutus][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Azure resurssin hallinnassa: yleiskatsaus]: ../azure-resource-manager/resource-group-overview.md
[Tunnisteiden avulla voit järjestää Azure-resurssit]: ../resource-group-using-tags.md
[Azure laskun ymmärtäminen]: ../billing/billing-understand-your-bill.md
[Hanki tietoja tuominen Microsoft Azure-resurssin kulutus]: ../billing-usage-rate-card-overview.md
