<properties 
   pageTitle="Luomisesta NSGs ARM-tilassa Azure-portaalissa | Microsoft Azure"
   description="Lue, miten voit luoda ja ottaa käyttöön Azure-portaalissa KÄDESSÄ NSGs"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-manage-nsgs-using-the-azure-portal"></a>Voit hallita NSGs Azure-portaalissa

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luoda NSGs perinteinen käyttöönotto-mallissa](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Esimerkki PowerShell-komennot alla toiminta yksinkertainen ympäristössä, jossa on jo luotu perusteella yllä skenaario. Jos haluat suorittaa komentoja, niin kuin ne näkyvät tässä asiakirjassa-ensin Muodosta testiympäristössä ottamalla [tätä mallia](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla. Seuraavia ohjeita käyttää **RG NSG** mallin otettiin käyttöön resurssiryhmän nimi.

## <a name="create-the-nsg-frontend-nsg"></a>Luo NSG edusta-NSG

Luo **NSG edusta** NSG yllä skenaarion esitetyllä tavalla, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Selaa >** > **verkon käyttöoikeusryhmät**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Valitse **Lisää** **Verkko-käyttöoikeusryhmät** -sivu.
  
    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. **Luo verkko-käyttöoikeusryhmän** , sivu-luominen NSG *NSG edusta* -nimisen *RG NSG* resurssiryhmä ja valitse sitten **Luo**.

    ![Azure portal - NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Sääntöjen luominen aiemmin NSG

Sääntöjen luominen aiemmin luotuun NSG Azure-portaalista, noudata seuraavia ohjeita.

2. Valitse **Selaa >** > **verkon käyttöoikeusryhmät**.

3. Valitse NSGs-luettelosta **NSG FrontEnd** > **saapuvien suojaussäännöt**

    ![Azure portal - NSG edusta](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. **Saapuvien suojaussäännöt**luettelossa valitse **Lisää**.

    ![Azure portal - säännön lisääminen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. **Lisää saapuvien suojauksen sääntö** -sivu-nimeltä *web säännön* ensisijaisuuden *200* salliminen access, *TCP* porttiin *80* , minkä tahansa AM jostain muusta lähteestä säännön luominen ja valitse sitten **OK**. Huomaa, että useimmat näistä asetuksista on jo oletusarvot.

    ![Azure portal - asetusten](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. Muutaman sekunnin kuluttua näet NSG uusi sääntö.

    ![Azure portal - uusi sääntö](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Toista vaiheet 6 luo saapuvan liikenteen sääntö nimeltä *rdp säännön* ensisijaisuuden *250* salliminen access, *TCP* portti *3389* , minkä tahansa AM jostain muusta lähteestä.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Liitä NSG edusta-aliverkon avulla

1. Valitse **Selaa >** > **resurssiryhmien** > **RG NSG**.
2. Valitse **...** **RG NSG** -sivu  >  **TestVNet**.

    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Valitse **asetukset** -sivu **aliverkosta** > **FrontEnd** > **verkon käyttöoikeusryhmän** > **NSG edusta**.

    ![Azure portal - aliverkon asetukset](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. **Edusta** -sivu valitsemalla **Tallenna**.

    ![Azure portal - aliverkon asetukset](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Luo NSG Taustajärjestelmä NSG

Voit luoda **NSG Taustajärjestelmä** NSG ja liittää sen **Taustajärjestelmä** aliverkon, noudata seuraavia ohjeita.

1. Toista vaiheet [Luo NSG edusta-NSG](#Create-the-NSG-FrontEnd-NSG) niminen *NSG Taustajärjestelmä* NSG luominen
2. Toista [aiemmin NSG Luo säännöt](#Create-rules-in-an-existing-NSG) , jotka luovat **Saapuvan liikenteen** säännöt alla olevan taulukon ohjeita.

  	|Saapuvan liikenteen säännön|Lähtevän säännön|
  	|---|---|
  	|![Azure portal - saapuvan liikenteen säännön](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Azure portal - lähtevällä säännöllä](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Toista vaiheet **NSG Taustajärjestelmä** NSG liitettäväksi **Taustajärjestelmä** aliverkon [NSG edusta-aliverkon haluat liittää](#Associate-the-NSG-to-the-FrontEnd-subnet) .

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [aiemmin NSGs](virtual-network-manage-nsg-arm-portal.md) hallinta
- [Ota loki käyttöön](virtual-network-nsg-manage-log.md) NSGs.