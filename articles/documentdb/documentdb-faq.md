<properties 
    pageTitle="DocumentDB tietokantaan kysymykset – usein kysytyt kysymykset | Microsoft Azure" 
    description="Etsi vastauksia usein kysyttyihin kysymyksiin Azure DocumentDB NoSQL asiakirjan tietokannan palvelun JSON. Tietokannan kysymyksiin kapasiteettia, suorituskykyä ja skaalausta." 
    keywords="Tietokannan kysymyksiä, usein kysyttyjä kysymyksiä, documentdb, azure Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Usein kysyttyjä kysymyksiä DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Tietokannan kysymyksiä Microsoft Azure DocumentDB perusteet

### <a name="what-is-microsoft-azure-documentdb"></a>Mikä on Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB on blazing nopeasti, maailman asteikko NoSQL asiakirjan tietokanta nimellä--palvelun, joka tarjoaa monipuolisia kyselyt rakenteen vapauttaa tietojen päälle, auttaa määritettäviä ja luotettava suorituskyky ja mahdollistaa nopean kehittäminen potti palautettua mukaan potenssiin hallitun ympäristö ja yhteys Microsoft Azure. DocumentDB on web-mobiili-gaming oikean ratkaisu ja IoT sovellusten ennakoitavissa siirtonopeuden, suuri käytettävyys, pieni viive ja rakenteen vapaa tietomallin ollessa avaimen vaatimuksia. DocumentDB tarjoaa joustavuutta rakenteen ja indeksoinnin kautta alkuperäisen JSON tietomallin RTF ja usean tiedoston tapahtumien tukee integroitu JavaScript kanssa.  
  
Lisää tietokannan kysymyksiä, vastauksia ja käyttöönotto ja tämän palvelun avulla on artikkelissa [DocumentDB asiakirjat-sivu](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Millaisia tietokanta on DocumentDB?
DocumentDB on NoSQL asiakirjan pystysuuntaisen tietokantaan, joka tallentaa tiedot JSON-muodossa.  DocumentDB tukee sisäkkäisiä, Mykistä-contained tietojen rakenteet, jotka voidaan suorittaa kysely monipuolisia DocumentDB [SQL-kyselyn kieliopin](documentdb-sql-query.md)kautta. DocumentDB on erinomainen suorituskyky tapahtumien käsittely palvelinpuolen JavaScript kautta [tallennettujen toimintosarjojen, käynnistimien, ja käyttäjän määrittämät funktiot](documentdb-programming.md). Tietokannan tukee myös developer tunable yhdenmukaisuuden tasot liittyvät [suorituskyvyn tasot](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Täytyykö DocumentDB tietokantojen taulukot, kuten relaatiotietokannasta (RDBMS)?
Ei, DocumentDB tallentaa tiedot JSON asiakirjojen sivustokokoelmat.  Lisätietoja DocumentDB resursseja on artikkelissa [DocumentDB resurssin mallin ja -käsitteistä](documentdb-resources.md). Saat lisätietoja siitä, miten NoSQL ratkaisuja, kuten DocumentDB eroaa relaatio ratkaisuja [NoSQL ja SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Tukee DocumentDB tietokantojen rakenteen tiedot?
Kyllä, DocumentDB avulla voit tallentaa haluamaansa JSON tiedostoja ilman rakenteen määritys tai vihjeitä sovellukset. Tiedot ovat heti käytettävissä kyselyn DocumentDB SQL-kysely-liittymän kautta.   

### <a name="does-documentdb-support-acid-transactions"></a>Tukeeko DocumentDB HAPPAMAN tapahtumat?
Kyllä, DocumentDB tukee asiakirjojen välisten tapahtumia JavaScript tallennettujen toimintosarjojen ja käynnistimien prosentteina. Tapahtumat suodatetut kunkin sivustokokoelman sisällä osio ja HAPPAMAT tavalla kuin kaikki suoritetaan tai mitään eristetty muut samanaikaisesti suoritetaan koodi ja käyttäjän pyynnöt.  Jos poikkeukset ovat ilmenee – palvelin puoli suorittamisen sovelluksen JavaScript-koodia, koko tapahtuma on peruutettu. Lisätietoja tapahtumista on artikkelissa [tietokannan ohjelma tapahtumia](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Mitkä ovat DocumentDB tavallisesti laatikkomäärät?  
Missä Automaattinen skaalaus-ennakoitavissa suorituskyvyn nopea millisekunnin vastauksen kertaa järjestystä ja mahdollisuuden pitää kyselyn rakenne vapaa tietojen päälle IoT sovellukset on tärkeää DocumentDB on hyvä uusi Internetiä, mobiili-gaming. DocumentDB kohdealueella nopean kehitystä ja sitä tukevat sovelluksen tietomallien Jatkuva toisto. Sovellukset, jotka on luotu käyttäjän sisällön ja tietojen hallinta ovat [yleisiä DocumentDB Käytä palvelupyynnöt](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Miten DocumentDB tarjoavat ennakoitavissa suorituskyvyn?
[Pyyntö yksikkö](documentdb-request-units.md) on DocumentDB siirtonopeuden mitta. 1 RU vastaa 1 kt asiakirjan GET siirtonopeuden. DocumentDB jokaisen toiminto, kuten lukuja, kirjoituksia, SQL-kyselyjä ja tallennettu toimintosarja suorituskerran on deterministic RU arvo toiminnon suorittaminen edellyttää siirtonopeuden perusteella. Sen sijaan, että Pohdi suorittimen, IO ja muistin ja miten ne kaikki vaikuttaa sovelluksen siirtonopeuden mieleesi kannalta yksittäisen RU mitta.

DocumentDB valikoimien voidaan varata valmistellun siirtonopeuden kannalta siirtonopeuden sekunnissa RUs kanssa. Minkä tahansa asteikko-sovelluksia voit ensisijainen yksittäisten pyyntöjen mitata niiden RU arvot ja säännöstä sivustokokoelmat käsittelemään pyynnön yksiköiden kokonaismäärä yli kaikki palvelupyynnöt. Voit laajentaa tai skaalata alaspäin oman sivustokokoelman siirtonopeuden kuin sovelluksen evolve tarpeisiin. Lisätietoja pyynnön yksiköt ja ohjeita määritettäessä kokoelmaa on, lue [hallinta suorituskykyyn ja kapasiteettiin](documentdb-manage.md) ja yritä [siirtonopeuden Laskimen](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Onko DocumentDB HIPAA yhteensopiva?
Kyllä, DocumentDB on yhteensopiva HIPAA. HIPAA muodostaa käyttöä paljastaminen, ja erikseen tunnistaa kuntoa, tutustu. Lisätietoja on artikkelissa [Microsoft Valvontakeskus](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Mitkä ovat tallennustilarajojen, DocumentDB? 
Ei ole teoreettinen rajoitettu tietoja apuohjelmista voi tallentaa DocumentDB kokonaismäärä. Jos haluat tallentaa yli 250 Gigatavua yksittäisen sivustokokoelman tietojen, ota [yhteyttä tukeen](documentdb-increase-limits.md) on lisätty tilin kiintiön. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Mitkä ovat DocumentDB siirtonopeuden rajat? 
Ei ole teoreettinen rajoitettu yhteensä määrää nopeus, jotka tukevat kokoelma-DocumentDB, jos havainnollistamiseen voi jakaa karkeasti tasaisesti osion näppäimet on riittävän suuri määrä. Halutessasi ylittää 250,000 pyynnön yksiköt ja toisen kohti sivustokokoelman tai tilin Ota [yhteyttä tukeen](documentdb-increase-limits.md) on lisätty tilin kiintiön. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Kuinka paljon Microsoft Azure DocumentDB maksaa?
Lue lisätietoja [DocumentDB hinnoittelutiedot](https://azure.microsoft.com/pricing/details/documentdb/) -sivulle. DocumentDB käyttökustannukset määräytyvät käytössä tuntimäärä kokoelmien on online-sivustokokoelmien määrä ja kulutettu tallentamista ja valmistellun siirtonopeuden valikoimien varten. 

### <a name="is-there-a-free-account-available"></a>Onko käytettävissä maksuton-tili?
Jos ole ennen käyttänyt Azure, voit rekisteröityä [Azure vapaa-tili](https://azure.microsoft.com/free/), jonka avulla voit 30 päivää ja 200 Kokeile Azure palvelut. Tai jos sinulla on Visual Studio-tilaus, olet [150-vapaa Azure hyvitykset kuukaudessa](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oikeutettu käyttämään Azure-palvelusta.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Miten voin hankkia DocumentDB Lisää apua?
Jos tarvitset ohjeita, valitse [Pinon ylivuoto](http://stackoverflow.com/questions/tagged/azure-documentdb)- [Azure DocumentDB MSDN Developer keskustelupalstoilla](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB)US saavuttaminen tai järjestäminen [DocumentDB suunnitteluryhmät ryhmän kanssa keskustelu 1:1](http://www.askdocdb.com/). Voit pysyä ajan tasalla uusimmista DocumentDB ja sen toimintojen, noudata us [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB määrittäminen

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Miten voin kirjautuminen Microsoft Azure DocumentDB varten?
Microsoft Azure DocumentDB on käytettävissä [Azure Portal][azure-portal].  Ensin sinun on kirjauduttava Microsoft Azure-tilaukseen.  Kun kirjaudut Microsoft Azure-tilaus, voit lisätä DocumentDB tilin Azure-tilaukseen. Saat ohjeita DocumentDB-tilin lisääminen [Luo DocumentDB tietokannan tili](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Mikä on perustyyli avaimen?
Perustyyli avain on suojaustunnus, voit käyttää kaikkien resurssien tiliä varten. Henkilöt, joilla avain on luku- ja kirjoitusoikeudet kaikille resursseille tietokannan tilin. Ole varovainen, kun jakaminen perustyyli avaimet. Perusavaimen perustyyli ja toissijaisen avaimen ovat saatavilla [Azure Portal] **näppäimet **-sivu[azure-portal]. Saat lisätietoja näppäimet [näkymän, kopioi ja muodosta uudelleen sarjanumerot pikanäppäimet](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Miten voin luoda tietokannan?
Voit luoda tietokantoja, joissa [Azure-portaalin]() ohjeiden mukaisesti [Luo DocumentDB tietokanta](documentdb-create-database.md), yksi [DocumentDB SDK: T](documentdb-sdk-dotnet.md)tai [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)läpi.  

### <a name="what-is-a-collection"></a>Mikä on kokoelma?
Kokoelma on JSON asiakirjojen ja liittyvän JavaScript-sovelluksen logiikkaa säilö. Kokoelma on laskutettavan kokonaisuus, jossa [kustannukset](documentdb-performance-levels.md) määräytyy siirtonopeuden ja storaged käytetään. Kokoelmien voi olla yksi tai useampi osioiden/palvelin ja käsitellään käytännössä rajoittamaton tallennus- tai siirtonopeuden tietomääristä skaalata.

Kokoelmien ovat myös DocumentDB laskutuksen kohteet. Valikoimien laskutetaan valmistellun siirtonopeuden ja tallennustilaa, jota käytetään kerran tunnissa perusteella. Lisätietoja on artikkelissa [DocumentDB hinnat](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Miten käyttäjät ja käyttöoikeudet määritetään?
Voit luoda käyttäjät ja käyttöoikeudet, käytä jotakin [DocumentDB SDK: T](documentdb-sdk-dotnet.md) tai [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)läpi.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Tietokannan kysymyksiä Microsoft Azure DocumentDB vastaan kehittäminen

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Kuinka voit tehdä käynnistän kehittäminen DocumentDB vastaan?
[SDK: T](documentdb-sdk-dotnet.md) ovat käytettävissä .NET, Python, Node.js, JavaScript ja Java.  Kehittäjät voidaan hyödyntää myös [RESTful HTTP-ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn781481.aspx) kanssa eri alustojen ja eri kielten DocumentDB resursseja. 

Esimerkkejä, joiden DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)ja [Python](documentdb-python-samples.md) SDK: T ovat käytettävissä GitHub.

### <a name="does-documentdb-support-sql"></a>Tukeeko DocumentDB SQL?
DocumentDB SQL-kyselykieli on parannettu alijoukko SQL tukemat kyselytoimintoja. DocumentDB SQL-kyselykieli on RTF hierarkkisia ja Relaatio-operaattorit ja laajennettavuus kautta JavaScript-pohjainen käyttäjän määrittämät funktiot (UDF). JSON kieliopin mallinnus JSON asiakirjojen puut tarroja nimellä solmut-muodossa, jota DocumentDB automaattisen indeksoinnin avulla sekä SQL-kyselyn kielioppia DocumentDB, käyttää avulla.  Lisätietoja siitä, miten voit käyttää SQL-kieliopin on Ota [Kyselyn DocumentDB] [ query] artikkelissa.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Mitä DocumentDB tukemat tietotyypit ovat?
Tuetut DocumentDB alkeistyyppi tietotyypit ovat samat kuin JSON. JSON on yksinkertainen tyyppi-järjestelmään, joka koostuu merkkijonoja, lukuja (IEEE754 kaksinkertainen tarkkuus) ja totuusarvoja - arvo on TOSI, EPÄTOSI ja Null-arvoja.  Monimutkaisempia tietotyyppejä, kuten päivämäärä ja aika, GUID-tunnus tai Int64 geometria voidaan esittää sekä JSON ja DocumentDB Sisäkkäiset objektit {} operaattori ja matriisien []-operaattorin käyttäminen luomisen avulla. 

### <a name="how-does-documentdb-provide-concurrency"></a>Miten DocumentDB antaa samanaikainen?
DocumentDB tukee HTTP kohteen tunnisteita tai etags optimistisen hallinnan (OCC). Jokaisen DocumentDB resurssilla etag ja etag on määritetty palvelimeen aina, kun asiakirja päivitetään. Etag ylä- ja nykyisen arvon sisältyvät vastauksen viestit. Etags voidaan päättää, jos resurssin päivitetäänkö palvelin sallimaan Jos vastine-otsikkoon. Jos vastine-arvo on tarkistettava vastaan etag-arvo. Jos etag-arvo vastaa palvelimen etag-arvo, resurssi päivitetään. Etag ei ole enää voimassa, jos palvelin hylkää toimintoa, jossa "HTTP 412 ennakkoehto virhe" vastauksen koodi. Asiakas on sitten refetch hankkia etag nykyarvo resurssin resurssi. Lisäksi etags voidaan ja jos-ei mitään-vastine ylätunniste määrittääksesi, jos uudelleen noutaa resurssin tarvita. 

Jos haluat käyttää optimistisen .NET, käyttää [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) luokkaa. Katso .NET otoksen [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) github DocumentManagement-malli.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Kuinka DocumentDB tapahtumat suorittaa?
DocumentDB tukee kielen integroitu tapahtumia JavaScript tallennettujen toimintosarjojen ja käynnistimien kautta. Kaikki tietokannan toiminnot sisällä komentosarjoja koskevat suoritetaan tilannevedoksen eristystaso suodatetut kokoelman jos se on kokoelma single-osion tai tiedostoja yhdessä saman osion avaimen arvon kokoelmassa, jos kokoelman osioitu. Tiedostoversioiden (ETags) tilannevedoksen otetaan tapahtuman alkuun ja vahvistetun vain silloin, kun komentosarja on luotu. JavaScript-koodia ilmoittaa virheestä, jos tapahtuma on peruutettu. Saat lisätietoja [DocumentDB: palvelinpuolen ohjelmointi](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Miten Lisää tiedostot voivat lisätä yhdeksi DocumentDB? 
Lisää tiedostojen suorittaa kyselyjä DocumentDB kolmella eri tavalla:

- Tietojen siirtotyökalun, kuvatulla tavalla [DocumentDB tiedot tuodaan](documentdb-import-data.md).
- Asiakirjan Azure-portaalissa kuvatulla tavalla [joukkolisäys asiakirjan Resurssienhallinnassa tiedostot](documentdb-view-json-document-explorer.md#BulkAdd)Resurssienhallinnassa.
- Tallennettujen toimintosarjojen, kuvatulla tavalla [DocumentDB: palvelinpuolen ohjelmointi](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>Tukeeko DocumentDB resurssin linkin välimuistiin?
Kyllä, koska DocumentDB on RESTful palvelu, resurssi linkkejä ei voi muuttaa ja voi olla välimuistiin. DocumentDB asiakkaiden voit määrittää lukee kaikki resurssi, kuten asiakirjan tai sivustokokoelman ja niiden paikallinen kopioi vain, kun palvelinversion on muuttuvat päivitys vastaan "If-ei mitään-vastine"-otsikko. 

### <a name="is-a-local-instance-of-documentdb-available"></a>On paikallinen esiintymä DocumentDB?
Paikallinen esiintymä DocumentDB ei ole käytettävissä tällä hetkellä. Voit seurata paikallinen emulaattori ja äänestä se [palautetta keskustelupalsta](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)-tila.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
