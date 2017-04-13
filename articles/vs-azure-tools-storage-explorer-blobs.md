<properties
    pageTitle="Azure-Blob-säiliö resurssien tallennustilan Resurssienhallinnassa (ennakkoversio) | Microsoft Azure"
    description="Hallitse Azure-Blob-säilöjen ja BLOB Storage Resurssienhallinnassa (ennakkoversio)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Azure-Blob-säiliö resurssien tallennustilan Resurssienhallinnassa (ennakkoversio)

## <a name="overview"></a>Yleiskatsaus

[Azure-Blob-säiliö](./storage/storage-dotnet-how-to-use-blobs.md) on projektiin paljon erimuotoisia tietoja, kuten binaaritietoja tai tiedot-, jota voi käyttää mitä tahansa maailmanlaajuisesti HTTP tai HTTPS-palvelu.
Voit käyttää Blob-objektien tallennustilaan voit näyttää tietoja julkisesti maailmalle tai voit tallentaa sovelluksen tietoja yksityisesti. Tässä artikkelissa käydään toimimaan blob säilöjen Explorer Storage (ennakkoversio) ja BLOB käyttäminen.

## <a name="prerequisites"></a>Edellytykset

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- [Lataa ja asenna tallennustilan Explorer (ennakkoversio)](http://www.storageexplorer.com)
- [Yhteyden muodostaminen Azure tallennustilan tili tai palvelu](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Luo blob-säilö

Kaikki Blob-objektien on sijaittava blob säilön, joka on yksinkertaisesti looginen ryhmittely BLOB-objektit. Tilin voi sisältää rajoittamattoman säilöjä ja kunkin säilön voidaan tallentaa BLOB rajoittamaton määrä.

Seuraavat vaiheet osoittavat luomisesta blob-säilö tallennustilan Resurssienhallinnan (ennakkoversio).

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan tilin määräaika, jonka haluat luoda blob-säilö.
1.  **Blob-objektien säilöjen**hiiren kakkospainikkeella ja - pikavalikko - kohdassa **Luo Blob-säilö**.

    ![Luo blob säilöt-pikavalikko][0]

1.  Tekstiruudun näkyy alla **Blob säilöt** -kansio. Kirjoita nimi blob-säilö. Nimeäminen blob säilöjen artikkelissa [säilö nimeämiskäytäntö säännöt](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) -osassa luettelon säännöt ja rajoitukset.

    ![Blob-objektien säilöjen tekstiruudun luominen][1]

1.  Paina **Enter** kun olet valmis luomaan blob-säilö tai Peruuta **ESC-näppäintä** . Kun blob-säilö on luotu, se näkyy kohdassa valitun tallennustilan tilin **Blob säilöt** -kansio.

    ![Luotu BLOB-säilö][2]

## <a name="view-a-blob-containers-contents"></a>Blob-säilö sisällön tarkasteleminen

Blob-objektien säilöt sisältävät BLOB-objektit ja kansiot (joka voi sisältää myös BLOB).

Seuraavat vaiheet kuvaavat blob-säilö tallennustilan Resurssienhallinnan (ennakkoversio) sisällön tarkasteleminen:

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan tiliä, joka sisältää blob-säilö haluat tarkastella.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Haluatko tarkastella blob-säilö hiiren kakkospainikkeella ja - pikavalikko - kohdassa **Avaa Blob-säilö editori**.
Voit myös kaksoisnapsauttaa blob-säilö haluat tarkastella.

    ![Avaa blob-säilö editorin pikavalikko][19]

1.  Pääikkunassa näkyy blob-säilö sisältö.

    ![BLOB-säilö-editori][3]

## <a name="delete-a-blob-container"></a>Poista blob-säilö

Blob-objektien säilöjen voi helposti luotujen ja poistettujen tarpeen mukaan. (Näytetään, kuinka voit poistaa yksittäisiä BLOB-viittaa, [hallinta-BLOB-blob-säilö](./#managing-blobs-in-a-blob-container).)

Seuraavat vaiheet kuvaavat blob Storage Resurssienhallinnan (ennakkoversio) säilön poistaminen:

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan tiliä, joka sisältää blob-säilö haluat tarkastella.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Hiiren kakkospainikkeella poistettavan blob-säilö ja - pikavalikko - kohdassa **Poista**.
Voit myös painaa **Poista** , jos haluat poistaa valitun blob-säilö.

    ![Poista blob-säilö pikavalikko][4]

1.  Valitse **Kyllä** , jos vahvistus-valintaikkuna.

    ![Poista blob-säilö vahvistus][5]

## <a name="copy-a-blob-container"></a>Kopioi blob-säilö

Tallennustilan Explorer (ennakkoversio) mahdollistaa blob-säilö Kopioi Leikepöydälle ja liitä blob kyseisen säilön tallennustilan toiseen tiliin. (Saat kopioimisesta yksittäisiä BLOB-viittaa, [hallinta-BLOB-blob-säilö](./#managing-blobs-in-a-blob-container).)

Seuraavat vaiheet osoittavat kopioimisesta blob-säilö yhden tallennustilan tilin.

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan tiliä, joka sisältää blob-säilö haluat kopioida.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Haluat kopioida blob-säilö hiiren kakkospainikkeella ja - pikavalikosta - Valitse **Kopioi Blob-säilö**.

    ![Kopioi blob-säilö pikavalikko][6]

1.  Hiiren kakkospainikkeella haluamaasi "kohde"-tallennustilan tilin, johon haluat liittää blob-säilö ja - pikavalikko - kohdassa **Liitä Blob-säilö**.

    ![Liitä blob-säilö pikavalikko][7]

## <a name="get-the-sas-for-a-blob-container"></a>Hanki Suojaussidokset blob-säilö

[Jaettu käyttö allekirjoitus (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) tarjoaa valtuutetun resurssien tallennustilan tilissäsi.
Tämä tarkoittaa, että voit myöntää asiakkaan rajoitetut käyttöoikeudet objektien tallennustila-tilisi vuosipoiston annettuna kautena ajan ja käyttöoikeudet-määritetyn eikä sinun tarvitse jakaa tilin pikanäppäimet.

Seuraavat vaiheet kuvaavat luomisesta SAS blob-säilö varten:

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan-tiliä, joka sisältää, jonka haluat saada SAS blob-säilö.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Hiiren kakkospainikkeella haluamaasi blob-säilö ja - pikavalikosta – Valitse **Hae jaettu Access-allekirjoitus**.

    ![Hae SAS pikavalikko][8]

1.  **Jaettu Access allekirjoitus** -valintaikkunassa määrittää käytännön, alkamis-ja vanhenemista, aikavyöhykkeen sekä käyttää resurssin haluamasi tasot.

    ![Saat näkyviin SAS vaihtoehtoja][9]

1.  Kun olet valmis SAS asetusten määrittäminen, valitse **Luo**.

1.  Toisen **Jaettu Access-varmenne** -valintaikkuna sitten avautuu näkymään, jossa näkyvät sekä URL-osoite ja voit käyttää tallennustilan resurssia QueryStrings blob-säilö.
Valitse **Kopioi** URL-Osoitteen, jonka haluat kopioida Leikepöydälle vieressä.

    ![Kopioi SAS URL-osoitteet][10]

1.  Kun olet valmis, valitse **Sulje**.

## <a name="manage-access-policies-for-a-blob-container"></a>Käytäntöjen hallinta blob-säilö

Seuraavissa vaiheissa esitellään hallinnasta (lisääminen ja poistaminen) käyttää blob-säilö käytännöt:

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan-tiliä, joka sisältää blob-säilö jonka käytäntöjen haluat hallita.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Valitse haluamasi blob-säilö ja - pikavalikko - kohdassa **Käytäntöjen hallinta**.

    ![Hallitse access käytännöt pikavalikko][11]

1.  **Käytäntöjen** -valintaikkunan sisältyviä jo luonut valitun blob-säilö access käytännöt.

    ![Käytännön käyttöasetukset][12]        

1.  Accessin käytännön hallinta-tehtävän mukaan seuraavasti:

    - **Lisää uuden käytännön access** - Valitse **Lisää**. Kun luonut, **Käytäntöjen** -valintaikkunassa näkyy lisätyn käyttöoikeuskäytäntö (oletusasetuksilla).
    - **Muokkaa käytäntöä access** - Tee haluamasi muutokset ja valitse **Tallenna**.
    - **Poista käyttöoikeuskäytäntö** - valitsemalla **Poista** poistettavan käyttöoikeuskäytäntö vieressä.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Julkisen käyttöoikeustason määrittäminen blob-säilö

Oletusarvoisesti jokaisella blob-säilö on määritetty "Ei julkisen käyttöoikeuksia".

Seuraavat vaiheet osoittavat blob-säilö yleisön tason määrittäminen.

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan-tiliä, joka sisältää blob-säilö joiden haluat hallita käytäntöjen.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Valitse haluamasi blob-säilö ja - pikavalikko - kohdassa **Aseta yleisön taso**.

    ![Määritä yleisön tason pikavalikko][13]

1.  Määritä **Aseta säilö yleisön taso** -valintaikkunasta haluamasi käyttöoikeustaso.

    ![Yleisön tason asetusten määrittäminen][14]

1.  Valitse **Käytä**.

## <a name="managing-blobs-in-a-blob-container"></a>Hallinta BLOB-blob-säilö

Kun olet luonut blob-säilö, voit ladata blob kyseisen blob-säilö, lataa blob paikalliseen tietokoneeseen, Avaa blob paikalliseen tietokoneeseen ja paljon muuta.

Seuraavat vaiheet osoittavat blob säilöön hallinnasta BLOB-objektit (ja kansiot).

1.  Avaa Resurssienhallinta Storage (esikatselu).
1.  Laajenna vasemmanpuoleisessa ruudussa tallennustilan tiliä, joka sisältää blob-säilö haluat hallita.
1.  Laajenna tallennustilan tilin **Blob säilöjen**.
1.  Kaksoisnapsauta blob-säilö haluat tarkastella.
1.  Pääikkunassa näkyy blob-säilö sisältö.

    ![Näytä blob-säilö][3]

1.  Pääikkunassa näkyy blob-säilö sisältö.

1.  Haluat suorittaa tehtävän mukaan seuraavasti:

    - **Tiedostojen lataaminen blob-säilö**

        1.  Valitse pääikkunassa-työkalurivin **Lataa**ja valitse sitten **Lataa tiedostoja** avattavasta valikosta.

            ![Lataa tiedostot-valikko][15]

        1.  **Tiedostojen lataaminen** -valintaikkunassa valitse kolme pistettä (****...)-painiketta, valitse tiedostot, jonka haluat ladata **tiedostoja** teksti-ruudun oikealla puolella.

            ![Lataa tiedostot asetukset][16]

        1.  Määritä **Blob tyyppi**tyyppi. [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) artikkelissa kerrotaan blob eri välisiä eroja.

        1.  Voit myös määrittää kohdekansio, johon valitut tiedostot ladataan. Jos kohdekansio ei ole valittu, se luodaan.

        1.  Valitse **Lataa**.

    - **Kansion lataaminen blob-säilö**

        1.  Valitse pääikkunassa-työkalurivin **Lataa**ja valitse sitten **Lataa kansion** avattavasta valikosta.

            ![Lataa kansio-valikko][17]

        1.  **Lataa kansio** -valintaikkunassa, valitse Valitse kansio, jonka sisällön haluat ladata **kansion** teksti-ruudun oikealla puolella olevat kolme pistettä (****...)-painike.

            ![Lataa Kansioasetukset][18]

        1.  Määritä **Blob tyyppi**tyyppi. [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) artikkelissa kerrotaan blob eri välisiä eroja.

        1.  Voit myös määrittää kohdekansio, johon valitun kansion sisältöä ladataan. Jos kohdekansio ei ole valittu, se luodaan.

        1.  Valitse **Lataa**.

    - **Lataa blob paikalliseen tietokoneeseen**

        1.  Valitse blob, jonka haluat ladata.

        1.  Valitse pääikkunassa-työkalurivin **Lataa**.

        1.  **Määritä tallennussijainti ladatut blob** -valintaikkunassa kohtaa, johon haluat ladata Blob-objektien sijainti ja nimi, jonka haluat antaa sille.  

        1.  Valitse **Tallenna**.

    - **Avaa Blob-objektien paikallisessa tietokoneessa**

        1.  Valitse blob, jonka haluat avata.

        1.  Valitse pääikkunassa työkaluriviltä **Avaa**.

        1.  Blob ladataan ja avata liittyvät Blob-objektien pohjana tiedostotyyppi-sovelluksesta.

    - **Kopioi Leikepöydälle blob**

        1.  Valitse blob, josta haluat kopioida.

        1.  Valitse pääikkunassa työkaluriviltä **Kopioi**.

        1.  Vasemmassa ruudussa Siirry blob-säilö ja kaksoisnapsauttamalla tarkasteleminen pääikkunassa.

        1.  Valitse **Liitä** luoda kopion blob pääikkunassa-työkalurivin.

    - **Poista blob**

        1.  Valitse poistettavan Blob-objektien.

        1.  Valitse pääikkunassa työkaluriviltä **Poista**.

        1.  Valitse **Kyllä** , jos vahvistus-valintaikkuna.

## <a name="next-steps"></a>Seuraavat vaiheet

- Näytä [uusin tallennustilan Explorer (ennakkoversio) Vapauta muistiinpanot ja videoita](http://www.storageexplorer.com).
- Lisätietoja [sovellusten Azure BLOB-objektit, taulukoita, olevien, ja tiedostojen](https://azure.microsoft.com/documentation/services/storage/)luomisesta.

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png