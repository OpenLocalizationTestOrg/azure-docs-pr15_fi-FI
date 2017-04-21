# <a name="using-cdn-for-azure"></a>Azure CDN käyttäminen

Azure sisällön toimituksen verkon (CDN) on kehittäjät yleisen ratkaisun välittää suuren kaistanleveyden sisällön tallentamalla välimuistiin BLOB-objektit ja Laske tapauksissa fyysinen solmujen Yhdysvalloissa, Eurooppa, Aasian, Australian ja Etelä-Amerikka staattiseksi sisällöksi. CDN solmu sijainneista nykyiseen luetteloon Tutustu [Azure CDN solmu sijainnit].

Tämän tehtävän sisältää seuraavat vaiheet:

* [Vaihe 1: Tallennustilan tilin luominen](#Step1)
* [Vaihe 2: Luo uusi CDN päätepiste tallennustilan tilin](#Step2)
* [Vaihe 3: CDN-sisällön käyttäminen](#Step3)
* [Vaihe 4: CDN-sisällön poistaminen](#Step4)

Eduista CDN välimuistiin Azure tiedot ovat:

-   Parantaa suorituskykyä ja käyttäjän kohdata käyttäjille, jotka ovat huomattavasti sisältölähteen, ja käyttävät sovellukset missä monet internet trips tarvitaan Lataa sisältö
-   Suuren hajautettu mittakaavan käsittelemään paremmin välittömästi suuren kuormituksen aikana sano tapahtuman, kuten tuotteen julkistaminen alkuun

CDN asiakkaista nyt käyttää Azure CDN [Azure perinteinen portal]. CDN on lisäosa ominaisuus tilaukseen, ja se on erillinen [Laskutus palvelupaketin].

<a id="Step1"> </a>
<h2>Vaihe 1: Tallennustilan tilin luominen</h2>

Seuraavien ohjeiden avulla voit luoda uuden Azure tilauksen tallennustilan tilin. Tallennustilan tilin antaa käyttöoikeuden Azure liittyviä palveluja. Tallennustilan tilin edustaa parhaan mahdollisen nimitilaa käyttäminen Azure tallennustilan palvelun osat: Blob-palvelujen ja jonon palvelujen Table services. Saat lisätietoja tallennustilan Azure-palveluista [Azure-tallennustilan Services-palvelujen avulla](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Tallennustilan tilin luominen on oltava joko palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan liittyvät tilaukseen.

> [AZURE.NOTE] Lisätietoja tätä toimintoa käyttämällä Azure Service Management-Ohjelmointirajapinnan on ohjeaiheessa- [Tallennustilan tilin luominen](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) viittaus.

**Azure-tilauksen tallennustilan tilin luominen**

1.  Kirjaudu sisään [Azure perinteinen portal].
2.  Valitse vasemmassa alakulmassa **Uusi**. Valitse **Uusi** -valintaikkunan **Tietopalvelut**ja valitse **tallennustilan** **Nopea luominen**.

    **Luo tallennustilan tili** -valintaikkuna tulee näyttöön.

    ![Tallennustilan tilin luominen][create-new-storage-account]

4. Kirjoita **URL-osoite** -kenttään alitoimialueen nimen. Tämän arvon voi olla 3 – 24 pieniä kirjaimia ja numeroita.

    Tämä arvo muuttuu isäntänimi sisällä URI, jota käytetään osoite Blob, jonossa tai taulukon resurssit-tilausta. Osoitteen säilö resurssin Blob-palvelussa, milloin käyttää URI-Osoitteen muodossa, jossa * &lt;StorageAccountLabel&gt; * **Enter URL-Osoitteen**kirjoittamasi arvo viittaa:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Tärkeä:** URL-osoite-otsikko-tallennustilan tilin URI alitoimialueen lomakkeiden ja on oltava yksilöllinen kesken isännöityä palvelun Azure-tietokannassa.

    Tätä arvoa käytetään myös portaalissa tai tallennustilan tilin nimi käytettäessä ohjelmallisesti tähän tiliin.

5.  **Alue/affiniteetti ryhmän** avattavasta luettelosta Valitse alueen tai affiniteetti ryhmän tallennustila-tilille. Valitse affiniteetti-ryhmän alueen sijaan, jos haluat, että Windows Azure palvelut, joita käytät samaa tietokeskuksen olevan tallennustilan palvelujen. Tämä voi parantaa suorituskykyä ja ei veloituksia ovat kertyneet juniin.  

    **Huomautus:** Voit luoda affiniteetti ryhmän Avaa hallinta-portaalin **asetukset** -alue, **affiniteetti**ryhmät ja valitse sitten **Lisää affiniteetti-ryhmän** tai **Lisää**. Voit luoda ja hallita affiniteetti Windows Azure Service Management Ohjelmointirajapinnan käyttäminen. Lisätietoja on artikkelissa [toimenpiteet affiniteetti ryhmät].

6. **Tilauksen** avattavasta luettelosta Valitse tilaus, jota käytetään tallennustilan-tilin kanssa.
7.  Valitse **Luo tallennustilan tilin**. Tallennustilan tilin luominen voi kestää useita minuutteja.
8.  Voit tarkistaa tallennustilan-tili on luotu, varmista, että tili näkyy luettelossa tilassa **Online** **Storage** kohteet.

<a id="Step2"> </a>
<h2>Vaihe 2: Luo uusi CDN päätepiste tallennustilan tilin</h2>

Kun salli tallennustilan tilin käyttö CDN tai isännöidään palvelu, yleisesti saatavilla olevien objektien soveltuvat CDN reunan välimuistiin. Jos muokkaat objekti, joka on tällä hetkellä välimuistiin CDN, uusi sisältö eivät kautta CDN, kunnes CDN päivittää sen sisältöä, kun välimuistin sisältö--elinaika on kulunut.

**Luo uusi CDN päätepiste tallennustilan tilin**

1. [Azure perinteinen portal]-siirtymisruudun, valitse **CDN**.

2. Valitse valintanauhan valitsemalla **Uusi**. Valitse **Uusi** -valintaikkunan **App Services**, sitten **CDN**sitten **Nopea luominen**.

3. Valitse **Origin toimialue** avattavasta valikosta tallennustilan-tili, jonka loit edellisessä osassa luettelosta tallennustilaa tunnuksilla. 

4. Luo uusi päätepiste valitsemalla **Luo** .

5. Kun päätepiste on luotu, se näkyy päätepisteet tilauksen luettelo. Luettelonäkymä näyttää käyttämään välimuistissa olevaa sisältöä sekä origin toimialueen URL-osoite. 

    Origin toimialue on sijaintiin, josta CDN välimuistiin sisältöä. Origin toimialueen voi olla tallennustilan tilin tai pilvipalveluun; tarkoitetaan tässä esimerkissä käytetään tallennustilan tilin. Tallennustilan sisällön välimuistiin reunapalvelimet mukaan joko välimuisti-komponenttien asetuksia, jotka voit määrittää tai välimuistiin verkoston oletusarvo-heuristiikkaa. 


    > [AZURE.NOTE] Luotu päätepisteen määritystä ei heti ole käytettävissä; voi kestää jopa 60 minuuttia rekisteröinnin leviäminen CDN verkon kautta. Käyttäjä yrittää käyttää CDN toimialuenimen heti saattaa tulla tilakoodin 400 (virheelliset pyynnön), ennen kuin sisältö on saatavilla CDN kautta.

<a id="Step3"> </a>
<h2>Vaihe 3: Access CDN sisältö</h2> 

Välimuistin sisältöä käyttöön CDN käyttää annettu portaalissa CDN URL-osoite. Välimuistin blob-osoite on seuraavanlainen:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Vaihe 4: Poistaa sisältöä CDN</h2>

Jos et halua enää välimuistiin objektin-Azure sisällön toimituksen verkon (CDN), voit tehdä seuraavat toimet:

-   Azuren Blob-objektien voit poistaa blob julkisen säilöstä.
-   Voit tehdä säilöä sen sijaan, että julkinen yksityinen. Lisätietoja on kohdassa [Säilöjä ja BLOB rajoita käyttöoikeus](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Voit poistaa käytöstä tai poistaa CDN päätepisteen hallinta-portaalissa.
-   Voit muokata isännöityä palvelun vastaamaan enää pyyntöihin objektin.

Objektin välimuistiin jo CDN säilyy välimuistissa, kunnes, aika-to-live objektille on kulunut. Kun aika-to-live on kulunut, CDN Tarkista onko CDN päätepisteen on edelleen voimassa ja nimettömänä käytettävissä objekti. Jos osoite ei ole enää objektin tallennetaan.

Voit poistaa välittömästi sisältöä on tällä hetkellä ei tueta Azure hallinta-portaalin. Ota yhteyttä [Azure tukevat](https://azure.microsoft.com/support/options/) Jos haluat tyhjentää heti sisältöä. 

## <a name="additional-resources"></a>Lisäresursseja

-   [Azure affiniteetti-ryhmän luominen]
-   [Toimintaohje: tallennustilan tilien hallinta Azure-tilauksen]
-   [Palvelun hallinnasta Ohjelmointirajapinta]
-   [Voit yhdistää CDN sisältöä mukautetulla toimialueella]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN solmu sijainnit]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure perinteinen portal]: https://manage.windowsazure.com/
  [laskutuksen suunnitelma]: /pricing/calculator/?scenario=full
  [Azure affiniteetti-ryhmän luominen]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Palvelun hallinnasta Ohjelmointirajapinta]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Voit yhdistää CDN sisältöä mukautetulla toimialueella]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
