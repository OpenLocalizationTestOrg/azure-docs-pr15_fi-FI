<properties
 pageTitle="Sovelluskehittäjän opas - kyselykieltä | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - kyselykieltä IoT-toiminnosta twins, menetelmistä ja töiden tietojen hakemiseen käytetty kuvaus"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Viittaus - kyselykieltä twins ja työt

## <a name="overview"></a>Yleiskatsaus

IoT toiminnosta on tehokas SQL kaltaisessa kieli, jota haluat noutaa tiedot, jotka koskevat [laitteen twins] [ lnk-twins] ja [töiden][lnk-jobs]. Tässä artikkelissa esitellään:

* Tärkeimmät ominaisuudet esittely IoT keskittimeen kyselyn kielen ja
* Kielen yksityiskohtainen kuvaus.

## <a name="getting-started-with-twin-queries"></a>Sekä kyselyjen käytön aloittaminen

[Laitteen twins] [ lnk-twins] voi olla haluamaansa JSON objekteja tunnisteet ja ominaisuudet. IoT keskittimeen sallii kyselyn laitteen twins kuin yhden JSON-asiakirja, joka sisältää kaikki sekä tiedot.
Oletetaan esimerkiksi, että IoT-toiminnossa twins rakenne on seuraava:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT keskittimeen paljastaa laitteen twins asiakirjan kokoelman kutsutaan **laitteet**muodossa.
Seuraava kysely palauttaa niin laitteen twins koko sarjan:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT keskittimeen SDK: T] [ lnk-hub-sdks] tukevat sivutus suuri tulosten.

IoT toiminnosta avulla noutaa twins suodattaminen haluamaansa vaatimuksia. Esimerkiksi

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

hakee twins määrittäminen **meille** **location.region** tunniste.
Totuusarvo-operaattorit ja aritmeettiset vertailuja tuetaan esimerkiksi sekä,

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

hakee kaikki twins määritetty lähettämään telemetriatietojen kovin usein minuutin välein kuin Yhdysvalloissa sijaitseva. Käytön helpottamiseksi on myös mahdollista Matriisivakioiden käyttäminen yhdessä **-** ja **NTÄSSÄ** (ei sisälly) operaattoreita. Esimerkiksi

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

hakee kaikki twins, joka ilmoittaa wifi tai langallisen yhteyden. [WHERE-lauseen] viitata[ lnk-query-where] suodatustoiminnot täyden viittauksen osa.

Ryhmittely ja koosteet tuetaan myös. Esimerkiksi

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Palauttaa kunkin telemetriatietojen määrityksen tila laitteiden määrää.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Yllä olevassa esimerkissä on kuvattu tilanne, jossa kolme laitteiden raportoitu onnistuneen määritys, kaksi ovat edelleen asetusten ottamisesta käyttöön ja jokin raportoi virheestä.

### <a name="c-example"></a>C#-Esimerkki

Kyselytoimintoja on määritetty [C# palvelu SDK] [ lnk-hub-sdks] - **RegistryManager** -luokka.
Tässä on esimerkki yksinkertaisen kyselyn:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Huomaa, kuinka **kyselyn** objektin esiintymää sivun kokoisena (enintään 1 000) ja valitse useita sivuja noudettu kutsumalla **GetNextAsTwinAsync** menetelmiä useita kertoja.
On tärkeää muistaa, että kyselyobjektin paljastaa useita **seuraavan\***, riippuen kyselyn sarjoituksen vaihtoehdoista, eli sekä tai projektin objekteja tai vain teksti, jota käytetään, kun käytät ennusteiden Json.

### <a name="node-example"></a>Solmu-Esimerkki

Kyselytoimintoja on määritetty [solmu palvelu SDK] [ lnk-hub-sdks] - **rekisterin** objekti.
Tässä on esimerkki yksinkertaisen kyselyn:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Huomaa, kuinka **kyselyn** objektin esiintymää sivun kokoisena (enintään 1 000) ja valitse useita sivuja noudettu kutsumalla **nextAsTwin** menetelmiä useita kertoja.
On tärkeää muistaa, että kyselyobjektin paljastaa useita **seuraavan\***, riippuen kyselyn sarjoituksen vaihtoehdoista, eli sekä tai projektin objekteja tai vain teksti, jota käytetään, kun käytät ennusteiden Json.

### <a name="limitations"></a>Rajoitukset

Tällä hetkellä ennusteiden tuetaan vain, kun käytät koosteet, eli-koostetun kyselyt voi käyttää vain `SELECT *`. Lisäksi kooste tuetaan vain yhdessä ryhmittely.

## <a name="getting-started-with-jobs-queries"></a>Töiden kyselyjen käytön aloittaminen

[Töiden] [ lnk-jobs] voi suorittaa toimintoja joukoissa laitteiden. Kunkin laitteen sekä sisältää tiedot, jossa se on osa nimeltä **työt**kokoelman töiden.
Loogisesti,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Tämä kokoelma ei tällä hetkellä queriable **devices.jobs** IoT keskittimeen kyselykielen nimellä.

Esimerkiksi saat kaikki työt (nykyisiä ja ajoitettu), jotka vaikuttavat yhteen laitteeseen, voit käyttää seuraavaan kyselyyn:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Huomaa, miten tämä kysely tarjoaa laitekohtaiset tila (ja mahdollisesti suora menetelmä vastaus) kunkin projektin palautetaan.
On myös mahdollista suodattaa haluamaansa totuusarvo ehtoja, valitse kaikki objektit **devices.jobs** sivustokokoelman ominaisuudet.
Esimerkiksi seuraava kysely:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

hakee kaikki valmiit sekä päivityksen työt laitteen **myDeviceId** , jotka on luotu syyskuu 2016.

On myös mahdollista laitteen kohti tulokset yhteen työn hakemiseen.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Rajoitukset
Tällä hetkellä **devices.jobs** kyselyt eivät tue:

* Ennusteiden, eli vain `SELECT *` on mahdollista.
* Ehdot, jotka viittaavat laite-sekä lisäksi projektin ominaisuudet kuin yllä;
* Suorittaa koosteet, kuten laskeminen, keskiarvo, Ryhmittele.

## <a name="basics-of-an-iot-hub-query"></a>IoT keskittimeen kyselyn perusteet

Jokainen IoT keskittimeen kyselyn muodostuu Valitse ja lausekkeita ja valinnainen WHERE ja GROUP BY lauseet. Jokainen kyselyn suoritetaan kokoelma JSON asiakirjoja, kuten laitteen twins. FROM-lauseen osoittaa asiakirjan kokoelman iteroitava (**laitteiden** tai **devices.jobs**). Valitse WHERE-lauseen suodatin on otettu käyttöön. Koosteet, kyseessä on ryhmitelty tämän vaiheen tulokset kuin määritetty GROUP BY-lauseen ja kunkin ryhmän, rivin luodaan määritysten mukaisesti SELECT-lauseessa.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-lause

**< From_specification > FROM** -lause voi oletetaan, että vain kaksi arvoa: **Lähettäjä laitteet**-kyselyn laitteen twins tai **FROM devices.jobs**, kyselyn työn kohti laitteen tiedot.

## <a name="where-clause"></a>WHERE-lause

**Missä < filter_condition >** -lause on valinnainen. Määrittää ehdot, jotka lähettäjä-sivustokokoelman JSON tiedostoissa on täytettävä ollakseni tuloksen mukaan. Mikä tahansa JSON-tiedosto on annettava tulokseksi "true" tulos sisällytettävät määritetyt ehdot.

Sallitun edellytykset on kuvattu osassa [lausekkeiden ja ehtoja][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-lause

(**Valitse < select_list >**) SELECT-lauseessa on pakollinen ja määrittää arvot ovat noudettu kyselystä. Määrittää JSON-arvoja käytetään uusien JSON-objektien luomiseen, muut elementit lähettäjä-sivustokokoelman suodatetut (ja halutessasi ryhmitellyt) alijoukko ennuste vaiheen Luo uusi JSON-objekti, rakennettava SELECT-lauseessa annetuilla arvoilla.

SELECT-lauseen kieliopin seuraavat asiat:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

**attribute_name** viittaa missä tahansa ominaisuuden JSON asiakirjan lähettäjä-sivustokokoelman. Esimerkkejä SELECT-lauseet löytyvät [aloittaminen sekä kyselyt] [ lnk-query-getstarted] osa.

Tällä hetkellä valinnan lauseet erilainen kuin **Valitse \* ** tuetaan vain twins kooste kyselyjä.

## <a name="group-by-clause"></a>GROUP BY-lause

**GROUP BY < group_specification >** -lause on valinnainen vaihe, joka on voidaan suorittaa WHERE-lauseen, ja valitse määritetty ennuste ennen suodatuksen jälkeen. Ryhmittelee asiakirjojen määritteen arvon perusteella. Koostetun arvot määritettyinä SELECT-lauseen avulla luodaan ryhmiin.

Esimerkki kyselyn käyttämällä GROUP BY on:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

GROUP BY muodollinen syntaksi on seuraava:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

**attribute_name** viittaa missä tahansa ominaisuuden JSON asiakirjan lähettäjä-sivustokokoelman. 

GROUP BY-lauseeseen tällä hetkellä tuetaan vain, kun kysely suoritetaan twins.

## <a name="expressions-and-conditions"></a>Lausekkeiden ja ehdot

Korkean tason- *lauseke*:

* Esiintymän JSON tyyppi (eli totuusarvo, numero, merkkijono, taulukko tai objektin), tulos ja
* Määrittää käsittelevistä laitteen JSON asiakirjaan tai valmiin operaattorit ja funktioiden vakiot tulevat tiedot.

*Ehdot* ovat lausekkeita, jotka arvo on totuusarvo, eli erilainen kuin totuusarvo **Tosi** pidetään **Epätosi** (mukaan lukien **null** **Määrittämätön**, minkä tahansa objektin tai matriisista esiintymän, mikä tahansa merkkijono ja selkeästi totuusarvon **Epätosi**) minkä tahansa vakio.

Lausekkeiden syntaksi on seuraava:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

Jos:

| Symboli | Määritys |
| -------------- | -----------------|
| attribute_name | Minkä tahansa lähettäjä-sivustokokoelman JSON asiakirjan ominaisuus. |
| unary_operator | Mikä tahansa yksiparametrinen operaattori, kuten operaattoreita. |
| binary_operator | Minkä tahansa binaarinen operaattori, kuten operaattoreita. |
| decimal_literal | Liukuluku kymmenjärjestelmän lukuna ilmaistuna. |
| hexadecimal_literal | Merkkijonon "0 x', jota seuraa numerosarja heksadesimaaliluvun numeroiden mukaan ilmaistu luku. |
| string_literal | Merkkijono literaalien ovat järjestyksen nolla vähintään Unicode-merkkejä tai ESC järjestyksiä vastaavan Unicode-merkkijonoja. Merkkijono literaalien kirjoitetaan puolilainausmerkkeihin (heittomerkki: ") tai lainausmerkki (lainausmerkki:"). Sallittu kokonaiseen: `\'`, `\"`, `\\`, `\uXXXX` Unicode-merkkien määrittämiä 4 heksadesimaaliluvun numeroa. |

### <a name="operators"></a>Operaattorit

Käytä seuraavia operaattoreita tuetaan:

| Tuoteperhe | Operaattorit |
| -------------- | -----------------|
| Aritmeettinen | +,-,*,/,% |
| Loogisen | JA- TAI EI |
| Vertailu |  =,! = <>,, < =, > = <> |

## <a name="next-steps"></a>Seuraavat vaiheet

Lue, miten voit suorittaa kyselyjä käyttämällä [IoT keskittimeen SDK: T]sovelluksia[lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md