<properties 
    pageTitle="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Azure-tallennustilan Resurssienhallinnassa | Microsoft Azure" 
    description="Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Azure-tallennustilan Resurssienhallinnassa" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Tietojen siirtäminen ja sieltä pois Azure-Blob-säiliö Azure-tallennustilan Resurssienhallinnassa

Azure tallennustilan Explorer on ilmainen työkalu Microsoftilta, jonka avulla voit käyttää Windows, Mac OS ja Linux Azuren tallennustilaan tiedoilla. Tässä ohjeaiheessa kerrotaan, miten voit ladata ja tietojen lataaminen Azure-blob-säiliö. Työkalu voi ladata [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com/).

Tietojen siirtäminen tai Azure-Blob-säiliö tekniikoista ohjeita on linkitetty tähän:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Jos käytössäsi on AM, joka on määritetty tarjoamia [tietoja tiede Virtual koneet Azure-](machine-learning-data-science-virtual-machines.md)komentosarjoja, sitten Azure-tallennustilan Explorer on jo asennettu AM.
 
> [AZURE.NOTE] Johdatus Azure-blob-säiliö, saat lisätietoja [Azure-Blob-perustiedot](../storage/storage-dotnet-how-to-use-blobs.md) ja [Azure-Blob-palvelu](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Edellytykset

Tämän asiakirjan oletetaan, että Azure tilauksen tallennustilan tilin ja vastaavan tallennustilan näppäimen tilin. Ennen kuin lataaminen ja latauksen, sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin avainta. 

- Voit määrittää Azure tilauksen-kohdassa [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).
- Lisätietoja tallennustilan tilin luominen ja tilin ja avaintiedoista on artikkelissa [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md). Merkitse muistiin pikanäppäin tallennustilan tilin, kun tarvitset avaimeen muodostaa yhteyden tiliisi Azure-tallennustilan Explorer-työkalulla.
- Azure-tallennustilan Explorer-työkalun voi ladata [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com/). Hyväksy oletusarvot asennuksen aikana.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure-tallennustilan Resurssienhallinnassa 

Seuraavat vaiheet asiakirjan miten Lataa ja lataa tiedot Azure-tallennustilan Resurssienhallinnassa. 

1.  Käynnistä Microsoft Azure-tallennustilan Explorer.
2.  Kun kosketat **... tiliisi kirjautuminen** ohjatun toiminnon, valitse **Azure tilin asetukset** -kuvaketta, valitse **Lisää tili** ja anna tunnistetiedot. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Voit noutaa **Azuren tallennustilaan muodostaminen** ohjatun toiminnon, valitsemalla **Yhdistä Azure-tallennustilan** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Anna pikanäppäin Azure-tallennustilan tilin **Muodosta yhteys Azuren tallennustilaan** ohjattu ja valitse sitten **Seuraava**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Kirjoita **tilin nimi** -ruutuun tallennustilan tilin nimi ja valitse sitten **Seuraava**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Lisätty tallennustilan tilin nyt pitäisi luetteloitu. Voit luoda blob-säilö tallennustilan tilillä tilin **Blob säilöt** -solmu hiiren kakkospainikkeella, valitse **Luo Blob-säilö**ja nimi.
7. Tietojen lataaminen säilön, valitse kohde-säilö ja napsauttamalla **Lataa** -painiketta.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Valitse **...** **tiedostot** -ruudun oikealla puolella, valitse, valitse yksi tai useita tiedostoja ladataan tiedostojärjestelmästä ja valitse Aloita tiedostojen lataamisesta **Lataa** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Lataa tiedot, valitsemalla blob vastaavan säilön ja lataa ja valitse **Lataa**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


