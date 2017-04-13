<properties 
    pageTitle="Mukauta Hadoop klustereiden ryhmän tietojen tiede prosessin | Microsoft Azure" 
    description="Suositut Python moduulit saataville mukautetun Azure Hdinsightiin Hadoop klustereissa."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Azure Hdinsightiin Hadoop klustereiden mukautetaan ryhmän tietojen tiede-prosessi 

Tässä artikkelissa kuvataan mukauttamisesta HDInsight Hadoop-klusterin asentamalla Anaconda 64-bittinen (Python 2.7) kunkin solmun, kun klusterin on valmisteltu HDInsight-palvelulle. Se näyttää myös käyttäminen headnode voit lähettää mukautetun klusterin työt. Mukauttamisen tekee monet Suositut Python-moduuleja, jotka sisältyvät Anaconda kätevästi käytettävissä käyttäjän määrittämät funktiot (UDF), jotka on suunniteltu käsittelemään klusterin tietueiden rakenne. Tässä skenaariossa menetelmät ohjeita Katso, [miten voit lähettää rakenteen kyselyt](machine-learning-data-science-move-hive-tables.md#submit).

Alla on linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit määrittää tietojen tiede ympäristöissä eri versio käyttää [Ryhmän tietojen tiede prosessi (TDSP)](data-science-process-overview.md)-valikko.

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Azure Hdinsightiin Hadoop-klusterin mukauttaminen

Jos haluat luoda mukautetun HDInsight Hadoop-klusterin, käyttäjät tarvitsevat [**Azure perinteinen Portal**](https://manage.windowsazure.com/)kirjautua valitsemalla **Uusi** vasemmassa alakulmassa ja sitten Valitse DATA SERVICES -> HDINSIGHT -> **Mukautettu luominen** ja tuo näkyviin **Klusterin tiedot** -ikkunaa. 

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Lisää määritys-sivulla 1 luoda klusterin nimen, ja hyväksy oletusarvot muita kenttiä. Napsauta nuolta, jos haluat siirtyä seuraavaan määritys-sivulla. 

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Määritys-sivulla 2 Lisää **Tiedot SOLMUJEN**määrän, **Alue/VIRTUAL verkko**-ja valitse **Pää-solmu** ja **Tietojen solmu**. Siirry seuraavaan määritys-sivulla nuolta.

>[AZURE.NOTE] **Alue/VIRTUAL verkko** on sama kuin tallennustilan tili, jota käytetään HDInsight Hadoop-klusterin suorittaminen alueen. Muussa tapauksessa neljännen määritys-sivulla käyttäjien on tarkoitus käyttää tallennustilan tilin ei näy, **Tilin nimi**avattavasta luettelosta.

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Määritys-sivulla 3 Anna käyttäjänimi ja salasana HDInsight Hadoop-klusterin. **Älä** Valitse _Enter rakenne ja Oozie Metastore_. Valitse Siirry seuraavaan määritys-sivulla olevaa nuolta. 

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Määritä määrityssivulla 4-tallennustilan tilin nimi HDInsight Hadoop-klusterin oletusarvo-säilö. Jos käyttäjien valita _luoda oletusarvon_ **Oletusarvo-SÄILÖ** avattavasta luettelosta, säilö, jossa on sama nimi kuin klusterin luodaan. Voit siirtyä viimeiseen määrityssivulla nuolta.

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Lopullinen **Komentosarjatoiminnot** määritys sivulla **lisääminen komentosarja-toiminnon** painiketta ja tekstikenttiä täyttää seuraavat arvot.
 
* **Nimi** - komentosarja-toiminto nimenä mitä tahansa merkkijonoa. 
* **SOLMUTYYPPI** - Valitse **kaikki solmut**. 
* **KOMENTOSARJAN URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* on tallennustilan tilin julkinen säilö 
    * *getgoing* Käytämme PowerShell komentosarjan tiedostojen käyttäjät työn Azure helpottamiseksi. 
* **Parametrit** - (jätä tyhjäksi)

Valitse lopuksi Aloita mukautetun HDInsight Hadoop-klusterin luominen valintamerkkiä. 

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Käyttää Hadoop-klusterin pää-solmu

Käyttäjien on otettava etäkäyttöpalvelimen Azure Hadoop-klusterin, ennen kuin he voivat käyttää Hadoop-klusterin pään solmu RDP kautta. 

1. [**Azure perinteinen Portal**](https://manage.windowsazure.com/)kirjautuminen, valitse **HDInsight** vasemmalla, Hadoop-klusterin klustereiden luettelosta, **määritys** -välilehti ja valitse sitten sivun alareunassa **Käyttöön REMOTE** -kuvaketta.
    
    ![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. Kirjoita **Määrittäminen Etätyöpöytä** -ikkunan käyttäjänimi ja salasana-kentissä, ja valitse päättymispäivän etäkäyttöä varten. Valitse käyttöön pään solmun Hadoop-klusterin etäkäyttöpalvelimen valintamerkkiä.

    ![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Käyttäjänimi ja salasana etäkäyttöä varten eivät ole, käyttäjänimi ja salasana, joita käytetään silloin, kun olet luonut Hadoop-klusterin. Nämä ovat joukko tunnistetiedot. Lisäksi etäkäyttöpalvelimen vanhenemispäivää on oltava kuluvasta päivästä seitsemän päivän ajalta.

Kun etäkäyttöpalvelimen on otettu käyttöön, valitsemalla **Yhdistä** remote sivun alareunassa pään solmu kyselyjä. Kirjaudut Hadoop-klusterin pään solmu kirjoittamalla etäkäyttöpalvelimen käyttäjälle, jonka olet määrittänyt aiemmin tunnistetiedot.

![Työtilan luominen](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Seuraavat vaiheet kehittyneen analyysin prosessin yhdistetään [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja voi sisältää ohjeet, joita tietojen siirtäminen HDInsight, käsitellä ja kuuntele sinne valmistelussa learning tiedoista Azure Konepohjaisten Oppimistekniikoiden.

Saat ohjeita siitä, miten voit käyttää Python moduulit, jotka sisältyvät Anaconda pään käyttäjän määrittämät funktiot (UDF), jota käytetään prosessin tallennettujen klusterin rakenteen tietueiden klusterin solmun [lähettäminen rakenteen kyselyt](machine-learning-data-science-move-hive-tables.md#submit) .

 
