<properties 
    pageTitle="SQL-syntaksi ja SQL kyselyn DocumentDB | Microsoft Azure" 
    description="Lisätietoja SQL-syntaksi, tietokannan käsitteitä ja SQL-kyselyjä DocumentDB, NoSQL tietokannan. SQL voi käyttää DocumentDB JSON kyselykielen." 
    keywords="SQL-syntaksi, sql-kyselyn, sql-kyselyjä, json-kyselykieltä, tietokannan käsitteitä ja sql-kyselyjä-koostefunktiot"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-kysely ja DocumentDB SQL-syntaksi
Microsoft Azure DocumentDB tukee kyseltäessä asiakirjojen JSON-kyselykieltä käyttäen SQL (Structured Query Language). DocumentDB on todella rakenteen vapaa. JSON-tietomallin suoraan tietokantamoduuli sitoutunut, jotka se sisältää JSON tiedostojen automaattisen indeksoinnin eksplisiittinen rakenteen tai toissijaisen indeksien luominen tarvitsematta. 

Suunnitellessasi kyselykielen DocumentDB varten seikat mielessä kaksi tavoitteet:

-   Sen sijaan, että inventing JSON kyselyn uusi kieli on etsimäsi tukevat SQL. SQL on eniten tuttu ja Suositut kysely-kielellä. DocumentDB SQL sisältää muodollinen ohjelmoinnin mallin monipuolisia kyselyjen JSON tiedostojen päälle.
-   JSON asiakirjan tietokantana voi suorittaa JavaScript suoraan tietokantamoduuli emme valitsemien JavaScript-ohjelmoinnin mallia perustana Microsoftin query Language. DocumentDB SQL sijaitsee JavaScript tyyppi järjestelmän, lausekkeen arvioinnista ja funktion kutsu. Tässä kohdassa – Ota sisältää luonnollinen ohjelmoinnin mallin relaatio ennusteiden, hierarkkisia siirtyminen JSON asiakirjoja, itse liitokset, paikkatietojen kyselyt ja painaa käyttäjän määrittämät funktiot (UDF) kirjoitettu kokonaan JavaScript-kesken muita ominaisuuksia. 

Microsoft uskoo, että nämä ominaisuudet ovat pienentämisestä hankaukselle sovelluksen ja tietokannan välillä-näppäintä ja tärkeitä tuottavuutta.

On suositeltavaa aloittaminen katselemalla seuraavassa videossa, jossa Aravind Ramachandran näkyy DocumentDB's kyseltäessä ominaisuuksia, ja ohjesisältöä Microsoftin [Kyselyn leikkikenttä](http://www.documentdb.com/sql/demo), jossa voit kokeilla DocumentDB ja suorittaa SQL-kyselyjä Microsoftin tietojoukko.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Palaa sitten tämän artikkelin kohtaa, johon asetetaan ensin SQL-kyselyn opetusohjelma, joka opastaa sinua joitakin yksinkertaisia JSON-asiakirjoja ja SQL-komentoja.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>SQL-komentoja DocumentDB käytön aloittaminen
Nähdäksesi DocumentDB SQL töissä japanin alkavat joitakin yksinkertaisia JSON-asiakirjoja ja käydä läpi joitakin yksinkertaisia kyselyitä, jotka perustuvat sitä. Ota huomioon nämä kaksi perheille kaksi JSON asiakirjoja. Huomautus DocumentDB, jossa ei ole annettava minkä tahansa rakenteita tai toissijaisen indeksien luominen erikseen. Tarvitsemme yksinkertaisesti JSON asiakirjojen DocumentDB sivustokokoelman ja sen jälkeen kyselyn. Seuraavassa on yksinkertainen JSON asiakirjan Andersen perhe, vanhemmat, lasten (ja niiden lemmikkieläimet), osoite ja rekisteröinnin tiedot. Asiakirjassa on merkkijonoja, lukuja, totuusarvoja, matriisien ja sisäkkäiset ominaisuudet. 

**Asiakirjan**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Tässä on yksi muotoiltu ero – toisen tiedoston `givenName` ja `familyName` käytetään sen sijaan, että `firstName` ja `lastName`.

**Asiakirjan**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Nyt yritetään muutaman kyselyitä, jotka perustuvat tiedoista selvittääksesi, jotkin DocumentDB SQL keskeisiä ominaisuuksia. Esimerkiksi seuraava kysely palauttaa asiakirjat, jossa tunnus-kenttä vastaa `AndersenFamily`. Koska `SELECT *`, kyselyn tulos on valmis JSON asiakirja:

**Kyselyn**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Nyt tilanne missä annettava alustaa toiseen muotoon JSON-tuloste. Tämä kysely projektit uuden JSON-objekti, jossa on kaksi valitut kentät, nimi ja Kaupunki, kun sen osoite kaupunki on sama nimi kuin tila. Tässä tapauksessa "NY, NY" vastaa.

**Kyselyn**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Tulokset**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Seuraava kysely palauttaa kaikki määritetyn lasten nimet tuoteperheeseen, jonka tunnus vastaa `WakefieldFamily` vyöhykkeen asuinpaikan kaupungin perusteella.

**Kyselyn**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Tulokset**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Haluamme korostaa muutamia huomionarvoiset ominaisuuksia DocumentDB kyselykielen olemme katsomista mennessä esimerkeistä:  
 
-   DocumentDB SQL toimii JSON-arvojen, se käsitellään puun muotoinen kohteiden rivien ja sarakkeiden sijaan. Tämän vuoksi kielen voit viitata solmujen puun milloin tahansa haluamaansa syvyyttä, kuten `Node1.Node2.Node3…..Nodem`vastaavien relaatio SQL viittaavia kahden osan viittaus `<table>.<column>`.   
-   Structured query language toimii rakenteen pienempi tiedot. Tämän vuoksi Tyyppijärjestelmä on sidottava dynaamisesti. Saman lauseke aiheuttaa erilaista eri asiakirjoja. Kyselyn tulos on kelvollinen JSON-arvo, mutta ei välttämättä on kiinteä rakenteen.  
-   DocumentDB tukee vain tarkkojen JSON-tiedostoja. Tämä tarkoittaa järjestelmän tyyppi ja lausekkeiden on rajoitettu käsitellä JSON tyyppejä. Katso lisätietoja [JSON-määritys](http://www.json.org/) .  
-   DocumentDB sivustokokoelman on JSON asiakirjojen rakenteen vapaa säilö. Tietoja yksiköt ja niiden asiakirjojen kokoelman suhteiden implisiittisesti oteta talteen, sisältymisiä ja perusavain ja viiteavain avaimen suhteet mukaan. Tämä on tärkeää kuvasuhde, nykyarvo, joka osoittaa ottaen huomioon jäljempänä tässä artikkelissa käsiteltävät sisäisessä asiakirjan liitokset.

## <a name="documentdb-indexing"></a>DocumentDB indeksointi

Ennen kuin olemme tuoda DocumentDB SQL-syntaksi, kannattaa tutustuminen DocumentDB indeksoinnin rakenne. 

Tietokannan indeksit tarkoituksena on tukemaan kyselyjen eri lomakkeiden ja muotojen pienin resurssin kulutus (kuten suorittimen ja i /) ja käyttämisessä on hyvä siirtonopeuden ja pieni viive. Oikea indeksin tietokantakyselyn valinta tarvitaan usein paljon suunnittelu ja vuorovaikutteisuudesta. Tämän menetelmän aiheuttaa hankala rakenteen pienempi tietokantoja, jossa tiedot ei vastaa tarkka rakennetta ja kehitystyö nopeasti. 

Kun on suunniteltu indeksoinnin alirakenne DocumentDB, emme vuoksi määrittää seuraavat tavoitteet:

-   Indeksi tiedostoja ilman edellyttävät rakennetta: indeksoinnin alirakenne ei edellyttävät rakennetta tietoja tai tehdä määritysoletuksia rakenteen tietoja asiakirjoista. 

-   Tehokas, rich hierarkkisia ja relaatio kyselyt tuki: hakemiston tukee DocumentDB-kyselykieltä tehokkaasti, mukaan lukien hierarkkisia ja relaatio ennusteiden tuki.

-   Yhtenäinen kyselyjen in face of kirjoituksia kestävä tilavuus tuki: hyvin kirjoittaminen siirtonopeuden yhdenmukaisia kyselyjen toiminnoista, indeksi on päivittää soluissa, tehokkaasti ja verkossa menettämisen tapahtuessa kirjoituksia kestävä määrä. Yhtenäinen indeksin päivitys on tärkeää yhteyshenkilönä kyselyt, jossa käyttäjän määritetty asiakirjan palvelun yhdenmukaisuuden tasolla.

-   Usean vuokraajan tuki: annettu varaus mukaan mallin resurssin hallinnon yli alihallinnat, indeksi-päivitykset suoritetaan järjestelmäresurssit (suorittimen, muistin ja i/toimintoja sekunnissa) mukaan kohdistettujen replikan budjetissa. 

-   Tallennustilan tehokkuuden: kustannusten tehokkuutta levyn tallennustilan katseltavan indeksi on sidotut ja ennakoitavissa. Tämä on tärkeää, koska DocumentDB avulla kehittäjä voi tehdä perusteella kompromissien yleisrasite suhteessa kyselyn suorituskykyä indeksin välillä.  

Viittaavat [DocumentDB näytteiden](https://github.com/Azure/azure-documentdb-net) MSDN-sivuston esittää, kuinka kokoelman indeksoinnin käytännön esimerkkejä. Katsotaan nyt tuoda tietoja DocumentDB SQL-syntaksi.


## <a name="basics-of-a-documentdb-sql-query"></a>DocumentDB SQL-kyselyn perusteet
Jokainen kyselyn koostuu SELECT-lauseessa ja valinnainen FROM ja WHERE-lauseet ANSI SQL-standardit kohden. Yleensä kunkin kyselyn luetteloidaan FROM-lauseen lähde. Valitse WHERE-lauseen suodatin on otettu käyttöön lähteen hakemiseen alijoukkoa JSON asiakirjoja. SELECT-lauseessa käytetään lopuksi Project pyydetty JSON arvot, valitse-luettelosta.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-lause
`FROM <from_specification>` -Lause on valinnainen, paitsi jos lähde on suodatettu tai suunnitellut myöhemmin kyselyssä. Tämä lause on määrittää, joihin kysely on toimittava tietolähteen. Yleisesti koko kokoelman on lähde, mutta jokin Määritä alijoukkoa kokoelman sen sijaan. 

Kyselyn, kuten `SELECT * FROM Families` ilmaisee, että koko perheille sivustokokoelman tulosjoukko, jolle luettelointi lähde. Erityistä tunnus pääkansio voidaan edustavan sivustokokoelman sijaan kokoelman nimi. Seuraava luettelo sisältää säännöt, jotka ovat voimassa kysely:

- Kokoelma voi olla viittaama, kuten `SELECT f.id FROM Families AS f` tai yksinkertaisesti `SELECT f.id FROM Families f`. Tässä `f` vastaa `Families`. `AS`on valinnainen avainsana, aliaksen tunnus.

-   Huomaa, että kerran viittaama, alkuperäinen lähde ei voi olla sidottu. Esimerkiksi `SELECT Families.id FROM Families f` on kielisääntöjen, koska tunnus "Perheille" ei voi enää ratkaista.

-   Kaikki ominaisuudet, joita on oltava viittaus on oltava täydellinen. Tarkka rakenteen ehdokasvaltio puuttuessa tämä pakotetaan minkä tahansa moniselitteinen sidontojen välttämiseksi. Tämän vuoksi `SELECT id FROM Families f` on kielisääntöjen, koska se `id` ei ole sidottu.
    
### <a name="sub-documents"></a>Sisäinen asiakirjat
Lähteen voi myös pienentää pienempi alijoukkoa. Esimerkiksi luettelointi vain aliraportti puun jokaiseen asiakirjaan, voit aliraportti pääkansio voi sitten muuttuvat lähde-seuraavan esimerkin mukaisesti.

**Kyselyn**

    SELECT * 
    FROM Families.children

**Tulokset**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Kun edellä olevassa esimerkissä käytetään matriisin lähteenä, objektin voi myös tietolähteeksi, joka on seuraavassa esimerkissä-kohdassa. Mikä tahansa kelvollinen JSON arvo (ei ole määritetty), jotka löytyvät lähteen katsotaan merkitsemiseen kyselyn tuloksessa. Jos jotkin perheille ei ole `address.state` arvo, ne jätetään pois kyselyn tuloksessa.

**Kyselyn**

    SELECT * 
    FROM Families.address.state

**Tulokset**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-lause
WHERE-lauseen (**`WHERE <filter_condition>`**) on valinnainen. Määrittää ehdot, jotka JSON-tiedostot ovat lähde on täytettävä ollakseni tuloksen mukaan. Mikä tahansa JSON-tiedosto on annettava tulokseksi "true" pidetään tuloksen määritetyt ehdot. WHERE-lauseen käytetään indeksi-kerroksen lähde-tiedostot, jotka voivat kuulua tuloksen suora pienimmän alijoukon määrittämiseksi. 

Seuraava kysely pyytää asiakirjoja, jotka sisältävät name-ominaisuutta, jonka arvo on `AndersenFamily`. Muu asiakirja, jossa ei ole name-ominaisuutta, tai jos arvo vastaa `AndersenFamily` ei sisälly. 

**Kyselyn**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Edellisen esimerkin ilmeni tasa yksinkertaisen kyselyn. DocumentDB SQL tukee myös skalaarinen lausekkeita lausekkeet. Yleisimmin käytettyjä ovat binaarinen ja Yksiarvoinen lausekkeita. Ominaisuuden viittauksia JSON Lähdeobjektin ovat myös kelvollinen lausekkeita. 

Binaarinen seuraavia operaattoreita tällä hetkellä tueta ja voidaan käyttää kyselyissä, kuten seuraavissa esimerkeissä:  
<table>
<tr>
<td>Aritmeettinen</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bittitaso</td>    
<td>|, &, ^, <<, >>, >>> (nolla täytön oikea vaihto) </td>
</tr>
<tr>
<td>Loogisen</td>
<td>JA- TAI EI</td>
</tr>
<tr>
<td>Vertailu</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Merkkijono</td> 
<td>|| (KETJUTA)</td>
</tr>
</table>  

Katsotaan joitakin kyselyitä binaarinen operaattorien osoitteessa.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Etumerkki-operaattoreita +,-, ~ ei tueta myös ja voi käyttää kyselyiden sisällä, kuten seuraavassa esimerkissä:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Lisäksi binaarinen ja Yksiarvoinen operaattoreita ominaisuuden viittaukset sallitaan myös. Esimerkiksi `SELECT * FROM Families f WHERE f.isRegistered` palauttaa JSON-asiakirja, joka sisältää ominaisuuden `isRegistered` missä ominaisuuden arvo on sama kuin JSON `true` arvo. Muut arvot (EPÄTOSI, null tai määrittämätön, `<number>`, `<string>`, `<object>`, `<array>`, jne) liidit lukematta tuloksesta lähdetiedostossa. 

### <a name="equality-and-comparison-operators"></a>Operaattorien tasa ja vertailu
Seuraavassa taulukossa tasa vertailuja saatu DocumentDB SQL minkä tahansa kahden JSON eri välillä.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>A</strong>
         </td>
         <td valign="top">
            <strong>Ei määritetty</strong>
         </td>
         <td valign="top">
            <strong>Null</strong>
         </td>
         <td valign="top">
            <strong>Totuusarvo</strong>
         </td>
         <td valign="top">
            <strong>Numero</strong>
         </td>
         <td valign="top">
            <strong>Merkkijono</strong>
         </td>
         <td valign="top">
            <strong>Objektin</strong>
         </td>
         <td valign="top">
            <strong>Matriisi</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Ei määritetty<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Null<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Totuusarvo<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Numero<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Merkkijono<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objektin<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
         <td valign="top">
Ei määritetty </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matriisi<strong>
         </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
Ei määritetty </td>
         <td valign="top">
            <strong>Okei</strong>
         </td>
      </tr>
   </tbody>
</table>

Muiden vertailu operaattoreita, kuten >, > =,! =, < ja < =, seuraavien sääntöjen käyttäminen:   

-   Välillä vertailun tulokset määrittämätön.
-   Kaksi objektia tai kaksi vertailu taulukoita Määrittämätön tulokset.   

Jos suodattimen skalaarinen lausekkeen tulos on määrittämätön, vastaavan asiakirjan ei sisällytetä tulos, koska Määrittämätön ei loogisesti laskea "true".

### <a name="between-keyword"></a>Avainsanojen välillä
Voit myös express kyselyitä, jotka perustuvat arvoja, ANSI SQL BETWEEN-avainsana. Merkkijonojen tai numeroita voidaan käyttää välillä.

Esimerkiksi tämä kysely palauttaa kaikki perhe tiedostot, joiden ensimmäisen lapsen luokka on välillä (sekä mukaan lukien) 1 – 5. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Toisin kuin ANSI-SQL: ssä voit käyttää myös BETWEEN-lauseen FROM-lauseen seuraavassa esimerkissä.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Jotta kysely-suorittamisen nopeammin muista indeksoinnin käytännön, jossa on käytössä alueen indeksi vastaan mikä tahansa numeerinen ominaisuudet/polut, jotka on suodatettu BETWEEN-lauseessa. 

Tärkein käyttämällä BETWEEN DocumentDB ja ANSI SQL välinen ero on, että voit ilmaista alueen kyselyitä, jotka perustuvat eri tyyppisten ominaisuudet – esimerkiksi voi olla "luokka" luku (5) kaikki asiakirjat ja merkkijonot muiden ("grade4"). Tällaisissa tapauksissa kuten JavaScript-kohdassa vertailun kahden eri tuloksen "Määrittämätön"- ja asiakirjan ohitetaan.

### <a name="logical-and-or-and-not-operators"></a>Looginen (ja, tai eikä) operaattorit
Loogisten operaattorien toimivat totuusarvoja. Loogisen totuusversiona taulukoiden operaattoreista näkyvät seuraavassa taulukossa.

TAI|TOSI|EPÄTOSI|Ei määritetty
---|---|---|---
TOSI|TOSI|TOSI|TOSI
EPÄTOSI|TOSI|EPÄTOSI|Ei määritetty
Ei määritetty|TOSI|Ei määritetty|Ei määritetty

JA|TOSI|EPÄTOSI|Ei määritetty
---|---|---|---
TOSI|TOSI|EPÄTOSI|Ei määritetty
EPÄTOSI|EPÄTOSI|EPÄTOSI|EPÄTOSI
Ei määritetty|Ei määritetty|EPÄTOSI|Ei määritetty

EI|  |
---|---
TOSI|EPÄTOSI
EPÄTOSI|TOSI
Ei määritetty|Ei määritetty

### <a name="in-keyword"></a>Kirjoita avainsana
IN-avainsanan jälkeen voidaan tarkistaa, onko määritetty arvo vastaa mitä tahansa luettelon. Esimerkiksi tämä kysely palauttaa kaikki perheen asiakirjoja missä tunnus on "WakefieldFamily" tai "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Tässä esimerkissä palauttaa kaikki tiedostot, joissa tila on määritetty arvoja.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Kolmiarvoinen (?) ja yhdistetyn (?)-operaattorit
Kolmiarvoinen ja yhdistetyn operaattoreita voidaan luoda ehtolausekkeiden, esimerkiksi C#- ja JavaScript Suositut ohjelmoinnin kielten samalla. 

Kolmiarvoinen (?) operaattori voi olla erittäin hyödyllinen luodessaan uusia JSON ominaisuuksia suoraan selaimessa. Esimerkiksi nyt voit kirjoittaa kyselyjä luokitella luokan tasot kyselyjä ihmisten luettavassa muodossa, kuten alla kuvatulla tavalla helppo ja Keskitaso/Lisäasetukset.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Voit sisällyttää operaattori, alla kyselyn puhelut.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Kuin muut kyselyn operaattoreilla tehtäviä, jos minkä tahansa tiedoston ehtolauseke viitatun ominaisuudet puuttuvat tai verrattavat ovat eri sitten tiedostot jätetään pois kyselyn tuloksissa.

Yhdistetyn (?)-operaattoria voidaan käyttää tehokkaasti tarkistaa, ovatko ominaisuuden (tietovälinettä on määritetty) asiakirjassa. Tästä on hyötyä, kun kysely suoritetaan vastaan puolirakenteinen tai usean tyyppisiä tietoja. Tämä kysely esimerkiksi palauttaa "Sukunimi" Jos tai "Sukunimi", jos sitä ei ole käytettävissä.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Tarjottu ominaisuuden seuraaja
Voit myös käyttää ominaisuudet tarjottu ominaisuus-operaattorin käyttäminen `[]`. Esimerkiksi `SELECT c.grade` ja `SELECT c["grade"]` tulkitaan samaksi merkiksi. Seuraavalla syntaksilla on hyödyllinen, kun haluat päästä ominaisuus, joka sisältää välilyöntejä, erikoismerkkejä, tai jakaa on sama nimi kuin SQL-avainsana tai varattu sana tapahtuu.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-lause
SELECT-lauseen (**`SELECT <select_list>`**) on pakollinen ja määrittää arvot ovat noutaa kyselyn, aivan kuten ANSI SQL: ssä. Osajoukko, joka on suodatettu päälle asiakirjoja välitetään sivulle ennuste vaiheeseen, jossa määritetty JSON-arvot haetaan ja uuden JSON objektin muodostetaan, kunkin syötteen välitetty se sivulle. 

Seuraavassa esimerkissä esitetään tyypillinen valintakysely. 

**Kyselyn**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Sisäkkäiset ominaisuudet
Seuraavassa esimerkissä on projisoiminen kaksi sisäkkäisiä ominaisuudet `f.address.state` ja `f.address.city`.

**Kyselyn**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Ennuste tukee myös JSON lausekkeita, kuten seuraavassa esimerkissä.

**Kyselyn**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Katsotaan rooli `$1` tähän. `SELECT` -Lauseessa on JSON-objektin luominen ja sillä ei ole avainta ei anneta, Käytämme implisiittinen argumentin muuttujien nimissä alkaen `$1`. Esimerkiksi tämä kysely palauttaa kahden implisiittinen argumentin muuttujan, jossa `$1` ja `$2`.

**Kyselyn**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Tunnuksen määritys
Nyt laajentaa japanin yllä olevassa esimerkissä kanssa eksplisiittinen tunnuksen määritys arvoista. SE on käytettävä tunnuksen määritys avainsana. Huomaa, että se on valinnainen, ja toisen arvon kuin projisoiminen esitetyllä `NameInfo`. 

Siltä varalta, että kysely on saman niminen kaksi ominaisuudet-tunnuksen määritys on käytettävä nimeäminen uudelleen niin, että ne ovat disambiguated arvioidut tuloksessa jompikumpi tai molemmat seuraavista ominaisuudet.

**Kyselyn**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalaaripäivämäärän lausekkeet
Lisäksi ominaisuuden viittaukset SELECT-lauseen tukee myös skalaarinen lausekkeita, kuten vakiot, aritmeettisia lausekkeita, loogisista lausekkeista. Seuraavassa on esimerkiksi "Hei-maailman" yksinkertaisen kyselyn.

**Kyselyn**

    SELECT "Hello World"

**Tulokset**

    [{
      "$1": "Hello World"
    }]


Seuraavassa on monimutkaisempi esimerkki, jossa skalaarinen lausekkeessa.

**Kyselyn**

    SELECT ((2 + 11 % 7)-2)/3   

**Tulokset**

    [{
      "$1": 1.33333
    }]


Seuraavassa esimerkissä skalaarinen lausekkeen tulos on totuusarvo.

**Kyselyn**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Tulokset**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Objektin ja taulukon luominen
Toisen avaimen ominaisuus DocumentDB SQL on matriisi tai objektin luominen. Edellisessä esimerkissä Huomaa, että luomaasi uutta JSON objektia. Vastaavasti jokin voit myös käyttää matriisin seuraavien esimerkkien mukaisesti.

**Kyselyn**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Tulokset**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>ARVO-avainsana
**Arvo** -avainsanan avulla on helppo JSON palautusarvo. Esimerkiksi alla kysely palauttaa skalaari `"Hello World"` sijaan `{$1: "Hello World"}`.

**Kyselyn**

    SELECT VALUE "Hello World"

**Tulokset**

    [
      "Hello World"
    ]


Seuraava kysely palauttaa JSON-arvon ilman `"address"` tulokset otsikko.

**Kyselyn**

    SELECT VALUE f.address
    FROM Families f 

**Tulokset**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Seuraavassa esimerkissä laajentaa tätä, jos haluat näyttää, miten voit palauttaa JSON alkeistyyppi arvot (JSON puun lehtitaso). 

**Kyselyn**

    SELECT VALUE f.address.state
    FROM Families f 

**Tulokset**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operaattori
Erityistä operaattori (*) tuetaan asiakirjan Project-on. Käytetään, kun se on oltava vain suunnitellut kenttä. Samalla tavoin kyselyn `SELECT * FROM Families f` on voimassa, `SELECT VALUE * FROM Families f ` ja `SELECT *, f.id FROM Families f ` eivät ole kelvollisia.

**Kyselyn**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Tulokset**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>Ylimmät operaattori
Ylimmät avainsana voidaan rajoittaa kyselyn arvojen määrän. Kun ylä käytetään yhdessä ORDER BY-lauseen, tulosjoukko on rajoitettu järjestetyn arvojen; ensimmäisen N määrä Muussa tapauksessa se palauttaa ensimmäisen N näytettävien tulosten määrää Määrittämätön järjestyksessä. Paras käytäntö on SELECT-lauseessa, käytä aina ORDER BY-lauseen ja TOP-lause. Tämä on ainoa tapa osoittamaan laadukkaampi rivit vaikuttavat ALKUUN. 


**Kyselyn**

    SELECT TOP 1 * 
    FROM Families f 

**Tulokset**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

ENSIMMÄISET voidaan vakion arvo (kuten edellä) tai Parametroitu kyselyjen käyttäminen muuttuja-arvoa. Lisätietoja on artikkelissa Parametroitu kyselyjen alla.

## <a name="order-by-clause"></a>ORDER BY-lause
Kuten ANSI-SQL-valinnainen Order By-lause voi sisältää tehtäessä. Lause voi sisältää valinnainen Nouseva tai LASKEVA-argumentti, voit määrittää järjestyksen, jossa tulokset on noudettu. Lisätietoja tarkastella Order By-viitata [Tilauksen DocumentDB ongelmatilanteita mukaan](documentdb-orderby.md).

Seuraavassa on esimerkiksi kysely, joka hakee perheille asuvat kaupungin nimen järjestyksessä.

**Kyselyn**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Tulokset**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

Ja tässä kysely, joka hakee perheille luontipäivän, joka on tallennettu nimellä kausi edustava luku järjestyksessä aikaa, ts-1. tammi 1970 kulunut aika sekuntia.

**Kyselyn**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Tulokset**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Tietokannan käsitteitä ja SQL-kyselyjä
### <a name="iteration"></a>Iteraation
Uusi rakenne on lisätty kautta **IN** -avainsanan jälkeen DocumentDB SQL tuen tarjoamiseen Iterointi JSON matriisin päälle. FROM-tietolähteen tuki iteraation. Aloitetaan seuraavassa esimerkissä:

**Kyselyn**

    SELECT * 
    FROM Families.children

**Tulokset**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Nyt katsotaan toisen kyselyn, joka suorittaa iteraation lasten sivustokokoelman päälle. Huomautus tulostus-matriisissa erotus. Tässä esimerkissä jakaa `children` ja yksittäisen matriisin yhdeksi objektiksi tulokset.  

**Kyselyn**

    SELECT * 
    FROM c IN Families.children

**Tulokset**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Tämän avulla voidaan lisäksi voit suodattaa yksittäisiä tekstien matriisin seuraavan esimerkin mukaisesti.

**Kyselyn**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Tulokset**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Yhdistää
Suhteellisen tietokannan taulukoiden välillä ei tarvitse on erittäin tärkeää. On looginen vaalikelpoisuus suunnitteluun normitettu rakenteet. Estä tämän DocumentDB käsitellään rakenteen vapauttaminen tiedostojen denormalisoidun tietomalliin. Tämä on looginen vastikkeen a "itse liittyä".

Syntaksi, joka tukee kieli on < from_source1 > liity < from_source2 > liity... Liity < from_sourceN >. Yleistä Tämä palauttaa joukon **N**- monikot (monikon **N** arvoilla). Kunkin monikon on valmistettu merkkijonoavaimen kaikki sivustokokoelman aliakset niiden vastaaviin joukot päälle arvoja. Toisin sanoen tämä on täydellinen ristitulon liitosta osallistuminen siirtämistä.

Seuraavissa esimerkeissä JOIN-lause toiminta. Seuraavassa esimerkissä tulos on tyhjä, koska ristitulon kunkin asiakirjan lähteestä ja tyhjän joukon on tyhjä.

**Kyselyn**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Tulokset**  

    [{
    }]


Seuraavassa esimerkissä liitos on välillä pääkansio ja `children` aliraportti neliöjuuren. On ristitulon kaksi JSON objektista toiseen. Lasten on taulukko ei ole voimassa liitoksen jälkeen on käsittely yksittäisen pääkansio, joka on lasten-matriisi. Näin ollen tulos sisältää vain kaksi tuloksia, koska matriisin kunkin asiakirjan ristitulon tuottaa täsmälleen vain yhden tiedoston.

**Kyselyn**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Tulokset**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Seuraavassa esimerkissä esitetään, Lisää tavalliseen liitoksen:

**Kyselyn**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Tulokset**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Huomaa, että Huomaa on `from_source` , **Liity** -lause on käydä läpi. Niin työnkulku tässä tapauksessa on seuraavanlainen:  

-   Laajenna matriisin kunkin alielementti **c-näppäinyhdistelmää** .
-   Käytä ristitulon kunkin alielementti **c** , joka on käytössä ensimmäiseksi tiedoston **f** ylimmällä kanssa.
-   Project-pääkansio objektin **f** name-ominaisuus yksinään. 

Ensimmäinen asiakirjan (`AndersenFamily`) on vain yksi alielementti, joten tulosjoukko sisältää vain tässä asiakirjassa vastaavan yhtenä objektina. Toisessa asiakirjassa (`WakefieldFamily`) sisältää kaksi lasten. Siis ristitulon tulos tuottaa kunkin ali-ja tuloksena on kaksi objektia, yhteen kunkin lapsen tässä asiakirjassa vastaavan erillinen objekti. Huomaa, että molemmissa asiakirjoissa pääkansion kentät saman, aivan kuin tuttuun tapaan ristitulon tulos.

LIITOKSEN reaali apuohjelma on lomakkeen monikot-muodossa, jossa ei ole vaikeaa projektin ristitulon. Lisäksi on huomaamaan alla olevasta esimerkistä, voi suodattaa, antaa käyttäjien käyttäjän valitsit täyty mukaan monikot yleinen ehdon monikon yhdistelmän.

**Kyselyn**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Tulokset**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



Tässä esimerkissä on luonnollinen laajennus edellisessä esimerkissä, ja tekee kaksinkertainen liity. Niin ristitulon, voi tarkastella Pseudo-Interface seuraava koodi.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`on yksi lapsi, joilla on yksi pet. Niin, ristitulon tuottaa yhden rivin (1*1*1)-tästä. WakefieldFamily on kuitenkin kaksi lasten, mutta vain yksi lapsen "Jesse" on lemmikkieläimet. Jesse on 2 lemmikkieläimet vaikka. Näin ollen ristitulon tulos tuottaa 1*1*, 2 = 2 riviä tästä.

Seuraavassa esimerkissä on muita suodatin `pet`. Jättää huomiotta kaikki monikot, jossa pet nimi ei ole "Varjostus". Huomaa, että olemme on luoda monikot matriiseja monikon, elementtejä suodatin-ja project yhdistelmän elementit. 

**Kyselyn**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Tulokset**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integrointi
DocumentDB tarjoaa suoritetaan JavaScript-pohjaisen sovelluksen logiikkaa suoraan tallennettujen toimintosarjojen ja käynnistimien sivustokokoelmat-ohjelmoinnin mallin. Näin sekä:

-   Mahdollisuus erinomainen suorituskyky tapahtumien CRUD-toimintojen ja kyselyitä, jotka perustuvat asiakirjat kokoelman JavaScript runtime suoraan tietokantamoduuli laaja integrointi perusteella. 
-   Luonnollisen mallinnus Hallintavuo, muuttujan rajaus- ja varauksen ja integrointi poikkeuksen käsittely perusalkioiden tietokannan tapahtumien kanssa. Lisätietoja DocumentDB tuki JavaScript-integroinnin tutustumaan JavaScript Serverin puoli ohjelmoitavuus ohjeissa.

###<a name="user-defined-functions-udfs"></a>Käyttäjän määrittämät funktiot (UDF)
Sekä jo määrittänyt tämän artikkelin DocumentDB SQL: ssä support for käyttäjän määrittämät funktiot (UDF). Skalaaripäivämäärän UDF erityisesti tuetaan, jossa kehittäjät voit siirtää yhtään ja monta argumenteissa ja palauttaa yhden argumentin aiheuttaa takaisin. Kaikkien näiden argumenttien tarkistetaan parhaillaan oikeudellinen JSON-arvoja.  

DocumentDB SQL-syntaksi on laajennettu tukemaan mukautetun sovelluksen logiikkaa käyttämällä näitä käyttäjän määrittämät funktiot. UDF voi rekisteröidä DocumentDB ja sitten viitata osana SQL-kysely. Itse asiassa UDF on suunniteltu exquisitely, jota kyselyt. Tämä vaihtoehto, työssäkäyvien UDF ei ole yhteydessä objekti, joka JavaScript muuntyyppisten (tallennettujen toimintosarjojen ja käynnistimien) on pääsy. Kyselyjen suorittaminen vain luku-muodossa, koska ne voi suorittaa ensisijaisen tai toissijaisen replikoita. Tämän vuoksi UDF on suunniteltu toimimaan toissijainen replikoiden toisin kuin JavaScript.

Alla on esimerkki siitä, miten UDF rekisteröity DocumentDB tietokantaa, erityisesti asiakirjan sivustokokoelman-kohdassa.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Edellisen esimerkin Luo UDF, jonka nimi on `REGEX_MATCH`. Kaksi JSON merkkijonoarvoa hyväksyy `input` ja `pattern` ja tarkistaa, onko ensimmäisen vastineita mallia, joka on määritetty toisessa käyttämällä JavaScript string.match()-toimintoa.


On nyt käyttää tätä UDF ennuste kyselyn. UDF täytyy olla tarkennettu kirjainkoko on merkitsevä etuliite "udf." Kun kutsua kyselyjen kuluessa. 

>[AZURE.NOTE] Ennen 3/17/2015 DocumentDB tuettu UDF puhelut ilman "udf." etuliitteen, kuten Valitse REGEX_MATCH(). Tämä puheluja kaava on vähennetty.  

**Kyselyn**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Tulokset**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDF myös avulla voidaan sisällä suodattimen, kuten alla olevasta esimerkistä myös qualified kanssa "udf." etuliite:

**Kyselyn**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Tulokset**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Oikeastaan UDF ovat kelvollisia skalaarinen lausekkeita ja voi käyttää ennusteiden ja suodattimet. 

Laajenna UDF virtaa, katsotaan toinen esimerkki ehdollinen logiikka kanssa:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Alla on esimerkki, joka käyttää UDF.

**Kyselyn**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Tulokset**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Kun edellä olevaa esimerkkiä korostaa UDF integroi JavaScript-kieli power DocumentDB SQL antamaan monipuolisia ohjelmoitava käyttöliittymä, voit tehdä monimutkaisia kirjaamisesta, ehdollinen logiikka inbuilt JavaScript runtime ominaisuuksien avulla.

DocumentDB SQL tarjoaa argumenttien UDF kunkin asiakirjan lähteessä käsittelyn UDF nykyisen vaiheessa (WHERE-lauseen tai SELECT-lauseessa). Tulos on rekisteröity yleinen suorittamisen putkijohto saumattomasti. Jos mainittujen ominaisuuksien mukaan UDF parametrit eivät ole käytettävissä JSON-arvo, parametrin pidetään Määrittämätön ja näin ollen UDF kutsu kokonaan ohitetaan. Lisää vastaavasti UDF tulos ei ole määritetty, jos se ei sisälly tulos. 

Yhteenveto-UDF on suoritettava monimutkainen liiketoimintalogiikan kyselyn osana hienoa työkalua.

### <a name="operator-evaluation"></a>Operaattori arviointi
DocumentDB, mukaan, JSON, tietokanta on alan piirtää JavaScript-operaattorit ja sen arviointi semantiikkaan liittyvien vastineita. Kun DocumentDB yrittää säilyttää JavaScript semantiikkaan liittyvien kannalta JSON tuki-toiminnon laskennan mikä taas joissakin tapauksissa.

DocumentDB SQL toisin kuin perinteisen SQL-tyyppisten arvojen ovat usein tiedetä kunnes arvot haetaan todella tietokannan. Jotta voit suorittaa kyselyjä tehokkaasti, operaattorit useimmilla on tarkka vaatimukset. 

DocumentDB SQL ei tehdä implisiittinen muunnokset, toisin kuin JavaScript. Esimerkiksi kyselyn, kuten `SELECT * FROM Person p WHERE p.Age = 21` vastaa asiakirjoja, jotka sisältävät ikä-ominaisuuden, jonka arvo on 21. Muu asiakirja, jonka ikä vastaa merkkijonon "21" tai muu valmistua jatkuva variaatiot, kuten "021", "21.0", "0021", "00021", jne ei löydy. Tämä on sen sijaan JavaScript-koodia, joissa merkkijonoarvot ovat luvuiksi implisiittisesti muuntaa (ex operaattorin perusteella: ==). Tämä vaihtoehto on tärkeää tehokkaan indeksin vastaavat DocumentDB SQL. 

## <a name="parameterized-sql-queries"></a>Parametroitu SQL-kyselyjä
DocumentDB tukee kyselyjen parametreilla esittää tuttuja @ merkintätapaa. Parametroitu SQL on tehokkaat käsittely- ja vuotamaan käyttäjän syötteenä, estää vahingossa SQL lisäämisen tietojen näyttäminen. 

Voit esimerkiksi kirjoittaa kysely, joka kestää Sukunimi ja osoite tila parametreiksi ja suorittaa sen eri arvojen Sukunimi ja osoite käyttäjän syötteiden perusteella.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Pyyntö sitten voidaan lähettää DocumentDB Parametroitu JSON-kyselynä, alla.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Argumentin ylös voidaan määrittää Parametroitu käyttäminen, kuten alla.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parametriarvot voi olla mikä tahansa kelvollinen JSON (merkkijonoja, lukuja, totuusarvoja, null, vaikka matriiseja tai sisällä JSON). Myös DocumentDB on rakenteen pienempi, parametreja ei tarkisteta vastaan tarkistamistekniikan.

##<a name="built-in-functions"></a>Sisäiset funktiot
DocumentDB tukee myös sisäiset funktiot useita yleisiä toimintoja, joita voidaan käyttää sisällä kyselyt, kuten käyttäjän määrittämät funktiot (UDF).

<table>
<tr>
<td>Matemaattiset funktiot</td> 
<td>ABS, PYÖRISTÄ.KERR.ylös, eksponentti, FLOOR, LOG, LOG10, POWER, PYÖRISTÄ-FUNKTIOTA, merkki, neliöjuuri, NELIÖN, Katkaise, ACOS, ASIN, ATAN, ATN2, COS-COT, astetta, PI, RADIAANEINA, SIN ja TAN</td>
</tr>
<tr>
<td>Kirjoita tarkistuksen Funktiot</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED ja IS_PRIMITIVE</td>
</tr>
<tr>
<td>Merkkijonofunktiot</td>   
<td>KETJUTETTU, sisältää ENDSWITH, INDEX_OF, vasemmalle, pituus, pienet, LTRIM, korvaa, TOISINTO, VASTAKKAINEN, oikealle, RTRIM, STARTSWITH, ALIMERKKIJONO tai YLÄKULMASSA</td>
</tr>
<tr>
<td>Matriisi-Funktiot</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH ja ARRAY_SLICE</td>
</tr>
<tr>
<td>Paikkatietojen Funktiot</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID ja ST_ISVALIDDETAILED</td>
</tr>
</table>  

Jos käytät parhaillaan käyttäjän määrittämiä funktioita (UDF), jolle sisäinen funktio on nyt saatavilla, sinun on käytettävä vastaavan valmiin funktion, kun se käyttäjästä tulee suoritetaan nopeammin ja tehokkaammin. 

### <a name="mathematical-functions"></a>Matemaattiset funktiot
Matemaattiset funktiot kunkin suorittaa laskutoimituksen, yleensä syöttöarvojen, argumentteina annetaan ja numeerisen arvon perusteella. Seuraavassa on tuettu sisäiset matemaattiset funktiot sisällysluettelon.

<table>
<tr>
<td><strong>Käyttö</strong></td>
<td><strong>Kuvaus</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (num_expr)</a></td> 
<td>Palauttaa määritetyn numeerisen lausekkeen (positiivinen) absoluuttisen arvon.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">PYÖRISTÄ.KERR.ylös (num_expr)</a></td> 
<td>Palauttaa pienimmän arvon kokonaisluku suurempi tai yhtä suuri määritetty numeerinen lauseke.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">PYÖRISTÄ.KERR.alas (num_expr)</a></td> 
<td>Palauttaa suurimman kokonaisluvun enintään määritetty numeerinen lauseke.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (num_expr)</a></td> 
<td>Palauttaa määritetyn numeerisen lausekkeen eksponentti.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (num_expr [, base])</a></td> 
<td>Palauttaa luvun luonnollisen logaritmin määritetyn numeerisen lausekkeen tai luvun logaritmin käyttämällä annettua kantalukua.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">Log10 (num_expr)</a></td> 
<td>Palauttaa määritetyn numeerisen lausekkeen kantaluku 10 Logaritminen arvon.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">Pyöristä (num_expr)</a></td> 
<td>Palauttaa numeerisen arvon, pyöristettynä lähimpään kokonaislukuarvo.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">Katkaise (num_expr)</a></td> 
<td>Palauttaa numeerisen arvon, lähinnä kokonaisluku katkaistaan.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">Neliöjuuri (num_expr)</a></td>   
<td>Palauttaa määritetyn numeerisen lausekkeen neliöjuuren.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">NELIÖ (num_expr)</a></td>   
<td>Palauttaa määritetyn numeerisen lausekkeen neliön.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POTENSSI (num_expr; num_expr)</a></td>   
<td>Palauttaa määrittämäsi arvon määritetyn numeerisen lausekkeen potenssiin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">ETUMERKKI (num_expr)</a></td>   
<td>Palauttaa määritetyn numeerisen lausekkeen-merkki-arvon (1, 0, 1).</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (num_expr)</a></td>   
<td>Palauttaa kulman, radiaaneina, jonka kosini on määritetty numeerinen lauseke. kutsutaan myös arkuskosinin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ASIN (num_expr)</a></td>   
<td>Palauttaa kulman, radiaaneina, jonka sini on määritetty numeerinen lauseke. Tätä kutsutaan myös arkussinin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (num_expr)</a></td>   
<td>Palauttaa kulman, radiaaneina, jonka tangentti on määritetty numeerinen lauseke. Tätä kutsutaan myös arkustangentin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (num_expr)</a></td>   
<td>Palauttaa kulman, radiaaneina välillä positiivinen x- ja säteet kohteen alkuperäisestä pisteeseen (y, x), jossa x ja y ovat arvoja, on määritetty liukuluku kahdesta lausekkeesta.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (num_expr)</a></td> 
<td>Palauttaa radiaaneina määritetyn lausekkeessa trigonometriset määritetyn kulman kosinin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (num_expr)</a></td> 
<td>Palauttaa radiaaneina määritetyn numeerisen lausekkeen trigonometriset määritetyn kulman kotangentin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">ASTEET (num_expr)</a></td> 
<td>Palauttaa vastaavan kulman radiaaneina määritetyn kulman astetta.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PII)</a></td>   
<td>Palauttaa piin vakion arvo.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RADIAANIT (num_expr)</a></td> 
<td>Palauttaa radiaaneina, jos numeerinen lauseke-asteina.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (num_expr)</a></td> 
<td>Palauttaa radiaaneina määritetyn lausekkeessa trigonometriset määritetyn kulman sinin.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (num_expr)</a></td> 
<td>Palauttaa luvun tangentin syötteen lausekkeen määritettyyn lausekkeeseen.</td>
</tr>

</table> 

Voit esimerkiksi suorittaa kyselyjä, kuten jompikumpi seuraavista:

**Kyselyn**

    SELECT VALUE ABS(-4)

**Tulokset**

    [4]

Tärkein verrattuna ANSI SQL DocumentDB's-funktioiden välinen ero on, että ne on suunniteltu hyvin käyttäminen rakenteen pienempi ja sekaviittausten rakenne. Esimerkiksi jos sinulla on asiakirja, jossa koko-ominaisuuden puuttuu tai on muu kuin numeerinen arvo, kuten "Tuntematon" ja valitse asiakirja ohitetaan päälle, sen sijaan, että palauttaa virheen.

### <a name="type-checking-functions"></a>Kirjoita tarkistuksen Funktiot
Tarkistuksen tyyppi-funktioiden avulla voit tarkistaa lausekkeen SQL-kyselyjä sisällä. Tyyppi tarkistuksen Funktiot avulla voidaan määrittää, millaista ominaisuuksia asiakirjoja suoraan selaimessa, kun se on muuttujan tai tuntematon. Seuraavassa on tuettu sisäistä tarkistuksen Funktiot sisällysluettelon.

<table>
<tr>
  <td><strong>Käyttö</strong></td>
  <td><strong>Kuvaus</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, jos arvo on matriisin.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, jos arvo on totuusarvo.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, jos arvo on null.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, jos arvo on luku.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, onko arvo tyypin JSON-objekti.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, onko arvo tyypin merkkijonon.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, jos se on määritetty arvo.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (lauseke)</a></td>
  <td>Palauttaa totuusarvon, joka ilmaisee, onko arvo tyypin merkkijono, luku, totuusarvo tai null.</td>
</tr>

</table>

Näiden funktioiden käyttämisestä voi nyt suorittaa kyselyjä, kuten jompikumpi seuraavista:

**Kyselyn**

    SELECT VALUE IS_NUMBER(-4)

**Tulokset**

    [true]

### <a name="string-functions"></a>Merkkijonofunktiot
Seuraavat skalaarifunktiot suorittaa toiminnon merkkijonon Syöttöarvon ja palauttaa merkkijonon, numeerisia tai totuusarvoja, arvo. Valmiin merkkijonofunktiot sisällysluettelon näin:

Käyttö|Kuvaus
---|---
[PITUUS (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Palauttaa määritetyn merkkijonon merkkien määrän.
[KETJUTETTU (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Palauttaa merkkijonon, joka on saatu ketjuttaa kahden tai useamman merkkijonoarvoa.
[ALIMERKKIJONO (str_expr, num_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Palauttaa osan merkkijonolauseke.
[STARTSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Palauttaa totuusarvon, joka ilmaisee onko ensimmäisen merkkijonolauseke toinen päättyy
[ENDSWITH (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Palauttaa totuusarvon, joka ilmaisee onko ensimmäisen merkkijonolauseke toinen päättyy
[SISÄLTÄÄ (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Palauttaa totuusarvon, joka ilmaisee onko ensimmäisen merkkijonolauseke, joka sisältää toisen.
[INDEX_OF (str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Palauttaa ensimmäisen esiintymän toisen alkamiskohdan merkkijonolauseke ensimmäistä määritettyä merkkijonolauseke tai -1, jos merkkijonoa ei löydy.
[LEFT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Palauttaa määritetyn määrän merkkejä merkkijonon, joka vasemmanpuoleinen osa.
[RIGHT (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Palauttaa määritetyn määrän merkkejä merkkijonon, joka oikeanpuoleinen osa.
[LTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Palauttaa merkkijonolauseke, kun se poistaa alussa olevat tyhjät.
[RTRIM (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Palauttaa merkkijonolauseke jälkeen kaikki lopussa olevat tyhjät katkaistaan.
[PIENET (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Palauttaa pieniksi kirjaimiksi merkin tietojen muuntamisen jälkeen merkkijonolauseke.
[ISOT (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Palauttaa merkkijonolauseke, kun olet muuntanut kirjaimiksi merkit isoiksi kirjaimiksi.
[Korvaa (str_expr, str_expr, str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Korvaa kaikki määritetyn merkkijonoarvon esiintymät toisen merkkijonon arvon.
[TOISINTO (str_expr, num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Toistaa merkkijonoarvo määritetyn määrän kertoja.
[KÄÄNNÄ (str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Palauttaa merkkijonoarvon käänteisessä järjestyksessä.

Näiden funktioiden käyttämisestä voidaan nyt suorittaa kyselyjä seuraavalla tavalla. Jos esimerkiksi löydät sen myöhemmin nimi isoin kirjaimin seuraavasti:

**Kyselyn**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Tulokset**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Tai KETJUTA merkkijonoja, kuten tässä esimerkissä:

**Kyselyn**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Tulokset**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Merkkijonofunktiot voidaan myös WHERE-lauseen avulla voidaan suodattaa tulokset, kuten seuraavassa esimerkissä:

**Kyselyn**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Tulokset**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Matriisi-Funktiot
Seuraavat skalaarifunktiot suorittaa toiminnon matriisin Syöttöarvon ja palaa numeerinen, totuusarvo tai matriisista arvon. Näin sisällysluettelon valmiin matriisi funktioita:

Käyttö|Kuvaus
---|---
[ARRAY_LENGTH (arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Palauttaa määritetyn taulukon lausekkeen elementtien määrä.
[ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Palauttaa taulukon, joka on saatu ketjuttaa kahden tai useamman taulukon arvot.
[ARRAY_CONTAINS (arr_expr, lauseke)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Palauttaa totuusarvon, joka ilmaisee, onko taulukko sisältää määritetyn arvon.
[ARRAY_SLICE (arr_expr num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Palauttaa matriisin lausekkeen osa.

Matriisi funktioita voidaan käsitellä matriisin JSON kuluessa. Seuraavassa on esimerkiksi kysely, joka palauttaa kaikki tiedostot, joissa yksi vanhemmat on "Mikko Wakefield". 

**Kyselyn**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Tulokset**

    [{
      "id": "WakefieldFamily"
    }]

Seuraavassa on toisen tässä esimerkissä käytetään ARRAY_LENGTH saat kohti perheen alikohteiden määrä.

**Kyselyn**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Tulokset**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Paikkatietojen Funktiot

DocumentDB tukee seuraavia Avaa Geospatiaalisia Consortium (OGC) sisäiset funktiot geospatiaalisia kyselyt. Lisätietoja geospatiaalisia tuki DocumentDB, on artikkelissa [Azure DocumentDB geospatiaalisia tietojen käsitteleminen](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Käyttö</strong></td>
  <td><strong>Kuvaus</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Palauttaa kahden GeoJSON kohdassa-lausekkeet etäisyys.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Palauttaa totuusarvolauseke, joka ilmaisee, onko GeoJSON-kohdan ensimmäisessä argumentissa määritettyä sisällä GeoJSON monikulmio toisena argumenttina.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Palauttaa totuusarvon, joka ilmaisee, onko määritetty GeoJSON piste- tai monikulmion lauseke kelvollinen.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Palauttaa JSON-arvo, joka sisältää totuusarvon arvo, jos määritetty GeoJSON piste- tai monikulmion lauseke on voimassa ja jos se on virheellinen, lisäksi merkkijonoarvona syy.</td>
</tr>
</table>

Paikkatietojen funktioiden avulla voidaan suorittaa lähialue querries vastaan paikkatietojen tiedot. Seuraavassa on esimerkiksi kysely, joka palauttaa kaikki perheen tiedostot, jotka ovat valmiita ST_DISTANCE-funktion käyttäminen määritettyyn sijaintiin 30 km. 

**Kyselyn**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Tulokset**

    [{
      "id": "WakefieldFamily"
    }]

Jos sisällytät paikkatietojen indeksoinnin indeksoinnin käytännön, valitse "etäisyys kyselyjen" voi served tehokkaasti hakemiston kautta. Saat lisätietoja paikkatietojen indeksoinnin alla olevasta osiosta. Jos sinulla ei ole määritetty polkujen paikkatietojen indeksin, voit silti suorittaa paikkatietojen kyselyjen määrittämällä `x-ms-documentdb-query-enable-scan` pyynnön otsikkoon arvojoukko "true". .NET-kohdassa tämä voidaan toteuttaa välittämällä **FeedOptions** valinnaista argumenttia kyselyt, joilla [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) arvo on TOSI. 

ST_WITHIN voidaan tarkistaa Jos pisteen osuu monikulmion. Yleisesti monikulmio avulla voidaan esittää rajoja, kuten postinumeroiden tai valtion rajat luonnollinen muodostumiin. Uudelleen Jos sisällytät paikkatietojen indeksoinnin indeksoinnin käytännön, valitse "tatti" kyselyt voidaan served tehokkaasti hakemiston kautta. 

Monikulmion argumenttien ST_WITHIN voivat sisältää vain yhden soi, eli monikulmio ei saa olla niihin ratkaisuja. Tarkista pisteiden monikulmion ST_WITHIN kyselyn sallittu enimmäismäärä [DocumentDB rajoitukset](documentdb-limits.md) .

**Kyselyn**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Tulokset**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Miten ristiriitaiset DocumentDB kyselyssä, jos sijainti-arvon määritetty argumentti on Vääränlainen tai virheellinen ja valitse se arvioi **Määrittämätön** ja laskea asiakirjan ohitetaan kyselyn tuloksista tyypit toimii samalla. Kysely palauttaa tuloksia, suorita ST_ISVALIDDETAILED, virheenkorjaus miksi spatail tyyppi on virheellinen.     

ST_ISVALID ja ST_ISVALIDDETAILED voidaan tarkistaa, onko paikkatietojen objektin kelvollinen. Esimerkiksi seuraava kysely tarkistaa pisteen poissa-alueen leveyttä arvon (-132.8) kanssa. ST_ISVALID palauttaa vain totuusarvon ja ST_ISVALIDDETAILED palauttaa Boolean ja merkkijonon, joka sisältää syy, miksi viestiä pidetään virheellinen.

**Kyselyn**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Tulokset**

    [{
      "$1": false
    }]

Näiden funktioiden avulla voidaan myös tarkistaa monikulmio. Esimerkiksi seuraavat Käytämme ST_ISVALIDDETAILED vahvistamiseen monikulmion, joka ei ole suljettu. 

**Kyselyn**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Tulokset**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Joka rivittää paikkatietojen Funktiot ja SQL-syntaksi DocumentDB varten. Nyt voit tarkastella, kuinka LINQ kyselyt tekee ja miten se toimii kanssa syntaksi on katsomista tähän mennessä.

## <a name="linq-to-documentdb-sql"></a>LINQ DocumentDB SQL
LINQ on .NET ohjelmoinnin mallia, joka ilmaisee laskenta kuin virtaa objektien kyselyjä. DocumentDB sisältää asiakkaan puoli kirjaston LINQ liityntäkohdat tekemällä mahdolliseksi välinen JSON ja .NET-objektit ja yhdistämismäärityksen LINQ osajoukko-muunnos kyselyt DocumentDB kyselyihin. 

Alla olevassa kuvassa arkkitehtuuri tukevat LINQ kyselyjen DocumentDB.  Käyttävä DocumentDB kehittäjät voivat luoda **IQueryable** objekti, joka tekee suoraan toimittajaa DocumentDB kysely, joka kääntää sitten LINQ kyselyn DocumentDB kyselyn. Kyselyn välitetään sitten noutaa tulokset JSON-muodossa joukko DocumentDB-palvelimeen. Palautettujen tulosten ovat poistaa stream asiakaspuolen .NET-objektien siirtäminen.

![Arkkitehtuuri tukevat LINQ kyselyjen DocumentDB - SQL-syntaksi, JSON-kyselykieltä, tietokannan käsitteitä ja SQL-kyselyjä][1]
 


### <a name="net-and-json-mapping"></a>.NET ja JSON yhdistäminen
.NET-objektit ja JSON asiakirjojen yhdistämisen on luonnollinen - jäsenen tietokenttiin on nyt yhdistetty JSON-objekti, jossa kenttänimi on nyt yhdistetty objektin "avain"-osa ja "arvo"-osa on määritetty arvo-osaan objektin rekursiivisesti. Esimerkki. Luotu perhe-objekti on nyt yhdistetty JSON asiakirjan alla kuvatulla tavalla. Ja päinvastoin, JSON asiakirja on yhdistetty takaisin .NET-objekti.

**C#-luokka**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ SQL-käännös
DocumentDB kyselyn palveluntarjoajan suorittaa parhaat työmäärään yhdistämismäärityksen LINQ kyselyn DocumentDB SQL-kyselyyn. Seuraava kuvaus Microsoft oletetaan lukija on basic tuttu kuin LINQ.

Ensin tyyppi-järjestelmän sovellus tukee kaikkia JSON primitiivityyppejä – numeerisissa tyypeissä, totuusarvo merkkijono tai null. Vain näitä JSON tiedostotyyppejä tuetaan. Seuraavat skalaarinen lausekkeet ovat tuettuja.

-   Vakion arvot – nämä sisältää vakion alkeistyyppi tietotyyppien arvoja kyselyn arvioidaan aikaan.

-   Ominaisuuden tai matriisista indeksin lausekkeista lausekkeet viittaavat ominaisuus, objektiin tai matriisin elementti.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Aritmeettiset lausekkeita - tällaisia yleiset aritmeettiset lausekkeet numeerisia tai loogisia arvoja. Kattavaan luetteloon Lisätietoja on SQL-määritys.

        2 * family.children[0].grade;
        x + y;

-   Merkkijonolauseke vertailu - tällaisia vertailu merkkijonoarvon, jotkin vakiona merkkijonoarvo.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objektin/matriisin luominen lauseke - lausekkeet palauttaa koostetun arvon tai anonyymi tyyppiseen objektiin tai matriisin tällaisia objekteja. Nämä arvot voivat olla ylemmän tason.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Tuetut LINQ operaattorien luettelo
Ohessa on luettelo tuetuista LINQ operaattorien DocumentDB .NET SDK mukana LINQ-palvelussa.

-   **Valitse**: ennusteiden kääntää SQL Valitse myös objektin rakentaminen
-   **Jossa**: suodattimien kääntää SQL WHERE ja tukevat käännös välillä & &, || ja! SQL-operaattoreita
-   **SelectMany**: nettoutuksen purkamiseen matriisien SQL JOIN-osan avulla. Ketju/sisäkkäiset suodatettavan taulukkoelementtien lausekkeiden avulla voidaan
-   **Lajittelu- ja OrderByDescending**: vastaa ORDER BY Nouseva tai laskeva:
-   **CompareTo**: kääntää alueen vertailuja. Usein käyttää merkkijonojen yhteydessä, koska ne eivät vastaa .NET
-   **Ota**: vastaa SQL-sivun, rajoittaminen kyselyn tulokset
-   **Matemaattiset funktiot**: tukee käännös kohteesta. VERKON 's itseisarvo-Acos-Asin Atan-enimmäismäärän Cos, eksponentti, Floor, loki, Log10, Pow, PYÖRISTÄ-funktiota, merkki, Sin, neliöjuuri, Tan, Truncate vastaava SQL sisäiset funktiot.
-   **Merkkijonofunktiot**: tukee käännös kohteesta. VERKON on yhdistetty, sisältää EndsWith, IndexOf, Laske, ToLower, TrimStart, korvaa, vastakkainen, TrimEnd, StartsWith, alimerkkijono, ToUpper vastaava SQL sisäiset funktiot.
-   **Matriisi funktioita**: tukee käännös kohteesta. VERKON on yhdistetty, sisältää ja määrää vastaava SQL sisäiset funktiot.
-   **Geospatiaalisia tunniste Funktiot**: tukee käännös kanta vaihtoehtoja etäisyys sijainnin IsValid ja IsValidDetailed vastaava SQL sisäiset funktiot.
-   **Käyttäjän määrittämän funktion tunniste funktion**: tukee käännös kanta menetelmästä UserDefinedFunctionProvider.Invoke vastaavien käyttäjän määrittämät funktio.
-   **Muut**: tukee käännös yhdistetyn ja ehdolliset operaattorit. Kääntää sisältää merkkijono sisältää, ARRAY_CONTAINS tai SQL-IN kontekstin mukaan.

### <a name="sql-query-operators"></a>SQL-kyselyn operaattoreita
Seuraavassa on joitakin esimerkkejä, jotka osoittavat, miten joitakin vakio LINQ kyselytoimintoja muunnetaan alaspäin DocumentDB kyselyt.

#### <a name="select-operator"></a>Valitse operaattori
Syntaksi on `input.Select(x => f(x))`, jossa `f` on skalaarinen lauseke.

**LINQ lambda lauseke**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ lambda lauseke**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ lambda lauseke**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany operaattori
Syntaksi on `input.SelectMany(x => f(x))`, jossa `f` on skalaarinen lauseke, joka palauttaa sivustokokoelman tyyppi.

**LINQ lambda lauseke**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Jos operaattori
Syntaksi on `input.Where(x => f(x))`, jossa `f` skalaarilauseke, joka palauttaa totuusarvon, joka on.

**LINQ lambda lauseke**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ lambda lauseke**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Koosteen SQL-kyselyjä
Yllä operaattoreita voidaan muodostetut lomakkeen tehokkaampia kyselyt. Koska DocumentDB tukee sisäkkäisiä sivustokokoelmat-kokoonpano joko voidaan yhdistetään tai sisäkkäisiä.

#### <a name="concatenation"></a>Yhdistäminen 

Syntaksi on `input(.|.SelectMany())(.Select()|.Where())*`. Ketjutetut kyselyn voit aloittaa on valinnainen `SelectMany` kyselyn perään useita `Select` tai `Where` operaattorit.


**LINQ lambda lauseke**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ lambda lauseke**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ lambda lauseke**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ lambda lauseke**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Sisäkkäisiä

Syntaksi on `input.SelectMany(x=>x.Q())` Q missä `Select`, `SelectMany`, tai `Where` operaattori.

Sisäkkäiset kyselyssä sisemmän kyselyn otetaan käyttöön muut elementit ulompi keräämisessä. Yksi tärkeä ominaisuus on sisemmän kyselyn voidaan viitata elementit ulompi kokoelmassa, kenttien Itseliitokset.

**LINQ lambda lauseke**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ lambda lauseke**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ lambda lauseke**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>SQL-kyselyiden suorittaminen
DocumentDB paljastaa resursseja kautta REST-Ohjelmointirajapinta, joka voidaan kutsua minkä tahansa kielen voi tehdä HTTP/HTTPS-pyyntöjen perusteella. Lisäksi DocumentDB tarjoaa useita Suositut kieliä, kuten .NET-, Node.js-, JavaScript- ja Python ohjelmoinnin kirjastoissa. Kaikki REST-Ohjelmointirajapinnalla ja eri kirjastoihin tukevat SQL-kysely. .NET SDK tukee LINQ lisäksi SQL kyselyt.

Seuraavissa esimerkeissä, voit luoda kyselyn ja lähetä se vastaan DocumentDB tietokanta-tili.

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA
DocumentDB on se avaa RESTful ohjelmoinnin mallista HTTP-yhteyden kautta. Tietokannan tilit on valmisteltu Azure-tilauksen käyttäminen. DocumentDB resurssin mallin koostuu resurssien tietokanta-tili, joista jokaisella on käytettävissä käyttämällä loogisen tai vakaata URI määrä. Resurssien joukon kutsutaan syötteen tässä asiakirjassa. Tietokantojen joukko koostuu tietokannan tilin useita sivustokokoelmat, joista kussakin kunkin mitä-– Ota sisältävät asiakirjat, UDF ja muut resurssityypit.

Näillä tiedoilla basic vuorovaikutus-malli on HTTP-verbien – HAE, HYLLYTETTY, Kirjaa ja poista niiden vakio tulkinnassa. Kirjaa verbin käytetään uusi resurssi luontia varten, suorittamiseen tallennetun toimintosarjan tai myöntämistä DocumentDB kyselyn. Kyselyjen aina luetaan vain toimintoja, joiden puoli-tehosteet.

Seuraavissa esimerkeissä VIESTIIN DocumentDB kyselyn vastaan kahden otoksen asiakirjoja sisältävät kokoelman on tarkistanut tähän mennessä. Kyselyssä on yksinkertainen suodatus-JSON name-ominaisuutta. Huomaa, `x-ms-documentdb-isquery` ja sisältötyyppi: `application/query+json` osoittamaan, että toiminto on kyselyn otsikot.


**Pyyntö**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Tulokset**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Toinen esimerkki näyttää monimutkaisia kysely, joka palauttaa useita tuloksia liitosta.

**Pyyntö**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Tulokset**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Jos kyselyn tuloksia ei sovi tulosten yksittäiseltä sivulta ja valitse REST-Ohjelmointirajapinnalla palauttaa jatkumista kautta `x-ms-continuation-token` vastauksen otsikon. Asiakkaat voivat Sivuta tulokset sisällyttämällä otsikon seuraavat tulokset. Tuloksia sivulla määrä voidaan ohjata myös kautta `x-ms-max-item-count` luku otsikon.

Voit hallita tietojen kelpoisuus käytännön kyselyjen avulla `x-ms-consistency-level` otsikon, kuten kaikki REST API-pyynnöt. Istunnon yhdenmukaisuuden, se on tarpeen Päivitä myös uusimmat `x-ms-session-token` kyselypyynnön eväste otsikko. Huomaa, että kyselyn pohjana sivustokokoelman indeksoinnin käytännön voi myös vaikuttaa kyselytulokset yhtenäisyyden. Kun indeksoinnin käytäntöasetukset oletusarvon loppu indeksi on aina ajan tasalla asiakirjan sisällön ja kyselytulokset vastaavat valinnut tietojen yhdenmukaisuuden. Jos indeksoinnin käytäntö on katsottu viive-kyselyt voi palauttaa vanhentuneiden tulokset. Saat lisätietoja viitata [DocumentDB yhdenmukaisuuden tasot] [consistency-levels].

Jos määritetty indeksoinnin käytäntö käyttöön kokoelman ei tue määritettyä kyselyä, DocumentDB-palvelin palauttaa 400-pyyntö ei kelpaa". Tämä palautetaan alueen kyselyitä, jotka perustuvat polut määritetty hash (tasa) haut, sekä poistaa eksplisiittisesti indeksoinnista polkuja. `x-ms-documentdb-query-enable-scan` Otsikon voidaan määrittää kyselyn voit tarkistaa indeksin ei ole käytettävissä.

### <a name="c-net-sdk"></a>C# (.NET) SDK-PAKETISSA
.NET SDK tukee LINQ ja SQL kyselyt. Seuraavassa esimerkissä on otettu käyttöön aiemmin tässä asiakirjassa Perussuodatus-kyselyn suorittaminen.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Tässä esimerkissä vertaa kahden testaavat kunkin asiakirjan ominaisuudet ja käyttää anonyymi ennusteiden. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Seuraava esimerkki näyttää liitosten, LINQ SelectMany kautta.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



.NET-asiakasohjelman iteroivaa automaattisesti läpi kaikki sivut kyselyn tulosten foreach lohkot yllä esitetyllä tavalla. Tuotu REST API-osan Kyselyasetukset ovat käytettävissä käyttämällä .NET SDK myös `FeedOptions` ja `FeedResponse` luokkien CreateDocumentQuery-menetelmää. Sivujen määrä voi säätää avulla `MaxItemCount` asetus. 

Voit määrittää sivutus myös eksplisiittisesti luomalla `IDocumentQueryable` avulla `IQueryable` objekti, valitse lukemalla` ResponseContinuationToken` arvot ja siirtämällä niitä uudelleen nimellä `RequestContinuationToken` - `FeedOptions`. `EnableScanInQuery`Voit määrittää käyttöön tarkistukset, kun kysely ei tue määritetty indeksoinnin käytäntö. Osioitu loppu, voit käyttää `PartitionKey` Suorita kysely vastaan yhden osion (vaikka DocumentDB automaattisesti voit purkaa tätä kyselyn tekstin), ja `EnableCrossPartitionQuery` suorittaa kyselyjä, jotka on ehkä suoritettava useita osioita vastaan. 

Viittaavat [DocumentDB .NET näytteiden](https://github.com/Azure/azure-documentdb-net) Lisää esimerkkejä sisältävän kyselyt. 

### <a name="javascript-server-side-api"></a>Palvelinpuolen JavaScript-Ohjelmointirajapinnan 
DocumentDB tarjoaa suoritetaan JavaScript-pohjaisen sovelluksen logiikkaa suoraan tallennettujen toimintosarjojen ja käynnistimien sivustokokoelmat-ohjelmoinnin mallin. JavaScript-logiikkaa rekisteröity sivustokokoelman tasolla sitten voit lähettää tiedostoja toiminnan tietyn sivustokokoelman toimintoja tietokannan. Nämä toiminnot ovat rivittää ympäröivä HAPPAMAN tapahtumat.

Seuraavassa esimerkissä esitetään, miten käytöstä JavaScript-palvelimen API queryDocuments kyselyjen sisä tallennettujen toimintosarjojen ja käynnistimien.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Koostefunktiot

Koostefunktiot tuki on Works, mutta jos tarvitset määrä tai summa-toimintoa sen jälkeen, voit toteuttaa saman tuloksen usealla eri tavalla.  

Lue polun:

- Voit suorittaa koostefunktioita tietojen hakeminen ja suorittamalla laskeminen paikallisesti. Se on kannata käyttää halpaa kyselyn ennuste, kuten `SELECT VALUE 1` sen sijaan, että koko asiakirjan, kuten `SELECT * FROM c`. Näin voit suurentaa käsitteleminen kullakin sivulla tulosten siten vältät muita tarvetta palata palvelun tarvittaessa tiedostojen määrä.
- Voit myös pienentää verkon latenssin toistuva PYÖRISTÄ-funktiota trips-tallennetun toimintosarjan. Katso tallennetun toimintosarjan, joka laskee annetun suodatin-kyselyn Laske otoksen [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Tallennetun toimintosarjan voit antaa käyttäjien monipuolisia liiketoimintalogiikan sekä tekevät koosteet tehokas tapa yhdistää.

Kirjoita polku:

- Muu yleisiä kaava on koota valmiiksi "Kirjoita" polku tulokset. Tämä on erityisen houkuttelevien, kun "lukea" pyynnöt määrä on suurempi kuin "kirjoittaa" pyyntöihin. Kerran valmiiksi koostetun, tulokset ovat käytettävissä lukea pyyntöä yhden pisteen.  Paras tapa valmiiksi koota DocumentDB on määrittäminen käynnistimen, joka on yhteydessä kunkin "kirjoitus-ja Päivitä metatiedot asiakirjasta, jossa on kysely, joka on parhaillaan sattui uusimmat tulokset. Esimerkiksi Tutki [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) -malli, joka päivittää minSize, maxSize ja metatietojen asiakirjan kokoelman virhettä. Otosten laajennettavissa päivittämään laskuri, summa, jne.

##<a name="references"></a>Viittaukset
1.  [Azure DocumentDB esittely][introduction]
2.  [DocumentDB SQL-määritys](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB .NET-objektit](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB yhdenmukaisuuden tasot][consistency-levels]
5.  ANSI SQL-2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON [http://json.org/](http://json.org/)
7.  JavaScript-määrityksen [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Suurten tietokantojen [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611) kyselyn arvioinnin tekniikat
10. Kyselyn käsittely rinnakkain relaatiotietokannasta järjestelmien IEEE tietokoneen Society painamalla 1994
11. Tan lu, Ooi, kysely käsittely rinnakkain relaatiotietokannasta järjestelmien IEEE tietokoneen Society painamalla 1994.
12. Saattanut Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar Andrew'lle Tomkins: Possu latinalainen: tietojen käsittely-SIGMOD 2008 ei niin-ulkoinen kielen.
13.     G. Graefe. Kyselyn optimointi tehdyt raamit. IEEE tietojen Eng. BULL., 18 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
