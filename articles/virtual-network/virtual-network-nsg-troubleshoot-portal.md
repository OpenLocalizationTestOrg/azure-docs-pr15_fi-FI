<properties 
   pageTitle="Vianmääritys verkon käyttöoikeusryhmät - portaalin | Microsoft Azure"
   description="Opi suorittamaan verkon käyttöoikeusryhmät Azure Resurssienhallinta käyttöönoton mallin Azure-portaalissa."
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

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Verkon käyttöoikeusryhmät Azure-portaalissa vianmääritys

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShellin](virtual-network-nsg-troubleshoot-powershell.md)

Jos määritetty verkon käyttöoikeusryhmät (NSGs) virtuaalikoneen (AM) ja esiintyy AM yhteysongelmat, tässä artikkelissa on yleiskatsaus diagnostiikka ominaisuuksia NSGs ongelmien toimenpiteitä varten.

NSGs avulla voit ohjata liikenne, että työnkulku ja siitä uloskirjautuminen näennäiskoneiden (VMs). NSGs voi suojata aliverkosta Azure Virtual Network (VNet), verkkoliittymät (NIC) tai molempia. Tehokas sääntöjä NIC ovat säännöt, jotka ovat käytetty NIC NSGs ja se on yhdistetty aliverkon yhteenlaskeminen. Nämä NSGs yli säännöt voi joskus ristiriidassa keskenään ja vaikuttaa AM verkkoyhteyden.  

Voit tarkastella kaikkia tehokkaita suojaussäännöt-että NSGs oman AM NIC käytössä. Tässä artikkelissa kerrotaan vianmääritys AM yhteysongelmat sääntöjen käyttäminen Azure resurssien hallinnan käyttöönottomalli. Jos et ole tottunut VNet ja NSG käsitteitä, lue [Virtual verkko](virtual-networks-overview.md) - ja [Verkko-käyttöoikeusryhmät](virtual-networks-nsg.md) yleiskatsaus artikkelit.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Vianmääritys AM liikenteen suunnan tehokkaita suojaussäännöt avulla

Tilanne, jossa seuraa on esimerkki yleisen yhteyden ongelman:

AM, jonka nimi on *VM1* on osa nimeltä *Subnet1* sisällä VNet, nimeltä *WestUS VNet1*aliverkon. Yhteyden muodostaminen käyttämällä RDP TCP-portin 3389 AM epäonnistuu. NSGs käytetään NIC- *VM1 NIC1* ja aliverkon *Subnet1*. TCP-portin 3389 liikenteen on sallittu verkkoliittymän *VM1 NIC1*liittyvät NSG kuitenkin TCP ping VM1's portti 3389 epäonnistuu.

Tässä esimerkissä käytetään TCP-portin 3389, kun seuraavat vaiheet voidaan määrittää saapuvan ja lähtevän epäonnistuneita minkä tahansa portin kautta.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Näytä virtual machine tehokkaita suojaussäännöt

Mikä AM NSGs vianmäärityksen seuraavasti:

Voit tarkastella tehokkaita suojaussäännöt täydellinen luettelo NIC-AM itse. Voit myös lisätä, Muokkaa ja poista NIC ja aliverkon NSG säännöt tehokkaita säännöt-sivu, jos sinulla on käyttöoikeudet, voit suorittaa nämä toiminnot.

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. Valitse **Lisää palveluja**ja valitse sitten **näennäiskoneiden** luettelossa, joka tulee näkyviin.
3. Valitse luettelosta, joka tulee vianmäärityksestä AM ja AM-sivu, jossa asetukset tulee näkyviin.
4. Valitse **diagnosointi & ongelmien** ja valitse Yleinen ongelma. Tässä esimerkissä **en saa muodostettua yhteyttä Windows-AM** on valittuna. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Vaiheet näkyvät kohdassa ongelman seuraavassa kuvassa esitetyllä tavalla: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Valitse *tehokkaita suojaussäännöt ryhmän* suositeltua vaihetta luettelossa.

6. **Hae tehokkaita suojaussäännöt** -sivu näkyy seuraavassa kuvassa esitetyllä tavalla:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Huomaa kuvan seuraavat osat:

    - **Laajuus:** Asettaminen *VM1*AM valittuna vaiheessa 3.
    - **Verkkoliittymän:** *VM1 NIC1* on valittuna. AM voi olla useita verkon liityntäkohdat (NIC). Kunkin NIC: ssä voi olla yksilöllinen tehokkaita suojaussäännöt. Vianmääritys, kun joudut ehkä tarkastella tehokkaita suojaussäännöt kunkin NIC-sivustosta.
    - **Liittyvät NSGs:** NSGs voi suojata Verkkokortti ja aliverkon Verkkokortti on yhdistetty. Olevassa kuvassa NSG on käytössä Verkkokortti ja se on yhdistetty aliverkon. Voit valita muokata sääntöjä NSGs NSG lisättävät nimet.
    - **VM1 nsg-välilehti:** Kuvassa näkyy Sääntöluettelo on NSG käytetty NIC-sivustosta. Useita oletusarvon säännöt luodaan Azure aina, kun NSG luodaan. Et voi poistaa oletusarvon sääntöjä, mutta voit ohittaa ne ja suuri prioriteetti-sääntöjä. Lisätietoja oletusarvoisen säännöt artikkeliin [NSG yleiskatsaus](virtual-networks-nsg.md#default-rules) .
    - **Kohde-sarakkeen:** Joitakin sääntöjä on tekstiä, sarakkeen samalla, kun muut käyttävät osoitteiden etuliitteiden. Teksti on oletusarvoinen tunnisteita käytetään suojauksen säännön, kun se on luotu nimi. Tunnisteet ovat järjestelmän mukana tunnukset, jotka vastaavat useita etuliitteitä. **Osoitteiden etuliitteiden** sivu etuliitteitä valitsemalla säännön tunniste, kuten *AllowInternetOutBound*luettelo.
    - **Lataa:** Sääntöluettelo voi olla pitkä. Voit ladata offline-tilassa analysointiin sääntöjen .csv-tiedoston valitsemalla **Lataa** ja tallentamalla tiedosto.
    - **AllowRDP** Saapuvan liikenteen sääntö: tämän säännön avulla AM RDP yhteydet.
7. Näytä käytetty aliverkon, NSG tehokkaita säännöt seuraavassa kuvassa esitetyllä tavalla **Subnet1 NSG** -välilehdessä: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Huomaa *denyRDP* **saapuvien** sääntö. Saapuvan liikenteen säännöt käytetään aliverkon arvioidaan ennen sääntöjä käytetään Verkkokäyttöliittymän. Estä sääntöä käytetään, aliverkon, koska pyynnön muodostaa TCP 3389 epäonnistuu, koska Salli säännön osoitteessa Verkkokortti koskaan arvioidaan. 

    *DenyRDP* sääntö on miksi RDP-yhteys epäonnistui tuntemattomasta syystä. Poisto olisi ratkaisemaan ongelman.

    >[AZURE.NOTE]Jos Verkkokortti liittyvät AM ei ole käynnissä tai NSGs eivät ole kohdistettu NIC tai aliverkon, mitään sääntöä ovat näkyvissä.

8. Voit muokata NSG säännöt valitsemalla *Subnet1 NSG* **Liittyvä NSGs** -osassa.
   **Subnet1 NSG** -sivu avautuu. Voit muokata sääntöjä suoraan valitsemalla **Saapuvan liikenteen säännöt**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Poistaminen *denyRDP* saapuvan liikenteen säännön **Subnet1 NSG** ja lisäämällä *allowRDP* säännön, tehokkaita säännöt-luettelo näyttää seuraavassa kuvassa:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Varmista, että TCP-portin 3389 on avoinna avaamalla RDP-yhteyden AM tai PsPing-työkalun avulla. Voit lukea lisää PsPing lukemalla [PsPing lataussivulle](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Näytä verkkoliittymän tehokkaita suojaussäännöt

Jos AM-liikenne työnkulku vaikuttaa tietyn NIC varten, voit tarkastella verkon liittymät kontekstista NIC luettelon kaikista tehokkaita säännöt toimimalla seuraavasti:

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. Valitse **Lisää palveluja**ja valitse sitten **verkon liityntäkohdat** luettelossa, joka tulee näkyviin.
3. Valitse Verkkokäyttöliittymän. Seuraavassa kuvassa NIC nimeltä *VM1 NIC1* on valittuna.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Huomaa, että **laajuus** on määritetty verkkoliittymän valittuna. Lisätietoja näkyvät lisätiedot, lue tämän artikkelin **Vianmääritys NSGs varten AM** -osan vaiheessa 6.

    >[AZURE.NOTE] NSG poistetaan verkon-liittymän, jos aliverkon NSG on edelleen voimassa-annetun NIC-sivustosta. Tässä tapauksessa tulosteen vain näkyy aliverkosta NSG säännöt. Sääntöjen näkyviin vain, jos Verkkokortti on liitetty AM.

4. Voit muokata sääntöjä suoraan NIC: ssä tai aliverkon liittyvää NSGs. Lisätietoja Lue, miten vaihe 8 on tämän artikkelin **näkymän tehokasta suojausta koskevat virtual machine** -osassa.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Näytä verkon käyttöoikeusryhmän (NSG) tehokkaita suojaussäännöt

Kun NSG sääntöjen muokkaaminen haluat ehkä tarkastella tietyn AM lisätään säännöt vaikutus. Kaikki NIC joka annetun NSG on käytössä, voit tarkastella luettelon kaikista tehokkaita suojaussäännöt tarvitsematta vaihtaa kontekstin ja poistaa tietyn NSG-sivu. Vianmääritys NSG tehokkaita sääntöjen, toimimalla seuraavasti:

1. Kirjaudu Azure-portaalissa osoitteessa https://portal.azure.com.
2. Valitse **Lisää palveluja**ja valitse sitten **verkon käyttöoikeusryhmät** luettelossa, joka tulee näkyviin.
3. Valitse NSG. Seuraavassa kuvassa NSG nimeltä VM1 nsg on valittuna.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Huomaa edellisen kuvan seuraavat osat:

    - **Laajuus:** Määritä valitun NSG.
    - **Virtuaalikoneen:** Jos aliverkon otetaan käyttöön NSG, otetaan käyttöön kaikissa verkkoliittymät liitetty liitetty aliverkon kaikki VMs. Tässä luettelossa näkyvät kaikki VMs tämän NSG otetaan käyttöön. Voit valita minkä tahansa AM luettelosta.

    >[AZURE.NOTE] Jos NSG käytetään vain tyhjään aliverkon, VMs eivät näy. Jos NSG käytetään NIC: ssä, joka ei ole liitetty AM, kyseiset NIC ei näkyvät myös. 
    - **Verkko-liittymän:** AM voi olla useita Verkkoliittymät. Voit valita verkkoliittymän valitun AM liitetään.
    - **AssociatedNSGs:** Milloin tahansa NIC: ssä voi olla jopa kaksi tehokkaita NSGs, yksi Verkkokortti ja toinen aliverkon. Vaikka alue on valittu VM1-nsg, jos Verkkokortti on tehokas aliverkon NSG, tulos näkyy sekä NSGs.
4. Voit muokata sääntöjä suoraan liittyy NIC tai aliverkon NSGs. Lisätietoja Lue, miten vaihe 8 on tämän artikkelin **näkymän tehokasta suojausta koskevat virtual machine** -osassa.

Lisätietoja näkyvät lisätiedot, lue tämän artikkelin **näkymän tehokasta suojausta koskevat virtual machine** -osan vaiheessa 6.

>[AZURE.NOTE] Vaikka aliverkon ja NIC kaikilla voi olla vain yksi NSG niitä käytetään, NSG voi liittää useita NIC- ja useita aliverkosta.

## <a name="considerations"></a>Huomioon otettavia seikkoja

Ota seuraavat seikat huomioon, kun yhteysongelmien vianmääritys:

- Oletusarvon NSG säännöt Internetistä saapuvien käytön estäminen ja sallia vain VNet saapuva liikenne. Sääntöjen kannattaa lisätä eksplisiittisesti sallimaan saapuvan Internet-halutulla tavalla.
- Jos ei ole NSG suojaussäännöt vuoksi AM verkkoyhteyden epäonnistuu, syy voi olla ongelman:
    - Palomuuriohjelmiston sisällä AM-käyttöjärjestelmä
    - Määritetty virtual laitteiden tai paikallisen liikenne ohjautuu. Internet-liikenne on ohjattu paikalliseen kautta pakotettu tunneling. Kun tämä asetus, sen mukaan, miten paikallisen verkkolaitteiston käsittelee tämän liikenne ei välttämättä toimi on RDP/SSH yhteys Internet-yhteyttä AM. Lue, miten voit selvittää reitin ongelmia, jotka voivat olla estämään etenemisen liikenteen ja siitä uloskirjautuminen AM artikkeliin [Vianmääritys tiet](virtual-network-routes-troubleshoot-powershell.md) . 
- Jos olet peered VNets, oletusarvoisesti, VIRTUAL_NETWORK tunnisteen laajenee automaattisesti sisällytettävät etuliitteiden peered VNets. Voit tarkastella seuraavia etuliitteitä **ExpandedAddressPrefix** -luettelosta vianmääritys VNet peering connectivity liittyvät ongelmat. 
- Tehokas suojaussäännöt näkyvät vain onko NSG liittyvät AM NIC ja aliverkon. 
- Jos argumentissa on ei ole liitetty Verkkokortti NSGs tai aliverkon ja sinulla on julkinen IP-osoite, että AM myönnetyt, kaikki portit on avoin saapuva ja lähtevä käytön. Jos AM on julkinen IP-osoite, NSGs soveltaminen NIC tai aliverkon on erittäin suositeltavaa.