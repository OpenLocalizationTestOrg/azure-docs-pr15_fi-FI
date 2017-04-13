<properties
    pageTitle="Mikä on Azure haun | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Azure haku on täysin hallitun isännöityä cloud search-palvelun. Lisätietoja tämän ominaisuuden yhteenveto."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Mikä on Azure haun?

Azure haku on cloud-muodossa--hakupalvelun ratkaisu, joka Delegoi palvelimen nimen ja infrastruktuurin hallinnan Microsoftille, jolloin valmis avulla-palvelussa, voit lisätä tiedot ja lisää web tai mobiilisovelluksen haun avulla. Azure haun avulla voit helposti lisätä hakukokemuksen Yksinkertainen [REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) tai [.NET SDK](search-howto-dotnet-sdk.md) käyttäminen ilman hallinta Etsi infrastruktuuri tai tulossa haun asiantuntija-sovelluksiin.

## <a name="give-your-users-a-powerful-search-experience"></a>Antaa käyttäjille tehokas haku-toiminto

**Tehokas kyselyt** voi muotoilla [yksinkertaisen kyselyn syntaksi](https://msdn.microsoft.com/library/azure/dn798920.aspx), joka tarjoaa loogisilla operaattoreilla, lause haku-operaattoreita, jälkiliite operaattoreita, operaattorien järjestys. Lisäksi [Lucene kyselysyntaksia](https://msdn.microsoft.com/library/azure/mt589323.aspx) ottaa epäselvältä haun, lähialue haun, termin kehittämällä ja säännöllisiä lausekkeita. Azure haun tukee myös mukautettuja Sanastollinen analyzers, jotta sovelluksesi käsittelemään monimutkaisia hakukyselyjen foneettinen vastaavat ja säännöllisiä lausekkeita.

**Kielen tuki** on [hankittavissa 56 eri kielillä](https://msdn.microsoft.com/library/azure/dn879793.aspx). Käytä Lucene analyzers ja Microsoft analyzers (Tarkennusperuste luonnollisen kielen käsittelyn Officen ja Bing vuosina), Azure haun voit analysoida sovelluksen hakuruutuun käsittelemään älykkäästi kielikohtaiset lingvistiset tiedot, mukaan lukien verbien aikamuodot, sukupuoli, epäsäännöllinen monikollista substantiivit (esimerkiksi "hiirtä' ja"hiirtä"), word poistaa seostuksestaan, word uusinta (kielten ilman välilyöntejä) ja lisää teksti.

**Hakuehdotukset** voi ottaa käyttöön autocompleted haku-palkit ja täydentävä kyselyt. Osittaisen haun syöte käyttäjinä [Todellinen tiedostojen indeksi ohjelma ehdottaa](https://msdn.microsoft.com/library/azure/dn798936.aspx) Kirjoita.

**Osumien korostaminen** [avulla](https://msdn.microsoft.com/library/azure/dn798927.aspx) käyttäjät voivat tarkastella koodikatkelman tekstin kunkin tulos, joka sisältää niiden kyselyn vastineita. Voit valita kentät, jotka palauttavat korostetun katkelmat.

**Kohdistettua siirtymistä** lisätään helposti hakutulossivulla Azure haulla. Käytä [vain yhden kyselyparametri](https://msdn.microsoft.com/library/azure/dn798927.aspx), Azure haku palauttaa kaikki tarvittavat tiedot muodostaa kohdistettua hakuja, jotta käyttäjät voivat siirtyä alaspäin ja suodattaa hakutulokset (kuten suodatin luettelokohteet hinta-alueen tai brändiä) sinua sovelluksen käyttöliittymässä.

Voit käsitellä älykkäästi, suodattaa ja näyttää Maantieteellisten sijaintien **GEO paikkatietojen** tuki. Azure haun avulla käyttäjät voivat tietojen perusteella, hakutulos lähialue määritettyyn sijaintiin tai tietyn maantieteellisen alueen perusteella. Tässä videossa kerrotaan, miten se toimii: [kanavan 9: Azure haku- ja Geospatiaalisia tietoja](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Suodattimien** avulla voidaan yhdistää kohdistettua siirtymistä sovelluksen Käyttöliittymän, parantaa kyselyn muodostamisessa helposti ja suodatettava-käyttäjän tai developer-määrittämät ehdot. Luoda tehokkaita suodattimia käyttämällä [OData-syntaksia](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Antaa kehittäjille helposti käytettävällä-palvelun kanssa

**Suuren käytettävyyden** varmistaa erittäin luotettavaa haku-käyttökokemus. Kun skaalata oikein, [Azure haku on 99,9 % SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Lopusta loppuun-ratkaisun Azure Etsi **hallittu täysin** edellyttää täysin ei infrastruktuurin hallinta. Palvelun voit helposti mukautettava tarpeidesi mukaan skaalauksen kaksi mitat käsittelemään asiakirjan lisätallennustilan, kysely Lataa tai molemmat.

**Tietojen integroinnin** avulla [Indeksoijilla](https://msdn.microsoft.com/library/azure/dn946891.aspx) avulla Azure haun indeksoimaan automaattisesti Azure SQL-tietokanta, Azure DocumentDB tai [Azure-Blob-säiliö](search-howto-indexing-azure-blob-storage.md) synkronoimaan search-indeksin ensisijainen tietovaraston sisällön.

**Asiakirjan selvittäminen** on käytettävissä (olevan esikatselu) [ja indeksoi pää tiedostomuodot](search-howto-indexing-azure-blob-storage.md) mukaan lukien Microsoft Officen sekä PDF- ja HTML-tiedostoja.

**Etsi liikenne analytics** ovat [helposti kerätä ja analysoida](search-traffic-analytics.md) lukituksen havainnollistamisen mitä käyttäjät ovat kirjoittaa hakuruutuun.

**Yksinkertainen näkyvissä pistemäärä** on tärkein etu Azure haku. [Näkyvissä pistemäärä profiilit](https://msdn.microsoft.com/library/azure/dn798928.aspx) käytetään sallimaan organisaatioille mallin asiayhteyteen funktiona arvot itse asiakirjoja. Esimerkiksi haluta uudempaan tuotteiden tai diskontataan tuotteiden näkyvän suurempi hakutuloksista. Voit myös luoda tulosmalli profiilit tunnisteita käyttämällä mukautettuja näkyvissä pistemäärä perustuu asiakkaan haun asetukset olet seurata ja tallentaa erikseen.

**Lajittelu** monikenttäisen indeksin rakenteen kautta tarjoaa ja vaihtaa sitten kysely-aikaa yhteen haku-parametrin.

**Sivutus** ja rajoitusten hakutuloksia on [helppoa, kun hienoksi syksyn ohjausobjektin](search-pagination-page-layout.md) , joka Azure haun tarjoaa hakutuloksia.  

**Erikoishaku** mahdollistavat ongelman kyselyitä, jotka perustuvat kaikki indeksejä oikean tilin Azure-portaalista, jotta voit helposti testata kyselyt ja tarkentaa tulosmalli profiilit.

## <a name="how-it-works"></a>Toiminta

### <a name="1-provision-service"></a>1. palvelun valmisteleminen
Voit asettamasi [Azure-portaalista](https://portal.azure.com/) tai [Azure resurssien hallinnan API](https://msdn.microsoft.com/library/azure/dn832684.aspx)Azure Search-palvelun.

Sen mukaan, miten search-palvelun määrittäminen käytät joko palvelu, joka jaetaan muiden Azure haku-tilaajille vapaa taso, tai [maksettu taso](https://azure.microsoft.com/pricing/details/search/) , dedicates resursseja voi käyttää vain palvelussa. Palvelun valmisteltaessa valitset myös tietokeskuksen, joka isännöi palvelun alue.

Palvelun, voit valita mitkä tason mukaan voi skaalata palvelusi kahden: 1) Lisää replikoiden oman valmiuksia käsitellä isoja kysely Lataa ja 2) lisää osioita, jos haluat lisätä useampia tiedostoja tallennustila kasvaa. Käsittelyn asiakirjan tallennustilan ja kyselyjen siirtonopeuden erikseen, voit mukauttaa search-palvelun tietyn tarpeisiin.

### <a name="2-create-index"></a>2. indeksin luominen
Ennen kuin voit ladata Azure-hakupalvelun sisältöä, määritä ensin Azure-hakuindeksin. Indeksi on kuin tietokantataulukon, joka sisältää tietoja ja hyväksyä hakukyselyt. Voit määrittää hakemiston mallin haluat etsiä samalla tietokannan kenttiin asiakirjojen rakenteen yhdistäminen.

Näiden indeksien rakenteen joko voi luoda Azure-portaalissa tai ohjelmallisesti [käyttämällä .NET SDK](search-howto-dotnet-sdk.md) tai [REST API](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kun indeksi on määritetty, voit ladata tietojen Azure Search-palvelu, johon se on myöhemmin indeksoitu.

### <a name="3-index-data"></a>3. tietojen indeksointiin
Kun olet määrittänyt kentät ja indeksi määritteitä, voit ryhtyä ladata sisältöä hakemiston esittely. Voit käyttää joko push tai salaus puretaan mallin tietojen lataaminen indeksi.

Salaus puretaan mallin annetaan Indeksoijilla, joka voidaan määrittää tarvittaessa tai ajoitetut päivitykset (katso [indeksointitoiminto toiminnot (Azure haun palvelun REST API)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), joten voit helposti ingest tiedot ja tiedot muuttuvat Azure DocumentDB, Azure SQL-tietokanta, Azure-Blob-säiliö tai SQL Server Azure-AM ylläpidettävä kautta.

Push-malli on tarkoitettu SDK tai käyttää päivitettyä tiedostojen lähettäminen indeksin REST API kautta. Tietoja voi push-miltei mistä tahansa tietojoukko JSON-muodossa. Katso [lisääminen, päivittäminen, tai tiedostojen poistaminen](https://msdn.microsoft.com/library/azure/dn798930.aspx) tai [käyttämisestä .NET SDK)](search-howto-dotnet-sdk.md) tietojen lataaminen on kuvattu.

### <a name="4-search"></a>4. search
Kun olet täyttänyt Azure-hakuindeksin, voit nyt [ongelman hakukyselyt](https://msdn.microsoft.com/library/azure/dn798927.aspx) palvelupäätepiste REST API- tai .NET SDK yksinkertainen pyyntöjen käyttöä.

## <a name="try-it-now-for-free"></a>Kokeile sitä nyt (maksuton!)
Voit kokeilla Azure haun tänään! Jos sinulla on jo Azure-tili, voit [säätää vapaa taso-palvelu](search-create-service-portal.md).

Jos sinulla ei ole Azure-tili, voit kokeilla ei maksuton, 60 minuutin istunnon tilaa pakollinen. Siirry [Yritä Azure App palvelu](http://go.microsoft.com/fwlink/p/?LinkId=618214) ja valitse "Web App-sovelluksen." Valitse sitten Aloita "ASP.NET + Azure Etsi"-malli.
