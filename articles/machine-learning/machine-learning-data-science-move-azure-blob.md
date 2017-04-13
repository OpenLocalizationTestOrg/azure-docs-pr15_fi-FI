<properties
    pageTitle="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö | Microsoft Azure"
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
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Tietojen siirtäminen tai Azure-Blob-säiliö tekniikoista ohjeita on linkitetty tähän:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Mitä tapaa parhaiten vaihtelee tilanteen mukaan. [Tilanteita, joissa kehittyneen analyysin Azure koneen Learning-](machine-learning-data-science-plan-sample-scenarios.md) artikkelin avulla voit määrittää tietojen tiede työnkulut kehittyneen analyysin prosessissa käytetyn erilaisille tarvittavat resurssit.

> [AZURE.NOTE] Johdatus Azure-blob-säiliö löydät lisätietoja [Azure](../storage/storage-dotnet-how-to-use-blobs.md) -Blob-objektien perustoimintojen ja [Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx)-Blob-palveluun.

Vaihtoehtoisesti voit [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) avulla: 

- luoda ja ajoittaa myyntijakso, lataa Azure-blob-säiliö 
- siirtää julkaistun Azure koneen Learning-verkkopalvelun 
- ennakoivan analytics-tulokset ja 
- Lataa tulokset tallennustilan. 

Lisätietoja on artikkelissa [Luo ennakoivan putkistot Azure Data Factory ja Azure koneen Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Edellytykset

Tämän asiakirjan oletetaan, että Azure tilauksen tallennustilan tilin ja vastaavan tallennustilan näppäimen tilin. Ennen kuin lataaminen ja latauksen, sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin avainta.

- Voit määrittää Azure tilauksen-kohdassa [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).
- Lisätietoja tallennustilan tilin luominen ja tilin ja avaintiedoista on artikkelissa [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).
