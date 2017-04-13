<properties
    pageTitle="Suorituskyvyn parhaat käytännöt SQL Serverin | Microsoft Azure"
    description="SQL Server optimoinnista Microsoft Azure VMs tarjoaa parhaita käytäntöjä."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt

## <a name="overview"></a>Yleiskatsaus

Tämä artikkeli sisältää SQL Server optimoinnista Microsoft Azure Virtual Machine parhaat käytännöt. Kun käytät Azuren näennäiskoneiden SQL Server-Suosittelemme edelleen käyttää samaa tietokannan suorituskyvyn säätö SQL Server paikallisen server-ympäristössä käytettävissä olevat vaihtoehdot. Kuitenkin relaatiotietokannasta julkisen pilvipohjaisia suorituskyky määräytyy monet eri tekijät, kuten virtual machine koon ja tietojen levyjä määritys.

Kun luot SQL Server-kuvia, [harkitse valmistelu oman VMs Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md). SQL Server VMs valmisteltu resurssien hallinta-portaalissa toteuttaa kaikki näitä parhaita käytäntöjä, mukaan lukien tallennustilan kokoonpanon.

Tässä artikkelissa on kohdistettu Saapuminen *parhaan* suorituskyvyn SQL Server Azure VMs. Jos havainnollistamiseen päivityshistorian, joka optimointi alla eivät ehkä tarvitse. Harkitse suorituskyvyn tarpeet ja työmäärää kuviot arvioit suositukset.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Nopea tarkistusluettelo

Seuraavassa on lyhyt tarkistusluettelo suorituskyvyn SQL Server-Azuren näennäiskoneiden:

|Alue|Optimointi|
|---|---|
|[AM kokoa](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) tai uudempi versio SQL Enterprise Edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) tai uudempi versio SQL vakio- ja Web-versioissa.|
|[Tallennustilan](#storage-guidance)|Käytä [Premium-tallennustilan](../storage/storage-premium-storage.md). Vakio tallennustilan suositellaan vain keskihajonta-ehtoa.<br/><br/>[Tallennustilan tilin](../storage/storage-create-storage-account.md) ja SQL Server AM pitäminen samalla alueella.<br/><br/>Poista käytöstä Azure [geo tarpeettomat storage](../storage/storage-redundancy.md) (replikoinnin geo) tallennustilan tilin.|
|[Levyjen](#disks-guidance)|Käytä vähintään 2 [P30 levyjen](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1: n lokitiedostojen; 1 datatiedostot ja TempDB).<br/><br/>Tallentaminen tai kirjaaminen käyttöjärjestelmän tai tilapäinen levyjä käytön välttäminen<br/><br/>Ota käyttöön lukea välimuistiin isännöinnin datatiedostot ja TempDB näiden levyjen asemasta.<br/><br/>Älä ota välimuistiin tallentamisen levyt isännöinnin lokitiedosto.<br/><br/>Tärkeää: Lopettaa SQL Server-palvelu, kun Azure AM-levyn välimuistin asetuksia.<br/><br/>STRIPE useita Azure tietojen levyjä saat paremman IO siirtonopeuden.<br/><br/>Muoto, jossa on kuvattu kohdistus koot.|
|[I/O](#io-guidance)|Tietokannan-sivun pakata.<br/><br/>Ota käyttöön pikaviestien tiedoston alustus datatiedostoja varten.<br/><br/>Rajaa tai poistaa käytöstä tietokannan kasvamaan automaattisesti.<br/><br/>Poista käytöstä autoshrink tietokannan.<br/><br/>Siirrä kaikki tietokannat tietojen levyjä, mukaan lukien järjestelmän tietokannat.<br/><br/>Siirtää tietoja levyjen SQL Serverin virhe loki- ja tiedoston kansioita.<br/><br/>Määritä oletusarvoinen varmuuskopiointi- ja sijainnit.<br/><br/>Lukitun sivut käyttöön.<br/><br/>Käytä SQL Serverin suorituskyvyn korjaukset.|
|[Ominaisuus erikois](#feature-specific-guidance)|Varmuuskopioi suoraan Blob-objektien tallennustilaan.|

Lisätietoja *siitä, miten* ja *Miksi* tekemään näitä optimointi Tarkista tiedot ja seuraavissa osissa esitettyjä ohjeita.

## <a name="vm-size-guidance"></a>AM koon ohjeet

Suorituskyvyn luottamuksellisia sovelluksia on suositeltavaa, että käytät seuraavia [näennäiskoneiden koot](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 tai uudempi versio

- **SQL Server Standard- ja Web-versiot**: DS2 tai uudempi versio


## <a name="storage-guidance"></a>Tallennustilan ohjeet

DS-sarjan (sekä DSv2 sarjan ja GS sarjan) VMs tuki [Premium-tallennustilan](../storage/storage-premium-storage.md). Premium tallennustilan suositellaan kaikki tuotannon työmäärät.

> [AZURE.WARNING] Vakio tallennustila on erilaiset viiveet suurempia ja kaistanleveys ja suositellaan vain keskihajonta/testi toiminnoista. Tuotannon työmääriä käytettävä Premium-tallennustilan.

Lisäksi on suositeltavaa, että luot saman data Centerissä oman SQL Server-näennäiskoneiden vähentämään viiveitä siirto Azure-tallennustilan tilin. Kun tallennustilan tilin luominen käytöstä geo replikoinnin sellaisina yhdenmukaisia kirjoittaminen tilauksen useille levyille on ei. Harkitse sen sijaan määrittäminen SQL Serverin tietojen palauttaminen tekniikka välillä kahden Azure tietolähteen keskikohdan mukaan. Lisätietoja on artikkelissa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Levyjen ohjeet

Valitse Azure-AM on tärkeimmät levyn kolmenlaisia:

- **OS levyn**: luodessasi Azure-virtuaalikoneen platform liittäminen (merkintä **C** -asema) vähintään yksi levy käyttöjärjestelmän levyn AM. Tämä levy on tallennettu sivun Blob-objektien tallennustilaan Näennäiskiintolevyn.
- **Tilapäinen levyn**: Azuren näennäiskoneiden sisältävät toisen tilapäinen levyn nimi levyn ( **D**merkintä: asema). Tämä on DVD-levyllä, joka voi käyttää työtilan solmun.
- **Tietoja levyjen**: Voit myös liittää muita levyjä virtuaalikoneen kuin tietojen levyille ja nämä tallennetaan tallennustilaan sivun BLOB-objektit.

Seuraavissa kohdissa kuvataan näiden eri levyjen suosituksia.

### <a name="operating-system-disk"></a>Käyttöjärjestelmän levy

Käyttöjärjestelmä-levy on, voit käynnistää ja ottaa käyttöön käyttöjärjestelmän käynnissä versiona Näennäiskiintolevyn ja **C** -asema merkintä.

Välimuistin käytännön käyttöjärjestelmän levyn oletusarvo on **Luku-/ kirjoitusoikeudet**. Suorituskyvyn luottamuksellisia sovellukset Suosittelemme tietojen levyjä käyttää sen sijaan, että käyttöjärjestelmän levy. On kohdassa tietoja levyille alla.

### <a name="temporary-disk"></a>Tilapäinen levy

Väliaikaisten-asema, **D**merkintä: asema, ei säilytetä Azure-blob-säiliö. Älä tallenna käyttäjän tietokantatiedostoja tai käyttäjän tapahtuman lokitiedostojen **D**: asema.

D-sarjan, Dv2 sarjan ja G-sarjan VMs nämä VMs tilapäinen asema, perustuu Suoritettaessa. Jos havainnollistamiseen hyödyntää paksu TempDB (esimerkiksi väliaikaisia objekteja tai monimutkaisia liitokset) tallentaminen TempDB **D** -asemassa saattaa johtaa suurempiin TempDB siirtonopeuden ja pienempi TempDB viive.

VMs, jotka tukevat Premium Storage (DS-sarjan, DSv2 sarjan ja GS sarja) Suosittelemme TempDB ja/tai puskurin resurssivarantoon tunnisteet tallentamisesta DVD-levyllä, joka tukee Premium tallennustilan luku välimuisti käytössä. On poikkeuksena tämä suositus; Jos TempDB-käyttö on paljon kirjoittaminen, voit toteuttaa paremman suorituskyvyn tallentamalla TempDB **D** paikallisesta asemasta, joka on myös Suoritettaessa-pohjaisen tietokoneen nämä koot käyttöön.

### <a name="data-disks"></a>Tietoja levyjen

- **Tietojen ja lokitiedostojen levyjen tietojen käyttäminen**: vähintään 2 Premium tallennustilan [P30 levyjen](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) jossa yksi levykkeellä lokitiedostojen ja toinen sisältää tietoja-tiedostot ja TempDB käyttää.

- **Levyn raitasarjoittamista**: Lisää siirtonopeuden voit lisätä muita tietoja levyjä ja käyttää levyn raitasarjoittamista. Määrittääksesi tietojen levyjen määrä haluat analysoida IOPS käytettävissä tiedot ja kirjaudu levyiltäsi määrä. Nämä tiedot artikkelissa taulukot IOPS [AM tiedostokoko](virtual-machines-windows-sizes.md) ja koon seuraavassa artikkelissa: [Levyjen Premium tallennustilan avulla](../storage/storage-premium-storage.md). Käytä seuraavia ohjeita:

    - Käytä Windows 8: ssa ja Windows Server 2012 tai uudempi versio, [Tallennustilan välilyöntejä](https://technet.microsoft.com/library/hh831739.aspx). Määritä raitakoko 64 Kilotavua, ja OLTP toiminnoista ja 256 Kilotavua, tietojen laajemmissa toiminnoista ja välttää suorituskykyyn vaikuttavia osion kohdistusvirhe vuoksi. Määritä lisäksi sarakkeiden määrä = fyysisten levyjen määrä. Määrittää yli 8 levyjen tallennustilan tarkistaminen (ei palvelimen hallinnan UI) PowerShellin avulla on määritetty vastaamaan levyjen määrä sarakkeiden määrä. Katso lisätietoja [Tallennustilan välilyöntejä](https://technet.microsoft.com/library/hh831739.aspx)määrittämisestä [Windows PowerShellin tallennustilan välilyöntejä cmdlet-komennoista](https://technet.microsoft.com/library/jj851254.aspx)

    - Windows 2008 R2 tai aiemmin voit käyttää dynaamisiksi (OS Raidallinen asemat) ja raitakoko on aina 64 Kilotavua. Huomaa, että tämä asetus on poistettu saavuttaman Windows 8: ssa ja Windows Server 2012. Lisätietoja on artikkelissa osoitteessa [Virtual levy-palvelu on siirtymisessä Windows tallennustilan hallinta API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx)tuki-lause.

    - Jos havainnollistamiseen ei ole kirjautunut paljon resursseja ja erillinen IOPs ei tarvita, voit määrittää yhden tallennustilan resurssivarantoon. Muussa tapauksessa luo kaksi tallennustilan jakavat yhden lokitiedostojen ja toinen tallennustilan resurssivarannon tiedot tiedostot ja TempDB. Määrittää kuormituksen odotuksiasi perusteella tallennustilan kunkin ryhmän liittyvät levyjen määrä. Muista, että AM erikokoisia Salli eri määrän liittyvien tietojen levyjä. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-windows-sizes.md).

    - Jos et käytä Premium Storage (keskihajonta/testi skenaariot), suositus on Lisää enimmäismäärä on tietoja levyjen tukemat oman [AM kokoa](virtual-machines-windows-sizes.md) ja käytä levyn raitasarjoittamista.

- **Välimuistin käytännön**: Premium tallennustilan tietojen levyjä, ota käyttöön luku välimuisti isännöinnin datatiedostot ja TempDB vain tiedot levyille. Jos et käytä Premium tallennustilaa, älä ota kaikki välimuistiin mitään tietoja levyille. Ohjeita määrittäminen levyn välimuistiin, on ohjeaiheissa on seuraavissa artikkeleissa: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) ja [Määritä AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Lopeta SQL Server-palvelu, kun välimuisti-asetuksen muuttaminen Azure AM levyjen minkä tahansa tietokannan vioittumisen vahinkojen välttämiseksi.

- **NTFS kohdistus kokoa**: tietojen levyn muotoilussa on suositeltavaa, että käytät 64 Kilotavua kohdistus yksikön koon tietojen ja lokitiedostojen sekä TempDB.

- **Levyn projektinhallinnan parhaiden käytäntöjen**: kun poistaa tietoja DVD-levyllä tai muuttaa sen välimuistityyppi lopettaa SQL Server-palvelun muutoksen aikana. Kun välimuistin asetukset muuttuvat OS levyllä, Azure lopettaa AM, muuttaa välimuistin ja käynnistyy AM. Tietoja levyn välimuistiasetukset muuttuvat, kun AM ei ole keskeytetty, mutta tietojen levyn irrotettu AM muutoksen aikana ja liittäminen sitten uudelleen.

    >[AZURE.WARNING] Virhe SQL Server-palvelun pysäyttäminen aikana näitä toimintoja voi aiheuttaa tietokannan vioittumisen.

## <a name="io-guidance"></a>I/o-ohjeet

- Premium tallennustilan kanssa paras tulos saavutetaan, kun sovellus ja pyyntöjä parallelize. Premium tallennustila on suunniteltu tilanteissa, joissa IO jonon pituus on suurempi kuin 1, niin näet yksittäisen threaded serial pyynnöt mahdollisimman vähän suorituskyvyn voitot (vaikka heillä ei olisikaan tallennustilaa on paljon resursseja). Esimerkiksi Tämä voi olla vaikutusta single threaded testitulokset suorituskyvyn analysointi työkaluja, kuten SQLIO.

- Harkitse [tietokannan sivun pakkaaminen](https://msdn.microsoft.com/library/cc280449.aspx) , kun se voi parantaa suorituskykyä i/o tehostettu toiminnoista. Tietojen pakkaus voi kuitenkin kasvattaa suorittimen kulutus tietokantapalvelimeen.

- Harkitse käyttöönottamista pikaviestien tiedoston alustus vähentää alkuperäisen tiedoston jakamista varten tarvittava aika. Pikaviestien tiedoston alustus hyödyntää myöntää SQL Server (MSSQLSERVER)-palvelutili, jolla SE_MANAGE_VOLUME_NAME ja sen lisääminen **Suorittaa aseman ylläpitotehtäviä** suojauskäytäntö. Jos käytät SQL Server-ympäristössä kuva Azure-palvelu oletustilin (NT Service\MSSQLSERVER) ei ole lisätty **Suorittaa aseman ylläpitotehtäviä** suojauskäytäntö. Toisin sanoen pikaviestien tiedoston alustus ei ole otettu käyttöön SQL Server Azure-ympäristö kuva. Kun olet lisännyt **Suorittaa aseman ylläpitotehtäviä** suojauskäytäntö SQL Server-tilin, Käynnistä SQL Server-palvelu uudelleen. Voi olla suojaustietoja tämän toiminnon avulla. Lisätietoja on artikkelissa [Tietokannan tiedoston alustus](https://msdn.microsoft.com/library/ms175935.aspx).

- pelkästään vara odottamattomia kasvun pidetään **kasvamaan automaattisesti** . Hallitse tietojen ja kirjaudu-kasvu päivittäinen kasvamaan automaattisesti kanssa. Jos käytössä on kasvamaan automaattisesti, valmiiksi kasvaa ohjatulla koon tiedosto.

- Varmista, että **autoshrink** ei ole käytössä, voit välttää tarpeettomat katseltavan, jotka voivat heikentää suorituskykyä.

- Siirrä kaikki tietokannat tietojen levyjä, mukaan lukien järjestelmän tietokannat. Lisätietoja on artikkelissa [Siirtää järjestelmän tietokannat](https://msdn.microsoft.com/library/ms345408.aspx).

- Siirtää tietoja levyjen SQL Serverin virhe loki- ja tiedoston kansioita. Tämä voidaan toteuttaa SQL Serverin määritysten hallinta SQL Server-esiintymän hiiren kakkospainikkeella ja valitsemalla Ominaisuudet. Virhe loki- ja tiedoston asetuksia voi muuttaa **Käynnistysparametrit** -välilehdessä. Dump-kansio on määritetty **Lisäasetukset** -välilehti. Seuraavassa näyttökuvassa näkyy mistä virhe log käynnistys-parametrin.

    ![SQL-ErrorLog näyttökuva](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Määritä oletusarvoinen varmuuskopiointi- ja sijainnit. Käytä suosituksia tämän artikkelin ja tee haluamasi muutokset palvelimeen ominaisuudet-ikkunassa. Ohjeita on artikkelissa [Tarkastele tai muuta oletus sijainnit tietojen ja lokitiedostojen (SQL Server Management Studiossa)](https://msdn.microsoft.com/library/dd206993.aspx). Seuraavassa näyttökuvassa on esitetty esitellään missä muutosten tekeminen.

    ![SQL-tietojen loki-ja varmuuskopiointi](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Ota lukittujen sivujen vähentää IO ja sivutus tehtävät. Lisätietoja on artikkelissa [Lukitse sivut (Windows) muisti-asetuksen käyttöön](https://msdn.microsoft.com/library/ms190730.aspx).

- Jos käytössäsi on SQL Server 2012, asenna Service Pack 1 kumulatiivinen päivitys 10. Tämä päivitys sisältää huonosti i/o korjaus, kun valitse suorittaa kyselyjä tilapäinen table-lause SQL Server 2012: ssa. Lisätietoja tämän [knowledge base-artikkelissa](http://support.microsoft.com/kb/2958012).

- Harkitse tiedostot Pakkaaminen siirrettäessä sisään tai ulos, Azure.

## <a name="feature-specific-guidance"></a>Ominaisuuden tiettyjä ohjeita

Joissakin käyttöönotoissa voi päästä suorituskykyä edut monimutkaisemman määritysten menetelmillä. Seuraavassa luettelossa esitellään joitakin SQL Server-ominaisuuksia, jotka auttavat suorituskyvyn parantamiseksi:

- **Azure-tallennustilan varmuuskopio**: suoritettaessa varmuuskopioiden Azuren näennäiskoneiden SQL Serveriä, voit käyttää [SQL Server Backup URL-osoitteeseen](https://msdn.microsoft.com/library/dn435916.aspx). Tämä ominaisuus on käytettävissä SQL Server 2012 SP1 CU2 alkaen ja suositellaan liitettyjä tietoja levyjen varmuuskopion. Kun olet varmuuskopiointi/palauttaa ja Azure tallennustilan noudata annettu, [SQL Server varmuuskopion URL-Osoitteen parhaita käytäntöjä ja vianmääritys ja-Azuren tallennustilaan tallennettuja varmuuskopioista palauttaminen](https://msdn.microsoft.com/library/jj919149.aspx)suosituksia. Voit automatisoida nämä [Automaattisen varmuuskopioinnin Azuren näennäiskoneiden SQL Server-palvelimen](virtual-machines-windows-classic-sql-automated-backup.md)käyttäminen varmuuskopiot.

    SQL Server 2012, ennen kuin voit käyttää [SQL Server Backup Azure-työkalu](https://www.microsoft.com/download/details.aspx?id=40740). Tämä työkalu auttaa lisäämään varmuuskopion siirtonopeuden käyttämällä useita varmuuskopion raita kohteet.

- **SQL Server-datatiedostoja Azure**: Tämä uusi ominaisuus [SQL Server-datatiedostoja Azure](https://msdn.microsoft.com/library/dn385720.aspx)on SQL Server 2014 alkaen. SQL Serverin käytön Azure datatiedostojen esitellään käyttäviksi Azure tietojen levyjen vastaa suorituskyvyn ominaisuudet.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos olet kiinnostunut SQL Server- ja Premium tallennustilan Lisää perusteellisesti, on artikkelissa [Käytä Azure Premium tallennustilan näennäiskoneiden SQL Serveriin](virtual-machines-windows-classic-sql-server-premium-storage.md).

Suojauksen parhaista käytännöistä on [SQL Server Azuren näennäiskoneiden suojaustietoja](virtual-machines-windows-sql-security.md).

Tarkista muut SQL Server Virtual Machine ohjeruudun ohjeaiheet [Azuren näennäiskoneiden yleiskatsaus SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)-palvelussa.
