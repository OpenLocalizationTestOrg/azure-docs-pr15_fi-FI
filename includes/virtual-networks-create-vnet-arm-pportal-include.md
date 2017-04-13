## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Luomisesta VNet Azure-portaalissa

Jos haluat luoda VNet, skenaarion perusteella Azure esikatselu-portaalissa, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Uusi** > **Verkko** > **Virtual verkko**-sitten **Resurssien hallinta** **käyttöönoton mallin valitseminen** luettelosta ja valitse sitten **Luo**, joka näkyy alla olevassa kuvassa.

    ![Luo VNet Azure-portaalissa](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Alla olevassa kuvassa osoitetulla tavalla voit määrittää VNet asetukset **Luo virtuaalisia verkko** -sivu.

    ![Luo VPN-sivu](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Valitse **resurssiryhmä** ja resurssien ryhmä, johon haluat lisätä VNet, tai valitsemalla Lisää uusi resurssiryhmä VNet **Luo uusi** . Alla olevassa kuvassa resurssin uusi resurssiryhmä kutsutaan **TestRG**ryhmäasetukset. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](../articles/resource-group-overview.md#resource-groups).

    ![Resurssiryhmä](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Tarvittaessa muuttaa oman VNet **tilaus** ja **sijainti** -asetuksia. 

6. Jos et halua nähdä VNet **Startboard**-ruuduksi, poista käytöstä **Startboard PIN-tunnuksen**. 

7. Valitse **Luo** ja Muokkaa ruudun **luominen VPN** -niminen alla olevassa kuvassa osoitetulla tavalla.

    ![Luominen VPN-ruutu](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Odota VNet luodaan ja valitse sitten **kaikki asetukset** **Virtual verkko** -sivu > **aliverkosta** > **Lisää** tarkastelu alapuolella.

    ![Osoitteiden lisääminen Azure-portaalissa](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Määritä *taustatietokannan* , aliverkon aliverkon asetukset alla kuvatulla tavalla ja valitse sitten **OK**. 

    ![Aliverkon asetukset](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Huomaa aliverkosta, luettelo alla olevassa kuvassa osoitetulla tavalla.

    ![VNet aliverkosta luettelo](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
