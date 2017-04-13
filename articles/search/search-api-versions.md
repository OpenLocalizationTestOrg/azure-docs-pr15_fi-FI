<properties
   pageTitle="Azure haun API-versioiden | Microsoft Azure | Haun Ohjelmointirajapinta"
   description="Version käytäntö Azure haun REST API ja .NET SDK asiakas-kirjastossa."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Azure hakutoiminnossa API-versiot

Azure haku palauttaa ulos toimintojen päivitykset säännöllisesti. Joskus, mutta eivät aina päivitykset edellyttävät että Microsoftin API uuden version julkaiseminen säilyttämiseksi yhteensopivuus aiempien versioiden kanssa. Uuden version julkaiseminen avulla voit hallita, milloin ja miten integroit haun palvelupäivityksistä koodisi.

Yleensä Yritämme julkaista uusia versioita vain silloin, kun se on tarpeen, koska se voi liittyä joitakin työmäärään päivittäminen koodin käyttämään uutta API-versiota. Olemme julkaisee uuden version vain, jos annettava muuttaa tiettyjä Ohjelmointirajapinnan niin, että vaihdot yhteensopivuus aiempien versioiden kanssa. Tämä voi johtua vuoksi ratkaisut aiemmin ominaisuuksia tai uusia ominaisuuksia, muuttaa olemassa olevan API pinta vuoksi.

Olemme noudattamalla saman säännön SDK-päivitykset. Azure-haun SDK seuraa [semanttinen versiointi](http://semver.org/) sääntöjä, mikä tarkoittaa, että sen versio on kolme osaa: pää ja aliversio koontiversion numero (esimerkiksi 1.1.0). Olemme julkaistaan uuden pääversion SDK vain, jos kyseessä muutokset, jotka sivunvaihtojen yhteensopivuus aiempien versioiden kanssa. Olemme kasvattaa aliversion sitovat toimintojen päivitykset ja virheenkorjauksia varten on vain kasvattaa muodosta-versio.

##<a name="snapshot-of-current-versions"></a>Nykyinen versio tilannevedoksen 

Alla on kaikki nykyiset versiot tilannevedoksen ohjelmointi liittymät Azure Etsi. Vaiheittaisiin ja muut tiedot löytyvät myöhemmin tämän asiakirjan osiin.

Liityntäkohdat|Uusin pääversio|Tila
----------|-------------------------|------
[.NET SDK-PAKETISSA](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Yleisesti saatavilla, julkaissut helmikuussa 2016
[.NET SDK esikatselu](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0-esikatselu|Julkaistu elokuussa 2016 esikatselu
[Palvelun REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Yleisesti saatavilla
[Palvelun REST API esikatselu](search-api-2015-02-28-preview.md)|2015-02-28-esikatselu|Esikatselu
[REST API hallinta](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Yleisesti saatavilla

REST API, mukaan lukien varten `api-version` jokaisesta tarvitaan. Tämä on helppo kohdentamisen tietyn version tiedostomuodossa, kuten esikatselu API. Seuraavassa esimerkissä kuvataan, miten `api-version` -parametri on määritetty:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Vaikka sivupyynnön on `api-version`, on suositeltavaa, että käytät saman version API pyynnöt. Tämä on erityisen TOSI, kun uusia API versioita esitellä määritteet tai toiminnoista, joita ei tunnista aiempiin versioihin verrattuna. Sekoittamalla API versioita voi olla tahattomia vaikutukset ja voidaan välttää.
> 
Palvelun REST API ja hallinnan REST API ovat versiotietoja toisistaan riippumatta. Versioita minkä tahansa samanlaiset on muiden satunnaisten.

Yleisesti saatavilla (tai GA) API voi käyttää tuotannon ja Azure service level Agreement käsitellään. Preview-versio on koe ominaisuudet, joita ei aina siirretä GA-versioon. **On erittäin suositeltavaa, että vastaan esikatselun ohjelmointirajapinnan tuotannon sovellusten käyttäminen.**

##<a name="sdk-version-roadmap"></a>SDK versio ohjeet

Jokaisessa versiossa .NET SDK jotakin tietyn palvelun REST API-versio. Toiminnot on esiin REST API-ensin ja sitten toteutettu SDK: ssa.

.NET SDK on nyt yleisesti saatavilla ja työ on jo käynnissä seuraava versio. Seuraavassa taulukossa etsii eteenpäin SDK tulevissa versioissa niin, että sinulla on kuitenkin verrata mitä on tulossa Seuraava.

.NET SDK-versio|REST API-versio|Ominaisuudet|EETA
----------------|----------------|--------|---
1.1|2015-02-28|Lucene kyselyn syntaksi|Helmikuussa 2016
2.0-esikatselu|2015-02-28-esikatselu|Mukautetun analyzers, Azure-Blob-objektien ja Indeksoijilla taulukon kenttien yhdistämismäärityksiä ETags|Elokuussa 2016
2.x|Uusi GA API-versio|Sama kuin 2.0-esikatselu|Aikainen Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Esikatselu ja yleensä käytettävissä versiotiedot

Azure haun aina valmiiksi versiot koe ominaisuudet – REST-Ohjelmointirajapinnalla ensin, sitten esijulkaisuversiot .NET SDK kautta.

Esikatselutoiminnot ei taata uuteen GA-versioon. Olisi GA-version toimintoja pidetään vakaata ja todennäköisesti muuttaa lukuun ottamatta pieni yhteensopivan korjaukset ja parannukset, esikatselu-ominaisuudet ovat käytettävissä testaus ja vuorovaikutteisuudesta koskevan palautteen kerääminen ominaisuuksien suunnittelu ja käyttöönotto. 

Koska esikatselutoiminnot voivat muuttua, Suosittelemme kuitenkin vastaan kirjoittaminen tuotannon tunnus, jolla on riippuvuus esikatselu-versioissa. Jos käytössäsi on vanhempi preview-versio, on suositeltavaa yleisesti saatavilla (GA)-versioon siirtyminen. 

Saat .NET SDK: koodi siirron ohjeet tukikäytännöistä [päivittäminen .NET SDK](search-dotnet-sdk-migration.md).

Yleiseen käyttöön tarkoittaa, että Azure haku on nyt palvelun tason sopimuksessa (SLA). SLA tukikäytännöistä [Azure haun palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

