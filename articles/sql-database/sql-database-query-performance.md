<properties 
   pageTitle="Azure SQL-tietokannan kyselyn suorituskykyä tietoja" 
   description="Useimmat Keskusyksikön muissa kyselyt kyselyn suorituskyvyn seurantaa tunnistaa Azure SQL-tietokannan." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL-tietokannan kyselyn suorituskykyä tietoja


Hallinta ja relaatiotietokannasta suorituskyvyn säätö on hankalaa tehtävä, joka edellyttää merkittäviä osaamisalueet ja aikaa sijoitus. Kyselyn suorituskykyä tietoja avulla voit käyttää vähemmän aikaa tietokannan suorituskyvyn vianmääritys toimittamalla seuraavat:

- Tietokantojen resurssi (DTU)-kulutus tarkempaa tietoja. 
- Suorittimen ja keston/suorituskertojen määrä, jotka mahdollisesti voit optimoitu suorituskyvyn paraneminen mukaan kyselyjä.
- Sen tekstin ja historiatietojen resurssien käytön tarkasteleminen voi siirtyä alaspäin kyselyn. 
- Suorituskyvyn säätäminen huomautukset, jotka näyttävät [SQL Azure-tietokannan Advisor](sql-database-advisor.md) toimintoja  



## <a name="prerequisites"></a>Edellytykset

- Kyselyn suorituskykyä tietoja on käytettävissä vain Azure SQL-tietokannan Vipuventtiili V12.
- Kyselyn suorituskykyä tietoja edellyttää, että [Kysely säilö](https://msdn.microsoft.com/library/dn817826.aspx) on aktiivinen tietokannan. Jos kyselyn kaupan ei ole käynnissä, portaalin kysyy, haluatko ottaa sen käyttöön.

 
## <a name="permissions"></a>Käyttöoikeudet

[Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md) seuraavat oikeudet on käytettävä kyselyn suorituskykyä tietoja: 

- Tarkastella yläreunan resurssin vievää kyselyt ja kaaviot tarvitaan **lukijan**, **omistaja**, **osallistuja**, **SQL DB osallistuja**tai **SQL Server osallistuja** -oikeudet. 
- Voit tarkastella KyselyTeksti tarvitaan **omistaja**, **osallistuja**, **SQL DB osallistuja**tai **SQL Server osallistuja** -oikeudet.



## <a name="using-query-performance-insight"></a>Kyselyn suorituskykyä tietoja käyttämällä

Kyselyn suorituskykyä tietoja on helppo käyttää:

- Avaa [Azure-portaali](https://portal.azure.com/) ja Etsi tietokanta, jonka haluat tarkistaa. 
  - Valitse vasemmanpuoleisessa osassa valikosta tuki sekä vianmääritysohjeita on kohdassa "Kyselyn suorituskykyä tietoja".
- Tarkista top resurssien käyttö-kyselyjen luettelon ensimmäinen välilehti.
- Valitse voi tarkastella sen yksittäisten kysely.
- Avaa [SQL Azure-tietokannan Advisor](sql-database-advisor.md) ja tarkista, onko suositusten käytettävissä.
- Käytä liukusäätimiä tai Zoomaa kuvakkeet havaittujen välin muuttaminen.

    ![suorituskyvyn koontinäyttö](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Tietoja muutaman tunnin on SQL-tietokantaan antamaan kyselyn suorituskykyä havainnollistamisen kaupasta kyselyn: llä. Jos tietokannalla on aktiviteettia tai kyselyn kaupan ei ole aktiivinen tietyn ajanjakson aikana, kaaviot on tyhjä, aika-asteikon näyttämiseen. Voit ottaa käyttöön kyselyn kaupan milloin tahansa, jos se ei ole käynnissä.   



## <a name="review-top-cpu-consuming-queries"></a>Tarkista yläreunan suorittimen käyttö kyselyissä

[Portaalissa](http://portal.azure.com) seuraavasti:

1. Siirry SQL-tietokanta ja valitse **kaikki asetukset** > **tuki + vianmääritys** > **kyselyn suorituskykyä tietoja**. 

    ![Kyselyn suorituskykyä tietoja][1]

    Suosituimmat kyselyt näkymä avataan ja suorittimen vievää kyselyjä näkyvät.

1. Valitse kaavion lisätietoja ympärille.<br>Sivun näyttää Yleinen DTU % tietokannalle, palkit näyttää suorittimen % aikaista valitun kyselyjen valitun aikavälin (esimerkiksi, jos **edellisen viikon** valitaan kukin palkki edustaa yhtä päivää).

    ![Suosituimmat kyselyt][2]

    Ala-ruudukon edustaa näkyvissä kyselyjen kootut tiedot.

  - Kyselytunnus – yksilöllinen kyselyn tietokannan sisällä.
  - Suorittimen kysely aikana havaittavia väli (vaihtelee koostefunktio).
  - Kysely kesto (vaihtelee koostefunktio).
  - Tietyn kyselyn suorituskerran määrä.

    Valitse tai poista yksittäisiä kyselyitä, voit lisätä tai poistaa niitä kaaviosta käyttämällä valintaruutuja.

1. Jos tiedoissa on vanhentuneita, napsauta **Päivitä** -painiketta.
1. Voit käyttää liukusäätimiä ja Zoomauspainikkeet Huomautus aikaväli ja tutkia piikkarit:  ![asetukset](./media/sql-database-query-performance/zoom.png)
1. Voit myös halutessasi eri näkymä voit valita **Mukautettu** -välilehti ja määritä:
  
  - Arvo (Suoritin, kesto, suorituskertojen määrä)
  - Aikaväli (viime viikolla, viime kuussa viimeisen 24 tuntia). 
  - Kyselyjen määrä.
  - Koostefunktio.

    ![asetukset](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Yksittäisen kyselyn tietojen tarkasteleminen

Voit tarkastella Kyselytiedot seuraavasti:

1. Valitse mikä tahansa kysely Suosituimmat kyselyt-luettelosta.

    ![tiedot](./media/sql-database-query-performance/details.png)

1. Tiedot-näkymässä avautuu ja kyselyjen suorittimen kulutus/kesto/suorituskertojen määrä on eritellä ajan kuluessa.
1. Valitse kaavion lisätietoja ympärille.
  - Ylimmän kaavio näyttää rivi, jonka yleinen tietokannan DTU % ja pylväiden suorittimen % kulutettu valitun kyselyn.
  - Toisessa kaaviossa näkyy kokonaiskesto valitun kyselyn.
  - Ala-kaavio näyttää kokonaismäärän suorituskerran valitun kyselyn.
    
    ![Kyselytiedot][3]

1. Voit myös käyttää liukusäätimiä, Zoomauspainikkeet tai **asetuksia** voit mukauttaa näkymän kyselyn tiedot tai valita eri ajan kuluessa.

## <a name="review-top-queries-per-duration"></a>Tarkista yläreunan kyselyiden kesto kohden

Kyselyn suorituskykyä tietoja viimeisimmät päivityksessä on otettu käyttöön kaksi uudet arvot, joka auttaa sinua huomaamaan mahdolliset pullonkaulat: keston ja suorittamisen määrä.<br>

Kyselyissä on eniten mahdollisuuksia lukitseminen pidempi resursseja, estäminen muilta käyttäjiltä ja skaalattavuus rajoittaminen. He ovat myös parhaat ehdokkaiden optimointi.<br>

Voit määrittää pitkään käynnissä kyselyjen seuraavasti:

1. Avaa **Mukautettu** -välilehden kyselyn suorituskykyä tietoja valitun tietokannan
1. Arvot olevan **keston** muuttaminen
1. Valitse Kyselyt ja Huomautus väli
1. Kooste-funktio
  - **Summa** laskee yhteen kaikki kyselyn suoritusaika koko Huomautus aikavälin aikana.
  - **Maks** löytää kyselyt, mitä suoritusaika oli suurin koko Huomautus väliajoin.
  - **Avg** etsii kaikki kyselyn suorituskerran keskimääräinen suoritusaika ja näyttää keskiarvot näiden ulos yläreunassa. 

    ![kyselyn kesto][4]

## <a name="review-top-queries-per-execution-count"></a>Tarkista yläreunan kyselyiden kohden suorituskertojen määrä

Suuri määrä suorituskerran ehkä eivät vaikuta itse tietokannan ja resurssien käyttö voi olla pieni, mutta sovelluksen yleistä saada hidasta.

Joissakin tapauksissa erittäin suuri suorituskertojen määrä voi johtaa kasvua verkon trips PYÖRISTÄ-funktiota. PYÖRISTÄ-funktiota trips vaikuttaa merkittävästi suorituskykyä. He ovat veloittaa verkon latenssin ja edeltävät palvelimen viive. 

Esimerkiksi tietopohjaisten WWW sivustoja käyttää raskaasti jokaisen käyttäjäpyynnön tietokanta. Kun yhteys jakavan auttaa parantavat verkkoliikennettä ja käsittelyn kuormituksen tietokantapalvelimen voi heikentää suorituskykyä.  Yleiset neuvoja on pitää PYÖRISTÄ-funktiota trips suora vähän.

Tunnistaa usein suorittaa kyselyjä ("chatty") kyselyt:

1. Avaa **Mukautettu** -välilehden kyselyn suorituskykyä tietoja valitun tietokannan
1. Muuta arvot voidaan **suorituskertojen määrä**
1. Valitse Kyselyt ja Huomautus väli

    ![kyselyn suorituskertojen määrä][5]

## <a name="understanding-performance-tuning-annotations"></a>Tietoja suorituskyvyn säätäminen huomautukset 

Kun tutkiminen havainnollistamiseen-kyselyn suorituskykyä tietoja, saatat huomata kuvakkeita, joissa on pystyviivan kaavion päälle.<br>

Nämä kuvakkeet ovat huomautuksia; ne edustavat suorituskykyyn vaikuttavia [SQL Azure-tietokannan Advisor](sql-database-advisor.md)toimintoja. Hiiren huomautuksen voit hakea perustietoja toiminnon:

![kyselyn Huomautus][6]

Jos haluat lisätietoja tai käytä advisor suositus, napsauta kuvaketta. Se avautuu-toiminnon yksityiskohdat. Jos se on aktiivinen suositus voit käyttää sitä suoraan poissa-komennolla.

![kyselyn huomautuksen yksityiskohdat][7]

### <a name="multiple-annotations"></a>Useita huomautuksia. ###

Se on mahdollista, että vuoksi zoomaustason huomautukset, jotka ovat lähellä toisiaan saada kutistettu yhdeksi. Tämä edustaa määräten kuvakkeen mukaan, napsauttaminen avaa uusi sivu, jossa luettelo ryhmitelty huomautukset tulee näytetään.
Hajautettuna kyselyt ja suorituskyvyn säätö toiminnot auttavat ymmärtämään havainnollistamiseen. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Kyselyn suorituskykyä tietoja kyselyn kaupan määrityskohde optimointi

Oman kyselyn suorituskykyä tietoja käytön aikana tulla kyselyn kaupan seuraavista ilmoituksista:

- "Kyselyn kaupan ei ole määritetty oikein, tämä tietokanta. Napsauttamalla tätä saat lisätietoja."
- "Kyselyn kaupan ei ole määritetty oikein, tämä tietokanta. Napsauta tätä asetusten muuttamiseen." 

Viestit näkyvät yleensä kyselyn kaupan ei voi kerätä uusia tietoja. 

Ensimmäinen tapauksessa tapahtuu, kun kysely säilö on vain luku-tilaan ja parametrit on määritetty optimaalisesti. Voit korjata tämän kyselyn kaupan koon suurentaminen tai poistamalla kyselyn kaupan.

![qds-painike][8]

Toisessa tapauksessa tapahtuu, kun kysely säilö on poistettu käytöstä tai parametrit ei ole määritetty optimaalisesti. <br>Voit muuttaa säilytys- ja sieppaus-käytäntö ja ota kyselyn kaupan suoritetaan alla olevia komentoja tai suoraan portaalin:

![qds-painike][9]

### <a name="recommended-retention-and-capture-policy"></a>Suositeltu säilytys- ja sieppaus käytäntö

Säilytyskäytännöt on kahdenlaisia:

- Koon mukaan – Jos asetukseksi automaattinen se puhdistaa tiedot automaattisesti, kun lähellä enimmäiskoon on saavutettu.
- Ajan perusteella - oletusarvoisesti olemme arvoksi 30 päivää, mikä tarkoittaa sitä, jos kyselyn kaupan suoritetaan loppumassa, se poistetaan kyselyn tiedot 30 päivää vanhemmat

Sieppaa käytännön voi asettaa:

- **Kaikki** – sieppaa kaikki kyselyt.
- **Automaattinen** – epäsäännölliset kyselyt ja kyselyjen vähäinen Käännä ja suorittamisen ajallisen ohitetaan. Raja-suorittamisen määrä, käännä ja runtime kesto määritetään sisäisesti. Tämä on oletusarvo.
- **Ei mitään** – kyselyn säilö pysäyttää sieppaus uusissa kyselyissä, kuitenkin yhä runtime tilasto jo otettu kyselyiden kerätään.
    
Suosittelemme, että kaikki käytäntöjen määrittäminen ja automaattinen SIIVOA käytännön 30 päivää:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Suurenna kysely-kaupasta. Tämä voi suorittaa muodostaa yhteyden tietokantaan ja lähettämällä kyselyn seuraavat:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Nämä asetukset otetaan käyttöön saat myöhemmin kyselyn kaupan Keräätkö uusia kyselyjä, mutta jos et halua odottaa voit poistaa kyselyn kaupan. 
> [AZURE.NOTE] Seuraava kysely suoritetaan poistaa kaikki nykyiset tiedot kyselyn Storessa. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Yhteenveto

Kyselyn suorituskykyä tietoja auttaa sinua ymmärtämään kyselyn kuormituksen vaikutus ja miten se liittyy tietokannan resurssin kulutus. Tämän ominaisuuden haluat muissa kyselyjen sivun tietoja ja tunnistamisen korjattava, ennen kuin jotteivät ne niistä.




## <a name="next-steps"></a>Seuraavat vaiheet

Valitse lisäsuosituksia tietoja SQL-tietokannan suorituskyvyn parantamiseen [suositukset](sql-database-advisor.md) - **Kyselyn suorituskykyä tietoja** -sivu.

![Performance Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

