<properties 
    pageTitle="Muuntaa WordPress Multisite Azure sovelluksen-palvelussa" 
    description="Lue, miten voit ottaa aiemmin WordPress online, luoda Azure valikoiman avulla ja muunna se WordPress Multisite" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Muuntaa WordPress Multisite Azure sovelluksen-palvelussa

## <a name="overview"></a>Yleiskatsaus

*[Ben Lobaugh]mukaan[ben-lobaugh], [Microsoft Open Technologies Inc.][ms-open-tech]*

Tässä opetusohjelmassa opit ottaa aiemmin WordPress online, luoda Azure valikoiman avulla ja muuntaa sen WordPress Multisite Asenna. Lisäksi opit mukautetun toimialueen määrittäminen kullekin asentamisessa alisivustot.

Oletetaan, että sinulla on aiemmin asennetun WordPress. Jos et noudata siinä annettuja ohjeita [Azure valikoimasta WordPress web-sivuston]luominen[website-from-gallery].

Muuntaminen aiemmin WordPress yksittäisen sivuston Asenna Multisite on yleensä melko helppoa ja monia alkuperäinen vaiheista tulla suoraan [Luo A-verkosto] [ wordpress-codex-create-a-network] [WordPress Codex](http://codex.wordpress.org)-sivulla.

Aloitetaanpa.

## <a name="allow-multisite"></a>Salli Multisite

Sinun on ensin otettava käyttöön Multisite kautta `wp-config.php` tiedostoa, jonka **WP\_Salli\_MULTISITE** vakio. Voit muokata web app-tiedostoja kahdella tavalla: ensimmäinen on FTP ja toinen Git kautta. Jos olet perehtynyt määrittäminen kahdella eri tavalla, lue seuraavat opetusohjelmiin:

* [Web-sivuston PHP MySQL- ja FTP][website-w-mysql-and-ftp-ftp-setup]

* [Web-sivuston PHP MySQL ja Git][website-w-mysql-and-git-git-setup]

Avaa `wp-config.php` sähköpostikansioon editorissa tiedosto ja lisää yllä seuraava `/* That's all, stop editing! Happy blogging. */` rivi.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Varmista, Tallenna tiedosto ja lataa palvelimeen.

## <a name="network-setup"></a>Verkon määrittäminen

Kirjaudu sivustosi *wp järjestelmänvalvoja* -alueeseen ja sovelluksen pitäisi näkyä nimeltä **Verkon määritys** **Työkalut** -valikossa uusi kohde. **Verkon** asetukset ja anna tarvittavat tiedot verkkoon.

![Verkon asennusnäyttö][wordpress-network-setup]

Tässä opetusohjelmassa käyttää *alikansioiden* sivuston rakennetta, koska se on aina toimi ja olemme olla määrittämään mukautettujen toimialueiden kunkin alisivuston myöhemmin opetusohjelman. Kuitenkin pitäisi olla mahdollista määritys alitoimialue asentaa, jos toimialueen DNS yleismerkkien [Azure-portaalin](https://portal.azure.com) ja määritys – Yhdistä oikein.

Lisätietoja aliraportti toimialueen ja alikansio asetuksista on artikkelissa [multisite verkon tyypit] [ wordpress-codex-types-of-networks] WordPress Codex artikkeli.

## <a name="enable-the-network"></a>Verkon ottaminen käyttöön

Verkko on määritetty tietokannan, mutta ei yhtä vaihetta käyttöön verkon toimintoja. Viimeistele `wp-config.php` asetukset ja varmista `web.config` reitittää oikein sivustosta.


Kun olet napsauttanut *Verkon määritys* -sivulla **Asenna** -painike, WordPress yrittää päivittää `wp-config.php` ja `web.config` tiedostot. Voit tulee aina tarkistaa tiedostot varmistamiseksi päivityksiä ei onnistunut. Jos et, tämä näyttö esittää tarvittavat päivitykset. Muokata ja tallentaa tiedostoja.


Nämä päivitykset, sinun on kirjauduttava ja kirjaudu takaisin wp järjestelmänvalvojan koontinäytön tekeminen jälkeen.

Nyt on muita valikon palkin nimetty **Omien sivustojen**hallinta. Tässä valikossa voit määrittää uuden verkon kautta **Verkon järjestelmänvalvojien** koontinäyttöön.

## <a name="adding-custom-domains"></a>Mukautettujen toimialueiden lisääminen

[WordPress My Domain yhdistäminen] [ wordpress-plugin-wordpress-mu-domain-mapping] laajennuksen helpottaa postitusosoitteet mukautettujen toimialueiden lisääminen sivustolle verkossa. Jotta laajennus toimii oikein sinun on suoritettava joitain lisäasetuksia portaalissa ja myös toimialueen rekisteröintipalvelussa.

## <a name="enable-domain-mapping-to-the-web-app"></a>Ota käyttöön toimialueen määritys web App-sovellukseen

**Vapaa** [App palvelun](http://go.microsoft.com/fwlink/?LinkId=529714) suunnitelman tila ei tue mukautettujen toimialueiden lisääminen verkkosovelluksissa. Sinun on vaihdettava **jaettu** tai **Vakio** -tilassa. Toiminto:

* Kirjaudu sisään Azure-portaaliin ja Etsi web-sovellus. 
* Valitse **asetusten** **Skaalaa** -välilehdessä.
* Valitse **Yleiset**-kohdassa *JAETTU* tai *VAKIO*
* Valitse **Tallenna**

Voit saada viestin, sinulta kysytään, haluatko tarkistaa muutokset ja kuittaa koodiin nyt voi syntyä kustannus-käyttö ja määritykset, voit määrittää mukaan.

Kestää joitakin sekunteja käsittelemään uudet asetukset on nyt ajoissa Aloita toimialueen määrittäminen.

## <a name="verify-your-domain"></a>Toimialueen vahvistaminen

Ennen kuin Azure Web Apps-sovellusten avulla voit yhdistää sivuston toimialue, sinun on varmistaa, että sinulla on lupa yhdistämään toimialueen. Voit tehdä DNS-merkintä on lisättävä uusi CNAME-tietue.

* Kirjaudu sisään toimialueen DNS-hallintaan
* Luo uusi CNAME- *awverify*
* Osoita *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

Voi kestää jonkin aikaa DNS-muutokset tulevat voimaan koko, joten jos seuraavat vaiheet eivät toimi heti, valitse cup, esimerkiksi, ja valitse toinen käyttäjä palaa lomaltaan ja yritä uudelleen.

## <a name="add-the-domain-to-the-web-app"></a>Toimialueen lisääminen web Appiin

Palaa koodiin palvelun Azure-portaalissa, valitse **asetukset**ja valitse sitten **Mukautetut toimialueet ja SSL**.

*SSL-asetukset* ovat näkyvissä, näet kentät, johon syötät kaikki toimialueet, johon haluat liittää web App-sovellukseen. Jos toimialuetta ei ole mainittu tässä, se eivät ole käytettävissä riippumatta siitä, miten DNS-toimialue on määritetty sisällä WordPress-määritystä varten.

![Hallinta-valintaikkunan mukautetut toimialueet][wordpress-manage-domains]

Kun olet kirjoittanut toimialueen tekstiruutuun, Azure tarkistaa aiemmin luotu CNAME-tietue. Jos DNS on ei täysin välitetty, punainen ilmaisin näkyy. Jos se on onnistuu, näet vihreä valintamerkki. 

Ota seuraavat seikat huomioon luettelossa valintaikkunan alareunassa IP-osoite. Tarvitset tätä toimialueesi A-tietue: n määrittäminen.

## <a name="setup-the-domain-a-record"></a>Määritä toimialueen tietue

Jos muita vaiheita ei onnistunut, voi nyt määrittää toimialueen Azure web App-sovelluksen kautta DNS-A-tietueen. 

On tärkeää muistaa tähän Azure verkkosovelluksissa Hyväksy sekä CNAME-tietueet-, mutta *on* käytettävä A-tietue käyttöön oikean toimialueen määritys. CNAME ei voi siirtää toisen CNAME-TIETUE, eli voit luoda Azure YOUR_DOMAIN.azurewebsites.net kanssa.

Käytä edellisessä vaiheessa IP-osoite, palaa DNS-hallintaan ja määritä A-tietue osoittamaan, että IP.


## <a name="install-and-setup-the-plugin"></a>Asennus ja määritys-laajennus

WordPress Multisite tällä hetkellä ole valmiita tapa yhdistää mukautetut toimialueet. On kuitenkin kutsutaan [WordPress My Domain yhdistäminen] laajennuksen[ wordpress-plugin-wordpress-mu-domain-mapping] , joka lisää toimintoja. Kirjaudu sisään sivustoosi verkon järjestelmänvalvoja-osaan ja asenna laajennus **WordPress My Domain yhdistäminen** .

Kun asentaminen ja aktivoiminen laajennus, käy **asetukset** > määrittäminen laajennus**Toimialueen yhdistämismääritys** . Ensimmäisen tekstiruudun *IP-osoite*, kirjoittanut käyttämäsi toimialueen A-tietueen määrittäminen IP-osoite. Määritä tarvittavat *Asetukset toimialueen* Microsoftiin (oletusarvot ovat usein hieno) ja valitse **Tallenna**.

## <a name="map-the-domain"></a>Määritä toimialue

Käy haluat yhdistää toimialueen niin, että sivuston **raporttinäkymät-ikkunan** . Valitse **Työkalut** > **Toimialueen yhdistäminen** ja kirjoita uusi toimialue tekstiruutu ja sitten **Lisää**.

Oletusarvon mukaan uusi toimialue kirjoitettava automaattisesti sivuston toimialueeseen. Jos haluat lisätä kaikki liikenne lähettää uusi toimialue, valitse ennen tallentamista *Ensisijainen toimialue Tämä blogi* -valintaruutu. Voit lisätä sivuston toimialueiden rajattomasti, mutta vain yksi voi olla ensisijainen.

## <a name="do-it-again"></a>Tee uudelleen

Azure Web Apps-sovellusten avulla voit lisätä rajattomasti toimialueiden web App-sovellukseen. Toisen toimialueen lisäämistä sinun on on suorittaa kunkin toimialueen **toimialuenimen** ja **toimialueen tietueen määritys** osat.  

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
