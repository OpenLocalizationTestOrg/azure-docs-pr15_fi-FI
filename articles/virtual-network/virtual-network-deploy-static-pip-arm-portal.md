<properties 
   pageTitle="Käyttöönotto AM kanssa staattinen julkiseen IP Azure portaalin resurssien hallinnan avulla | Microsoft Azure"
   description="Opi ottamaan VMs kanssa staattinen julkiseen IP zure portaalin resurssien hallinnan avulla"
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
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-the-azure-portal"></a>Ottaa käyttöön AM kanssa staattinen julkiseen IP Azure-portaalissa

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Luo AM staattinen julkiseen IP 

Luomiseen AM staattinen julkiseen IP-osoite Azure-portaalissa, noudata seuraavia ohjeita.

1. Selaimessa, siirry [Azure portal](https://portal.azure.com) ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse portaalin yläreunan vasemmalla olevasta yläkulmassa **Uusi**>>**Laske**>**Windows Server 2012 R2-Palvelinkeskukseen**.
3. **Valitse käyttöönottomalli** -luettelossa valitse **Resurssienhallinta** ja sitten **Luo**.
4. **Perustiedot** -sivu, valitse Lisää AM tiedot alla kuvatulla tavalla ja valitse sitten **OK**.

    ![Azure portal - perusteet](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)

5. **Valitse koko** -sivu-kohdasta **A1 vakio** alla kuvatulla tavalla ja valitse sitten **Valitse**.

    ![Azure portal - koon valitseminen](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)

6. Valitse **asetukset** -sivu **julkiseen IP-osoite**ja sitten **Luo julkinen IP-osoite** , sivu- **tehtävän**, valitse **staattinen** alla kuvatulla tavalla. Ja valitse sitten **OK**.

    ![Azure portal - Luo julkinen IP-osoite](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)

7. Valitse **asetukset** -sivu valitsemalla **OK**.
8. Tarkista **Yhteenveto** -sivu, alla kuvatulla tavalla ja valitse sitten **OK**.

    ![Azure portal - Luo julkinen IP-osoite](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)

9. Huomaa uusi ruutu koontinäytössä.

    ![Azure portal - Luo julkinen IP-osoite](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)

10. Kun AM on luotu, **asetukset** -sivu näkyy alla kuvatulla tavalla

    ![Azure portal - Luo julkinen IP-osoite](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)