<properties 
   pageTitle="Tiet - portaalissa vianmääritys | Microsoft Azure"
   description="Opi suorittamaan tiet Azure Resurssienhallinta käyttöönoton mallin Azure-portaalissa."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
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
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Vianmääritys tiet Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShellin](virtual-network-routes-troubleshoot-powershell.md)

Jos kohtaat verkkoyhteyksien ongelmat ja tai -että Azure virtuaalikoneen (AM), tiet saattaa vaikuttavat AM-liikenne kulkee. Tässä artikkelissa on yleiskatsaus diagnostiikka ominaisuuksia tiet ongelmien toimenpiteitä varten.

Reitin liittyvät aliverkosta eikä ovat voimassa kaikki verkkoliittymät (NIC)-kyseisen aliverkon. Tiet seuraavanlaisia voi suojata kunkin verkkoliittymän:

- **Järjestelmän tiet:** Oletusarvoisesti jokaisella aliverkon luotu Azure Virtual verkossa (VNet) on järjestelmän reititystaulukot, joiden avulla paikallinen VNet liikenne paikallisen liikenteen VPN yhdyskäytävien kautta ja Internet-liikenne. Järjestelmän tiet on olemassa myös peered VNets.
- **Erityisen tiet:** Välitetty verkkoliittymät ExpressRoute tai sivuston sivuston VPN-yhteydet. Lisätietoja erityisen reititys lukemalla [erityisen kanssa VPN yhdyskäytävien](../vpn-gateway/vpn-gateway-bgp-overview.md) ja [ExpressRoute yleiskuvaus](../expressroute/expressroute-introduction.md) artikkelit.
- **Käyttäjän määrittämä tiet (UDR):** Jos käytössäsi on verkon virtual laitteiden tai on pakotettu tunneling liikenteen paikalliseen verkkoon sivusto sivusto VPN-verkon kautta, saatat joutua käyttäjäkohtainen tiet (UDRs) aliverkon reitti-taulukkoon liittyvät. Jos et ole aiemmin käyttänyt UDRs, tutustu artikkeliin [käyttäjän määrittämä tiet](virtual-networks-udr-overview.md#user-defined-routes) .

Eri tiet, joka voidaan lisätä verkko-käyttöliittymä, jossa voi olla vaikea määrittää, mitkä kooste tiet tulevat voimaan. Vianmääritystä AM verkkoyhteyden, voit tarkastella tehokkaita tiet verkko-liittymän Azure resurssien hallinnan käyttöönottomalli.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Vianmääritys AM liikenteen suunnan tehokkaita tiet avulla

Tässä artikkelissa käytetään esimerkkinä seuraava tilanne esitä tehokkaita tiet verkko-liittymän vianmääritys:

AM (*VM1*) yhteydessä VNet (*VNet1*, etuliite: 10.9.0.0/16) ei pysty muodostamaan yhteyttä VM(VM3)-juuri peered VNet (*VNet3*, etuliite 10.10.0.0/16). On erityisen eikä UDRs reitittää käytetty VM1 NIC1 verkkoliittymän yhteydessä AM, vain järjestelmän tiet otetaan käyttöön.

Tässä artikkelissa kerrotaan, miten voit yhteysvirhe, syy tehokkaita tiet ominaisuuksien käyttäminen Azure Resurssienhallinta käyttöönottomalli.
Kun esimerkissä käytetään vain järjestelmän tiet, samat vaiheet voidaan määrittää saapuvan ja lähtevän epäonnistuneita päälle reitityksen minkä tahansa.

>[AZURE.NOTE] Jos oman AM on liitetty useita NIC, valitse tehokkaita tiet kunkin NIC vianmääritys verkkoyhteyksien ongelmat ja sieltä AM.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Näytä tehokkaan tiet virtual tietokoneelle

Kooste tiet, joita käytetään AM näkyviin toimimalla seuraavasti:

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. Valitse **Lisää palveluja**ja valitse sitten **näennäiskoneiden** luettelossa, joka tulee näkyviin.
3. Valitse luettelosta, joka tulee vianmäärityksestä AM ja AM-sivu, jossa asetukset tulee näkyviin.
4. Valitse **diagnosointi & ongelmien** ja valitse Yleinen ongelma. Tässä esimerkissä **en saa muodostettua yhteyttä Windows-AM** on valittuna. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Vaiheet näkyvät kohdassa ongelman seuraavassa kuvassa esitetyllä tavalla: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Valitse *tehokkaita tiet* suositeltua vaihetta luettelossa.

6. **Tehokas tiet** sivu näkyy seuraavassa kuvassa esitetyllä tavalla:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Jos yhteyttä AM on vain yksi NIC, se on oletusarvoisesti valittuna. Jos sinulla on useampi kuin yksi NIC, valitse, jonka haluat tarkastella tehokkaita tiet NIC.

    >[AZURE.NOTE] Jos Verkkokortti liittyvät AM ei ole käynnissä, tehokkaita tiet ei näytetä. Vain ensimmäiset 200 tehokkaita tiet näkyvät portaalissa. Täydellisen luettelon Valitse **Lataa**. Voit suodattaa tulokset ladatut .csv-tiedoston.

    Huomaa seuraavat tulokset:
    - **Tietolähteen**: osoittaa ryhmän tyypin reitin. Järjestelmän tiet näytetään *Oletus*, UDRs on näkyy *käyttäjän* ja yhdyskäytävän tiet (staattinen tai erityisen) näkyvät *VPNGateway*.
    - **Tila**: ilmaisee tehokkaita reitin tilan. Mahdolliset arvot ovat *käytössä* vai *ei kelpaa*.
    - **AddressPrefixes**: määrittää osoitteen etuliite tehokkaita reitin CIDR merkintätapaa. 
    - **nextHopType**: osoittaa tietyn reitin seuraavan kohteen. Mahdolliset arvot ovat *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*tai *Null*. Arvo on *Null* -UDR **nextHopType** saattaa johtua virheellinen reitin. Esimerkiksi jos **nextHopType** on *VirtualAppliance* ja virtual verkon laitteen AM ei ole valmisteltu/käytössä-tilassa. Jos ei ei Gatewayn valmisteltu/suoritetaan annetun VNet **nextHopType** on *VPNGateway* , reitin saattaa tulla virheellisiä.
    
7. Reittiä ei ole lueteltu *WestUS VNET3* VNet (etuliite 10.10.0.0/16)- *WestUS VNet1* (etuliite 10.9.0.0/16) kuvassa edellisessä vaiheessa. Seuraavassa kuvassa peering linkki on *yhteys katkaistu* -tila:
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Kaksisuuntaisen linkin peering on katkennut, jossa kerrotaan, miksi VM1 ei voitu muodostaa *WestUS VNet3* VNet VM3.

8. Seuraavassa kuvassa näytetään tiet luomisen jälkeen kaksisuuntaisen peering linkkiä:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Lisää vianmääritys tilanteessa pakotettu tunneling ja reitin arvioinnissa Lue tämän artikkelin [huomioon otettavia seikkoja](virtual-network-routes-troubleshoot-portal.md#Considerations) -osio.

### <a name="view-effective-routes-for-a-network-interface"></a>Näytä tehokkaan tiet verkko-liittymän

Jos verkon liikenteen suunnan vaikuttaa tietyn verkko-liittymän (NIC), näet luettelon kaikista tehokkaita tiet NIC suoraan. Kooste tiet, joita käytetään NIC näkyviin toimimalla seuraavasti:

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. **Lisää palveluja**ja valitse sitten **verkon liityntäkohdat**
3. Etsi NIC nimi luettelosta tai valitse se luettelosta, joka tulee näkyviin. Tässä esimerkissä **VM1 NIC1** on valittuna.
4. Valitse **tehokkaita tiet** **verkkoliittymän** -sivu, seuraavassa kuvassa esitetyllä tavalla:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Valittu verkkoliittymän oletusarvoisesti **laajuus** .

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Näytä tehokkaan tiet reitti-taulukolle

Muokattaessa käyttäjäkohtainen tiet (UDRs) reitin taulukon haluat ehkä tarkistettava tiet, valitse tietty AM lisätään vaikutus. Reititystaulukon voi liittää aliverkosta mistä tahansa numerosta. Voit nyt tarkastella tehokkaita tiet, kaikki tietyn reitti-taulukko on liitetty, NIC tarvitsematta vaihtaa kontekstin ja poistaa tietyn reitin taulukko-sivu.

Tässä esimerkissä UDR (*UDRoute*) on määritetty reititystaulukon (*UDRouteTable*). Tätä vaihtoehtoa, toimi lähettää kaikki Internet-liikenne *Subnet1* - *WestUS VNet1* , VNet verkon virtual laitteen (NVA) on sama VNet *Subnet2* kautta. Seuraavassa kuvassa näkyy reitin:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Saat reitin taulukon kooste tiet, toimimalla seuraavasti:

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. **Lisää palveluja**ja valitse sitten **reitti-taulukot**
3. Etsi reitti-taulukkoa, haluat nähdä kooste tiet, ja valitse se luettelosta. Tässä esimerkissä **UDRouteTable** on valittuna. Valitun reitityksen taulukon sivu näkyy seuraavassa kuvassa esitetyllä tavalla:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Valitse **Tehokkaita tiet** **reitti-taulukko** -sivu. **Laajuus** on määritetty reitin taulukosta.
5. Reititys-taulukko voidaan lisätä useita aliverkosta. Valitse luettelosta tarkasteltavan **aliverkon** . Tässä esimerkissä **Subnet1** on valittuna.
6. Valitse **Verkkokäyttöliittymän**. Kaikki valitun aliverkon yhteydessä NIC näkyvät. Tässä esimerkissä **VM1 NIC1** on valittuna.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Jos Verkkokortti ei ole liitetty käynnissä AM, ei ole tehokas tiet ovat näkyvissä.

## <a name="considerations"></a>Huomioon otettavia seikkoja

Seuraavat asiat kannattaa pitää mielessä, kun tarkistaminen tiet luettelo palauttaa:

- Reititys perustuu-pisimmän etuliitteen vastine (LPM) kesken UDRs, erityisen ja järjestelmän tiet. Jos näkyvissä on useampi kuin yksi reitin saman LPM vastine, valitse reitti valitaan perusteella alkuperän seuraavassa järjestyksessä:
    - Käyttäjän määrittämät reitin
    - Erityisen reitin
    - Reitin järjestelmän (oletus)

    Tehokas tiet, jossa näet vain tehokkaita tiet, jotka ovat LPM vastineita tyylivalikoimien tiet perusteella. Miten tiet ovat todella arvioidun annetun NIC näyttämällä Tämä helpottaa usein tietyn tiet, joka saattaa vaikuttavat, että AM yhteyden vianmääritys.

- Jos olet UDRs ja lähettää liikenne verkon virtual laitteeseen (NVA) kanssa *VirtualAppliance* kuin **nextHopType**, varmistettava, että IP-välityksen on otettu käyttöön vastaanottaminen liikenne NVA tai paketteja olevat kohteet poistetaan. 
- Jos pakotettu tunnelointi on käytössä, kaikki Internet-liikenteen oikeutta paikallisen. Kun tämä asetus, sen mukaan, miten paikallista käsittelee tämän liikenne ei välttämättä toimi RDP/SSH Internetistä, että AM. 
  Pakotettu tunneling voidaan ottaa käyttöön:
    - Jos sivusto sivusto VPN määrittämällä nextHopType kuin VPN-yhdyskäytävän ja käyttäjän määrittämät reititys (UDR)
    - Jos oletusarvoinen Reitti ilmoitetaan erityisen päälle
- VNet peering tietoliikenteen toimisi oikein, **nextHopType** *VNetPeering* reittiä, järjestelmän on oltava peered VNet etuliite alueen. Jos esimerkiksi reitin ei ole ja VNet peering linkki näyttää OK:
    - Odota hetki ja yritä uudelleen, jos se on äskettäin peering linkki. Joskus kestää kauemmin leviäminen tiet aliverkon kaikki Verkkoliittymät.
    - Verkko-käyttöoikeusryhmän (NSG) sääntöjä saattaa vaikuttavat liikenne kulkee. Lisätietoja on artikkelissa [Vianmääritys verkon käyttöoikeusryhmät](virtual-network-nsg-troubleshoot-portal.md) .
