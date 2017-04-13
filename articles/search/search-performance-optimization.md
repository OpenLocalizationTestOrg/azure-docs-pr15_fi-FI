<properties 
    pageTitle="Azure haun suorituskyky ja optimointi huomioitavista | Microsoft Azure" 
    description="Paranna suorituskykyä Azure haun ja määrittää parhaan mahdollisen asteikko" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure haun suorituskyky ja optimointi huomioon otettavia seikkoja

Hyvien hakuja on monta mobile onnistumisen ja verkkosovellusten. Kiinteistöt, valitse käytettävä auton kauppapaikat online-luetteloihin Pikahaun tuloksia vaikuttavat ja asiakas-toiminto. Tämä asiakirja on tarkoitettu löytämään parhaat käytännöt hankkiminen mahdollisimman paljon irti Azure Etsi, erityisesti tilanteita skaalattavuus, edistyksellisten vaatimuksia monikielisyyden tukea Ohje tai mukautetun luokittelu.  Lisäksi tässä asiakirjassa ympärille sisäiset ja kattaa tavoista, jotka toimivat tehokkaasti tosielämän asiakas-sovelluksissa.

## <a name="performance-and-scale-tuning-for-search-services"></a>Suorituskyky ja Etsi palveluita parantaminen asteikko

Olemme kaikki käytetään hakukoneet, kuten Bing- ja Google- ja tarjoavat erinomainen suorituskyky.  Tuloksena, kun asiakkaat haun käyttävä sivusto tai mobiilisovelluksen, ne olettaa suorituskyvyn muistuttavat.  Kun optimoiminen haun suorituskyvystä parhaan tavoilla on keskittyminen viive, eli ajan kyselyn kestää suorittaa ja palauttaa tuloksia.  Kun haku viive optimoiminen on tärkeä ottaa:

1. Valitse kohde viive (tai enimmäisajan), joka pyytää tyypillinen haun otettava suorittamiseen.

2. Luo ja testaa vastaan search-palvelun kanssa realistisia tietojoukko mitata viive näitä prosentteja todellinen työmäärä.

3. Aloita vähän kyselyitä sekunnissa (QPS) ja siirry kasvata suorittaa testi, kunnes kyselyviiveen laskee määritettyyn kohdearvoon viive alapuolelle.  Tämä on tärkeää ensisijaisen, jotka auttavat suunnittelemaan akselille, kun sovellus kasvaa käyttö.

4. Jos mahdollista, käyttää HTTP-yhteyksiä.  Jos käytössäsi on Azure haun .NET SDK-paketissa, se tarkoittaa pitäisi käyttää uudelleen esiintymä tai [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) esiintymän, ja jos käytössäsi on REST-Ohjelmointirajapinta, yksi HttpClient pitäisi käyttää uudelleen.
 
Nämä luomisen aikana testin työmääriä, on joitakin ominaisuuksia Azure haun kannattaa pitää mielessä:

1. Se on mahdollista push niin monta haun kyselyt samalla kertaa, että Azure-hakupalvelun resurssit täyttyminen.  Tällöin näet HTTP 503 vastauksen koodit.  Tästä syystä on parasta eri alueiden Nähdäksesi viive korvaukset erot, kun lisäät useita search-pyyntöjen pyyntöjen haku alkaa.

2. Azure Etsi sisällön lataamisen vaikuttaa yleistä suorituskykyä ja Azure Search-palvelun viive.  Jos haluat lähettää tietoja, kun käyttäjät suorittavasi, on tärkeä ottaa huomioon tehtäviä testejä.

3. Kaikki hakukyselyn suorittaa suorituskyvyn samalla tasolla.  Esimerkiksi asiakirjan haku tai Etsi ehdotus yleensä suorittaa nopeammin kuin kysely, joka sisältää useita muita ja suodattimet.  Kannattaa tehdä haluamiasi huomioon testejä luotaessa eri kyselyt.  

4. Search-pyyntöjen muunnelma on tärkeä, koska Jos suoritat saman search-pyyntöjen jatkuvasti, tietojen tallentaminen välimuistiin käynnistyy jotta suorituskyvyn ulkoasua, kuin se Lisää erillisiä kyselyn avulla määrittää.

> [AZURE.NOTE] [Visual Studio kuormituksen testaus](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) on hyvä tapa suorittaa oman vertailutestien sen avulla voit suorittaa pyyntöjen, koska kyselyitä, jotka perustuvat Azure haun suorittamiseen tarvitset ja mahdollistaa parallelization pyyntöihin.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Skaalausta Azure hakeminen hyvin kyselyn korvaukset ja rajoittanut pyynnöt

Kun saa liikaa kyselyn pyynnöt tai suurempi kuin kohde viive-kurssit-parantavat kyselyn kuormituksen, voit hakea pienentää viive korvaukset jollakin seuraavista tavoista:

1. **Suurenna replikoiden:**  Replikan muistuttaa tietojen salliminen Azure haun voivat ladata useita kopioita vastaan kopio.  Kaikki kuormituksen tasaaminen ja replikoiden tietojen replikoinnin hallitsee Azure haun ja voit muuttaa määrä on kohdistettu palvelun milloin tahansa.  Voit varata enintään 12 replikoiden vakio search-palvelun ja 3 replikoiden Perushaku-palvelussa.  Replikat voidaan säätää [Azure Portal](search-create-service-portal.md) tai [Azure haun hallinnan API](search-get-started-management-api.md)avulla.

2. **Suurenna haun taso:**  Azure haku on [tasojen määrän](https://azure.microsoft.com/pricing/details/search/) ja kaikkien näiden tasoa tarjoaa suorituskyvyn eri tasoille.  Joskus voi olla niin paljon kyselyjä, jotka olet taso ei voi antaa riittävän alhainen viive korvaukset myös silloin, kun se on replikoita on saavutettu.  Tässä tapauksessa sinun kannattaa harkita hyödyntäminen suurempi haun tasoissa, kuten Azure haun S3 taso, joka on tilanteita, joissa on paljon asiakirjoja ja erittäin suuri kyselyn toiminnoista.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Hidas yksittäisten kyselyiden hakujen Azure skaalaus

Miksi viive korvaukset voi olla hidas voivat johtua on yksittäinen kyselystä kestää liian kauan.  Tässä tapauksessa lisääminen replikoita ei parantavat viive-kurssit.  Tässä esimerkissä on kaksi vaihtoehtoa käytettävissä:

1. **Suurenna osiot** Osio on tarkoitetut tietojen jakaminen koko resursseja.  Tästä syystä lisättäessä toinen osio tietojen saa jakaminen kahdeksi.  Kolmas osion jakautuu indeksi kolmen, jne.  Tämä on myös, että joissakin tapauksissa hidas kyselyjen toimii nopeammin vuoksi laskenta parallelization tehosteen.  On muutamia esimerkkejä kohtaa, johon on tullut tämän parallelization erittäin hyvin käyttäminen kyselyissä, joissa on pieni selektiivisyys kyselyt.  Tämä koostuu kyselyjä, jotka vastaavat useita tiedostoja tai kun faceting tarvitsee antaa laskee paljon tiedostoja päälle.  Koska ei tarvita sija suunnittelet asiakirjojen tai tiedostojen määrän laskeminen laskenta paljon, lisäämällä ylimääräisiä osioiden avulla voidaan luoda lisää laskenta.  

   Voi olla enintään 12 osioita vakio hakupalvelun ja 1-osion Perushaku-palvelussa.  Osioiden voidaan säätää [Azure Portal](search-create-service-portal.md) tai [Azure haun hallinnan API](search-get-started-management-api.md)avulla.

2. **Rajoittaa hyvin kardinaliteetti kentät:** Suuri kardinaliteetti kentän koostuu facetable tai suodatettavia kentän, joka on yksilöllisiä arvoja merkittäviä määrä, ja tuloksena on paljon resursseja laskemiseen tulosten päälle.   Esimerkiksi Product ID-tunnus tai kuvauksen kentän määrittäminen facetable suodatettavia tehdä hyvin kardinaliteetti koska tiedostoon tiedoston arvot on yksilöllinen. Suuri kardinaliteetti kenttien määrän rajoittaminen mahdollisuuksien mukaan.

3. **Suurenna haun taso:**  Siirtäminen ylöspäin suurempi Azure haun taso voi olla hidas kyselyjen suorituskyvyssä erikseen.  Kunkin korkeampi taso sisältää myös nopeamman suorittimen ja muistia, joka voi olla positiivinen vaikutus kyselyn suorituskykyä.

## <a name="scaling-for-availability"></a>Käytettävyyden skaalaus

Replikoiden paitsi pakettitiedoston kyselyviive, mutta voit myös sallia suuren.  Yksittäisen replikaan säännöllisiä käyttökatkot vuoksi palvelimen käynnistyy olisi odotat jälkeen ohjelmistopäivitykset tai muita ylläpito tapahtumia, jotka suoritetaan.  Tuloksena on tärkeä ottaa huomioon, jos sovellus edellyttää suuren käytettävyyden hakujen (kysely) sekä kirjoituksia (indeksoinnin tapahtumien).  Azure haku on kaikki maksettu Etsi seuraavien määritteiden kanssa tarjousten SLA asetukset:

- 2 replikoiden hyvin saatavuuden vain luku-työmääriä (kysely)
- 3 tai Lisää replikoida hyvin saatavuuden luku-ja kirjoitusoikeudet työmääriä (kyselyjen ja indeksoinnin)

Lisätietoja tästä Siirry [Azure haun taso-palvelusopimus](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Koska replikoiden tietojen kopioita, on useita replikoita avulla Azure Etsi koneen käynnistyy ja ylläpito vastaan replikaan kyselyjen Jatka voidaan kohdistaa muiden replikoiden samalla kerralla.  Tästä syystä myös tarvitset ottaa huomioon, miten tämä käyttökatkot voivat vaikuttaa kyselyitä, jotka on nyt voidaan kohdistaa yhden vähemmän tiedot kopion.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Skaalausta geo distributed toiminnoista ja tarjoaa geo luotettavuutta

Geo distributed toiminnoista löydät huomattavasti tietokeskuksen, jossa Azure-hakupalvelun nykyisessä käyttäjät on suurempi viive-kurssit.  Tästä syystä on usein tärkeää olla haun palveluihin tai alueet, jotka ovat lähempänä toisiaan nämä käyttäjät ovat.  Azure haku ei ole tällä hetkellä automaattisesta geo replikoiminen Azure hakuindeksin eri alueilla, mutta on joitakin menettelytapoja, joita voidaan käyttää, voit tehdä tämän prosessin yksinkertainen toteuttaminen ja hallinta. Nämä ovat Jäsennettyjen seuraava joitakin osia.

Etsi palveluita geo jaettu joukko tavoitteena on vähintään kaksi indeksit käytettävissä on vähintään kaksi alueilla, jossa käyttäjä reititetään Azure Search-palvelun, joka tarjoaa pienin viive, kuten se näkyy tässä esimerkissä:

   ![Alueen palvelujen rajat-välilehti][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Tietojen pitäminen synkronoituina kaikkiin palveluihin Azure haku

On kaksi vaihtoehtoa: pitäminen haussa palvelujen synkronoiminen jotka koostuvat joko käyttämällä [Azure haun indeksointitoiminto](search-indexer-overview.md) tai Push-Ohjelmointirajapinnan (tunnetaan myös nimellä [Azure haun REST API](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Azure haun Indeksoijilla

Jos käytössäsi on Azure haun indeksointitoiminto, tietojen muutokset ovat jo tuominen keskitetyn datastore, kuten SQL Azure-DB- tai DocumentDB. Kun luot uuden haun-palveluun, voit vain luotava uusi Azure haun indeksointitoiminto palvelun, joka osoittaa tämän saman datastore. Sen mukaan, aina, kun uusi muutokset tulevat tiedot säilöön ne sitten indeksoidaan eri Indeksoijilla mukaan.  

Tässä on esimerkki siitä, miltä, arkkitehtuuri näyttää.

   ![Yksi tietolähde, jossa jaettu indeksointitoiminto ja palvelun yhdistelmät][2]


### <a name="push-api"></a>Push-Ohjelmointirajapinta 
Jos käytössäsi on Azure haun Push-Ohjelmointirajapinnan päivittämään [Azure-hakuindeksin sisältöön](https://msdn.microsoft.com/library/dn798930.aspx), voit pitää eri haun palvelujen synkronoiminen mukaan valitseminen muutokset kaikkiin haku-palveluihin aina, kun päivitys on pakollinen.  Kun tämä on tärkeää varmistaa, että käsittelemään tilanteissa, joissa yksi hakupalvelun päivitys epäonnistuu ja vähintään yhden päivitykset onnistuu.

## <a name="leveraging-azure-traffic-manager"></a>Hyödyntäminen Azure liikenteen hallinta

[Azure liikenteen hallinta](../traffic-manager/traffic-manager-overview.md) mahdollistavat reitin pyynnöt useita geo sijaitsevat sivustoja, varmuuskopioidaan sitten useita Azure haku-palveluissa.  Se voi tutkia Azure haun varmistaa, että se on käytettävissä ja reitittää vaihtoehtoisen Etsi palveluita käyttökatkot Jos käyttäjien on etu liikenne-hallinta.  Jos ovat reititys search-pyyntöjen Azure sivustojen kautta, Azure liikenteen hallinta lisäksi avulla voit ladata saldo tapaukset, joissa sivusto on ylöspäin, mutta ei Azure haku.  Tässä on esimerkki mitä arkkitehtuurista, joka hyödyntää liikenteen hallinta.

   ![Rajat-välilehti alue-palvelujen kanssa keskitetyn liikenteen hallinta][3]

## <a name="monitoring-performance"></a>Suorituskyvyn valvominen

Azure haun tarjoaa mahdollisuuden analysoida ja valvoa suorituskykyä palvelun [Haun liikenne Analytics (STA)](search-traffic-analytics.md)kautta. STA, kautta voit kirjautua yksittäisiä search-toiminnot sekä koostetun arvot myös Azure-tallennustilan tilin, valitse voit käsitellä analyysia varten tai näyttää Power BI.  Käytä STA arvot, voit tarkastella Tuottavuustilastot, kuten kyselyt tai kyselyn vastauksen kertaa määrän keskiarvo.  Lisäksi toimintojen kirjaaminen avulla voit siirtyä tiedot tietyn haku-toimintoa.

STA on tärkeä työkalu ymmärtää viive korvaukset Azure Etsi kyseisen näkökulmasta.  Koska kysely-suorituskyvyn mittarit kirjautunut perustuvat kyselyn kestää täysin käsitellään Azure hakutoiminnossa (se on pyydetty, kun se lähetetään ajasta) aika, pystyt tämän toiminnon avulla voit selvittää, jos viive ongelmat ovat Azure haun palvelun reunasta tai ulkopuolella palvelu, kuten verkon latenssin.  

## <a name="next-steps"></a>Seuraavat vaiheet

Jokaiselle lisätietoja hinnoittelu tasoa ja palveluiden rajoituksista on artikkelissa [Azure hakutoiminnossa palvelun rajoitukset](search-limits-quotas-capacity.md).

Käy lisätietoja osio ja replikan yhdistelmät [kapasiteetin suunnittelua](search-capacity-planning.md) .

Lisää siirtyminen suorituskykyyn ja jotkin ottamisesta käyttöön tässä artikkelissa kuvatut optimointi esittelyt seuraavassa videossa:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png