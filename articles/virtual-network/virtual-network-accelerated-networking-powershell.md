<properties 
   pageTitle="Nopeutettu verkko virtual tietokoneelle - PowerShell | Microsoft Azure"
   description="Opettele määrittämään nopeutettu verkko Azure virtual-tietokoneeseen, PowerShellin avulla."
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
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Nopeutettu verkko virtual tietokoneelle

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShellin](virtual-network-accelerated-networking-powershell.md)

Nopeutettu verkko mahdollistaa yhden pääkansion i/o Virtualization (SR IOV) virtual tietokoneeseen (AM) sen verkon suorituskyvyn parantaminen huomattavasti. Tehokas Tämä polku ohittaa pienentämisestä viive, Synkronointivirheiden ja suorittimen käyttö eniten vaativia verkon työmääriä, valitse tuetuista AM tyypeistä käytettäväksi Tietopolkua isännän. Tässä artikkelissa kerrotaan, miten määrittäminen Azure Resurssienhallinta käyttöönoton mallin nopeutettu verkko PowerShellin Azure avulla. Voit myös luoda AM nopeutettu sovelluksen Azure-portaalissa. Lisätietoja, valitse tämän artikkelin alkupuolella olevia ohjeita Azure Portal oleva ruutu.

Seuraavassa kuvassa on kaksi näennäiskoneiden (AM) kanssa ja ilman nopeutettu verkko välisen:

![Vertailu](./media/virtual-network-accelerated-networking-powershell/image1.png)

Ilman nopeutettu verkko kaikki verkko-liikenne ja siitä uloskirjautuminen AM on siirtää isännän ja virtual valitsin. Virtuaalinen valitsin sisältää kaikki käytännön käyttäminen, kuten verkon käyttöoikeusryhmät, käyttöoikeusluettelot, eristystaso ja muut virtualisoidussa verkkopalvelut verkkoliikennettä. Lisätietoja, lue [Hyper-V verkon virtualisointi ja Virtual valitsin](https://technet.microsoft.com/library/jj945275.aspx) -artikkelista.

Kun nopeutettu verkko-verkkoliikenteen saapuu verkkokortti (NIC) ja välitetään AM. Kaikki verkon käytäntöjä, jotka virtual valitsin koskee ilman nopeutettu verkko luomista ja laitteiden käyttöön. Laitteiston käytäntö otetaan käyttöön mahdollistaa NIC eteenpäin verkkoliikennettä suoraan AM ohittaminen isännän ja virtual valitsin säilyttäen kaikki sitä käytetään isännän käytännön avulla.

Mitä etuja on nopeutettu verkko koskevat vain AM, jotka on otettu käyttöön. Saat parhaan tuloksen on paras mahdollinen, jotta tämä toiminto on vähintään kaksi saman VNet yhteydessä VMs.  Kun yhteydessä VNets tai yhdistäviä paikallisen yli, tämä ominaisuus on mahdollisimman vähän vaikuttavia tekijöitä yleinen viive.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Edut

- **Alemman viive / suurempi pakettien määrä toisen (pps):** Virtuaalinen Vaihda poistaminen Tietopolkua poistaa pakettien kuluvaa aikaa käsittelyä isännän ja paketteja, jotka voidaan käsitellä sisällä AM määrä kasvaa.
- **Rajoitetun synkronointivirheiden:** Virtuaalinen valitsin käsittely, riippuu käytännön käytetään korjattava suorittimen, jokaisen käsittelyn työmäärää. Käytännön käyttäminen laitteistoon purkaminen poistaa kyseisen vaihtelevuudesta mukaan välittää paketteja suoraan AM poistaminen AM viestintä ja kaikkiin ohjelmiston keskeytyksiä ja konteksti valitsimet host.
- **Väheni suorittimen käyttö:** Ohittaminen virtual Vaihda vastaanottavaan johtaa vähemmän suorittimen käyttö käsittelyn verkkoliikennettä.

## <a name="limitations"></a>Rajoitukset

Seuraavat rajoitukset olemassa, kun käytät tätä ominaisuutta:
 
- **Verkko-liittymän luominen:** Nopeutettu verkko voi ottaa käyttöön vain näkyy Verkko-käyttöliittymä.  Se ei voi ottaa käyttöön Verkko-liittymään.
- **AM luominen:** Verkkoliittymän nopeutettu tukevassa käytössä vain voidaan liittää AM AM luomisen yhteydessä. Verkkoliittymän ei voi liittää aiemmin AM.
- **Alueet:** Tarjoaa Länsi keskitetyn US ja Länsi Europe Azure alueilla vain. Määritä alueiden laajentaa myöhemmin.
- **Tuettu käyttöjärjestelmä:** Microsoft Windows Server 2012 R2: n ja Windows Server 2016 Technical Preview 5. Linux-ja Windows Server 2012 lisätään pian.
- **AM kokoa:** Standard_D15_v2 ja Standard_DS15_v2 on ainoa tuettu AM esiintymän koot. Lisätietoja on artikkelissa [Windows AM koot](../virtual-machines/virtual-machines-windows-sizes.md) . Tuetut AM esiintymän koot joukko laajentaa myöhemmin.

Näiden rajoitusten muutokset ilmoitettava [päivitykset Azure Virtual verkko](https://azure.microsoft.com/updates/accelerated-networking-in-preview) -sivun kautta.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Luo Windows AM nopeutettu verkko

1. Avaa PowerShellin komentorivi-ikkuna ja jäljellä olevat vaiheet tämän osan yhden PowerShell-istunnossa. Jos sinulla ei ole vielä asentanut ja määrittänyt PowerShell, suorita [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) -artikkelin ohjeiden mukaisesti.
2. Rekisteröitymään esikatseluun lähettää sähköpostia tilauksen tunnus [Nopeutettu verkko tilaukset](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) ja käyttötarkoitus. Älä tee vaiheet, kunnes, kun vastaanotat sähköpostiviestin, jossa ilmoitetaan, että sinulla on hyväksytty yhdeksi esikatselu.
3. Voit rekisteröidä valmius tilauksen kirjoittamalla seuraavat komennot:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Korvaa *westcentralus* toiseen sijaintiin, tukee tätä ominaisuutta (tarvittaessa) on tämän artikkelin [rajoitukset](#limitations) -osassa luetellut nimi. Kirjoita seuraava komento muuttujan sijainnin määrittäminen:

        $locName = "westcentralus"

5. Korvaa *RG1* nimi, joka sisältää näkyy verkkokäyttöliittymä ja määritä asiakirja sitten kannattaa luoda seuraavat komennot resurssiryhmä:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Muuta *VM1 NIC1* mitä haluat verkon-liittymän nimi ja kirjoita seuraava komento:

        $NICName = "VM1-NIC1"

7. Verkkoliittymän on oltava yhdistettynä aliverkon aiemmin luotuun Azure Virtual verkossa oleville (VNet) samassa paikassa ja [tilauksen](../azure-glossary-cloud-terminology.md#subscription) verkon-liittymän nimellä. Lue lisää VNets lukemalla artikkelin [Virtual yleistä](virtual-networks-overview.md) , jos et ole tottunut ne. Luomiseen VNet Suorita [Create VNet](virtual-networks-create-vnet-arm-ps.md) -artikkelin ohjeiden mukaisesti. Muuta seuraavat $Variables "arvot" VNet ja haluat muodostaa verkkoliittymän aliverkon nimiä.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Jos et tiedä aiemmin VNet, valitse vaiheessa 3 sijainti nimeä, kirjoita seuraavat komennot:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Palautettu luettelo on tyhjä, jos haluat luoda VNet sijaintiin. Luodaksesi VNet, suorita [Luo virtuaalisia verkko](virtual-networks-create-vnet-arm-ps.md) -artikkelin ohjeiden mukaisesti.

    Aliverkosta sisällä VNet nimen, kirjoita seuraavat komennot ja vaihda *Subnet1* yläpuolella aliverkon nimi:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Kirjoita Hae VNet ja aliverkon ja määrittää niitä muuttujat seuraavista komennoista.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Määritä aiemman julkiseen IP address resurssin, jotka voidaan liittää verkko-liittymän, joten voit liittyä siihen Internetin välityksellä. Jos et halua käyttää verkko-liittymällä AM Internetin välityksellä, voit ohittaa tämän vaiheen. Ilman julkiseen IP-osoite on yhdistäminen AM kohteesta toiseen saman VNet yhteydessä AM. 

    Muuttaa aiemmin luodun julkisen IP osoite resurssin nimi, joka on sijainnissa luomaasi-Verkkokäyttöliittymän ja, joka ei ole liitetty toiseen verkkoliittymän *PIP1* . Muuta tarvittaessa $rgName nimen resurssiryhmä julkisen IP-osoite-resurssi sijaitsee, ja kirjoita seuraava komento:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Jos et tiedä aiemman julkiseen IP address resurssin nimen, kirjoita seuraavat komennot:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Jos **IPConfiguration** -sarakkeen arvoa ei ole palautettu tulosteesta julkiseen IP-osoite resurssin ei ole liitetty verkko-liittymällä ja voidaan käyttää. Jos luettelo on tyhjä tai ei ole käytettävissä julkiseen IP address resursseja, voit luoda yksi uusi AzureRmPublicIPAddress-komennon avulla.

    >[AZURE.NOTE] Julkisten IP-osoitteiden on korko.vuosi maksu. Lisätietoja hinnat lukemalla [IP-osoite hinnoittelu](https://azure.microsoft.com/pricing/details/ip-addresses) -sivulla.
10. Jos päätit olla lisääminen julkiseen IP-osoite resurssin liittymään, poista *- PublicIPAddress $PIP1* seuraavan komennon lopussa. Muodostaa verkon-liittymän nopeutettu verkko kirjoittamalla seuraava komento:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Määrittää verkkoliittymän AM luotaessa AM vaiheet 3 ja 6 [Create AM](../virtual-machines/virtual-machines-windows-ps-create.md) -artikkelin ohjeiden mukaisesti. Vaiheessa 6-2 korvaa *Standard_A1* jokin AM koot, on tämän artikkelin [rajoitukset](#limitations) -osassa.

    >[AZURE.NOTE] Jos olet muuttanut $locName, $rgName tai $nic muuttujat tämän artikkelin *nimi* , Luo AM artikkelissa vaihe 6 epäonnistuu. Voit kuitenkin muuttaa muuttujien *arvoihin* .

12. Kun AM on luotu, Lataa [nopeutettu verkko ohjaimen](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), yhdistäminen AM ja suorita driver installer sisällä AM.

13. Napsauta hiiren kakkospainikkeella Windows-painiketta ja valitse **Laitehallinta**. Varmista, että **Mellanox ConnectX 3 Virtual funktion Ethernet-sovittimen** näyttää **Verkko** -asetuksissa laajennettuna, seuraavassa kuvassa esitetyllä tavalla:

    ![Laitehallinta](./media/virtual-network-accelerated-networking-powershell/image2.png)