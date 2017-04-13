<properties 
   pageTitle="DNS-asetusten määrittäminen kahden Azure virtual verkon välillä | Microsoft Azure" 
   description="Opettele määrittämään VPN-yhteydet ja toimialuenimen selvitys kahden virtual verkon välillä ja HBase geo-replikoinnin määrittämisestä." 
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

# <a name="configure-dns-between-two-azure-virtual-networks"></a>DNS-asetusten määrittäminen kahden Azure virtual verkon välillä

> [AZURE.SELECTOR]
- [Määritä VPN-yhteys](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-asetusten määrittäminen](hdinsight-hbase-geo-replication-configure-dns.md)
- [Määrittää HBase replikoinnin.](hdinsight-hbase-geo-replication.md) 


Lue, miten voit lisätä ja määrittää DNS-palvelimet Azure virtual verkkoihin käsittelemään nimenselvitys sisällä ja virtual verkkojen.

Tässä opetusohjelmassa on [sarjan] toinen osa[ hdinsight-hbase-geo-replication] HBase geo-replikoinnin luominen:

- [Määritä VPN-yhteys kahden virtual verkon välillä][hdinsight-hbase-geo-replication-vnet]
- DNS-asetusten määrittäminen virtual verkkojen (Tässä opetusohjelmassa)
- [Määrittää HBase geo replikoinnin.][hdinsight-hbase-geo-replication]


Seuraavassa kaaviossa on kuvattu kaksi virtual verkkojen luomasi määrittäminen [VPN-yhteys kahden virtual verkon välillä][hdinsight-hbase-geo-replication-vnet]:

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

- **Kaksi Azure virtual verkostoa VPN-yhteys**.  Katso ohjeet [kahden Azure virtual verkon välillä VPN-yhteyden määrittäminen][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Azure-palvelu ja virtuaalikoneen nimien on oltava yksilöllinen. Nimi, jota käytetään tässä opetusohjelmassa on Contoso-[Azure palvelun/AM nimi]-[EU / US]. Esimerkiksi Contoso-VNet-EU on Azure virtual verkon Pohjois Europe tietokeskuksen; Contoso-DNS-US on DNS-palvelin Yhdysvaltojen Itä tietokeskuksen AM. Sinun täytyy tulee omia nimiä.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Azure-virtuaalikoneissa, jota käytetään DNS-palvelimet luominen

**Voit luoda virtual koneen sisällä Contoso-VNet-EU kutsutaan Contoso-DNS-EU**

1.  Valitse **Uusi**, **Laske**, **VIRTUAALIKONEEN**, **Mistä VALIKOIMA**.
2.  Valitse **Windows Server 2012 R2-Palvelinkeskukseen**.
3.  Kirjoita:
    - **VIRTUAALIKONEEN nimi**: Contoso-DNS-EU
    - **Uuden käyttäjänimi**: 
    - **Uusi salasana**: 
4.  Kirjoita:
    - **PILVIPALVELUSSA**: Luo uusi pilvipalvelussa
    - **Alue/AFFINITEETTI ryhmän/VIRTUAL verkon**: (Valitse Contoso-VNet-EU)
    - **VIRTUAL verkon ALIVERKOSTA**: aliverkon 1
    - **TALLENNUSTILAN tilin**: automaattisesti luotu tallennustilan tilillä
    
        Cloud palvelunimi on sama kuin virtuaalikoneen nimi. Tässä tapauksessa on Contoso-DNS-EU. Seuraavien näennäiskoneiden voit valita käyttää samaa pilvipalvelussa.  Kaikki saman pilvipalvelussa kohdassa näennäiskoneiden jakaa saman virtual verkko- ja jälkiliitteen.

        Tallennustilan tilin käytetään virtuaalikoneen kuvatiedoston tallennuspaikka. 
    - **PÄÄTEPISTEET**: (alaspäin ja valitse **DNS**) 

Kun virtuaalikoneen on luotu, katso, sisäisen IP- ja ulkoisen IP.

1.  Napsauta virtuaalikoneen nimeä, **Contoso-DNS-EU**.
2.  Valitse **koontinäyttö**.
3.  Kirjoita muistiin:
    - JULKINEN VIRTUAL IP-OSOITE
    - SISÄINEN IP-OSOITE


**Voit luoda virtual koneen sisällä Contoso-VNet-US kutsutaan Contoso-DNS-fi** 

- Toista sama menettely, voit luoda virtual tietokoneeseen seuraavin arvoin:
    - VIRTUAALIKONEEN nimi: Contoso-DNS-fi
    - ALUE/AFFINITEETTI ryhmän/VIRTUAL verkon: Valitse Contoso-VNET-fi
    - VPN-ALIVERKOSTA: Aliverkon-1
    - TALLENNUSTILAN tilin: Käytä automaattisesti luotu tallennustilan-tiliä
    - PÄÄTEPISTEET: (Valitse DNS)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Kaksi näennäiskoneiden staattinen IP-osoitteiden määrittäminen

DNS-palvelimet edellyttää staattinen IP-osoitteet.  Tämä vaihe ei voi tehdä perinteinen Azure-portaalista. Voit käyttää PowerShellin Azure.

**Voit määrittää kahden näennäiskoneiden staattinen IP-osoite**

1. Avaa Windows PowerShell ise:.
2. Suorita seuraavat cmdlet-komennot.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Palvelun nimi on cloud palvelunimi. Koska DNS-palvelin on ensimmäinen virtuaalikoneen cloud-palvelun, cloud palvelunimi on sama kuin virtuaalikoneen nimi.

    Voit joutua päivittämään palvelun nimi ja nimen nimet, jotka sinulla on.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Lisää DNS-palvelin-roolin kahden näennäiskoneiden

**Lisää DNS-palvelin-roolin Contoso-DNS-EU**

1.  Valitse **näennäiskoneiden** Azure perinteinen-portaalista vasemmalla puolella. 
2.  Valitse **Contoso-DNS-EU**.
3.  Valitse **raporttinäkymät-IKKUNAN** yläreunassa.
4.  Valitse **Yhdistä** ala- ja noudata ohjeita muodostaa virtuaalikoneen RDP kautta.
2.  Sisällä RDP-istunnon napsauta Avaa aloitusnäytön vasemmassa alakulmassa Windows-painiketta.
3.  Valitse **Palvelimen hallinta** -ruutu.
4.  Valitse **Lisää rooleja ja ominaisuuksia**.
5.  Valitse **Seuraava**
6.  Valitse **Roolipohjainen tai ominaisuuksiin perustuvaan asennus**ja valitse sitten **Seuraava**.
7.  Valitse DNS virtuaalikoneen (se on korostettuna jo) ja valitse sitten **Seuraava**.
8.  Valitse **DNS-palvelimeen**.
9.  Valitse **Lisää ominaisuuksia**ja valitse sitten **Jatka**.
10. Valitse **Seuraava** kolme kertaa ja valitse sitten **Asenna**. 

**Lisää DNS-palvelin-roolin Contoso-DNS-fi**

- Toista vaihe vaiheelta, miten DNS-roolin lisääminen **Contoso-DNS-US**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Määritä DNS-palvelimet virtual verkkoihin

**Voit rekisteröidä kaksi DNS-palvelimet**

1.  Azure perinteinen-portaalista Valitse **Uusi**, **VERKKOPALVELUITA**, **VPN** **Rekisteröi DNS-palvelimeen**.
2.  Kirjoita:
    - **Nimi**: Contoso-DNS-EU
    - **DNS-palvelimen IP-osoite**: 10.1.0.4 – IP-osoitteeseen on vastaavat DNS server virtuaalikoneen IP-osoite.
     
3.  Toista Contoso-DNS-US rekisteröidä seuraavat asetukset:
    - **Nimi**: Contoso-DNS-fi
    - **DNS-palvelimen IP-osoite**: 10.2.0.4

**Voit määrittää kaksi DNS-palvelimet kaksi virtual verkkoihin**

1.  Valitse Valitse perinteinen portaalin vasemmanpuoleisessa ruudussa **verkot** .
2.  Valitse **Contoso-VNet-EU**.
3.  Valitse **Määritä**.
4.  Valitse **Contoso-DNS-EU: N** **dns-palvelimet** -osiosta.
5.  Valitse sivun alareunassa **Tallenna** ja Vahvista valitsemalla **Kyllä** .
6.  Toista **Contoso-DNS-US** DNS-palvelimen määrittäminen virtual **Contoso-VNet-fi** -verkkoon.

Kaikki, jotka on otettu käyttöön virtual verkkoihin näennäiskoneiden on käynnistettävä päivittää DNS server-määritys.

**Näennäiskoneiden käynnistettävä uudelleen**

1. Valitse **näennäiskoneiden** Azure perinteinen-portaalista vasemmalla puolella.
2. Valitse **Contoso-DNS-EU**.
3. Valitse **raporttinäkymät-ikkunan** yläreunassa.
4. Valitse **Käynnistä** alareunassa.
5. Toista samat vaiheet, Käynnistä uudelleen **Contoso-DNS-US**.


##<a name="configure-dns-conditional-forwarders"></a>Määritä ehdollinen DNS-välittäjiä

Kunkin virtual verkon DNS-palvelimen ratkaista vain virtual verkon DNS nimet. Sinun täytyy määrittäminen ehdollisen forwarder osoittamaan nimi ratkaisut peer virtual verkon peer DNS-palvelimeen.

Ehdollinen forwarder määrittämiseen, sinun tarvitsee tietää kaksi DNS-palvelimien toimialueen loppuliitteet. DNS-liitteet voivat olla erilaisia käytit, kun olet luonut näennäiskoneiden pilvipalveluihin määritysten mukaan. DNS-liite kunkin käyttää virtual verkossa sinun on lisättävä ehdollinen forwarder. 

**Löydät kaksi DNS-palvelimet toimialueen liitteitä**

1. RDP **Contoso-DNS-EU**kyselyjä.
2. Avaa Windows PowerShell console tai komentokehote.
3. Suorita **ipconfig**ja kirjoita se muistiin **yhteyteen liittyvän DNS-liite**.
4. Sulje RDP-istunnon, tarvitset sitä vielä. 
5. Toista samat vaiheet kieliominaisuuksien **yhteyteen liittyvän DNS-liite** **Contoso-DNS-US**.


**Voit määrittää DNS-välittäjiä**
 
1.  Valitse **Contoso-DNS-EU**RDP-istunnon vasemmassa alakulmassa Windows-näppäintä.
2.  Valitse **Valvontatyökalut**.
3.  Valitse **DNS**.
4.  Laajenna vasemmanpuoleisessa ruudussa **DSN**, **Contoso-DNS-EU**.
5.  Anna seuraavat tiedot:
    - **DNS-toimialue**: Määritä DNS-liitteen Contoso-DNS-US. Esimerkki: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-osoitteet, master-palvelimet**: Kirjoita 10.2.0.4, joka on Contoso-DNS-USA käyttäjän IP-osoite.
6.  Paina **ENTER**-näppäintä ja valitse sitten **OK**.  Voit nyt voi ratkaista Contoso-DNS-EU Contoso-DNS-USA käyttäjän osoite.
7.  Toista vaihe vaiheelta, miten DNS-forwarder lisääminen Contoso-DNS-US virtuaalikoneen seuraavin arvoin DNS-palvelu:
    - **DNS-toimialue**: Kirjoita Contoso-DNS-EU: n DNS-liitettä. 
    - **IP-osoitteet, master-palvelimet**: Kirjoita 10.2.0.4, joka on Contoso-DNS-EU: n IP-osoite.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testaa nimenselvitys virtual verkkojen

Nyt voit testata host nimenselvitys virtual verkkojen. Palomuuri estää ping oletusarvoisesti.  Voit ratkaista DNS server näennäiskoneiden (sinun on käytettävä FQDN) peer-verkoissa nslookup.  


##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on opit määrittäminen nimenselvitys virtual verkkojen VPN-yhteyttä. Muiden kaksi artikkelit sarjaan kuuluvat:

- [Kaksi Azure virtual verkkojen välillä VPN-yhteyden määrittäminen][hdinsight-hbase-geo-replication-vnet]
- [Määrittää HBase geo replikoinnin.][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
