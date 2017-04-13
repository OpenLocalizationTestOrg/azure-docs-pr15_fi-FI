<properties
   pageTitle="Yhteysasetusten perinteinen käyttöönoton mallin VNet VNet | Microsoft Azure"
   description="Voit yhdistää Azure virtual verkkojen ehtorivien PowerShell ja Azure perinteinen-portaalin."
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
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Perinteinen käyttöönoton mallin VNet VNet yhteyden määrittäminen

> [AZURE.SELECTOR]
- [Resurssien hallinta - portaalissa Azure](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Resurssien hallinta - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Perinteinen - perinteinen Portal](virtual-networks-configure-vnet-to-vnet-connection.md)



Tässä artikkelissa käydään läpi vaiheet ja luoda ja liittää virtual verkkojen ehtorivien perinteinen käyttöönottomalli (tunnetaan myös nimellä Service Management). Seuraavat vaiheet Azure perinteinen portaalin avulla voit luoda VNets ja yhdyskäytävien ja PowerShell, VNet VNet-yhteyden määrittäminen. Et voi määrittää yhteys-portaalissa.

![VNet VNet yhteys-kaavio](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien VNet VNet yhteydet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Seuraavassa taulukossa on tällä hetkellä käytettävissä käyttöönoton mallit ja viestintämenetelmien VNet VNet määrityksiä. Kun määritysvaiheet artikkeli on käytettävissä, on linkki suoraan tästä taulukosta.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Tietoja VNet VNet yhteyksistä

Yhteyden muodostaminen toiseen virtual virtual verkon (VNet VNet) on samanlainen kuin yhteyden virtual verkon paikallisen sivuston sijainnissa. Yhteyksien molempien tietotyyppien käyttää VPN-yhdyskäytävän suojatun tunnelin IP/IKE. 

Yhdistät VNets voi olla eri tilaukset ja eri alueiden osalta. Voit yhdistää VNet VNet tietoliikenteen usean sivuston määrityksiä. Voit vahvistaa verkkotopologioita, joka yhdistää paikallisen-yhteyksien väliset VPN-yhteyttä.


### <a name="why-connect-virtual-networks"></a>Miksi yhteyden virtual verkkoja?

Voit yhdistää virtual verkkojen seuraavista syistä:

- **Toimintojen välinen alueen geo-redundanssin ja geo tavoitettavuus**
    - Voit määrittää oman geo replikoinnin tai synkronoinnin suojattua yhteyttä siirtymättä Internetiin yhteydessä oleva päätepisteet päälle.
    - Azure kuormituksen ja Microsoftin tai kolmansien osapuolten klusteroinnin tekniikka voit määrittää erittäin käytettävissä kuormituksen vikasietoisuusominaisuuksilla geo useiden Azure alueiden välillä. Tärkeää esimerkki on määrittäminen SQL aina käyttöön käytettävyys ryhmiin levittäminen useita Azure alueiden välillä.

- **Alueellisten monitasoisten sovellusten vahva eristystaso reunaa**
    - Samalla alueella voit määrittää monitasoisten sovellusten yhteydessä sivustoa vahva eristystaso ja välisten taso tietosuojaa useita VNets kanssa.

- **Toimintojen väliset organisaation tietoliikenteen Azure-tilaukseen**
    - Jos sinulla on useita tilauksia Azure, voit muodostaa työmääriä eri tilaukset-yhdessä suojatusti virtual verkkojen välillä.
    - Keskisuuret ja palveluntarjoajat voit ottaa organisaatioiden välinen tiedonsiirto suojatun VPN-tekniikka Azure kuluessa.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>Perinteinen VNets VNet VNet usein kysytyt kysymykset

- Virtuaalinen verkot voivat olla saman tai toisen-tilauksessa.

- Virtuaalinen verkot voivat olla saman tai toisen Azure alueilla (sijainti).

- Pilvipalveluun tai kuormituksen päätepisteen ei kattavat virtual verkkojen, vaikka ne ovat yhteydessä toisiinsa.

- Useiden virtual verkostojen yhdistäminen toisiinsa ei edellytä VPN-laitteita.

- VNet VNet tukee Azure Virtual verkkojen yhdistämistä. Se ei tue yhdistäviä näennäiskoneiden tai cloud services, joka ei ole asennettu virtual verkkoon.

- VNet VNet edellyttää dynaaminen reititys yhdyskäytävät. Azure staattinen reititys yhdyskäytävien ei tueta.

- VPN-yhteyden voi käyttää samanaikaisesti usean sivuston VPN-yhteydet. On enintään 10 VPN-tunneleita VPN VPN yhdyskäytävän yhteyden muiden verkkojen virtual tai paikallisen sivustot.

- Osoite-välilyöntejä virtual verkkojen ja paikallisen lähiverkon sivustojen on päällekkäin. Päällekkäiset osoite välilyöntejä aiheuttaa virtual verkkoja tai lataus epäonnistuu netcfg tiedostojen luominen.

- Tarpeettomien tunneleita virtual verkkojen kahdet välillä ei tueta.

- Kaikki VPN-tunneleita VNet P2S VPN-yhteydet, mukaan lukien jakaa kaistanleveyttä VPN-yhdyskäytävän ja saman VPN-yhdyskäytävän käytettävyyttä SLA Azure-tietokannassa.

- VNet VNet liikenne kulkevan Azure kuvaa runkoverkkoa.


## <a name="step1"></a>Vaihe 1 - IP-osoitealueet suunnitteleminen

On tärkeää päättää, joka määrittää virtual verkot käytetään alueita. Varmista tässä kokoonpanossa ei mitään VNet alueiden päällekkäin toisiinsa tai paikallinen verkkoihin muodostavat yhteyden kanssa.

Seuraavassa taulukossa näkyy, miten voit määrittää oman VNets. Käytä vain ohjeena alueet. Kirjoita muistiin virtual lopettaminen tulostusalueet. Tarvitset näitä tietoja myöhemmin vaiheet.

**Esimerkki asetukset**

|VPN  |Osoitetilaa varten               |Alue      |Muodostaa yhteyden paikalliseen verkkoon kirjauduttaessa sivustoon|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |Yhdysvaltain Länsi     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Japani Itä  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Vaihe 2: Luo VNet1

Tässä vaiheessa nyt luoda VNet1. Kun esimerkkien avulla, muista korvata oman arvoilla. Jos yhteyttä VNet on jo, ei tarvitse tehdä tässä vaiheessa. Mutta haluat varmistaa, että IP-osoitealueet ole päällekkäin toisen VNet alueiden kanssa tai mihin tahansa muita VNets, johon haluat muodostaa yhteyden.

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com). Tässä artikkelissa Käytämme perinteinen portaalin, koska joitakin tarvittavat asetukset eivät ole vielä käytettävissä Azure-portaalissa.

2. Valitse näytön vasemmassa alakulmassa **Uusi** > **Verkkopalveluita** > **VPN** > Aloita ohjattu määritystoiminto**Mukautettu luominen** . Kun siirryt ohjatussa toiminnossa, Lisää määritetyt arvot jokaiselle sivulle.

### <a name="virtual-network-details"></a>VPN-tiedot

Anna Virtual verkon tiedot-sivulla seuraavat tiedot:

  ![VPN-tiedot](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Nimi** - virtual verkon nimi. Esimerkiksi VNet1.
  - **Sijainti** : Kun luot virtual verkon voit liittää sen Azure sijainnin (alue). Jos haluat, että VMs, jotka on otettu virtual verkon Länsi US fyysisesti sijaitsee, valitse kyseiseen sijaintiin. Et voi muuttaa sijaintiin liittyvät virtual verkon sen luomisen jälkeen.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-palvelimet ja VPN-yhteys

Valitse DNS-palvelimet ja VPN-yhteys-sivulla seuraavat tiedot ja valitse sitten oikeassa alakulmassa seuraava-nuoli.

  ![DNS-palvelimet ja VPN-yhteys](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS-palvelimet** - DNS-palvelimen nimi ja IP-osoite tai valitse aiemmin rekisteröidyn DNS-palvelin avattavasta valikosta. Tämä asetus luo DNS-palvelimeen. Sen avulla voit määrittää DNS-palvelimet, jota haluat käyttää virtual verkoston nimenselvitys. Jos haluat nimenselvitys virtual verkon välillä, sinun on määrittää DNS-palvelimeen kuin käyttämällä nimenselvitys, joka tarjoaa Azure.
- Älä valitse P2S tai S2S connectivity valintaruudut. Napsauta oikeassa alakulmassa Siirry seuraavassa näytössä olevaa nuolta.

### <a name="virtual-network-address-spaces"></a>VPN osoite välilyöntejä

Määritä, jota haluat käyttää virtual verkon osoitealueita välilyöntejä Virtual verkko-osoite-sivulla. Nämä ovat dynaamisia IP-osoitteet (tapahtuneen), joka määritetään VMs ja muut rooli esiintymät, jotka virtual verkoston käyttöön. 

Jos olet luomassa VNet, joka on valmisteltu yhteys paikalliseen verkkoon, on erityisen tärkeää avulla haluamasi alueet, joita käytetään paikallista verkkoa varten, joka ei ole päällekkäinen alueen valitseminen. Tässä tapauksessa sinun täytyy verkonvalvoja sointuvat. Verkonvalvoja on ehkä carve alueen IP-osoitteiden paikallisen verkon osoite tilasta voit käyttää omaa VNet ulos.

  ![Virtuaalinen verkko-osoitteen välilyöntejä-sivu](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Osoitetilan** - mukaan lukien käynnistäminen IP-osoite ja osoitteiden määrä. Tarkista osoite välilyöntejä, voit määrittää ole päällekkäin osoite välilyöntejä, joihin sinulla on paikallisen verkon avulla. Tässä esimerkissä Käytämme 10.1.0.0/16 VNet1 varten.
  - **Lisää aliverkon** - mukaan lukien käynnistäminen IP-osoite ja osoitteiden määrä. Lisää aliverkosta eivät ole pakollisia, mutta voit halutessasi luoda erilliset aliverkon VMs, joka on staattinen tapahtuneen. Tai voit halutessasi oman VMs aliverkon, joka on erillään muiden roolin esiintymien.
 
Luo alkaa oikeassa alakulmassa sivun ja virtual verkon **Napsauta valintamerkkiä** . Kun se on valmis, näyttöön tulee "Luotu" verkkojen sivulla luetellut tila-kohdassa.

## <a name="step-3---create-vnet2"></a>Vaihe 3 – Luo VNet2

Seuraavaksi toistamalla edellä kuvatut toimet voit luoda toisen VNet. Myöhemmissä vaiheissa yhdistät kaksi VNets. Voit viitata [Esimerkki asetukset](#step1) vaiheessa 1. Jos yhteyttä VNet on jo, ei tarvitse tehdä tässä vaiheessa. Sinun on kuitenkin varmistaa, että IP-osoitealueet ei ole päällekkäinen muiden verkostojen VNets tai paikalliseen, johon haluat muodostaa yhteyden avulla.

## <a name="step-4---add-the-local-network-sites"></a>Vaihe 4 – lähiverkon sivustojen lisääminen

Kun luot VNet VNet määrityksen, sinun täytyy paikalliseen verkkoon kirjauduttaessa sivustojen, jotka näkyvät **Paikallisen verkkojen** sivun portaalin määrittäminen. Azure käyttää sivustosta paikalliseen verkkoon kirjauduttaessa määritettyjä asetuksia ja kysyttävä, kuinka voit reitittää liikenteen VNets välillä. Voit määrittää haluamasi sivustosta paikalliseen verkkoon kirjauduttaessa käytettävä nimi. Kannattaa käyttää jotakin kuvaavia, kun valitset arvo avattavasta luettelosta myöhemmissä vaiheissa.

Esimerkiksi VNet1 muodostaa paikalliseen verkkoon kirjauduttaessa sivustoon, jonka luot nimeltä "VNet2Local". VNet2Local asetusten sisältävät osoitteiden etuliitteiden VNet2 varten ja VNet2 yhdyskäytävän julkiseen IP-osoite. VNet2 muodostaa yhteyden paikalliseen verkkoon kirjauduttaessa sivuston luomisen nimeltä "VNet1Local", joka sisältää osoitteiden etuliitteiden VNet1 ja VNet1 yhdyskäytävän julkiseen IP-osoite.

### <a name="localnet"></a>Lisää VNet1Local lähiverkossa-sivusto

1. Valitse näytön vasemmassa alakulmassa **Uusi** > **Verkkopalveluita** > **VPN** > **Lisääminen paikalliseen verkkoon kirjauduttaessa**.

2. **Määritä lähiverkossa-tiedot** -sivulla **nimi**, kirjoita nimi, jonka haluat edustavan verkkoon, johon haluat muodostaa yhteyden. Tässä esimerkissä "VNet1Local" Voit käyttää viittaamaan IP-osoitealueet ja yhdyskäytävän VNet1 varten.

3. Määritä **VPN laitteen IP-osoite (valinnainen)**, mikä tahansa kelvollinen julkiseen IP-osoite. Yleensä vakiomittoja VPN-laitteen todellinen ulkoinen IP-osoite. Käytät VNet VNet määrityksiä julkinen IP-osoite, joka on liitetty yhdyskäytävä oman VNet. Mutta koska, että olet ei ole vielä luonut yhdyskäytävän, voit määrittää minkä tahansa kelvollinen julkiseen IP-osoite paikkamerkkinä. Älä jätä tämä kenttä tyhjäksi – ei ole tässä määrityksessä on valinnainen. Myöhemmin Siirry takaisin nämä asetukset ja määrittää niitä vastaavat Gatewayn IP-osoitteet, kun Azure luo se. Siirry seuraavaan näyttöön nuolta.

4. Kirjoita **osoite-sivulla Määritä**IP address alue- ja osoite määrä VNet1 varten. Tämä on vastattava täsmälleen alue, joka on määritetty VNet1. Azure käyttää, ja haluat reitittää liikenteen VNet1 tarkoitettu Määritä IP-osoitealueet. Valitse Luo lähiverkossa valintamerkki.

### <a name="add-the-local-network-site-vnet2local"></a>Lisää VNet2Local lähiverkossa-sivusto

Yllä mainittujen ohjeiden avulla voit luoda paikallisen verkko-sivusto "VNet2Local". [Esimerkki asetukset](#step1) vaiheessa 1 arvot voidaan viitata tarvittaessa.

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Kunkin VNet osoittamaan paikallisen verkon määrittäminen

Kunkin VNet on osoitettava vastaaviin paikalliseen verkkoon, jonka haluat reitittää liikenteen. 

#### <a name="for-vnet1"></a>VNet1 varten

1. Siirry virtual verkon **VNet1** **määrittäminen** -sivulla. 
2. Valitse sivusto yhteyden "Muodosta yhteys paikalliseen verkkoon kirjauduttaessa" ja valitse sitten **VNet2Local** kuin paikalliseen verkkoon kirjauduttaessa avattavasta valikosta. 
3. Tallenna asetukset.

#### <a name="for-vnet2"></a>VNet2 varten

1. Siirry virtual verkon **VNet2** **määrittäminen** -sivulla. 
2. Valitse sivusto yhteyden "Muodosta yhteys paikalliseen verkkoon kirjauduttaessa" ja valitse Valitse **VNet1Local** avattavasta valikosta kuin paikalliseen verkkoon kirjauduttaessa. 
3. Tallenna asetukset.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Vaihe 5 – kunkin VNet yhdyskäytävän määrittäminen

Määritä kunkin virtual verkon dynaaminen reititys yhdyskäytävä. Tämä määritys ei tue staattinen reititys yhdyskäytävät. Jos käytössäsi on VNets, jotka on aiemmin määritettyyn ja joka on jo dynaaminen reititys yhdyskäytävät, ei tarvitse tehdä tässä vaiheessa. Jos yhteyttä yhdyskäytävien staattinen reititys, tarvitset niitä ja luo uudelleen nimellä dynaaminen reititys yhdyskäytävät. Jos poistat yhdyskäytävän, sille julkisen IP-osoitteen saa julkaissut eikä sinun tarvitse palata ja määrittää paikallisen verkkojen ja VPN-laitteet, joissa on uusi julkinen IP-osoite uuden yhdyskäytävän uudelleen.

1. Varmista **verkot** -sivulla, että virtual verkon tila-sarakkeessa on **luotu**.

2. Valitse virtual verkon nimi **nimi** -sarakkeessa. Tässä esimerkissä Käytämme "VNet1".

3. **Dashboard** -sivun Huomaa, että tämä VNet ei ole vielä määritetty yhdyskäytävä. Näet tilan kuitenkin määrittäminen käyttämäsi yhdyskäytävän vaiheet.

4. Valitse sivun alareunassa **Luo yhdyskäytävä** ja **Dynaaminen reititys**. Kun järjestelmä pyytää sinua vahvistamaan, että haluat luoda yhdyskäytävän, valitse Kyllä.

    ![Yhdyskäytävän tyyppi](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Käyttämäsi yhdyskäytävän luotaessa Huomaa yhdyskäytävän kuva sivun keltainen muuttuu, ja teksti "Luominen yhdyskäytävän". Yleensä kestää noin 30 minuuttia Gatewayn luominen.

6. Toista samat vaiheet VNet2. Sinun ei tarvitse ensimmäisen VNet yhdyskäytävän, ennen kuin alat luoda yhdyskäytävän oman VNet suorittamiseen.

7. Kun yhdyskäytävän tila muuttuu "Yhteyden", kukin yhdyskäytävän julkiseen IP-osoite on näkyvissä koontinäytön. Kirjoita muistiin IP-osoitetta, joka vastaa, kunkin VNet varoen sekoita niitä. Nämä ovat IP-osoitteet, joita käytetään muokattaessa paikkamerkin IP-osoitteiden kunkin paikallisen verkon VPN-laitetta varten.

## <a name="step-6---edit-the-local-network"></a>Vaihe 6: Muokkaa lähiverkossa

1. **Paikallinen verkkojen** sivulla paikallisen verkon nimi, jota haluat muokata nimeä ja valitse sitten sivun alareunassa **Muokkaa** . **VPN-laitteen IP-osoite**Kirjoita yhdyskäytävän, joka vastaa VNet IP-osoite. Esimerkiksi varten VNet1Local, sijoita VNet1 myönnetyt yhdyskäytävän IP-osoite. Valitse sivun alareunassa olevaa nuolta.

2. Valitse **Määritä osoitetilaa varten** -sivulla oikeassa alakulmassa valintamerkki ilman, että kaikki muutokset.

## <a name="step-7---create-the-vpn-connection"></a>Vaihe 7 – VPN-yhteyden luominen

Kun kaikki edelliset vaiheet on suoritettu, Määritä IP/IKE ennalta jaetut avaimet ja yhteyden muodostaminen. Tämä toimintaohjeiden käyttää PowerShell ja ei voi määrittää portaalissa. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen Azure PowerShell cmdlet-komentoja. Varmista, että voit ladata uusimman version Service Management (k) Cmdlet-komentoja. 

1. Avaa Windows PowerShellin ja kirjaudu sisään.

        Add-AzureAccount

2. Valitse tilaus, jotka sijaitsevat oman VNets.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Luo yhteyksiä. Esimerkeissä Huomaa jaetun avaimen ovat täsmälleen samat. Jaetun avaimen on oltava aina samat.


    VNet1 VNet2 yhteyteen

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 yhteyteen

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Odota, että yhteydet alustaa. Kun yhdyskäytävä on alustaa, yhdyskäytävän näyttää seuraavan kuvan.

    ![Yhdyskäytävän tila - yhteydessä](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Seuraavat vaiheet

Voit lisätä näennäiskoneiden virtual verkkoihin. Katso lisätietoja [näennäiskoneiden ohjeissa](https://azure.microsoft.com/documentation/services/virtual-machines/) .



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
