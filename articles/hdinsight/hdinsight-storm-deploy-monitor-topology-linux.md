<properties
   pageTitle="Ottaa käyttöön ja hallita Apache myrsky topologioissa Linux-pohjaiset HDInsight | Microsoft Azure"
   description="Opettele käyttöön, seurata ja hallita Apache myrsky topologioissa myrsky Raporttinäkymät-ikkunan käyttäminen Linux-pohjaiset Hdinsightista. Visual Studio Hadoop työkaluilla."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Käyttöönotto ja Apache myrsky topologioissa Linux-pohjaiset HDInsight-hallinta

Opi tässä asiakirjassa, hallintaan ja valvontaan myrsky topologioissa Linux-pohjaiset myrsky HDInsight klustereiden-käyttöjärjestelmässä.

> [AZURE.IMPORTANT] Tämän artikkelin vaiheet pätevät Linux-pohjaiset myrsky, HDInsight-klusterissa. Lisätietoja käyttöönotto ja seurantaa topologioissa-Windows-pohjaisesta Hdinsightista on artikkelissa [käyttöönotto ja hallita Apache myrsky topologioissa Windows-pohjaisesta HDInsight-](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Edellytykset

* **A Linux-pohjaiset myrsky HDInsight-klusterissa**: katso ohjeet klusterin luominen [Apache myrsky HDInsight-käytön aloittaminen](hdinsight-apache-storm-tutorial-get-started-linux.md)

* **Tuntemusta SSH ja SCP**: Katso lisätietoja SSH ja SCP käyttämisestä HDInsight seuraavasti:

    * **Linux, Unix tai OS X-asiakkaat**: Katso [Käytä SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X tai Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-asiakkaita**: [Käytä SSH kanssa Linux-pohjaiset Hadoop HDInsight Windows-](hdinsight-hadoop-linux-use-ssh-windows.md) kohdassa

* **SCP asiakkaan**: Tämä on annettu kaikki Linux, Unix ja OS X-järjestelmien kanssa. Windows-asiakkaat on suositeltavaa PSCP, joka on käytettävissä [painovärit, muste lataussivulle](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Käynnistä myrsky topologian

### <a name="using-ssh-and-the-storm-command"></a>SSH ja myrsky-komennolla

1. SSH avulla voit muodostaa yhteyden HDInsight-klusterin. Vaihda **käyttäjänimi** SSH kirjautumisnimi nimi. Korvaa **CLUSTERNAME** HDInsight-klusterinimi:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Lisätietoja HDInsight-klusterin muodostaminen SSH avulla Katso seuraavat asiakirjat:
    
    * **Linux, Unix tai OS X-asiakkaat**: Katso [Käytä SSH Linux-pohjaiset Hadoop HDInsight Linux, OS X tai Unix-kanssa](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-asiakkaita**: [Käytä SSH kanssa Linux-pohjaiset Hadoop HDInsight Windows-](hdinsight-hadoop-linux-use-ssh-windows.md) kohdassa

2. Seuraavalla komennolla voit käynnistää Esimerkki topologian:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Esimerkki WordCount topologian klusterin avautuu. Se luo lauseiden satunnaisesti ja esiintymiskertojen laskeminen virkkeet kunkin sanan.

    > [AZURE.NOTE] Lähetettäessä topologian klusteriin on ensin kopioitava sisältävä klusterin ennen kuin käytät purkki tiedosto `storm` komento. Tämä onnistuu käyttämällä `scp` komennon asiakaskoneelta, jossa tiedosto sijaitsee. Esimerkiksi`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Yhteyttä klusterin sisältyvät jo WordCount esimerkiksi ja myrsky starter muita esimerkkejä `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Ohjelmallisesti

Voit asentaa on verkkotopologia ohjelmallisesti myrsky HDInsight-toimittamalla ylläpidettävä yhteyttä klusterin Nimbus-palvelussa. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) on esimerkki Java-sovellus, jossa kerrotaan, miten käyttöön ja Käynnistä topologian Nimbus-palvelun kautta.

## <a name="monitor-and-manage-using-the-storm-command"></a>Seurata ja hallita myrsky-komennon avulla

`storm` Apuohjelman avulla voit käsitellä käynnissä topologioissa komentoriviltä. Seuraavassa on usein käytettävien komentojen luettelo. Käytä `storm -h` komennot täydellinen luettelo.

### <a name="list-topologies"></a>Luettelon topologioissa

Käytä seuraava komento Luetteloi kaikki käynnissä topologioissa:

    storm list
    
Tämä voi palauttaa tietoja seuraavankaltaiselta:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Aktivoinnin ja aktivoiminen uudelleen

Aktivoinnin poistamisesta on verkkotopologia keskeyttää se ennen kuin se on lopetettu tai aktivoida uudelleen. Aktivoinnin ja aktivoi uudelleen käyttämällä seuraavaa:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Lopettaa käynnissä topologian

Myrsky topologioissa, kerran aloittaminen on edelleen käytössä, kunnes pysäytetään. Voit lopettaa on verkkotopologia, käytä seuraavaa komentoa:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Saattaa jälleen tasapainoon

Järjestelmä tarkistaa topologian rinnakkaisuus yhdistelemällä on verkkotopologia avulla. Esimerkiksi jos klusterin voit lisätä Lisää muistiinpanoja muuttamisen yhdistelemällä sallii käynnissä topologian, jotta uusi solmujen avulla.

> [AZURE.WARNING] Yhdistelemällä on verkkotopologia ensin poistaa aktivoinnin topologian, sitten sisältää työntekijöiden tasaisesti klusterin yli sitten palauttaa lopuksi tilaan, jossa se oli ennen yhdistelemällä tapahtui topologian. Niin Jos topologian oli aktiivinen, se muuttuu aktiivinen uudelleen. Jos se on poistettu, se pysyy käytöstä.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Seurata ja hallita myrsky käyttöliittymässä

Myrsky-Käyttöliittymän tarjoaa web-käyttöliittymän käyttöä käynnissä topologioissa ja sisältyy HDInsight-klusterin. Voit tarkastella myrsky-Käyttöliittymä, käyttää selaimen avaamiseen __https://CLUSTERNAME.azurehdinsight.net/stormui__missä __CLUSTERNAME__ yhteyttä klusterin nimen.

> [AZURE.NOTE] Jos sinua pyydetään antamaan käyttäjänimi ja salasana, kirjoita klusterin järjestelmänvalvojaan (järjestelmänvalvojat) ja salasana, että voit käyttää, kun luot klusterin.


### <a name="main-page"></a>Pääsivun

Pääsivulta myrsky käyttöliittymän sisältää seuraavat tiedot:

* **Klusterin yhteenveto**: perustietoja myrsky-klusterin.

* **Topologian yhteenveto**: käynnissä topologioissa luettelo. Tässä osassa linkkien avulla voit tarkastella lisätietoja tietyn topologioissa.

* **Valvojan yhteenveto**: myrsky valvojan tietoja.

* **Nimbus määritys**: klusterin Nimbus määritykset.

### <a name="topology-summary"></a>Topologian yhteenveto

Linkin valitseminen **topologian yhteenveto** -osassa näkyvät topologian seuraavat tiedot:

* **Topologian yhteenveto**: perustietoja topologian.

* **Topologian toiminnot**: hallinnan toiminnot, jotka voit tehdä topologian.

  * **Aktivoi**: ansioluettelot käsittely käytöstä topologian.

  * **Poista aktivointi**: pysäyttää käynnissä topologian.

  * **Saattaa jälleen tasapainoon**: säätää topologian rinnakkaisuus. Sinun pitäisi saattaa jälleen tasapainoon käynnissä topologioissa sen jälkeen, kun olet muuttanut klusterin solmujen määrän. Näin topologian Säädä rinnakkaisuus korvaamaan klusterin solmut kasvaa tai pienentyä määrän.

    Lisätietoja on kohdassa <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">tietoa myrsky topologian rinnakkaisuus</a>.

  * **Lopettaa**: myrsky topologian päättyy määritetyn aikakatkaisun jälkeen.

* **Topologian tilasto**: topologian tilastoja. **Ikkuna** -sarakkeen linkkien avulla voit määrittää jäljellä olevat tapahtumat ajankohta sivulla.

* **Spouts**: topologian käyttämä spouts. Tässä osassa linkkien avulla voit tarkastella tietyn spouts lisätietoja.

* **Bolts**: topologian käyttämä Pultit. Tässä osassa linkkien avulla voit tarkastella tietyn Pultit lisätietoja.

* **Topologian määrittäminen**: valitun topologian määrittäminen.

### <a name="spout-and-bolt-summary"></a>Nokkaan ja lukko yhteenveto

Valitsemalla nokkaan **Spouts** tai **Bolts** osat näyttää valitun kohteen seuraavat tiedot:

* **Osan yhteenveto**: perustietoja nokkaan tai lukko.

* **Nokkaan/lukko tilasto**: nokkaan tai lukko tilastoja. **Ikkuna** -sarakkeen linkkien avulla voit määrittää jäljellä olevat tapahtumat ajankohta sivulla.

* **Syötteen tilasto** (vain lukita): laitteen käyttämä syötteen virtaa tietoja.

* **Tulosteen tilasto**: Tämä lähettämän virtaa tietoja spout tai lukita.

* **Pesänselvittäjät**: tietoja nokkaan tai lukko esiintymät. Valitse tietyn suorittaja, voit tarkastella valmistettu tässä esiintymässä vianmääritystiedot loki **portin** .

* **Virheet**: Tämä virhe mitään tietoja spout tai lukita.

## <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Myrsky-Käyttöliittymän rakentuu REST-Ohjelmointirajapinnalla, jotta voit suorittaa vastaavat hallintaan ja valvontaan toimintoja käyttämällä REST-Ohjelmointirajapinnalla. REST-Ohjelmointirajapinnalla avulla voit luoda mukautettuja työkaluja myrsky topologioissa seuranta ja hallinta.

Lisätietoja on artikkelissa [Myrsky Käyttöliittymän REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Seuraavassa on REST-Ohjelmointirajapinnalla käyttäminen Apache myrsky-Hdinsightista.

> [AZURE.IMPORTANT] Myrsky REST-Ohjelmointirajapinnalla ei ole yleisesti saatavilla Internetin välityksellä ja on voi käyttää SSH-tunnelia HDInsight-klusterin pään solmu. Lisätietoja luomisesta ja käyttämisestä SSH tunnelin on artikkelissa [Käytä SSH Tunneling käyttämään Ambari web-Käyttöliittymä, Resurssienhallinta, JobHistory, NameNode, Oozie, ja muut web-Käyttöliittymä päivän](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Base URI

Perus Linux-pohjaiset HDInsight klustereiden REST-Ohjelmointirajapinta-URI on käytettävissä pään solmu **https://HEADNODEFQDN:8744/api/v1/**; kuitenkin pään solmu toimialuenimi luodaan klusterin luonnin aikana ja ei ole staattinen.

Voit etsiä täydellinen toimialuenimi (FQDN) klusterin pään solmun usealla eri tavalla:

* __Valitse SSH istunnon__: komennolla `headnode -f` SSH-istunnosta klusteriin.

* __Ambari verkosta__: Valitse sivun yläreunasta __palvelut__ ja valitse sitten __myrsky__. Valitse __Myrsky Käyttöliittymän Server__ __Yhteenveto__ -välilehti. Solmun myrsky Käyttöliittymän ja REST API käynnissä olevaan FQDN on sivun yläreunassa.

* __Ambari REST-Ohjelmointirajapinnalla__: komennolla `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` hakemiseen solmu myrsky Käyttöliittymän ja REST API käynnissä olevat tiedot. Korvaa klusterin järjestelmänvalvojan salasanan __salasana__ . Korvaa __CLUSTERNAME__ klusterinimeä. Vastauksessa "host_name" tapahtuma sisältää solmun täydellinen toimialuenimi.

### <a name="authentication"></a>Todennus

REST API-pyynnöt käytettävä **perustodentamista**, niin voit käyttää HDInsight klusterin järjestelmänvalvojan käyttäjänimeä ja salasanaa.

> [AZURE.NOTE] Koska perustodentamista on lähetetty käyttämällä pelkkänä tekstinä, sinun tulee **aina** käyttöön HTTPS suojaamiseen klusterin tietoliikennetapahtumat.

### <a name="return-values"></a>Palautettavat arvot

Tietoja, joka palautetaan REST-Ohjelmointirajapinnan on ehkä vain käytettävä klusteriin tai samaan Azure Virtual verkossa kuin klusterin näennäiskoneiden. Esimerkiksi täydellinen toimialuenimi (FQDN) palautti Zookeeper palvelimia ei voi käyttää Internetissä.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut käyttöönotto ja seurata topologioissa myrsky Raporttinäkymät-ikkunan avulla, katso, miten voit [kehittää Java-pohjainen topologioissa käyttämällä maven-testi](hdinsight-storm-develop-java-topology.md).

Katso Lisää Esimerkki topologioissa luettelo on [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md).
