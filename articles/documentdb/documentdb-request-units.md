<properties 
    pageTitle="Pyydä DocumentDB yksiköiden | Microsoft Azure" 
    description="Lue lisätietoja ymmärtää, Määritä ja arvioida DocumentDB pyynnön yksikön vaatimuksia." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Pyydä DocumentDB yksiköt
Nyt käytettävissä: DocumentDB [pyynnön yksikön Laskimen](https://www.documentdb.com/capacityplanner). Lisätietoja [oman siirtonopeuden tarvitsee Estimating](documentdb-request-units.md#estimating-throughput-needs).

![Siirtonopeuden laskuri][5]

##<a name="introduction"></a>Johdanto
Tässä artikkelissa on yleiskatsaus [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)yksiköiden pyynnön. 

Luettuasi tämän artikkelin pystyt seuraaviin kysymyksiin:  

-   Mitä ovat Pyydä yksiköt ja pyydä ovat?
-   Miten kokoelma pyynnön yksikön kapasiteetin määrittäminen?
-   Miten oma sovelluksen pyynnön yksikön tarvitsee arvioida?
-   Mitä tapahtuu, jos voin ylittää pyynnön yksikön kapasiteetin kokoelman?


##<a name="request-units-and-request-charges"></a>Pyydä yksiköt ja pyydä kulut
DocumentDB toimittaa resurssien *varaaminen* tyydyttämiseksi sovelluksen siirtonopeuden on nopea ja ennakoitavissa suorituskykyä.  Sovelluksen lataaminen ja käyttää kuviot muutos ajan mittaan, koska DocumentDB avulla voit suurentaa tai pienentää varattu siirtonopeuden käytettävissä sovelluksen helposti.

DocumentDB, jossa on varattu siirtonopeuden on määritetty myyntialueella pyyntö käsitellään sekunnissa.  Voit ajatella pyynnön yksiköt siirtonopeuden valuuttana, jolla voit *varata* määrän ja taata pyynnön yksiköt sovelluksen sekunnissa perusteella.  Kunkin DocumentDB - kirjoitettaessa asiakirjaa, suoritat kyselyn, asiakirjan päivittäminen - toiminnon käyttämän suorittimen ja muistin IOPS.  Eli kunkin toiminnon veloitetaan *pyynnön maksu*, joka ilmaistaan *pyynnön*mittayksikkö.  Tietoja tekijät, jotka vaikuttavat pyynnön yksikön kuluja sekä sovelluksen siirtonopeuden vaatimukset avulla voit suorittaa sovelluksen kustannukset mahdollisimman tehokkaasti. 

##<a name="specifying-request-unit-capacity"></a>Pyyntö yksikön kapasiteetin määrittäminen
Kun luot DocumentDB sivustokokoelman, määrittää sekunnissa (RUs) haluat varattu eikä kokoelman pyynnön yksiköiden määrän.  Kun kokoelma on luotu, RUs määritetty koko kohdistus on varattu eikä kokoelman käytettäväksi.  Valikoimien on varmasti on varattu ja eristetty siirtonopeuden ominaisuudet.  

On tärkeää muistaa, että DocumentDB toimii varaus-mallin. Toisin sanoen ovat laskuttaa koskien kuinka siirtonopeuden *varattu* sivustokokoelman riippumatta siitä, *kuinka paljon, että siirtonopeuden käytetään aktiivisesti*.  Ota kuitenkin huomioon, joka sovelluksen lataaminen, tiedot ja käyttö kuvioiden muuttaminen voit helposti skaalata ylä-ja määrä varattu RUs DocumentDB SDK: T tai [Azure-portaalissa](https://portal.azure.com).  Katso lisätietoja voin asteikko siirtonopeuden ylös ja alas, [DocumentDB suorituskyvyn tasot](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Pyyntö yksikön huomioon otettavia seikkoja
Kun varata DocumentDB kokoelman pyynnön yksiköiden määrän, on tärkeä ottaa huomioon seuraavat muuttujat:

- **Asiakirjan koko**. Kun asiakirjan koot suurentaa kulutettu lukeminen tai tiedostoon tiedot myös kasvattaa yksiköt.
- **Asiakirjan ominaisuus määrä**. Kaikki ominaisuudet, kirjoittaa asiakirjan kulutettu yksiköt indeksoinnin olettaen oletusarvon kasvattaa kuin ominaisuuden määrä kasvaa.
- **Tietojen yhdenmukaisuuden**. Kun käytät tietojen kelpoisuus tasot vahvan tai joka Staleness, yksikköä Lisää kulutettu voit lukea tiedostoja.
- **Indeksoidut ominaisuudet**. Indeksi-käytäntö kunkin sivustokokoelman määrittää, mitkä ominaisuudet ovat indeksoituja oletusarvoisesti. Voit pienentää pyynnön yksikkö-kulutus rajoittamalla näytettävien indeksoitujen ominaisuuksien tai ottamalla viive-indeksointi.
- **Asiakirjan indeksoinnin**. Kunkin asiakirjan indeksoidaan automaattisesti oletusarvon mukaan voit käyttää vähemmän pyynnön yksiköt Jos et halua indeksoida jotkin tiedostojen.
- **Kyselyn kuviot**. Kyselyn monimutkaisuutta vaikuttaa, kuinka monta pyytää yksikköä on käytetty toiminnon. Predikaatit määrä, laatu predikaatit, ennusteiden, UDF määrä ja lähde-tietojoukko suuruus vaikuttaa kyselyn toiminnot kustannukset.
- **Komentosarjan käyttö**.  Kyselyt, joiden tallennettujen toimintosarjojen ja käynnistimien tarjoaman pyynnön yksiköt parhaillaan suoritettavia toimintoja monimutkaisuuden perusteella. Kun kehität sovelluksen, Tarkasta pyynnön maksu otsikko ja ymmärtää helpommin, miten kutakin toimintoa muissa pyynnön yksikön kapasiteetti.

##<a name="estimating-throughput-needs"></a>Siirtonopeuden tarpeiden arvioiminen
Pyyntö yksikkö mittaa normitettu pyynnön käsittelyn kustannukset. Yksittäisen pyynnön yksikkö edustaa lukemiseen (joko itse linkin tai tunnus) 1 kt JSON tiedoston yhteen, jossa on 10 (lukuun ottamatta järjestelmän ominaisuudet) yksilöllinen ominaisuusarvoihin käsittely kapasiteetti. Luoda (Lisää), korvaa tai poistaa asiakirjan pyyntö käyttää Lisää käsittely-palvelusta ja siten enemmän pyynnön yksiköitä.   

> [AZURE.NOTE] Perusaikataulun 1 pyynnön yksikön 1 kt tiedoston vastaa yksinkertainen GET itse linkki tai tiedosto tunnus.

###<a name="use-the-request-unit-calculator"></a>Pyyntö yksikön Laskimen
Jotta asiakkaiden hieno paranna niiden siirtonopeuden arviot, on web-pohjaisten [pyynnön yksikön Laskimen](https://www.documentdb.com/capacityplanner) avulla arvioida pyynnön yksikön edellytyksistä tavalliset toiminnot, kuten:

- Asiakirjan Luo (kirjoituksia)
- Asiakirjan lukee
- Asiakirjan poistaa
- Asiakirjapäivitykset

Työkalu myös tuki arviointi tietojen tallennustilan tarpeiden perusteella antaisit otoksen asiakirjat.

-Työkalulla on helppoa:

1. Yhden tai useamman edustavan JSON-tiedostojen lataaminen.

    ![Tiedostojen lisääminen pyynnön yksikön laskuri][2]

2. Arvioida tietojen tallennuksen vaatimukset, Kirjoita tallennettavan tiedostojen määrä.

3. Kirjoita asiakirjan määrä luominen, lukea, päivittää ja poistaa toiminnot vaativat (perusteella toisen kohden). Arvioi pyyntö yksikön maksut asiakirjan päivitys-toimintoa, vaiheen 1, joka sisältää tyypillinen kentän päivitykset otoksen asiakirjan kopion lataaminen  Esimerkiksi jos asiakirjan Muokkaa yleensä lastLogin ja userVisits kaksi ominaisuuksia, valitse yksinkertaisesti kopioida mallitiedostoa, Päivitä nämä kaksi ominaisuuksien arvot ja lataa kopioitu asiakirja.

    ![Anna pyynnön yksikön Laskimen siirtonopeuden vaatimukset][3]

4. Valitse Laske ja Tarkastele tuloksia.

    ![Pyydä yksikön Laskimen tulokset][4]

>[AZURE.NOTE]Jos sinulla on tiedostotyypit, jotka vaihtelevat huomattavasti koosta ja indeksoitujen ominaisuuksien määrä, lataa kunkin *tyyppi* , Tyypillinen esimerkki työkalu ja sitten Laske tulokset.

###<a name="use-the-documentdb-request-charge-response-header"></a>Käytä DocumentDB pyynnön maksu vastauksen otsikon
Jokainen vastaus DocumentDB-palvelusta on mukautetun otsikon (x-ms-pyynnön-maksu), joka sisältää pyynnön yksiköt kulutettu pyynnön. Tämä ylätunniste on myös DocumentDB SDK: T kautta. .NET-SDK RequestCharge on ResourceResponse objektin ominaisuus.  Kyselyjen DocumentDB kyselyn Explorer Azure-portaalissa on suoritettu kyselyjen maksu tietojen.

![Tutkittaessa RU kulujen kyselyn Resurssienhallinnassa][1]

Tässä yhteydessä huomioon, yksi keino arviointi varattu siirtonopeuden sovelluksesi vaatii määrä on tallentavan käynnissä vastaan edustavan asiakirja, jota käytetään sovelluksen mukaan tavalliset toiminnot liittyvät pyynnön yksikkökulun ja sitten arviointi toimintoja, jotka on tarkoitus kunkin toisen suorittamiseen.  Muista mittaa ja ovat tyypillisiä-kyselyiden ja sekä DocumentDB komentosarjan käyttö.

>[AZURE.NOTE]Jos sinulla on tiedostotyypit, jotka vaihtelevat huomattavasti koosta ja indeksoitujen ominaisuuksien määrä, tietue kunkin *tyyppi* , tyypillinen liittyvät sovellettavan toiminnon pyynnön yksikön maksu.

Esimerkki:

1. Tallenna pyyntö yksikkökulun luominen (lisääminen) tyypillinen asiakirjan. 
2. Tietueen lukemisen tyypillinen asiakirjan pyynnön yksikkö-maksu.
3. Tietueen pyynnön yksikkökulun tyypillinen tiedoston päivittämistä.
3. Tyypillinen, Yleiset asiakirjan kyselyjen pyynnön yksikkö-maksu-tietue.
4. Pyyntö yksikkökulun mukautetun komentosarjojen (tallennettujen toimintosarjojen, käynnistimien, käyttäjän määrittämien funktioiden) hyödyntää sovelluksen tallentaminen
5. Laske tarvittavat pyynnön yksiköt annettu toimintojen suorittamiseen kunkin toisen on tarkoitus arvioitu määrä.

##<a name="a-request-unit-estimation-example"></a>Pyyntö yksikön arvio-Esimerkki
Ota huomioon seuraavat noin 1 Knowledge Base-tiedosto:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Tiedostot ovat minified DocumentDB, siten, että järjestelmä yllä tiedoston koko on hieman pienempi kuin 1 kt.


Seuraavassa taulukossa on esitetty lähes pyynnön yksikön ovat tyypillisiä toimille tiedostoa koskevan (lähes pyynnön yksikkökulun oletetaan, että tilin yhdenmukaisuuden taso on määritetty "Istunnon" ja että kaikki asiakirjat indeksoidaan automaattisesti):

Toiminto|Pyydä yksikön maksu 
---|---
Asiakirjan luominen|NOIN 15 RU 
Luku-tiedosto|NOIN 1 RU
Kyselyn asiakirjan tunnuksen mukaan|~2.5 RU

Lisäksi tässä taulukossa lähes pyynnön yksikön ovat tyypillisiä kyselyjen käytössä sovelluksessa:

Kyselyn|Pyydä yksikön maksu|# Palautetut tiedostot.
---|---|--- 
Valitse ruoka tunnuksen mukaan|~2.5 RU|1 
Valitse ruoka valmistajan|NOIN 7 RU|7
Valitse ruoka-ryhmä ja tilauksen leveyden mukaan|NOIN 70 RU|100
Valitse yläreunan 10 ruoat ruoka-ryhmässä|NOIN 10 RU|10

>[AZURE.NOTE]RU kulujen määräytyvät palauttamien tiedostojen määrä.

Nämä ohjeet on arviointiin tämän sovelluksen annettu toiminnot ja kyselyjen Odotamme sekunnissa RU vaatimukset:

Toiminnon/kysely|Arvioitu määrä sekunnissa|Pakollinen RUs 
---|---|--- 
Asiakirjan luominen|10|150 
Luku-tiedosto|100|100 
Valitse ruoka valmistajan|25|175 
Valitse ruoka-ryhmä|10|700 
Valitse yläreunan 10|15|150 yhteensä|155|1275

Tässä tapauksessa Odotamme keskimääräinen nopeus-vaatimus 1,275 RU/s.  Pyöristäminen ylöspäin lähimpään 100 on valmistella 1,300 RU/s tämän sovelluksen keräämistä varten.

##<a id="RequestRateTooLarge"></a>Suurempi kuin varattu siirtonopeuden rajoitukset
Et voi peruuttaa, että pyyntö Yksikkökulutus arvioidaan sekunnissa korvaus. Sovellusten, jotka ylittävät valmistellun pyynnön Yksikköhinta kokoelman kyseisen sivustokokoelman pyynnöt rajoitettu, kunnes nopeus laskee varattu tason alapuolelle. Rajoituksen toteutuessa palvelimen synkronointiraja lopettaa pyyntö, jonka RequestRateTooLargeException (HTTP-tilakoodin 429) ja palauttaa x-ms-yritä-jälkeen-ms-otsikon, joka kertoo ajan millisekunteina, käyttäjän on odotettava ennen uudelleenyritysvälin pyynnön.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Jos käytössäsi on .NET asiakkaan SDK ja LINQ-kyselyt ja sitten yleensä sinun tarvitse koskaan tätä poikkeusta käsitellä tavalla .NET asiakkaan SDK nykyisen version saa tämän vastauksen implisiittisesti kiinni, kunnioitetaan palvelimessa määritetyn uudelleen jälkeen otsikon ja yrittää pyynnön suorittaa uudelleen. Ellei tilisi käyttää samanaikaisesti useita asiakkaita, seuraava uudelleen onnistuu.

Jos sinulla on useampi kuin yksi asiakas kumulatiivisesti liiketoiminnan pyynnön, korko yllä uudelleen oletustoiminnan ei ehkä riittää ja asiakkaan palauttaa DocumentClientException tilakoodi 429 sovellukseen. Esimerkiksi tapauksissa kannattaa harkita, yritä toiminta ja sovelluksen virheen käsittely rutiinit tai lisääntyvien varattu siirtonopeuden kokoelman funktioiden käsitteleminen.

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja varattu Azure DocumentDB tietokantojen nopeus, tutki nämä resurssit:
 
- [DocumentDB hinnat](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB kapasiteetin hallinta](documentdb-manage.md) 
- [DocumentDB tietojen mallinnus](documentdb-modeling-data.md)
- [DocumentDB suorituskyvyn tasot](documentdb-partition-data.md)

Saat lisätietoja DocumentDB on Azure DocumentDB [ohjeissa](https://azure.microsoft.com/documentation/services/documentdb/). 

Aloita asteikko ja suorituskyvyn testaaminen oikeilla DocumentDB, katso [suorituskyvyn ja mittakaava testaaminen Azure DocumentDB kanssa](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
