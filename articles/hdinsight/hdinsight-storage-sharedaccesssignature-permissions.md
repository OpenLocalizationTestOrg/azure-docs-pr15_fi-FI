<properties
pageTitle="HDInsight rajoittaa tietojen jaettu Access allekirjoitusten käyttäminen"
description="Opettele käyttämään jaettu Access allekirjoitukset HDInsight käytön rajoittaminen tallennustilan Azure-BLOB tallennettuja tietoja."
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
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Käytön rajoittaminen HDInsight-tietojasi Azure tallennustilan jaettu Access allekirjoitusten käyttäminen

HDInsight käyttää tallennustilan Azure-BLOB tietojen tallentamista varten. Kun Hdinsightista on oltava täydet oikeudet käyttää oletusarvon tallennustilan klusterin blob-voivat rajoittaa oikeuksia muiden BLOB klusterin käyttämä tallennettuja tietoja. Jos esimerkiksi haluat tietoja vain luku-tilaan. Voit tehdä jaettu Access allekirjoitusten käyttäminen.

Jaetun Access allekirjoitukset (SAS) ovat Azure tallennustilan tilien ominaisuus, joka mahdollistaa tietojen käyttörajoitukset. Esimerkiksi tarjoaa vain luku-tietojen käytön. Tässä asiakirjassa kerrotaan käyttämisestä SAS käyttöön luku- ja vain luettelon blob-säilö Hdinsightista.

##<a name="requirements"></a>Vaatimukset

* Azure tilauksen

* C# tai Python. C#-esimerkkikoodi annetaan Visual Studio ratkaisuksi.

    * Visual Studio on oltava versiossa 2013 tai 2015
    
    * Python on oltava 2.7 tai uudempi versio

* Linux-pohjaiset HDInsight klusterin tai [PowerShellin Azure] [ powershell] – Jos sinulla on aiemmin Linux-pohjaiset klusterin, voit käyttää Ambari jaettu Access allekirjoituksen lisääminen klusterin. Muussa tapauksessa PowerShellin Azure avulla voit luoda uuden klusterin ja jakaa Access allekirjoituksen lisääminen klusterin luonnin aikana.

* Esimerkki tiedostojen [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Tämä säilö on seuraavasti:

    * Visual Studio-projektiin, voit luoda tallennustilan säilö, tallennetut käytännön ja Suojaussidokset HDInsight käytettäväksi
    
    * Python komentosarjan, voit luoda tallennustilan säilö, tallennetut käytännön ja Suojaussidokset HDInsight käytettäväksi
    
    * PowerShell-komentosarjaa, voit luoda uuden HDInsight-klusterin ja määrittää sen käyttämään Suojaussidokset.

##<a name="shared-access-signatures"></a>Jaettu käyttö allekirjoitukset

On jaettu Access allekirjoitukset kahdenlaisia:

* Ad hoc: alkamisaika, voimassaolon ajan ja Suojaussidokset käyttöoikeudet ovat kaikki määritetyn SAS URI (tai epäsuora siinä tapauksessa, jossa alkamisaika jätetään pois).

* Tallennettu Käyttöoikeuskäytäntö: tallennetut käyttöoikeuskäytäntö on määritetty resurssin säilön - blob-säilö, taulukon, jonossa tai jaettua tiedostoresurssia - ja avulla voidaan hallita jaettua käyttöä allekirjoituksia rajoitukset. Kun SAS liittää tallennetun käyttöoikeuskäytäntö Suojaussidokset perii rajoitteet - alkamisaika, voimassaolon ajan ja käyttöoikeudet - määritetty tallennetut käyttöoikeuskäytäntö.

Kaksi muotoa välinen ero on tärkeää yksi tärkeimmistä tilanne: kumottujen varmenteiden. Suojaussidokset on URL-osoite, kuka tahansa, joka hakee Suojaussidokset voit käyttää sitä, riippumatta siitä, joka pyytää sen aluksi. Jos Suojaussidokset on julkaistu yleisesti, se voidaan kaikkien maailmaa. SAS, jotka on jaettu on voimassa, kunnes tapahtuu jokin seuraavista neljästä:

1. Valitse Suojaussidokset määräajan määritettynä on saavutettu.

2. Tallennetun access-käytännön Suojaussidokset käyttämät määräajan määritettynä on saavutettu (Jos tallennettu käyttöoikeuskäytäntö viitataan ja se määrittää voimassaolon ajan). Näin voi käydä joko, koska varoittava tai olet muokannut voimassaolon ajan aiemman, jossa on yksi tapa kumota Suojaussidokset on tallennettu käyttöoikeuskäytäntö.

3. Suojaussidokset käyttämät tallennetut käyttöoikeuskäytäntö poistetaan, joka on toinen tapa poistaa Suojaussidokset. Huomaa, että jos tallennettu käyttöoikeuskäytäntö täsmälleen saman niminen luoda uudelleen, aiemmin SAS tunnusten uudelleen kelvollinen käyttöoikeuksista on tallennettu access käytännön (olettaen että, Suojaussidokset voimassaolon ajan ei läpäissyt) mukaan. Jos ovat aikoo kumota Suojaussidokset, muista käyttää eri nimellä, jos käyttöoikeuskäytäntö voimassaolon ajan myöhemmin uudelleen.

4. Tili-näppäintä, joka luotiin Suojaussidokset on luotava uudelleen. Huomaa, että tämä aiheuttaa kaikki sovelluksen osat kyseisen tilin avaimen avulla voi epäonnistua tarkistamiseen, kunnes ne päivitetään tilille-näppäintä tai äskettäin regeneroitu tili-näppäintä.

> [AZURE.IMPORTANT] Jaettu käyttö allekirjoituksen URI on liitetty allekirjoituksen luomiseen käytetty tilin avain ja liittyvän tallennettu käyttöoikeuskäytäntö (jos saatavilla). Jos ei ole tallennettu käyttöoikeuskäytäntö ei määritetä, kumota jaettua käyttöä allekirjoituksen ainoa tapa on muutettava tili-näppäintä. 

On suositeltavaa, että käytät tallennettu käytön käytännöt aina, niin, että voit kumota allekirjoitukset tai laajentaa erääntymispäivä tarpeen mukaan. Asiakirjan käyttö ohjeita tallennettu käytäntöjen SAS luomiseen.

Lisätietoja jaettujen Access allekirjoitukset on kohdassa [tietoa SAS-malli](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Tallennetun-käytännön luominen ja luo Suojaussidokset

Tällä hetkellä sinun on luotava tallennetut käytännön ohjelmallisesti. Löydät C# sekä Python Esimerkki luominen tallennetut käytännön ja Suojaussidokset [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

###<a name="create-a-stored-policy-and-sas-using-c"></a>Tallennetun käytännön ja käyttämällä C SAS luominen\#

1. Avaa ratkaisun Visual Studiossa.

2. Napsauta ratkaisunhallinnassa __SASToken__ projektin hiiren kakkospainikkeella ja valitse __Ominaisuudet__.

3. Valitse __asetukset__ ja lisää seuraavat arvot:

    * StorageConnectionString: Tallennustilan tili, jolle haluat luoda tallennettu käytännön ja Suojaussidokset, yhteysmerkkijono. Muodon on oltava `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` missä `myaccount` tallennustilan tilin nimi ja `mykey` tallennustilan tilin avain.
    
    * ContainerName: Tallennustilan tilin, jonka haluat rajoittaa säilö.
    
    * SASPolicyName: Tallennetut käytännön, joka luodaan käytettävät nimi.
    
    * FileToUpload: Tiedosto, joka voi ladata säilön polku.
    
4. Suorita projekti. Konsoli-ikkuna tulee näkyviin, ja seuraavankaltaiselta tiedot näytetään, kun Suojaussidokset on luotu:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Tallentavat SAS käytännön tunnuksen, sinun on tämä liitettäessä tallennustilan tilin HDInsight-klusterin. Sinun on myös tallennustilan tilin nimi sekä säilön nimi.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Tallennetun käytännön ja käyttämällä Python SAS luominen

1. Avaa SASToken.py-tiedosto ja muuta seuraavat arvot:

    * käytännön\_nimi: nimi tallennettu käytännön, joka luodaan.
    
    * tallennustilan\_tilin\_nimi: tallennustilan tilin nimi.
    
    * tallennustilan\_tilin\_avaimen: tallennustilan tilin avain.
    
    * tallennustilan\_säilö\_nimi: säilö-tallennustilan tilin, jonka haluat rajoittaa.
    
    * Esimerkki\_tiedoston\_polku: polku tiedostoon, jota ladataan säilöön

2. Komentosarjan suorittaminen Se näyttää seuraavankaltaiselta SAS tunnuksen, kun komentosarja on valmis:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Tallentavat SAS käytännön tunnuksen, sinun on tämä liitettäessä tallennustilan tilin HDInsight-klusterin. Sinun on myös tallennustilan tilin nimi sekä säilön nimi.
    
##<a name="use-the-sas-with-hdinsight"></a>Suojaussidokset käyttäminen Hdinsightiin

HDInsight-klusterin luotaessa on määritettävä ensisijainen tallennustilan-tili ja voit halutessasi määrittää lisätallennustilaa tilit. Molempia näistä tavoista tallennustilan lisäämisen edellyttävät tallennustilan tilit ja säilöjä, joita käytetään käyttöoikeudet.

Jaettu Access allekirjoituksen avulla voit rajoittaa säilöön, sinun on lisättävä mukautettu tapahtuman klusterin työnkulkuun __core-sivusto__ .

* __Windows -__ tai __Linux-pohjaiset__ HDInsight klustereiden, voit tehdä tämän klusterin luonnissa PowerShellin avulla.

* Voit muuttaa __Linux-pohjaiset__ HDInsight klustereiden määritystä käyttämällä Ambari klusterin luonnin jälkeen.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Luo uusi klusteri, joka käyttää Suojaussidokset

Esimerkki, jossa käytetään Suojaussidokset HDInsight-klusterin luominen sisältyy `CreateCluster` säilö hakemistoon. Voit käyttää noudattamalla seuraavia ohjeita:

1. Avaa `CreateCluster\HDInsightSAS.ps1` tiedoston tekstieditorissa ja muokata asiakirjan alussa seuraavat arvot.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Esimerkiksi muuttaa `'mycluster'` haluat luoda klusterin nimeä. SAS-arvojen tulee vastata edelliset vaiheet arvot, luotaessa tallennustilan tilin ja Suojaussidosten tunnuksen.
    
    Kun olet muuttanut arvot, Tallenna tiedosto.

1. Avaa uusi Azure PowerShell-kehotteessa. Jos olet perehtynyt PowerShellin Azure tai se ei ole asennettu, katso [asentaminen ja määrittäminen PowerShellin Azure][powershell].

2. Käytä seuraavia kehotettaessa tarkistamiseen Azure-tilaukseen:

        Login-AzureRmAccount
    
    Pyydettäessä Azure tilauksen tilillä kirjautuminen.
    
    Jos kirjautumisen on liitetty useita Azure tilauksia, joudut ehkä käyttää `Select-AzureRmSubscription` , jota haluat käyttää tilaus.

2. Muuttaa kehotettaessa hakemistoja `CreateCluster` HDInsightSAS.ps1 tiedoston sisältävä kansio. Suorita komentosarja seuraavat avulla
        
        .\HDInsightSAS.ps1
    
    Kun komentosarja on suoritettu, se kirjaa tulosteen PowerShell kehotteen luodessaan resurssi-ryhmä ja tallennustilaa tilit. Se sitten pyytää sinun on annettava käyttäjän HTTP HDInsight-klusterin. Tämä on käyttäjätilillä käyttää HTTP/s käyttöoikeus klusterin suojaamiseen.
    
    Jos olet luomassa Linux-pohjaiset klusterin, voit myös pyytää SSH käyttäjänimeä ja salasanaa. Tämä käytetään etäyhteyden klusterin Kirjaudu sisään.
    
    > [AZURE.IMPORTANT] Kun ohjelma pyytää HTTP/s tai SSH käyttäjänimi ja salasana, sinun on annettava salasana, joka täyttää seuraavat ehdot:
    >
    > - On oltava vähintään 10 merkkiä
    > - On sisällettävä vähintään yksi numero
    > - On sisällettävä vähintään yksi muita kuin aakkosnumeerisia merkki
    > - On sisällettävä vähintään yksi ISO tai pieni kirjain


Kestää jonkin aikaa tämän komentosarjan suorittaminen edellyttää, yleensä noin 15 minuuttia. Kun komentosarja on valmis ilman virheitä, klusterin on luotu.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Päivitä aiemmin luodun klusterin Suojaussidokset käyttäminen

Jos sinulla on aiemmin Linux-pohjaiset klusterin, voit lisätä Suojaussidokset työnkulkuun __core-sivuston__ avulla seuraavasti:

1. Avaa yhteyttä klusterin Ambari Internetistä UI. Tämän sivun osoite on https://YOURCLUSTERNAME.azurehdinsight.net. Kun sinulta kysytään, todentaa järjestelmänvalvojan nimi (järjestelmänvalvojat) ja-salasanalla klusterin luomisessa käytetty klusteriin.

2. Ambari web-Käyttöliittymä vasemmasta reunasta Valitse __HDFS__ ja valitse sitten sivun keskellä __kokoonpanomääritysten yhteydessä__ -välilehti.

3. Valitse __Lisäasetukset__ -välilehti ja vieritä sitten Etsi __Mukautettu core-sivusto__ -osassa.

4. Laajenna __Mukautettu core-sivusto__ -osa ja valitse Siirry loppuun ja valitse __Lisää ominaisuus...__ -linkki. Käytä seuraavia arvoja __avaimen__ ja __arvon__ kentät:

    * __Avain__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Arvo__: SAS palauttama suoritit aiemmin C#- tai Python sovellus
    
    Korvaa __CONTAINERNAME__ säilössä, voit käyttää C# tai SAS sovelluksen. Korvaa __STORAGEACCOUNTNAME__ käytit tallennustilan tilin nimi.

5. Napsauta __Lisää__ -painiketta, Tallenna tämä avain ja arvo ja valitse sitten Tallenna määritysten muutokset __Tallenna__ -painiketta. Kun sinulta kysytään, lisätä kuvauksen muuttaminen ("lisääminen SAS tallennustilan access" esimerkiksi) ja valitse sitten __Tallenna__.

    Kun muutokset on tehty, valitse __OK__ .

    > [AZURE.IMPORTANT] Toiminto tallentaa määritysten muutokset, mutta useita services on käynnistettävä uudelleen, ennen kuin muutokset tulevat voimaan.

6. Ambari sivuston Käyttöliittymä Valitse __HDFS__ vasemman reunan luettelosta ja valitse sitten __Uudelleen kaikkien__ __Palvelun toiminnot__ avattavasta luettelosta oikealla. Pyydettäessä, valitse __käyttöön ylläpito__ ja sitten Valitse __Conform uudelleen kaikki".

    Tämä toimenpide on toistettava MapReduce2 ja kuitenkaan luettelosta sivun vasemmassa reunassa tapahtumat.

7. Ylläpidon tila __Palvelun toiminnot__ Avaa luettelo, kun ne on käynnistettävä uudelleen, valitse jokaisessa ja poista käytöstä.

##<a name="test-restricted-access"></a>Käytön rajoitusten testaaminen

Voit varmistaa, että olet rajoittanut käyttöoikeudet, tekemällä seuraavat toimet:

* Muodostaa yhteyttä klusterin __Windows-pohjaisesta__ HDInsight klustereiden Etätyöpöydän avulla. Lisätietoja on kohdassa [Connecto käyttämällä RDP Hdinsightista](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Kun yhteys on muodostettu, Avaa komentorivi työpöydällä __Hadoop komentorivin__ -kuvakkeen avulla.

* Yhteyden muodostaminen klusterin __Linux-pohjaiset__ HDInsight klustereiden SSH avulla. SSH käyttäminen Linux-pohjaiset klustereiden Katso tietoja seuraavasti:

    * [Linux-pohjaiset Hadoop HDInsight Linux, OS X ja Unix-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Kun yhteys klusterin, varmista, että voit vain luku- ja luettelon kohteita SAS tallennustilan tilin seuraavien vaiheiden avulla:

1. Kirjoita kehotettaessa seuraavasti. Korvaa __SASCONTAINER__ säilön luotiin SAS tallennustilan tilin nimi. Korvaa __SASACCOUNTNAME__ Suojaussidokset käytettävän tallennustilan tilin nimi:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    Tämä tuo näyttöön säilö, joihin sisältyvät tiedosto, joka on ladattu sisällön säilö ja Suojaussidokset luontipäivämäärä.
    
2. Seuraavat avulla voit varmistaa, että voit lukea tiedoston sisällön. Korvaa __SASCONTAINER__ ja __SASACCOUNTNAME__ kuin edellisessä vaiheessa. Korvaa edellisen komennon näkyvät tiedoston nimi __tiedoston nimi__ :

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Tämä on luettelo tiedoston sisällön.
    
3. Lataa tiedosto paikallisen tiedostojärjestelmän käyttämällä seuraavaa:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Tämä Lataa tiedosto paikalliseen __testfile.txt__-tiedostoon.

4. Paikallinen tiedosto ladataan tiedosto nimeltä __testupload.txt__ SAS säilössä käyttämällä seuraavaa:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Näyttöön tulee sanoma, joka on seuraavanlainen:
    
        put: java.io.IOException
        
    Tämä virhe ilmenee, koska tallennussijainti on luku + luettelosta vain. Tietojen tallennussijainti klusterin, joka on kirjoitettava oletusarvo-tallennustila käyttämällä seuraavaa:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Tällä hetkellä toimintoa kannattaa suorittaa loppuun.
    
##<a name="troubleshooting"></a>Vianmääritys

###<a name="a-task-was-canceled"></a>Tehtävä on peruutettu.

__Ongelmia__: luodessasi käyttämällä PowerShell-komentosarjaa klusterin, näyttöön voi tulla seuraava virhesanoma:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Syy__: Tämä virhe voi ilmetä, jos käytät salasanaa järjestelmänvalvojan/HTTP-yhteyden käyttäjän klusterin tai (Linux-pohjaiset klustereiden,) SSH käyttäjä.

__Ratkaisu__: Käytä salasanaa, joka täyttää seuraavat ehdot:

- On oltava vähintään 10 merkkiä
- On sisällettävä vähintään yksi numero
- On sisällettävä vähintään yksi muita kuin aakkosnumeerisia merkki
- On sisällettävä vähintään yksi ISO tai pieni kirjain

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt oppinut olet rajoitetun tallennustilan lisääminen HDInsight-klusterin, lue yhteyttä klusterin tietojen käyttäminen:

* [Rakenteen käyttäminen Hdinsightiin](hdinsight-use-hive.md)

* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)

* [MapReduce käyttäminen Hdinsightiin](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
