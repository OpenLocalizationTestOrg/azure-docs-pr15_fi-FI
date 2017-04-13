<properties
    pageTitle="Käyttää sävyä Hadoop HDInsight Linux klustereiden | Microsoft Azure"
    description="Opettele ja käyttämisestä Hadoop klustereiden HDInsight Linux sävy."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Asentaminen ja käyttäminen sävyä HDInsight Hadoop klustereiden

Opettele sävyä asentaminen HDInsight Linux klustereiden ja reitittää pyynnöt sävyä tunneling avulla.

## <a name="what-is-hue"></a>Mikä on sävyä?

Värisävy on käytettävä Hadoop-klusterin käsitellä Web-sovellusten. Voit käyttää sävyä Selaa tallennustilan liittyvä Hadoop-klusterin (kyseessä HDInsight klustereiden WASB), suorita rakenteen työt ja Possu komentosarjojen jne. Seuraavat osat ovat käytettävissä värisävy-asennusten HDInsight Hadoop-klusterin.

* Mehiläisvaha rakenne-editori
* Possu
* Metastore hallinta
* Oozie
* FileBrowser (joka puheen WASB oletusarvon säilöön)
* Työn selaimessa

> [AZURE.WARNING] Osien HDInsight-klusterin mukana tuetaan täysin ja Microsoft Support auttavat eristämään ja ratkaista ongelmat, jotka liittyvät komponentit.
>
> Mukautettujen osien saa järkevän tukea helpottavat edelleen ongelman vianmäärityksen. Tämä saattaa aiheuttaa ratkaisemiseksi tai sinulta kysytään, haluatko osallistuminen käytettävissä olevat kanavat Avaa lähde-tekniikoiden laaja osaamisalueet, tekniikkaa löytyi. Esimerkiksi ovat yhteisön sivustoja, joita voidaan käyttää, kuten: [HDInsight MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Myös Apache projektien on projektisivustojen [http://apache.org](http://apache.org), esimerkiksi: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Asenna sävyä komentosarja-toimintojen käyttäminen

Komentosarja-toiminnon avulla voidaan asentaa sävyä Linux-pohjaiset HDInsight-klusterin.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-HUE-uber-v02.SH
    
Tässä osassa on ohjeet Azure-portaalissa klusteri valmisteleminen komentosarjan avulla. 

> [AZURE.NOTE] Azure PowerShell, Azure-CLI, HDInsight .NET SDK tai Azure Resurssienhallinta malleja myös voidaan lisätä komentosarjatoiminnot. Voit myös käyttää komentosarjatoiminnot klustereiden jo käytössä. Lisätietoja on artikkelissa [mukauttaminen HDInsight klustereiden komentosarjan toimintojen](hdinsight-hadoop-customize-cluster-linux.md).

1. Käynnistä valmistelu klusteri [valmisteleminen HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal)klustereissa Linux ohjeiden mukaisesti, mutta älä tee valmistelu.

    > [AZURE.NOTE] Asentamiseksi sävyä HDInsight klustereiden suositellut headnode koko on vähintään A4 (8 sydämiä, 14 Gigatavun muistia).

2. Valitse **Vaihtoehtoinen määritys** -sivu **Komentosarjatoiminnot**ja anna tiedot alla kuvatulla tavalla:

    ![Anna komentosarjan toiminnon parametrit värisävy] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Anna komentosarjan toiminnon parametrit värisävy")

    * __Nimi__: Kirjoita kutsumanimi komentosarja-toiminnon.
    * __KOMENTOSARJAN URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __HEAD__: Valitse tämä vaihtoehto
    * __Työntekijän__: Jätä tämä kenttä tyhjäksi.
    * __ZOOKEEPER__: Jätä tämä kenttä tyhjäksi.
    * __Parametrit__: Jätä tämä kenttä tyhjäksi.

3. **Komentosarjatoiminnot**alareunassa Tallenna kokoonpano **valinta** -painikkeen avulla. Lopuksi painikkeella **Valitse** **Vaihtoehtoinen määritys** -sivu alareunassa Tallenna valinnainen kokoonpanotietoja.

4. Jatka valmistelu klusterin kuvatulla tavalla [Linux klustereiden säännöstä Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>HDInsight klustereiden sävyä käyttäminen

SSH Tunneling on ainoa tapa käyttää sävy klusteriin käynnissä ollessaan. Siirry suoraan kohtaan klusterin headnode, jossa värisävy on käynnissä liikenne Tunneling kautta SSH avulla. Kun klusterin on päättynyt valmistelu, sävyä käyttäminen HDInsight Linux-klusterin seuraavien vaiheiden avulla.

1. Tietojen [Käyttäminen SSH Tunneling käyttämään Ambari web-Käyttöliittymä, Resurssienhallinta, JobHistory, NameNode, Oozie, ja muut web-Käyttöliittymä on](hdinsight-linux-ambari-ssh-tunnel.md) luoda käyttämällä SSH tunnelin poistamisesta järjestelmästä asiakasohjelman HDInsight-klusterin ja määritä sitten selain käyttää SSH tunnelin välityspalvelinta.

2. Kun olet luonut SSH tunnelin ja määrittänyt sen liikenne välityspalvelimen selaimesi, löydät ensisijainen pään solmu isäntänimi. Voit tehdä tämän yhdistämällä SSH käyttäminen portin 22 klusterin. Esimerkiksi `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` jossa __käyttäjänimi__ on SSH käyttäjänimi ja __CLUSTERNAME__ on yhteyttä klusterin nimen.

    Lisätietoja SSH käyttämisestä on artikkelissa seuraavat asiakirjat:

    * [SSH käyttäminen Linux-pohjaiset HDInsight Linux-, Unix- tai Mac OS X-asiakasohjelma](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Windows-asiakasohjelmassa Linux-pohjaiset HDInsight SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Kun yhteys on muodostettu, käytä seuraavaa komentoa hankkiminen ensisijainen headnode täydellinen toimialuenimi:

        hostname -f

    Tämä palauttaa nimi seuraavankaltaiselta:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Tämä on ensisijainen headnode hostname, jossa värisävy-sivuston sijaitsee.

2. Avaa värisävy-portaalissa osoitteessa http://HOSTNAME:8888 selaimen avulla. Vaihda HOSTNAME hankit edellisessä vaiheessa nimi.

    > [AZURE.NOTE] Kun kirjaudut ensimmäisen kerran, voit pyydetään kirjautua värisävy-portaalin tilin luominen. Tässä tunnistetiedot rajataan portaalin ja järjestelmänvalvojan tai määrittämäsi aikana säännöstä klusterin SSH käyttäjän tunnistetietoja ei liity.

    ![Kirjaudu sisään värisävy-portaaliin] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Määritä tunnistetiedot värisävy-portaalissa")

### <a name="run-a-hive-query"></a>Rakenteen kyselyn suorittaminen

1. Värisävy-portaalista **Kyselyn Editors**ja valitse sitten rakenne-editorin avaaminen **rakenne** .

    ![Käytä rakenne] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Käytä rakenne")

2. **Auttaminen** -välilehden **tietokannan**pitäisi näkyä **hivesampletable**. Tämä on esimerkkitaulukko kaikki olevien HDInsight Hadoop varausyksiköiden mukana toimitettu. Kirjoita otoksen kyselyn oikeanpuoleisen ruudun ja tarkastella tulos **tulokset** -välilehden-ruudussa näyttökuvan esitetyllä tavalla.

    ![Kyselyn suorittaminen rakenne] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Kyselyn suorittaminen rakenne")

    Voit myös nähdä visuaalisen esityksen tuloksen **kaavio** -välilehti.

### <a name="browse-the-cluster-storage"></a>Selaa klusterin tallennustilaan

1. Valitse **Tiedosto selaimessa** värisävy-portaalista valikkorivin oikeassa yläkulmassa.

2. Oletusarvon mukaan tiedoston selain avaa **/user/myuser** -kansio. Valitse oikea käyttäjän kansion polku, siirry Azure tallennustilan säilö klusterin liittyvät ylimmällä ennen vinoviiva.

    ![Käytä tiedosto selaimessa] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Käytä tiedosto selaimessa")

3. Tiedoston tai kansion käytettävissä olevat toiminnot saat napsauttamalla hiiren kakkospainikkeella. Tiedostojen lataaminen nykyisen hakemiston oikeassa alakulmassa olevaa **Lataa** -painikkeen avulla. **Uusi** -painikkeella voit luoda uusia tiedostoja tai kansioita.

> [AZURE.NOTE] Sävy tiedosto selaimessa vain näyttää liittyvän HDInsight-klusterin oletusarvon säilön sisältöä. Lisää tallennustilaa tilit/säilöjen, jotka saattavat kytketty klusterin ei voi käyttää tiedostoa selaimessa. Kuitenkin muita säilöjen klusterin liittyvät aina voi käyttää for rakenteen työt. Jos kirjoitat komennon esimerkiksi `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` rakenne-editorissa, näet myös muita säilöjen sisällön. Tämä komento **newcontainer** ei ole klusterin liittyvät oletusarvon säilö.

## <a name="important-considerations"></a>Tärkeitä

1. Asennuksessa sävyä komentosarja asentaa vain-klusterin ensisijainen headnode.

2. Asennuksen aikana Hadoop palveluihin (HDFS, kuitenkaan, MR2, Oozie) uudelleen päivittämistä määritystä varten. Kun komentosarja on päättynyt sävyä, se voi kestää jonkin aikaa muiden Hadoop Services alkavan. Tämä voi vaikuttaa suorituskykyyn värisävy on alun perin. Kun kaikki palvelut käynnistäminen, sävy on täysin.

3.  Sävy ymmärrä Tez töitä, eli nykyisen oletusarvo rakenne. Jos haluat käyttää MapReduce rakenteen suorittamisen-ohjelma, päivittää, käytä seuraavaa komentoa komentosarja script:

        set hive.execution.engine=mr;

4.  Klustereiden Linux sijoittamisen tilanne, jossa palveluita käyttävät ensisijainen headnode, kun Resurssienhallinta voidaan käyttää toissijaisen. Tällainen tilanne voi aiheuttaa virheitä (kuten alla) käytettäessä sävyä haluat tarkastella käynnissä töiden klusterin. Voit tarkastella projektin tiedot, kun työ on valmis.

    ![Portaalin värisävy-virhe] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Portaalin värisävy-virhe")

    Tämä on tunnettu ongelma. Voit kiertää tämän ongelman muokata Ambari niin, että aktiivinen Resurssienhallinta toimii myös ensisijainen headnode.

5.  Sävy ymmärtää WebHDFS, kun HDInsight klustereiden käyttää Azuren tallennustilaan käyttäminen `wasbs://`. Mukautettu komentosarja komentosarja-toiminnon käyttämisestä asentaa, WebWasb, joka yhteenvedon WASB WebHDFS-yhteensopiva palvelu on. Vaikka värisävy-portaalin lukee HDFS (esimerkiksi kun siirrät hiiren osoitin **Tiedosto selaimessa**) paikassa, se tulee tulkita WASB.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Asenna Giraph-HDInsight klustereiden](hdinsight-hadoop-giraph-install-linux.md). Klusterin mukauttaminen avulla voit asentaa Giraph HDInsight Hadoop klustereiden. Giraph voit suorittaa graph käsittely käyttämällä Hadoop ja sitä voidaan käyttää Azure Hdinsightista.

- [Asenna Solr-HDInsight klustereiden](hdinsight-hadoop-solr-install-linux.md). Klusterin mukauttaminen avulla voit asentaa Solr HDInsight Hadoop klustereiden. Solr avulla voit suorittaa toimintoja tehokas haku tallennettuja tietoja.

- [Asenna R-HDInsight klustereiden](hdinsight-hadoop-r-scripts-linux.md). Klusterin mukauttaminen avulla voit asentaa R HDInsight Hadoop klustereiden. R on Avaa lähde ja tilastollisia tietojenkäsittely ympäristössä. Se on satoja valmiin tilastolliset funktiot ja oma ohjelmointikieli, joka yhdistää toiminnalliset ja Käytä olio-ohjelmoinnin ominaisuuksia. Se sisältää myös monipuolisia graafisia ominaisuuksia.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
