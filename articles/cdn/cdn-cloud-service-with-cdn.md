<properties
    pageTitle="Integroi pilvipalveluun Azure CDN | Microsoft Azure"
    description="Opetusohjelma, avulla opit pilvipalvelussa, joka on integroitu Azure CDN päätepisteen sisällön käyttöönotto"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Integroi Azure CDN pilvipalveluun

Pilvipalveluun voi integroida Azure CDN, että tarjoillaan sisältöä pilvipalvelussa sijainnista. Tämän menetelmän avulla seuraavat edut:

- Helposti käyttöön ja Päivitä kuvien, komentosarjojen ja tyylisivut pilvipalvelussa sijaitsevaan project-kansioissa
- Päivitä helposti pilvipalvelussa, kuten jQuery tai käynnistyksen versiot NuGet-paketit
- Web-sovelluksen ja että CDN served sisällön kaikki Visual Studio samalla käyttöliittymästä hallinta
- Web-sovelluksen ja CDN served sisältöä työnkulun yhdistetty käyttöönotto
- Integroi ASP.NET niputus ja minification Azure CDN

## <a name="what-you-will-learn"></a>Mitä opit ##

Tässä opetusohjelmassa kerrotaan, miten voit:

-   [Azure CDN päätepisteen integrointi pilvipalvelussa sijaitsevaan ja yhteyshenkilönä staattiseksi sisällöksi Azure CDN-Web-sivuille](#deploy)
-   [Oman pilvipalvelussa staattiseksi sisällöksi välimuistin asetusten määrittäminen](#caching)
-   [Yhteyshenkilönä sisällön ohjauskoneen toiminnot Azure CDN kautta](#controller)
-   [Toimii yhteen ja minified sisällön Azure CDN säilyttämällä komentosarjojen virheenkorjaus-ratkaisun Visual Studiossa](#bundling)
-   [Määritä varmistusta komentosarjojen ja CSS Azure CDN ollessa offline-tilassa](#fallback)

## <a name="what-you-will-build"></a>Mitä muodostetaan ##

Otetaan käyttöön käyttämällä oletusarvoista ASP.NET MVC mallin cloud palvelun Web rooli, lisää koodi tukemaan sisältöä integroitu Azure-CDN, kuten kuva, ohjauskoneen toiminnon tulokset ja, oletusarvo JavaScript ja CSS-tiedostot ja koodin määrittäminen varakyselyjen järjestelmä nippujen served silloin, kun CDN on offline-tilassa, myös kirjoittaminen.

## <a name="what-you-will-need"></a>Tarvittavat komponentit ##

Tässä opetusohjelmassa on seuraavat edellytykset:

-   Aktiivinen [Microsoft Azure-tiliin](/account/)
-   Visual Studio 2015 [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) kanssa

> [AZURE.NOTE] Sinun on suoritettava tässä opetusohjelmassa Azure tilin:
> + Voit [avata Azure-tili maksutta](/pricing/free-trial/) – hyvitykset Hae avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään enintään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten sivustot.
> + Voit [aktivoida MSDN tilaajan edut](/pricing/member-offers/msdn-benefits-details/) - Your MSDN-tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Ottaa käyttöön pilvipalveluun ##

Tässä osassa otetaan käyttöön oletusarvoinen ASP.NET MVC sovelluksen mallissa Visual Studio 2015 cloud palvelun Web-roolin ja integroida uusi CDN päätepiste. Noudata seuraavia ohjeita:

1. Visual Studio 2015, Luo uusi Azure pilvipalvelussa valikkoriviltä siirtymällä **Tiedosto > Uusi > Projekti > Cloud > Azure pilvipalvelussa**. Anna sille nimi ja valitse **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. **ASP.NET Web rooli** ja valitse **>** painiketta. Valitse OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. **MVC** ja valitse **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Julkaise verkko-roolin nyt Azure pilvipalveluun. Cloud palvelun projektin hiiren kakkospainikkeella ja valitse **Julkaise**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Jos et ole vielä kirjautunut tuominen Microsoft Azure-Valitse **Lisää tili...** -valikko ja valitse **Lisää tili** -valikkovaihtoehto.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Kirjaudu sisään-sivulla Kirjaudu sisään Microsoft-tiliä, voit käyttää Azure tilin aktivoimiseen.
7. Kun olet kirjautunut sisään, valitse **Seuraava**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Oletetaan, että et ole luonut cloud palvelun tai tallennustilan tilin, Visual Studio avulla voit luoda molemmat. **Luo pilvipalvelussa ja tili** -valintaikkunassa haluamasi palvelun nimi ja valitse haluamasi alue. Valitse **Luo**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Julkaise asetukset-sivun Tarkista määritykset ja valitse **Julkaise**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Julkaisuprosessin pilvipalveluihin kestää kauan. Ota käyttöön Web Ota käyttöön kaikki roolit-vaihtoehdon voit tehdä virheenkorjaus paljon nopeammin että pilvipalvelussa antamalla Web-roolien nopea (mutta väliaikainen) päivitykset. Lisätietoja tämä asetus on artikkelissa [Azure-työkaluilla pilvipalveluun julkaiseminen](http://msdn.microsoft.com/library/ff683672.aspx).

    **Microsoft Azure tehtävän loki** näkyy, että julkaiseminen tila on **Valmis**, kun luot CDN päätepiste, joka on integroitu tämän pilvipalvelussa.

    >[AZURE.WARNING] Jos julkaisemisen jälkeen ja otettu käyttöön pilvipalvelussa sanoma tulee näkyviin, sitä todennäköisesti, koska olet ottanut käyttöön pilvipalvelussa käytetään [Vieras OS, joka ei sisällä .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Voit ratkaista ongelman ottamalla [.NET 4.5.2 käynnistys-tehtäväksi](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Luo uusi profiili CDN

CDN profiili on kokoelma CDN päätepisteet.  Kunkin profiilin sisältää vähintään yhden CDN päätepisteet.  Haluat ehkä käyttää profiileja järjestämiseen CDN-päätepisteet internet-toimialueen, web-sovelluksen tai muun kriteerin perusteella.

> [AZURE.TIP] Jos sinulla on jo CDN-profiili, jota haluat käyttää tässä opetusohjelmassa, siirry [Luo uusi CDN päätepiste](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Luo uusi CDN päätepiste

**Luo uusi CDN päätepiste tallennustilan tilin**

1. Siirry [Azure hallinta-portaalin](https://portal.azure.com)CDN profiilin.  Voit ehkä on kiinnitetty sitä Raporttinäkymät-ikkunan edellisessä vaiheessa.  Jos sinulla ei ole, löydät sen valitsemalla **Selaa**ja valitse **CDN-profiileista**ja valitsemalla haluat lisätä oman päätepiste profiilissa.

    CDN profiili-sivu tulee näkyviin.

    ![CDN profiili][cdn-profile-settings]

2. Napsauta **Lisää päätepisteen** -painiketta.

    ![Lisää päätepisteen-painike][cdn-new-endpoint-button]

    **Lisää päätepisteen** -sivu tulee näkyviin.

    ![Lisää päätepisteen sivu][cdn-add-endpoint]

3. Anna tämän CDN päätepisteen **nimi** .  Tätä nimeä käytetään käyttää välimuistissa resurssien etsiminen toimialueen `<EndpointName>.azureedge.net`.

4. Valitse **Origin tyyppi** avattavasta valikosta *pilvipalvelussa*.  

5. Valitse pilvipalvelussa sijaitsevaan **Origin hostname** avattavasta valikosta.

6. Jätä oletusarvot **Origin polku**, **Origin toimialuenimi**ja **protokolla/Origin portin**.  Sinun on määritettävä vähintään yksi protokolla (HTTP tai HTTPS).

7. Luo uusi päätepiste valitsemalla **Lisää** .

8. Kun päätepiste on luotu, se näkyy päätepisteet profiilin luettelo. Luettelonäkymä näyttää käyttämään välimuistissa olevaa sisältöä sekä origin toimialueen URL-osoite.

    ![CDN päätepiste][cdn-endpoint-success]

    > [AZURE.NOTE] Päätepisteen heti eivät voi käyttää.  Voi kestää jopa 90 minuuttia rekisteröinnin leviäminen CDN verkon kautta. Käyttäjä yrittää käyttää CDN toimialuenimen heti saattaa tulla tilakoodin 404, kunnes sisältö on käytettävissä CDN kautta.

## <a name="test-the-cdn-endpoint"></a>Testaa CDN päätepiste

Kun julkaisemisen tila on **Valmis**, Avaa selain ja siirry * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. Omien asetusten tätä URL-osoite on:

    http://camservice.azureedge.net/Content/bootstrap.css

Joka vastaa CDN päätepisteen osoitteessa seuraava origin URL-osoite:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Kun siirryt * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, sen mukaan, selaimella, näyttöön tulee kehotus ladata tai avata bootstrap.css, joka oli julkaistu verkkosovelluksen.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Voit käyttää vastaavasti Julkinen URL-Osoitteen etsiminen * *http://*&lt;palvelun nimi >*.cloudapp.net/** suoraan CDN päätepiste. Esimerkki:

-   Script polusta .js-tiedosto
-   Mitä tahansa /Content sisällön tiedoston polku
-   Ohjaimen tai toiminnon
-   Jos kyselymerkkijonon on käytössä CDN päätepisteen, minkä tahansa kyselyn merkkijonojen URL-osoite

Itse asiassa yllä kokoonpanoa, voit isännöidä koko pilvipalvelussa- * *http://*&lt;cdnName >*.azureedge.net/**. Jos voin siirtyä **http://camservice.azureedge.net/ ** saan toiminnon tulos aloitus-ja indeksistä.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Tämä ei tarkoita, kuitenkin, että se on aina hyvä (tai yleensä hyvä) tukemaan Azure CDN läpi koko pilvipalvelussa. Joitakin huomautukset ovat seuraavat:

-   Tämän menetelmän edellyttää, että koko sivuston olevan julkinen, koska Azure CDN ei voi käyttää minkä tahansa arkaluontoista sisältöä tällä hetkellä.
-   Jos CDN päätepisteen menee offline-tilassa jostakin syystä, onko suunniteltu ylläpito- tai käyttäjän virhe koko pilvipalvelussa menee offline-tilaan paitsi asiakkaat voivat ohjataan origin URL-Osoitteen * *http://*&lt;palvelun nimi >*.cloudapp.net/**.
-   Vaikka ja mukautetut välimuisti-komponenttien asetukset (katso [määrittäminen välimuistiasetukset staattiset tiedostot oman pilvipalvelussa](#caching)) CDN päätepisteen ei paranna suorituskykyä erittäin dynaamisen sisällön. Jos yritit ladata kotisivun CDN päätepisteen kuin edellä, Huomaa, että kului vähintään viisi sekuntia ladata oletuskotisivu ensimmäistä kertaa, joka on melko yksinkertainen sivu. Kuvitellaan, mitä käydä asiakas-ratkaisun Jos sivu sisältää dynaamisen sisällön, joka on päivitettävä minuutin välein. Lyhyt välimuistin vanheneminen, joka vastaa usein käytetyt välimuistin epäonnistumisten CDN päätepisteen osoitteessa käsittelevä CDN päätepisteen dynaamisen sisällön edellyttää. Tämä hurts pilvipalvelussa sijaitsevaan suorituskykyä ja defeats CDN käyttötarkoituksen.

Vaihtoehtona on selvittää, mitä sisältöä tukemaan Azure CDN-että pilvipalvelussa tapaus-välein. Tämän vuoksi tullut jo käyttämisestä sisällön lomaketiedostot CDN päätepisteestä. Voin kerrotaan, kuinka tukemaan tietyn ohjauskoneen-toiminto läpi CDN päätepisteen- [yhteyshenkilönä sisällön ohjauskoneen toiminnot Azure CDN kautta](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Välimuistiin asetusten staattiset tiedostot pilvipalvelussa sijaitsevaan ##

Oman pilvipalvelussa integrointi Azure CDN voit määrittää miten välimuistiin CDN päätepisteen staattiseksi sisällöksi. Voit tehdä tämän Avaa *Web.config* Web roolin projektin (kuten WebRole1) ja lisää `<staticContent>` elementin `<system.webServer>`. Alla olevassa XML määrittää välimuistin voimassa kolme päivää.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Kun teet näin, kaikki staattisen tiedostot oman pilvipalvelussa tarkkailla CDN välimuistin saman sääntö. Välimuistiasetukset eritellympiä hallintaan Lisää *Web.config* -tiedosto kansioon ja asetuksia siellä. Esimerkiksi *Web.config* -tiedoston lisääminen *\Content* -kansioon ja sisällön korvaaminen seuraavat XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Tämä asetus valitaan, kaikki staattiset tiedostot välimuistiin 15 päivää *\Content* -kansiosta.

Lisätietoja siitä, miten voit määrittää `<clientCache>` elementti, katso [välimuistiin &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

Valitse [yhteyshenkilönä sisällön ohjauskoneen toiminnot Azure CDN kautta](#controller)voin näkyvät myös siitä, miten voit määrittää ohjauskoneen toiminnon tulosten välimuistiasetukset CDN välimuistin.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Yhteyshenkilönä sisällön ohjauskoneen toiminnot Azure CDN kautta ##

Kun cloud palvelun Web-rooli integroida Azure CDN on suhteellisen helppoa tukemaan Azure CDN kautta ohjauskoneen toiminnot-sisältöä. Kuin käsittelevä oman pilvipalvelussa suoraan Azure CDN (osoittaa yläpuolella) [Maarten Balliauw](https://twitter.com/maartenballiauw) , kuinka se tehdään hauska MemeGenerator ohjauskoneen [pelkistäviä viive Azure CDN kanssa verkossa](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Voin yksinkertaisesti toistamaan sen tähän.

Oletetaan, että pilvipalvelussa haluat luoda memes perusteella nuorten Chuck Norris kuvan (valokuvan [Jari](http://www.flickr.com/photos/alan-light/218493788/)valo) seuraavasti:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Sinulla on yksinkertainen `Index` toiminto, jonka avulla voit määrittää superlatiivin kuvan asiakkaat Luo meme sitten, kun ne julkaiseminen toiminnon. Koska Chuck Norris, todennäköisesti odottanut tällä sivulla voit liittyä ole Suositut yleisesti. Tämä on hyvä esimerkki käsittelevä osittain dynaamisen sisällön Azure CDN kanssa.

Noudata edellä esiteltyjä ohjauskoneen toiminto: n määrittäminen:

1. *\Controllers* -kansioon Luo uusi .cs tiedosto nimeltä *MemeGeneratorController.cs* ja korvaa sisällön seuraava koodi. Muista korvata korostetun osan CDN nimi.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Kaksoisnapsauta oletusarvo `Index()` toiminto ja valitse **Lisää näkymä**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Hyväksy alla asetukset ja sitten **Lisää**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Avaa uusi *Views\MemeGenerator\Index.cshtml* ja sisällön korvaaminen seuraavat yksinkertainen HTML superlatiivin lähettämistä varten:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Julkaise pilvipalvelussa uudelleen ja siirry * *http://*&lt;palvelun nimi >*.cloudapp.net/MemeGenerator/Index** selaimessa.

Kun lähetät lomakkeen arvot kohteeseen `/MemeGenerator/Index`, `Index_Post` toiminto menetelmä palauttaa linkki `Show` toiminto menetelmä vastaaviin syötteen tunnuksessa on virhe. Kun olet napsauttanut linkkiä, pääset seuraava koodi:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Jos paikallisen virheenkorjaus on liitetty, näkyviin tulee paikallisen uudelleenohjaus Normaali virheenkorjaus kokemusta. Jos se on käynnissä pilvipalvelussa, valitse se ohjaa:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Joka vastaa CDN päätepisteen osoitteessa seuraava origin URL-osoite:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Voit käyttää `OutputCacheAttribute` määrite `Generate` menetelmää voit määrittää, miten toiminnon tulos välimuistiin, joka tukee Azure CDN. Seuraava koodi Määritä 1 tunti (3 600 sekuntia) välimuistin voimassaolon.

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Vastaavasti voit voi toimia sisällön ohjauskoneen toiminnon määrittäminen oman pilvipalvelussa kautta Azure-CDN välimuistiin haluamasi vaihtoehto.

Seuraavan osion voin kerrotaan, kuinka tukemaan Azure CDN – CSS ja yhdistettyjä ja minified komentosarjoja.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Integroi ASP.NET niputus ja minification Azure CDN ##

Komentosarjojen ja CSS-tyylisivut harvoin muuttuvat, ja niitä kaiken lähdetietoina Azure CDN välimuistin. Helpoin tapa niputus ja minification integroida Azure CDN käsittelevä Azure CDN läpi koko Web-rooli on. Kuitenkin kuin ehkä halua tehdä tämän, voin kerrotaan, kuinka voit tehdä sen, kun säilyttäminen haluamasi develper kokemus ASP.NET niputus ja minification, kuten:

-   Hyvien virheenkorjaus tila-toiminto
-   Virtaviivaistettu käyttöönotto
-   Komentosarjan/CSS-versiopäivitykset työasemaohjelmista heti päivitykset
-   Kun CDN päätepisteen epäonnistuu varakyselyjen järjestelmä
-   Pienennä koodin muokkaaminen

Avaa **WebRole1** projekti, jonka loit [liittää Azure CDN päätepisteen Azure verkkosivusto- ja toimii staattinen sisältölähdettä Azure CDN verkkosivua](#deploy), *App_Start\BundleConfig.cs* ja tutustu `bundles.Add()` menetelmäkutsujen.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Ensimmäinen `bundles.Add()` lauseen Lisää komentosarja-pikaoppaista näennäiskansio `~/bundles/jquery`. Avaa *Views\Shared\_Layout.cshtml* nähdäksesi, kuinka komentosarjan pikaoppaista tunnisteen muodostetaan. Sinun pitäisi Etsi seuraava rivi Razor koodi:

    @Scripts.Render("~/bundles/jquery")

Kun Razor koodi suoritetaan Azure-Web-rooli, se on mahdollista `<script>` seuraavanlainen komentosarja-pikaoppaista-tunniste:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Kun se suoritetaan Visual Studiossa kirjoittamalla `F5`, se hahmonnetaan kunkin komentosarjatiedosto olevien erikseen (yllä tapauksessa vain yksi komentosarjatiedosto on töihin):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Näin voit virheenkorjaus oman kehitysympäristö JavaScript-koodia pienentämisestä samanaikainen asiakasyhteydet (Niputustoiminnon) ja parantaa tiedoston lataamisen tuotannon suorituskyvyn (minification). Se on hyvä ominaisuus säilyttää Azure CDN integrointi. Lisäksi hahmontamisen töihin on jo automaattisesti luotu versiomerkkijono, koska haluat toistaa vastaavia toimintoja jotta aina, kun päivität jQuery-versiosi NuGet kautta, se voidaan päivittää osoitteessa asiakaspuolen mahdollisimman pian.

Noudata alla olevia ohjeita integrointi ASP.NET niputus ja minification CDN päätepisteen kanssa.

1. Takaisin sisään *App_Start\BundleConfig.cs*, muokata `bundles.Add()` tapoja eri [Pikaoppaista konstruktori](http://msdn.microsoft.com/library/jj646464.aspx), sellainen, joka määrittää CDN-osoitteen. Voit tehdä tämän korvaa `RegisterBundles` menetelmä määritelmä seuraava koodi:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Varmista, että korvaa `<yourCDNName>` Azure CDN nimellä.

    Tavallinen kirjaimin olet määrittämässä `bundles.UseCdn = true` ja huolellisesti CDN URL-osoite lisätään jokaiselle pikaoppaista. Esimerkki: ensimmäisen konstruktoria koodissa:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    on sama kuin:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Tämä konstruktori kertoo ASP.NET niputus ja minification hahmonnetaan yksittäisen komentosarjatiedostot aloittaessasi paikallisesti, mutta määritetty CDN osoite käyttämään kyseistä komentosarja. Huomaa kuitenkin kaksi tärkeitä ominaisuuksia ja tämän huolellisesti CDN URL-osoite:

    -   Alkuperäinen tämä CDN URL-osoite on `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, joka on todella näennäiskansio komentosarjan töihin oman pilvipalvelussa.
    -   Käytät CDN konstruktori, koska töihin CDN komentosarja-tunniste ei enää sisällä hahmontamisen URL-Osoitteen automaattisesti luotu versio merkkijonon. Sinun on luotava manuaalisesti yksilöllinen versiomerkkijono aina, kun komentosarja-pikaoppaista on muokattu pakottaminen osoitteessa Azure CDN epäonnistumisten. Yksilöivä versiomerkkijono on säilyvät muuttumattomina kautta, kun otat käyttöön Suurenna osumia osoitteessa Azure CDN töihin käyttöönoton elinkaaren yhtä aikaa.
    -   Kyselymerkkijonon v = < W.X.Y.Z > vedetään *Properties\AssemblyInfo.cs* -Web-roolin projektisi. Voit määrittää käyttöönoton työnkulun, joka sisältää kasvavat Kokoonpanoversio aina, kun julkaiset Azure. Tai voit muokata *Properties\AssemblyInfo.cs* vain projektin kasvavan versiomerkkijono automaattisesti aina, kun luot-käyttämällä yleismerkki "*". Esimerkki:

            [assembly: AssemblyVersion("1.0.0.*")]

        Muut strategia voi tehostaa luodaan yksilöllinen merkkijono elinkaaren käyttöönoton ajaksi toimii tähän.

3. Julkaise pilvipalvelussa ja käyttää aloitussivulle.

4. Näytä sivun HTML-koodi. Sinun pitäisi nähdä hahmontaa yksilöllinen versio merkkijonolla aina, kun julkaiset pilvipalveluun, tekemäsi muutokset CDN URL-Osoitteen. Esimerkki:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Visual Studiossa virheenkorjaus Visual Studiossa pilvipalvelussa kirjoittamalla `F5`.,

6. Näytä sivun HTML-koodi. Näet kunkin komentosarjatiedosto hahmontaa yksitellen niin, että sinulla on yhtenäinen virheenkorjaus, ilmenee Visual Studiossa.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Varakyselyjen järjestelmä CDN URL-osoitteet ##

Azure CDN päätepisteen epäonnistuu jostakin syystä, kun haluat verkkosivulle on automaattinen käyttämään origin verkkopalvelin lataamisen JavaScript- tai automaattinen varakyselyjen-asetus. On vakavia menetät kuvia verkkosivuston CDN ole käytettävissä, mutta paljon vakavia menetät tärkeitä sivun toimintoja komentosarjojen ja tyylisivut vuoksi.

[Pikaoppaista](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) -luokka sisältää ominaisuuden, jonka avulla voit määrittää varakyselyjen järjestelmä CDN epäonnistumiseen [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) . Jos haluat käyttää tätä ominaisuutta, noudata seuraavia ohjeita:

1. Web-roolin projekti Avaa *App_Start\BundleConfig.cs*, johon lisäsit CDN URL-osoite kunkin [Pikaoppaista konstruktori](http://msdn.microsoft.com/library/jj646464.aspx), ja tee varakyselyjen järjestelmä lisääminen oletusarvon nippujen korostetun seuraavat muutokset:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Kun `CdnFallbackExpression` on ei null-komentosarjan syötetään HTML: Testaa, onko töihin on ladattu, ja jos ei käsitellä töihin suoraan origin verkkopalvelin. Tämä ominaisuus on määritettävä JavaScript-lauseke, joka testaa, onko vastaaviin CDN töihin on ladattu oikein. Tarvittavat kunkin pikaoppaista Testaa lausekkeen eroaa sisällön mukaan. Yllä oletusarvon nippujen: varten

    -   `window.jquery`määritetään jquery-{versio} .js
    -   `$.validator`määritetään jquery.validate.js
    -   `window.Modernizr`määritetään modernizer-{versio} .js
    -   `$.fn.modal`määritetään bootstrap.js

    Olet ehkä jo huomannut, että minulla ei ole määrittänyt CdnFallbackExpression varten `~/Cointent/css` pikaoppaista. Tämä johtuu siitä, että käytössä on [System.Web.Optimization ohjelmavirhe](https://aspnetoptimization.codeplex.com/workitem/104) , injects `<script>` sijaan odotettua varakyselyjen CSS-tunniste `<link>` tunniste.

    Ei, mutta [Ember Consulting ryhmän](https://github.com/EmberConsultingGroup)tarjoamia hyvä [Tyylin Pikaoppaista varmistusta](https://github.com/EmberConsultingGroup/StyleBundleFallback) .

2. Käytä vaihtoehtoista menetelmää CSS, .cs uuden tiedoston luominen Web roolin projektin *App_Start* kansioon *StyleBundleExtensions.cs*ja korvata sen sisällön [GitHub koodilla](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. Valitse *App_Start\StyleFundleExtensions.cs*nimeä nimitilan Web-roolin nimi (esimerkiksi **WebRole1**).

3. Palaa `App_Start\BundleConfig.cs` ja muokata viimeisin `bundles.Add` lauseen kanssa korostetun seuraava koodi:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Menetelmä tunniste käyttää samaa verrata lisätäkseen komentosarjan HTML-muodossa, voit tarkistaa, DOM vastaavaa luokan nimeä, säännön nimi ja määrittää CSS-pikaoppaista ja takaisin origin verkkopalvelin sisältö kuuluu, jos se epäonnistuu vastaavan säännön arvo.

4. Julkaise pilvipalvelussa uudelleen ja käyttää aloitussivulle.

5. Näytä sivun HTML-koodi. Lisättyä komentosarjojen tulisi löytää seuraavankaltaiselta:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Huomaa, että lisättyä komentosarjan CSS-pikaoppaista sisältää edelleen automaatiokoodia jääneeseen kohteesta `CdnFallbackExpression` rivin ominaisuutta:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Mutta koska ensimmäinen osa || lauseke palauttaa aina true (suoraan yllä olevasta allekirjoitusrivillä), document.write()-funktio suoritetaan aina.

## <a name="more-information"></a>Lisätietoja ##
- [Yleistä Azure sisällön toimittamisen verkon (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Azure CDN käyttäminen](cdn-create-new-endpoint.md)
- [ASP.NET niputus ja Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
