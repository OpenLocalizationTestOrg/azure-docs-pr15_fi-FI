<properties
    pageTitle="Asennus R Linux-pohjaiset HDInsight | Microsoft Azure"
    description="Opettele asentaminen ja R avulla voit mukauttaa Linux-pohjaiset Hadoop klustereiden."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Asentaminen ja käyttäminen HDInsight Hadoop klustereiden R

Voit asentaa R kaikentyyppisissä klusterin Hadoop HDInsight-klusterin mukauttaminen **Komentosarja-toiminnon** avulla. Näin tietojen tutkijoiden ja analyytikot käyttöön R tehokkaita MapReduce/kuitenkaan ohjelmoinnin framework käsitellä suuria tietomääriä, Hadoop-klustereiden, jotka on otettu Hdinsightista.

> [AZURE.IMPORTANT] Ojentamassa Hdinsightista varten [premium taso](https://azure.microsoft.com/pricing/details/hdinsight/) sisältää R palvelimen HDInsight-klusterin osana. Näin R-komentosarjojen käyttää MapReduce ja ohjattu hajautettu funktiolauseita. Lisätietoja on artikkelissa [käyttäminen HDInsight R-palvelimeen](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Mikä on R?

<a href="http://www.r-project.org/" target="_blank">R projektin tilastollinen tietojenkäsittely</a> on Avaa lähde kieli- ja tilastollisia tietojenkäsittely ympäristössä. R on satoja valmiiden tilastolliset funktiot ja oma ohjelmointikieli, joka yhdistää toiminnalliset ja Käytä olio-ohjelmoinnin ominaisuuksia. Se sisältää myös monipuolisia graafisia ominaisuuksia. R on ensisijainen monipuolisemman ympäristön eniten professional ilmiöjoukosta ja tutkijoiden-kentissä on useita erilaisia.

R komentosarjojen suorittamisen HDInsight Hadoop varausyksiköt, joka on mukautettu komentosarja-toiminnon luomisen avulla voit asentaa R-ympäristössä. R on yhteensopiva Azure Blob Storage (WASB), niin, että siihen tallennettuja tietoja voidaan käsitellä R käyttäminen Hdinsightista.

## <a name="what-the-script-does"></a>Komentosarjan mitä

Komentosarja-toiminnon avulla R asentaminen HDInsight-klusterin asentaa seuraavat Ubuntu paketteja, jotka tarjoavat perus R-asennus:

* [r base](http://packages.ubuntu.com/precise/r-base): perus GNU R-paketti
* [r-base-keskihajonta](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R-paketit

Myös seuraavat RHadoop paketit asennetaan, joka antaa integrointi MapReduce ja HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): R kehittäjät voivat käyttää Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): R kehittäjät voivat käyttää Hadoop HDFS (WASB HDInsight varten)

Lisäksi seuraavat R-paketit on asennettu:

| R-paketti | Sen avulla |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Heikko R Java liittymään. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R ja C++ integrointi. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | JSON R onnistu tai poistaa objekteja |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Kokonaisluku vektorit bittitason toimenpiteet-funktiot. |
| [käsittely](https://cran.r-project.org/web/packages/digest/index.html) | Luo salauksen Hash näyteliuos R objekteja. |
| [toimi](https://cran.r-project.org/web/packages/functional/index.html) | Curry, käytä ja muut-järjestys-Funktiot |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Joustavasti uudelleenjärjestely ja koota tietoja. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Yksinkertainen ja yhtenäinen kääreisiin yleisiä Merkkijonotoiminnoista. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Jakaminen ja käyttäminen tietojen yhdistämisestä työkaluja. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Siirtymisen tilastotiedot, GIF, Base64, OHJETIEDOSTO AUC ikkunassa Työkalut jne. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Lähentää merkkijonon vastaavat ja merkkijono etäisyys funktiot. |

## <a name="install-r-using-script-actions"></a>Asenna R komentosarja-toimintojen käyttäminen

Komentosarja-toiminnon avulla R asentaminen HDInsight-klusterin. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.SH
    
Tässä osassa on ohjeet käyttämään komentosarja, kun luot uuden klusterin Azure-portaalissa. 

> [AZURE.NOTE] Azure PowerShell, Azure-CLI, HDInsight .NET SDK tai Azure Resurssienhallinta malleja myös voidaan lisätä komentosarjatoiminnot. Voit myös käyttää komentosarjatoiminnot klustereiden jo käytössä. Lisätietoja on artikkelissa [mukauttaminen HDInsight klustereiden komentosarjan toimintojen](hdinsight-hadoop-customize-cluster-linux.md).

1. Käynnistä valmistelu klusteri [valmisteleminen Linux-pohjaiset HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal)klustereissa ohjeiden mukaisesti, mutta älä tee valmistelu.

2. Valitse **Vaihtoehtoinen määritys** -sivu **Komentosarjatoiminnot**ja anna seuraavat tiedot:

    * __Nimi__: Kirjoita kutsumanimi komentosarja-toiminnon.
    * __KOMENTOSARJAN URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __HEAD__: Valitse tämä vaihtoehto
    * __Työntekijän__: Valitse tämä vaihtoehto
    * __ZOOKEEPER__: Valitse tämä vaihtoehto, jos asennetaan Zookeeper-solmu.
    * __Parametrit__: Jätä tämä kenttä tyhjäksi

3. **Komentosarjatoiminnot**alareunassa Tallenna kokoonpano **valinta** -painikkeen avulla. Lopuksi painikkeella **Valitse** **Vaihtoehtoinen määritys** -sivu alareunassa Tallenna valinnainen kokoonpanotietoja.

4. Jatka valmistelu klusterin kuvatulla tavalla [säännöstä Linux-pohjaiset HDInsight klustereiden](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>R komentosarjojen suorittamisen

Kun klusterin on päättynyt valmistelu, käytä R MapReduce-toiminnon klusterin seuraavien vaiheiden avulla.

1. Yhdistä käyttäen SSH HDInsight-klusterin:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Lisätietoja HDInsight SSH käyttämisestä on seuraavissa artikkeleissa:

    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Valitse `username@hn0-CLUSTERNAME:~$` Kysy, kirjoita seuraava komento vuorovaikutteinen R-istunnon aloittaminen:

        R

3. Kirjoita seuraava R-ohjelma. Tämä luo numerot 1 – 100 ja kertoo 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Ensimmäisen rivin puhelujen RHadoop kirjaston rmr2, jota käytetään MapReduce toimintoja.

    Toisen rivin arvot 1 – 100 ja luo sitten tallentaa ne Hadoop tiedoston järjestelmän käyttäen `to.dfs`.

    Kolmas rivi luo MapReduce prosessin avulla rmr2 toimintoja ja käsittely alkaa. Raportissa pitäisi näkyä vierittää aiempia käsittely alkaa, kun useita rivejä.

4. Voit tarkastella tilapäinen polkua, joka MapReduce tulos on tallennettu, käyttämällä seuraavaa:

        print(calc())

    Tämä on oltava kaltaisen `/tmp/file5f615d870ad2`. Todellinen tulosteen katselemista toimimalla seuraavasti:

        print(from.dfs(calc))

    Tulosteen pitäisi näyttää tältä:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Lopeta R Kirjoita seuraavat tiedot:

        q()


## <a name="next-steps"></a>Seuraavat vaiheet

- [Asentaminen ja käyttäminen sävyä HDInsight-klusterit](hdinsight-hadoop-hue-linux.md). Värisävy on sivuston Käyttöliittymä, joka on helppo luoda, suorittaa ja Tallenna Possu ja rakenteen työt sekä oman HDInsight Selaa oletusarvo-tallennustilan klusterin.

- [Asenna Giraph-HDInsight klustereiden](hdinsight-hadoop-giraph-install.md). Klusterin mukauttaminen avulla voit asentaa Giraph HDInsight Hadoop klustereiden. Giraph voit suorittaa graph käsittely käyttämällä Hadoop ja sitä voidaan käyttää Azure Hdinsightista.

- [Asenna Solr-HDInsight klustereiden](hdinsight-hadoop-solr-install.md). Klusterin mukauttaminen avulla voit asentaa Solr HDInsight Hadoop klustereiden. Solr avulla voit suorittaa toimintoja tehokas haku tallennettuja tietoja.

- [Asenna värisävy-HDInsight klustereiden](hdinsight-hadoop-hue-linux.md). Klusterin mukauttaminen avulla voit asentaa sävyä HDInsight Hadoop klustereiden. Värisävy on käytettävä Hadoop-klusterin käsitellä Web-sovellusten.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
