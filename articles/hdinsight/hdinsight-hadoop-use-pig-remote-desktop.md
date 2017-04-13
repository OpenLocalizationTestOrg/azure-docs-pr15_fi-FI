<properties
   pageTitle="Hadoop Possu käyttäminen Etätyöpöytä HDInsight | Microsoft Azure"
   description="Opettele suorittamaan Possu latinalainen lauseet Etätyöpöytäyhteys Windows-pohjaisesta Hadoop-klusterin HDInsight-Possu-komennolla."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Possu työt pitäminen Etätyöpöytäyhteys

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Tässä tiedostossa on vaiheittainen Possu latinalainen lauseet pitäminen Etätyöpöytäyhteys Windows-pohjaisesta HDInsight-klusterin Possu-komennon avulla. Possu latinalainen voit MapReduce sovellusten luominen kuvaava tietojen muunnokset sijaan yhdistää ja vähentää funktiot.

Tässä asiakirjassa kerrotaan, miten

##<a id="prereq"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavasti.

* Windows-pohjaisesta HDInsight (Hadoop-HDInsight)-klusterin

* Asiakastietokone, Windows 10: ssä, Windows 8: ssa tai Windows 7: ssä

##<a id="connect"></a>Etätyöpöytä yhteydessä

Etätyöpöytä käyttöön HDInsight-klusterin ja muodostaa siihen voidaan [muodostaa käyttämällä RDP HDInsight klustereihin](hdinsight-administer-use-management-portal.md#rdp)ohjeita noudattamalla.

##<a id="pig"></a>Possu-komennolla

2. Kun olet Etätyöpöytäyhteys tai aloittaa **Hadoop-komentoa** käyttämällä työpöydällä olevaa kuvaketta.

2. Aloita Possu-komentoa käyttämällä seuraavaa:

        %pig_home%\bin\pig

    Voit valita jommankumman kanssa `grunt>` kehote.

3. Kirjoita seuraava komento:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Tämä komento lataa sample.log-tiedoston sisällön LOKIT-tiedostoon. Voit tarkastella tiedoston sisällön käyttämällä seuraava komento:

        DUMP LOGS;

4. Muuntaa tiedot käyttämällä vain kirjaamistaso noutamiseen kunkin tietueen säännöllinen lauseke:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **VEDOSTA** avulla voit tarkastella tietoja muunnoksen jälkeen. Tässä tapauksessa `DUMP LEVELS;`.

5. Jatka käyttöä muunnoksia seuraavista väittämistä avulla. Käytä `DUMP` haluat katsella Tulosta muunnoksen jokaisen vaiheen jälkeen.

    <table>
    <tr>
    <th>Lause</th><th>Kuvaus</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = SUODATTIMEN tasot mukaan LOGLEVEL ei ole null.</td><td>Poistaa rivit, jotka sisältävät null-arvon log tason ja tallentaa tulokset FILTEREDLEVELS kyselyjä.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = ryhmän FILTEREDLEVELS LOGLEVEL; mukaan</td><td>Ryhmittelee rivit log tason mukaan ja tallentaa tulokset GROUPEDLEVELS kyselyjä.</td>
    </tr>
    <tr>
    <td>TAAJUUDET = foreach GROUPEDLEVELS Luo ryhmä LOGLEVEL, Laske (FILTEREDLEVELS. LOGLEVEL), kun laskeminen;</td><td>Luo uusi joukko, joka sisältää kunkin yksilöllinen lokin arvo ja kuinka monta kertaa se tapahtuu. Tämä on tallennettu TIHEYDET tuominen</td>
    </tr>
    <tr>
    <td>TULOS = tilauksen TIHEYDET Laske DESC;</td><td>Log-tasot tilaukset (laskeva) määrän mukaan ja tallentaa tulokseen</td>
    </tr>
    </table>

6. Voit myös tallentaa muunnoksen tulokset käyttämällä `STORE` lause. Esimerkiksi seuraava komento tallentaa `RESULT` yhteyttä klusterin oletusarvo-tallennustilan säiliön **/example/data/pigout** hakemistoon:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Tiedot tallennetaan järjestysluvut **osa nnnnn**määritettyyn kansioon. Jos kansio on jo luotu, näyttöön tulee virhesanoma.

7. Lopeta grunt kehote, kirjoita seuraava-lause.

        QUIT;

###<a name="pig-latin-batch-files"></a>Possu latinalainen erä tiedostot

Possu-komennon avulla voit myös suorittaa Possu latinalainen, jotka sisältyvät tiedoston.

3. Sen grunt kehotteen sulkemisen jälkeen Avaa **Muistio** ja luo uusi tiedosto nimeltä **pigbatch.pig** **PIG_HOME %** -kansiossa.

4. Kirjoita tai liitä seuraavat rivit **pigbatch.pig** -tiedosto ja tallenna se:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Seuraavat avulla voit suorittaa **pigbatch.pig** tiedoston possu-komennon avulla.

        pig %PIG_HOME%\pigbatch.pig

    Kun erätyö on valmis, pitäisi näkyä seuraava tulos, joka on oltava sama kuin kun käyttänyt `DUMP RESULT;` aiemmissa vaiheissa:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Yhteenveto

Kuten näet, Possu-komennon avulla voit vuorovaikutteisesti suorittaa MapReduce toimintoja, tai Possu latinalainen työt, jotka on tallennettu komentojonotiedosto.

##<a id="nextsteps"></a>Seuraavat vaiheet

Yleisiä tietoja HDInsight Possu:

* [Possu käyttäminen Hadoop-Hdinsightiin](hdinsight-use-pig.md)

Lisätietoja muista tavoista voit käsitellä Hadoop-HDInsight:

* [Hadoop HDInsight-rakenteen käyttäminen](hdinsight-use-hive.md)

* [Hadoop-HDInsight MapReduce käyttäminen](hdinsight-use-mapreduce.md)
