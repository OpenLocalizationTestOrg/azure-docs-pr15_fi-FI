<properties
    pageTitle="Skenaariot tunnistamisesta ja suunnitteleminen advanced analytics-tietojenkäsittely | Microsoft Azure"
    description="Suunnittele kehittyneen analyysin sivustokokoelmatyyppien numerosarjan tuoteavaimeen liittyviä kysymyksiä."
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


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Tunnistaminen skenaariot ja kehittyneen analyysin tietojen käsittely suunnitteleminen

Mitä resursseja, olisi aiot järjestysnumeroiden määrittäminen ympäristössä niin, että Älä kehittyneen analyysin käsittelystä tietojoukko Tässä artikkelissa ehdottaa kysyä kysymyksiä, jotka auttavat tunnistaa tehtäviä ja resursseja asiaa käyttämässäsi skenaariossa. Ylätason vaiheiden järjestyksen ennakoivan analysoinnissa on ääriviivat [ryhmän tietojen tiede prosessi (TDSP) ominaisuudet?](data-science-process-overview.md). Kaikki nämä vaiheet edellyttävät resurssit tietyn skenaarion liittyvät tehtävät. Tärkeimmät kysymykset tunnistavan käyttämässäsi skenaariossa koskevat tiedot Logistiikka-ominaisuuksia, laatua tietojoukkoja-työkalut ja haluat tehdä analyysin kielet.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistista kysymyksiä: tietojen sijainnit ja liikkuvuus
Logistista kysymyksiä koskevat **tietolähteen**, **kohde kohde** Azure-tietokannassa sijainnin ja tietojen, kuten aikataulu, summa ja resurssien siirtämistä koskevat vaatimukset. Tiedot on ehkä voi siirtää useita kertoja tunnin aikana. Käytetty vaihtoehto on paikallisten tietojen siirtäminen joitakin lomakkeen tallennustilaa Azure ja valitsemalla sitten tietokoneen Learning Studio kyselyjä.

1. **Tietolähteen kuvaus** On paikallisia tai Pilvipalvelun? Esimerkki:
    - Tiedot on julkisesti saatavilla kohdassa HTTP-osoite.
    - Tiedot sijaitsevat paikallisen tai verkkoon.
    - Tiedot on SQL Server-tietokantaan.
    - Tiedot tallennetaan Azure säilytykseen

2. **Mikä on Azure kohde?** Jos se on oltava käsittelyn tai mallinnus? Esimerkki:
    - Azure-Blob-säiliö
    - SQL Azure-tietokannat
    - SQL Server Azure AM
    - HDInsight (Hadoop-Azure) tai rakenne-taulukot
    - Azure koneen oppiminen
    - Käytettäväksi Azure levyjä.

3. **Miten Oletko siirtää tietoja?** Seuraavissa ohjeaiheissa kuvattuja ohjeita ja resursseista ingest tai ladata eri tallennustilan ja käsittelyn ympäristöissä monenlaisia tietoja.

    -  [Lataa tietojen analysoinnissa tallennustilan ympäristöissä](machine-learning-data-science-ingest-data.md)
    -  [Koulutus tietojen tuominen Azure koneen Learning Studio eri tietolähteistä](machine-learning-data-science-import-data.md).

4. **Täytyykö tiedot voidaan siirtää säännöllisin väliajoin tai joita olet muokannut siirron aikana?** Kannattaa käyttää Azure tietojen Factory (SYÖTTÖ), kun tiedot on siirrettävä jatkuvasti, etenkin jos hybrid tilanne, joka käyttää sekä paikalliset ja cloud resurssien liittyy tai kun tiedot on tapahtumallinen tai tarvitsee voi muokata tai lisätä siihen aikana jätettävien liiketoimintalogiikan. Lisätietoja-kohdassa [paikallinen SQL server Azure Data Factory ja SQL Azure-tietojen siirtäminen](machine-learning-data-science-move-sql-azure-adf.md)

5. **Kuinka paljon tietoja on siirrettävä Azure?** Tietojoukkoja, joka on erittäin suuri saa tiettyjä ympäristöjen tallennustilaa. Esimerkki on lisätietoja kokorajoitukset koneen Learning Studio seuraavan osion. Tässä tapauksessa otoksen tietoja voidaan käyttää määrityksen aikana. Lisätietoja siitä, miten alas otoksen Azure eri ympäristöissä tietojoukko on artikkelissa [ryhmän tietojen tiede prosessin mallitiedot](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Tietojen ominaisuudet kysymyksiä: tyyppi, muoto ja koko
Näihin kysymyksiin osat ovat avainasemassa suunnittelu tallennustilaa ja käsittelyn Ympäristöt, joista jokaisella on eri tietotyyppeihin on järkevämpää ja joista jokaisella on tietyt rajoitukset.

1. **Mitkä ovat tietotyyppien?** Esimerkki:
    - Numeerinen
    - Categorical
    - Merkkijonojen
    - Binaarinen

2. **Tietojen muotoilun?** Esimerkki:
    - CSV-tiedosto (CSV) tai sarkaineroteltuun (TSV) tasainen tiedostoja
    - Pakattu tai puretaan
    - Azure-BLOB-objektit
    - Hadoop-rakenne-taulukot
    - SQL Server-taulukot

2. **Kuinka suuri on tietoja?**
    - Pieni: pienempi kuin 2 gt
    - Normaali: On yli 2 gt ja pienempi kuin 10 Gigatavua
    - Suuri: Suurempi kuin 10 Gigatavua

Tee Azure koneen Learning Studiossa esimerkiksi:

- Tietomuotojen ja Azure koneen Learning Studiossa, katso [muotoilut ja tietojen tietotyyppien tuettu](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) osa tukemien luettelo.
- Lisätietoja Azure koneen Learning Studio rajoitukset on artikkelissa **kuinka suuri tietojoukon voidaan Omat moduulit?** [tuominen ja vieminen koneen Learning tiedot](machine-learning-faq.md#machine-learning-studio-questions) -osa

Lisätietoja Azure muiden analytics prosessissa käytetyn rajoituksista on artikkelissa [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Tietojen laadun kysymyksiä: tarkasteluun ja vanhat käsittely

1. **Mitä tiedät tietoja?** Tutkia tietoja, kun haluat käyttää ymmärtäminen sen perusominaisuudet. Mitä analysointia ja trendien tunnistamista se näytteillepanoa, mikä harha on on tai ei näy, kuinka monta arvoa. Tämä vaihe on tärkeää ennen käsittely koskevan osallistuminen voi ehdottaa sopivimmat ominaisuudet tai kirjoita analyysin, sekä laatia suunnitelmien muita tietojen keräämistä varten tarvittavat laajuus määrittämiseksi. Tunnusluvut laskeminen ja piirtäminen visualisoinnit on hyödyllisiä tietoja tarkastukseen tekniikoita. Lisätietoja siitä, miten voit tutkia tietojoukko Azure eri ympäristöissä on artikkelissa [tietojen ryhmän tietojen tiede-prosessin](machine-learning-data-science-explore-data.md).

2. **Tarvitseeko tiedot esikäsittely tai puhdistus?**
Esikäsittely ja puhdistus tiedot ovat tärkeitä tehtäviä, jotka yleensä on suoritettava ennen tietojoukon avulla voidaan tehokkaasti koneen learning. Tietoja on usein häiriöistä ja epäluotettavista ja voi puuttua arvot. Näitä tietoja käyttämällä mallinnus voidaan tuottaa harhaanjohtavia tulokset. Kuvaus-kohdassa [tehtävät parannettu tietokoneen tietojen valmisteleminen Koulujen](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Työkalut ja kielet kysymyksiä
On useita vaihtoehtoja sen mukaan, mitä kieliä ja kehitys ympäristöissä tai Työkalut tarvitset tai on eniten conformable avulla.

1. **Mitä kieliä, jota haluat käyttää analysointia varten**  
    - R
    - Python
    - SQL

2. **Mitä Työkalut tietojen analysointiin kannattaa käyttää?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - Komentosarjaa kieli, jota käytetään komentosarjan kielen Azure resurssien hallinta.
    - [Azure koneen Learning Studio](machine-learning-what-is-ml-studio/)
    - [Vallankumous Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter muistikirjat](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Määritä kehittyneen analyysin-skenaario
Kun olet vastannut edellisessä osassa kysymyksiin, olet valmis, voit selvittää, mitä skenaarion parhaiten sopivan tapaus. Esimerkki skenaariot kuvattuja [kehittyneen analyysin-Azure koneen Learning tilanteita, joissa](machine-learning-data-science-plan-sample-scenarios.md).
