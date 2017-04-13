<properties
    pageTitle="Azure App palvelun web Appin advanced config ja laajennukset"
    description="Käyttää XML-tiedoston Transformation(XDT) ilmoitukset, muuntamiseen ApplicationHost.config-tiedoston Azure App Service-sovellukseen ja yksityinen laajentamisesta käyttöön mukautettu hallinnan toiminnot."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App palvelun web Appin advanced config ja laajennukset

[Asiakirjan parannus](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) ilmoitusten avulla voit muuntaa [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) -tiedoston Azure App Service-sovellukseen. Voit käyttää myös XDT ilmoitukset yksityinen laajentamisesta käyttöön mukautettu web app hallinnan toiminnot. Tässä artikkelissa on esimerkki PHP hallinnan web app tunniste on, joka mahdollistaa web-liittymän kautta PHP asetusten hallinta.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Määritysten Lisäasetukset ApplicationHost.config kautta
Sovelluksen Service-ympäristö tarjoaa joustavuutta ja hallinnan web-sovelluksen kokoonpanon. Vaikka vakio IIS ApplicationHost.config kokoonpanotiedosto ei voi muokata suoraan App palvelu, ympäristö tukee määritettäviä ApplicationHost.config Muunna mallin perusteella XML asiakirjan muunnos (XDT).

Hyödyntää muunto-toiminto luo ApplicationHost.xdt tiedosto XDT sisältö ja sivusto-pääkansio (d:\home\site) [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console)-kohdassa sijainti. Voit joutua käynnistämään Web App, muutokset tulevat voimaan.

ApplicationHost.xdt seuraavassa esimerkissä esitetään, kuinka voit lisätä uuden mukautetun ympäristömuuttuja web App-sovellukseen, joka käyttää PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Lokitiedoston Muunna tila ja tiedot on käytettävissä LogFiles\Transform FTP-kansiosta.

Katso Lisää esimerkkejä [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Huomautus**<br />
Moduulit-luettelosta osat `system.webServer` ei voi poistaa tai tilattava, mutta lisäykset luetteloon on mahdollista.


##<a id="extend"></a>Laajenna web Appissa

###<a id="overview"></a>Yleistä yksityinen web app-laajennukset

Sovelluksen palvelu tukee web app-laajennuksia laajennettavuus-kohdan hallintatehtäviin varten. Joitakin sovelluksen ympäristö ominaisuudet ovat itse asiassa toteutettu esiasennettu tunnisteesta. Kun esiasennettu platform-laajennukset ei voi muokata, voit luoda ja määrittää yksityisen tunnisteesta web-sovelluksen. Tämä toiminto on riippuvainen myös XDT ilmoitukset. Avaimen yksityinen web app-laajennuksen luomisen vaiheet ovat seuraavat:

1. Web app-laajennus **sisällön**: kaikki sovelluksen palvelun tukemat web-sovelluksen luominen
2. Web-sovelluksen laajennuksen **ilmoitus**: ApplicationHost.xdt-tiedoston luominen
3. Web-sovelluksen laajennuksen **käyttöönoton**: sisällön sijoittaminen SiteExtensions-kansio`root`

Web-sovelluksen sisäisiä linkkejä osoitettava suhteessa sovelluksen polkua ApplicationHost.xdt tiedoston polku. ApplicationHost.xdt tiedostoon muutoksia edellyttää web app-Roskakori.

**Huomautus**: Lisätietoja tärkeimmät elementit on osoitteessa [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Esimerkki sisältyy Arvovirtakarttojen luominen ja ottaminen käyttöön yksityinen web app-laajennuksen. Lähdekoodin PHP hallinta, joka on esimerkiksi voi ladata [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Web app-tunniste Esimerkki: PHP hallinta

PHP hallinta on web app-laajennus, jonka avulla web app-järjestelmänvalvojat voivat helposti tarkastella ja määrittää WWW-käyttöliittymän avulla sen sijaan, että voit muokata PHP .ini-tiedostot suoraan PHP. Yleisten määritysten tiedostot PHP sisällyttää php.ini tiedoston sijaitseva Program Files ja. user.ini koodiin pääkansio, sijaitsevaa tiedostoa. Koska php.ini-tiedostoa ei voi muokata suoraan App Service-ympäristössä, PHP hallinta-laajennus käyttää. user.ini tiedoston asetus muutokset tulevat voimaan.

####<a id="PHPwebapp"></a>PHP hallinnan web-sovelluksen

Seuraavassa on PHP hallinnan käyttöönotto aloitus-sivulla:

![TransformSitePHPUI][TransformSitePHPUI]

Kuten näet, web app-laajennus on aivan kuin tavallinen web-sovelluksen, mutta muita ApplicationHost.xdt tiedoston sijoitetaan pääkansio (Lisätietoja ApplicationHost.xdt tiedoston ovat käytettävissä seuraavassa osassa on tämän artikkelin) web-sovelluksen kanssa.

PHP hallinta-laajennus on luotu käyttämällä Visual Studio ASP.NET MVC 4 Web-sovelluksen malli. Ratkaisu Explorerista seuraavassa näkymässä näkyy rakenteen PHP hallinta-laajennus.

![TransformSiteSolEx][TransformSiteSolEx]

Tiedoston i/o vain määräten logiikan on osoittamaan web Appin wwwroot-kansion sijainti. Koodin seuraavassa esimerkissä esitetään, ympäristön muuttujan "ALOITUS" osoittaa web-sovelluksen pääkansion polku ja wwwroot polku on rakennettava liittämällä "site\wwwroot":

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Kun sinulla on kansiopolun, voit käyttää säännöllisesti tiedostotoiminnot i/o lukemiseen ja kirjoittamiseen tiedostoihin.

Yhden pisteen web app on hyvä muistaa osalta sisäisiä linkkejä käsittely.  Jos sinulla on linkkejä HTML-tiedostot, jotka antavat absoluuttiset polut sisäisiä linkkejä web-sovellukseen, sinun on varmistettava, linkit ovat $a tunniste nimesi siinä muodossa kuin oman pääkansio. Tämä tarvita, sillä alanumerosi pääkansio on nyt "/`[your-extension-name]`/" sen sijaan, että käytössä vain "/", niin missään sisäinen linkit on päivitettävä vastaavasti. Oletetaan esimerkiksi, että koodisi sisältää linkin seuraavasti:

`"<a href="/Home/Settings">PHP Settings</a>"`

Kun linkki on osa web app-laajennuksen, linkki on oltava seuraavassa muodossa:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Voit kiertää tämän tarpeen mukaan joko vain suhteellisten polkujen web-sovelluksen tai, jos ASP.NET-sovelluksia käyttämällä `@Html.ActionLink` menetelmä, joka luo linkit.

####<a id="XDT"></a>ApplicationHost.xdt-tiedosto

Web app-laajennus koodi siirtyy kohdassa %HOME%\SiteExtensions\[oma tunniste-nimi]. Olemme Soita tämä tunniste pääkansio.  

Voit rekisteröidä web app-laajennus applicationHost.config-tiedoston, tarvitset Sijoita tiedosto nimeltä ApplicationHost.xdt tunniste päätasolla. ApplicationHost.xdt-tiedoston sisällön pitäisi näyttää seuraavalta:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Valitset tunniste nimeksi nimi on sama nimi kuin tunniste pääkansio.

Tämä on vaikutus lisääminen uusi Sovelluspolku `system.applicationHost` sivustoluettelon palvelujen ohjauksen hallinta-sivustoon. Palvelujen ohjauksen hallinta-sivusto on sivuston hallinta-lopuksi tietyn access-tunnuksilla. URL-osoite on `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web app-tunniste käyttöönotto

Asenna web app-laajennus, voit käyttää FTP kopioi kaikki web-sovelluksen tiedostot `\SiteExtensions\[your-extension-name]` web Appissa, johon haluat asentaa tunniste-kansioon.  Muista kopioida ApplicationHost.xdt tiedostoa tähän sijaintiin. Käynnistä uudelleen käyttöön tunnisteen koodiin.

Sinun pitäisi nähdä web app-laajennuksen etsiminen:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Huomaa, että URL-osoite näyttää vain web app-URL-osoite, kuten lukuun ottamatta sitä, että se käyttää HTTPS ja sisältää ".scm".

Voi estää kaikki yksityinen (ei ole valmiiksi asennettu) tunnisteet web App-ohjelman aikana kehittäminen ja tutkimusten lisäämällä sovelluksen asetukset-näppäimen kanssa `WEBSITE_PRIVATE_EXTENSIONS` ja arvoa `0`.

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
