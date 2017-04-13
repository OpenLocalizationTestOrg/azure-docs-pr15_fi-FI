<properties
   pageTitle="Hadoop käyttövihjeitä Linux-pohjaiset HDInsight | Microsoft Azure"
   description="Ohjatun Linux-pohjaiset HDInsight (Hadoop) klustereiden tuttuja Linux-ympäristössä käytössä Azure pilveen käyttöönoton vihjeitä."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Lisätietoja HDInsight Linux

Linux-pohjaiset Azure Hdinsightiin klustereiden Anna Hadoop tutussa Linux-ympäristössä, käynnissä Azure pilveen. Useimmat, saat sen pitäisi toimia täsmälleen samalla tavalla kuin Hadoop-Linux-asennuksen. Tämän asiakirjan puhelujen erot, sinun olisi otettava huomioon ulos.

##<a name="prerequisites"></a>Edellytykset

Monissa tämän asiakirjan ohjeita käytetään seuraavat apuohjelmat, jotka on ehkä asennettava järjestelmään.

* [cURL](https://curl.haxx.se/) - web-pohjaisten palvelujen yhteydessä käytettävä
* [jq](https://stedolan.github.io/jq/) - käytettävä jäsentää JSON asiakirjat
* [Azure CLI](../xplat-cli-install.md) - etähallinta Azure-palveluiden avulla

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Toimialueiden nimet

Täydellinen toimialuenimi (FQDN) käyttämään muodostettaessa yhteyttä klusterin Internetistä on ** &lt;clustername >. azurehdinsight.net** tai (SSH) ** &lt;clustername-ssh >. azurehdinsight.net**.

Sisäisesti-klusterin kunkin solmun on nimi, joka on määritetty klusterin määritysten aikana. Voit etsiä klusterin nimet, vieraile __isännät__ -sivun Ambari Web-Käyttöliittymän tai käyttää seuraavaa palauttamaan-isäntien luettelon Ambari REST-Ohjelmointirajapinnalla:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Korvaa __salasana__ salasana järjestelmänvalvojan tililtä ja __CLUSTERNAME__ yhteyttä klusterin nimi. Tämä palauttaa JSON-asiakirja, jossa on klusterin isännät luettelo ja valitse jq hakee `host_name` kunkin host klusterin arvo elementti.

Jos haluat etsiä tietyn palvelun solmun nimen, voit tehdä kyselyn Ambari osaa. Esimerkiksi löytääksesi isännät HDFS nimi solmun tehdä seuraavia asioita.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Tämä palauttaa kuvaava palvelun JSON asiakirjan ja sitten jq hakee vain `host_name` isäntien arvo.

## <a name="remote-access-to-services"></a>Etäkäyttö-palveluihin

* **Ambari (Internet)** - https://&lt;clustername >. azurehdinsight.net

    Klusterin järjestelmänvalvoja ja salasanasi avulla todennetaan ja kirjaudu sitten Ambari. Tämä käyttää klusterin järjestelmänvalvoja ja salasana.

    Todennus on tekstimuotoisten - Käytä aina HTTPS, jos haluat varmistaa, että yhteys on suojattu.

    > [AZURE.IMPORTANT] Kun Ambari yhteyttä klusterin on käytettävissä suoraan Internetin kautta, joitakin toimintoja riippuvainen käyttäminen solmujen klusterin käyttämän sisäisen toimialueen nimi. Tämä on sisäisen toimialueen nimi ja ei ole julkinen, näyttöön tulee "palvelin ei löydy" virheitä, kun yrität käyttää joitakin ominaisuuksia Internetin välityksellä.
    >
    > Käyttää kaikkia ominaisuuksia Ambari web-Käyttöliittymä, käytä SSH-tunnelia välityspalvelimen web-liikenne paikalliseen klusterin pään solmu. [Käytä SSH Tunneling käyttämään Ambari web-Käyttöliittymä, Resurssienhallinta, JobHistory, NameNode, Oozie, ja muut web-Käyttöliittymä on](hdinsight-linux-ambari-ssh-tunnel.md) artikkelissa

* **Ambari (REST)** - https://&lt;clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Todentaa klusterin järjestelmänvalvoja ja salasanasi avulla.
    >
    > Todennus on tekstimuotoisten - Käytä aina HTTPS, jos haluat varmistaa, että yhteys on suojattu.

* **WebHCat (Templeton)** - https://&lt;clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Todentaa klusterin järjestelmänvalvoja ja salasanasi avulla.
    >
    > Todennus on tekstimuotoisten - Käytä aina HTTPS, jos haluat varmistaa, että yhteys on suojattu.

* **SSH** - &lt;clustername >-ssh.azurehdinsight.net porttiin 22 ja 23. Porttia 22 käytetään yhteyden muodostamiseen ensisijainen headnode, kun 23 käytetään yhteyden muodostamiseen toissijaisen. Katso lisätietoja pään solmuissa, [käytettävyys ja luotettavuutta Hadoop varausyksiköt Hdinsightista](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Voit käyttää vain klusterin pään solmujen kautta SSH asiakastietokoneessa. Kun yhteys on muodostettu, voit käyttää työntekijä solmut sitten käyttämällä SSH headnode.

## <a name="file-locations"></a>Oletuskansiot

Hadoop liittyvät tiedostot löytyvät klusterin solmuissa `/usr/hdp`. Tämä kansio sisältää seuraavat alikansiot:

* __2.2.4.9-1__: kansion nimi on käyttämä HDInsight, joten yhteyttä klusterin numeron voi olla eri kuin tässä luettelossa Hortonworks tietojen Platform-versiota.
* __nykyisen__: Tämä kansio on linkkejä __2.2.4.9-1__ kansion kansiot ja siten, että sinun ei tarvitse kirjoittaa versionumero (jotka voivat muuttua) on aina, kun haluat käyttää tiedostoa.

Esimerkkitietoja ja PURKKI tiedostoja löytyy Hadoop Distributed (HDFS)-järjestelmän tai Azure Blob storage "/ Esimerkki ' tai ' wasbs: / / / esimerkki".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS Azure-Blob-säiliö ja tallennustilaa parhaat käytännöt

Useimmat Hadoop jaot HDFS on palautettua klusterin tietokoneissa paikallinen tallennus. Samalla, kun kyseessä on tehokasta, voi olla kallista pilvipohjainen ratkaisun kohtaa, johon olet peritään kerran tunnissa tai minuuttien Laske resurssien mukaan.

HDInsight käyttää Azure-Blob-säiliö oletusarvo-kaupasta, joka sisältää seuraavat edut:

* Halpaa pitkään tallennustila

* Helppokäyttötoiminnot-ulkoisten palvelujen, kuten sivustot, lataa ja lataa tiedosto-apuohjelmien, eri kielellä SDK: T ja selaimet

Koska HDInsight oletusarvo-kaupasta, voit tavallisesti ei tarvitse tehdä mitään sitä käytetään. Esimerkiksi seuraava komento sisältyviä **/example/data** -kansioon, joka on tallennettu Azure-Blob-säiliö tiedostot:

    hdfs dfs -ls /example/data

Jotkut komennot saattaa edellyttää, että voit määrittää, että käytät Blob-objektien tallennustilaan. Näiden, voit kirjoittaa komento **wasb: / /**, tai **wasbs: / /**.

HDInsight avulla voit yhdistää usean Blob storage tilin klusterin. Muun kuin oletusarvoisen Blob storage tilille tietojen käyttämistä varten, voit käyttää muodossa **wasbs: / /&lt;container-name>@&lt;tilin nimi >.blob.core.windows.net/**. Seuraavassa on esimerkiksi luettelo määritetyssä säilössä ja Blob storage tilin **/example/data** -kansion sisältö:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Mitä Blob-objektien tallennustilaan klusterin on käytössä?

Klusterin luonnin aikana valittuna olevan Azure-tallennustilan tilin ja säilö tai luoda uuden. Valitse olet luultavasti unohtanut se. Voit etsiä säilö ja tallennustilaa oletustilin Ambari REST-Ohjelmointirajapinnalla käyttämällä.

1. Käytä seuraavaa komentoa käyttämällä kääntö HDFS määritysten tietojen hakeminen ja suodattamista [jq](https://stedolan.github.io/jq/)avulla:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Tämä palauttaa käytetty palvelimeen ensimmäisen määritys (`service_config_version=1`,) joka sisältää tiedot. Jos haet arvo, joka on muokattu klusterin luonnin jälkeen, voit joutua luettelon määritys-versiot ja hakea uusimman.

    Tämä palauttaa arvon seuraavanlainen, jossa __SÄILÖ__ on oletusarvo-säilö ja __ACCOUNTNAME__ on Azure-tallennustilan tilin nimi:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Halutun resurssiryhmän tallennustilan tilin, voit käyttää [Azure CLI](../xplat-cli-install.md). Korvaa __ACCOUNTNAME__ noutaa Ambari tallennustilan tilin nimi seuraava komento:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Tämä palauttaa tilin resurssiryhmän nimi.
    
    > [AZURE.NOTE] Jos mitään tämän komennon, joudut ehkä Azure-CLI Siirry Azure Resurssienhallinta-tilassa ja suorita-komento uudelleen. Siirry Azure Resurssienhallinta-tilassa, käytä seuraavaa komentoa.
    >
    > `azure config mode arm`
    
2. Hae tallennustilan tilin avain. Korvaa __RYHMÄN_NIMI__ resurssiryhmä-edellisessä vaiheessa. Vaihda __ACCOUNTNAME__ tallennustilan tilin nimi.

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Tämä palauttaa tilin perusavain.

Löytyvät myös Azure-portaalissa tallennustilan tiedot:

1. Valitse [Azure Portal](https://portal.azure.com/)HDInsight-klusterin.

2. Valitse __kaikki asetukset__ __Essentials__ -osassa.

3. Valitse __asetukset__-kohdasta __Azure-tallennustilan avaimet__.

4. Valitse __Azure-tallennustilan näppäimet__-luettelossa tallennustilan tunnuksilla. Tässä näkyvät tallennustilan tilin tiedot.

5. Valitse avaimen kuvake. Tässä näkyvät tallennustilan tilin avaimet.

### <a name="how-do-i-access-blob-storage"></a>Miten voin käyttää Blob-objektien tallennustilaan?

Muu kuin klusterista Hadoop-komennon kautta on erilaisia tapoja käyttää BLOB:

* [Mac-, Linux-ja Windows Azure CLI](../xplat-cli-install.md): käyttöliittymä komennot Azure käsittelyyn. Kun asennus on valmis, käytä `azure storage` käyttämisestä tallennus-komento tai `azure blob` blob ominaisten komentojen.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python komentosarjan käsittelyyn BLOB Azuren tallennustilaan.

* SDK: T monenlaisia:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Tallennustilan REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Yhteyttä klusterin skaalaus

Klusterin skaalaus ominaisuuden avulla voit muuttaa klusteriin, joka toimii Azure Hdinsightiin eikä sinun tarvitse poistaa ja luo uudelleen klusterin käyttämät tiedot solmujen määrän.

Voit suorittaa skaalauksen toimet, kun muut työt tai prosessit ovat käynnissä klusterissa.

Eri klusterin tyypit vaikuttavat skaalaus seuraavasti:

* __Hadoop__: kun skaalaus-klusterin solmujen määrän alaspäin-klusterin palvelut uudelleen. Tämä voi aiheuttaa työt käyttöön tai odottaa epäonnistuu skaalauksen toiminnon päätyttyä. Työt voidaan tiedot, kun toiminto on valmis.

* __HBase__: alueellisen palvelimet ovat automaattisesti saapuva skaalauksen toiminnon suorittamisen muutaman minuutin kuluessa. Jos saldo alueellisen palvelimet manuaalisesti, käytä seuraavasti:

    1. Yhdistä käyttäen SSH HDInsight-klusterin. Lisätietoja SSH käyttämisestä HDInsight-kohdassa jokin seuraavista asiakirjoista:

        * [SSH käyttäminen HDInsight Linux, Unix ja Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [SSH käyttäminen Windows Hdinsightiin](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Aloita HBase-liittymän käyttämällä seuraavaa:

            hbase shell

    2. Kun HBase-liittymän on ladattu, avulla voit tasata manuaalisesti alueellisen palvelimet seuraavasti:

            balancer

* __Myrsky__: sinulla olisi saattaa jälleen tasapainoon kaikki käynnissä olevat myrsky topologioissa jälkeen skaalauksen toiminto on suoritettu. Näin voit muuttaa rinnakkaisuus asetukset-klusterin solmut uusi numero topologian. Saattaa jälleen tasapainoon käynnissä topologioissa, käyttämällä jotakin seuraavista vaihtoehdoista:

    * __SSH__: muodostaa yhteyttä palvelimeen ja saattaa jälleen tasapainoon on verkkotopologia seuraava komento avulla:

            storm rebalance TOPOLOGYNAME

        Voit myös määrittää parametrit korvaavat alun perin myöntämä topologian rinnakkaisuus vihjeet. Esimerkiksi `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` uudelleen topologian 5 työntekijä prosesseja, sininen nokkaan osan 3 pesänselvittäjät ja 10 pesänselvittäjät keltainen lukko-osan.

    * __Myrsky Käyttöliittymän__: saattaa jälleen tasapainoon topologian, myrsky käyttöliittymässä seuraavien vaiheiden avulla.

        1. Avaa selaimessa, missä CLUSTERNAME myrsky-klusterin nimen __https://CLUSTERNAME.azurehdinsight.net/stormui__ . Pyydettäessä Kirjoita HDInsight klusterin (järjestelmänvalvojat) järjestelmänvalvojan nimi ja salasana klusterin luotaessa määritetty.

        3. Valitse Haluatko saattaa jälleen tasapainoon topologiasta ja __saattaa jälleen tasapainoon__ -painiketta. Määritä viive ennen rebalance toiminto suoritetaan.

Lisätietoja HDInsight-klusterin Skaalaus-kohdassa:

* [Azure-portaalissa voit hallita Hadoop varausyksiköt Hdinsightiin](hdinsight-administer-use-portal-linux.md#scaling)

* [Hallitse Hadoop varausyksiköt HDinsight Azure PowerShell-toiminnolla](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Miten voin asentaa sävy (tai muun Hadoop-osa)?

Hdinsightista on hallitun palvelu, mikä tarkoittaa, että klusterin solmujen saa hävitetään ja valmistella uudelleen automaattisesti Azure Jos havaitaan ongelma. Tästä syystä ole suositellaan manuaalinen asennus kohteita suoraan-klusterisolmut. Käytä sen sijaan [HDInsight komentosarjatoiminnot](hdinsight-hadoop-customize-cluster.md) , kun sinun on asennettava seuraavasti:

* Palvelun tai web-sivusto, kuten ohjattu tai sävy.
* Osa, joka edellyttää useita solmuissa klusterin määritysmuutoksia. Ympäristömuuttuja esimerkiksi tarvittavat, kirjaaminen hakemiston tai määritystiedoston luominen luominen.

Komentosarjatoiminnot ovat Bash komentosarjoja, jotka suorituksen aikana klusteri valmisteleminen ja avulla voidaan asentaa ja määrittää lisäosat klusterin. Esimerkki komentosarjojen toimitetaan asentamiseen seuraavat osat:

* [Sävy](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Lisätietoja kehittämisestä komentosarja-toiminnot on artikkelissa [kehitys HDInsight komentosarja-toiminnon](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>JAR tiedostot

Kokeile Hadoop-toimintoja toimitetaan itsenäinen purkki tiedostot, jotka sisältävät osana MapReduce työ- tai -funktioissa Possu tai rakenteen sisällä. Nämä voidaan asentaa komentosarja-toimintojen käyttäminen, kun ne usein ei edellytä ja vain voidaan ladata klusterin valmistelun jälkeen ja käyttää suoraan. Jos haluat, että osa SOPIMUSKOHDASSA reimaging klusterin makle, voit tallentaa tiedoston purkki WASB.

Esimerkiksi jos haluat käyttää [DataFu](http://datafu.incubator.apache.org/)uusimman version, voit ladata purkki, joka sisältää projektin ja lataa se HDInsight-klusterin. Noudata DataFu ohjeista käyttämisestä Possu tai rakenne.

> [AZURE.IMPORTANT] Joitakin osia, joka on erillinen purkki tiedostoja HDInsight mukana, mutta ne eivät ole polun. Jos haluat löytää tietyn osan, voit etsiä sen yhteyttä klusterin seurannan:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Tämä palauttaa vastaavia purkki tiedostoja polun.

Jos klusterin on jo osan erillinen purkki-tiedostona, mutta haluat käyttää eri versiota, voit ladata osan uusi versio klusterin ja yritä käyttäminen työt.

> [AZURE.WARNING] Osien HDInsight-klusterin mukana tuetaan täysin ja Microsoft Support auttavat eristämään ja ratkaista ongelmat, jotka liittyvät komponentit.
>
> Mukautettujen osien saa järkevän tukea helpottavat edelleen ongelman vianmäärityksen. Tämä saattaa aiheuttaa ratkaisemiseksi tai sinulta kysytään, haluatko osallistuminen käytettävissä olevat kanavat Avaa lähde-tekniikoiden laaja osaamisalueet, tekniikkaa löytyi. Esimerkiksi ovat yhteisön sivustoja, joita voidaan käyttää, kuten: [HDInsight MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Myös Apache projektien on projektisivustojen [http://apache.org](http://apache.org), esimerkiksi: [Hadoop](http://hadoop.apache.org/), [Ohjattu](http://spark.apache.org/).

## <a name="next-steps"></a>Seuraavat vaiheet

* [Siirtää Windows-pohjaisesta HDInsight Linux-pohjaiset](hdinsight-migrate-from-windows-to-linux.md)
* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)
* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
* [MapReduce työt käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)
