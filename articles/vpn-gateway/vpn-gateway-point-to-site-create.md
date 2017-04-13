<properties
   pageTitle="Perinteinen portaalissa Azure Virtual verkkoon pisteen sivuston VPN yhdyskäytävä-yhteyden määrittäminen | Microsoft Azure"
   description="Suojattu yhteys Azure Virtual verkon luomalla pisteen sivuston VPN-yhdyskäytävän yhteys."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>VNet, perinteinen portaalissa pisteen sivuston yhteyden asetusten määrittäminen

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Perinteinen - Azure Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Perinteinen - perinteinen Portal](vpn-gateway-point-to-site-create.md)

Pisteen sivuston (P2S) määrityksen voit luoda suojatun yhteyden yksittäisiä asiakastietokoneesta virtual verkkoon. P2S yhteyden on hyödyllinen, kun haluat muodostaa yhteyttä VNet remote sijainnista, kuten kotona tai konferenssiin, tai jos sinulla on vain muutama asiakkaat, jotka on virtual verkkoon.

Tässä artikkelissa käydään läpi luomalla VNet **perinteinen käyttöönoton mallin** **perinteinen portal**-kohdassa sivuston yhteyden.

Pisteen sivuston yhteydet, eivät edellytä VPN-laitetta tai julkiselle IP-osoite, jos haluat tutustua. VPN-yhteys on muodostettu yhteys käynnistämällä asiakastietokoneesta. Saat lisätietoja pisteen sivuston yhteydet [VPN yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [suunnittelu- ja rakenne](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien P2S yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Seuraavassa taulukossa on kaksi käyttöönoton mallia ja käytettävissä käyttöönottomenetelmät P2S määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Tavallinen työnkulku

![Piste-,-sivusto-kaavion] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "pisteen sivuston")
 
Seuraavat vaiheet käy läpi vaiheet, Luo pisteen sivuston suojatun yhteyden virtual verkkoon. 

Pisteen sivuston yhteyden määritys on eritellä neljä osaa. Siinä järjestyksessä, jossa määrität seuraavissa osissa on tärkeää. Älä ohita vaiheet tai siirtyä eteenpäin.

- **Osa 1** Luo virtuaalisia verkko- ja VPN-yhdyskäytävän.
- **Osa 2** Luo varmenteiden todentamiseen käytetty ja latautumista varten.
- **Osa 3** Voit viedä ja asenna asiakkaan varmenteet.
- **Osa 4** Määritä VPN-asiakasohjelman.

## <a name="vnetvpn"></a>Osa 1 - virtual verkko- ja VPN-yhdyskäytävän luominen

### <a name="part-1-create-a-virtual-network"></a>Osa 1: Luo virtuaalisia verkko

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/). Näiden ohjeiden avulla perinteinen Azure portal-portaalissa Tällä hetkellä ei voi luoda P2S yhteyden Azure-portaalissa.

2. Valitse näytön vasemmassa alakulmassa **Uusi**. Valitse siirtymisruudussa **Verkkopalveluita**ja valitse sitten **VPN**. Valitse Aloita ohjattu määritystoiminto **Mukautettu luominen** .

3. Valitse **Virtual verkon tiedot** -sivulla seuraavat tiedot ja valitse sitten oikeassa alakulmassa seuraava-nuoli.
    - **Nimi**: virtual verkon nimi. Esimerkiksi "VNet1". Tämä on nimi, joka viittaa asentaessasi VMs tämän VNet.
    - **Sijainti**: sijainti suoraan liittyvät resurssit (VMs) sijaitsevan haluamaasi fyysinen sijainti (alue). Esimerkiksi jos haluat VMs, sijoitettava fyysisesti Yhdysvaltojen Itä virtual verkoston käyttöön, valitse kyseiseen sijaintiin. Et voi muuttaa alueen liittyvät virtual verkon sen luomisen jälkeen.

4. Valitse **DNS-palvelimet ja VPN-yhteys** -sivulla seuraavat tiedot ja valitse sitten oikeassa alakulmassa seuraava-nuoli.
    - **DNS-palvelimet**: DNS-palvelimen nimi ja IP-osoite tai valitse aiemmin rekisteröidyn DNS-palvelin pikavalikosta. Tämä asetus luo DNS-palvelimeen. Sen avulla voit määrittää DNS-palvelimet, jota haluat käyttää virtual verkoston nimenselvitys. Jos haluat käyttää Azure oletusarvon nimi tarkkuus-palvelua, jätä tämä kohta tyhjäksi.
    - **Määritä kohdan sivuston VPN**: Valitse valintaruutu.

5. Määritä IP-osoitealueita, josta VPN-asiakkaat saavat IP-osoite, kun yhteys on luotu **Pisteen sivuston Connectivity** -sivulla. On muutama säännöt, jotka voit määrittää osoitealueet. On tärkeää varmistaa, voit määrittää alueen peittää paikallisen verkossa sijaitsevat alueiden avulla.

6. Seuraavat tiedot ja valitse sitten Seuraava-nuoli.
 - **Osoitetilaa varten**: Sisällytä aloitus IP-osoite ja CIDR (osoite määrä).
 - **Lisää osoitetilaa**: Lisää osoitetilaa vain, jos sitä tarvitaan verkon suunnittelu.

7. Määritä, jota haluat käyttää virtual verkon osoitealueita **Välilyöntejä Virtual verkko-osoite** -sivulla. Nämä ovat dynaamisia IP-osoitteet (tapahtuneen), joka määritetään VMs ja muut rooli esiintymät, jotka virtual verkoston käyttöön.<br><br>On tärkeää erityisesti alue, joka ei ole päällekkäinen avulla alueet, joita käytetään paikallista verkkoa varten. Verkonvalvoja, kuka on ehkä carve ulos alueen IP-osoitteiden paikallisen verkon osoite tilasta voit käyttää virtual verkkoa varten on sointuvat.

8. Seuraavat tiedot ja valitse sitten Aloita virtual verkoston luominen valintamerkki.
 - **Osoitetilan**: Lisää sisäinen IP-osoitealueita, jota haluat käyttää virtual verkon, mukaan lukien käynnistäminen IP- ja määrä. On tärkeää avulla haluamasi alueet, joita käytetään paikallista verkkoa varten, joka ei ole päällekkäinen alueen valitseminen. 
 - **Lisää aliverkon**: Lisää aliverkosta eivät ole pakollisia, mutta voit halutessasi luoda erilliset aliverkon VMs, joka on staattinen tapahtuneen. Tai voit halutessasi oman VMs aliverkon, joka on erillään muiden roolin esiintymien.
 - **Lisää yhdyskäytävän aliverkon**: yhdyskäytävän aliverkon tarvitaan pisteen sivuston VPN-yhteyttä. Lisää yhdyskäytävän aliverkon napsauttamalla tätä. Yhdyskäytävän aliverkon käytetään vain VPN-yhdyskäytävä.

9. Kun virtual verkossa on luotu, näyttöön tulee **luotu** kohdasta **tila** verkot-sivulla Azure perinteinen portaalissa. Kun virtual verkossa on luotu, voit luoda oman dynaaminen reititys yhdyskäytävä.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Osa 2: Dynaaminen reititys Gatewayn luominen

Yhdyskäytävän tyyppi on oltava määritettynä yhtä dynaaminen. Staattinen reititys yhdyskäytävät eivät toimi tätä ominaisuutta.

1. Azure perinteinen portaalissa **verkkojen** sivulla Valitse virtual verkko, jotka olet luonut ja **Dashboard** -sivulle.

2. Valitse **Luo yhdyskäytävä**osoitteessa **Dashboard** -sivulla. Näyttöön tulee kysymys, **Haluatko luoda yhdyskäytävän virtual verkon "VNet1"**. Valitse **Kyllä** Aloita yhdyskäytävän luominen. Voi kestää noin 15 minuuttia, voit luoda yhdyskäytävän.

## <a name="generate"></a>Osa 2 – Luo ja lataa varmenteet

Varmenteita käytetään tarkistamiseen pisteen sivuston VPN-yhteydet VPN-asiakkaille. Voit käyttää enterprise-todistus-ratkaisun luoma pääkansion varmenteen tai voit käyttää itse allekirjoitettua varmennetta. Voit ladata enintään 20 varmenteet Azure. Kun .cer-tiedosto on ladattu palvelimeen, Azure voi todentaa asiakkaat, joilla asennettu Asiakasvarmenne sen sisältämiä tietoja avulla. Asiakkaan varmenne on voidaan luoda samaa varmennetta, joka edustaa .cer-tiedosto.

Tässä osassa toimi seuraavasti:

- Hanki varmenteen päämyöntäjien .cer-tiedosto. Tämä voi olla itse allekirjoitetun varmenteen tai voit käyttää yrityksen varmenteen järjestelmän.
- .Cer-tiedoston lataaminen Azure.
- Luo asiakkaan varmenteet.

### <a name="root"></a>Osa 1: Hanki varmenteen päämyöntäjien .cer-tiedosto

Jos käytössäsi on enterprise-sertifikaatin järjestelmän, hanki pääkansio-varmenne, jota haluat käyttää .cer-tiedoston. [Osa 3](#createclientcert)Luo asiakkaan varmenteet varmenteen päämyöntäjien.

Jos et käytä enterprise-todistus-ratkaisun, tarvitset pääkansion itse allekirjoitetun sertifikaatin luomiseen. Windows 10-ohjeita on voidaan viitata [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md). Artikkelissa käydään läpi makecert avulla voit luoda itse allekirjoitetun varmenteen ja vie tiedot sitten .cer-tiedosto.

### <a name="upload"></a>Osa 2: Lataa Azure perinteinen portaaliin pääkansion varmenteen .cer-tiedosto

Luotettu varmenteen lisääminen Azure. Kun lisäät Base64-koodattu X.509 (.cer) tiedoston Azure-ovat kehottaa Azure luotettava ylimmällä tasolla, joka edustaa tiedoston.

1. Valitse Azure perinteinen portal virtual verkon **Varmenteet** -sivulla **Lataa varmenteen päämyöntäjien**.

2. **Lataa sertifikaatti** -sivulla Selaa varmenne .cer-pääkansio ja valitse sitten valintamerkki.

### <a name="createclientcert"></a>Osa 3: Luo Asiakasvarmenne

Luo seuraavaksi asiakkaan varmenteet. Voit luoda yksilöllisiä varmenteen joko kunkin asiakkaan, joka muodostaa yhteyden tai voit käyttää samaa varmennetta useasta asiakasohjelmasta käsin. Luodaan yksilöllinen asiakkaan varmenteet etuna on mahdollisuus kumota yhtä varmennetta tarvittaessa. Muussa tapauksessa kaikki on käytössä sama Asiakasvarmenne ja huomaat, että haluat kumota yksi asiakas varmennetta, jos tarvitset Luo ja asentaa uusia sertifikaatteja kaikki ohjelmat, joita käyttää varmenteen todentamiseen.

- Jos käytössäsi on enterprise-todistus-ratkaisun, Luo Asiakasvarmenne yleisiä nimi-arvo muodossa 'name@yourdomain.com', sijaan NetBIOS "Toimialue\käyttäjänimi" Muotoile. 

- Jos käytössäsi on itse allekirjoitettua varmennetta, nähdä luomiseen Asiakasvarmenne [käyttäminen itse allekirjoitettua päämyöntäjien varmenteet pisteen sivuston määrityksiä](vpn-gateway-certificates-point-to-site.md) .

## <a name="installclientcert"></a>Osa 3 - vienti ja asenna Asiakasvarmenne

Asenna Asiakasvarmenne kussakin tietokoneessa, johon haluat muodostaa yhteyden virtual verkkoon. Asiakasvarmenne tarvitaan todennusta varten. Voit automatisoida asentaminen sertifikaattia tai voit asentaa manuaalisesti. Seuraavat vaiheet käy läpi asentaminen sertifikaattia manuaalisesti.

1. Voit viedä Asiakasvarmenne, voit käyttää *certmgr.msc*. Napsauta hiiren kakkospainikkeella Asiakasvarmenne, jonka haluat viedä, valitse **Kaikki tehtävät**ja valitse sitten **Vie**.
2. Vie asiakkaan varmenne ja yksityinen avain. Tämä on *.pfx* -tiedosto. Varmista, että tallentaa tai muista salasana (avain), jonka olet määrittänyt tämän todistuksen.
3. Kopioi asiakastietokone *.pfx* -tiedosto. Asiakastietokoneen Kaksoisnapsauta asennusohjeet *.pfx* -tiedosto. Anna pyydettäessä salasana. Älä muokkaa asennuksen sijainti.

## <a name="vpnclientconfig"></a>Osa 4 - VPN-asiakasohjelman määrittäminen

Muodosta yhteys virtual verkkoon, edellyttää VPN-asiakasohjelman määrittäminen. Asiakkaan vaatii Asiakasvarmenne ja VPN asiakkaan määritykset, jotta yhteyden. Määrittäminen VPN-asiakas, tee seuraavat vaiheet järjestyksessä.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Osa 1: VPN asiakasohjelman määritysten paketin luominen

1. Siirry Azure perinteinen portal virtual verkon **Dashboard** -sivun oikeassa alakulmassa olevaa quick glance-valikosta. Asiakas-palveluissa tuetut käyttöjärjestelmät luetteloon, tutustu [pisteen sivuston yhteydet](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN yhdyskäytävän usein kysytyt kysymykset-osa. VPN-asiakasohjelman paketti sisältää kokoonpanotietoja määrittäminen Windowsin VPN-Asiakasohjelmistoa. Paketin ei asennu lisäohjelmistoa. Asetukset ovat virtual verkkoon, johon haluat muodostaa yhteyden.<br><br>Valitse Lataa-paketti, joka vastaa asiakas-käyttöjärjestelmä, johon se asennetaan:
 - 32-bittinen asiakkaat Valitse **Lataa 32-bittinen asiakkaan VPN-paketti**.
 - 64-bittinen asiakkaat Valitse **Lataa 64-bittinen asiakkaan VPN-paketti**.

2. Kestää muutaman minuutin kuluttua voit luoda asiakas-paketti. Kun pakkaaminen on valmis, voit ladata tiedoston. *.Exe* -tiedosto, jonka ladata voidaan tallentaa turvallisesti paikalliseen tietokoneeseen.

3. Kun olet Luo ja lataa VPN-asiakasohjelman paketin perinteinen Azure-portaalista, voit asentaa asiakas-paketti, joista haluat ottaa yhteyttä virtual verkkoon asiakastietokoneen. Jos haluat asentaa useita asiakastietokoneiden asiakkaan VPN-paketti, varmista, että jokainen myös asennettuna asiakkaan sertifikaatti.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Osa 2: Asenna asiakkaan VPN-verkon määritys-paketti

1. Kopioi määritystiedosto paikallisesti tietokoneeseen, johon haluat muodostaa yhteyden virtual verkon ja kaksoisnapsauta .exe-tiedosto. 

2. Kun paketti on asennettu, voit aloittaa VPN-yhteyttä. Microsoft ei allekirjoittanut määritys-paketti. Voit haluat kirjautua käyttämällä organisaation allekirjoitetun palvelua paketti tai Kirjaudu [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)itse. Voidaanko pakkaaminen kirjautumatta. Kuitenkin paketin ei ole kirjautunut, näyttöön tulee varoitus paketin asennuksen yhteydessä.

3. Asiakastietokoneen Siirry **Verkkoasetukset** ja valitse **VPN-yhteyttä**. Näet luettelossa yhteys. Se näytetään virtual verkon nimen, muodostaa yhteyden, ja näyttää seuraavanlaiselta: 

    ![VPN-asiakas] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-asiakas")


### <a name="part-3-connect-to-azure"></a>Osa 3: Azure yhdistäminen

1. Muodostaa yhteyttä VNet asiakastietokoneen, siirry VPN-yhteydet ja Etsi VPN-yhteyden, jonka loit. Sen nimi on sama nimi kuin virtual verkossa. Valitse **Muodosta yhteys**. Ponnahdusikkunoiden viesti saattaa tulla näkyviin, joka viittaa sertifikaatin avulla. Jos näin käy, valitse **Jatka** käyttämään laajentamiseen. 

2. **Yhteyden** tila-sivulla valitsemalla **Muodosta** yhteys käynnistämiseen. Jos näkyviin **Valitse varmenne** -viesti, varmista, että Asiakasvarmenne, jossa on yksi, johon haluat muodostaa. Jos se ei ole, käyttää oikea sertifikaatti avattavan luettelon nuolta ja valitse sitten **OK**.

    ![VPN-asiakasohjelman 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Asiakkaan VPN-yhteyden")

3. Yhteys nyt vahvistettava.

    ![VPN-asiakasohjelman 3] (./media/vpn-gateway-point-to-site-create/connected.png "Asiakkaan VPN-yhteyden 2")

### <a name="part-4-verify-the-vpn-connection"></a>Osa 4: Tarkista VPN-yhteys

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

Jos haluat lisätietoja Virtual verkkojen, katso [Virtual verkon asiakirjat](https://azure.microsoft.com/documentation/services/virtual-network/) -sivu.
