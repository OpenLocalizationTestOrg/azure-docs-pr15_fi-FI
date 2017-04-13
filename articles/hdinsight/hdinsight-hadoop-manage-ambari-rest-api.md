<properties
   pageTitle="Seurata ja hallita HDInsight klustereiden Apache Ambari REST-Ohjelmointirajapinnan käyttäminen | Microsoft Azure"
   description="Opettele Ambari avulla voit seurata ja hallita Linux-pohjaiset HDInsight klustereiden. Tässä asiakirjassa kerrotaan käyttämisestä Ambari REST-Ohjelmointirajapinnalla HDInsight klustereiden mukana."
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

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Toteutetun HDInsight klustereiden Ambari REST-Ohjelmointirajapinnalla

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari yksinkertaistaa hallintaa ja antamalla helposti web-Käyttöliittymä ja REST API Hadoop-klusterin seuranta. Ambari sisältyy Linux-pohjaiset HDInsight klustereiden ja käytetään klusterin ja tekemällä määritysmuutoksia. Tässä asiakirjassa kerrotaan Ambari REST-ohjelmointirajapinnalla suorittamalla yleisiä tehtäviä avulla kääntö käytön perusteet.

##<a name="prerequisites"></a>Edellytykset

* [cURL](http://curl.haxx.se/): kääntö on Office kaikissa ympäristöissä-apuohjelma, jonka avulla voidaan käsitellä REST API komentorivillä. Tässä asiakirjassa sitä käytetään vaihtamaan Ambari REST-ohjelmointirajapinnalla.
* [jq](https://stedolan.github.io/jq/): jq on tarkoitettu JSON-tiedostojen käyttäminen Office kaikissa ympäristöissä komentorivin apuohjelma. Tässä asiakirjassa sitä käytetään jäsentää Ambari REST-Ohjelmointirajapinnalla palauttamat JSON-tiedostoja.
* [Azure CLI](../xplat-cli-install.md): Office kaikissa ympäristöissä komentoriviltä apuohjelma Azure palvelujen käyttöä.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Mikä on Ambari?

[Apache Ambari](http://ambari.apache.org) tekee Hadoop hallinta yksinkertaisempi antamalla helposti käytettävällä luodun sivuston Käyttöliittymän, jonka avulla voidaan valmistella, hallita ja seurata Hadoop klustereiden. Kehittäjät integroida näiden ominaisuuksien verkkosovelluksistaan [Ambari REST API](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)avulla.

Ambari annetaan Linux-pohjaiset HDInsight klustereiden oletusarvoisesti.

##<a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Perus Ambari REST-Ohjelmointirajapinnalla HDInsight-URI on https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, missä __CLUSTERNAME__ yhteyttä klusterin nimen.

> [AZURE.IMPORTANT] Klusterinimeä URI (CLUSTERNAME.azurehdinsight.net) täydellinen toimialueen nimi (FQDN)-osassa ollessa asennustavan muut esiintymät URI kirjainkoko on merkitsevä. Esimerkiksi jos yhteyttä klusterin nimi on MyCluster, seuraavassa on kelvollinen URI:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Seuraavat URI palauttaa virheen, koska toinen esiintymän nimi ei ole oikein.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Yhteyden muodostaminen Ambari HDInsight-edellyttää HTTPS. Kun todennustapa yhteyden, sinun on käytettävä järjestelmänvalvojan tilin nimi (oletus on __järjestelmänvalvoja__) ja salasana, jotka on annettu klusterin luonnin yhteydessä.

Seuraavassa esimerkissä kääntö tekemään vastaan REST-Ohjelmointirajapinnalla GET-pyynnössä. Korvaa klusterin järjestelmänvalvojan salasanan __salasana__ . Korvaa __CLUSTERNAME__ klusterin nimi:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Vastaus on JSON-asiakirja, joka alkaa tiedot seuraavankaltaiselta:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Tämä on JSON, ne on helpompi JSON-jäsentimen avulla voit käsitellä tietoja. Kuten seuraavassa esimerkissä käytetään jq näytettävä vain `health_report` elementti.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Esimerkki: Hae klusterin FQDN

Kun käsittelet HDInsight, joudut ehkä tietää klusterin solmun täydellinen toimialuenimi (FQDN). Voit helposti noutaa FQDN eri solmujen klusterin avulla seuraavasti:

* __Päätä solmujen__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Työntekijän solmujen__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper solmujen__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Huomaa, että kukin näistä esimerkeistä seurata samoissa:

1. Osa, joka on tiedettävä kysely suoritetaan näiden solmuissa.
2. Hae `host_name` elementtejä, jotka sisältävät nämä solmujen täydellinen toimialuenimi.

`host_components` Palautuksen tiedoston osa sisältää useita kohteita. Käyttämällä `.host_components[]`, ja määrittämällä sitten liikerata elementissä käy läpi kunkin kohteen ja tuoda esiin polkua olevaa arvoa. Jos haluat vain yhden arvon, kuten FQDN ensimmäiseen voit palauttaa kokoelman kohteet ja valitse sitten tiettyä kohdetta:

    jq '[.host_components[].HostRoles.host_name][0]'

Tämä palauttaa ensimmäisen FQDN kokoelmasta.

##<a name="example-get-the-default-storage-account-and-container"></a>Esimerkki: Hanki säilö ja tallennustilaa oletustilin

Kun luot HDInsight-klusterin, sinun on käytettävä Azure-tallennustilan tilin ja blob-säilö kuin oletusarvoinen tallennustilan klusterin. Ambari avulla voit hakea näitä tietoja klusterin luomisen jälkeen. Esimerkiksi jos haluat ohjelmallisesti kirjoittaa tiedot suoraan säilö.

Seuraavat noutaa WASB URI klustereiden oletusarvon tallennustilaa:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Tämä palauttaa käytetty palvelimeen ensimmäisen määritys (`service_config_version=1`,) joka sisältää tiedot. Jos haet arvo, joka on muokattu klusterin luonnin jälkeen, voit joutua luettelon määritys-versiot ja hakea uusimman.

Tämä palauttaa arvon samalla tavalla kuin seuraavassa esimerkissä, jossa __SÄILÖ__ on oletusarvo-säilö ja __ACCOUNTNAME__ on Azure-tallennustilan tilin nimi:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Voit käyttää sitten nämä tiedot ja [Azure CLI](../xplat-cli-install.md) tietojen lataaminen säilöstä.

1. Resurssiryhmä-tallennustilan tilin hakeminen Korvaa __ACCOUNTNAME__ noutaa Ambari tallennustilan tilin nimi:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Palauttaa arvon tilin resurssiryhmän nimi.
    
    > [AZURE.NOTE] Jos mitään tämän komennon, joudut ehkä Azure-CLI Siirry Azure Resurssienhallinta-tilassa ja suorita-komento uudelleen. Siirry Azure Resurssienhallinta tilassa seuraavalla komennolla:
    >
    > `azure config mode arm`
    
2. Hae tallennustilan tilin avain. Korvaa __RYHMÄN_NIMI__ resurssiryhmä-edellisessä vaiheessa. Vaihda __ACCOUNTNAME__ tallennustilan tilin nimi.

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Tässä esimerkissä palauttaa tilin perusavain.
    
3. Lataa-komennon avulla voit tallentaa tiedoston säilö:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Korvaa __ACCOUNTNAME__ tallennustilan tilin nimi. Korvaa __ACCOUNTKEY__ noudettu aiemmin avain. __TIEDOSTOPOLKU__ on ladattava __BLOBPATH__ ollessa säilössä polku tiedoston polku.

    Esimerkiksi jos haluat näkyvän HDInsight osoitteessa wasbs://example/data/filename.txt tiedoston, valitse __BLOBPATH__ olisi `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Esimerkki: Päivitä Ambari määritys

1. Hae nykyiset määritykset, jotka Ambari tallentaa "uudet määritykset":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Tässä esimerkissä palauttaa klusterin asennettuihin osien JSON asiakirja, joka sisältää nykyiset määritykset (merkittyä _tunniste_ -arvo). Seuraavassa esimerkissä on ote Ohjattu klusterin tyyppi palauttamia tietoja.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Tästä luettelosta haluat kopioida osan nimeä (esimerkiksi __Ohjattu\_thrift\_sparkconf__ ja __tunniste__ -arvo.
    
2. Noutaa osan ja tunnisteen määrityskohde seuraavat-komennon avulla. Korvaa __Ohjattu thrift-sparkconf__ ja __ensimmäisen__ osan ja -tunniste, jonka haluat hakea määrityskohde.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Kääntö hakee JSON-asiakirja ja valitse jq käytetään tehdä muutoksia tietoihin, jotta he voivat luoda mallin. Mallin käytetään sitten arvojen lisääminen tai muokkaaminen. Erityisesti toimii seuraavasti:
    
    * Luo yksilöllinen arvo, joka sisältää merkkijonon "version" ja päivämäärän, joka on tallennettu __newtag__.
    * Luo uusi uudet määritykset pääkansion asiakirjan.
    * Saa sisällön `.items[]` array ja lisää sen __desired_config__ osan-kohdassa.
    * Poistaa __Hyperlinkkiviittaus__, __versio__ja __Config__ osat näitä elementtejä ei ole tarpeen lähettää uudet asetukset.
    * Lisää uusi __merkintä__ -elementin ja sen arvoksi asetetaan __versio ###__. Numero-osa perustuu nykyisen päivämäärän. Kunkin määritys on oltava yksilöllinen tunniste.
    
    Lopuksi tiedot tallentuvat __newconfig.json__ asiakirjaan. Asiakirjan rakenteen pitäisi näkyä samalla tavalla kuin seuraavassa esimerkissä:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Avaa __Ominaisuudet__ -objektin __newconfig.json__ asiakirjan ja muokata ja lisää arvot. Seuraavassa esimerkissä muuttuu arvosta __"spark.yarn.am.memory"__ __"1 g"__ __"3 g"__ ja, ja Lisää uusi osa, jonka arvo on __"256 m"__ __"spark.kryoserializer.buffer.max"__ .

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Tallenna tiedosto, kun olet tehnyt haluamasi muutokset.

4. Käytä seuraavaa komentoa lähettää Ambari päivitetyt määritykset.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Tämä komento piiput kääntö pyynnön, joka lähettää sen klusterin uusi uudet määritykset kuin __newconfig.json__ -tiedoston sisällön. Kääntö pyynnön palauttaa JSON asiakirjan. Tämän asiakirjan __versionTag__ elementti on vastattava lähettämäsi versio ja __kokoonpanomääritysten yhteydessä__ objekti sisältää pyydetty määritysmuutoksia.

###<a name="example-restart-a-service-component"></a>Esimerkki: Käynnistä service-osa

Tässä vaiheessa Jos tarkastelet Ambari web-Käyttöliittymä, ohjattu palvelun ilmaisee, että se on käynnistettävä uudelleen, ennen kuin uudet asetukset tulevat voimaan. Seuraavien vaiheiden avulla voit käynnistää-palvelun.

1. Ylläpidon tila palvelun ohjattu käyttöön käyttää seuraavasti:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Tämä komento lähettää JSON asiakirjan palvelimeen (sisältyvät `echo` -lauseke eli) joka Ylläpitotila otetaan käyttöön.
    Voit tarkistaa, että palvelu on nyt ylläpito-tilassa käyttämällä seuraavia:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Tämä arvo palauttaa `"ON"`.

3. Käytä seuraavaksi palvelun käytöstä seuraavasti:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Tämä komento palauttaa vastauksen seuraavankaltaiselta.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    `href` Tämän URI palauttama arvo on käytössä klusterin solmun sisäinen IP-osoite. Voit käyttää sitä klusterin ulkopuolella, korvaa "10.0.0.18:8080" osa klusterin FQDN. Esimerkiksi seuraava komento hakee pyynnön tilaa.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Jos tämä arvo palauttaa `"COMPLETED"` sitten kutsu on valmis.

4. Kun edelliseen pyyntö on valmis, seuraavat avulla voit käynnistää sen.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Palvelu käynnistyy, kun se on uusia määrityksiä.

5. Lopuksi Ylläpitotilan käytöstä seuraavat avulla.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Seuraavat vaiheet

Katso täydellinen viittaus REST API- [Ambari API viittaus V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Ambari joitakin toimintoja ole käytössä, eikä se hallitsee HDInsight; pilvipalvelussa esimerkiksi lisäämällä tai poistamalla isännät klusterista tai uusia palveluita lisätään.
