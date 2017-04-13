<properties 
   pageTitle="Määritä VPN-yhdyskäytävän Azure perinteinen portaalissa | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi virtual verkon VPN-yhdyskäytävän määrittäminen ja muuttamalla yhdyskäytävän VPN reitityksen tyyppi."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>VPN-yhdyskäytävän perinteinen käyttöönoton mallin määrittäminen


Jos haluat luoda suojatun rajat paikallisen yhteyden Azure ja paikallisen sijainnin välillä, tarvitset yhdyskäytävän VPN-yhteyden määrittäminen. Perinteinen käyttöönotto-mallissa yhdyskäytävän voi olla jokin VPN reititys kahdenlaisia: staattinen tai dynaaminen. Voit valita tyyppi vaihtelee sen mukaan, sekä palvelupakettisi verkon suunnittelu ja paikallisen VPN-laite, jota haluat käyttää. 

Esimerkiksi joidenkin connectivity asetukset, kuten kohdassa sivusto-yhteyden edellyttävät dynaaminen reititys yhdyskäytävän. Jos haluat määrittää oman yhdyskäytävän tukea pisteen sivuston (P2S) yhteydet ja sivusto sivusto (S2S)-yhteyden, on määrittäminen dynaaminen reititys yhdyskäytävän, vaikka sivusto sivusto voidaan määrittää joko yhdyskäytävän VPN reitityksen tyyppi. 

Lisäksi Varmista, että haluat muodostaa yhteyden laite tukee VPN reitityksen tyyppi, jonka haluat luoda. Lisätietoja on artikkelissa [VPN-laitteiden tietoja](vpn-gateway-about-vpn-devices.md).


**Tässä artikkelissa tietoja** 

Tässä artikkelissa on kirjoitettu perinteinen käyttöönoton mallin [perinteinen portal](https://manage.windowsazure.com) (ei Azure portaalin). 

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Yleistä

Seuraavat vaiheet opastusta määrittäminen VPN-yhdyskäytävän Azure perinteinen-portaalissa. Nämä vaiheet koskevat vain yhdyskäytävien virtual verkkojen, joka on luotu käyttämällä perinteinen käyttöönottomalli. Kaikki yhdyskäytävien asetukset eivät tällä hetkellä käytettävissä Azure-portaalissa. Kun ne ovat, emme Luo uusi joukko ohjeita, jotka koskevat Azure-portaaliin.


1. [Luo VPN-yhdyskäytävän oman VNet](#create-a-vpn-gateway)

1. [Kerää tiedot, VPN-laitteen määrittäminen](#gather-information-for-your-vpn-device-configuration)

1. [Määritä VPN-laite](#configure-your-vpn-device)

1. [Tarkista paikalliseen verkkoon kirjauduttaessa alueet ja VPN yhdyskäytävän IP-osoite](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Ennen aloittamista

Ennen kuin määrität oman yhdyskäytävän, sinun on luoda virtuaalisia verkossa. Voit luoda paikallisen-yhteyksien virtual verkon ohjeita on kohdassa [virtual verkon sivusto sivusto VPN-yhteyden määrittäminen](vpn-gateway-site-to-site-create.md)tai [virtual verkon pisteen sivuston VPN-yhteyden määrittäminen](vpn-gateway-point-to-site-create.md). Tee seuraavat toimet VPN-yhdyskäytävän ja kerätä tietoja, sinun on määritettävä VPN-laite. 

Jos sinulla on jo VPN-yhdyskäytävän ja haluat muuttaa reititys VPN-tyyppi, katso, [miten voit muuttaa käyttämäsi yhdyskäytävän VPN reititys tyypin](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>VPN Gatewayn luominen

1. [Azure perinteinen portal](https://manage.windowsazure.com)- **verkoissa** sivulla Varmista, että virtual verkon tila-sarakkeessa on **luotu**.

1. Valitse virtual verkon nimi **nimi** -sarakkeessa.

1. **Dashboard** -sivun Huomaa, että tämä VNet ei ole vielä määritetty yhdyskäytävä. Näet tämän tilan kuitenkin määrittäminen käyttämäsi yhdyskäytävän vaiheet.

![Yhdyskäytävää ei luotu](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Valitse seuraavaksi sivun alaosassa **Luo yhdyskäytävä**. Voit valita *Reititys staattinen* tai *Dynaaminen reititys*. Voit valita VPN reititys tyyppi vaihtelee muutamia tekijät. Esimerkiksi VPN-laitteesi tukee ja Tarvitsetko tukevat pisteen sivuston yhteydet. Tarkista Vahvista VPN reitityksen tyyppi, sinun on [Tietoja VPN laitteiden Virtual verkkoyhteydessä](vpn-gateway-about-vpn-devices.md) . Kun yhdyskäytävä on luotu, et voi muuttaa yhdyskäytävän VPN reititys eri ilman poistaminen ja luominen uudelleen yhdyskäytävän välillä. Kun järjestelmä pyytää sinua vahvistamaan, että haluat luoda yhdyskäytävän, valitse **Kyllä**.

![Yhdyskäytävän VPN reititys-tyyppi](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Käyttämäsi yhdyskäytävän luotaessa Huomaa yhdyskäytävän grafiikan sivulla keltainen muuttuu, ja teksti *Luominen yhdyskäytävä*. Voi kestää jopa 45 minuuttia yhdyskäytävän luominen. Odota, kunnes yhdyskäytävä on valmis, ennen kuin voit siirtää eteenpäin ja muut asetukset.

![Yhdyskäytävän luominen](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Kun yhdyskäytävän muuttuu *yhteyden*, voit kerätä tietoja, sinun on VPN-laitteeseesi.

![Yhdyskäytävän yhdistäminen](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Kerää tiedot, VPN-laitteen määrittäminen

Yhdyskäytävän luonnin jälkeen kerää tietoja VPN-laitteen määrittäminen. Nämä tiedot sijaitsee virtual verkon **Dashboard** -sivulla:

1. **Yhdyskäytävän IP-osoite-** IP-osoite löytyy **Dashboard** -sivu. Et voi nähdä tiedot vasta sen jälkeen, kun käyttämäsi yhdyskäytävän luominen on valmis.

1. **Jaettu avain-** Valitse näytön alareunassa **Hallinta-näppäintä** . Valitse salaisuus Leikepöydän, kopioiminen ja liittäminen ja tallenna se vieressä olevaa kuvaketta. Tämä painike toimii vain, kun kyseessä on yksittäinen S2S VPN-tunnelin. Jos sinulla on useita S2S VPN-tunneleita, käytä *Hae Virtual verkon yhdyskäytävän jaettu avaimen* API tai PowerShell cmdlet-komennon.

![Hallitse avain](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Määritä VPN-laite

Kun olet suorittanut edellä kuvatut vaiheet, tai verkon järjestelmänvalvoja on määrittäminen VPN-laite, jotta he voivat luoda yhteyden. Voit tarkastella [Tietoja VPN laitteita Virtual verkkoyhteydessä](vpn-gateway-about-vpn-devices.md) Lisätietoja VPN-laitteet.

Kun VPN-laite on määritetty, voit tarkastella päivitetyt yhteystiedot Dashboard-sivulla, että VNet.

Voit myös käyttää jotakin seuraavista komennoista Testaa yhteys:

|                      | Cisco ASA             | Cisco ISR/ASR         | Juniper SSG/ISG | Juniper SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Tarkista tärkeimmät suojaussidosten**  | Näytä crypto isakmp sa | Näytä crypto isakmp sa | Hanki ike eväste  | Näytä suojaus ike suojaus-liitos   |
| **Tarkista pikatilan suojaussidosten** | Näytä crypto IP sa  | Näytä crypto IP sa  | Hae sa          | Näytä IP suojaus-liitos |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Tarkista paikalliseen verkkoon kirjauduttaessa alueet ja VPN yhdyskäytävän IP-osoite

### <a name="verify-your-vpn-gateway-ip-address"></a>Tarkista VPN yhdyskäytävän IP-osoite

Gateway voi muodostaa yhteyttä VPN-laitteen IP-osoite on oikein määritetty lähiverkossa, jonka olet määrittänyt paikallisen-kokoonpanon. Tavallisesti tämä on määritetty sivuston sivuston määritysten aikana. Jos olet aiemmin käyttänyt tätä paikalliseen verkkoon kirjauduttaessa jonkin toisen laitteen kanssa tai IP-osoite on muutettu tähän paikalliseen verkkoon, kuitenkin Muokkaa asetuksia Määritä oikea yhdyskäytävän IP-osoite.

1. Tarkista yhdyskäytävän IP-osoite, valitse **verkkojen** portaalin vasemmanpuoleisessa ruudussa ja valitse **Paikallinen verkkojen** sivun yläreunassa. Näet kunkin lähiverkossa, jotka olet luonut VPN-yhdyskäytäväosoite. Jos haluat muokata IP-osoite, valitse VNet ja valitse sivun alareunassa **Muokkaa** .

1. Valitse **Määritä lähiverkossa-tiedot** -sivulla Muokkaa IP-osoite ja valitse sitten sivun alareunassa seuraava-nuoli.

1. **Määritä osoitetila** -sivulla Napsauta oikeassa alakulmassa Tallenna asetukset valintamerkkiä.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Tarkista paikallisen lopettaminen-osoitealueet

Paikallisen sijaintiin yhdyskäytävän kautta kulkevan oikea tietoliikenteen haluat varmistaa, että kukin IP-osoitealueita on määritetty. Kunkin alueen on oltava lueteltuna Azure **Paikallisen verkot** -määritys. Paikallisen sijaintisi verkon määritysten mukaan voi olla hieman suuri tehtävän. Liikenteestä, joka on sidottu IP-osoite, joka on sisällytetty luettelossa alueiden lähetetään VPN VPN-yhdyskäytävän kautta. Alueet luettelossa ei tarvitse olla yksityinen alueita, vaikka sinun kannattaa tarkistaa paikallisen määritys voi vastaanottaa saapuvan liikenteen.

Voit lisätä tai muokata alueita lähiverkossa, seuraavien ohjeiden mukaisesti.

1. Muokattava lähiverkossa IP-osoitealueet portaalin vasemmanpuoleisessa ruudussa **verkot** valitsemalla ja valitse sitten **Paikallisen verkkojen** sivun yläreunassa. Valitse-portaalissa helpoin tapa tarkastella alueet, jotka on lueteltu on **Muokkaa** sivua. Jos haluat nähdä oman alueita, valitse VNet ja valitse sivun alareunassa **Muokkaa** .

1. Valitse **Määritä lähiverkossa-tiedot** -sivulla ei tehdä muutoksia. Valitse sivun alareunassa seuraava-nuoli.

1. **Määritä osoitetila** -sivulla Tee muutokset tilaa verkon osoite. Valitse Tallenna kokoonpanosi valintamerkki.

## <a name="how-to-view-gateway-traffic"></a>Yhdyskäytävän liikenne tarkasteleminen

Voit tarkastella yhdyskäytävän ja yhdyskäytävän liikenteen VPN **Dashboard** -sivulla.

**Dashboard** -sivulla voit tarkastella seuraavasti:

- Yhdyskäytävän-tiedot ja tietojen kautta juoksutus tietojen määrään.

- Jotka on määritetty virtual verkon DNS-palvelimien nimet.

- Käyttämäsi yhdyskäytävän ja VPN-laitteen välinen yhteys.

- Jaetun avaimen, jota käytetään, voit määrittää yhdyskäytävän yhteyden VPN-laitteeseen.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Voit muuttaa oman Gatewayn VPN reititys-tyyppi

Sillä jotkin connectivity määritykset ovat käytettävissä vain yhdyskäytävän reititys tietyntyyppisten, saatat huomata, että sinun on muutettava yhdyskäytävän VPN aiemmin VPN-yhdyskäytävän reititys tyyppi. Jos esimerkiksi haluat pisteen sivuston yhteyksien lisääminen jo olemassa oleva yhteys sivusto sivusto, jossa on staattinen yhdyskäytävän. Pisteen sivuston yhteydet edellyttävät dynaaminen yhdyskäytävä. Tämä tarkoittaa P2S-yhteyden määrittäminen, sinun on muutettava käyttämäsi yhdyskäytävän VPN reitityksen tyyppi-staattinen dynaamiseksi.

Jos haluat muuttaa yhdyskäytävän VPN reitityksen tyyppi aiemmin yhdyskäytävän poisto ja luo sitten uusi reititys sisältötyyppi uusi yhdyskäytävä. Ei tarvitse poistaa yhdyskäytävän reititys tyypin muuttaminen koko virtual verkossa.

Ennen kuin muutat käyttämäsi yhdyskäytävän VPN reitityksen tyyppi, muista Varmista, että VPN-laitteesi tukee reititys tyyppi, jota haluat käyttää. Lataa uusi reititys määritys-mallit ja VPN-laitteen tarkistaminen on ohjeaiheessa [Tietoja VPN laitteiden Virtual verkkoyhteydessä](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Kun poistat VPN VPN-yhdyskäytävän, VIP liitetty yhdyskäytävä on julkaistu. Voit luoda yhdyskäytävän uudelleen, kun uusi VIP määritetään sen.

1. **Poista aiemmin VPN-yhdyskäytävä.**

    Virtual verkon **Dashboard** -sivulla Siirry sivun alareunaan ja valitse **Poista yhdyskäytävä**. Antamalla ilmoituksen, että yhdyskäytävä on poistettu. Kun saat ilmoituksen näytössä, että yhdyskäytävä on poistettu, voit luoda uuden yhdyskäytävän.

1. **Luo uusi VPN-yhdyskäytävä.**

    Luo uusi yhdyskäytävä sivun yläosassa toimien avulla: [VPN Gatewayn luominen](#create-a-vpn-gateway).


## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisätä näennäiskoneiden virtual verkkoon. Katso, [miten voit luoda mukautetun virtual machine](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Jos haluat määrittää pisteen sivuston VPN-yhteyden, artikkelissa [määrittäminen pisteen sivuston VPN-yhteyden](vpn-gateway-point-to-site-create.md).

 
