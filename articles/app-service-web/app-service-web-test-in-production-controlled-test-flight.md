<properties
    pageTitle="Flighting (Betatestaus) Azure-sovelluksen palvelun käyttöönotto"
    description="Opettele flight sovelluksen uusista ominaisuuksista tai beetatestaajien testata Päivityksesi lopusta loppuun tässä opetusohjelmassa. Yhdistää asioiden App Service-ominaisuuksia, kuten jatkuva julkaiseminen, paikkaa, liikenne reititys ja sovelluksen tiedot-integrointi."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting (Betatestaus) Azure-sovelluksen palvelun käyttöönotto

Tässä opetusohjelmassa näytetään, miten voit tehdä *flighting ominaisuuksissa* integroimalla [Azure App palvelu](http://go.microsoft.com/fwlink/?LinkId=529714) ja [Azure hakemuksen tiedot](/services/application-insights/)eri ominaisuuksia. 

*Flighting* on käyttöönottoprosessin, joka vahvistaa uusi ominaisuus tai muuttaa todellisia asiakkaiden rajoitettu määrä ja jonkin tärkeän Testaa tuotannon skenaariossa. Se muistuttaa beta testaus ja kutsutaan joskus "valvottu testi lento". Monta suurille yrityksille web-sivuston kanssa tätä tapaa kannattaa käyttää aikainen vahvistus käyttämiseksi niiden käytännössä [Joustava](https://en.wikipedia.org/wiki/Agile_software_development)kehittämisen app päivitykset. Azure sovelluksen-palvelun avulla voit testi tuotannon integroida jatkuva julkaiseminen ja toteuttaa saman DevOps skenaarion sovelluksen tiedot. Tämän menetelmän etuja ovat:

- **Todellinen palautetta _ennen_ päivitykset julkaistaan tuotannon voitto** - parempi kuin kasvun palautetta, kun vapautat riittää on kasvun palautetta, ennen kuin vapautat. Voit testata todellinen käyttäjä liikenteen ja toimintojen päivityksiä alkuvaiheessa tuotteen elinkaaren Microsoftiin.
- **Parantaa [Jatkuva testi perustuva kehittäminen (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** - testi tuotannon integroimalla jatkuva integrointi ja instrumentation hakemuksen tiedot, joiden kanssa käyttäjän vahvistus tapahtuu alkuvaiheessa ja automaattisesti tuotteen elinkaaren. Tämä vähentää ajan sijoitukset manuaalinen testin suorittaminen.
- **Optimoi työnkulun testaaminen** - automatisoimalla jatkuva seuranta instrumentation, jonka testi mahdollisesti suoritettavat erityyppisten yksittäisen prosessi, kuten [integrointi](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [käytettävyyttä](https://en.wikipedia.org/wiki/Usability_testing), Helppokäyttötoiminnot, sovellus on lokalisoitu, [suorituskyvyn](https://en.wikipedia.org/wiki/Software_performance_testing), [Suojaus](https://en.wikipedia.org/wiki/Security_testing)ja [hyväksymisen](https://en.wikipedia.org/wiki/Acceptance_testing)testit tavoitteisiin.

Flighting käyttöönoton ei lähes reititys on live liikenne. Näiden käyttöönoton haluat Hanki tietoja mahdollisimman pian onko olisi Odottamaton virhe, kansiohierarkiassa, käyttäjän kokemus ongelmat. Muista, että käsiteltävä reaali asiakkaille. Näin voit tehdä sen oikea-ja sinun on tehtävä Varmista, että flighting käyttöönoton on määritetty kerää kaikki tiedot, jotta voit tehdä päätöksen, seuraava vaihe on. Tässä opetusohjelmassa näytetään, miten voit kerätä tietoja hakemuksen tiedot, mutta voit käyttää uuden Relic tai muita tekniikoita, joka sopii käyttämässäsi skenaariossa. 

## <a name="what-you-will-do"></a>Mitä hyötyä

Tässä opetusohjelmassa kerrotaan, miten voit tuoda yhteen, kun haluat esikatsella App palvelun sovelluksen tuotannon seuraavissa tilanteissa:

- [Reitin tuotannon liikenne](app-service-web-test-in-production-get-start.md) beeta-sovellukseen
- [Välineen sovelluksen](../application-insights/app-insights-web-track-usage.md) hyödyllisiä arvot hankkiminen
- Jatkuvasti beeta-sovelluksen käyttöönotto ja seurata live app arvot
- Vertaa arvot tuotannon-sovellus ja beeta-sovelluksen, miten muutokset kääntää tulokset välillä

## <a name="what-you-will-need"></a>Tarvittavat komponentit

-   Azure-tili
-   [GitHub](https://github.com/) -tili
- Visual Studio 2015 – voit ladata [yhteisön edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Git Shell (asennettu [GitHub for Windowsin](https://windows.github.com/)kanssa) – tämän avulla voit suorittaa saman istunnon Git ja PowerShell-komennot
-   Uusimman [PowerShellin Azure](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bittien
-   Tavallinen tietoja seuraavasti:
    -   [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) mallin käyttöönottoa (katso [Ota KOMPLEKSI sovelluksen laadukkaampi Azure](app-service-deploy-complex-application-predictably.md))
    -   [Git](http://git-scm.com/documentation)
    -   [PowerShellin](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Sinun on suoritettava tässä opetusohjelmassa Azure tilin:
> + Voit [avata Azure-tili maksutta](/pricing/free-trial/) – hyvitykset Hae avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten Web Apps.
> + Voit [aktivoida Visual Studio tilaajan edut](/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.
>
> Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="set-up-your-production-web-app"></a>Tuotannon web-sovelluksen määrittäminen

>[AZURE.NOTE] Tässä opetusohjelmassa käytetty komentosarja määritetään automaattisesti jatkuva julkaiseminen GitHub-säilöstä. Tämä edellyttää, että GitHub tunnistetiedot on jo tallennettu Azure, muuten komentosarjalla toteutettavan käyttöönoton epäonnistuu yritettäessä web Apps-sovellusten tietolähteen ohjausobjektin asetusten määrittäminen.
>
>Azure GitHub tunnistetietojen tallentamiseen Luo verkkosovellukseen [Azure-portaalin](https://portal.azure.com/) ja [määrittää GitHub-yhdistelmäympäristössä](app-service-continuous-deployment.md#Step7). Tarvitset vain vain yhden kerran.

Tyypillinen DevOps skenaariossa sovellus, joka toimii live Azure-tietokannassa on ja haluat tehdä siihen muutoksia jatkuva julkaiseminen kautta. Tässä skenaariossa otetaan käyttöön tuotannon malli, joka on kehitetty ja testattu.

1.  Voit luoda oman rinnakkainen [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tietovaraston. Lisätietoja oman rinnakkainen luomisesta on artikkelissa [Rinnakkainen Repo](https://help.github.com/articles/fork-a-repo/). Oman rinnakkainen luodaan, kun näet sen selaimessa.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Avaa Git Shell-istunto. Jos sinulla ei ole vielä Git Shell, asenna [GitHub Windows](https://windows.github.com/) .
3.  Luo paikalliseen Kloonaa, että rinnakkainen suorittamalla seuraavan komennon:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Kun paikallinen Kloonaa, siirry * &lt;repository_root >*\ARMTemplates ja suorita deploy.ps1 komentosarjan yksilöllinen jälkiliitteellä alla kuvatulla tavalla:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Kun sinulta kysytään, kirjoita haluamasi käyttäjänimi ja salasana tietokannan käytön. Muista tietokannan tunnistetietoja, koska haluat määrittää ne uudelleen, kun päivität resurssiryhmän.

    Raportissa pitäisi näkyä eri Azure resurssien valmistelu etenemisen. Kun käyttöönotto on valmis, komentosarja Käynnistä sovellus selaimessa ja antaa sinulle helpossa muodossa äänimerkki.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Takaisin Git Shell-istunnossa suorittaa:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Kun komentosarja on valmis, palaa Selaa edusta-osoite (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) nähdäksesi tuotannon sovelluksen.
5.  Kirjaudu sisään [Azure-portaaliin](https://portal.azure.com/) ja tutustu mitä luodaan.

    Sinun pitäisi nähdä saman resurssiryhmä, jossa kaksi verkkosovelluksissa `Api` nimi-liitettä. Jos tarkastelet ryhmän resurssinäkymään, myös näet SQL-tietokantaan ja palvelimen ja sovelluksen palvelusopimus väliaikaisen paikkojen web Apps-sovellukset. Selata eri resursseja ja vertaa niitä * &lt;repository_root >*\ARMTemplates\ProdAndStage.json näet, miten ne määritetään mallissa.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Olet määrittänyt tuotannon-sovellus.  Nyt oletetaan, että voit saada palautetta niistä käytettävyyttä on heikko sovelluksen. Jotta voit päättää tutkia. Aiot soittimen palautteen antaminen sovelluksen.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Tutkia: Seuranta/arvot asiakas-sovelluksesi soittimen

5. Avaa * &lt;repository_root >*\src\MultiChannelToDo.sln Visual Studiossa.
6. Palauttaa kaikki Nuget paketit napsauttamalla ratkaisu > **Ratkaisu NuGet pakettien hallinta** > **palauttaminen**.
6. Hiiren kakkospainikkeella **MultiChannelToDo.Web** > **Lisätä hakemuksen tiedot Telemetriatietojen** > **Asetusten määrittäminen** > Muuta resurssiryhmä ToDoApp*&lt;your_suffix >* > **Lisääminen sovelluksen tietoja Project**.
7. Avaa sivu **MultiChannelToDo.Web** sovelluksen tietoja resurssin Azure-portaalissa. Valitse **sovelluksen kunto** -osassa **Opettele selaimen sivun kuormituksen tietojen kerääminen** > Kopioi koodi.
7. Lisää kopioidut JS instrumentation koodia * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml juuri sulkeminen ennen `<heading>` tunniste. Se on sisällettävä sovelluksen tietoja resurssin yksilöllinen instrumentation-näppäintä.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Lähetä mukautetut tapahtumat sovelluksen havainnollistamisen hiiren napsautuksella lisäämällä leipätekstin alareunassa seuraava koodi:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Tämä käyttöösi JavaScript-katkelma lähettää mukautetun tapahtuman hakemuksen tiedot aina, kun käyttäjä napsauttaa mitä tahansa web App-sovelluksessa.

12. Käyttöliittymän Git Vahvista ja siirtää muutokset GitHub oman rinnakkainen. Odota sitten asiakkaiden Päivitä selain.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Vaihda tuotannon käyttöön app muutokset:

        .\swap –Name ToDoApp<your_suffix>

13. Etsi sovellus havainnollistamisen resurssi, jonka määritit. Valitse mukautetut tapahtumat.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Jos et näe mittaukset mukautetut tapahtumat, odota muutama minuutti ja valitse **Päivitä**.

Oletetaan, että näet kaavioon, kuten alla:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

Ja sen alapuolella tapahtuman ruudukko:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

ToDoApp-sovelluksen koodin mukaan **painike** -tapahtuma vastaa Lähetä-painiketta ja **INPUT** -tapahtuman vastaa tekstiruutuun. Tähän mennessä asioita katsoo sen perustelluksi. Kuitenkin vaikuttaa siltä, ettei on hyvä määrän napsautuksia ja hyvin muutamalla napsautuksella Tehtäväkohteita ( **LI** tapahtumien).

Näin voit lomake perustuu oman hypoteesien joillakin käyttäjillä olevat sekoittaa, käyttöliittymän-osa on valittavana ja tavallista kohdistin on tyylejä tekstin valinnan, kun osoitin viedään luettelokohteet ja niiden kuvakkeet.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Tämä voi johtua contrived esimerkki. Aiot kuitenkin tehdä parannus sovelluksen ja suorita sitten flighting käyttöönoton käytettävyyttä palautetta käyttämistä live asiakkaille.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Soittimen seuranta/arvot server-sovelluksesi
Tämä on tangentin, koska osoittaa Tässä opetusohjelmassa skenaarion käsitellään vain asiakas-sovelluksen kanssa. Kuitenkin täydellisiä, määrität palvelinpuolen-sovellus.

6. Hiiren kakkospainikkeella **MultiChannelToDo** > **Lisätä hakemuksen tiedot Telemetriatietojen** > **Asetusten määrittäminen** > Muuta resurssiryhmä ToDoApp*&lt;your_suffix >* > **Lisääminen sovelluksen tietoja Project**.
12. Käyttöliittymän Git Vahvista ja siirtää muutokset GitHub oman rinnakkainen. Odota sitten asiakkaiden Päivitä selain.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Vaihda tuotannon käyttöön app muutokset:

        .\swap –Name ToDoApp<your_suffix>

Joka on tämä!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Tutkia: Lisää paikka-tunnisteet asiakas-sovelluksen arvot

Tässä osassa määritetään eri käyttöönoton paikkojen paikka kielikohtaiset telemetriatietojen lähettäminen samalle resurssille hakemuksen tiedot. Näin voit verrata telemetriatietojen tietojen tietoliikenteen eri paikkojen (käyttöönoton ympäristöissä) voit helposti tarkistaa sovelluksen muutosten vaikutus. Samanaikaisesti voit erottaa tuotannon liikenne muusta, jotta voit jatkaa seurannassa tuotannon sovelluksen tarpeen mukaan.

Koska olet kerännyt tiedot asiakkaan toiminnan, se index.cshtml [Lisää telemetriatietojen-alustaja JavaScript-koodia](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) . Jos haluat testata palvelinpuolen suorituskyvyn, esimerkiksi voit tehdä myös samalla tavalla server-koodisi (katso [Tietoja API Application mukautetut tapahtumat ja arvot](../application-insights/app-insights-api-custom-events-metrics.md).

1. Lisää ensin koodin bewteen kahden `//` kommentit alla JavaScript-koodia estäminen, että olet lisännyt `<heading>` tunnisteen aiemmin.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Alustaja-koodi `appInsights` Lisää objekti mukautetun ominaisuuden nimi `Environment` jokaisen se lähettää telemetriatietojen osaan.

2. Lisää seuraavaksi mukautetun ominaisuuden [paikka asetus](web-sites-staged-publishing.md#AboutConfiguration) Azure-koodiin. Voit tehdä tämän suorittamalla seuraavat komennot Git Shell-istunnossa.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Projektin Web.config määrittää jo `environment` asetus. Kun tämä asetus, kun testaat sovelluksen paikallisesti, että arvot olla merkitty `VS Debugger`. Kun painat muutokset Azure, Azure etsii ja käytä `environment` sovelluksen sijaan web-sovelluksen kokoonpanon määrittäminen ja että mittarit olla merkitty `Production`.

3. Vahvista push koodin muutokset oman haarautumissiirtymä-GitHub ja odota, että uuden sovelluksen (Päivitä selain tarvetta)-käyttäjille. Kestää noin 15 minuuttia uuden ominaisuuden lisääminen sovelluksen tietoja `MultiChannelToDo.Web` resurssi.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Nyt, siirry **Mukautetut tapahtumat** -sivu uudestaan ja suodattaa määritetty `Environment=Production`. Voit nyt pitäisi käyttää näkyviin kaikki uudet mukautetut tapahtumat tuotannon paikka, jossa on suodatin.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Napsauta Tallenna nykyiset arvot Explorer-asetukset, esimerkiksi **Suosikit** -painiketta **Mukautetut tapahtumat: tuotannon**. Voit helposti siirtyä tämän ja käyttöönoton paikka näkymän välillä myöhemmin.

    > [AZURE.TIP] Harkitse jopa tehokkaampi analytics [integrointi sovelluksen havainnollistamisen tehostaminen Power BI resurssin](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Lisää paikka-tunnisteet server-sovelluksen arvot
Uudelleen täydellisiä, määrität palvelinpuolen-sovellus. Toisin kuin asiakkaan sovellus, joka on instrumented JavaScript-paikka-tunnisteet palvelimen sovelluksen instrumented .NET-koodilla.

1. Avaa * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Lisää koodi estä alla juuri ennen sulkeminen nimitilan aaltosulje.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Nimi tarkkuus virheiden korjaaminen lisäämällä `using` lauseet on alle alkuun tiedoston:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Lisää alapuolelle koodi alkuun `Application_Start()` menetelmää:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Vahvista push koodin muutokset oman haarautumissiirtymä-GitHub ja odota, että uuden sovelluksen (Päivitä selain tarvetta)-käyttäjille. Kestää noin 15 minuuttia uuden ominaisuuden lisääminen sovelluksen tietoja `MultiChannelToDo` resurssi.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Päivitys: Määrittäminen beeta-haara

2. Avaa * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json ja Etsi `appsettings` resurssit (Etsi `"name": "appsettings"`). On 4 ne yhteen kunkin paikka. 

2. Kunkin `appsettings` resurssi, Lisää `"environment": "[parameters('slotName')]"` asetus loppuun `properties` matriisi. Älä unohda toisistaan pilkulla edellisen rivin loppuun.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Lisäämäsi `environment` kaikki mallin paikkojen asetus.
    
2. Saman tiedoston, Etsi `slotconfignames` resurssit (Etsi `"name": "slotconfignames"`). On 2 ne, yksittäisistä sovelluksista.

2. Kunkin `slotconfignames` resurssi, Lisää `"environment"` loppuun `appSettingNames` matriisi. Älä unohda toisistaan pilkulla edellisen rivin loppuun.

    Olet tehnyt `environment` sauvaa asettaminen sen vastaaviin käyttöönoton paikka sekä sovellukset app.  

3. Suorita seuraavat komennot saman resurssin ryhmän liitteen, jota käytit ennen Git Shell-istunnossa.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Kun sinulta kysytään, Määritä SQL tietokanta tunnistetietoja kuin ennen. Kun sinua pyydetään päivittämään resurssiryhmän, kirjoita sitten `Y`, valitse `ENTER`.

    Kun komentosarja on valmis, kaikkien resurssien alkuperäisen resurssiryhmän säilytetään, mutta uusi nimeltä "beta" luodaan se määrityksillä nimellä "Väliaikaisen" paikka, joka on luotu alkuun.

    >[AZURE.NOTE] Tämä menetelmä käyttöönoton eri ympäristöissä poikkeaa [Joustava software development Azure App palvelussa](app-service-agile-software-development.md)menetelmää. Tässä kohdassa voit luoda käyttöönoton ympäristöissä, käyttöönoton paikkaa, jossa muotoilussa luoda käyttöönoton ympäristöissä resurssin ryhmiin. Hallinta käyttöönoton ympäristöissä resurssin ryhmiin avulla voit pitää tuotantoympäristössä kehittäjät pääse muokkaamaan, mutta se ei ole helppoa testaaminen tuotannon, jotka voit tehdä helposti paikkaa.

Halutessasi voit myös luoda alfa sovelluksen suorittamalla

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Tässä opetusohjelmassa, olet juuri edelleen käyttää beeta-sovellus.

## <a name="update-push-your-updates-to-the-beta-app"></a>Päivitys: Push Päivityksesi beeta-sovellukseen

Palaa sovelluksen, jonka haluat parantaa.

1. Varmista, että sivu on nyt beta-haara

        git checkout beta

2. - * &lt;Repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, Etsi `<li>` tunnisteen ja lisää `style="cursor:pointer"` määrite valitsemalla alla kuvatulla tavalla.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Vahvista ja push Azure.

4. Varmista, että on nyt muutos beeta-paikka siirtymällä http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Jos muutos ei näy vielä, Päivitä selain saadaksesi uuden javascript-koodia.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nyt kun olet luonut beeta-paikka käynnissä muutoksen, olet valmis suorittamaan flighting käyttöönotto.

## <a name="validate-route-traffic-to-the-beta-app"></a>Vahvista: Reitin liikenne beeta-sovellukseen

Tässä osassa reitittää liikenteen beeta-sovellukseen. Esittelyn tarkkuuden, for sake of aiot reitittää käyttäjän liikenne merkittäviä osa. Todellisuudessa haluat reitittää liikenteen määrä riippuu tietyn tilannettasi. Esimerkiksi jos sivusto on osoitteessa microsoft.com asteikon, valitse joudut ehkä alle yhden prosentin yhteensä liikenne saadakseen hyödyllisiä tietoja.

1. Git Shell-istunnossa suorittamalla seuraavat komennot puolet tuotannon liikenne reitittämiseen beeta-paikka:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  `ReroutePercentage=50` Ominaisuus määrittää, että 50 prosenttia tuotannon liikenne reititetään beeta-sovelluksen URL-osoite (määrittämän `ActionHostName` ominaisuutta).

2. Nyt siirtyä http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50 prosenttia liikenne uudelleenohjataan nyt beeta-paikka.

3. Suodatin arvot sovelluksen tiedot-resurssi, ympäristön = "beta".

    > [AZURE.NOTE] Jos tallennat tämän suodatettua näkymää toiseen suosikiksi, valitse voit helposti kääntää metrisillä explorer-näkymiä tuotannon ja beeta-näkymien välillä.

Oletetaan, että sovelluksen tiedot-kohdassa jotain seuraavankaltaiselta:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Ei vain tämä näkyy, että Valitse on monta napsautuksella `<li>` tunnisteita, mutta on ylijännitesuojan napsautusten `<li>` tunnisteet. Voit sitten päätellä henkilöiden on löytää uuden `<li>` tunnisteet on valittavana ja ovat nyt poistamalla kaikki aiemmin suoritettu projektitehtävien-sovelluksessa.

Flighting käyttöönoton tietojen perusteella voit päättää, että uusi Käyttöliittymä on valmis tuotannon.

## <a name="go-live-move-your-new-code-into-production"></a>Siirry live: uuden koodin siirtäminen tuotannon

Voit nyt siirtää Päivityksesi tuotannon. Mitä on hyvä on, että kun tiedät, että Päivityksesi on jo vahvistettu _ennen kuin_ se on miten tuotannon. Nyt voit eri neljänneksien käyttöönottoa. Koska teit päivitys AngularJS asiakas-sovellukseen, vain asiakaspuolen koodin vahvistaminen. Jos muutosten tekeminen taustatietokantaan verkko-Ohjelmointirajapinnan-sovellukseen, voitu tarkistaa muutokset vastaavasti ja helposti.

1. Poista Git Shell-liikenne reititys sääntö suorittamalla seuraavan komennon:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Suorita Git-komennot:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Odota muutama minuutti väliaikaisen paikka käyttöön uusi koodi ja valitse Käynnistä http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net voit varmistaa, että uusi päivitys on lämmitettävä-väliaikaisen paikka. Muista että rinnakkainen perusmuodon haara on linkitetty sovelluksen väliaikaisen paikka.

3. Vaihda nyt kyselyjä tuotannon väliaikaisen paikka

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Yhteenveto ##

Azure App palvelun helpottaa pienten - keskisuurten yritysten voit esikatsella asiakkaan osoittava niiden sovellusten tuotannon julkaiseminen, joka on tehty perinteisesti suuri yrityksissä. Tässä opetusohjelmassa on Toivottavasti antanut tiedot on tuotava yhdessä App palvelu ja hakemuksen tiedot tekemään mahdollista flighting käyttöönotto ja testaa tuotannon myös muissa tilanteissa DevOps maailmaa. 

## <a name="more-resources"></a>Lisää resursseja ##

-   [Joustava software development Azure-sovelluksen ominaisuudet](app-service-agile-software-development.md)
-   [Väliaikaisen ympäristöissä Azure App palvelun web-sovellusten määrittäminen](web-sites-staged-publishing.md)
-   [Monimutkaisen laadukkaampi Azure-sovelluksen käyttöönotto](app-service-deploy-complex-application-predictably.md)
-   [Authoring Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)
-   [JSONLint - JSON tarkistajan](http://jsonlint.com/)
-   [Git haarautumista – Basic haarautuminen ja yhdistäminen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Projektin Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
