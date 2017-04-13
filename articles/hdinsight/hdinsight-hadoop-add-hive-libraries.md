<properties
pageTitle="Lisää rakenteen kirjastojen HDInsight-klusterin luonnin aikana | Azure"
description="Lisätietoja kirjastojen rakenne (jar tiedostot) lisäämisestä HDInsight-klusterin klusterin luonnin aikana."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Lisää rakenteen kirjastojen HDInsight-klusterin luonnin aikana

Jos sinulla on kirjastot, jota käytät usein HDInsight-rakenne, tämä asiakirja sisältää tietoja komentosarja-toiminnon avulla voit ladata etukäteen kirjastoja klusterin luonnin aikana. Tämä on kirjastot yleisesti saatavilla rakenne (ei tarvitse [Lisätä JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) avulla voit ladata ne.)

##<a name="how-it-works"></a>Toiminta

Klusterin luotaessa voit myös määrittää komentosarja-toiminnon, joka suoritetaan komentosarjan klusterin solmuissa, kun niitä luodaan. Tässä asiakirjassa komentosarja hyväksyy yksittäinen parametri, joka on WASB sijainti, joka sisältää kirjastot (tallennettu purkki tiedostoina), jotka ladataan valmiiksi.

Klusterin luonnin aikana komentosarja Luetteloi tiedostot, kopioi ne `/usr/lib/customhivelibs/` directory pää- ja työntekijä solmuissa lisää ne sitten `hive.aux.jars.path` -ominaisuutta `core-site.xml` tiedosto. Valitse Linux-pohjaiset klustereiden se myös päivittää `hive-env.sh` tiedosto, jonka tiedostojen sijainti.

> [AZURE.NOTE] Kirjastot komentosarja-toimintojen käyttäminen tässä artikkelissa on käytettävissä seuraavissa tilanteissa:
>
> * __Linux-pohjaiset HDInsight__ - __rakenne komentorivin__, __WebHCat__ (Templeton) ja __HiveServer2__käytettäessä.
> * __Windows-pohjaisesta HDInsight__ - käytettäessä __rakenne komentorivin__ ja __WebHCat__ (Templeton).

##<a name="the-script"></a>Komentosarja

__Komentosarjojen sijainti__

Saat __Linux-pohjaiset klustereiden__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

For __Windows-pohjaisesta klustereiden__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Vaatimukset__

* Komentosarjat on otettava käyttöön __Head solmujen__ ja __Työntekijä solmujen__.

* Haluat asentaa tölkki on tallennettava Azure-Blob-objektien tallennustilaan __yksittäisen säilö__. 

* Tallennustilan-tiliä, joka sisältää valikoiman purkki tiedostot __on__ voidaan linkittää HDInsight-klusterin luonnin aikana. Tämä onnistuu jollakin seuraavista tavoista:

    * Ole-klusterin tallennustilan oletustilin säilössä.
    
    * Valitse linkitetty säilytykseen säilössä ole. Esimerkiksi portaalissa voit käyttää __Vaihtoehtoinen määritys__, lisätä tallennustilaa __linkitetty tallennustilan tilit__ .

* Säilön WASB polku on määritettävä parametrina komentosarja-toiminnon. Esimerkiksi jos tölkki tallennetaan säilö __kirjastoa__ tallennustilan tilin nimi __mystorage__-parametrin olisi __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Tämän asiakirjan oletetaan, että olet jo tallennustilan tilin luominen, blob-säilö ja ladata tiedostoja. 
    >
    > Jos et ole luonut tallennustilan tilin, voit tehdä tämän palvelun [Azure portal](https://portal.azure.com). Voit käyttää apuohjelmaa, kuten [Azure-tallennustilan Explorer](http://storageexplorer.com/) voit luoda uuden säilön tili ja tiedostojen lataaminen.

##<a name="create-a-cluster-using-the-script"></a>Komentosarjan käyttäminen klusterin luominen

> [AZURE.NOTE] Seuraavia ohjeita voit luoda uuden Linux-pohjaiset HDInsight-klusterin. Jos haluat luoda uuden Windows-pohjaisesta klusterin, valitse __Windows__ kuin klusterin OS klusterin luotaessa ja käytä (PowerShell) Windows-komentosarjan bash komentosarja sijaan.
> 
> PowerShellin Azure tai HDInsight .NET SDK avulla voit myös luoda klusterin tämän komentosarjan avulla. Lisätietoja näistä menetelmistä on kohdassa [Mukauta HDInsight klustereiden komentosarjan toimintojen](hdinsight-hadoop-customize-cluster-linux.md).

1. Käynnistä valmistelu klusteri [valmisteleminen HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal)klustereissa Linux ohjeiden mukaisesti, mutta älä tee valmistelu.

2. Valitse **Vaihtoehtoinen määritys** -sivu **Komentosarjatoiminnot**ja anna tiedot alla kuvatulla tavalla:

    * __Nimi__: Kirjoita kutsumanimi komentosarja-toiminnon.
    * __KOMENTOSARJAN URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __HEAD__: Valitse tämä vaihtoehto
    * __Työntekijän__: Valitse tämä vaihtoehto.
    * __ZOOKEEPER__: Jätä tämä kenttä tyhjäksi.
    * __Parametrit__: säilö ja tallennustilaa tilille, joka sisältää tölkki WASB-osoitteen. Esimerkiksi __wasbs://libs@mystorage.blob.core.windows.net/__.

3. **Komentosarjatoiminnot**alareunassa Tallenna kokoonpano **valinta** -painikkeen avulla.

4. Valitse **Vaihtoehtoinen määritys** -sivu __Linkitetyt tallennustilan asiakkaat__ ja valitse __Lisää tallennustilaa näppäintä__ -linkki. Tallennustilan-tili, joka sisältää tölkki ja sitten __Valitse__ -painikkeiden avulla asetukset Tallenna ja __Valinnaisia määritys__ -sivu.

5. Tallenna valinnainen kokoonpanotietoja **Vaihtoehtoinen määritys** -sivu alareunassa **valinta** -painikkeen avulla.

6. Jatka valmistelu klusterin kuvatulla tavalla [Linux klustereiden säännöstä Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md#portal).

Klusterin luominen on valmis, kun osaat käyttää lisätty – tämä komentosarja-rakenne, käyttämättä tölkki `ADD JAR` lause.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä asiakirjassa on opit miten voit lisätä rakenteen kirjastojen sisältämät purkki tiedostot HDInsight-klusterin klusterin luonnin aikana. Katso lisätietoja käsittelemisestä rakenteen [Käyttäminen rakenne HDInsight kanssa](hdinsight-use-hive.md)
