<properties 
   pageTitle="Julkisten IP-osoitteiden käyttämisestä virtual verkossa"
   description="Opettele määrittämään virtual verkon käyttää julkiseen IP-osoitteet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Julkiseen IP-osoitetilaa Virtual verkon (VNet)

Virtual verkkojen (VNets) voivat sisältää sekä julkisista ja yksityisistä (RFC 1918 osoite estää) IP address välilyöntejä. Kun lisäät julkiseen IP-osoitealueita, sitä käsitellään osana yksityinen VNet IP-osoitetilaa, joka on vain tavoitettavissa VNet yhdistetty VNets ja paikallisen sijainnista.

Alla olevassa kuvassa näkyy VNet, joka sisältää julkisista ja yksityisistä IP-osoite välilyöntejä.

![Käsitteellinen julkiseen IP](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Miten julkiseen IP-osoitealueita lisätään?

Voit lisätä julkiseen IP-osoitealueita samalla tavalla kuin Lisää yksityiset IP-osoitealueita; joko *netcfg* -tiedoston avulla tai lisäämällä määritykset [Azure portal](http://portal.azure.com). Voit lisätä julkiseen IP-osoitealueita, kun luot oman VNet tai voit siirtyä takaisin ja lisätä sen myöhemmin. Alla olevassa esimerkissä sekä julkisista ja yksityisistä IP osoite välilyöntejä saman VNet määritetty.

![Julkiseen IP-osoite-portaalissa](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Onko rajoitettu?

Liittyy muutama IP-osoitealueet, jotka eivät ole sallittuja:

- 224.0.0.0/4 (moni)

- 255.255.255.255/32 (lähetys)

- 127.0.0.0/8 (silmukka)

- 169.254.0.0/16 (link-local)

- 168.63.129.16/32 (sisäinen DNS)

## <a name="next-steps"></a>Seuraavat vaiheet

[DNS-palvelimet VNet käyttämä hallinta](virtual-networks-manage-dns-in-vnet.md)