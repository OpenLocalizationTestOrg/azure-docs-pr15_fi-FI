<properties
    pageTitle="Hadoop kuitenkaan Access-sovelluksen kirjautuu ohjelmallisesti | Microsoft Azure"
    description="Access-sovelluksen kirjautuu ohjelmallisesti Hadoop HDInsight-klusterin."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Windows-pohjaisesta HDInsight kirjautuu kuitenkaan Access-sovellus

Tässä ohjeaiheessa kerrotaan, miten voit käyttää kuitenkaan (vielä toisen resurssin neuvottelija)-sovelluksissa, jotka olet lopettanut Hadoop-klusterin Azure HDInsight-lokit

> [AZURE.NOTE] Tämä asiakirja koskee vain Windows-pohjaisesta HDInsight klustereihin. Tietojen käyttäminen kuitenkaan kirjautuu Linux-pohjaiset HDInsight klustereiden, ohjeaiheessa [kuitenkaan Access-sovelluksen kirjautuu Linux-pohjaiset Hadoop-Hdinsightiin](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Edellytykset

- Windows-pohjaisesta HDInsight-klusterin.  Katso [luominen Windows-pohjaisesta Hadoop varausyksiköt Hdinsightista](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>KUITENKAAN aikajana-palvelin

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Kuitenkaan aikajanan Server</a> tarjoaa yleisiä tietoja valmiiden sovellusten sekä framework kielikohtaiset hakemuksen tiedot kaksi eri liittymää kautta. Tarkemmin:

* Tallennustilan ja HDInsight klustereiden yleisen sovelluksen tietojen noutaminen on jo käytössä 3.1.1.374-versiolla tai sitä uudemmassa versiossa.
* Aikajanan palvelimen framework kielikohtaiset sovelluksen tiedot-osa ei ole tällä hetkellä käytettävissä HDInsight klustereiden.


Yleisiä tietoja sovellusten sisältää seuraavat lajittelee tiedot:

* Sovellustunnus sovelluksen yksilöivä tunnus
* Sovelluksen aloittaneen käyttäjän
* Tehtävä suorittamiseen sovelluksen yritykset tiedot
* Sovelluksen käyttäjän yritys käyttää säilöt

Valitse HDInsight-klustereiden tiedot tallennetaan Azure resurssien hallinnan avulla historia-Store Azuren tallennustilaan oletustilin oletusarvo-säilössä. Tämän valmiiden sovellusten yleisiä tietoja voi hakea REST API-Liittymän kautta:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>KUITENKAAN sovellukset ja lokit

KUITENKAAN tukee useita ohjelmoinnin malleja (MapReduce parhaillaan jonkin niistä) irtautus Resurssienhallinta sovelluksen ajoituksen/seuranta. Tämä tapahtuu yleinen *ResourceManager* (RM), kohti työntekijä-solmun *NodeManagers* (NMS: n käynnistämä) ja sovelluksen kohti *ApplicationMasters* (AMs) kautta. Sovelluksen kohti AM verkkoliikenteen resurssit (suorittimen, muistin, levyn, verkon) sovelluksen käytön RM. Resurssinhallinta toimii NMS: n käynnistämä myöntää nämä resurssit, jotka myönnetään *säilöjen*nimellä. AM on vastuussa säilöt RM. määrittämän etenemisen seuranta Sovellus saattaa edellyttää useita säiliöiden laatu sovelluksen mukaan.

Lisäksi kunkin sovelluksen voivat koostua useista *sovellus yrittää* jotta valmis, kun kaatuu tai AM välisen menetyksiä ja RM. Näin ollen säilöjen myönnetään tietyn yrityksellä sovelluksen. Tasolla säilön sisältää perustiedot yksikön työstä maksettavan korvauksen kuitenkaan-sovelluksen yhteydessä ja säilön puitteissa tehty työ suoritetaan, säilö on kohdistettu yksittäisen työntekijän solmun. Katso [Kuitenkaan käsitteitä] [ YARN-concepts] edelleen käyttöä.

Sovelluksen lokit (ja lokitiedostot on liitetty säilö) ovat kriittisiä-virheenkorjaus ongelmallinen Hadoop-sovellukset. KUITENKAAN on hyvä framework kerätään, yhdistäminen ja [Log kooste] sovelluksen lokien tallentaminen[ log-aggregation] ominaisuus. Lokitiedoston kooste-toiminto on käytettäessä sovelluksen lokit selkeämpää-lokit kokoaa yhteen kaikki säilöt työntekijä solmun yli ja tallentaa ne yhden koostetun lokitiedoston kohti työntekijä solmu käyttöjärjestelmän sovelluksen päätyttyä. Sovelluksen voi käyttää lähimpään sataan tai säilöjen tuhansia, mutta kaikki säilöt suorittaa yhteen työntekijä solmu lokit aina kootaan yhteen tiedostoon, tuloksena on yksi lokitiedoston työntekijä solmu käyttää sovelluksen kohden. Lokitiedoston kooste on oletusarvoisesti käytössä HDInsight klustereiden (3.0-versiota ja yllä), ja koostetut lokit löytyvät oletusarvo-säilössä yhteyttä klusterin seuraavaan sijaintiin:

    wasbs:///app-logs/<user>/logs/<applicationId>

Sijainti- *käyttäjä* on sovelluksen aloittaneen käyttäjän nimi ja *applicationId* on sovelluksen kuitenkaan RM. määrittämä yksilöivä tunnus

Koostetun lokit eivät ole suoraan luettavissa, kun ne on kirjoitettu [TFile][T-file], [binaarimuodossa] [ binary-format] indeksoi säilö. KUITENKAAN työkalujen CLI Vedostaa lokitiedostot sovellusten tai säilöjen halutut pelkkänä tekstinä. Voit tarkastella lokitiedostot tekstimuotoista suorittamalla jompikumpi seuraavista kuitenkaan komennot suoraan-klusterisolmut (sen jälkeen, kun muodostat yhteyden siihen kautta RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>KUITENKAAN ResourceManager Käyttöliittymä

KUITENKAAN ResourceManager Käyttöliittymän klusterin headnode suoritetaan, ja niitä voi käyttää Azure portaalin Raporttinäkymät-ikkunan kautta: 

1. Kirjaudu [Azure-portaaliin](https://portal.azure.com/). 
2. Vasemmalla olevasta valikosta valitsemalla **Selaa**, valitse **HDInsight klustereiden**, valitse Windows-pohjaisesta klusterin, jota haluat käyttää kuitenkaan sovelluksen lokit.
3. Valitse yläreunan **raporttinäkymät-ikkunan**. Näkyviin tulee uusi selaimella avattu sivun nimeltä **HDInsight kyselyn Console**-välilehti.
4. Valitse **HDInsight kyselyn konsolin** **Kuitenkaan Käyttöliittymän**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
