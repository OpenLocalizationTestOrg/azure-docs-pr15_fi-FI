<properties
   pageTitle="Azure-portaalissa Azure Virtual verkkoon pisteen sivuston VPN yhdyskäytävä-yhteyden määrittäminen | Microsoft Azure"
   description="Suojattu yhteys Azure Virtual verkon luomalla pisteen sivuston VPN-yhdyskäytävän yhteys Azure-portaalissa."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>VNet, Azure-portaalissa pisteen sivuston yhteyden asetusten määrittäminen

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Perinteinen - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Tässä artikkelissa käydään läpi luomalla VNet pisteen sivuston yhteyden perinteinen käyttöönoton mallin Azure-portaalissa. Pisteen sivuston (P2S) määrityksen voit luoda suojatun yhteyden yksittäisiä asiakastietokoneesta virtual verkkoon. P2S yhteyden on hyödyllinen, kun haluat muodostaa yhteyttä VNet remote sijainnista, kuten kotona tai neuvotteluun. Tai jos vain on muutama asiakkaat, jotka on virtual verkkoon.

Pisteen sivuston yhteydet, eivät edellytä VPN-laitetta tai julkiselle IP-osoite, jos haluat tutustua. VPN-yhteys on muodostettu yhteys käynnistämällä asiakastietokoneesta. Saat lisätietoja pisteen sivuston yhteydet [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [VPN yhdyskäytävän](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien P2S yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on kaksi käyttöönoton mallia ja käytettävissä käyttöönottomenetelmät P2S määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavallinen työnkulku 

![Piste-,-sivusto-kaavion] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pisteen sivuston")


Seuraavissa osissa käy läpi vaiheet, Luo pisteen sivuston suojatun yhteyden virtual verkkoon. 

1. VPN- ja VPN-yhdyskäytävän luominen
2. Luo varmenteet
3. Lataa .cer-tiedosto
4. Luo VPN asiakasohjelman määritysten-paketti
5. Asiakastietokone
6. Yhteyden muodostaminen Azure

### <a name="example-settings"></a>Esimerkki asetukset

Voit käyttää esimerkiksi seuraavia asetuksia:

- **Nimi: VNet1**
- **Tilaa osoite: 192.168.0.0/16**
- **Aliverkon nimi: FrontEnd**
- **Aliverkon osoitealueita: 192.168.1.0/24**
- **Tilaus:** Jos sinulla on useita tilauksia, varmista, että käytät oikeaa.
- **Resurssiryhmä: TestRG**
- **Sijainti: Itä US**
- **Yhteyden tyyppi: pisteen sivuston**
- **Asiakkaan osoitetilaa: 172.16.201.0/24**. VPN-asiakkaille, jotka muodostavat yhteyden käyttämällä pisteen sivuston tätä yhteyttä VNet saavat IP-osoitteen määritettyyn ryhmään.
- **GatewaySubnet: 192.168.200.0/24**. Yhdyskäytävän aliverkon käytettävä nimi "GatewaySubnet".
- **Kokoa:** Valitse yhdyskäytävän tuote, jota haluat käyttää.
- **Reitityksen tyyppi: dynaaminen**

## <a name="vnetvpn"></a>Osa 1 - virtual verkko- ja VPN-yhdyskäytävän luominen

### <a name="createvnet"></a>Osa 1: Luo virtuaalisia verkko

Jos sinulla ei vielä ole virtual verkkoon, luo sellainen. Näyttökuvat on esitetty esimerkkejä. Varmista, arvojen korvaaminen omalla. Voit luoda VNet Azure-portaalissa, tekemällä seuraavat toimet: 

1. Selaimessa, siirry [Azure portal](http://portal.azure.com) ja Kirjaudu tarvittaessa sisään Azure-tili.

2. Valitse **Uusi**. Kirjoita **haku marketplace** -kenttään "Virtual Network". Etsi **VPN** palautetut luettelosta ja valitse Avaa **VPN** -sivu.

    ![Etsi VPN-sivu] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Etsi VPN-sivu")

3. VPN-sivu, **Valitse käyttöönottomalli** -luettelosta alareunassa Valitse **perinteinen**ja valitse sitten **Luo**.

    ![Valitse käyttöönottomalli] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Valitse käyttöönottomalli")

4. **Luo virtuaalisia verkko** -sivu-VNet-asetusten määrittäminen. Tämä sivu Lisää ensimmäisen osoitetilan ja yksittäisen aliverkon osoitealueita. Kun olet luonut VNet, voit siirtyä takaisin ja lisätä osoite välilyöntejä ja muita aliverkosta.

    ![Luo VPN-sivu] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Luo VPN-sivu")

5. Varmista, että **tilaus** on oikea. Voit muuttaa tilaukset vieressä olevaa avattavan luettelon avulla.

6. Valitse **resurssiryhmä** ja joko aiemmin luotu resurssien ryhmä tai luoda uuden kirjoittamalla uusi resurssiryhmä nimi. Jos luot uuden ryhmän, nimeä resurssiryhmä suunnitellun määritys-arvojen mukaan. Lisätietoja resurssiryhmät käy [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Valitse seuraavaksi oman VNet **sijainnin** asetuksia. Sijainnin määrittävät, johon resurssien avulla voit ottaa käyttöön tässä VNet tallennetaan.

8. Valitse **raporttinäkymät-ikkunan kiinnittäminen** , jos haluat, että voit etsiä oman VNet helposti koontinäytössä ja valitse sitten **Luo**.
    
    ![Raporttinäkymät-ikkunan kiinnittäminen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "Raporttinäkymät-ikkunan kiinnittäminen")


9. Valittuasi Luo näkyy ruudun raporttinäkymän, jotka vaikuttavat oman VNet etenemisen. Ruutu muuttuu, kun VNet luodaan.

    ![Luominen virtual verkko-ruutu] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Luominen virtual verkko-ruutu")

10. Kun olet luonut virtual verkossa, jotta voit käsitellä nimenselvitys voit lisätä DNS-palvelimen IP-osoite. Avaa virtual verkoston asetusten, valitse DNS-palvelimia ja lisää DNS-palvelin, jota haluat käyttää IP-osoite. Tämä asetus ei luoda uuden DNS-palvelimeen. Muista lisätä DNS-palvelin, jossa resurssit voivat viestiä.

Kun virtual verkossa on luotu, näyttöön tulee **luotu** kohdasta **tila** verkot-sivulla Azure perinteinen portaalissa.

### <a name="gateway"></a>Osa 2: Luo yhdyskäytävä aliverkon ja dynaaminen reititys yhdyskäytävän

Tässä vaiheessa luodaan yhdyskäytävän aliverkon ja dynaaminen reititys yhdyskäytävän. Perinteinen käyttöönoton mallin Azure Portalissa yhdyskäytävän aliverkon ja yhdyskäytävän luominen voidaan toteuttaa saman näiden määritysten avulla.

1. Siirry-portaalissa, johon haluat luoda yhdyskäytävän virtual verkkoon.

2. Valitse sivu virtual verkon, valitse **Yhteenveto** -sivu VPN-yhteydet-osassa **Yhdyskäytävä**.

    ![Luo yhdyskäytävä napsauttamalla tätä] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Luo yhdyskäytävä napsauttamalla tätä")


3. Valitse **Uusi VPN-yhteyden** , sivu **pisteen sivuston**.

    ![P2S yhteyden tyyppi] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S yhteyden tyyppi")

4. **Asiakkaan osoitetilaa**Lisää IP-osoitealueita. Tämä on solualue, josta VPN-asiakkaiden saavat IP-osoitteen muodostettaessa. Poista automaattinen täytetty alueen ja lisätä omia.

    ![Asiakas-osoitetilaa varten] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Asiakas-osoitetilaa varten")

5. Valitse **Luo yhdyskäytävä heti** -valintaruutu.

    ![Luo yhdyskäytävä heti] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Luo yhdyskäytävä heti")

6. Valitse **Valinnainen yhdyskäytävän määritysten** Avaa **yhdyskäytävän määritys** -sivu.

    ![Valitse valinnainen yhdyskäytävän määritys] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Valitse valinnainen yhdyskäytävän määritys")

7. Valitse Lisää **yhdyskäytävän aliverkon** **aliverkon määrittää tarvittavat asetukset** . Vaikka ei voi luoda yhdyskäytävän aliverkon /29 mahdollisimman pieni, on suositeltavaa, että luot suurempi aliverkon, joka sisältää useita osoitteita valitsemalla vähintään /28 tai /27. Tämä sallii tarpeeksi osoitteiden sopimaan mahdollista lisämäärityksiä, haluat ehkä myöhemmin.

    >[AZURE.IMPORTANT] Kun käsittelet yhdyskäytävän aliverkosta, vältä liittämisestä verkon käyttöoikeusryhmän (NSG) yhdyskäytävä on käytettävissä aliverkon ulkopuolella. Avautuvassa verkon suojaus-ryhmä, johon haluat tämän aliverkon saattaa aiheuttaa VPN-yhdyskäytävän lakkaa toimimasta oikein. Saat lisätietoja verkon käyttöoikeusryhmät [verkon käyttöoikeusryhmän ominaisuudet?](../articles/virtual-network/virtual-networks-nsg.md)

    ![Lisää GatewaySubnet] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "Lisää GatewaySubnet")

8. Yhdyskäytävän **koon**valitseminen Tämä on yhdyskäytävän tuote, voit luoda VPN-yhdyskäytävä. Oletus-tuote on **Basic**-portaalissa. Lisätietoja yhdyskäytävän tuotteissa Katso [Lisätietoja VPN yhdyskäytäväasetukset](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![yhdyskäytävän kokoa](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Valitse käyttämäsi yhdyskäytävän **Reititys tyyppi** . P2S määritykset edellyttävät **dynaaminen** reititys tyyppi. Valitse **OK** , kun olet määrittänyt tämän sivu.

    ![Määritä reitityksen tyyppi] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Määritä reitityksen tyyppi")

10. Valitse **Uusi VPN-yhteyden** , sivu **OK** sivu alareunassa Aloita luominen VPN-yhdyskäytävä. Tämä voi kestää jopa 45 minuuttia. 


## <a name="generatecerts"></a>Osa 2 – Luo varmenteet

Azure käyttävät sertifikaatteja tarkistamiseen pisteen sivuston VPN-yhteydet VPN-asiakkaille. Voit viedä Base-64-koodattu X.509 .cer-tiedosto enterprise-todistus-ratkaisun luoma pääkansion varmenteen tai pääkansion itse allekirjoitetun varmenteen julkisten varmenteen tiedot (ei yksityinen avain). Valitse tuotava julkisen varmenteen tietojen varmenteen päämyöntäjien Azure. Lisäksi tarvitset Luo Asiakasvarmenne varmenteen päämyöntäjien asiakkaille. Kukin asiakas haluaa virtual verkkoon P2S-yhteyden avulla, on oltava asiakkaan varmenne, joka on luotu varmenteen päämyöntäjien asennettuna.

### <a name="cer"></a>Osa 1: Hanki varmenteen päämyöntäjien .cer-tiedosto


Jos käytössäsi on enterprise-ratkaisun, voit käyttää aiemmin luotu sertifikaattiketju. Jos käytössäsi ei ole yrityksen CA-ratkaisun, voit luoda pääkansion itse allekirjoitetun varmenteen. Yksi keino itse allekirjoitetun varmenteen luomisesta on makecert.

- Jos käytössäsi on enterprise-sertifikaatin järjestelmän, hanki pääkansio-varmenne, jota haluat käyttää .cer-tiedoston. 

- Jos et käytä enterprise-todistus-ratkaisun, tarvitset pääkansion itse allekirjoitetun sertifikaatin luomiseen. Windows 10-ohjeita on voidaan viitata [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md).

1. Saadakseen .cer-tiedosto varmenteen Avaa **certmgr.msc** ja Etsi pääkansion varmenne. Kaksoisnapsauta pääkansion itse allekirjoitettua varmennetta, **kaikki**tehtävät ja valitse sitten **Vie**. **Varmenteen vieminen-toiminnossa**avautuu.

2. Ohjatun Valitse **Seuraava**, valitse **ei, älä vie yksityinen avain**ja valitse sitten **Seuraava**.

3. Valitse **Viennin tiedostomuoto** -sivulla Valitse **Base-64-koodattu X.509 (. CER).** Valitse sitten **Seuraava**. 

4. Napsauta **Tiedoston vieminen** **Selaa** haluamaasi kohtaan, johon haluat viedä varmenne. **Tiedostonimi**-sertifikaatin tiedoston nimeksi. Valitse sitten **Seuraava**.

5. Valitse varmenteen vieminen **loppuun** .


### <a name="genclientcert"></a>Osa 2: Luo Asiakasvarmenne

Voit luoda yksilöllisiä varmenteen joko kunkin asiakkaan, joka muodostaa yhteyden tai voit käyttää samaa varmennetta useasta asiakasohjelmasta käsin. Luodaan yksilöllinen asiakkaan varmenteet etuna on mahdollisuus kumota yhtä varmennetta tarvittaessa. Muussa tapauksessa kaikki on käytössä sama Asiakasvarmenne ja huomaat, että haluat kumota yksi asiakas varmennetta, jos tarvitset Luo ja asentaa uusia sertifikaatteja kaikki ohjelmat, joita todentaa sertifikaatin avulla.

- Jos käytössäsi on enterprise-todistus-ratkaisun, Luo Asiakasvarmenne yleisiä nimi-arvo muodossa 'name@yourdomain.com', sen sijaan, että "toimialueen nimi\käyttäjänimi"-muodossa. 

- Jos käytössäsi on itse allekirjoitettua varmennetta, nähdä luomiseen Asiakasvarmenne [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md) .

### <a name="exportclientcert"></a>Osa 3: Vie Asiakasvarmenne

Asenna Asiakasvarmenne kussakin tietokoneessa, johon haluat muodostaa yhteyden virtual verkkoon. Asiakasvarmenne tarvitaan todennusta varten. Voit automatisoida asentaminen sertifikaattia tai voit asentaa sen manuaalisesti. Seuraavat vaiheet käy läpi asentaminen sertifikaattia manuaalisesti.

1. Voit viedä Asiakasvarmenne, voit käyttää *certmgr.msc*. Napsauta hiiren kakkospainikkeella Asiakasvarmenne, jonka haluat viedä, valitse **Kaikki tehtävät**ja valitse sitten **Vie**.
2. Vie asiakkaan varmenne ja yksityinen avain. Tämä on *.pfx* -tiedosto. Varmista, että tallentaa tai muista salasana (avain), jonka olet määrittänyt tämän todistuksen.

## <a name="upload"></a>Osa 3 – lataa pääkansion varmenteen .cer-tiedosto

Kun yhdyskäytävä on luotu, voit ladata Azure luotetun varmenteiden varmenne .cer-tiedosto. Voit ladata tiedostoja enintään 20 varmenteet. Ei ladata Azure varmenteen päämyöntäjien yksityinen avain. Kun .cer-tiedosto on ladattu palvelimeen, Azure käyttää sitä todennetaan asiakkaat, jotka virtual verkkoon.

1. Valitse sivu, että VNet **VPN-yhteydet** -osassa **piste-,-sivuston avaaminen VPN-yhteyden **asiakkaiden** kuva** sivu.

    ![Asiakkaat] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Asiakkaat")

2. **Pisteen sivuston yhteyttä** sivu, valitse **Hallitse varmenteet** Avaa **Varmenteet** -sivu.<br>

    ![Varmenteet-sivu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Valitse **Varmenteet** -sivu **Lataa** Avaa **Lataa varmenne** -sivu.<br>

    ![Lataa Varmenteet-sivu](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Valitse kansio-kuvan .cer-tiedosto selaamalla. Valitse tiedosto ja valitse sitten **OK**. Päivitä sivu, voit tarkastella ladattuja varmenteen **Varmenteet** -sivu.

    ![Lataa sertifikaatti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Osa 4 - Luo VPN asiakasohjelman määritysten-paketti

Muodosta yhteys virtual verkkoon, edellyttää VPN-asiakasohjelman määrittäminen. Asiakastietokone edellyttää Asiakasvarmenne ja ERISNIMI VPN asiakasohjelman määritysten paketin yhdistäminen.

VPN-asiakasohjelman paketti sisältää kokoonpanotietoja määrittäminen Windowsin VPN-Asiakasohjelmistoa. Paketin ei asennu lisäohjelmistoa. Asetukset ovat virtual verkkoon, johon haluat muodostaa yhteyden. Asiakas-palveluissa tuetut käyttöjärjestelmät luetteloon, tutustu [pisteen sivuston yhteydet](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN yhdyskäytävän usein kysytyt kysymykset-osa. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>Luo VPN asiakasohjelman määritysten-paketti

1. Azure-portaalissa **Yhteenveto** -sivu, että VNet **VPN-yhteydet**, valitse Avaa **piste-,-sivuston VPN-yhteyden asiakkaan grafiikka** sivu.
2. Yläreunassa, **piste-,-sivuston VPN yhteyden** sivu, valitse paketti, joka vastaa asiakas-käyttöjärjestelmä, johon se asennetaan:

 - 64-bittinen asiakkaat Valitse **VPN-asiakasohjelman (64-bittinen)**.
 - 32-bittinen asiakkaat Valitse **VPN-asiakasohjelman (32-bittinen)**.

    ![Lataa VPN asiakasohjelman määritysten paketti](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Näyttöön tulee sanoma, Azure aiheuttamaa VPN asiakasohjelman määritysten pakkaaminen virtual verkon. Muutaman minuutin kuluttua paketin luodaan ja näet viestin, että paketti on ladattu paikallisessa tietokoneessa. Tallenna paketti kokoonpanotiedosto. Voit asentaa tämä asiakastietokoneessa, joka muodostaa yhteyden P2S virtual verkkoon.


## <a name="clientconfiguration"></a>Vaihe 5 – asiakastietokone

### <a name="part-1-install-the-client-certificate"></a>Osa 1: Asenna asiakkaan varmenne

Kunkin asiakaskoneen täytyy olla Asiakasvarmenne, jotta todennusta. Kun asennat sertifikaattia, sinun on salasanaa, jolla on luotu sertifikaattia vietäessä.

1. Kopioi asiakastietokone .pfx-tiedosto.
2. Kaksoisnapsauta asennusohjeet .pfx-tiedosto. Älä muokkaa asennuksen sijainti.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Osa 2: Asenna VPN asiakasohjelman määritysten-paketti

Voit käyttää VPN asiakasohjelman määritysten paketissa kunkin asiakaskoneen, jos versio vastaa arkkitehtuuri asiakkaalle.

1. Kopioi määritystiedosto paikallisesti tietokoneeseen, johon haluat muodostaa yhteyden virtual verkon ja kaksoisnapsauta .exe-tiedosto. 

2. Kun paketti on asennettu, voit aloittaa VPN-yhteyttä. Microsoft ei allekirjoittanut määritys-paketti. Voit haluat kirjautua käyttämällä organisaation allekirjoitetun palvelua paketti tai Kirjaudu [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)itse. Voidaanko pakkaaminen kirjautumatta. Kuitenkin paketin ei ole kirjautunut, näyttöön tulee varoitus paketin asennuksen yhteydessä.

3. Asiakastietokoneen Siirry **Verkkoasetukset** ja valitse **VPN-yhteyttä**. Näet luettelossa yhteys. Virtuaalinen verkon nimi näkyy muodostaa yhteyden, ja näyttää seuraavanlaiselta: 

    ![VPN-asiakas] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN-asiakas")

## <a name="connect"></a>Osa 6 - Azure yhdistäminen

### <a name="connect-to-your-vnet"></a>Muodostaa yhteyttä VNet

1. Muodostaa yhteyttä VNet asiakastietokoneen, siirry VPN-yhteydet ja Etsi VPN-yhteyden, jonka loit. Sen nimi on sama nimi kuin virtual verkossa. Valitse **Muodosta yhteys**. Ponnahdusikkunoiden viesti saattaa tulla näkyviin, joka viittaa sertifikaatin avulla. Jos näin käy, valitse **Jatka** käyttämään laajentamiseen. 

2. **Yhteyden** tila-sivulla valitsemalla **Muodosta** yhteys käynnistämiseen. Jos näkyviin **Valitse varmenne** -viesti, varmista, että Asiakasvarmenne, jossa on yksi, johon haluat muodostaa. Jos se ei ole, käyttää oikea sertifikaatti avattavan luettelon nuolta ja valitse sitten **OK**.

    ![Yhdistäminen] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "Asiakkaan VPN-yhteyden")

3. Yhteys nyt vahvistettava.

    ![Yhteys muodostettu yhteys] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Yhteyttä")

### <a name="verify-the-vpn-connection"></a>Tarkista VPN-yhteys

1. Varmista, että VPN-yhteys on aktiivinen, Avaa järjestelmänvalvojan oikeuksin suoritettava komentokehote ja suorita *ipconfig/kaikki*.
2. Tarkastele tuloksia. Huomaa, että olet saanut IP-osoite on yksi pisteen sivuston connectivity osoitealueita, että olet määrittänyt oman VNet luotaessa osoitteissa. Tuloksena saadaan jotakin tämän tapaista:

Esimerkki:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisätä näennäiskoneiden virtual verkkoon. Katso, [miten voit luoda mukautetun virtual machine](../virtual-machines/virtual-machines-windows-classic-createportal.md).