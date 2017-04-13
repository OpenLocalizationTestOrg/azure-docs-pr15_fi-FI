<properties
    pageTitle="HDInsight mukauttamiseen klustereiden R käytössä | Microsoft Azure"
    description="Opi asentamaan R komentosarja-toiminnon avulla ja R käyttäminen HDInsight klustereiden."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Asentaminen ja käyttäminen HDInsight Hadoop klustereiden R

Katso mukauttamisesta Windows perusteella HDInsight-klusterin r-komentosarja-toiminnon avulla ja R käyttämisestä HDInsight klusterit. Ojentamassa Hdinsightista varten [premium taso](https://azure.microsoft.com/pricing/details/hdinsight/) sisältää R palvelimen HDInsight-klusterin osana. Näin R-komentosarjojen käyttää MapReduce ja ohjattu hajautettu funktiolauseita. Lisätietoja on artikkelissa [käyttäminen HDInsight R-palvelimeen](hdinsight-hadoop-r-server-get-started.md). Lisätietoja Linux-pohjaiset klusterin R käyttämisestä on artikkelissa [asentaminen ja käyttäminen R HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
Voit asentaa R kaikentyyppisissä klusterin (Hadoop, myrsky, HBase, ohjattu) Azure Hdinsightiin *Komentosarja-toiminnon*avulla. Esimerkki komentosarjan R asennetaan HDInsight-klusterin on käytettävissä vain luku-Azure tallennustilan Blob-objektien [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)-palvelussa. 

**Aiheeseen liittyviä artikkeleita**

- [Asentaminen ja käyttäminen R HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Luo Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md): yleisiä tietoja HDInsight klustereiden luomisesta
- [Mukauta HDInsight-klusterin komentosarja-toiminnon käyttäminen][hdinsight-cluster-customize]: yleisiä tietoja mukauttamisesta HDInsight klustereiden komentosarja-toiminnon käyttäminen
- [Kehitä komentosarja-toiminnon komentosarjojen Hdinsightiin](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Mikä on R?

<a href="http://www.r-project.org/" target="_blank">R projektin tilastollinen tietojenkäsittely</a> on Avaa lähde kieli- ja tilastollisia tietojenkäsittely ympäristössä. R on satoja valmiiden tilastolliset funktiot ja oma ohjelmointikieli, joka yhdistää toiminnalliset ja Käytä olio-ohjelmoinnin ominaisuuksia. Se sisältää myös monipuolisia graafisia ominaisuuksia. R on ensisijainen monipuolisemman ympäristön eniten professional ilmiöjoukosta ja tutkijoiden-kentissä on useita erilaisia.

R on yhteensopiva Azure Blob Storage (WASB), niin, että siihen tallennettuja tietoja voidaan käsitellä R käyttäminen Hdinsightista.  

## <a name="install-r"></a>Asenna R

[Esimerkki komentosarja](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R asennetaan HDInsight-klusterin on käytettävissä vain luku-blob Azuren tallennustilaan. Tässä osassa on ohjeet käyttämään komentosarjan Azure-portaalissa klusterin luomisen aikana.

> [AZURE.NOTE] Esimerkki komentosarja otettiin HDInsight klusterin versio 3.1 kanssa. Saat lisätietoja HDInsight klusterin versioista [HDInsight klusterin versiot](hdinsight-component-versioning.md).

1. Kun luot HDInsight-klusterin-portaalista, valitse **Valinnainen määritys**ja valitse sitten **Komentosarjatoiminnot**.
2. Anna **Komentosarjatoiminnot** -sivulla seuraavat arvot:

    ![Käytä komentosarja-toiminnon klusterin mukauttamiseen] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Käytä komentosarja-toiminnon klusterin mukauttamiseen")

    <table border='1'>
        <tr><th>Ominaisuus</th><th>Arvo</th></tr>
        <tr><td>Nimi</td>
            <td>Määritä komentosarja-toiminnon, kuten <b>Asentaa R</b>nimi.</td></tr>
        <tr><td>Komentosarjan URI</td>
            <td>Määritä komentosarja, joka käynnistyy, voit mukauttaa klusterin, esimerkiksi <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i> URI</td></tr>
        <tr><td>Solmutyyppi</td>
            <td>Määritä solmujen, jossa mukauttaminen komentosarja suoritetaan. Voit valita <b>Kaikki solmut</b>, <b>vain Head solmujen</b>tai <b>Työntekijä solmujen</b> vain.
        <tr><td>Parametrit</td>
            <td>Määritä parametreja, jos vaatii komentosarja. Asenna R komentosarja kuitenkin edellytä parametreja, jotta voit jättää tämän tyhjäksi.</td></tr>
    </table>

    Voit lisätä useita komentosarja-toiminnon asentamisesta klusterin useita osia. Kun olet lisännyt komentosarjat, napsauttamalla Käynnistä crating klusterin valintamerkkiä.

Voit myös asentaa R HDInsight käyttämällä tai PowerShellin Azure Hdinsightiin .NET SDK komentosarja. Ohjeet näitä ohjeita on tämän artikkelin.

## <a name="run-r-scripts"></a>R komentosarjojen suorittamisen
Tässä osassa kuvataan R-komentosarjan suorittaminen Hadoop-klusterin HDInsight kanssa.

1. **Muodosta Etätyöpöytäyhteys klusteriin**: From-portaalissa etätyöpöydän käyttöön luomasi r asennettu klusterin ja liitä klusterin. Ohjeita on artikkelissa [etäyhteyden muodostaminen HDInsight klustereiden RDP avulla](hdinsight-administer-use-management-portal.md#rdp).

2. **Avaa R-konsolin**: R asennuksen sijoittaa linkin R-konsoliin pään solmun työpöydälle. Valitse Avaa R-konsolin.

3. **Suorita R-komentosarja**: R komentosarja voidaan suorittaa suoraan R-konsolin liittämällä sen, valitsemalla sen ja painamalla ENTER-näppäintä. Tässä on esimerkki komentosarjan, joka luo numerot 1 – 100 ja kertoo 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Kaksi ensimmäistä riviä Soita RHadoop kirjastot, jotka asennetaan R. Viimeisen rivin tulostaa konsoliin tulokset. Tulosteen pitäisi näyttää tältä:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Asenna R Aure PowerShellin avulla

Katso [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Esimerkki havainnollistaa ohjattu Azure PowerShellin asennusohjeet. Haluat mukauttaa komentosarja käyttämään [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Asenna käyttämällä .NET SDK R

Katso [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Esimerkki havainnollistaa käyttämällä .NET SDK ohjattu asentaminen. Haluat mukauttaa komentosarja käyttämään [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## <a name="see-also"></a>Katso myös

- [Asentaminen ja käyttäminen R HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Luo Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md): yleisiä tietoja HDInsight klustereiden luomisesta
- [Mukauta HDInsight-klusterin komentosarja-toiminnon käyttäminen][hdinsight-cluster-customize]: yleisiä tietoja mukauttamisesta HDInsight klustereiden komentosarja-toiminnon käyttäminen
- [Kehitä komentosarja-toiminnon komentosarjojen Hdinsightiin](hdinsight-hadoop-script-actions.md)
- [Asentaminen ja käyttäminen ohjattu HDInsight klustereiden][hdinsight-install-spark]: asentamisesta ohjattu komentosarja-toiminnon malli
- [Asenna Giraph-HDInsight klustereiden](hdinsight-hadoop-giraph-install.md): komentosarja-toiminnon otoksen asentamisesta Giraph
- [Asenna Solr-HDInsight klustereiden](hdinsight-hadoop-solr-install-linux.md): komentosarja-toiminnon otoksen asentamisesta Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
