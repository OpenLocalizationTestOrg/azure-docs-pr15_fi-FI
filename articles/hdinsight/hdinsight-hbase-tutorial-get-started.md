<properties
    pageTitle="HBase opetusohjelma: Aloita HBase Hadoop | Microsoft Azure"
    description="Katso tämä HBase opetusohjelma, voit aloittaa käytön Apache HBase Hadoop HDInsight. Taulukoiden luominen HBase käyttöliittymä ja kyselyjen ne käyttämällä rakenne."
    keywords="Apache hbase hbase, hbase shell-hbase-opetusohjelma"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase opetusohjelma: Aloita Apache HBase käyttäminen Windows-pohjaisesta Hadoop-Hdinsightiin

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Opettele HBase klustereiden luominen HDInsight ja HBase taulukoiden luominen kyselyn taulukoiden käyttämällä Apache rakenne. Artikkelissa HBase Yleistietoja [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview].

Windows-pohjaisesta HDInsight klustereiden on tämän asiakirjan tiedot. Saat lisätietoja Windows-pohjaisesta klustereiden-sivun ylälaidassa sarkainvalitsinta avulla voit siirtyä.

> [AZURE.NOTE] Windows-pohjaisesta HDInsight-HBase (versio 0.98.0) on vain käytettäväksi HDInsight 3.1 klustereiden (Apache Hadoop ja perustuvat kuitenkaan 2.4.0). Versiotiedot-kohdassa [uudet myöntämä HDInsight Hadoop-klusterin versioissa?][hdinsight-versions]

## <a name="before-you-begin"></a>Ennen aloittamista

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ennen kuin aloitat HBase Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **A Microsoft Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Työasema** ja Visual Studio 2013 tai uudempi: ohjeita on artikkelissa [Asenna Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Luo HBase klusteri

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Voit luoda HBase-klusterin Azure-portaalissa**

1. Kirjautuminen [Azure portal][azure-management-portal].
2. Valitse **Uusi** tai **+** -vasemmassa yläreunassa kulman ja valitse sitten **tiedot + Analytics**- **Hdinsightista**.
3. Kirjoita seuraavat arvot:

    - **Klusterinimi** - tunnistavan tämän klusterin nimi.
    - **Klusterin tyyppi** - Valitse **HBase**.
    - **Klusterin käyttöjärjestelmän** – Valitse **Windows**.  Luomisen Linux-pohjaiset HBase klusterin, katso lisätietoja artikkelista [HBase opetusohjelma: Aloita Apache HBase käyttäminen HDInsight (Linux) Hadoop](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Version** – Valitse HBase-versio.
    - **Tilauksen** – Valitse Azure tilauksen käytetään tämän klusterin luomiseen.
    - **Resurssiryhmä** - Luo uusi Azure resurssiryhmä, tai valitse olemassa. Lisätietoja on artikkelissa [Azure Resurssienhallinta yleiskatsaus](azure-resource-manager/resource-group-overview.md)
    - **Tunnistetiedot** - Windows-pohjaisten klusterin, voit luoda klusterin käyttäjä (a.k.a HTTP-käyttäjä, WWW-palvelun käyttäjän HTTP) ja etätyöpöydän käyttäjä. Valitse **Ota käyttöön etätyöpöytä** Lisää remote työpöydän käyttäjän tunnistetietoja. Seuraavassa osassa edellyttää RDP.
    - **Tietolähde** - Luo uusi tallennustilan Azure-tili, tai valitse aiemmin Azure-tallennustilan tilin voidaan käyttää käyttöjärjestelmän klusterin. Tallennustilan oletussijainti määrittää klusterin sijainti sijainti. Tallennustilan oletustilin ja klusterin on yhtä Etsi saman tietokeskuksen.
    - **Solmun hinnat tasoa** – valitse numero HBase-klusterin palvelinten alue

        > [AZURE.WARNING] Suuren käytettävyyden HBase palveluja, sinun on luotava klusteria, joka sisältää vähintään **kolme** solmujen. Näin varmistat, että jos yksi solmu siirtyy, HBase tietoalueita ovat käytettävissä muissa solmuissa.

        > Jos oppivat HBase, aina valita klusteri 1, ja poista klusterin jokaisen käytön pienentää jälkeen.

    - **Valinnainen määritys** : Määritä Azure virtual verkko-komentosarjan toimintojen määrittämistä ja lisää tallennustilaa tilejä.

4. Valitse **Luo**.

>[AZURE.NOTE] HBase-klusterin poistamisen jälkeen, voit luoda toisen HBase klusterin käyttämällä samaa tallennustilan oletustilin ja oletusarvon blob-säilö. Uuden klusterin noutaa luomasi alkuperäisen klusterin HBase taulukot. Epäyhtenäisyydet välttämiseksi Suosittelemme, että poistat HBase taulukot, ennen kuin poistat klusterin.

## <a name="create-tables-and-insert-data"></a>Taulukoiden luominen ja tietojen lisääminen

Tällä hetkellä on kaksi tapaa, jolla voit käyttää HBase. Tässä osassa käsitellään HBase-liittymän avulla. Seuraavassa osassa kerrotaan käyttämällä .NET SDK.

Useimmat käyttäjät tiedot näkyvät ruudukon muodossa:

![hdinsight hbase taulukkomuotoiset tiedot][img-hbase-sample-data-tabular]

HBase, joka on BigTable toteutus samat tiedot näyttää seuraavanlaiselta:

![hdinsight hbase bigtable tiedot][img-hbase-sample-data-bigtable]

Se tehdä kannattaa, kun olet valmis seuraavaan vaiheeseen.  

**Jos haluat käyttää HBase-käyttöliittymä**

1. Yhteyden muodostaminen HBase-klusterin HDInsight-RDP avulla. RDP-ohjeita on kohdassa [hallinta Hadoop varausyksiköt portaalissa Azure Hdinsightiin][hdinsight-manage-portal].
2. Olet RDP-istunnon Napsauta työpöydällä **Hadoop komentorivin** pikakuvaketta.
3. Avaa HBase-liittymän:

        cd %HBASE_HOME%\bin
        hbase shell

4. Luo kaksi saraketta perheille HBase:

        create 'Contacts', 'Personal', 'Office'
        list
5. Lisää tietoja:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase käyttöliittymä][img-hbase-shell]

6. Hae yhdelle riville

        get 'Contacts', '1000'

    Näet saman tuloksen kuin tarkistus-komennon avulla, koska on vain yksi rivi.

    Lisätietoja Hbase Taulukkorakenteen artikkelissa [Johdatus HBase rakenteen rakenne][hbase-schema]. Katso Lisää HBase komentoja [Apache HBase opas][hbase-quick-start].


6. Lopeta käyttöliittymä

        exit

**Jos haluat kuormituksen tiedot joukkona yhteystiedot HBase taulukkoon**

HBase sisältää useita eri tapoja tietojen lataaminen taulukoiksi. Lisätietoja on artikkelissa [joukkona ladataan](http://hbase.apache.org/book.html#arch.bulk.load).


Tietoja mallitiedosto on ladattu julkisen blob-säilö wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Tiedoston sisältö on:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Voit luoda tekstitiedostosta ja lataa tiedosto tallennustilan-tiliä, voit halutessasi. Lisätietoja on kohdassa [Lataa tiedot Hadoop projekteille HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Tässä menetelmässä edellisen prosessin vaiheessa luomasi yhteystiedot HBase taulukon.

1. Sisällä RDP-istunnon Valitse työpöydällä **Hadoop komentoriviltä** pikakuvake.
2. Muuta kansion:

        cd %HBASE_HOME%\bin

3. Suorita seuraava komento muuntamiseen StoreFiles ja säilö suhteellinen polussa Dimporttsv.bulk.output määrittämää datatiedosto:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Suorita lataamaan /example/data/storeDataFileOutput tiedot taulukkoon HBase seuraava komento:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Voit avata HBase-käyttöliittymän ja tarkistus-komennon avulla luettelon taulukon sisältöä.



## <a name="use-hive-to-query-hbase-tables"></a>Rakenteen avulla HBase taulukot

Voit tehdä kyselyn rakenne käyttämällä HBase tallennettuja tietoja. Tässä osassa Luo rakennetaulukko, joka yhdistää HBase-taulukkoon ja käyttää sitä HBase-taulukon tietojen kyselyyn.

**Avaa klusterin Raporttinäkymät-ikkunan**

1. Siirry **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Kirjoita Hadoop käyttäjätilin käyttäjänimi ja salasana. Käyttäjän oletusnimi on **järjestelmänvalvojan** ja salasana on kirjoitettu luomisen aikana. Avaa uusi selaimen välilehti.
6. Valitse **Rakenne-editorin** sivun yläreunassa. Rakenne-editori on seuraavanlainen:

    ![HDInsight-klusterin Raporttinäkymät-ikkunan.][img-hdinsight-hbase-hive-editor]

**Rakenne-kyselyt**

1. Kirjoita HiveQL seuraavaa komentosarjaa editoriin rakenne ja valitse **Lähetä** rakenne taulukon, joka yhdistää HBase-taulukon luominen. Varmista, että luomasi mallitaulukon viitata aiemmin tässä opetusohjelmassa käyttämällä HBase-liittymän, ennen kuin suoritat tässä asiakirjassa.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Odota, kunnes **tilapäivitykset **Valmis**** .

2. Kirjoita HiveQL seuraavaa komentosarjaa rakenne-editoriin ja valitse sitten **Lähetä**. Rakenne-kyselyn kyselyt HBase-taulukon tiedot:

        SELECT count(*) FROM hbasecontacts;

4. Noutaa rakenne-kyselyn tulokset, valitse **Tarkasteleminen** linkkiä **Työn istunnon** -ikkunassa, kun työ on päättynyt. Hakutuloksissa näkyy vain yhden projektin kohdetiedosto koska yhden tietueen valitseminen HBase-taulukkoon.




**Voit selata kohdetiedosto**

1. Valitse kysely-konsolin **Tiedosto selaimessa**.
2. Valitse Azure tallennustilan tili, jota käytetään käyttöjärjestelmän HBase-klusterin.
3. Napsauta HBase klusterinimeä. Oletusarvoinen Azure-tallennustilan tilin säilö käyttää klusterinimeä.
4. Valitse **käyttäjä**ja valitse sitten **järjestelmänvalvoja**. (Tämä on Hadoop-käyttäjänimen.)
6. Valitse **Muokattu viimeksi** aika, joka vastaa aikaa, kun valitse rakenne-kyselyn suorituksen projektin nimi.
4. Valitse **stdout**. Tallenna tiedosto ja Avaa tiedosto Muistiossa. Ole yhtä kohdetiedosto.

    ![HDInsight HBase rakenteen editorin tiedosto selaimessa][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>.NET HBase REST API asiakas-kirjastossa

Lataa HBase REST API asiakas-kirjastoon, .NET-GitHub ja luoda projektin niin, että voit käyttää HBase .NET SDK. Seuraavassa on ohjeita tämän tehtävän.

1. Luo uusi C# Visual Studio Windows Desktop Console-sovellus.
2. Avaa NuGet paketin hallinta-konsolin valitsemalla **Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**.
3. Suorita seuraava komento NuGet konsolissa:

        Install-Package Microsoft.HBase.Client

5. Lisää seuraavat **käyttämällä** lauseet tiedoston yläosassa:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. **Pää** -funktio korvaa seuraavasti:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Määritä ensin kolme muuttujaa **Main** -funktiossa.
8. Painamalla **F5** sovelluksen käyttämiseen.

## <a name="check-cluster-status"></a>Klusterin tilan tarkistaminen

HBase HDInsight mukana Web-Käyttöliittymän klustereiden seurantaa varten. Web-Käyttöliittymän voit pyytää tilastotiedot tai alueiden tietoja.

Avaa Web-Käyttöliittymän on RDP klusterin kyselyjä ja sitten valitsemalla HMaster tiedot Web-Käyttöliittymän pikakuvakkeen työpöydälle, tai käyttää seuraavaa URL-Osoitetta selaimessa:

    http://zookeeper[0-2]:60010/master-status

Löydät linkin nykyisen aktiivisen HBase perusmuodon solmun, joka isännöi Web-Käyttöliittymän suuri käytettävyys-klusterin.

##<a name="delete-the-cluster"></a>Klusterin poistaminen
Epäyhtenäisyydet välttämiseksi Suosittelemme, että poistat HBase taulukot, ennen kuin poistat klusterin.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Mitä seuraavaksi?
Tässä HDInsight HBase opetusohjelmassa opit HBase-klusterin luominen sekä luoda taulukoita ja tarkastella tietoja HBase runko kyseisten taulukoiden. Voit myös asiat kuinka käyttää HBase taulukoissa ja Opi käyttämään HBase C# REST API HBase-taulukon luominen ja tietojen noutaminen taulukosta kyselyn rakenne.

Lisätietoja on artikkelissa:

- [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview].
HBase on rakennettu Hadoop, joka tarjoaa RAM ja vahva yhdenmukaisuuden rakenteeton ja semistructured suurista tietomääristä Apache, Avaa-lähde-NoSQL tietokanta.
- [Luo HBase klustereiden Azure Virtual verkossa][hdinsight-hbase-provision-vnet].
VPN-integroinnin HBase klustereiden voidaan ottaa käyttöön virtual samassa verkossa kuin sovelluksia niin, että sovellukset voivat viestiä HBase suoraan.
- [Määritä HBase replikointi Hdinsightista](hdinsight-hbase-geo-replication.md). Opettele määrittämään HBase replikoinnin yli kaksi Azure palvelinkeskusten.
- [Analysoi Twitter markkinatunnelma kanssa HBase HDInsight][hbase-twitter-sentiment].
Opi laskemaan reaaliaikainen [markkinatunnelma analyysi](http://en.wikipedia.org/wiki/Sentiment_analysis) big datasta käyttämällä HBase HDInsight-Hadoop-klusterin.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
