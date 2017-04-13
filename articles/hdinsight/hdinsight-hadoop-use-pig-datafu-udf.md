<properties
pageTitle="Possu HDInsight-DataFu käyttäminen"
description="DataFu on kokoelma kirjastojen Hadoop käytettäväksi. Katso, miten voit käyttää DataFu Possu kanssa HDInsight-klusterin."
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
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Possu HDInsight-DataFu käyttäminen

DataFu on kokoelma, Avaa lähde-kirjastojen Hadoop käytettäväksi. Tässä asiakirjassa kerrotaan DataFu käyttämisestä HDInsight-klusterin ja voit käyttää Possu DataFu käyttäjän määrittämät funktiot (UDF).

##<a name="prerequisites"></a>Edellytykset

* Azure tilaus.

* Azure HDInsight-klusterin (Linux tai Windows-pohjainen)

* Tavallinen tuntemusta [käyttämällä HDInsight-Possu](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Asenna DataFu Linux-pohjaiset Hdinsightiin

> [AZURE.NOTE] DataFu on asennettu Linux-pohjaiset klustereiden versio 3.3 tai uudempi versio ja Windows-pohjaisesta klustereiden. Se ei ole asennettu Linux-pohjaiset klustereiden kuin 3.3.
>
> Jos käytössäsi on Linux-pohjaiset klusterin 3.3 tai uudempi versio tai Windows-pohjaisesta klusterin, voit ohittaa tämän osion.

DataFu voidaan ladata ja asentaa maven-testi säilöstä. Seuraavien vaiheiden avulla voit lisätä DataFu HDInsight-klusterin:

1. Muodostaa yhteyttä Linux-pohjaiset HDInsight-klusterin SSH avulla. Lisätietoja SSH käyttämisestä HDInsight-kohdassa jokin seuraavista asiakirjoista:

    * [Linux-pohjaiset Hadoop HDInsight Linux, OS X ja Unix-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Seuraavalla komennolla voit ladata DataFu purkki tiedoston wget-apuohjelman avulla tai kopioi ja Liitä linkki selaimella, Aloita lataus.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Tiedoston lataaminen seuraavaksi HDInsight-klusterin tallennustila oletusarvon. Tämä mahdollistaa tiedoston kaikkien solmujen klusterin ja tiedoston säilyvät tallennustilaa, vaikka Poista ja luo klusterin.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Edellä olevassa esimerkissä tallentaa purkkiin- `wasbs:///example/jars` koska tämä kansio on jo klusterin-tallennustilan. Voit milloin haluat HDInsight klusterin säilössä.

##<a name="use-datafu-with-pig"></a>Possu DataFu käyttäminen

Tässä osassa vaiheissa oletetaan tuntemiasi Possu käyttäminen HDInsight ja tarjota ainoastaan Possu latinalainen-lauseet eivät ohjeita siitä, miten voit käyttää niitä klusterin. Katso lisätietoja Possu käyttämisestä HDInsight, [Käytä Possu Hdinsightista](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Kun käytät Linux-pohjaiset HDInsight-klusterin DataFu-Possu, sinun on rekisteröitävä Possu latinalainen-lauseella purkki tiedosto:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu on rekisteröity Windows-pohjaisesta HDInsight klustereiden oletusarvon mukaan.

Yleensä määrittää tunnusnimen DataFu funktiot. Esimerkki:

    DEFINE SHA datafu.pig.hash.SHA();
    
Tämä määrittää sähköpostitunnuksen nimetyn `SHA` hajautuksessa funktion SHA varten. Sitten voit tämä Possu latinalainen komentosarjan syötteen tietojen Hajautuksen luominen. Esimerkiksi seuraavat korvaa syöttötiedot nimet hash-arvo:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Jos tätä käytetään syötteen seuraavat tiedot:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Se luo seuraava tulos:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja DataFu tai Possu, seuraavat asiakirjat:

* [Apache DataFu Possu opas](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Possu käyttäminen Hdinsightiin](hdinsight-use-pig.md)
