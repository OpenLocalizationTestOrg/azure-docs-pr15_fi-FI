<properties 
    pageTitle="Miten voit luoda verkkosovellukseen Redis välimuistin | Microsoft Azure" 
    description="Opettele luomaan verkkosovellukseen Redis välimuistin kanssa" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Miten voit luoda verkkosovellukseen Redis välimuisti

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Tässä opetusohjelmassa näytetään luomisesta ja web App-sovellukseen Azure-sovelluksen palvelun ASP.NET web-sovelluksen käyttöönotto Visual Studio 2015 avulla. Sovelluksen malli näkyy ryhmän tilastotietoja tietokannasta ja eri tapoja käyttää Azure Redis välimuistin tallentamiseen ja hakea tietoja välimuistista. Kun olet suorittanut opetusohjelman on, joka lukee ja kirjoittaa tietokannalle optimoitu Azure Redis välimuistin kanssa ja Azure ylläpidettävä verkkosovellukseen.

Opit:

-   Miten voit luoda ASP.NET MVC 5-web-sovelluksen Visual Studiossa.
-   Voit käyttää tietoja tietokanta käyttämällä kohteen Framework.
-   Voit parantaa tietojen ja vähentää tietokannan kuormituksen tallentamalla ja Azure Redis välimuistin käyttäminen tietojen hakeminen yksitellen.
-   Käyttämisestä Redis.txt lajiteltu määrittäminen hakemaan Ylin 5 ryhmiä.
-   Voit valmistella Azure resurssit-sovelluksen Resurssienhallinta mallin avulla.
-   Voit julkaista Azure Visual Studiossa sovelluksen.

## <a name="prerequisites"></a>Edellytykset

Viimeistele opetusohjelman, sinulla on oltava seuraavat edellytykset.

-   [Azure-tili](#azure-account)
-   [Visual Studio 2015 Azure SDK .NET kanssa](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-tili

Sinulla on oltava suorittamiseen opetusohjelman Azure tili. Voit:

* [Avaa Azure-tili maksutta](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Saat hyvitykset, jonka avulla voit kokeilla maksettu Azure services. Vaikka hyvitysten on käytetty, voit pitää tili ja Azure palveluiden ja -toimintojen käytön.
* [Aktivoi Visual Studio tilaajan etuja](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN-tilauksen tutustutaan hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 Azure SDK .NET kanssa

Opetusohjelman on kirjoitettu Visual Studio 2015 [Azure SDK.NET](../dotnet-sdk.md) 2.8.2 kanssa tai uudempi versio. [Lataa uusimmat Azure SDK, tähän Visual Studio 2015 julkaistut](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio asennetaan automaattisesti SDK, jos sinulla ei ole jo.

Jos käytössäsi on Visual Studio 2013, voit [ladata Visual Studio 2013: n uusimmat Azure SDK-paketissa](http://go.microsoft.com/fwlink/?LinkID=324322). Jotkin näytöt voivat näyttää eri kuin tässä opetusohjelmassa näytetään kuvat.

>[AZURE.NOTE] Sen mukaan, kuinka monta SDK riippuvuudet on jo käyttämääsi laitteeseen SDK asentaminen saattaa kestää kauan, useita minuuteista vähintään puolessa tunnissa.

## <a name="create-the-visual-studio-project"></a>Visual Studio projektin luominen

1. Avaa Visual Studio ja valitse **Tiedosto**, **Uusi** **Projekti**.

2. Laajenna **Visual C#** -solmu **Mallit** -luettelosta, valitse **Cloud**ja sitten **ASP.NET Web-sovelluksen**. Varmista, että **.NET Framework 4.5.2** on valittuna.  Kirjoita **ContosoTeamStats** **nimi** -tekstiruutuun ja valitse **OK**.
 
    ![Projektin luominen][cache-create-project]

3. Valitse **MVC** projektityyppi. Poista **pilveen Host** -valintaruudun valinta. Saat opetusohjelman [Varaa Azure resurssit](#provision-the-azure-resources) ja [julkaista Azure sovelluksen](#publish-the-application-to-azure) seuraavat vaiheet. Esimerkki valmistelu App palvelun web-sovelluksen Visual Studio jättämällä **Host pilveen** valittuna on artikkelissa [Azure App palvelu, ASP.NET- ja Visual Studio Web App -sovellusten käytön aloittaminen](../app-service-web/web-sites-dotnet-get-started.md).

    ![Valitse project-malli][cache-select-template]

4. Valitse **OK** , jos haluat luoda projektin.

## <a name="create-the-aspnet-mvc-application"></a>ASP.NET-MVC-sovelluksen luominen

Tämän opetusohjelman-osassa Luo basic sovellus, jossa lukee ja näyttää ryhmän tilastotiedot tietokannasta.

-   [Lisää malli](#add-the-model)
-   [Lisää ohjain](#add-the-controller)
-   [Näkymien määrittäminen](#configure-the-views)

### <a name="add-the-model"></a>Lisää malli

1. Napsauta **Ratkaisunhallinnassa** **Mallit** hiiren kakkospainikkeella ja valitse **Lisää**- **luokka**. 

    ![Lisää malli][cache-model-add-class]

2. Kirjoita `Team` luokan nimi ja valitse **Lisää**.

    ![Lisää mallin luokka][cache-model-add-class-dialog]

3. Korvaa `using` lauseet yläreunaan `Team.cs` tiedoston seuraavaan lauseita käyttäen.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Korvaa määritelmään `Team` seuraavat koodikatkelman, joka sisältää päivitetty luokan `Team` luokan määritys sekä joitakin muita kohteen Framework helper luokkia. Lisätietoja koodin ensimmäinen menetelmä kohteen kehys, jota käytetään tässä opetusohjelmassa on artikkelissa [koodin ensin uuden tietokannan](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Kaksoisnapsauta **Ratkaisunhallinnassa** **web.config** , avaa se.

    ![Web.config][cache-web-config]

3.  Lisää seuraavat yhteysmerkkijonon `connectionStrings` osa. Yhteysmerkkijonon nimen on oltava samat kohteen Framework tietokannan kontekstin luokan nimi on `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Kun olet lisännyt, `connectionStrings` osan pitäisi näyttää seuraavalta.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Lisää ohjain

1. Paina **F6-näppäintä** voit luoda projektin. 
2. Napsauta **ohjaimet** -kansiota hiiren kakkospainikkeella ja valitse **Lisää**- **ohjain** **Ratkaisunhallinnassa**.

    ![Lisää ohjain][cache-add-controller]

3. Valitse **MVC 5 ohjauskoneen näkymiä, käyttämällä kohde Framework**ja sitten **Lisää**. Jos saat virheilmoituksen, kun valitset **Lisää**, varmista, että projektin sisäisten ensin.

    ![Lisää ohjauskoneen luokka][cache-add-controller-class]

5. Valitse **ryhmän (ContosoTeamStats.Models)** **mallin luokan** avattavasta luettelosta. Valitse **TeamContext (ContosoTeamStats.Models)** **tietojen konteksti luokan** avattavasta luettelosta. Kirjoita `TeamsController` - **ohjain** nimi-tekstiruutuun (jos se ei ole automaattisesti täytetty). Valitse **Lisää** controller-luokan luominen ja lisääminen oletusarvo-näkymistä.

    ![Ohjauskoneen määrittäminen][cache-configure-controller]

4. **Ratkaisunhallinnassa**Laajenna **Global.asax** ja kaksoisnapsauta sitä **Global.asax.cs** .

    ![Global.asax.cs][cache-global-asax]

5. Lisää seuraavat kaksi lauseiden käyttämisestä yläosassa olevan tiedoston, valitse toinen lauseita käyttäen.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Lisää seuraava rivi koodin lopussa `Application_Start` menetelmää.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. Laajenna **Ratkaisunhallinnassa** `App_Start` ja kaksoisnapsauta `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Korvaa `controller = "Home"` -seuraava koodi `RegisterRoutes` menetelmä, jolla `controller = "Teams"` seuraavan esimerkin mukaisesti.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Näkymien määrittäminen

1. **Ratkaisunhallinnassa**Laajenna **näkymät** -kansioon ja valitse sitten **jaettu** kansio ja kaksoisnapsauta **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Sisällön muuttaminen `title` elementin ja korvaa `My ASP.NET Application` kanssa `Contoso Team Stats` seuraavan esimerkin mukaisesti.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. Valitse `body` -osassa Päivitä ensimmäisen `Html.ActionLink` lauseen ja korvaa `Application name` kanssa `Contoso Team Stats` ja korvaa `Home` kanssa `Teams`.
    -   Ennen:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Jälkeen:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Muutokset][cache-layout-cshtml-code]

4. Paina **Ctrl + F5** voivat laatia ja suorita sovellus. Tämän version sovellus lukee tulokset suoraan-tietokannasta. Huomautus: **Luo uusi**, **Muokkaa**, **tiedot**ja **Poista** toiminnot, jotka on sovelluksen automaattisesti lisätty **MVC 5 ohjauskoneen näkymiä, käyttämällä kohde Framework** scaffold mukaan. Opetusohjelman seuraavassa osassa lisäät Redis välimuistin optimoida tietojen käyttöä ja antaa lisäominaisuudet-sovellukseen.

![Starter-sovellus][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Määritä sovellus käyttää Redis välimuisti

Tämän opetusohjelman-osassa määrität sovelluksen tallentamiseen ja Contoso ryhmän tilastotiedot noutaa Azure Redis välimuistin erillisen [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) välimuistin käyttävä malli.

-   [Jos haluat käyttää StackExchange.Redis sovelluksen kokoonpanon määrittäminen](#configure-the-application-to-use-stackexchangeredis)
-   [Päivitä palauttamaan tulokset välimuistin tai tietokannan TeamsController-luokka](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Luo, Muokkaa ja poista menetelmiä toimimaan välimuistin päivittäminen](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Päivitä toimimaan välimuistin ryhmiä indeksi-näkymä](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Jos haluat käyttää StackExchange.Redis sovelluksen kokoonpanon määrittäminen

1. Määritä asiakassovellus StackExchange.Redis NuGet pakkaaminen Visual Studiossa, napsauta **Ratkaisunhallinnassa** projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**. 

    ![NuGet pakettien hallinta][redis-cache-manage-nuget-menu]

2. Kirjoita hakuruutuun **StackExchange.Redis** , valitse haluamasi versio tuloksista ja valitse **Asenna**.

    ![StackExchange.Redis NuGet-paketti][redis-cache-stack-exchange-nuget]

    NuGet-paketin lataaminen ja lisää tarvittavaa kokoonpanoa viitteet asiakassovellus Azure Redis välimuistin käyttö StackExchange.Redis välimuistin avulla. Jos haluat käyttää **StackExchange.Redis** asiakkaan kirjaston vahva nimeltä versio, valitse **StackExchange.Redis.StrongName**; Valitse muussa **StackExchange.Redis**.

3. **Ratkaisunhallinnassa**Laajenna **ohjaimet** -kansio ja kaksoisnapsauta sitä **TeamsController.cs** .

    ![Ryhmät-ohjain][cache-teamscontroller]

4. Lisää seuraavat kaksi lauseita **TeamsController.cs**käyttäen.

        using System.Configuration;
        using StackExchange.Redis;

5. Lisää seuraavat kaksi ominaisuuksia, jotta `TeamsController` luokka.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Luo tiedosto tietokoneessa nimeltä `WebAppPlusCacheAppSecrets.config` ja sijoita se ei uloskuitattava lähdekoodin malli-sovelluksen sisään olisi päätät kuitattava sisään muualla olevaan sijaintiin. Tässä esimerkissä `AppSettingsSecrets.config` tiedosto sijaitsee `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Muokkaa `WebAppPlusCacheAppSecrets.config` tiedoston ja lisää seuraavat sisältöä. Jos suoritat sovelluksen paikallisesti näitä tietoja käytetään Azure Redis välimuistin esiintymään. Myöhemmin-opetusohjelman valmistella erillisen Azure Redis välimuistin ja Päivitä välimuistin käyttäjänimi ja salasana. Jos et aio malli-sovelluksen käyttämiseen paikallisesti voit ohittaa tämän tiedoston ja seuraavat vaiheet, jotka viittaavat tiedoston luomista, koska kun otat käyttöön Azure sovellus hakee välimuistin yhteyden tiedot Web-sovelluksen asetus ja tästä tiedostosta. Koska `WebAppPlusCacheAppSecrets.config` ei ole otettu käyttöön, sovelluksen Azure, et tarvitse sitä paitsi, jos aiot Suorita sovellus paikallisesti.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Kaksoisnapsauta **Ratkaisunhallinnassa** **web.config** , avaa se.

    ![Web.config][cache-web-config]

3. Lisää seuraava `file` osoittaa `appSettings` elementti. Jos olet käyttänyt eri nimi tai sijainti, korvaa arvot siirron esimerkin mukaisesti.
    -   Ennen:`<appSettings>`
    -   Jälkeen:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    ASP.NET-runtime yhdistää merkinnät-vaihtoehto ulkoisen tiedoston sisällön `<appSettings>` elementti. Suoritettavan ohittaa tiedoston-määrite, jos määritettyä tiedostoa ei löydy. Tietoja oman (yhteysmerkkijonon avulla välimuistin) eivät ole lähdekoodin sovelluksen mukana. Kun otat käyttöön Azure-web-sovelluksen `WebAppPlusCacheAppSecrests.config` tiedostoa ei voi ottaa käyttöön (joka on haluamasi). Voit määrittää näitä tietoja Azure useilla tavoilla ja tässä opetusohjelmassa ne on määritetty automaattisesti, kun voit [valmistella Azure resurssien](#provision-the-azure-resources) myöhemmin opetusohjelmassa vaihe. Lisätietoja Azure tietoja käyttämisestä on artikkelissa [käyttöönottaminen salasanoja ja muita luottamuksellisia tietoja ASP.NET ja Azure App parhaat käytännöt](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Päivitä palauttamaan tulokset välimuistin tai tietokannan TeamsController-luokka

Tässä esimerkissä ryhmän tilastotiedot voidaan hakea tietokannasta tai välimuistista. Ryhmän tilasto on tallennettu nimellä sarjoitettu välimuistin `List<Team>`, ja myös joukkona lajiteltu käyttämällä Redis.txt tietotyypit. Noudettaessa kohteiden lajiteltuja mukaisiksi, voit hakea jotkin kaikki tai kyselyn tiettyjen kohteiden. Tässä esimerkissä kyselyn lajitellun joukon Ylin 5 työryhmät luokitellut wins määrän mukaan.

>[AZURE.NOTE] Ei ole pakollinen ryhmän tilastotiedot tallennuspaikoista välimuistin useita muotoja voi käyttää Azure Redis välimuistin. Tässä opetusohjelmassa esitellään joitakin eri tavalla ja eri tietotyyppien välimuistitietojen avulla voit käyttää useita muotoja.



1. Lisää seuraava teksti käyttämällä lauseet `TeamsController.cs` tiedoston yläosassa toisella lauseita käyttäen.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Korvaa nykyinen `public ActionResult Index()` menetelmä seuraavat toteutukseen.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Lisää seuraavista kolmesta tavasta, `TeamsController` luokan toteuttamisesta `playGames`, `clearCache`, ja `rebuildDB` switch-lause, joka on lisätty edellisen koodikatkelman tiedostotyyppien toiminto.

    `PlayGames` Tapa päivittää ryhmän tilastotiedot simuloimalla Vuodenaika-kentät, visualisointi n, tallentaa tulokset tietokantaan ja tyhjentää välimuistin nyt vanhentuneita tietoja.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    `RebuildDB` Menetelmä alustaa ryhmiä oletusarvoiset tietokannan uudelleen, Luo tilastotiedot niiden ja tyhjentää välimuistin nyt vanhentuneita tietoja.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    `ClearCachedTeams` Tapa poistaa kaikki välimuistiin tallennetut ryhmän tilastotiedot välimuistista.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Lisää seuraavat neljä menetelmät `TeamsController` luokan toteuttamisesta hakeminen ryhmän tilastotiedot välimuistin ja tietokannan eri tavoin. Palauttaa eri tavalla `List<Team>` joka näytetään näkymän perusteella.

    `GetFromDB` Menetelmä lukee ryhmän tilasto-tietokannasta.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    `GetFromList` Menetelmä lukee ryhmän tilastotiedot välimuistista kuin sarjoitettu `List<Team>`. Jos epäonnistumisten ryhmän tilastotiedot lukea tietokantaa ja tallennettu välimuistin seuraavaa kertaa varten. Tässä esimerkissä esimerkissä on käytössä JSON.NET Sarjatoiminto onnistu .NET-objekteja, ja välimuistista. Lisätietoja on artikkelissa [Azure Redis välimuistin .NET-objektien käyttäminen](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    `GetFromSortedSet` Menetelmä lukee ryhmän tilastotiedot välimuistiin tallennetut lajiteltuja mukaisiksi. Jos epäonnistumisten ryhmän tilastotiedot lukea tietokantaa ja lajiteltu joukkona välimuistiin tallennettujen.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    `GetFromSortedSetTop5` Menetelmä lukee ensimmäiset 5 ryhmiä välimuistissa lajiteltuja mukaisiksi. Se käynnistyy tarkastamalla, onko välimuisti `teamsSortedSet` avain. Jos tämä avain ei ole käytössä, `GetFromSortedSet` menetelmää kutsutaan ryhmän tilastoja ja tallentaa ne välimuistin. Seuraavaksi välimuistissa lajitellun joukon haetaan Ylin 5 ryhmiä, jotka on palautettu.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Luo, Muokkaa ja poista menetelmiä toimimaan välimuistin päivittäminen

Rakennustelineet-koodi, joka on luotu osana tässä esimerkissä sisältää tavoista lisätä, muokata ja poistaa ryhmiä. Milloin tahansa työryhmän on lisätty, muokattu tai poistettu, välimuistin tiedot vanhentuu. Tässä osassa muokkaat nämä kolme tapaa tyhjentää välimuistiin työryhmät välimuistia ei ole synkronoitu tietokannassa.

1. Selaa `Create(Team team)` menetelmä `TeamsController` luokka. Puhelun `ClearCachedTeams` menetelmä, kuten seuraavassa esimerkissä.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Selaa `Edit(Team team)` menetelmä `TeamsController` luokka. Puhelun `ClearCachedTeams` menetelmä, kuten seuraavassa esimerkissä.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Selaa `DeleteConfirmed(int id)` menetelmä `TeamsController` luokka. Puhelun `ClearCachedTeams` menetelmä, kuten seuraavassa esimerkissä.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Päivitä toimimaan välimuistin ryhmiä indeksi-näkymä

1. Laajenna **näkymät** -kansioon, valitse **ryhmät** -kansio ja kaksoisnapsauta **Index.cshtml** **Ratkaisunhallinnassa**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Etsi tiedoston yläosassa seuraava osa kappale.

    ![Toimi-taulukko][cache-teams-index-table]

    Tämä on linkki Luo uusi ryhmä. Korvaa kappaleen osan seuraavassa taulukossa. Tässä taulukossa on uuden ryhmän luominen, uusi hauskaa ja visualisointi n toistaminen, -välimuistin tyhjentäminen, haetaan ryhmät eri tiedostomuodoissa välimuistista, haetaan ryhmät tietokannasta ja joiden ajan tasalla esimerkkitiedoilla tietokannan toiminto linkeistä.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Vieritä näytön alareunaan **Index.cshtml** tiedoston ja lisää seuraava `tr` niin, että se on tiedoston edellisen taulukon viimeisen rivin elementti.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Rivi näyttää arvon `ViewBag.Msg` joka sisältää nykyisen toimintoa, joka on määritetty, kun napsautat jotakin edellisessä vaiheessa toimintolinkit tilaraportin.   

    ![Tilasanoma][cache-status-message]

4. Paina **F6-näppäintä** voit luoda projektin.

## <a name="provision-the-azure-resources"></a>Varaa Azure resurssit

Isännöimiseen sovelluksesi Azure-tietokannassa on ensin valmisteltava Azure palvelut, jotka sovellus edellyttää. Tässä opetusohjelmassa malli-sovellus käyttää seuraavia Azure palveluita.

-   Azure Redis.txt välimuisti
-   Sovelluksen palvelun Web Appissa
-   SQL-tietokantaan

Käyttöön uuteen tai aiemmin luotuun resurssiryhmä valittua palveluista, napsauta seuraavat **käyttöönotto Azure** -painiketta.

[! [Käyttöönottaminen Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Tämä **käyttöönotto Azure** -painike käyttää [Luo verkkosovellukseen plus Redis välimuistin ja SQL-tietokantaan](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure pikaopas](https://github.com/Azure/azure-quickstart-templates) mallia valmistella palveluista ja määritä yhteysmerkkijonon SQL-tietokantaan ja Azure Redis välimuistin yhteysmerkkijonon sovelluksen-asetusta.

>[AZURE.NOTE] Jos sinulla ei ole Azure-tili, voit [luoda vapaa Azure-tili](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) -vain muutaman minuutin.

**Ota Azure** -vaihtoehdon siirryt Azure-portaaliin ja aloittaa luominen mallin kuvaaman resurssit.

![Ottaa käyttöön Azure][cache-deploy-to-azure-step-1]

1. Valitse **Mukautettu käyttöönottoa** sivu käyttää, ja valitse aiemmin luotu resurssiryhmä tai luoda uuden Azure tilaus ja resurssien ryhmä-sijainti.
2. Määritä järjestelmänvalvojan tilin nimi **Parametrit** -sivu (**ADMINISTRATORLOGIN** - Älä käytä **järjestelmänvalvoja**), järjestelmänvalvojan kirjautumissalasana (**ADMINISTRATORLOGINPASSWORD**) ja tietokannan nimeä (**nimi**). Muut parametrit on määritetty sovelluksen ilmainen, isännöinnin suunnitelma ja SQL-tietokantaan ja Azure Redis välimuisti, joka ei sisältyvät vapaa taso alemman kustannukset asetukset.
3. Muuta muita asetuksia halutessasi säilyttää oletusarvoisesti ja valitsemalla **OK**.


![Ottaa käyttöön Azure][cache-deploy-to-azure-step-2]

1. Valitse **Tarkista juridiset ehdot**.
2. Lue ehdot, valitse **Osta** -sivu ja valitse **Osta**.
3. Aloita valmistelu resursseja, valitse **Mukautettu käyttöönottoa** sivu **luominen** .

Käyttöönoton etenemisen tarkasteleminen, valitse napsauttamalla ilmaisinalueen kuvaketta ja valitse **käyttöönotto**.

![Käyttöönotto aloitettu][cache-deployment-started]

Voit tarkastella käyttöönoton tilan **Microsoft.Template** -sivu.

![Ottaa käyttöön Azure][cache-deploy-to-azure-step-3]

Kun valmistelu on valmis, voit julkaista Azure sovellusta Visual Studio.

>[AZURE.NOTE] Virheitä, joita voi ilmetä valmistelun aikana näkyvät **Microsoft.Template** -sivu. Yleisiä virheitä on liikaa SQL-palvelimia tai liian monta sovelluksen ilmainen isännöinnin suunnitelmien tilauskohtaisten. Korjaa virheitä ja Käynnistä valitsemalla **Ota uudelleen** **Microsoft.Template** sivu tai **Ota Azure** -painiketta Tässä opetusohjelmassa.

## <a name="publish-the-application-to-azure"></a>Julkaise Azure sovelluksen

Tässä vaiheessa opetusohjelman julkaista Azure sovelluksen ja suorita se pilveen.

1. Visual Studio **ContosoTeamStats** projektin hiiren kakkospainikkeella ja valitse **Julkaise**.

    ![Julkaiseminen][cache-publish-app]

2. Valitse **Microsoft Azure App palvelu**.

    ![Julkaiseminen][cache-publish-to-app-service]

3. Käytetään luotaessa Azure resurssien tilaus, laajenna sisältämiä resursseja resurssiryhmän, valitse haluamasi Web-sovellus ja valitse **OK**. Jos käytit Web App-nimi alkaa **sivuston** joitakin muita merkkejä perään **käyttöönotto Azure** -painiketta.

    ![Valitse Web Appissa][cache-select-web-app]

4. Valitse **Tarkista yhteys** ja tarkasta asetukset ja valitse sitten **Julkaise**.

    ![Julkaiseminen][cache-publish]

    Julkaisuprosessin kestää hetken kuluttua ja selaimen käynnistetään suorittaminen Esimerkki sovelluksen. Jos näkyviin tulee DNS-virhe, kun tarkistaminen tai julkaisun ja Azure resurssien sovelluksen valmistelu prosessi on valmis äskettäin, odota hetki ja yritä uudelleen.

    ![Lisätty välimuisti][cache-added-to-application]

Seuraavassa taulukossa on kuvattu kunkin toimintolinkki malli-sovelluksesta.

| Toiminto                  | Kuvaus                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Luo uusi              | Luo uusi ryhmä.                                                                                                                                               |
| Toista Vuodenaika-kentät             | Toista Vuodenaika-kentät, visualisointi n, Päivitä ryhmän Tilasto ja poista valinta vanhentunutta ryhmän tietoja välimuistista.                                                                          |
| Tyhjennä välimuisti             | Poista ryhmä-tilasto välimuistista.                                                                                                                             |
| Luettelon välimuistista         | Voit hakea ryhmän tilasto välimuistista. Jos näkyvissä on epäonnistumisten, ladata tilasto tietokannasta ja välimuistiin tallentaminen seuraavaa kertaa varten.                                        |
| Lajitellun joukon välimuistista   | Hakea ryhmän tilasto käyttämällä lajitellut välimuistista. Jos näkyvissä on epäonnistumisten, ladata tilasto tietokannasta ja Tallenna käyttämällä lajitellut välimuistin.  |
| Ensimmäiset 5 ryhmiä välimuistista  | Noutaa Ylin 5 ryhmiä käyttämällä lajitellut välimuistista. Jos näkyvissä on epäonnistumisten, ladata tilasto tietokannasta ja Tallenna käyttämällä lajitellut välimuistin. |
| DB lataaminen            | Voit hakea ryhmän tilasto tietokannasta.                                                                                                                       |
| DB muodostaminen uudelleen              | Tietokannan muodostaminen uudelleen ja lataa se uudelleen ryhmän mallitiedot.                                                                                                        |
| Muokkaa / tiedot / poistaminen | Muokkaa ryhmän, tarkastella ryhmän tietoja, Poista ryhmä.                                                                                                             |


Napsauta Toiminnot ja kokeilla noutaa tiedot eri lähteistä. Ei eroja eri tavoista tietojen hakeminen tietokannasta ja välimuistin suorittamiseen kuluvaa aikaa.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Poista resursseja, kun olet valmis-sovelluksella

Kun olet valmis malli opetusohjelman sovelluksessa, voit poistaa Azure resurssien säästää kustannus- ja resurssien avulla. Jos kaikki resurssit sisältyvät saman resurssiryhmän **käyttöönotto Azure** painikkeella [Varaa Azure resurssit](#provision-the-azure-resources) -osassa, voit poistaa ne yhteen yhdellä kertaa poistamalla resurssiryhmän.

1. Kirjautuminen [Azure-portaaliin](https://portal.azure.com) ja valitse **resurssiryhmiä**.
2. Kirjoita resurssiryhmän nimi **... kohteiden suodattaminen** tekstiruutu.
3. Valitse **...** oikealla puolella resurssiryhmä.
4. Valitse **Poista**.

    ![Poista][cache-delete-resource-group]

5. Kirjoita resurssiryhmän nimi ja valitse **Poista**.

    ![Vahvista poistaminen][cache-delete-confirm]

Hetken kuluttua resurssiryhmän ja kaikki sen sisältämät resursseja poistetaan.

>[AZURE.IMPORTANT] Huomautus poistaminen resurssiryhmä ei voi peruuttaa, ja Poista pysyvästi resurssiryhmän ja kaikki sen resurssit. Varmista, että et vahingossa poista väärä resurssiryhmä tai resurssit. Jos olet luonut resurssien isännöinnin tässä esimerkissä aiemmin resurssiryhmä sisällä, voit poistaa kullekin resurssille yksitellen niiden vastaaviin lavat.

## <a name="run-the-sample-application-on-your-local-machine"></a>Esimerkkisovelluksen käyttämiseen paikallisessa tietokoneessa

Suorita sovellus paikallisesti tietokoneeseen, tarvitset erillisen Azure Redis välimuistin, johon välimuistin tiedot. 

-   Jos olet julkaissut sovelluksesi Azure edellisessä kohdassa kuvatulla tavalla, voit käyttää Azure Redis välimuistin ‑esiintymä, jossa on valmisteltu vaiheen aikana.
-   Jos sinulla on toinen aiemmin Azure Redis välimuistin esiintymän, voit, voit suorittaa tämän mallin paikallisesti.
-   Jos haluat luoda Azure Redis välimuistin esiintymän, voit noudattaa [Luo välimuistin](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)ohjeita.

Kun olet valinnut tai luonut käyttämään välimuistin, siirry Azure-portaalissa välimuistin ja noutaa [isäntänimi](cache-configure.md#properties) ja välimuistin [pikanäppäimet](cache-configure.md#access-keys) . Katso ohjeet [määrittäminen Redis välimuistiasetukset](cache-configure.md#configure-redis-cache-settings).

1. Avaa `WebAppPlusCacheAppSecrets.config` [Määritä sovellus käyttää Redis välimuistin](#configure-the-application-to-use-redis-cache) Tässä opetusohjelmassa editorilla valittua vaiheessa luomasi tiedosto.

2. Muokkaa `value` määrite ja korvaa `MyCache.redis.cache.windows.net` välimuistin, [isäntänimi](cache-configure.md#properties) ja määritä joko [ensisijaisen tai toissijaisen](cache-configure.md#access-keys) välimuistin salasana.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Paina **Ctrl + F5** sovelluksen käyttämiseen.

>[AZURE.NOTE] Huomaa, että koska sovelluksen tietokanta, mukaan lukien on käytössä paikallisesti ja Redis välimuistin nykyisessä Azure-tietokannassa, välimuistin ehkä näy kohdassa suorittaa tietokannan. Parhaan mahdollisen suorituskyvyn asiakassovellus ja Azure Redis välimuistin esiintymää on oltava samassa sijainnissa. 

## <a name="next-steps"></a>Seuraavat vaiheet

-   Lisätietoja [ASP.NET](http://asp.net/) -sivuston [Käytön aloittaminen ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) .
-   Katso Lisää esimerkkejä ASP.NET Web-sovelluksen luominen sovelluksen palvelun [Luo ja ota käyttöön Azure-sovelluksen palvelun ASP.NET web-sovellus](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) - [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Yhdistä [esittely](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Katso Lisää quickstarts-HealthClinic.biz esittely, [Azure Developer Työkalut Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Lisätietoja kohteen kehys, jota käytetään tässä opetusohjelmassa [koodin ensin uuden tietokannan](https://msdn.microsoft.com/data/jj193542) toimintatapa.
-   Lisätietoja [Azure-sovelluksen palvelun verkkosovelluksissa](../app-service-web/app-service-web-overview.md).
-   Lue, miten [näytön](cache-how-to-monitor.md) välimuistin Azure-portaalissa.

-   Azure Redis välimuistin premium-ominaisuuksia
    -   [Pysyvyyttä Premium Azure Redis välimuistin määrittäminen](cache-how-to-premium-persistence.md)
    -   [Käyttövarmuuden lisäämiseksi Premium Azure Redis välimuistin määrittäminen](cache-how-to-premium-clustering.md)
    -   [Määrittäminen Premium Azure Redis välimuistin VPN-tuki](cache-how-to-premium-vnet.md)
    -   Katso lisätietoja kokoa, liikenteen ja kaistanleveyden kanssa premium tallentaa [Azure Redis välimuistin usein kysytyt kysymykset](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) .



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

