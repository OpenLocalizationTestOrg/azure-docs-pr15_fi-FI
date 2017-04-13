<properties
  pageTitle="Käytä DevOps ympäristöissä tehokkaasti web Appissa"
  description="Opettele käyttämään käyttöönoton paikkojen voit määrittää ja hallita useita sovelluksen kehittämisen ympäristöissä"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Käytä DevOps ympäristöissä tehokkaasti web Apps-sovellukset

Tämän artikkelin avulla voit määrittää ja hallita web sovelluksen käyttöönottoa varten, kuten väliaikainen kysymysten ja vastausten ja tuotannon kehityksen sovelluksen on useita versioita. Katsotaan tietyn tarpeen käyttöönotto kehitysympäristö sovelluksesi jokaisessa versiossa. Kuten Vastauksiin ympäristön voidaan käyttää ryhmän kehittäjien Testaa sovelluksen laatua, ennen kuin voit siirtää muutokset tuotannon.
Useita kehittäminen ympäristöissä määrittäminen voi olla hankalaa tehtävän, kun haluat seurata, resurssien (suorittaminen, web App-sovelluksen, tietokannan, välimuistin jne.) ja ottaa käyttöön kaikissa ympäristöissä koodia.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Tuotannon ympäristössä (vaihe, keskihajonta, kysymysten ja vastausten) määrittäminen
Kun olet tuotannon verkkosovellukseen tekemiseksi, seuraava vaihe on luominen tuotantoympäristössä. Jotta voit käyttää käyttöönoton paikkojen Varmista, että käytössäsi on **Vakio** - tai **Premium** App palvelun suunnittelu-tilassa. Käyttöönoton paikkojen ovat todella live web Apps-sovellusten kanssa omia isäntänimet. Web app-sisältö- ja osia voi vaihtaa kaksi käyttöönoton paikkaa, mukaan lukien tuotannon paikan välillä. Käyttöönotto sovelluksesi käyttöönoton paikka on seuraavat edut:

1. Voit tarkistaa väliaikaisen käyttöönoton paikka web app muutokset ennen vaihtaminen tuotannon paikka kanssa.
2. Käyttöönoton verkkosovellukseen ensin paikka ja vaihtaminen kyselyjä tuotannon varmistaa, että kaikki esiintymät paikka on lämmitettävä ennen parhaillaan vaihtaa paikkaa kyselyjä tuotannon. Näin käyttökatkot, kun otat käyttöön web Appissa. Liikenne uudelleenohjaus on saumaton ja ei ole pyynnöt olevat kohteet poistetaan, Vaihda toimintojen vuoksi. Koko työnkulku voidaan automatisoida määrittämällä [Automaattisen Vaihda](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) vanhat Vaihda vahvistus ei tarvita, kun.
3. Jälkeen Vaihda aiemmin vaiheistettu web Appilla paikka on nyt edellisen tuotannon web Appissa. Jos vaihtaa paikkaa kyselyjä tuotannon paikka muutokset ovat ei vastaa odotuksiasi, voit tehdä saman Vaihda heti, jos haluat saada "viimeisen tunnetut hyvä koodiin" takaisin.

Määrittää väliaikaisen käyttöönoton paikka, artikkelissa [määrittäminen väliaikaisen ympäristöissä web Apps-sovellukset App Azure-palvelussa](web-sites-staged-publishing.md). Jokaisen ympäristön Sisällytä omassa resurssien määrittäminen, jos web app esimerkissä tietokannan sitten tuotannon ja väliaikaisen web Appin tulisi käyttää eri tietokannat. Lisää väliaikaisen kehittäminen ympäristön resursseja, kuten tietokannan, tallennustilan tai välimuistin määrittämisestä väliaikaisen kehitysympäristö.

## <a name="examples-of-using-multiple-development-environments"></a>Esimerkkejä useita kehitys-Ympäristöt

Projektin pitäisi seurata lähde-koodin hallinta vähintään ympäristöissä, ja -ympäristö, mutta kun käyttämisestä sisällön hallinta-sovelluksen kehysten jne olemme saattaa ilmetä ongelmia jossa sovellus ei tue ruutuun Tämä skenaario. Tämä pätee joistakin Suositut kehysten käsitellyt alapuolella. Runsaasti kysymyksiä tulee voi ladata, kun käsittelet CMS/kehysten esimerkiksi

1. Voit jakaa pois eri ympäristöissä
2. Mitä tiedostoja Voinko muuttaa ja ei vaikuttaa framework version päivitykset
3. Ympäristön kokoonpanon hallinta
4. Moduulit ja laajennukset-version päivitykset, core framework version päivitykset hallinta

Voit määrittää useita ympäristön projektin monella ja seuraavissa esimerkeissä on yksi yhden tällaisen menetelmän vastaaviin sovellukset.

### <a name="wordpress"></a>WordPress
Tässä osassa kerrotaan paikkojen käytön WordPress käyttöönoton työnkulun määrittämisestä. WordPress, kuten useimmat CMS-ratkaisuja ei tue useita kehittäminen ympäristöissä ulos ruutuun käsitteleminen. Web App-palvelun sovellukset on joitakin ominaisuuksia, jotka helpottavat tallentaa määritysasetukset koodisi ulkopuolella.

Ennen kuin luot väliaikaisen paikka, Määritä Sovelluskoodin tukemaan useita ympäristössä. Tukemaan useita ympäristöissä WordPress, sinun on muokattava `wp-config.php` paikallista kehittämistä web-sovellukseen Lisää seuraava koodi tiedoston alussa. Tämä toiminto antaa valita valitun-ympäristöön konfiguroinnin sovelluksen.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Luo kansio nimeltä web app pääkansio-kohdassa `config` ja tiedoston kahden tiedostojen lisääminen: `wp-config.azure.php` ja `wp-config.local.php` edustava azure ja paikallisen ympäristön tarpeen mukaan.

Kopioi seuraavat `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Määrittäminen yllä suojauksen näppäimet voi auttaa parhaillaan hakkeroitu koodiin estää, käytä niin yksilölliset arvot. Jos haluat luoda suojauksen näppäimet edellä mainituista merkkijonon, voit avata automaattinen luontitoiminto voit luoda uusia näppäimet/arvoja tässä [linkin] (https://api.wordpress.org/secret-key/1.1/salt) avulla

Kopioi seuraava koodi- `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Käytä suhteellisia polkuja
Viimeinen kuitenkaan on määritettävä WordPress-sovellus käyttää suhteellisia polkuja. WordPress tallentaa tietokannan URL-tiedot. Näin siirtäminen sisällön ympäristöstä toiseen vaikeaa, kun haluat päivittää tietokannan aina, kun siirrät paikallisen vaiheen tai tuotannon ympäristöt vaiheen. Voit pienentää ongelmat, jotka voivat aiheuttaa käyttöönottoon tietokannan aina, kun otat ympäristöstä toiseen riskiä käyttää [suhteellinen pääkansion linkit laajennuksen](https://wordpress.org/plugins/root-relative-urls/) joka voidaan asentaa WordPress järjestelmänvalvojan koontinäytön tai ladata sen manuaalisesti [täältä](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


Lisää seuraavat määritykset, että `wp-config.php` ennen tiedoston `That's all, stop editing!` kommentti:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivoi laajennuksen kautta `Plugins` WordPress järjestelmänvalvojan raporttinäkymät-valikko. WordPress app pysyvä linkki asetusten tallentaminen.

#### <a name="the-final-wp-configphp-file"></a>Lopullisessa `wp-config.php` tiedosto
WordPress Core-päivityksiä ei vaikuta oman `wp-config.php`, `wp-config.azure.php` ja `wp-config.local.php` tiedostot. Lopuksi tämän miten `wp-config.php` tiedosto näyttää tältä

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Vaiheet-ympäristön määrittäminen
Oletetaan, että sinulla on jo käytössä Azure-Web-kirjautuminen [Azure hallinnan Preview](http://portal.azure.com) -portaaliin WordPress verkkosovellukseen ja WordPress web-sovellus. Jos sovellusten ei voit luoda sen Marketplacesta. Jos haluat lisätietoja, [napsauttamalla tätä](web-sites-php-web-site-gallery.md).
Valitse Asetukset -> käyttöönoton paikkojen -> Lisää luomalla käyttöönoton paikka nimi vaihe. Käyttöönoton paikka on toiseen web-sovelluksen jakamisen kuin ensisijaisen web app-edellä luotu samoja resursseja.

![Luo vaiheen käyttöönoton paikka](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Lisää toinen MySQL-tietokantaan, vaikkapa `wordpress-stage-db` resurssin ryhmään `wordpressapp-group`.

 ![Lisää resurssiryhmä MySQL-tietokantaan](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Päivitä yhteysmerkkijonojen vaiheen käyttöönotto-paikka, valitse juuri luomasi tietokannan `wordpress-stage-db`. Huomaa, että tuotannon web app- `wordpressprodapp` ja väliaikaisen web Appin `wordpressprodapp-stage` on osoitettava eri tietokantoihin.

#### <a name="configure-environment-specific-app-settings"></a>Ympäristön kielikohtaiset sovelluksen asetusten määrittäminen
Kehittäjät voit tallentaa avain-arvo merkkijonon paria Azure kokoonpanotietoja, kun sovelluksen asetukset-verkkosovellus liittyvät osana. Suorituksen palvelun Web sovellukset automaattisesti hakee nämä arvot-ne ovat käytettävissä koodin suorittamisen-sovellukseen. Arvopaperin-perspektiivin, joka on hyvä puoli hyötyä jälkeen luottamuksellisia tietoja, kuten tietokannan yhteysmerkkijonot salasanojen aina näy tekstimuodossa tiedoston kuten `wp-config.php`.

Tämän prosessin, jäljempänä on hyötyä, kun suoritat sisältää sekä tiedoston ja tietokannan muutokset WordPress-sovelluksen:
- WordPress version päivittäminen
- Lisää uusia tai muokata tai päivittää laajennukset
- Lisää uusia tai muokata tai päivittää teemat

Määrittää sovelluksen asetukset:

- tietokannan tietoja
- ottaminen käyttöön tai poistaminen käytöstä WordPress kirjaaminen
- WordPress tietoturva-asetukset

![Sovelluksen asetusten Wordpress web App](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Varmista, että olet lisännyt oman tuotannon web app- ja vaiheen vapaan app-asetukset. Huomaa, että tuotannon web app- ja väliaikaisen web App-sovelluksen käyttää eri tietokannat.
Poista kaikki paitsi WP_ENV asetukset parametrit **Paikka asetus** valintaruudun valinta. Tämä Vaihda web-sovelluksen sekä tiedoston sisältöä ja tietokannan määritykset. Jos **Paikka-asetus** on **valittu**, web app-sovelluksen asetusten ja merkkijonon yhteysmäärityksen ei siirretä eri ympäristöissä kun tekevät SWAP-toimintoa ja näin ollen jos tietokannan muutokset ovat näkyvissä tämä ei katkaisee tuotannon koodiin.

Paikallista kehittämistä ympäristön web-sovelluksen käyttöönotto vaiheen web app-ja tietokanta käyttämällä WebMatrix tai tool(s), kuten FTP, Git tai PhpMyAdmin valittua.

![WWW-matriisin Julkaise WordPress web app-valintaikkuna](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Selaa ja testaa väliaikaisen koodiin. Tilanne, jossa web Appin teema on päivitettävä, koska näin väliaikaisen web Appissa.

![Selaa väliaikaisen online ennen paikkojen vaihtaminen](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Jos kaikki näyttää hyvältä, valitse väliaikaisen koodiin haluat siirtää sisältöä tuotantoympäristössä **Vaihda** -painiketta. Tässä tapauksessa vaihdetaan web app- ja tietokannan eri ympäristöissä jokaisen **vaihdon** aikana.

![Vaihda esikatselu muuttuu WordPress varten](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Jos sinun on vain push-tiedostoja (ei ole tietokannan päivitysten) tilanne, sitten **Tarkista** **Paikka-asetus** tietokantaan, jotka liittyvät *sovelluksen* ja *merkkijonot yhteysasetukset* -web-sovelluksen asetus sivu Azure esikatselu-portaalin ennen kuin teet SWAP. Tämän palvelupyynnön DB_NAME DB_HOST, DB_PASSWORD, DB_USER merkkijono oletusasetus olisi ei näkyvät esikatselu muuttuu, kun teet **Vaihda**. AT tällä kertaa, kun olet valmis **Vaihda** -toiminnon WordPress web Appissa on päivitykset tiedostoja **vain**.

Ennen kuin teet VAIHDON näin tuotannon WordPress web Appin ![tuotannon online ennen paikkojen vaihtaminen](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Vaihda-toiminnon jälkeen teeman on päivitetty tuotannon koodiin.

![Tuotannon web Appin jälkeen paikkojen vaihtaminen](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

Tilanne, kun haluat **peruuttaa**, voit tuotannon web app-asetukset ja valitsemalla Vaihda web app- ja tietokannan tuotannon väliaikaisen paikka **Vaihda** -painike. Tärkeää muistaa on, että jos tietokannan muutokset kuuluvat ajankohtana, **Vaihda** -toiminto, valitse uudelleen käyttöön, sinun täytyy ottaa käyttöön tietokannan väliaikaisen koodiin seuraavan kerran muuttuu nykyiseen tietokantaan, joka voi olla edellisen tietokantaan tai vaiheen tietokannan väliaikaisen web-sovelluksen.

#### <a name="summary"></a>Yhteenveto
Jos haluat generalize tietokannan sovelluksen prosessi

1. Asenna sovellus paikalliseen ympäristöön
2. Sisällytä ympäristön tietty määritys (paikalliset ja Azure Web App-sovelluksen)
3. Palvelun Web sovellukset – väliaikaisen, tuotannon oman ympäristöissä määrittäminen
4. Jos sinulla on jo käytössä Azure tuotannon-sovelluksen, synkronoida tuotannon sisältöä (tiedostot/koodi + tietokannan) paikallisen ja väliaikaisen ympäristön.
5. Kehittää sovelluksen paikalliseen ympäristöön
6. Vie tuotannon koodiin kohdassa ylläpito tai lukittu tila ja Synkronoi tietokannan sisällön tuotannon väliaikaisen ja keskihajonta-Ympäristöt
7. Ottaa käyttöön väliaikaisen ympäristön ja testi
8. Ottaa käyttöön tuotantoympäristössä
9. Toista vaiheet 4 – 6

### <a name="umbraco"></a>Umbraco
Tässä osassa opit, miten Umbraco CMS käyttää mukautettuja moduuli ottaa yli useita DevOps ympäristössä. Tässä esimerkissä on sisäkkäistä useita kehittäminen ympäristöissä hallintaan.

[Umbraco CMS](http://umbraco.com/) on popular.NET CMS ratkaisuja monta sovelluskehittäjille joka tarjoaa [Courier2](http://umbraco.com/products/more-add-ons/courier-2) moduulin kehittäminen käyttöön väliaikaisen tuotannon ympäristöt. Voit helposti luoda paikallisen kehitysympäristö Visual Studio tai WebMatrix Umbraco CMS-verkkosovelluksessa.

1. Umbraco web-sovelluksen luominen Visual Studiossa, [napsauttamalla tätä](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Jotta voit luoda Umbraco web app-sovelluksessa WebMatrix, [napsauttamalla tätä](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Muista aina poistaa `install` kansio, valitse sovelluksen ja koskaan lataa se vaihe vai tuotannon verkkosovelluksissa. Tässä opetusohjelmassa, voin käyttää WebMatrix

#### <a name="set-up-a-staging-environment"></a>Väliaikaisen ympäristön määrittäminen
- Luoda käyttöönoton paikka Umbraco CMS web Appissa, jos sinulla on jo Umbraco CMS web app-sovelluksessa ja suorittamalla edellä mainituista. Jos et voit luoda sen Marketplacesta.

- Päivitä vaiheen käyttöönotto-paikka, valitse juuri luomasi tietokannan **umbraco vaiheen db -**yhteysmerkkijono. Tuotannon web app (umbraositecms-1)- ja väliaikaisen web app (umbracositecms 1-tason) **on** osoittamalla eri tietokannat.

![Päivitä yhteysmerkkijonon väliaikaisen web app, jossa uusi väliaikainen tietokanta](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Valitse **Hae Julkaisuasetukset** käyttöönoton paikka **vaiheen**. Tämä Lataa Julkaise asetustiedoston, joka tallentaa kaikki tiedot, joita tarvitaan Visual Studio tai Web-matriisin julkaista sovelluksesi paikallista kehittämistä web Appista Azure web App-sovellukseen.

 ![Hae julkaista väliaikaisen web Appin määrittäminen](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Avaa paikallinen kehittäminen koodiin **WebMatrix** tai **Visual Studio**. Tässä opetusohjelmassa käytän Web-matriisi ja sinun on ensin väliaikaisen verkkosovelluksessa Julkaise asetukset-tiedoston tuominen

![Julkaise tuontiasetusten Umbraco käyttäminen WWW-matriisi](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Tarkista muutokset-valintaikkunassa ja paikallinen web-sovelluksen käyttöönotto Azure web App-sovellukseen, *umbracositecms 1-vaiheen*. Kun otat tiedostoja suoraan väliaikaisen web-sovelluksen käyttöön jätät tiedostoista `~/app_data/TEMP/` kansio, kun ne luodaan, kun vaiheen web Appin alkuun. Kannattaa myös jättää `~/app_data/umbraco.config` Tallenna nimellä, luodaan.

![Julkaise verkko-matriisissa muutosten tarkasteleminen](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Jälkeen onnistuneesti väliaikaisen web Appin Umbraco paikallisen online julkaiseminen, Selaa väliaikaisen web App-sovellukseen ja suorittaa muutama testejä minkä tahansa ongelmia.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 käyttöönoton moduulin määrittäminen
[Courier2](http://umbraco.com/products/more-add-ons/courier-2) -moduulia voi push sisältöä, tyylisivut, kehitystä moduulit ja lisää yksinkertainen kakkospainikkeella väliaikaisen web Appista tuotannon web App Lisää näissä vapaa ominaisuuksissa ja, tuotannon koodiin kun päivitys otettaessa riskin pienentäminen.
Ostaa käyttöoikeuden Courier2 toimialueen `*.azurewebsites.net` ja mukautetun toimialueen (vaikkapa http://abc.com) Kun olet ostanut käyttöoikeudet, Aseta ladatut käyttöoikeuden (. LIC-tiedosto)- `bin` kansio.

![Avattavan käyttöoikeus-tiedosto kohdassa bin-kansio](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Lataa Courier2 paketti [täältä](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Vaiheen web App-sovellukseen kirjautuminen, sano http://umbracocms-site-stage.azurewebsites.net/umbraco ja sitten **Kehitystyökalut** -valikkoa ja valitse **paketit**. Valitse **Asenna** paikallinen pakkaus

![Umbraco paketin asennusohjelman](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Lataa courier2-paketin asennuksen avulla.

![Courier moduulin paketin lataaminen](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Voit määrittää on päivitettävä courier.config tiedosto **Config** kansiossa koodiin.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Valitse `<repositories>`, tuotannon sivuston URL-osoite ja käyttäjän tiedot. Jos käytössäsi on oletusarvoinen Umbraco jäsenyyden tarjoaja, Lisää hallinta käyttäjän tunnus <user> osa. Jos käytössäsi on mukautettu Umbraco jäsenyyden toimittaja, käytä `<login>`,`<password>` Courier2 moduuliin tietää, miten voit muodostaa yhteyden tuotannon-sivustoon. Lisätietoja on Tarkista Courier moduulin [ohjeissa](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) .

Vastaavasti Courier-moduulin asentaminen tuotannon sivuston ja määritä se vaiheen web Appissa, osoita sen vastaaviin courier.config tiedostossa tässä esitetyllä tavalla

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Courier2-välilehden Umbraco CMS web app raporttinäkymät-ikkunassa ja valitse sijainnit. Säilön nimi näkyy mainitun `courier.config`. Toimi samoin oman tuotannon ja väliaikaisen verkkosovelluksissa.

![Näytä web app kohdesäilöön](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Nyt avulla, joidenkin tuotannon sivuston väliaikaisen sivuston sisällön käyttöönotto. Siirry sisällön ja valita aiemmin luodun sivun tai Luo uusi sivu. Valitse olemassa olevan sivun Omat web Appista, jossa sivun otsikko muuttuu aloittaminen **– Uusi** ja valitse **Tallenna**ja Julkaise.

![Muuta sivun otsikon ja julkaiseminen](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Valitse seuraavaksi muokattu sivu ja *Napsauta hiiren kakkospainikkeella* Näytä kaikki asetukset. Valitse **Courier** tarkastelemaan käyttöönotto-valintaikkuna. Valitse **Ota** aloittaa käyttöönotto

![Courier moduulin käyttöönotto-valintaikkuna](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Tarkista muutokset ja valitse sitten Jatka.

![Courier moduulin käyttöönoton valintaikkunan Tarkastele muutoksia](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Käyttöönoton loki näyttää käyttöönotto onnistui.

 ![Tarkastele käyttöönoton lokeja Courier moduulista](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Selaa tuotannon koodiin näkevän muutokset ovat näkyvissä.

 ![Selaa tuotannon web Appissa](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Lisätietoja käyttämisestä Courier, tarkista ohjeissa.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Umbraco CMS-version päivittäminen

Courier eivät auta käyttöönotto päivittäminen Umbraco CMS yhdestä versiosta toiseen. Päivitystä Umbraco CMS-versiota, yhteensopivuusongelmia on tarkistavan mukautetun moduulit tai kolmannen osapuolen moduulit ja Umbraco Core-kirjastojen kanssa. Paras käytäntö

1. AINA varmuuskopioida web app- ja tietokanta ennen kuin teet päivityksen. Azure Web Appissa, voit määrittää automaattisen varmuuskopioinnin lisääminen sivustoihin varmuuskopioinnin ominaisuus ja palauttaa sivuston Jos tarvitaan käyttämällä palauttaa ominaisuus. Lisätietoja on artikkelissa [koodiin varmuuskopioiminen](web-sites-backup.md) ja [palauttaminen web Appissa](web-sites-restore.md).

2. Tarkista, jos käytössäsi on kolmannen osapuolen paketit ovat yhteensopivia kuvalla, johon olet päivittämässä. Lataa sivu paketti, Tarkista yhteensopivuus Project-Umbraco CMS-versio.

Lisätietoja päivittäminen web Appin paikallisesti ohjeiden kuin mainitaan [Tässä](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Kun paikallinen development-sivusto on päivitetty, väliaikaisen web App-sovelluksen muutosten julkaiseminen. Testaa sovelluksesi ja jos kaikki näyttää hyvältä, käytä **Vaihda** -painiketta, **Vaihda** väliaikaisen sivuston tuotannon web App-sovellukseen. **Vaihda** -toimintoa suoritettaessa näet muutokset, jotka voi vaikuttaa web-sovelluksen kokoonpanon. **Vaihda** tämä toiminto on ovat vaihtaminen web Apps-sovelluksista ja -tietokannat. Tämä tarkoittaa sitä, kun Vaihda tuotannon web Appissa, osoittavat umbraco vaiheen db-tietokantaan ja väliaikaisen web Appin osoittaa umbraco tuot db-tietokantaan.

![Vaihda esikatselu Umbraco CMS käyttöönotto](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Hyödyntää web app- ja tietokannan vaihtaminen:
1. Mahdollistaa Jos ongelmia sovelluksen palauttamaan web-sovelluksen kanssa toiseen, **Vaihda** edellisen version.
2. Päivitystä varten tarvitset tiedostoja ja tietokannan väliaikaisesta web App-sovelluksen ja tuotannon web app-tietokannan käyttöönotto. On monia asioita, jotka voit siirtyä virhe, kun tiedostoja ja tietokannan käyttöönotto. Paikkojen **vaihtaminen** -ominaisuuden avulla voimme vähentää käyttökatkot päivittämisen aikana ja vähentää virheitä, joita voi ilmetä, kun otat muutokset.
3. Ansiosta voit tehdä **A / B testaaminen** [Testing tuotannon](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) -toiminnon avulla

Tässä esimerkissä kerrotaan platform joustavuutta jossa voit luoda mukautetun moduulit samalla Umbraco Courier moduulin käyttöönoton hallitsemaan ympäristössä.

## <a name="references"></a>Viittaukset
[Joustava software development Azure-sovelluksen ominaisuudet](app-service-agile-software-development.md)

[Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen](web-sites-staged-publishing.md)

[Tuotannon käyttöönoton paikkojen web käytön estäminen](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
