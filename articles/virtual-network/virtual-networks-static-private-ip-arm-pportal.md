<properties 
   pageTitle="Staattinen yksityinen IP määrittämisestä ARM-tilassa Azure-portaalissa | Microsoft Azure"
   description="Tietoja yksityisen IP-osoitteet (tapahtuneen) ja kuinka voit hallita niiden ARM-tilassa Azure-portaalissa"
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

# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Staattinen yksityisen IP-osoitteen määrittämisestä Azure-portaalissa

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [hallita staattinen yksityinen IP-osoite perinteinen käyttöönotto-mallissa](virtual-networks-static-private-ip-classic-pportal.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Esimerkki ohjeita odottavat yksinkertainen ympäristössä, jossa on jo luotu. Jos haluat suorittaa ohjeita, niin kuin ne näkyvät tässä asiakirjassa, luo ensin kuvattu [luominen vnet](virtual-networks-create-vnet-arm-pportal.md)testiympäristössä.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Luomisesta AM testikäyttöön staattinen yksityiset IP-osoitteet

Et voi määrittää yksityisen staattinen IP-osoitteen AM Resurssienhallinta käyttöönoton tilassa luonnin aikana mukaan Azure-portaalissa. Sinun on luotava ensin AM, tehn määrittäminen sen avulla voidaan staattiset yksityinen IP.

Luo AM, nimeltä *DNS01* VNet, jonka nimi on *TestVNet* *edusta* -aliverkon, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Uusi** > **Laske** > **Windows Server 2012 R2 Palvelinkeskukseen**, Huomaa, että **käyttöönoton mallin valitseminen** luettelosta näkyy jo **Resurssienhallinta**ja valitse sitten **Luo**, joka näkyy alla olevassa kuvassa.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3. Kirjoita **Perustiedot** -sivu (*DNS01* tässä skenaariossa) luoda AM nimi paikallisen järjestelmänvalvojatili ja salasana, joka näkyy alla olevassa kuvassa.

    ![Perustiedot-sivu](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. Varmista, että valittu **sijainti** on *Yhdysvaltojen Keski*-ja valitse kohdasta **resurssiryhmä**, **Valitse aiemmin luotu** sitten **resurssiryhmä** uudelleen, valitse ja valitse sitten *TestRG*ja valitse sitten **OK**.

    ![Perustiedot-sivu](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5. **Valitse koko** -sivu, valitse **A1 vakio**ja valitse sitten **Valitse**.

    ![Valitse kokoa-sivu](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6. **Asetukset** -sivu, varmista, että seuraavat ominaisuudet määritetään asetetaan alla arvot ja valitse sitten **OK**.

    -**Tallennustilan tilin**: *vnetstorage*
    - **Verkko**: *TestVNet*
    - **Aliverkon**: *FrontEnd*

    ![Valitse kokoa-sivu](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7. **Yhteenveto** -sivu valitsemalla **OK**. Huomaa alla ruutu näkyy koontinäyttöön.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Voit noutaa staattisen yksityinen IP-osoitetiedot AM varten

Staattinen yksityinen IP-osoitetiedot, joka on luotu käyttämällä edellä kuvatut vaiheet AM katselemista suorittaa seuraavia ohjeita.

1. Azure Azure-portaalista valitsemalla **SELAA kaikki** > **näennäiskoneiden** > **DNS01** > **kaikkia asetuksia** > **verkkoliittymät** ja valitse sitten vain siinä luettelossa.

    ![Käyttöönotto AM-ruutu](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2. Valitse **Verkko-liittymän** , sivu **kaikki asetukset** > **IP-osoitteet** ja huomaat **varauksen** ja **IP-osoite** -arvoja.

    ![Käyttöönotto AM-ruutu](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Staattinen yksityisen IP-osoitteen lisääminen aiemmin luotuun AM
Voit lisätä luotu edellä kuvatulla tavalla AM staattinen yksityinen IP-osoite, noudata seuraavia ohjeita:

1. Valitse yllä **IP-osoitteet** -sivu, valitse **tehtävän** **staattinen** .
2. Kirjoita *192.168.1.101* **IP**-osoite ja valitse sitten **Tallenna**.

    ![Luo AM Azure-portaalissa](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] Jos jälkeen valitsemalla **Tallenna** , huomaat, että varauksen silti määrittää **dynaamiseksi**, se tarkoittaa sitä, että olet kirjoittanut IP-osoite on jo käytössä. Kokeile eri IP-osoite.

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Staattinen yksityinen IP-osoite poistamisesta AM
Jos haluat poistaa edellä luotu AM staattinen yksityinen IP-osoite, noudata vaiheen avulla.
    
1. **Tehtävän** **dynaaminen** kohdasta yllä **IP-osoitteet** -sivu, ja valitse sitten **Tallenna**.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [varattu julkiseen IP](virtual-networks-reserved-public-ip.md) -osoitteet.
- Lisätietoja [esiintymän tason julkiseen IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -osoitteista.
- Tietojen kysyminen [varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).