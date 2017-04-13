<properties 
   pageTitle="Perinteinen virtual verkkojen yhdistämisestä Resurssienhallinta virtual verkot portaalin | Microsoft Azure"
   description="Perinteinen VNets ja Resurssienhallinta VNets VPN-yhdyskäytävän ja portaalin välillä VPN-yhteyden luominen"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Virtual verkostojen yhdistäminen eri käyttöönoton mallit-portaalissa

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShellin](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure on kaksi hallinta-mallit: perinteinen ja resurssien hallinta (RM). Jos olet käyttänyt Azure jonkin aikaa, sinulla on luultavasti Azure VMs ja esiintymän roolit perinteinen VNet käynnissä. Uudempaan VMs ja roolin esiintymät saattaa olla käynnissä VNet, joka on luotu resurssien hallinta. Tässä artikkelissa käydään läpi perinteinen VNets liittämisestä Resurssienhallinta VNets, jotta resurssien sijaitsee erillisessä käyttöönoton mallit keskenään yhdyskäytävä-yhteyden kautta. 

Voit luoda yhteyden VNets, jotka ovat eri tilaukset ja eri alueiden välillä. Voit muodostaa VNets, joka on jo yhteyden paikallisen verkkoihin, kunhan yhdyskäytävään, joka on määritetty kanssa on dynaaminen tai reitityksen. Saat lisätietoja VNet VNet yhteydet [VNet VNet usein kysytyt kysymykset](#faq) on tämän artikkelin lopussa.

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Käyttöönotto-malleja ja viestintämenetelmien VNet VNet yhteydet

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Päivitämme seuraavan taulukon kuin uusia artikkeleita ja muita työkaluja palvelupakettiisi määritysten. Artikkelin ollessa käytettävissä on linkki suoraan taulukosta.<br><br>

**VNet VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Ennen kuin aloitat

Seuraavat vaiheet opastusta tarvittavat dynaaminen tai reitin yhdyskäytävän määrittäminen kunkin VNet ja luo välillä yhdyskäytävien VPN-yhteyden asetukset. Tämä määritys ei tue staattinen tai käytännön yhdyskäytävät. 

Tässä artikkelissa Käytämme perinteinen portaalin, Azure-portaalin ja PowerShell. Tällä hetkellä ei ole mahdollista vain Azure-portaalissa määritysten luomiseen.

### <a name="prerequisites"></a>Edellytykset

 - Molemmat VNets on jo luotu.
 - VNets osoitealueet Älä päällekkäin keskenään tai päällekkäin muiden yhteydet, jotka yhdyskäytävien voidaan yhdistää tulostusalueet avulla.
 - Olet asentanut PowerShellin cmdlet-komennot (1.0.2 tai uudempi). Lisätietoja on kohdassa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) . Varmista, että olet asentanut Service Management (k) ja resurssin Manager (RM) Cmdlet-komentoja. 

### <a name="values"></a>Esimerkki asetukset

Esimerkki-asetuksilla viittauksena.

**Perinteinen VNet asetukset**

VNet nimi = ClassicVNet <br>
Sijainti = Länsi US <br>
VPN osoite välilyöntejä = 10.0.0.0/24 <br>
Aliverkon 1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Paikallinen verkkonimi = RMVNetLocal <br>

**Resurssienhallinta VNet asetukset**

VNet nimi = RMVNet <br>
Resurssiryhmä = RG1 <br>
VPN IP-osoite välilyöntejä = 192.168.0.0/16 <br>
Aliverkon 1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Sijainti = Itä US <br>
VPN yhdyskäytävän nimi = RMGateway <br>
Yhdyskäytävän julkiseen IP-nimi = gwpip <br>
Yhdyskäytävän tyyppi = VPN <br>
VPN-tyyppi = reitin perusteella <br>
Paikallinen yhdyskäytävien = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Osa 1: Perinteinen VNet asetusten määrittäminen

Tässä osassa on luoda perinteinen VNet lähiverkossa ja yhdyskäytävän. Perinteinen-portaalin käyttäminen tämän osion ohjeita. Tällä hetkellä Azure portaalin ei tarjoa kaikkia asetuksia, jotka koskevat perinteinen VNet.

### <a name="part-1---create-a-new-local-network"></a>Osa 1 - Luo uusi paikallinen verkko

Avaa [perinteinen portal](https://manage.windowsazure.com) ja kirjaudu sisään Azure-tili.

1. Valitse vasemmassa alakulmassa näyttö, **Uusi** > **Verkkopalveluita** > **VPN** > **Lisää paikalliseen verkkoon kirjauduttaessa**.

2. Kirjoita **Määritä lähiverkossa-tiedot** -ikkunaan haluat muodostaa yhteyden RM VNet nimi. Kirjoita **VPN laitteen IP-osoite (valinnainen)** -ruutuun mikä tahansa kelvollinen julkiseen IP-osoite. Tämä on vain väliaikainen paikkamerkki. Voit muuttaa IP-osoitteen myöhemmin. Valitse ikkunan oikeassa alareunassa olevaa nuolipainiketta.
 
3. **Määritä osoitetila** -sivulla **Käynnistäminen IP** -tekstiruutuun Kirjoita verkon etuliite ja CIDR estä Resurssienhallinta-VNet haluat muodostaa yhteyden. Tätä asetusta käytetään reitittämiseen RM VNet osoitetilaa.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Osa 2 – Liitä oman VNet paikalliseen verkkoon

1. **Virtuaalinen verkkojen** sivun yläreunassa Siirry Virtual verkkojen näytön ja sitten Valitse perinteinen VNet. Valitse oman VNet-sivulla voit siirtyä määritys-sivulla **Määritä** .

2. Valitse **Muodosta yhteys paikalliseen verkkoon kirjauduttaessa** -valintaruutu, jos **sivusto sivusto connectivity** yhteys-osassa. Valitse lähiverkon, jonka loit. Jos sinulla on useita paikallisen verkkoja, jonka loit, muista valita luomasi edustavan Resurssienhallinta VNet avattavasta valikosta haluamasi vaihtoehto.

3. Valitse sivun alareunassa **Tallenna** .

### <a name="part-3---create-the-gateway"></a>Osa 3 - yhdyskäytävän luominen

1. Kun olet tallentanut asetukset, valitse **raporttinäkymät-ikkunan** sivun muuttaminen Dashboard-sivun yläreunassa. Raporttinäkymät-ikkunan sivun alaosassa **Luo yhdyskäytävä**napsauttamalla ja valitse sitten **Dynaaminen reititys**. Valitse **Kyllä** Aloita käyttämäsi yhdyskäytävän luominen. Dynaaminen reititys yhdyskäytävän vaaditaan määritysten.

2. Odota, että yhdyskäytävä on luotu. Joskus voi vaatia vähintään 45 minuuttia suorittamiseen.

### <a name="ip"></a>Osa 4: Tarkastele yhdyskäytävän julkiseen IP-osoite

Kun yhdyskäytävä on luotu, voit tarkastella yhdyskäytävän IP-osoite **Dashboard** -sivu. Tämä on käyttämäsi yhdyskäytävän julkiseen IP-osoite. Kirjoita muistiin tai kopioi julkiseen IP-osoitteeseen. Käytät sitä uudempi vaiheessa luodessasi lähiverkossa Resurssienhallinta VNet kokoonpanon.


## <a name="creatermgw"></a>Osa 2: Resurssienhallinta VNet asetusten määrittäminen

Tässä osassa on luoda Resurssienhallinta VNet VPN-yhdyskäytävän ja paikalliseen verkkoon kirjauduttaessa. Seuraavat vaiheet ennen ei alkaa, kun perinteinen VNet yhdyskäytävän julkiseen IP-osoite on noudettu.

Näyttökuvien on esitetty esimerkkejä. Varmista, arvojen korvaaminen omalla. Jos luot määritysten hioa nimellä, tarkista nämä [arvot](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Osa 1 - Luo yhdyskäytävä aliverkon

Ennen yhteyden muodostamista virtual verkon yhdyskäytävän, sinun on luoda yhdyskäytävän aliverkon, johon haluat muodostaa virtual verkossa. Luo yhdyskäytävä aliverkon CIDR Laske /28 tai suurempi (/ 27/26 jne.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Selaimessa Siirry [Azure portal](http://portal.azure.com) ja kirjaudu sisään Azure-tili.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Osa 2 – VPN Gatewayn luominen


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Osa 3 - paikalliseen verkkoon kirjauduttaessa Gatewayn luominen

Paikalliseen verkkoon kirjauduttaessa yhdyskäytävän viittaa yleensä paikallisen sijainti. Se kertoo Azure mitä IP-osoitetta alueet reitti sijainti ja laitteen kyseisessä sijainnissa julkiseen IP-osoite. Kuitenkin tässä tapauksessa se viittaa osoitealueita ja perinteinen VNet tai VPN-yhdyskäytävän liittyvää julkiseen IP-osoite.

Anna lähiverkon yhdyskäytävän nimi, jonka Azure viitata siihen. Voit luoda paikallisen yhdyskäytävien VPN-yhdyskäytävän luonnin aikana. Tässä määrityksessä, voit käyttää julkinen IP-osoite, joka on määritetty perinteinen VNet-yhdyskäytävän [edellisessä osassa](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Osa 4 - julkiseen IP-osoitteen kopioiminen

Kun VPN-yhdyskäytävä on luonut, kopioi julkiseen IP-osoitetta, joka on liitetty yhdyskäytävä. Sitä käytetään, kun määrität perinteinen VNet lähiverkon asetukset. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Osa 3: Muokata lähiverkossa perinteinen VNet

Avaa [perinteinen portal](https://manage.windowsazure.com).

2. Perinteinen-portaalissa vasemmassa reunassa alaspäin ja valitse **verkot**. Valitse **verkot** -sivulla **Paikallisen verkkojen** sivun yläreunassa. 

3. Valitse lähiverkon, jonka määritit osa 1. Valitse sivun alareunassa **Muokkaa**.

4. **Määritä lähiverkossa-tiedot** -sivulla Korvaa paikkamerkin IP-osoite, jonka loit edellisessä osassa Resurssienhallinta yhdyskäytävän julkiseen IP-osoite. Napsauta nuolta, siirry seuraavaan osioon. Tarkista, että **Osoitetilaa** ovat oikein ja valitse sitten Hyväksy muutokset valintamerkki.

## <a name="connect"></a>Osa 4: Yhteyden luominen

Tässä osassa on luoda VNets välinen yhteys. Tämän ominaisuuden vaiheet pätevät PowerShell. Et voi luoda yhteyden joko portaaleihin. Varmista, että olet ladannut ja asentanut perinteinen (k) ja resurssin Manager (RM) PowerShellin cmdlet-komennot.


1. Kirjaudu sisään Azure PowerShell console-tilin. Seuraavan cmdlet-komennon kysyy kirjautumistietosi Azure-tilillesi. Kun lokiin kirjaaminen, tilin asetukset ladataan niin, että ne ovat käytettävissä PowerShellin Azure.

        Login-AzureRmAccount 

    Saat luettelon käytettävissä Azure tilauksistasi, jos sinulla on useita tilauksia.

        Get-AzureRmSubscription

    Määritä tilaus, jota haluat käyttää. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Lisää perinteinen PowerShell cmdlet-komentojen käyttäminen Azure-tili. Voit tehdä seuraava komento:

        Add-AzureAccount

3. Määritä jaetun avaimen suorittamalla seuraava näyte. Tässä esimerkissä `-VNetName` on perinteinen VNet nimi ja `-LocalNetworkSiteName` on määrittämäsi lähiverkossa kun perinteinen portaalissa määritetty nimi. `-SharedKey` On arvo, joka voi luoda ja määrittää. Tässä määrittämäsi arvon on oltava sama arvo, joka määritetään seuraavassa vaiheessa luodessasi yhteys.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Luo VPN-yhteyden suorittamalla seuraavat komennot:
    
    **Määritä muuttujat**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Yhteyden muodostaminen**<br> Huomaa, että `-ConnectionType` on "IP", ei Vnet2Vnet. Tässä esimerkissä `-Name` on nimi, johon haluat soittaa yhteys. Seuraava esimerkki luo yhteyden nimeltä "*rm-,-perinteinen-yhteyden*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Tarkista yhteys

Voit tarkistaa yhteyden perinteinen portaalin, Azure portal tai PowerShellin avulla. Seuraavien vaiheiden avulla voit tarkistaa yhteys. Arvojen korvaaminen omalla.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet VNet usein kysytyt kysymykset

Saat lisätietoja VNet VNet yhteydet tarkastella usein kysytyt kysymykset.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


