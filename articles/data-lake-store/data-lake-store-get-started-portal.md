<properties 
   pageTitle="Aloittaminen järvi tietovaraston | Azure" 
   description="Tietosäilö järvi-tilin luominen ja suorittaa perustoimintoja järvi tietovaraston portaalin avulla" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Opettele Azure-portaalin avulla Azure järvi tietosäilö-tilin luominen ja suorittaa peruslaskutoimituksia, kuten luoda kansiot-tiedostojen lataaminen onedriveen ja tietoja, poista tili, jne. Lisätietoja järvi tietosäilö on [Yleiskatsaus, Azure järvi tietosäilö](data-lake-store-overview.md).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Opit nopeasti ja ladattua?

Katso seuraavilta videoilta Lake tietovaraston käytön aloittaminen.

* [Tietosäilö järvi-tilin luominen](https://mix.office.com/watch/1k1cycy4l4gen)
* [Tietosäilö järvi tietojen Resurssienhallinnassa tietojen hallinta](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Luo tili Azure järvi tietosäilö

1. Kirjaudu uudessa [Azure-portaalissa](https://portal.azure.com).

2. **Uusi**ja valitsemalla **tiedot + tallennustilan** **Azure Lake Tietosäilölle**. Lue **Azure järvi tietosäilö** -sivu ja valitse sitten **Luo** sivu vasemmassa alakulmassa.

3. Anna **Uusi järvi tietosäilö** -sivu arvot alla näyttökuvan esitetyllä tavalla:

    ![Luo uusi Azure järvi tietovaraston tili] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Luo uusi Azure tietojen järvi tili")

    - **Tilauksen**. Valitse kohdassa haluat luoda uuden järvi tietovaraston tilin tilaus.
    - **Resurssiryhmä**. Valitse aiemmin resurssiryhmä tai napsauta **Luo resurssiryhmä** , jos haluat luoda. Resurssiryhmä on säilö, joka sisältää sovelluksen liittyvät resurssit. Lisätietoja on artikkelissa [Azure resurssiryhmät](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Sijainti**: Valitse sijainti, johon järvi tietosäilö-tilin luominen.

4. Valitse **Kiinnitä Startboard** , jos haluat järvi tietosäilö-tili, jota voi käyttää Startboard.

5. Valitse **Luo**. Jos valitsit kiinnittäminen startboard tili, on toteutettu takaisin startboard ja näet järvi tietosäilö-tili-toiminnon edistymisen. Kun valmistelun yhteydessä järvi tietosäilö-tilin, tili-sivu näkyy.

6. Laajenna **Essentials** avattavasta järvi tietosäilö-tilin tiedot, kuten resurssin ryhmittely on osa, sijainti, jne. Napsauta linkkejä muihin resursseihin liittyviä järvi tietovaraston **Pika-aloitus** -kuvaketta.

    ![Oma Azure Lake Tietosäilölle tili] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Oma Azure tietojen järvi tili")

## <a name="createfolder"></a>Kansioiden luominen Azure järvi tietovaraston tili

Voit luoda kansioita järvi tietovaraston tilissä hallintaa ja tallentaa tiedot.

1. Avaa juuri luomasi järvi tietovaraston tili. Vasemmanpuoleisessa ruudussa valitsemalla **Selaa**, valitse **Järvi tietosäilö**, ja valitse sitten järvi tietosäilö-sivu-tilin nimi, jonka alle haluat luoda kansioita. Jos asiakkaan startboard kiinnitetty, napsauta kyseisen tilin-ruutua.

2. Valitse **Tietoresurssien**järvi tietosäilö-tilin sivu.

    ![Tietosäilö järvi tilin kansioiden luominen] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Tietosäilö järvi tilin kansioiden luominen")

3. Tietosäilö järvi-tilin sivu- **Uusi**kansio, kirjoita uuden kansion nimi ja valitse sitten **OK**.
    
    ![Tietosäilö järvi tilin kansioiden luominen] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Tietosäilö järvi tilin kansioiden luominen")
    
    Juuri luomasi kansion listataan **Hallinta** -sivu. Voit luoda sisäkkäisiä kansioita enintään kaikille tasoille.

    ![Tietoja järvi tilin kansioiden luominen] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Tietoja järvi tilin kansioiden luominen")


## <a name="uploaddata"></a>Tietojen lataaminen Azure järvi tietovaraston tili

Voit ladata tiedot suoraan päätasolle Azure järvi tietovaraston tili tai kansioon, jonka olet luonut tilin. Näyttökuvan, valitse noudattamalla tiedoston lataaminen alikansion **Hallinta** -sivu. Tämä näyttökuvan tiedosto ladataan mukaisesti (merkitty punainen kehys) navigointipolun alikansioon.

Jos etsit joitakin mallitietoja lataaminen, saat **Ambulanssi Data** -kansion [Azure tietojen Lake Git säilöön](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Tietojen lataaminen] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Tietojen lataaminen")


## <a name="properties"></a>Ominaisuudet ja toiminnot, jotka ovat käytettävissä tallennetut tiedot

Valitse lisätyn tiedoston **Ominaisuudet** -sivu. Tiedosto, ja voit suorittaa tiedoston toiminnot liittyvät ominaisuudet ovat käytettävissä tämä sivu. Voit myös kopioida koko polku tiedoston Azure järvi tietovaraston tilisi, korostettuna alla näyttökuvan punainen-ruutuun.

![Valitse tietojen ominaisuudet] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Valitse tietojen ominaisuudet")

* Valitse **Esikatselu** , jos haluat esikatsella tiedoston suoraan selaimessa. Voit määrittää muodon muuttaminen sekä esikatselu. **Esikatselu**, valitse **Muotoile** - **Tiedoston esikatselu** -sivu ja määritä **Esikatselu tiedostomuoto** -sivu-asetukset, kuten rivien määrän, jos haluat näyttää koodaus-erotin, joka käyttää jne.

  ![Esikatselu-tiedostomuoto] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Esikatselu-tiedostomuoto")

* Valitse Lataa tiedosto tietokoneeseen **Lataa** .

* Valitse nimeä tiedosto uudelleen valitsemalla **nimetä tiedoston uudelleen** .

* Valitse poistettava tiedosto **Poista tiedosto** .


## <a name="secure-your-data"></a>Suojata tietoja

Voit suojata Azure järvi tietovaraston tiliä Azure Active Directory- ja käyttöoikeuksien hallinta (käyttöoikeusluettelot) tallennettuja tietoja. Ohjeita, jotka hallinnasta on artikkelissa [Azure järvi tietovaraston tietojen Securing](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Poista Azure järvi tietovaraston tili

Azure järvi tietovaraston tilin poistaminen järvi tietosäilö-sivu, valitse **Poista**. Vahvistamaan toiminnon, sinua pyydetään antamaan haluat poistaa tilin nimi. Kirjoita tilin nimi ja valitse sitten **Poista**.

![Poista tietojen järvi tili] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Poista tietojen järvi tili")


## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Accessin vianmäärityslokit järvi tietovaraston varten](data-lake-store-diagnostic-logs.md)
