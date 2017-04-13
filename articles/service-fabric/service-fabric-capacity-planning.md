<properties
   pageTitle="Palvelun kangasta sovellusten suunnitteleminen kapasiteetin | Microsoft Azure"
   description="Käsitellään pakollinen palvelun kangasta sovelluksen suorittaminen solmujen määrän selvittäminen"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Kapasiteetin palvelun kangasta sovellusten suunnitteleminen


Tämän asiakirjan avulla opit arvioida varat (suorittimessa, RAM-Muistia, levytilasta), sinun on suoritettava Azure palvelun kangasta-sovellukset. On yhteinen resurssin tarpeen voi muuttua ajan kuluessa. Tarvitset yleensä muutaman resurssin kehittää/testi palvelun ja edellyttävät sitten Lisää resursseja, sinun on siirryttävä tuotannon ja sovelluksen kasvaa suosion. Suunnitellessasi sovellusta Pohdi kautta pitkään vaatimukset ja asetusta, joka mahdollistaa palvelusi skaalattavissa hyvin asiakkaan tarvittaessa.

 Palvelun kangasta klusterin luodessasi voit päättää, mitä tiedostotyyppejä näennäiskoneiden (VMs) muodostavat klusterin. Kunkin AM sisältyy rajoitetun resurssien suorittimessa (Sydämiä ja nopeus), kaistanleveys, RAM-Muistia ja levytilasta. Kun palvelusi kasvaa ajan kuluessa, voit päivittää VMs, joka tarjoaa enemmän resursseja ja/tai Lisää VMs lisääminen yhteyttä klusterin. Suoritettava viimeksi sinun on arkkitehti palvelusi aluksi niin se voit hyödyntää uusia VMs, klusterin dynaamisesti lisätään.

Joidenkin palvelujen hallinta pieni, ei itse VMs tietoja. Vuoksi kapasiteetin palvelun suunnittelu kannattaa keskittyä ensisijaisesti suorituskyvyn, mikä tarkoittaa, VMs tarvittavat suorittimessa (Sydämiä ja nopeuden) valitsemalla. Lisäksi kannattaa harkita kaistanleveys, mukaan lukien, kuinka usein verkon siirrot ovat määrä ja kuinka paljon tietoja siirretään. Jos palvelun on suoritettava sekä palvelun käyttö kasvaa, voit lisätä Lisää VMs verkon pyytää yli kaikki VMs klusterin ja lataa saldo.

Joilla hallitaan suuria tietomääriä VMs-palveluissa kapasiteetin suunnittelu kannattaa keskittyä ensisijaisesti kokoa. Näin Harkitse huolellisesti kapasiteetista AM RAM-Muistia ja levytilasta. Näennäismuistin hallintajärjestelmän Windowsissa on levytilaa näyttää RAM-Muistia sovelluksen koodiin. Lisäksi palvelun kangasta runtime on Automaattinen sivutus vain niiden tietojen pitäminen muistiin ja kylmän tietojen siirtämistä levylle. Näin sovellukset voivat käyttää enemmän muistia kuin on fyysisesti käytettävissä AM. Ottaa RAM-Muistia yksinkertaisesti parantaa suorituskykyä, koska AM pitää levyn lisätallennustilan RAM-muistia. Voit valita AM on riittävän suuri AM tiedot, jotka haluat tallentaa levyn. Vastaavasti AM on riittävästi RAM-Muistia tarjota Microsoftiin suorituskykyä. Jos yhteyttä palvelun tiedot kasvaa ajan kuluessa, voit lisätä Lisää VMs klusterin ja osion tiedot kaikki VMs yli.

## <a name="determine-how-many-nodes-you-need"></a>Määrittää, kuinka monta solmujen tarvitset

Jakaminen-palvelussa voit skaalata, että palvelun tiedot. Saat lisätietoja jakaminen [Jakaminen palvelun kangasta](service-fabric-concepts-partitioning.md). Kunkin osion on sovitetaan yhdelle AM, mutta useiden (pieni) osioiden voidaan sijoittaa mihin yksittäisen AM. Ottaa Lisää pieni osioita joustavuutta suurempi kuin on muutama suurempi osiot. Trade-off on, että palvelun kangasta katseltavan on paljon osioiden kasvaa ja tapahtuma toimintoja ei voida suorittaa osioiden välillä. On myös useita mahdollinen verkkoliikenteen Jos service-koodi on usein käyttää tietoa, joka tallennetaan eri osiot. Palvelun suunnitellessasi Ota huomioon huolellisesti nämä ammattilaisille ja haittoja tehokkaita osioinnin strategia vuoroon.

Oletetaan, että sovelluksesi on tilallisten palvelu, jolla on säilön koko, joka odotat kasvattamista DB_Size gigatavua vuodessa. Olet valmis lisäämään useita sovelluksia (ja osioiden), kun kohtaat kasvu vuoden lisäksi.  Replikoinnin kerroin (RF), joka määrittää määrä on palvelun vaikuttaa yhteensä DB_Size. Yhteensä DB_Size yli replikoita on DB_Size kerrottuna replikoinnin kertoimen.  Node_Size edustaa levyn tilaa/RAM solmu, jota haluat käyttää palvelun kohden. Parhaan mahdollisen suorituskyvyn DB_Size olisi mahdu muistiin yli klusterin ja Node_Size, joka on AM RAM-Muistia ympärillä on valittava. Varaamalla Node_Size, joka on suurempi kuin RAM-Muistia kapasiteetti ovat käyttäisit palvelun kangasta runtime myöntämä sivutus. Näin ollen suorituskyvyn ei ehkä ole optimaalista, jos koko tietojen pidetään kuuma (sen jälkeen tiedot on sivutettu/ulos). Useiden palveluiden missä vain murto-tietojen kuuma, on kuitenkin edullisempaa.

Parhaan mahdollisen suorituskyvyn vaaditaan solmujen määrän käytettävät luvut seuraavasti:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Kasvu-tili

Haluat ehkä perusteella, jonka oletat kasvaa, lisäksi DB_Size, kun aloitit kanssa palvelussa DB_Size solmujen määrän laskemiseen. Näkyvien solmujen määrän kasvavan sitten, kun palvelun kasvaa niin, että näkyvien solmujen määrän ovat ei ylimitoitettava. Mutta osioiden määrän perusteella, joita tarvitaan, kun palveluun osoitteessa suurin kasvu solmujen määrän.

Se on hyvä olla joissakin ylimääräisiä tietokoneissa käytettävissä milloin tahansa, jotta voit käsitellä minkä tahansa odottamattomia piikkarit tai epäonnistumiseen (Jos esimerkiksi muutama VMs siirtyy alaspäin).  Vaikka ylimääräinen kapasiteetti on määritettävä odotettu piikkarit käyttämällä lähtökohtana voi varata muutamia ylimääräisiä VMs (5-10 prosenttia ylimääräisiä).

Yksittäisen tilallisten palvelun edeltävän olettaa. Jos sinulla on useampi kuin yksi tilallisten palvelu, sinun on lisättävä DB_Size liittyvät muihin palveluihin kaava. Vaihtoehtoisesti voit laskea erikseen kumpaankin palveluun on oma tilallisten solmujen määrän.  Palvelussa voi olla replikoiden tai osioita, jotka eivät ole saapuva. Ota huomioon, että osioita voi olla myös muita enemmän tietoja. Saat lisätietoja jakaminen [jakaminen tietovisualisoinnin parhaista käytännöistä](service-fabric-concepts-partitioning.md). Kuitenkin edellisen kaava on osio ja replikan agnostic, koska palvelun kangasta varmistetaan, että replikat ovat levittäminen kesken solmut optimoitu tavalla.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Käytä laskentataulukko lasketaan

Nyt sijoittaa oletetaan, että jotkin reaalilukujen kaava. Ole [Esimerkki laskentataulukon](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) esitetään, kuinka voit suunnitella sovellus, joka sisältää kolmenlaisia tieto-objektit kapasiteetti. Kullekin objektille on lähentää sen kokoa ja kuinka monta objekteja, jotka on oltava Odotamme. On myös valita haluamme kunkin objektityyppi, kuinka monta replikoita. Laskentataulukon laskee muistin klusterin tallennetaan.

Olemme kirjoittamalla AM koko ja kuukausihinta. AM koon perusteella laskentataulukon ilmoittaa osioita, sinun on käytettävä Jaa sovittamiseksi fyysisesti solmujen tietojen vähimmäismäärä. On useita osioita, jotta sovelluksesi tietyn laskenta saattaa Microsoftiin ja verkkoliikenteen on. Laskentataulukko näkyy osioita, jotka hallitaan käyttäjän profiiliin objektien määrä on kasvanut 1 – 6.

Kaikkien näiden tietojen perusteella, laskentataulukko näkyy nyt, että voitu fyysisesti saat kaikki haluamasi osiot ja replikoiden tietoja 26 solmu klusterissa. Kuitenkin tämän klusterin tiheään pakata, jotta voit halutessasi joitakin muita solmujen solmu virheet ja päivitykset. Laskentataulukossa näkyvät myös on enemmän kuin 57 solmujen avulla, että arvoa ei ole muita sillä on tyhjä solmujen. Haluat ehkä uudelleen, siirry yllä 57 solmujen silti solmu virheet ja päivitykset. Laskentataulukon vastaamaan sovelluksen erityistarpeita voi säätää tehosteasetuksilla.   

![Laskentataulukon lasketaan][Image1]



## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu [palvelun kangasta jakaminen services] [ 10] lisätietoja jakaminen palvelussa.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
