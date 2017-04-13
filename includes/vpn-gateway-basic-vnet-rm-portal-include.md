Voit luoda VNet Resurssienhallinta käyttöönoton mallin Azure-portaalissa, noudata seuraavia ohjeita. Näyttökuvien on esitetty esimerkkejä. Varmista, arvojen korvaaminen omalla. Lisätietoja virtual verkkojen käyttämisestä on artikkelissa [Virtual yleistä](../articles/virtual-network/virtual-networks-overview.md).

1. Selaimessa, siirry [Azure portal](http://portal.azure.com) ja Kirjaudu tarvittaessa sisään Azure-tili.

2. Valitse **Uusi**. Kirjoita **haku marketplace** -kenttään "Virtual Network". Etsi **VPN** palautetut luettelosta ja valitse Avaa **VPN** -sivu.

    ![Etsi näennäinen verkon resurssin sivu] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Etsi VPN resurssin sivu")

3. VPN-sivu, **Valitse käyttöönottomalli** -luettelosta alareunassa Valitse **Resurssienhallinta**ja valitse sitten **Luo**.


    ![Valitse Resurssienhallinta] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Valitse Resurssienhallinta")

4. **Luo virtuaalisia verkko** -sivu-VNet-asetusten määrittäminen. Kun olet täyttänyt kentät, punainen huutomerkki tulee vihreä valintamerkki, kun kenttään kirjoitetut merkit ovat kelvollisia.

    ![Kentän kelpoisuuden tarkistus] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Kentän kelpoisuuden tarkistus")

5. **Luo virtuaalisia verkko** -sivu näyttää samalta seuraavan esimerkin. Voi olla arvot, jotka ovat automaattinen täytetty. Jos näin on, arvojen korvaaminen omalla.

    ![Luo VPN-sivu] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Luo VPN-sivu")

6. **Nimi**: näennäinen verkon nimi.

7. **Osoitetilan**: Kirjoita osoite. Jos sinulla on useita Lisää osoite välilyöntejä, Lisää ensimmäisen osoitetilaa. Voit lisätä osoitteen välilyöntejä myöhemmin, kun olet luonut VNet.
 
8. **Aliverkon nimi**: Lisää aliverkon nimi ja aliverkon osoitealueita. Voit lisätä muita aliverkosta myöhemmin, kun olet luonut VNet.

10. **Tilaus**: Varmista, että luettelossa tilaus on oikea. Voit muuttaa tilaukset vieressä olevaa avattavan luettelon avulla.

11. **Resurssiryhmä**: aiemmin luotu resurssien ryhmä tai luoda uuden kirjoittamalla uusi resurssiryhmä nimi. Jos luot uuden ryhmän, nimeä resurssiryhmä suunnitellun määritys-arvojen mukaan. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](resource-group-overview.md#resource-groups).

12. **Sijainti**: oman VNet sijainti. Sijainti määrittää, missä resurssien avulla voit ottaa käyttöön tässä VNet sijaitsevat.

13. Valitse **raporttinäkymät-ikkunan kiinnittäminen** , jos haluat, että voit etsiä oman VNet helposti koontinäytössä ja valitse sitten **Luo**.
    
    ![Raporttinäkymät-ikkunan kiinnittäminen] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "raporttinäkymät-ikkunan kiinnittäminen")

14. Valittuasi **Luo**näkyy ruudun raporttinäkymän, jotka vaikuttavat oman VNet etenemisen. Ruutu muuttuu, kun VNet luodaan.

    ![Luominen virtual verkko-ruutu] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Luominen virtual verkko-ruutu")