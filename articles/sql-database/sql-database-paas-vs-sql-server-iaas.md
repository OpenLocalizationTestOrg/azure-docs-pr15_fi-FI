<properties
    pageTitle="SQL (PaaS) tietokannan ja SQL Server-VMs (IaaS) pilveen | Microsoft Azure"
    description="Katso, mitä cloud SQL Server-asetus sopii sovelluksesi: SQL Azure (PaaS)-tietokanta tai pilveen-Azuren näennäiskoneiden SQL Server."
    services="sql-database, virtual-machines"
    keywords="SQL Server pilvipalveluun, SQL Server pilvipalvelussa, PaaS tietokannan cloud SQL Server-DBaaS"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Valitse pilvestä SQL Server-asetus: SQL Azure (PaaS)-tietokanta tai SQL Server Azure VMs (IaaS)

Azure on kaksi vaihtoehtoa: isännöinnin: Microsoft Azure SQL Server-toiminnoista:

* [Azure SQL-tietokanta](https://azure.microsoft.com/services/sql-database/): alkuperäisen pilveen, eli service (PaaS)-tietokanta tai tietokantaan serviceksi (DBaaS), joka on optimoitu ohjelmiston nimellä--palvelun (SaaS) sovellusten kehittämiseen ympäristö A SQL-tietokantaan. Siinä on yhteensopivuus useimmat SQL Server-ominaisuudet. Lisätietoja PaaS on artikkelissa [PaaS ominaisuudet](https://azure.microsoft.com/overview/what-is-paas/).
* [SQL Server-Azuren näennäiskoneiden](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server asennettu ja pilveen, Windows Server näennäiskoneiden (VMs) Azure, eli infrastruktuuri palveluna (IaaS)-sovelluksessa.
Azuren näennäiskoneiden SQL Server on tarkoitettu siirtäminen aiemmin SQL Server-sovellukset. Kaikki versiot ja SQL Server-versioiden ovat käytettävissä. Siinä on 100 %: n yhteensopivuus SQL Serveristä, joilla voit isännöidä niin monta tietokantojen tarpeen mukaan ja suoritettaessa rajat tietokannan tapahtumiksi. Siinä on täydet oikeudet, SQL Server-ja Windows.

Lue, miten kunkin asetuksen esittämiseen Microsoft tiedot-ympäristössä ja ohjeita, jotka vastaavat oikean vaihtoehdon business vaatimuksia. Onko kustannuksia tai mahdollisimman vähän hallinta ennen monipuolisista priorisoida tämän artikkelin avulla voit päättää hallintatavan valitseminen toimittaa sinulle tärkeisiin yrityksen tarpeen mukaan.


## <a name="microsofts-data-platform"></a>Microsoftin tietojen ympäristö

Ensimmäinen toimea keskusteluihin Azure paikallisen SQL Server-tietokannat ja ymmärtää on, että voit käyttää sitä kaikki. Microsoftin tietojen ympäristö hyödyntää SQL Server-tekniikkaan ja mahdollistaa sen käytön fyysinen paikallisen koneet, yksityinen cloud-ympäristössä, kolmannen osapuolen isännöityä yksityinen cloud-ympäristössä ja julkisen pilveen. SQL Server Azure virtual mchines mahdollistaa yksilöllinen ja monipuolisen yrityksen tarpeiden paikallisen ja cloud isännöimä käyttöönotoissa yhdistelmän käytettäessä palvelintuotteiden Kehitystyökalut ja ammattitaidon samat kaikissa näissä ympäristöissä avulla.

   ![Cloud SQL Server-asetukset: SQL Serverin IaaS tai SaaS SQL-tietokannan pilveen.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Kaavion tarkastelu molemmista voit jakauma (X-akselilla) infrastruktuuri kohdalla hallinta taso ja kustannukset tehokkuuden tietokannan tason koonnin ja (Y-akselin) automaatio aste.

Sovelluksen suunnitellessasi neljä perusasetukset ovat käytettävissä isännöinnin sovelluksen SQL Server-osassa:

- SQL Server-virtualisoidussa Fyysinen tietokoneissa
- SQL Server-ympäristöön virtualisoidussa koneet (yksityinen pilvipalvelu)
- SQL Server Azure Virtual Machine (Microsoft julkisen pilvipalvelu)
- Azure SQL-tietokanta (Microsoft julkisen pilvipalvelu)

Seuraavissa osissa kerrotaan, SQL Serveristä Microsoft julkisen pilvipalvelussa: Azure SQL-tietokanta ja SQL Server Azure VMs. Lisäksi yleisiä liiketoiminta-motivators Tutustu määrittämiseksi sovelluksen parhaiten sopivan vaihtoehdon.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Tarkempi katsaus Azure SQL-tietokanta ja SQL Server Azure VMs

**Azure SQL-tietokanta** on suhteellinen tietokannan-nimellä--palvelu (DBaaS) isännöidään Azure pilveen, joka on alan *Ohjelmiston nimellä--palvelun (SaaS)* ja *Platform nimellä--palvelun (PaaS)*luokkiin. [SQL-tietokanta](sql-database-technical-overview.md) on muodostettu standardoitu laitteiston ja ohjelmiston, joka on omistaja, isännöidään ja Microsoftin. SQL-tietokanta, jossa voit kehittää suoraan-palvelu valmiita ominaisuuksia ja toimintoja. Kun käytät SQL-tietokantaan, voit tukiyhteyksiä asetuksilla skaalata ylös- tai ulos suurempi Power palvelukatkoksia kanssa.

**SQL Server-Azuren näennäiskoneiden (VMs)** kuuluu alan luokkaan *Infrastruktuurin nimellä--palvelun (IaaS)* ja avulla voit suorittaa SQL Serverin sisällä virtual kone pilveen. Samoin kuin SQL-tietokantaan, se on muodostettu standardoitu laitteita, jotka omistan, isännöidään ja Microsoftin. Kun käytät SQL Server AM, voit joko maksu-kuin olet sijainnista SQL Server-käyttöoikeus sisältää jo SQL Server-kuva tai helposti voit käyttää aiemmin luotuja käyttöoikeuksia. Voit myös helposti asteikko-ylös/alas ja Pysäytä/jatka AM tarpeen mukaan.

Yleensä SQL näistä vaihtoehdoista on optimoitu eri tarkoituksiin:

- **SQL-tietokanta** on optimoitu vähentää kokonaiskustannuksiin pienin valmistelu ja hallita useita tietokantoja. Se vähentää jatkuvaa hallinta kustannukset, koska sinulla ei ole hallittavan näennäiskoneiden, käyttöjärjestelmä tai tietokantaohjelman. Ei tarvitse hallita päivityksiä, suuri käytettävyys tai [varmuuskopiot](sql-database-automated-backups.md). Yleensä Azure SQL-tietokantaan voit lisätä tietokantojen hallitsee yksittäinen numero IT tai kehitys resurssi.
- **SQL Server Azure VMs käytössä** on tarkoitettu Azure aiemmin sovellusten siirtäminen tai laajentaminen yhdistelmäympäristöissä pilveen paikallisen sovellukset. Lisäksi voit käyttää SQL Server virtual machine kehittää ja testata perinteinen SQL Server-sovelluksia. SQL Server Azure VMs, jossa on täysi järjestelmänvalvojan oikeudet erillinen SQL Server-esiintymän ja pilvipohjainen AM. Organisaatiolla jo on IT-resurssit käytettävissä säilyttää näennäiskoneiden on sopivaa vaihtoehtoa. Näiden ominaisuuksien avulla voit luoda erittäin mukautetun järjestelmän sovelluksen suorituskykyä ja käytettävyyden vaatimukset.

Seuraavassa taulukossa on yhteenveto SQL-tietokantaan ja SQL Server Azure VMs tärkeimmät ominaisuudet:

|       | SQL-tietokantaan | SQL Server Azure virtuaalikoneen|
| -------------- | ------------ | ---------------------- |
| **Paras:** | Cloud suunniteltu uusien sovellusten kehittämisen ja markkinoinnin rajoitusten sisältävät. |Sovellukset, jotka edellyttävät nopea siirron pilveen mahdollisimman vähän muutokset. Nopea kehittäminen ja testaa tilanteissa, kun et halua ostaa paikalliset tuotannon SQL Server-laite. |
|| Ryhmistä, joilla on valmiita suuren käytettävyyden, palauttaminen ja päivitys tietokannan. |Ryhmistä, joilla voit määrittää ja hallita suuren käytettävyyden, palauttaminen ja korjausta SQL Server. Jotkin annettu Tämä yksinkertaistaa huomattavasti automaattinen ominaisuuksia. |
||Ryhmät, joiden et halua pohjana käyttöjärjestelmä ja Hakumääritysten asetusten hallinta.| Jos tarvitset mukautetun ympäristössä, jossa on täysi järjestelmänvalvojan oikeudet.|
||Enintään 1 TT tietokannat tai suurempi tietokannoissa, jotka voivat olla [vaaka- tai pystysuunnassa osioitu](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) kaavan asteikko-kohtaa.|SQL Server-esiintymien kanssa enintään 64 TT tallennustilaa. Esiintymän tukee niin monta tietokantojen tarpeen mukaan. |
||[Rakennuksen ohjelmiston nimellä--palvelun (SaaS) sovellukset](sql-database-design-patterns-multi-tenancy-saas-applications.md).| Siirtyminen ja enterprise- ja hybrid sovellusten.|
|||||
|**Resurssit:**|Et halua käyttää IT-resurssit määritys ja pohjana oleva infrastruktuuri hallinnointi, mutta haluat keskittyä sovelluskerroksen.|Sinun on IT resurssien määrittäminen ja hallinta. Jotkin annettu Tämä yksinkertaistaa huomattavasti automaattinen ominaisuuksia.|
|**Kokonaiskustannuksia:**|Jättää pois laitteiston kustannuksia ja vähentää järjestelmänvalvojan kustannukset.|Jättää pois laitteiston kustannukset.|
|**Liiketoiminnan jatkuvuus:**|Sisäinen virhe poikkeama infrastruktuurin ominaisuuksien lisäksi Azure SQL-tietokanta sisältää ominaisuuksia, kuten [automaattisen varmuuskopioinnin](sql-database-automated-backups.md), [Ajankohta tiedostoa](sql-database-recovery-using-backups.md#point-in-time-restore), [Geo palauttaminen](sql-database-recovery-using-backups.md#geo-restore)ja [Aktiivinen Geo-replikoinnin](sql-database-geo-replication-overview.md) niin, että Liiketoiminnan jatkuvuus. Lisätietoja on artikkelissa [SQL-tietokantaan Liiketoiminnan jatkuvuus yleiskatsaus](sql-database-business-continuity.md).|SQL Server Azure VMs voit hyvin käytettävyyttä ja tietojen palauttaminen ratkaista tietokantasi tunnistenimiin tarpeittesi määrittäminen. Tämän vuoksi voi olla järjestelmä, joka on optimoitu erittäin sovelluksen. Voit testata ja näyttää failovers itse tarpeen mukaan. Lisätietoja on artikkelissa [suuren käytettävyyden ja SQL Server-Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).|
|**Hybrid cloud:**|Paikallisen sovelluksen voit käyttää tietoja Azure SQL-tietokantaan.|SQL Server Azure VMs tietokoneeseen voi asentaa sovellukset, jotka suoritetaan osittain pilvipalvelussa ja osittain paikallisen. Voit esimerkiksi laajentaa paikallisen verkko- ja Active Directory-toimialueen pilveen [Azure Virtual verkon](../virtual-network/virtual-networks-overview.md)kautta. Lisäksi voit tallentaa paikallisen datatiedostojen Azuren tallennustilaan käyttäen [SQL Server Azure datatiedostot](http://msdn.microsoft.com/library/dn385720.aspx). Lisätietoja on artikkelissa [Johdanto SQL Server 2014 Hybrid pilveen](http://msdn.microsoft.com/library/dn606154.aspx).|
||Tukee [SQL Serverin tapahtumien replikointia](https://msdn.microsoft.com/library/mt589530.aspx) tilaaja, voit kopioida tietoja muodossa.|Täysin tukee [SQL Serverin tapahtumien replikoinnin](https://msdn.microsoft.com/library/mt589530.aspx), [AlwaysOn käytettävyys ryhmät](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md), Integration Services ja Log toimitus replikointia tiedot. Lisäksi perinteinen SQL Server-varmuuskopiot tuetaan täysin|
|||||
|||||

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Liiketoiminnan motivaatioita Azure SQL-tietokanta tai SQL Server Azure VMs valitsemiseen

### <a name="cost"></a>Kustannukset

Onko olet käynnistys, kassavirrasta tai aiemmin luotuja yritys, joka toimii tiiviisti budjetin rajoitteet-ryhmän strapped, rajoitettu rahoitus on usein ensisijainen ohjain voit isännöidä tietokantoja valitsemisessa. Tässä osassa kerrotaan tietoja laskutus- ja käyttöoikeudet tapauksen näistä vaihtoehdoista relaatiotietokannasta Azure perusteet: SQL-tietokantaan ja SQL Server Azure VMs. Opit myös tietoja laskemisesta yhteensä kustannukset.

#### <a name="billing-and-licensing-basics"></a>Laskutus ja käyttöoikeudet perusteet

**SQL-tietokantaan** myydään asiakkaille palveluna ei käyttöoikeuden kanssa.  [SQL Server Azure VMs](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md) myydään mukana käyttöoikeuksin, maksat minuutissa. Jos sinulla on aiemmin käyttöoikeus, voit myös käyttää sitä.  

Tällä hetkellä **SQL-tietokanta** on käytettävissä useita palvelutasot, jotka kaikki ovat laskutettu kerran tunnissa kiinteää korkoa palvelun tason perusteella ja valitset suorituskyvyn taso. Lisäksi on laskuttaa lähtevän Internet-tietoliikenteen osoitteessa [tiedonsiirron korvaukset](https://azure.microsoft.com/pricing/details/data-transfers/)säännöllisesti. Basic, Vakio ja Premium-palvelutasot on suunniteltu ennakoitavissa suorituskyky olisi suorituskyvyn tasoja vastaamaan sovelluksen piikin vaatimusten kanssa. Voit vaihtaa palvelutasot ja suorituskyvyn tasoon sovelluksen vaihteleva siirtonopeuden tarpeita vastaavaksi. Jos tietokannassa on suuri tapahtumien määrä ja tarvitsee tukevat monia käyttäjiä, on suositeltavaa Premium-palvelutaso. Katso uusimmat tiedot nykyisen tuetut palvelutasot, [Azure SQL-tietokannan palvelutasot](sql-database-service-tiers.md). Voit myös luoda [joustavasti tietokannan jakavat](sql-database-elastic-pool.md) tietokannan esiintymien välillä suorituskykyyn resurssien jakamiseen.

**SQL-tietokantaan**database-ohjelmisto on automaattisesti määritetty asennettu ja päivittää Microsoftilta, mikä vähentää hallinnan kustannukset. Lisäksi [valmiin varmuuskopiointi](sql-database-automated-backups.md) sen ominaisuuksia auttaa sinua saavuttamiseksi säästöjen kustannukset, erityisesti silloin, kun sinulla on runsaasti tietokannat.

**SQL Server Azure VMs**voit käyttää mitä tahansa antamalla ympäristö SQL Serverin kuvaa (joka sisältää käyttöoikeuden) tai tuo SQL Server-käyttöoikeus. Tuetut SQL Server versiot (2008R2, 2012, 2014, 2016) ja (Kehitystyökalut, Express, Web, Vakio, yrityksen)-versiot ovat käytettävissä. Lisäksi tuo-Your-omistaja-käyttöoikeus (BYOL) kuvat ovat käytössä. Kun käyttämällä Azure-kuvia, toiminnallisia kustannukset määräytyy AM koon ja valitset SQL Server-version. AM koon tai SQL Server-version, vaikka maksaa minuutissa käyttöoikeuksien myöntämistä kustannukset SQL Serverin ja Windows Server sekä AM-levyjen Azuren tallennustilaan kustannukset. Voit käyttää SQL Server tarvitset ilman ostavaa lisäksi SQL Server käyttöoikeuksien minuutissa-vaihtoehto. Jos tuot SQL Server-käyttöoikeus Azure-perittävän Windows Server ja tallennustilaa kustannusten vain. Lisätietoja tuo-your-omista käyttöoikeuksista on artikkelissa [Käyttöoikeuden Mobility Azure Software Assurance](https://azure.microsoft.com/pricing/license-mobility/).

#### <a name="calculating-the-total-application-cost"></a>Yhteensä kustannuksen

Kun alat käyttää cloud-ympäristössä, kustannusten käynnissä sovelluksen sisältää kehittämisen ja hallinnan kustannuksia sekä julkinen cloud ympäristö palvelun kustannukset.

Näin yksityiskohtaiset Kustannuslaskenta sovelluksen Azure VMs SQL-tietokantaan ja SQL Server-tietokoneessa:

**Kun käytät Azure SQL-tietokantaan:**

*Kokonaiskustannus sovelluksen = erittäin pienennetyn hallinta kustannukset + software development kustannukset + SQL-tietokantaan kustannus*

**Kun käytät SQL Server Azure VMs:**

*Kokonaiskustannus sovelluksen = erittäin pienennetyn software development kustannukset + hallinnan kustannukset + SQL Server- ja Windows Server käyttöoikeuksien myöntämistä kustannukset + Azure tallennustilan kustannukset*

Saat lisätietoja hinnat on seuraavissa resursseissa:

- [SQL-tietokannan hinnat](https://azure.microsoft.com/pricing/details/sql-database/)
- [Virtuaalikoneen hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/) [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) - ja [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure hinnat laskuri](https://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] On vain pienen osan SQL Serverin ominaisuuksia, jotka eivät ole käytettävissä tai SQL-tietokanta ei ole käytettävissä. Lisätietoja on [SQL-tietokannan Yleiset rajoitukset ja ohjeita](sql-database-general-limitations.md) ja [SQL-tietokannan Transact-SQL-tiedot](sql-database-transact-sql-information.md) . Jos siirrät aiemman SQL Server-ratkaisun pilveen, katso [siirtäminen SQL Server-tietokantaan Azure SQL-tietokantaan](sql-database-cloud-migrate.md). Siirtyessäsi aiemmin luotua paikallisen SQL Server-sovellusta SQL-tietokantaan, harkitse päivittämistä sovelluksen ominaisuuksista, joita cloud services tarjouksen hyödyntää. Esimerkiksi kannattaa harkita isännöidä niin, että kustannukset edut että sovelluskerroksen [Azure Web App-palveluun](https://azure.microsoft.com/services/app-service/web/) tai [Azure pilvipalveluihin](https://azure.microsoft.com/services/cloud-services/) avulla.

### <a name="administration"></a>Hallinta

Monta yrityksille siirtymän pilvipalveluun päätös on paljon tietoja purkaminen monimutkaisuudesta hallinta sellaisena kuin se on kustannus. Microsoft hallitsee **SQL-tietokantaan**, pohjana olevan laitteen. Microsoft automaattisesti kopioi kaikki tiedot, jolloin suuren käytettävyyden, määrittää ja päivittää database-ohjelmisto, hallitsee kuormituksen tasaamisen ja onko läpinäkyvä automaattisesti, jos palvelin epäonnistui. Voit hallita tietokannan edelleen, mutta et enää tarvitse tietokantamoduuli, server-käyttöjärjestelmä tai laitteiston hallitsemiseen.  Kohteet, voit edelleen hallinta esimerkiksi tietokantojen ja kirjautumiset, indeksi ja kyselyn säätäminen ja tarkistaminen ja suojaus.

**SQL Server Azure VMs**sinulla on täydet oikeudet-käyttöjärjestelmän ja SQL Server-esiintymän määrittäminen. AM, jossa on voit päättää, milloin haluat päivittää/päivitys tietokannan ohjelmiston ja käyttöjärjestelmän ja milloin tahansa lisäohjelmistoa kuten anti-virus. Jotkin automaattinen ominaisuudet toimitetaan yksinkertaistaa huomattavasti aikavyöhykkeiden, varmuuskopiointi ja suuri käytettävyys. Lisäksi voit määrittää koon AM levyjen määrä ja tallennustilaa niiden määrityksiä. Azure avulla voit tarvittaessa muuttaa AM kokoa. Lisätietoja on artikkelissa [virtuaalikoneen ja Azure Cloud palvelun koot](../virtual-machines/virtual-machines-linux-sizes.md). 

### <a name="service-level-agreement-sla"></a>Tason palvelusopimus (SLA)

Monta IT-osastoille kokouksen velvoitteiden ylä-ja aika, palvelussa palvelutasosopimusta (SLA) on yläreunan prioriteetti. Tässä osassa on Tarkista mitä SLA koskee kunkin tietokantoja isännöinnin vaihtoehto.

**SQL-tietokannasta** Basicin, Vakio ja Premium palvelutasot Microsoft tarjoaa saatavuus SLA 99,99 prosenttia. Katso uusimmat tiedot [Tason palvelusopimus](https://azure.microsoft.com/support/legal/sla/sql-database/). Katso uusimmat tiedot SQL-tietokantaan palvelutasot ja tuettujen liiketoiminnan jatkuvuuden palvelupaketin [Palvelutasot](sql-database-service-tiers.md).

**SQL Server Azure VMs käytössä**Microsoft tarjoaa saatavuus 99.95 %, joka kattaa vain virtuaalikoneen SLA. Tämä SLA eivät kuulu käynnissä olevat AM (kuten SQL Server) prosessit ja edellyttää isännöit vähintään kaksi käytettävyys määrittäminen AM esiintymät. Katso uusimmat tiedot [AM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Tietokannan hyvän käytettävyyden (HA) VMs sisällä kannattaa määrittää jokin tuettu suuren käytettävyyden asetuksista SQL Serveristä, kuten [AlwaysOn käytettävyys ryhmät](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx). Tuetut suuri käytettävyys-vaihtoehtoa käytettäessä ei voi luoda uusia SLA, mutta voit toteuttaa > 99,99 % tietokantojen käytettävyyttä.

### <a name="market"></a>Market aika

**SQL-tietokanta** on oikea ratkaisu cloud-sovelluksissa, kun tuottavuutta ja nopea aika market ovat kriittisiä. Ohjelmallisesti DBA kaltaisessa toiminnot on sopivaa cloud architects ja kehittäjille, kun se laskee edellyttää pohjana käyttöjärjestelmä ja tietokannan hallinta. Voit esimerkiksi käyttää [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) ja [PowerShellin cmdlet-komennot](http://msdn.microsoft.com/library/azure/dn546726.aspx) voit automatisoida ja hallita tietokantojen tuhansia järjestelmänvalvojan toimintoja. Ominaisuudet, kuten [Joustavasti tietokannan jakavat](sql-database-elastic-pool.md) avulla voit keskittyä sovelluskerroksen ja pitää ratkaisu markkinoilla nopeammin.

**SQL Server Azure VMs käytössä** on helppoa, jos olemassa oleva vai uusi-sovellukset edellyttävät suuria tietokantoja, toisiinsa tietokantojen tai käyttää kaikkia ominaisuuksia SQL Server-tai Windows. Kannattaa myös hyvin ratkaistavaan, kun haluat siirtää aiemmin paikalliseen sovellukset ja tietokannat kuin Azure-on. Sinun ei tarvitse esityksen, sovellusten ja tietojen kerrokset, koska säästää aikaa ja budjetti-rearchitecting aiemmin ratkaisu. Sen sijaan voit keskittyä siirtyminen kaikki ratkaisut Azure ja tekemällä joitain suorituskyvyn optimointi, joka saattaa edellyttää Azure-ympäristössä. Lisätietoja on artikkelissa [Suorituskyvyn SQL Server-Azuren näennäiskoneiden parhaat käytännöt](../virtual-machines/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Yhteenveto

Tässä artikkelissa tarkasteltavia SQL-tietokantaan ja SQL Server-Azuren näennäiskoneiden (VMs) ja käsitellyt yleisiä liiketoiminta-motivators, joka voi olla vaikutusta päätöksesi. Seuraavassa taulukossa on yhteenveto ehdotukset, jotka on otettava huomioon:

Valitse **Azure SQL-tietokanta** , jos:

- Uusi pilvipohjainen sovellusten, voit hyödyntää kustannukset säästöjen ovat luominen ja suorituskyvyn optimointi cloud services, anna. Tämän menetelmän on täysin hallitun pilvipalvelussa etuja, auttaa alemman ensimmäisen kerran,-markkinoilla ja antaa pitkään kustannukset optimointi.

- Haluat määrittää Microsoft Suorita yleisiä hallinnan tietokantoja ja Vaadi vahvempi käytettävyys palvelutasosopimuksia tietokantojen.



Valitse **SQL Server Azure VMs** , jos:

- Käytössäsi on aiemmin paikalliseen-sovelluksia, jotka haluat siirtää tai laajentaa pilveen, tai jos haluat luoda suurempi kuin 1 TT: N enterprise-sovelluksia. Tämän menetelmän on hyötyä 100 %: n SQL-yhteensopivuus, suuren tietokannan kapasiteettia, SQL Server- ja Windows täydet oikeudet, ja sen suojaamista tunneling paikalliseen. Tämän menetelmän pienentää kehittäminen ja muokata olemassa olevien sovellusten kustannuksiin.

- Olet aiemmin IT-resurssit ja voit kädessä omista korjaaminen, varmuuskopiot ja tietokannan suuren käytettävyyden. Huomaa, että jotkin automaattinen ominaisuudet yksinkertaistaa huomattavasti näistä toiminnoista. 


## <a name="next-steps"></a>Seuraavat vaiheet
- Katso [SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen Azure-portaalissa minuutteina](sql-database-get-started.md) SQL-tietokantaan, joiden avulla pääset alkuun.
- Katso [SQL-tietokantaan hinnat] (https://azure.microsoft.com/pricing/details/sql-database/).
- Katso [säännöstä SQL Server-virtual tietokoneeseen Azure-tietokannassa](../virtual-machines/virtual-machines-windows-portal-sql-server-provision.md) SQL Server Azure VMs, joiden avulla pääset alkuun.
- Katso [SQL Server Azure virtuaalikoneen: Oppimispolku](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).
