<properties
    pageTitle="Komentosarja-toiminnon avulla voit asentaa Solr Hadoop-klusterin | Microsoft Azure"
    description="Opettele mukauttamaan HDInsight-klusterin kanssa Solr komentosarja-toiminnon avulla."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Asentaminen ja käyttäminen Solr HDInsight Hadoop klustereiden

Katso, miten voit mukauttaa Windows perustuvat HDInsight-klusterin kanssa Solr komentosarja-toiminnon avulla ja Solr avulla voit hakea tietoja. Lisätietoja Linux-pohjaiset klusterin Solr käyttämisestä on artikkelissa [asentaminen ja käyttäminen Solr-HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Voit asentaa Solr kaikentyyppisissä klusterin (Hadoop, myrsky, HBase, ohjattu) Azure Hdinsightiin *Komentosarja-toiminnon*avulla. Esimerkki komentosarjan Solr asennetaan HDInsight-klusterin on käytettävissä vain luku-Azure tallennustilan Blob-objektien [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)-palvelussa. 

Esimerkki komentosarja toimii vain HDInsight klusterin versio 3.1. Katso lisätietoja HDInsight-klusterin versioissa, [HDInsight-klusterin versiot](hdinsight-component-versioning.md).

Tämän artikkelin käytetty mallikomentosarja Luo Windows-pohjaisesta Solr klusterin tietty määritys. Jos haluat määrittää Solr-klusterin eri sivustokokoelmat, shards, mallit, replikoiden, jne., Solr binaaritiedostot ja komentosarja on muokattava vastaavasti.

**Aiheeseen liittyviä artikkeleita**

- [Asentaminen ja käyttäminen Solr HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Luo Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md): yleisiä tietoja HDInsight klustereiden luomisesta.
- [Mukauta HDInsight-klusterin komentosarja-toiminnon käyttäminen][hdinsight-cluster-customize]: yleisiä tietoja mukauttamisesta HDInsight klustereiden komentosarja-toiminnon avulla.
- [Kehittää komentosarja-toiminnon komentosarjojen Hdinsightista](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Mikä on Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> on enterprise-haku-ympäristö, jonka avulla tehokas tietojen teksti-haku. Kun Hadoop mahdollistaa tallentaminen ja hallinta suuria määriä tietoja, Apache Solr on hakutoimintojen nopeasti tietojen hakemiseen. 

## <a name="install-solr-using-portal"></a>Asenna Solr-portaalissa

1. Aloita luominen klusteriin valitsemalla **Luo mukautettu** -vaihtoehdon, [Luo Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md#portal)-palvelussa kuvatulla.
2. Valitse ohjatun toiminnon **Komentosarjatoiminnot** -sivulla tietojen näyttäminen komentosarja-toiminnon **lisääminen komentosarja-toiminnon** , alla kuvatulla tavalla:

    ![Käytä komentosarja-toiminnon klusterin mukauttamiseen] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Käytä komentosarja-toiminnon mukauttamiseen klusteriin")

    <table border='1'>
        <tr><th>Ominaisuus</th><th>Arvo</th></tr>
        <tr><td>Nimi</td>
            <td>Määritä komentosarja-toiminnon nimi. Esimerkiksi <b>Asentaa Solr</b>.</td></tr>
        <tr><td>Komentosarjan URI</td>
            <td>Määritä-tunniste URI (Uniform Resource) komentosarjan, joka suoritetaan klusterin mukauttamiseen. Esimerkiksi <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Solmutyyppi</td>
            <td>Määritä solmujen, jossa mukauttaminen komentosarja suoritetaan. Voit valita <b>kaikki solmut</b>, <b>vain Head solmujen</b>tai <b>vain työntekijä solmujen</b>.
        <tr><td>Parametrit</td>
            <td>Määritä parametreja, jos vaatii komentosarja. Asenna Solr komentosarja ei edellytä parametreja, niin voit jättää tämän tyhjäksi.</td></tr>
    </table>

    Voit lisätä useita komentosarja-toiminnon asentamisesta klusterin useita osia. Kun olet lisännyt komentosarjat, napsauta Aloita klusterin luominen valintamerkkiä.


## <a name="use-solr"></a>Käytä Solr

On aloitettava indeksoinnin Solr tietojen joidenkin tiedostojen kanssa. Voit käyttää Solr suorittaa hakuja indeksoidut tiedot. Seuraavien toimien Solr käyttäminen HDInsight-klusterin:

1. **Käytä RDP Remote Desktop Protocol (), remote kyselyjä ja Solr asennettu HDInsight-klusterin**. Azure-portaalista käyttöön luomasi Solr asennettu ja sitten etäyhteyksien kyselyjä klusterin klusterin etätyöpöydän kautta. Ohjeita on artikkelissa [etäyhteyden muodostaminen HDInsight klustereiden RDP avulla](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Indeksi Solr lataamalla datatiedostot**. Kun indeksoit Solr, tiedostojen siirtäminen-toimintoon, jonka haluat etsiä. Luoda hakemiston Solr käyttämällä RDP remote klusterin kyselyjä, siirry työpöydälle, Avaa Hadoop-komentoriviltä ja siirry **C:\apps\dist\solr-4.7.2\example\exampledocs**. Suorita seuraava komento:

        java -jar post.jar solr.xml monitor.xml

    Näkyviin tulee seuraava tulos konsolissa:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Post.jar-apuohjelman indeksoi Solr kahden otoksen asiakirjoja, **solr.xml** ja **monitor.xml**. Post.jar-apuohjelma ja malli-tiedostot ovat käytettävissä Solr asennuksen.

3. **Käytä Solr Raporttinäkymät-ikkunan avulla voit hakea indeksoidut tiedostot**. Avaa Internet Explorer HDInsight-klusterin RDP-istunnon ja Käynnistä osoitteessa Solr Raporttinäkymät-ikkunan **http://headnodehost:8983/solr / #/**. Valitse **collection1**vasemmanpuoleisessa ruudussa **Core valitseminen** avattavasta luettelosta, ja sisällä, valitse **kysely**. Esimerkkinä voit valita ja palauttaa kaikki Omat asiakirjat Solr, anna seuraavat arvot:

    * Kirjoita **q** -tekstiruutuun ** \*:**\*. Tämä palauttavat kaikki tiedostot, jotka on indeksoitu Solr. Jos haluat etsiä tietyn merkkijonossa asiakirjat, voit syöttää kyseisen merkkijonon.
    
    * Valitse **wt** teksti-ruutuun muodossa. Oletusarvo on **json**. Valitse **kyselyn suorittaminen**.

    ![Käytä komentosarja-toiminnon klusterin mukauttamiseen] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Suorita kysely Solr koontinäytössä")
    
    Tulosteen palauttaa kahden asiakirjoja, jotka on käytetty indeksoinnin Solr. Tulos näyttää jotakuinkin seuraavasti:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Suositus: Varmuuskopioi indeksoidut tiedot liittyvät HDInsight-klusterin Azure-Blob-säiliö Solr**. Hyvä käytäntö on Varmuuskopioi indeksoidut tiedot-sivulle Azure-Blob-säiliö Solr klusterisolmut. Seuraavien toimien tarvittaessa:

    1. Avaa Internet Explorer RDP-istunnon ja valitse seuraava URL-osoite:

            http://localhost:8983/solr/replication?command=backup

        Pitäisi näkyä vastausta seuraavasti:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Siirry Etäistunto, {SOLR_HOME}\{sivustokokoelman} \data. Klusterin luotu komentosarjan kautta on oltava **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Tähän sijaintiin pitäisi näkyä *samalla nimellä luoda tilannevedoksen kansion *tilannevedoksen.* aikaleiman***.

    3. Zip-tilannevedos-kansio ja lataa se Azure-Blob-säiliö. Hadoop komentoriviltä Siirry tilannevedos-kansion sijainti käyttämällä seuraava komento:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Tämä komento kopioi tilannevedoksen /example/data tai valitse Oletus tallennustilan tiliin liitetyn klusterin säilössä.

## <a name="install-solr-using-aure-powershell"></a>Asenna Solr Aure PowerShellin avulla

Katso [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Esimerkki havainnollistaa ohjattu Azure PowerShellin asennusohjeet. Haluat mukauttaa komentosarja käyttämään [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Asenna Solr .NET SDK: N avulla

Katso [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Esimerkki havainnollistaa käyttämällä .NET SDK ohjattu asentaminen. Haluat mukauttaa komentosarja käyttämään [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).



## <a name="see-also"></a>Katso myös

- [Asentaminen ja käyttäminen Solr HDinsight Hadoop varausyksiköt (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Luo Hadoop varausyksiköt HDInsight](hdinsight-provision-clusters.md): yleisiä tietoja HDInsight klustereiden luomisesta.
- [Mukauta HDInsight-klusterin komentosarja-toiminnon käyttäminen][hdinsight-cluster-customize]: yleisiä tietoja mukauttamisesta HDInsight klustereiden komentosarja-toiminnon avulla.
- [Kehittää komentosarja-toiminnon komentosarjojen Hdinsightista](hdinsight-hadoop-script-actions.md).
- [Asentaminen ja käyttäminen ohjattu HDInsight klustereiden][hdinsight-install-spark]: asentamisesta ohjattu otoksen komentosarja-toiminnon.
- [R asentaminen HDInsight klustereiden][hdinsight-install-r]: komentosarja-toiminnon otoksen asentamisesta R.
- [Asenna Giraph-HDInsight klustereiden](hdinsight-hadoop-giraph-install.md): komentosarja-toiminnon otoksen asentamisesta Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
