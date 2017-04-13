<properties
   pageTitle="Laske kontekstin asetukset R palvelimen HDInsight (ennakkoversio) | Microsoft Azure"
   description="Lisätietoja eri Laske kontekstin käytettävissä olevat asetukset käyttäjille, R-palvelimen kanssa HDInsight (ennakkoversio)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Laske kontekstin asetukset R palvelimen HDInsight (ennakkoversio)

Microsoft Azure Hdinsightiin (ennakkoversio) R-palvelimelle sisältää uusimmat ominaisuuksia, joilla R-pohjainen analytics. Se vastaa tilisi tallennustilan [Azure-Blob](../storage/storage-introduction.md "Azure-Blob-säiliö") tai paikallinen Linux tiedostojärjestelmästä säilön HDFS tallennetuista tiedoista. Koska R-palvelimeen on muodostettu Avaa lähde R, R-pohjaisten sovellusten, voit luoda voidaan hyödyntää 8000 + Avaa lähde R-paketti. Ne voidaan hyödyntää myös [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Vallankumous Analytics ScaleR"), Microsoftin big datasta analytics paketti, joka sisältää R palvelimen käytöstä.  

Reuna-solmu Premium-klusterin kätevästi muodostaa yhteyttä klusterin ja R-komentosarjojen suorittamisen. Reuna-solmu, jossa voit halutessasi suorittaa ScaleR's parallelized hajautettu Funktiot sydämiä reunan solmu-palvelimen kautta. Halutessaan suorittaa ne klusterin solmujen välillä käyttämällä ScaleR's Hadoop kartan pienentäminen tai ohjattu Laske kontekstit.

## <a name="compute-contexts-for-an-edge-node"></a>Laske reuna-solmu kontekstit

Yleensä R-komentosarjan, joka suoritetaan R Serverissä reuna-solmu suoritetaan R kääntäjän-solmun. Poikkeus on nämä vaiheet, joiden kutsua ScaleR-funktiota. ScaleR puhelut Suorita Laske-ympäristössä, joka määräytyy määritettyjen ScaleR Laske yhteydessä.  Kun suoritat R-komentosarjan reuna-solmu, Laske kontekstin mahdolliset arvot ovat paikallisen peräkkäisiä ('paikallinen"), paikallinen rinnakkain ("localpar"), kartan vähentää ja ohjattu.

Paikallinen' ja 'localpar' asetukset eroavat vain miten rxExec puhelut suoritetaan. Molemmat suorittaa vastaanotto-funktion vastaamatta tuleviin puheluihin rinnakkain tavalla yli kaikki käytettävissä olevat sydämiä ellei toisin ScaleR numCoresToUse vaihtoehto, kuten rxOptions(numCoresToUse=6) avulla. Seuraavassa on esitetty eri Laske konteksti-vaihtoehdoista

| Laske konteksti  | Määrittäminen                      | Suorittamisen konteksti                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Paikallinen peräkkäisiä | rxSetComputeContext('local')    | Parallelized suorittamisen lukuun ottamatta rxExec reuna-solmu-palvelimen kautta sydämiä soittaa, joka suoritetaan peräkkäin |
| Paikallinen rinnakkain   | rxSetComputeContext('localpar') | Reuna-solmu palvelimen yli sydämiä parallelized suorittamisen                                 |
| Ohjattu            | RxSpark()                       | Hajautettu suorittamisen kautta ohjattu yli solmut HDI klusterin parallelized      |
| Kartan pienentäminen       | RxHadoopMR()                    | Hajautettu suorittamisen kautta kartan vähentää yli solmut HDI klusterin parallelized |


Oletetaan, että haluat parallelized suorittamisen suorituskyvyn parantamiseksi, valitse vaihtoehtoja on kolme. Minkä vaihtoehdon valitset määräytyy laatu analytics työsi ja koko ja sijainti tiedoissa.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Ohjeet koskevat Laske konteksti

Ei tällä hetkellä ole kaavaa, joka kertoo, jossa laskemiseen käytettävä yhteydessä. On, mutta jotkin pääperiaatteet, joiden avulla voit määrittää sopiva vaihtoehto tai vähintään avulla voit tarkentaa valintasi, ennen kuin suoritat ensisijainen. Näitä koonnelmaan ovat:

1.  Paikallinen Linux-tiedostojärjestelmässä on nopeampaa kuin HDFS.
2.  Toistuvien analyysien on nopeampaa, jos tiedot ovat paikallisen ja jos se on XDF.
3.  On suositeltavaa käyttää tekstin tietolähteen; tiedot jonkin verran Jos tietojen määrä on suurempi, muuntaa sen XDF ennen analyysi.
4.  Katseltavan kopioiminen tai tietojen analysointia varten reuna-solmu streaming tulee muokata haluamallaan hyvin suuria määriä tietoja varten.
5.  Ohjattu on nopeampaa kuin kartan vähentää Hadoop analyysia varten.

Antaa nämä periaatteet joitakin yleisiä sääntöjä perussäännöt valitsemalla Laske kontekstin ovat seuraavat:

### <a name="local"></a>Paikallinen

- Jos tietojen analysointia varten määrä on pieni, eikä se ei edellytä hajonta, virtauttaa suoraan analysis-toiminto ja paikallisen' tai 'localpar'.
- Jos tietojen analysointia varten määrä on pienten ja keskisuurten ja vaatii hajonta, sitten kopioiminen paikallisen tiedostojärjestelmän, tuomalla sen XDF ja analysoida paikallisen' tai 'localpar'.

### <a name="hadoop-spark"></a>Hadoop ohjattu

- Jos tietojen analysointia varten määrä on suuri, tuo ne, XDF HDFS (paitsi jos tallennustila on ongelma)- ja analysoida 'Ohjattu' kautta.

### <a name="hadoop-map-reduce"></a>Hadoop kartan pienentäminen

- Käytä vain, jos käytössä ilmenee käyttäjäystävällinen ongelma ohjattu Laske yhteydessä käyttöä, koska yleensä voi olla hidas.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Sisäinen rxSetComputeContext ohjeita

Saat lisätietoja ja esimerkkejä ScaleR Laske kontekstit Ohje-R rxSetComputeContext-menetelmä esimerkiksi tekstiin:

    > ?rxSetComputeContext

Voit myös katsoa "ScaleR Distributed tietojenkäsittely apuviivaan" ei käytettävissä [R palvelimen MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R palvelimen MSDN-sivuston") kirjastosta.


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa opit, miten voit luoda uuden HDInsight-klusterin, joka sisältää R-palvelimeen. Opit, myös SSH istunnosta R-konsolin käytön perusteet. Nyt voit lukea tuttuihin muita tapoja käyttäminen HDInsight R-palvelimeen on seuraavissa artikkeleissa:

- [Yleistä Hadoop R-palvelin](hdinsight-hadoop-r-server-overview.md)
- [Aloita Hadoop R-palvelimen kanssa](hdinsight-hadoop-r-server-get-started.md)
- [Lisää RStudio palvelin HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure tallennustilan asetukset R palvelimen HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
