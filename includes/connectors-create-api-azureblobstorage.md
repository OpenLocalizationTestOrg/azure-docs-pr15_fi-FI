### <a name="prerequisites"></a>Edellytykset
- Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)
- [Azure-Blob-säiliö tilin](../articles/storage/storage-create-storage-account.md) tallennustilan tilin nimi ja sen pikanäppäin. Nämä tiedot näkyy ominaisuudet-tallennustilan tilin Azure-portaalissa. Lisätietoja on artikkelissa [Azure-tallennustilan](../articles/storage/storage-introduction.md).

Ennen kuin käytät tiliäsi Azure-Blob-säiliö logiikan-sovelluksessa, Yhdistä Azure-Blob-säiliö-tiliisi. Voit tehdä tämän helposti sisällä logiikan sovelluksen Azure-portaalissa.  

Yhdistä Azure-Blob-säiliö-tilisi seuraavasti:  

1. Logiikan sovelluksen luominen. Logiikan sovellusten suunnittelussa Lisää käynnistin ja lisää sitten toiminnon. Valitse avattavassa luettelossa **Näytä Microsoft hallitun ohjelmointirajapinnan** ja Kirjoita hakukenttään "blob". Valitse jokin seuraavista:  

    ![Azure-Blob-säiliö yhteyden luominen vaihe](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Jos et ole aiemmin luonut mahdolliset yhteydet Azure säilöön, sinua kehotetaan yhteystiedot:   

    ![Azure-Blob-säiliö yhteyden luominen vaihe](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Anna tallennustilan tilin tiedot. Tarvittavat ominaisuudet tähdellä.

    | Ominaisuus | Tiedot |
|---|---|
| Yhteyden nimi * | Kirjoita mitään yhteyttä. |
| Azure-tallennustilan tilin nimi * | Kirjoita tallennustilan tilin nimi. Tallennustilan tilin nimi näkyy tallennustilan ominaisuuksiin Azure-portaalissa. |
| Azure-tallennustilan tilin pikanäppäin * | Kirjoita tallennustilan tilin-näppäintä. Pikanäppäimet näkyvät tallennustilan ominaisuudet Azure-portaalissa. |

    Nämä käytetään sallivat logiikan sovelluksen yhteyden ja käyttää tietoja. 

4. Valitse **Luo**.

5. Huomaa, yhteys on luotu. Nyt jatkaa logiikan sovelluksen muita ohjeita: 

    ![Azure-Blob-säiliö yhteyden luominen vaihe](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
