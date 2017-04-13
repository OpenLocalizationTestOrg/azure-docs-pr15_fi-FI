## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>VNet, Azure-portaalissa verkon config-tiedoston luominen

Azure määrittää xml-tiedoston avulla kaikki käytettävissä olevat VNets tilaukseen. Voit ladata tämän tiedoston ja muokata sitä, voit muokata tai poistaa aiemmin VNets ja luoda uusia. Tässä asiakirjassa Lue, miten voit ladata tätä tiedostoa, verkon määritysten (tai netcgf)-tiedostona, tarkoitetun ja Muokkaa Luo uusi VNet. Lisätietoja verkko-kokoonpanotiedosto [Azure virtual verkon määritysten rakenteen](https://msdn.microsoft.com/library/azure/jj157100.aspx) tarkistaminen

Voit luoda VNet, netcfg tiedoston palvelun Azure-portaalissa, noudata seuraavia ohjeita.

1. Siirry selaimella, http://manage.windowsazure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Vieritä alaspäin palveluluetteloon ja sitten **verkoissa** tarkastelu alapuolella.

    ![Azure virtual verkot](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. Valitse sivun alareunassa **VIE** -painiketta, alla kuvatulla tavalla.

    ![Vie-painike](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. Valitse **Vie verkon määritys** -sivulla vietävä VPN-määritys-tilaus ja valitse sitten sivun vasemmalla olevasta alakulmassa valintamerkkipainiketta.
5. Noudata selaimen ohjeessa **NetworkConfig.xml** -tiedosto tallennetaan. Varmista, että Huomautus, johon tallennat tiedoston.
6. Avaa tiedosto, jonka tallensit vaiheessa 5 kuvankäsittelyohjelmalla minkä tahansa XML- tai teksti ja Etsi **<VirtualNetworkSites>** elementti. Jos sinulla on jo luotu verkkoja, kunkin verkon näkyvät kuin oma **<VirtualNetworkSite>** elementti.
7. Tässä skenaariossa on kuvattu virtual verkoston luominen, Lisää seuraavat XML alle **<VirtualNetworkSites>** elementti:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  Tallenna tiedosto verkon määritys.
9.  Azure-portaalissa, valitse sivun vasemmalla olevasta alakulmassa **Uusi**ja valitse **VERKKOPALVELUITA**ja valitse sitten **VPN**ja valitse sitten **TUONTIMÄÄRITYS** , alla olevassa kuvassa osoitetulla tavalla.

    ![Tuontimääritys](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  Valitse **Tuo verkko-kokoonpanotiedosto** -sivulla valitsemalla **SELAA, tiedosto...**, valitse Selaa kansioon, olet tallentanut tiedoston kohdassa 8, valitse tiedosto ja valitse sitten **Avaa**. Alla olevassa kuvassa pitäisi muistuttaa web-sivua. Valitse sivun oikeassa alakulmassa, siirry seuraavaan vaiheeseen nuolipainiketta.

    ![Tuo verkon määritys tiedoston sivulla](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   **Verkoston luominen** -sivulla Huomaa, että uuden VNet tapahtuma, alla olevassa kuvassa osoitetulla tavalla.

    ![Verkko-sivun luominen](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Napsauta valintaruutupainiketta luomiseen VNet sivun oikeassa alakulmassa. Muutaman sekunnin kuluttua oman VNet ne näkyvät luettelossa käytettävissä VNets alla olevassa kuvassa osoitetulla tavalla.

    ![Uusi virtual verkko](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)