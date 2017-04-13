<properties
    pageTitle="Luo tietojen ominaisuudet rakenteen käyttäminen Hadoop-klusterin | Microsoft Azure"
    description="Esimerkkejä rakenteen kyselyitä, jotka Luo ominaisuuksia Azure Hdinsightiin Hadoop-klusterin tallennettuja tietoja."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Tietojen ominaisuudet luominen Hadoop-klusterin, rakenteen käyttäminen

Tässä asiakirjassa näkyy luomisesta ominaisuudet rakenteen käyttäminen Azure Hdinsightiin Hadoop-klusterin tallennettuja tietoja. Näitä rakenne-kyselyt käyttävät upotetun rakenne käyttäjän määrittämät funktiot (UDF), komentosarjat, jossa on tarkoitettu.

Luomiseksi ominaisuuksien toimintoja voidaan muistin paljon resursseja. Rakenne-kyselyiden suorituskykyä tulee Lisää Kriittinen tällaisissa tapauksissa ja voidaan parantaa mukaan säätäminen tiettyjä parametreja. Viimeisessä osassa kuvattua nämä parametrit säätäminen.

Kyselyitä, jotka on esitetty esimerkkejä ovat [Kokousesitelmän taksin työmatkan tietojen](http://chriswhong.com/open-data/foil_nyc_taxi/) skenaariot toimitetaan myös [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)säilöön. Nämä kyselyt on jo on määritetty tietojen rakenne, ja olet valmis suorittamaan toimitettavat. Viimeisessä osassa parametrit, joita käyttäjät voivat paranna niin, että rakenne kyselyjen suorituskykyä voi parantaa myös käsitellään.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit luoda tietojen ominaisuudet eri ympäristöissä. Tämä onkin vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että sinulla on:

* Luoda Azure-tallennustilan tilin. Jos tarvitset ohjeita, katso [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Valmisteltu mukautetun Hadoop-klusterin HDInsight-palvelussa.  Jos tarvitset ohjeita, katso [Mukauttaminen Azure Hdinsightiin Hadoop klustereiden Advanced analysoinnissa](machine-learning-data-science-customize-hadoop-cluster.md).
* Tiedot on ladattu Azure Hdinsightiin Hadoop klustereiden rakenne-taulukoihin. Jos se ei ole, noudata tietojen lataaminen rakenteen taulukoiden ensin [Luo ja lataa tiedot rakenteen taulukoihin](machine-learning-data-science-move-hive-tables.md) .
* Käytössä klusterin etäkäyttö. Jos tarvitset ohjeita, on artikkelissa [Head solmu, Hadoop klusterin](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Ominaisuuden luominen

Tässä osassa kuvataan useita esimerkkejä siitä jonka ominaisuuksia voit tuota rakenteen käyttäminen. Kun olet luonut lisäominaisuuksia, voit lisätä sarakkeita kuin aiemmin luotuun taulukkoon tai Luo uusi taulukko, jossa lisäominaisuudet ja perusavaimen, joka on liitetty sitten alkuperäisen taulukon kanssa. Seuraavassa on esitetty Esimerkkejä:

1. [Taajuus-pohjainen ominaisuuden luominen](#hive-frequencyfeature)
2. [Riskien Categorical muuttujat binaarinen luokitus](#hive-riskfeature)
3. [Poimia Datetime-kentän ominaisuudet](#hive-datefeatures)
4. [Ominaisuuksien poimiminen tekstikenttä](#hive-textfeatures)
5. [Laske GPS koordinaatit välinen etäisyys](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Taajuus-pohjainen ominaisuuden luominen

Kannattaa usein käyttää categorical muuttujan tasoja tiheydet tai tiettyjen yhdistelmät useita categorical muuttujien tasot tiheydet laskemiseen. Käyttäjät voivat käyttää seuraavaa komentosarjaa nämä taajuudet laskemiseen:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Riskien Categorical muuttujien binaarinen luokittelu

Binaarinen luokitukseen annettava ei-numeerinen categorical muuttujat muuntaa numeerisen ominaisuuksia, kun käytössä vain mallit kestää numeerinen ominaisuuksia. Tämä tapahtuu korvaamalla ei-numeerinen kunkin tason numeerinen riskin. Tässä osassa on Näytä jotkin yleinen rakenne-kyselyt, jotka laskevat categorical muuttujan riskin arvot (log todennäköisyydellä).


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Tässä esimerkissä muuttujat `smooth_param1` ja `smooth_param2` määritetään tasaiset riskin arvot lasketaan tiedoista. Riskien on välillä -Inf ja Inf. Riskien > 0 osoittaa, että kohde on yhtä suuri kuin 1 todennäköisyys on suurempi kuin 0,5.

Riskien jälkeen taulukon on laskettu, käyttäjät voivat määrittää riskin arvot taulukon yhdistämällä riski-taulukkoon. Edellisen osan määritettyä liittävä kyselyn rakenne.

###<a name="hive-datefeatures"></a>Ominaisuuksien poimiminen Datetime-kentät

Rakenteen sisältyy UDF joukko käsittelyyn datetime-kentät. Rakenne-datetime oletusmuoto on "yyyy-MM-dd 00:00:00" ("1970-01-01 12:21:32 ' esimerkiksi). Tässä osassa on näkyy esimerkkejä, jotka Pura päivä, kuukausi-, kuukausi datetime-kentästä ja muita esimerkkejä, jotka muuntavat datetime-muodossa merkkijonon muut kuin oletusmuoto on datetime-merkkijono on kohdassa Muotoile.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Kyselyn rakenne oletetaan, että *& #60; datetime-kentän >* on oletusarvoinen datetime-muodossa.

Jos datetime-kenttää ei ole oletusmuoto, sinun on datetime-kentän muuntaminen Unix aikaleiman ensin ja sitten Muunna Unix-aikaleiman datetime-merkkijono, joka on oletusmuoto. Kun DateTime-arvo on kohdassa muoto, käyttäjät voivat käyttää upotetun datetime UDF Pura ominaisuuksia.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Tässä kyselyssä Jos *& #60; datetime-kentän >* on esimerkiksi kuvion *03/26/2015 12:04:39*, *"& #60; datetime-kentän kuvio >'* pitäisi olla `'MM/dd/yyyy HH:mm:ss'`. Testaa se käyttäjät voivat suorittaa

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* tässä kyselyssä on esiasennettuna kaikki Azure Hdinsightiin Hadoop klustereiden oletusarvoisesti kun varausyksiköiden ovat valmistelun yhteydessä.


###<a name="hive-textfeatures"></a>Ominaisuuksien poimiminen tekstikentät

Kun taulukko rakenne on tekstikenttä, joka sisältää merkkijonon sanat, jotka on erotettu toisistaan välilyönnillä, seuraava kysely purkaa merkkijonon ja sanamäärä merkkijonon pituuden.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Laske etäisyydet väliset GPS koordinaatit

Tämän osan kyselyn voi suojata suoraan Kokousesitelmän taksin työmatkan tietoihin. Tämä kysely tarkoituksena on näyttää, miten upotetun matemaattiset funktiot rakenteen luomiseen ominaisuuksia.

Kentät, joita käytetään tässä kyselyssä ovat nouto- ja dropoff sijainnit, nimeltä GPS koordinaatit *nouto\_pituutta*, *nouto\_leveyttä*, *dropoff\_pituutta*, ja *dropoff\_leveyttä*. Kyselyitä, jotka laskevat nouto- ja dropoff koordinaatteja suoraan etäisyys ovat seuraavat:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Matemaattisia kaavoja, jotka laskevat etäisyys kaksi GPS koordinaatit löytyy <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type komentosarjat</a> -sivustossa Peter Lapisu tekijä. Valitse hänen Javascript-funktion `toRad()` on vain *lat_or_lon*pii/180*, joka muuntaa asteet radiaaneiksi. Tässä kohdassa *lat_or_lon * on leveyttä tai pituutta. Koska rakenne ei tarjoa funktion `atan2`, mutta funktion `atan`, `atan2` funktion täytäntöön `atan` yllä rakenne-kysely, jossa määritelmän annettu <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>-funktion.

![Työtilan luominen](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Upotettu UDF löytyy <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache rakenne wiki</a> **Sisäiset funktiot** -osan rakenne täydellinen luettelo).  

## <a name="tuning"></a>Laajennettu aiheita: parantaa kyselyn nopeus hienosäätää rakenne parametrit

Rakenne-klusterin parametrin oletusasetukset eivät välttämättä ole sopii rakenne-kyselyiden ja kyselyjen käsitellään tiedot. Tässä osiossa käsiteltävät aiheet joitakin parametreja, käyttäjät voivat paranna parantaa rakenteen kyselyjen suorituskykyä. Käyttäjien on lisättävä parametrin säätäminen kyselyt ennen tietojen käsittelystä kyselyt.

1. **Java keon tilaa**: kyselyjen liittyminen suuria tietojoukkoja tai pitkien tietueiden käsittely- **keon loppumassa** on yksi yleinen virhe. Sen virittää määrittämällä parametreja *mapreduce.map.java.opts* ja *mapreduce.task.io.sort.mb* haluamasi arvot. Tässä on esimerkki:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Tämä parametri varaa 4 Gigatavun muistia Java keon kohtaan, ja myös tekee lajittelusta tehokkaamman kohdistamalla muistia sen. Se on hyvä toistaa nämä kohdistukset kanssa, jos ne on minkä tahansa työn virhe virheet keon tilaa.

2. **DFS estää kokoa** : Tämä parametri määrittää tiedot, jotka tiedostojärjestelmän tallentaa pienin yksikkö. Esimerkkinä että jos DFS koko on 128 Mt, valitse kaikki tiedot, jonka koko on pienempi kuin ja enintään 128 Mt tallennetaan yhtenä solualueena, kun tietojen, joka on suurempi kuin 128 Mt varataan ylimääräisiä lohkot. Valitsemalla vähän estä koon aiheuttaa suuria yleiskulut Hadoop, koska nimi-solmu on käyttänyt paljon enemmän pyyntöjä Etsi tiedostoon liittyvät asiaa estä. Suositeltu asetus käsitellään tarvittaessa gigatavua (tai suurempi) tiedot ovat:

        set dfs.block.size=128m;

3. **Optimointi liittyä rakenne-toiminto** : kun join-toiminnot kartta ja vähennä puitteissa yleensä Vähennä vaiheessa, joskus asetetaan valtava voitot voidaan saavuttaa ajoituksen liitokset kartan vaiheessa (eli "mapjoins"). Ohjaamaan rakenteen toiminto aina, kun se on mahdollista määrittää on:

        set hive.auto.convert.join=true;

4. **Toistokertojen määrän mappers rakenne** : kun Hadoop-toiminnolla käyttäjä voi määrittää, kuinka monta pienennyslaitteita varten, mappers määrä on yleensä ei voi määrittää käyttäjän. Huijata, joka sallii ohjausobjektin tämän numeron jonkin verran on valita Hadoop muuttujat, *mapred.min.split.size* ja *mapred.max.split.size* kartan kunkin tehtävän koon määritetään mukaan:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Yleensä *mapred.min.split.size* oletusarvo on 0, joka *mapred.max.split.size* on **Long.MAX** ja joka *dfs.block.size* on 64 Megatavua. Kuin näemme, annetut tiedot koon säätäminen muuttujat "määrittämällä" ne jotta Microsoft pystyy paranna käytetään mappers määrä.

5. Alla on mainittu joitakin muita enemmän **Lisäasetukset** optimoinnista rakenne. Näiden avulla voit yhdistää ja vähentää tehtävät varattu ja voi olla hyödyllistä tweaking suorituskykyä. Muista, että *mapreduce.reduce.memory.mb* ei voi olla suurempi kuin muistin kunkin työntekijän solmun Hadoop-klusterin.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
