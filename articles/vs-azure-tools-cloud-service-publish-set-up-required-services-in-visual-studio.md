<properties
   pageTitle="Valmistele julkaiseminen tai Visual Studio Azure sovelluksen ottaminen käyttöön | Microsoft Azure"
   description="Lue ohjeet cloud ja tallennustilaa tili-palvelujen määrittämiseen ja Azure-sovelluksen kokoonpanon määrittäminen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="prepare-to-publish-or-deploy-an-azure-application-from-visual-studio"></a>Valmistele julkaiseminen tai Visual Studio Azure sovelluksen ottaminen käyttöön

## <a name="overview"></a>Yleiskatsaus

Ennen kuin voit julkaista cloud palvelun projektin, sinun on määritettävä seuraavat palvelut:

- Suorita oman roolit Azure-ympäristössä **pilvipalvelussa**

- On- **tallennustilan tilin** , joka sisältää Blob, jonossa ja taulukon palveluja.

Ohjeiden avulla palveluista ja sovelluksen määrittäminen


## <a name="create-a-cloud-service"></a>Luo pilvipalveluun

Julkaiseminen Azure pilvipalveluun, sinun on luotava pilvipalveluun, joka suoritetaan oman roolit Azure-ympäristössä. Voit luoda pilvipalveluun [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)tämän artikkelin **luominen pilvipalveluun käyttämällä Azure perinteinen portal**-kohdassa kuvatulla tavalla. Voit luoda pilvipalveluun Visual Studiossa ohjatun julkaisutoiminnon avulla.

### <a name="to-create-a-cloud-service-by-using-visual-studio"></a>Voit luoda pilvipalveluun käyttämällä Visual Studio

1. Avaa pikavalikko Azure projektin ja sitten **Julkaise**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)

1. Jos et ole kirjautunut sisään, kirjaudu sisään käyttäjätunnuksella ja salasanalla Microsoft-tili tai organisaation tiliä, joka liittyy Azure tilauksen.

1. Valitse Siirry **asetukset** -sivulla **Seuraava** -painiketta.

    ![Ohjatun julkaisutoiminnon Yleiset asetukset](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)

1. Valitse **Luo uusi** **Cloud Services** -luettelosta. **Luo Azure-palvelut** -valintaikkuna tulee näyttöön.

1. Kirjoita cloud-palvelun nimi. Palvelun URL-osoite osa ja näin ollen on oltava yksilöivä nimi. Nimi ei ole merkitystä.

### <a name="to-create-a-cloud-service-by-using-the-azure-classic-portal"></a>Voit luoda pilvipalveluun Azure perinteinen-portaalissa

1. Kirjaudu Microsoft-sivuston [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (valinnainen) Cloud Services-palvelut, jotka olet jo luonut luettelo näkyviin valitsemalla sivun vasemmassa reunassa Cloud Services-linkki.

1. Valitse **+** kuvaketta vasemmassa alakulmassa kulman ja valitse sitten **Pilvipalvelussa** valikossa, joka tulee näkyviin. Toinen näyttö on kaksi vaihtoehtoa: **Nopea luominen** ja **Mukautettu luominen**, tulee näkyviin. Jos valitset **Nopea luominen**, voit luoda pilvipalveluun määrittämällä sen URL-osoite ja alue, johon se fyysisesti isännöityjen. Jos valitset **Mukautettu luominen**, voit julkaista pilvipalveluun heti määrittämällä pakkaus (.cspkg-tiedosto), kokoonpanotiedosto (.cscfg) ja varmennetta. Luo mukautettu ei tarvita, jos haluat julkaista pilvipalvelussa sijaitsevaan Azure Projectissa **Julkaise** -komennolla. **Julkaise** -komento on käytettävissä Azure projektin pikavalikon.

1. Valitse Julkaise myöhemmin Visual Studiossa pilvipalvelussa sijaitsevaan **Nopea luominen** .

1. Määritä pilvipalvelussa sijaitsevaan nimi. Täydellinen URL-osoite näkyy nimen vieressä.

1. Valitse luettelosta alue, jossa Useimmat käyttäjät sijaitsevat.

1. Ikkunan alareunassa Valitse **Luominen pilvipalvelussa** -linkki.

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

Tallennustilan tilin pääsee Blob, jonossa ja taulukon palvelut. Voit luoda tallennustilan tilin käyttämällä Visual Studio tai [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="to-create-a-storage-account-by-using-visual-studio"></a>Tallennustilan tilin luominen käyttämällä Visual Studio

1. **Ratkaisunhallinnassa**Avaa pikavalikon **tallennustilan** -solmu ja valitse sitten **Luo tallennustilan tili**.

    ![Luo uusi tallennustilan Azure-tili](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)

1. Valitse tai anna seuraavat tiedot tallennustilan uuden tilin **Luominen tallennustilan tili** -valintaikkunassa.
    - Azure tilaus, johon haluat lisätä tallennustilaa-tili.
    - Haluat käyttää uuden tallennustilan tilin nimi.
    - Alueen tai affiniteetti ryhmän (esimerkiksi Länsi US tai Itä-Aasian).
    - Replikoinnin tyyppi, jota haluat käyttää tallennustilan tilin, kuten Geo tarpeettomat.

1. Kun olet valmis, valitse **Luo**. Uusi tallennustilan tili näkyy **Palvelimen**Explorerissa **tallennustilan** -luettelossa.

### <a name="to-create-a-storage-account-by-using-the-azure-classic-portal"></a>Tallennustilan tilin luominen käyttämällä Azure perinteinen portal

1. Kirjaudu Microsoft-sivuston [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkId=253103) .

1. (Valinnainen) Voit tarkastella tallennustilan asiakkaiden valitsemalla sivun vasemmassa reunassa Ohjauspaneelin **tallennustilan** -linkki.

1. Valitse sivun vasemmassa alakulmassa **+** kuvaketta.

1. Valitse näkyviin tulevan valikon **tallennustilan**ja valitse **Nopea luominen**.

1. Anna tallennustilan tilin nimi, joka aiheuttaa yksilöllinen URL-osoite.

1. Anna pilvipalvelussa sijaitsevaan nimi. Täydellinen URL-osoite näkyy nimen vieressä.

1. Valitse luettelon alueista, alueen, jossa Useimmat käyttäjät sijaitsevat.

1. Määrittää, haluatko geo replikoinnin käyttöön. Jos otat geo replikoinnin, tiedot tallennetaan useita toimipisteitä pienenee tappio. Tämä toiminto on suunniteltu tallennustilan kalliimpi, mutta voit pienentää kustannukset ottamalla geo sijainti, kun luot tallennustilan tilin sijaan lisäämisestä ominaisuutta myöhemmin. Lisätietoja on artikkelissa [Geo replikoinnin](http://go.microsoft.com/fwlink/?LinkId=253108).

1. Ikkunan alareunassa valitsemalla **Luo tallennustilan tili** -linkki.

Kun olet luonut tallennustilan-tilisi, näet URL-osoitteet, joiden avulla voit käyttää kaikkien Azure liittyviä palveluja ja ensisijaisen ja toissijaisen pikanäppäimet tilissäsi resursseja. Painettavat näppäimet avulla todentaa pyyntöihin vastaan liittyviä palveluja.

>[AZURE.NOTE] Toissijainen pikanäppäin on samat käyttöoikeudet kuin ensisijaisen pikanäppäin tallennustilan-tiliisi ja luodaan varmuuskopioina olisi ensisijainen pikanäppäin on käsiin. Lisäksi on suositeltavaa luoda access-näppäimiä säännöllisesti uudelleen. Voit muokata yhteyden merkkijono-asetus, joka käyttää toissijaisen avaimen silloin, kun Luo perusavaimen, ja voit muokata sitä käyttäminen regeneroitu perusavaimen toissijaisen avaimen uudelleen.

## <a name="configure-your-app-to-use-services-provided-by-the-storage-account"></a>Kun sovellus käyttää palveluja tallennustilan tilin määrittäminen

Sinun on määritettävä rooli, joka käyttää käyttää Azure tallennustilan palveluja, jotka olet luonut liittyviä palveluja. Toiminto, voit käyttää useita palvelun määrityksiä Azure projektin. Oletusarvon mukaan kaksi luodaan Azure projektin. Käyttämällä useita palvelun määrityksiä käyttää samaa yhteysmerkkijonoa koodissa mutta yhteysmerkkijonon eri arvo on kunkin palvelun määritykset. Esimerkiksi voit yksi palvelun määritys ja virheenkorjaus paikallisesti käyttämällä Azure tallennustilan emulaattori sovelluksen ja eri määritykset, voit julkaista Azure sovellusta. Saat lisätietoja palvelun määrityksiä [Määrittäminen oma Azure projektin käyttämällä useita palvelun määrityksiä](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="to-configure-your-application-to-use-services-that-the-storage-account-provides"></a>Voit määrittää sovelluksesi voi käyttää tallennustilan tilin tarjoamat palvelut

1. Avaa Visual Studio Azure ratkaisu. Napsauta ratkaisunhallinnassa Avaa pikavalikko kunkin roolin Azure projektin, joka käyttää liittyviä palveluja ja valitse **Ominaisuudet**. Roolin nimen sisältävä sivu näkyy Visual Studio-editorissa. Sivun näyttää kentät **määritys** -välilehti.

1. Valitse rooli ominaisuusikkunat- **asetukset**.

1. Valitse **Palvelun Configuration** -luettelosta nimi, jota haluat muokata palvelun määritykset. Jos haluat tehdä muutoksia kaikkiin tämän roolin service-määrityksiä, voit valita **Kaikki määritykset**.  Lisätietoja palvelun määrityksiä päivittämisestä on kohdassa **Hallinta yhteyden merkkijonot tallennustilan tilien** artikkelissa [Azure-pilvipalvelussa Visual Studiossa roolien määrittäminen](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Jos haluat muokata mitä tahansa merkkijonoa yhteysasetukset, valitse **...** asetus-painiketta. **Luo tallennustilan yhteysmerkkijono** -valintaikkuna.

1. Valitse **Yhdistä käyttäen**, **tilaus** -vaihtoehto.

1. Valitse **tilaus** -luettelossa tilaus. Jos tilausluettelosta ei sisällä haluamasi vaihtoehto, valitse **Lataa Julkaisuasetukset** -linkki.

1. Valitse **tilin nimi** -luettelosta tallennustilan tilin nimi. Azure Työkalut hakee tallennustilan tilin tunnistetiedot automaattisesti .publishsettings-tiedoston avulla. Voit määrittää tallennustilan tilin tunnistetiedot manuaalisesti, **manuaalisesti tunnistetiedot** -vaihtoehto ja jatka sitten tätä toimintoa. Saat tallennustilan tilin nimi ja perusavain [Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885). Jos et halua määrittää-tallennustilan tilin asetukset manuaalisesti, valitse Sulje valintaikkuna **OK** -painiketta.

1. Valitse **Enter tallennustilan tilin** tunnistetiedot-linkki.

1. Kirjoita **tilin nimi** -ruutuun tallennustilan tilin nimi.

    >[AZURE.NOTE] Kirjaudu sisään [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885)ja valitse sitten **tallennustilaa** -painike. Portaalin näkyy tallennustilan tilien luettelo. Jos valitset tilin, se-sivu avautuu. Voit kopioida tallennustilan tilin nimi tältä sivulta. Jos käytät perinteistä portaalin aiemman version, tallennustilan tilin nimi näkyy **Tallennustilan tilit** -näkymä. Kopioi tämä nimi, korosta tämän näkymän **Ominaisuudet** -ikkunassa ja valitse sitten Ctrl + C-näppäimiä. Liitä nimi Visual Studiossa, valitse **tilin nimi** -tekstiruutuun ja valitse sitten Ctrl + V-näppäimiä.

1. **Tiliavain tiliavain** -ruutuun perusavaimeksi, kirjoita tai kopioi ja liitä se [Azure perinteinen portal](http://go.microsoft.com/fwlink/?LinkID=213885).
    Jos haluat kopioida avaimeen:

    1. Sopivan tallennustilan tilin sivun alareunassa voit **Hallita näppäimet** -painiketta.

    1. **Näppäimet käytön hallinta** -sivulla Valitse ensisijainen pikanäppäin teksti ja valitse sitten Ctrl + C-näppäimiä.

    1. Azure-työkalujen Liitä **Tiliavain tiliavain** -kenttään avain.

    1. Sinun on valittava jokin seuraavista vaihtoehdoista, voit selvittää, kuinka palvelu käyttää tallennustilan-tili:
        - **Käytä HTTP**. Tämä on vakio. Esimerkiksi `http://<account name>.blob.core.windows.net`.
        - **Käytä HTTPS** suojattua yhteyttä varten. Esimerkiksi `https://<accountname>.blob.core.windows.net`.
        - **Määritä mukautettu päätepisteet** kunkin kolme palvelua. Voit kirjoittaa nämä päätepisteet Valitse palvelu-kenttään.

        >[AZURE.NOTE] Jos luot mukautetun päätepisteet, voit luoda monimutkaisia yhteysmerkkijonon. Kun tämä muoto, voit määrittää tallennustilan Palvelupäätepisteet, jotka sisältävät mukautettua toimialuenimeä, joka on rekisteröity tallennustilan tilin Blob-palvelussa. Voit myös antaa vain yhden säilön kautta jaettua käyttöä allekirjoituksen blob resurssien käyttöoikeuksia. Lisätietoja mukautettujen päätepisteiden luomisesta on artikkelissa [Määrittäminen Azure tallennustilan yhteyden merkkijonoja](storage-configure-connection-string.md).

1. Tallenna muutokset yhteyden merkkijono, valitse **OK** -painiketta ja valitse sitten työkalurivillä **Tallenna** -painiketta. Kun olet tallentanut muutokset, voit avata tämän yhteysmerkkijonon arvo koodissa [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx)avulla. Kun julkaiset Azure sovellusta, valitse palvelun määritykset, joka sisältää yhteysmerkkijonoa Azure-tallennustilan tilin. Kun sovellus on julkaistu, varmista, että sovelluksen toimii oikein vastaan Azure liittyviä palveluja

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja sovellusten julkaiseminen Azure Visual Studio artikkelissa [julkaiseminen pilvipalveluun Azure-työkalujen avulla](vs-azure-tools-publishing-a-cloud-service.md).
