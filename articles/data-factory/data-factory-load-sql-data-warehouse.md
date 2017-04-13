<properties 
    pageTitle="Lataa tiedot teratavua SQL-tietovarasto | Microsoft Azure" 
    description="Esitellään, miten 1 TT: n tiedot voidaan ladata kyselyjä Azure SQL-tietovarasto 15 minuutin ajan alle Azure Data Factory" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>Lataa 1 TT Azure SQL-tietovarasto 15 minuutin ajan alle Azure Data Factory [Ohjattu kopiointi]
[Azure SQL-tietovarasto](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) on voi käsittelyn valtaviin tietomäärien, Relaatio- ja relaatio tietokanta pilvipohjainen, asteikko-kohtaa.  Rakennettu erittäin rinnakkain käsittely (MPP) arkkitehtuuri SQL-tietovarasto on tarkoitettu yrityksen data warehouse toiminnoista.  Siinä on cloud joustavuus skaalata tallennustilan ja Laske itsenäisesti joustavasti.

Käytön aloittaminen Azure SQL-tietovarasto on entistä helpompaa **Azure Data Factory**työskentelytapaan avulla.  Azure Data Factory on täysin hallitun pilvipohjainen tietojen integrointi palvelun, joka voidaan täytä SQL-tietovarasto aiemmin järjestelmän ja tallentaminen aikaasi arvioit SQL-tietovarasto ja analytics-ratkaisuja selvästi tiedoilla.  Seuraavassa on tietoja ladataan Azure SQL-tietovarasto käyttämällä Azure Data Factory avaimen edut:

- **Voit määrittää helppokäyttöinen**: 5 vaihe intuitiivisen ohjattu, jossa ei ole komentosarjan pakollinen.
- **Rich tietovaraston tuki**: tukee monenlaisia paikallisen ja pilvipohjaisia Microsoft Data.
- **Suojatun ja yhteensopiva**: tieto siirretään HTTPS- tai ExpressRoute ja yleisen palvelun tavoitettavuuden varmistaa tietojen jättää koskaan maantieteellinen reunaa
- **Käyttämällä PolyBase ennennäkemättömien suorituskyky** – käyttämällä Polybase on tietojen siirtäminen SQL Azure-tietovarasto tehokkaasti. Väliaikaisen blob-ominaisuuden avulla voit tehostaa suuren kuormituksen aikana nopeuksia kaikki erityyppisiä tietoja stores lisäksi Azure-Blob-säiliö, joka Polybase tukee oletusarvoisesti.

Tämän artikkelin avulla opit käyttämään ohjatun tietojen Factory kopion ladata 1 TT tietoja from Azure-Blob-säiliö Azure SQL-tietovarasto-kohdassa 15 minuutin ajan yli 1.2 GBps siirtonopeuden.

Tässä artikkelissa on vaiheittaiset ohjeet tietojen siirtäminen SQL Azure-tietovarasto kyselyjä käyttämällä ohjattua Kopioi. 

> [AZURE.NOTE] Katso [tietojen ja sieltä pois Azure SQL-tietovarasto Azure Data Factory käyttämällä siirtäminen](data-factory-azure-sql-data-warehouse-connector.md) artikkelissa yleisiä tietoja ominaisuuksista sekä Data Factory-tietojen siirtäminen SQL Azure-tietovarasto. 
> 
> Voit myös luoda putkistot Azure portaalissa, Visual Studio PowerShellin jne. Katso [Opetusohjelma: tietojen kopioiminen Azure-Blob Azure SQL-tietokanta](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) on vaiheittaiset ohjeet kopio-toimintojen käyttäminen Azure Data Factory nopeasti ongelmatilanteita varten.  

## <a name="prerequisites"></a>Edellytykset
- Azure-Blob-säiliö: tämän kokeen käyttää Azure Blob Storage (GRS) TPC-H testauksen tietojoukko tallentamista varten.  Jos sinulla ei ole Azure-tallennustilan tilin, lue, [miten voit luoda tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account).
- Tietojen [TPC H](http://www.tpc.org/tpch/) : tarkastellaan käyttää TPC-H testauksen tietojoukko.  Saadakseen ne, sinun on käytettävä `dbgen` -TPC-H-Työkalut, jossa avulla voit luoda dataset.  Voit joko ladata lähdekoodin `dbgen` [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) työkaluista ja kääntää sitä itse tai Lataa [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools)käännetty binaariluvuksi.  Suorita seuraavat komennot luomiseen 1 TT: n tietuetiedostoon dbgen.exe `lineitem` taulukon levitä yli 10 tiedostot:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Kopioi Azure-Blob luodut tiedostot nyt.  [Siirrä tiedot ja paikallisen tiedoston sähköpostijärjestelmään Azure Data Factory avulla](data-factory-onprem-file-system-connector.md) voit tehdä viitata SYÖTTÖ kopioimalla.   
- Azure SQL-tietovarasto: tämän kokeen lataa tiedot Azure SQL-tietovarasto luotu 6,000 DWUs

    Lisätietoja [luominen Azure SQL-tietovarasto](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) n tarkat ohjeet tietovarasto SQL-tietokannan luomiseen.  Jos haluat mahdollista kuormituksen parhaan suorituskyvyn kyselyjä käyttämällä Polybase SQL-tietovarasto, valitse tietojen varasto yksiköt (DWUs) suorituskyky-asetus, jolla on 6,000 DWUs sallittu enimmäismäärä.

    > [AZURE.NOTE] 
    > Kun ladataan Azure-Blob-tietojen lataaminen suorituskyky on suoraan suhteellisten DWUs SQL-tietovarasto määrittämistä määrän:
    > 
    > 1 TT ladataan 1 000 DWU SQL-tietovarasto kestää 87min (noin 200MBps siirtonopeuden) 1 TT ladataan 2 000 DWU SQL-tietovarasto kestää 46min (noin 380MBps siirtonopeuden) 1 TT ladataan 6 000 DWU SQL-tietovarasto on 14min (~1.2GBps nopeus) 

    Jos haluat luoda SQL-tietovarasto 6,000 DWUs, siirtämällä suorituskyvyn aivan oikealla:

    ![Suorituskyvyn liukusäädin](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Aiemmin luodun tietokannan, joka ei ole määritetty 6,000 DWUs voit skaalata sitä ylös Azure-portaalissa.  Siirry Azure-portaalissa tietokannan ja **Skaalaus** -painike on **Yhteenveto** -ruudussa seuraavan kuvan mukaisesti:

    ![Skaalaa-painike](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Valitse **asteikko** -painikkeen napsauttaminen avaa seuraavat Ohjauspaneelin siirtämällä suurimman arvon ja sitten **Tallenna** -painiketta.

    ![Skaalaus-valintaikkuna](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Tämän kokeen Azure SQL-tietovarasto käyttäminen tietojen latautuu `xlargerc` resurssiluokkaa.

    Paras mahdollinen siirtonopeuden saavuttamiseksi kopio on suoritettava kuuluvat SQL-tietovarasto käyttäjän `xlargerc` resurssiluokkaa.  Lue, miten voit tehdä seuraavat [Muuta käyttäjän resurssin luokka-Esimerkki](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Luo kohde Taulukkorakenteen Azure SQL-tietovarasto tietokannan suorittamalla DDL-lause:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Valmis valmistelevat vaiheisiin on nyt määrittäminen käyttämällä ohjattua kopioiminen Kopioi tehtävä.

## <a name="launch-copy-wizard"></a>Käynnistä ohjattu kopiointi

1.  Kirjaudu sisään [Azure portal](https://portal.azure.com).
2.  Valitse **+ Uusi** vasemmassa yläkulmassa, valitse **liiketoimintatietojen + analytics**ja sitten **Data Factory**. 
6. Valitse **uudet tiedot factory** -sivu:
    1. Kirjoita **LoadIntoSQLDWDataFactory** **nimi**.
        Azure tietojen factory nimen on oltava yksilöivä. Jos saat virheilmoituksen: **tietojen factory nimi "LoadIntoSQLDWDataFactory" ei ole käytettävissä**, tietojen factory (esimerkiksi yournameLoadIntoSQLDWDataFactory) nimen muuttaminen ja yritä uudelleen. Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.  
     
    2. Valitse Azure **tilauksen**.
    3. Resurssiryhmän jokin seuraavista toimista: 
        1. Valitse Valitse aiemmin luotu resurssiryhmä **käyttää aiemmin luotuja** .
        2. Valitse **Luo uusi** resurssiryhmä nimi.
    3. Valitse tiedot-factory **sijainti** .
    4. Valitse **raporttinäkymät-ikkunan kiinnittäminen** valintaruutu sivu alareunassa.  
    5. Valitse **Luo**.
10. Kun luominen on valmis, näyttöön tulee **Data Factory** -sivu, seuraavassa kuvassa esitetyllä tavalla:

    ![Tietoja factory-kotisivu](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Valitse Data Factory-kotisivulla Käynnistä **Ohjattu kopiointi** **kopioida tiedot** -ruutu. 

    > [AZURE.NOTE] Jos huomaat, että selain pysähtyy "Sallimisesta...", **Estä kolmannen osapuolen evästeet ja sivuston tiedot** -asetus käytöstä ja poista valinta (tai) säilyttää sen käytössä ja luoda poikkeuksen **login.microsoftonline.com** ja yritä sitten ohjatun toiminnon käynnistäminen uudelleen.


## <a name="step-1-configure-data-loading-schedule"></a>Vaihe 1: Määritä tietojen lataaminen aikataulu
Ensimmäiseksi on määrittäminen tietojen lataaminen aikataulu.  

**Ominaisuudet** -sivulla:
1. Kirjoita **tehtävänimi** **CopyFromBlobToAzureSqlDataWarehouse**
2. Valitse **kerran Suorita** -vaihtoehto.   
3. Valitse **Seuraava**.  

![Ohjattu - ominaisuudet-sivun kopioiminen](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Vaihe 2: Määritä lähde
Tässä osassa näytetään, miten voit määrittää lähde: Azure-Blob sisältävän rivin 1 TT TPC-H kohta tiedostot.

Valitse **Azure-Blob-säiliö** , kuten tiedot tallennetaan, ja valitse **Seuraava**.

![Ohjattu kopiointi - Valitse lähdesivu](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Täytä Azure Blob storage tilin yhteystiedot ja valitse **Seuraava**.

![Ohjattu kopiointi - tietolähteen yhteystietoja](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Valitse **kansio** , jossa on TPC H rivi-tiedostot ja valitse **Seuraava**.

![Kopioi ohjattu - syötteen kansio](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Kun valitset **Seuraava**, tiedoston muotoiluasetukset havaitaan automaattisesti.  Varmista, että sarake-erotin on "|"sijaan oletusarvon pilkku' tai '.  Valitse **Seuraava** , kun tiedot on esikatsellaan.

![Ohjattu kopiointi - tiedoston muotoiluasetukset](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Vaihe 3: Määritä kohde
Tässä osassa kerrotaan, miten voit määrittää kohde: `lineitem` Azure SQL-tietovarasto tietokannan taulukkoon.

Valitse kohde-kaupan **Azure SQL-tietovarasto** ja valitse **Seuraava**.

![Ohjattu kopiointi - Valitse kohde tietosäilö](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Täytä Azure SQL-tietovarasto yhteystiedot.  Varmista, että määrität käyttäjälle, joka on roolin jäsen `xlargerc` (Katso lisätietoja **edellytykset** -osio), ja valitse **Seuraava**. 

![Ohjattu kopiointi - kohteen yhteyden tiedot](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Valitse kohdetaulukko ja valitse sitten **Seuraava**.

![Kopioi Ohjattu - taulukon määritys-sivu](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Hyväksy oletusasetukset sarakkeen yhdistäminen ja valitse **Seuraava**.

![Kopioi Ohjattu - rakenteen määritys-sivu](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Vaihe 4: Suorituskyvyn asetukset

**Salli polybase** on valittuna oletusarvoisesti.  Valitse **Seuraava**.

![Kopioi Ohjattu - rakenteen määritys-sivu](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Vaihe 5: Käyttöönotto ja lataa tulokset seuranta
Valitse **Valmis** -painike, jos haluat ottaa käyttöön. 

![Ohjattu kopiointi - yhteenveto-sivu](media/data-factory-load-sql-data-warehouse/summary-page.png)

Kun asennus on valmis, valitse `Click here to monitor copy pipeline` seurannassa Suorita edistymisen kopio.

Valitse Kopioi putkijohto luomasi **Toiminta Windows** -luettelossa.

![Ohjattu kopiointi - yhteenveto-sivu](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Voit tarkastella suorittaa tiedot **Tehtävän ikkunan Explorer** oikeanpuoleisessa, mukaan lukien tietojen lukeminen lähteestä ja kirjoittaa ne kohde, kesto ja keskimääräinen siirtonopeuden, suorita kopio.

Kuten näet seuraavista näyttökuvan, 1 TT kopioidaan tietovarasto SQL Azure-Blob-säiliö noudatit 14 minuutin saavuttamiseksi tehokkaasti 1,22 GBps siirtonopeuden!

![Ohjattu kopiointi - onnistui-valintaikkuna](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Parhaat käytännöt
Seuraavassa on joitakin parhaita käytäntöjä Azure SQL-tietovarasto tietokannan käynnissä:

- Käytä suuremman resurssiluokkaa, kun INDEKSIN COLUMNSTORE ladataan.
- Harkitse tehokkaampaa liitokset hash-jakauman avulla Valitse sarake sen sijaan, että oletusarvon mukaista Mikko jakauman.
- Saat nopeamman kuormituksen nopeuksia kannattaa käyttää keon lyhytkestoisia tiedot.
- Luo tilastotietoja, kun olet valmis ladataan Azure SQL-tietovarasto.

Katso lisätietoja [SQL Azure-tietovarasto parhaat käytännöt](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Seuraavat vaiheet
- [Ohjattu tietojen Factory kopio](data-factory-copy-wizard.md) - artikkelissa on tietoja Ohjattu kopiointi. 
- [Kopioi tehtävän suorituskyky ja säätäminen guide](data-factory-copy-activity-performance.md) - artikkelissa on viittaus suorituskyvyn mitat ja säätäminen opas.

