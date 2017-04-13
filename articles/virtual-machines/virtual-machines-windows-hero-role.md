<properties
    pageTitle="IIS: N asentaminen ensimmäisen Windows-AM | Microsoft Azure"
    description="Voit kokeilla ensimmäisen Windows virtuaalikoneen asentamalla IIS- ja avataan portti 80 Azure-portaalissa."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Voit kokeilla roolin asentamisesta Windows-AM
    
Kun ensimmäisen virtuaalikoneen (AM) tekemiseksi, voit siirtää asentaa ohjelmistoja ja palveluja. Tässä opetusohjelmassa, tarkastellaan palvelimen hallinnan käyttämisestä Windows Server AM IIS: N asentaminen. Tämän jälkeen luodaan verkon suojauksen ryhmän (NSG) Avaa portti 80 IIS-liikennettä Azure portaalin avulla. 

Jos et ole vielä luonut ensimmäisen AM, siirry takaisin [Luo ensimmäinen Windows virtuaalikoneen Azure-portaalissa](virtual-machines-windows-hero-tutorial.md) ennen jatkamista Tässä opetusohjelmassa.

## <a name="make-sure-the-vm-is-running"></a>Varmista, AM on käynnissä

1. Avaa [Azure portal](https://portal.azure.com).
2. Valitse toiminto-valikosta **näennäiskoneiden**. Valitse virtuaalikoneen luettelosta.
3. Jos tila on **pysäytetty (Deallocated)**, valitse **Käynnistä** -painiketta AM **Essentials** -sivu. Jos tila on **käytössä**, voit siirtää seuraavaan vaiheeseen.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Yhteyden muodostaminen virtuaalikoneen ja sisäänkirjautuminen

1.  Valitse toiminto-valikosta **näennäiskoneiden**. Valitse virtuaalikoneen luettelosta.

3. Valitse **Yhdistä**varten virtuaalikoneen sivu. Tämä luo ja lataa Remote Desktop Protocol-tiedosto (.rdp-tiedosto), joka muistuttaa pikakuvakkeen muodostaa yhteyttä tietokoneeseen. Haluat ehkä tallentaa tiedoston työpöydälle käytön helpottamiseksi. **Avaa** tiedosto muodostaa yhteyttä AM.

    ![Näyttökuva esittää, kuinka voit muodostaa yhteyden oman AM Azure portal](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Näyttöön tulee varoitus, joka .rdp on tuntematon Publisherista. Tämä on Normaali. Etätyöpöytä-ikkunassa Valitse Jatka **Yhdistä** .

    ![Näyttökuva Tuntematon julkaisija koskeva varoitus](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Kirjoita käyttäjänimi ja salasana, jonka loit, kun loit AM paikallisen tilin Windowsin suojaus-ikkunaan. Käyttäjänimi kirjaus *vmname*& #92; *käyttäjänimi*, valitse **OK**.

    ![Näyttökuva AM nimi, käyttäjänimi ja salasana](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Näyttöön tulee varoitus, varmennetta ei voi vahvistaa. Tämä on Normaali. Valitse **Kyllä** ja Vahvista jäsenyys virtuaalikoneen kirjautumalla sisään.

    ![Näyttökuva viestin lisätietoja AM tunnistetiedot](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Jos suoritat ongelmia, kun muodostat yhteyden, katso [vianmääritys etätyöpöydän yhteydet, Windows-pohjaisesta Azure Virtual koneen](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>IIS: N asentaminen oman AM

Nyt kun olet kirjautunut AM, emme asentaa palvelimen rooli niin, että voit kokeilla Lisää.

1. Avaa **Serverin hallinta** , jos se ei ole vielä avattu. Napsauta **Käynnistä** -painiketta ja valitse sitten **Palvelimen hallinta**.
2. **Palvelimen hallinta**ja valitse vasemmanpuoleisessa ruudussa **Paikalliseen palvelimeen** . 
3. Valitse-valikosta **Hallitse** > **Lisää rooleja ja toiminnot**.
4. Lisää rooleja ja valitse **Asennustyyppi** -sivulla ohjatun ominaisuuksien **Roolipohjainen tai ominaisuuksiin perustuvaan**asennus ja valitse sitten **Seuraava**.

    ![Näyttökuva Lisää rooleja ja ohjatun ominaisuuksien välilehti asennustyyppi varten](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Valitse AM palvelimen käytetään varannon ja valitse **Seuraava**.
6. Valitse **Roolit** -sivulla **Web Server (IIS)**.

    ![Näyttökuva Lisää rooleja ja ohjatun ominaisuuksien-välilehti, palvelinrooleista](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Valita ponnahdusikkunassa lisätään IIS tarvittavat ominaisuudet Varmista, että **Sisällytä hallintatyökalut** on valittuna ja valitse sitten **Lisää ominaisuuksia**. Ponnahdusvalikossa sulkeutuessa, valitse ohjatussa **Seuraava** -painiketta.

    ![Näyttökuva ponnahdusikkunoiden Vahvista IIS-roolin lisääminen](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Valitse ominaisuudet-sivulla **Seuraava**.
9. Valitse **Web Server rooli (IIS)** -sivulla **Seuraava**. 
10. Valitse **Roolipalvelut** -sivulla **Seuraava**. 
11. Valitse **Vahvista** -sivulla **Asenna**. 
12. Kun asennus on valmis, valitse **Sulje** ohjattu toiminto.



## <a name="open-port-80"></a>Avaa portti 80 

Jotta oman AM Hyväksy saapuva liikenne portin 80 kautta sinun on saapuvan liikenteen säännön lisääminen verkko-käyttöoikeusryhmän. 

1. Avaa [Azure portal](https://portal.azure.com).
2. Valitse **näennäiskoneiden** AM, jonka loit.
3. Näennäiskoneiden asetuksia Valitse **verkkoliittymät** ja valitse sitten luodun verkon-liittymän.

    ![Näyttökuva verkkoliittymät virtuaalikoneen-asetusta](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. Napsauta verkko-liittymän **Essentials** **verkon käyttöoikeusryhmän**.

    ![Näyttökuva, jossa verkko-liittymän Essentials-osassa](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. Varten NSG **Essentials** -sivu on oltava yksi aiemmin oletusarvon saapuvien sääntö, joka mahdollistaa AM kirjautuminen **Oletusarvo-Salli rdp** . Lisää toinen saapuvien sääntö, joka sallii IIS-liikenne. Valitse **Saapuvan suojauksen sääntö**.

    ![Näyttökuva kohdasta tutustuminen NSG varten](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. **Saapuvan liikenteen säännöt**Valitse **Lisää**.

    ![Näyttökuva, jossa valitsemalla Lisää suojaussääntö](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. **Saapuvan liikenteen säännöt**Valitse **Lisää**. Kirjoita **80** Port (portti)-alueen ja varmista, että **Salli** on valittuna. Kun olet valmis, valitse **OK**.

    ![Näyttökuva, jossa valitsemalla Lisää suojaussääntö](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Lisätietoja NSGs, saapuvan ja lähtevän liikenteen säännöt-kohdassa [Salli ulkoinen käyttö oman AM Azure-portaalissa](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>IIS-oletussivustoon yhdistäminen

1. Azure-portaalissa Valitse **näennäiskoneiden** ja valitse sitten oman AM.
2. Kopioi **julkiseen IP-osoite** **Essentials** -sivu.

    ![Näyttökuva, jossa voit etsiä oman AM julkiseen IP-osoite](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Avaa selain ja kirjoita osoiteriville julkiseen IP-osoitteelle tältä: http://<publicIPaddress> ja valitse **Enter** , siirry osoitteeseen.
3. Selaimen kannattaa avata oletusarvon IIS-web-sivu. Se näyttää tältä:

    ![Näyttökuva osoittaa, mitä oletusarvon IIS-sivu näkyy selaimessa](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Seuraavat vaiheet

- Voit myös kokeilla ja [liität tiedot DVD-levyllä](virtual-machines-windows-attach-disk-portal.md) virtuaalikoneen. Tietoja levyjen antaa virtuaalikoneen lisätallennustilaa.
