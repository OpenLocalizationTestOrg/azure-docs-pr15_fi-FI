<properties 
   pageTitle="SharePoint-sovittimen StorSimple | Microsoft Azure"
   description="Tässä artikkelissa kuvataan asentaminen ja määrittäminen tai poistaminen StorSimple sovittimen SharePoint server-klusterin SharePoint."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Asenna ja määritä StorSimple sovittimen SharePoint

## <a name="overview"></a>Yleiskatsaus

StorSimple sovittimen SharePoint on osa, jonka avulla voit antaa Microsoft Azure StorSimple joustavia tallennustilan ja tietojen suojauksen SharePoint server-klustereihin. Voit siirtää SQL Server-sisältötietokantojen binaarinen suuri objektin (BLOB) sisältöä Microsoft Azure StorSimple hybrid cloud tallennustilan laitteeseen sovittimen.

StorSimple sovittimen SharePoint toimii Remote BLOB Storage (RBS)-palvelun ja käyttää SQL Server Remote BLOB-OBJEKTIEN tallennustilaan ominaisuus rakenteeton SharePoint-sisältö on tallennettu (muodossa BLOB) tiedostopalvelimessa, varmuuskopioidaan StorSimple laite.

>[AZURE.NOTE] StorSimple sovittimen SharePoint tukee SharePoint Server 2010: n Remote BLOB Storage (RBS). SharePoint Server 2010: n ulkoinen BLOB Storage (EBS) eivät tue.

- Voit ladata StorSimple sovittimen SharePoint-siirtymällä [StorSimple sovittimen SharePoint] [ 1] Microsoft Download Centeristä.

- Lisätietoja suunnittelun RBS ja RBS rajoitukset Siirry [Deciding RBS SharePoint 2013: n käyttämään] [ 2] tai [RBS (SharePoint Server 2010) suunnitteleminen][3].

Tämä yleiskatsaus loput kuvataan lyhyesti StorSimple sovittimen roolin SharePoint-ja SharePoint-kapasiteetti ja suorituskyvyn rajoitukset, sinun olisi otettava huomioon ennen kuin voit asentaa ja määrittää sovittimen. Kun olet tarkistanut nämä tiedot, siirry [SharePoint-asennuksen sovittimen StorSimple](#storsimple-adapter-for-sharepoint-installation) Aloita sovittimen määrittäminen.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple sovittimen SharePoint-edut

SharePoint-sivuston sisältö on tallennettu tietoina rakenteeton BLOB vähintään yksi sisältötietokantoja. Oletusarvon mukaan tietokannat isännöidään tietokoneissa, joissa on SQL Serverin ja SharePoint server-klusterin sijaitsevat. Koko-muissa paikallisen tallennustilan suuria määriä BLOB voit nopeasti kasvaa. Tästä syystä voit etsiä toisen, edullinen tallennustilan ratkaisu. SQL Server on Remote Blob Storage RBS, jolla voit tallentaa BLOB sisältöä tiedostojärjestelmässä SQL Server-tietokannan ulkopuolella-tekniikkaa. RBS-BLOB-objektit voivat sijaita tietokoneen SQL Serveriä suorittava tiedostojärjestelmässä tai ne on tallennettu tiedostojärjestelmässä palvelimen toisessa tietokoneessa.

RBS edellyttää, että käytät RBS-palveluun, kuten SharePoint-StorSimple-sovittimen käyttöön RBS SharePointissa. SharePointin StorSimple-sovittimen toimii RBS, jolloin voidaan siirtää BLOB Microsoft Azure StorSimple järjestelmä varmuuskopioida palvelimeen. Microsoft Azure StorSimple tallentaa BLOB-tietojen paikallisesti tai pilvipalvelussa, käyttö perusteella. BLOB-objektit, jotka ovat hyvin aktiivinen (yleensä kutsutaan kuuma tietojen tai taso 1) sijaitsevat paikallisesti. Vähemmän aktiivinen tiedot ja arkistointia sijaitsevat pilvessä. Kun otat käyttöön sisältötietokannan RBS, uusi luotu SharePoint-BLOB-OBJEKTIEN sisältö tallennetaan StorSimple laitteessa ja ei sisältötietokannan.

RBS Microsoft Azure StorSimple käyttöönoton sisältää seuraavat edut:

- Liukuvan BLOB-sisältö erilliseen palvelimeen voit pienentää kyselyn kuormituksen SQL Serveristä, joka voi parantaa SQL Serverin vastausajan. 

- Azure StorSimple käyttää kopioinnin peruuttaminen ja pakkaus tiedot koon pienentäminen.

- Azure StorSimple on tietosuojaa paikallisten ja cloud tilannevedoksia. Myös, jos lisäät tietokantaan StorSimple laitteeseen, voit varmuuskopioida sisältötietokannan ja BLOB yhdessä kaatua yhdenmukaisesti. (Sisältötietokannan siirtäminen laitteen tuetaan vain StorSimple 8000 sarjan laitteella. Tätä ominaisuutta ei tueta 5000 tai 7000 sarjan.)

- Azure StorSimple on tietojen palauttaminen toimintoja, kuten automaattisesti, tiedosto- ja palauttaminen (mukaan lukien testi palautus) ja tietojen nopeaan palauttamiseen.

- Voit käyttää tietojen palauttaminen ohjelmiston Kroll Ontrack PowerControls, kuten StorSimple tilannevedosten BLOB-OBJEKTIEN tietoja SharePoint-sisällön palauttamisen Kohdetason. (Tietojen palauttaminen ohjelmisto on erillinen osto.)

- SharePointin StorSimple-sovittimen kytketään SharePointin keskitetty hallinta-portaalissa, joilla voit hallita koko SharePoint-ratkaisun keskitetystä sijainnista.

BLOB-sisällön siirtäminen tiedostojärjestelmän voit antaa muiden kustannuksia ja etuihin. Esimerkiksi RBS avulla voit vähentää edellyttää taso 1 kallista tallennustilaa ja, koska se pienenee sisältötietokannan, RBS vähentää tarvitaan SharePoint server-klusterin tietokantojen määrä. Kuitenkin muut tekijät, kuten tietokannan kokorajoituksista ja -RBS sisällön määrän voi vaikuttaa myös tallennustilaa. Saat lisätietoja kustannuksia ja RBS eduista [RBS (SharePoint Foundation 2010) suunnitteleminen] [ 4] ja [Deciding RBS SharePoint 2013: n käyttämään][5].

### <a name="capacity-and-performance-limits"></a>Kapasiteetti ja suorituskyvyn rajoitukset

Ennen kuin pidät RBS käyttäminen SharePoint-ratkaisussa, sinun olisi otettava huomioon testattu suorituskyvyn ja kapasiteetin rajat SharePoint Server 2010: n ja SharePoint Server 2013: n ja kuinka nämä raja-arvot liittyvät hyväksyttävällä suorituskykyä. Lisätietoja on artikkelissa [ohjelmiston rajat ja rajoitukset SharePoint 2013: n](https://technet.microsoft.com/library/cc262787.aspx).

Tarkista seuraavasti, ennen kuin määrität RBS:

- Varmista, että sisältö (sisältötietokannan kokoa) sekä kaikki liittyvät externalized BLOB koon kokonaiskoko enintään SharePoint tukee RBS kokorajoituksen. Tämä rajoitus on 200 gt. 

    **Mittayksikön sisältötietokannan ja BLOB-OBJEKTIEN koon**

     1. Suorita kysely keskitetyn hallinnan WFE. Käynnistä SharePoint-hallintaliittymän ja kirjoita sitten saat sisältötietokantojen koon seuraavia Windows PowerShell-komentoa:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Tässä vaiheessa saa levyn sisältötietokannan kokoa.

     2. Suorita jompikumpi seuraavista SQL-kyselyjä SQL Management Studiossa kunkin sisältötietokannan SQL server-ruutu ja lisää tulos numero saadun-vaihe 1.

        Anna SharePoint 2013: n sisältötietokantojen:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Anna SharePoint 2010: n sisältötietokantojen:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Tässä vaiheessa saa BLOB-objektit, jotka on externalized kokoa.

- On suositeltavaa tallentaa kaikki BLOB-OBJEKTIEN ja tietokannan sisältö paikallisesti StorSimple laitteeseen. StorSimple laite on kahden solmun klusterin suuren. Sisältötietokantojen ja BLOB markkinoille StorSimple laite on suuri käytettävyys.

    Perinteinen SQL Server siirron parhaita käytäntöjä avulla voit siirtää sisältötietokannan StorSimple laitteeseen. Siirrä tietokanta vasta, kun tietokanta BLOB sisältöä on siirretty jaettua tiedostoresurssia RBS: n kautta. Jos haluat siirtää sisältötietokannan StorSimple laitteeseen, on suositeltavaa sisältötietokannan tallennustilan määrittämistä ensisijainen aseman laitteen.

- Microsoft Azure StorSimple, jos Porrastettu asemista, valitse tällä ei voi taata paikallisesti tallennetut laite ei tasoisen, Microsoft Azure-pilvitallennustilaa StorSimple sisältöä. Näin ollen on suositeltavaa käyttää yhdessä SharePoint RBS StorSimple paikallisesti kiinnitetyt asemat. Näin varmistat, että kaikki BLOB sisältö pysyy StorSimple laitteeseen paikallisesti ja siirretään ei Microsoft Azure.

- Jos Älä tallenna sisältötietokantojen StorSimple laitteeseen, käytä perinteinen SQL Server suuren käytettävyyden parhaita käytäntöjä, jotka tukevat RBS. SQL Server-klusteroinnin tukee RBS, SQL Server aikana peilausta ei tue. 

>[AZURE.WARNING] Jos et ole ottanut RBS, Emme suosittele sisältötietokannan siirtäminen StorSimple laite. Tämä on testaamattomia määritys.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>SharePoint-asennuksen sovittimen StorSimple

Ennen kuin asennat StorSimple sovittimen SharePoint-StorSimple-laitteen määrittäminen ja varmista, että SharePoint server-klusterin ja SQL Server-esiintymän täyttävät kaikki edellytykset. Tässä opetusohjelmassa kuvataan määritykset-vaatimusten sekä ohjeet asentaminen ja päivittäminen SharePoint StorSimple sovittimen. 

## <a name="configure-prerequisites"></a>Määritä edellytykset

Ennen kuin asennat StorSimple sovittimen SharePoint, varmista, että StorSimple laitteen, SharePoint server-klusterin ja SQL Server-esiintymän täytettävä seuraavat vaatimukset.

### <a name="system-requirements"></a>Järjestelmävaatimukset

SharePointin StorSimple-sovittimen toimii seuraavassa laitteiston ja ohjelmiston:

- Tuetut käyttöjärjestelmän – Windows Server 2008 R2 SP1, Windows Server 2012: ssa tai Windows Server 2012 R2 

- Tuetut SharePoint-versiot – SharePoint Server 2010 tai SharePoint Server 2013

- Tuetut SQL Server-versiot – SQL Server 2008: n Enterprise Edition, SQL Server 2008 R2: n Enterprise Edition tai SQL Server 2012: n Enterprise Edition

- Tuetut laitteet StorSimple – StorSimple 8000 sarjan, StorSimple 7000 sarjan tai StorSimple 5000 sarja.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple laitteen määritysten edellytykset

StorSimple laite on Estä laite ja edellyttää sellaisenaan tiedostopalvelimessa, jossa tiedot voidaan ylläpitää. On suositeltavaa, että käytät olemassa olevan palvelimen sijaan muussa palvelimessa kuin SharePoint-klusterin. Tämä tiedostopalvelin on oltava sama paikallinen lähiverkossa (LAN) SQL Server-tietokone, jossa sisältötietokantojen. 

>[AZURE.TIP] 
>
>- Jos määrität suuren SharePoint-klusterin, kannattaa ottaa käyttöön suuren tiedostopalvelimeen myös.
>
>- Jos Älä tallenna sisältötietokannan StorSimple laitteeseen, käytä perinteinen suuren käytettävyyden parhaita käytäntöjä, jotka tukevat RBS. SQL Server-klusteroinnin tukee RBS, SQL Server aikana peilausta ei tue. 

Varmista, että StorSimple laitteessa on määritetty oikein ja sopiva tietomääristä tukemaan SharePointin asennuksen yhteydessä on määritetty ja käytettävissä olevat SQL Server-tietokoneessa. Siirry [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough.md) , jos sinulla ei ole vielä otettu käyttöön ja määritetty StorSimple laitteen. Huomautus StorSimple; laitteen IP-osoite Sinun on se aikana StorSimple sovittimen SharePoint-asennuksen. 

Varmista lisäksi, jota käytetään BLOB-OBJEKTIEN externalization äänenvoimakkuuden täyttää seuraavat vaatimukset:

- Äänenvoimakkuuden on muotoiltu 64 Kilotavua kohdistus yksikön koon.

- Web-edusta (WFE) ja sovelluksen palvelinten on oltava voivat käyttää äänenvoimakkuuden yleinen nimeäminen konferenssi UNC-polkua kautta. 

- SharePoint server-klusteriin on oltava määritettynä äänenvoimakkuuden kirjoittaminen.

>[AZURE.NOTE] Kun olet asentanut ja -sovittimen, kaikki BLOB-OBJEKTIEN externalization on käydä läpi StorSimple laitteen (laitteen esittää asemat SQL Server- ja tallennustilaa tasoa hallinta). Et voi käyttää muiden kohteiden BLOB-OBJEKTIEN externalization.
 
Jos aiot käyttää StorSimple tilannevedoksen hallinta BLOB-OBJEKTIEN ja tietokannan tietojen tilannevedosten, muista asentaa StorSimple tilannevedoksen hallinta tietokantapalvelimen niin, että se käyttää SQL Writer-palvelu toteuttamisesta Windows aseman varjostus kopioi Service (VSS). 

>[AZURE.IMPORTANT] StorSimple tilannevedoksen hallinta ei tue SharePoint VSS Writer ja sovelluksen yhdenmukaisia SharePoint-tietojen tilannevedosten ei viedä. SharePoint-skenaario StorSimple tilannevedoksen hallinta tarjoaa kaatumisen johdonmukaisia varmuuskopiot. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint-klusterin määritys edellytykset

Varmista, että SharePoint server-klusterin on määritetty oikein, seuraavasti:

- Varmista, että SharePoint server-klusterin on kunnossa-tilaan ja tarkista seuraavat asiat: 

- Kaikki SharePoint WFE ja sovelluksen palvelimet rekisteröity klusterissa on käynnissä ja olla pinged, asennetaan StorSimple sovittimen SharePoint-palvelimesta.

- SharePoint-ajastinpalvelun (SPTimerV3 tai SPTimerV4) on käytössä kunkin WFE palvelin- ja.

- SharePoint-ajastinpalvelun ja jossa SharePointin keskitetyn hallinnan sivuston suoritetaan IIS-sovellussarjan on järjestelmänvalvojan oikeudet. 

- Varmista, että Internet Explorerin parannettu suojauskontekstin (IE ESC) on poistettu käytöstä. Internet Explorerin ESC käytöstä seuraavasti:

    1. Sulje kaikki Internet Explorer-esiintymät.

    2. Aloita Serverin hallinta.

    3. Valitse vasemmanpuoleisessa ruudussa **Paikalliseen palvelimeen**.

    4. **Valitse oikeanpuoleisessa ruudussa **Internet Explorerin suojaus-parannetun**vieressä.**

    5. **Järjestelmänvalvojat**ja valitse sitten **ei käytössä**.

    6. Valitse **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Remote BLOB Storage RBS edellytykset

Varmista, että käytät tuetut SQL Server-version. Seuraavista versioista on tuettu ja käyttää RBS:

- SQL Server 2008: n Enterprise Edition

- SQL Server 2008 R2: n Enterprise Edition

- SQL Server 2012: n Enterprise Edition

BLOB voidaan externalized vain asemat, jotka StorSimple laitteen esittää SQL Server. Ei ole muita BLOB-OBJEKTIEN externalization kohteet ovat tuettuja.

Kun olet suorittanut kaikki tarvittavat määritysvaiheet, siirry [SharePoint-StorSimple-sovittimen asentaminen](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>Asenna SharePoint StorSimple

Seuraavien vaiheiden avulla voit asentaa StorSimple sovittimen SharePoint. Jos asennat ohjelmiston, katso [päivittäminen tai asentaa StorSimple sovittimen SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Asennuksessa tarvittava aika määräytyy SharePoint-tietokantojen SharePoint server-klusterin kokonaismäärän.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>RBS määrittäminen

Kun olet asentanut StorSimple sovittimen SharePoint-määritettävä RBS seuraavassa kuvatulla tavalla.

>[AZURE.TIP] SharePointin StorSimple-sovittimen kytketään keskitetty hallinta-sivun salliminen RBS käyttöön tai poistettu käytöstä SharePoint-klusterin kunkin sisältötietokannan. Kuitenkin käyttöönoton tai käytöstäpoiston RBS-sisältötietokannan aiheuttaa IIS-salasanan, joka klusterin määritysten mukaan voit hetkeksi häiritä SharePoint WWW-edusta (WFE) käytettävyyttä. (Tekijöistä, kuten FEP kuormituksen, nykyisen palvelimen kuormituksen ja niin edelleen, voit rajoittaa tai poistaa tämän häiriöt.) Suojaa käyttäjien häiriöitä, on suositeltavaa, että ottaminen käyttöön tai estää RBS vain aikana suunniteltu ylläpito-ikkuna.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Määritä muistista

Kun objektit on poistettu SharePoint-sivustosta, ne eivät poistetaan automaattisesti RBS kaupan asemasta. Sen sijaan asynkroninen, tausta-ylläpito-ohjelma poistaa yhteydetön BLOB tiedosto-kaupasta. Järjestelmänvalvojat voivat ajoittaa säännöllisesti suorittaa tämän prosessin, tai he voivat aloittaa sen tarvittaessa.

Tämä ylläpito-ohjelma (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) asennetaan automaattisesti kaikki SharePoint WFE-palvelimet ja sovelluksen palvelimissa, kun otat RBS. Ohjelma on asennettu seuraavaan sijaintiin: *asema käynnistys*: Files\Microsoft SQL-Remote-Blob-säiliö 10.50\Maintainer\

Lisätietoja määrittäminen ja käyttäminen ylläpito-ohjelma on [Säilyttää RBS SharePoint Server 2013: n][8].

>[AZURE.IMPORTANT] RBS ylläpitäjää ohjelma on paljon resursseja. Olisi Ajoita sen toimimaan vain aikana Vaalea toiminnan SharePoint-klusterin.

### <a name="delete-orphaned-blobs-immediately"></a>Poista yhteydetön BLOB heti

Jos haluat poistaa yhteydetön BLOB heti, voit seuraavien ohjeiden mukaisesti. Huomaa, että nämä ohjeet ovat esimerkki siitä, miten voit tehdä tämän SharePoint 2013-ympäristössä, jossa seuraavat osat:

- Sisältötietokannan on WSS_Content.
- SQL Server on SHRPT13 SQL12\SHRPT13.
- Web-sovelluksen nimi on SharePoint – 80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Päivittäminen tai asentaa StorSimple sovittimen SharePoint

Noudattamalla seuraavia vaiheita SharePoint Serverin ja asenna SharePoint StorSimple sovittimen, yksinkertaisesti päivittäminen tai aiemmin luotuun SharePoint-palvelinfarmiin sovittimen sen uudelleen. 

>[AZURE.IMPORTANT] Ennen kuin yrität päivittää SharePoint-ohjelmisto ja/tai päivitys tai asentaa StorSimple sovittimen SharePoint, tarkista seuraavat tiedot:
>
>- Tiedostot, jotka on aiemmin siirretty ulkoisille RBS kautta ei ole käytettävissä, RBS-ominaisuus on otettu käyttöön uudelleen, kunnes uudelleenasennus on valmis. Rajoittaminen käyttäjään kohdistuvat suorittaa minkä tahansa päivitys tai uudelleenasennuksen aikana suunniteltu ylläpito-ikkuna.
>
>- Päivityksen/uudelleenasennus tarvittavan ajan voi vaihdella sen mukaan, SharePoint-tietokantojen SharePoint server-klusterin kokonaismäärän.
>
>- Kun päivitys/uudelleenasennus on valmis, sinun on RBS käyttöön sisältötietokantojen. Lisätietoja on kohdassa [Määritä RBS](#configure-rbs) .
>
>- Jos olet määrittämässä RBS SharePoint-klusterin, jossa on erittäin suuri määrä (yli 200) tietokantoja, **Keskitetty hallinta** -sivun saattaa kadota itsestään. Jos näin tapahtuu, Päivitä sivu. Tämä ei vaikuta määritysprosessi.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple sovittimen SharePoint poistamista varten

Seuraavassa kuvataan, siirrä BLOB-objektit takaisin SQL Server-sisältötietokantojen ja poistamalla SharePoint StorSimple sovittimen. 

>[AZURE.IMPORTANT] Sinun on siirrettävä BLOB-objektit takaisin sisältötietokantojen, ennen kuin poistat sovittimen-ohjelmisto. 

### <a name="before-you-begin"></a>Ennen aloittamista 

Kerää seuraavat tiedot, ennen kuin tietojen siirtäminen takaisin SQL Server-sisältötietokantojen ja aloita sovittimen poistaminen:

- Kaikki tietokannat, jossa on käytössä RBS nimet
- UNC-polkua määritetty BLOB-säilö

### <a name="move-the-blobs-back-to-the-content-databases"></a>Siirrä BLOB-objektit takaisin sisältötietokantojen

Ennen kuin poistat SharePoint-ohjelmiston StorSimple-sovittimen, kaikki BLOB-objektit, jotka olivat externalized on siirrettävä takaisin SQL Server-sisältötietokantoja. Jos yrität poistaa StorSimple sovittimen SharePoint, ennen kuin siirryt kaikki Blob-objektien takaisin sisältötietokantojen, näyttöön tulee seuraava virhesanoma.

![Varoitussanoma](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>Jos haluat siirtää BLOB-objektit takaisin sisältötietokantojen 

1. Lataa kutakin externalized objektia.

2. Avaa **SharePointin keskitetty hallinta** -sivulla ja Etsi **Järjestelmäasetuksia**. 

3. Valitse **Azure StorSimple** **Määrittäminen StorSimple sovittimen**.

4. **StorSimple sovittimen määrittäminen** -sivulla napsauta alla kunkin sisältötietokantojen, jonka haluat poistaa ulkoiset BLOB-OBJEKTIEN tallennustilaan **käytöstä** -painiketta. 

5. Poista SharePoint-objektit ja ladata ne sitten uudelleen.

Vaihtoehtoisesti voit käyttää Microsoft` RBS Migrate()` PowerShell cmdlet-komennon SharePoint mukana. Katso lisätietoja [artikkelista sisällön sisään tai ulos RBS](https://technet.microsoft.com/library/ff628255.aspx).

Kun siirrät BLOB-objektit takaisin sisältötietokannan, siirry seuraavaan vaiheeseen: [Poista sovittimen](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Poista sovittimen

Kun siirrät BLOB-objektit takaisin SQL Server-sisältötietokantojen, asennuksen StorSimple sovittimen SharePoint jollakin seuraavista vaihtoehdoista.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Poista sovittimen asennusohjelman avulla 

1. Kirjaudu web edusta (WFE)-palvelimen järjestelmänvalvojan oikeuksin tilin avulla.
2. Kaksoisnapsauta StorSimple sovittimen SharePoint-asennusohjelma. Ohjattu asennus käynnistyy.

    ![Ohjattu asennus](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Valitse **Seuraava**. Seuraava sivu tulee näkyviin.

    ![Ohjatun asennuksen Poista sivu](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Valitse Valitse poistaminen **poistaa** . Seuraava sivu tulee näkyviin.

    ![Ohjatun asennuksen vahvistus-sivu](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Valitse Vahvista **poistaminen** . Seuraavat edistyminen-sivu tulee näkyviin.

    ![Ohjatun asennuksen edistyminen-sivu](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Kun poisto on valmis, Valmis-sivu tulee näkyviin. Valitse **Valmis** , sulje ohjattu asennus.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Sovittimen asennuksen poistaminen Ohjauspaneelin avulla 

1. Avaa Ohjauspaneeli ja valitse sitten **Ohjelmat ja toiminnot**.

2. Valitse **StorSimple-sovittimen, SharePoint**ja valitse sitten **Poista asennus**. 

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
