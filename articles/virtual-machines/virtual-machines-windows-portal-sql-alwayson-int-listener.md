<properties
   pageTitle="Luo Listener AlwaysOn availabilty ryhmän SQL Serverin Azuren näennäiskoneiden"
   description="Vaiheittaiset ohjeet SQL Serverin Azuren näennäiskoneiden kuuntelija AlwaysOn availabilty ryhmän luomista varten"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Määritä sisäisen kuormituksen AlwaysOn käytettävyys ryhmän Azure

Tässä ohjeaiheessa kerrotaan, miten resurssien hallinnan malli Azuren näennäiskoneiden SQL Server AlwaysOn käytettävyys ryhmän sisäinen kuormituksen luominen. AlwaysOn-käytettävyys ryhmän edellyttää kuormituksen tasauspalvelun, kun Azuren näennäiskoneiden SQL Server-esiintymien ovat. Kuormituksen tallentaa käytettävyys ryhmän kuuntelua IP-osoite. Jos käytettävyys-ryhmän laajuinen käyttämään alueet, kunkin alueen tarvitsee kuormituksen tasauspalvelun.

Tämän tehtävän suorittamiseen tarvitset on otettu käyttöön resurssien hallinnan malli Azuren näennäiskoneiden SQL Server AlwaysOn käytettävyys-ryhmä. SQL Server sekä näennäiskoneiden on kuuluttava käytettävyys samat. Voit käyttää [Microsoft-mallien](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) AlwaysOn käytettävyys-ryhmän luominen automaattisesti Azure Resurssienhallinta. Tämä malli luo sisäinen kuormituksen automaattisesti puolestasi. 

Halutessasi voit [manuaalisesta aiemmin AlwaysOn käytettävyys-ryhmään](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Tässä ohjeaiheessa edellyttää, että laiduntamismahdollisuuksien ryhmien on jo määritetty.  

Aiheeseen liittyvät artikkelit ovat seuraavat:

 - [Määritä AlwaysOn käytettävyys ryhmät Azure AM (Graafisen)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [VNet VNet yhteysasetusten Azure Resurssienhallinta ja PowerShellin avulla](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Vaiheet

Kävelemällä – tämä asiakirja luo ja Määritä kuormituksen Azure-portaalissa. Joka on valmis, kun määrität klusterin IP-osoite-kuormituksen käytettävät AlwaysOn käytettävyys ryhmän kuuntelua.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Luo ja Määritä kuormituksen Azure-portaalissa

Tämän tehtävän osan käyt Azure-portaalissa seuraavasti:

1. Luo kuormituksen ja Määritä IP-osoite

1. Määritä Taustajärjestelmä resurssivarantoon

1. Luo näytteenottimen 

1. Kuormituksen sääntöjen määrittäminen

>[AZURE.NOTE] Jos SQL-palvelimien ovat eri resurssiryhmiä ja alueiden osalta, käyt kaikki nämä vaiheet kahdesti, kerran kunkin resurssi-ryhmässä.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. luominen kuormituksen ja IP-osoitteen määrittäminen

Ensimmäiseksi on luotava kuormituksen. Avaa resurssiryhmä, joka sisältää SQL Server-näennäiskoneiden Azure-portaalissa. Valitse resurssi-ryhmästä **Lisää**.

- Etsi **kuormituksen**. Valitse hakutuloksista **Kuormituksen**, jotka on julkaistu **Microsoft**.

- Valitse **Kuormituksen** -sivu valitsemalla **Luo**.

- **Luo kuormituksen**, Määritä kuormituksen seuraavasti:

| Asetus | Arvo |
| ----- | ----- |
| **Nimi** | Tekstin nimi, joka edustaa kuormituksen. Esimerkiksi **sqlLB**. |
| **Rakenne** | **Sisäinen** |
| **VPN** | Valitse virtual verkko, jotka ovat SQL-palvelimiin.   |
| **Aliverkon**  | Valitse SQL-palvelimien olevat aliverkon. |
| **Tilauksen** | Jos sinulla on useita tilauksia, saattavat näkyä tähän kenttään. Valitse tilaus, jonka haluat resurssiin liittyvien. Se on yleensä sama subcription kaikista resurssien käytettävyyden ryhmän.  |
| **Resurssiryhmä** | Valitse SQL-palvelimien olevat resurssiryhmä. | 
| **Sijainti** | Valitse Azure SQL-palvelimien olevien sijainti. |

- Valitse **Luo**. 

Azure Luo kuormituksen, jonka määritit yläpuolella. Kuormituksen kuuluu tiettyyn verkkoon, aliverkon, resurssiryhmä ja sijainti. Kun Azure on valmis, tarkista kuormituksen tasauspalvelun asetusten Azure-tietokannassa. 

Nyt voit määrittää kuormituksen tasauspalvelun IP-osoite.  

- Valitse kuormituksen tasauspalvelun **asetusten** sivu- **IP-osoite**. **IP-osoite** -sivu näyttää, että kyseessä on yksityinen kuormituksen saman virtual verkossa kuin SQL-palvelimet. 

- Määritä seuraavat asetukset: 

| Asetus | Arvo |
| ----- | ----- |
| **Aliverkon** | Valitse SQL-palvelimien olevat aliverkon. |
| **Tehtävän** | **Staattinen** |
| **IP-osoite** | Kirjoita käyttämättömät virtual IP-osoite aliverkosta.  |

- Tallenna asetukset.

Kuormituksen on nyt IP-osoite. Tallenna tämä IP-osoite. Voit käyttää IP-osoitteen luodessasi kuuntelija klusterin. Tämän artikkelin PowerShell-komentosarja, käytä tämän osoite `$ILBIP` muuttuja.



## <a name="2-configure-the-backend-pool"></a>2. Määritä Taustajärjestelmä resurssivarantoon

Seuraava vaihe on luoda Taustajärjestelmä osoitevarannon. Azure puhelujen Taustajärjestelmä osoite resurssivarantoon *Taustajärjestelmä resurssivarantoon*. Tässä tapauksessa Taustajärjestelmä varanto on kaksi SQL-palvelinten käytettävyys-ryhmässä osoitteet. 

- Valitse resurssi-ryhmässä luomasi kuormituksen. 

- Valitse **asetukset**, **Taustajärjestelmä jakavat**.

- Valitse **Taustajärjestelmä osoite jakavat**, **Lisää** Taustajärjestelmä osoite resurssivarantoon luomiseen. 

- Kirjoita **nimi**-kohdassa **Lisää Taustajärjestelmä resurssivarantoon** Taustajärjestelmä ryhmän nimi.

- Valitse **näennäiskoneiden** **+ Lisää virtual machine**. 

- Valitse **Valitse näennäiskoneiden** **käytettävyys Määritä** ja määritä laiduntamismahdollisuuksien määrittää, että SQL Server-näennäiskoneiden kuuluvat.

- Kun olet valinnut Määritä käytettävyys, valitse **Valitse näennäiskoneiden**. Valitse kaksi näennäiskoneiden, joka isännöi SQL Server-esiintymien käytettävyys-ryhmässä. Valitse **Valitse**. 

- Valitse **OK** ja sulje näiden **Valitse näennäiskoneiden**ja **Lisää Taustajärjestelmä resurssivarantoon**. 

Azure päivittää Taustajärjestelmä osoite-ryhmän asetuksia. Käytettävyys määrittäminen on nyt kaksi SQL-palvelimet resurssivarantoon.

## <a name="3-create-a-probe"></a>3. näytteenottimen luominen

Seuraavaksi näytteenottimen luomiseen. Näytteenottimen määrittää, miten Azure tarkistaa joka SQL-palvelimien tällä hetkellä omistaa käytettävyys ryhmän kuuntelua. Azure tutkia palvelun IP-osoitetta, joka määritetään, kun luot näytteenottimen portin perusteella.

- Valitse kuormituksen tasauspalvelun **asetusten** sivu- **Probes**. 

- Valitse **Lisää** **Probes** -sivu.

- Määritä näytteenottimen **Lisää näytteenottimen** -sivu. Voit määrittää näytteenottimen käyttämällä seuraavat arvot:

| Asetus | Arvo |
| ----- | ----- |
| **Nimi** | Näytteenottimen edustava teksti nimi. Esimerkiksi **SQLAlwaysOnEndPointProbe**. |
| **Protokolla** | **TCP** |
| **Port (portti)** | Voit käyttää mitä tahansa porttiin. Esimerkiksi *59999*.    |
| **Väli**  | *5* | 
| **Perusasemassa kynnysarvo**  | *2* | 

- Valitse **OK**. 

>[AZURE.NOTE] Varmista, että määrität portti on auki sekä SQL-palvelimien palomuurin. Molempien palvelinten vaadi saapuvan liikenteen sääntö, jota käytetään porttinumeroa. Lisätietoja on kohdassa [Lisää tai Muokkaa palomuurisääntöä](http://technet.microsoft.com/library/cc753558.aspx) . 

Azure Luo näytteenottimen. Azure käyttää näytteenottimen Testaa mitä SQL Server on kuuntelua käytettävyys-ryhmän.

## <a name="4-set-the-load-balancing-rules"></a>4. kuormituksen sääntöjen määrittäminen

Määrittää kuormituksen säännöt. Kuormituksen Vastatilin sääntöjä määrittää, miten kuormituksen reitittää liikenteen SQL-palvelimien. Tämä kuormituksen otetaan käyttöön suoraan palvelimen Palauta, koska vain yksi, kaksi SQL-palvelimien koskaan omistaa resurssin käytettävyys ryhmän listener kerrallaan.

- Kuormituksen tasauspalvelun **asetukset** -sivu valitsemalla **Lataa Vastatilin säännöt**. 

- Valitse **Lisää** **kuormituksen tasaamisen säännöt** -sivu.

- Voit määrittää säännön kuormituksen käyttämällä **Lisää ladata Vastatilin säännöt** -sivu. Käytä seuraavia asetuksia: 

| Asetus | Arvo |
| ----- | ----- |
| **Nimi** | Sääntöjen kuormituksen edustava teksti nimi. Esimerkiksi **SQLAlwaysOnEndPointListener**. |
| **Protokolla** | **TCP** |
| **Port (portti)** | *1433*   |
| **Taustajärjestelmä portti** | *1433*. Huomaa, että tämä poistetaan käytöstä, sillä tätä sääntöä käytetään **Irrallinen IP (Palauta suoraan server)**.   |
| **Tutkia** | Käyttää nimeä, jonka loit näytteenottimen tämän kuormituksen. |
| **Istunnon viljelykasvissa**  | **Ei mitään** | 
| **Aikakatkaisun (minuutteina)**  | *4* | 
| **Irrallinen IP (Palauta suoraan server)**  | **Käytössä** | 

 >[AZURE.NOTE] Joudut ehkä vierittämään-sivu, saat näkyviin kaikki asetukset.

- Valitse **OK**. 

- Azure määrittää kuormituksen säännön. Kuormituksen on nyt määritetty reitti-liikenne paikalliseen SQL Server, joka isännöi kuuntelua käytettävyys-ryhmän. 

Tässä vaiheessa resurssiryhmän on kuormituksen, yhteyden muodostaminen sekä SQL Server-tietokoneissa. Kuormituksen on myös IP-osoite SQL Server AlwaysOn laiduntamismahdollisuuksien ryhmän listener niin, että kummassakin machine voit vastata käytettävyys-ryhmiin.

>[AZURE.NOTE] Jos SQL-palvelimet on kaksi eri alueilla, toista vaiheet muissa alueen. Kunkin alueen edellyttää kuormituksen tasauspalvelun. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Määritä klusteri käyttämään kuormituksen tasauspalvelun IP-osoite 

Seuraavaksi kuuntelua klusterin ja tuoda kuuntelua verkossa. Voit tehdä tämän seuraavasti: 

1. Luo automaattisesti klusterin laiduntamismahdollisuuksien ryhmän kuuntelutoiminto 

1. Tuo kuuntelua verkossa

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. laiduntamismahdollisuuksien ryhmän kuuntelijaa luoda automaattisesti klusterin

Tässä vaiheessa voit luoda manuaalisesti käytettävyys ryhmän kuuntelua automaattisesti klusterin hallinta ja SQL Server Management Studiossa (SSMS).

- Azure virtuaalikoneen, joka isännöi ensisijainen se muodostaa RDP avulla. 

- Avaa automaattisesti klusterin hallinta.

- Valitse **verkot** -solmu ja klusterin verkon taulukon nimi. Tätä nimeä käytetään `$ClusterNetworkName` muuttujan PowerShell-komentosarjaa.

- Laajenna klusterinimeä ja valitse sitten **roolit**.

- Valitse **roolit** -ruudussa käytettävyys ryhmänimeä hiiren kakkospainikkeella ja valitse sitten **Lisää resurssin** > **Asiakkaan tukiasemaa**.

- Kirjoita **nimi** -ruutuun nimen antaminen tämän uusi kuuntelu ja valitse sitten **seuraavan** kahdesti ja valitse sitten **Valmis**. Ei tuoda listener tai resurssi online tässä vaiheessa.

 >[AZURE.NOTE] Uusi kuuntelu nimi on verkkonimi, jonka avulla sovellukset yhteyden muodostaminen SQL Server käytettävyys-ryhmän ominaisuudet.

- **Resurssit** -välilehti ja valitse juuri luomasi Client Access-kohdassa Laajenna. IP-resurssia hiiren kakkospainikkeella ja valitse Ominaisuudet. IP-osoitteen nimi muistiin. Voit käyttää tätä nimeä `$IPResourceName` muuttujan PowerShell-komentosarjaa.

- Valitse **IP-osoite** **Staattinen IP-osoite** ja määritä staattinen IP-osoite on sama osoite, jota käytit, kun määrität kuormituksen tasauspalvelun IP-osoite Azure-portaalissa. Ota käyttöön NetBIOS tätä osoitetta ja valitse **OK**. Toista tämä vaihe kunkin IP-resurssin, jos ratkaisu ulottuu useita Azure VNets. 

- Klusterisolmu, joka isännöi tällä hetkellä ensisijainen se, valitse Avaa laajennettuja PowerShell ise: ja liitä seuraavat komennot uusi komentosarja.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Päivitä muuttujat ja suorita IP-osoite ja portin määrittäminen uusi kuuntelu PowerShell-komentosarjaa.

 >[AZURE.NOTE] SQL-palvelimet ovat eri alueilla, jos haluat suorittaa PowerShell-komentosarjaa kahdesti. Ensimmäisen kerran käyttämällä klusterin IP resurssinimi-klusterin verkkonimi ja lataa ensimmäisen resurssiryhmä tasaustoiminto IP-osoite. Toisen kerran käyttämällä klusterin IP resurssinimi-klusterin verkkonimi ja lataa toisen resurssiryhmä tasaustoiminto IP-osoite.

Klusterin on nyt yritysresurssi listener käytettävyys ryhmän.

## <a name="2-bring-the-listener-online"></a>2. Tuo kuuntelua verkossa

Käytettävyys ryhmä-listener resurssille määritetty voit tuoda kuuntelua verkossa niin, että sovellusten muodostaa käytettävyys-ryhmä ja kuuntelua tietokannoissa.

- Siirry takaisin käyttämään automaattisesti klusterin Manager. Laajenna **roolit** ja Korosta käytettävyys-ryhmä. **Resurssit** -välilehden listener nimeä hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

- Valitse **riippuvuudet** -välilehti. Jos luettelossa on lueteltu useita resursseja, varmista, että IP-osoitteiden on tai ei ja riippuvuudet. Valitse **OK**.

- Listener nimeä hiiren kakkospainikkeella ja valitse **Ota käyttöön**.


- Kun listener on online- **resurssit** -välilehti, napsauta käytettävyys-ryhmää hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

- Luo riippuvuus listener nimi resurssi (ei IP address resurssien nimi). Valitse **OK**.


- Käynnistä SQL Server Management Studiossa ja yhdistä se ensisijainen.


- Siirry **AlwaysOn suuri käytettävyys** | **käytettävyys ryhmien** | **käytettävyys ryhmän kuuntelijoita**. 


- Pitäisi tulla näkyviin listener nimi, jonka loit automaattisesti klusterin hallinta. Listener nimeä hiiren kakkospainikkeella ja valitse **Ominaisuudet**.


- **Port (portti)** -ruutuun Määritä porttinumero käytettävyys ryhmän kuuntelua avulla voit käyttää aiemmin $EndpointPort (1433 oli oletusarvo), valitse **OK**.

Nyt on SQL Server AlwaysOn käytettävyys ryhmän Azuren näennäiskoneiden resurssien hallinta-tilassa. 

## <a name="test-the-connection-to-the-listener"></a>Testaa yhteys kuuntelutoiminto

Testaa yhteys:

1. SQL Server-palvelimeen, joka on samassa virtual verkossa, mutta se ei ole RDP. Tämä on muita SQL Server-klusterin.

1. Testaa yhteys **sqlcmd** apuohjelmalla. Esimerkiksi seuraava komentosarja muodostaa **sqlcmd** yhteyden ensisijainen se kuuntelua Windows-todennuksen avulla:

        sqlcmd -S <listenerName> -E

SQLCMD yhteyden muodostaa automaattisesti sen mukaan, kumpi SQL Server-esiintymän isännöi ensisijainen se. 

## <a name="guidelines-and-limitations"></a>Ohjeita ja rajoitukset

Huomautus kuormituksen laiduntamismahdollisuuksien ryhmän listener käyttämällä sisäinen Azure-tietokannassa, noudata seuraavia ohjeita:

- Vain yksi sisäinen laiduntamismahdollisuuksien ryhmän listener tuetaan kohti pilvipalvelussa, koska kuuntelutoiminto on määritetty kuormituksen ja on vain yksi sisäinen kuormituksen. On kuitenkin voi luoda multipe ulkoisen kuuntelijoita. 

- Sisäinen kuormituksen kanssa voit käyttää vain saman virtual verkoston-kuuntelua.
 
