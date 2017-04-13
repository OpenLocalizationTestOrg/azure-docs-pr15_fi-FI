<properties
   pageTitle="Avaa portit AM, Azure-portaalissa | Microsoft Azure"
   description="Lue, miten voit avata portin / Luo päätepisteen oman Windows-AM käyttämällä resurssien hallinnan käyttöönottomalli Azure-portaalissa"
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
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Avaaminen AM portit Azure Azure-portaalissa
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Pikakomennot
Voit myös [Azure PowerShellin avulla seuraavasti](virtual-machines-windows-nsg-quickstart-powershell.md).

Luo verkko-käyttöoikeusryhmän. Valitse resurssiryhmä-portaalissa, valitse **Lisää**, ja valitse Etsi ja valitse 'Verkko-käyttöoikeusryhmän':

![Verkon käyttöoikeusryhmän lisääminen](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Verkko-käyttöoikeusryhmän nimi, valitse tai luoda resurssiryhmän ja valitse sijainti. Valitse **Luo** , kun olet valmis:

![Verkon käyttöoikeusryhmän luominen](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Valitse uusi verkko-käyttöoikeusryhmän. Valitse "saapuvien suojaussäännöt" ja valitse sitten **Lisää** -painiketta säännön luominen:

![Saapuvien säännön lisääminen](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Anna uuden säännön nimi. Portti 80 on jo lisätty oletusarvoisesti. Tämä sivu on, jossa muuttaa lähde, protokolla ja kohde lisättäessä lisäsääntöjä verkko-käyttöoikeusryhmän. Valitse **OK** , jos haluat luoda säännön:

![Saapuvien säännön luominen](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Viimeinen vaihe on verkko-käyttöoikeusryhmän liitettävä aliverkon tai tietyn Verkkokäyttöliittymän. Yhdistää verkko-käyttöoikeusryhmän japanin aliverkon. Valitse "Aliverkosta" ja valitse sitten **Liitä**:

![Yhdistää verkko-käyttöoikeusryhmän aliverkon](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Valitse virtual verkossa, ja valitse sitten sopiva aliverkon:

![Verkon käyttöoikeusryhmän liittäminen virtual verkko](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Olet nyt luonut verkon käyttöoikeusryhmän, luotu saapuvan liikenteen sääntö, joka mahdollistaa tietoliikenteen porttiin 80 ja liittyvän aliverkon. Kaikki kyseisen aliverkon yhdistäminen VMs ovat tavoitettavissa porttiin 80.


## <a name="more-information-on-network-security-groups"></a>Saat lisätietoja verkon käyttöoikeusryhmät
Tässä Pikakomennot mahdollistavat pääset hyvin alkuun liikenne juoksutus oman AM. Verkon käyttöoikeusryhmät on useita ominaisuuksia ja rakeisuuden resurssien käyttöoikeuksien hallintaan. Voit lukea lisää [verkon käyttöoikeusryhmän ja tähän Käyttöoikeusluettelon sääntöjen](../virtual-network/virtual-networks-create-nsg-arm-ps.md)luomisesta.

Voit määrittää verkoston käyttöoikeusryhmiä ja Käyttöoikeusluettelon säännöt Azure Resurssienhallinta mallit osana. Lue lisää [Verkko-käyttöoikeusryhmät mallien](../virtual-network/virtual-networks-create-nsg-arm-template.md)luomisesta.

Jos haluat käyttää portin edelleenlähetystä, voit yhdistää ulkoisen portin oman AM sisäinen porttiin, käytä kuormituksen tasauspalvelun ja verkko-osoitteen käännöksen (NAT) sääntöjä. Haluat ehkä näyttää porttinumeroa 8080 ulkoisesti ja on liikenne ohjataan AM portti TCP 80. Voit opetella [luominen Internetiin yhteydessä oleva-kuormituksen](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä esimerkissä luoda yksinkertaisen säännön sallimaan HTTP-liikenne. Voit etsiä lisätietoja yksityiskohtaisempia ympäristöissä luomisesta on seuraavissa artikkeleissa:

- [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md)
- [Mikä verkon suojauksen ryhmän (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Yleistä kuormituksen tasoitusmääritykset Azure resurssin hallinnasta](../load-balancer/load-balancer-arm.md)
