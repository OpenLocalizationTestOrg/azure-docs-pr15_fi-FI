<properties 
   pageTitle="Hallitse NSGs esikatselu-portaalin resurssien hallinnan avulla | Microsoft Azure"
   description="Opi hallitsemaan exising NSGs esikatselu-portaalin resurssien hallinnan avulla"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-preview-portal"></a>Hallitse NSGs esikatselu-portaalissa

> [AZURE.SELECTOR]
- [Portal](virtual-network-manage-nsg-arm-portal.md)
- [PowerShellin](virtual-network-manage-nsg-arm-ps.md)
- [Azure CLI](virtual-network-manage-nsg-arm-cli.md)

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Tietojen hakeminen

Voit tarkastella aiemmin luotuja NSGs, säännöt noutaminen aiemmin NSG ja selvitä, mitä resursseja NSG on liitetty.

### <a name="view-existing-nsgs"></a>Tarkastele aiemmin NSGs
Jos haluat tarkastella kaikkien aiemmin NSGs Tilauksen, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Selaa >** > **verkon käyttöoikeusryhmät**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Tarkista NSGs luettelossa **verkon käyttöoikeusryhmät** -sivu.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

Voit tarkastella NSGs luettelo **RG NSG** resurssiryhmän noudattamalla seuraavia ohjeita. 

1. Valitse **resurssiryhmät >** > **RG NSG** > **...**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Etsi resurssien luettelon kohteiden näyttäminen NSG-kuvaketta, kuten alla **resurssit** -sivu.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)
         
### <a name="list-all-rules-for-an-nsg"></a>NSG on lueteltu kaikki säännöt

Voit tarkastella NSG **NSG edusta**-niminen sääntöjä, noudata seuraavia ohjeita. 

1. Napsauta **NSG edusta** **verkon käyttöoikeusryhmät** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehdessä **Saapuvan liikenteen säännöt**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. **Saapuvan liikenteen säännöt** -sivu tulee näkyviin alla kuvatulla tavalla.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Valitse **asetukset** -välilehdessä **Lähtevän suojaussäännöt** Nähdäksesi lähtevän liikenteen säännöt.

>[AZURE.NOTE] Voit tarkastella oletusarvon sääntöjä, sivu, jossa näkyy säännöt yläreunassa kuvaketta **oletusarvon säännöt** .

### <a name="view-nsgs-associations"></a>Näytä NSGs yhteydet

Voit tarkastella mitä **NSG edusta** -NSG on resursseja yhdistää noudattamalla seuraavia ohjeita.

1. Napsauta **NSG edusta** **verkon suojausryhmien** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehdessä voit tarkastella tietoja aliverkosta liittyvät NSG **aliverkosta** .

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Valitse **asetukset** -välilehdessä voit tarkastella mitä NIC liittyvät NSG **verkon liityntäkohdat** .

## <a name="manage-rules"></a>Sääntöjen hallinta

Voit lisätä sääntöjä aiemmin NSG, muokata aiemmin luotuja sääntöjä ja Poista säännöt.

### <a name="add-a-rule"></a>Lisää sääntö

Lisää sääntö salliminen **Saapuvan** liikenteen porttiin **443** -konetta **NSG edusta** NSG, noudata seuraavia ohjeita.

1. Napsauta **NSG edusta** **verkon käyttöoikeusryhmät** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehdessä **Saapuvan liikenteen säännöt**.
3. Valitse **Lisää** **Saapuva suojaussäännöt** -sivu. Valitse sitten **Lisää saapuvien suojauksen sääntö** -sivu täytä arvot alla kuvatulla tavalla ja valitse sitten **OK**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

4. Muutaman sekunnin kuluttua Huomaa **Saapuvan liikenteen säännöt** -sivu uusi sääntö.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Säännön muuttaminen

Jos haluat muuttaa luotu edellä sallimaan **Internet** vain saapuvan liikenteen sääntö, noudata seuraavia ohjeita.

1. Napsauta **NSG edusta** **verkon käyttöoikeusryhmät** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehden yläpuolella luotu sääntö.
3. **Salli https** -sivu, valitse Muuta **lähde** -ominaisuutta, alla kuvatulla tavalla ja valitse sitten **Tallenna**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Säännön poistaminen

Jos haluat poistaa säännön, joka on luotu edellä, noudata seuraavia ohjeita.

1. Napsauta **NSG edusta** **verkon suojausryhmien** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehden yläpuolella luotu sääntö.
3. Valitse **Salli https** -sivu valitsemalla **Poista**ja valitse sitten **Kyllä**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Hallitse yhteydet

Voit liittää NSG aliverkosta, ja NIC. Voit myös dissociate NSG minkä tahansa resurssin, se on liitetty.

### <a name="associate-an-nsg-to-a-nic"></a>Liitä NSG NIC: ssä voit

Liitä **NSG edusta** NSG **TestNICWeb1** NIC: ssä, noudata seuraavia ohjeita.

1. Napsauta **NSG edusta** **verkon käyttöoikeusryhmät** -sivu- tai **resurssit** -sivu, kuten alla.
2. Valitse **asetukset** -välilehdessä **verkkoliittymät** > **liittää** > **TestNICWeb1**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Dissociate NSG-NIC: ssä

Voit dissociate **NSG edusta** - **TestNICWeb1** NIC NSG, noudata seuraavia ohjeita.

1. Azure-portaalista, valitse **resurssiryhmät >** > **RG NSG** > **...**  >  **TestNICWeb1**.
2. Valitse **TestNICWeb1** , sivu **suojausasetuksia...**  > **None**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

>[AZURE.NOTE] Voit myös liittää, kaikki olemassa olevat NSG NIC tämä sivu.

### <a name="dissociate-an-nsg-from-a-subnet"></a>Dissociate aliverkosta NSG

Voit dissociate **NSG edusta** NSG **edusta** -aliverkosta, noudata seuraavia ohjeita.

1. Azure-portaalista, valitse **resurssiryhmät >** > **RG NSG** > **...**  >  **TestVNet**.
2. Valitse **asetukset** -sivu **aliverkosta** > **FrontEnd** > **verkon käyttöoikeusryhmän** > **ei mitään**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. **Edusta** -sivu valitsemalla **Tallenna**.

![Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Liitä NSG aliverkon avulla

Jos haluat liittää **NSG edusta** NSG **FronEnd** aliverkon uudelleen, noudata seuraavia ohjeita.

1. Azure-portaalista, valitse **resurssiryhmät >** > **RG NSG** > **...**  >  **TestVNet**.
2. Valitse **asetukset** -sivu **aliverkosta** > **FrontEnd** > **verkon käyttöoikeusryhmän** > **NSG edusta**.
3. **Edusta** -sivu valitsemalla **Tallenna**.

>[AZURE.NOTE] Voit myös liittää NSG thh NSG **asetukset** -sivu aliverkon.

## <a name="delete-an-nsg"></a>Poista NSG

Voit poistaa NSG vain, jos se ei liity mihin tahansa resurssiin. Jos haluat poistaa NSG, noudata seuraavia ohjeita.

1. Azure-portaalista, valitse **resurssiryhmät >** > **RG NSG** > **...**  >  **NSG edusta**.
2. Valitse **asetukset** -sivu **verkkoliittymät**.
3. Jos määritettynä on minkä tahansa NIC luettelossa, valitse Verkkokortti ja noudata [Dissociate NSG NIC-](#Dissociate-an-NSG-from-a-NIC)vaiheessa 2.
4. Toista vaihe 3 kunkin NIC-sivustosta.
5. Valitse **asetukset** -sivu **aliverkosta**.
6. Jos määritettynä on minkä tahansa aliverkosta luettelossa, valitse aliverkon ja noudata ohjeita, 2 ja 3 [Dissociate aliverkosta NSG](#Dissociate-an-NSG-from-a-subnet).
7. Vierittää vasemmalle **NSG edusta** -sivu, valitse sitten **Poista** > **Kyllä**.

[Azure portal - NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ota loki käyttöön](virtual-network-nsg-manage-log.md) NSGs.
