## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Luomisesta VNet Azure-portaalissa

Voit luoda VNet, yllä skenaarion perusteella, noudata seuraavia ohjeita.

1. Siirry selaimella, http://manage.windowsazure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Valitse **Uusi** > **VERKKOPALVELUITA** > **VPN** > **Mukautettu luominen** alla olevassa kuvassa osoitetulla tavalla.

    ![Luo VNet-portaalissa](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Valitse **Virtual verkon tiedot** -sivulla VNet **nimi** , valitse **sijainti**ja valitse sitten, siirry vaiheeseen 2 sivun oikeassa alakulmassa olevaa nuolta. Alla olevassa kuvassa Microsoftin skenaarion asetukset.

    ![VPN-tiedot-sivu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Määritä **DNS-palvelimet ja VPN-yhteys** -sivun nimi ja IP-osoite enintään 9 DNS-palvelinten käyttäminen. Jos et määritä DNS-palvelimeen, että VNet käyttää sisäinen nimeämiskäytäntö tarkkuus tarkkuuden Azure myöntämä. Tässä tapauksessa emme ei määrittää DNS-palvelimet.
5. Jos haluat antaa oman VNet pisteen sivuston VPN-käytön, ota käyttöön **Määritä pisteen sivuston VPN** -valintaruutu. Jos pisteen sivuston VPN-yhteyttä ei määritetä, voit lisätä sen, että VNet milloin tahansa luonnin jälkeen. Tässä tapauksessa emme määritetään ei pisteen sivuston VPN-yhteyttä.
6. Jos haluat antaa sivuston sivuston VPN-yhteys välillä oman VNet ja toinen VNet tai paikalliseen verkkoon, ota käyttöön **Määritä sivusto sivusto VPN** -valintaruutu ja määritä, jota haluat käyttää **ExpressRoute** tai Huomautus sekä verkon nimi muodostaa yhteyden. Jos sivusto sivusto VPN-yhteyttä ei määritetä, voit lisätä sen, että VNet milloin tahansa luonnin jälkeen. Tässä tapauksessa emme määritetään ei sivusto sivusto VPN-yhteyttä.
7. Valitse Siirry vaiheeseen 3 alla olevassa kuvassa Microsoftin skenaarion asetukset-sivun oikeassa alakulmassa olevaa nuolta.

    ![DNS-palvelimet ja VPN-yhteyden sivun](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. **Välilyöntejä Virtual verkko-osoite** -sivulla **Käynnistäminen IP**- *10.0.0.0* VNet osoitetilaa muuttamiseen napsauttamalla ja kirjoita sitten ensimmäinen osoitetilaa, jota haluat käyttää. Tässä tapauksessa kirjoita *192.168.0.0*. 
9. Valitse **CIDR (osoite määrä)** -kohdasta aliverkkopeite bittien määrän. Tässä tapauksessa valitse *16 (65536)*.
10. Valitse **ALIVERKOSTA**Napsauta *aliverkon 1* ja anna aliverkon tarvittaessa. Tässä skenaariossa nimetä sen *FrontEnd*.

    >[AZURE.NOTE] Jos valitset nimi-tekstiruutuun, aliverkon ulkopuolella voit voi muokata nimeä, jos aliverkon uudelleen. Voit korjata, sinun on poistettava aliverkon napsauttamalla X-painiketta sen oikealla puolella, Lisää uusi aliverkon sitten vaiheessa 13 kuvatulla tavalla.

11. Määritä ensimmäisen aliverkon **Käynnistäminen IP** aliverkon ensimmäinen IP-osoite. Tässä tapauksessa kirjoita *192.168.1.0*.
12. Valitse ensimmäinen aliverkon aliverkon peite bittien määrän **CIDR (osoite määrä)** . Tässä tapauksessa valitse *24 (256)*.
13. Valitse Lisää uusi aliverkon **Lisää aliverkon** tarvittaessa. Tässä tapauksessa lisää aliverkon ja toista vaiheet 10 – 12 määrittäminen VNet alla olevassa kuvassa osoitetulla tavalla.

    ![VPN osoite välilyöntejä-sivu](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Napsauta valintaruutupainiketta luomiseen VNet sivun oikeassa alakulmassa. Muutaman sekunnin kuluttua oman VNet ne näkyvät luettelossa käytettävissä VNets alla olevassa kuvassa osoitetulla tavalla.

    ![Uusi virtual verkko](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)