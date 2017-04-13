<properties
   pageTitle="Luo VNet Peering Azure-portaalissa | Microsoft Azure"
   description="Opettele luomaan virtual verkon Azure portaalin resurssien hallinnan avulla."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Luo VPN-peering Azure-portaalissa

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Jos haluat luoda VNet-peering skenaarion perusteella Azure-portaalissa, noudata seuraavia ohjeita.

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Muodostaa VNET peering sinun on luotava kaksi linkkiä, yksi jokaista suuntaa kahden VNets välillä. Voit luoda VNET VNET1 VNET2 peering linkki ensin. Portaalin, valitse **Selaa** > **Valitse Virtual verkot**

    ![Luo VNet peering Azure-portaalissa](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. Virtuaalinen verkkojen sivu, valitse Valitse VNET1, Peerings ja sitten Lisää

    ![Valitse peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. Lisää Peering-sivu on peering linkin nimi LinkToVnet2, valitse tilaus ja peer Virtual verkon VNET2, valitse OK.

    ![Linkki VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Kun tämä VNET peering linkki on luotu. Näet Linkkitilan seuraavalla tavalla:

    ![Linkin tilan](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Luo seuraavaksi VNET2 VNET1 VNET peering linkkiä. Virtuaalinen verkkojen sivu, valitse Valitse VNET2, Peerings ja sitten Lisää

    ![Vertaisjärjestelmä kuin muut VNet](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. Lisää Peering, sivu-peering linkin nimi LinkToVnet1, valitse tilaus ja peer Virtual verkkoon, valitse OK.

    ![Luominen VPN-ruutu](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Kun tämä VNET peering linkki on luotu. Näet Linkkitilan seuraavalla tavalla:

    ![Lopullinen Linkkitilan](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Tarkista tila LinkToVnet2 ja se muuttuu yhdistetty sekä.  

    ![Lopullinen Linkkitilan 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

    > [AZURE.NOTE] VNET peering määritetään vain, jos molemmat linkkejä.

Liittyy muutama määritettäviä ominaisuuksia kunkin linkin:

|Vaihtoehto|Kuvaus|Oletusarvo|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Onko verkko-osoitteissa Peer VNet Virtual_network tunnisteen sisällyttää tila|Kyllä|
|AllowForwardedTraffic|Onko liikenne ei peräisin peered VNet hyväksytään tai poistetaan|Ei|
|AllowGatewayTransit|Vertaisjärjestelmä VNet VNet yhdyskäytävän käyttämään avulla|Ei|
|UseRemoteGateways|Käytä omaa peer VNet yhdyskäytävää. Vertaisjärjestelmä VNet on oltava määritettynä yhdyskäytävän ja AllowGatewayTransit on valittuna. Et voi käyttää tämä vaihtoehto, jos sinulla on määritetty yhdyskäytävän|Ei|

Kunkin linkkiä VNet peering on joukko yläpuolella ominaisuudet. Työkaluportaaliin ja voit linkkiä VNet Peering ja muuttaa kaikki käytettävissä olevat vaihtoehdot, napsauta Tallenna, jos haluat tehdä muutoksen vaikutus.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2. Tässä esimerkissä Käytämme kaksi tilaukset A ja B kaksi käyttäjää UserA ja UserB oikeuksilla tilausten tarpeen mukaan
3. Portaalissa, valitse Selaa, valitse Virtual verkot. Valitse VNET ja sitten Lisää.

    ![Käyttötilanne 2 Selaa-painike](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. Valitse Lisää access-sivu, valitse rooli ja valitse verkon osallistuja, Lisää käyttäjiä, kirjoita UserB Kirjaudu nimi ja valitse OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Tämä ei ole tarpeen, peering voi muodostaa, vaikka käyttäjien Nosta yksitellen peering pyynnöt niiden vastaaviin Vnets, kunhan pyynnöt vastaa. Muut VNet sellaisten käyttäjien lisääminen paikalliseen VNet käyttäjinä määritys-portaalissa on helpompaa.

5. Valitse Kirjaudu UserB, joilla on oikeus käyttäjää SubscriptionB Azure-portaaliin. Noudata edellä ohjeita voit lisätä UserA verkon avustaja.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    > [AZURE.NOTE] Voit kirjautua ulos ja kirjaudu sisään sekä käyttäjäistuntojen selaimessa, jotta luvan on otettu käyttöön onnistuneesti.

6. Kirjaudu sisään käyttäjänä UserA-portaaliin, siirry VNET3-sivu, valitse Peering, tarkista "voin selvittää my Resurssitunnus" valintaruutu ja kirjoita resurssin tunnus VNET5 muoto.

    / subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}

    ![Resurssitunnus](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Kirjaudu sisään UserB ja noudata edellä vaiheessa peering linkin luominen VNET5 VNet3-portaaliin.

    ![Resurssitunnus 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Peering muodostetaan ja Virtual-konetta VNet3 pitäisi vaihtamaan VNet5 virtual minkä tahansa tietokoneen kanssa

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Ensimmäisessä vaiheessa, VNET peering linkit HubVnet VNET1. Huomaa, että Salli välittää liikenne-vaihtoehto ei ole valittuna linkin.

    ![Perustietoja Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Seuraavassa vaiheessa voi luoda HubVnet VNET1 peering linkkejä. Huomaa, että Salli välittää liikenne-vaihtoehto on valittuna.

    ![Perustietoja Peering](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Kun peering on muodostettu, voit tämän [artikkelin](virtual-network-create-udr-arm-ps.md) ja määritä käyttäjän määrittämät Route(UDR) uudelleenohjaaminen VNet1 liikenne kautta virtual laitteen, voit käyttää sen ominaisuuksia. Määrittäessäsi seuraava siirräntävälien osoite reitin voit määrittää sen peer VNet HubVNet virtual laitteen IP-osoite


[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]



1. Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.

2. Tässä skenaariossa peering VNET muodostaa haluat luoda vain yksi linkki Azure Resurssienhallinta perinteinen tekemääsi virtual verkosta. Ei- **VNET1** **VNET2**avulla. Portaalin, valitse **Selaa** > Valitse **Virtual verkot**

3. Valitse **VNET1**Virtual verkot-sivu. Valitse **Peerings**ja valitse sitten **Lisää**.

4. Nimi linkin lisääminen Peering-sivu. Tätä kutsutaan **LinkToVNet2**. Vertaisjärjestelmä tiedot-kohdassa Valitse **perinteinen**.

5. Valitse tilaus ja vertaisjärjestelmä VPN- **VNET2**. Valitse OK.

    ![Linkittäminen Vnet1 Vnet 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)

6. Kun tämä VNet peering linkki on luotu, kaksi virtual verkot ovat peered ja osaat on seuraavissa artikkeleissa:

    ![Peering yhteyden tarkistaminen](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)


## <a name="remove-vnet-peering"></a>Poista VNet Peering

1.  Siirry selaimella, http://portal.azure.com ja Kirjaudu tarvittaessa sisään Azure-tili.
2.  Siirry VPN-sivu, valitse Peerings napsauttamalla linkkiä, jonka haluat poistaa, napsauta Poista-painiketta.

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Kun haluat poistaa linkin VNET peering, peer Linkkitilan siirtyvät yhteys katkaistu.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. Tässä tilassa ei voi luoda linkin uudelleen peer Linkkitilan muuttuu Mahdollisuuden löytäjä. On suositeltavaa poistaa sekä linkit, ennen kuin voit luoda uudelleen VNET peering.
