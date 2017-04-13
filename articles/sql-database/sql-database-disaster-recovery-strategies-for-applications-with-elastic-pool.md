<properties 
   pageTitle="Cloud ratkaisujen suunnitteleminen käyttämällä SQL-tietokannan Geo-replikointi palauttaminen | Microsoft Azure"
   description="Lue, miten voit suunnitella cloud-ratkaisun tietojen palauttamista varten valitsemalla oikean automaattisesti kuvion."
   services="sql-database"
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA" 
   ms.date="07/16/2016"
   ms.author="sashan"/>

# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pool"></a>Sovellusten SQL-tietokannan joustavasti resurssivarantoon varalle 

Olemme asiat pilvipalveluihin eivät ole erehtymättömiä ja kriittinen tapaukset vuosien voit ja tapahtuu. SQL-tietokanta on useita ominaisuuksia säätää Liiketoiminnan jatkuvuus sovelluksesi nämä tapaukset toteutuessa. Tietokantojen [joustavasti jakavat](sql-database-elastic-pool.md) ja erillinen tukevat tietojen palauttaminen ominaisuuksia samanlaisia. Tässä artikkelissa kuvataan useita DR strategioita, Lisää joustavasti jakavat, joka hyödyntää SQL-tietokantaan liiketoiminnan jatkuvuuden nämä ominaisuudet.

Tässä artikkelissa tarkoitetaan Käytämme canonical SaaS ISV sovelluksen kuvio:

<i>Nykyaikainen pilvestä perusteella web application säännösten yksi SQL-tietokantaan loppu kullekin käyttäjälle. ISV on suuri määrä asiakkaat ja käyttää vuoksi useat tietokannat nimeltään vuokraajan tietokannat. Koska vuokraajan tietokantoja on yleensä odottamatonta toimintaa kuviot, ISV käyttää joustavasti resurssivarantoon Soita kustannusten tietokannan hyvin ennakoitavissa pitkän ajan. Joustavasti resurssivarantoon yksinkertaistaa myös suorituskyvyn hallinta, kun käyttäjä tehtävän piikkejä. Lisäksi vuokraajan tietokantojen sovellus käyttää myös useita tietokantoja Hallitse käyttäjäprofiileja, suojaus, kerätä käyttötavat jne. Yksittäisten alihallinnat saatavuuden ei vaikuta sovelluksen käytettävyys kokonaisuus. Kuitenkin käytettävyyttä ja suorituskyvyn tietokantojen hallinta on erittäin tärkeää sovelluksen-funktio ja jos hallinta-tietokantoja on offline-tilassa koko sovellus on offline-tilassa.</i>  

Paperin loput käsittelemme solualueeseen skenaariot tiukat käytettävyys vaatimusten luotuja kustannukset luottamuksellisia käynnistys-sovelluksista, jotka kattavat DR strategioita.  

## <a name="scenario-1-cost-sensitive-startup"></a>Käyttötilanne 1. Kustannusten luottamuksellisia käynnistys

<i>Olen käynnistys yrityksen ja olen kustannusten erittäin luottamuksellisia.  Haluan yksinkertaistaa verkkosivuston käyttöönotto- ja sovelluksen ja olen valmis on rajoitettu SLA yksittäisten asiakkaiden. Mutta haluan varmistetaan kokonaisuutena ei koskaan offline-tilassa.</i>

Tyydyttämiseksi yksinkertaisuuden vaatimus tulee asentaa vuokraajan piilotetusta yhden joustavasti resurssivarantoon Azure alueen valittua ja ota käyttöön kuin geo replikoida erillinen tietokannat hallinta-tietokannat. Käytä alihallinnat palauttaminen-geo palauttaminen, joka sisältää ilman lisäkustannuksia. Tietokantojen hallinta käytettävyyttä, jotta ne on oltava geo replikoida toisen alueen (vaihe 1). Tässä skenaariossa tietojen palauttaminen-määritys jatkuvaa kustannukset on toissijainen tietokannat kokonaiskustannukset. Seuraavassa kaaviossa on esitelty määritysten.

![Kuva 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Jos ensisijainen alueen käyttökatkosta palautus vaihe vaiheelta, miten sovellus verkossa on kuvattu seuraavan kaavion avulla.

- Välittömästi automaattisesti johdon tietokannat DR alue (2). 
- Muuta sovelluksen yhteysmerkkijonon osoittamaan DR-alue. Kaikki uudet tilit ja vuokraajan tietokannat luodaan DR alueen. Olemassa olevat asiakkaat näkevät tietonsa tilapäisesti poissa käytöstä.
- Voit luoda joustavasti varanto on sama kokoonpano kuin alkuperäinen resurssivarantoon (3). 
- Geo palauttaminen avulla voit luoda kopioita vuokraajan tietokantoja (4). Voit harkitse käynnistävä yksittäisiä palauttaa peruskäyttäjän yhteyksien mukaan tai käytät joitakin muita sovelluksen tietyn prioriteetin mallia.

Tässä vaiheessa sovelluksesi on online-tilaan DR alueen, mutta jotkin asiakkaat ilmetä viive käytettäessä tietonsa.

![Kuva 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Jos käyttökatkosta on väliaikainen, on mahdollista, että ensisijainen alueen palautetaan mukaan Azure ennen kuin kaikki palauttaa ovat valmiit, DR alueen. Tässä tapauksessa kannattaa orchestrate siirtäminen sovelluksen ensisijainen alue. Kestää esitelty Seuraava kaavio vaiheet.
 
- Peruuta kaikki avoimet geo-Palauta pyynnöt.   
- Automaattisesti hallinta tietokannat ensisijainen alueen (5). Huomautus: Alueen palautuksen jälkeen vanha primaareista automaattisesti tullut secondaries. Nyt ne vaihtuu roolit uudelleen. 
- Muuta sovelluksen yhteysmerkkijonon osoittamaan yhteys ensisijainen alue. Nyt kaikki uudet tilit ja vuokraajan tietokannat luodaan ensisijainen alueen. Jotkin asiakkaat näkevät tietonsa tilapäisesti poissa käytöstä.   
- Määrittää kaikkien tietokantojen DR resurssivarantoon jotta niitä ei voi muokata DR alueen (6) vain luku-tilaan. 
- Kunkin tietokannalle DR sarjassa, joka on muuttunut palautus nimeäminen uudelleen tai poista vastaavan tietokannat ensisijainen varannon (7). 
- Kopioi päivitetyt tietokantojen DR käytetään varannon ensisijainen resurssivarantoon (8). 
- Poista DR resurssivarantoon (9)

Tässä vaiheessa sovelluksen näkyvät online ensisijainen alue, jossa kaikki vuokraajan tietokannoissa käytettävissä ensisijainen resurssivarantoon.

![Kuva 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Avaimen **hyötyä** , tämä strategia on pieni tietojen taso kopion jatkuvaa kustannukset. Varmuuskopioiden otetaan automaattisesti SQL-tietokanta-palvelu ei ole sovelluksen suorittamaan ja ilman lisäkustannuksia.  Kustannus on syntynyt vain, kun joustavasti tietokannat palautetaan. **Trade-off** on valmis palautus vuokraajan kaikkien tietokantojen vievät paljon aikaa. Se riippuu kokonaissumma palauttaa käynnistät DR alueen lukumäärä ja yleisesti koko vuokraajan tietokannat. Vaikka joitakin alihallinnat palauttaa priorisoida muiden päälle, sinun pikaluistelukilpailuissa muut palauttaa, jotka on aloitettu poikkeavaa palvelu arbitrate ja Pienennä olemassa olevat asiakkaat tietokantojen Yleinen vaikutus rajoita kanssa. Lisäksi vuokraajan tietokantojen palautus ei käynnisty, kunnes uusi joustavasti varanto DR alueen luodaan.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Käyttötilanne 2. Kehittynyt sovelluksen Porrastettu-palvelun kanssa 

<i>Olen kehittynyt SaaS-sovelluksen Porrastettu palvelun tarjoukset ja eri palvelutasosopimuksia kokeiluversion asiakkaat ja maksaa asiakkaiden kanssa. Kokeiluversio-asiakkaiden minun mahdollisimman paljon pienentää. Kokeiluversion asiakkaat voivat viedä aikaa, mutta haluan pienentää sen todennäköisyys. Maksavien asiakkaiden minkä tahansa käyttökatkot on lentojen riskin. Joten haluan Varmista, että kyseinen maksaa asiakkaat voivat aina tietoihinsa.</i> 

Tukemaan tässä skenaariossa pitäisi erottaa kokeiluversion alihallinnat muista maksettu alihallinnat mukaan laskemisesta erillisessä joustavasti jakavat. Kokeiluversion asiakkaiden on pienempi eDTU vuokraajan ja pienempi SLA pidempi palauttaminen ajan kohden. Maksavien asiakkaiden olisi suurempi eDTU vuokraajan ja uudempi versio SLA kohden kanssa resurssivarantoon. Voit varmistaa palauttaminen pienin aika maksavien asiakkaiden vuokraajan tietokantoja on oltava geo replikoida. Seuraavassa kaaviossa on esitelty määritysten. 

![Kuva 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Ensimmäinen tilanne, kuten hallinta-tietokannat on aivan aktiivinen niin itsenäisen geo replikoida tietokannan käyttää sitä (1). Näin varmistat, että uusi asiakas-tilaukset, profiilin päivitykset ja muut hallintatoiminnot ennakoitavissa suorituskykyä. Ensisijainen alue on alue, jossa hallinta-tietokannat primaareista asuvat ja alue, jossa hallinta-tietokannat secondaries asuvat päivitetään DR-alue.

Maksavien asiakkaiden vuokraajan tietokantoja on aktiivinen tietokantojen "maksettu" varannon ensisijainen alueen valmistelun yhteydessä. Toissijainen resurssivarantoon saman niminen DR alueen pitäisi valmistella. Kunkin vuokraajan olisi geo replikoida toissijainen resurssivarantoon (2). Tämä ottaa käyttöön nopeasti palautus kaikkien vuokraajan tietokantojen käyttäminen automaattisesti. 

Jos käyttökatkosta ensisijainen alueen, palautus vaihe vaiheelta, miten sovellus verkossa esitelty seuraavan kaavion:

![Kuva 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

- Onnistu heti hallinta tietokannat DR alueen (3) kautta.
- Voit muuttaa sovelluksen yhteysmerkkijonon osoittamaan DR-alue. Nyt kaikki uudet tilit ja vuokraajan tietokannat luodaan DR alueen. Kokeiluversion asiakkaista lukee tietonsa tilapäisesti poissa käytöstä.
- Automaattisesti maksettu vuokraajan tietokantojen resurssivarantoon DR-alue, jos haluat palauttaa heti käytettävyyden (4). Koska vikasietotilaa on nopea metatietojen tason muutos, kannattaa harkita optimointia missä yksittäisiä failovers on saatu pyydettäessä esiin peruskäyttäjän yhteydet. 
- Toissijainen sovellussarjan eDTU-kokoa on pienempi kuin ensisijaisen sillä toissijainen tietokantojen vain kapasiteetti käsittelemään muuta lokit secondaries olon aikana, jos lisäät tulee välittömästi resurssivarannon resursseja nyt sopimaan kaikki alihallinnat (5) koko työmäärää. 
- Luo uusi joustavasti varanto on sama nimi ja sama kokoonpano DR alueen kokeiluversion asiakkaat-tietokantoja (6). 
- Kun kokeiluversion asiakkaiden varanto on luotu, geo palauttaminen avulla voit palauttaa yksittäisen vuokraajan kokeiluversion tietokantojen uusi ryhmä (7). Voit harkitse käynnistävä yksittäisiä palauttaa peruskäyttäjän yhteyksien mukaan tai käytät joitakin muita sovelluksen tietyn prioriteetin mallia.

Tässä vaiheessa sovellus on online-tilaan DR-alue. Kaikkia asiakkaita käyttämään tietonsa samalla, kun kokeiluversion asiakkaiden ilmetä viive tietonsa käytettäessä.

Kun ensisijainen alue on palautettu mukaan Azure *jälkeen* voit jatkaa sovelluksen suorittamisen alueella olevat DR alueen sovelluksen palautetut tai voit päättää takaisin ensisijainen alue. Jos ensisijainen alue on palautettu *ennen* automaattisesti prosessi on valmis, ota huomioon puuttuessa Edellinen saman tien. Tuntisesta vievät esitelty seuraavan kaavion vaiheet: 
 
![Kuva 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

- Peruuta kaikki avoimet geo-Palauta pyynnöt.   
- Automaattisesti hallinta-tietokannat (8). Kun haluamasi alue palautus on vanha ensisijainen oli muuttuvat automaattisesti toissijaisen. Nyt on ensisijainen uudelleen.  
- Automaattisesti maksettu vuokraajan-tietokannat (9). Vastaavasti alueen palautuksen jälkeen vanha primaareista automaattisesti muuttuvat secondaries. Nyt ne saattavat muuttua primaareista uudelleen. 
- Määritä palautettu kokeiluversion tietokannoissa, jotka ovat muuttuneet DR alueen vain luku-tilassa (10).
- Kunkin tietokannalle kokeiluversion asiakkaiden DR sarjassa, palautus on muutettu kokeiluversion asiakkaiden ensisijainen varannon (11) vastaavan tietokannan poistaminen tai nimeäminen uudelleen. 
- Kopioi päivitetyt tietokantojen DR käytetään varannon ensisijainen resurssivarantoon (12). 
- Poista DR resurssivarantoon (13) 

> [AZURE.NOTE] Automaattisesti-toiminto on asynkroninen. Pienennä on tärkeää vuokraajan tietokantojen automaattisesti komennon suorittamista erissä vähintään 20 tietokantojen palauttamisaika. 

Avaimen **hyötyä** , tämä strategia on, että se sisältää maksavien asiakkaiden suurin SLA. Se myös takaa uutta kokeiluversiota on suojattu heti, kun kokeiluversion DR varanto on luotu. **Trade-off** on, että nämä asetukset kasvattaa maksettu asiakkaiden vuokraajan tietokantojen kokonaiskustannukset toissijainen DR resurssivarantoon kustannusten mukaan. Lisäksi jos toissijaisen varanto on erikokoinen, maksavien asiakkaiden pienempi suorituskyky olla automaattisesti jälkeen ennen kuin DR alueen resurssivarantoon päivitys on valmis. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Tapaus 3. Maantieteellisesti jaetun sovelluksen Porrastettu-palvelun kanssa

<i>Minulla on Porrastettu palvelun tarjoukset kehittynyt SaaS-sovellukseen. Haluan hyvin kohdetoimialueen SLA tarjoaminen maksettu asiakkaat ja verkkohuijausyritysten vaikutus toteutuessa katkokset sillä edes lyhyt keskeytyskohdasta heikentää asiakkaan tyytymättömyyden. On tärkeää, että maksavien asiakkaiden voit aina tietoihinsa. Kokeet ovat ilmaisia ja ei SLA tarjotaan kokeilujakson aikana.</i> 

Tässä skenaariossa tukemaan pitäisi olla kolme erillistä joustavasti jakavat. Kaksi yhtä koon jakavat kanssa hyvin eDTUs tietokantaa kohden tulee valmisteltu sisältämään maksettu asiakkaiden vuokraajan tietokantojen kaksi eri alueilla. Kolmannen resurssivarantoon kokeiluversion alihallinnat, jotka sisältävät on pienempi eDTUs tietokantaa kohden ja valmisteltava yhteen kaksi aluetta.

Voit varmistaa katkokset palauttaminen pienin ajan maksavien asiakkaiden vuokraajan tietokantoja on oltava geo replikoida 50 %: lla ensisijainen tietokantojen kussakin kummallakin alueella. Vastaavasti kunkin alueen on toissijainen tietokantojen 50 prosenttia. Tällä tavalla Jos alue on offline-tilassa vain 50 % maksettu asiakkaiden tietokantojen heikentyä ja on automaattisesti. Muita tietokantoja säilyvät. Seuraavassa kaaviossa on esitetty nämä määritykset:

![Kuva 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Edellisen käyttötavoista, kuten hallinta-tietokannat on aivan aktiivinen niin kannattaa määrittää niitä erillinen geo replikoida tietokannat (1). Näin varmistat, että uusi asiakas-tilaukset, profiilin päivitykset ja muut hallintatoiminnot ennakoitavissa suorituskykyä. Alueen A olisi hallinta-tietokannat ensisijainen alue ja alueen B käytetään hallinta-tietokannat palauttamista varten.

Maksavien asiakkaiden vuokraajan tietokantoja on myös geo replikoida mutta primaareista ja secondaries Jaa alueen A ja B (2)-alueen välillä. Tällä tavalla, joihin käyttökatkosta vaikuttaa vuokraajan ensisijainen tietokantojen voit automaattisesti sen alueen ja malleja. Toinen puoli vuokraajan tietokantoja ei heikentyä ollenkaan. 

Seuraavassa kaaviossa on kuvattu palautus vaihe vaiheelta, miten toimia, jos käyttökatkosta ilmenee alueen A.

![Kuva 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

- Onnistu heti hallinta tietokantojen alueen B (3) kautta.
- Voit muuttaa sovelluksen yhteysmerkkijonon osoittamaan alueen B. muokata johdon tietokannat, varmista, että uusi tili ja vuokraajan tietokannat luodaan alueen B ja vuokraajan aiemmin luotujen tietokantojen löytyvät on myös hallinta-tietokannat. Kokeiluversion asiakkaista lukee tietonsa tilapäisesti poissa käytöstä.
- Automaattisesti maksettu vuokraajan tietokantojen resurssivarantoon 2 alue B, jos haluat palauttaa heti käytettävyyden (4). Koska vikasietotilaa on nopea metatietojen tason muutos, kannattaa harkita optimointia missä yksittäisiä failovers on saatu pyydettäessä esiin peruskäyttäjän yhteydet. 
- Nyt jälkeen ryhmä 2 sisältää vain ensisijainen tietokantoja yhteensä työmäärää varannon kasvattaa, joten tulee välittömästi Lisää eDTU kokoonsa (5). 
- Luo uusi joustavasti varanto on sama nimi ja sama kokoonpano alueen B kokeiluversion asiakkaat-tietokantoja (6). 
- Ryhmän luomisen jälkeen geo palauttaminen avulla voit palauttaa yksittäisen vuokraajan kokeiluversion tietokannan resurssivarantoon (7). Voit harkitse käynnistävä yksittäisiä palauttaa peruskäyttäjän yhteyksien mukaan tai käytät joitakin muita sovelluksen tietyn prioriteetin mallia.


> [AZURE.NOTE] Automaattisesti-toiminto on asynkroninen. Pienennä on tärkeää vuokraajan tietokantojen automaattisesti komennon suorittamista erissä vähintään 20 tietokantojen palauttamisaika. 

Tässä vaiheessa sovellus on online-tilaan alueella B. Kaikkia asiakkaita käyttämään tietonsa samalla, kun kokeiluversion asiakkaiden ilmetä viive tietonsa käytettäessä.

Kun alueen A on palautettu, sinun täytyy päättää, jota haluat käyttää alueen B kokeiluversion asiakkaiden tai tuntisesta kokeiluversion asiakkaiden resurssivarannon käyttäminen alueen A. Yhden ehdon voi olla vuokraajan kokeiluversion tietokantojen muokattu palautuksen jälkeen %. Riippumatta siitä, jota tarvitset saldo uudelleen maksettu alihallinnat kaksi jakavat välillä. Seuraavassa kaaviossa on kuvattu prosessin Jos vuokraajan kokeiluversion tietokantojen ette takaisin alueen A.  
 
![Kuva 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

- Peruuttaa kokeiluversion DR resurssivarantoon kaikki avoimet geo-Palauta pyynnöt.   
- Automaattisesti hallinnan tietokanta (8). Alueen palautuksen jälkeen vanha ensisijainen oli automaattisesti sai toissijaisen. Nyt on ensisijainen uudelleen.  
- Valitse mitkä maksettu vuokraajan tietokantojen epäonnistuu takaisin resurssivarantoon 1 – Valmistele automaattisesti niiden secondaries (9). Alueen palautuksen jälkeen kaikki tietokannat varannon 1 sai automaattisesti secondaries. Nyt 50 % niistä tulee primaareista uudelleen. 
- Pienentää resurssivarantoon 2, alkuperäinen eDTU (10).
- Määritä kaikki palauttaa kokeiluversion tietokantojen alueen B vain luku-tilassa (11).
- Kokeiluversion DR resurssivarantoon, joka on muuttunut palautus kunkin tietokannan nimeä tai poista vastaava tietokanta kokeiluversion ensisijainen varannon (12). 
- Kopioi päivitetyt tietokantojen DR käytetään varannon ensisijainen resurssivarantoon (13). 
- Poista DR resurssivarantoon (14) 

Tämä strategia avaimen **etuja** ovat:

- Se tukee eniten kohdetoimialueen SLA maksavien asiakkaiden sillä sillä varmistetaan käyttökatkosta et voi vaikuttaa yli 50 prosenttia vuokraajan tietokannat. 
- Se takaa uutta kokeiluversiota on suojattu heti, kun kirjausketju DR resurssivarantoon luodaan palautuksen aikana. 
- Se sallii toissijainen tietokantojen resurssivarantoon 1 ja 2 resurssivarantoon 50 prosenttia on varmasti on vähemmän aktiivinen ensisijainen tietokantojen varannon resursseja käyttää tehokkaammin.

Tärkeimmät **valinnat** ovat seuraavat:

- Hallinta-tietokannat vastaan CRUD-toiminnot on pienempi viive yhdistetty alue kuin A yhdistetty alue B, kun ne voidaan kohdistaa hallinta-tietokannat ensisijainen loppukäyttäjille käyttäjille.
- Monimutkaisempia hallinnan tietokannan rakenteen edellyttää. Esimerkiksi vuokraajan kunkin tietueen on on sijainti-tunniste, joka on vaihdettava automaattisesti ja tuntisesta aikana.  
- Maksavien asiakkaiden pienempi kuin tavanomainen suorituskyky saattaa olla, ennen kuin alueella B resurssivarantoon päivitys on valmis. 

## <a name="summary"></a>Yhteenveto

Tässä artikkelissa keskitytään SaaS ISV usean vuokraajan-sovellus käyttää tietokannan tason varalle. Valitset määrittäminen perustuu sovellus, kuten business-mallin haluat tarjoaminen asiakkaille, budjetin rajoituksen jne SLA tarpeiden mukaan … Kunkin kuvattu strategia erittelee edut ja trade-off, jotta voit tehdä päätöksen. Lisäksi tietylle sovellukselle todennäköisesti Sisällytä Azure muita osia. Jotta Tarkista niiden liiketoiminnan jatkuvuuden ohjeet ja orchestrate tietokannan tason heidän kanssaan palauttaminen. Lisätietoja palautus Azure tietokannan sovellusten hallinta-viitata [palauttaminen suunnitteleminen cloud ratkaisuja](./sql-database-designing-cloud-solutions-for-disaster-recovery.md) .  


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja palauttaminen automaattisen varmuuskopioinnin käyttämisestä on artikkelissa [tietokannan palvelun käynnistämä-varmuuskopioista palauttaminen](sql-database-recovery-using-backups.md)
- Lisätietoja nopeampaa palautusasetukset on artikkelissa [Aktiivinen-Geo-replikoinnin](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
