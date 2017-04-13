<properties
    pageTitle="Tietoja Factory opetusohjelma: ensimmäisen tietojen myyntijakso | Microsoft Azure"
    description="Azure Data Factory Tässä opetusohjelmassa näytetään, miten voit luoda ja ajoittaa tietojen factory, joka käsittelee rakenteen komentosarjan käyttäminen Hadoop-klusterin tietoja."
    services="data-factory"
    keywords="Azure tietojen factory opetusohjelman, hadoop klusterin, hadoop-rakenne"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Opetusohjelma: Oman ensimmäisen myyntijakso prosessin tietoihin käyttämällä Hadoop-klusterin luominen 
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)

Tässä opetusohjelmassa voit luoda oman ensimmäisen Azure tietojen factory tietojen putkijohto, joka käsittelee tietoja suorittamalla rakenteen komentosarja Azure Hdinsightiin (Hadoop)-klusterissa kanssa. 

> [AZURE.NOTE] Tässä artikkelissa ei tarjoa Azure Data Factory Käsitteellinen yleiskatsaus. Käsitteellinen yleiskatsaus palvelun artikkelissa [Azure Data Factory esittely](data-factory-introduction.md). Katso [tietoja Factory oppimispolku](https://azure.microsoft.com/documentation/learning-paths/data-factory/) suositellut tavan siirtyä Microsoftin sisällön Data Factory lisätietoja.

## <a name="whats-covered-in-this-tutorial"></a>Mitä käsitellään tässä opetusohjelmassa? 
**Azure Data Factory** avulla voit luoda tietojen **siirtämistä** ja tietojen **käsittely** tehtävien kuin tietoihin perustuvien työnkulkuja (kutsutaan myös tietojen putkistot). Kerrotaan, miten voit luoda ensimmäisen tietojen putkijohto ja tietojen käsittely (tai muuntamiseen) aktiviteetin. Tässä aktiviteetti käyttää HDInsight Hadoop-klusterin Muunna ja analysoida otoksen web-lokit.  

Tässä opetusohjelmassa toimimalla seuraavasti:

1.  Luo **tietojen factory**. Tietoja factory voi olla yksi tai enemmän tietoja putkijohdot siirtäminen ja käsitellä tietoja. 
2.  Luo **linkitetty palvelut**. Voit luoda linkitetyn palvelun linkitettävän tietosäilö tai Laske palvelun tiedot factory. Tietosäilö, kuten Azuren tallennustilaan on i/tietojen toiminnan putkijohto. Laske-palvelun, kuten HDInsight Hadoop-klusterin prosessit/muunnoksia tiedot.    
3.  Luo syötteen ja tulosteen **tietojoukkoja**. Kirjoita tietojoukon edustaa putkijohto toimintaa syötteen ja tulostus-tietojoukko toiminnon tulos.
3.  Luo **myyntijakso**. Putkijohto voi olla vähintään yksi toiminnot (esimerkkejä: kopioi aktiviteetti, HDInsight rakenne tehtävän). Tässä esimerkissä käytetään HDInsight Hadoop-klusterin rakenteen komentosarjaa suorittava HDInsight rakenteen tehtävän. Komentosarja luo ensin taulukko, joka viittaa Azuren Blob-objektien tallennustilaan tallennettuja raaka web lokitiedot ja sitten osioiden perustietoja vuoden ja kuukauden mukaan.

    On kahdentyyppisiä tuettavat Azure Data Factory mukaan. Ne ovat: [tietojen siirtämistä toimintojen](data-factory-data-movement-activities.md) ja [tietojen muunnos](data-factory-data-transformation-activities.md). On vain yksi tietojen siirtämistä activity, joka on kopio-tehtävän. Tässä opetusohjelmassa et käytä kopioi-tehtävän. Opetusohjelmaan, kopio-toiminnon käyttämisestä, katso [Opetusohjelma: kopioi tiedot azuren blob-Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). HDInsight rakenne tehtävän käytät Tässä opetusohjelmassa on Data Factory tukemat tietojen muunnos-toimintoja.  
 
Tässä on esimerkki tietojen tehdas luot tässä opetusohjelmassa **kaavionäkymä** . 

![Data Factory opetusohjelmassa kaavionäkymä](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

Tässä opetusohjelmassa **inputdata** kansioon **adfgetstarted** Azure-blob-säilö sisältää yhden tiedoston input.log. Lokitiedosto on kolme kuukautta tapahtumat: tammikuussa, helmikuussa ja maaliskuussa, 2016. Seuraavassa on esimerkki rivit jokaiselle kuukaudelle input-tiedostossa. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Myyntijakso HDInsight rakenne aktiviteettiin käsittelyn tiedoston tehtävän suorittaa rakenteen komentosarjan HDInsight-klusterin osioiden syötteen tietoja vuoden ja kuukauden mukaan. Komentosarja luo kolme tulostus-kansioita, jotka sisältävät tiedoston tapahtumat kuukausittain.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Esimerkki riveiltä yllä, ensimmäinen (ja 2016-01-01) on kirjoitettu 000000_0 tiedoston kuukauden = 1-kansio. Vastaavasti toinen kirjoitetaan tiedoston kuukauden = 2 kansio ja kolmas yhden tiedoston kirjoitetaan kuukauden = 3 kansio.  


## <a name="pre-requisites"></a>Vaatimukset
Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavat edellytykset:

1.  **Azure tilauksen** – Jos sinulla ei ole Azure tilauksen voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/) artikkelissa, kuinka voit hankkia ilmainen kokeiluversio tili.

2.  **Azuren tallennustilaan** – tietojen tallentamista tässä opetusohjelmassa Azure-tallennustilan tilin käyttäminen. Jos sinulla ei ole Azure-tallennustilan tilin, on artikkelissa [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) . Luotuasi tallennustilan tilin Huomautus **tilin nimi** ja **Pikanäppäin**. Katso [näkymässä, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Tiedostojen lataaminen opetusohjelman Azuren tallennustilaan
Ennen opetusohjelman aloittamista, sinun täytyy valmistautua Azure-tallennustilan tilin kanssa mallitiedostojen opetusohjelman.

1. Lataa **komentosarja** -kansion **adfgetstarted** blob-säilö kyselyn rakennetiedostoon (HQL).
2. Tiedoston lataaminen **adfgetstarted** blob-säilö **inputdata** -kansioon. 

#### <a name="create-hql-script-file"></a>Luo HQL-komentosarjatiedosto 

1. **Muistion** käynnistäminen ja liitä seuraava HQL. Tämä rakenne-komentosarja luo kaksi taulukkoa: **WebLogsRaw** ja **WebLogsPartitioned**. Valitse **Tiedosto** -valikon ja valitse **Tallenna nimellä**. Siirry kiintolevyn **C:\adfgetstarted** -kansioon. Valitse * *kaikki tiedostot (*.*) **varten** Tallenna, kun kirjoitat** kentän. Kirjoita **partitionweblogs.hql** varten **tiedostonimi**. Varmista, että **koodaus** valintaikkunan alareunassa-kentän arvoksi **ANSI**. Jos näin ei ole, aseta sen arvoksi **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Suorituksen Data Factory putkijohto rakenne-toimintojen välittää arvot **inputtable** ja **partitionedtable** parametrit seuraavat koodikatkelman esitetyllä tavalla:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname** on Azure-tallennustilan tilin nimi.
 
#### <a name="create-a-sample-input-file"></a>Luo syötteen mallitiedosto
Muistiossa, Luo tiedosto nimeltä **input.log** - **c:\adfgetstarted** seuraavat sisältöä: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Syötteen tiedoston ja HQL-tiedoston lataaminen Azure-Blob-objektien tallennustilaan

Tässä osassa on ohjeet input.log ja partitionweblogs.hql tiedostojen kopioiminen Azure-Blob-säiliö **AzCopy** -työkalun avulla. Voit käyttää mitä tahansa valittua työkalua (esimerkiksi: [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com/), [CloudXPlorer ClumsyLeaf ohjelmien](http://clumsyleaf.com/products/cloudxplorer) tässä tehtävässä.   
     
1. Lataa [uusin versio **AzCopy**](http://aka.ms/downloadazcopy)tai [uusimman preview-versiosta](http://aka.ms/downloadazcopypr). Katso [käyttämisestä AzCopy](../storage/storage-use-azcopy.md) artikkelista-apuohjelmalla.
2. Siirry kansioon, c:\adfgetstarted ja suorittamalla seuraavan komennon: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Tämä komento lataa **input.log** tiedoston tallennustilan-tilille (**adfgetstarted** säilö ja **inputdata** kansio). Korvaa **storageaccountname** tallennustilan tilin ja **storageaccesskey** nimiä ja tallennustilaa pikanäppäin.

    > [AZURE.NOTE] Tämä komento luo säilö **adfgetstarted** Azure-Blob-objektien tallennustilaan ja kopioi **input.log** tiedoston paikallisesta asemasta säilön **inputdata** -kansioon. 
    
3. Kun tiedosto on ladattu, näet samalla AzCopy kuin yksi tulos.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Suorita seuraava komento **partitionweblogs.hql** -tiedoston lataaminen **adfgetstarted** säilö **komentosarja** -kansio. Näin komento: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Edellytykset täyttyvät. Voit luoda tietojen-factory jollakin seuraavista tavoista. Valitse ylä-tai suorittaa opetusohjelman linkkejä välilehteä. 

- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)