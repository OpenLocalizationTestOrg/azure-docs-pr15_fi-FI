<properties
   pageTitle="Apache myrsky ja HBase tunnistimen tietojen analysoiminen | Microsoft Azure"
   description="Opettele yhdistäminen Apache myrsky virtual verkoston kanssa. Käyttää myrsky HBase prosessin tunnistimen tietojen tapahtuma-toiminnosta ja visualisointi D3.js kanssa."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Apache myrsky, tapahtuma-toiminnosta ja HDInsight (Hadoop) HBase tunnistimen tietojen analysoiminen 

Opettele käyttämällä Apache myrsky HDInsight tunnistimen tietojen Azure tapahtuma-toiminnosta ja tallentaa kyselyjä Apache HBase HDInsight-visualisointi D3.js käynnissä Azure Web-sovelluksen avulla.

Azure Resurssienhallinta-malli, tässä asiakirjassa kerrotaan, miten voit luoda useita Azure resursseja resurssiryhmän. Tarkemmin sanottuna luomilleen Virtual Azure-verkkoon, kaksi HDInsight varausyksiköt (myrsky ja HBase) ja Azure Web App-sovelluksessa. Reaaliaikainen web Raporttinäkymät-ikkunan node.js soveltaminen otetaan käyttöön automaattisesti web App-sovellukseen.

> [AZURE.NOTE] Tämä asiakirja ja esimerkissä, tiedot on testattu Linux-pohjaiset HDInsight 3.3 ja 3,4 klusterin versiot.

## <a name="prerequisites"></a>Edellytykset

* Azure tilaus. Katso [Hae Azure maksuttoman kokeiluversion](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Et tarvitse aiemmin HDInsight-klusterin; ohjeita tämän asiakirjan Luo on seuraavissa resursseissa:
    >
    > * Azure Virtual verkossa
    > * HDInsight-klusterissa myrsky (Linux-pohjaiset, 2 työntekijän solmujen)
    > * HDInsight-klusterissa HBase (Linux-pohjaiset, 2 työntekijä solmujen)
    > * Azure Web App, joka isännöi web-koontinäyttö

* [Node.js](http://nodejs.org/): Voit esikatsella paikallisesti että kehitysympäristö-web-koontinäytön käytetään.

* [Java- ja JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): käytetään kehittämiseen myrsky topologian.

* [Maven-testi](http://maven.apache.org/what-is-maven.html): rakentamiseen ja kääntää projektin.

* [Git](http://git-scm.com/): lataa projektin GitHub avulla.

* __SSH__ asiakkaan: yhdistää Linux-pohjaiset HDInsight-klustereihin. Lisätietoja SSH käyttäminen Hdinsightista on artikkelissa seuraavat asiakirjat.

    * [SSH käyttäminen HDInsight Windows-asiakasohjelma](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [SSH käyttäminen HDInsight Linux-, Unix- tai Mac-asiakasohjelma](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] On myös on pääsy `scp` -komento, jolla voidaan kopioida tiedostoja paikallisen kehitysympäristö ja käyttämällä SSH HDInsight-klusterin välillä.

## <a name="architecture"></a>Arkkitehtuuri

![arkkitehtuurikaavio](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Tässä esimerkissä sisältää seuraavat osat:

* **Azure tapahtuman keskittimet**: sisältää tiedot, jotka on kerätty anturit. Tässä esimerkissä sovellus on, joka luo tiedot.

* **Myrsky HDInsight-**: tarjoaa reaaliaikainen tietojen käsittely tapahtuma-toiminnosta.

* **HBase HDInsight-**: tarjoaa jatkuva NoSQL tietosäilö tiedoille, kun se on käsitellyt myrsky.

* **Azure Virtual verkkopalvelu**: mahdollistaa suojatun myrsky HDInsight- ja HBase-HDInsight klustereiden välisten yhteyksien.

    > [AZURE.NOTE] Virtuaalinen verkon tarvitaan voi käyttää Java HBase asiakas-Ohjelmointirajapinta, kun se ei näy HBase klustereiden julkisen Gatewayn päälle. HBase ja myrsky klustereiden asentaminen samaan virtual verkostoon sallii myrsky klusterin (tai muita järjestelmän virtual verkossa) käyttää suoraan HBase asiakkaan Ohjelmointirajapinnan käyttäminen.

* **Raporttinäkymät-ikkunan sivuston**: esimerkki-koontinäyttö, kaaviot tietojen reaaliajassa.

    * Sivusto on toteutettu Node.js, niin, että se voidaan suorittaa testikäyttöön asiakkaan sellaisille käyttöjärjestelmille tai se voi ottaa käyttöön Azure sivustot.

    * [Socket.IO](http://socket.io/) käytetään reaaliaikaisen viestinnän myrsky topologiasta ja välillä.

        > [AZURE.NOTE] Tämä on käyttöönoton. Voit käyttää mitä tahansa communications framework, kuten raaka WebSockets tai SignalR.

    * [D3.js](http://d3js.org/) käytetään kaavion tiedot, jotka on lähetetty sivustoon.

> [AZURE.IMPORTANT] Kaksi klustereiden ovat tarvittaessa ei ole tuettu menetelmä luo yhden HDInsight-klusterin myrsky ja HBase.

Topologian lukee tietoja tapahtuma-toiminnosta [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) -luokan avulla ja kirjoittaa tietoja yhdeksi HBase [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) -luokan avulla. Viestintä sivustoa tehdään [socket.io client.java](https://github.com/nkzawa/socket.io-client.java)avulla.

Seuraavassa on esimerkkejä joistain muista topologian.

![topologian kaavio](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Tämä on hyvin yksinkertaistettu näkymä topologian. Suorituksen aikana kunkin komponentin esiintymä luodaan kunkin osion, joka on luettava tapahtumaa-toiminnossa. Nämä esiintymät jakautuu-klusterin solmut ja tietojen reititetään niiden välillä seuraavasti:
>
> * Tiedot nokkaan jäsennin on kuormitus tasataan.
> * Raporttinäkymät-ikkunan ja HBase jäsennin tiedot on ryhmitelty laitteen tunnus niin, että samaa komponenttia Toiminnonkulku aina viestejä samaan laitteeseen.

### <a name="topology-components"></a>Hakutopologian osien

* **EventHub nokkaan**: nokkaan on Apache myrsky versio 0.10.0 annettu tai uudempi versio.

    > [AZURE.NOTE] Tässä esimerkissä käytetään tapahtuman keskittimet nokkaan edellyttää myrsky HDInsight-klusterin version 3.3 tai 3.4. Lisätietoja tapahtuman keskittimet käyttäminen myrsky HDInsight-käyttöjärjestelmää aiempi versio on artikkelissa [prosessin tapahtumia tapahtuman Azure HDInsight-myrsky lihavoituna](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: tiedot, jotka nokkaan lähettämän on raaka JSON, ja toisinaan useita tapahtuma on lähetetty kerrallaan. Tämä lukko esitellään, miten nokkaan lähettämän tiedot lukea ja lähettää sen uuden muodossa kuin kyselyä, joka sisältää useita kenttiä.

* **DashboardBolt.java**: tämä esitellään, miten Java Socket.io asiakas-kirjaston avulla voit lähettää tiedot reaaliaikainen web Raporttinäkymät-ikkunan.

Tässä esimerkissä käytetään [valovirta](https://storm.apache.org/releases/0.10.0/flux.html) , framework, jotta topologian määritys sisältyy YAML tiedostoissa. On kaksi:

* __ei-hbase.yaml__ – Käytä tätä tiedostoa, kun testaaminen topologian kehittäminen-ympäristössä. Se ei käytä HBase osia, koska et voi käyttää HBase Java API-klusterin sijaitsevat virtual verkon ulkopuolelta.

* __hbase.yaml kanssa__ – Käytä tätä tiedostoa otettaessa topologian myrsky-klusterin. Tiedostossa käytetään HBase osien jälkeen, kun se suoritetaan HBase-klusterin virtual samassa verkossa.

## <a name="prepare-your-environment"></a>Lisätietoja ympäristön valmistelemisesta

Tämä esimerkki käyttää, sinun on luotava Azure tapahtuman keskittimeen, jossa lukee myrsky topologian.

### <a name="configure-event-hub"></a>Määritä tapahtumaa-toiminnossa

Tapahtuma-toiminnossa on tässä esimerkissä tietolähde. Seuraavien vaiheiden avulla voit luoda uuden tapahtuman-toiminnossa.

1. [Azure-portaaliin](https://portal.azure.com)ja valitse **+ Uusi** -> __Asioita Internet__ -> __Tapahtuman keskittimet__.

2. __Luo Namespace__ -sivu Suorita seuraavat toimet:

    1. Kirjoita __nimi__ nimitilan.
    2. Valitse hinnoittelutiedot taso. __Tavallinen__ on riittävä tässä esimerkissä.
    3. Valitse Azure __tilauksen__ käyttäminen.
    4. Valitse aiemmin luotu resurssiryhmä tai luoda uuden.
    5. Valitse __sijainti__ tapahtumaa-toiminnossa.
    6. Valitse __Kiinnitä Raporttinäkymät-ikkunan__ja valitse sitten __Luo__.

3. Kun sitten luontiprosessi on valmis, että nimitilan tapahtuman keskittimet-sivu tulee näkyviin. Tässä kohdassa Valitse __+ Lisää tapahtumaa-toiminnossa__. Valitse __Luo tapahtumaa-toiminnossa__ , sivu nimi __sensordata__ ja valitse sitten __Luo__. Jättää muut kentät, oletusarvot.

4. Valitse __Tapahtuma keskittimet__oman nimitilan tapahtuman keskittimet-sivu. Valitsemalla __sensordata__ .

5. Valitse __jaettu käytäntöjen__sensordata tapahtumaa-toiminnossa, sivu. __+ Lisää__ linkin avulla voit lisätä käytettävissään seuraavat käytännöt:


  	| Käytännön nimi | Vaateita koskevat rajoitukset |
  	| ----- | ----- |
  	| laitteet | Lähetä |
  	| myrsky | Kuuntele |

5. Valitse molemmat käytännöistä ja huomioi __PERUSAVAIMEN__ arvoa. Sinun on arvo tulevien vaiheet sekä käytännöt.

## <a name="download-and-configure-the-project"></a>Lataa ja Määritä projektin

Lataa projektin GitHub seuraavat avulla.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Kun komento on valmis, sinun on seuraava kansiorakenne:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Tässä asiakirjassa ei siirry tässä esimerkissä;-koodin tarkat tiedot koodi on kuitenkin täysin Kommentoitu.

Avaa tiedosto **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** ja tapahtumaa-toiminnossa tietojen lisääminen seuraavista riveistä:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] Tässä esimerkissä oletetaan, että olet käyttänyt __myrsky__ käytännön, joka on __kuunnella__ ryhmän nimen ja että tapahtumaa-toiminnossa nimi on __sensordata__.

 Tallenna tiedosto, kun olet lisännyt nämä tiedot.

## <a name="compile-and-test-locally"></a>Käännä ja testaa paikallisesti

Ennen testaus, sinun on käynnistettävä Raporttinäkymät-ikkunan tarkasteleminen topologian tulosteen ja luo tietojen tallentamiseen tapahtumaa-toiminnossa.

> [AZURE.IMPORTANT] Tämä topologian HBase-osa ei ole aktiivinen, kun testausta paikallisesti HBase-klusterin Java-Ohjelmointirajapinnan eivät voi käyttää Azure Virtual verkossa, joka sisältää varausyksiköiden ulkopuolella.

### <a name="start-the-web-application"></a>Web-sovelluksen käynnistäminen

1. Avaa uusi komentokehote tai terminaalissa ja muuttaa kansioiden **hdinsight-eventhub-Esimerkki/Raporttinäkymät-ikkunan**ja sitten seuraavalla komennolla riippuvuudet tarvitsemia web-sovelluksen asentaminen:

        npm install

2. Käytä seuraavaa komentoa web-sovelluksen käyttöön:

        node server.js

    Pitäisi näkyä viesti, joka on seuraavanlainen:

        Server listening at port 3000

2. Avaa selain ja kirjoita **http://localhost:3000 /** osoitteeksi. Näyttöön tulee sivu, joka on seuraavankaltaiselta:

    ![Web-koontinäyttö](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Jätä tämä komentorivi tai päätteen auki. Testauksen jälkeen Lopeta verkkopalvelin Ctrl + C avulla.

### <a name="start-generating-data"></a>Aloita tietojen luotaessa

> [AZURE.NOTE] Tämän osan vaiheet Käytä Node.js, jotta niitä voi käyttää missä tahansa ympäristössä. Katso kielen muita esimerkkejä **SendEvents** -kansio.

1. Avaa uusi komentokehote, runko tai terminaalissa ja muuttaa kansioiden **hdinsight-eventhub-Esimerkki/SendEvents/nodejs**ja sitten seuraavalla komennolla voit asentaa sovelluksen tarvitsemat riippuvuudet:

        npm install

2. Avaa **app.js** tiedostoa tekstieditorissa ja Lisää aiemmin hankit tapahtumaa-toiminnossa koskevat tiedot:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] Tässä esimerkissä oletetaan, että __sensordata__ ovat käyttäneet tapahtumaa-toiminnossa ja __laitteiden__ nimeksi käytännön, joka on __Lähetä__ ryhmän nimen.

2. Käytä seuraavaa komentoa lisätäksesi uusia merkintöjä tapahtumaa-toiminnossa:

        node app.js

    Raportissa pitäisi näkyä tulosteen useita rivejä, jotka sisältävät tiedot lähetetään tapahtumaa-toiminnossa. Nämä näkyy seuraavankaltaiselta:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Käynnistä topologian

2. Avaa uusi komentokehote, runko tai pääte ja muuta __Hdinsight-eventhub-Esimerkki/TemperatureMonitor__hakemistoja ja Käynnistä topologian seuraava komento avulla:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Jos käytössäsi on PowerShell-käyttää seuraavaa sijaan:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Jos ovat Linux/Unix/OS X-järjestelmän ja on [asennettu myrsky kehittäminen-ympäristössä](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), voit käyttää seuraavia komentoja:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Tämä käynnistää topologian määritetty paikallisen tilassa __ei hbase.yaml__ -tiedostossa. Arvot __dev.properties__ -tiedostossa on tapahtuman keskittimet yhteystiedot. Kun alkanut, topologian lukee tapahtumat tapahtuma-toiminnosta ja lähettää ne paikallisessa tietokoneessa käynnissä Raporttinäkymät-ikkunan. Näyttöön tulee rivit näkyvät web-koontinäyttö, seuraavankaltaiselta:

    ![Raporttinäkymä, jossa on tietoja](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Koontinäytön ollessa käynnissä, käytä `node app.js` komennon lähettää uusia tietoja tapahtuman porttiin edellä kuvatut vaiheet. Lämpötila arvot satunnaisesti luodaan, koska kaavio on päivitettävä suuri muutosten näyttäminen lämpötilan.

    > [AZURE.NOTE] Sinun on oltava __Hdinsight-eventhub-Esimerkki/SendEvents/Nodejs__ hakemistossa käytettäessä `node app.js` komento.

3. Kun olet varmistanut, että tämä toimii, Lopeta topologian käyttämällä näppäinyhdistelmää Ctrl + C. Voit lopettaa paikallisen verkkopalvelin myös Ctrl + C.

## <a name="create-a-storm-and-hbase-cluster"></a>Luo myrsky ja HBase klusteri

Jotta voit suorittaa topologian HDInsight ja HBase lukko käyttöön, sinun on luotava uusi myrsky klusterin ja HBase klusterin. Tämän osan vaiheet Luo uusi Azure Virtual verkon ja myrsky ja HBase klusterin virtual verkon käyttöön [Azure Resurssienhallinta-mallin](../resource-group-template-deploy.md) avulla. Mallin myös Luo Azure Web App-sovelluksessa ja ottaa käyttöön koontinäyttö kopio siihen.

> [AZURE.NOTE] Virtuaalinen verkon käytetään niin, että käytössä myrsky klusterin topologian voivat suoraan viestiä HBase klusterin HBase Java-Ohjelmointirajapinnan käyttäminen.

Tässä asiakirjassa Resurssienhallinta malli sijaitsee julkisen blob-säilö osoitteessa __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Valitse Seuraava painike Azure kirjautuminen ja avaa Resurssienhallinta-malli Azure-portaalissa.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Valitse **Parametrit** -sivu Anna seuraavat tiedot:

    ![HDInsight-parametrit](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: tätä arvoa käytetään myrsky perus nimeksi ja HBase klusterit. Esimerkiksi kirjoittaminen __hdi__ Luo nimetty __myrsky hdi__ myrsky klusterin ja nimeltä __hbase hdi__HBase-klusterin.
    * __CLUSTERLOGINUSERNAME__: myrsky ja HBase klustereiden järjestelmänvalvojan käyttäjänimi.
    * __CLUSTERLOGINPASSWORD__: myrsky ja HBase klustereiden järjestelmänvalvojan salasanaa.
    * __SSHUSERNAME__: SSH käyttäjän myrsky ja HBase klustereiden luomiseen.
    * __SSHPASSWORD__: myrsky ja HBase klustereiden SSH käyttäjän salasanan.
    * __Sijainti__: alue, joka varausyksiköiden luodaan.
    
    Valitse __OK__ , jos haluat tallentaa parametrit.
    
3. __Resurssiryhmä__ -osan avulla voit luoda uusi resurssiryhmä tai valitse aiemmin luotu.

4. Valitse __resurssin ryhmän sijainti__ avattavasta valikosta tekemäsi valinnan __sijainnin__ parametrin samaan sijaintiin.

5. Valitse __ehdot__ja valitse sitten __Luo__.

6. Lopuksi __raporttinäkymät-ikkunan kiinnittäminen__ ja valitse __Luo__. Luo varausyksiköiden kestää noin 20 minuutin ajan.

Kun resursseja on luotu, sinut ohjataan resurssiryhmän klustereiden ja web Raporttinäkymät-ikkunan sisältävä sivu.

![Resurssien ryhmä-sivu vnet ja klustereiden](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Huomaa, että HDInsight-klustereiden nimet on __Myrsky BASENAME__ ja __hbase BASENAME__BASENAME on antamasi mallin nimi. Seuraavat nimet käyttäminen myöhemmin vaiheet varausyksiköiden yhdistettäessä. Huomaa, että dashboard-sivuston nimi on __basename-Raporttinäkymät-ikkunan__. Käytät tätä myöhemmin tarkasteltaessa koontinäyttö.

## <a name="configure-the-dashboard-bolt"></a>Määritä Raporttinäkymät-ikkunan lukko

Tietojen lähettäminen käyttöön kuin verkkosovellukseen Raporttinäkymät-ikkunan, sinun on muokattava seuraava rivi __dev.properties__ tiedoston:

    dashboard.uri: http://localhost:3000

Muuta `http://localhost:3000` , `http://BASENAME-dashboard.azurewebsites.net` ja Tallenna tiedosto. Korvaa __BASENAME__ pääkansion nimi, jonka annoit edellisessä vaiheessa. Voit myös tarkastella URL-osoite ja valitse koontinäyttö aiemmin luotu resurssiryhmä.

## <a name="create-the-hbase-table"></a>HBase-taulukon luominen

Jos haluat tallentaa tiedot HBase, emme on luotava taulukon. Yleensä haluat luoda valmiiksi resurssien avulla myrsky täytyy kirjoittaa, kuin yrität luoda sisällä myrsky topologian resursseja voi johtaa koodin yritit luoda samalle resurssille useita, eri aikajaksoille kopioita. Resurssien ulkopuolella topologian luominen ja käyttäminen vain lukeminen ja kirjoittaminen ja analytics myrsky.

1. SSH avulla voit muodostaa yhteyden HBase klusterin SSH käyttäjän ja salasanan antamasi mallin klusterin luonnin aikana. Jos yhteyden muodostaminen esimerkiksi `ssh` komennon, voit käyttää seuraavaa syntaksia:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    Tämä komento korvaa __käyttäjänimi__ SSH käyttäjänimi antamasi luotaessa klusterin ja __BASENAME__ antamasi pääkansion nimi. Kirjoita pyydettäessä SSH käyttäjän salasana.

2. Aloita HBase-liittymän SSH istunnosta.

        hbase shell
    
    Kun käyttöliittymä on ladattu, näet `hbase(main):001:0>` kehote.

3. HBase-shell-Kirjoita seuraava komento luo taulukko tunnistimen tietojen tallennusta varten:

        create 'SensorData', 'cf'

4. Varmista, että taulukossa on luotu käyttämällä seuraava komento:

        scan 'SensorData'
        
    Tämä palauttaa tiedot samalla tavalla kuin seuraavassa esimerkissä, joka ilmaisee, että taulukossa on 0 riviä.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Kirjoita Lopeta HBase-liittymän seuraavasti:

        exit

## <a name="configure-the-hbase-bolt"></a>Määritä HBase lukko

Kirjoittaa HBase myrsky klusterin, sinun on määritettävä HBase lukko HBase-klusterin määritysten tiedot. Helpoin tapa on Lataa __hbase site.xml__ klusterin ja lisätä sen projektin. On myös kommentointi useita __pom.xml__ -tiedosto, joka lataa myrsky hbase osan ja tarvittavat riippuvuudet riippuvuudet.

> [AZURE.IMPORTANT] Sinun on ladattava myös oman myrsky HDInsight-klusterin 3.3 tai 3.4 klusterissa; annettujen myrsky hbase.jar-tiedosto Tässä versiossa on käännetty toimimaan HBase 1.1.x, jota käytetään HBase HDInsight 3.3 ja 3,4 klustereiden. Jos käytät myrsky hbase osan muualta, se voi kääntää vastaan HBase vanhempi versio.

### <a name="download-the-hbase-sitexml"></a>Lataa hbase-site.xml

Komentoriviltä __hbase site.xml__ -tiedoston lataaminen klusterin SCP avulla. Seuraavassa esimerkissä tilalle luotaessa klusterin ja __BASENAME__ perus-nimisen voit edellä annettu SSH käyttäjän __käyttäjänimi__ . Kirjoita pyydettäessä SSH käyttäjän salasana. Korvaa `/path/to/TemperatureMonitor/resources/hbase-site.xml` TemperatureMonitor projektin tämän tiedoston polku.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Tämä Lataa __hbase site.xml__ polkua.

### <a name="download-and-install-the-storm-hbase-component"></a>Lataa ja asenna myrsky hbase osa

1. Komentoriviltä __myrsky hbase.jar__ -tiedoston lataaminen myrsky klusterin SCP avulla. Seuraavassa esimerkissä tilalle luotaessa klusterin ja __BASENAME__ perus-nimisen voit edellä annettu SSH käyttäjän __käyttäjänimi__ . Kirjoita pyydettäessä SSH käyttäjän salasana.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Tämä Lataa tiedosto nimeltä `storm-hbase-####.jar`, jossa ### on tämän klusterin myrsky versionumero. Muistiin tämän numeron, kun sitä käytetään myöhemmin.

2. Seuraava komento avulla voit asentaa tämän osan paikallisen maven-testi säilöön, kehitys-ympäristöön. Näin löydät paketin projektin muodostettaessa maven-testi. Korvaa __####__ ja tiedostonimi sisältää versionumero.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Jos käytössäsi on PowerShell, kirjoita seuraava komento:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Projektin myrsky hbase-komponentin ottaminen käyttöön

1. Avaa __TemperatureMonitor/pom.xml__ -tiedosto ja poista seuraavat rivit:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Poistaa vain nämä kaksi riviä; Älä poista mitään rivien välissä.
    
    Ottaa käyttöön useita osia, joita tarvitaan HBase käyttämällä hbase-laitteen kanssa kommunikoitaessa.

2. Seuraavat rivit Etsi ja korvaa __####__ kanssa myrsky hbase-tiedoston aiemmin lataamaasi versionumero.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Versionumero on vastattava versio, jolla on asennettaessa osan siirtäminen paikalliseen maven-testi-säilöön, kuten maven-testi laskee näiden tietojen lataaminen osa projektia luotaessa.

2. Tallenna tiedosto __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Muodosta-paketin ja ota käyttöön ratkaisun Hdinsightiin

Kehittäminen ympäristössäsi myrsky topologian käyttöön myrsky klusterin seuraavien vaiheiden avulla.

1. __TemperatureMonitor__ -hakemistosta Käytä seuraavaa komentoa suorittamiseen uusi versio ja projektin PURKKI paketin luominen:

        mvn clean compile package

    Tämä luo tiedosto nimeltä **TemperatureMonitor 1.0-SNAPSHOT.jar** **kohdekansion projektin** .

2. __TemperatureMonitor 1.0-SNAPSHOT.jar__ -tiedoston lataaminen myrsky-klusterin scp avulla. Seuraavassa esimerkissä tilalle luotaessa klusterin ja __BASENAME__ perus-nimisen voit edellä annettu SSH käyttäjän __käyttäjänimi__ . Kirjoita pyydettäessä SSH käyttäjän salasana.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Useita minuutteja Lataa tiedosto, voi kestää, kun se on useita megatavua.

    Käyttää scp __dev.properties__ -tiedoston lataaminen sisältää tietoja, joita käytetään tapahtuman keskittimet ja raporttinäkymiä.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Kun ladatut tiedostot, muodosta yhteys käyttämällä SSH klusterin.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. SSH istunnosta seuraavalla komennolla voit käynnistää topologian.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Tämä käynnistää topologian __hbase.yaml kanssa__ tiedoston topologian määritelmän ja määritys-arvojen avulla __dev.properties__ -tiedostossa.

3. Kun topologian on jo alkanut, Avaa selaimessa sivustoon, voit julkaista Azure, käytä `node app.js` komento tietojen lähettäminen tapahtumaa-toiminnossa. Raportissa pitäisi näkyä web koontinäytön Päivitä tietojen näyttämiseen.

    ![Raporttinäkymät-ikkunan](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase tietojen tarkasteleminen

Kun olet lähettänyt topologian käyttämään tietoja `node app.js`, seuraavien vaiheiden avulla voit muodostaa yhteyden HBase ja varmista, että tiedot on kirjoitettu aiemmin luomasi taulukon.

1. SSH avulla voit muodostaa yhteyden HBase-klusterin.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Aloita HBase-liittymän SSH istunnosta.

        hbase shell
    
    Kun käyttöliittymä on ladattu, näet `hbase(main):001:0>` kehote.

2. Näytä taulukon rivit:

        scan 'SensorData'
        
    Tämä tulee palauttaa tietoja, seuraavan kaltainen, joka ilmaisee, että taulukossa on 0 riviä.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Tarkistustoiminto palauttavat vain enintään 10 riviä taulukosta.

## <a name="delete-your-clusters"></a>Poista oman klustereiden

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Klustereiden, tallennustilan ja web App-sovelluksen poistaminen samalla kertaa, poista resurssiryhmän, joka sisältää ne.

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt oppinut myrsky avulla voit lukea tietoja tapahtuman keskittimet ja tallentaa kyselyjä HBase visualisointi ulkoisen Raporttinäkymät-ikkunan tiedot käyttämällä Socket.io ja D3.js.

* Katso Lisää esimerkkejä myrsky topologioissa HDInsight kanssa:

    * [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md)

* Lisätietoja Apache myrsky on [Apache myrsky](https://storm.incubator.apache.org/) -sivustossa.

* Lisätietoja HBase HDInsight-kohdassa [HBase HDInsight-yleiskuvauksessa](hdinsight-hbase-overview.md).

* Lisätietoja Socket.io on [socket.io](http://socket.io/) -sivustossa.

* Saat lisätietoja D3.js [D3.js - perustuvat asiakirjat](http://d3js.org/).

* Lisätietoja luominen topologioissa Java-artikkelissa [kehittää Java topologioissa Apache myrsky HDInsight-varten](hdinsight-storm-develop-java-topology.md).

* Saat lisätietoja luomisesta topologioissa .NET [topologioissa C kehittää # varten Apache myrsky Visual Studiossa HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
