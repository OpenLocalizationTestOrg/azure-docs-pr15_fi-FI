<properties
   pageTitle="Hadoop sika käyttää SSH-HDInsight-klusterin | Microsoftin Azure"
   description="Opi kuinka kytkeä Hadoop Linux-pohjainen klusteri SSH ja sitten Käytä sika sika latinalaisen lauseita suoritetaan vuorovaikutteisesti tai eräajona."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Sika-työt suoritetaan Linux-pohjainen klusteri sika-komennolla (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tässä asiakirjassa käydä läpi prosessi, jossa yhteyden muodostaminen Azure HDInsight Linux-pohjainen klusteri käyttämällä Secure Shell (SSH), suoritetaan vuorovaikutteisesti, sika latinalaisen lauseita sika-komennon avulla tai eräajona.

Ohjelmointikielen sikojen latinalaisen avulla voit kuvata muunnoksia, joita käytetään tuottamaan haluttuun siirtomuotoon syöttötiedot.

> [AZURE.NOTE] Jos ovat jo tuttuja Hadoop Linux-pohjaiset palvelimet käyttää, mutta uusia HDInsight on [Linux-pohjaiset HDInsight vinkkejä](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Edellytykset

Tämän artikkelin vaiheiden suorittamiseen tarvitset seuraavat.

* Linux-pohjainen HDInsight (Hadoop-HDInsight)-klusteriin.

* SSH-asiakasohjelma. Linux-, Unix- ja Mac OS olisi mukana SSH-asiakasohjelma. Windows-käyttäjien on ladattava asiakasohjelma, kuten [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Muodostaa yhteyden SSH

Muodostaa täydellinen toimialuenimi (FQDN) HDInsight-klusterin SSH-komennon avulla. FQDN on annoit sitten klusterin, **. azurehdinsight.net**. Esimerkiksi seuraavat muodostaa **myhdinsight**-nimisen klusterin.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

HDInsight-klusterin luonnin yhteydessä, **Voit antaa sertifikaatin avaimen SSH-todennusta varten** , saatat joutua yksityisen avaimen sijainnin määrittäminen asiakkaan-järjestelmään.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Jos antamasi salasanan todennus SSH** HDInsight-klusterin luonnin yhteydessä täytyy antaa salasana, kun sinua kehotetaan tekemään niin.

Lisätietoja SSH: n avulla HDInsight on [Käyttää SSH Hadoop Linux-pohjainen, Linux, OS X ja Unix-HDInsight kanssa](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Painovärit, muste (Windows-asiakkaat)

Windows ei tarjoa sisäänrakennettu SSH-asiakasohjelma. On suositeltavaa käyttää **painovärit, muste**, jonka voi ladata osoitteesta [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Lisätietoja painovärit, muste on [Käyttää SSH HDInsight Windows-Linux-pohjaiset Hadoop kanssa ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Sika-komennolla

2. Kun yhteys on muodostettu, seuraavan komennon avulla käynnistää sika command-line interface (CLI).

        pig

    Hetken kuluttua näkyviin tulee `grunt>` Kysy.

3. Kirjoita seuraava lause.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Tämä komento lataa LOKIT, sample.log-tiedoston sisältö. Voit tarkastella tiedoston sisältöä käyttämällä seuraavia.

        DUMP LOGS;

4. Seuraavaksi muuntaa tiedot soveltamalla säännöllinen lauseke purkaa kustakin tietueesta vain kirjaamistaso avulla seuraavasti.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **VEDOKSEN** avulla voit tarkastella tietoja muunnoksen jälkeen. Tässä tapauksessa `DUMP LEVELS;`.

5. Edelleen otetaan käyttöön muunnoksia käyttämällä seuraavia komentoja. Käytä `DUMP` voit tarkastella tuloksia lasketaan kunkin vaiheen jälkeen.

    <table>
    <tr>
    <th>Lause</th><th>Mitä se tekee</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = tasot SUODATTIMEN mukaan LOGLEVEL ei ole null.</td><td>Poistaa rivejä, jotka sisältävät null-arvon kirjauksen taso ja tallentaa tulokset yhdeksi FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = ryhmän FILTEREDLEVELS mukaan LOGLEVEL;</td><td>Ryhmittelee rivit lokitiedoston tason mukaan ja tallentaa tulokset GROUPEDLEVELS-järjestelmään.</td>
    </tr>
    <tr>
    <td>TAAJUUKSIEN = foreach GROUPEDLEVELS Luo ryhmä kuin LOGLEVEL-määrä (FILTEREDLEVELS. LOGLEVEL) kuin laskea;</td><td>Luo uusi joukko, joka sisältää kunkin lokin yksilöllinen arvo on ja kuinka monta kertaa se tapahtuu. Tallentaa ne TAAJUUDET.</td>
    </tr>
    <tr>
    <td>TULOS = tilauksen TAAJUUKSIEN määrä DESC;</td><td>Tilaukset lokitasot mukaan (laskeva) määrä ja tallentaa tulokseen.</td>
    </tr>
    </table>

6. Voit myös tallentaa muunnoksen tulokset käyttämällä `STORE` lause. Esimerkiksi seuraavat säästää `RESULT` varastointi Oletussäilö klusterisi **/example/data/pigout** -hakemistoon.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Tiedot on tallennettu **osa-nnnnn**järjestysluvut määritetystä kansiosta. Jos kansio on jo olemassa, näyttöön tulee virhe.

7. Lopeta grunt-kehote, kirjoita seuraava lause.

        QUIT;

###<a name="pig-latin-batch-files"></a>Sika latinalainen komentojonotiedostoja.

Sika-komennon avulla voit myös suorittaa sika latinalainen tiedostossa.

3. Kun poistut grunt-kehote, seuraavalla komennolla, STDIN-putken **pigbatch.pig**-tiedostoon. Tämä tiedosto luodaan kotikansio tilin olet kirjautunut varten SSH-istunto.

        cat > ~/pigbatch.pig

4. Kirjoita tai liitä seuraavat rivit ja käytä sitten lopuksi Ctrl + D.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Seuraavat avulla voit suorittaa **pigbatch.pig** -tiedoston sika-komennolla.

        pig ~/pigbatch.pig

    Kun eräajo on päättynyt, näyttöön pitäisi tulla seuraava tulos, joka olisi sama kuin jos käytit `DUMP RESULT;` edellisissä vaiheissa.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Yhteenveto

Kuten näet, sika-komennon avulla voit vuorovaikutteisesti MapReduce työvaiheiden sika latinalaisen avulla sekä suorittaa komentojonotiedoston tallennetut tiliotteet.

##<a id="nextsteps"></a>Seuraavat vaiheet

Sika HDInsight-yleistiedot

* [Hadoop HDInsight-sikaa käyttäminen](hdinsight-use-pig.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight.

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Käyttää MapReduce Hadoop HDInsight-](hdinsight-use-mapreduce.md)
