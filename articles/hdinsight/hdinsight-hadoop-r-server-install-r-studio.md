<properties
    pageTitle="Asenna RStudio R-palvelimeen HDInsight (ennakkoversio) | Microsoft Azure"
    description="Voit asentaa RStudio HDInsight (ennakkoversio) R-palvelimeen."
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
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Asennuksen RStudio R palvelimelle HDInsight (ennakkoversio)

On useita integroitua ympäristöissä (IDE) käytettävissä R tänään, kuten Microsoftin viimeksi ilmoitettiin [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)-työkaluja työpöydän ja palvelimen [RStudio](https://www.rstudio.com/products/rstudio-server/)tai Walware's Pimennys perustuva [StatET](http://www.walware.de/goto/statet)perhe. Suosituimpien Linux välillä on [RStudio palvelimeen](https://www.rstudio.com/products/rstudio-server/) , joka sisältää selainpohjaisia IDE remote asiakkaiden.  RStudio Serverin asentamisesta HDInsight Premium-klusterin reuna-solmu sisältävät koko IDE kehityksen ja R-komentosarjojen suorittamisen R-palvelimen kanssa klusterin, ja se voi olla huomattavasti tuottavuutta kuin oletusarvoinen R-konsolin käyttöä.

Tässä artikkelissa kerrotaan RStudio Server yhteisön (maksuton)-version asentamisesta klusterin reuna-solmu mukautetun komentosarjan avulla. Jos käytät mieluummin RStudio Server kaupallisesti käyttöoikeus Pro-version, noudata asennusohjeita [RStudio](https://www.rstudio.com/products/rstudio/download-server/)palvelimesta.

> [AZURE.NOTE] Tässä asiakirjassa vaiheet edellyttävät HDInsight-klusterin R-palvelimelle ja ei toimi oikein Jos käytössäsi on HDInsight-klusterin Jos R on asennettu [Asentaa R komentosarja-toiminnon](hdinsight-hadoop-r-scripts-linux.md)avulla.

## <a name="prerequisites"></a>Edellytykset

* Azure Hdinsightiin klusterin asennettu R-palvelimen kanssa. Katso ohjeet [R palvelimelle HDInsight klustereiden käytön aloittaminen](hdinsight-hadoop-r-server-get-started.md).
* SSH-asiakas. Linux ja Unix jaot tai Macintosh OS X `ssh` komento on annettu käyttöjärjestelmän kanssa. (Windows) on suositeltavaa [Cygwin](http://www.redhat.com/services/custom/cygwin/) [OpenSSH-vaihtoehto](https://www.youtube.com/watch?v=CwYSvvGaiWU)tai [painovärit, muste](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Käytä mukautettua komentosarjaa klusterin RStudio asentaminen

1. Määritä klusterin reuna-solmu. Seuraavassa on R-palvelimen kanssa HDInsight-klusterin, pää-solmu ja reunan solmu nimeämiskäytäntö.

    * Pää - solmu`CLUSTERNAME-ssh.azurehdinsight.net`
    * Reuna - solmu`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH kyselyjä käyttämällä yllä nimeämismuoto klusterin reuna-solmu. 
 
    * Jos olet muodostamassa Linux asiakaskoneesta, artikkelissa [etäyhteyden muodostaminen Linux-pohjaiset HDInsight-klusterin](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Jos muodostat yhteyden Windows-asiakasohjelmassa artikkelissa [etäyhteyden muodostaminen käyttämällä painovärit, muste Linux-pohjaiset HDInsight-klusterin](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Kun yhteys on muodostettu, muuttuvat pääkansion käyttäjän klusterin. SSH istunnon aikana Käytä seuraavaa komentoa.

        sudo su -

4. Lataa mukautettu komentosarja RStudio asentamiseksi. Käytä seuraavaa komentoa.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Mukautettu komentosarja-tiedoston käyttöoikeuksien muuttaminen ja Suorita komentosarja. Käytä seuraavia komentoja.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Jos olet käyttänyt SSH salasanan luotaessa HDInsight-klusterin R-palvelimen kanssa, voit ohittaa tämän vaiheen ja siirry seuraavaan. Jos käytit SSH avain sen sijaan voit luoda klusterin, sinun on määritettävä SSH käyttäjän salasanan. Tarvitset salasanan RStudio yhdistettäessä. Suorita seuraavat komennot. Kun sinulta kysytään **nykyisen Kerberos-salasanan**, painamalla **ENTER**-näppäintä.  Huomaa, että sinun on korvattava `USERNAME` HDInsight-klusterin SSH käyttäjän kanssa.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Jos salasana on onnistuneesti, näkyy seuraavanlaisen sanoman.

        passwd: password updated successfully


    Sulje istunto SSH.

7. Luo SSH-tunnelia klusterin tulona yhdistämällä `localhost:8787` asiakaskoneeseen HDInsight-klusterin. Sinun on luotava SSH tunnelin, ennen kuin avaat uusi selausistunto.

    * Linux-asiakas- tai Windows-asiakkaaseen [Cygwin](http://www.redhat.com/services/custom/cygwin/) ja valitse Avaa pääte istunnon ja käytä seuraavaa komentoa.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Korvaa **käyttäjänimi** HDInsight-klusterin SSH käyttäjän kanssa ja **CLUSTERNAME** HDInsight-klusterin nimellä voit käyttää myös SSH näppäintä salasanan sijaan lisäämällä`-i id_rsa_key`     

    * Jos Windows-asiakasohjelman käyttäminen painovärit, muste sitten

        1.  Avaa painovärit, muste ja kirjoita yhteyden tietoja. Jos et ole aiemmin käyttänyt painovärit, muste, lisätietoja [Käyttää SSH Linux-pohjaiset Hadoop HDInsight Windows-ja](hdinsight-hadoop-linux-use-ssh-windows.md) siitä, miten voit käyttää sitä Hdinsightista.
        2.  Vasemmalla puolella valintaikkunan **luokka** -osasta Laajenna **yhteyden**, laajenna **SSH**ja valitse sitten **tunneleita**.
        3.  Anna **asetukset, joilla hallitaan SSH portin edelleenlähetys** -lomakkeen seuraavat tiedot:

            * **Lähdeportti** - asiakas, johon haluat lähettää porttiin. Esimerkiksi **8787**.
            * **Kohde** - kohteeseen, joka on yhdistettävä paikalliseen asiakastietokoneeseen. Esimerkiksi **localhost:8787**.

            ![Luo SSH tunnelin] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Luo SSH tunnelin")

        4. Lisää valitsemalla **Lisää** asetuksia ja valitse sitten **Avaa** SSH-yhteyden.
        5. Kun sinulta kysytään, kirjaudu sisään palvelimeen. Tämä muodostaa SSH-istunnon ja ota käyttöön tunnelin.

8. Avaa selain ja kirjoita seuraava URL-osoite tunnelin kirjoitit portin perusteella.

        http://localhost:8787/ 

9. Voit pyydetään antamaan SSH käyttäjänimi ja salasana, muodostaa yhteyttä klusterin. Jos olet käyttänyt SSH avain klusterin luomisen aikana, sinun on annettava salasana, jonka loit vaiheessa 5.

    ![Yhteyden muodostaminen R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Luo SSH tunnelin")

10. Voit testata, onko RStudio asennuksen onnistuneen, testi suorittamalla komentosarja, joka suorittaa R perusteella MapReduce ja ohjattu töiden klusterin. Palaa SSH konsolin ja kirjoita seuraavat komennot Lataa testi komentosarjan suorittaminen RStudio.

    * Jos olet luonut Hadoop-klusterin r, käyttää tätä komentoa.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Jos olet luonut Ohjattu klusterin r, käyttää tätä komentoa.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. RStudio näet lataamaasi Testikomentosarja. Kaksoisnapsauta tiedostoa, avaa se, tiedoston sisällön valinta ja valitse sitten **Suorita**. Raportissa pitäisi näkyä tulosteen **Console** -ruudussa.
 
    ![Asennuksen testaaminen] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Asennuksen testaaminen")

Kirjoita on jokin muu vaihtoehto `source(testhdi.r)` tai `source(testhdi_spark.r)` komentosarjan suorittaminen.

## <a name="see-also"></a>Katso myös

- [Laske kontekstin asetukset R palvelimen HDInsight klustereiden](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure tallennustilan asetukset R palvelimen HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
