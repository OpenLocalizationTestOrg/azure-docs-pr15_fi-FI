<properties 
   pageTitle="Nopeutettu verkko virtual tietokoneelle - portaalin | Microsoft Azure"
   description="Opettele määrittämään nopeutettu verkko Azure virtual-tietokoneeseen, Azure-portaalissa."
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
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Nopeutettu verkko virtual tietokoneelle

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShellin](virtual-network-accelerated-networking-powershell.md)

Nopeutettu verkko mahdollistaa yhden pääkansion i/o Virtualization (SR IOV) virtual tietokoneeseen (AM) sen verkon suorituskyvyn parantaminen huomattavasti. Tehokas Tämä polku ohittaa pienentämisestä viive, Synkronointivirheiden ja suorittimen käyttö eniten vaativia verkon työmääriä, valitse tuetuista AM tyypeistä käytettäväksi Tietopolkua isännän. Tässä artikkelissa kerrotaan, miten määrittäminen Azure Resurssienhallinta käyttöönoton mallin nopeutettu verkko Azure-portaalin avulla. Voit myös luoda AM nopeutettu sovelluksen Azure PowerShellin avulla. Lisätietoja, valitse ohjeaiheen yläreunassa PowerShell-ruutu.

Seuraavassa kuvassa on kaksi näennäiskoneiden (AM) kanssa ja ilman nopeutettu verkko välisen:

![Vertailu](./media/virtual-network-accelerated-networking-portal/image1.png)

Ilman nopeutettu verkko kaikki verkko-liikenne ja siitä uloskirjautuminen AM on siirtää isännän ja virtual valitsin. Virtuaalinen valitsin sisältää kaikki käytännön käyttäminen, kuten verkon käyttöoikeusryhmät, käyttöoikeusluettelot, eristystaso ja muut virtualisoidussa verkkopalvelut verkkoliikennettä. Lisätietoja, lue [Hyper-V verkon virtualisointi ja Virtual valitsin](https://technet.microsoft.com/library/jj945275.aspx) -artikkelista.

Kun nopeutettu verkko-verkkoliikenteen saapuu verkkokortti (NIC) ja välitetään AM. Kaikki verkon käytäntöjä, jotka virtual valitsin koskee ilman nopeutettu verkko luomista ja laitteiden käyttöön. Laitteiston käytäntö otetaan käyttöön mahdollistaa NIC eteenpäin verkkoliikennettä suoraan AM ohittaminen isännän ja virtual valitsin säilyttäen kaikki sitä käytetään isännän käytännön avulla.

Mitä etuja on nopeutettu verkko koskevat vain AM, jotka on otettu käyttöön. Saat parhaan tuloksen on paras mahdollinen, jotta tämä toiminto on vähintään kaksi saman VNet yhteydessä VMs. Kun yhteydessä VNets tai yhdistäviä paikallisen yli, tämä ominaisuus on mahdollisimman vähän vaikuttavia tekijöitä yleinen viive.

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

1. Rekisteröidy esikatselu lähettämällä sähköpostiviestin [Nopeutettu verkko tilaukset](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) Tilaustunnus ja tarkoituksena on toimia. Älä tee vaiheet, kunnes, kun vastaanotat sähköpostiviestin, jossa ilmoitetaan, että sinulla on hyväksytty yhdeksi esikatselu.
2. Kirjaudu Azure-portaalissa osoitteessa http://portal.azure.com.
3. Luo AM toimimalla valitsemalla seuraavat vaihtoehdot [luominen Windowsin AM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) artikkelin ohjeita:
    - Valitse käyttöjärjestelmä on tämän artikkelin rajoitukset-osassa.
    - Valitse sijainti (alue) on tämän artikkelin rajoitukset-osassa.
    - Valitse AM on tämän artikkelin rajoitukset-osassa. Jos jokin tuettu koot ei ole luettelossa, valitse **Näytä kaikki** Valitse laajennetusta luettelosta, **Valitse koko** -sivu.
    - Valitse **asetukset** -sivu valitsemalla *Ota käyttöön* **Accelerated verkko**-seuraavassa kuvassa esitetyllä tavalla:

        ![Asetukset](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] Nopeutettu verkko-vaihtoehto on näkyvissä vain, jos olet:
    >
    >- Esikatselu kyselyjä hyväksytty
    >- Valittu tuettu käyttöjärjestelmä, sijainti ja AM koot rajoitukset on tämän artikkelin kohdassa.

5. Kun AM on luotu, Lataa [nopeutettu verkko ohjaimen](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), Yhdistä ja kirjaudu sisään AM ja suorita driver installer AM sisällä.
6. Napsauta hiiren kakkospainikkeella Windows-painiketta ja valitse **Laitehallinta**. Varmista, että **Mellanox ConnectX 3 Virtual funktion Ethernet-sovittimen** näyttää **Verkko** -asetuksissa laajennettuna, seuraavassa kuvassa esitetyllä tavalla:

    ![Laitehallinta](./media/virtual-network-accelerated-networking-portal/image2.png)