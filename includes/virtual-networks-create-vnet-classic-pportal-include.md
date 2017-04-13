## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Perinteinen VNet luominen Azure-portaalissa

Luo perinteinen VNet, yllä skenaarion perusteella, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Uusi** > **Verkko** > **Virtual verkossa**, **Valitse käyttöönoton mallin** luettelosta näkyy jo **perinteinen**ja valitse sitten **Luo**, joka näkyy alla olevassa kuvassa.

    ![Luo VNet Azure-portaalissa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Valitse **Virtual verkko** -sivu VNet **nimi** ja valitse sitten **osoitetilaa varten**. VNet ja sen ensimmäisen aliverkon osoite tilaa asetusten määrittäminen ja valitse sitten **OK**. Alla olevassa kuvassa CIDR estoasetukset Microsoftin skenaarion.

    ![Osoitteen tila-sivu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Valitse **Resurssiryhmä** ja resurssien ryhmä, johon haluat lisätä VNet, tai **Luo uusi resurssiryhmä** VNet lisääminen uusi resurssiryhmä. Alla olevassa kuvassa resurssin uusi resurssiryhmä kutsutaan **TestRG**ryhmäasetukset. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Luo resurssien ryhmä-sivu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Tarvittaessa muuttaa oman VNet **tilaus** ja **sijainti** -asetuksia. 

6. Jos et halua nähdä VNet **Startboard**-ruuduksi, poista käytöstä **Startboard PIN-tunnuksen**. 

7. Valitse **Luo** ja Muokkaa ruudun **luominen VPN** -niminen alla olevassa kuvassa osoitetulla tavalla.

    ![Luo VNet-portaalissa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Odota VNet luodaan ja kun ruudun alla, valitse Lisää muita aliverkosta.

    ![Luo VNet-portaalissa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. **Määritysten** pitäisi näkyä oman VNet, alla kuvatulla tavalla. 

    ![Luo VNet-portaalissa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Valitse **aliverkosta** > **Lisää**, kirjoita **nimi** ja määritä aliverkon ulkopuolella **osoitealueita (CIDR estä)** ja valitse sitten **OK**. Alla olevassa kuvassa Microsoftin Nykyinen skenaario asetuksia.

    ![Luo VNet Azure-portaalissa](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)