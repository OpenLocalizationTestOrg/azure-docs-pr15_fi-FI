<properties
    pageTitle="Yritysluokan WordPress Azure App palvelun | Microsoft Azure"
    description="Lue, miten voit isännöidä yritysluokan WordPress-sivustoa Azure sovelluksen-palvelusta"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Yritysluokan WordPress Azure sovelluksen-palvelusta

Azure sovelluksen-palvelu sisältää tehtävä kriittinen, suuri asteikko [WordPress] skaalattava, turvallinen ja helppokäyttöinen käyttöympäristön[ wordpress] sivustot. Microsoft itse suorittaa yritysluokan sivustoissa, kuten [Office] [ officeblog] ja [Bing] [ bingblog] blogit. Tässä asiakirjassa kerrotaan, kuinka voivat käyttää Azure palvelun Web sovellukset ja ylläpitävät yritysluokan, pilvipohjainen WordPress-sivusto, voit käsitellä päivittäin erittäin paljon vierailijat.

## <a name="architecture-and-planning"></a>Arkkitehtuuri ja suunnittelu

Vain kaksi järjestelmävaatimukset ovat basic WordPress-asennuksen.

* **MySQL-tietokantaan** – saatavana [ClearDB Azure Marketplacesta][cdbnstore], tai voit hallita omia MySQL-asennus-Azuren näennäiskoneiden joko [Windows] [ mysqlwindows] tai [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB sisältää useita erilaisia ominaisuuksia kullekin MySQL määrityksiä. Katso [Azure kaupan] [ cdbnstore] lisätietoja Azure-kaupan kautta tai suoraan [ClearDB](http://www.cleardb.com/pricing.view)sivustossa tarkastelu tarjouksista.

* **PHP 5.2.4 tai suurempi** -sovelluksen Azure-palvelu on tällä hetkellä [PHP-versio 5.4, 5,5, ja 5.6][phpwebsite].

    > [AZURE.NOTE] On suositeltavaa aina käynnissä PHP varmistaa, käytössäsi ovat uusimmat tietoturvakorjauksia uusimman version.

### <a name="basic-deployment"></a>Tavallinen käyttöönotto

Käytä vain edellytykset, voit luoda basic ratkaisu Azure alueella.

![yhden Azure alueen ylläpidettävä Azure web app- ja MySQL-tietokantaan][basic-diagram]

Kun tämä Salli asteikko-kohtaa sovelluksen luomalla useita Web Apps-sovellusten esiintymiä sivuston, kaikki nykyisessä tietyn maantieteellisen alueen tiedot-keskukset kuluessa. Tämän alueen ulkopuolella käyttäjiltä saattaa kohdassa hidas vastauksen kertaa käytettäessä sivuston ja tietoja-keskukset tällä alueella siirtyy alaspäin, niin sovelluksen.


### <a name="multi-region-deployment"></a>Monille käyttöönotto

Azure [Liikenteen hallinta][trafficmanager], ei skaalaudu WordPress sivuston eri useita maantieteellisillä alueilla käyttämisessä käyttäjille on vain yksi URL-osoite. Kaikki käyttäjät voivat olla kautta liikenteen hallinta ja reititetään sitten kuormituksen Vastatilin määrittäminen perustuu alueen.

![Azure-web-sovelluksen, ylläpidettävä alueille käyttämällä CDBR suuren käytettävyyden reitittimen reitittämiseen MySQL eri alueilla][multi-region-diagram]

Kullakin alueella WordPress sivuston skaalata edelleen useita Web Apps-sovellusten esiintymiä eri, mutta tämä skaalaus on aluekohtaisia; suuri tietoliikenne alueiden skaalata eri tavalla kuin pienen liikenne niistä.

Replikoinnin ja useiden MySQL-tietokantojen reititys voidaan toteuttaa käyttämällä ClearDB's [CDBR suuri käytettävyys reitittimen] [ cleardbscale] (kuten vasen) tai [MySQL-klusterin CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Monille-ympäristö, jonka media tallennustilan ja tallentamisesta välimuistiin
Jos sivuston hyväksyy lataukset tai host mediatiedostoja, käytä Azure-Blob-säiliö. Jos tarvitset tallentamisesta välimuistiin, harkitse [Redis välimuistin][rediscache], [Memcache pilveen](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)tai jokin muiden välimuistiin tarjousten [Azure-kaupasta](http://azure.microsoft.com/gallery/store/).

![Azure web-sovelluksen, ylläpidettävä alueille CDBR suuren käytettävyyden reitittimen käytön MySQL hallitun välimuistin ja CDN Blob-objektien tallennustilaan][performance-diagram]

Blob-objektien tallennustilaan on geo jaettu eri alueilla oletusarvoisesti, joten sinun ei tarvitse huolehtia replikoiminen tiedostoja kaikissa sivustoissa. Voit myös ottaa Azure [Sisällön jakauma (CDN)] [ cdn] Blob-objektien tallennustilaan, joka jakaa tiedostoja Lopeta solmut lähemmäksi kävijät.

### <a name="planning"></a>Suunnittelu

#### <a name="additional-requirements"></a>Lisävaatimukset

Voit tehdä tämän... | Käytä tätä...
------------------------|-----------
**Lataa ja tallentaa suuria tiedostoja** | [WordPress laajennuksen käytön Blob-objektien tallennustilaan][storageplugin]
**Sähköpostin lähettäminen** | [SendGrid] [ storesendgrid] ja [WordPress laajennuksen käytön SendGrid][sendgridplugin]
**Mukautettuja toimialuenimiä** | [Mukautetun toimialuenimen määrittäminen Azure App palvelun][customdomain]
**HTTPS** | [HTTPS käyttöön verkkosovellukseen Azure sovelluksen-palvelussa][httpscustomdomain]
**Edeltävien vahvistus** | [Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen][staging] <p>Siirtyminen verkkosovellukseen väliaikaisesta tuotannon myös siirtää WordPress-määritys. Varmista, että kaikki asetukset on päivitetty tuotannon sovelluksen vaatimukset ennen siirtymistä vaiheistettu sovelluksen kyselyjä tuotannon.</p>
**Valvonta ja vianmääritys** | [Ota käyttöön vianmäärityksen kirjaus käyttöön web Apps-sovellusten Azure App palvelun] [ log] ja [Web-sovellusten valvonta Azure sovelluksen-palvelussa][monitor]
**Ottaa käyttöön sivustossa** | [Azure-sovelluksen palvelun web-sovelluksen käyttöönotto][deploy]

#### <a name="availability-and-disaster-recovery"></a>Käytettävyys ja tietojen palauttaminen

Voit tehdä tämän... | Käytä tätä...
------------------------|-----------
**Lataa saldo sivustoja** tai **geo-jakaa sivustoja** | [Reitin liikenne Azure liikenteen hallinta][trafficmanager]
**Varmuuskopiointi ja palauttaminen** | [Varmuuskopioi verkkosovellukseen Azure-sovelluksen palvelun] [ backup] ja [palauttaa Azure-sovelluksen palvelun verkkosovellukseen][restore]

#### <a name="performance"></a>Suorituskyky

Suorituskyvyn pilveen saavutetaan ensisijaisesti kautta asteikko-kohtaa, ja välimuistiin tallentaminen kuitenkin muistin kaistanleveys ja määritteet Web Apps-sovelluksista isännöintipalvelu pitäisi pidettävä.

Voit tehdä tämän... | Käytä tätä...
------------------------|-----------
**Tietoja sovelluksen palvelun esiintymän ominaisuuksia.** | [Hinnoittelutiedot, mukaan lukien sovelluksen palvelutasot ominaisuuksia][websitepricing]
**Välimuistin resurssit** | [Välimuistin Redis][rediscache], [Memcache pilveen](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)tai jokin muiden välimuistiin tarjousten [Azure-kaupasta](/gallery/store/)
**Skaalaa sovelluksen** | [Skaalaa verkkosovellukseen Azure App palvelun] [ websitescale] ja [ClearDB suuri käytettävyys reititys][cleardbscale]. Jos päätät isännöidä ja hallita MySQL-asennuksen, sinun kannattaa harkita [MySQL-klusterin CGE] [ cge] skaalaus-kohtaa varten

#### <a name="migration"></a>Siirron

On aiemmin luotuun WordPress-sivustoon siirtyminen Azure App palvelun kahdella eri tavalla.

* ** [WordPress Vie] [ export] ** -blogin, jotka voidaan tuoda sitten Azure App palvelu [WordPress tuontitoiminto laajennuksen]WordPress uuteen sivustoon sisällön viedään[import].

    > [AZURE.NOTE] Vaikka tämä prosessi voi siirtää sisältöä, se eivät siirry minkä tahansa laajennukset, teemat tai muita muokkauksia. Ne on asennettava uudelleen manuaalisesti.

* **Manuaalinen siirron** - [sivuston varmuuskopioiminen] [ wordpressbackup] ja [tietokannan][wordpressdbbackup], valitse manuaalisesti palauttaa Azure-sovelluksen palvelun web App-sovellukseen ja liittyvät MySQL-tietokantaan, voit siirtää erittäin mukautettujen sivustojen ja välttää tedium asennuksen manuaalisesti laajennukset, teemojen ja muut mukautukset.

## <a name="step-by-step-instructions"></a>Vaiheittaiset ohjeet

### <a name="create-a-wordpress-site"></a>WordPress sivuston luominen

1. Käytä [Azure Marketplacesta] [ cdbnstore] - [arkkitehtuuri ja suunnittelu](#planning) -kohdassa, että isännöit sivuston voimassa havaittu luominen koon MySQL-tietokantaan.

2. [Luo WordPress verkkosovellukseen Azure App palvelussa] noudattamalla[ createwordpress] WordPress verkkosovelluksen luomiseen. Luodessasi web Appissa, valitse **Käytä aiemmin luotuun MySQL-tietokantaan** ja valitse vaiheessa 1 luotu tietokanta.

Jos olet siirtymässä aiemmin luotuun WordPress-sivustoon, katso lisätietoja [artikkelista Azure valmis WordPress sivusto](#Migrate-an-existing-WordPress-site-to-Azure) jälkeen uusi web-sovelluksen.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Aiemmin luodun WordPress-sivuston siirtäminen Azure

Kuten [arkkitehtuuri ja suunnittelu](#planning) -osassa, on kaksi tapaa WordPress sivuston siirtäminen.

* **vieminen ja tuominen** - sivustojen paljon mukautuksia tai johon haluat siirtää sisällön.

* **Varmuuskopiointi ja palauttaminen** - sivuston mukauttaminen paljon kohtaa, johon haluat siirtää kaikki.

Siirtää sivustosi jollakin seuraavissa osissa.

#### <a name="the-export-and-import-method"></a>Menetelmä vieminen ja tuominen

1. Käytä [WordPress Vie] [ export] Vie nykyisen sivuston.

2. Luo [WordPress sivuston luominen](#Create-a-new-WordPress-site) -osan ohjeita käyttämällä verkkosovellukseen.

3. Kirjaudu sisään WordPress sivuston Web Apps-sovellusten ja valitse **laajennukset** -> **Lisää uusi**. Etsi ja asenna **WordPress tuonti** -laajennus.

4. Kun tuonti-laajennus on asennettu, valitse **Työkalut** -> **tuominen**ja valitse sitten **WordPress** käyttää WordPress tuonti-laajennus.

5. Valitse **Tuo WordPress** -sivulla, **Valitse tiedosto**. Nykyisen WordPress sivuston viety WXR tiedosto selaamalla ja valitse sitten **Lataa tiedosto palvelimeen ja tuoda**.

6. Valitse **Lähetä**. Sinua kehotetaan tuonti onnistui.

8. Kun olet tehnyt nämä vaiheet, Käynnistä uudelleen sen web app-sivu sivuston [Azure portal][mgmtportal].

Kun olet tuonut sivustoon, joudut ehkä suorittamalla seuraavat vaiheet eivät sisälly tuontitiedoston asetukset käyttöön.

Jos käytit tämä... | Tee näin
------------------ | ----------
**Pysyvät linkit** | Uuden sivuston WordPress-koontinäyttö, valitse **asetukset** -> **pysyvät linkit** ja sitten Päivitä pysyvät linkit-rakenne
**Kuva/median linkkejä** | Voit päivittää linkit uuteen paikkaan, käytä [Velvet asetusta Päivitä URL-osoitteet laajennuksen][velvet], Etsi ja korvaa-työkalun tai manuaalisesti tietokannassa
**Teemat** | Valitse **Ulkoasu** -> **Teema** ja Päivitä tarvittaessa sivuston teemaa
**Valikot** | Jos teeman tukee valikot, linkit aloitussivulle ehkä ne upotettuna vanha alikansiota. Valitse **Ulkoasu** -> **valikot** ja päivittää ne

#### <a name="the-backup-and-restore-method"></a>Varmuuskopiointi ja palauttaminen-menetelmällä

1. Varmuuskopioi aiemmin WordPress sivuston mainittujen tietojen avulla osoitteessa [WordPress varmuuskopioiden][wordpressbackup].

2. Varmuuskopioi tiedot käyttämällä [Tietokannan varmuuskopioiminen]aiemmin luotu tietokanta[wordpressdbbackup].

3. Tietokannan luominen ja palauttaa varmuuskopion.

    1. Ostaa uuden tietokannan [Azure Marketplacesta][cdbnstore]- tai [Windows] MySQL-tietokantaan[ mysqlwindows] tai [Linux] [ mysqllinux] AM.

    2. MySQL-asiakas, kuten [MySQL Workbench][workbench], muodostaa yhteyden uuteen tietokantaan ja tuoda WordPress tietokannan.

    3. Voit muuttaa toimialueen tapahtumat toimialueen Azure App Service-tietokannan päivittäminen. Esimerkiksi mywordpress.azurewebsites.net. [Etsi ja korvaa WordPress tietokantojen komentosarjan] [ searchandreplace] voit muuttaa turvallisesti kaikki esiintymät.

4. Web-sovelluksen luominen Azure-portaalissa ja julkaise WordPress varmuuskopion.

    1. Web-sovelluksen luominen [Azure portal] [ mgmtportal] kanssa tietokanta käyttämällä **uutta** -> **Web + Mobile** -> **Azure Marketplacesta** -> **Verkkosovelluksissa** -> **Web Appin + SQL** (tai **Web Appin + MySQL**) -> **Luo**. Määritä tarvittavat asetukset tyhjä verkkosovelluksen luomiseen.

    2. Varmuuskopiointiin WordPress **wp config.php** tiedosto ja avaa se-editorissa. Korvaa seuraavat tiedot uuteen MySQL-tietokantaan.

        * **DB_NAME** - käyttäjänimi ja tietokanta

        * **DB_USER** - käyttäjänimi, jota käytetään tietokannan

        * **DB_PASSWORD** - käyttäjän salasana

        Kun olet muuttanut nämä tietueet, Tallenna ja sulje **wp config.php** -tiedosto.

    3. Käytä [käyttöönotto verkkosovellukseen Azure-sovelluksen palvelun] [ deploy] tiedot, jotta käyttöönotto-menetelmää haluat käyttää ja WordPress varmuuskopion otetaan käyttöön web App-sovellukseen, sovelluksen Azure-palvelussa.

5. Kun WordPress-sivustossa on otettu käyttöön, sinun pitäisi käyttää uusi sivusto (kuin sovelluksen palvelun web app-)-muodossa *. azurewebsite.net sivuston URL-osoite.

### <a name="configure-your-site"></a>Oman sivuston määrittäminen

Kun WordPress-sivusto on luotu tai siirretty, käyttää lisätoimintoja ottaminen käyttöön tai suorituskyvyssä seuraavat tiedot.

Voit tehdä tämän... | Käytä tätä...
------------- | -----------
**Sovelluksen palvelun suunnitelman tilan, koon ja ota käyttöön skaalaus määrittäminen** | [Skaalaa verkkosovellukseen Azure sovelluksen-palvelussa][websitescale]
**Ota jatkuva tietokantayhteyksiä** | Oletusarvon mukaan WordPress ei käytä pysyvä tietokantayhteyksiä, mikä saattaa aiheuttaa yhteys tietokannan muuttuvat rajoittanut jälkeen useita yhteyksiä. Jotta pysyvät yhteydet, asenna (pysyvät yhteydet sovittimen laajennus) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Suorituskyvyn parantaminen** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Poista käytöstä ARR eväste</a> - voi parantaa suorituskykyä käytettäessä WordPress useita Web Apps-sovellusten esiintymiä</p></li><li><p>Ota käyttöön välimuisti. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis välimuisti</a> <a href="https://wordpress.org/plugins/redis-object-cache/">objektin välimuistiin WordPress laajennuksen Redis</a>tai käytä jotakin muiden <a href="/gallery/store/">Azure kaupan</a> välimuistiin tarjousten voidaan käyttää (ennakkoversio)</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Voit tehdä WordPress nopeammin Wincache</a> - Wincache on käytössä oletusarvoisesti Web Apps-sovellukset</p></li><li><p><a href="../web-sites-scale/">Azure-sovelluksen palvelun verkkosovellukseen asteikko</a> ja <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB suuri käytettävyys reititys</a> tai <a href="http://www.mysql.com/products/cluster/">MySQL-klusterin CGE</a></p></li></ul>
**Käytä BLOB storage** | <ol><li><p><a href="../storage-create-storage-account/">Azure-tallennustilan tilin luominen</a></p></li><li><p>Katso miten geo käyttöön <a href="../cdn-how-to-use/">Sisällön jakauman verkon (CDN)</a> -jakaa BLOB tallennettuja tietoja.</p></li><li><p>Asenna ja määritä <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure-tallennustilan WordPress laajennuksen</a>.</p><p>Yksityiskohtaiset asennuksen ja laajennus määritystietoja on artikkelissa <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">käyttöoppaassa</a>.</p> </li></ol>
**Sähköpostin ottaminen käyttöön** | Ota käyttöön <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> Azure-säilöä. Asenna <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">laajennus SendGrid</a> WordPress.
**Mukautetun toimialuenimen määrittäminen** | [Mukautetun toimialuenimen määrittäminen Azure App palvelun][customdomain]
**Ottaa käyttöön mukautettua toimialuenimeä HTTPS** | [HTTPS käyttöön verkkosovellukseen Azure sovelluksen-palvelussa][httpscustomdomain]
**Lataa saldo tai geo-sivuston jakaminen** | [Reitittää liikenteen Azure liikenteen hallinta][trafficmanager]. Jos käytät mukautettua toimialuetta, Lisätietoja on kohdassa [Configure mukautettua toimialuenimeä Azure-sovelluksen palvelun] [ customdomain] tietojen käyttäminen mukautettuja toimialuenimiä liikenteen hallinta
**Ottaa käyttöön automaattisen varmuuskopioinnin suunnittelu** | [Azure-sovelluksen palvelun verkkosovellukseen varmuuskopiointi][backup]
**Ota Diagnostiikan kirjaus** | [Ota Diagnostiikan kirjaus käyttöön verkkosovelluksissa Azure sovelluksen-palvelussa][log]

## <a name="next-steps"></a>Seuraavat vaiheet

* [WordPress optimointi](http://codex.wordpress.org/WordPress_Optimization)

* [Muuntaa WordPress Multisite Azure sovelluksen-palvelussa](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB päivittäminen Azure ohjattu](http://www.cleardb.com/store/azure/upgrade)

* [WordPress isännöinti alikansion koodiin Azure sovelluksen-palvelussa](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Vaiheittaiset ohjeet: Käyttämällä Azure WordPress sivuston luominen](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Isännöidä aiemmin WordPress Azure-blogi](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Aika pysyvät linkit-WordPress ottaminen käyttöön](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Siirtää ja suorittamisesta WordPress blogimerkinnän Azure sovelluksen-palvelusta](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Suorittaminen WordPress Azure App palvelun maksutta](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [Valitse Azure 2 minuuttia tai vähemmän WordPress](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [WordPress blogin siirtäminen Azure - osa 1: Azure WordPress blogin luominen](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [WordPress blogin siirtäminen Azure - osa 2: sisällön siirtäminen](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [WordPress blogin siirtäminen Azure - osa 3: mukautetun toimialueen määrittäminen](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [WordPress blogin siirtäminen Azure - osa 4: aika pysyvät linkit ja URL-osoite uudelleen säännöt](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [WordPress blogin siirtäminen Azure - osan 5: siirtyvät alikansion pääkansio](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Voit määrittää WordPress verkkosovellukseen Azure-tilisi](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping WordPress Azure-määrittäminen](http://www.johnpapa.net/wordpress-on-azure/)

* [Vihjeitä Azure WordPress](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
