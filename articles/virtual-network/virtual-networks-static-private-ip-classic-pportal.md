<properties 
   pageTitle="Staattinen yksityinen IP määrittämisestä Azure-portaalissa perinteistä | Microsoft Azure"
   description="Tietoja yksityinen staattinen IP-osoitteet ja hallinnasta ne perinteistä Azure-portaalissa"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-the-azure-portal"></a>Staattinen yksityisen IP-osoitteen määrittämisestä (perinteinen) Azure-portaalissa

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [hallita staattinen yksityinen IP-osoite-resurssien hallinnan käyttöönottomalli](virtual-networks-static-private-ip-arm-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Esimerkki ohjeita odottavat yksinkertainen ympäristössä, jossa on jo luotu. Jos haluat suorittaa ohjeita, niin kuin ne näkyvät tässä asiakirjassa, luo ensin kuvattu [luominen vnet](virtual-networks-create-vnet-classic-pportal.md)testiympäristössä.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Määrittäminen staattinen yksityinen IP-osoite AM luotaessa
Luo AM, nimeltä *DNS01* VNet, nimeltä *TestVNet* staattinen yksityinen IP *192.168.1.101*, jossa *edusta* -aliverkon, noudata seuraavia ohjeita:

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Uusi** > **Laske** > **Windows Server 2012 R2 Palvelinkeskukseen**, Huomaa, että **käyttöönoton mallin valitseminen** luettelosta näkyy jo **perinteinen**, ja valitse sitten **Luo**.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3. Kirjoita **Luo AM** -sivu (*DNS01* tässä skenaariossa) luoda AM nimi paikallisen järjestelmänvalvojatili ja salasana.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. Valitse **Valinnainen määritys** > **verkon** > **VPN**, ja valitse sitten **TestVNet**. Jos **TestVNet** ei ole käytettävissä, varmista, että käytät *Yhdysvaltojen Keski* -sijainti ja luonut testiympäristössä, joka on kuvattu tämän artikkelin alussa.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5. Varmista, että valittuna aliverkon on *edusta*- **Verkko** -sivu ja valitse kohdassa **IP-osoitteet**, **IP-osoitteiden määritys** -kohdasta **staattinen**ja kirjoita sitten *192.168.1.101* **IP-osoitetta,** joka näkyy alla.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure04.png)   

6. **Valitse **IP-osoitteet** -sivu** ja valitse **Verkko** -sivu valitsemalla **OK** ja sitten **OK** **Valinnainen config** -sivu.
7. Valitse **Luo** **Luominen AM** -sivu. Huomaa alla ruutu näkyy koontinäyttöön.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Voit noutaa staattisen yksityinen IP-osoitetiedot AM varten

Staattinen yksityinen IP-osoitetiedot, joka on luotu käyttämällä edellä kuvatut vaiheet AM katselemista suorittaa seuraavia ohjeita.

1. Azure Azure-portaalista valitsemalla **SELAA kaikki** > **näennäiskoneiden (perinteinen)** > **DNS01** > **kaikkia asetuksia** > **IP-osoitteet** ja IP-osoitteiden määritys- ja IP-osoitteen alapuolella tarkastelu ilmoitus.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Staattinen yksityinen IP-osoite poistamisesta AM
Jos haluat poistaa edellä luotu AM staattinen yksityinen IP-osoite, noudata seuraavia ohjeita.
    
1. Yllä **IP-osoitteet** -sivu, valitse **dynaaminen** oikealla puolella **IP-osoitteiden määritys**ja valitse **Tallenna**ja valitsemalla **Kyllä**.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Staattinen yksityisen IP-osoitteen lisääminen aiemmin luotuun AM
Voit lisätä luotu edellä kuvatulla tavalla AM staattinen yksityinen IP-osoite, noudata seuraavia ohjeita:

1. Valitse **IP-osoitteet** -sivu on yllä, **staattinen** **IP-osoitteiden määritys**oikealla.
2. Kirjoita *192.168.1.101* **IP-osoite**ja valitse sitten **Tallenna**ja valitse sitten **Kyllä**.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [varattu julkiseen IP](virtual-networks-reserved-public-ip.md) -osoitteet.
- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -osoitteista.
- Tietojen kysyminen [varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).