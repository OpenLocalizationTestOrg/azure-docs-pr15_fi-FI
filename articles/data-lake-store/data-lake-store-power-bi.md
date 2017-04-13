<properties
   pageTitle="Tietosäilö järvi tietojen analysointia käyttämällä Power BI | Microsoft Azure"
   description="Käytä Power BI Azure tietojen järvi säilöön tallennettuja tietojen analysointia varten"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Tietosäilö järvi tietojen analysointia käyttämällä Power BI

Tässä artikkelissa kerrotaan Power BI Desktopin käyttäminen analysointiin ja Visualisoi Azure tietojen järvi säilöön tallennettuja tietoja.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure järvi tietovaraston tili**. Ohjeiden etsiminen [Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md). Tässä artikkelissa oletetaan, että on jo luonut järvi tietovaraston tilin **mybidatalakestore**ja ladata mallitiedosto (**Drivers.txt**). Tämän mallitiedoston on ladattavissa [Azure tietojen Lake Git](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)säilöstä.

- **Power BI Desktopin**. Voit ladata [Microsoft Download](https://www.microsoft.com/en-us/download/details.aspx?id=45331)Centeristä. 


## <a name="create-a-report-in-power-bi-desktop"></a>Power BI Desktopin raportin luominen

1. Käynnistä Power BI Desktopin tietokoneeseen.

2. Valitse **Aloitus** -valintanauhasta **Nouda**tiedot ja valitse sitten Lisää. **Nouda tiedot** -valintaikkunassa **Azure**, **Azure tietojen järvi**kauppa ja valitse sitten **Yhdistä**.

    ![Yhteyden muodostaminen järvi tietosäilö] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Yhteyden muodostaminen järvi tietosäilö")

3. Jos näet valintaikkuna on kehittäminen vaiheen Connectorista, halutessaan Jatka.

4. **Microsoft Azure järvi tietosäilö** -valintaikkunassa Anna tilisi järvi tietosäilö URL-osoite ja valitse sitten **OK**.

    ![Tietosäilö järvi URL-osoite] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Tietosäilö järvi URL-osoite")

5. Valitse seuraavassa valintaikkunassa Kirjaudu järvi tietovaraston tilille **Kirjautuminen** . Sinut ohjataan organisaation kirjautumissivulla. Kirjaudu sisään tilille kehotteiden mukaisesti.

    ![Kirjaudu sisään järvi tietosäilö] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Kirjaudu sisään järvi tietosäilö")

6. Kun olet kirjautunut sisään, valitse **Yhdistä**.

    ![Yhteyden muodostaminen järvi tietosäilö] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Yhteyden muodostaminen järvi tietosäilö")

7. Seuraavassa valintaikkunassa näkyy järvi tietovaraston tiliisi lataamasi tiedosto. Tarkista tiedot ja valitse sitten **Lataa**.

    ![Lataa tiedot järvi tietosäilö] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Lataa tiedot järvi tietosäilö")

8. Kun tiedot on ladattu Power BI-näet seuraavat kentät **kentät** -välilehti.

    ![Tuotu kentät] (./media/data-lake-store-power-bi/imported-fields.png "Tuotu kentät")

    Kuitenkin esittää ja analysoida tietoja, emme mieluummin tiedot ovat käytettävissä seuraavat kentät kohden

    ![Halutuksi kentät] (./media/data-lake-store-power-bi/desired-fields.png "Halutuksi kentät")

    Seuraavat vaiheet päivitämme kyselyn muuntaa tuodut tiedot, valitse haluamasi muoto.

9. Valitse **Aloitus** -valintanauhan **Muokkaa kyselyt**.

    ![Kyselyjen muokkaaminen] (./media/data-lake-store-power-bi/edit-queries.png "Kyselyjen muokkaaminen")

10. Valitse Kyselyeditorissa **Content** -saraketta, valitse **binaariluvun**.

    ![Kyselyjen muokkaaminen] (./media/data-lake-store-power-bi/convert-query1.png "Kyselyjen muokkaaminen")

11. Tiedosto-kuvaketta, joka vastaa lataamasi **Drivers.txt** tiedosto tulee näkyviin. Napsauta tiedostoa hiiren kakkospainikkeella ja valitse **CSV**.  

    ![Kyselyjen muokkaaminen] (./media/data-lake-store-power-bi/convert-query2.png "Kyselyjen muokkaaminen")

12. Tulos näkyy alla kuvatulla tavalla. Tiedot ovat nyt käytettävissä, joiden avulla voit luoda muodossa.

    ![Kyselyjen muokkaaminen] (./media/data-lake-store-power-bi/convert-query3.png "Kyselyjen muokkaaminen")

13. **Aloitus** -valintanauhassa **Sulje**ja käytä ja valitse sitten **Sulje ja käyttää**.

    ![Kyselyjen muokkaaminen] (./media/data-lake-store-power-bi/load-edited-query.png "Kyselyjen muokkaaminen")

14. Kun kysely on päivitetty, **kentät** -välilehti näkyy käytettävissä visualisoinnin uusia kenttiä.

    ![Päivitetty kentät] (./media/data-lake-store-power-bi/updated-query-fields.png "Päivitetty kentät")

15. Anna meidän edustavan kunkin kaupungin tietyn maan ohjaimet ympyräkaavion luominen. Kiellä, tee seuraavat valinnat.

    1. Valitse visualisoinnit-välilehdessä ympyräkaavion symbolia.

        ![Ympyräkaavion luominen] (./media/data-lake-store-power-bi/create-pie-chart.png "Ympyräkaavion luominen")

    2. Sarakkeet, jotka ovat siirtymällä käyttää ovat **sarake 4** (kaupungin nimen) ja **sarakkeen 7** (maan nimi). Vedä **kentät** -välilehden näiden sarakkeiden **Visualisoinnit** -välilehdestä alla kuvatulla tavalla.

        ![Luo visualisoinnit] (./media/data-lake-store-power-bi/create-visualizations.png "Luo visualisoinnit")

    3. Ympyräkaavion pitäisi nyt muistuttaa alla kuvatun kaltaisia.

        ![Ympyräkaavio] (./media/data-lake-store-power-bi/pie-chart.png "Luo visualisoinnit")

16. Valitsemalla tietyn maan sivun tason suodattimia, näet nyt ohjaimet kunkin kaupungin valitun maan määrä. Valitse **Visualisoinnit** -välilehdessä **sivun tasolla suodattimet**, valitse **Brasilia**.

    ![Valitse maa] (./media/data-lake-store-power-bi/select-country.png "Valitse maa")

17. Ympyräkaavion päivitetään automaattisesti näytönohjaimia Brasilian kaupungeissa.

    ![Maassa ohjaimet] (./media/data-lake-store-power-bi/driver-per-country.png "Ohjaimet maan kohden")

18. Valitse **Tiedosto** -valikosta Tallenna visualisointi Power BI Desktopin-tiedostona **tallentaminen** .

## <a name="publish-report-to-power-bi-service"></a>Raportin julkaiseminen Power BI-palvelu

Kun olet luonut visualisointeja Power BI Desktopin, voit jakaa sen muiden kanssa julkaisemalla sen Power BI-palveluun. Ohjeita siitä, miten voit tehdä on artikkelissa [Power BI Desktop Julkaise](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Katso myös

* [Tietosäilö järvi käyttäminen tietojen järvi Analytics tietojen analysointia](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
