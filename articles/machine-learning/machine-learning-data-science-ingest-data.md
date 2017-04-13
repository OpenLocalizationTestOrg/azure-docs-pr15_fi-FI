<properties 
    pageTitle="Lataa tietojen analysoinnissa ympäristöissä tallennustilan | Microsoft Azure" 
    description="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Lataa tietojen analysoinnissa tallennustilan ympäristöissä

Ryhmän tietojen tiede-prosessi edellyttää tietoja nautittuina tai eri tallennustilan ympäristöissä käsitellään tai analysoida prosessin kussakin vaiheessa parhaiten eri ladattu. Tietojen kohteet käytettyjä käsittelyä varten ovat Azure Blob-objektien tallennustilaan, SQL Azure-tietokannoista, SQL Server Azure AM, HDInsight (Hadoop) ja Azure koneen Learning. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit ingest tietojen tuominen kohde näissä ympäristöissä, jossa tiedot tallennetaan ja käsitellään.

Tekniset ja liiketoimintatarpeiden sekä alkuperäisen sijainnin, muotoileminen ja koon tietojen määräytyy kohde-ympäristössä, johon tiedot vaativat nieltäväksi analyysin tavoitteet. Monimutkaisessa tilanne edellyttämään tietoja on siirrettävä saavuttamiseksi tarvitaan ennakoivan mallin tehtävät erilaisia eri ympäristöjen välillä. Tässä järjestyksessä tehtävien voivat olla esimerkiksi tietojen tarkasteluun, vanhat käsittely, puhdistus, alaspäin esimerkkejä ja mallin koulutus.
