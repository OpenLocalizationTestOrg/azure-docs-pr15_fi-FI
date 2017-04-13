<properties
    pageTitle="Suuri käytettävyys ja SQL Server palauttaminen | Microsoft Azure"
    description="Keskustelun erilaisia HADR strategioita Azuren näennäiskoneiden SQL Serveriä."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>SQL Server Azuren näennäiskoneiden suuri käytettävyys ja tietojen palauttaminen

## <a name="overview"></a>Yleiskatsaus

Microsoft Azuren näennäiskoneiden (VMs) SQL Serverin kanssa auttaa alentaa suuri käytettävyys ja tietojen palauttaminen (HADR) ratkaisu. Azuren näennäiskoneiden, sekä hybrid ratkaisuja vain Azure-tuetaan useimmat SQL Server HADR ratkaisuja. Vain Azure-ratkaisussa koko HADR järjestelmä suorittaa Azure-tietokannassa. Hybrid määritys-osa-ratkaisua suorittaa Azure ja muita osan suoritetaan paikallisen organisaation. Azure-ympäristön joustavuutta mahdollistaa Siirry Azure kokonaan tai osittain budjetin ja SQL Server-tietokannan järjestelmien HADR vaatimukset täyttävän.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="understanding-the-need-for-an-hadr-solution"></a>Tietoja HADR-ratkaisun edellyttää

Näin voit varmistaa, että tietokannan on HADR ominaisuuksista, joita palvelun tason sopimuksen (SLA) edellyttää ylöspäin. Siitä, että Azure on suuri käytettävyys-järjestelmiä palvelu kenties pilvipalveluihin ja virheen palautus tunnistus näennäiskoneiden, kuten ei ole itse takaa täytät haluamasi SLA. Menetelmien suojaa VMs suuren käytettävyyden, mutta ei sisällä VMs SQL Serveriä suuren käytettävyyden. Se on mahdollista epäonnistuu AM ollessa online-tilassa ja kunnossa SQL Server-esiintymän. Lisäksi myös järjestelmiä Azure myöntämä Salli vuoksi tapahtumia, kuten palauttamisesta ohjelmiston tai laitteiston virheet ja käyttöjärjestelmän päivitykset VMs käyttökatkot suuren käytettävyyden.

Lisäksi Geo tarpeettomat Storage (GRS) Azure-tietokannassa, jotka on toteutettu kutsutaan geo replikoinnin ominaisuutta, eivät välttämättä ole riittävän palauttamisratkaisun tietokannan. Koska geo replikoinnin lähettää tietoja asynkronisesti, viimeisimmät päivitykset katoavat tietojen yhteydessä. Lisätietoja geo replikoinnin rajoitukset on kuvattu [Geo replikointia ei tue tietojen ja lokitiedostojen erillisessä levyille](#geo-replication-support) -osassa.

## <a name="hadr-deployment-architectures"></a>HADR käyttöönoton arkkitehtuureihin

SQL Server HADR tekniikoita, joita tuetaan Azure ovat:

- [Aina käytettävyys ryhmittelee](https://technet.microsoft.com/library/hh510230.aspx)
- [Tietokantapeilausta](https://technet.microsoft.com/library/ms189852.aspx)
- [Log toimitus](https://technet.microsoft.com/library/ms187103.aspx)
- [Varmuuskopiointi ja palauttaminen Azure Blob Storage-palvelun kanssa](https://msdn.microsoft.com/library/jj919148.aspx)
- [Valitse aina automaattisesti klusterin esiintymiä](https://technet.microsoft.com/library/ms189134.aspx)

On mahdollista yhdistää technologies yhdessä toteuttamaan SQL Server-ratkaisun, joka on suuri käytettävyys ja tietojen palauttaminen ominaisuuksia. Sen mukaan, voit käyttää teknologian yhdistelmäympäristö voi pyytää VPN-tunnelin Azure virtual verkoston kanssa. Seuraavissa osissa kerrotaan joistakin Esimerkki käyttöönoton arkkitehtuureihin.

## <a name="azure-only-high-availability-solutions"></a>Vain Azure: suuren käytettävyyden ratkaisuja

Voit on suuri käytettävyys-ratkaisun SQL Server Azure-tietokannassa aina käyttöön käytettävyys ryhmien käyttäminen tietokannan tai tietokannan peilausta.

| Tekniikka                               | Esimerkki arkkitehtuureihin                    |
| ---------------------------------------- | ---------------------------------------- |
| **Aina käytettävyys ryhmittelee**        | Kaikkien käytettävyyden replikoiden Azure VMs käytössä hyvin käytettävyyden samalla alueella. Haluat määrittää toimialueen ohjauskoneen AM, koska Windows Server-automaattisesti klusterointi (WSFC) edellyttää Active Directory-toimialueen.<br/> ![Aina käytettävyys ryhmittelee](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_ha_always_on.gif)<br/>Lisätietoja on ohjeaiheessa [Määrittäminen aina käyttöön käytettävyys ryhmistä Azure (Graafisen)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). |
| **Valitse aina automaattisesti klusterin esiintymiä** | Automaattisesti klusterin esiintymät (FCI), jotka edellyttävät jaettua tallennustilan voi luoda 2 eri tavoilla.<br/><br/>1. Valitse tallennustilan kolmannen osapuolen klusteroinnin ratkaisun tukeman Azure VMs käytön kahden solmun-WSFC FCI. Katso tietyn esimerkissä käytetään SIOS DataKeeper, [suuri käytettävyys WSFC ja 3 osapuolten ohjelmistojen SIOS Datakeeper jaettua tiedostoresurssia](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>2. Valitse etä-iSCSI kohde Azure VMs käytön kahden solmun-WSFC FCI jaettu estä tallennustilan ExpressRoute kautta. Esimerkiksi NetApp yksityinen Storage (NPS) paljastaa iSCSI-kohteen kanssa Equinix Azure VMs ExpressRoute kautta.<br/><br/>Kolmannen osapuolen jaettua tallennustilan ja tietojen replikoinnin ratkaisuja Ota yhteyttä toimittajan automaattisesti tietojen käyttämisestä liittyvät ongelmat.<br/><br/>Huomaa, että FCI päälle [Azure tallentamista](https://azure.microsoft.com/services/storage/files/) ei tueta, koska tämä ratkaisu ei käytä Premium-tallennustilan. Yritämme tukee tätä pian. |

## <a name="azure-only-disaster-recovery-solutions"></a>Vain Azure: tietojen palauttaminen ratkaisut

Voit antaa palauttamisratkaisun aina käyttöön käytettävyys ryhmien käyttäminen Azure SQL Server-tietokannat, tietokantapeilaus tai varmuuskopiointi ja palauttaminen ja tallennustilaa BLOB-objektit.

| Tekniikka                               | Esimerkki arkkitehtuureihin                    |
| ---------------------------------------- | ---------------------------------------- |
| **Aina käytettävyys ryhmittelee**        | Käytettävyys replikoiden käynnissä useita palvelinkeskusten palauttaminen Azure VMs yli. Valmis sivusto käyttökatkosta suojaa rajat-alueen ratkaisu. <br/> ![Aina käytettävyys ryhmittelee](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_alwayson.png)<br/>Alueella kaikki replikat on oltava sama pilvipalvelussa ja saman VNet. Koska kunkin alueen on erillinen VNet, ratkaisujen edellyttävät VNet VNet yhteys. Lisätietoja on artikkelissa [määrittäminen sivuston sivuston VPN Azure perinteinen-portaalissa](../vpn-gateway/vpn-gateway-site-to-site-create.md). |
| **Tietokantapeilausta**                   | Lyhennys ja peilikuva- ja palvelimessa on eri palvelinkeskusten tietojen palauttamista varten. Sinun on otettava käyttöön palvelimen varmenteiden käyttäminen, koska active directory-toimialueen ei voi olla useita palvelinkeskusten.<br/>![Tietokantapeilausta](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_dbmirroring.gif) |
| **Varmuuskopiointi ja palauttaminen Azure Blob Storage-palvelun kanssa** | Tuotannon tietokantojen varmuuskopioida suoraan Blob-objektien tallennustilaan eri joten tietojen palauttamista varten.<br/>![Varmuuskopiointi ja palauttaminen](./media/virtual-machines-windows-sql-high-availability-dr/azure_only_dr_backup_restore.gif)<br/>Lisätietoja on artikkelissa [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="hybrid-it-disaster-recovery-solutions"></a>Hybrid IT: Tietojen palauttaminen ratkaisut

Voit antaa tietojen palauttaminen ratkaista SQL Server-tietokannat hybrid IT-ympäristössä, aina käyttöön käytettävyys ryhmät, tietokantapeilausta, toimitus log ja varmuuskopiointi ja palauttaminen Azure blogin tallennustilan kanssa.

| Tekniikka                               | Esimerkki arkkitehtuureihin                    |
| ---------------------------------------- | ---------------------------------------- |
| **Aina käytettävyys ryhmittelee**        | Jotkin käytettävyys replikoiden käynnissä Azure VMs ja muiden replikoiden käynnissä paikalliseen sivustojenvälisen tietojen palauttamista varten. Tuotantolaitoksen voi olla joko paikallisessa tai Azure palvelinkeskukseen.<br/>![Aina käytettävyys ryhmittelee](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_alwayson.gif)<br/>Koska kaikki käytettävyys replikat on oltava sama WSFC klusterin, WSFC-klusterin on kattavat verkkojen (usean aliverkon WSFC klusterin). Tämä kokoonpano edellyttää VPN-yhteyden Azure ja paikallisen verkon välillä.<br/><br/>Saat tietokantoja onnistuneen palauttaminen sinun täytyy asentaa myös replikan toimialueen ohjauskoneen tietojen palauttaminen-sivustossa.<br/><br/>Se on mahdollista käyttämällä ohjattua replika Lisää SSMS Azure replikan lisääminen aiemmin luotuun aina käyttöön käytettävyys ryhmään. Lisätietoja on kohdassa opetusohjelma: Laajenna aina käyttöön käytettävyys ryhmäsi Azure. |
| **Tietokantapeilausta**                   | Yksi kumppanin käynnissä Azure-AM ja muut suorittaminen paikalliset sivustojenvälisen palauttaminen palvelimen varmenteiden käyttäminen varten. Kumppaneille ei tarvitse olla sama Active Directory-toimialueen ja edellyttää VPN-yhteyttä ei ole.<br/>![Tietokantapeilausta](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_dbmirroring.gif)<br/>Toisen tietokantapeilaus tilanne koskee yhdessä kumppanin käynnissä Azure-AM ja muita käynnissä paikallisesti sivustojenvälisen palauttaminen saman Active Directory-toimialueen. Tarvitaan [VPN-yhteyden Azure virtual verkon ja paikallisen verkon välillä](../vpn-gateway/vpn-gateway-site-to-site-create.md) .<br/><br/>Saat tietokantoja onnistuneen palauttaminen sinun täytyy asentaa myös replikan toimialueen ohjauskoneen tietojen palauttaminen-sivustossa. |
| **Log-toimitus**                         | Azure-AM ja muut käynnissä yksi palvelin paikalliset sivustojenvälisen tietojen palauttamista varten. Log toimitus riippuu siitä, Windows-tiedostojen jakaminen, joten VPN-yhteyden Azure virtual verkon ja paikallisen verkon välillä ei tarvita.<br/>![Log toimitus](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_log_shipping.gif)<br/>Saat tietokantoja onnistuneen palauttaminen sinun täytyy asentaa myös replikan toimialueen ohjauskoneen tietojen palauttaminen-sivustossa. |
| **Varmuuskopiointi ja palauttaminen Azure Blob Storage-palvelun kanssa** | Paikalliset tuotannon tietokantojen suoraan palauttaminen Azure-blob-säiliö haluat varmuuskopioida.<br/>![Varmuuskopiointi ja palauttaminen](./media/virtual-machines-windows-sql-high-availability-dr/hybrid_dr_backup_restore.gif)<br/>Lisätietoja on artikkelissa [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md). |

## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>SQL Server-HADR Azure-tietokannassa tärkeitä

Azure VMs, varasto ja verkko on erilaisia kuin paikallisen,-virtualisoidussa IT-infrastruktuurin toiminnallisia ominaisuuksia. Onnistunut käyttöönotto HADR SQL Server Azure-ratkaisun edellyttää ymmärtää nämä erot ja suunnitella, jotta ratkaisu.

### <a name="high-availability-nodes-in-an-availability-set"></a>Suuren käytettävyyden solmujen käytettävyyden määrittäminen

Käytettävyys joukot Azure avulla voit sijoittaa suuren käytettävyyden solmut erillisessä vika toimialueet (FDs) ja Päivitä Domains (UDs). Azure VMs sijoittaa käytettävyys samat, saat käyttöön ne samaan pilvipalvelussa. Vain saman pilvipalvelussa solmut voivat osallistua käytettävyys samat. Lisätietoja on kohdassa [Manage näennäiskoneiden käytettävyyttä](virtual-machines-windows-manage-availability.md).

### <a name="wsfc-cluster-behavior-in-azure-networking"></a>Azure verkko WSFC klusterin toiminta

Azure RFC yhteensopiva DHCP-palvelu voi aiheuttaa tiettyjä WSFC klusterin käyttömahdollisuudet epäonnistuu vuoksi klusterin verkkonimi on varattu kaksinkertainen IP-osoite, kuten sama IP-osoite yhdeksi klusterisolmut luomista. Tämä on ongelma, kun otat aina käyttöön käytettävyys ryhmiä, joka määräytyy WSFC-ominaisuus.

Harkitse skenaarion, kun kahden solmun klusterin luodaan ja online-tilaan:

1. Klusterin kirjautuu ja valitse NODE1 pyytää klusterin verkkonimi dynaamisesti määritetty IP-osoite.

2. NODE1 oman IP-osoite kuin IP osoitetta ei anneta DHCP-palvelu, koska DHCP-palvelu tunnistaa, että pyyntö tulee NODE1 itse.

3. Windows havaitsee, että kaksoiskappaleiden osoite on määritetty sekä NODE1 klusterin verkkonimi ja klusterin oletusryhmän epäonnistuu online-tilaan.

4. Klusterin oletusryhmän siirtää NODE2, joka käsittelee NODE1's IP-osoite klusterin IP-osoite ja tuo klusterin oletusryhmään verkossa.

5. Kun NODE2 yrittää muodostaa yhteyden NODE1, NODE1 suunnattu pakettien jätä koskaan NODE2, koska se ratkaistaan NODE1's IP-osoite itse. NODE2 ei pysty muodostamaan yhteyden NODE1, menettää quorum ja sulkee klusterin.

6. Tässä vaiheessa NODE1 lähettää paketteja NODE2, mutta NODE2 ei vastaa. NODE1 menettää quorum ja klusterin sulkee.

Tässä skenaariossa voidaan välttää määrittämällä käyttämättömät staattinen IP-osoite, kuten link-local IP-osoite, kuten 169.254.1.1, jotta voit tuoda klusterin verkkonimi on verkossa klusterin verkkonimi. Yksinkertaistamaan prosessia, katso [Määrittäminen Windows automaattisesti klusterin aina käyttöön käytettävyys ryhmien Azure-tietokannassa](http://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx).

Lisätietoja on ohjeaiheessa [Määrittäminen aina käyttöön käytettävyys ryhmistä Azure (Graafisen)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

### <a name="availability-group-listener-support"></a>Käytettävyys listener tuki

Käytettävyys ryhmän kuuntelijoita tuetaan Azure virtuaalilaitteiksi Windows Server 2008 R2, Windows Server 2012: ssa tai Windows Server 2012 R2 Windows Server 2016. Tuki on mahdollista kuormituksen päätepisteet, jotka on otettu käyttöön, jotka ovat käytettävissä ryhmän solmut Azure VMs avulla. Sinun on noudatettava toimimaan sekä asiakassovellukset, joissa on Azure samoin kuin käynnissä paikallisen kuuntelijoita määräten määritysvaiheet.

Vaihtoehtoja on kaksi tärkeimmät oman listener määrittämisestä: ulkoinen (julkinen) tai sisäinen. Ulkoinen (julkisten) listener käyttää vastakkaisten kuormituksen internet ja on yhdistetty kanssa julkisen Virtual IP (VIP), joita voit käyttää Internetin välityksellä. Sisäinen listener käyttää sisäistä kuormituksen ja tukee vain saman Virtual verkoston asiakkaita. Joko tasaustoiminto tyyppiä ladata, sinun on otettava suoraan palvelin palauttaa. 

Jos käytettävyys-ryhmän ulottuu useita Azure aliverkosta (kuten käyttöönoton yli ulottuvalla Azure alueet)-asiakasohjelman yhteysmerkkijonon on sisällettävä "**MultisubnetFailover = True**". Tämä aiheuttaa rinnakkain yhteyksien eri aliverkosta replikoiden kanssa. Ohjeita kuuntelija määrittämisestä on artikkelissa

- [Määritä ILB-listener aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md).
- [Määritä ulkoisen listener, aina käyttöön käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-ext-listener.md).

Voit kuitenkin muodostaa yhteyden käytettävyys replikoissa erikseen muodostamalla yhteyden suoraan palveluesiintymä. Myös, koska aina käyttöön käytettävyys ryhmät ovat yhteensopivia tietokantapeilaus asiakkaiden kanssa, voit muodostaa yhteyden käytettävyys replikat tietokantapeilaus kumppanit, kunhan replikoita on määritetty tietokantapeilausta samalla tavoin:

- Replikaan ensisijainen ja toissijainen replikaan

- Toissijainen se on määritetty ei voi lukea (**Luettavissa toissijainen** vaihtoehto määritetty **ei**)

Esimerkki-asiakasohjelman yhteyden merkkijono, joka vastaa tietokannan peilausta kaltaisessa määritysten ADO.NET-tai SQL Server Native Client on alla:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

Lisätietoja asiakasyhteydet-kohdassa:

- [SQL Server Native Client yhteyden merkkijonon avainsanojen käyttäminen](https://msdn.microsoft.com/library/ms130822.aspx)
- [Asiakkaiden muodostaa tietokannan peilausistunnossa (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
- [Yhteyden muodostaminen Hybrid IT-Listener käytettävyys-ryhmä](http://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
- [Käytettävyys ryhmän kuuntelijoita asiakasyhteydet ja sovelluksen automaattisesti (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
- [Tietokannan peilausta yhteyden merkkijonojen käyttäminen käytettävyyden ryhmiin](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Verkon hybrid TIETOTEKNIIKAN viive

HADR ratkaisu olettaen, että ajanjaksojen kanssa hyvin verkon latenssin paikalliseen verkkoon ja Azure välillä voi olla tulisi ottaa käyttöön. Otettaessa replikoiden Azure käytettävä asynkroninen Vahvista sijaan synkronisen Vahvista synkronoinnin tila. Kun käyttöönotto tietokantapeilaus palvelimet sekä paikallisen ja Azure-tietokannassa, käytä erinomainen suorituskyky-tilaa sijaan suuri suojaus-tilassa.

### <a name="geo-replication-support"></a>GEO replikoinnin tuki

Azure kohtauksen GEO-replikoinnin ei tue datatiedosto ja lokitiedoston saman tietokannan eri levyille tallennetaan. GRS kopioi muutokset kunkin levyn itsenäisesti ja asynkronisesti. Tämä järjestelmä takaa kirjoitus-tilauksen yksittäisen levyn geo replikoida tulosteessa, mutta ei yli useita levyjä geo replikoida kopioita sisällä. Jos määrität tietokannan tietojen tallentamista sen datatiedosto ja erillinen levyille lokitiedostoon, palautetun levyjen huono jälkeen voi olla päivitetty kopio datatiedoston kuin lokitiedoston, joka katkaisee kirjoituksen täydennys Kirjaudu sisään SQL Server- ja tapahtumien ACID-ominaisuudet. Jos sinulla ei ole mahdollisuus poistaa käytöstä geo-replikoinnin tallennustilan tilin, kannattaa säilyttää kaikki tiedot ja lokitiedostojen tietyn tietokannan saman levyn. Jos käytössäsi on useampi kuin yksi levyn tietokannan koon vuoksi, haluat ottaa käyttöön jonkin yllä olevassa luettelossa Varmista tietojen kopion tietojen palauttaminen ratkaisuja.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat luoda Azure virtuaalikoneen SQL Serverin kanssa, katso [valmistelu SQL Server Virtual Machine-Azure](virtual-machines-windows-portal-sql-server-provision.md).

Saat parhaan suorituskyvyn SQL Server Azure-AM käytössä-kohdassa [Suorituskyvyn parhaat käytännöt SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-performance.md)ohjeet.

Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).

### <a name="other-resources"></a>Muut resurssit

- [Asenna uusi Active Directory-metsää Azure-tietokannassa](../active-directory/active-directory-new-forest-virtual-machine.md)
- [Luo WSFC klusteri varten aina Azure AM ryhmien käytettävyys](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)
