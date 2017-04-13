<properties
   pageTitle="Rekisteröi järvi tietovaraston tietojen Azure tietoluettelon | Microsoft Azure"
   description="Rekisteröi järvi tietovaraston tietojen Azure-tietoluettelo"
   services="data-lake-store,data-catalog" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Rekisteröi järvi tietovaraston tietojen Azure-tietoluettelo

Tässä artikkelissa opit miten Azure järvi tietovaraston integroida Azure tietoluettelon kirjastonäkymiä tietojen organisaation integroimalla tietoluettelon avulla. Lisätietoja luettelointia tiedot on artikkelissa [Azure tietoluettelon](../data-catalog/data-catalog-what-is-data-catalog.md). Skenaariot, jossa voit käyttää tietojen on artikkelissa [Azure tietoluettelon Yleisiä tilanteita, joissa](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- Tietojen järvi kaupan julkisen esikatselu **käyttöön Azure tilauksen** . Katso [ohjeet](data-lake-store-get-started-portal.md#signup).

- **Azure järvi tietovaraston tili**. Ohjeiden etsiminen [Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md). Tässä opetusohjelmassa uutiskirjeistä luoda kutsutaan **datacatalogstore**Lake tietosäilö-tili. 

    Kun olet luonut tilin, otoksen tietojoukon lataaminen. Tässä opetusohjelmassa Lataa uutiskirjeistä kaikki .csv-tiedostoissa [Azure tietojen Lake Git säilöön](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/) **AmbulanceData** -kansiossa. Voit käyttää eri asiakkaiden, kuten [Azure-tallennustilan Explorer](http://storageexplorer.com/)tietojen lataaminen blob-säilö.

- **Azure tietojen luettelon**. Organisaatiossa on jo luotu organisaation Azure-tietoluettelon. Vain yhden luettelon sallitaan kunkin organisaation.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Rekisteröi Lake Tietosäilölle tietoluettelon lähteenä

>[AZURE.VIDEO adcwithadl] 

1. Siirry `https://azure.microsoft.com/services/data-catalog`, ja valitse **Aloita**.

2. Kirjaudu sisään Azure tietoluettelon-portaalissa, ja valitse **Julkaise tiedot**.

    ![Rekisteröi tietolähde] (./media/data-lake-store-with-data-catalog/register-data-source.png "Rekisteröi tietolähde")

3. Valitse seuraavalla sivulla **Käynnistä sovellus**. Tämä Lataa sovellus luettelon tiedosto tietokoneesta. Kaksoisnapsauta luettelon tiedostoa Käynnistä sovellus.

4. Valitse Tervetuloa-sivulla **Kirjaudu sisään**ja anna tunnistetiedot.

    ![Aloitusnäyttö] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Aloitusnäyttö")

5. Valitse Valitse tietolähde-sivulla Valitse **Azure tietojen järvi**ja valitse sitten **Seuraava**.

    ![Valitse tietolähde] (./media/data-lake-store-with-data-catalog/select-source.png "Valitse tietolähde")

6. Seuraavalla sivulla on järvi tietovaraston tilin nimi, jonka haluat rekisteröidä tietoluettelon. Jätä muut asetukset oletukseksi ja valitse **Yhdistä**.

    ![Muodosta yhteys tietolähteeseen] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Muodosta yhteys tietolähteeseen")

7. Seuraavalla sivulla voidaan jakaa seuraavat osat.

    a. **Palvelimen hierarkia** -ruutu kuvaa järvi tietovaraston tilin kansiorakenne. **$Root** Lake tietovaraston tilin pääkansio ja **AmbulanceData** on luoda järvi tietosäilö-tilin kansion.

    b. **Käytettävissä olevat objektit** -ruudussa luetellaan tiedostot ja kansiot **AmbulanceData** -kansioon.

    c-näppäinyhdistelmää. **Objektien rekisteröity valinta** näyttää tiedostot ja kansiot, jotka haluat rekisteröidä Azure tietoluettelon.

    ![Näytä-tietorakenne] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Näytä-tietorakenne")

8. Tässä opetusohjelmassa, Rekisteröi kaikki tiedostot kansiossa. Kohdalla valitse (![siirtää objekteja](./media/data-lake-store-with-data-catalog/move-objects.png "siirtää objekteja"))-painiketta voit siirtää kaikki tiedostot **voi rekisteröidä objekteihin** -valintaruutu. 

    Tiedot on rekisteröity organisaationlaajuisen tietoluettelon, koska se on recommened Lisää joitakin metatietoja, jonka avulla voit myöhemmin voit etsiä nopeasti tietoja. Voit esimerkiksi lisätä sähköpostiosoitteen tietojen omistajan (esimerkiksi yksi kuka ladataan tiedot) tai lisätä tunnisteen tiedot. Näyttökuvan alla näkyy tunnisteen kaavaan lisätään tietoihin.

    ![Näytä-tietorakenne] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Näytä-tietorakenne")

    Valitse **Rekisteröi**.

8. Seuraavat näyttökuvan ilmaisee, että tiedot on rekisteröitynyt tietoluettelon.

    ![Rekisteröityminen on valmis] (./media/data-lake-store-with-data-catalog/registration-complete.png "Näytä-tietorakenne")

9. Valitse **Näytä Portal** palaa tietoluettelon portaalin ja varmista, että voit nyt käyttää rekisteröidyt tiedot-portaalista. Jos haluat etsiä tietoja, voit käyttää käytit, kun tietoihin rekisteröiminen tunniste.

    ![Luettelon tietojen hakeminen] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Luettelon tietojen hakeminen")

10. Nyt voit suorittaa toimintoja, kuten lisätä huomautuksia ja dokumentaatio tiedot. Lisätietoja on seuraavissa linkeissä.
    * [Huomautuksia tietoluettelon tietolähteet](../data-catalog/data-catalog-how-to-annotate.md)
    * [Tietoluettelo Asiakirjatietolähteet](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Katso myös

* [Huomautuksia tietoluettelon tietolähteet](../data-catalog/data-catalog-how-to-annotate.md)
* [Tietoluettelo Asiakirjatietolähteet](../data-catalog/data-catalog-how-to-documentation.md)
* [Integroi järvi tietovaraston Azure muiden palvelujen kanssa](data-lake-store-integrate-with-other-services.md)
