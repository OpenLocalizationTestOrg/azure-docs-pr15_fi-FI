<properties
    pageTitle="Yhdistä Windows VMs pilvipalvelussa | Microsoft Azure"
    description="Yhdistä Windows näennäiskoneiden luotu Azure pilvipalvelussa tai VPN perinteinen käyttöönottomalli."
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

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Yhdistä Windows näennäiskoneiden luotu perinteinen käyttöönoton mallin virtual verkko- tai cloud-palvelun kanssa

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Windowsin näennäiskoneiden luotu perinteinen käyttöönottomalli sijoitetaan aina pilvipalvelussa. Pilvipalvelussa sijaitsevaan toimii säilön ja antaa yksilöllinen julkinen DNS-nimi, julkinen IP-osoite ja joukko päätepisteet käyttämään virtuaalikoneen Internetin välityksellä. Pilvipalvelussa sijaitsevaan voi olla virtual verkossa, mutta ei ole tarpeen. Voit myös [yhdistää Linux näennäiskoneiden virtual verkko- tai cloud-palvelun kanssa](virtual-machines-linux-classic-connect-vms.md).

Jos pilvipalveluun ei ole virtual verkkoon, sen nimi on *erillinen* pilvipalvelussa. Erillisestä pilvipalvelussa näennäiskoneiden voivat vain viestiä muiden näennäiskoneiden käyttämällä muiden näennäiskoneiden julkisen DNS-nimiä ja liikenne kulkee Internetin välityksellä. Jos pilvipalveluun on virtual verkkoon, kyseisen pilvipalvelussa näennäiskoneiden voivat viestiä virtual verkon kaikkien muiden näennäiskoneiden lähettämättä liikenteen Internetin välityksellä.

Jos lisäät oman näennäiskoneiden saman erillinen pilvipalvelussa, voit edelleen käyttää kuormituksen tasaamisen ja käytettävyyden joukot. Lisätietoja on artikkelissa [kuormituksen tasaamisen näennäiskoneiden](virtual-machines-windows-load-balance.md) ja [Hallitse näennäiskoneiden käytettävyyttä](virtual-machines-windows-manage-availability.md). Et voi kuitenkin järjestää näennäiskoneiden-aliverkosta tai erillinen pilvipalvelussa liitetään paikalliseen verkkoon. Tässä on esimerkki:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut virtual machine, on kannattaa [lisätä tiedot levyn](virtual-machines-windows-classic-attach-disk.md) siten, että palveluiden ja toiminnoista on tietojen tallennussijainti. 