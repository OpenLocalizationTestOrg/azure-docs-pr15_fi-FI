1. Siirry portal, **Uusi**. Kirjoita Etsi "Näennäinen yhdyskäytävien". Etsi **Virtual yhdyskäytävien** return haun ja valitse tapahtuma. **Luo VPN-yhdyskäytävä** -sivu avautuu.
2. Valitse **Luo** **Virtual yhdyskäytävien** sivu alareunassa. **Luo VPN-yhdyskäytävä** -sivu avautuu. Täytä arvot VPN-yhdyskäytävä.

    ![Luo VPN yhdyskäytävän sivu-kentät] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Luo VPN yhdyskäytävän sivu-kentät")

3. **Nimi**: käyttämäsi yhdyskäytävän nimeä. Tämä ei ole sama kuin nimeäminen yhdyskäytävän aliverkon. Luot yhdyskäytävän objektin nimi on.

4. **Yhdyskäytävän tyyppi**: Valitse **VPN-yhteyttä**. VPN kohdeyhdyskäytävä käyttää VPN-yhdyskäytävän tyyppi **VPN-yhteyttä**. 

5. **VPN-tyyppi**: Valitse VPN-tyyppi, joka on määritetty kokoonpanosi. Useimmiten edellyttävät reitti-pohjainen VPN-tyyppi.

6. **Tuote**: Valitse yhdyskäytävän SKU avattavasta valikosta. Avattavan luettelon luetellut tuotteissa määräytyvät sen mukaan, mitä valitset VPN-tyyppi.

7. **Sijainti**: Säädä osoittamaan virtual verkon sijainti **sijainti** -kenttään.
 
8. Valitse virtual verkko, johon haluat lisätä tämän yhdyskäytävä. Valitse **Virtual verkon** Avaa **Valitse näennäinen verkko** -sivu. Valitse VNet. Jos et näe omaa VNet, varmista, että alue, jossa virtual verkon sijaitsee osoittaa **sijainti** -kenttään.

9. Valitse julkinen IP-osoite. Valitse Avaa **Valitse julkinen IP-osoite** -sivu **julkiseen IP-osoite** . Valitse **Luo uusi +** Avaa **Luo julkinen IP-osoite-sivu**. Kirjoita nimi julkiseen IP-osoite. Tämä sivu Luo julkinen IP address objektin, johon julkisen IP-osoitteen dynaamisesti määritetään.<br>Valitse **OK** , jos haluat tallentaa tekemäsi muutokset tämä sivu.

10. **Tilaus**: Varmista, että oikea tilaus on valittuna.

11. **Resurssiryhmä**: tämän asetuksen määräytyy virtuaaliverkko, jossa voit valita. 

12. Älä säädä **sijainti** , kun olet määrittänyt edellisiin asetuksiin.

13. Tarkista asetukset. Voit valita **Kiinnitä Raporttinäkymät-ikkunan** alareunassa sivu, jos haluat, että käyttämäsi yhdyskäytävän näkyvän koontinäytössä.

14. Valitse Aloita luominen yhdyskäytävän **luominen** . Asetukset tarkistetaan, jolloin näkyviin tulee "käyttöönotto Virtual verkon yhdyskäytävän" koontinäytössä-ruutu. Yhdyskäytävän luominen voi kestää jopa 45 minuuttia. Voit joutua päivittämään portaalin sivusi valmiit tilan tarkasteleminen.

    ![Käyttöönotto Virtual yhdyskäytävien] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Käyttöönotto näennäinen yhdyskäytävien")

11. Kun yhdyskäytävä on luotu, voit tarkastella IP-osoite, jolle on määritetty siihen katsomalla virtual verkko-portaalissa. Yhdyskäytävän näkyy yhdistetyn laite. Voit valita yhdistetyn laite (VPN yhdyskäytävä), kun haluat tarkastella tarkempia tietoja.



