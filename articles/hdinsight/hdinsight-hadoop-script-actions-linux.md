<properties
    pageTitle="Komentosarjan toiminnon kehitys HDInsight Linux-pohjaiset | Microsoft Azure"
    description="Voit mukauttaa Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnolla. Komentosarjatoiminnot ovat voi mukauttaa Azure Hdinsightiin klustereiden klusterin kokoonpanon määrittäminen tai lisäpalveluja, työkalut tai muiden ohjelmistojen asentaminen klusterin. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Komentosarjan toiminnon kehitys HDInsight

Komentosarjatoiminnot ovat voi mukauttaa Azure Hdinsightiin klustereiden klusterin kokoonpanon määrittäminen tai lisäpalveluja, työkalut tai muiden ohjelmistojen asentaminen klusterin. Voit käyttää komentosarjatoiminnot klusterin luonnin aikana tai käynnissä klusterissa.

> [AZURE.NOTE] Tässä asiakirjassa on Linux-pohjaiset HDInsight klustereiden. Lisätietoja komentosarjatoiminnot käyttämisestä Windows-pohjaisesta klustereiden on artikkelissa [komentosarjan toiminnon kehitys HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Mitkä ovat komentosarjatoiminnot?

Komentosarjatoiminnot ovat Bash komentosarjoja, joka suoritetaan Azure klusterin solmuissa Tee määritysmuutoksia tai asenna ohjelmisto. Komentosarja-toiminto suoritetaan pääkansio ja klusterisolmut täydet oikeudet.

Komentosarjatoiminnot voi suojata – seuraavista tavoista:

| Tämän toiminnon avulla voit käyttää komentosarjan... | Klusterin luonnin aikana... | Käynnissä olevat klusterissa... |
| ----- |:-----:|:-----:|
| Azure Portal | ✓ | ✓ |
| Azure PowerShell | ✓ | ✓ |
| Azure CLI | &nbsp; | ✓ |
| HDInsight .NET SDK-paketissa | ✓ | ✓ |
| Azure Resurssienhallinta-malli | ✓ | &nbsp; |

Lisätietoja näillä tavoilla voit käyttää komentosarjatoiminnot-kohdassa [Mukauta HDInsight klustereiden komentosarja-toimintojen käyttäminen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Komentosarjan kehittäminen parhaat käytännöt

Kun kehität HDInsight-klusterin mukautettu komentosarja, käytettävissä on useita parhaita käytäntöjä, kannattaa pitää mielessä:

- [Kohteen Hadoop-versio](#bPS1)
- [Kohdeluettelo-käyttöjärjestelmän versio](#bps10)
- [Koonneet vakaata komentosarja-resurssit](#bPS2)
- [Käytä valmiiksi käännetty resursseja](#bPS4)
- [Varmista, että klusterin mukauttaminen komentosarja on idempotent](#bPS3)
- [Varmista klusterin arkkitehtuuri suuri käytettävyys](#bPS5)
- [Määritä mukautettujen osien käyttämään Azure-Blob-säiliö](#bPS6)
- [Kirjoita tiedot STDOUT ja STDERR](#bPS7)
- [Tiedostojen tallentaminen ASCII LF rivin päätteet kanssa](#bps8)
- [Yritä funktioiden avulla voit palauttaa lyhytkestoisia virheet](#bps9)

> [AZURE.IMPORTANT] Komentosarjatoiminnot on suoritettava 60 minuutin kuluessa tai ne on aikakatkaisu. Solmun valmistelun aikana komentosarja on suorittanut samanaikaisesti muiden asennus ja määritys-prosessien. Resurssit, kuten suorittimen aika tai verkon kaistanleveyden kilpailua saattaa aiheuttaa komentosarjan kestää kauemmin kuin kehittäminen-ympäristössä.

### <a name="bPS1"></a>Kohteen Hadoop-versio

Eri versioiden Hdinsightista on Hadoop-palveluiden ja -osien asennettu eri versioita. Jos komentosarja odottaa tietyn osan tai palvelu-version, käytä vain komentosarja, joka sisältää tarvittavat osat HDInsight-version kanssa. Voit etsiä osan versioista HDInsight sisältyvät tiedot käyttämällä [HDInsight osan versiotietojen](hdinsight-component-versioning.md) asiakirjaa.

###<a name="bps10"></a>Kohdeluettelo-käyttöjärjestelmäversio

Linux-pohjaiset HDInsight perustuu Ubuntu Linux-jakauman. Eri versioiden Hdinsightista ovat riippuvaisia Ubuntu, jotka saattavat vaikuttaa miten komentosarja toimii eri versioita. Esimerkiksi Ubuntu-versiot, jotka käyttävät Upstart perustuvat HDInsight 3,4 ja aiempi versio. Version 3.5 perustuu Ubuntu 16.04, joka käyttää Systemd. Systemd ja Upstart luottavat eri komentoja, jotta komentosarja on kirjoitettava molempien käyttöä varten.

Toisen tärkeitä HDInsight 3.4 ja 3.5 eroavat toisistaan, `JAVA_HOME` viittaa nyt Java 8.

Voit tarkistaa käyttöjärjestelmäversio käyttämällä `lsb_release`. Seuraava koodikatkelmat, sävyä asennuksen komentosarjan esittelee tietoja sen selvittämisestä, onko komentosarja käynnissä Ubuntu 14 tai 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Voit etsiä koko komentosarjan, joka sisältää näitä katkelmat osoitteessa https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Ubuntu, jota käytetään HDInsight-versiota Katso [HDInsight osan version](hdinsight-component-versioning.md) asiakirjan.

Mitä eroa Systemd ja Upstart on artikkelissa [Systemd Upstart käyttäjille](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Koonneet vakaata komentosarja-resurssit

Varmista että komentosarjat ja komentosarjan käyttämät resurssit ovat käytettävissä klusterin elinkaaren ja että näiden tiedostojen versiot eivät muutu toistaminen. Nämä resurssit ovat pakollisia, jos uusien solmujen lisätään klusterin aikana skaalaus toimintoja.

Paras käytäntö on ladattava ja arkistoida kaikki Azure-tallennustilan tilin tilauksen.

> [AZURE.IMPORTANT] Tallennustilan tilillä on oltava tallennustilan oletustili klusterin tai julkinen, vain luku-säilön tallennustilan-tilissä.

Esimerkiksi Microsoftin mallit tallennetaan [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) tallennustilan-tili, mikä on julkinen, vain luku-säilö ylläpitämä HDInsight-ryhmän.

### <a name="bPS4"></a>Käytä valmiiksi käännetty resursseja

Voit vähentää komentosarjan suorittamiseen kuluvaa aikaa, vältä toiminnoista, joita kääntää lähdekoodin resursseilla. Sen sijaan Käännä valmiiksi resurssit ja tallentaa binaarinen versio Azure-Blob-säiliö niin, että se nopeasti voi ladata klusteriin komentosarja.

### <a name="bPS3"></a>Varmista, että klusterin mukauttaminen komentosarja on idempotent

Komentosarjojen on suunniteltava on idempotent mielessä, jos komentosarja on suorittanut useita kertoja, varmistaa, että klusterin palautetaan samaan tilaan aina, kun se on suorittanut.

Esimerkiksi jos mukautettu komentosarja asentaa sovelluksen /usr/local/bin sen ensin Suorita ja valitse kunkin myöhemmin suorittamisen komentosarja tarkistaa, onko sovelluksessa on jo olemassa /usr/local/bin sijainnissa ennen jatkamista muut vaiheet komentosarja.

### <a name="bPS5"></a>Varmista klusterin arkkitehtuuri suuri käytettävyys

Linux-pohjaiset HDInsight klustereiden sisältää kaksi pään solmut, jotka eivät sisällä klusterin aktiivisia ja toiminnot ovat komentosarja suoritettiin molempien solmujen. Jos olet asentanut osat vain yksi pään solmu, suunnittelet komentosarjan, joka vain Asenna osa johonkin klusterin kaksi pään solmujen.

> [AZURE.IMPORTANT] Oletusarvo-palvelut asennetaan Hdinsightista on suunniteltu haluamallasi tavalla, mutta tämä toiminto ei ole laajennettu mukautettu asennettu kautta komentosarjan ctions osien kaksi pään solmujen välillä päälle epäonnistuu. Jos tarvitset komponentteja asennettuna on erittäin käytettävissä komentosarja-toiminnon avulla, oma automaattisesti järjestelmä, joka käyttää kaksi pään solmujen on pantava täytäntöön.

### <a name="bPS6"></a>Määritä mukautettujen osien käyttämään Azure-Blob-säiliö

Osat, jotka olet asentanut klusterin joutua, joka käyttää Hadoop Distributed (HDFS)-järjestelmän tallennustilan oletusasetukset. HDInsight käyttää Azure Blob Storage (WASB) oletusarvo-tallennustilan. Tämä on HDFS yhteensopiva tiedostojärjestelmässä, jatkuu tietoja, vaikka klusterin poistetaan. Voit käyttää WASB HDFS sijaan asentaa osat kannattaa määrittää.

Esimerkiksi seuraavat kopioi giraph examples.jar tiedoston paikallisen tiedostojärjestelmän WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Kirjoita tiedot STDOUT ja STDERR

Komentosarjojen suorittamisen aikana STDOUT ja STDERR kirjoitettujen tiedot kirjataan ja voi tarkastella Ambari web-Käyttöliittymä.

> [AZURE.NOTE] Ambari on vain, jos klusterin on luotu. Jos käytät komentosarja-toiminnon klusterin luonnin aikana ja luominen epäonnistuu, saat muita tapoja käyttää kirjautuneena tietoja vianmääritys-osassa [Mukauta HDInsight klusterit komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) .

Useimmat apuohjelmat ja asennuksen paketteja jo kirjoittaa tiedot STDOUT ja STDERR, mutta voit halutessasi lisätä uusia kirjaaminen. Tekstin lähettäminen STDOUT käytön `echo`. Esimerkki:

        echo "Getting ready to install Foo"

Oletusarvon mukaan `echo` lähettää merkkijonon STDOUT. Jos haluat ohjata kohteeseen STDERR, Lisää `>&2` ennen `echo`. Esimerkki:

        >&2 echo "An error occurred installing Foo"

STDOUT (1, joka on oletusarvo, sitä ei ole mainittu tässä,)-sovellukseen lähetettyjä tietoja ohjaa STDERR (2). Lisätietoja IO uudelleenohjaus on artikkelissa [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Katso lisätietoja tarkastelemisesta komentosarjatoiminnot tallentamat tiedot [mukauttaminen HDInsight klustereiden komentosarja-toiminnon käyttäminen](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>Tiedostojen tallentaminen ASCII LF rivin päätteet kanssa

Bash komentosarjoja pitää tallentaa ASCII-muoto, viivoilla LF lopetettu. Jos tiedostot tallennetaan UTF-8, jotka voivat sisältää DBCS-tilauksen Merkitse alussa tiedoston tai rivin päätteet, CRLF, joka on yhteinen Windows editors, komentosarja epäonnistuu virheellisten seuraavankaltaiselta:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Yritä funktioiden avulla voit palauttaa lyhytkestoisia virheet

Kun lataamisen asentaminen pakettien piharakennus get tai muita toimintoja, lähettää tietoja Internetin kautta, toiminto saattaa epäonnistua lyhytkestoisia verkko virheiden vuoksi. Esimerkiksi remote resurssi on yhteydessä, saatetaan varmuuskopion solmun epäonnistumista päälle.

Jos haluat tehdä komentosarjan joustavat lyhytkestoisia virheitä, voit toteuttaa uudelleen logiikan. Seuraavassa on esimerkki funktiota, joka suorittaa siihen välitetty komentoja ja (Jos komento epäonnistuu,) uudelleen enintään kolme kertaa. Se odottaa kahden sekunnin ajan välillä kunkin uudelleen.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Seuraavassa on esimerkkejä tämän funktion käyttämisestä.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Mukautettujen komentosarjojen Helper menetelmät

komentosarjan toiminnon helper menetelmät ovat apuohjelmia, jota voit käyttää mukautettuja komentosarjoja kirjoitettaessa. Nämä [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)määritetään ja voidaan lisätä komentosarjojen avulla seuraavasti:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Tämä on seuraavia apuohjelmia käytettäväksi komentosarja:

| Avustaja-käyttö | Kuvaus |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Lataa tiedosto lähteen URL-osoite määritetty tiedostopolku. Oletusarvon mukaan se ei korvaa aiemmin luotu tiedosto. |
| `untar_file TARFILE DESTDIR` | Purkaa tar-tiedoston (käyttämällä `-xf`,) kohdekansioon. |
| `test_is_headnode` | Jos suoritettiin klusterin pään solmun, funktio palauttaa arvon 1. Muussa tapauksessa 0. |
| `test_is_datanode` | Jos nykyinen solmu (työntekijän) tietojen solmu, palauttaa arvon 1; Muussa tapauksessa 0. |
| `test_is_first_datanode` | Jos nykyinen solmu on ensimmäinen tiedot (työntekijän) solmu (nimeltään workernode0) palauttaa arvon 1; Muussa tapauksessa 0. |
| `get_headnodes` | Palauttaa klusterin headnodes täydellinen toimialuenimi. Nimet ovat luetteloerotin. Virhe palautetaan tyhjä merkkijono. |
| `get_primary_headnode` | Saa ensisijainen headnode täydellinen toimialuenimi. Virhe palautetaan tyhjä merkkijono. |
| `get_secondary_headnode` | Saa toissijainen headnode täydellinen toimialuenimi. Virhe palautetaan tyhjä merkkijono. |
| `get_primary_headnode_number` | Saa ensisijainen headnode numeerinen liitteen. Virhe palautetaan tyhjä merkkijono. |
| `get_secondary_headnode_number` | Saa toissijainen headnode numeerinen liitteen. Virhe palautetaan tyhjä merkkijono. |

## <a name="commonusage"></a>Yleiset käyttötavat

Tässä osassa on ohjeet yksityiskohtaisista joitakin yleisiä käyttötavat, joita saattaa ilmetä mukautettua komentosarjaa kirjoitettaessa.

### <a name="passing-parameters-to-a-script"></a>Parametrien siirtäminen komentosarja

Joissakin tapauksissa komentosarja voi vaatia parametrit. Klusterin järjestelmänvalvojan salasanan koota voi hakea tietoja Ambari REST-Ohjelmointirajapinnalla.

Komentosarjan välitetään parametrit on nimeltään _sijainti parametrit_ja määritetyt `$1` ensimmäiseen parametrin `$2` toisella, ja siten-on. `$0`sisältää itse komentosarja nimen.

Välittää komentosarjan parametrien arvot olisi kirjoitettava puolilainausmerkkeihin (') niin, että välitetty arvo tulkitaan literaalimerkit erityistä käsittelyä ei ole annettu sisältää merkkejä, kuten '! ".

### <a name="setting-environment-variables"></a>Ympäristömuuttujat määrittäminen

Ympäristömuuttuja määrittäminen tehdään seuraavasti:

    VARIABLENAME=value

Missä VARIABLENAME muuttujan nimi. Voit käyttää muuttujan tämän jälkeen, `$VARIABLENAME`. Esimerkiksi määrittämään SALASANAN nimeltä ympäristömuuttuja taulukkona sijainti-parametrin arvon vakiomittoja seuraavasti:

    PASSWORD=$1

Seuraavien tietojen käyttöä voi käyttää `$PASSWORD`.

Ympäristömuuttujat määrittäminen komentosarja olemassa vain komentosarja puitteissa. Joissakin tapauksissa joudut ehkä lisätä järjestelmän leveä ympäristömuuttujia, joka on käytössä, kun komentosarja on päättynyt. Tämä on yleensä niin, että yhteyttä klusterin SSH kautta käyttäjät voivat käyttää komentosarjan asentama osat. Voit tehdä tämän lisäämällä ympäristömuuttuja `/etc/environment`. Esimerkiksi seuraava Lisää __HADOOP\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Mukautettujen komentosarjojen tallennuspaikkaa sijaintien käyttö

Komentosarjojen avulla voit mukauttaa klusterin on joko olevan tallennustilan oletustili klusterin tai, jos toisessa tallennustilan tilin julkisen vain luku-säilössä. Jos komentosarja käyttää muualla resurssit nämä on myös kaikkien käytettävissä olevan (vähintään julkisen vain luku). Esimerkiksi voit ladata tiedoston klusterin käyttämällä `download_file`.

Tallentaa tiedoston käytettävissä Azure-tallennustilan tilin mukaan (esimerkiksi tallennustilan oletustilin) klusterin, toimi nopeasti, sillä tämä on Azure verkoston sisällä.

### <a name="checking-the-operating-system-version"></a>Käyttöjärjestelmäversion tarkistaminen

Eri versioiden Hdinsightista ovat riippuvaisia Ubuntu tiettyjä versioita. Voi olla käyttöjärjestelmä, joka on etsittävä komentosarjan erot. Esimerkiksi haluat asentaa binaaritiedosto, joka on sidottu Ubuntu versio.

Voit tarkistaa käyttöjärjestelmäversio `lsb_release`. Esimerkiksi seuraavat kerrotaan, miten haluat viitata eri tar tiedoston käyttöjärjestelmän version mukaan:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Käyttöönoton komentosarja-toiminnon tarkistusluettelo

Näin on tapahtunut valmisteltaessa ottamaan komentosarjat:

- Sijoita tiedostoja, jotka sisältävät mukautetut komentosarjat paikkaa, jossa on käytettävissä klusterin solmut käyttöönoton aikana. Tämä voi olla mikä tahansa oletus-tai määrittää tällä hetkellä klusterin käyttöönottoa tai muita kaikkien käytettävissä olevan tallennustilan säilö tallennustilan tilejä.

- Lisää tarkistukset komentosarjoja, varmista, että he suorittavat impotently, jolloin komentosarja voidaan suorittaa useita kertoja, saman solmun kyselyjä.

- Tilapäinen tiedosto hakemisto-/tmp avulla voit pitää komentosarjat käyttämä ladatut tiedostot ja sitten Puhdista ne sen jälkeen, kun komentosarjat on suoritettu.

- Silloin, kun OS tason asetukset tai Hadoop service määritysten tiedostot ovat muuttuneet, haluat ehkä HDInsight-palveluiden uudelleenkäynnistäminen niin, että ne Nosta OS tason asetuksia, kuten määrittäminen komentosarjat ympäristössä muuttujat.

## <a name="runScriptAction"></a>Komentosarja-toiminnon suorittaminen

Komentosarjatoiminnot avulla voit mukauttaa HDInsight klustereiden Azure Azure PowerShellin Azure resurssien hallinta (ARM) malleja tai HDInsight .NET SDK-portaalissa. Katso ohjeet [komentosarja-toiminnon käyttämisestä](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Mukautetun komentosarjamallit

Microsoft toimittaa komentosarjamalleja components asennetaan HDInsight-klusterin. Komentosarjamallit ja ohjeet niiden käyttämisestä on osoitteessa vaihtoehdoista:

- [Asentaminen ja käyttäminen sävyä HDInsight klustereiden](hdinsight-hadoop-hue-linux.md)
- [Asentaminen ja käyttäminen HDInsight Hadoop klustereiden R](hdinsight-hadoop-r-scripts-linux.md)
- [Asentaminen ja käyttäminen Solr HDInsight klustereiden](hdinsight-hadoop-solr-install-linux.md)
- [Asentaminen ja käyttäminen Giraph HDInsight klustereiden](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Edellä linkitetyt tiedostot ovat Linux-pohjaiset HDInsight klustereiden. Saat komentosarjoja, jotka toimivat Windows-pohjaisesta HDInsight- [komentosarjan toiminnon kehitys HDInsight (Windows)](hdinsight-hadoop-script-actions.md) tai käytä käytettävissä linkkejä kunkin artikkelin yläreunassa.

##<a name="troubleshooting"></a>Vianmääritys

Seuraavassa on virheitä saattaa ilmetä on kehittänyt komentosarjojen avulla:

__Error__: `$'\r': command not found`. Jäljessä `syntax error: unexpected end of file`.

_Syy_: Tämä virhe aiheutuu, kun komentosarja rivit pääty CRLF. UNIX-järjestelmiä olettaa vain LF kuin rivin loppuun.

Tämä ongelma ilmenee useimmin kun komentosarja on luotu Windows-ympäristöön CRLF on päättymässä monta tekstin editors Windows yleisiä viivan.

_Ratkaisu_: Jos vaihtoehdon tekstieditori, valitse Unix-muodossa tai LF rivin loppuun. Voi myös käyttää seuraavia komentoja Unix-järjestelmässä voit muuttaa CRLF LF:

> [AZURE.NOTE] Seuraavat komennot vastaavat suurin piirtein, siten, että ne vaihdetaan CRLF rivin päätteet LF. Valitse jokin käytettävissä järjestelmässä apuohjelmien perusteella.

| Komento | Huomautuksia |
| ------- | ----- |
| `unix2dos -b INFILE` | Alkuperäinen tiedosto varmuuskopioidaan kanssa. BAK tunniste |
| `tr -d '\r' < INFILE > OUTFILE` | OUTFILE sisältää vain LF päätteet sisältävä versio |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Tämä muuttaa tiedoston suoraan ilman uuden tiedoston luominen |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | OUTFILE sisältää vain LF päätteet sisältävä versio.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Syy_: Tämä virhe ilmenee, kun komentosarja on tallennettu UTF-8-tavuinen tilauksen Merkitse (Tuoterakenne) kanssa.

_Ratkaisu_: Tallenna tiedosto ASCII-muodossa tai UTF-8 ilman Tuoterakenteen nimellä. Voi myös luoda uuden tiedoston ilman Tuoterakenteen Linux tai Unix-järjestelmään seuraava komento avulla:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Korvaa yllä olevan komennon __SYÖTTÖTIEDOSTO__ Tuoterakenteen sisältävä tiedosto. __OUTFILE__ on oltava uuden tiedostonimi, joka sisältää ilman Tuoterakenteen komentosarja.

## <a name="seeAlso"></a>Seuraavat vaiheet

* Lue, miten [mukauttaminen HDInsight klustereiden komentosarja-toiminnon käyttäminen](hdinsight-hadoop-customize-cluster-linux.md)

* Käytä [HDInsight .NET SDK viittaus](https://msdn.microsoft.com/library/mt271028.aspx) lisätietoja, joilla hallitaan HDInsight .NET-sovellusten luominen

* Opettele käyttämään muiden hallinta-toimintojen suorittamiseen HDInsight klustereiden [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) avulla.
