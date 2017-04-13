<properties
    pageTitle="Virtauttaa Analytics tulostaa: tallennus-analyysin asetukset | Microsoft Azure"
    description="Lisätietoja kohdistamisen Stream Analytics tietojen tulostaa asetuksia, kuten Power BI for analyysitulokset."
    keywords="muunnetaan, analyysitulokset ja tietojen säilytysasetukset"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Virtauttaa Analytics tulostaa: tallennus-analyysi-asetukset

Kun authoring Stream Analytics-työ, harkitse kuinka tuloksena olevat tiedot kulutettu. Kuinka Stream Analytics työn tulokset tarkastella ja kohtaa, johon voit tallentaa sen?

Jotta voit ottaa käyttöön sovelluksen kuviot erilaisia, Azure Stream Analytics on eri toiminnot tulosteen tallentaminen ja tarkasteleminen analyysitulokset. Tämä on helppo tarkastella projektin tulostus ja joustavuutta kulutus ja työn tulosteen tallennustilaan tietovaraston ja muita varten. Määritetty työn tuloksen on oltava olemassa ennen työ on alkanut ja tapahtumien Käynnistä juoksutus. Esimerkiksi jos käytät Blob-objektien tallennustilaan tulos, työn ei luo tallennustilan tilin automaattisesti. Se on luotava käyttäjän, ennen kuin ASA työ on alkanut.

## <a name="azure-data-lake-store"></a>Azure järvi tietosäilö

Virta Analytics tukee [Azure Lake Tietosäilölle](https://azure.microsoft.com/services/data-lake-store/). Tämä avulla voit tallentaa minkä tahansa kokoa, tyyppi ja nieltynä nopeuden toiminnalliset ja kokeilevaa analysoinnissa. Tällä hetkellä luominen ja määrittäminen järvi tietovaraston tulostaa tuetaan vain-perinteinen Azure-portaalissa. Lisäksi Stream Analytics on oikeutta käyttää järvi tietovaraston. [Tietojen järvi tulosteen artikkelissa](stream-analytics-data-lake-output.md)käsitellään todennus ja tilaaminen tietojen järvi säilön Preview (tarvittaessa).

### <a name="authorize-an-azure-data-lake-store"></a>Määritä Azure tietojen järvi-säilö

Kun järvi tietosäilö on valittu tulos Azure hallinta-portaalissa, voit pyydetään sallivat yhteyden aiemmin luotuun järvi Tietosäilölle.  

![Määritä järvi tietosäilö](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Täytä sitten järvi tietovaraston tulosteen ominaisuuksien tarkastelu alla:

![Määritä järvi tietosäilö](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Seuraavassa taulukossa on lueteltu ominaisuuksien nimet ja niiden järvi tietovaraston tulosteen luomiseen tarvitaan kuvaus.

<table>
<tbody>
<tr>
<td><B>OMINAISUUDEN NIMI</B></td>
<td><B>KUVAUS</B></td>
</tr>
<tr>
<td>Tunnus</td>
<td>Tämä on kutsumanimi, jota käytetään kyselyissä voidaan ohjata tämän tietojen järvi Store kyselyn tuloksiin.</td>
</tr>
<tr>
<td>Tilin nimi</td>
<td>Kohtaa, johon haluat lähettää jaettavaan järvi tietosäilö tilin nimi. Voit valita jommankumman kanssa avattavasta luettelosta järvi tietovaraston tileistä, johon kirjautuneena portaaliin käyttäjällä on käyttöoikeus.</td>
</tr>
<tr>
<td>Polun etuliite kuvion [<I>Valinnainen</I>]</td>
<td>Tiedostopolku, Tallenna tiedostosi määritetyn tietojen järvi kaupan tilin avulla. <BR>{date}, {time}<BR>Esimerkki 1: folder1/lokit / {date} / {aika}<BR>Esimerkki 2: folder1/lokit / {date}</td>
</tr>
<tr>
<td>Päivämäärämuoto [<I>Valinnainen</I>]</td>
<td>Jos etuliite polku käytetään päivämäärä-tunnuksen, voit valita päivämäärämuodon jossa tiedostojen järjestetään. Esimerkki: VVVV/KK/PP</td>
</tr>
<tr>
<td>Aikamuoto [<I>Valinnainen</I>]</td>
<td>Jos aika-tunnuksen käytetään etuliite-polku, Määritä Aikamuoto, johon tiedostot on järjestetty. Ainoa tuettu arvo on tällä hetkellä HH.</td>
</tr>
<tr>
<td>Tapahtuman Sarjatoiminto muoto</td>
<td>Sarjatoiminto tulosteen tietojen muoto. JSON, CSV ja Avro tuetaan.</td>
</tr>
<tr>
<td>Koodaus</td>
<td>Jos CSV- tai JSON Muotoile koodauksen on määritetty. UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä.</td>
</tr>
<tr>
<td>Erottimen</td>
<td>Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoitettaessa CSV-tiedot. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki.</td>
</tr>
<tr>
<td>Muotoile</td>
<td>Käytettävissä vain JSON Sarjatoiminto. Erotettu viiva osoittaa, että tulos muotoiltu siten, että kunkin uuden rivin erotettu JSON-objektin. Matriisin määrittää tulosteen muotoillaan JSON objektien matriisina.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Tietosäilö järvi luvan uusiminen

Tarvitset tarkistamiseen järvi tietovaraston tilin uudelleen, jos salasana on muuttunut, vaikka työtäsi on luotu tai viimeksi todennettu.

![Määritä järvi tietosäilö](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-tietokantaan

[Azure SQL-tietokanta](https://azure.microsoft.com/services/sql-database/) voidaan kuin tulos, tiedot, jotka ovat relaatiotietoja laatu tai sisältö on ylläpidettävä relaatiotietokannasta riippuvat sovellukset. Virta Analytics työt kirjoittaa aiemmin luotuun taulukkoon Azure SQL-tietokantaan.  Huomaa, että taulukko rakenne on oltava täsmälleen samat kentät ja että työtäsi tulosteen tyypit. [Azure SQL-tietovarasto](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) voidaan määrittää myös tulos SQL-tietokantaan tulostusasetukset sekä (tämä on esikatselu-toimintoa) kautta. Alla olevassa taulukossa on lueteltu ominaisuuksien nimet ja kuvaukset luomiseen SQL-tietokanta-tuloste.

| Ominaisuuden nimi | Kuvaus |
|---------------|-------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä ohjaamaan tähän tietokantaan kyselyn tuloksiin. |
| Tietokannan | Jos haluat lähettää jaettavaan tietokannan nimi |
| Palvelimen nimi | SQL-tietokantapalvelimen nimi |
| Käyttäjänimi | Käyttäjänimi, jolla on pääsy tietokantaan kirjoittaminen |
| Salasana | Muodosta yhteys tietokantaan salasana |
| Taulukko | Taulukkonimi, johon tulosteen kirjoitetaan. Tämä taulukko rakenne on vastattava täsmälleen määrä kenttiä ja niiden tiedostotyyppien työn tulosteen luomaa olevaan taulukkonimi on kirjainkoko on merkitsevä. |

> [AZURE.NOTE] Azure SQL-tietokanta-tarjoaa on tällä hetkellä tueta työ-tulostus Stream Analytics. Kuitenkin Azure Virtual Machine käynnissä SQL Serverin tietokantaan, joka on liitetty ei tueta. Tämä on saatetaan muuttaa tulevissa versioissa.

## <a name="blob-storage"></a>Blob-objektien tallennustilaan

Blob-objektien tallennustilaan on edullinen ja skaalattava ratkaisua tallentaminen pilvipalveluun rakenteeton suurista tietomääristä.  Azure-Blob-säiliö ja sen käyttöä koskeva esittely miten [Voit käyttää BLOB](../storage/storage-dotnet-how-to-use-blobs.md)ohjeissa.

Alla olevassa taulukossa on lueteltu ominaisuuksien nimet ja kuvaukset luomisen blob-tuloste.

<table>
<tbody>
<tr>
<td>OMINAISUUDEN NIMI</td>
<td>KUVAUS</td>
</tr>
<tr>
<td>Tunnus</td>
<td>Tämä on kutsumanimi, jota käytetään kyselyissä voidaan ohjata tämän Blob-objektien tallennustilaan kyselyn tulosteet.</td>
</tr>
<tr>
<td>Tallennustilan tilin</td>
<td>Jos haluat lähettää jaettavaan tallennustilan tilin nimi.</td>
</tr>
<tr>
<td>Tallennustilan tilin avain</td>
<td>Tallennustilan-tiliin liittyvät salausavaimen.</td>
</tr>
<tr>
<td>Tallennustilan säilö</td>
<td>Säilöjen on tallennettu Microsoft Azure-Blob-palveluun BLOB looginen ryhmittely. Kun lataat blob Blob-palveluun, sinun on määritettävä kyseisen blob säilö.</td>
</tr>
<tr>
<td>Polun etuliite kuvion [valinnainen]</td>
<td>Kirjoittamalla oman BLOB määritetyn säilöön tiedostopolku.<BR>PATH-voit valita yhden tai useamman esiintymät seuraavat 2 muuttujat avulla voit määrittää, jotka on kirjoitettu BLOB korkojakso:<BR>{date}, {time}<BR>Esimerkki 1: cluster1/lokit / {date} / {aika}<BR>Esimerkki 2: cluster1/lokit / {date}</td>
</tr>
<tr>
<td>Päivämäärämuoto [valinnainen]</td>
<td>Jos etuliite polku käytetään päivämäärä-tunnuksen, voit valita päivämäärämuodon jossa tiedostojen järjestetään. Esimerkki: VVVV/KK/PP</td>
</tr>
<tr>
<td>Aikamuoto [valinnainen]</td>
<td>Jos aika-tunnuksen käytetään etuliite-polku, Määritä Aikamuoto, johon tiedostot on järjestetty. Ainoa tuettu arvo on tällä hetkellä HH.</td>
</tr>
<tr>
<td>Tapahtuman Sarjatoiminto muoto</td>
<td>Sarjatoiminto tulosteen tietojen muoto.  JSON, CSV ja Avro tuetaan.</td>
</tr>
<tr>
<td>Koodaus</td>
<td>Jos CSV- tai JSON Muotoile koodauksen on määritetty. UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä.</td>
</tr>
<tr>
<td>Erottimen</td>
<td>Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoitettaessa CSV-tiedot. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki.</td>
</tr>
<tr>
<td>Muotoile</td>
<td>Käytettävissä vain JSON Sarjatoiminto. Erotettu viiva osoittaa, että tulos muotoiltu siten, että kunkin uuden rivin erotettu JSON-objektin. Matriisin määrittää tulosteen muotoillaan JSON objektien matriisina.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Tapahtuma-toiminnossa

[Tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/) on erittäin skaalattava Julkaise tilaa tapahtuman ingestor. Se voi kerätä tapahtumia sekunnissa miljoonia.  Yksi käyttöä tulosteen tapahtumaa-toiminnossa on, kun Stream Analytics työn tulos on toisen streaming työn syötteen.

On muutama parametrit, joita tarvitaan määrittää tapahtumaa-toiminnossa tietojen virtaa tulos.

| Ominaisuuden nimi | Kuvaus |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä ohjaamaan tapahtuma-keskittimeen kyselyn tuloksiin. |
| Palvelun Bus Namespace | Palvelun Bus nimitila on joukko messaging kohteiden säilö. Kun olet luonut uuden tapahtuman-keskittimeen, luomasi myös palvelun Bus nimitila |
| Tapahtuma-toiminnossa | Tapahtuma-toiminnossa tulosteen nimi |
| Tapahtuman keskittimeen käytännön nimi | Jaetun access-käytäntö, joka voidaan luoda tapahtuman keskittimeen määrittäminen-välilehdessä. Kukin jaetun access-käytäntö on nimi määritetyt käyttöoikeudet, ja valintanäppäimet |
| Tapahtuman keskittimeen käytännön avain | Jaettu pikanäppäin todennetaan pääsy palvelun Bus nimitila |
| Osion avainsarakkeen [valinnainen] | Sarake sisältää tapahtumaa-toiminnossa tulostukseen osio-näppäintä. |
| Tapahtuman Sarjatoiminto muoto | Sarjatoiminto tulosteen tietojen muoto.  JSON, CSV ja Avro tuetaan. |
| Koodaus | CSV-ja JSON UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä |
| Erottimen | Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoittamista tiedot CSV-muodossa. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki. |
| Muotoile | Käytettävissä vain JSON tyyppi. Erotettu viiva osoittaa, että tulos muotoiltu siten, että kunkin uuden rivin erotettu JSON-objektin. Matriisin määrittää tulosteen muotoillaan JSON objektien matriisina. |

## <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com/) voidaan kuin tulos Stream Analytics projektin antamaan monipuolisia visualisoinnin käyttökokemusta analyysin tulokset. Tätä ominaisuutta voi käyttää toiminnallisia raporttinäkymiä, raportin luonti ja metrijärjestelmän perustuva raportointi.

### <a name="authorize-a-power-bi-account"></a>Määritä Power BI-tili

1.  Kun Power BI on valittu tulos Azure hallinta-portaalissa, sinua pyydetään vahvistamaan aiemmin luodun Power BI-käyttäjän tai Luo uusi Power BI-tili.  

    ![Määritä Power BI-käyttäjä](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Luo uusi tili, jos et vielä on yksi ja valitse sitten Määritä nyt.  Näytön seuraavanlaisia esitetään.  

    ![Power BI Azure-tili](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Tässä vaiheessa avulla sallimisesta Power BI-tulostus työpaikan tai oppilaitoksen tiliä. Jos et ole vielä kirjautunut Power BI, valitse Kirjaudu nyt-painiketta. Työpaikan tai oppilaitoksen tiliä, jota käytät Power BI voi olla eri kuin Azure tilaustili, jotka olet kirjautunut sisään.

### <a name="configure-the-power-bi-output-properties"></a>Power BI-tulostus-ominaisuuksien määrittäminen

Kun Power BI-tilin todennus, voit määrittää ominaisuuksia Power BI-tulostusta varten. Seuraavassa taulukossa on ominaisuuden nimien luettelossa ja niiden kuvaukset, voit määrittää Power BI-tuloste.

| Ominaisuuden nimi | Kuvaus |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä ohjaamaan PowerBI-tulostukseen kyselyn tuloksiin. |
| Ryhmän työtila | Käyttöön tietoja voidaan jakaa muiden Power BI-käyttäjien kanssa voit valita ryhmien sisällä Power BI-tilillesi tai valitse "Omat työtilan", jos et halua kirjoittaa ryhmälle.  Aiemmin luodun ryhmän päivittämiseen tarvitaan uudistamalla Power BI-käyttöoikeuden. | 
| Tietojoukon nimi | Tietojoukon nimi halutun käyttämään Power BI-tulostusta varten |
| Taulukkonimi | Anna Power BI-tulostus tietojoukko-kohdassa taulukkonimi. Tällä hetkellä Stream Analytics työt Power BI-tulosteen voi olla vain yksi taulukko tietojoukko |

Katso hallintapaketteihin määrittämisestä Power BI-tulostus ja Raporttinäkymät-ikkunan, on artikkelissa [Azure Stream Analytics ja Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Ei erikseen luoda tietojoukko ja taulukon Power BI-koontinäytön. Tietojoukko ja taulukon täytetään automaattisesti kun työ on alkanut ja projekti alkaa pumppaus tulosteen Power BI. Huomaa, että projektin kyselyä ei luoda tuloksia, jos tietojoukko ja taulukon ei luoda. Huomaa myös, jos Power BI oli jo tietojoukko ja taulukko, jossa on sama nimi kuin säädetty Stream Analytics työn, olemassa olevia tietoja korvataan.

### <a name="renew-power-bi-authorization"></a>Uusi Power BI-todennus

Tarvitset tarkistamiseen Power BI-tilin uudelleen, jos salasana on muuttunut, vaikka työtäsi on luotu tai viimeksi todennettu. Jos multi-factor Authentication (MFA) on määritetty Azure Active Directory (AAD)-vuokraajan sinun on myös uusi Power BI-todennus kahden viikon välein. Tämä ongelma päätyy yleensä on työn tuloksia ja "Tarkista käyttäjän virhe"-toimintoa lokit:

  ![Power BI päivityksen suojaustunnuksen virhe](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Voit ratkaista ongelman pysäyttää käynnissä olevan projektin ja siirry Power BI-tuloste.  "Uusi luvan"-linkki ja Käynnistä työtäsi edellisen pysäytetty kerran tietojen menetyksen välttämiseksi.

  ![Power BI uusia todennus](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Taulukkotallennus

[Azure-taulukkotallennus](../storage/storage-introduction.md) on käytettävissä, erittäin skaalattava tallennustilan niin, että sovelluksen automaattisesti skaalata täyttävän käyttäjän tarvittaessa. Taulukkotallennus on Microsoftin NoSQL avain/määrite säilö yhtä voidaan hyödyntää jäsenneltyjen tietojen ja rakenteen vähemmän rajoitukset. Azure-taulukkotallennus voidaan pysyvyys ja tehokkaan noutaminen tietojen tallentamiseen.

Seuraavassa taulukossa on lueteltu ominaisuuksien nimet ja niiden kuvaukset taulukon tulosteen luomiseen.

| Ominaisuuden nimi | Kuvaus |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä voidaan ohjata tämän taulukon säilöön kyselyn tuloksiin. |
| Tallennustilan tilin | Jos haluat lähettää jaettavaan tallennustilan tilin nimi. |
| Tallennustilan tilin avain | Tallennustilan näkyy tiliin liitetty pikanäppäin. |
| Taulukkonimi | Taulukon nimi. Taulukon Hae luodaan Jos sitä ei ole. |
| Osion avain | Osion avaimen sisältävän esitystapa-sarakkeen nimi. Osion avain on yksilöllinen osion taulukon, joka on yksikön perusavaimen ensimmäinen osa. On merkkijonoarvo, joka voi olla enintään 1 Kilotavun kokoinen. |
| Rivin avain | Rivin avaimen sisältävän esitystapa-sarakkeen nimi. Riviavain on yksilöllinen kohteen annetun-osioon. Se lomakkeiden yksikön perusavaimen toista osaa. Riviavain on merkkijonoarvo, joka voi olla enintään 1 Kilotavun kokoinen. |
| Erän koko | Erän-toiminnolle tietueiden määrän. Yleensä oletusarvo on riittävä useimmissa töiden, voit tarkistaa [taulukon erä-toiminnolla määritys](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) lisätietoja tämän asetuksen muokkaaminen. |

## <a name="service-bus-queues"></a>Palvelun Bus olevien

[Palvelun Bus olevien](https://msdn.microsoft.com/library/azure/hh367516.aspx) tarjoavat ensimmäinen-First Out (FIFO) viestin lähettämisen vähintään yksi kilpailevien kuluttajille. Yleensä viestit odotetaan vastaanotetut ja käsittelee ajallinen järjestyksessä, jossa ne on lisätty jonossa ja viesti on vastaanotettu ja käsitellä vain yhden viestin kuluttaja vastaanottajia.

Alla olevassa taulukossa on lueteltu ominaisuuksien nimet ja kuvaukset jonon tulosteen luomiseen.

| Ominaisuuden nimi | Kuvaus |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä ohjaamaan palvelun Bus jonon kyselyn tuloksiin. |
| Palvelun Bus Namespace | Palvelun Bus nimitila on joukko messaging kohteiden säilö. |
| Jonon nimi | Palvelun Bus jonossa nimi. |
| Jonon käytännön nimi | Kun luot jonon, voit myös luoda jaettuun käyttöön käytännöt jonon määrittäminen-välilehdessä. Kukin jaetun access-käytäntö on nimi määritetyt käyttöoikeudet, ja valintanäppäimet. |
| Jonon käytännön avain | Jaettu pikanäppäin todennetaan pääsy palvelun Bus nimitila |
| Tapahtuman Sarjatoiminto muoto | Sarjatoiminto tulosteen tietojen muoto.  JSON, CSV ja Avro tuetaan. |
| Koodaus | CSV-ja JSON UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä |
| Erottimen | Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoittamista tiedot CSV-muodossa. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki. |
| Muotoile | Käytettävissä vain JSON tyyppi. Erotettu viiva osoittaa, että tulos muotoiltu siten, että kunkin uuden rivin erotettu JSON-objektin. Matriisin määrittää tulosteen muotoillaan JSON objektien matriisina. |

## <a name="service-bus-topics"></a>Palvelun Bus aiheita

Vaikka palvelun Bus olevien yksi yhteen yhteydenottotapa lähettäjän ja vastaanottajan välisen, [Palvelun Bus ohjeaiheissa](https://msdn.microsoft.com/library/azure/hh367516.aspx) on yksi-moneen-yhteys lomakkeen.

Seuraavassa taulukossa on lueteltu ominaisuuksien nimet ja niiden kuvaukset taulukon tulosteen luomiseen.

| Ominaisuuden nimi | Kuvaus |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tunnus | Tämä on kutsumanimi, jota käytetään kyselyissä ohjaamaan palvelun Bus ohjeaihe kyselyn tuloksiin. |
| Palvelun Bus Namespace | Palvelun Bus nimitila on joukko messaging kohteiden säilö. Kun olet luonut uuden tapahtuman-keskittimeen, luomasi myös palvelun Bus nimitila |
| Aiheen nimi | Aiheet ovat messaging tapahtuman keskittimet ja olevien kohteiden. Ne on suunniteltu tapahtuman virtaa kerätään useita eri laitteissa ja palvelut. Aiheen luomisen jälkeen se annetaan myös tiettyjen nimi. Aiheen lähetetyt viestit eivät ole käytettävissä, paitsi jos tilaus on luotu, joten varmista, että vähintään yksi tilaukset, valitse haluamasi aihe |
| Aiheen käytännön nimi | Kun luot aiheen, voit myös luoda jaetun käytäntöjen aiheen määrittäminen-välilehdessä. Kukin jaetun access-käytäntö on nimi määritetyt käyttöoikeudet, ja valintanäppäimet |
| Aiheen käytännön avain | Jaettu pikanäppäin todennetaan pääsy palvelun Bus nimitila |
| Tapahtuman Sarjatoiminto muoto | Sarjatoiminto tulosteen tietojen muoto.  JSON, CSV ja Avro tuetaan. |
| Koodaus | Jos CSV- tai JSON Muotoile koodauksen on määritetty. UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä |
| Erottimen | Käytettävissä vain CSV Sarjatoiminto. Virta Analytics tukee useita yleisiä erottimet sarjoittamista tiedot CSV-muodossa. Tuetut arvot ovat pilkku, puolipiste, tila, välilehden ja pystysuora palkki. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on täysin hallitun NoSQL asiakirjan tietokannan palvelu, joka tarjoaa kyselyn ja tapahtumat rakenteen tiedot, ennakoitavissa ja luotettava suorituskykyä ja nopeasti.

Seuraavassa taulukossa on lueteltu ominaisuuksien nimet ja niiden kuvaukset DocumentDB tulosteen luomiseen.

<table>
<tbody>
<tr>
<td>OMINAISUUDEN NIMI</td>
<td>KUVAUS</td>
</tr>
<tr>
<td>Tilin nimi</td>
<td>DocumentDB-tilin nimi.  Tämä on myös tilin päätepiste.</td>
</tr>
<tr>
<td>Tilin avain</td>
<td>DocumentDB tilin jaetun pikanäppäin.</td>
</tr>
<tr>
<td>Tietokannan</td>
<td>DocumentDB tietokannan nimi.</td>
</tr>
<tr>
<td>Sivustokokoelman nimi kuvio</td>
<td>Sivustokokoelman nimi kuviota, jota käytetään loppu. Sivustokokoelman nimen muoto on rakennettava käyttämällä valinnainen {osion}-tunnuksen, mistä osioiden aloittaa 0.<BR>Esimerkiksi Kun se on julkaistu ovat kelvollisia syötteiden:<BR>MyCollection {osion}<BR>MyCollection<BR>Huomaa, että sivustokokoelmat on oltava olemassa ennen Stream Analytics-työ on käynnissä ja ei luoda automaattisesti.</td>
</tr>
<tr>
<td>Osion avain</td>
<td>Tulosteen tapahtumat määritetään näppäintä jakaminen tulosteen yli sivustokokoelmat kentän nimi.</td>
</tr>
<tr>
<td>Tiedostotunnisteen</td>
<td>Määrittää perusavaimen tulosteen tapahtumia, joka lisää tai Päivitä toiminnot kentän nimen perusteella.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet
Sinun on otettu käyttöön Stream Analytics hallitun palvelun streaming analytics asioita Internet-tietojen perusteella. Lisätietoja tämän palvelun on seuraavissa artikkeleissa:

- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
