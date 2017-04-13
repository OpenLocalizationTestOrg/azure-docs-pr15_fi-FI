<properties
    pageTitle="Esimerkkejä yleisten käyttötavat Stream Analytics kyselyn | Microsoft Azure"
    description="Yleiset Azure Stream Analytics kyselyn mallit "
    keywords="kyselyn esimerkkejä"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Kyselyn esimerkkejä yleisten Stream Analytics-käyttötavat

## <a name="introduction"></a>Johdanto

Kyselyjen Azure Stream Analytics ilmaistaan kaltaisessa SQL-kyselyn kielellä, joka on kuvattu [Stream Analytics kyselyn kieli](https://msdn.microsoft.com/library/azure/dn834998.aspx) -opas.  Tässä artikkelissa käsitellään useita yleisiä kyselyn kuviot käytännön skenaariot perusteella ratkaisuja.  Se on keskeneräisen työn ja päivitetään jatkuvasti uusi kuvioita säilyvät.

## <a name="query-example-data-type-conversions"></a>Kyselyn Esimerkki: tietojen tietotyyppimuunnosten
**Kuvaus**: määrittää ominaisuuksien tyypit syötteen stream.
Esimerkiksi auton paino tulossa-syötteen muodossa kuin merkkijonot ja on suoritettava INT muunnetaan summaa sitä.

**IME**:

| Tee | Aika | Weight (paino) |
| --- | --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01 – 01T00:00:02.0000000Z | "2000" |

**Tulos**:

| Tee | Weight (paino) |
| --- | --- |
| Honda | 3000 |

**Ratkaisu**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Selitys**: sen Määritä weight (paino)-kentän CAST ilmoituksen avulla (Katso tuetut tietotyypit luettelo [tähän](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Kyselyn Esimerkki: käyttämällä tykkää/ei järjestelmän mallia vastaavia
**Kuvaus**: Tarkista, että tapahtuma-kenttäarvo vastaa tietyn merkkijonon esimerkiksi Palauta käyttöoikeus erottelusivuja, jotka alkavat kirjaimella A ja perässä 9

**IME**:

| Tee | LicensePlate | Aika |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | AAA-999 | 2015-01 – 01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01 – 01T00:00:03.0000000Z |

**Tulos**:

| Tee | LicensePlate | Aika |
| --- | --- | --- |
| Toyota | AAA-999 | 2015-01 – 01T00:00:02.0000000Z |
| Nissan | ABC 369 | 2015-01 – 01T00:00:03.0000000Z |

**Ratkaisu**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Selitys**: Tarkista, että LicensePlate-kenttäarvo alkaa A sitten on mikä tahansa merkkijono nolla tai useampia ja loppuosa on 9 LIKE-ilmoituksen avulla. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Kyselyn Esimerkki: Määritä logiikan eri tapauksissa/arvot (isot-lauseet)
**Kuvaus**: Anna eri laskenta kentälle, jotkin ehtojen perusteella.
Ongelman esimerkiksi merkkijonon kuvaus, kuinka monta autojen välitetty saman määräten tapaukseen on 1.

**IME**:

| Tee | Aika |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Tulos**:

| CarsPassed | Aika |
| --- | --- | --- |
| 1 Honda | 2015-01 – 01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01 – 01T00:00:10.0000000Z |

**Ratkaisu**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Selitys**: tapaus lauseen pystyy antamaan eri laskenta, jotkin ehdot (Microsoftin tapaus autojen kooste-ikkunassa määrää) perusteella.

## <a name="query-example-send-data-to-multiple-outputs"></a>Kyselyn Esimerkki: tietojen lähettäminen useita tulostus
**Kuvaus**: tietojen lähettäminen useisiin tulosteen kohteisiin yksittäisestä työstä.
Esimerkiksi raja-pohjainen-ilmoituksen tietojen analysointia ja arkistoida kaikki tapahtumat-blob-säiliö

**IME**:

| Tee | Aika |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Output1**:

| Tee | Aika |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Output2**:

| Tee | Aika | Laske |
| --- | --- | --- |
| Toyota | 2015-01 – 01T00:00:10.0000000Z | 3 |

**Ratkaisu**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Selitys**: INTO-lauseen kertoo Stream Analytics joka kirjoittaa tiedot tietosuojatiedoissa tulostaa.
Ensimmäinen kysely on läpivientikysely tiedot on vastaanotettu, että olemme nimeltä ArchiveOutput tulos.
Toista kyselyä ei joitakin yksinkertaisia kooste ja suodattaminen ja lähettää tulokset edeltävät suunnattujen varoitusmenetelmien järjestelmään.
*Huomautus*: Voit myös käyttää CTEs tulokset (eli kanssa-lauseet) useita tulosteen lauseet – tämä on lisätty etuna avaaminen vähemmän lukijat syötteen lähde.
Esimerkiksi 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Kyselyn Esimerkki: laskien yksilölliset arvot
**Kuvaus**: sanamäärän yksilöllinen kenttäarvoja, jotka näkyvät virta aika-ikkunassa.
Kuinka monta yksilöllinen merkki autot välitetty esimerkiksi maksullinen booth 2 toisessa ikkunassa kautta?

**IME**:

| Tee | Aika |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Honda | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | 2015-01 – 01T00:00:03.0000000Z |

**Tulos:**

| Laske | Aika |
| --- | --- |
| 2 | 2015-01 – 01T00:00:02.000Z |
| 1 | 2015-01 – 01T00:00:04.000Z |

**Ratkaisu:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Selitys:** Olemme Tee alkuperäisen kooste yksilöllinen helpottaa niiden laskeminen ja saat ikkunan päälle.
Olemme Tee, kuinka monta on käytössä – annettu yksilöiviä arvoja ikkunassa hakea saman aikaleima ja toinen kooste-ikkuna on oltava mahdollisimman vähän koostaa ei ensimmäisen vaiheen 2 windows tekee kooste.

## <a name="query-example-determine-if-a-value-has-changed"></a>Kyselyn Esimerkki: määrittää, jos arvo on muuttunut#
**Kuvaus**: Tarkista, onko erilainen kuin nykyinen arvo on esimerkiksi edellisen auton sama Tee nykyinen auton maksullinen matkalla edellisen arvo?

**IME**:

| Tee | Aika |
| --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |

**Tulos**:

| Tee | Aika |
| --- | --- |
| Toyota | 2015-01 – 01T00:00:02.0000000Z |

**Ratkaisu**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Selitys**: Käytä VIIVE vilkaista kyselyjä syötteen virtauttaa tapahtuman takaisin ja varmista mittaaminen. Valitse vertaa sitä nykyisen tapahtuman, tee ja siirtää tapahtumaa, jos ne ovat eri.

## <a name="query-example-find-first-event-in-a-window"></a>Kyselyn Esimerkki: Etsi ensimmäinen ikkunassa
**Kuvaus**: Etsi ensimmäinen auton 10 minuutin välein väli?

**IME**:

| LicensePlate | Tee | Aika |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Tulos**:

| LicensePlate | Tee | Aika |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Ratkaisu**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Kun oletetaan, että muuta ongelma ja Etsi ensimmäinen Auto, erityisesti tehdä 10 minuutin välein väli.

| LicensePlate | Tee | Aika |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ratkaisu**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Kyselyn Esimerkki: Etsi viimeinen ikkunassa
**Kuvaus**: Etsi viimeisen auton 10 minuutin välein väli.

**IME**:

| LicensePlate | Tee | Aika |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Tulos**:

| LicensePlate | Tee | Aika |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ratkaisu**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Selitys**: kyselyssä on kaksi vaihetta – ensimmäinen etsii uusimmat aikaleima windows 10 minuutin. Toinen vaihe yhdistää ja Etsi tapahtumia, jotka vastaavat kunkin ikkunassa viimeisen aikaleimat alkuperäisen stream ensimmäisen kyselyn tulokset. 

## <a name="query-example-detect-the-absence-of-events"></a>Kyselyn Esimerkki: tunnistaa tapahtumien puuttuminen
**Kuvaus**: Tarkista, että virta ei ole arvoa, joka vastaa tietyt ehdot.
Esimerkiksi 2 peräkkäiset autot-samaan tilaan kirjoittanut maksullinen tien 90 muutamassa?

**IME**:

| Tee | LicensePlate | Aika |
| --- | --- | --- |
| Honda | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |
| Honda | AAA-999 | 2015-01 – 01T00:00:02.0000000Z |
| Toyota | DEF 987 | 2015-01 – 01T00:00:03.0000000Z |
| Honda | GHI 345 | 2015-01 – 01T00:00:04.0000000Z |

**Tulos**:

| Tee | Aika | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01 – 01T00:00:02.0000000Z | AAA-999 | ABC 123 | 2015-01 – 01T00:00:01.0000000Z |

**Ratkaisu**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Selitys**: Käytä VIIVE vilkaista kyselyjä syötteen virtauttaa tapahtuman takaisin ja varmista mittaaminen. Valitse vertaa sitä tee nykyisen tapahtuman ja tulosteen tapahtuman, jos ne ovat samat ja VIIVE avulla saat tietoja edellisen Auto.

## <a name="query-example-detect-duration-between-events"></a>Kyselyn Esimerkki: tunnistaa tapahtumien väliin jäävä aika
**Kuvaus**: Etsi tietyn tapahtuman kestoa. Web-clickstream annettu määrittävät aika-ominaisuutta.

**IME**:  
  
| Käyttäjän | Toiminto | Tapahtuman | Aika |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Aloittaminen | 2015-01 – 01T00:00:01.0000000Z |
| user@location.com | RightMenu | Lopeta | 2015-01 – 01T00:00:08.0000000Z |
  
**Tulos**:  
  
| Käyttäjän | Toiminto | Kesto |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Ratkaisu**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Selitys**: Käytä viimeisen funktiota hakemiseen viimeisen aika-arvon, kun tapahtumalaji on "Start". Huomaa, että EDELLISEN toiminnon osoittamaan, että tulos on laskettu yksilöivä käyttäjäkohtainen käyttää OSION mukaan [käyttäjä].  Kyselyn on 1 tunti yläraja ajan erotuksen 'Start' ja 'Lopeta' tapahtumat, mutta se on määritettävä kuin tarpeen mukaan (raja DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Kyselyn Esimerkki: tunnistaa ehdon kesto
**Kuvaus**: Etsi tilanne, kuinka kauan ulos.
Oletetaan esimerkiksi, että ohjelmavirhe, jotka ovat kaikki autoista on virheellinen paino (yli 20 000 punta) – haluamme ohjelmavirhe keston laskemiseen.

**IME**:

| Tee | Aika | Weight (paino) |
| --- | --- | --- |
| Honda | 2015-01 – 01T00:00:01.0000000Z | 2000: ssa |
| Toyota | 2015-01 – 01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01 – 01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01 – 01T00:00:08.0000000Z | 2000: ssa |

**Tulos**:

| StartFault | EndFault |
| --- | --- |
| 2015-01 – 01T00:00:02.000Z | 2015-01 – 01T00:00:07.000Z |

**Ratkaisu**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Selitys**: Käytä VIIVE Näytä syötteen virta 24 tuntia ja Etsi esiintymät, joissa StartFault ja StopFault on koottu mukaan weight < 20000.

## <a name="query-example-fill-missing-values"></a>Kyselyn Esimerkki: Täytä puuttuvat arvot
**Kuvaus**: tilanteista, joissa puuttuvat arvot muodossa, tuottaa stream tapahtumien kanssa säännöllisin väliajoin.
Luo tapahtuman esimerkiksi 5 sekunnin välein, joka ilmoittaa viimeksi näkyy arvopistettä.

**IME**:

| t | arvo |
|--------------------------|-------|
| "2014-01 – 01T06:01:00" | 1 |
| "2014-01 – 01T06:01:05" | 2 |
| "2014-01 – 01T06:01:10" | 3 |
| "2014-01 – 01T06:01:15" | 4 |
| "2014-01 – 01T06:01:30" | 5 |
| "2014-01 – 01T06:01:35" | 6 |

**Näytä (10 ensimmäistä riviä)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01 – 01T14:01:00.000Z | 2014-01 – 01T14:01:00.000Z | 1 |
| 2014-01 – 01T14:01:05.000Z | 2014-01 – 01T14:01:05.000Z | 2 |
| 2014-01 – 01T14:01:10.000Z | 2014-01 – 01T14:01:10.000Z | 3 |
| 2014-01 – 01T14:01:15.000Z | 2014-01 – 01T14:01:15.000Z | 4 |
| 2014-01 – 01T14:01:20.000Z | 2014-01 – 01T14:01:15.000Z | 4 |
| 2014-01 – 01T14:01:25.000Z | 2014-01 – 01T14:01:15.000Z | 4 |
| 2014-01 – 01T14:01:30.000Z | 2014-01 – 01T14:01:30.000Z | 5 |
| 2014-01 – 01T14:01:35.000Z | 2014-01 – 01T14:01:35.000Z | 6 |
| 2014-01 – 01T14:01:40.000Z | 2014-01 – 01T14:01:35.000Z | 6 |
| 2014-01 – 01T14:01:45.000Z | 2014-01 – 01T14:01:35.000Z | 6 |

    
**Ratkaisu**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Selitys**: Tämä kysely luo tapahtumien 5 sekunnin välein ja siirtää tiedot, jotka on vastaanotettu ennen viimeinen. [Hopping ikkuna] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping ikkuna - Azure Stream Analytics") kesto määrittää, kuinka kauas taaksepäin kysely näyttää Etsi uusimman tapahtuman (Tässä esimerkissä 300 sekuntia).


## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
