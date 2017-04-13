<properties
    pageTitle="R HDInsight-ominaisuudet Johdanto R palvelimeen HDInsight (ennakkoversio) | Microsoft Azure"
    description="Mikä on HDInsight (ennakkoversio) ja Opi käyttämään R palvelimen suuri Tietoanalyysi sovellusten luomisen R-palvelimeen."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Yleistä R HDInsight-palvelimelle \(esikatselu\)

Microsoft Azure Hdinsightiin Premium-ja Microsoft R palvelin on nyt käytettävissä vaihtoehtona luodessasi Azure Hdinsightiin klustereiden. Uusi ominaisuus voi sisältää tietojen tutkijoiden ilmiöjoukosta ja R ohjelmoijille skaalattava, voit tarvittaessa käytön distributed maksutapojen analytics-Hdinsightista.

Klustereiden voit esitystapa projekteihin ja tehtäviin mukaan ja poistettu kun olet ei enää tarvita. Koska ne ovat osa Azure Hdinsightiin, näiden klustereiden sisältää yritystason 24/7 tuki, 99,9 % käytettävyyttä SLA ja joustavuutta integroida Azure ekosysteemissä muita osia.

>[AZURE.NOTE] R palvelin on käytettävissä vain HDInsight Premium. Tällä hetkellä HDInsight Premium on käytettävissä vain Hadoop ja ohjattu klustereiden. Tällä hetkellä voi käyttää R palvelimen vain Hadoop ja ohjattu klustereiden HDInsight. Lisätietoja on artikkelissa [eri palvelutasot ja Hadoop osien käytettävissä Hdinsightista ovat?](hdinsight-component-versioning.md).

R HDInsight-palvelimelle sisältää uusimmat ominaisuudet R-pohjainen analysoinnissa suuria tietojoukkoja, jotka on ladattu Azure-Blob-säiliö. Koska R-palvelimeen on muodostettu Avaa lähde R, R-pohjaisten sovellusten, voit luoda voidaan hyödyntää jokin 8000 + Avaa lähde R-pakettien sekä ScaleR, Microsoftin big datasta analytics paketti, joka sisältää R palvelimen käytöstä.

Reuna-solmu Premium klustereiden on kätevä muodostaa yhteyttä klusterin ja R-komentosarjojen suorittamisen. Reuna-solmu, jossa voit halutessasi suorittaa ScaleR's parallelized hajautettu Funktiot sydämiä reunan solmu-palvelimen kautta. Halutessaan suorittaa ne klusterin solmujen välillä käyttämällä ScaleR's Hadoop kartan pienentäminen tai ohjattu Laske kontekstit.

Mallien tai ennusteiden tuloksen analyyseja voi ladata, käytä paikallista. Ne voivat myös voidaan operationalized muualla Azure-tietokannassa, kuten [Azure koneen Learning Studio](http://studio.azureml.net) - [WWW-palvelun](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md)kautta.

## <a name="get-started-with-r-on-hdinsight"></a>R HDInsight-käytön aloittaminen

Sisällytettävät HDInsight-klusterin R palvelimeen sinun on luotava Hadoop tai Ohjattu klusterin Premium-vaihtoehto luodessasi klusterin Azure-portaalissa. Klusterin molempien tietotyyppien käyttää samaa kokoonpanoa, mukaan lukien R palvelimen tietojen solmut klusterin ja reuna-solmu R palvelinpohjaiset analytics purkamisen vyöhykkeen nimellä. Saat tarkempia hallintapaketteihin, luomisesta klusterin [Käytön aloittaminen HDInsight R-palvelimeen](hdinsight-hadoop-r-server-get-started.md) .

## <a name="learn-about-data-storage-options"></a>Lisätietoja tietojen säilytysasetukset

HDInsight varausyksiköiden oletusarvon tallennustila on yhdistetty blob-säilö HDFS tiedostojärjestelmässä Blob-objektien tallennustilaan. Näin varmistat, että jostakin tiedot on ladattu klusterin tallennustilan tai kirjoittamisen aikana analyysi-klusterin tallennustilan tehdään jatkuva. Jos haluat kopioida tiedot ja sieltä pois blob [AzCopy](../storage/storage-use-azcopy.md) -apuohjelmalla.

Lisäksi Blob-objektien tallennustilaan, voit halutessasi [Azure järvi tietosäilö](https://azure.microsoft.com/services/data-lake-store/) käytön yhteyttä klusterin. Jos käytät tietojen järvi, valitse voit Blob-säiliö ja tietojen Lake HDFS tallennuksessa.

Voit käyttää myös [Azure tiedostojen](../storage/storage-how-to-use-files-linux.md) tallennustila-asetus käyttöön reuna-solmu. Azure tiedostojen avulla voit ottaa käyttöön jaetun tiedoston, joka on luotu Azuren tallennustilaan Linux-tiedostojärjestelmässä. Lisätietoja tietojen säilytysasetukset R palvelimen HDInsight-klusterissa on [säilytysasetukset HDInsight klustereiden R-palvelimeen](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Klusterin Access R-palvelin

Kun olet luonut klusterin R-palvelimen kanssa, voit muodostaa yhteyden SSH/painovärit, muste klusterissa reuna-solmu R-konsolin. Voit myös yhdistää selaimen kautta, jos haluat asentaa valinnainen RStudio palvelimen IDE reuna-solmu. Lisätietoja RStudio asentamisesta on kohdassa [Asentaminen RStudio palvelimen HDInsight klustereiden](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Kehittää ja R-komentosarjojen suorittamisen

R-komentosarjojen luominen ja suorittaminen käyttää 8000 + Avaa lähde R-paketti lisäksi parallelized ja hajautettu rutiinit ScaleR-kirjastossa. Yleensä komentosarjan, joka suoritetaan R Serverissä reuna-solmu suoritetaan R kääntäjän-solmun. Poikkeus on nämä vaiheet, jotka ScaleR Soita toimivan Laske kontekstin minkälainen määrittäminen Hadoop kartan vähentämiseksi (RxHadoopMR) tai ohjattu (RxSpark).

Tällöin funktio suoritetaan hajautettu tavalla yli näiden tietojen klusterin (tehtävä) solmut, joka liittyy viitatun tietoja. Lisätietoja eri Laske konteksti-asetuksista on artikkelissa [Laske HDInsight Premium R-palvelimen asetusten kontekstissa](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operationalize malli

Kun tietojen mallinnus on valmis, voit operationalize mallin, jotta uudet tiedot sekä Azuren ja paikallisten ennusteiden. Tämä toimenpide on nimeltään näkyvissä pistemäärä. Seuraavassa on muutamia esimerkkejä.

### <a name="score-in-hdinsight"></a>Kirjainarvosanan Hdinsightiin

Pisteet HDInsight-, kirjoita R-funktio, joka kutsuu tehdä uudesta datatiedostosta, joka on ladattu tallennustilan tiliisi ennusteiden mallin. Tallenna ennusteiden takaisin tallennustilan tilin. Reuna-solmu yhteyttä klusterin tai ajoitetun työn avulla voit suorittaa säännöllisestä pyydettäessä.  

### <a name="score-in-azure-machine-learning"></a>Kirjainarvosanan Azure koneen oppiminen

Pisteet Azure koneen Learning WWW-palvelun avulla, voit julkaista mallin Azure web-palvelu käyttämällä Avaa lähde Azure koneen Learning R-paketti, nimeltään [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) . Asiasta tämä paketti on asennettu valmiiksi reuna-solmun. Seuraavaksi tilat ja tietokoneen Learning avulla voit luoda käyttöliittymän WWW-palvelun ja kutsu WWW-palvelun näkyvissä pistemäärä tarvittaessa.

Jos valitset tämän vaihtoehdon, tarvitset ScaleR mallin objekteja muuntaminen vastaavat Avaa lähde-mallin objekteista WWW-palvelun kanssa käytettäväksi. Tämän voi tehdä käyttämällä ScaleR pakotus Funktiot, kuten `as.randomForest()` ensemble perustuva malleille.


### <a name="score-on-premises"></a>Paikalliseen pistemäärä

Pisteet paikalliseen mallin luomisen jälkeen, voit muuntaa sarjaksi R mallin, lataa se, onnistu poistaa sen ja käyttää sitä sitten näkyvissä pistemäärä uudet tiedot. Uudet tiedot voit sija kuvatulla [HDInsight pistelasku](#scoring-in-hdinsight) menetelmän avulla tai käyttämällä [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Ylläpidä klusterin

### <a name="install-and-maintain-r-packages"></a>Asentaa ja ylläpitää R-paketit

R-paketteja, jota käytetään yleensä tarvitaan reuna-solmu, koska useimmat R-komentosarjat suoritetaan siellä. Asennetaan uusia R-pakettien reuna-solmu, voit käyttää tavallisen `install.packages()` menetelmä R.

Useimmissa tapauksissa ei tarvitse asentaa muita R-pakettien tietojen solmuissa käytettäessä vain rutiinit ScaleR kirjastosta klusterin yli. Voit joutua kuitenkin muita pakettien tukemaan tietojen solmuissa **rxExec** tai **RxDataStep** suorittamisen.

Tällaisissa tapauksissa Lisää paketit on määritettävä komentosarja-toiminnon avulla klusterin luomisen jälkeen. Lisätietoja on artikkelissa [luominen HDInsight-klusterin R-palvelimen kanssa](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop kartan vähentää muistin asetusten muuttaminen

Klusterin voi muokata, joka on käytettävissä R-palvelimeen, kun se suoritetaan kartan Pienennä työn muistin muuttamiseen. Voit muokata klusterin käyttää Apache Ambari-Käyttöliittymä, joka on saatavana yhteyttä klusterin Azure portaalin sivu. Ohjeita siitä, miten voit käyttää yhteyttä klusterin Ambari Käyttöliittymän on artikkelissa [Hallitse HDInsight klustereiden Ambari Web-Käyttöliittymän avulla](hdinsight-hadoop-manage-ambari.md).

On myös mahdollista muuttaa muistin, joka on käytettävissä R palvelimen käyttämällä Hadoop valitsimet **RxHadoopMR** puhelun seuraavasti:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Skaalaa yhteyttä klusterin

Aiemmin luodun klusterin voi skaalata ylös tai alas portaalin kautta. Skaalauksen, voit käyttää muita resursseja, sinun on ehkä suurempi käsittely tehtävien tai voit skaalata takaisin klusterin kun ei käytetä. Saat ohjeet skaalata klusterin, [hallinta HDInsight klustereiden](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Ylläpidä järjestelmä

Ylläpito suoritetaan pohjana Linux VMs, HDInsight-klusterin off-hours OS korjaukset ja muiden päivitysten aikana. Yleensä ylläpito tehdään 3:30 Suomen (perustuvat paikallista aikaa, jos AM), joka maanantai ja torstai. Päivitykset suoritetaan siten, että ne eivät vaikuttaa yli vuosineljänneksen klusterin kerrallaan.  

Koska pään solmut ovat tarpeettomia ja kaikki tiedot solmujen vaikuttaa, työt, joissa on käytössä tänä aikana voi hidastaa. Ne olisi silti suorittaa loppuun, mutta. Kaikki mukautetut ohjelmistoa tai paikalliset tiedot, joihin sinulla on säilytetään yli ylläpito näitä tapahtumia, ellei kriittinen virhe ilmenee, joka edellyttää klusterin muodosta.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Tutustu erilaisiin IDE R palvelimen HDInsight-klusterissa

HDInsight Premium-klusterin Linux reuna-solmu on purkamisen vyöhykkeen R-pohjainen analyysia varten. Kun klusteriin, voit käynnistää R palvelimeen console-liittymän kirjoittamalla **R** Linux komentokehotteen. Console-käyttöliittymän käyttöä on laajennettu, jos suoritat tekstieditorissa R komentosarjan kehittämiseen toisessa ikkunassa ja leikkaa ja liitä komentosarja osiin R-konsolin tarpeen mukaan.

R-komentosarjan kehittämiseksi kehittyneempiä työkalu on käytettäväksi työpöydällä, R-pohjainen IDE, kuten Microsoftin viimeksi ilmoitettiin [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Tämä on perhe [RStudio](https://www.rstudio.com/products/rstudio-server/)työpöydän ja palvelimen työkaluja. Voit käyttää myös Walware's Pimennys perustuva [StatET](http://www.walware.de/goto/statet).

Toinen vaihtoehto on IDE asennetaan Linux reuna-solmu itse.  Suositut valinta on [RStudio palvelimeen](https://www.rstudio.com/products/rstudio-server/), joka sisältää selainpohjaisia IDE remote asiakkaiden. RStudio Serverin asentamisesta HDInsight Premium-klusterin reuna-solmu sisältävät koko IDE kehittämisen ja R-komentosarjojen R-palvelimen kanssa klusterin suorittaminen. Voi olla huomattavasti tuottavuutta kuin R-konsolin.  Jos haluat käyttää RStudio Server, katso [Asentaminen RStudio palvelin HDInsight klustereiden](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Lisätietoja hinnat

Vakio HDInsight-klustereiden maksuja rakenne maksut, jotka liittyvät HDInsight Premium-klusterin R-palvelimen kanssa samalla tavalla. Ne perustuvat pohjana VMs koonmuuttokahvan nimi, tiedot ja reunan solmujen, johon on lisätty core tunnin tehtävä Premium. Lisätietoja HDInsight Premium hinnoittelu, mukaan lukien hinnat Public Preview ja 30 päivän maksuton käytettävyyttä aikana Katso [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja alla olevista linkeistä R-palvelimen käyttäminen HDInsight klustereiden tietoja.

- [Käytön aloittaminen R HDInsight-palvelimelle](hdinsight-hadoop-r-server-get-started.md)

- [Lisää RStudio palvelin HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Laske kontekstin asetukset R palvelimen HDInsight (ennakkoversio)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure tallennustilan asetukset R palvelimen HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
