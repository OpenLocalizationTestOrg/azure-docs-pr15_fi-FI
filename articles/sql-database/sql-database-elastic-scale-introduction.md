<properties
    pageTitle="Skaalauksen ulos Azure SQL-tietokannan | Microsoft Azure"
    description="Service (SaaS) kehittäjät ohjelmiston voit helposti luoda joustavasti, skaalattava tietokantojen näillä työkaluilla pilveen"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="scaling-out-with-azure-sql-database"></a>Azure SQL-tietokanta ja laajentaminen

Voit helposti skaalata Azure SQL-tietokantoja, joissa **Joustavasti** Tietokantatyökalut. Nämä työkaluja ja ominaisuuksia mahdollistavat lähes rajoittamattoman tietokannan resurssit **Azure SQL** -tietokannan avulla voit luoda ratkaisuja tapahtumien toiminnoista ja erityisesti ohjelmiston kuin Palvelusovellusten (SaaS). Koostuvat joustavasti tietokannan ominaisuudet seuraavasti:

* [Joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md): asiakkaan kirjasto on toiminto, jonka avulla voit luoda ja ylläpitää sharded tietokannat.  Katso [joustavasti Tietokantatyökalut käytön aloittaminen](sql-database-elastic-scale-get-started.md).
* [Joustavasti tietokantatyökalu Jaa ja yhdistäminen](sql-database-elastic-scale-overview-split-and-merge.md): siirtää tietoja sharded tietokantojen välillä. Tästä on hyötyä tietojen siirtämistä usean vuokraajan tietokannasta yhden vuokraajan tietokantaan (tai päinvastoin). Katso [joustavasti tietokannan Jaa Yhdistä työkalun opetusohjelma](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Joustavasti tietokannan projektit](sql-database-elastic-jobs-overview.md) (esikatselu): työt avulla voit hallita paljon Azure SQL-tietokannat. Suorittaa helposti järjestelmänvalvojan toimintoja, kuten rakenteen muutokset, tunnistetietojen hallinta, viittaus tietojen päivittämisen, suorituskykyyn liittyvä tietojen kerääminen tai vuokraajan (asiakas) telemetriatietojen sivustokokoelman käyttämällä työt.
* [Joustavasti tietokantakyselyn](sql-database-elastic-query-overview.md) (esikatselu): Voit suorittaa Transact-SQL-kyselyn, joka ulottuu useiden tietokantojen. Näin raportointityökalujen esimerkiksi Excel PowerBI, Tableau, yhteys.
* [Joustavasti tapahtumat](sql-database-elastic-transactions-overview.md): tämän ominaisuuden avulla voit suorittaa tapahtumia, jotka ulottuvat useita tietokantoja Azure SQL-tietokantaan. .NET-sovellusten avulla ADO .NET joustavasti tietokanta-tapahtumat, ja integroida tuttuja ohjelmoinnin kokemus [System.Transaction luokkien](https://msdn.microsoft.com/library/system.transactions.aspx)avulla.

Alla olevassa kuvassa näkyy arkkitehtuuri, joka sisältää suhteessa tietokantojen kokoelma **joustavasti tietokannan lisäominaisuuksia** .

Tässä kuvassa tietokannan värit vastaavat mallit. Tietokantojen sama väri jakaa samaan rakenteeseen.

1. **Azure SQL-tietokantoja** joukko isännöidään Azure käyttämällä sharding arkkitehtuuri.
2. **Joustavasti tietokannan asiakkaan kirjaston** käytetään shard hallintaa varten.
3. Tietokantojen alijoukkoa liikkeelle **joustavasti tietokannan resurssivarantoon**. (Katso [resurssivarantoon ominaisuudet?](sql-database-elastic-pool.md)).
4. Luodun **joustavasti tietokannan työn** toimii ajoitettu tai ajoittamattomaan T-SQL-komentosarjoja kaikki tietokannoissa.
5. **Jaa Yhdistä työkalun** käytetään voit siirtää tietoja yhden shard.
6. **Joustavasti tietokantakyselyn** avulla voit kirjoittaa kyselyn, joka ulottuu kaikki tietokannat shard määrittäminen.
7. Voit suorittaa tapahtumia, jotka ulottuvat useita tietokantoja **joustavasti tapahtumat** . 


![Joustavasti Tietokantatyökalut][1]


## <a name="why-use-the-tools"></a>Miksi työkaluilla?

Saavuttamiseksi joustavuus ja mittakaava, cloud sovellukset on jo yksinkertaista VMs ja blob-säiliö yksinkertaisesti lisääminen tai vähentäminen yksiköt tai suurenna power. Mutta se on ollut tilallisten tietojen käsittely relaatiotietokannasta hankala. Näissä tilanteissa kulutusluottomuotoja haasteet:

* Ja kapasiteetin havainnollistamiseen relaatiotietokannasta-osan pienentäminen.
* Hallinta kohdepisteet, joita voi syntyä vaikuttavia tietyn osan tiedoista – kuten erityisen varattu loppu-Asiakas (Alihallinta).

Perinteisesti Tällaisia tilanteita, joissa on korjattu investointien suurempi skaalaus tietokannan palvelimissa sovellus tukee. Tämä asetus on rajoitettu kuitenkin pilveen, jossa kaikki käsittely tapahtuu ennalta määritettyjä tärkeämpää laitteistoon. Sen sijaan tietojen jakaminen ja useita samannimisen rakenteellinen tietokantoja käsitteleminen (asteikko-kohtaa kuvion nimeltään "sharding") on vaihtoehtoinen perinteinen asteikko ylöspäin toimintamalleja sekä kustannusten ja joustavuus.

## <a name="horizontal-and-vertical-scaling"></a>Vaaka- ja skaalaus

Alla olevassa kuvassa vaaka- ja mittasuhteita skaalauksen, joka voi skaalata joustavasti tietokantojen basic tavalla.

![Vaaka-ja pystysuuntainen Scaleout][2]

Skaalauksen vaakasuunnassa viittaa lisäämällä tai poistamalla tietokantojen säätää kapasiteetin tai yleistä suorituskykyä. Tätä kutsutaan myös "skaalaus-. Sharding, jossa tiedot on osioitu yli samannimisen rakenteellisen tietokantojen kokoelma on yleinen tapa toteuttamisesta skaalauksen vaakasuunnassa.  

Skaalauksen pystysuunnassa viittaa suurenee tai pienenee yksittäisiä tietokannan suorituskykytaso — Tämä tunnetaan myös nimellä "skaalaus."

Useimmat cloud-akseli tietokantasovelluksia käyttämällä strategioita näiden kahden yhdistelmä. Esimerkiksi ohjelmiston kuin palvelusovelluksen saa käyttää valmistelu end-asiakkaille skaalauksen vaakasuunnassa ja skaalauksen pystysuunnassa, jotta jokaisen end-asiakkaan tietokannan Suurenna tai Pienennä resurssien tarpeen mukaan työmäärää.

* Skaalauksen vaakasuunnassa hallitaan [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md)käytöstä.

* Skaalauksen pystysuunnassa tehdään muuttaminen service-tason Azure PowerShell cmdlet-komentojen avulla tai lisäämällä tietokantojen joustavasti resurssivarantoon.

## <a name="sharding"></a>Sharding

*Sharding* on tekniikalla, jotta suurten tietomäärien samannimisen rakenteen jakaa riippumaton tietokantojen määrä. On erityisen suosittu cloud kehittäjien luominen ohjelmiston Service (SAAS) tarjouksia Lopeta asiakkaat ja yritykset kanssa. Lopeta asiakkaiden viitataan usein "alihallinnat". Sharding voidaan tarvita jokin muu luku syistä:  

* Tietojen määrä on liian suuri sopimaan yhteen tietokantaan rajoitukset
* Yleistä työmäärää tapahtuman siirtonopeuden ylittää yhden tietokannan ominaisuudet
* Alihallinnat saattaa edellyttää fyysinen toisistaan erillään, jotta erillisten tietokantojen tarvittavat kunkin vuokraajakohteen
* Tietokannan eri osien täytyy mahdollisesti sijaitsevat eri paikkojen vaatimustenmukaisuus, suorituskyky tai geopoliittisten syistä.

Muissa tilanteissa, kuten nieltynä tietojen hajautettu laitteilla sharding voidaan täyttää tietokannoissa, jotka on järjestetty toimialueeseesi joukko. Esimerkiksi erillisellä on varattu kunkin päivän tai viikon. Siinä tapauksessa sharding avain voi olla kokonaisluvun (käytössä kaikilla riveillä sharded taulukot) päivämäärä ja päivämääräalueen tietojen noutaminen kyselyjen on reititetty soveltamalla alijoukkoon tietokantojen alueeseen poistettavaan.

Sharding toimii parhaiten, kun sovellus jokaisen tapahtuman voidaan rajoittaa sharding avaimen yksittäisen arvon. Varmistetaan, että kaikki tapahtumat on paikallisen tietokannan avaamiseksi.

## <a name="multi-tenant-and-single-tenant"></a>Usean vuokraajan ja yksi vuokraajan

Jotkin sovellukset käyttävät helpoin tapa luominen erillisellä kunkin vuokraajakohteen. Tämä on **yhtä vuokraajan sharding kaava** , joka eristää, varmuuskopiointi ja palauttaminen mahdollisuuden ja skaalausta osoitteessa vuokraajan rakeisuuden resurssin. Yhtä vuokraajan sharding tietokantojen on liitetty tiettyyn vuokraajan tunnus-arvo (tai asiakkaan avainarvon), mutta avaimen ei aina tarvitse itse tiedoissa. Asiakas-kirjasto voidaan yksinkertaistaa ja sovelluksen vastuu sivupyynnön reitittämiseen oikeaan tietokantaan – on.

![Yhtä vuokraajan ja usean vuokraajan][4]

Muiden skenaariot pack useita alihallinnat yhteen yhdeksi tietokantojen kuin eristämisestä niitä eri tietokantoihin. Tämä on tyypillinen **usean vuokraajan sharding kuvio** – ja voivat perustuva sovelluksen hallitsee paljon hyvin pieni alihallinnat seikka. Usean vuokraajan sharding tietokannan taulukoiden rivit ovat kaikki suunniteltu avain vuokraajan tunnus tai sharding-näppäintä. Taas sovelluksen taso on vastuussa reititys vuokraajan pyynnön haluamasi tietokanta ja tämä tukee joustavasti tietokannan asiakas-kirjasto. Lisäksi rivin käyttäjätason suojauksen voidaan Suodata rivit kunkin vuokraajan käyttöoikeus – Lisätietoja on artikkelissa [usean vuokraajan sovellusten joustavasti Tietokantatyökalut ja rivin käyttäjätason suojauksen](sql-database-elastic-tools-multi-tenant-row-level-security.md). Tietoja tietokantojen kesken uudelleenjakamista voivat olla tarpeen usean vuokraajan sharding kuvion ja Tämä helpottaa joustavasti tietokantatyökalu Jaa ja yhdistäminen. Lisätietoja SaaS-sovellusten käyttäminen joustavasti jakavat kuviot rakenne-kohdassa [Rakenne kuviot usean vuokraajan SaaS sovellusten Azure SQL-tietokantaan](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-to-single-tenancy-databases"></a>Siirrä tiedot useita yhden vuokraajan tietokantoihin

SaaS sovelluksen luotaessa on tyypillinen tarjota mahdollisille asiakkaille ohjelmiston kokeiluversio. Tässä tapauksessa on edullinen usean vuokraajan tietokannan käytettävät tiedot. Mahdollinen asiakas tullessa asiakkaan yhden vuokraajan tietokannan on kuitenkin paremmin, koska se sisältää suorituskyvyn parantamiseksi. Jos asiakkaan olit luonut tietojen kokeilujakson aikana, tietojen siirtäminen yhden vuokraajan uuden tietokannan usean vuokraajan [Jaa ja yhdistäminen-työkalun](sql-database-elastic-scale-overview-split-and-merge.md) avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Esimerkki sovellusta, joka osoittaa asiakirjakirjaston Katso [joustavasti Datababase työkalut käytön aloittaminen](sql-database-elastic-scale-get-started.md).

Jos haluat muuntaa aiemmin luotujen tietokantojen työkaluilla, katso [artikkelista aiemmin luotujen tietokantojen asteikko itsestään](sql-database-elastic-convert-to-use-elastic-tools.md).

Joustavasti tietokannan resurssivarannon tiedot näkyviin Katso [hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md)tai Luo uusi ryhmä [opetusohjelma](sql-database-elastic-pool-create-portal.md).  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

