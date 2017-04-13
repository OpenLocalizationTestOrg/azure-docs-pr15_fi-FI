<properties
    pageTitle="Integroi tallennustilan tilin CDN | Microsoft Azure"
    description="Opettele käyttämään Azure sisällön toimituksen verkon (CDN) aikana suuren kaistanleveyden sisällön tallentamalla välimuistiin BLOB-Azure-tallennustilan."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Integroi tallennustilan tilin CDN

CDN voidaan ottaa käyttöön välimuistin sisällön Azure-tallennustilan. Siinä on kehittäjät yleisen ratkaisun välittää suuren kaistanleveyden sisällön tallentamalla välimuistiin BLOB-objektit ja Laske tapauksissa fyysinen solmujen Yhdysvalloissa, Eurooppa, Aasian, Australian ja Etelä-Amerikka staattiseksi sisällöksi.


## <a name="step-1-create-a-storage-account"></a>Vaihe 1: Tallennustilan tilin luominen

Seuraavien ohjeiden avulla voit luoda uuden Azure tilauksen tallennustilan tilin. Tallennustilan tilin antaa käyttöoikeuden Azure liittyviä palveluja. Tallennustilan tilin edustaa parhaan mahdollisen nimitilaa käyttäminen Azure tallennustilan palvelun osat: Blob-palvelujen ja jonon palvelujen Table services. Lisätietoja on [Microsoft Azuren tallennustilaan johdanto](../storage/storage-introduction.md)viittaavat.

Tallennustilan tilin luominen on oltava joko palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan liittyvät tilaukseen.

> [AZURE.NOTE] Voit luoda tallennustilan tilin, mukaan lukien Azure-portaalin ja Powershell eri tavoilla.  Tässä opetusohjelmassa, että voit käyttää Azure-portaalissa.  

**Azure-tilauksen tallennustilan tilin luominen**

1.  Kirjautuminen [Azure Portal](https://portal.azure.com).
2.  Valitse vasemmassa yläkulmassa **Uusi**. Valitse **Uusi** -valintaikkunan Valitse **tietojen + tallennustilan**ja valitse sitten **tallennustilan tilin**.

    **Luo tallennustilan tili** -sivu tulee näkyviin.

    ![Tallennustilan tilin luominen][create-new-storage-account]

4. Kirjoita alitoimialueen nimi **nimi** -kenttään. Tämän arvon voi olla 3 – 24 pieniä kirjaimia ja numeroita.

    Tämä arvo muuttuu isäntänimi sisällä URI, jota käytetään osoite Blob, jonossa tai taulukon resurssit-tilausta. Osoitteen säilö resurssin Blob-palvelussa, milloin käyttää URI-Osoitteen muodossa, jossa * &lt;StorageAccountLabel&gt; * **Enter URL-Osoitteen**kirjoittamasi arvo viittaa:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Tärkeä:** URL-osoite-otsikko-tallennustilan tilin URI alitoimialueen lomakkeet ja on oltava yksilöllinen kesken isännöityä palvelun Azure-tietokannassa.

    Tätä arvoa käytetään myös portaalissa tai tallennustilan tilin nimi käytettäessä ohjelmallisesti tähän tiliin.

5. Jätä oletusarvot **käyttöönoton mallin**, **tilin tyyppi**, **suorituskykyä**ja **replikoinnin**. 

6. Valitse **tilaus** , jota käytetään tallennustilan-tilin kanssa.

7. Valitse tai luo **Resurssiryhmä**.  Lisätietoja resurssiryhmät on artikkelissa [Azure Resurssienhallinta yleiskatsaus](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Valitse tallennustilan tilin sijainti.

8. Valitse **Luo**. Tallennustilan tilin luominen voi kestää useita minuutteja.


## <a name="step-2-create-a-new-cdn-profile"></a>Vaihe 2: Luo uusi CDN profiili

CDN profiili on kokoelma CDN päätepisteet.  Kunkin profiilin sisältää vähintään yhden CDN päätepisteet.  Haluat ehkä käyttää profiileja järjestämiseen CDN-päätepisteet internet-toimialueen, web-sovelluksen tai muun kriteerin perusteella.

> [AZURE.TIP] Jos sinulla on jo CDN-profiili, jota haluat käyttää tässä opetusohjelmassa, siirry [vaiheeseen 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Vaihe 3: Luo uusi CDN päätepiste

**Luo uusi CDN päätepiste tallennustilan tilin**

1. Siirry [Azure hallinta-portaalin](https://portal.azure.com)CDN profiilin.  Voit ehkä on kiinnitetty sitä Raporttinäkymät-ikkunan edellisessä vaiheessa.  Jos sinulla ei ole, löydät sen valitsemalla **Selaa**ja valitse **CDN-profiileista**ja valitsemalla haluat lisätä oman päätepiste profiilissa.

    CDN profiili-sivu tulee näkyviin.

    ![CDN profiili][cdn-profile-settings]

2. Napsauta **Lisää päätepisteen** -painiketta.

    ![Lisää päätepisteen-painike][cdn-new-endpoint-button]

    **Lisää päätepisteen** -sivu tulee näkyviin.

    ![Lisää päätepisteen sivu][cdn-add-endpoint]

3. Anna tämän CDN päätepisteen **nimi** .  Tätä nimeä käytetään käyttää välimuistissa resurssien etsiminen toimialueen `<endpointname>.azureedge.net`.

4. Valitse *tallennustilan* **Origin tyyppi** avattavasta valikosta.  

5. Valitse tallennustilan tilin **Origin hostname** avattavasta valikosta.

6. Jätä oletusarvot **Origin polku**, **Origin toimialuenimi**ja **protokolla/Origin portin**.  Sinun on määritettävä vähintään yksi protokolla (HTTP tai HTTPS).

    > [AZURE.NOTE] Tässä määrityksessä mahdollistaa kaikkien oman julkisesti näkyvissä säilöjä välimuistiin tallentamisen CDN tallennustilan tiliisi.  Jos haluat rajoittaa yksittäisen säilöön, käytä **Origin polku**.  Huomaa säilö on oltava näkyvyys määrittäminen, julkinen.

7. Luo uusi päätepiste valitsemalla **Lisää** .

8. Kun päätepiste on luotu, se näkyy päätepisteet profiilin luettelo. Luettelonäkymä näyttää käyttämään välimuistissa olevaa sisältöä sekä origin toimialueen URL-osoite.

    ![CDN päätepiste][cdn-endpoint-success]

    > [AZURE.NOTE] Päätepisteen heti eivät voi käyttää.  Voi kestää jopa 90 minuuttia rekisteröinnin leviäminen CDN verkon kautta. Käyttäjä yrittää käyttää CDN toimialuenimen heti saattaa tulla tilakoodin 404, kunnes sisältö on käytettävissä CDN kautta.


## <a name="step-4-access-cdn-content"></a>Vaihe 4: Access CDN sisältö

Välimuistin sisältöä käyttöön CDN käyttää annettu portaalissa CDN URL-osoite. Välimuistin blob-osoite on seuraavanlainen:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Kun salli tallennustilan tilin käyttö CDN tai isännöidään palvelu, yleisesti saatavilla olevien objektien soveltuvat CDN reunan välimuistiin. Jos muokkaat objekti, joka on tällä hetkellä välimuistiin CDN, uusi sisältö eivät kautta CDN, kunnes CDN päivittää sen sisältöä, kun välimuistin sisältö--elinaika on kulunut.

## <a name="step-5-remove-content-from-the-cdn"></a>Vaihe 5: Poistaa sisältöä CDN

Jos et halua enää välimuistiin objektin-Azure sisällön toimituksen verkon (CDN), voit tehdä seuraavat toimet:

-   Voit tehdä säilöä sen sijaan, että julkinen yksityinen. Lisätietoja on kohdassa [Manage anonyymi lukuoikeudet säilöjä ja BLOB-objektit](../storage/storage-manage-access-to-resources.md) .
-   Voit poistaa käytöstä tai poistaa CDN päätepisteen hallinta-portaalissa.
-   Voit muokata isännöityä palvelun vastaamaan enää pyyntöihin objektin.

Objektin välimuistiin jo CDN säilyvät välimuistissa, kunnes, aika-to-live objektille on kulunut tai päätepisteen tyhjentää. Kun aika-to-live on kulunut, CDN Tarkista onko CDN päätepisteen on edelleen voimassa ja nimettömänä käytettävissä objekti. Jos osoite ei ole enää objektin tallennetaan.


## <a name="additional-resources"></a>Lisäresursseja

-   [Voit yhdistää CDN sisältöä mukautetulla toimialueella](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
