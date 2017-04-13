<properties
    pageTitle="Yleiskatsaus SQL Server Azure-Virtuaalikoneissa | Microsoft Azure"
    description="Tietoja koko SQL Server-versioiden suorittaminen Azure Virtual tietokoneissa. Suora linkkejä kaikkiin SQL Server AM kuvia ja aiheeseen liittyvää sisältöä."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Yleiskatsaus SQL Server Azure-Virtuaalikoneissa

Tässä ohjeaiheessa kerrotaan asetusten käynnissä SQL Server Azure-virtuaalikoneissa (VMs) sekä [linkit portaalin kuvat](#option-1-create-a-sql-vm-with-per-minute-licensing) ja [yleisten tehtävien](#manage-your-sql-vm)yleiskatsaus.

>[AZURE.NOTE] Jos SQL Server tutuilla ja Haluatko nähdä ottamisesta käyttöön SQL Server-AM, katso [säännöstä SQL Server-virtual tietokoneeseen Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Yleiskatsaus
Jos olet tietokannan järjestelmänvalvoja tai kehittäjä, Azure VMs lisäämistapaa paikallisen SQL Server-toiminnoista ja sovellusten pilveen. Seuraavassa videossa on SQL Server Azure VMs tekninen yleiskatsaus.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Videon käsitellään seuraavilla alueilla:

|Aika|Alue|
|---|---|
| 00:21 | Mitkä ovat Azure VMs? |
| 01:45 | Tietoturva |
| 02:50 | Yhteys |
| 03:30 | Tallennustilan luotettavuutta ja suorituskykyä |
| 05:20 | AM koot |
| 05:54 | Suuri käytettävyys ja SLA |
| 07:30 | Määritysten tuki |
| 08:00 | Seuranta |
| 08:32 | Esittely: Luo SQL Server 2016-AM |

>[AZURE.NOTE] Videon ohjeessa on SQL Server 2016, mutta Azure tarjoaa useita versioita, SQL Server 2008, 2012, 2014 ja 2016 AM kuvia. 

## <a name="scenarios"></a>Skenaariot
On useita syitä, voit halutessasi isännöidä Azure tietoja. Jos sovellus on Siirry Azure ja parantaa suorituskykyä, voit myös siirtää tiedot. Mutta ei muita etuja. Pystyt käyttämään useita yleinen tavoitettavuus- ja tietojen palauttamisen tietojen keskikohdan mukaan automaattisesti. Tiedot on myös erittäin suojatun ja kestävät.

SQL Server Azure VMs käytössä on yksi vaihtoehto Azure relaatio tietojen tallentamiseen. Se on hyvä vaihtoehto tilanteissa. Haluat esimerkiksi määrittää Azure AM samalla tavalla kuin mahdollista paikallisen SQL Server-tietokoneeseen. Tai voit suorittaa saman tietokantapalvelimen muita sovelluksia ja palveluja. On kaksi tärkeimmät resursseja, joiden avulla voit ajatella paremmin skenaariot ja huomioon otettavia seikkoja:

 - [Azuren näennäiskoneiden SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) on paras skenaarioiden käyttämällä SQL Server Azure VMs yleisiä tietoja. 
 - [Pilvestä SQL Server-vaihtoehdon valitseminen: SQL Azure (PaaS)-tietokanta tai SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) sisältää yksityiskohtaiset vertailun SQL-tietokantaan ja SQL Serveriä AM.

## <a name="create-a-new-sql-vm"></a>Luo uusi SQL-AM
Seuraavissa osissa on SQL Server virtuaalikoneen valikoima kuvien suorat linkit Azure-portaaliin. Sen mukaan, voit valita kuvan voit joko maksamisen SQL Serverin kustannukset kohti minuutin välein-käyttöoikeuksien myöntämisestä tai voit tuoda Omat käyttöoikeuden (BYOL).

Etsi tämän prosessin vaiheittaiset ohjeet opetusohjelmassa [säännöstä SQL Server-virtual tietokoneeseen Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md). Tarkista myös [suorituskyvyn SQL Server VMs parhaita käytäntöjä](virtual-machines-windows-sql-performance.md), joissa kerrotaan, miten voit valita haluamasi tietokoneen koko ja muita ominaisuuksia valmistelun aikana.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Vaihtoehto 1: Luo SQL-AM minuutissa käyttöoikeudet
Seuraavassa taulukossa on käytettävissä SQL Server-kuvia virtuaalikoneen valikoiman matriisi. Valitse Aloita luomalla uuden SQL-AM määritettyä versiota, edition ja käyttöjärjestelmän linkin.

|Versio|Käyttöjärjestelmä|Edition|
|---|---|---|
|**SQL-2016**|Windows Server 2012 R2|[Yrityksen](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2) [Vakio](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2) [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [keskihajonta](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL-2014 SP1**|Windows Server 2012 R2|[Yrityksen](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Vakio](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2)- [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2)- [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL-2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2)- [standardin](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2) [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Yrityksen](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Vakio](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2)- [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2)- [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2)- [standardin](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2) [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Yrityksen](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Vakio](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012)- [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012)- [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2: N SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2)- [standardin](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2) [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2: N SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Vaihtoehto 2: Luo SQL-AM aiemmin käyttöoikeuksin
Voit myös tuoda Omat käyttöoikeuden (BYOL). Tässä skenaariossa maksat vain ilman mitään muita kulujen SQL Server-lisensointi AM. Haluat käyttää omia käyttöoikeus, käyttää matriisissa SQL Server-versiot, versiot ja käyttöjärjestelmät alla. Portaalissa nämä kuva nimet ovat etuliite **{BYOL}**.

|Versio|Käyttöjärjestelmä|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Yrityksen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Vakio BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Yrityksen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Vakio BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Yrityksen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Vakio BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Voit käyttää BYOL AM kuvia, jos sinulla on ja Enterprise Agreement-sopimus [Käyttöoikeuden Mobility Azure Software Assurance](https://azure.microsoft.com/pricing/license-mobility/)kanssa. Tarvitset myös SQL Server, jota haluat käyttää version/version voimassa oleva käyttöoikeus. Sinun täytyy [BYOL tarvittavat tiedot Microsoftille](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) valmistelu oman AM **10** päivän kuluessa.

## <a name="manage-your-sql-vm"></a>SQL-AM hallinta
Valmistelun SQL Server-AM jälkeen on useita valinnainen hallintatehtäviä. Monia määrittäminen ja hallinta SQL Server tarkalleen siinä muodossa kuin hallita paikallisen SQL Server-esiintymän. Jotkin tehtävät ovat kuitenkin tietyn Azure. Seuraavissa osissa on korostettu joillakin alueilla on linkkejä lisätietoihin.

### <a name="connect-to-the-vm"></a>Yhteyden muodostaminen AM
Yksi yleisin hallinta-ohjeita on muodostaa yhteyden SQL Server-AM työkaluja, kuten SQL Server Management Studiossa (SSMS) kautta. Ohjeita siitä, miten voit muodostaa yhteyden oman uuden SQL Server-AM on ohjeaiheessa [yhteyden, SQL Server Virtual tietokoneen Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Tietojen siirtäminen
Jos sinulla on aiemmin luotuun tietokantaan, kannattaa siirtää, juuri valmistellun SQL-AM. Luettelo siirron asetukset ja ohjeita on artikkelissa [siirtäminen SQL Server Azure-AM-tietokantaan](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Suuren käytettävyyden määrittäminen
Jos asetat suuren käytettävyyden, harkitse määrittäminen SQL Server-käytettävyys ryhmät. Tämä edellyttää useita Azure VMs virtual verkossa. Azure-portaalissa on malli, joka määrittää määritysten puolestasi. Lisätietoja on artikkelissa [määrittäminen AlwaysOn käytettävyys-ryhmän Azure Resurssienhallinta näennäiskoneiden](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Määritä manuaalisesti käytettävyys ryhmän ja liittyvän listener ohjeartikkelissa [AlwaysOn käytettävyys-ryhmien määrittäminen Azure AM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Katso suuren käytettävyyden muuta huomioonotettavaa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Varmuuskopioi tiedot
Azure VMs voit hyödyntää [Automaattisia varmuuskopiointi](virtual-machines-windows-sql-automated-backup.md), joka luo säännöllisesti varmuuskopiot tietokantasi Blob-objektien tallennustilaan. Voit käyttää tätä tapaa myös manuaalisesti. Lisätietoja on artikkelissa [Käytä Azure tallennustila SQL Server varmuuskopiointi ja palauttaminen](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Yleiskatsaus kaikki varmuuskopiointi- ja asetukset-kohdassa [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatisoida päivitykset
Azure VMs [Automaattinen korjaaminen](virtual-machines-windows-sql-automated-patching.md) järjestää avulla ylläpito-ikkuna, jossa tärkeitä Windowin ja SQL Server päivittyy automaattisesti.

### <a name="customer-experience-improvement-program-ceip"></a>Osallistu Käyttömukavuuden kehitysohjelmaan (CEIP)
Oletusarvon mukaan käytössä ohjelma (Käyttömukavuuden). Raporttien lähettää säännöllisesti Microsoft SQL Server parantamiseen. Ei ole hallinta tehtävän pyytää Käyttömukavuuden Kehitysohjelman paitsi, jos haluat poistaa käytöstä valmistelun jälkeen. Voit mukauttaa tai poistaa Käyttömukavuuden yhdistämällä AM etätyöpöydän kanssa. Suorita **SQL Server-virhe ja käyttöraportin** -apuohjelma. Raportoinnin käytöstä ohjeiden mukaan. 

Saat lisätietoja [Hyväksy käyttöoikeusehdot](https://msdn.microsoft.com/library/ms143343.aspx) aiheen kohdassa Käyttömukavuuden Kehitysohjelman. 

## <a name="next-steps"></a>Seuraavat vaiheet
[Tutustu Oppimispolkuun](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) Azuren näennäiskoneiden SQL Server.

Lisää kysymys? Katso ensin [SQL Server Azure näennäiskoneiden usein kysytyt kysymykset](virtual-machines-windows-sql-server-iaas-faq.md). Mutta myös lisätä kysymyksensä tai kommenttinsa SQL AM aiheita ottaa yhteyttä Microsoftin ja yhteisön alareunaan.
