<properties 
    pageTitle="Azure haun Developer Esimerkkitapaus: Kuinka WhatToPedia rakennettu infomedia portal Microsoft Azure | Microsoft Azure | Isännöityjen pilvipalvelussa haku" 
    description="Opettele luominen tiedot portaalia ja metatiedot hakukone Azure hakutoiminto, cloud isännöidään hakupalvelu kehittäjille." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Azure haun Kehitystyökalut-Esimerkkitapaus

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Miten [WhatToPedia.com](http://whattopedia.com/) rakennettu Microsoft Azure infomedia-portaalissa

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Suuri idea</font> 


Tutustu verrata on muodostaa tiedot-portaalissa, joka auttaa shoppers jälleenmyyjät kautta erittäin asiaa, kohdistettu haun mobiilikäyttöä samalla matka miten portaaleihin vastine matkailijoille määrittäminen Hotellit, ravintolat ja Viihde uncharted alueella-yhteyttä. 

Olemme kuvitella portal näyttää poikkeuksellisesti laadukkaita hakuja jälleenmyyjältä tietojen markkinoilla päälle, Etsi sijainti ja muut Suositeltavat kohteet-jälleenmyyjältä perusteella stores shoppers tukea on. Olemme Määritä hakukonetta alkuperäinen tietojoukko kanssa, mutta tarkempaa arvo rakennetaan jaettuina jälleenmyyjältä-tilaajille, jotka käyttävät yritykset tietoja avulla. Kampanjat, uusi kauppa, Suositut logojen ja sisäiset otettava services-– kaikki ovat esimerkkejä tiedot, jotka sivustossamme lisäarvoa. Nämä tiedot itse raportoidaan ja integroitu haku-corpus, kun kuin tilaajan Rekisteröi jälleenmyyjältä. Mainonta sekä tilaus-mallin avulla sekä uuden business tuotto-muodossa.

Haku on täysin cloud-ympäristössä puhuvien ensisijainen käyttäjä-vuorovaikutus-malli. Tarkoitetaan asteikko ja pieni kustannukset miltei kaikki tieto on Tee-Ohjausobjektin lähde portaalin myös ovat online-palvelun kautta. Käyttää hakukone, joka sisältää ominaisuuksia, annettava ulos-ruudussa Hakusovelluksen voit luoda nopeasti, eikä sinun tarvitse luoda ja hallita haun moduuliin molemmista.

## <a name="what-we-built"></a>Olemme hallinta

WhatToPedia on pysäyttämisestä infomedia yritys. Olemme laadittuihin [WhatToPedia.com](http://whattopedia.com/) – - tällä hetkellä testi alan Pohjois-Eurooppa, joiden tarkistusluettelo päivämäärä on 2. helmikuussa 2015. Microsoftin asiakas on ensisijaisesti Tiili ja laastin verkkojen edullinen verkkosivustoa, joka on helppo hallita ja ylläpitää käyttäville.

Tutustu tehtävä on herättää shoppers hyvien online-hakuja kautta, kehittämällä tuloksia kaupunki tai ympäristö, logojen tallentaa nimiä tai tallentaa tyyppejä. Lainanantajia shoppers on heijastusvaikutuksia, voit tilata Microsoftin portaalisivuston motivoimisesta jälleenmyyjät. Tilaukset ovat maksullisten, edullinen määrä.

 ![][7] 

Rekisteröityminen tilauksen jälkeen jälleenmyyjälle ohittaa luotuun profiiliin niiden (luoma alun perin us ostettujen tiedoista), päivittäminen ja muita tietoja Kampanjat esitellyt logojen ja ilmoitukset. Sisäiset ominaisuuksia, kuten puhuttavista, valuuttojen hyväksytty, Verovapaa ostokset, voi olla itse ilmoitetaan herättää paremmin shoppers kuka ne vaikuttavat näyttöä.

## <a name="who-we-are"></a>Olemme käyttävien

Voin käsitellyt Jesper Boelling johtaa Developer osoitteessa WhatToPedia suunnitteluun ratkaisun nimeni on Thomas Segato (Microsoft Consulting). 

WhatToPedia on pysäyttämisestä markkinoinnin sen uuden portaalin business, Ruotsi, johon useimmat 60,000 jälleenmyyjiltä ovat Tiili ja laastin pk (pienten ja keskisuurten yritysten)-testi. Koska Microsoft tietää ostokset Euroopan henkilön puhuu useita kieliä ja suorittaa useita valuuttoja, emme Luo ratkaisuja, jotka mahtuu monikielisten ostosovelluksen. Syy tarvitaan ja löydy, hakukone, joka tukee Microsoftin monikielisten vaatimukset Azure hakutoiminnolla.

Azure Search ei Ottelu-vaihtaja Microsoftin projektille. Ennen Azure haun saatavuuden ilmoitetaan huomattavia energiajärjestelmien toimi muodostamisen oman hakukone kinks kautta. Ottaa Azure Etsi online-palvelun poistettu suurimmista tekninen ja hallinnollinen Hylkäysrajan Microsoftin ratkaisun, jonka tarkoitus käytön markkinoille nopeammin ja tehokkaamman hakuja.  

## <a name="how-we-did-it"></a>Miten on ollut

Tutustu näkö on vain pilvipalveluihin perustuvan valmis infrastruktuurin luonnissa. Microsoft valittiin kuin strategisten ympäristössä, koska se on palvelu, joka tarjoaa tarvittavat palvelut (yhteistyö sekä kehitys), skaalata demand ja edullinen hinnat.
 
### <a name="high-level-components"></a>Ylätason osat

On suunniteltu business, ei pelkästään sivuston. Tukevat koko työmäärään tarvitaan monia työkaluilla ja sovelluksilla. Microsoft Visual Studio ja Visual Studio Team Services kehittämistä, ryhmän Foundation Service (TFS) Online Ohjausobjektin lähde-ja hallinta, viestintä ja yhteiskäyttö Office 365: ssä ja kurssin Microsoft Azure kaikkien sivuston liittyviä toimintoja ja tallennustilaa scrum annettujen. Visual Studio IDE annettu suoraan-valmistelu Azure-integrointi TFS online antamisen muita tuottavuutta tehon lisäys.

Seuraavassa kuvassa on kuvattu korkean tason osia käytetään WhatToPedia infrastruktuuria.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Kuinka Käytämme Microsoft Azure

Katsoo edellisen kaavion vihreät ruudut, näet, että WhatToPedia-ratkaisun muodostanut palveluista:

- [Azure haku](https://azure.microsoft.com/services/search/)
- [Azure sivustoihin MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure WebJobs ajoitettujen tehtävien](../app-service-web/websites-webjobs-resources.md)
- [Azure SQL-tietokanta](https://azure.microsoft.com/services/sql-database/)
- [Azure-BLOB-säiliö](https://azure.microsoft.com/services/storage/)
- [SendGrid sähköpostin lähettämisestä](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Ratkaisu hyvin sydän on tietoja ja Etsi. Alla on esitelty etenemisen asiakkaan Reseller-palvelun tiedot:

  ![][9]

Ensisijainen tietosäilö on jälleenmyyjältä ja Azure SQL-tietokannan tiedot. Tämä sisältää alkuperäisen tietojoukko sekä jälleenmyyjältä kielikohtaiset tiedot lisätään ajan kuluessa. Esimerkissä on käytössä Azure-WebJob päivitykset julkaiseminen haun corpus hakutoiminnolla Azure SQL-tietokantaan.

### <a name="presentation-layer"></a>Esityksen kerros

Portaalissa on Azure-sivusto, toteutettu MVC 4-ja [Twitter-automaattinen](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29). Microsoft valitsi MVC, koska siinä on paljon selkeämpi lähestymistapa HTML-muodossa kuin ASP.NET Lomakepohjainen kehittäminen. Vältä useilla eri laitteilla sovellukset luoda ja ylläpitää useiden mobile ympäristöjen Twitter-automaattinen valittiin tukemaan kaikki laitteet ja ympäristöissä.

### <a name="authentication-permissions-and-sensitive-data"></a>Todennus ja käyttöoikeudet luottamukselliset tiedot

Shoppers selata sivustoa anonyymisti. Sellaisenaan ei ole kirjautuminen vaatimukset shoppers eikä Microsoft tallentaa kuluttaja mitään tietoja. 

Jälleenmyyjät ovat eri Tarinan. Tässä kohdassa tallennetaan julkisen profiilitietoja, laskutustiedot ja mediasisältöä, joita he haluavat näyttää sivustossa. Jokaisen jälleenmyyjältä, joilla sivuston mukaan Hae käyttäjän kirjautuminen, ennen kuin teet päivitykset tilaajan profiilin käyttäjä todennetaan.  Kaikki tilaajan tiedot tallennetaan suojatusti Azure SQL-tietokanta ja Azure-BLOB-OBJEKTIEN tallennustilaan.
Olemme valinnut .NET lomakepohjaisen todennuksen määrittäminen perustuu todennus-mallin. Microsoft valitsi tämän menetelmän sen Yksinkertaisuuden; Tarvitsemme ei roolit-, Käyttöliittymän tuki- ja muita ylimääräiset ominaisuuksia, jotka sisältyvät muista tavoista. 

Voit varmistaa, että jälleenmyyjät näkyviin vain tiedot, joihin kuuluu niihin, luomaasi jälleenmyyjältä kunkin jälleenmyyjältä, jota käytetään kaikissa myöhemmin tunnus lukeminen ja kirjoittaminen toimia jälleenmyyjältä tiedot. Tämän menetelmän kanssa löydetyillä ei annettava tietokannan käyttöoikeuksien myöntäminen yksittäisiä jälleenmyyjät. Kaikki jälleenmyyjät käsitellä järjestelmän yhden tietokannan rooli-tunnuksella jälleenmyyjältä kuin Microsoftin tietojen eristystaso tekniikka.

Koska Microsoftin on kaikki tietoja edeltävät tehosteita (ajo Lisää business jälleenmyyjät, luominen ja tilaa kannustaa) on, voit piirtää viivoja osoitteessa käsittely ostot WWW-sivuston kautta. Näin ollen ei löydä ostoskori sivuston, joka helpottaa Microsoftin suojausvaatimukset. 

Toinen on käytetty yksinkertaistaminen oli ulkoistaa Microsoftin laskutus- ja -tilien koronmaksupäivien toimintoja. Reititys asiakkaan maksutiedot suoraan kolmannen osapuolen mukaan ([SveaWebPay](http://www.sveawebpay.se/)) pienennetään liittäminen tallentamisesta ja suojaa luottamukselliset tiedot Microsoftin tietojen stores riskejä. 

### <a name="search-engine"></a>Hakukone

Microsoftin ratkaisun core on rakennettu Azure-hakupalvelun hakukonetta. Aluksi on luotu mukautettu hakukone, mutta tämän prosessin aikana on toteutunut monimutkaisuus työmäärään on erittäin suuri varmasti ja, joka pyytää us huomioitavia muita vaihtoehtoja. 

Perusominaisuudet, jotka olivat tärkeimmät US mukana:

- Suodattimet
- Kohdistettua siirtymistä varten
- Lisäyksen tulokset
- Sivutuksen AJAX kautta

Internet-haku toi us seuraavassa videossa, jonka tuloksena että Kokeile Azure haku: [Perinpohjaisesti käsittelevään artikkeliin TechEd Europe-palvelussa](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Jälkeen katsominen videon, emme voi luoda prototyypin, perusteella olemme tuli. Olemme oli jo tietomallin MVC, koska prototyypin luominen on helppoa, koska tiedot sisälsivät haettavissa olevia termejä ja on ollut tehnyt varten miten on etsimäsi pinta, lajitella ja suodattaa tietoja koskevat vaatimukset. 

Tämä johtuu siitä, miten on muodostettu prototyypin.

**Azure hakupalvelun asetusten määrittäminen**

1. Kirjautuminen perinteinen Azure-portaaliin ja tutustu tilauksen Search-palvelun. On käytetty (maksuton ja tutustu tilaus) jaettuun versioon.
2. Indeksin luominen Prototyypin, varten on käytetty portaalin Käyttöliittymän haun kentät ja luoda tulosmalli profiileja. Tutustu tulosmalli profiilin perustuu paikkatietojen: maan | kaupungin | osoite (katso: Lisää tulosmalli profiilit).
3. Kopioi URL-Osoitteen ja järjestelmänvalvojan Microsoftin tiedostojen api-näppäintä. Tämä avain on palvelun hakusivun portaalissa ja niitä käytetään tarkistamiseen-palveluun.
    
**Kehittää haun indeksointitoiminto työ – Windows Console**

1. Kaikki jälleenmyyjät lukea tietokantaa.
2. Soita Azure haun Service API lataaminen jälleenmyyjät yksi kerrallaan (katso: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Ominaisuuden määrittäminen tietokannan, reseller indeksoidaan lisäävän indeksoinnin. On ollut lisäämällä 'indeksointitoiminto'-kenttä, joka tallentaa indeksin tila kunkin profiilin (Indeksoitu tai ei). 

Katso, jota käytetään indeksointitoiminto työn koodikatkelman liite.

**Etsi Web-portaalin – kehittää MVC**

1. Soita Azure-hakupalvelun Etsi kaikki asiakirjojen käyttämistä varten (katso: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Pura seuraamisen search-palvelun vastauksen (käyttämällä json.net http://james.newtonking.com/json)
   - Tulokset
   - Rakenteita
   - Tulosmäärät
   - Kehittää käyttöliittymä ja näyttämään hakutuloksia, rakenteita ja laskee (olemme oli jo tämä).

Tämä on koodi on käytetty käyttämistä Azure haun tulokset:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Kehittämällä sijainnin mukaan**

Tärkeimmät vaatimus vahvistamiseksi prototyypin todennäköisesti sisällyttää sijainti avainsana lisääminen kyselyyn. Se on tärkeä Microsoftin-portaaliin, jos käyttäjä lisää kaupungin nimen haku kyselyn, joka jälleenmyyjät annetun kaupungissa Järjestä suurempi kuin jälleenmyyjät ottaa Kaupunki-avainsanan kuvaus. Tämä vaatimus varten on käytetty tulosmalli profiilin järjestetään suurempi kuin muiden kenttien Kaupunki-kenttään.

**Useita kieliä tukevat**

Olemme tarvittavat oikea hakutulokset näkyvät oikeat kielet ja anna muokkausruutua samat tulokset eri kielillä. Tämä ongelma molemmilta puolilta on: 

- Useiden kielten sanojen hakeminen
- Näytä etsinnän tulokset oikea kieli

Esityksen osan selviä lisääminen asiakirjan kullekin kielelle lokalisoitu tekstillä ja ominaisuus-kielellä. Kun käyttäjän kirjoittamat hakusana, että käyttäjän `$filter` lausekkeiden suodatettavan kielen käyttäjä on voinut poistaa.

Jokaiseen tiedostoon on piilotettu-ominaisuus nimeltä "kaupunkien" rakennettu sivustokokoelman tyyppi. Tämä ominaisuus tallentaa kaupunkien nimien kaikilla kielillä, jossa monikielisiä Etsi käyttäjä.

###<a name="data-storage"></a>Tietojen tallentaminen

Kaikki tiedot (profiilin, tilaus ja accounting) tallennetaan SQL-tietokantaan. Kaikki mediatiedostot on tallennettu Azure-BLOB-säiliö, kuten kuvia ja videoita, jotka on annettu jälleenmyyjän. Tiedostojen lähettäminen; tehosteita eristää käyttämällä eri BLOB-OBJEKTIEN tallennustilaan tiedostot ovat koskaan yhtä mingled sivustoa, joten ei tarvitse sivusto uudelleen aina, kun kaavaan lisätään tiedostoja.

Tutustu tallennustilan rakenne tärkeitä etuna on se useita kehittäjät voivat jakaa yhden kehittäminen-tallennustilan. Jokin WhatToPedia projektin edellytyksistä ei voi luoda kehitysympäristö 15 minuuttia, mukaan lukien reseller tietoja, kuvia ja videoita. Käytön uusimmat tiedot TFS Online-sivustosta, suorittamalla SQL-komentosarja ja tuo työn, ympäristössä, jossa on valmis voi olla stood hetkessä ollenkaan. Käytäntö myös parantaa väliaikaisesta.

###<a name="webjobs"></a>WebJobs

Tietojen päivittäminen indeksiin Azure WebJobs avulla. Luomalla haun indeksointitoiminto työn indeksoinnin osa on helppo integroida Microsoftin ratkaisun. Vain on tehty koodin muutos oli sopimaan indeksointitoiminto työn Lisää `Indexed` Microsoftin tietomalliin osoittamaan indeksin tila-kentän. Aina, kun uusi profiili on lisätty tai päivitetty, `Indexed` kentän arvo on EPÄTOSI. Sama koskee jälleenmyyjältä muuttuessa yhteystietoluetteloonsa profiilitiedot portaalin kautta.  

Työn etsii kaikki rivit, joiden `Indexed` arvo on EPÄTOSI. Rivin löytyy, kun asiakirja kirjataan Azure Etsi ja valitse sitten `Indexed` -kentän arvo on TOSI. Microsoft ei ole lisäämiseen ja tietojen päivittämistä, koska Azure-hakupalvelun todella kestää varoen tämän suunnitteleminen. Jos lisäät asiakirjaan, joka on jo olemassa-palvelun tehdä päivityksen automaattisesti.

Web-töistä on kehitetty, voit ladata Azure sivustoihin ZIP-tiedostoina, purettuna ja ajoitetaan console-sovelluksina.

Resurssit on varattu 5 minuuttia ajoitetun web-tehtäväksi. On laskettu, että palvelun kestää noin kolme minuuttia ladata palvelimeen asiakirjoja, 3 000, joka on Microsoftin vaatimukset kuluessa. 

> [AZURE.NOTE] Tällä prototyypin indeksointitoiminto ominaisuus, joka otettiin viimeksi Azure hakutoiminnolla. Tämä ominaisuus oli liian myöhäistä meille käyttämällä Microsoftin first Release-ohjelmassa, mutta se näkyy Ratkaise ongelma on käytetty Microsoftin indeksointitoiminto työ, eli voit automatisoida tietojen lataaminen toiminnot.


###<a name="backup-strategy"></a>Varmuuskopioinnin määrittäminen

On suunniteltu usean tasoisen varmuuskopion strategia kriittinen virhe alaspäin palautus yksittäisen tapahtuman skenaariot solualueen palauttaminen. Voit suojata varat sisältävät kolmenlaisia (sivuston, tilaajan tietojen ja media-tiedostot). 

Ensin säilyttämällä web-sivuston lähdekoodin TFS Online, on tiedettävä, että jos sivuston, emme voi uudelleen sen julkaisemalla TFS. 

Tilaajan tietojen Azure SQL-tietokantaan on eniten luottamuksellisia resurssi. Olemme takaisin tämän avulla sisäisiä ominaisuus (katso [Azure SQL-tietokannan varmuuskopiointi ja palauttaminen](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Varmuuskopioinnin aikataulu on täydellinen tietokannan varmuuskopiointi kerran viikossa, eroavuus tietokannan varmuuskopioita kerran päivässä ja tapahtuman Lokin varmuuskopioiden 5 minuutin välein.  Annetut tiedot koon, tämä ratkaisu on enemmän kuin riittävä Microsoftin välittömästi ja arvioidut tietomääriä.

Kolmas kuva ja videotiedostoja tallennetaan Azure-BLOB-OBJEKTIEN tallennustilaan. Olemme ovat edelleen arvioimisen ultimate varmuuskopion suunnitelman tämä tiedoille ottaen huomioon Cloudberry Explorerin Azure mahdolliset ratkaisuksi. Nyt Käytämme WebJob kuvien ja videoiden kopioiminen toiseen paikkaan.

##<a name="what-we-learned"></a>Olemme asiat

Olemme oli jo tietoja, koska se on helposti muodostaa käsitteiden. Tunnin kuluessa on ollut prototyypin rakenteita sisältävän luokitellut laskureita, sivutus-profiileista ja etsinnän tulokset. Tulos on niin tarkan olemme päättäneet poistaa joitakin esitetään asiakkaan suodattimet. 

Suurimmista muutokset yllätä meille, oli, kuinka nopeasti on lisätietoja Azure haun ja kuinka paljon on käytössä takaisin. Literaaleina on muodostettu, käsitteiden muutaman tunnin (Katso alla oleva huomautus), 500 koodin rivit tilalle edusta-sovellus (plus uusi WebJob)-koodi 3 riviä ja parempia tuloksia. 

Aiemmin koodia toteutettu sivutus, määrät ja muita toimintoja, jotka ovat vakio, jos haluat etsiä. Azure-haun avulla on palata tulokset sisältävät haun osumia-rakenteita, sivutus tietoja, laskee--kaikki tiedostot on tarvittavat ja on ollut toimittamaan molemmista. Käytettävissä on myös lisäyksen ja valmiista kohdistettua siirtymistä, joka ei ole on Microsoftin alkuperäisen ratkaisussa.

Käyttöönoton yhteydessä mahdollisimman haasteellista on, että se on Preview-versiosta ja tiedot ja jaetun kokemukset on vaikeaa. Kun on yhdistetty muutaman pistettä, löydetyillä Azure Search-palvelun avulla on melko yksinkertainen vuoksi sen REST API ja JSON tietojen muoto. Emme voi soittaa puitteissa suoraan eniten Avaa lähde-laajennukset, kuten JQuery JSON.Net ja emme voi käyttää työkaluja, kuten Fiddler nopea vuorovaikutteisuudesta ja virheiden. 

> [AZURE.NOTE] Lisäksi on prepped tiedot, se auttoi niiden us rakentaminen prototyypin jo ymmärtää haun tekniikka toiminta, että us tuottavuutta ja lisää appreciative valmiita ominaisuuksia. Jos haluat päästä alkuun-haun kyselyn rakenteeseen, kohdistettua siirtymistä varten ja suodattimet, olisi odotat prototyyppiä kestää kauemmin. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Esityksen hakusivun rakenteita hallinta

Yksi Microsoftin learnings aikana sekä käsitteiden oli suunnitella huolellisesti toimiston ulkopuolella rakenteita. Kun olet ladannut paljon tietoja ratkaisuun, on tuli rakenteita jo pelkkä viestien määrä on liian suuri esittää käyttäjille. 

Olemme selviä rajoittaminen pinta Laske-parametrin. Laske-parametrin joutuu Kova raja palauttaa käyttäjän rakenteita määrän. Linkki, joka sisältää keskustelun Laske-parametrin löytyy [tähän](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>Tehtävien ajoittamisessa WebJobs

Azure haku ei vain miellyttävämpää muutokset yllätä us varten. Olemme löydetään WebJobs avulla voit automatisoida Microsoftin tietojen lataaminen toimintojen Azure Etsi on suojausta päätason Microsoftin edellisen menetelmän, jossa palauttamiseen käyttämällä erillinen AM, joka on käynnissä Windows ajoitus ajoitettujen tehtävien hakuindeksin päivittämistä varten. WebJobs on helpompaa määrittämiseen ja virheenkorjaus-ja kurssin paljon halvempia tarvitse maksaa erillinen AM.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB Storage Explorer päivityksessä kuvat

Löydetyillä, [Azure BLOB Storage Explorer](https://azurestorageexplorer.codeplex.com/) (käytettävissä codeplexissä) avulla voit olla erittäin hyödyllistä kuva- ja video sivuston päivitysten hallintaan. Käytämme se kehitystyökalun kuin manuaalinen päivittäminen kuvia ja videoita, jotka ovat osa Microsoftin pääsivusto. Olemme todennut, että se on joustavampaa kuin muutokset käyttöönotto-portaaliin ja jättää pois koko testin iteraation aina annettava Päivitä kuva. 

##<a name="a-few-final-words"></a>Lopullinen pari sanaa

Kiitos, jos haluat hyvien henkilöille, joilla on WhatToPedia, jolla sallitaan us jakaminen niiden Tarinan!  

Toivottavasti löytämäsi Tämä tapaustutkimus hyödyllisiä. Jos siirryt ilmeisesti Azure, voin suosittelee muutaman resurssien nopeuden voit pitkin:

- [MSDN-keskustelupalsta Azure-haun avulla](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow on myös tunnisteen](http://stackoverflow.com/questions/tagged/azure-search)
- [Ohjeet sivun Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure haun MSDN: N ohjeet](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Lisäys: Haun indeksointitoiminto WebJob

Seuraava koodi muodostaa rakentaminen prototyypin-osassa mainittu indeksointitoiminto.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
