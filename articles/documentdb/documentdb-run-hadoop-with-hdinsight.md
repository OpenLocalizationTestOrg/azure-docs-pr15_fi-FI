<properties
    pageTitle="Suorita Hadoop työn DocumentDB ja Hdinsightista | Microsoft Azure"
    description="Opettele suorittaa yksinkertaisen rakenne, Possu ja MapReduce työn DocumentDB ja Azure Hdinsightista."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Suorita Hadoop työn DocumentDB ja Hdinsightista

Tässä opetusohjelmassa näytetään, miten voit suorittaa [Apache rakenne][apache-hive], [Apache Possu][apache-pig], ja [Apache Hadoop] [ apache-hadoop] MapReduce työt Azure Hdinsightiin DocumentDB's Hadoop Connectorilla. DocumentDB's Hadoop Connectorin avulla DocumentDB lähde- ja rakenne, Possu ja MapReduce töiden käsittelytoiminto edustajana. Tässä opetusohjelmassa käyttää DocumentDB sekä tietolähteen kohde Hadoop työt.

Kun olet suorittanut tämän opetusohjelman, jonka voi seuraaviin kysymyksiin:

- Miten tietoja ladataan DocumentDB rakenne, Possu tai MapReduce avulla?
- Miten tiedot tallennetaan DocumentDB rakenne, Possu tai MapReduce avulla?

On suositeltavaa aloittaminen katselemalla seuraavassa videossa, jossa on suorittaa rakenteen työn DocumentDB ja Hdinsightista kautta.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Palaa sitten tämän artikkelin kohtaa, johon saat tarkat tiedot, miten voit suorittaa analytics työt DocumentDB tiedot.

> [AZURE.TIP] Tässä opetusohjelmassa oletetaan, että edellisen kokemus Apache Hadoop, rakenne ja/tai Possu avulla. Jos ole ennen käyttänyt Apache Hadoop, rakenne ja Possu, on suositeltavaa ohjesisältöä [Apache Hadoop ohjeissa][apache-hadoop-doc]. Tässä opetusohjelmassa myös oletetaan, että olet DocumentDB edellisen kokemusta ja DocumentDB tili. Jos ole aiemmin käyttänyt DocumentDB tai sinulla ei ole DocumentDB-tili, katso Ota Microsoftin [Aloittaminen] [ getting-started] sivulle.

Älä on aikaa opetusohjelman ja haluat vain koko PowerShell-komentosarjamallit hankkiminen rakenne, Possu ja MapReduce? Ei ole ongelma, poistaa heidät [tästä][documentdb-hdinsight-samples]. Lataaminen on myös näitä esimerkkejä hql, possu ja java-tiedostot.

## <a name="NewestVersion"></a>Uusin versio

<table border='1'>
    <tr><th>Hadoop Connector-versio</th>
        <td>1.2.0</td></tr>
    <tr><th>Komentosarjan Uri</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Muokkauspäivämäärä</th>
        <td>04/26 ja 2016</td></tr>
    <tr><th>Tuetut HDInsight-versiot</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Muutosloki</th>
        <td>Päivitetty DocumentDB Java SDK 1.6.0</br>
            Tukee nyt osioitua sivustokokoelmat lähde-ja käsittelytoiminto</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Edellytykset
Ennen kuin noudattamalla tässä opetusohjelmassa, varmista, että käytössäsi ovat seuraavat:

- DocumentDB-tili, tietokannan ja sivustokokoelman tiedostojen. Lisätietoja on artikkelissa [Käytön aloittaminen DocumentDB][getting-started]. Tietojen tuominen DocumentDB tiliisi [DocumentDB tuo työkalun][documentdb-import-data].
- Siirtonopeuden. Lukee ja kirjoittaa HDInsight-lasketaan kohti asetetun pyynnön-yksiköt loppu. Lisätietoja on artikkeleissa [Provisioned siirtonopeuden, pyyntö yksiköt ja tietokannan][documentdb-manage-throughput].
- Lisää tallennetun toimintosarjan kunkin kapasiteetin tulosteen sivustokokoelman. Tallennettujen toimintosarjojen käytetään siirtäminen tuloksena olevat tiedostot. Lisätietoja on artikkelissa [sivustokokoelmat ja valmistellun siirtonopeuden][documentdb-manage-document-storage].
- Kapasiteetin rakenne, Possu tai MapReduce työt tuloksena olevat tiedostot. Lisätietoja on artikkelissa [Hallitse DocumentDB kapasiteetin ja suorituskyvyn][documentdb-manage-collections].
- [*Valinnainen*] Lisää sivustokokoelman kapasiteetti. Lisätietoja on artikkelissa [Provisioned asiakirjojen säilyttäminen ja yleisrasite indeksi][documentdb-manage-document-storage].

> [AZURE.WARNING] Siten vältät aikana töiden uusia sivustokokoelman luomista, voit joko tulostaa tulokset stdout, tallentaa WASB säilöön, tai määrittää aiemmin luodusta sivustokokoelman. Aiemmin luotu kokoelma, joka määrittää, uusia asiakirjoja luodaan sisällä kokoelma ja jo aiemmin luotuja tiedostoja vain heikentyä, jos *tunnuksista*on ristiriita. **Yhdistimen automaattisesti korvaa aiemmin luotuja tiedostoja tunnus ristiriitoja**. Voit ottaa tämän toiminnon käytöstä määrittämällä upsert-asetuksen arvoksi false. Jos upsert on EPÄTOSI ja ristiriitoja ilmenee, Hadoop-työ epäonnistuu; raportoinnin tunnus ristiriita-virheen.


## <a name="ProvisionHDInsight"></a>Vaihe 1: Uuden HDInsight-klusterin luominen
Tässä opetusohjelmassa mukauttaminen HDInsight-klusterin Azure-portaalista komentosarja-toiminnon avulla. Tässä opetusohjelmassa Käytämme portaalin Azure HDInsight-klusterin luominen. Ohjeita siitä, miten voit käyttää PowerShellin cmdlet-komennot tai HDInsight .NET SDK on Kuittaa ulos [mukauttaminen HDInsight klustereiden komentosarja-toiminnon käyttäminen] [ hdinsight-custom-provision] artikkelissa.

1. Kirjautuminen [Azure Portal][azure-portal].

2. Valitse **+ Uusi** vasemmassa siirtymisruudussa uusi sivu yläreunan Etsintäpalkki **HDInsight** hakeminen ylälaidassa.

3. **Microsoftin** julkaisemia **HDInsight** näkyvät tulokset yläreunassa. Napsauta sitä ja valitse sitten **Luo**.

4. Uusi HDInsight-klusterin luominen sivu, **Klusterinimi** ja valmistella, valitse resurssi **tilauksen** .

    <table border='1'>
        <tr><td>Klusterinimi</td><td>Klusterin nimi.<br/>
   DNS-nimen on Käynnistä-painiketta ja aakkosnumeeristen merkkien perässä voi olla katkoviivat.<br/>
   Kentän on oltava merkkijonon 3 ja 63 merkkien välillä.</td></tr>
        <tr><td>Tilauksen nimi</td>
            <td>Jos sinulla on useampi kuin yksi Azure-tilauksia, valitse tilaus, joka isännöi HDInsight-klusterin. </td></tr>
    </table>

5. Valitse **Valitse klusterin tyyppi** ja määritä seuraavat ominaisuudet määritetyt arvot.

    <table border='1'>
        <tr><td>Klusterin tyyppi</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Klusterin taso</td><td><strong>Vakio</strong></td></tr>
        <tr><td>Käyttöjärjestelmä</td><td><strong>Windows</strong></td></tr>
        <tr><td>Versio</td><td>uusin versio</td></tr>
    </table>

    Valitse nyt, **Valitse**.

    ![Anna Hadoop HDInsight alkuperäisen klusterin tiedot][image-customprovision-page1]

6. Valitse Kirjaudu sisään ja etäkäyttöpalvelimen tunnistetiedot **tunnistetiedot** . Valitse **klusterin kirjautumisen käyttäjänimi** ja **klusterin kirjautumissalasana**.

    Jos haluat ottaa remote yhteyttä klusterin kyselyjä, valitse *Kyllä* sivu alareunassa ja Anna käyttäjänimi ja salasana.

7. Valitse **Tietolähteen** Aseta tietojen käytön ensisijainen sijainti. Valitse **Valinnan menetelmä** ja määritä jo olemassa olevan tallennustilan tilin tai luoda uuden.

8. Määritä **Oletusarvoinen säilö** ja **sijainnin**saman sivu. Ja **Valitse**sitten.

    > [AZURE.NOTE] Valitse sijainti lähellä DocumentDB tilin alueen suorituskyvyn parantamiseksi

8. Valitse Valitse numero ja kirjoita solmujen **hinnoittelu** . Voit pitää oletusarvo-määritys ja skaalata työntekijä solmujen määrän myöhemmin.

9. Valitse **Valinnainen määritysten**jälkeen **komentosarjatoiminnot** vaihtoehtoinen määritys-sivu.

    Kirjoita komentosarjatoiminnot, voit mukauttaa HDInsight-klusterin seuraavat tiedot.

    <table border='1'>
        <tr><th>Ominaisuus</th><th>Arvo</th></tr>
        <tr><td>Nimi</td>
            <td>Määritä komentosarja-toiminnon nimi.</td></tr>
        <tr><td>Komentosarjan URI</td>
            <td>Määritä komentosarja, joka käynnistyy, voit mukauttaa klusterin URI.</br></br>
   Kirjoita: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Pää</td>
            <td>Valitse valintaruutu, jos haluat suorittaa PowerShell-komentosarjaa pää-solmu sivulle.</br></br>
            <strong>Valitse tämä valintaruutu</strong>.</td></tr>
        <tr><td>Työntekijän</td>
            <td>Valitse valintaruutu, jos haluat suorittaa PowerShell-komentosarjaa työntekijä-solmu sivulle.</br></br>
            <strong>Valitse tämä valintaruutu</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Valitse valintaruutu, jos haluat suorittaa tarralle Zookeeper PowerShell-komentosarjaa.</br></br>
            <strong>Ei tarvita</strong>.
            </td></tr>
        <tr><td>Parametrit</td>
            <td>Määritä parametreja, jos vaatii komentosarja.</br></br>
            <strong>Parametrien ei tarvita</strong>.</td></tr>
    </table>

10. Luo joko uusi **Resurssiryhmä** tai Käytä aiemmin luotua resurssi-ryhmää Azure-tilauksen mukaisesti.

11. Katso nyt **raporttinäkymät-ikkunan kiinnittäminen** seurata sen käyttöönoton ja sitten **Luo**.

## <a name="InstallCmdlets"></a>Vaihe 2: Asenna ja määritä PowerShellin Azure

1. Azure PowerShellin asentaminen. Ohjeita löytyy [tähän][powershell-install-configure].

    > [AZURE.NOTE] Voit myös vain kyselyille rakennetta, voit käyttää HDInsight's online rakenne-editorin. Voit tehdä kirjautuminen [Azure Portal][azure-portal], Valitse vasemmanpuoleisessa ruudussa voit tarkastella HDInsight-klustereiden **HDInsight** . Valitse haluat suorittaa rakenteen kyselyt klusterin ja valitse sitten **Kysely-konsolin**.

2. Avaa Azure PowerShell integroitu komentosarjat ympäristössä:
    - Tietokoneessa, jossa Windows 8: ssa tai Windows Server 2012 tai uudempi versio voit käyttää sisäinen haku. Aloitusnäytön Kirjoita **powershell ise:** ja valitse **Enter**.
    - Tietokoneessa, jossa versio kuin Windows 8: ssa tai Windows Server 2012 Käynnistä-valikon avulla. Kirjoita **komentokehotteeseen** hakuruutuun ja valitse hakutuloksista, valitse Käynnistä-valikosta **komentokehote**. Kirjoita komentokehotteeseen **powershell_ise** ja sitten **ENTER-näppäintä**.

3. Lisää Azure-tili.
    1. Kirjoita Console-ruudussa **Lisää AzureAccount** ja sitten **ENTER-näppäintä**.
    2. Kirjoita sähköpostiosoite ja Azure-tilaus ja valitse **Jatka**.
    3. Kirjoita salasana Azure tilauksen.
    4. Valitse **Kirjaudu sisään**.

4. Seuraavassa kaaviossa tunnistaa Azure PowerShell-komentosarjoja ympäristön tärkeät osat.

    ![Kaavion Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Vaihe 3: Suorita rakenteen työn DocumentDB ja Hdinsightista

> [AZURE.IMPORTANT] Kaikki muuttujat < > merkkinä on täytettävä käyttämällä määritykset.

1. Määritä seuraavat muuttujat PowerShell-komentosarjaa-ruudussa.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Aloitetaan luominen kyselymerkkijono. Olemme kirjoittaa rakenteen kyselyn, joka vie kaikki asiakirjat järjestelmä aikaleimat (_ts) ja yksilölliset tunnukset (_rid) DocumentDB kokoelmasta laskee yhteen kaikki asiakirjat minuutit mukaan, ja tallentaa tulokset takaisin DocumentDB-kokoelmaan.</p>

    <p>Ensin luodaan DocumentDB Microsoftin kokoelmasta rakennetaulukko. Lisää seuraavat koodikatkelman PowerShell-komentosarjaa ruudun <strong>jälkeen</strong> koodikatkelman # 1. Varmista, että voit lisätä valinnainen DocumentDB.query parametrin t Poista.välit Microsoftin vain _ts asiakirjoja ja _rid.</p>

    > [AZURE.NOTE]**Nimeäminen DocumentDB.inputCollections ei ollut virheen.** Kyllä, emme Salli lisäämällä useita sivustokokoelmat syötteeksi: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Seuraavaksi luodaan rakennetaulukko tulostus-kokoelmaan. Tulosteen tiedoston ominaisuuksia on kuukausi, päivä, tunti, minuutti ja suorituskertojen määrä.

    > [AZURE.NOTE]**Vielä uudelleen nimeäminen DocumentDB.outputCollections ei ollut virheen.** Kyllä, emme Salli useita sivustokokoelmat lisäämisellä tulos: </br>
"*DocumentDB.outputCollections*"="*\<DocumentDB tulosteen sivustokokoelman nimi 1\>*,*\<DocumentDB tulosteen sivustokokoelman nimi 2\>*" </br> Sivustokokoelman nimet on erotettu ilman välilyöntejä, käyttämällä vain yhden pilkku. </br></br>
Asiakirjojen on jaettu pyöreän pöydän yli useita sivustokokoelmat. Asiakirjojen erän tallennetaan yhden sivustokokoelman ja valitse toinen erän asiakirjat tallennetaan seuraavaan sivustokokoelman ja niin edelleen.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Lopuksi luodaan kirjata asiakirjoja kuukausi, päivä, tunti ja minuutti ja sijoittaa tulokset takaisin tulosteen rakennetaulukko.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Lisää seuraavat komentosarjan koodikatkelman, edellisen kyselyn rakenne työn määrityksen luominen.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Voit myös käyttää-tiedoston valitsin, joka määrittää HDFS HiveQL komentosarja-tiedosto.

6. Lisää seuraavat koodikatkelman alkamisajan Tallenna ja Lähetä rakenne-työ.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Lisää seuraava odottamaan rakenteen työn suorittamiseen.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Lisää seuraava vakio tulos ja alkamis- ja päättymisajat tulostamista.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Suorita** uusi komentosarja! **Valitse** vihreä Suorita-painiketta.

10. Tarkista tulokset. Kirjaudu sisään [Azure Portal][azure-portal].
    1. Valitse <strong>Selaa</strong> , valitse vasemmanpuoleisesta ruudusta. </br>
    2. Valitse <strong>kaikki</strong> sitten Selaa-ruudun oikeassa alkuun. </br>
    3. Etsi ja valitse <strong>DocumentDB tilit</strong>. </br>
    4. Etsi seuraavaksi <strong>DocumentDB tili</strong>, sitten <strong>DocumentDB tietokanta</strong> ja <strong>DocumentDB sivustokokoelman</strong> liittyvä tulostus-sivustokokoelman kyselyn rakenne.</br>
    5. Valitse lopuksi <strong>Asiakirjan Explorer</strong> <strong>Kehitystyökalut</strong>alapuolella.</br></p>

    Näet rakenne-kyselyn tulokset.

    ![Rakenne-kyselytulokset][image-hive-query-results]

## <a name="RunPig"></a>Vaihe 4: Suorita Possu työn DocumentDB ja Hdinsightista

> [AZURE.IMPORTANT] Kaikki muuttujat < > merkkinä on täytettävä käyttämällä määritykset.

1. Määritä seuraavat muuttujat PowerShell-komentosarjaa-ruudussa.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Aloitetaan luominen kyselymerkkijono. Olemme kirjoittaa Possu kyselyn, joka vie kaikki asiakirjat järjestelmä aikaleimat (_ts) ja yksilölliset tunnukset (_rid) DocumentDB kokoelmasta laskee yhteen kaikki asiakirjat minuutit mukaan, ja tallentaa tulokset takaisin DocumentDB-kokoelmaan.</p>
    <p>Lataa tiedostot ensin DocumentDB-Hdinsightista. Lisää seuraavat koodikatkelman PowerShell-komentosarjaa ruudun <strong>jälkeen</strong> koodikatkelman # 1. Varmista, että DocumentDB kyselyn lisääminen valinnainen DocumentDB Kyselyparametrin rajauskohta Microsoftin vain _ts asiakirjoja ja _rid.</p>

    > [AZURE.NOTE]Kyllä, emme Salli lisäämällä useita sivustokokoelmat syötteeksi: </br>
"*\<DocumentDB Input sivustokokoelman nimi 1\>*,*\<DocumentDB Input sivustokokoelman nimi 2\>*"</br> Sivustokokoelman nimet on erotettu ilman välilyöntejä, käyttämällä vain yhden pilkku. </b>

    Asiakirjojen on jaettu pyöreän pöydän yli useita sivustokokoelmat. Asiakirjojen erän tallennetaan yhden sivustokokoelman ja valitse toinen erän asiakirjat tallennetaan seuraavaan sivustokokoelman ja niin edelleen.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Seuraavaksi pelkän japanin asiakirjat kuukauden, päivän, tunti, minuutti ja suorituskertojen määrä.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Lopuksi japanin tulosten tallentaminen Microsoftin uusi tulostus-kokoelmaan.

    > [AZURE.NOTE]Kyllä, emme Salli useita sivustokokoelmat lisäämisellä tulos: </br>
"\<DocumentDB tulosteen sivustokokoelman nimi 1\>,\<DocumentDB tulosteen sivustokokoelman nimi 2\>"</br> Sivustokokoelman nimet on erotettu ilman välilyöntejä, käyttämällä vain yhden pilkku.</br>
Asiakirjojen on jaettu pyöreän pöydän yli useita sivustokokoelmat. Asiakirjojen erän tallennetaan yhden sivustokokoelman ja valitse toinen erän asiakirjat tallennetaan seuraavaan sivustokokoelman ja niin edelleen.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Lisää seuraavat komentosarjan koodikatkelman, Possu työn määrityksen luominen edellisen kyselyn.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Voit myös käyttää-tiedoston valitsin, joka määrittää HDFS Possu komentosarja-tiedosto.

6. Lisää seuraavat koodikatkelman alkamisajan Tallenna ja Lähetä Possu työn.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Lisää seuraava odottamaan Possu työn suorittamiseen.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Lisää seuraava vakio tulos ja alkamis- ja päättymisajat tulostamista.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Suorita** uusi komentosarja! **Valitse** vihreä Suorita-painiketta.

10. Tarkista tulokset. Kirjaudu sisään [Azure Portal][azure-portal].
    1. Valitse <strong>Selaa</strong> , valitse vasemmanpuoleisesta ruudusta. </br>
    2. Valitse <strong>kaikki</strong> sitten Selaa-ruudun oikeassa alkuun. </br>
    3. Etsi ja valitse <strong>DocumentDB tilit</strong>. </br>
    4. Etsi seuraavaksi <strong>DocumentDB tilin</strong>, valitse <strong>DocumentDB tietokanta</strong> ja <strong>DocumentDB sivustokokoelman</strong> liittyvän Possu kyselyn tulos sivustokokoelman.</br>
    5. Valitse lopuksi <strong>Asiakirjan Explorer</strong> <strong>Kehitystyökalut</strong>alapuolella.</br></p>

    Näet Possu kyselyn tulokset.

    ![Possu kyselytulokset][image-pig-query-results]

## <a name="RunMapReduce"></a>Vaihe 5: Suorita MapReduce työn DocumentDB ja Hdinsightista

1. Määritä seuraavat muuttujat PowerShell-komentosarjaa-ruudussa.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Olemme suorittaa MapReduce työn, joka laskee kunkin asiakirjan ominaisuus DocumentDB kokoelmasta suorituskertojen määrä. Lisää tämä komentosarja koodikatkelman **jälkeen** yllä koodikatkelman.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties v01.jar sisältyy DocumentDB Hadoop yhdistimen mukautettu asennus.

3. Lisää seuraava komento lähettää MapReduce työn.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Lisäksi MapReduce työ-määritys antaisit myös HDInsight-klusterin nimi kohtaa, johon haluat suorittaa MapReduce työn ja tunnistetiedot. Käynnistä-AzureHDInsightJob on käyttämiseen liittyvän synkronoimisen puhelu. Voit tarkistaa projektin päätyttyä, käyttämällä *Odota AzureHDInsightJob* cmdlet-komento.

4. Lisää seuraava komento Tarkista virheitä ja MapReduce suoritetaan.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Suorita** uusi komentosarja! **Valitse** vihreä Suorita-painiketta.

6. Tarkista tulokset. Kirjaudu sisään [Azure Portal][azure-portal].
    1. Valitse <strong>Selaa</strong> , valitse vasemmanpuoleisesta ruudusta.
    2. Valitse <strong>kaikki</strong> sitten Selaa-ruudun oikeassa alkuun.
    3. Etsi ja valitse <strong>DocumentDB tilit</strong>.
    4. Etsi seuraavaksi <strong>DocumentDB tili</strong>, sitten <strong>DocumentDB tietokanta</strong> ja <strong>DocumentDB sivustokokoelman</strong> liittyvä tulostus-sivustokokoelman MapReduce työn alla.
    5. Valitse lopuksi <strong>Asiakirjan Explorer</strong> <strong>Kehitystyökalut</strong>alapuolella.

    Näet tulokset MapReduce työtäsi.

    ![MapReduce kyselytulokset][image-mapreduce-query-results]

## <a name="NextSteps"></a>Seuraavat vaiheet

Onnittelen! Suoritit vain Azure DocumentDB ja HDInsight ensimmäisen rakenne, Possu ja MapReduce työt.

Olemme Avaa Orpotermit Microsoftin Hadoop-yhdistin. Jos haluat muuttaa, voit osallistua- [GitHub][documentdb-github].

Lisätietoja on seuraavissa artikkeleissa:

- [Kehittää Documentdb Java-sovellukseen][documentdb-java-application]
- [Kehitä Java MapReduce ohjelmien HDInsight Hadoop][hdinsight-develop-deploy-java-mapreduce]
- [Aloita käyttäminen HDInsight-rakenne Hadoop analysointia mobile luuri käyttöä varten][hdinsight-get-started]
- [MapReduce käyttäminen Hdinsightiin][hdinsight-use-mapreduce]
- [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
- [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
- [Mukauta HDInsight klustereiden komentosarja-toiminnon käyttäminen][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
