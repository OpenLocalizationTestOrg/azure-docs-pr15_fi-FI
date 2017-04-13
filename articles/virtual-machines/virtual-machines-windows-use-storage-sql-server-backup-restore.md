<properties
    pageTitle="Käytä Azure tallennustilan SQL Serverin varmuuskopiointi ja palauttaminen | Microsoft Azure"
    description="Lue, miten voit varmuuskopioida SQL Server Azure-tallennustilan. Tässä artikkelissa kerrotaan edut varmuuskopioimalla SQL-tietokantoja Azure-tallennustilan."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Käytä Azure tallennustilan SQL Server varmuuskopiointi ja palauttaminen

## <a name="overview"></a>Yleiskatsaus

SQL Server 2012 SP1 CU2 alkaen voit nyt kirjoittaa SQL Serverin varmuuskopioiden suoraan Azure-Blob-tallennuspalvelu. Voit käyttää tätä toimintoa takaisin enintään ja palauttaa paikallisen SQL Server-tietokantaan tai SQL Server-tietokantaan, valitse Azure virtuaalikoneen Azure-Blob-palvelusta. Cloud varmuuskopiointi tarjoaa edut käytettävyyden, rajattomasti geo replikoida sähköntuotannosta tallennustilan ja tiedot ja siirron helpottaminen pilvestä. Voit myöntää varmuuskopiointi ja palauttaminen lauseet Transact-SQL- tai SMO avulla.

SQL Server 2016 esitellään uusia ominaisuuksia; Voit suorittaa silmänräpäyksessä varmuuskopiot ja erittäin nopeasti palauttaa [tiedoston tilannevedoksen varmuuskopion](http://msdn.microsoft.com/library/mt169363.aspx) .

Tässä ohjeaiheessa kerrotaan, miksi voi valita, haluatko käyttää Azure tallennustilan SQL varmuuskopioiden hakeminen ja kuvataan liittyvät komponentit. Tämän artikkelin lopussa resurssien avulla voit käyttää vaiheittaiset ohjeet ja lisätietoja tämän palvelun käyttäminen SQL Server-varmuuskopiot.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Azure-Blob-palvelun eduista SQL Server varmuuskopioiden hakeminen

On useita haasteisiin, kun SQL Server varmuuskopioiminen yleisölle tarkoitetun. Näihin haasteisiin Sisällytä tallennusvälineiden hallinta-tallennustilan virheen pääsy sähköntuotannosta varasto ja laitekokoonpanon riskiä. Monia näistä haasteita on osoitettu varmuuskopioiden SQL Server Azure-Blob-säilöpalvelun avulla. Ota huomioon seuraavat edut:

- **Käyttöä helpottavat**: tallentaminen varmuuskopiot Azure BLOB voi olla kätevä, joustava ja helppo käyttää sähköntuotannosta-vaihtoehtoa. SQL Server-varmuuskopiot sähköntuotannosta tallennustila luominen voi olla yhtä helppoa kuin muokkaamalla aiemmin komentosarjojen/työt käyttämään **Varmuuskopiointi-URL** -syntaksia. Sähköntuotannosta tallennustilan yleensä pitäisi olla riittävän kaukana tuotannon tietokannan sijainti, johon haluat estää yhden huono, jotka saattavat vaikuttaa sekä sähköntuotannosta ja tuotannon tietokannan sijainnit. Valitsemalla [geo toisinto oman Azure-BLOB](../storage/storage-redundancy.md)on ylimääräisiä kerroksen, jotka saattavat vaikuttaa koko alueen tietojen suojaamisesta.
- **Varmuuskopiointi-arkisto**: Azure-Blob-objektien tallennustilaan palvelussa usein käytettyjen nauha-vaihtoehto, jos haluat arkistoida varmuuskopioiden parempi vaihtoehto. Nauha tallennustilan saattaa edellyttää fyysinen kuljetus suojaaminen mediatiedostojen sähköntuotannosta tilojen ja toimenpiteet. Tallentaminen varmuuskopiot Azure-Blob-säiliö tarjoaa hetkessä, erittäin käytettävissä durable arkistointi vaihtoehto.
- **Hallitut laitteiston**: ei ole katseltavan laitteiston hallinnan Azure-palvelujen kanssa. Azure palveluiden hallinta laitteiston ja anna geo replikoinnin redundancy ja laitteisto suojautumista.
- **Rajoittamaton tallennustilan**: ottamalla Azure BLOB välitön varmuuskopiointi pystyt käyttämään lähes rajoittamattoman tallennustilan. Voit myös Azure virtuaalikoneen-levyn varmuuskopioiminen enintään on rajoitukset koneen koon perusteella. Ei levyä, voit liittää Azure virtual-koneen varmuuskopioiden hakeminen määrän rajoitukset. Tämä rajoitus on erittäin suuri erillisen 16 levyjen ja vähemmän pienempi esiintymien.
- **Varmuuskopiointi käytettävyys**: Azure BLOB tallennettuja varmuuskopioita ovat käytettävissä missä tahansa ja milloin tahansa ja niitä voi käyttää helposti palauttaa paikallisen SQL Serveriä tai toiseen SQL Server-Azure Virtual Machine-ilman edellyttää tietokannan liittäminen tai irrottaminen käynnissä tai lataamalla ja liittämisestä Näennäiskiintolevyn varten.
- **Kustannukset**: maksat palvelu, jota käytetään. Voi olla edullinen sähköntuotannosta ja varmuuskopion arkisto-asetus. Lue [Azure hinnoittelu Laskimen](http://go.microsoft.com/fwlink/?LinkId=277060 "Hinnat Laskimen")ja [Azure hinnat artikkelissa](http://go.microsoft.com/fwlink/?LinkId=277059 "hinnat artikkelissa") lisätietoja.
- **Tallennustilan tilannevedoksia**: kun tietokantatiedostot on tallennettu Azuren Blob-objektien ja käytössäsi on SQL Server 2016, voit käyttää [tiedoston tilannevedoksen varmuuskopion](http://msdn.microsoft.com/library/mt169363.aspx) suorittamaan silmänräpäyksessä varmuuskopiot ja erittäin nopeasti palauttaa.

Lisätietoja on artikkelissa [SQL Serverin varmuuskopiointi- ja Azure Blob Storage-palvelussa](http://go.microsoft.com/fwlink/?LinkId=271617).

Kaksi osaa esitellä Azure Blob storage-palvelua, mukaan lukien tarvittavat SQL Server-osat. On tärkeää ymmärtää, osat ja niiden vuorovaikutusta onnistuneesti varmuuskopiointi ja palauttaminen Azure Blob storage-palvelusta.

## <a name="azure-blob-storage-service-components"></a>Azure-Blob Storage palvelun osat

Seuraavat Azure osat käytetään, kun varmuuskopioiminen Azure Blob storage-palveluun.

| Osa               | Kuvaus                          |
|---------------------|-------------------------------|
| **Tallennustilan tilin** | Tallennustilan tili on tallennustilan palvelun aloituskohdan. Azure-Blob-säiliö-palvelun käyttöön luominen Azure-tallennustilan tilin. Lisätietoja Azure Blob storage palvelua Tutustu [Azure Blob Storage-palvelun käyttöä varten](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Säilö** | Säilön on joukko BLOB ryhmittely ja voidaan tallentaa BLOB rajoittamaton määrä. Kirjoittaminen SQL Server varmuuskopion Azure-Blob-palveluun, sinun on oltava vähintään pääkansion säilön luotiin. |
| **BLOB** | Tiedosto, jonka kaikki tyyppi ja koko. BLOB-objektit ovat käytettävissä seuraavassa URL-osoite muodossa: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Saat lisätietoja sivun BLOB [tietoja lohko ja sivun BLOB-objektit](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-osat

Seuraavat SQL Server-osat käytetään, kun varmuuskopioiminen Azure Blob storage-palveluun.

| Osa               | Kuvaus                          |
|---------------------|-------------------------------|
| **URL-OSOITE** | URL-Osoitteen määrittää tunnus URI (Uniform Resource) yksilöllinen varmuuskopion tiedostoon. URL-Osoitetta käytetään sijainti ja SQL Server-varmuuskopiotiedoston nimi. URL-osoite on osoitettava todellinen blob, ei pelkästään säilön. Jos blob ei ole, se luodaan. Jos aiemmin Blob-objektien on määritetty, varmuuskopiointi epäonnistuu, ellei > muodon jossa valitsinta. Seuraavassa on esimerkki varmuuskopiointi-komennon määrität URL-osoite: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS suositellaan, mutta sitä ei tarvita. |
| **Tunnistetietojen** | Muodosta ja Azure Blob storage palveluun todennetaan tarvittavat tiedot tallennetaan olevan tunnistetiedon.  SQL Server-tunnistetiedot on luotava SQL Serverin kirjoittaa varmuuskopioiden Azure-Blob- tai palauta se järjestyksessä. Lisätietoja on artikkelissa [SQL Server-tunnistetiedot](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Jos haluat kopioida ja lataa varmuuskopio Azure Blob storage-palveluun, sinun on käytettävä tallennustilan-asetus sivun blob-tyyppi, jos aiot käyttää tätä tiedostoa palauttaminen toimintoja varten. Estä blob-tyypin palauttaa epäonnistuu virheen vuoksi.

## <a name="next-steps"></a>Seuraavat vaiheet

1. Luo Azure-tili, jos sinulla ei ole vielä yksi. Jos arvioit Azure, kannattaa [maksuton](https://azure.microsoft.com/free/).

1. Valitse jokin seuraavista opetusohjelmia, jotka käydä läpi tekemistä palautuksen ja tallennustilaa tilin luominen.

    - **SQL Server 2014**: [Opetusohjelma: SQL Server 2014 varmuuskopiointi ja palauttaminen Microsoft Azure Blob Storage palvelun](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [Opetusohjelma: Microsoft Azure Blob storage-palvelun käyttäminen SQL Server 2016 tietokantojen](https://msdn.microsoft.com/library/dn466438.aspx)

1. Tarkista [SQL Serverin varmuuskopiointi- ja Microsoft Azure Blob Storage-palvelun kanssa](https://msdn.microsoft.com/library/jj919148.aspx)alkaen lisäohjeita.

Jos sinulla on ongelmia, lue ohjeaihe [SQL Server varmuuskopio URL-Osoitteen parhaiden käytäntöjen ja vianmääritys](https://msdn.microsoft.com/library/jj919149.aspx).

Muiden SQL Serverin varmuuskopiointi ja palauttaminen asetukset, katso [Varmuuskopiointi- ja SQL Server Azuren näennäiskoneiden](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
