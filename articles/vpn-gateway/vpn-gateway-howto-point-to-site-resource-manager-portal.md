<properties 
   pageTitle="Resurssien hallinnan käyttöönottomalli ja Azure-portaalin käyttäminen virtual verkkoon pisteen sivuston VPN yhdyskäytävä-yhteyden määrittäminen | Microsoft Azure"
   description="Suojattu yhteys Azure Virtual verkon luomalla pisteen sivuston VPN-yhdyskäytävän yhteys Resurssienhallinta ja Azure-portaalin käyttäminen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>VNet, Azure-portaalissa pisteen sivuston yhteyden asetusten määrittäminen

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Perinteinen - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Pisteen sivuston (P2S) määrityksen voit luoda suojatun yhteyden yksittäisiä asiakastietokoneesta virtual verkkoon. P2S yhteyden on hyödyllinen, kun haluat muodostaa yhteyttä VNet remote sijainnista, kuten kotona tai konferenssiin, tai jos sinulla on vain muutama asiakkaat, jotka on virtual verkkoon. 

Pisteen sivuston yhteydet, eivät edellytä VPN-laitetta tai julkiselle IP-osoite, jos haluat tutustua. VPN-yhteys on muodostettu yhteys käynnistämällä asiakastietokoneesta. Saat lisätietoja pisteen sivuston yhteydet [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [suunnittelu- ja rakenne](vpn-gateway-plan-design.md).

Tässä artikkelissa käydään läpi luomalla VNet pisteen sivuston yhteyden Resurssienhallinta käyttöönoton mallin Azure-portaalissa.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien P2S yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on kaksi käyttöönoton mallit ja käytettävissä käyttöönoton viestintämenetelmien P2S määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavallinen työnkulku 

![Piste-,-sivusto-kaavion] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pisteen sivuston")

### <a name="example"></a>Esimerkkiarvot

- **Nimi: VNet1**
- **Tilaa osoite: 192.168.0.0/16**<br>Tässä esimerkissä Käytämme vain yksi osoitetilaa varten. Voit valita useamman kuin yhden osoitetilaa oman VNet.
- **Aliverkon nimi: FrontEnd**
- **Aliverkon osoitealueita: 192.168.1.0/24**
- **Tilaus:** Jos sinulla on useita tilauksia, varmista, että käytät oikeaa.
- **Resurssiryhmä: TestRG**
- **Sijainti: Itä US**
- **GatewaySubnet: 192.168.200.0/24**
- **VPN yhdyskäytävän nimi: VNet1GW**
- **Yhdyskäytävän tyyppi: VPN**
- **VPN-tyyppi: reitin perusteella**
- **Julkinen IP-osoite: VNet1GWpip**
- **Yhteyden tyyppi: pisteen sivuston**
- **Asiakkaan osoitteen resurssivarantoon: 172.16.201.0/24**<br>VPN-asiakkaille, jotka muodostavat yhteyden käyttämällä pisteen sivuston tätä yhteyttä VNet saa asiakkaan osoitteen varannon IP-osoite.

## <a name="before-beginning"></a>Ennen kuin aloitat

- Varmista, että sinulla on Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>Osa 1 – virtual verkoston luominen

Jos olet luomassa määritysten kuin hioa, voit viitata [esimerkkiarvoja](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. Lisää muita osoitetilan ja aliverkosta

Voit lisätä muita osoitetilan ja aliverkosta oman VNet, kun se on luotu.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. yhdyskäytävän aliverkon luominen

Ennen yhteyden muodostamista virtual verkon yhdyskäytävän, sinun on luoda yhdyskäytävän aliverkon, johon haluat muodostaa virtual verkossa. Jos mahdollista kannattaa luoda yhdyskäytävän aliverkon käyttämän CIDR joukon /28 tai /27 sopimaan tulevien lisämäärityksiä vaatimukset tarpeeksi IP-osoitteet.

Näyttökuvat sisältö on esitetty esimerkki. Muista käyttää GatewaySubnet osoitealueita, joka vastaa kokoonpanon pakolliset arvot.

#### <a name="to-create-a-gateway-subnet"></a>Voit luoda yhdyskäytävän aliverkon

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. (valinnainen) DNS-palvelimen määrittäminen

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Osa 2 – VPN Gatewayn luominen

Pisteen sivuston yhteydet tarvitaan seuraavia asetuksia:

- Yhdyskäytävän tyyppi: VPN
- VPN-tyyppi: reitin perusteella

### <a name="to-create-a-virtual-network-gateway"></a>Voit luoda virtuaalisen yhdyskäytävien

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Osa 3 – Luo varmenteet

Azure käyttävät sertifikaatteja tarkistamiseen pisteen sivuston VPN-yhteydet VPN-asiakkaille. Voit viedä Base-64-koodattu X.509 .cer-tiedosto enterprise-todistus-ratkaisun luoma pääkansion varmenteen tai pääkansion itse allekirjoitetun varmenteen julkisten varmenteen tiedot (ei yksityinen avain). Valitse tuotava julkisen varmenteen tietojen varmenteen päämyöntäjien Azure. Lisäksi tarvitset Luo Asiakasvarmenne varmenteen päämyöntäjien asiakkaille. Kukin asiakas haluaa virtual verkkoon P2S-yhteyden avulla, on oltava asiakkaan varmenne, joka on luotu varmenteen päämyöntäjien asennettuna.

### <a name="getcer"></a>1. hankkiminen varmenteen päämyöntäjien .cer-tiedosto

Jos käytössäsi on enterprise-ratkaisun, voit käyttää aiemmin luotu sertifikaattiketju. Jos käytössäsi ei ole yrityksen CA-ratkaisun, voit luoda pääkansion itse allekirjoitetun varmenteen. Yksi keino itse allekirjoitetun varmenteen luomisesta on makecert.

- Jos käytössäsi on enterprise-sertifikaatin järjestelmän, hanki pääkansio-varmenne, jota haluat käyttää .cer-tiedoston. 

- Jos et käytä enterprise-todistus-ratkaisun, tarvitset pääkansion itse allekirjoitetun sertifikaatin luomiseen. Windows 10-ohjeita on voidaan viitata [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md).

1. Saadakseen .cer-tiedosto varmenteen Avaa **certmgr.msc** ja Etsi pääkansion varmenne. Kaksoisnapsauta pääkansion itse allekirjoitettua varmennetta, **kaikki**tehtävät ja valitse sitten **Vie**. **Varmenteen vieminen-toiminnossa**avautuu.

2. Ohjatun Valitse **Seuraava**, valitse **ei, älä vie yksityinen avain**ja valitse sitten **Seuraava**.

3. Valitse **Viennin tiedostomuoto** -sivulla Valitse **Base-64-koodattu X.509 (. CER).** Valitse sitten **Seuraava**. 

4. Napsauta **Tiedoston vieminen** **Selaa** haluamaasi kohtaan, johon haluat viedä varmenne. **Tiedostonimi**-sertifikaatin tiedoston nimeksi. Valitse sitten **Seuraava**.

5. Valitse varmenteen vieminen **loppuun** .

### <a name="generateclientcert"></a>2. Luo Asiakasvarmenne

Voit luoda yksilölliset varmenteen joko kunkin asiakkaan, joka muodostaa yhteyden, tai voit käyttää samaa varmennetta useasta asiakasohjelmasta käsin. Luodaan yksilöllinen asiakkaan varmenteet etuna on mahdollisuus kumota yhtä varmennetta tarvittaessa. Muussa tapauksessa kaikki on käytössä sama Asiakasvarmenne ja huomaat, että haluat kumota yksi asiakas varmennetta, jos tarvitset Luo ja asentaa uusia sertifikaatteja kaikki ohjelmat, joita todentaa sertifikaatin avulla.

- Jos käytössäsi on enterprise-todistus-ratkaisun, Luo Asiakasvarmenne yleisiä nimi-arvo muodossa 'name@yourdomain.com', sen sijaan, että "toimialueen nimi\käyttäjänimi"-muodossa. 

- Jos käytössäsi on itse allekirjoitettua varmennetta, nähdä luomiseen Asiakasvarmenne [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md) .

### <a name="exportclientcert"></a>3. asiakas-varmenteen vieminen

Asiakasvarmenne tarvitaan todennusta varten. Sen jälkeen luodaan sertifikaattia, vie se. Voit viedä Asiakasvarmenne asennetaan myöhemmin kunkin asiakaskoneen.

1. Voit viedä Asiakasvarmenne, voit käyttää *certmgr.msc*. Napsauta hiiren kakkospainikkeella Asiakasvarmenne, jonka haluat viedä, valitse **Kaikki tehtävät**ja valitse sitten **Vie**.
2. Vie asiakkaan varmenne ja yksityinen avain. Tämä on *.pfx* -tiedosto. Varmista, että tallentaa tai muista salasana (avain), jonka olet määrittänyt tämän todistuksen.

## <a name="addresspool"></a>Osa 4 - Lisää asiakkaan osoitteen resurssivarantoon

1. Kun VPN-yhdyskäytävä on luotu, siirry VPN-yhdyskäytävä-sivu **asetukset** -osassa. Napsauta **asetukset** -osassa **pisteen sivuston määritysten** Avaa **määritys** -sivu.

    ![Valitse sivusto-sivu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "Valitse sivusto-sivu")

2. **Osoitteen varanto** on resurssivarantoon IP-osoitteet, josta asiakkaat, jotka muodostavat saavat IP-osoite. Lisää osoiteryhmän ja valitse sitten **Tallenna**.

    ![asiakkaan osoitteen resurssivarantoon] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "asiakkaan osoitteen resurssivarantoon")

## <a name="uploadfile"></a>Osa 5 – Lataa pääkansion varmenteen .cer-tiedosto

Kun yhdyskäytävä on luotu, voit ladata Azure luotetun varmenteiden varmenne .cer-tiedosto. Voit ladata tiedostoja enintään 20 varmenteet. Ei ladata Azure varmenteen päämyöntäjien yksityinen avain. Kun .cer-tiedosto on ladattu palvelimeen, Azure käyttää sitä todennetaan asiakkaat, jotka virtual verkkoon.

1. Siirry **pisteen sivuston määritysten** sivu. Tämä sivu **varmenteen päämyöntäjien** -osassa Lisää .cer-tiedostoja.

    ![Valitse sivusto-sivu] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "Valitse sivusto-sivu")

2. Varmista, että olet vienyt varmenteen päämyöntäjien Base-64-koodattu X.509 (.cer)-tiedosto. Haluat viedä tässä muodossa, jotta voit avata varmenteen tekstieditorilla.
3. Avaa tekstieditorissa, esimerkiksi Muistiossa varmenne. Kopioi vain seuraavassa osassa:

    ![varmenteen tiedot] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "varmenteen tiedot")

4. Liitä varmenteen tietojen portaalin **Julkiset varmenteen tiedot** -osaan. Laittaa varmenteen nimi **nimi** -paikkaan ja valitse sitten **Tallenna**. Voit lisätä enintään 20 varmenteet.

    ![varmenteen lataaminen] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "varmenteen lataaminen")

## <a name="clientconfig"></a>Osa 6 – lataa ja asenna VPN asiakasohjelman määritysten-paketti

Yhteyden muodostaminen käyttämällä P2S Azure asiakkaiden on oltava sekä Asiakasvarmenne ja asennettu VPN asiakasohjelman määritysten paketin. VPN asiakasohjelman määritysten paketit ovat Windows-asiakkaiden käytettävissä. 

VPN-asiakasohjelman paketti sisältää määrittäminen VPN-Asiakasohjelmistoa, joka on valmiina Windowsissa. Kokoonpano on, johon haluat muodostaa VPN-yhteyttä. Paketin ei asennu lisäohjelmistoa. Katso lisätietoja [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. **Pisteen sivuston määritysten** sivu, valitse **Lataa VPN-asiakasohjelman** Avaa **Lataa VPN-asiakas** -sivu.

    ![VPN-asiakasohjelman lataaminen] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "VPN-asiakasohjelman lataaminen")

2. Valitse oikea paketti asiakkaan ja valitse sitten **Lataa**. 64-bittinen asiakkaat Valitse **AMD64**. 32-bittinen asiakkaat Valitse **x86**.

3. Asenna paketti asiakastietokoneen. Jos saat SmartScreen-valikko, valitse **Lisätietoja**, sitten asentaminen paketin **suorittaminen silti** .

4. Asiakastietokoneen Siirry **Verkkoasetukset** ja valitse **VPN-yhteyttä**. Näet luettelossa yhteys. Se näytetään nimi virtual verkko, jossa se muodostaa yhteyden ja näyttää samalla tavalla kuin tässä esimerkissä: 

    ![VPN-asiakas] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-asiakas")

## <a name="installclientcert"></a>Osa 7 – Asenna asiakkaan varmenne

Kunkin asiakaskoneen täytyy olla Asiakasvarmenne, jotta todennusta. Kun asennat sertifikaattia, sinun on salasanaa, jolla on luotu sertifikaattia vietäessä.

1. Kopioi asiakastietokone .pfx-tiedosto.
2. Kaksoisnapsauta asennusohjeet .pfx-tiedosto. Älä muokkaa asennuksen sijainti.

## <a name="connect"></a>8 - osan yhdistäminen Azure

1. Muodostaa yhteyttä VNet asiakastietokoneen, siirry VPN-yhteydet ja Etsi VPN-yhteyden, jonka loit. Sen nimi on sama nimi kuin virtual verkossa. Valitse **Muodosta yhteys**. Ponnahdusikkunoiden viesti saattaa tulla näkyviin, joka viittaa sertifikaatin avulla. Jos näin käy, valitse **Jatka** käyttämään laajentamiseen. 

2. **Yhteyden** tila-sivulla valitsemalla **Muodosta** yhteys käynnistämiseen. Jos näkyviin **Valitse varmenne** -viesti, varmista, että Asiakasvarmenne, jossa on yksi, johon haluat muodostaa. Jos se ei ole, käyttää oikea sertifikaatti avattavan luettelon nuolta ja valitse sitten **OK**.

    ![VPN-asiakasohjelman 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Asiakkaan VPN-yhteyden")

3. Yhteys nyt vahvistettava.

    ![VPN-asiakasohjelman 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Asiakkaan VPN-yhteyden 2")

## <a name="verify"></a>Osa 9 – Tarkista yhteys

1. Varmista, että VPN-yhteys on aktiivinen, Avaa järjestelmänvalvojan oikeuksin suoritettava komentokehote ja suorita *ipconfig/kaikki*.

2. Tarkastele tuloksia. Huomaa, että olet saanut IP-osoite on yksi osoitteet pisteen sivuston VPN asiakkaan osoitteen varannon, jonka olet määrittänyt kokoonpanoa kuluessa. Tuloksena saadaan jotakin tämän tapaista:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="add"></a>Voit lisätä tai poistaa varmenteet

Voit poistaa Azure luotetun varmenteiden varmenne. Kun poistat luotettu varmenteen, asiakkaan sertifikaatteja, joita luotiin varmenteen päämyöntäjien ei enää voi muodostaa Azure pisteen sivuston kautta. Jos haluat muodostaa asiakkaat, ne on asennettava uusi Asiakasvarmenne, joka on luotu sertifikaatti, joka on luotettu Azure.

Voit hallita käytettävissäsi olevia kumottu asiakkaan varmenteet **pisteen sivuston määritysten** sivu. Tämä on sivu, jota käytit lataaminen [luotetun varmenteiden varmenne](#uploadfile).

## <a name="revokeclient"></a>Voit hallita käytettävissäsi olevia kumottu asiakkaan varmenteet

Voit kumota asiakkaan varmenteet. Kumottujen varmenteiden luettelo voit estää valikoivasti pisteen sivuston connectivity yksittäisiä asiakkaan varmenteet perusteella. Jos poistat pääkansion varmenne-.cer Azure, poistaa kaikki asiakkaan varmenteet luodaan ja kirjautunut kumottu pääkansio-todistus-käyttöä. Jos haluat kumota tietyn Asiakasvarmenne ei pääkansio, tee niin. Sen mukaan, joka luotiin varmenteen päämyöntäjien varmenteet on edelleen voimassa. 

Yleinen käytäntö on pääkansio sertifikaatin avulla voit hallita ryhmän tai organisaation tasolla, access tarkasti rajattuja valvoa yksittäisten käyttäjien kumottu asiakkaan varmenteet ilmenevän vian.

Voit hallita käytettävissäsi olevia kumottu asiakkaan varmenteet **pisteen sivuston määritysten** sivu. Tämä on sivu, jota käytit lataaminen [luotetun varmenteiden varmenne](#uploadfile).


## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisätä virtual machine virtual verkkoon. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.