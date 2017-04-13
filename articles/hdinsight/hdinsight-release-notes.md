<properties
    pageTitle="Julkaisutiedot Hadoop osista Azure Hdinsightiin | Microsoft Azure"
    description="Uusimman julkaisutiedot ja Azure Hdinsightiin Hadoop osia versiot. Saat Hadoop Apache myrsky ja HBase kehittämiseen liittyviä vinkkejä ja tiedot."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Hadoop osien Azure Hdinsightiin julkaisutiedot

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Muistiinpanojen R HDInsight-palvelimelle 10/26/2016-versiossa

- R palvelimelle HDInsight-klusterin valmistelu on suunniteltu siten.
- HDInsight R-palvelimeen on nyt käytettävissä säännöllisesti HDInsight "R Server-klusterin tyyppi, ja se asennetaan enää erillisessä HDInsight-sovelluksessa. Reuna-solmu ja R palvelimen binaaritiedostot nyt valmisteltu R Server-klusterikäyttöönoton osana. Tämä parantaa nopeuden ja luotettavuutta valmistelu. Hinnoittelu mallin R palvelimen päivittyy vastaavasti.
- R palvelimen klusterin tyyppi hinta perustuu nyt vakio taso hinnan sekä R palvelimen lisämaksu hinta. Premium-tason nyt varataan Premium-ominaisuuksia on käytettävissä erilaisia klusterin välillä ja ei käytetä R-klusterin palvelintyyppi. Tämä muutos ei vaikuta tehokkaita hinnat R-palvelimen, se muuttuu vain maksut esitystavan laskussa. Kaikki olemassa olevat R palvelimen klustereiden toimivat edelleen ja ARM-mallien jatkaa toimivan heikentämisestä ilmoitus asti. **On suositeltavaa vaikka voit päivittää komentosarjaan määritetty käyttöönoton käyttämään uutta ARM-mallia.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Muistiinpanojen, R-palvelimeen HDInsight 08/30/2016-versiossa

Täysi versio väriavaruuden Linux-pohjaiset HDInsight klustereiden tässä versiossa käyttöön:

|HDI |HDI klusterin versio   |HDP |HDP muodosta   |Ambari muodosta |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Windows-pohjaisesta HDInsight klustereiden käyttöön tässä versiossa täydellisen version väriavaruuden:

|HDI |HDI klusterin versio   |HDP |HDP muodosta     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Muistiinpanojen, R-palvelimeen HDInsight 08/17/2016-versiossa

- R palvelin 8.0.5 – pääasiassa ohjelmavirhe korjaus-versiossa. Katso lisätietoja [R Serverin julkaisutiedot](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) . 
- Reuna-solmu – [R-paketin](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) AzureML-paketin mahdollistaa R mallit julkaistaan ja kulutettu Azure ML-web-palvelu.  Saat lisätietoja Microsoftin ["yleiskatsaus, R Server-HDInsight"](hdinsight-hadoop-r-server-overview.md) -artikkelin kohdassa ["Operationalize mallin"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) .
- Linux riippuvuudet [ylimmät 100 suosituimpia R-pakettien](https://github.com/metacran/cranlogs) – nämä Linux paketin riippuvuudet on asennettu valmiiksi.  
- Vaihtoehto, jos haluat käyttää CRAN repo lisääminen R paketteja tietojen solmut. Saat lisätietoja Microsoftin ["Käytön aloittamisessa R HDInsight-palvelimelle"](hdinsight-hadoop-r-server-get-started.md) -artikkelin kohdassa ["Asenna R pakettien"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) .
- Parannettu luotettavuus R palvelimen valmistelu klustereiden luotaessa.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Muistiinpanojen HDInsight 08/01/2016-versiossa

Täysi versio väriavaruuden Linux-pohjaiset HDInsight klustereiden tässä versiossa käyttöön:

|HDI |HDI klusterin versio   |HDP |HDP muodosta   |Ambari muodosta |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Windows-pohjaisesta HDInsight klustereiden käyttöön tässä versiossa täydellisen version väriavaruuden:

|HDI |HDI klusterin versio   |HDP |HDP muodosta     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi ohjattu, Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Muutokset HDInsight 3.4 klustereihin | Seuraavat rakenteen määritykset oletusarvoa on muutettu suorituskyvyn parantamiseksi <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Palvelun    | Kaikki| PUUTTUU|
| Tähän julkaisuun sisältyvien seuraavia korjaustapoja | RAKENNE-13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Palvelun    | Kaikki| PUUTTUU

## <a name="notes-for-07142016-release-of-hdinsight"></a>Muistiinpanojen 07/14/2016 HDInsight-versiossa

Täysi versio väriavaruuden Linux-pohjaiset HDInsight klustereiden tässä versiossa käyttöön:

|HDI |HDI klusterin versio   |HDP |HDP muodosta   |Ambari muodosta |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Windows-pohjaisesta HDInsight klustereiden käyttöön tässä versiossa täydellisen version väriavaruuden:

|HDI |HDI klusterin versio   |HDP |HDP muodosta     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Muistiinpanojen 07/07/2016 HDInsight-versiossa

Täysi versio väriavaruuden Linux-pohjaiset HDInsight klustereiden tässä versiossa käyttöön:

|HDI |HDI klusterin versio   |HDP |HDP muodosta   |
|----|----------------------|----|------------|
|3.2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Windows-pohjaisesta HDInsight klustereiden käyttöön tässä versiossa täydellisen version väriavaruuden:

|HDI |HDI klusterin versio   |HDP |HDP muodosta     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi ohjattu, Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [IntelliJ HDInsight-Työkalut](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ IntelliJ VERRATA ‑laajennuksen HDInsight ohjattu klustereiden on nyt integroitu Azure-työkalujen avulla. Se tukee Azure SDK v2.9.1 uusin Java SDK: T- ja sisältää kaikki kohteeseen Hdinsightiin ‑laajennuksen IntelliJ ominaisuudet.| Työkalut    | Ohjattu| PUUTTUU|
| [Pimennys HDInsight-Työkalut](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure työkalujen Pimennys, tukee nyt HDInsight ohjattu klustereiden. Ottaa käyttöön seuraavat ominaisuudet. <ul><li>Luo ja kirjoittaa ohjattu sovelluksen helposti Scala ja Java ensimmäisen luokkaan authoring tuki IntelliSense Automaattinen muotoilu, Virheentarkistus jne.</li><li>Testaa ohjattu sovelluksen paikallisesti.</li><li>Lähettää työt HDInsight Ohjattu klusterin ja noutaa tulokset.</li><li>Kirjaudu sisään Azure sekä kaikki liittyvät Azure tilauksistasi ohjattu klustereiden käyttää.</li><li>Siirry kaikki liittyvät tallennustilan HDInsight Ohjattu klusterin resurssit.</li></ul>| Työkalut    | Ohjattu| PUUTTUU

Tässä versiossa alkaen on muuttanut Linux-pohjaiset HDInsight klustereiden käytännön Vieras OS-aikavyöhykkeiden. Uuden käytännön lähinnä voit vähentää huomattavasti vuoksi korjataan käynnistyy. Uuden käytännön jatkossakin Linux klustereiden kaikkia maanantai tai torstai aloittaen 12 AM UTC-ajan Porrastettu tavalla kaikissa annetun klusterin solmujen korjaustiedoston näennäiskoneiden (VMs). Kuitenkin kaikki tietyn AM käynnistetään vain kerran enintään 30 päivän välein vuoksi Vieras OS päivitetään. Lisäksi ensimmäisen uudelleenkäynnistyksen juuri luomasi klusterin ei tapahdu aikaisintaan klusterin luominen päivämäärään 30 päivää.

>[AZURE.NOTE] Muutokset koskevat vain juuri luomasi klustereihin tämän julkaisuversio suurempi tai yhtä.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Muistiinpanojen 06/06/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

|HDP    |HDI versio    |Ohjattu versio  |Ambari koontiversio    |HDP koontiversio|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi ohjattu, Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Ohjattu HDInsight-on yleisesti saatavilla | Tässä versiossa näyttää parannukset käytettävyyden, skaalattavuus ja Avaa lähde Apache ohjattu HDInsight-tuottavuutta. <ul><li>Teollisuuden käytettävyys 99,9 %, mikä helpottaa sopii uudempaan yrityksen työmääriä SLA alussa.</li><li>Skaalattavissa tallennustilan kerros Azure tietojen järvi säilöä.</li><li>Jokaisen vaiheen tietojen tarkasteluun ja development tuottavuutta työkaluja. Mukautetun ohjattu ydin Jupyter muistikirjojen käyttöön vuorovaikutteinen tietojen tarkasteluun, BI-koontinäytöt, kuten Power BI, Tableau ja Qlik integrointi on hyvä tietojen jakaminen ja jatkuva raportointia varten, IntelliJ laajennus on luotettava tapa pitkällä aikavälillä koodin Palvelutietojen kehittäminen ja virheenkorjaus.</li></ul>| Palvelun    | Ohjattu| PUUTTUU|
| IntelliJ HDInsight-Työkalut | Tämä on HDInsight ohjattu klustereiden IntelliJ VERRATA-‑laajennuksen. Ottaa käyttöön seuraavat ominaisuudet.<ul><li>Luo ja kirjoittaa ohjattu sovelluksen helposti Scala ja Java ensimmäisen luokkaan authoring tuki IntelliSense Automaattinen muotoilu, Virheentarkistus jne.</li><li>Testaa ohjattu sovelluksen paikallisesti.</li><li>Lähettää työt HDInsight Ohjattu klusterin ja noutaa tulokset.</li><li>Kirjaudu sisään Azure sekä kaikki liittyvät Azure tilauksistasi ohjattu klustereiden käyttää.</li><li>Siirry kaikki liittyvät tallennustilan HDInsight Ohjattu klusterin resurssit.</li><li>Siirry työt historia ja HDInsight Ohjattu klusterin työtiedot.</li><li>Virheenkorjaus ohjattu työt etäyhteyden pöytätietokoneesta.</li></ul>| Työkalut    | Ohjattu| PUUTTUU

## <a name="notes-for-05132016-release-of-hdinsight"></a>Muistiinpanojen 05/13/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi ohjattu, Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Tiedostojen versiopäivitys ja muut virheenkorjauksia | Tässä versiossa päivittää HDInsight-klusterin ohjattu-version 1.6.1 ja korjaa muut virheet| Palvelun    | Ohjattu| PUUTTUU

## <a name="notes-for-04112016-release-of-hdinsight"></a>Muistiinpanojen 04/11/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-muuttumattomina)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Mukautetun metastore päivityksen ongelmat HDI 3.4 varten | Klusterin luominen epäonnistuu mukautetun metastore, jota käytettiin aiemmin toiseen HDInsight-klusteriin alemman versiota käytettäessä. Syynä on päivityksen komentosarjavirhe, joka on nyt korjattu| Klusterin luominen    | Kaikki | PUUTTUU
| Livy palautus | Projektin tila vikasietoisuudelle säädetään Livy lähetettyjä projektille | Luotettavuuden | Ohjattu Linux| PUUTTUU
| Sisällön Jupyter HA | Mahdollistaa tallentamiseen ja lataamiseen Jupyter muistikirjan sisältö ja tallennustilaa klusterin liittyvää tiliä sieltä. Lisätietoja on artikkelissa [ytimet käytettävissä Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Muistikirjat | Ohjattu Linux| PUUTTUU
| HiveContext Jupter muistikirjoissa poistaminen | Käytä `%%sql` magic sijaan `%%hive` tärkeä. SqlContext vastaa hiveContext. Lisätietoja on artikkelissa [ytimet käytettävissä Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Muistikirjat    | Ohjattu klustereiden Linux| PUUTTUU
| Ohjattu aiempi heikentämisestä | Vanhemman version ohjattu 1.3.1 poistetaan palvelusta 5/31 | Palvelun | Ohjattu klustereiden Windows | PUUTTUU

## <a name="notes-for-03292016-release-of-hdinsight"></a>Muistiinpanojen 03/29/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - muuttamatta)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - muuttamatta)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - muuttamatta)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Lisätty HDInsight 3.4 ja päivitetyn HDP-versioissa kaikki HDInsight klustereiden | Tässä versiossa on lisännyt HDInsight v3.4 (perustuu HDP 2.4) ja päivittänyt myös muita HDP versioita. HDP 2.4 julkaisutiedot ovat käytettävissä [seuraavassa](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) ja saat lisätietoja HDInsight versiot voivat olla löytyneen [tähän](hdinsight-component-versioning.md).| Palvelun    | Kaikki Linux klustereiden| PUUTTUU
| HDInsight Premium | Hdinsightista on nyt saatavilla kahteen luokkaan - vakio- ja Premium. Ohjattu klusterit Linux HDInsight Premium on tällä hetkellä esikatselussa ja se on käytettävissä vain Hadoop. Katso lisätietoja [tästä](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Palvelun    | Hadoop ja ohjattu Linux| PUUTTUU
| Microsoft R-palvelin | HDInsight Premium on Microsoft R-palvelimeen, joka voi olla mukana Hadoop ja ohjattu klustereiden Linux. Lisätietoja on artikkelissa [Yleistä, R-palvelimelle Hdinsightista](hdinsight-hadoop-r-server-overview.md).| Palvelun    | Hadoop ja ohjattu Linux| PUUTTUU
| Ohjattu 1.6.0 | HDInsight 3.4 klustereiden sisältävät nyt ohjattu 1.6.0| Palvelun    | Ohjattu klustereiden Linux| PUUTTUU
| Jupyter muistikirjan parannukset | Ohjattu klustereiden käytettävissä Jupyter muistikirjat on nyt muita ohjattu ytimet. Ne sisältävät käyttö, kuten %% tärkeä, automaattinen visualisointi ja integrointi Python visualisoinnin kirjastojen (kuten matplotlib). Lisätietoja on artikkelissa [ytimet käytettävissä Jupyter muistikirjat](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Palvelun | Ohjattu klustereiden Linux | PUUTTUU

## <a name="notes-for-03222016-release-of-hdinsight"></a>Muistiinpanojen 03/22/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - muuttamatta)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - muuttamatta)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - muuttamatta)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kaikki HDInsight klustereiden päivitetyn HDInsight-versiot | Tässä versiossa on päivitetty kaikki HDInsight klustereiden HDInsight-versiot| Palvelun    | Kaikki| PUUTTUU


## <a name="notes-for-03102016-release-of-hdinsight"></a>Muistiinpanojen 03/10/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - muuttamatta)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kaikki HDInsight klustereiden päivitetyn HDInsight-versiot | Tässä versiossa on päivitetty kaikki HDInsight klustereiden HDInsight-versiot| Palvelun    | Kaikki| PUUTTUU

## <a name="notes-for-01272016-release-of-hdinsight"></a>Muistiinpanojen 01/27/2016 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - muuttamatta)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kaikki HDInsight klustereiden päivitetyn HDInsight-versiot | Tässä versiossa on päivitetty kaikki HDInsight klustereiden HDInsight-versiot| Palvelun    | Kaikki| PUUTTUU

## <a name="notes-for-12022015-release-of-hdinsight"></a>Muistiinpanojen 12/02/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - muuttamatta)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - muuttamatta)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Lisätty HDInsight 3.3 ja päivitetyn HDP-versioissa kaikki HDInsight klustereiden | Tässä versiossa on lisännyt HDInsight v3.3 (perustuu HDP 2.3) ja päivittänyt myös muita HDP versioita. HDP 2.3 julkaisutiedot ovat käytettävissä [seuraavassa](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) ja saat lisätietoja HDInsight versiot voivat olla löytyneen [tähän](hdinsight-component-versioning.md).| Palvelun    | Kaikki| PUUTTUU

## <a name="notes-for-11302015-release-of-hdinsight"></a>Muistiinpanojen 11/30/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Päivitetyt HDInsight-versiot HDInsight klustereiden ja HDP versiot HDInsight-3,2 varausyksiköt (Windows ja Linux) | Tässä versiossa on päivitetty HDInsight-ja HDP | Palvelun    | Kaikki| PUUTTUU


## <a name="notes-for-10272015-release-of-hdinsight"></a>Muistiinpanojen 10/27/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - muuttamatta)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Päivitetty HDInsight-versioiden kaikki HDInsight varausyksiköt (Windows ja Linux) | Tässä versiossa on päivitetty HDInsight-ja HDP | Palvelun    | Kaikki| PUUTTUU
| Kiinteä Windows Jupyter ohjattu klustereiden isoilla kirjaimilla klustereiden kanssa | Klustereiden, joka oli määritetty isoilla kirjaimilla DNS-nimien oli ongelmat Jupyter muistikirjat vuoksi origin-pyynnön-valintaruutu. Korjaa oli Jupyter's määritysten DNS-nimen muuttaminen pienet kirjaimet. | Palvelun    | Ohjattu HDInsight (Windows)| PUUTTUU


## <a name="notes-for-10202015-release-of-hdinsight"></a>Muistiinpanojen 10/20/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP oletusversion HDP 2.2 on muutettu | HDInsight Windows klustereiden oletusversio muutetaan HDP 2.2. HDInsight versio 3,2 (HDP 2.2) on yleisesti saatavilla kulunut helmikuussa 2015. Tämä muutos kääntää klusterin oletusversio vain, kun eksplisiittinen valinta ei ole tehty varattaessa Azure portaalin sekä PowerShellin cmdlet-komennot ja SDK klusterin. | Palvelun    | Kaikki| PUUTTUU                  |
|Muutosten AM nimimuotoa käyttöönotto useita HDInsight-Linux klustereiden yhdessä Virtual verkossa | Tuki käyttöönotto useita yhdessä virtual verkossa HDInsight Linux klustereiden lisätään tässä versiossa. Osana, virtuaalikoneen nimimuodon klusterissa on muuttunut headnode\*, workernode\* ja zookeepernode\* , hn\*, wn\*, ja zk\* tarpeen mukaan. Se ei ole suositeltavaa, jos haluat ottaa suoraan riippuvuus virtuaalikoneen nimimuodon, koska tämä on saatetaan muuttaa. Kirjoita Määritä "hostname -f"-paikallisessa tietokoneessa tai Ambari API avulla isäntien ja isännät osien yhdistäminen. Voit etsiä lisätietoja [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) ja [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Palvelun | HDInsight klustereiden Linux | PUUTTUU |
| Määritysmuutoksia | Klustereiden HDInsight 3.1, seuraavat määritykset on nyt käytössä: <ul><li>tez.yarn.ATS.Enabled ja yarn.log.server.url. Näin aikajanan sovelluspalvelin ja Log palvelimen voivan yhteyshenkilönä lokit ulos.</li></ul>Klustereiden HDInsight 3,2 on muokattu seuraavat määritykset: <ul><li>mapreduce.fileoutputcommitter.Algorithm.version on määritetty 2. Näin FileOutputCommitter V2-version käyttöä.</li></ul> | Palvelun | Kaikki | PUUTTUU |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Muistiinpanojen 09/09/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - muuttamatta)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - muuttamatta)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kaikki HDInsight klustereiden päivitetyn HDInsight-versiot | Tässä versiossa on päivitetty HDInsight-versiot | Palvelun    | Kaikki| PUUTTUU                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Muistiinpanojen 07/31/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - muuttamatta)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - muuttamatta)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Korjaa Ohjattu klusterin solmu uudelleen imaging-työnkulku | Kiinteä ohjelmavirhe, joka aiheuttaa Ohjattu klusterin solmut ei sen jälkeen uudelleen kuvan palauttaminen | Palvelun    | Ohjattu| PUUTTUU                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Muistiinpanojen 07/31/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - muuttamatta)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - muuttamatta)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Kaikki HDInsight klustereiden päivitetyn HDInsight-versiot | Tässä versiossa on päivitetty HDInsight-versiot | Palvelun    | Kaikki| PUUTTUU                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Muistiinpanojen 07/07/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - muuttamatta)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Tämä julkaisu sisältää seuraavat päivitykset.

| Otsikko                                           | Kuvaus                                          | Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK) | Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky) | JIRA (jos saatavilla) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Päivitetty HDInsight-3,2 klustereiden HDP-versiot | Tässä versiossa HDInsight 3,2 ottaa käyttöön HDP 2.2.6.1-0012 | Palvelun    | Kaikki                                                 | PUUTTUU                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Muistiinpanojen 06/26/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - muuttamatta)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Päivitetty HDInsight-3,2 klustereiden HDP-versiot</td>
<td>Tässä versiossa HDInsight 3,2 ottaa käyttöön HDP 2.2.6.1</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Muistiinpanojen 06/18/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Lisää HTTPS-portit</td>
<td>Pilvipalvelussa sijaitsevaan Avaa nyt 5 portit 8001 8005 kuuluvat esimerkiksi-klusterissa osoitteessa https://<clustername>.azurehdinsight.net:8001/. Näitä URL-osoitteet pyynnöt todennetaan Käytä samaa perustodentamista salasana-järjestelmä porttiin 443. Porttien sitoa active headnode samaan porttiin. Komentosarjatoimintoja voidaan tehdä asiakkaan palvelujen kuuntelevat porttien headnode ja reitin klusterin ulkopuolella.</td>
<td>Pilvipalvelussa</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Katkonainen MapReduce sekoitus ongelman HDInsight 3,2</td>
<td>Suuri klustereiden tuloksena on satunnaiset tehtävän virheet-kartan vähentää sekoitus harvinaisissa, ajoittaiset kilpa ehdon korjata. Lisätietoja on kohdassa <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> .</td>
<td>Hadoop Core</td>
<td>Kaikki</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>

<tr>
<td>Siirry Azure Java uusimman HDInsight 3,2 2.2 SDK</td>
<td>Siirtää Java WASB ohjaimen käyttämän Azure SDK uusimman version. Uusimman SDK on muutama korjaukset ja sama julkaisutiedot on osoitteessa https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoop Core</td>
<td>Kaikki</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>Siirry HDP 2.1.15 HDInsight 3.1 klustereiden varten</td>
<td>Julkaisemisen Hortonworks julkaisutiedot ovat käytettävissä <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">seuraavassa</a>.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Muistiinpanojen 06/04/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - muuttamatta)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>502 – virheellinen yhdyskäytävä-virheen myrsky klustereiden korjaaminen</td>
<td>Tässä versiossa korjaa ohjelmavirhe vaikuttavia työn lähetyksen API, jotka aiheuttivat sivustosta alaspäin uudelleenkäynnistyksen jälkeen.</td>
<td>Palvelun</td>
<td>Myrsky</td>
<td>PUUTTUU</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Muistiinpanojen 06/01/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - muuttamatta))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - muuttamatta)
* SDK 1.5.8


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Eri virheenkorjauksia</td>
<td>Tässä versiossa korjaa klusteri valmisteleminen liittyvät virheet.</td>
<td>Palvelun</td>
<td>Klusterin kaikentyyppisillä</td>
<td>PUUTTUU</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Muistiinpanojen 05/27/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Muut klusterin versiot ja SDK ei käyttöön tässä versiossa osana.


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>HDP 2.2 päivitys</td>
<td>Tässä versiossa HDInsight 3,2 sisältää HDP 2.2.6, ja yhdistää useita tärkeitä virheenkorjauksia Hdinsightista. Koko julkaisutiedot on saatavilla kohdassa <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 julkaisutiedot</a>.</td>
<td>HDP</td>
<td>Klusterin kaikentyyppisillä</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Muuta oletusarvoista kuitenkaan säilö muistin asetukset</td>
<td>Tämän päivityksen oletusarvon käytettävissä oleva muisti kuitenkaan säilöjen (yarn.nodemanager.resource.memory megatavun ja yarn.scheduler.maximum kohdistus-Mt), voit käynnistää solmu hallinnan avulla kasvaa 5632 megatavua. Aiemmin tämä on vähentää 4608 Mt, mutta erilaisia projektin suoritetaan perusteella, uusi arvo on annettava paremmin luotettavuutta ja suorituskykyä useimmat töihin näin ollen on paremmin oletusarvo. Tavanomainen, jos sinun on kriittinen riippuvuuden muistin-määritysten, määritä se erikseen klusterin luomisen aikana.</td>
<td>HDP</td>
<td>Klusterin kaikentyyppisillä</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Oletusarvoinen Config välistä eroa HBase ja myrsky klustereiden varten</td>
<td>Tämä päivitys palauttaa Hbase ja myrsky klustereiden käyttämään kuitenkaan kokoonpanomääritysten yhteydessä samat arvot kuin Hadoop klustereiden. Tämä on valmiiksi välistä eroa kaikki klusterin välillä.</td>
<td>HDP</td>
<td>HBase, myrsky</td>
<td>PUUTTUU</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Muistiinpanojen 05/20/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>SCP.NET EventHub tuki</td>
<td>Päivitetty klusterin paketit HDInsight myrsky tuo SCP.NET uudet ominaisuudet. Sinulla on nyt uusi API topologian muodostimen, jotka helpottavat EventHubSpout tai Java Spouts oikeudet. Sinun on päivitettävä SCP.NET asiakkaan SDK toimimaan uusi klustereiden kuin sopimusten on päivitetty. Lisätietoja uuden ohjelmointirajapinnan käyttö- ja release notes (mukaan lukien virheenkorjauksia) Tutustu SCP.NET nuget pakettiin sisältyy Lueminut-tiedosto.</td>
<td>Tooling ja</td>
<td>Myrsky HDInsight 3,2 klustereiden</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>JDBC ohjaimen päivitys</td>
<td>Päivittää ohjaimen sqljdbc_4.1.5605.100 tuetut SQL Server-version.</td>
<td>Metastore</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Muistiinpanojen 04/27/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - muuttamatta)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Korjaa DLL-riippuvuus</td>
<td>Poistaa HDInsight riippuvuus yksikön testi Framework.</td>
<td>SDK-PAKETISSA</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Ohjelmavirhe korjata ehto</td>
<td>Klusterin luominen pyyntö nyt odottaa HYLLYTETTY pyynnön hyväksyä ennen tilan kysely</td>
<td>SDK-PAKETISSA</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Muistiinpanojen 04/14/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - muuttamatta)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Tez virheenkorjauksia</td>
<td>Tämän version HDI 3,2 sisältyvät Apache TEZ 2214 ja TEZ 1923 korjaukset. Näitä tarvitaan erityisesti tiettyjen rakenne kyselyjen Tez, jotka edellyttävät sekoita merkittäviä määriä tietoa.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Muistiinpanojen 04/06/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - muuttamatta)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - muuttamatta)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - muuttamatta)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Voit poistaa joitakin sisäinen luokkien HDInsight Linux päivitykset.</td>
<td>SDK-PAKETISSA</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Kirjaston Avro 1.5.6</td>
<td>Lisätty <b>KnownTypeAttribute</b> menetelmän <b>GetAllKnownTypes</b>. Kiinteä NullReferenceException tyyppi on GetAllKnownTypes menetelmän null.</td>
<td>SDK-PAKETISSA</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Virheenkorjauksia</td>
<td>Eri virheenkorjauksia-palveluun</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Muistiinpanojen 04/01/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Mahdollisuus remote työpöydän tunnistetietoja .NET SDK kautta Windows klustereiden ottaminen käyttöön tai poistaminen käytöstä</td>
<td>Ohjelmallisesti tuen ottaminen käyttöön tai poistaminen Windows klustereiden RDP tunnistetietoja.</td>
<td>SDK-PAKETISSA</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Mahdollisuus käyttöön klustereiden remote työpöydän tunnistetietoja samalla, kun ne ovat Valmistellaan</td>
<td>Ohjelmallisesti tuen ottaminen käyttöön remote työpöydän tunnistetietoja kuin klusterin luodaan. Tämä poistaa kaksivaiheinen prosessi, jossa valmistellaan ensin klusterin ja etätyöpöydän ottaminen käyttöön.</td>
<td>SDK-PAKETISSA</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Päivitetyn Python 2.7.8 avulla</td>
<td>Päivitetyn HDInsight klustereiden Python 2.7.8, Python joka sisältää joitakin tärkeitä tietoturvan ratkaisuja HDInsight-versio 2.1, 3.0, 3.1 ja 3.2</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>KUITENKAAN määritysten muutos</td>
<td>Muutetut kuitenkaan määritysten yarn.resourcemanager.max valmis-sovelluksia tyypeissä klusterin HDInsight-versioiden 3.1 ja 3.2 1 000. Tämä arvo määrittää vain kuitenkaan käyttöliittymässä valmiiden sovellusten luettelossa. Saat lisätietoja sovelluksista, jotka on esitetty ennen sovellusten Käyttöliittymän näytetään luettelossa voit suoraan siirtyä historia-palvelimeen.</td>
<td>KUITENKAAN</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>HBase-klusterin solmujen koon muuttaminen</td>
<td>HBase klustereiden Salli nyt koon solmujen (ylös ja alas) HDInsight-versioiden 3.1 ja 3.2</td>
<td>Palvelun</td>
<td>HBase</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>JDBC päivitys</td>
<td>SQL-JDBC driver on päivitetty versio sqljdbc_4.0.2206.100 HDInsight versiota 3.2 varten. Tämä versio sisältää tärkeitä tietoturvan.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>JVM määritysten päivitys</td>
<td>Päivitetty JVM määritysten networkaddress.cache.ttl 300 sekuntia 1 HDInsight-versioiden 3.1 ja 3.2 oletusarvo-arvosta. Määritysten arvoksi ohjaa välimuistiin käytäntö onnistuu nimihaut nimi-palvelusta. Tämä korjaa liittyvät ja pienentäminen HBase klustereiden ohjelmavirhe.</td>
<td>Palvelun</td>
<td>HBase</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Azure-tallennustilan SDK Java 2.0 päivittäminen</td>
<td>HDInsight versiota 3.2 on päivitetty käyttämään Java Azure-tallennustilan SDK uusimman version. Tämä sisältää useita tärkeitä virheenkorjauksia nykyisen 0.6.0 versio.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Päivitetty Apache WASB-lähdekoodi</td>
<td>HDInsight versiota 3.2 nyt käyttää uusimmat koodi Apache Hadoop WASB tiedostojärjestelmän ohjain. Tämän muutoksen WASB ohjain on nyt pakattu erillisessä purkki. Tämä on pelkästään pakkaus muutoksen ja ei sisällä WASB ohjaimen toiminnan muutokset. PURKKI tiedoston nimi on hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Tiedostonimi-päivitykset näkyvät HDInsight 3,2 JAR</td>
<td>Tämän version 3.2 HDInsight päivityksen sisältää useita virheenkorjauksia ja muutamia sisäinen tölkki pakattu HDP osana on päivitetty. Huomaa, että PURKKI nämä tiedostot ovat sisäinen HDP-paketin, ei käyttävät suoraan asiakas-sovelluksissa. Sovellusten olisi pakkaa omaa versiota tölkki niin, että HDP sisäinen tölkki päivittäminen ei saa tapahtua asiakkaan sovellukset.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Muistiinpanojen 03/03/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - muuttamatta)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - muuttamatta)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - muuttamatta)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - muuttamatta)
* SDK 1.5.0 (muuttamatta)

Tämä julkaisu sisältää seuraavan päivityksen.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Luotettavuuden parannukset</td>
<td>Microsoft tehnyt korjaukset, joka mahdollistaa palvelun kanssa, jotka koskevat klusterin luominen kasvaa kuormituksen skaalata paremmin.</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Muistiinpanojen 02/18/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - muuttamatta)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - muuttamatta)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - muuttamatta)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>HDInsight-3,2 klustereiden</td>
<td>Hadoop 2.6/HDP2.2 on käytettävissä HDInsight-3,2 klustereiden. Se sisältää tärkeimmät muutokset kaikkiin Avaa lähde-osia. Lisätietoja on artikkelissa uudet HDInsight ja <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 julkaisutiedot</a>.</td>
<td>Avaa lähde-ohjelmisto</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>HDinsight Linux (ennakkoversio)</td>
<td>Klustereiden voidaan ottaa käyttöön Ubuntu Linux-käyttöjärjestelmässä. Lisätietoja on artikkelissa Linux HDInsight käytön aloittaminen.</td>
<td>Palvelun</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Myrsky yleinen käytettävyys</td>
<td>Apache myrsky klustereiden ovat yleensä käytettävissä. Lisätietoja on artikkelissa Aloita myrsky käyttäminen Hdinsightista.</td>
<td>Palvelun</td>
<td>Myrsky</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Virtuaalikoneen koot</td>
<td>Azure Hdinsightiin on käytettävissä Lisää virtuaalikoneen tyyppi ja koko. HDInsight voit hyödyntää A2 A7 koon laadittuihin Yleiset tarkoituksiin. D-sarjan solmujen, SSD asemat (SSDs) ja 60 prosenttia nopeampaa suoritinta; ominaisuus ja A8 ja A9 koot, joissa on InfiniBand tukea nopea verkko.</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Klusterin skaalaus</td>
<td>Voit muuttaa tietojen käynnissä HDInsight-klusterin solmujen määrän eikä sinun tarvitse poistaa tai luoda sen uudelleen. Tällä hetkellä vain Hadoop-kysely ja Apache myrsky klusterin on tämä mahdollista, mutta Apache HBase klusterin tyyppi on pian voit seurata tuki. Lisätietoja on artikkelissa hallinta HDInsight klustereiden.</td>
<td>Palvelun</td>
<td>Hadoop-myrsky</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Visual Studio sillä</td>
<td>Lisäksi Apache myrsky valmis sillä, Apache rakenne Visual Studiossa, sillä on päivitetty täydennys, paikallinen kelpoisuustarkistus ja muistin parannettu tuki. Lisätietoja on artikkelissa Visual Studio HDInsight Hadoop työkalut käytön aloittaminen.</td>
<td>Sillä</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Hadoop yhdistimen DocumentDB</td>
<td>Hadoop Connectorilla DocumentDB varten voit tehdä monimutkaisia koosteet, analysointia ja yhdistämismäärityksiä rakenteen pienempi JSON asiakirjoja tallennettu yli DocumentDB sivustokokoelmat tai tietokannan tilien välillä. Lisätietoja ja opetusohjelman on artikkelissa suorittaa Hadoop työt käyttämällä DocumentDB ja Hdinsightista.</td>
<td>Palvelun</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Virheenkorjauksia</td>
<td>Microsoft on tehnyt eri vähäisiä virheenkorjauksia HDInsight-palveluiden. Asiakkaan osoittava tapahtuu muutoksia ei odoteta.</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Muistiinpanojen 02/06/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - muuttamatta)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - muuttamatta)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK PUUTTUU

Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Virheenkorjauksia</td>
<td>Microsoft on tehnyt eri vähäisiä virheenkorjauksia HDInsight-palveluiden. Asiakkaan osoittava tapahtuu muutoksia ei odoteta.</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>HDP 2.1 ylläpito päivitys</td>
<td>HDInsight 3.1 on päivitetty käyttöön HDP 2.1.10.0. Lisätietoja on artikkelissa <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">julkaisutiedot HDP-2.1.10</a>. </td>
<td>Avaa lähde-ohjelmisto</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>HDP binaarinen päivitykset</td>
<td>Mitä tiedostolle nimet on päivitetty HBase on joitakin PURKKI tiedostoja. PURKKI nämä tiedostot käytetään sisäisesti HBase, joten se ei odoteta, asiakkaiden on oltava riippuvuus PURKKI tiedostojen nimet. Näitä ovat:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Avaa lähde-ohjelmisto</td>
<td>HBase</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Muistiinpanojen 1/29/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - muuttamatta)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - muuttamatta)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - muuttamatta)
* SDK PUUTTUU

Tämä julkaisu sisältää seuraavan päivityksen.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Sellaisiin alueen (esimerkiksi palvelu, osan tai SDK)</p></th>
<th>Klusterin tyyppi (esimerkiksi Hadoop, HBase tai myrsky)</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>

<td>Virheenkorjauksia</td>
<td>Microsoft on tehnyt joitakin tärkeitä virheenkorjauksia, jotka parantavat luotettavuutta HDInsight-klustereiden Azure päivitysten aikana.</td>
<td>Palvelun</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Muistiinpanojen 1/5/2015 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden tässä versiossa käyttöön:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - muuttamatta)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - muuttamatta)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - muuttamatta)


Tämä julkaisu sisältää seuraavat päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Twitter trendi ja Mahout perustuva elokuvan suositukset</td>
<td><p>Tässä versiossa HDInsight-kysely-konsolin on kaksi muita Esimerkkejä:</p>

<p><b>Twitter-trendi analyysi</b><br>
Julkisen API Twitter sivustoja varten on hyödyllinen tietolähde tietojen analysointiin ja tietoja Suositut trendejä. Tässä opetusohjelmassa tietoja Twitter-käyttäjät, jotka lähetetään useimmat tweets, joka sisältää tietyn sanan luettelo rakenteen avulla. </p>

<p><b>Mahout elokuvan suositus</b><br>
Apache Mahout on Apache Hadoop konepohjaisten oppimistekniikoiden kirjastoon. Mahout sisältää algoritmit käsittelyyn tietoja (kuten suodatus luokittelu ja klusterointi). Tässä opetusohjelmassa suositus ohjelma avulla luoda elokuvan suositukset, ystäviesi tuliko elokuvat perusteella.</p></td>
<td>Kyselyn konsoli</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Muuta rakenteen määritys oletusarvo: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Koon määritysten koskee automaattisesti muunnetun kartan liitokset. Arvo vastaa taulukoita ja jotka voidaan muuntaa hash kartat, jotka sopivat muistiin koot summa. Tämän arvon suurennettu edellisen version 10 Mt oletusarvon 128 megatavua. 128 Mt uusi arvo on kuitenkin aiheuttaa epäonnistuu määräpäivä muistin puutteen työt. Tässä versiossa palautetaan takaisin 10 Mt oletusarvoa. Asiakkaat edelleen voit ohittaa tämän arvon klusterin luonnin aikana niiden kyselyt ja taulukon kokoa. Lisätietoja tämän asetuksen ja miten voit korvata kohdassa <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimointi Automaattinen liittyminen muuntaminen</a> Hortonworks ohjeissa. </p></td>
<td>Rakenne</td>
<td>Hadoop-Hbase</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Muistiinpanojen 12/23/2014 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden käyttöön tässä versiossa on:

* HDInsight 2.1.10.420.1246783 (HDP versio ei muutu)
* HDInsight 3.0.6.420.1246783 (HDP versio ei muutu)
* HDInsight 3.1.1.420.1246783 (HDP versio ei muutu)

Tämä julkaisu sisältää seuraavan päivityksen.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>


<tr>
<td>Katkonainen klusterin luominen epäonnistuu liiallinen kuormituksen vuoksi</td>
<td><p>Parannettu algoritmin ladattavaksi HDP pakettien klusterin luonnin aikana mahdollistaa tehokkaamman käsittelyn vuoksi liiallinen lataaminen epäonnistuu.</p></td>
<td>Palvelun</td>
<td>Hadoop-Hbase, myrsky</td>
<td>PUUTTUU</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Muistiinpanojen 12/18/2014 HDInsight-versiossa

Tässä versiossa on seuraavan osan päivityksen.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Klusterin mukauttaminen Yleiset Avalability</a></td>
<td><p>Mukauttaminen mahdollistaa, jota voi mukauttaa Azure HDInsight-klustereiden projekteissa, jotka eivät ole käytettävissä Apache Hadoop ekosysteemissä. Uusi tällä toiminnolla voit kokeilla ja Hadoop projektien käyttöön Azure Hdinsightiin. Tämä on otettu käyttöön **Komentosarja-toiminnon** -toiminto, jonka voit muokata Hadoop klustereiden haluamaansa tavoilla käyttämällä mukautettujen komentosarjojen kautta. Mukauttamisen on käytettävissä kaikentyyppisissä HDInsight varausyksiköiden mukaan lukien Hadoop, HBase ja myrsky. Osoittamaan potenssiin tätä ominaisuutta olemme on kuvattu prosessin Suositut <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Ohjattu</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>ja <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> moduulit asentamiseen. Tässä versiossa Lisää myös ominaisuuksien asiakkaat voivat määrittää niiden mukautettu komentosarja-toiminnon kautta Azure-portaalissa on ohjeita ja parhaita käytäntöjä tietoja muodostaminen helper menetelmillä mukautettu komentosarja-toiminnot ja siitä, miten voit testata komentosarja-toiminnon ohjeita. </p></td>
<td>Ominaisuuden yleinen käytettävyys</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Muistiinpanojen 12/05/2014 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden käyttöön tässä versiossa on:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK puuttuu

Tämä julkaisu sisältää seuraavat osan päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>

<tr>
<td>Ohjelmavirhe korjaaminen: katkonainen Virhe lisättäessä paljon osioiden rakenne DDL taulukkoon </td>
<td><p>Jos näkyvissä on katkonainen yhteysvirhe metastore tietokannan rakenteen kanssa, lisättäessä paljon osioiden rakenteen taulukon, rakenne-DDL voi epäonnistua. Seuraava-lause näkyä rakenteen virheloki tämän virheen ilmetessä: </p><p>"[Main]-Virhe: ql. Ohjaimen (SessionState.java:printError(547)) - epäonnistui: suoritusvirhe, palaa koodin 1 org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction kutsuttiin mutta openTransactionCalls = 0. Tämä todennäköisesti osoittaa, että on tasaamattomat puhelut openTransaction/commitTransaction)"</p></td>
<td>Rakenne</td>
<td>Hadoop-Hbase</td>
<td>RAKENNE-482 (tämä on sisäinen JIRA, jotta se et voi olla lainausmerkeissä ulkoisesti. Ole mainittu tässä viitteeksi.)</td>
</tr>

<tr>
<td>Ohjelmavirhe korjaaminen: ajoittaiset jumittua HDInsight kyselyn konsolissa</td>
<td>Tällöin WebHCat lokin WebHCat avain työn voidaan näyttää seuraavan lauseen: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): ei voitu ladata tiedoston historia {wasb URL-osoitteen historian tiedostoon} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>RAKENNE-482 (tämä on sisäinen JIRA, jotta se et voi olla lainausmerkeissä ulkoisesti. Ole mainittu tässä viitteeksi.)</td>
</tr>

<tr>
<td>Ohjelmavirhe korjaaminen: ajoittaiset Piikkiin viive Hbase kyselyt</td>
<td>Jos näin käy, käyttäjät huomaat Hbase kyselyjen viive ajoittaiset piikin 3 sekuntia. </td>
<td>HDInsight-klusterin yhdyskäytävän</td>
<td>HBase</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>HDP JAR tiedoston nimi muuttuu</td>
<td>Saat HDI klusterin versio 3.0-palvelussa pari HDP asentama sisäinen PURKKI-tiedostojen muutokset. jota 6.1.26.jar on korvattu jota 6.1.26.hwx.jar. jota util-6.1.26.jar on korvattu jota util-6.1.26.hwx.jar. Nämä muutokset koskevat Hadoop, Mahout, WebHCat ja Oozie projektit.</td>
<td>Hadoop-Mahout, WebHCat Oozie</td>
<td>Hadoop-HBase</td>
<td>PUUTTUU</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Muistiinpanojen 11/21/2014 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden käyttöön tässä versiossa on:

* HDInsight 2.1.9.382.1169709 (11/14/2014 ei muutosta)
* HDInsight 3.0.5.382.1169709 (11/14/2014 ei muutosta)
* HDInsight 3.1.1.382.1169709 (11/14/2014 ei muutosta)
* HDINsight SDK 1.4.0

Tämä julkaisu sisältää seuraavat osan päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>

<tr>
<td>Käytettäessä sovelluksen lokit</td>
<td>Mahdollisuus luetteloida ohjelmallisesti-että klustereiden suoritettavien sovellusten ja lataa haluamasi sovellus kielikohtaiset tai säilö kielikohtaiset lokit virheenkorjaus ongelmallinen sovellusten avulla.</td>
<td>SDK-PAKETISSA</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Mahdollisuus määrittää alueen nimi IHdInsightClient.DeleteCluster </td>
<td>Azure Hdinsightiin SDK on mahdollisuus määrittää alueen nimi, kun käytät **DeleteCluster**. Näin eston asiakkaat, jotka ovat kaksi resursseja, joilla on sama nimi kuin eri alueilla ja ollut ei voi poistaa niistä.</td>
<td>SDK-PAKETISSA</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>**ClusterDetails** objektin palauttaa **DeploymentID** -kenttä, joka osoittaa klusterin yksilöllinen. Se taata on oltava yksilöivä klusterin luominen yritykset samannimistä.</td>
<td>SDK-PAKETISSA</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Muistiinpanojen 11/14/2014 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden käyttöön tässä versiossa on:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Tässä versiossa sisältää seuraavat uudet ominaisuudet-osan päivitykset ja virheenkorjauksia.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>

<tr>
<td>Komentosarja-toiminnon (ennakkoversio)</td>
<td>Klusterin mukauttaminen-ominaisuus, jonka avulla Hadoop muokkaamista esikatselu klusterit haluamaansa tavoilla mukautettuja komentosarjoja käyttäen. Tämän ominaisuuden avulla käyttäjät voivat kokeilla ja projektit, jotka eivät ole käytettävissä Apache Hadoop ekosysteemissä Azure Hdinsightiin klustereihin käyttöönotto. Mukauttaminen-toiminto on käytettävissä kaikentyyppisissä HDInsight klustereiden, mukaan lukien Hadoop, HBase ja myrsky.</td>
<td>Uusi ominaisuus</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Valmiin työt Azure sivustot ja tallennustilaa log analysointia varten</td>
<td>HDInsight kyselyn konsolissa on aloittaminen valikoima, joka tukee ratkaisuja, jotka toimivat tietojen tai mallitiedot.
<p>**Ratkaisuja, jotka työskentelevät tiedot**:<br>
Olemme olet luonut työt-joillekin yleisimmät tietojen analysointi-skenaariot säätää pohjana oman ratkaisujen luominen. Työn tietojen avulla voit nähdä, miten se toimii. Kun olet valmis, käytä osaamisesi luot ratkaisun, joka perustuu valmiin projektin.</p>
<p>**Ratkaisuja, jotka työskentelevät mallitiedot**:<br>
Lue, miten kävelemällä havainnollistetaan basic tilanteita (esimerkiksi web-lokit ja tunnistimen tietojen analysoiminen) HDInsight-käyttöä varten. Opit käyttämään HDInsight näiden tietojen analysointia ja siitä, miten voit muodostaa yhteyden tiedot muiden sovellusten ja palveluiden. Kesäolympialaisten visualisointi tietojen muodostamalla yhteyden Microsoft Excel on esimerkki tämän menetelmän potenssiin.</p></td>
<td>Kyselyn konsoli</td>
<td>Hadoop</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Templeton muistin muistivuodon korjaus</td>
<td>Templeton, jotka vaikuttavat asiakkaat, joilla oli pitkään suoritetut varausyksiköiden tai on lähettäminen 100s työn muistivuodon pyytää toisen on osoitettu. Se ilmenee järjestelmän Templeton 5xx virheet ja vaihtoehtoinen menetelmä ongelman oli-palvelun käynnistäminen uudelleen. Menetelmä ei enää tarvita.</td>
<td>Templeton</td>
<td>Kaikki</td>
<td>https://Issues.Apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Huomautus**: osoittamaan käytettäviksi klusterin mukauttaminen uusia ominaisuuksia ohjattu ja R-moduulin asentaminen klusterin komentosarja-toiminnon avulla ohjeita on mainittu. Lisätietoja on artikkelissa:

* [Asentaminen ja käyttäminen ohjattu 1.0 HDInsight klustereiden](hdinsight-hadoop-spark-install.md)
* [Asentaminen ja käyttäminen HDInsight Hadoop klustereiden R](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Muistiinpanojen 11/07/2014 HDInsight-versiossa

Täysi versio väriavaruuden HDInsight klustereiden joka käyttöön tässä versiossa on:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Tämä julkaisu sisältää seuraavat osan päivitykset.

<table border="1">
<tr>
<th>Otsikko</th>
<th>Kuvaus</th>
<th>Osa</th>
<th>Klusterin tyyppi</th>
<th>JIRA (jos saatavilla)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Tässä versiossa perustuu-Hortonworks tietojen ympäristö (HDP) 2.1.7. Lisätietoja on artikkelissa <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">HDP 2.1.7 julkaisutiedot</a>.</td>
<td>HDP</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>KUITENKAAN aikajana-palvelin</td>
<td>KUITENKAAN aikajanan Server (tunnetaan myös nimellä yleinen historia sovelluspalvelin) on käytössä oletusarvoisesti. Aikajana-palvelin on yleisiä tietoja valmiiden sovellusten (kuten tunnus, sovelluksen nimi, sovelluksen tila, sovelluksen lähetyksen aika ja sovelluksen valmistuminen aika).

Hakemuksen tiedot voidaan hakea pään solmun käyttäminen URI http://headnodehost:8188 tai kuitenkaan-komennon suorittaminen: kuitenkaan sovelluksen – luettelo – appStates kaikki.

Nämä tiedot myös voidaan hakea etäyhteyden mutta REST-Ohjelmointirajapinnalla osoitteessa https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Katso tarkempia tietoja <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Kuitenkaan aikajanan palvelimeen</a>.</td>
<td>Palvelun kuitenkaan</td>
<td>Hadoop-HBase</td>
<td>PUUTTUU</td>
</tr>

<tr>
<td>Klusterin käyttöönoton tunnus</td>
<td>Alkaen SDK versio 1.3.3.1.5426.29232, käyttäjät voivat käyttää kunkin klusterin, joka on myöntänyt HDInsight yksilöivä tunnus. Näin asiakkaat voivat selvittää klustereiden yksilöllisiä versioita, kun DNS-nimeä käytetään vaakasuunnassa luominen ja pudota skenaarioita.</td>
<td>SDK-PAKETISSA</td>
<td>Kaikki</td>
<td>PUUTTUU</td>
</tr>
</table>
<br>

**Huomautus**: Tässä versiossa on korjattu, joka esti koko versionumero näkyvät portaalissa tai palautetut SDK: N tai Windows PowerShellin ohjelmavirhe.

## <a name="notes-for-10152014-release"></a>Huomautuksia 10/15/2014: n julkaisu

Korjaustiedoston tässä versiossa korjattu muistivuoto Templeton, jotka vaikuttavat Templeton paksu käyttäjät. Käyttäjät, jotka ovat käyttäneet Templeton raskaasti näkyy joissakin tapauksissa se ilmenee järjestelmän kuin 500 virhekoodit koska pyynnöt ei ole tarpeeksi muisti virheitä. Vaihtoehtoinen menetelmä tälle ongelmalle oli Templeton-palvelun käynnistäminen uudelleen. Tämä ongelma on korjattu.


## <a name="notes-for-1072014-release"></a>Huomautuksia 10/7/2014: n julkaisu

* Päätepisteen Ambari käytettäessä "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", *host_name* kentän palauttaa täydellinen toimialuenimi (FQDN) sen sijaan, että vain isäntänimi solmun. Esimerkiksi "**headnode0**", joka palauttaa, saat FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**". Tämä muutos on tarvitaan helpottamaan missä useita klusterin tyyppejä (kuten HBase ja Hadoop) käyttöön yhden virtual verkoston skenaarioita. Tämä tapahtuu esimerkiksi käytettäessä taustatietokantaan ympäristö HBase Hadoop varten.

* On annettu uudet muistin asetukset HDInsight-klusterin oletusarvo-käyttöönottoa varten. Edellisen oletusasetukset muistia ei ottanut riittävästi huomioon suorittimen sydämiä otetaan käyttöön määrän lisäohjeita. Uudet muistin asetukset annettava paremmin oletukset (kuten Hortonworks suositukset). Voit muuttaa näitä, tutustu SDK-oppaat muuttamisesta palvelinklusterin määritykset. Seuraavassa taulukossa on eritelty uudet muistin asetukset, joita käytetään oletusarvon 4 suorittimen core (8 säilö) HDInsight-klusterin mukaan. (Arvot, joita on käytetty ennen tässä versiossa on myös toimitetaan parenthetically.)

<table border="1">
<tr><th>Osa</th><th>Muistin varaamista</th></tr>
<tr><td> yarn.Scheduler.Minimum kohdistus</td><td>768 Megatavua (aiemmin 512 Mt)</td></tr>
<tr><td> yarn.Scheduler.Maximum kohdistus</td><td>6144 Megatavua (muuttamatta)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 Megatavua (muuttamatta)</td></tr>
<tr><td>mapreduce.Map.Memory</td><td>768 Megatavua (aiemmin 512 Mt)</td></tr>
<tr><td>mapreduce.Map.Java.opts</td><td>Valitsee =-Xmx512m (aiemmin - Xmx410m)</td></tr>
<tr><td>mapreduce.Reduce.Memory</td><td>1536 Megatavua (aiemmin 1 024 Mt)</td></tr>
<tr><td>mapreduce.Reduce.Java.opts</td><td>Valitsee =-Xmx1024m (aiemmin - Xmx819m)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Resource</td><td>768 Megatavua (aiemmin 1 024 Mt)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Command</td><td>Valitsee =-Xmx512m (aiemmin - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 Megatavua (aiemmin 200 Mt)</td></tr>
<tr><td>tez.AM.Resource.Memory</td><td>1536 Megatavua (muuttamatta)</td></tr>

</table><br>

Saat lisätietoja kuitenkaan ja MapReduce Hortonworks tietojen ympäristössä, jota käytetään HDInsight käyttämä muistin asetukset [HDP muistin kokoonpanoasetusten määrittäminen](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks tuottaa myös työkalun laskemiseen ERISNIMI asetuksia.

Liittyy ja PowerShellin Azure Hdinsightiin SDK-virhesanoma: "*klusterin ei ole määritetty HTTP services käytön*":

* Tämä virhe on tunnettu [yhteensopivuusongelman](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , joka voi johtua HDInsight SDK-paketissa tai PowerShellin Azure version ja klusterin version ero. Klustereiden luotu 8/15 tai uudempi versio on uusi valmistelu ominaisuus kyselyjä virtual verkkojen tuki. Mutta tätä ominaisuutta ei oikein tulkita HDInsight SDK- tai PowerShellin Azure vanhemmat versiot. Tulos on joitakin projektin lähetyksen toiminnoissa epäonnistui. Jos käytät HDInsight SDK-ohjelmointirajapinnan tai Azure PowerShellin cmdlet-komennot (**Käytä AzureRmHDInsightCluster** tai **Käynnistä AzureRmHDInsightHiveJob**), voit lähettää työt, toimintoihin saattaa epäonnistua ja näyttöön tulee virhesanoma "*klusterin <clustername> ei ole määritetty HTTP services käytön*." Tai (sen mukaan, toiminto), näkyviin voi tulla muista virhesanomista, kuten "*ei voi muodostaa yhteyttä klusterin*".

* Voit ratkaista yhteensopivuusongelmien HDInsight SDK ja PowerShellin Azure uusinta versiota. On suositeltavaa päivitetään HDInsight-SDK versio 1.3.1.6 tai uudempi versio ja Azure PowerShell-työkalujen avulla versio 0.8.8 tai uudempi versio. Voit avata uusimman HDInsight: n SDK [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) ja miten [Voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md)Azure PowerShell-työkalut.



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Muistiinpanojen HDInsight 3.1 9/12/2014-versiossa

* Tässä versiossa perustuu-Hortonworks tietojen ympäristö (HDP) 2.1.5. Tässä versiossa korjaamista luettelo Tutustu artikkeliin [tässä versiossa kiinteä](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) -sivun Hortonworks sivuston.
* Possu kirjastot-kansioon on vaihdettu "avro-mapred-1.7.4.jar" tiedosto "avro-mapred-1.7.4-hadoop2.jar." Tämän tiedoston sisällön sisältää vähäisiä ohjelmavirhe korjaus, joka on sitovat. On suositeltavaa, että asiakkaat ei tehdä suora riippuvuus välttämiseksi sivunvaihdot, kun tiedostot on nimetty uudelleen PURKKI-tiedoston nimi.


## <a name="notes-for-8212014-release"></a>Huomautuksia 8/21/2014: n julkaisu

* Lisätään seuraava WebHCat määritykset (rakenne-7155), joka määrittää Templeton ohjauskoneen työn muistin oletusrajoitus 1 gigatavua. (Edellisen oletusarvo on 512 Megatavua.)

     templeton.Mapper.Memory.MB (= 1 024)

    * Tämä muutos korjaa seuraavan virheen, jotka rakenteen tietyt kyselyt oli suorittaa vuoksi alemman muistirajoitukset: "Säilö on käynnissä muistin rajoissa."
    * Palata vanha oletusarvot voit määrittää määritys-arvoksi 512 Azure PowerShellin kautta klusterin luominen aikaa käyttämällä seuraava komento:

        Lisää AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Isäntänimi zookeeper roolin on muutettu *zookeeper*. Tämä vaikuttaa nimenselvitys klusterin sisällä, mutta se ei vaikuta ulkoisen REST API. Jos sinulla on osat, jotka käyttävät *zookeepernode* isäntänimi, haluat päivittää niille uusi nimi. Kolme zookeeper solmujen uudet nimet ovat seuraavat:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase versio tuki matriisin päivitetään. Tuotannon HBase työmääriä tuetaan vain HDInsight versio 3.1 (HBase versio 0.98). (Joka on saatavilla esikatselu) 3.0-versiota ei tueta siirtäminen eteenpäin.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Huomautuksia klustereiden, jotka on luotu ennen 8/15/2014

Tai PowerShellin Azure Hdinsightiin SDK-virhesanoma "klusterin <clustername> ei ole määritetty HTTP services käytön" (tai toiminnon mukaan muiden, näyttöön tulee virhesanomia seuraavasti: "Muodostamasta yhteyttä klusterin") voidaan kohdata vuoksi versio PowerShellin Azure tai HDInsight-SDK ja klusterin välinen ero. Klustereiden luotu 8/15 tai uudempi versio on uusi valmistelu ominaisuus kyselyjä virtual verkkojen tuki. Tämä ominaisuus ei ole oikein luokittelee tai PowerShellin Azure Hdinsightiin SDK-paketissa, joka tuottaa virheitä työn lähetyksen toimintojen vanhemmat versiot. Jos käytät HDInsight SDK-ohjelmointirajapinnan tai Azure PowerShell cmdlet-komennot (esimerkiksi Käytä AzureRmHDInsightCluster tai Käynnistä AzureRmHDInsightHiveJob) voit lähettää työt, toimintoihin voi epäonnistua jokin virhesanomista, joka on kuvattu.

Voit ratkaista yhteensopivuusongelmien HDInsight SDK ja PowerShellin Azure uusinta versiota. On suositeltavaa päivitetään HDInsight-SDK versio 1.3.1.6 tai uudempi versio ja Azure PowerShell-työkalujen avulla versio 0.8.8 tai uudempi versio. Voit avata uusimman HDInsight: n SDK- [NuGet][nuget-link]. Voit käyttää Azure PowerShell-työkaluja käyttämällä [Microsoftin WWW-ympäristö asennusohjelma][webpi-link].


## <a name="notes-for-7282014-release"></a>Huomautuksia 7/28/2014: n julkaisu

* **Käytettävissä olevat kaksi uutta HDInsight**: on laajennettu HDInsight maantieteellinen tavoitettavuuden kolme alueille. HDInsight-asiakkaille, voit luoda klustereiden näillä alueilla:
    * Itä-Aasian
    * Pohjois-keskitetyn USA
    * Etelä keskitetyn USA
* HDInsight versio 1.6 (HDP 1.1 ja Hadoop 1.0.3) ja Hdinsightista versio 2.1 (HDP1.3 ja Hadoop 1.2) poistetaan Azure-portaalista. Voit edelleen luoda Hadoop klustereiden näiden versioiden Azure PowerShell cmdlet-komennolla, [Uusi AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) tai käyttämällä [HDInsight SDK-paketissa](http://msdn.microsoft.com/library/azure/dn469975.aspx). Lue lisätietoja [HDInsight osan versiotiedot](hdinsight-component-versioning.md) -sivulle.
* Tässä versiossa Hortonworks tietojen ympäristö (HDP) muutokset:

<table border="1">
<tr><th>HDP</th><th>Muutokset</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Ei muutoksia</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Ei muutoksia</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>zookeeper: ["3.4.5.2.1.3.0-1948"] ["3.4.5.2.1.3.2-0002"] -></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Huomautuksia 6/24/2014: n julkaisu

Tämä julkaisu sisältää parannuksia HDInsight-palveluun:

* **HDP 2.1 käytettävyys**: HDInsight 3.1 (joka sisältää HDP 2.1) on yleisesti saatavilla ja uusi klustereiden oletusversio.
* **HBase – Azure portaalin parannukset**: Microsoft tekee HBase klustereiden käytettävissä esikatselu. Voit luoda HBase klustereiden-portaalista vain muutamalla napsautuksella. 

HBase, jossa voit luoda erilaisia reaaliaikainen työmääriä HDInsight, valitse vuorovaikutteinen sivustoista, jotka toimivat suuria tietojoukkoja palveluihin päätepisteestä miljoonia sensor ja telemetriatietojen tietojen tallentamista. Seuraava vaihe on näistä toiminnoista Hadoop työt tietojen analysointia varten ja se on mahdollista HDInsight PowerShellin Azure ja rakenteen klusterin Raporttinäkymät-ikkunan kautta.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout esiasennettuna HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/) on asennettu valmiiksi HDInsight 3.1 Hadoop klustereiden, joten voit suorittaa Mahout töitä ilman, että muut klusterin määrittämisessä. Esimerkiksi voit remote Hadoop-klusterin Remote Desktop Protocol (RDP) avulla ja ilman lisätoimia, voit suorittaa Hei maailma Mahout seuraava komento:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Saat lisätietoja kuluttajaversion näin [Breiman Esimerkki](https://mahout.apache.org/users/classification/breiman-example.html) ohjeissa Apache Mahout-sivustossa.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Rakenne-kyselyt voi käyttää Tez HDInsight 3.1

0,13 rakenne on käytettävissä HDInsight 3.1 ja se pystyy käynnissä kyselyjen Tez, jotka voidaan hyödyntää parannat tehokkuutta.
Tez ei ole oletusarvoisesti rakenteen kyselyjen Ota käyttöön. Voit käyttää on valita, valitse. Voit ottaa Tez suorittamalla seuraavat koodikatkelman:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks on julkaistu erittelyn Tez rakenteen kyselyn suorituskykyparannukset vakio viitearvoja toimitetussa. Lisätietoja on artikkelissa [esikuva Apache rakenne 13 yrityksen Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Saat lisätietoja rakenteen käyttäminen Tez [Tez-rakenne](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Yleinen käytettävyys
HDInsight-Hadoop 2.2 versiossa Microsoft on määrittänyt HDInsight-kaikki pää paikkojen, jossa Azure on käytettävissä. Tarkemmin sanottuna Länsi Eurooppa ja varaaja Aasian palvelinkeskusten on tuotu verkossa. Näin asiakkaat Etsi klustereiden, joten, joka on lähellä ja mahdollisesti samalla yhteensopivuusvaatimukset-alueelle.


###<a name="dos--donts-between-cluster-versions"></a>Mainittuja & liitteitä päivän klusterin versioiden välillä

**Oozie metastores HDInsight 3.1-klusterin käyttämisestä on yhteensopiva ei aiempien versioiden kanssa HDInsight 2.1 klustereiden ja niitä voi käyttää tätä aiemman version kanssa**.

Mukautetun Oozie metastore tietokannan otettu käyttöön HDInsight 3.1-klusterin ei voi käyttää HDInsight 2.1-klusterin kanssa. Tämä on tapauksessa, vaikka metastore ovat peräisin HDInsight 2.1-klusterin. Tässä skenaariossa ei tueta, koska metastore rakenteen saa päivittää käytettäessä HDInsight 3.1-klusterin, joten se ei enää ole yhteensopiva HDInsight 2.1 klustereiden vaatii metastore. Yrität käyttää Oozie-metastore, jota on käytetty HDInsight 3.1-klusterin hahmonnetaan HDInsight 2.1 klusterin hyödytön.

**Oozie metastores ei voi jakaa klustereiden.**

Oozie metastores on liitetty tiettyyn klustereiden ja niitä ei voi jakaa muiden klustereiden.

###<a name="breaking-changes"></a>Tärkeimmät muutokset

**Etuliite syntaksissa**: vain "wasbs: / /" syntaksi voi käyttää HDInsight 3.1 ja 3.0 klustereiden. Vanhempi "ilmastusta: / /" syntaksi voi käyttää HDInsight 2.1 ja 1,6 klustereiden, mutta se ei tue HDInsight 3.1 tai 3.0 klustereiden. Tämä tarkoittaa, että toimitettu HDInsight-3.1 tai 3.0 klusterin töitä, jotka käyttävät erikseen "ilmastusta: / /" syntaksi epäonnistuu. "Wasbs: / /" syntaksi tulisi käyttää sen sijaan. Myös kaikki HDInsight 3.1 tai 3.0 klustereiden, jotka on luotu aiemmin metastore, joka sisältää resurssien eksplisiittisten viittauksia lähetetyt työt "ilmastusta: / /" syntaksi epäonnistuu. Nämä metastores on luotava uudelleen käyttämällä "wasbs: / /" syntaksia osoite resursseja.


**Portit**: HDInsight-palvelun porteista ovat muuttuneet. Porttinumerot, joka on käytössä on Windows-käyttöjärjestelmän tilapäiset Port (portti)-alueella. Portit varataan automaattisesti ennalta määritettyjä tilapäiset alueen lyhytkestoinen Internet protocol-pohjainen yhteyksiä varten. Sallitun Hortonworks tietojen ympäristö (HDP)-palvelun porttinumerot ovat uusia välttämiseksi encountering ristiriidoista, joita voi syntyä pään solmun palveluiden käyttämän porttien kanssa tämän alueen ulkopuolella. Uusi porttinumerot olisi aiheuta uusimmat muutokset. Käytettävät numerot ovat seuraavat:

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Nimi</th><th>Arvo</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.History.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 ja 3.0 (HDP 2.1 ja 2.0)**
<table border="1">
<tr><th>Nimi</th><th>Arvo</th></tr>
<tr><td>DFS.namenode.HTTP-osoite</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https-osoite</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP-osoite</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Riippuvuudet

Seuraavat riippuvuudet on lisätty HDInsight 3.x (HDP2.x):

* guice servlet
* optiq core
* javax.inject
* aktivointi
* jsr305
* geronimo-jaspic_1.0_spec
* Hei slf4j
* Java-xmlbuilder
* ANT
* Commons kääntäjän
* jdo-ohjelmointirajapinta
* Commons math3
* paranamer
* jaxb toteutuksen
* stringtemplate
* eigenbase xom
* Jersey servlet
* Commons suoritus
* jaxb-ohjelmointirajapinta
* jota all-palvelin
* janino
* xercesImpl
* optiq avatica
* jta
* eigenbase ominaisuudet
* groovy kaikki
* hamcrest core
* sähköposti
* linq4j
* jpam
* Jersey asiakas
* aopalliance
* geronimo-annotation_1.0_spec
* ANT avain
* Jersey guice
* XML-ohjelmointirajapinnan
* myyntivero ohjelmointirajapinta
* Asm commons
* Asm puun
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni kaikki
* nopeuden
* jettison
* snappy java
* jota kaikki
* Commons dbcp

Seuraavat riippuvuudet ei enää ole HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* muratti
* aspectjrt
* JSON
* Core
* jdo2-ohjelmointirajapinta
* avro mapred
* datanucleus lisääjinä
* jsp
* Commons-kirjaaminen-ohjelmointirajapinta
* Commons matemaattiset
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* snappy

###<a name="version-changes"></a>Muutokset

Seuraavat versiomuutokset on tehty välillä HDInsight 2.x (HDP1.x) ja Hdinsightista 3.x (HDP2.x):

* arvot-core: ["2.1.2"] ["3.0.0"] ->
* derbynet: ["10.4.2.0"] ["10.10.1.1"] ->
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* jasper kääntäjän: ["5.5.12"] ["5.5.23"] ->
* log4j: ['oleva 1.2.15', '1.2.16'] -> ["1.2.16", "1.2.17"]
* derbyclient: ["10.4.2.0"] ["10.10.1.1"] ->
* httpcore: ["4.2.4"] ["4.2.5"] ->
* hsqldb: ["1.8.0.10"] ["2.0.0"] ->
* jets3t: ["0.6.1"] ["0.9.0"] ->
* protobuf java: ["2.4.1"] ["2.5.0"] ->
* derby: ["10.4.2.0"] ["10.10.1.1"] ->
* jasper: ["Suorituksenaikainen-5.5.12'] -> [" Suorituksenaikainen-5.5.23']
* Commons daemon: ["1.0.1"] ["1.0.13"] ->
* datanucleus core: ["3.0.9"] ["3.2.10"] ->
* datanucleus-api-jdo: ["3.0.7"] ["3.2.6"] ->
* zookeeper: ["3.4.5.1.3.9.0-01320"] ["3.4.5.2.1.3.0-1948"] ->
* bonecp: ["0.7.1.RELEASE"] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Ohjaimet
SQL Serverin Java tietokannan Connnectivity (JDBC)-ohjain käytetään sisäisesti HDInsight ja ulkoisia toimintoja ei käytetä. Jos haluat muodostaa yhteyden HDInsight käyttämällä Open Database Connectivity (ODBC), käytä Microsoft rakenne ODBC-ohjain. Lisätietoja on artikkelissa [Microsoft rakenne ODBC-ohjaimella HDInsight yhdistäminen Excel](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Virheenkorjauksia

Tässä versiossa on on päivitetty seuraavista HDInsight-versioista useita virheenkorjauksia kanssa:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks julkaisutiedot

Hortonworks tietojen ympäristöissä (HDPs), jotka käyttävät HDInsight versio klustereiden julkaisutiedot ovat käytettävissä seuraavista sijainneista:

* HDInsight versio 3.1 käyttää Hadoop-normaalijakaumaa, joka perustuu [Hortonworks tietojen ympäristö 2.1.7][hdp-2-1-7]. Tämä on oletusarvo Hadoop-klusterin luomisen avulla Azure portaalin 11/7/2014 jälkeen. Ennen 11/7/2014 olisivat [Hortonworks tietojen ympäristö 2.1.1] HDInsight 3.1 klustereiden[hdp-2-1-1]

* HDInsight 3.0-versiota käyttää Hadoop-normaalijakaumaa, joka perustuu [Hortonworks tietojen ympäristö 2.0][hdp-2-0-8].

* HDInsight versio 2.1 käyttää Hadoop-normaalijakaumaa, joka perustuu [Hortonworks tietojen ympäristö 1.3][hdp-1-3-0].

* HDInsight versio 1.6 käyttää Hadoop-normaalijakaumaa, joka perustuu [Hortonworks tietojen ympäristö 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
