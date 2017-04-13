<properties 
    pageTitle="Joustavasti tietokannan asiakkaan kirjaston käyttäminen kohteen Framework | Microsoft Azure" 
    description="Käytä joustavasti tietokannan asiakkaan kirjasto ja kohteen Framework coding tietokannat" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Kohteen Framework joustavasti tietokannan asiakkaan kirjastosta 
 
Tämä asiakirja näyttää muutokset, joita tarvitaan [joustavasti Tietokantatyökalut](sql-database-elastic-scale-introduction.md)integroida kohteen Framework-sovelluksen. Keskitytään sähköpostiviestiä [shard Yhdistä hallinta](sql-database-elastic-scale-shard-map-management.md) ja [tietojen riippuva reititys](sql-database-elastic-scale-data-dependent-routing.md) kohteen Framework **Koodin ensimmäinen** menetelmä kanssa. EF [Koodin ensimmäisen-kirjoitustukea – uuden tietokannan](http://msdn.microsoft.com/data/jj193542.aspx) opetusohjelma on käynnissä olevassa esimerkissä tässä asiakirjassa. Tämän asiakirjan mukana otoksen koodi on osa joustavasti tietokannan Työkalut näytteiden Visual Studio MALLIKOODEJA määrittäminen.
  
## <a name="downloading-and-running-the-sample-code"></a>Lataa ja suorita Sample Code
Voit ladata tämän artikkelin koodi:

* Visual Studio 2012 tai uudempi versio vaaditaan. 
* Käynnistä Visual Studio. 
* Visual Studiossa valitsemalla Tiedosto -valikosta Uusi projekti. 
* "Uusi projekti-valintaikkunassa Siirry **Online-mallit** **Visual C#** ja kirjoita"joustavasti db"oikeassa yläkulmassa olevaan hakukenttään.
    
    ![Kohteen Framework ja joustavasti tietokannan otoksen app][1] 

    Valitse otosten kutsutaan **Azure SQL – kohteen Framework integrointi joustavasti DB työkaluja**. Hyväksyttyään käyttöoikeuden Lataa malli. 

Mallin suorittamiseen tarvitset kolmen tyhjän tietokantojen luominen Azure SQL-tietokantaan:

* Shard kartan Manager-tietokanta
* Tietokannan shard 1
* Shard 2 tietokanta

Kun olet luonut tietokannat, täytä paikkamerkkejä **Program.cs** -että Azure SQL-tietokantapalvelimen nimi, tietokannan nimen ja tunnistetiedot muodostaa tietokannat. Luoda ratkaisun käyttöön Visual Studiossa. Visual Studio lataa tarvittavat NuGet paketit joustavasti tietokannan asiakas-kirjaston kohteen Framework ja lyhytkestoisia vika käsittely muodosta yhteydessä. Varmista, että palauttaminen NuGet paketit on otettu käyttöön ratkaisu. Voit ottaa tämän asetuksen napsauttamalla hiiren kakkospainikkeella Visual Studio ratkaisunhallinnassa ratkaisutiedosto. 

## <a name="entity-framework-workflows"></a>Kohteen Framework työnkulut 

Kohteen Framework kehittäjät riippuvaisia seuraavat neljä työnkulkujen sovelluksia ja varmistaa pysyvyyden sovelluksen objektit: 

* **Koodin ensimmäisen (uusi tietokanta)**: EF kehittäjä luo mallin sovelluksen koodissa ja sitten EF muodostaa tietokannan siitä. 
* **Koodin ensimmäisen (aiemmin luotu tietokanta)**: kehittäjä avulla voi luoda mallin sovelluksen-koodin aiemmin luodun tietokannan EF.
* **Mallin ensimmäisen**: kehittäjä luo mallin EF suunnittelussa ja sitten EF tietokannan mallista.
* **Tietokannan ensimmäisen**: kehittäjä käyttää EF tooling määrittääkseen aiemmin luodun tietokannan mallia. 

Kaikki ‑ominaisuuksia luottavat hallita läpinäkyvä tietokantayhteyksiä ja tietokannan rakenteen sovelluksen DbContext-luokka. Keskustelua tarkemmin jäljempänä asiakirjassa eri konstruktoreja DbContext perus luokan Salli yhteyden luominen eri tasoilla tietokannan automaattiseen käynnistykseen ja rakenteen luominen. Haasteita johtuvat lähinnä siitä, että tietokannan yhteys hallinta-EF myöntämä Leikkaa kanssa tietojen riippuvaiset reititys liittymää hallinta yhteystapoja joustavasti tietokannan asiakas-kirjasto. 

## <a name="elastic-database-tools-assumptions"></a>Joustavasti tietokannan Työkalut perustiedot 

Katso termin määritelmät [joustavasti tietokannan Työkalut sanasto](sql-database-elastic-scale-glossary.md).

Joustavasti tietokannan asiakas-kirjastoon voit määrittää Ositukset sovelluksen tietojen kutsutaan shardlets. Shardlets sharding näppäintä tunnistetaan ja määritetään tietyt tietokannat. Sovelluksen ehkä niin monta tietokantojen tarpeen mukaan ja jakaa tarpeeksi kapasiteetti tai valita nykyisen business vaatimukset suorituskyvyn shardlets. Tietokantojen avainarvot sharding määritystä tallennetaan joustavasti tietokannan asiakkaan ohjelmointirajapinnan määrittämiä shard kartan mukaan. Emme ota tämän vain **Shard kartan hallinta**tai SMM lyhyt. Shard kartan toimii myös pyynnöt, jotka sisältävät sharding näppäintä tietokantayhteyksiä broker. Tätä ominaisuutta kuin **tietojen riippuva reititys**viitataan. 
 
Shard kartan hallinta suojaa käyttäjien epäyhtenäinen näkymien shardlet tietoihin, jotka voivat ilmetä samanaikainen shardlet hallintatoiminnot (kuten kunnolliselle tietojen yhden shard kohteesta toiseen) ovat tapahtuu. Voit tehdä shard kartat hallitsee asiakkaan kirjaston broker sovelluksen tietokantayhteyksiä. Näin lopettaa tietokantayhteys automaattisesti, kun shard hallintatoiminnot voi olla vaikutusta shardlet yhteys on luotu vain shard kartta-toimintoja. Tämän menetelmän on osittain EF käyttäjän toimintojen, kuten uusien yhteyksien luominen aiemmin luodun tietokannan olemassaolon tarkistavan integroida. Tutustu Huomautus on yleensä vakio DbContext konstruktoreja vain työpaikan luotettavasti suljettu tietokantayhteyksiä, joka voi monistaa turvallisesti EF, toimi. Rakenteen tarpeen mukaan joustavasti tietokanta on sen sijaan broker vain avattu yhteydet. Yksi voi ajatella, että sulkeminen asiakkaan kirjaston liittymillä ennen luovuttaa sen EF DbContext, yhteyden saattaa ratkaista ongelman. Kuitenkin sulkemalla yhteys ja avattava uudelleen, EF käyttäisit, yksi foregoes maksettavan korvauksen kirjaston kelpoisuustarkistus ja yhdenmukaisuuden tarkistukset. EF, siirrot toimintoja kuitenkin käyttää asiakasyhteydet hallittavan pohjana olevan tietokannan rakennetta, niin, että on läpinäkyvä-sovellukseen. Ihannetapauksessa haluamme säilyttää ja yhdistää kaikki nämä ominaisuudet sekä joustavasti tietokannan asiakkaan kirjastosta ja EF samassa sovelluksessa. Seuraavassa osassa kerrotaan nämä ominaisuudet ja vaatimukset tarkemmin. 


## <a name="requirements"></a>Vaatimukset 

Kun käsittelet joustavasti tietokannan asiakkaan kirjasto- ja yritys Framework API, haluamme säilyttää seuraavat ominaisuudet: 

* **Skaalaa-kohtaa**: Lisää tai poista tietokannat sharded sovelluksen kapasiteettivaatimukset tarvittaessa sovelluksen tiedot-taso. Tämä tarkoittaa ohjausobjektin päälle luominen ja poistaminen tietokantojen ja käyttämällä joustavasti tietokannan shard yhdistää hallinnan API tietokantojen ja shardlets yhdistämismääritysten hallinta. 

* **Yhdenmukaisuuden**: sovelluksen suojattu sharding ja käyttää tietoja riippuvaiset reititys ominaisuuksia asiakas-kirjasto. Vioittumisen tai väärä kyselytulokset välttämiseksi yhteydet on se shard kartan hallinnan avulla. Tämä säilyttää myös kelpoisuustarkistus ja yhdenmukaisuuden.
 
* **Koodin ensimmäisen**: säilyttää EF-tiedoston koodi ensimmäisen koaksiaalikaapeliin helppokäyttöisyys. -Koodin ensimmäisen sovelluksen luokkien yhdistetään läpinäkyvä pohjana olevan tietokannan rakenteita. Sovelluksen koodin toimii DbSets, joka peittää useimmat ominaisuuksia, jotka osallistuvat pohjana olevan tietokannan käsittelyn kanssa.
 
* **Rakenne**: kohteen Framework käsittelee alkuperäisen tietokannan rakenteen luonti ja myöhempiä rakenteen kehitystä siirrot kautta. Koska nämä ominaisuudet-sovelluksen mukauttamisesta on helppoa kuin tiedot siihen. 

Seuraavat ohjeet ohjaa miten koodin ensimmäisen sovellusten joustavasti Tietokantatyökalut nämä vaatimukset täyttävän. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Riippuvaiset reititys käyttämällä EF DbContext tiedot 

Kohteen Framework tietokantayhteyksiä hallitaan yleensä **DbContext**aliluokkien avulla. Luo nämä aliluokkien johtuvat **DbContext**. Tämä on, jossa voit määrittää oman **DbSets** , jotka toteuttavat tietokannan palautettua kokoelmien CLR-objektien sovelluksen. Tietoja riippuvaiset reitityksestä kontekstissa tarkastellaan useita hyödyllisiä ominaisuuksia, jotka eivät välttämättä ole pidä muissa EF koodin ensimmäisen sovelluksen tilanteissa: 

* Tietokanta on jo olemassa, ja rekisteröity joustavasti tietokannan shard määrityksen. 
* Sovelluksen rakenteen jo on otettu käyttöön (alla on esitetty)-tietokantaan. 
* Tietoja riippuva reititys yhteydet tietokantaan ovat liittymillä shard kartan. 

Jos haluat liittää **DbContexts** tietojen riippuva reititys asteikko-kohtaa:

1. Luo fyysinen tietokannan yhteydet kautta joustavasti tietokannan asiakkaan liittymät shard kartta-hallinta 
2. Yhteyden muodostaminen **DbContext** aliluokan rivitys
3. Siirtää yhteys varmistamiseksi EF reunassa käsittely tapahtuu sekä **DbContext** base-luokkia. 

Seuraava koodi-esimerkissä havainnollistetaan tätä tapaa. (Tämä koodi on myös mukana Visual Studio projektin)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Pääkohdat
* Uusi konstruktori korvaa-DbContext aliluokan oletuskonstruktoria 
* Uusi konstruktori on argumenttia, joita tarvitaan tietojen riippuvaiset reititys joustavasti tietokannan asiakkaan kirjaston kautta:
    * Voit käyttää tietojen riippuva reititys liittymät shard-kartta
    * tunnistaa shardlet, sharding-avain
    * yhteysmerkkijonon tietojen riippuva reititys yhteyden muodostamisesta shard tunnuksilla. 
 
* Perus luokan konstruktoria puhelu ottaa detour staattinen menetelmä, joka suorittaa eri vaiheiden suorittamisesta tarvittavat tiedot riippuva reitittämiseen. 
   * Se käyttää OpenConnectionForKey puhelun joustavasti tietokannan asiakkaan liittymät shard kartassa Avaa yhteyden.
   * Shard kartan Luo shard, joka sisältää tietyn sharding-näppäimen shardlet Avaa yhteys.
   * Avaa yhteyden siirretään takaisin DbContext haluat ilmaista, että tässä yhteydessä käytettävän EF sijaan auttaa muita EF luoda uuden yhteyden automaattisesti perus luokka-konstruktoriin. Tällä tavalla yhteys on on merkitty joustavasti tietokannan asiakkaan API, niin, että se takaa yhdenmukaisuuden shard kartan hallintatoiminnot-kohdassa.
 
  
Käytä uusi konstruktoria DbContext-aliluokka koodissa oletuskonstruktoria sijaan. Tässä on esimerkki: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Uusi konstruktoria avautuu, joka sisältää tiedot merkittyä **tenantid1**arvo shardlet shard yhteys. Koodin **käyttämällä** estoasetukset säilyy muuttumattomana käyttämään **DbSet** avulla EF shard **tenantid1**blogeja varten. Tämä muuttaa semantiikkaan liittyvien koodin käyttämisen estä siten, että kaikki tietokannan toiminnot ovat nyt suodatetut yhden shard, jossa **tenantid1** on käytettävissä. Esimerkiksi LINQ kyselyn päälle blogit **DbSet** palauttavat vain nykyisen shard tallennetun blogit, mutta ei muiden shards tallennetun tiedoston.  

#### <a name="transient-faults-handling"></a>Lyhytkestoisia virheitä käsittely
Microsoft Patterns ja käytännöt ryhmän julkaista [Lyhytkestoisia vika käsittely sovelluksen estä](https://msdn.microsoft.com/library/dn440719.aspx). Kirjaston käytetään yhdessä EF joustavasti asteikko asiakas-kirjaston kanssa. Varmista kuitenkin, että lyhytkestoisia poikkeuksen palauttaa kohtaan, jossa on voit varmistaa, että uusi konstruktoria käytetään lyhytkestoisia vian jälkeen niin, että kaikki uudet yhteysyritykseen tehdään käyttämällä emme ole määritteiltään erilainen konstruktoreja. Muussa tapauksessa oikea shard yhteyden ei välttämättä ja ei ole vahvistukset yhteys määritetään tapahtuvien muutosten shard karttaan. 

Seuraava koodi malli on kuvattu, miten SQL uudelleen käytännön voi käyttää uuden **DbContext** aliluokka konstruktoreja ympärille: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

Yllä koodissa **SqlDatabaseUtils.SqlRetryPolicy** on määritetty **SqlDatabaseTransientErrorDetectionStrategy** ja uudelleen laskeminen 10 ja 5 sekuntia odotusaika uudelleenyritykset välillä. Tämän menetelmän muistuttaa ohjeet EF ja käyttäjän käynnistämä tapahtumat (katso [rajoitukset yritetään suorittamisen strategioita (EF6 alkaen)](http://msdn.microsoft.com/data/dn307226). Molemmat tilanteet edellyttävät sovelluksen ohjaa alue, johon lyhytkestoisia poikkeuksen palauttaa: Avaa tapahtuma tai (kuten) luo konteksti-ERISNIMI konstruktoria, joka käyttää joustavasti tietokannan asiakas-kirjasto.

Määrittää, missä lyhytkestoisia poikkeukset kestää takaisin alueessa ei tarvitse estää myös valmiin **SqlAzureExecutionStrategy** EF mukana tulevaa käyttöä. **SqlAzureExecutionStrategy** tapauksessa Avaa yhteyden, mutta ei käytä **OpenConnectionForKey** ja ohittaa tämän vuoksi, joka suoritetaan osana **OpenConnectionForKey** puhelun vahvistus. Koodi-malli käyttää sen sijaan valmiin **DefaultExecutionStrategy** , joka sisältää myös EF. **SqlAzureExecutionStrategy**, eikä se toimii oikein uudelleen käytännön lyhytkestoisia vika käsittelyyn yhdessä. Suorittamisen käytäntö määritetään **ElasticScaleDbConfiguration** -luokka. Huomaa, että on päättänyt olla käyttää **DefaultSqlExecutionStrategy** , koska se ehdottaa käyttämään **SqlAzureExecutionStrategy** ilmetessä lyhytkestoisia poikkeukset - joka johtaa väärään toimintaa, kuten edellä. Lisätietoja eri uudelleen käytännöt ja EF on artikkelissa [Yhteyden Vikasietoisuudelle EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktori kirjoittaa uudelleen
Yllä koodi-esimerkeissä kuvataan oletusarvoinen konstruktori uudelleen kirjoittaa vaatii sovelluksen riippuvaiset reititys kohteen Framework tietojen käyttäminen. Seuraavassa taulukossa generalizes muiden konstruktoreja tätä tapaa. 


Nykyinen konstruktoria  | Tietoja kirjoittamalla konstruktorin | Perus konstruktoria | Huomautuksia
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |Yhteys on oltava shard kartan ja tietojen riippuva reititys-näppäintä. Tarvitse ohituksen automaattinen yhteyden luominen EF mukaan ja sen sijaan broker yhteyden shard kartan avulla. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |Yhteys on shard kartan ja tietojen riippuva reititys-näppäintä. Kiinteä tietokannan nimi tai yhteyden yhteysmerkkijono ei toimi siinä muodossa kuin ne ohituksen kelpoisuuden shard kartan avulla. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |Yhteys saada luotu tietyn shard kartta- ja sharding avaimen mallin mukaisesti. Käännetty mallin siirretään perus c'tor.
MyContext (DbConnection bool) |ElasticScaleContext (ShardMap, TKey, bool) |DbContext (DbConnection bool) |Yhteys on voidaan johtaa shard kartta- ja sitten-näppäintä. Se ei voi antaa syötteeksi, (paitsi jos syötteen jo käytössä, shard kartta- ja sitten-näppäintä). Boolean välitetään. 
MyContext (merkkijono, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, bool) |Yhteys on voidaan johtaa shard kartta- ja sitten-näppäintä. Se ei voi antaa syötteeksi, (paitsi jos syötteen käytti shard kartta- ja sitten-näppäintä). Käännetty mallin välitetään. 
MyContext (ObjectContext bool) |ElasticScaleContext (ShardMap TKey, ObjectContext, bool) |DbContext (ObjectContext bool) |Uusi konstruktori on varmistaa, että välitetty syötteeksi ObjectContext mitään yhteyttä uudelleen reititetty yhteyteen hallitsee joustavasti asteikko. Yksityiskohtainen kuvaus ObjectContexts on tämän artikkelin piiriin.
MyContext (DbConnection, DbCompiledModel, bool) |ElasticScaleContext (ShardMap TKey, DbCompiledModel, bool)| DbContext (DbConnection, DbCompiledModel, bool); |Yhteys on voidaan johtaa shard kartta- ja sitten-näppäintä. Yhteyttä ei voi antaa syötteeksi (paitsi jos syötteen jo käytössä, shard kartta- ja sitten-näppäintä). Mallin ja Boolean siirretään perus luokan konstruktoria. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Shard rakenteen käyttöönoton EF siirrot kautta 

Automaattisen rakenteen hallinta on yksikön Framework myöntämä käytön helpottamiseksi. Sovellusten joustavasti Tietokantatyökalut kontekstissa haluamme säilyttää valmistelu juuri luomasi shards mallin automaattisesti, kun tietokantojen lisätään sharded sovelluksen tätä ominaisuutta. Ensisijainen käyttötapaus on lisätä kapasiteetin sharded sovellusten EF tietojen taso-palvelussa. Tietokannan hallinta työmäärään käyttäisit EF-tiedoston ominaisuuksia rakenteen hallinnan vähentää EF rakennettu sharded-sovelluksella. 

Rakenteen käyttöönoton kautta EF siirrot toimii parhaiten **avaamaton yhteydet**. Tämä on toisin kuin tietojen riippuvaiset skenaarion reititys, joka on riippuvainen joustavasti tietokannan asiakkaan API määrittämiä avattu yhteys. Toinen ero on yhdenmukaisuuden vaatimus: toivottavaa varmistaa kaikkien tietojen riippuva reititys yhteyksien Suojaudu samanaikaiset shard kartan käytöstä, kun ei huolta alkuperäinen rakenne käyttöönoton uuteen tietokantaan, joka on vielä rekisteröity shard määrityksen ja vielä kohdistettu pidä shardlets kanssa. Olemme luottavat siksi säännöllisesti tietokantayhteyksiä tässä tilanteessa, eikä tietoja riippuva reititys.  

Tämä johtaa toimintatavan missä rakenteen käyttöönoton EF siirrot kautta on tiiviisti sekä uuden tietokannan rekisteröinnin shard sovelluksen shard rakenneruudussa nimellä. Tämä on riippuvainen seuraavat edellytykset: 

* Tietokanta on jo luotu. 
* Tietokanta on tyhjä – pitää käyttäjä ei ole rakenteen ja ei ole käyttäjätietoja.
* Tietokannan vielä ei voi käyttää joustavasti tietokannan asiakasohjelmalla ohjelmointirajapinnan tietojen riippuva reitittämiseen. 

Nämä edellytykset paikassa, jossa voit luoda Normaali yhdistelmävisualisoinnin avattu **SqlConnection** käyttö EF siirrot rakenteen käyttöönottoa varten. Seuraava koodi malli on kuvattu tämän menetelmän. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

Tässä esimerkissä näkyy menetelmä **RegisterNewShard** , Rekisteröi shard shard määrityksen, ottaa käyttöön rakenteen kautta EF siirrot ja tallentaa shard sharding avain määritystä. Se on riippuvainen konstruktori, **DbContext** aliluokan (**ElasticScaleContext** otoksessa), joka otetaan syötteeksi SQL yhteysmerkkijonon. Tämä konstruktori koodi on suora-eteenpäin, kuten seuraavassa esimerkissä: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Yksi ovat käyttäneet perus luokan perittyjä konstruktoria versio. Mutta koodi on varmistaa, että oletusarvon alustaja EF, käytetään, kun yhteys muodostetaan. Näin ollen lyhyt detour kyselyjä staattinen menetelmä ennen kuin soitat yhteysmerkkijonon perus luokan konstruktoria kyselyjä. Huomaa, että shards rekisteröinnin, pitäisi toimia eri app toimialueen tai prosessi varmistaa, että EF alustaja asetukset eivät ole ristiriidassa. 


## <a name="limitations"></a>Rajoitukset 

Tässä asiakirjassa kuvatut tavoista aiheuttaa pari rajoitukset: 

* EF-sovelluksia, jotka käyttävät **LocalDb** on ensin siirrettävä säännöllisesti SQL Server-tietokantaan ennen kuin käytät joustavasti tietokannan asiakkaan kirjastoon. Laajentaminen ja joustavasti asteikko sovelluksen kautta sharding ei ole mahdollista **LocalDb**. Huomaa, että kehittäminen edelleen käyttää **LocalDb**. 

* Sovelluksen tehdyt muutokset, jotka ilmentävät tietokannan rakenteen muutokset tarvitse käydä läpi EF siirrot-kaikki shards. Tämän asiakirjan mallikoodia ei osoittaa tähän. Kannattaa ehkä käyttää ConnectionString-parametrin päivitys-tietokannan käytöstä kaikki shards; päälle tai poimia päivityksen tietokannan käytön odotetaan siirron T-SQL-komentosarjan – komentosarjan-asetus ja käytä omaa shards T-SQL-komentosarja.  

* Valita pyynnön, sen oletetaan, että kaikki sen tietokannan käsittelyn sisältyy yksittäisen shard yksilöityä pyynnön myöntämä sharding-näppäintä. Kuitenkin tämä oletus ei aina pidä tosi. Esimerkiksi kun se ei ole mahdollista saada sharding näppäintä käytettävissä. Osoitteen tämä asiakas-kirjasto sisältää **MultiShardQuery** -luokka, joka sisältää yhteys-otetaan päälle useita shards kyselyitä varten. Käytön **MultiShardQuery** EF yhdessä oppiminen on tämän artikkelin laajemmin

## <a name="conclusion"></a>Tekemistä

Tässä asiakirjassa kuvattujen vaiheiden EF sovellukset voivat käyttää joustavasti tietokannan asiakkaan kirjaston ominaisuuksien tietoja riippuvaiset reitittämiseen mukaan optimointi käytössä EF sovelluksessa **DbContext** aliluokkien konstruktoreja. Tämä rajoittaa näiden paikkaa, jossa **DbContext** luokkien jo ole tarvittavat muutokset. Lisäksi EF sovellukset voivat jatkaa hyötyvät automaattisen rakenteen käyttöönoton yhdistämällä, jotka kutsuvat tarvittavat EF-siirtojen kanssa rekisteröinnin uuden shards ja yhdistämismääritykset shard määrityksen vaiheet. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 