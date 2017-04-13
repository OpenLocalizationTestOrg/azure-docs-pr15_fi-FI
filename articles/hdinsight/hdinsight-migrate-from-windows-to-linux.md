<properties
pageTitle="Siirtää Windows-pohjaisesta HDInsight Linux-pohjaiset HDInsight | Azure"
description="Opi siirtämään Windows-pohjaisesta HDInsight-klusterin Linux-pohjaiset HDInsight-klusterin."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Siirtää Windows-pohjaisesta HDInsight-klusterin Linux-pohjaiset klusterin

Samalla, kun Windows-pohjaisesta Hdinsightista on helppo tapa käyttää Hadoop pilveen, saatat huomata, että sinun Linux-pohjaiset klusteriin ja hyödyntää työkalut ja -tekniikoiden ratkaisu varten tarvittavat. Hadoop ekosysteemissä monella eri tavalla on kehitetty Linux-järjestelmiä ja jotkin eivät välttämättä ole käytettävissä Windows-pohjaisesta HDInsight käytettäväksi. Lisäksi monia kirjoista, videoita ja muita koulutusmateriaali oletetaan, että käytät Linux-järjestelmän Hadoop käsiteltäessä.

Tässä tiedostossa on tiedot HDInsight Windows ja Linux ja ohjeita siitä, miten voit siirtää olemassa olevat työmääriä Linux-pohjaiset klusterin välinen ero.

> [AZURE.NOTE] HDInsight klustereiden käyttäminen klusterin solmujen käyttöjärjestelmän Ubuntu pitkällä aikavälillä tuki (l.). Lisätietoja sekä muita osan versiotietojen tietoja Ubuntu käytettävissä kanssa HDInsight-versiossa on artikkelissa [HDInsight osan versiot](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Siirtotehtävät

Siirron Yleiset työnkulku on seuraavasti.

![Siirron Työnkulkukaavio](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lue kunkin osion tämän asiakirjan ymmärtää muutokset, jotka voidaan tarvita, kun siirtämisestä Linux-pohjaiset klusterin työnkulun, työt, jne.

2.  Luo Linux-pohjaiset klusterin testi/laatu assurance-ympäristössä. Lisätietoja Linux-pohjaiset klusterin luomisesta on artikkelissa [HDInsight luominen Linux-pohjaiset varausyksiköt](hdinsight-hadoop-provision-linux-clusters.md).

3.  Kopioi aiemmin työt, tietolähteet ja poistumia uuteen ympäristöön. Katso lisätietoja testi ympäristön-osassa kopioi tiedot.

4.  Suorita vahvistus testaaminen varmistaaksesi, että työt toimi odotetulla tavalla uuden klusterin.

Kun olet varmistanut, että kaikki toimii odotetulla tavalla, ajoittaa siirron käyttökatkot. Tämä käyttökatkot aikana tehdä seuraavat toimet.

1.  Voit varmuuskopioida kaikki lyhytkestoisia klusterisolmut paikallisesti tallennetut tiedot. Esimerkiksi jos pään solmu tallennettuihin tietoihin.

2.  Poista Windows-pohjaisesta-klusterin.

3.  Luo käyttämällä samaa oletusarvon tietovaraston, joka käyttää Windows-pohjaisesta klusterin Linux-pohjaiset klusterin. Näin voit jatkaa aiemmin tuotannon tietojen vastaan uuden klusterin.

4.  Tuo varmuuskopioitua lyhytkestoisia tietoja.

5.  Käynnistä työt ja jatkaa käsittelyä uuden klusterin avulla.

### <a name="copy-data-to-the-test-environment"></a>Tietojen kopioiminen testiympäristössä

Kopioi tiedot ja töiden useita eri tavalla, kuitenkin kahden tässä osassa on helpoin menetelmät suoraan tiedostojen siirtämiseen testi-klusterin.

#### <a name="hdfs-dfs-copy"></a>HDFS DFS kopio

Hadoop HDFS-komennolla suoraan tietojen kopioiminen tallennustilaan aiemmin tuotannon-klusterin tallennustilan uusi testi-klusterin seuraavien ohjeiden mukaisesti.

1. Etsi tallennustilan tilin ja oletusarvon säilö tiedot aiemmin klusterin. Voit tehdä tämän käyttämällä seuraavaa Azure PowerShell-komentosarjaa.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Luo Linux-pohjaiset varausyksiköiden HDInsight-asiakirjassa, voit luoda uuden testiympäristössä noudattamalla. Ennen kuin luot klusterin lopettaa ja valitse sen sijaan **Valinnainen määritys**.

3. Valitse **Linkitetty tallennustilan tilit**vaihtoehtoinen määritys-sivu.

4. Valitse **Lisää tallennustilaa-näppäintä**ja pyydettäessä, valitse vaiheessa 1 PowerShell-komentosarjaa palautti tallennustilan tili. Valitse **Valitse** jokainen sivu sulkea. Luo-klusterin.

5. Kun klusterin on luotu, muodosta yhteys käyttämällä **SSH.** Jos et ole käyttänyt SSH HDInsight kanssa, on seuraavissa artikkeleissa.

    * [Käytä SSH Linux-pohjaiset HDInsight Windows-asiakkaiden kanssa](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Käytä SSH Linux-pohjaiset HDInsight Linux, Unix ja Mac-asiakkaiden kanssa](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Tiedostojen kopioiminen uusi tallennustilan oletustilin linkitetyn tallennustilan tilin SSH-istunnon seuraava komento avulla. Korvaa SÄILÖ ja tilin säilö ja tilitiedot palauttama PowerShell-komentosarjaa vaiheessa 1. Korvaa tiedot datatiedostoon polulla.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Jos testiympäristössä ei ole kansiorakenne, joka sisältää tiedot, voit luoda sen avulla seuraava komento.

        hdfs dfs -mkdir -p /new/path/to/create

    `-p` Valitsin voi luoda polun kaikki kansiot.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Suora kopioi välillä Azuren tallennustilaan BLOB-objektit

Vaihtoehtoisesti voit käyttää `Start-AzureStorageBlobCopy` Azure PowerShell cmdlet-komento, voit kopioida BLOB storage ulkopuolella HDInsight tilien välillä. Katso lisätietoja, miten voit hallita Azure PowerShellin Azure-tallennustilan kanssa Azure-BLOB-osassa.

##<a name="client-side-technologies"></a>Asiakkaan tekniikat

Yleensä asiakkaan tekniikoita, kuten [Azure PowerShellin cmdlet-komennot](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) tai [.NET SDK Hadoop](https://hadoopsdk.codeplex.com/) edelleen toimii samalla Linux-pohjaiset klustereiden kanssa, kun ne ovat riippuvaisia REST API ovat samat molemmissa klusterin OS välillä.

##<a name="server-side-technologies"></a>Palvelinpuolen tekniikat

Seuraavassa taulukossa on ohjeet siirretään palvelinpuolen osia, jotka ovat Windows tietyn.

| Jos käytät tätä tekniikkaa... | Toimi näin... |
| ----- | ----- |
| **PowerShellin** (palvelinpuolen komentosarjoja, kuten klusterin luonnin aikana käytettävän komentosarja-toiminnot) | Uuden esimerkkitekstin kuin Bash komentosarjoja. Katso komentosarjatoiminnot, [mukauttaa Linux-pohjaiset HDInsight komentosarja-toimintojen](hdinsight-hadoop-customize-cluster-linux.md) ja [komentosarjan toiminnon kehitystä Linux-pohjaiset Hdinsightista](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (Palvelinpuolen komentosarjat) | Azure-CLI ollessa käytettävissä Linux se tule esiasennettu HDInsight-klusterin pään solmuissa. Jos tarvitse sitä varten palvelinpuolen komentosarjan, kohdassa [asentaminen Azure-CLI](../xplat-cli-install.md) Lisätietoja asentamisesta Linux-pohjaisten käyttöjärjestelmien työpöytäsovelluksiin. |
| **.NET komponentit** | .NET on ei täysin tueta Linux-pohjaiset HDInsight klustereiden. Linux-pohjaiset myrsky HDInsight-klusterit luotu jälkeen 10/28/2017 tuki C# myrsky topologioissa käyttämällä SCP.NET puitteissa. Lisää tuki .NET lisätään myöhemmin päivitykset. |
| **Win32 osia tai muuta vain Windows-tekniikkaa** | Ohjeet määräytyy osa tai tekniikkaa. Voit ehkä Etsi Linux kanssa yhteensopivalla versiolla tai joudut ehkä etsimään vaihtoehtoinen ratkaisu tai kirjoittaa tämä osa. |

##<a name="cluster-creation"></a>Klusterin luominen

Tässä osassa on tietoja klusterin luominen erot.

### <a name="ssh-user"></a>SSH käyttäjä

Linux-pohjaiset HDInsight klustereiden klusterisolmut remote pääsy **Suojattu runko (SSH)** -protokollan avulla. Toisin kuin Remote Desktop for Windows-pohjainen klustereiden useimmat SSH asiakkaat eivät tarjoa graafinen käyttäjäkokemuksen, mutta on sen sijaan, jonka avulla voit suorittaa komentoja klusterin komentorivin. Joissakin palveluissa (kuten [MobaXterm](http://mobaxterm.mobatek.net/),) on graafinen tiedoston järjestelmän selaimen lisäksi etätietokoneeseen komentorivin.

Klusterin luonnin aikana sinun on määritettävä SSH-käyttäjä ja joko **salasanan** tai **julkisen avaimen varmenne** todennusta varten.

On suositeltavaa käyttää julkisen avaimen varmenne, sillä se on paremmin suojattu kuin salasanalla. Todennus toimii luodaan allekirjoitetun julkinen/yksityinen avaimen lainausmerkeiksi ja sitten antamisen julkinen avain klusterin luotaessa. Muodostettaessa yhteyttä palvelimeen SSH asiakkaan yksityinen avain tarjoaa todennuksen yhteyden.

Katso lisätietoja SSH käyttämisestä HDInsight:

- [SSH käyttäminen HDInsight Windows-asiakkaiden](hdinsight-hadoop-linux-use-ssh-windows.md)

- [SSH käyttäminen HDInsight Linux, Unix ja OS X-asiakkaat](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Klusterin mukauttaminen

**Komentosarjatoiminnot** Linux-pohjaiset klustereiden käyttämisestä on kirjoitettava Bash komentosarjan. Samalla, kun komentosarjatoimintoja voi käyttää klusterin luonnin aikana, saat Linux-pohjaiset klustereiden ne myös voi käyttää suorittamiseen mukauttamisen jälkeen klusteriin on käytössä ja käynnissä. Lisätietoja on artikkelissa [mukauttaminen Linux-pohjaiset HDInsight komentosarja-toimintojen](hdinsight-hadoop-customize-cluster-linux.md) ja [komentosarjan toiminnon kehitystä Linux-pohjaiset Hdinsightista](hdinsight-hadoop-script-actions-linux.md).

Toisen mukauttaminen-ominaisuus on **käynnistyksen**. For Windows-klustereiden tämä avulla voit määrittää kirjastoja käytettäväksi sijainnin rakenteen kanssa. Klusterin luonnin jälkeen nämä kirjastot ovat automaattisesti käytettävissä on käytettävä rakenne-kyselyt käytettäväksi `ADD JAR`.

Tämä toiminto ei tarjoa Linux-pohjaiset klustereiden automaattinen. Käytä sen sijaan [Lisää rakenne kirjastojen klusterin luonnin aikana](hdinsight-hadoop-add-hive-libraries.md)kuvattu komentosarja-toiminnon.

### <a name="virtual-networks"></a>Virtuaalinen verkot

Windows-pohjaisesta HDInsight klusterit perinteinen Virtual verkkojen vain käsitteleminen, kun Linux-pohjaiset HDInsight klustereiden käytettävä Resurssienhallinta Virtual verkot. Jos resurssit ovat perinteinen Virtual verkossa, jossa Linux HDInsight-klusterin on muodostettava yhteys, katso [yhteyden perinteinen Virtual Network Resurssienhallinta Virtual verkkoon](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Lisätietoja määritysten vaatimukset Azure Virtual verkkojen käyttäminen Hdinsightista on artikkelissa [laajentaa HDInsight ominaisuuksia käyttämällä Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Hallinta ja seuranta

Monet web käyttöliittymät, olet saattanut käyttää Windows-pohjaisesta HDInsight, kuten Työhistoria tai kuitenkaan Käyttöliittymä, jossa ovat käytettävissä Ambari kautta. Lisäksi Ambari rakenne-näkymä sisältää voi suorittaa rakenteen kyselyjä käyttämällä selainta. Ambari Web-Käyttöliittymän on saatavilla osoitteessa https://CLUSTERNAME.azurehdinsight.net Linux-pohjaiset klustereiden.

Lisätietoja Ambari käsittelemisestä on artikkelissa seuraavat asiakirjat:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST-Ohjelmointirajapinnalla](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari ilmoitukset

Ambari on ilmoitusten järjestelmän, jotka kertovat sinulle klusterin mahdollisia ongelmia. Varoituksia näytetä punainen tai keltainen merkinnät Ambari Web-Käyttöliittymä, mutta voit myös hakea ne REST API-Liittymän kautta.

> [AZURE.IMPORTANT] Ambari ilmoitukset osoittavat, *voi* olla ongelmia, ei ole käytettävissä *on* ongelma. Esimerkiksi voit saada ilmoituksen, että HiveServer2 ei voi käyttää, vaikka voit avata sen normaalisti.
>
> Useita ilmoituksia on toteutettu väli-pohjainen kyselyitä, jotka perustuvat palvelu ja odota vastausta ylitä tiettyä aikaa. Juuri niin ilmoitusta ei välttämättä ole tarkoittaa, että palvelu ei ole käytettävissä, että se ei palauta odotettua aikarajan puitteissa tulokset.

Yleensä on arvioitava, onko ilmoituksen on ollut määrä pitkään tai vastaa käyttäjän ongelmat, jotka on aiemmin ilmoitettu klusterin kanssa ennen siihen toiminto sitä.

##<a name="file-system-locations"></a>Järjestelmän oletuskansiot

Linux-klusterin tiedostojärjestelmässä on asettelun eri tavalla kuin Windows-pohjaisesta HDInsight klustereiden. Seuraavan taulukon avulla Etsi käytettyjä tiedostoja.

| Haluat etsiä... | Se sijaitsee... |
| ----- | ----- |
| Määritys | `/etc`. Esimerkiksi`/etc/hadoop/conf/core-site.xml` |
| Lokitiedostojen | `/var/logs` |
| Hortonworks tiedot-ympäristö (HDP) | `/usr/hdp`. On kaksi kansioiden sijaitsee täällä sellainen, joka on HDP nykyinen versio (esimerkiksi `2.2.9.1-1`,) ja `current`. `current` Hakemistosta on symbolinen linkkejä tiedostojen ja kansioiden version numeron kansiossa sijaitseva ja on kätevä tapa HDP tiedostojen käyttäminen versiosta kuin numero muuttuu HDP-versio on päivittymisen myötä. |
| hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Jos tiedät, että tiedoston nimi, voit tehdä SSH istunnosta seuraava komento Etsi tiedostopolku:

    find / -name FILENAME 2>/dev/null

Voit myös käyttää yleismerkkejä ja tiedostonimi. Esimerkiksi `find / -name *streaming*.jar 2>/dev/null` palauttavat polku purkki tiedostoja, joissa on sana streaming tiedostonimen osana.

##<a name="hive-pig-and-mapreduce"></a>Rakenne ja Possu MapReduce

Possu ja MapReduce työmääriä ovat hyvin samankaltaisia-Linux-pohjaiset klustereiden - Tärkein ero on, jos Etätyöpöydän avulla voit muodostaa yhteyden Windows-pohjaisesta klusterin ja suorita työt, käytät SSH kanssa Linux-pohjaiset klustereiden.

- [Possu käyttäminen SSH](hdinsight-hadoop-use-pig-ssh.md)

- [MapReduce käyttäminen SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Rakenne

Seuraavassa kaaviossa on ohjeita siirtämisestä rakenne-toiminnoista.

| Valitse Windows-pohjaisia, käytetään... | Valitse Linux-pohjaiset... |
| ----- | ----- |
| **Rakenne-editori** | [Näkymän Ambari rakenne](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Jos haluat ottaa käyttöön Tez | Tez on Linux-pohjaiset klustereiden oletusarvon suorittamisen-ohjelma, joten set-lause ei enää tarvita. |
| CMD tiedostoja ja komentosarjoja kutsuttu rakenteen työhön palvelimessa | Bash komentosarjojen käyttäminen |
| `hive`Etätyöpöytä-komentoa | Käytä [Beeline](hdinsight-hadoop-use-hive-beeline.md) tai [SSH istunnosta rakenne](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Myrsky

| Valitse Windows-pohjaisia, käytetään... | Valitse Linux-pohjaiset... |
| ----- | ----- |
| Myrsky Raporttinäkymät-ikkunan | Myrsky raporttinäkymät-ikkuna ei ole käytettävissä. Katso [käyttöönotto ja hallita myrsky topologioissa Linux-pohjaiset HDInsight-](hdinsight-storm-deploy-monitor-topology-linux.md) tapoja lähettää topologioissa |
| Myrsky Käyttöliittymä | Myrsky-Käyttöliittymän on saatavilla osoitteessa https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio luominen, ottaminen käyttöön ja C#- tai hybrid topologioissa hallinta | Visual Studio avulla voidaan luoda ja ottaa käyttöön C# (SCP.NET) tai hybrid topologioissa Linux-pohjaiset myrsky HDInsight klustereiden luotu 10/28/2017 jälkeen, valitse. |

##<a name="hbase"></a>HBase

Linux-pohjaiset klustereiden HBase znode pääsisältötyyppiä on `/hbase-unsecure`. Määritä tämä minkä tahansa Java-asiakasohjelman määritysten alkuperäisen HBase Java API käyttävät sovellukset.

Saat Esimerkki-asiakas, joka määrittää tämän arvon [Java-pohjainen HBase-sovelluksen luominen](hdinsight-hbase-build-java-maven.md) .

##<a name="spark"></a>Ohjattu

Ohjattu klustereiden on käytettävissä Windows klustereiden esikatselua, aikana kuitenkin versiossa, ohjattu on käytettävissä vain Linux-pohjaiset klustereiden. Ei ole siirtymispolku Windows-pohjaisesta ohjattu esikatselu klusterista release Linux-pohjaiset ohjattu klusteriin.

##<a name="known-issues"></a>Tunnetut ongelmat

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory mukautetun .NET toiminnot

Azure Data Factory mukautetun .NET toiminnot ovat ei tällä hetkellä tueta Linux-pohjaiset HDInsight klustereiden. Sen sijaan jollakin seuraavista tavoista pitäisi käyttää toteuttamisesta mukautetut aktiviteetit SYÖTTÖ myyntijakso osana.

-   Suorita Azure erä resurssivarantoon .NET-toimintoja. On [käyttää mukautettuja toimintoja Azure Data Factory-myyntijakso](../data-factory/data-factory-use-custom-activities.md#AzureBatch) Käytä Azure erä linkitetty service-osassa

-   Ota käyttöön MapReduce tehtävän tehtävän. Lisätietoja on kohdassa [Käynnistää MapReduce ohjelmien Data Factory](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Rivin päätteet

Yleensä rivin päätteet Windows-pohjaisten järjestelmien käyttää CRLF, Linux-järjestelmiin käyttää LF. Jos tuottaa tai odottaa CRLF rivin päätteet tietojen saatat joutua muokkaamaan tuottajat tai kuluttajille toimimaan LF rivin loppuun.

Esimerkiksi PowerShellin Azure avulla kyselyn HDInsight Windows-pohjaisesta klusterissa palautettavien tietojen CRLF. Linux-pohjaiset klusterin saman kyselyn voi palauttaa LF. Monissa tapauksissa tämä ei ole väliä tietojen kuluttajille, mutta olisi selvittänyt ennen siirtoa Linux-klusteriin.

Jos sinulla on komentosarjoja, jotka suoritetaan suoraan Linux-klusterin solmut (kuten Python komentosarjan käyttää rakennetta tai MapReduce työn), käytä LF aina rivin loppuun. Jos käytät CRLF, näkyviin voi tulla virheitä suoritetaan komentosarjat Linux-pohjaiset klusterissa.

Jos tiedät, että komentosarjat eivät sisällä upotetun CR-merkkiseksi merkkijonoja, voit joukkomuokata muuttaa rivi-päätteet jollakin seuraavista tavoista:

-   **Jos sinulla on komentosarjoja, jotka aiot klusterin lataaminen**, seuraavista väittämistä PowerShellin avulla voit muuttaa rivin päätteet CRLF LF ennen komentosarjan lataaminen klusterin.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Onko komentosarjoja, jotka ovat jo klusterin tallennustilaa**, voit tehdä seuraavalla komennolla SSH istunnosta Linux-pohjaiset klusterin Muokkaa komentosarjaa.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Seuraavat vaiheet

-   [Opettele luomaan Linux-pohjaiset HDInsight klustereiden](hdinsight-hadoop-provision-linux-clusters.md)

-   [Yhteyden muodostaminen Linux-pohjaiset klusterin SSH käyttämällä Windows-asiakasohjelma](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Yhteyden muodostaminen Linux-pohjaiset klusterin käyttämällä SSH Linux-, Unix- tai Mac-asiakasohjelma](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Käyttämällä Ambari Linux-pohjaiset klusterin hallinta](hdinsight-hadoop-manage-ambari.md)
