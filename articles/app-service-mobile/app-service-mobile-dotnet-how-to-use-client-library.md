<properties
    pageTitle="Palvelun mobiilisovellukset hallitun asiakkaan kirjastoa (Windows | Xamarin) | Microsoft Azure"
    description="Opi käyttämään .NET-asiakasohjelman Azure App palvelun mobiilisovellukset Windows- ja Xamarin sovelluksilla."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Miten voit käyttää hallitun asiakkaan Azure-mobiilisovellukset

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa hallitun asiakirjakirjaston käyttäminen Azure palvelun Mobile sovellukset Windowsin sähköpostisovellus ja Xamarin sovellukset. Jos ole ennen käyttänyt Mobile-sovellusten, ota huomioon Viimeistellään ensin [Azure-mobiilisovellukset pikaopas] [ 1] opetusohjelma. Tässä oppaassa on keskittyä asiakkaan hallitun SDK-paketissa. Saat lisätietoja palvelinpuolen SDK: T Mobile-sovellusten ohjeissa [.NET Server SDK] [ 2] tai [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Oppaat

Asiakkaan SDK ohjeessa sijaitsee seuraavassa: [Azure Mobile-sovellusten .NET asiakkaan viittaus][4].
Löytyvät myös useita asiakkaan näytteiden [Azure näytteiden GitHub säilöön][5].

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

.NET-ympäristö tukee seuraavia ympäristöissä:

* Xamarin Android julkaisee API 19 – 24 (KitKat Nougat kautta)
* IOS 8.0 ja uudemmat versiot Xamarin iOS
* Yleinen Windows-ympäristössä
* Windows Phone 8.1
* Windows Phone 8.0 lukuun ottamatta Silverlight-sovellukset

"Tiedonkulun server-todennus käyttää näkymä esitetty UI.  Jos laite ei pysty esittämään näkymä-Käyttöliittymä, muista tavoista todennus tarvitaan.  Tämä SDK ei siis sovellu Watch type tai vastaavasti rajoitettu laitteiden.

##<a name="setup"></a>Asennus- ja edellytykset

Oletetaan, että olet jo luonut ja julkaista projektin Mobile-sovelluksen taustatietokannan, joka sisältää vähintään yhden taulukon.  Tämän artikkelin koodiin, taulukko nimetään `TodoItem` ja siinä on seuraavat sarakkeet: `Id`, `Text`, ja `Complete`. Tämä on sama taulukko luodaan, kun olet suorittanut [Azure-mobiilisovellukset pikaopas].

Vastaavat kirjoitettuja asiakkaan C# on seuraava luokka:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

[JsonPropertyAttribute] [ 6] käytetään määrittämään *PropertyName* yhdistäminen asiakastyypin ja taulukon välille.

Lisätietoja taulukoiden luominen mobiilisovellukset Taustajärjestelmä on artikkelissa [.NET Server SDK aiheen] [ 7] tai [Node.js Server SDK aiheen][8]. Jos olet luonut Mobile-sovelluksen Taustajärjestelmä käyttämällä pikaopas Azure-portaalissa, voit käyttää myös **helposti taulukot** -asetus [Azure portal].

###<a name="how-to-install-the-managed-client-sdk-package"></a>Toimintaohje: Asenna hallitun asiakkaan SDK-paketti

Käytä jollakin seuraavista tavoista asentaminen hallitun asiakkaan SDK-paketti, [NuGet]-mobiilisovellukset[9]:

+ **Visual Studio** Projektin hiiren kakkospainikkeella, valitse **NuGet pakettien hallinta**, Etsi `Microsoft.Azure.Mobile.Client` pakkaaminen ja valitse sitten **Asenna**.

+ **Xamarin Studio** Projektin hiiren kakkospainikkeella, valitse **Lisää** > **NuGet pakettien**, Etsi `Microsoft.Azure.Mobile.Client `pakkaaminen ja valitse sitten **Lisää paketin**.

Tiedostossa pääasiallista muista lisätä seuraavan **käyttäminen** lauseen:

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Toimintaohje: virheenkorjaussymbolit Visual Studiossa käsitteleminen

Microsoft.Azure.Mobile nimitilan merkit ovat käytettävissä [SymbolSource][10].  [SymbolSource ohjeita] [ 11] integroiminen SymbolSource Visual Studiossa.

##<a name="create-client"></a>Asiakkaan Mobile-sovellusten luominen

Seuraava koodi luo [MobileServiceClient] [ 12] objekti, jota käytetään käyttämään Mobile-sovelluksen Taustajärjestelmä.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Korvaa edellisen koodin `MOBILE_APP_URL` ja Mobile-sovelluksen Taustajärjestelmä URL-osoite, joka löytyy for Mobile-sovelluksen-taustatietokannan [Azure portal]-sivu. MobileServiceClient-objekti on oltava Yksiarvoinen.

## <a name="work-with-tables"></a>Taulukoiden käsitteleminen

Seuraavassa osassa kuvataan, miten haun ja tietueiden hakeminen ja muokata taulukon tietoja.  Käsitellään seuraavia aiheita:

* [Taulukkoviittauksen luominen](#instantiating)
* [Kyselyn tiedot](#querying)
* [Palautetut tietojen suodattaminen](#filtering)
* [Palauttaa tietojen lajitteleminen](#sorting)
* [Sivujen tietojen palauttaminen](#paging)
* [Valitse tietyt sarakkeet](#selecting)
* [Tietueen tunnuksen mukaan hakeminen](#lookingup)
* [Tyypittämättömän kyselyjen käsittely](#untypedqueries)
* [Tietojen lisääminen](#inserting)
* [Tietojen päivittäminen](#updating)
* [Tietojen poistaminen](#deleting)
* [Ristiriitojen ratkaisu ja optimistisen](#optimisticconcurrency)
* [Sidonta Windows-käyttöliittymä](#binding)
* [Sivun koon muuttaminen](#pagesize)

###<a name="instantiating"></a>Toimintaohje: taulukkoviittauksen luominen

Kaikki koodia, joka käyttää tai Muokkaa Taustajärjestelmä taulukon tietojen kutsuu funktiot `MobileServiceTable` objekti. Hanki taulukon viittaa soittamalla [GetTable] -menetelmä seuraavasti:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Palautettu objekti käyttää tavallista Sarjatoiminto-malli. Tyypittämättömän Sarjatoiminto mallin tuetaan myös. Seuraava esimerkki [Luo viittaus tyypittämättömän taulukkoon]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

Tyypittämättömän kyselyissä sinun on määritettävä pohjana OData-kyselymerkkijonon.

###<a name="querying"></a>Toimintaohje: kyselyn Mobile-sovelluksesta

Tässä osassa kuvataan, miten kyselyjen antamaan Mobile-sovelluksen taustatietokannan, joka sisältää seuraavat ominaisuudet:

- [Palautetut tietojen suodattaminen](#filtering)
- [Palauttaa tietojen lajitteleminen](#sorting)
- [Sivujen tietojen palauttaminen](#paging)
- [Valitse tietyt sarakkeet](#selecting)
- [Tunnuksen mukaan tietojen hakemiseen](#lookingup)

>[AZURE.NOTE]Palvelimen perustuva sivukoko pakotetaan voit estää kaikki rivit palautetaan.  Sivutus pitää suurten tietojoukkojen pyyntö oletusarvo-vaikuttavat heikentää palvelu.  Palauttaa yli 50 riviä `Skip` ja `Take` menetelmä kuvatulla tavalla [Palauta tiedot sivut].

###<a name="filtering"></a>Toimintaohje: Suodatin palautti tietoja

Seuraava koodi kuvataan, miten voit suodattaa tietoja lisäämällä `Where` lauseen kyselyn. Se palauttaa kaikki kohteet `todoTable` joiden `Complete` -ominaisuus on yhtä suuri kuin `false`. [Jos] -funktion koskee lause taulukon kysely suodattaminen rivin.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Voit tarkastella käyttämällä viestin tarkastus-ohjelmistoa, kuten selaimen Kehitystyökalut tai [Fiddler]taustaan lähettämä pyyntö URI. Jos tarkastelet pyynnön URI-Huomaa, että kyselymerkkijonon on muokattu:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

OData-pyyntö käännetään SQL-kyselyn Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Funktio, joka siirretään `Where` menetelmää voi olla haluamaansa useita ehtoja.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

Tässä esimerkissä kääntää SQL-kyselyyn Server SDK:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Tämä kysely voi jakaa myös useita lausekkeita:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Kaksi tapaa vastaavat ja voidaan käyttää tarkoittavat.  Entisen vaihtoehto&mdash;yhdistämisen kyselyssä useita predikaatit&mdash;on tiivis ja suositellut.

`Where` Lauseen tukee toiminnoista, joita voi kääntää OData-osajoukko. Toiminnot ovat seuraavat:

* Relaatio-operaattoreita (==,! =, <, < =, >, > =),
* Aritmeettisia operaattoreita (+, -, /, *, %),
* Numeron tarkkuus (Math.Floor, Math.Ceiling)
* Merkkijonofunktiot (pituus, alimerkkijono, korvaa, IndexOf, StartsWith ja EndsWith)
* Päiväys (vuosi, kuukausi, päivä, tunti, minuutti, toisen)-ominaisuudet
* Access objektin ominaisuudet ja
* Lausekkeet yhdistämällä seuraavia toimia.

Kun Server SDK tukee, harkitse [OData v3 ohjeissa].

###<a name="sorting"></a>Toimintaohje: Lajittele palautti tietoja

Seuraava koodi on kuvattu tietojen lajittelemisesta lisäämällä kyselyyn [Lajittelu] - tai [OrderByDescending] -funktiota. Se palauttaa kohteita `todoTable` lajiteltu nousevaan järjestykseen mukaan `Text` kentän.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Toimintaohje:-sivujen tietojen palauttaminen

Oletusarvon mukaan taustaan palauttaa vain tiedoston ensimmäiset 50 riviä. Voit suurentaa palautetut rivien määrä kutsumista [kestää] . Käytä `Take` sekä pyytää tietyn "sivu" yhteensä tietojoukon kysely palauttaa [Ohita] -menetelmää. Seuraava kysely suoritetaan, kun palauttaa ensimmäiset kolme kohdetta taulukossa.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Muokattu kysely ohittaa kolme ensimmäistä tulokset ja palauttaa seuraavat kolme tulokset. Kysely tuottaa toinen "sivu" tiedot, jossa sivun koko on kolme kohdetta.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

[IncludeTotalCount] -menetelmä pyytää _kaikkien_ kokonaismäärä tietueet, jotka on palautettu, ohittaa kaikki määritetyn sivutus/raja-lause:

    query = query.IncludeTotalCount();

Käytännön-sovelluksessa voit kyselyjen samalla tavalla kuin edellisessä esimerkissä hakulaite ohjausobjektin tai vastaa Käyttöliittymän Siirtyminen sivulta toiselle.

>[AZURE.NOTE]Ohittamaan 50 – Rivirajan-Mobile-sovelluksen taustassa on myös käyttää [EnableQueryAttribute] julkisen GET-menetelmällä ja määritä sivutus toiminnan. Kun käytettävä menetelmä palautetaan 1000 rivien enimmäismäärä määrittää seuraavasti:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Toimintaohje: Valitse tietyt sarakkeet

Voit määrittää, mitä ominaisuuksia joukkoa sisällyttää lisäämällä [Valitse] lause kyselyn tuloksiin. Esimerkiksi seuraava koodi näkyy vain yhden kentän valitseminen ja voit valita ja muotoilla useita kenttiä:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Kaikki tähän mennessä on kuvattu Funktiot ovat vapaasti lisättäviä, joten emme voi pitää ketjutus ne. Jokainen ketjutetut kutsu vaikuttaa Lisää kyselyn. Yksi esimerkki:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Toimintaohje: tunnuksen mukaan tietojen hakemiseen

[LookupAsync] -funktion avulla voidaan etsiä objektien tietokantaa, jossa on tietty.

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Toimintaohje: tyypittämättömän kyselyjen suorittaminen

Kun suoritat kyselyn, käyttämällä tyypittämättömän taulukko-objekti, sinun on määritettävä OData-kyselymerkkijonon soittamalla [ReadAsync], kuten seuraavassa esimerkissä:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Voit palata JSON-arvot, joiden avulla voit esimerkiksi ominaisuuden kantolaukku. Saat lisätietoja JToken ja Newtonsoft Json.NET [Json.NET] -sivustossa.

### <a name="inserting"></a>Toimintaohje: tietojen lisääminen Mobile-sovelluksen taustassa

Kaikki asiakastyyppien on oltava jäsen, jonka **tunnus**, joka on oletusarvoisesti merkkijonon nimi. Tämä **ID-tunnusta** tarvitaan suorittaa CRUD ja offline-synkronoinnin. Seuraava koodi on kuvattu uusien rivien lisääminen taulukkoon [InsertAsync] -menetelmällä. Parametri sisältää tiedot lisätään .NET-objekteina.

    await todoTable.InsertAsync(todoItem);

Jos yksilöllinen mukautettu tunnusarvo ei sisällytetä `todoItem` aikana Lisää GUID-tunnus on luonut palvelimeen.
Voit hakea tunnusta poistamiseksi objektia, kun kutsu palauttaa.

Jos haluat lisätä tyypittämättömän tiedot, ehkä käytä Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Tässä on esimerkki käyttämällä sähköpostiosoitteen yksilöllinen merkkijono ID:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Tunnus-arvojen käsitteleminen

Mobile-sovellusten tukee yksilöllinen mukautetun merkkijonoarvoa **taulukon tunnussarake** . Merkkijonoarvo avulla voit käyttää mukautettua arvot, kuten sähköpostiosoitteet tai käyttäjänimet tunnuksen sovellukset  Merkkijono tunnuksista on seuraavat edut:

* Tunnukset luodaan ilman, että matkassa tietokantaan.
* Tietueet on helpompi yhdistää eri taulukoista tai tietokantoihin.
* Tunnukset arvoja voi integroida paremmin sovelluksen logiikan.

Kun tunnus arvoksi ei ole lisätty tietueen, Mobile-sovelluksen Taustajärjestelmä Luo yksilöllinen arvo tunnuksen Voit luoda omat tunnukset, asiakkaan tai taustaan [Guid.NewGuid] -menetelmää.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Toimintaohje: Mobile-sovelluksen taustassa tietojen muokkaaminen

Seuraava koodi kuvataan, miten [UpdateAsync] menetelmän avulla voit päivittää tietuetta samalla tunnuksella uusilla tiedoilla. Parametrin sisältää tiedot päivitetään .NET-objekteina.

    await todoTable.UpdateAsync(todoItem);

Tyypittämättömän tietojen päivittämiseen ehkä käytä [Json.NET] seuraavasti:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

`id` Kenttä on määritettävä, kun teet päivitys. Taustaan käyttää `id` kentän tunnistaa päivittäminen riviä. `id` Saadaan kentän tulos `InsertAsync` Soita. `ArgumentException` Käynnistyy, jos yrität päivittää kohteen mutta estää `id` arvo.

###<a name="deleting"></a>Toimintaohje: Mobile-sovelluksen taustassa tietojen poistaminen

Seuraava koodi kuvataan, miten [DeleteAsync] menetelmän avulla voit poistaa olemassa olevaan esiintymään. Esiintymän tunnistetaan `id` kentän arvoksi on asetettu `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Voit poistaa tyypittämättömän tietojen ehkä käytä Json.NET seuraavasti:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Kun Poista pyydettävä tunnus on määritettävä. Muita ominaisuuksia ei siirretä palveluun tai palveluun osoitteessa ohitetaan. Tulos `DeleteAsync` kutsu on yleensä `null`. Siirtää tunnus voi hakea tulos `InsertAsync` Soita. A `MobileServiceInvalidOperationException` ilmenee, kun yrität poistaa lähdeluettelosta kohdetta, määrittämättä `id` kentän.

###<a name="optimisticconcurrency"></a>Toimintaohje: Käytä Optimistinen samanaikainen ristiriitojen ratkaiseminen

Kahden tai useamman asiakkaat voivat kirjoittaa muutokset samaan kohteeseen samanaikaisesti. Ristiriitojen tunnistus ilman viimeisen kirjoittaminen korvaa edellisen päivityksiä. **Optimistisen ohjausobjektin** oletetaan, että myyntitapahtumien voit vahvistaa, ja näin ollen ei käytä minkä tahansa resurssin lukitus.  Ennen kuin vahvistetaan tapahtuman optimistisen ohjausobjektin tarkistaa muun tapahtuman on muokannut tietoja. Jos tiedot on muokattu, committing tapahtuma on peruutettu.

Mobile-sovellusten tukee optimistisen ohjausobjektin muutosten jäljittäminen kunkin kohteen käyttäen `version` järjestelmän ominaisuuden sarake, joka on määritetty jokaiselle pinon Mobile-sovelluksen Taustajärjestelmä. Aina, kun tietue on päivitetty, Mobile-sovellusten asettaa `version` ominaisuuden tietueen ja uusi arvo. Sivupyynnön päivityksen aikana `version` ominaisuuden pyynnön mukana tietueen verrataan saman ominaisuuden tietueen palvelimessa. Jos versio välitetty kanssa pyynnön vastaa taustaan ja valitse asiakirjakirjaston aiheuttaa `MobileServicePreconditionFailedException<T>` poikkeuksen. Poikkeuksen mukana tyyppi on taustasta, joka sisältää tietueen palvelimet-versio. Sovelluksen voit käyttää näitä tietoja päättää, päivitys-pyyntö uudelleen käyttämällä oikean `version` arvo taustasta muutokset.

Sarakkeen määrittäminen taulukon luokka `version` järjestelmäominaisuus käyttöön optimistisen. Esimerkki:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Sovellusten tyypittämättömän taulukoiden käyttöön optimistisen määrittämällä `Version` Merkitse `SystemProperties` taulukon toimimalla seuraavasti.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Lisäksi optimistisen on otettu käyttöön, sinun on myös todellisen `MobileServicePreconditionFailedException<T>` poikkeuksen koodissa [UpdateAsync]soitettaessa.  Ratkaise ristiriita käyttämällä oikean `version` päivitetty tietue ja ratkaista tietueen [UpdateAsync] puhelua. Seuraava koodi näytetään, miten voit korjata kirjoitus ristiriitoja havaittu kerran:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Lisätietoja on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa] .

###<a name="binding"></a>Toimintaohje: Tietojen sidonta-mobiilisovellukset Windows-käyttöliittymä

Tässä osassa esitellään näyttäminen palautetut tiedot objektien Käyttöliittymän osat Windows-sovelluksessa.  Seuraava esimerkkikoodi sitoo puutteellinen kohteiden kysely luettelomuotoisena lähde. [MobileServiceCollection] Luo Mobile sovellukset-aware sidonta-sivustokokoelman.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Jotkin ohjaimet hallitun suorituksen aikana tukevat käyttöliittymään kutsutaan [ISupportIncrementalLoading]. Ohjausobjektien pyytää lisätiedot, kun käyttäjä vierittää avulla. Yleinen Windows-sovellusten kautta [MobileServiceIncrementalLoadingCollection], joka käsittelee automaattisesti ohjausobjektit kutsut liittymän valmiin tueta. Käytä `MobileServiceIncrementalLoadingCollection` Windows-sovellusten seuraavasti:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Käyttää uuden kokoelman Windows Phone 8 ja "Silverlight-sovellukset, `ToCollection` tunniste menetelmiä `IMobileServiceTableQuery<T>` ja `IMobileServiceTable<T>`. Lataa tiedot, soita `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Kun käytät luotu kutsumalla sivustokokoelman `ToCollectionAsync` tai `ToCollection`, saat kokoelma, joka on sidottu Käyttöliittymän ohjausobjekteja.  Tämä kokoelma on sivutus tietoinen siitä.  Koska kokoelman lataa tiedot verkosta, joskus lataaminen epäonnistuu. Näiden virheiden ohittaa `OnException` menetelmän `MobileServiceIncrementalLoadingCollection` käsittelemään poikkeukset puhelut tuloksena `LoadMoreItemsAsync`.

Mieti, jos taulukossa on useita kenttiä, mutta haluat näyttää jotkin niistä ohjausobjektissa. "[Valitse tietyt sarakkeet](#selecting)" edellisen osion ohjeet avulla voi valita tietyt sarakkeet näyttääksesi käyttöliittymässä.

###<a name="pagesize"></a>Sivun koon muuttaminen

Azure Mobile-sovellusten palauttaa enintään 50 kohdetta kussakin pyynnön oletusarvoisesti.  Voit muuttaa sivutus kokoa kasvattamalla sivun enimmäiskooksi asiakkaan ja palvelimen.  Pyydetty sivun kokoa, määrittää `PullOptions` käytettäessä `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Oletetaan, että olet tehnyt `PageSize` yhtä suuri kuin tai suurempi kuin 100 palvelimen sisällä, pyyntö palauttaa enintään 100 kohteet.

##<a name="#offlinesync"></a>Offline-tilassa taulukoiden käsitteleminen

Offline-tilassa taulukoiden käyttäminen paikallisten SQLite säilö-säilö tietojen käyttäminen offline-tilassa.  Kaikki taulukon tehdään vastaan paikallisen SQLite tallentaa etäpalvelimeen säilön sijaan.  Offline-tilassa taulukon luomiseen valmisteleminen projektin:

1. Visual Studiossa, napsauta hiiren kakkospainikkeella ratkaisun > **Ratkaisu... NuGet pakettien hallinta**ja valitse Etsi ja asenna **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet pakkaaminen ratkaisun kaikissa projekteissa.

2. (Valinnainen) Asenna jokin SQLite runtime-pakettien tukemaan Windows-laitteet:

    * **Windows 8.1 Runtimen:** Asenna [Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Asenna [Windows Phone 8.1 SQLite][4].
    * **Yleinen Windows-ympäristössä** Asenna [Yleinen Windows-Universal SQLite][5].

3. (Valinnainen). Windows-laitteita, valitse **viittaukset** > **Lisää viittaus...**, laajenna **Windows** -kansioon > **tunnisteet**ja valitse Ota käyttöön haluamasi **SQLite Windows** SDK sekä **Visual C++ 2013 Runtimen Windows** SDK.
    SQLite SDK nimet vaihtelevat hieman kussakin Windows-ympäristössä.

Ennen kuin taulukkoviittauksen voidaan luoda paikallista on valmisteltava:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Säilön alustus tehdään yleensä heti, kun asiakas on luotu.  **OfflineDbPath** pitäisi käyttää kaikissa ympäristöissä, jotka olet tukevat tiedostonimi.  Jos polku on täydellinen polku (eli se alkaa vinoviiva), valitse polku on käytössä.  Jos polku ei ole täydellinen, tiedosto sijoitetaan käyttöjärjestelmäkohtaiset sijainti.

* IOS-ja Android-laitteille oletusarvoisen polun on "Omat tiedostot-kansio.
* Windows-laitteiden oletusarvoisen polun on sovelluksen kielikohtaiset "AppData-kansio.

Taulukkoviittauksen saadaan käyttämällä `GetSyncTable<>` menetelmää:

    var table = client.GetSyncTable<TodoItem>();

Sinun ei tarvitse suorittaa todennusta offline-taulukkoa.  Tarvitset vain todennetaan viestinnässä Taustajärjestelmä-palvelussa.

###<a name="syncoffline"></a>Offline-tilassa taulukon synkronointi

Offline-taulukoita ei synkronoida kanssa taustaan oletusarvoisesti.  Synkronointi on jaettu kahteen osaan.  Voit push muutokset erikseen lataamisen uudet kohteet.  Tässä on tyypillinen synkronointi-menetelmää:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Jos ensimmäinen argumentti `PullAsync` on tyhjä, ja valitse vaiheittainen synkronointi ei käytetä.  Kunkin Synkronointitoiminto palauttaa kaikki tietueet.

SDK suorittaa implisiittisen `PushAsync()` ennen tietojen tietueita.

Ristiriita käsittely tapahtuu `PullAsync()` menetelmää.  Voit käsitellä ristiriidat online taulukoina samalla tavalla.  Ristiriita on valmistettu kun `PullAsync()` kutsutaan sijaan aikana Lisää, Päivitä tai poista. Jos useita ristiriitoja aktivoitu, ne on koottu yhteen MobileServicePushFailedException.  Käsittele kunkin virheen erikseen.

##<a name="#customapi"></a>Mukautetun Ohjelmointirajapinnan käyttäminen

Mukautetun API avulla voit määrittää mukautetun päätepisteet, joka näyttää server-toiminto, joka ei ole insert yhdistäminen, päivittäminen, poistaminen tai luku. Käyttämällä mukautettuja API voit määrittää tarkemmin messaging, mukaan lukien lukeminen ja määrittäminen HTTP viestiotsikot ja määrittämällä viestin tekstiosaan muodon kuin JSON.

Voit soittaa mukautetun API soittamalla [InvokeApiAsync] tavoilla asiakkaan. Esimerkiksi seuraava rivi koodin lähettää viestin pyynnön **completeAll** API taustassa:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Tähän lomakkeeseen on kirjoitettuja menetelmää puhelu ja edellyttää, että **MarkAllResult** palautustyyppi on määritetty. Kirjoitettuja- ja tyypittämättömän menetelmiä tuetaan.

##<a name="authentication"></a>Tarkista käyttöoikeudet

Mobile-sovellusten tukee käyttöoikeudet ja myöntää sovelluksen käyttäjille käyttämällä eri ulkoisen tunnistetietojen toimittajat: Facebook, Google, Microsoft-Account, Twitter ja Azure Active Directory. Voit määrittää käyttöoikeudet vain todennetut käyttäjät toiminnoista käytön taulukot. Voit käyttää myös todennettujen käyttäjien käyttäjätietojen toteuttavien palvelimen komentosarjojen käyttöoikeuksien myöntämissääntöjä. Lisätietoja on kohdassa opetusohjelma [todennusta Lisää sovellus].

Kaksi todennus kulkee tuetaan: _asiakkaan hallitaan_ ja _palvelimen hallitaan_ . Palvelimen hallittu-työnkulku on helpoin todennus-toiminto, kun se on riippuvainen tarjoajan web todennus-liittymän. Asiakkaan hallitun kulun sallii tarkempaa laitekohtaiset ominaisuudet-integrointia varten se on riippuvainen toimittajakohtaiset laitekohtaiset SDK: T.

>[AZURE.NOTE] On suositeltavaa käyttää asiakkaan hallitun työnkulku tuotannon-sovelluksissa.

Jos haluat määrittää käyttöoikeuden, sinun on rekisteröitävä sovelluksen kanssa vähintään yksi tunnistetietojen toimittajat.  Tunnistetietojen toimittaja Luo asiakkaan-tunnus ja asiakas-salaisuus, kun sovellus.  Nämä arvot määritetään sitten Azure App palvelun todennus/todennus käyttöön oman Taustajärjestelmä.  Saat lisätietoja noudattamalla tarkkoja opetusohjelmassa [todennusta Lisää sovellus].

Tässä osassa käsitellään on seuraavissa artikkeleissa:

+ [Asiakkaan rajoitetun käyttöoikeuden](#clientflow)
+ [Palvelimen rajoitetun käyttöoikeuden](#serverflow)
+ [Todennus-tunnuksen välimuistiin tallentaminen](#caching)

###<a name="clientflow"></a>Asiakkaan rajoitetun käyttöoikeuden

Sovelluksen voi tarjoajalta tunnistetietojen itsenäisesti ja anna palautetut tunnuksen kirjautumisen yhteydessä oman Taustajärjestelmä kanssa. Asiakas-työnkulku mahdollistaa ja tarjota käyttäjille yksittäisen Sign-käyttökokemuksen tai muokkaalinkki tietojen hakemiseen tunnistetietojen toimittaja. Asiakkaan työnkulku todennus on ensisijainen palvelin-työnkulku käyttämällä tunnistetietojen toimittaja SDK sisältää Lisää alkuperäisen UX ilmeen ja mahdollistaa muita mukauttaminen.

Esimerkkejä on tarkoitettu asiakkaan työnkulku todennus-kuvioita:

+ [Active Directory käyttöoikeuksien kirjastoon](#adal)
+ [Facebook- tai Google](#client-facebook)
+ [Live SDK-paketissa](#client-livesdk)

#### <a name="adal"></a>Tarkista käyttöoikeudet Active Directory käyttöoikeuksien-kirjaston kanssa

Voit käyttää Active Directory käyttöoikeuksien kirjaston (ADAL) Valmistele käyttäjien todentamiseen Azure Active Directory-todennusta asiakasohjelmassa.

1. Määritä mobiilisovelluksen-taustatietokannan, AAD Sign [määrittäminen sovelluksen palvelun Active Directory-kirjautumiset] -opetusohjelma. Varmista, että Viimeistele valinnainen vaihe native client-sovelluksen.
2. Visual Studio tai Xamarin Studio, Avaa projektiin ja Lisää viittaus `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet paketti. Kun etsit, Sisällytä ennakkoversion versiot.
3. Lisää seuraava koodi sovelluksen, käytössäsi on käyttöympäristön mukaan. Tee kunkin, seuraavat korvauksia:

    * Korvaa **Lisää MYÖNTÄJÄ-tähän** nimi, jossa sovellus on valmisteltu vuokraajan. Muoto on oltava https://login.windows.net/contoso.onmicrosoft.com. Tämän arvon voi kopioida [Azure perinteinen portal]Azure Active Directory-toimialueen-välilehdestä.
    * **Lisää-resurssi-tunnus-tähän** tilalle mobiilisovelluksen Taustajärjestelmä Asiakastunnus. Voit hankkia Asiakastunnus-portaalissa **Azure Active Directory-asetukset** -kohdassa **Lisäasetukset** -välilehti.
    * **Lisää-asiakas-tunnus-tähän** tilalle alkuperäisen asiakassovellus kopioitu Asiakastunnus.
    * **Lisää-REDIRECT-URI-tähän** tilalle sivuston _/.auth/login/done_ päätepisteen HTTPS-mallin avulla. Tämän arvon pitäisi olla samanlainen _https://contoso.azurewebsites.net/.auth/login/done_.

    Tarvittavat kunkin ympäristö koodin seuraavasti:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Kertakirjautumisen Facebookista tai Google tunnusta

Voit käyttää asiakkaan työnkulku tämän koodikatkelman Facebook-tai Google esitetyllä tavalla.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Yksittäisen Kirjaudu sisään käyttämällä Microsoft-Account kanssa Live SDK

Tarkista käyttöoikeudet, sinun on rekisteröitävä sovelluksen Microsoft-tiliä Developer Center-palvelussa. Mobile-sovelluksen taustassa määrittämistä rekisteröinnin tiedot. Voit luoda Microsoft-tilin rekisteröiminen ja yhdistä se Mobile-sovelluksen taustatietokannan, Suorita vaiheet [rekisteröidä sovelluksen käyttämään Microsoft-tilin käyttäjätunnus]. Jos sinulla on Windows-kaupan ja Windows Phone 8/Silverlight-versioiden sovelluksesi, Rekisteröi Windows-kaupan-versio.

Seuraava koodi todentaa Live SDK: N avulla ja käyttää palautetut tunnuksen kirjautuminen Mobile-sovelluksen Taustajärjestelmä.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Lisätietoja on [Windows Live SDK] ohjeissa.

###<a name="serverflow"></a>Palvelimen rajoitetun käyttöoikeuden

Kun olet rekisteröinyt tunnistetietojen toimittaja, soita [MobileServiceAuthenticationProvider] tarjoajan arvona [MobileServiceClient] [LoginAsync] -menetelmää. Esimerkiksi seuraava koodi käynnistää palvelimen työnkulku-kirjautuminen käyttämällä Facebook.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, muuta [MobileServiceAuthenticationProvider] arvo arvoksi palveluntarjoajan.

Palvelin-työnkulussa Azure App palvelun hallitsee OAuth todennus kulun näyttämällä valitun palvelun kirjautumissivulla.  Kun tunnistetietojen toimittaja palauttaa Azure App palvelun Luo sovelluksen Service-todennus-tunnuksen. [LoginAsync] -menetelmä palauttaa [MobileServiceUser], joka sisältää sekä todennetun käyttäjän [käyttäjätunnus] ja [MobileServiceAuthenticationToken]kuin JSON web-tunnuksen (JWT). Tunnuksessa voidaan välimuistin ja käyttää uudelleen, ennen kuin se päättyy. Lisätietoja on artikkelissa [välimuistin todennus-tunnuksen](#caching).

###<a name="caching"></a>Todennus-tunnuksen välimuistiin tallentaminen

Joissakin tapauksissa kirjautuminen menetelmä puhelu voidaan välttää ensimmäisen onnistuneen todennuksen jälkeen tallentamalla toimittajan todennus-tunnuksen.  Windows-kaupan ja UWP käyttää [PasswordVault] välimuistiin nykyisen tunnuksen todennus onnistunut-kirjautuminen, kun seuraavasti:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

Käyttäjätunnus-arvo on tallennettu tunnistetieto käyttäjänimi ja tunnus on tallennettu salasana. Seuraavien toteuttaminen tarkistaa **PasswordVault** välimuistiin tallennetut tunnistetiedot. Seuraavassa esimerkissä välimuistissa olevia käyttäjätietoja löytyy ja yrittää muussa todentamismenetelmä taustaan uudelleen:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Kun kirjaudut ulos käyttäjän, sinun on poistettava myös tallennettuja tunnistetietoja seuraavasti:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin sovellukset tunnistetietojen tallentaminen turvallisesti **tilin** objektin [Xamarin.Auth] API avulla. Esimerkki käytöstä nämä API-kohdassa [ContosoMoments valokuvien jakamisen otoksen] [AuthStore.cs] koodi-tiedosto.

Kun asiakkaan rajoitetun käyttöoikeuden, voit myös välimuistiin palveluntarjoajan, kuten Facebookissa tai Twitter saatu käyttöoikeustietue. Tunnuksessa on toimitettava pyytää uuden todennus suojaustunnuksen taustasta, seuraavasti:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Push-ilmoitukset

Seuraavat ohjeaiheet käsittelevät Push-ilmoitukset:

* [Rekisteröidy Push-ilmoitukset](#register-for-push)
* [Hanki Windows-kaupan paketin SUOJAUSTUNNUS](#package-sid)
* [Voit rekisteröidä Office kaikissa ympäristöissä mallit](#register-xplat)

###<a name="register-for-push"></a>Toimintaohje: Rekisteröidy Push-ilmoitukset

Asiakkaan Mobile-sovellusten avulla voit push-ilmoitukset Azure ilmoituksen keskittimet Rekisteröidy. Rekisteröinti, saat kahvaa, voit hankkia kohteesta käyttöjärjestelmäkohtaiset Push-ilmoituksen Service (PNS). Arvoksi sekä mahdolliset tunnisteet anna sitten, kun luot rekisteröinnin. Seuraava koodi rekisteröi Windows-sovellus push-ilmoituksia Windowsin ilmoituksen Service (WNS) kanssa:

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Jos sinulla on valitseminen WNS, valitse sinun täytyy [hankkia Windows-kaupan paketin SUOJAUSTUNNUS](#package-sid).  Saat lisätietoja Windows-sovelluksista, kuten Rekisteröidy mallin-merkintöjä, [Lisää push-ilmoituksia, jotka sovelluksen].

Pyytää tunnisteet asiakaskoneesta ei tueta.  Tunnisteen pyynnöt äänettömästi olevat kohteet poistetaan rekisteröinnin.
Jos haluat rekisteröidä laitteen tunnisteita, Luo mukautettu-Ohjelmointirajapinta, joka käyttää ilmoituksen keskittimet Ohjelmointirajapinnan suorittamiseen rekisteröinnin puolestasi.  Sen sijaan, että [Mukautettu Ohjelmointirajapinnan kutsu](#customapi) `RegisterNativeAsync()` menetelmää.

###<a name="package-sid"></a>Toimintaohje: hankkia Windows-kaupan paketin SUOJAUSTUNNUS

Paketin SUOJAUSTUNNUS tarvitaan Windows-kaupan sovellukset push-ilmoitusten ottaminen käyttöön.  Saat paketin SUOJAUSTUNNUS, voit rekisteröidä sovelluksen Windows-kaupan.

Voit hankkia tämän arvon:

1. Napsauta hiiren kakkospainikkeella Visual Studio ratkaisunhallinnassa Windows-kaupan sovelluksen projektin, valitse **Tallenna** > **Liittää App Store...**.
2. Ohjatun Valitse **Seuraava**, kirjaudu sisään käyttämällä Microsoft-tiliä, kirjoita sovelluksesi nimi **Varaa uuden sovelluksen nimi**ja valitse sitten **Varaa**.
3. Sen jälkeen, kun sovellus rekisteröinti on luotu, valitse sovelluksen nimen, valitse **Seuraava**ja valitse sitten **Liitä**.
4. Kirjaudu sisään käyttämällä Microsoft-Account [Windows keskihajonta keskelle] . **Omat sovellukset**-kohdassa loit sovelluksen rekisteröintiä.
5. Valitse **sovellusten hallinta** > **sovelluksen käyttäjätieto**- ja sitten vierittämällä alaspäin Etsi oman **Paketin SUOJAUSTUNNUS**.

SUOJAUSTUNNUS-paketin monenlaisia käsitellä sitä URI, jolloin sinun on käytettävä _ms-sovellus: / /_ mallina. Kirjoita muistiin paketti SUOJAUSTUNNUS muotoiltu ketjuttamalla arvoksi etuliitteenä versio.

Xamarin sovellukset edellyttävät muita lisäkoodin voi rekisteröidä sovellus iOS-tai Android-ympäristöissä käynnissä. Lisätietoja on aiheessa omaa ympäristöäsi varten:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Toimintaohje: Rekisteröi push-mallit, Office kaikissa ympäristöissä ilmoitukset lähetetään

Voit rekisteröidä malleja `RegisterAsync()` menetelmä malleja seuraavasti:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Mallisi pitäisi olla `JObject` kirjoittaa ja se voi sisältää useita malleja JSON-muodossa:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Menetelmä **RegisterAsync()** hyväksyy myös toissijaisen ruudut:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Kaikki tunnisteet poistetaan poissa arvopaperille rekisteröinnin yhteydessä. Asennuksia tai asennusten mallit tunnisteet lisäämisestä on ohjeaiheessa [käsitteleminen .NET palvelimeen SDK Azure Mobile-sovellusten].

Viittaavat käyttävien rekisteröityjen mallien ilmoitukset lähetetään [Ilmoitus keskittimet API].

##<a name="misc"></a>Muita ohjeita

###<a name="errors"></a>Toimintaohje: käsitellä virheitä

Kun taustaan tapahtuu virhe, asiakkaan SDK aiheuttaa `MobileServiceInvalidOperationException`.  Seuraavassa esimerkissä esitetään, jonka taustaan palauttaa poikkeuksen käsittelemisestä:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Toinen esimerkki käsittelystä virhetilanteita löytyvät [Mobile sovellusten tiedostot malli]. Esimerkki [LoggingHandler] on kirjaaminen edustajan käsittelijä (seuraava) kirjautua taustaan tehdään pyynnöt.

###<a name="headers"></a>Toimintaohje: mukauttaminen pyytää otsikot

Tukemaan tietyn sovelluksen-skenaario voit joutua muuttamaan mukauttaminen tietoliikenteen taustaan Mobile-sovelluksen kanssa. Haluat ehkä mukautetun otsikon lisääminen jokaisen lähtevän pyynnön tai jopa vastaukset tilakoodin. Voit käyttää mukautettu [DelegatingHandler], kuten seuraavassa esimerkissä:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Käyttöoikeuksien lisääminen sovelluksen]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md
[Sovelluksen lisääminen push-ilmoitukset]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Rekisteröi sovelluksen käyttämään Microsoft-tilin käyttäjätunnus]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Sovelluksen palvelun määrittäminen Active Directory-kirjautuminen]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[Luo viittaus tyypittämättömän taulukkoon]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[Lajittelu]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Ota]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Valitse]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Ohita]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Käyttäjätunnus]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Jos]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portal]: https://portal.azure.com/
[Azure perinteinen portal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windowsin keskihajonta Center]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK-paketissa]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Ilmoitus keskittimet API]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobiilisovellusten tiedostot-Esimerkki]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentaatio]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments valokuvan jakaminen-Esimerkki]: https://github.com/azure-appservice-samples/ContosoMoments