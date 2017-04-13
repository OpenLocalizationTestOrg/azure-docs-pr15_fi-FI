<properties
    pageTitle="HBase opetusohjelma: Aloita Linux-pohjaiset HBase varausyksiköt Hadoop | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase opetusohjelma: Aloita Apache HBase käyttäminen Linux-pohjaiset Hadoop-Hdinsightiin 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Opettele HBase-klusterin luominen HDInsight ja HBase taulukoiden luominen kyselyn taulukoiden käyttämällä rakenne. Artikkelissa HBase Yleistietoja [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview].

Tässä asiakirjassa on Linux-pohjaiset HDInsight klustereiden. Saat lisätietoja Windows-pohjaisesta klustereiden-sivun ylälaidassa sarkainvalitsinta avulla voit siirtyä.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat HBase Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Suojattu Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Luo HBase klusteri

Seuraavassa käytetään luomaan versio 3.4 Linux-pohjaiset HBase klusterin ja riippuvainen oletusarvo Azure-tallennustilan tilin Azure Resurssienhallinta-malli. Käytetään kuvatulla tavalla ja muiden klusterin luominen kautta parametrit on artikkelissa [Luo Linux-pohjaiset Hadoop varausyksiköt Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md).

1. Valitse Avaa malli Azure-portaalissa seuraavassa kuvassa. Malli sijaitsee julkisen blob-säilö. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. - **Mukautettu käyttöönottoa** sivu Anna seuraavat tiedot:

    - **Tilaus**: Valitse Azure tilauksen, jota käytetään luomaan klusterin.
    - **Resurssiryhmä**: Luo uusi Azure Resurssienhallinta-ryhmä tai Käytä aiemmin luotua.
    - **Sijainti**: Määritä resurssiryhmän sijainti. 
    - **ClusterName**: nimi, jonka aiot luoda HBase klusterin.
    - **Klusterin käyttäjätunnus ja salasana**: Kirjautuminen oletusnimi on **järjestelmänvalvoja**.
    - **SSH käyttäjänimi ja salasana**: oletusarvo-käyttäjänimi on **sshuser**.  Voit nimetä sen uudelleen.
     
    Muut parametrit ovat valinnaisia.  
    
    Kunkin klusterin on Azure Blob storage tili-riippuvuus. Kun olet poistanut klusterin, tiedot säilyvät ennallaan-tallennustilan tilin. Klusterin oletusarvon tallennustilan tilin nimi on klusterin nimi "store" perään. On englanninkielisissä mallin muuttujat-osassa.
        
3. Valitse **hyväksyn edellä mainittujen ehdot,**ja valitse sitten **Osta**. Haluat luoda klusterin kestää noin 20 minuutin ajan.


>[AZURE.NOTE] HBase-klusterin poistamisen jälkeen, voit luoda toisen HBase klusterin käyttämällä samaa oletusarvon blob-säilö. Uuden klusterin noutaa luomasi alkuperäisen klusterin HBase taulukot. Epäyhtenäisyydet välttämiseksi Suosittelemme, että poistat HBase taulukot, ennen kuin poistat klusterin.

## <a name="create-tables-and-insert-data"></a>Taulukoiden luominen ja tietojen lisääminen

SSH avulla voit muodostaa yhteyden HBase klustereiden ja sitten HBase Shell HBase taulukoita luodaan, lisää tiedot ja kysely. Lisätietoja SSH käyttämisestä on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop HDInsight Linux, Unix-tai OS X-](hdinsight-hadoop-linux-use-ssh-unix.md) ja [Käytä SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Useimmat käyttäjät tiedot näkyvät ruudukon muodossa:

![HDInsight HBase taulukkomuotoiset tiedot][img-hbase-sample-data-tabular]

HBase, joka on BigTable toteutus samat tiedot näyttää seuraavanlaiselta:

![HDInsight HBase bigtable tiedot][img-hbase-sample-data-bigtable]

Tätä saat kannattaa, kun olet valmis seuraavaan vaiheeseen.  


**Jos haluat käyttää HBase-käyttöliittymä**

1. SSH Suorita seuraava komento:

        hbase shell

4. Luo kaksi palstaa-perheille HBase:

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

    Lisätietoja HBase Taulukkorakenteen artikkelissa [Johdatus HBase rakenteen rakenne][hbase-schema]. Katso Lisää HBase komentoja [Apache HBase opas][hbase-quick-start].

6. Lopeta käyttöliittymä

        exit



**Jos haluat kuormituksen tiedot joukkona yhteystiedot HBase taulukkoon**

HBase sisältää useita eri tapoja tietojen lataaminen taulukoiksi.  Lisätietoja on artikkelissa [joukkona ladataan](http://hbase.apache.org/book.html#arch.bulk.load).


Tietoja mallitiedosto on ladattu julkisen blob-säilö *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Tiedoston sisältö on:

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

> [AZURE.NOTE] Tässä menetelmässä yhteystiedot HBase taulukon luomisen viimeinen ohjeiden mukaisesti.

1. -SSH, suorita seuraava komento muuntamiseen StoreFiles ja säilö suhteellinen polussa Dimporttsv.bulk.output määrittämää datatiedoston:.  Jos olet HBase Shell, Lopeta Lopeta-komennon avulla.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Suorita lataamaan /example/data/storeDataFileOutput tiedot taulukkoon HBase seuraava komento:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Voit avata HBase-käyttöliittymän ja tarkistus-komennon avulla luettelon taulukon sisältöä.



## <a name="use-hive-to-query-hbase"></a>Kyselyn HBase rakenteen avulla

Voit tehdä kyselyn HBase taulukoiden tietojen avulla rakenne. Tässä osassa Luo rakennetaulukko, joka yhdistää HBase-taulukkoon ja käyttää sitä HBase-taulukon tietojen kyselyyn.

1. Avaa **painovärit, muste**ja muodostaa yhteyttä klusterin.  Katso edellä annettuja ohjeita.
2. Avaa rakenne-käyttöliittymän.

       hive
3. Suorita seuraavat HiveQL komentosarja, joka yhdistää HBase taulukko rakenne-taulukon luominen. Varmista, että olet luonut viitata aiemmin tässä opetusohjelmassa käyttämällä HBase-liittymän, ennen kuin suoritat tässä asiakirjassa esimerkkitaulukko.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Suorita seuraava HiveQL komentosarja, HBase-taulukon tietojen kyselyyn:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Käytä HBase REST API kääntö käyttäminen

> [AZURE.NOTE] Kun käytät kääntö tai REST-communication WebHCat kanssa, on todentaa pyynnöt antamalla HDInsight-klusterin järjestelmänvalvojan käyttäjänimi ja salasana. Sinun on käytettävä klusterinimeä myös, tunnus URI (Uniform Resource) lähettämiseen pyynnöt palvelimeen osana.
>
> Tässä osassa komentojen **käyttäjänimi** korvaaminen käyttäjä voi todentaa klusterin ja korvaa **SALASANAN** käyttäjätilin salasanan. Korvaa **CLUSTERNAME** yhteyttä klusterin nimen.
>
> REST API on suojattu kautta [perustodentamista](http://en.wikipedia.org/wiki/Basic_access_authentication). Varmista aina pyynnöt Secure HTTP (HTTPS) avulla voit varmistaa, että tunnistetiedot lähetetään suojatusti palvelimeen.

1. Komentoriviltä Käytä seuraavaa komentoa varmistaaksesi, että voit muodostaa yhteyden HDInsight-klusterin:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Näyttöön tulee vastausta seuraavankaltaiselta:

        {"status":"ok","version":"v1"}

    Käyttää tätä komentoa parametrit ovat seuraavat:

    * **-u** - käyttäjänimen ja salasanan todennetaan pyynnön.
    * **G** - ilmaisee, että tämä on GET-pyynnössä.

2. Käytä seuraavaa komentoa HBase taulut-luettelo:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Luo uusi HBase taulukko, jossa on kaksi saraketta perheille seuraava komento avulla:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Rakenteen annetaan JSon-muodossa.

4. Seuraavalla komennolla voit lisätä tietoja:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Sinun täytyy base64 koodata -d Vaihda arvoja.  Valitse exmaple:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: henkilökohtaisen: nimi
    - Sm9obiBEb2xl: John Dole

    [Epätosi-rivi-avaimen](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) avulla voit lisätä useita (oletusmäärä) arvo.

5. Käytä seuraavaa komentoa saat rivin:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Saat lisätietoja HBase muiden [Apache HBase opas](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Klusterin tilan tarkistaminen

HBase HDInsight mukana Web-Käyttöliittymän klustereiden seurantaa varten. Web-Käyttöliittymän voit pyytää tilastotiedot tai alueiden tietoja.

SSH avulla voidaan myös tunnelin paikallisen pyynnöt, kuten web-pyyntöjen HDInsight-klusterin. Pyyntö reititetään sitten Pyydetty resurssi on aivan kuin se oli luotu HDInsight-klusterin pään solmun. Lisätietoja on artikkelissa [Käyttäminen SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Jos haluat muodostaa SSH tunneling istunnon**

1. Avaa **painovärit, muste**.  
2. Jos antamasi SSH avain luodessasi käyttäjätili luomisen aikana, sinun on tehtävä seuraavat toimet voit valita kun todennustapa klusterin yksityinen avain:

    **Luokka**Laajenna **yhteys**, laajenna **SSH**ja valitse **todennus**. Lopuksi Valitse **Selaa** ja valitse .ppk tiedosto, joka sisältää yksityinen avain.

3. Valitse **luokka**- **istunto**.
4. Painovärit, muste istunnon näytön Basic asetukset Kirjoita seuraavat arvot:

    - **Isäntänimi**: HDInsight server Host name (tai IP-osoite)-kentän SSH osoitetta. SSH-osoite on klusterin nimesi, valitse **-ssh.azurehdinsight.net**. Esimerkiksi *mycluster ssh.azurehdinsight.net*.
    - **Portti**: 22. Ssh ensisijainen headnode porttiin on 22.  
5. Vasemmalla puolella valintaikkunan **luokka** -osasta Laajenna **yhteys**, laajenna **SSH**ja valitse sitten **tunneleita**.
6. Anna seuraavat tiedot SSH portin edelleenlähetys lomakkeen hallinta-asetukset:

    - **Lähdeportti** - asiakas, johon haluat lähettää porttiin. Esimerkiksi 9876.
    - **Dynaaminen** – ottaa dynaaminen SOCKS välityspalvelimen reititys.
7. Lisää valitsemalla **Lisää** asetuksia.
8. Valitse **Avaa** valintaikkunan alareunassa SSH-yhteyden.
9. Kun sinulta kysytään, kirjaudu sisään SSH tilillä palvelimeen. Tämä muodostaa SSH-istunnon ja ota käyttöön tunnelin.

**Voit etsiä käyttämällä Ambari zookeepers täydellinen toimialuenimi**

1. Siirry https://<ClusterName>.azurehdinsight.net/.
2. Kirjoita tilin klusterin käyttäjätietojen kahdesti.
3. Valitse vasemmanpuoleisessa valikossa **zookeeper**.
4. Valitse jokin kolmesta **ZooKeeper palvelimen** linkeistä, yhteenveto-luettelosta.
5. Kopioi **isäntänimi**. Esimerkiksi zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Asiakasohjelman (Firefox) ja klusterin tilan tarkistaminen**

1. Avaa Firefox.
2. Valitse **Avaa valikko** -painiketta.
3. Valitse **asetukset**.
4. Valitse **Lisäasetukset**, valitse **Verkko**ja valitse sitten **asetukset**.
5. Valitse **Manuaalinen välityspalvelimen määritykset**.
6. Kirjoita seuraavat arvot:

    - **Socks Host (isäntä)**: localhost
    - **Portti**: käyttää määritit-painovärit, muste SSH tunneling samaa porttia.  Esimerkiksi 9876.
    - **SOCKS v5**: (valittu)
    - **Remote DNS**: (valittu)
7. Valitse **OK** , Tallenna muutokset.
8. Siirry http://&lt;täydellinen toimialuenimi ZooKeeper >: 60010/perustyyli-tila.

Suuri käytettävyys-klusterin etsii nykyinen aktiivinen HBase perusmuodon solmu, joka isännöi Web-Käyttöliittymän linkki.

##<a name="delete-the-cluster"></a>Klusterin poistaminen

Epäyhtenäisyydet välttämiseksi Suosittelemme, että poistat HBase taulukot, ennen kuin poistat klusterin.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä HDInsight HBase opetusohjelmassa opit HBase-klusterin luominen sekä luoda taulukoita ja tarkastella tietoja HBase runko kyseisten taulukoiden. Opit myös rakenne-kyselyn käyttämisestä HBase taulukoiden tietojen ja Opi käyttämään HBase C# REST API HBase-taulukon luominen ja tietojen noutaminen taulukosta.

Lisätietoja on artikkeleissa:

- [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview]: HBase on rakennettu Hadoop, joka tarjoaa RAM ja vahva yhdenmukaisuuden rakenteeton ja semistructured suurista tietomääristä Apache, Avaa-lähde-NoSQL tietokanta.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
