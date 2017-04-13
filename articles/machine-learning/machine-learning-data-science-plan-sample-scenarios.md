<properties
    pageTitle="Tilanteita, joissa kehittyneen analyysin prosessin ja Azure koneen Learning teknologian | Microsoft Azure"
    description="Valitse haluamasi skenaarioiden harjoittelu advanced ennakoivan analytics ryhmän tietojen tiede prosessin avulla."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Tilanteita, joissa kehittyneen analyysin-Azure koneen oppiminen

Tässä artikkelissa käsitellään erilaisia otoksen tietolähteet ja kohde tilanteita, joissa voit käsitellä [Ryhmän tietojen tiede prosessi (TDSP)](data-science-process-overview.md). TDSP on järjestelmällinen lähestymistapa ryhmät voivat tehdä yhteistyötä älykkäät sovellusten. Seuraavassa esitetään havainnollistetaan valittavissa olevat vaihtoehdot tietojen käsittely-työnkulun, jotka riippuvat tietojen ominaisuuksia, lähteet ja kohde säilöjen tietoihin Azure-tietokannassa.

**Päätöspuun** valitsemiseen otoksen käyttötavoista, joka sopii tiedot ja tavoitteen esitetään viimeisessä osassa.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Seuraavat osioissa näkyy esimerkkitilanne. Kunkin skenaarion tietojen tiede tai kehittyneen analyysin kulun sekä tukemaan Azure resurssit näkyvät.

>[AZURE.NOTE]**Kaikkien seuraavissa tilanteissa, sinun täytyy:**
><br/>
>* [Tallennustilan tilin luominen](../storage/storage-create-storage-account.md)
><br/>
>* [Azure koneen Learning-työtilan luominen](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Skenaario \#1: pieni Normaali taulukko dataset paikallisina tiedostoina

![Pieni Normaali paikallisina tiedostoina][1]

#### <a name="additional-azure-resources-none"></a>Azure lisäresursseja: ei mitään

1.  Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

2.  Lataa tietojoukko.

3.  Luoda Azure koneen Learning kokeen työnkulku ladatut tietojoukkoa alkaen.

## <a name="smalllocalprocess"></a>Skenaario \#2: pieni paikallisen tiedostoja, jotka edellyttävät käsittely Normaali tietojoukko

![Pieni Normaali paikallisen tiedostojen käsittely][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (IPython muistikirjan server)

1.  Azure Virtual Machine käynnissä IPython muistikirjan luominen

2.  Tietojen lataaminen Azure säilytykseen.

3.  Esikäsittely ja Puhdista tietojen IPython muistikirjaan tietojen käyttäminen Azure tallennustilan säilö.

4.  Muuntaa puhdistaa sarkainmuodossa tiedot.

5.  Tallenna Azure BLOB muunnettua havaintoaineistoa.

6.  Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

7.  Tietojen lukeminen Azure BLOB [Tietojen] [ import-data] moduuli.

8. Luoda Azure koneen Learning kokeen työnkulku nieltäväksi tietojoukkoa alkaen.

## <a name="largelocal"></a>Skenaario \#3: suuri tietojoukko paikallisten tiedostojen, kohdistamisen Azure-BLOB-objektit

![Suuri paikallisina tiedostoina][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (IPython muistikirjan server)

1.  Azure Virtual Machine käynnissä IPython muistikirjan luominen

2.  Tietojen lataaminen Azure säilytykseen.

3.  Esikäsittely ja tietojen IPython muistikirjaan tietojen käyttäminen Azure BLOB Puhdista.

4.  Muunna tarvittaessa puhdistaa taulukkomuodon, tiedot.

5.  Tietojen tutkiminen ja luo ominaisuuksia tarpeen mukaan.

6.  Pura pieni Normaali näyte.

7.  Tallenna Azure BLOB näytteeksi tiedot.

8. Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

9. Tietojen lukeminen Azure BLOB [Tietojen] [ import-data] moduuli.

10. Luoda Azure koneen Learning kokeen työnkulku alkaa nieltäväksi tietojoukkoa.


## <a name="smalllocaltodb"></a>Skenaario \#4: pieni paikallisina tiedostoina, SQL Server Azure-Virtal tietokoneen kohdistamisen Normaali tietojoukko

![Pieni SQL Azure-tietokannassa DB Normaali paikallisina tiedostoina][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (SQL Server tai IPython muistikirjan server)

1.  Luo Azure Virtual Machine käynnissä SQL Server + IPython muistikirja.

2.  Tietojen lataaminen Azure säilytykseen.

3.  Esikäsittely ja Azure tallennustilan säiliön IPython muistikirjan käyttäminen tietojen Puhdista.

4.  Muunna tarvittaessa puhdistaa taulukkomuodon, tiedot.

5.  Tietojen tallentaminen AM paikallinen tiedostot (AM IPython muistikirja on käynnissä, paikalliset asemat viitata AM asemat).

6.  Lataa tietoja Azure-AM käytössä SQL Server-tietokantaan.

    Vaihtoehto \#1: SQL Server Management Studiossa.

    - SQL Server AM kirjautuminen
    - Suorita SQL Server Management Studiossa.
    - Luo tietokanta ja kohde taulukot.
    - Käytä jotakin usean tuo menetelmiä lataa tiedot AM paikallinen tiedostoista.

    Vaihtoehto \#2: käyttämällä IPython muistikirjaan ei ole suositeltavaa, Normaali ja suurempia tietojoukkoja<!-- -->    
    - ODBC-yhteysmerkkijonon avulla voit käyttää SQL Server-AM.
    - Luo tietokanta ja kohde taulukot.
    - Käytä jotakin usean tuo menetelmiä lataa tiedot AM paikallinen tiedostoista.

7.  Tutkia tietoja, luoda ominaisuuksia tarpeen mukaan. Huomaa, että ominaisuuksia ei tarvitse sattui tietokannan taulukoiden. Huomautus vain tarvittavat kyselyn luoda niitä.

8. Päättää arvopisteiden otoksen koko, jos tarvitaan ja/tai toivottuja.

9. Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

10. Lue tietoja suoraan [Tietojen tuominen] SQL Server[ import-data] moduuli. Liitä tarvittavat kysely, joka poimii kentät, Luo ominaisuudet ja esimerkit tiedot suoraan- [Tietojen tuominen] tarvittaessa[ import-data] kyselyn.

11. Luoda Azure koneen Learning kokeen työnkulku alkaa nieltäväksi tietojoukkoa.

## <a name="largelocaltodb"></a>Skenaario \#5: paikallisina tiedostoina suuri dataset kohteen SQL Server Azure AM

![SQL Azure-tietokannassa DB suuri paikallisina tiedostoina][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (SQL Server tai IPython muistikirjan server)

1.  Luo Azure Virtual Machine SQL Server-ja IPython muistikirjan palvelimeen.

2.  Tietojen lataaminen Azure säilytykseen.

3.  (Valinnainen) Esikäsittely ja Puhdista tiedot.

    a.  Esikäsittely ja tietojen IPython muistikirjaan tietojen käyttäminen Azure BLOB Puhdista.

    b.  Muunna tarvittaessa puhdistaa taulukkomuodon, tiedot.

    c-näppäinyhdistelmää.  Tietojen tallentaminen AM paikallinen tiedostot (AM IPython muistikirja on käynnissä, paikalliset asemat viitata AM asemat).

4.  Lataa tietoja Azure-AM käytössä SQL Server-tietokantaan.

    a.  Kirjaudu SQL Serverin AM.

    b.  Jos tiedot on tallennettu ei ole jo, lataa tiedostot Azure tallennustilan säilö paikallinen AM kansioon.

    c-näppäinyhdistelmää.  Suorita SQL Server Management Studiossa.

    d.  Luo tietokanta ja kohde taulukot.

    e.  Käytä jotakin usean tuo menetelmiä lataa tiedot.

    f.  Jos taulukon liitokset tarvitaan, luo indeksit liitokset suorittamiseen.

     > [AZURE.NOTE] Suuria tietomääriä nopeammin lataaminen, on suositeltavaa, että osioitua taulukoiden luominen ja joukkona tuoda tiedot rinnakkain. Lisätietoja on artikkelissa [Rinnakkain tietojen tuominen SQL osioitu taulukoihin](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Tutkia tietoja, luoda ominaisuuksia tarpeen mukaan. Huomaa, että ominaisuuksia ei tarvitse sattui tietokannan taulukoiden. Huomautus vain tarvittavat kyselyn luoda niitä.

6.  Päättää arvopisteiden otoksen koko, jos tarvitaan ja/tai toivottuja.

7.  Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

8. Lue tietoja suoraan [Tietojen tuominen] SQL Server[ import-data] moduuli. Liitä tarvittavat kysely, joka poimii kentät, Luo ominaisuudet ja esimerkit tiedot suoraan- [Tietojen tuominen] tarvittaessa[ import-data] kyselyn.

9. Yksinkertainen Azure koneen Learning kokeen työnkulku alkaen ladatut tietojoukko

## <a name="largedbtodb"></a>Skenaario \#6: suuri SQL Serverin tietokantaan paikallinen, kohdistamisen SQL Server Azure Virtual Machine-tietojoukko

![Suuri SQL DB paikallinen SQL Azure-tietokannassa DB][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (SQL Server tai IPython muistikirjan server)

1.  Luo Azure Virtual Machine SQL Server-ja IPython muistikirjan palvelimeen.

2.  Käytä jotakin tietoja Vie menetelmät tietojen vieminen vedostiedostot SQL Server.

    > [AZURE.NOTE] Jos haluat siirtää kaikki tiedot paikallinen tietokanta, koko tietokanta Siirry Azure SQL Server-esiintymän vaihtoehtoisella (nopeampi) tavalla. Ohita vaiheet viedä tietoja, luoda tietokannan, ja kuormituksen/tuo tiedot kohdetietokannan ja vaihtoehtoista menetelmää.

3.  Lataa vedostiedostot Azure tallennustilan säilö.

4.  Lataa tiedot käynnissä Azure Virtual Machine SQL Server-tietokantaan.

    a.  Kirjaudu SQL Serverin AM.

    b.  Lataa tiedostot Azure säilytykseen paikallinen AM-kansioon.

    c-näppäinyhdistelmää.  Suorita SQL Server Management Studiossa.

    d.  Luo tietokanta ja kohde taulukot.

    e.  Käytä jotakin usean tuo menetelmiä lataa tiedot.

    f.  Jos taulukon liitokset tarvitaan, luo indeksit liitokset suorittamiseen.

    > [AZURE.NOTE] Suuria tietomääriä nopeammin lataaminen osioitua taulukoiden luominen ja samalla kertaa, tuoda tiedot rinnakkain. Lisätietoja on artikkelissa [Rinnakkain tietojen tuominen SQL osioitu taulukoihin](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Tutkia tietoja, luoda ominaisuuksia tarpeen mukaan. Huomaa, että ominaisuuksia ei tarvitse sattui tietokannan taulukoiden. Huomautus vain tarvittavat kyselyn luoda niitä.

6.  Päättää arvopisteiden otoksen koko, jos tarvitaan ja/tai toivottuja.

7.  Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

8. Lue tietoja suoraan [Tietojen tuominen] SQL Server[ import-data] moduuli. Liitä tarvittavat kysely, joka poimii kentät, Luo ominaisuudet ja esimerkit tiedot suoraan- [Tietojen tuominen] tarvittaessa[ import-data] kyselyn.

9. Yksinkertainen Azure koneen Learning kokeilla työnkulku ladatut tietojoukko alkaen.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Vaihtoehtoinen menetelmä voit kopioida koko tietokannan paikallisen SQL Serveristä Azure SQL-tietokanta

![Irrota paikallisen DB ja liitä SQL Azure-tietokannassa DB][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure lisäresursseja: Azure virtuaalikoneen (SQL Server tai IPython muistikirjan server)

Replikointia SQL Server-AM koko SQL Server-tietokantaan, sinun on kopioitava tietokannan palvelimesta sijaintiin tai toiseen, olettaen, että tietokanta ottaa tilapäisesti offline-tilassa. Voit tehdä saman SQL Server Management Studiossa Object Explorerissa tai käyttämällä vastaavat Transact-SQL-komentoja.

1. Irrota tietokannan lähde-kohtaa. Lisätietoja on artikkelissa [tietokannan irrota](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. Kopioi Resurssienhallinnan tai Windows-komentokehote-ikkunassa irrotettu tietokantatiedoston tai ja lokitiedoston tiedostoista tai Azure SQL Server-AM kohde haluamaasi kohtaan.
3. Liittää kopioidut tiedostot kohde SQL Server-esiintymän. Lisätietoja on artikkelissa [liittäminen tietokannan](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Siirtää tietokannan irrottaa ja liittää (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Skenaario \#7: paikallisen tiedostojen Big datasta kohde Azure Hdinsightiin Hadoop klustereissa tietokannan rakenne

![Big datasta paikallisen kohde rakenne][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Azure lisäresursseja: Azure Hdinsightiin Hadoop-klusterin ja Azure virtuaalikoneen (IPython muistikirjan server)

1.  Luo Azure Virtual Machine suorittaa IPython muistikirjan Serveriä.

2.  Azure Hdinsightiin Hadoop-klusterin luominen

3.  (Valinnainen) Esikäsittely ja Puhdista tiedot.

    a.  Esikäsittely ja tietojen IPython muistikirjaan tietojen käyttäminen Azure BLOB Puhdista.

    b.  Muunna tarvittaessa puhdistaa taulukkomuodon, tiedot.

    c-näppäinyhdistelmää.  Tietojen tallentaminen AM paikallinen tiedostoja (IPython muistikirjan suoritetaan AM, paikalliset asemat viitata AM asemat).

4.  Tietojen lataaminen oletusarvon säilön Hadoop-klusterin vaiheessa 2.

5.  Lataa tietoja Azure Hdinsightiin Hadoop-klusterin rakenne-tietokantaan.

    a.  Hadoop-klusterin pään solmu kirjautuminen

    b.  Avaa Hadoop-komentoriviltä.

    c-näppäinyhdistelmää.  Kirjoita rakenne-pääkansio komennolla `cd %hive_home%\bin` Hadoop komentoriviltä.

    d.  Suorita rakenteen Luo tietokanta ja taulukot, kyselyt ja lataa tiedot Blob-objektien tallennustilaan rakenne-taulukoihin.

    > [AZURE.NOTE] Jos tiedot on suuri, käyttäjät voivat luoda taulukko rakenne osiot. Valitse, käyttäjät voivat käyttää `for` silmukka-Hadoop komentorivin pään solmun lataa tiedot osion osioinut rakenne-taulukkoon.

6.  Tutkia tietoja ja luoda ominaisuuksia tarvittaessa Hadoop komentoriviltä. Huomaa, että ominaisuuksia ei tarvitse sattui tietokannan taulukoiden. Huomautus vain tarvittavat kyselyn luoda niitä.

    a.  Hadoop-klusterin pään solmu kirjautuminen

    b.  Avaa Hadoop-komentoriviltä.

    c-näppäinyhdistelmää.  Kirjoita rakenne-pääkansio komennolla `cd %hive_home%\bin` Hadoop komentoriviltä.

    d.  Tee rakenne kyselyt Hadoop komentoriviltä pään solmun Hadoop-klusterin tutkia tietoja ja luoda ominaisuuksia tarpeen mukaan.

7.  Jos tarvitaan ja/tai toivottuja, Sovita Azure koneen Learning Studiossa mallitiedot.

8.  Kirjautuminen [Azure koneen Koulujen Studio](https://studio.azureml.net/).

9. Lue tietoja suoraan `Hive Queries` [Tietojen] [ import-data] moduuli. Liitä tarvittavat kysely, joka poimii kentät, Luo ominaisuudet ja esimerkit tiedot suoraan- [Tietojen tuominen] tarvittaessa[ import-data] kyselyn.

10. Yksinkertainen Azure koneen Learning kokeilla työnkulku ladatut tietojoukko alkaen.

## <a name="decisiontree"></a>Päätöspuun skenaarion valintaa varten
------------------------

Seuraavassa kaaviossa on yhteenveto edellä kuvatun skenaariot ja Advanced Analytics-prosessi ja tekniikka vaihtoehtoja, jotka vievät eri skenaarioissa eritelty tehdyt. Huomaa, että tietojen käsittely, tarkasteluun, toiminto-tekniikka ja esimerkkejä saattaa kestää Siirrä vähintään yksi tapa/ympäristön--lähteessä, keskitaso, ja/tai kohde ympäristöissä – ja voivat jatkaa muutosten tarpeen mukaan. Kaavio on kuva joistain mahdollista kulkee on vain ja monipuolisen luettelointi ei sisällä.

![Esimerkki DS prosessin ongelmatilanteita skenaariot][8]

### <a name="advanced-analytics-in-action-examples"></a>Laajennettu Analytics-toiminto esimerkkejä

Lopusta loppuun Azure koneen Learning vaiheittaiset ohjeet, joissa käytetään Advanced Analytics-prosessi ja tekniikka käyttämällä julkisissa tietojoukoissa seuraavissa artikkeleissa:


* [Ryhmän tietojen tiede prosessin käytössä: SQL Serverin avulla](machine-learning-data-science-process-sql-walkthrough.md).
* [Ryhmän tietojen tiede prosessin käytössä: käyttämällä HDInsight Hadoop klustereiden](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
