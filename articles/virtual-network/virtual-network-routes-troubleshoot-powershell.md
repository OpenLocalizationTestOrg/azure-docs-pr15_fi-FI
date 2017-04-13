<properties 
   pageTitle="Vianmääritys tiet - PowerShell | Microsoft Azure"
   description="Opi suorittamaan tiet Azure Resurssienhallinta käyttöönoton mallin Azure PowerShellin avulla."
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

# <a name="troubleshoot-routes-using-azure-powershell"></a>Vianmääritys tiet Azure PowerShellin avulla

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

### <a name="view-effective-routes-for-a-network-interface"></a>Näytä tehokkaan tiet verkko-liittymän

Kooste tiet, joita käytetään verkon käyttöliittymä näkyviin toimimalla seuraavasti:

1.  Käynnistä PowerShellin Azure-istunnon ja Azure kirjautuessasi. Jos et ole tottunut PowerShellin Azure, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) -artikkelissa.

2.  Seuraava komento palauttaa kaikki tiet käytetty nimetty *VM1 NIC1* resurssiryhmä *RG1*Verkkokäyttöliittymän.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Jos et tiedä verkkoliittymän nimi, kirjoita seuraava komento noutaa kaikki verkkoliittymät-resurssin group.* nimet

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Seuraava tulos näyttää seuraavankaltaiselta kunkin reitityksen käytetty Verkkokortti on yhdistetty aliverkon tulos:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Huomaa seuraavat tulokset:
    - **Nimi**: tehokkaan reitityksen nimi voi olla tyhjä, ellet nimenomaisesti määritetty, käyttäjän määrittämä tiet. 
    - **Tila**: ilmaisee tehokkaita reitin tilan. Mahdolliset arvot ovat "Aktiivinen" tai "Ei kelpaa"
    - **AddressPrefixes**: määrittää osoitteen etuliite tehokkaita reitin CIDR merkintätapaa. 
    - **nextHopType**: osoittaa tietyn reitin seuraavan kohteen. Mahdolliset arvot ovat *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*tai *Null*. Arvo on *Null* -UDR **nextHopType** saattaa johtua virheellinen reitin. Esimerkiksi jos **nextHopType** on *VirtualAppliance* ja virtual verkon laitteen AM ei ole valmisteltu/käytössä-tilassa. Jos ei ei Gatewayn valmisteltu/suoritetaan annetun VNet **nextHopType** on *VPNGateway* , reitin saattaa tulla virheellisiä.
    - **NextHopIpAddress**: määrittää tehokkaita reitin seuraavan pisteen IP-osoite.
    
    Seuraava komento palauttaa tiet helpompi tarkastella taulukkoon:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Seuraava tulos on joitakin vastaanotettu kuvatuilla skenaarion tulos:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Reittiä ei ole lueteltu edellisessä vaiheessa *WestUS VNet3* VNet (etuliite 10.10.0.0/16)* * - *WestUS VNet1* (etuliite 10.9.0.0/16) tulosteessa. Seuraavassa kuvassa esitetyllä *WestUS VNet3* VNet VNet peering linkki on *yhteys katkaistu* -tila.
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Kaksisuuntaisen linkin peering on katkennut, jossa kerrotaan, miksi VM1 ei voitu muodostaa *WestUS VNet3* VNet VM3. Asennus kaksisuuntaisen VNet peering linkki uudelleen *WestUS VNet1* ja *WestUS VNet3* VNets. Palauttaa jälkeen VNet peering linkki on muodostettu oikein tulosteen seuraavasti:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Kun olet selvittänyt ongelman, voit lisätä, poistaa, tai muuta tiet ja reitittää taulukot. Kirjoita seuraava komento kiellä käytettävien komentojen luettelo:

        Get-Help *-AzureRmRouteConfig

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
    - Verkko-käyttöoikeusryhmän (NSG) sääntöjä saattaa vaikuttavat liikenne kulkee. Lisätietoja on artikkelissa [Vianmääritys verkon käyttöoikeusryhmät](virtual-network-nsg-troubleshoot-powershell.md) .
