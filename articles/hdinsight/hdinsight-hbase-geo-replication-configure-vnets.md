<properties 
   pageTitle="Kaksi virtual verkkojen välillä VPN-yhteyden määrittäminen | Microsoft Azure" 
   description="Opettele määrittämään VPN-yhteydet ja toimialuenimen selvitys kahden Azure virtual verkon, välillä ja HBase geo-replikoinnin määrittämisestä." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Kaksi Azure virtual verkkojen välillä VPN-yhteyden määrittäminen  

> [AZURE.SELECTOR]
- [Määritä VPN-yhteys](hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-asetusten määrittäminen](hdinsight-hbase-geo-replication-configure-dns.md)
- [Määrittää HBase replikoinnin.](hdinsight-hbase-geo-replication.md) 

Azure virtual sivusto sivusto verkkoyhteyden käyttää VPN-yhdyskäytävän suojatun tunnelin IP/IKE. VNets voi olla eri tilaukset ja eri alueiden osalta. Voit myös yhdistää VNet, VNet tietoliikenteen usean sivuston määrityksiä. On useita syitä, VNet VNet yhteyden:

- Toimintojen välinen alueen geo-redundanssin ja geo tavoitettavuus 
- Alueellisten monitasoisten sovellusten vahva eristystaso reunaa 
- Toimintojen väliset organisaation tietoliikenteen Azure-tilaukseen

Lisätietoja on artikkelissa [määrittäminen VNet VNet yhteyttä](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Voit avata Video:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

Tässä opetusohjelmassa kuuluu [sarjan] [ hdinsight-hbase-replication] HBase geo-replikoinnin luomisesta. 

- Määritä VPN-yhteys kahden virtual verkon (Tässä opetusohjelmassa) välillä
- [DNS-asetusten määrittäminen virtual verkoissa][hdinsight-hbase-geo-replication-dns]
- [Määrittää HBase geo replikoinnin.][hdinsight-hbase-geo-replication]

Seuraavassa kaaviossa on kuvattu kaksi virtual verkkojen luot tässä opetusohjelmassa:

![HDInsight HBase replikoinnin virtual verkkokaavio][img-vnet-diagram]
 

##<a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Työasema Azure PowerShellin avulla**.

    Ennen kuin suoritat PowerShell-komentosarjojen, varmista, että olet muodostanut yhteyden Azure tilauksen seuraavat cmdlet-komennolla:

        Add-AzureAccount

    Jos sinulla on useita tilauksia Azure, seuraavan cmdlet-komennon avulla voit määrittää voimassa oleva tilaus:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


>[AZURE.NOTE] Azure-palvelu ja virtuaalikoneen nimien on oltava yksilöllinen. Nimi, jota käytetään tässä opetusohjelmassa on Contoso-[Azure palvelun/AM nimi]-[EU / US]. Esimerkiksi Contoso-VNet-EU on Azure virtual verkon Pohjois Europe tietokeskuksen; Contoso-DNS-US on Yhdysvaltojen Itä-palvelinkeskuksen DNS-palvelin AM. Sinun täytyy tulee omia nimiä.
 

##<a name="create-two-azure-vnets"></a>Luo kaksi Azure VNets



**Voit luoda virtuaalisen verkon kutsutaan Contoso-VNet-EU Pohjois-Euroopan**

1.  Kirjautuminen [Azure perinteinen Portal][azure-portal].
2.  Valitse **Uusi**, **VERKKOPALVELUITA**, **VIRTUAL verkon** **MUKAUTETUN luominen**.
3.  Kirjoita:

    - **Nimi**: Contoso-VNet-EU
    - **Sijainti**: Pohjois Europe

        Tässä opetusohjelmassa käytetään Pohjois Eurooppa ja Yhdysvaltojen Itä palvelinkeskusten. Voit käyttää omaa palvelinkeskusten.
4.  Kirjoita:

    - **DNS-palvelin**: (jätä se tyhjäksi) 
    
        Sinun on DNS-palvelimeen nimenselvitys virtual verkkojen kuluessa. Siitä, milloin käytön antamalla Azure nimenselvitys ja käyttämään DNS-palvelimeen, katso lisätietoja [Nimi tarkkuus (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Katso ohjeet määrittäminen nimenselvitys välillä VNets [Määrittäminen DNS kaksi Azure virtual verkkojen välillä][hdinsight-hbase-dns].
  
    - **Määritä kohdan sivuston VPN**: (valitsematta)

        Pisteen sivuston ei koske Tämä skenaario.

    - **Määritä sivusto sivusto VPN**: (valitsematta)
    
        Määritetään sivuston sivuston VPN-yhteyden Azure virtual verkon Yhdysvaltojen Itä-palvelinkeskukseen.
5.  Kirjoita:

    -   **OSOITTEEN TILAA KÄYNNISTETÄÄN IP**: 10.1.0.0
    -   **OSOITTEEN TILAA CIDR**: / 16
    -   **Aliverkon 1 ALOITUS IP**: 10.1.0.0
    -   **Aliverkon 1 CIDR**: / 24

    Osoitetilaa ei ole päällekkäin US virtual verkoston kanssa.  

**Voit luoda virtuaalisen verkon kutsutaan Contoso-VNet-EU Länsi Euroopan**

- Toista edellisen menettelyn seuraavin arvoin:

    - **Nimi**: Contoso-VNet-fi
    - **Sijainti**: itä USA
     
    - **DNS-palvelin**: (jätä se tyhjäksi)
    - **Pisteen sivuston VPN-verkon määrittäminen**: (valitsematta)
    - **Määritä sivusto sivusto VPN**: (valitsematta)
     
    - **OSOITTEEN TILAA KÄYNNISTETÄÄN IP**: 10.2.0.0
    - **OSOITTEEN TILAA CIDR**: / 16
    - **Aliverkon 1 ALOITUS IP**: 10.2.0.0
    - **Aliverkon 1 CIDR**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Kahden VNets välillä VPN-yhteyden määrittäminen

###<a name="create-local-networks"></a>Luo paikalliseen verkoissa

Kun luot VNet VNet kokoonpanon, sinun täytyy kunkin VNet tunnistavan toistensa lähiverkon sivustojen määrittäminen. Tässä osassa määritettävä kunkin VNet kuin paikalliseen verkkoon. Paikallinen verkkojen jakaa saman IP-osoite välilyöntejä vastaavan VNet.

![Määritä Azure VPN-sivusto sivusto määritys - azure paikallisen verkkojen][img-vnet-lnet-diagram]


**Luo paikalliseen verkkoon kutsutaan vastaavat VNet-EU: Contoso-verkon osoitetilaa Contoso-LNet-EU**

1. Azure perinteinen-portaalista Valitse **Uusi**, **VERKKOPALVELUITA**, **VPN** **Lisääminen PAIKALLISEEN verkkoon KIRJAUDUTTAESSA**.
3. Kirjoita:

    - **Nimi**: Contoso-LNet-EU
    - **VPN-laitteen IP-osoite**: 192.168.0.1 (osoitteen päivitetään myöhemmin)

        Yleensä vakiomittoja VPN-laitteen todellinen ulkoinen IP-osoite. VNet VNet määritykset, käytät VPN yhdyskäytävän IP-osoite. Koska, että et ole luonut VPN-yhdyskäytäviä, kaksi VNets vielä, kirjoita arbitary IP-osoite ja palaa ratkaisemaan ongelman.
4.  Kirjoita:

    - **Osoite TILAA KÄYNNISTETÄÄN IP:** 10.1.0.0
    - **OSOITTEEN TILAA CIDR:** /16
    
    Tämä on vastattava täsmälleen alue, joka sisältää määritetyn aiemmin Contoso-VNet-EU.

**Luo paikalliseen verkkoon kutsutaan vastaavat Contoso-VNet-fi-verkkoon osoitetilaa Contoso-LNet-fi**

- Toista edellisen menettelyn seuraavilla parametreilla:

    - **Nimi**: Contoso-LNet-fi
    - **VPN-laitteen IP-osoite**: 192.168.0.1 (osoitteen päivitetään myöhemmin)
     
    - **OSOITTEEN TILAA KÄYNNISTETÄÄN IP**: 10.2.0.0
    - **OSOITTEEN TILAA CIDR**: / 16


###<a name="create-vpn-gateways"></a>Luo VPN yhdyskäytävät

Tässä määrityksessä on kaksi osaa. Ensin lähiverkossa VNet sivusto yhteyden määrittäminen ja luo sitten dynaaminen reititys VPN-yhteyttä. VNet VNet edellyttää Azure VPN yhdyskäytävien ja dynaaminen reititys VPN-yhteydet. Azure staattinen reititys VPN-yhteydet eivät ole tuettuja.

**Contoso-LNet-US, Contoso-VNet-EU sivusto sivusto-yhteyden määrittäminen**

1.  Valitse perinteinen Azure-portaalissa vasemmanpuoleisessa ruudussa **verkot**
2.  Valitse **Contoso-VNet-EU**.
3.  Valitse **CONFIGUE** -välilehti.
4.  Valitse **Muodosta yhteys paikalliseen verkkoon kirjauduttaessa**.
5.  Valitse **LÄHIVERKON** **Contoso-LNet-US**.
6.  Valitse **Lisää yhdyskäytävän aliverkon** välilyöntejä virtual verkko-osan.
7.  Valitse **Tallenna**.
8.  Vahvista valitsemalla **OK** .


**Jos haluat luoda VPN-yhdyskäytävän Contoso-VNet-EU**

1.  Valitse perinteinen Azure-portaalissa **KOONTINÄYTTÖ** -välilehti.
4.  Valitse **Luo yhdyskäytävä** sivulla ja valitse sitten **Dynaaminen reititys**.
5.  Vahvista valitsemalla **Kyllä** . Muokkaa yhdyskäytävän grafiikan sivulla keltainen muutoksista sekä lukee luominen yhdyskäytävän. Yleensä kestää noin 15 minuuttia, voit luoda yhdyskäytävän.

    Yhdyskäytävän tila muuttuu yhteyden, kun kukin Gatewayn IP-osoite on näkyvissä koontinäytön. Kirjoita muistiin IP-osoitetta, joka vastaa, kunkin VNet varoen sekoita niitä. Nämä ovat IP-osoitteet, joita käytetään muokattaessa paikkamerkin IP-osoitteiden paikallisen verkoissa VPN-laitetta varten.

6.  Tallenna kopio **YHDYSKÄYTÄVÄN IP-osoite**. Sitä käytetään Contoso-VNet-EU seuraavan osion VPN yhdyskäytävän IP-osoitteen määrittäminen.

**Jos haluat luoda VPN-yhdyskäytävän Contoso-VNet-EU**

- Toista nämä kaksi vaiheet Contoso-LNet-EU ja luo VPN-yhdyskäytävän Contoso-VNet-US sivusto sivusto-yhteyden määrittäminen Contoso-Vnet-US. Kun olet valmis, käytössäsi on VPN yhdyskäytävän IP-osoite, Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>VPN-laitteen paikallisen verkkojen IP-osoitteiden määrittäminen
Viimeisessä osassa Luo VPN-yhdyskäytävän kullekin VNets. Olet saanut IP-osoitteiden VPN-yhdyskäytävää. Nyt voit siirtyä takaisin paikalliseen verkkoon kirjauduttaessa VPN laitteen IP-osoitteiden määrittäminen.

**Contoso-LNet-EU VPN laitteen IP-osoitteen määrittäminen** 

1.  Azure perinteinen-portaalista Valitse vasemmanpuoleisessa ruudussa **verkot** .
2.  Valitse **Paikallinen VERKKOJEN** yläreunassa.
3.  Valitse **Contoso-LNet-EU**ja valitse sitten alareunassa **Muokkaa** .
4.  Päivitä **VPN laitteen IP-osoite**.  Tämä on sähköpostiosoite, sinulla on Contoso-VNET-EU KOONTINÄYTTÖ-välilehti.
5.  Napsauta hiiren kakkospainikkeella.
6.  Valitse Tarkista-painiketta.

**Contoso-LNet-US VPN laitteen IP-osoitteen määrittäminen** 

- Toista edellisen menettelyn VPN laitteen IP-osoitteen määrittäminen Contoso-LNet-US.

###<a name="set-vnet-gateway-keys"></a>Määritä VNet yhdyskäytävän näppäimet

Vnet yhdyskäytävien todentaa virtual verkkojen välisten yhteyksien jaetun avaimen avulla. Avain ei voi määrittää perinteinen Azure-portaalista. Sinun on käytettävä PowerShell tai .NET SDK.

**Voit määrittää näppäimet**

1. Avaa **Windows PowerShell ise:** - tai Windows PowerShell console-työaseman.
2. Päivitä seuraa-komentosarjan parametrit ja suorittaa sen:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Tarkista VPN-yhteys 

Ilman mitään VMs VNets käyttöön voit käyttää virtual visual verkkokaavion VNet raporttinäkymäsivu perinteinen Azure-portaalissa yhteyden tilan:

![HDInsight HBase replikoinnin virtual verkon VPN-yhteyden tila][img-vpn-status]
  



##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on opit kaksi Azure virtual verkkojen välillä VPN-yhteyden määrittämisestä. Muiden kaksi artikkelit sarjaan kuuluvat:

- [DNS-asetusten määrittäminen kahden Azure virtual verkon välillä][hdinsight-hbase-geo-replication-dns]
- [Määrittää HBase geo replikoinnin.][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
