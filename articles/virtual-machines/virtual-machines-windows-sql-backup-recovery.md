<properties
    pageTitle="Varmuuskopioiminen ja palauttaminen SQL Serverin | Microsoft Azure"
    description="Artikkelissa käsitellään varmuuskopiointi ja palauttaminen käynnissä-Azuren näennäiskoneiden SQL Server-tietokannat."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Azuren näennäiskoneiden SQL Server palauttaminen ja varmuuskopiointi

## <a name="overview"></a>Yleiskatsaus

SQL Server-tietokannan tietojen varmuuskopioinnin on kohdassa suojautuminen tietojen menettämisen sovelluksen tai käyttäjän virheiden vuoksi strategia tärkeä osa. Tämä koskee myös SQL Serveriä-Azuren näennäiskoneiden (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

SQL Server Azure VMs käynnissä alkuperäisen varmuuskopiointi ja palauttaminen käyttämällä levyjen varmuuskopiot kohteen tekniikoita. On kuitenkin levyjä, voit liittää Azure virtual-koneen [virtuaalikoneen koon](virtual-machines-linux-sizes.md)perusteella määrän rajoitukset. On myös Levynhallinta kannattaa katseltavan.

SQL Server 2014 alkaen voit varmuuskopioida ja palauttaa Microsoft Azure-Blob-säiliö. SQL Server 2016 sisältää myös parannuksia tämän vaihtoehdon. Lisäksi tietokantatiedostojen Microsoft Azure Blob-objektien tallennustilaan tallennettuja, SQL Server 2016 tarjoaa vaihtoehto silmänräpäyksessä varmuuskopioiden hakeminen ja nopean palauttaa Azure tilannevedosten käyttäminen. Tässä artikkelissa on yleisiä tietoja näistä asetuksista ja lisätietoja tukikäytännöistä [SQL Serverin varmuuskopiointi- ja Microsoft Azure Blob Storage-palvelun kanssa](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Katso keskustelu vaihtoehdoista varmuuskopioiminen hyvin suuria tietokantoja, [Usean teratavun SQL Serverin tietokannan varmuuskopiointi strategioita Azuren näennäiskoneiden](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Seuraavissa osissa sisältää SQL Server Azure virtuaalikoneen tueta eri versioiden tiedot.

## <a name="sql-server-virtual-machines"></a>SQL Server-näennäiskoneiden

Kun SQL Server-esiintymän oli käynnissä Azure Virtual Machine-tietokantatiedostojen jo sijaitsevat tiedot levyille Azure. Näiden levyjen live Azure-Blob-objektien tallennustilaan. Niin syyt varmuuskopioiminen tietokannan ja tavoista voit ottaa muuta hieman. Ota huomioon seuraavat. 

- Et enää tarvitse tietokannan varmuuskopioiden vaarallisilta laitteiston tai media koska Microsoft Azure tämän suojaa osana Microsoft Azure-palvelua.

- Haluat suojata käyttäjän virheitä tai arkistointia tarkoituksiin, säädöksellisiä syitä tai hallinnointia tietokannan varmuuskopioiden.

- Voit tallentaa varmuuskopiotiedoston suoraan Azure. Lisätietoja on seuraavissa kohdissa, joissa on SQL Server eri versioiden ohjeita.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 tukee SQL Server 2014 [Varmuuskopiointi ja palauttaminen ja Azure BLOB-objektit](https://msdn.microsoft.com/library/jj919148.aspx) ominaisuuksia. Mutta se on myös seuraavilla parannuksilla:

| 2016 parannus               | Tiedot                          |
|---------------------|-------------------------------|
| **Raitasarjoittamista**              | Kun varmuuskopioiminen Microsoft Azure-blob-säiliö, SQL Server 2016 tukee useita BLOB käyttöön suuria tietokantoja, enintään 12.8 TT varmuuskopioiminen varmuuskopion.      |
| **Tilannevedoksen varmuuskopiointi**                | Käyttämällä Azure tilannevedoksia SQL Server-tiedosto tilannevedoksen varmuuskopion on silmänräpäyksessä varmuuskopiot ja nopean palauttaa tietokannan tiedostojen tallennettu Azure Blob storage-palvelun avulla. Tämän ominaisuuden avulla voit yksinkertaistaa varmuuskopiointi ja palauttaminen käytännöt. Tiedoston tilannevedoksen varmuuskopion tukee myös pisteen aika palauttaminen. Lisätietoja on artikkelissa [Tilannevedoksen varmuuskopioiden tietokantatiedostojen Azure-tietokannassa](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Hallitun varmuuskopioinnin ajoitus**            | SQL Server hallitun varmuuskopio Azure tukee nyt mukautetun aikatauluja. Lisätietoja on artikkelissa [SQL Server hallitun varmuuskopio Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Opetusohjelmaan SQL Server 2016 Azure-Blob-säiliö käytettäessä ominaisuuksia, katso [Opetusohjelma: Microsoft Azure Blob storage-palvelun käyttäminen SQL Server 2016 tietokantojen](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014

SQL Server 2014 sisältää seuraavat parannukset:

1. **Varmuuskopiointi ja palauttaminen Azure**:

 - *SQL Server Backup URL-osoite* on nyt tuki SQL Server Management Studiossa. Voit varmuuskopioida Azure on nyt saatavilla, kun käytät varmuuskopioinnin tai palauttamisen tehtävä tai ylläpito suunnitelman ohjattua SQL Server Management Studiossa. Lisätietoja on artikkelissa [SQL Server Backup URL-osoitteeseen](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server hallitun varmuuskopiointi Azure* on uusia toimintoja, joka mahdollistaa automaattisen varmuuskopioinnin hallinta. Tämä on erityisen hyödyllinen automatisointi varmuuskopion hallinta SQL Server 2014 esiintymien Azure-tietokoneessa käynnissä. Lisätietoja on artikkelissa [SQL Server hallitun varmuuskopio Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automaattisen varmuuskopioinnin* on muita automaatio automaattisesti käyttöön *SQL Server hallitun varmuuskopio Azure* aiemmin luoduissa että uusissa piilotetusta, SQL Server-AM Azure-tietokannassa. Lisätietoja on artikkelissa [Automaattisen varmuuskopioinnin SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-automated-backup.md).
 - Yleiskatsaus SQL Server 2014 varmuuskopio Azure kaikki asetukset-kohdassa [SQL Server varmuuskopiointi ja palauttaminen Microsoft Azure Blob Storage-palvelun kanssa](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Salaus**: SQL Server 2014 tukee salaamaan tiedot, kun luot varmuuskopion. Tukee useita salausalgoritmit ja käytä osf varmenteen tai julkiseen-näppäintä. Lisätietoja on artikkelissa [Varmuuskopiointi-salausta](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012

Katso lisätietoja SQL Serverin varmuuskopiointi- ja SQL Server 2012: n [Varmuuskopiointi ja palauttaminen, SQL Server-tietokannat (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

SQL Server 2012 SP1: n kumulatiivisen päivityksen 2 alkaen voit takaisin enintään ja palauttaa Azure-Blob-säiliö-palvelusta. Tämä laajentamisesta voidaan varmuuskopioida SQL Server-tietokannat palvelimessa SQL Azure-virtuaalikoneen tai paikallisen esiintymä. Lisätietoja [SQL Serverin varmuuskopiointi- ja Azure Blob Storage-palvelussa](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Azure Blob storage-palvelun eduista muun muassa seuraavat voi ohittaa 16 levyn rajoitus levyjen, helpottaa hallinta-suora käytettävyys varmuuskopiotiedoston esiintymässä SQL Server-esiintymän Azure virtuaalikoneen käytössä tai erillisen paikallisen siirron tai tietojen palauttamista varten. Täydellisen luettelon toimintoja käyttämällä Azure blob storage-palvelu SQL Serverin varmuuskopioiden hakeminen [SQL Server varmuuskopiointi ja palauttaminen Azure Blob Storage Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)-kohdassa *etuja* .

Parhaiden käytäntöjen suosituksia ja vianmääritystietoja on artikkelissa [Varmuuskopiointi ja palauttaminen parhaita käytäntöjä (Azure Blob Storage Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008:

Lisätietoja SQL Serverin varmuuskopiointi ja palauttaminen SQL Server 2008 R2, [varmuuskopioinnista ja SQL Server (SQL Server 2008 R2) palauttaminen tietokannoissa](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server-varmuuskopiointi ja palauttaminen SQL Server 2008: ssa on artikkelissa [varmuuskopioinnista ja SQL Server (SQL Server 2008) palauttaminen tietokannoissa](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

Jos suunnittelet käyttöönoton, SQL Server Azure-AM, löydät valmistelu ohjeet seuraavat opetusohjelmassa: [SQL Server Virtual kone Azure Azure resurssien hallinta-toiminnon](virtual-machines-windows-portal-sql-server-provision.md).

Varmuuskopiointi ja palauttaminen avulla voidaan siirtää tietoja, on SQL Server Azure-AM mahdollisesti helpottaa tietojen siirron polkuja. Katso siirron asetukset ja suositusten koko keskustelun ja [siirtäminen SQL Server Azure-AM-tietokantaan](virtual-machines-windows-migrate-sql.md).

Tarkista muut [resurssit SQL Serveriä Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).
