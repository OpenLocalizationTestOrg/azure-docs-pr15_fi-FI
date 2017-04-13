<properties
    pageTitle="HDInsight valinnaiseksi | Microsoft Azure"
    description="Lue, miten voit luoda ja julkaista HDInsight-sovelluksia."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Julkaise HDInsight sovellukset Azure Marketplacesta

HDInsight-sovellus on sovellus, jonka käyttäjät voivat asentaa Linux-pohjaiset HDInsight-klusterissa. Nämä sovellukset voidaan kehittää Microsoft-riippumattomat toimittajat (ISV) tai itse. Tässä artikkelissa kerrotaan julkaiseminen HDInsight-sovelluksen kyselyjä Azure Marketplacesta.  Yleisiä tietoja julkaiseminen kyselyjä Azure Marketplace-kohdassa [Julkaise tarjouksen Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight-sovellukset käyttävät *Tuoda Omat oman käyttöoikeuden (BYOL)* -malli, jossa sovellus tarjoaja on vastuussa loppukäyttäjien sovelluksen käyttöoikeudet ja loppukäyttäjien ovat vain Peri Azure he luovat, kuten HDInsight-klusterin ja sen VMs/solmujen resurssien. Tällä hetkellä sovelluksesta laskutuksen ei tehdä Azure kautta.

Muut HDInsight-sovelluksen liittyvä artikkeli:

- [Asenna HDInsight sovellukset](hdinsight-apps-install-applications.md): Lue, miten voit asentaa sovelluksen oman klustereiden Hdinsightista.
- [Mukautetun HDInsight-sovellusten asentaminen](hdinsight-apps-install-custom-applications.md): Katso, miten voit asentaa ja testata mukautettuja HDInsight-sovelluksia.

 
## <a name="prerequisites"></a>Edellytykset

Jotta voit lähettää mukautetun sovelluksesi on marketplace on olet luonut ja testaa mukautetun sovelluksen. Seuraavissa artikkeleissa:

- [Mukautetun HDInsight-sovellusten asentaminen](hdinsight-apps-install-custom-applications.md): Katso, miten voit asentaa ja testata mukautettuja HDInsight-sovelluksia.

Sinun on myös rekisteröitävä Kehitystyökalut-tilillesi. Katso [julkaista tarjouksen Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) - ja [Luo Microsoft Developer-tili](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Määritä sovellus

On kaksi vaihetta julkaisemisen Azure Marketplace-sovellukset.  Ensin määritettävä **createUiDef.json** tiedoston ilmaisemaan, mihin klustereiden sovelluksesi on yhteensopiva; ja julkaista mallin Azure-portaalista. Seuraavassa on esimerkki createUiDef.json tiedostosta.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Kenttä  | Kuvaus   | Mahdolliset arvot|
|-------|---------------|----------------|
|tyypit  | Klusterin tyypit, jotka sovellus on yhteensopiva.    |Hadoop, HBase, myrsky, ohjattu (tai näiden yhdistelmä)|
|tasoa  | Sovellus on yhteensopiva klusterin tasoa.    |Vakio-Premium (tai molemmat)|
|versiot|  HDInsight klusterin Lisättävissä olevat sovellus on yhteensopiva.    |3.4|

## <a name="package-application"></a>Paketti-sovellus

Luo zip-tiedosto, joka sisältää kaikki tarvittavat tiedostot HDInsight-sovellusten asentaminen. Sinun on zip-tiedosto, valitse [Julkaise-sovelluksessa](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Katso otoksen osoitteessa [mukautetun HDInsight-sovellusten asentaminen](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] Sovelluksen asennuksen komentosarjan nimistä nimen on oltava yksilöllinen tietyn klusterin alla muodossa. Lisäksi jokin asennus- ja poisto komentosarjatoiminnot on oltava idempotent, mikä tarkoittaa, komentosarjat voidaan kutsua repeatly taas saman tuloksen.
    
    >   nimi":" [ketjutettu (' värisävy-asennus-v0 ","-", uniquestring('applicationName')]"
        
    >Huomautus on kolme osaa Komentosarjanimi:
        
    >   1. Komentosarjan nimen etuliitteen, joka sisältää sovelluksen nimen tai kiinnostavat sovelluksen nimi.
    >   2. A "-" luettavuuden.
    >   3. Yksilöllinen merkkijono-funktio, jolla parametrina sovelluksen nimeä.

    >   Esimerkki on yllä päät määrittäminen muuttumisesta: värisävy-asennus-v0-4wkahss55hlas pysyviä komentosarjan toiminto-luettelossa. Esimerkki JSON-paketti Katso [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Kaikki tarvittavat komentosarjoja.

> [AZURE.NOTE] (Mukaan lukien web Apps-tiedostot, jos ei mitään) sovelluksen tiedostot voivat sijaita kaikkien käytettävissä minkä tahansa päätepiste.

## <a name="publish-application"></a>Sovelluksen julkaiseminen

Toimi HDInsight valinnaiseksi seuraavasti:

1. Kirjaudu [Azure Julkaisemisportaali](https://publish.windowsazure.com/).
2. Valitse vasemmalle, jos haluat luoda uuden ratkaisun mallin **ratkaisu malleja** .
3. Kirjoita otsikko ja valitse sitten **Luo uusi ratkaisumalli**.
3. Valitse **Luo keskihajonta Center tili ja Azure kehitysohjelmaan** rekisteröidä yrityksesi, jos et ole vielä tehnyt niin.  Lisätietoja on kohdassa [Create Microsoft Developer-tili](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Valitse **Määritä joitakin topologioissa avulla pääset alkuun**. Ratkaisu mallin tarkoitetaan "", kaikki sen topologioissa. Voit määrittää useita topologioissa tarjous ja ratkaisu-mallissa. Tarjouksen on valittuna väliaikaisen, se on miten täyttää sen topologioissa. 
4. Topologian nimi ja valitse sitten plus-merkkiä.
5. Kirjoita uusi versio ja valitse sitten Plus-merkkiä.
6. Valmiiden [paketin](#package-application)sovelluksessa zip-tiedoston lataaminen  
7. Valitse **pyyntö sertifikaatin**. Microsoft todistus-työryhmän tarkastelevat tiedostoja ja sertifioiminen topologian.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Asenna HDInsight sovellukset](hdinsight-apps-install-applications.md): Lue, miten voit asentaa sovelluksen oman klustereiden Hdinsightista.
- [Mukautetun HDInsight-sovellusten asentaminen](hdinsight-apps-install-custom-applications.md): käyttöönotto yhdistelmävisualisoinnin julkaistu HDInsight-sovellus, Hdinsightista.
- [Mukauta Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnon käyttäminen](hdinsight-hadoop-customize-cluster-linux.md): Lue, miten voit asentaa lisää sovelluksia komentosarja-toiminnon avulla.
- [Luo Linux-pohjaiset Hadoop klusterit HDInsight Azure Resurssienhallinta mallien avulla](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lue, miten voit soittaa Resurssienhallinta mallien avulla voit luoda HDInsight klustereiden.
- [Käytä tyhjä reunan solmujen HDInsight](hdinsight-apps-use-edge-node.md): Opi käyttämään tyhjä reuna-solmu käyttäminen HDInsight-klusterin, HDInsight-sovellusten testaaminen ja isännöinnin HDInsight-sovellukset.

