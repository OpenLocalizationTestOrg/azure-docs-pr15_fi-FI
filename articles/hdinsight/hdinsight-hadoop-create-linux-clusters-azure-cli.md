<properties
    pageTitle="Luo Hadoop, HBase tai myrsky klustereiden HDInsight käyttäminen Office kaikissa ympäristöissä Azure CLI Linux | Microsoft Azure"
    description="Opettele luomaan Linux-pohjaiset HDInsight varausyksiköiden Office kaikissa ympäristöissä Azure CLI, Azure Resurssienhallinta mallit ja Azure REST-Ohjelmointirajapinnalla. Voit määrittää klusterin tyyppi (Hadoop, HBase tai myrsky) tai komentosarjojen avulla voit asentaa mukautettujen osien..."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Linux-pohjaiset klustereiden luominen käyttämällä Azure CLI Hdinsightiin

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure-CLI on Office kaikissa ympäristöissä komentorivin apuohjelma, jonka avulla voit hallita Azure-palveluita. Se voidaan, sekä Azure resurssien hallinnan mallit-HDInsight-klusterin, sekä liittyvät tallennustilan tilien ja muiden palvelujen luomiseen.

Azure Resurssienhallinta mallit ovat JSON-tiedostoja, jotka kuvaavat __resurssiryhmä__ ja kaikki sen resurssit (esimerkiksi Hdinsightista.) Malliin perustuvan tämän menetelmän avulla voit määrittää kaikki resurssit, jotka tarvitset HDInsight yhteen malliin. Se myös avulla voit hallita ryhmän muutokset kokonaisuutena __käyttöönotoissa__, joissa muutoksia koko ryhmään kautta.

Ohjeita tämän asiakirjan käy läpi luoda uuden HDInsight-klusterin Azure-CLI ja mallin avulla.

> [AZURE.IMPORTANT] Ohjeita tämän asiakirjan käyttää HDInsight-klusterin oletusmäärän työntekijä solmujen (4). Jos aio yli 32 työntekijä solmujen (klusterin luonnin aikana tai skaalauksen klusterin), on valittava pään solmu kokoa, ja vähintään 8 sydämiä 14 gt RAM-muistia.
>
> Saat lisätietoja solmu kokojen ja niihin liittyvät kustannukset, [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. Ohjeita tämän asiakirjan on testattu viimeksi Azure CLI versio 0.10.1.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Kirjaudu sisään Azure-tilaukseen

[Yhdistä Azure tilaukseen Azure komentorivivalitsimet Interface (Azure CLI)-](../xplat-cli-connect.md) kuvattu noudattamalla ja Yhdistä-tilaukseen __Kirjautuminen__ -menetelmällä.

##<a name="create-a-cluster"></a>Klusterin luominen

Seuraavat vaiheet on tehtävä komentokehotteen, runko tai pääte istunnon asennuksesta ja määrityksestä Azure-CLI jälkeen.

1. Käytä seuraavaa komentoa tarkistamiseen Azure-tilaukseen:

        azure login

    Sinua kehotetaan annettava käyttäjänimi ja salasana. Jos sinulla on useita tilauksia Azure- `azure account set <subscriptionname>` , jotka käyttävät Azure CLI komentoja tilauksen määrittämiseen.

3. Siirry Azure Resurssienhallinta tilassa seuraava komento:

        azure config mode arm

4. Luo resurssiryhmä. Tämä resurssiryhmä liittyvistä tallennustilan tilin ja sisältää HDInsight-klusterin.

        azure group create groupname location
        
    * Korvaa __Ryhmän_nimi__ ryhmän yksilöllinen nimi. 
    * Korvaa maantieteellisen alueen, jonka haluat luoda ryhmän __sijainti__ . 
    
        Kelvollinen sijaintien luetteloon, käytä `azure location list` komento ja käytä sitten jokin sijainnit- __nimi__ -sarakkeesta.

5. Tallennustilan tilin luominen. Tallennustilan tilin käytetään HDInsight-klusterin oletusarvo-tallennustilan.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Korvaa __Ryhmän_nimi__ edellisessä vaiheessa luotu ryhmän nimi.
     * Korvaa __sijainti__ edellisessä vaiheessa käytetään samaan sijaintiin. 
     * Korvaa __storagename__ tallennustilan tilin yksilöllinen nimi.
     
     > [AZURE.NOTE] Lisätietoja käyttää tätä komentoa parametrit on käyttää `azure storage account create -h` voit tarkastella ohjeen tämän komennon.

5. Noutaa käyttää tallennustilan tilin.

        azure storage account keys list -g groupname storagename
        
    * Korvaa __Ryhmän_nimi__ resurssin ryhmän nimeä.
    * Korvaa __storagename__ tallennustilan tilin nimi.
    
    Palautetut tiedot tallennetaan __Avain1__ __avaimen__ arvo.

6. Luo HDInsight-klusterin.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Korvaa __Ryhmän_nimi__ resurssin ryhmän nimeä.

    * Korvaa __Hadoop__ klusterin tyyppi, johon haluat luoda. Esimerkiksi `Hadoop`, `HBase`, `Storm` tai `Spark`.

        > [AZURE.IMPORTANT] HDInsight klustereiden olla erilaisia tyypit, jotka vastaavat työmäärää tai tekniikka, joka on optimoitu klusterin. Ei ole tuettu menetelmä luo klusteriin, joka yhdistää useita tyyppejä, kuten myrsky ja HBase yhden klusterissa. 

    * Korvaa __sijainti__ edellisessä vaiheessa käytetään samaan sijaintiin.

    * Korvaa __storagename__ tallennustilan tilin nimi.

    * Korvaa edellisessä vaiheessa saatu avain __StorageKey-arvoa__ . 

    * N `--defaultStorageContainer` parametri, klusterin on käytössä sama nimi kuin voit käyttää.

    * Korvaa __järjestelmänvalvoja__ ja __httppassword__ nimi ja salasana, joita haluat käyttää, kun klusterin käyttämisen HTTPS.

    * Korvaa __sshuser__ ja __sshuserpassword__ käyttäjänimi ja salasana, jotka haluat käyttämällä SSH klusterin käytettäessä

    Voi kestää useita minuutteja klusterin luontiprosessi loppuun. Yleensä noin 15.

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut käyttämällä Azure CLI HDInsight-klusterin, avulla opit yhteyttä klusterin käsittelemisestä seuraavasti:

###<a name="hadoop-clusters"></a>Hadoop klustereiden

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase klustereiden

* [HBase HDInsight-käytön aloittaminen](hdinsight-hbase-tutorial-get-started-linux.md)
* [Kehittää Java HBase HDInsight-sovelluksia](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Myrsky klustereiden

* [Kehitä Java topologioissa myrsky HDInsight-](hdinsight-storm-develop-java-topology.md)
* [Myrsky HDInsight-Python osien käyttäminen](hdinsight-storm-develop-python-topology.md)
* [Ottaa käyttöön ja topologioissa myrsky HDInsight-ja seuranta](hdinsight-storm-deploy-monitor-topology-linux.md)
