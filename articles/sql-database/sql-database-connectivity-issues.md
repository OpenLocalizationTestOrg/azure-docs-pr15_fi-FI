<properties
    pageTitle="Korjaa yhteyden SQL-virhe, virhe lyhytkestoisia | Microsoft Azure"
    description="Opettele liittyviä ongelmia, diagnosointi ja estää SQL-yhteysvirhe tai lyhytkestoisia virhe Azure SQL-tietokantaan. "
    keywords="SQL-yhteys, yhteysmerkkijonon, yhteysongelmat, lyhytkestoisia virhe, yhteysvirhe"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Vianmääritys, diagnosointi ja estää SQL yhteyden virheet ja lyhytkestoisia virheet SQL-tietokantaan

Tässä artikkelissa käsitellään estää, vianmääritys, diagnosointi ja pienentämään yhteyden virheitä sekä lyhytkestoisia asiakassovellus ilmenee, kun se toimii Azure SQL-tietokannan kanssa virheet. Lue, miten voit määrittää uudelleen logiikan, Luo yhteysmerkkijono ja muiden yhteyden muuttaminen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Lyhytkestoisia virheiden lyhytkestoisia

Lyhytkestoisia virhe - myös lyhytkestoisia vika - on pohjana syy, pian ratkaisee itse. Satunnainen syy lyhytkestoisia virheitä on, kun Azure järjestelmän nopeasti siirtää laitteistoresurssien kuormituksen-tasapainoa eri toiminnoista. Useimmat kokoonpanon uudelleenmääritys näitä tapahtumia valmiiksi usein alle 60 sekuntia. Kokoonpanon uudelleenmääritys tämän ajanjakson aikana voi olla yhteysongelmia Azure SQL-tietokantaan. Sovellusten Azure SQL-tietokannan muodostaminen olisi muodostettu toiminta lyhytkestoisia virheet, käsitellä niitä käyttämällä uudelleen logiikan niiden koodilla sijaan pinnoituksen ne käyttäjät sovellusvirheet.

Jos asiakasohjelma käyttää ADO.NET-ohjelmaa on neuvonut lyhytkestoisia virheestä **SqlException**throw. **Luku** -ominaisuuden niitä verrataan luettelon lyhytkestoisia ohjeaiheen yläreunassa: [SQL-virhekoodit asiakassovellukset SQL-tietokantaan](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Yhteyden ja komento

Yhteys SQL-yhteyden uudelleen tai vahvistaa sen uudelleen mukaan seuraavasti:

* **Yhteyden Kokeile tapahtuu lyhytkestoisia virhe**: yhteys yritetään jälkeen viivästyy muutaman sekunnin ajan.

* **SQL-kysely-komennon tapahtuu lyhytkestoisia virhe**: komento ei ole yritetään heti. Sen sijaan viiveen jälkeen, pitäisi olla juuri muodostaa yhteyttä. Komento voi sitten uudelleen.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Yritä logiikan lyhytkestoisia virheet


Asiakasohjelmia, ilmenee joskus lyhytkestoisia virhe ovat tehokkaamman, jos ne sisältävät uudelleen logiikan.


Kun ohjelma on yhteydessä Azure SQL-tietokanta 3 osapuolen middleware kautta, Kohdista toimittajan kanssa, onko middleware sisältää uudelleen logiikan lyhytkestoisia virheet.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Yritä periaatteet


- Yritetään avata yhteyden yritetään, jos virhe on lyhytkestoisia.


- SQL SELECT-lause, joka epäonnistuu lyhytkestoisia virheen yritetään ei suoraan.
 - Ajan tasalla yhteyden ja yritä sitten uudelleen Valitse.


- Kun SQL UPDATE-komento epäonnistuu lyhytkestoisia virheen, uuden yhteyden vahvistettava, ennen kuin päivitys yritetään.
 - Yritä logiikan on varmistettava, että koko tietokanta valmis tapahtuman tai jotka koko tapahtuma on peruutettu.


#### <a name="other-considerations-for-retry"></a>Muita huomioon otettavia seikkoja uudelleen


- Komentotiedosto, joka käynnistyy automaattisesti työtuntien jälkeen ja ennen aamu, joka kestää varaa hyvin potilaan kauan sen uudelleenyritykset välisten aikavälien kanssa.


- Käyttäjän käyttöliittymän ohjelman olisi huomioon ihmisten taipumusta tarttua toisiinsa antamaan liian pitkä Odota jälkeen.
 - Kuitenkin ratkaisun ei saa olla uudelleen muutaman sekunnin välein, koska käytäntö voi peitetään järjestelmän pyynnöt.


#### <a name="interval-increase-between-retries"></a>Aikavälin Suurenna uudelleenyritykset välillä



On suositeltavaa, että olet viivyttää 5 sekunnin ajan ennen ensimmäisen uudelleen. Yritetään lyhyempi 5 sekuntia riskien overwhelming pilvipalvelussa sijaitsevaan viiveen jälkeen. Kunkin myöhemmin uudelleen viive olisi eksponentiaalisesti enintään 60 sekuntia.

Keskustelun *estäminen kauden* asiakkaiden, jotka käyttävät ADO.NET sisältyy [SQL Server-yhteyden jakavan (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Voit myös määrittää uudelleenyritykset enimmäismäärä, ennen kuin ohjelma lopettaa itse.


#### <a name="code-samples-with-retry-logic"></a>MALLIKOODEJA ja yritä logiikka


MALLIKOODEJA, yritä logiikan sekä ohjelmoinnin kielet, jossa on osoitteessa:

- [SQL-tietokantaan ja SQL Serverin kirjastot](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testaa uudelleenyrityslogiikka


Voit esikatsella uudelleen logiikan-simuloida tai aiheuttaa virheen kuin voidaan korjata samalla, kun ohjelma on käynnissä.


##### <a name="test-by-disconnecting-from-the-network"></a>Testaa katkaisemalla verkossa


Voit testata uudelleen logiikan yksi tapa on katkaista verkon asiakastietokoneen, ohjelma on käynnissä. Virhe on:

- **SqlException.Number** = 11001
- Sanoma: "ei ole isäntää"


Osana uudelleen ensimmäisellä kerralla ohjelmasi voit korjata väärin kirjoitetut ja muodostaa yhteys.


Saavat tämän käytännön irrota tietokoneen verkosta, ennen kuin käynnistät ohjelman. Valitse ohjelma tunnistaa suorituksen aikana parametri, joka käynnistää ohjelman:

1. Lisää tilapäisesti 11001 lyhytaikainen katsottava virheitä luetteloa.
2. Yritä sen ensimmäisen yhteyden tavalliseen tapaan.
3. Kun virhe on pyydettyjen poistaminen luettelosta 11001.
4. Näytä viesti, jossa käyttäjää pyydetään kytke tietokone verkkoon.
 - Keskeytä edelleen suorittamisen käyttämällä **Console.ReadLine** -menetelmää tai keskustelun OK-painiketta. Käyttäjä painaa Enter-näppäintä jälkeen verkon kytketty tietokoneeseen.
5. Yritä uudelleen muodostaa, odotetaan onnistui.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Tietokannan nimi on kirjoitettu väärin muodostettaessa testaaminen


Ohjelman voi tarkoituksellisesti sivun reunaan väärin ennen ensimmäisen yhteysyritykseen käyttäjänimi. Virhe on:

- **SqlException.Number** = 18456
- Sanoma: "Kirjaudu käyttäjän 'WRONG_MyUserName' epäonnistui."


Osana uudelleen ensimmäisellä kerralla ohjelmasi voit korjata väärin kirjoitetut ja muodostaa yhteys.


Saavat tämän käytännön, ohjelma voi tunnistaa suorituksen aikana parametri, joka käynnistää ohjelman:

1. Lisää tilapäisesti 18456 lyhytaikainen katsottava virheitä luetteloa.
2. Lisää tarkoituksellisesti sivun reunaan 'WRONG_' käyttäjänimi.
3. Kun virheen pyydettyjen, poistaa 18456 luettelosta.
4. Poista 'WRONG_' käyttäjänimi.
5. Yritä uudelleen muodostaa, odotetaan onnistui.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection parametrit yhteyden uudelleen


Jos asiakasohjelma Luo Azure SQL-tietokantaan käyttämällä .NET Framework luokan **System.Data.SqlClient.SqlConnection**, sinun on käytettävä .NET 4.6.1 tai uudempi versio, jotta voit hyödyntää sen yhteyden uudelleen-toiminto. Tietoja toiminnon [tähän](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Kun luot [yhteysmerkkijonon](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) **SqlConnection** objektin-olisi järjestämiseen seuraavat parametrit arvojen määrän laskeminen:

- ConnectRetryCount &nbsp; &nbsp; *(oletusarvo on 1. Alue on 0 – 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(oletusarvo on 1 sekunti. Alue on 1 – 60.)*
- Yhteyden aikakatkaisu &nbsp; &nbsp; *(oletusarvo on 15 sekunnin kuluttua. Alue on 0-2147483647)*


Tarkemmin sanottuna valitut arvot varmista seuraavat tasa tosi:

- Yhteyden aikakatkaisu = ConnectRetryCount * ConnectionRetryInterval

Esimerkiksi jos määrä = 3 ja väli = 10 sekuntia aikakatkaisu, vain 29 sekuntia ei aivan helmi järjestelmän riittävästi aikaa sen 3 ja lopullinen uudelleen milloin yhteyden: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Yhteyden ja komento


**ConnectRetryCount** ja **ConnectRetryInterval** parametrien avulla Muodosta uudelleen ilman kertoo tai bothering ohjelmaa, kuten ohjelmaan, joka palauttaa ohjausobjektin **SqlConnection** objekti. Uudelleenyritykset voi ilmetä seuraavissa tilanteissa:

- mySqlConnection.Open menetelmäkutsu
- mySqlConnection.Execute menetelmäkutsu

Ei subtlety. Jos lyhytkestoisia virhe ilmenee, kun *kysely* suoritetaan, **SqlConnection** objektin uudelleen Yhdistä-toiminto ei ja sen varmasti ei yritä kyselyn. Kuitenkin **SqlConnection** tarkistaa nopeasti yhteyden ennen lähettämistä kyselyn suorittamista varten. Jos nopeasti Tarkista havaitsee yhteyden ongelman, **SqlConnection** yrittää suorittaa Yhdistä-toiminnon uudelleen. Jos yritä onnistuu, voit kysely lähetetään suorittamista varten.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Olisi ConnectRetryCount voidaan yhdistää sovelluksen uudelleen logiikkaa?

Oletetaan, että sovelluksesi on tehokkaat mukautetun uudelleen logiikan. Se saattaa uudelleen Yhdistä-toiminnon 4 kertaa. Jos lisäät **ConnectRetryInterval** ja **ConnectRetryCount** = 3 yhteysmerkkijono, voit kasvattaa 4 * 3 = 12 uudelleen laskeminen uudelleenyritykset. Haluat ehkä ei on suuri, uudelleenyritykset luku.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Yhteydet Azure SQL-tietokantaan

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Yhteyden: Yhteysmerkkijonon


Tarvittavat Azure SQL-tietokannan muodostaminen yhteysmerkkijonon poikkeaa hieman merkkijono, yhteyden muodostaminen Microsoft SQL Server. Voit kopioida yhteysmerkkijonon tietokannan [Azure-portaalissa](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Yhteys: IP-osoite


Sinun on määritettävä SQL-tietokantapalvelimeen, Hyväksy tietoliikenne IP-osoite, tietokone, jossa asiakas-ohjelmaa. Voit tehdä tämän muokkaamalla palomuuriasetukset [Azure-portaalin](https://portal.azure.com/)kautta.


Jos olet unohtanut Määritä IP-osoite, ohjelma epäonnistuu kätevää virhesanoma, joka ilmoittaa edellyttämät IP-osoite.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Lisätietoja on artikkelissa: [Toimintaohje: SQL-tietokantaan palomuurin asetusten määrittäminen](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Yhteyden: portit


Yleensä tarvitset vain porttia 1433 ovat avoinna lähtevän tietoliikenteen tietokoneessa, joka isännöi asiakasohjelmassa.


Esimerkiksi asiakasohjelman nykyisessä Windows-tietokoneessa, Windowsin palomuuri isännän avulla voit avata porttia 1433:


1. Avaa Ohjauspaneeli
2. &gt;Kaikki Ohjauspaneelin kohteet
3. &gt;Windowsin palomuuri
4. &gt;Lisäasetukset
5. &gt;Lähtevän liikenteen säännöt
6. &gt;Toiminnot
7. &gt;Uusi sääntö


Jos asiakasohjelman isännöidään Azure virtuaalikoneen (AM), lue:<br/>[Lisäksi ADO.NET 4.5 1433 portit ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Lisätietoja taustan cofiguration portit ja IP-osoite-kohdassa: [Azure SQL-tietokantaan palomuurin](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>-Yhteys: ADO.NET 4.6.1


Jos ohjelma käyttää ADO.NET-luokat, kuten **System.Data.SqlClient.SqlConnection** muodostaa Azure SQL-tietokantaan, on suositeltavaa, että käytät .NET Framework-versiota 4.6.1 tai sitä uudemmassa versiossa.


ADO.NET 4.6.1:

- Azure SQL-tietokanta on parannettu luotettavuus: avattaessa yhteyden **SqlConnection.Open** -menetelmällä. **Open** -menetelmää kattaa nyt parhaat työmäärään uudelleen järjestelmiä vastauksen lyhytkestoisia virheitä, tietyille yhteyden ajassa virheitä.
- Tukee yhteysvarannon. Tämä sisältää tehokas vahvistusta, joka toimi connection-objekti, ohjelma antaa.



Käytettäessä connection-objekti yhteysvarannon, on suositeltavaa, että ohjelma Sulje yhteys tilapäisesti käytettäessä ei ole heti sen. Avaamalla se uudelleen yhteyden ei ole kallista on tapa luoda uusi yhteys.


Jos käytössäsi on ADO.NET 4.0 tai sitä aiemmissa versioissa, on suositeltavaa, että päivität uusimman ADO.NET.

- Vuodesta marraskuun 2015 voit [ladata ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Vianmääritys

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Vianmääritys: Testaa, onko apuohjelmien muodostaa


Jos ohjelma epäonnistui tuntemattomasta muodostaa Azure SQL-tietokanta, yksi diagnostiikan vaihtoehto on Yritä yhdistää apuohjelma-ohjelman kanssa. Ihannetapauksessa apuohjelman yhteyden samassa kirjastossa, ohjelma käyttää avulla.


Missä tahansa Windows-tietokoneessa kokeile näiden apuohjelmien:

- SQL Server Management Studiossa (ssms.exe), joka yhdistää käyttämällä ADO.NET.
- SQLCMD.exe, joka yhdistää käyttämällä [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Kun yhteys on muodostettu, Testaa, onko lyhyt SQL SELECT query toimii.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Vianmääritys: Tarkista avoimet portit


Oletetaan, että epäilet, että yhteys yritykset epäonnistuvat portin ongelmien vuoksi. Tietokoneen voit suorittaa apuohjelma, joka raportoi porttimäärityksiä.


Linux seuraavat apuohjelmat voi olla hyötyä:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Muuta Esimerkkiarvo, joka on IP-osoite).


Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) apuohjelma voi olla hyötyä. Tässä on esimerkki-suorittamisen, joka suorittaa kysely portin tilanne Azure SQL-tietokanta-palvelimessa ja joka suoritettiin kannettavassa tietokoneessa:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Vianmääritys: Kirjaudu virheet


Katkonainen ongelma joskus parhaiten diagnosoidaan mukaan Yleiset kuvion tunnistaminen päivinä tai viikkoina päälle.


Asiakas voi auttaa vianmäärityksen kirjautumalla käsittelee kaikki virheet. Voit ehkä yhdistää ne tapahtumien virhe tiedoilla, jotka Azure SQL-tietokanta kirjautuu itse sisäisesti.


Yrityksen kirjaston 6 (EntLib60) on .NET hallitun luokkien helpottamiseksi kirjaaminen:

- [5 - yhtä helppoa kuin kuuluvien käytöstä lokia: kirjaamisen sovelluksen eston avulla](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Vianmääritys: Tarkastele järjestelmälokit virheet


Seuraavassa on joitakin Transact-SQL SELECT-lauseesta virheen, että kyselylokit ja muita tietoja.


| Kyselyn lokin | Kuvaus |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | [Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) näkymä on tietoja tapahtumista, kuten joitakin, jotka voivat aiheuttaa virheitä lyhytkestoisia tai yhteys epäonnistuu.<br/><br/>Ihannetapauksessa voi yhdistää **start_time** tai **end_time** arvot tiedoilla, kun asiakasohjelman ilmeni ongelmia.<br/><br/>**Vihje:** Muodostamalla yhteyden **perusmuodon** tietokannan suoritettava toiminto. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | [Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) näkymä on tapahtumatyypit, Lisää diagnostiikka koostetun määrät.<br/><br/>**Vihje:** Muodostamalla yhteyden **perusmuodon** tietokannan suoritettava toiminto. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Vianmääritys: SQL-tietokantaan lokin tapahtumat ongelman etsiminen


Voit etsiä tietoja Azure SQL-tietokannan lokin tapahtumat ongelma. Kokeile **perusmuodon** tietokannan seuraavan Transact-SQL SELECT-lauseen:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Muutama sys.fn_xe_telemetry_blob_target_read_file palautetut rivit


On seuraava mitä palautetut rivin voi näyttää esimerkiksi tältä. Null arvot eivät usein ole null muiden rivien.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Yrityksen kirjaston 6


Yrityksen kirjaston 6 (EntLib60) on kehys .NET luokkien, jonka avulla voit toteuttaa tehokkaat asiakkaiden pilvipalveluihin, joista toinen on Azure SQL-tietokanta-palvelu. Voit etsiä omistautunut kullekin alueelle, jossa EntLib60 auttaa ensimmäisen osuutesi aiheita:

- [Yrityksen kirjaston 6 – huhtikuussa 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Yritä logiikan lyhytkestoisia virheiden käsittelyä varten on yksi alue, jossa EntLib60 auttaa:

- [4 - perseverance, kaikki saavutuksensa salaisuus: lyhytkestoisia virheen käsittely-sovelluksen eston avulla](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] EntLib60 lähdekoodi on käytettävissä julkisen [Lataa](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft ei ole suunnitelmia halutaan edelleen toimintojen päivitykset ja ylläpito päivitykset EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 luokkien lyhytkestoisia virheet ja yritä uudelleen


Seuraavat EntLib60 luokat ovat erityisen hyödyllisiä uudelleen logiikan. Kaikki näihin ovat tai ovat edelleen nimitilan **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**‑kohdassa:

*Valitse nimitila* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** luokka
 - **ExecuteAction** menetelmä


- **ExponentialBackoff** luokka


- **SqlDatabaseTransientErrorDetectionStrategy** luokka


- **ReliableSqlConnection** luokka
 - **ExecuteCommand** menetelmä


Nimitilan **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** luokka

- **NeverTransientErrorDetectionStrategy** luokka


Seuraavassa on linkkejä EntLib60 tietoihin:

- Vapaa [kirjan Lataa: kehittäjän opas Microsoft Enterprise-kirjastoon, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Parhaat käytännöt: [Yritä yleisohjeita](../best-practices-retry-general.md) on erinomainen perusteellisempaa keskustelun, yritä logiikan.

- [Yrityksen kirjasto - sovelluksen estä lyhytkestoisia vika käsittely 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) NuGet lataaminen

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Kirjaamisen estäminen


- Estä kirjaaminen on erittäin joustava ja määritettävä ratkaisu, jonka avulla voit:
 - Luoda ja tallentaa lokin viestejä useisiin sijainteihin.
 - Voit luokitella ja suodattaa viestit.
 - Kerää tilannekohtaiset tietoja, jotka on hyötyä virheenkorjaus ja jäljitys sekä tarkistaminen ja Yleiset kirjaaminen vaatimuksia.


- Lokiin kirjaaminen-lohko käsittelee kohteesta lokiin kirjaaminen-toimintoja, niin, että sovellus-koodi on yhtenäinen riippumatta sijaintiin ja kohde kirjaaminen säilön laji.


Lisätietoja: [5 - kuin helppoa kuin kuuluvien käytöstä loki: lokiin kirjaaminen-sovelluksen eston avulla](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient menetelmä-lähdekoodi


Seuraavaksi **SqlDatabaseTransientErrorDetectionStrategy** -luokasta on C#-lähdekoodi, **IsTransient** -menetelmää. Lähdekoodin selvittää, mitkä virheet on pidetään lyhytkestoisia ja worthy, yritä saavuttaman huhtikuussa 2013.

Monia **//comment** rivit on poistettu tästä korostaa luettavuutta.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Seuraavat vaiheet

- Muita yleisiä Azure SQL-tietokannan yhteys-ongelmien vianmääritys käy [yhteysongelmien vianmääritys Azure SQL-tietokantaan](sql-database-troubleshoot-common-connection-issues.md).

- [SQL Server yhteysvarannon (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Retrying* on Apache 2.0 käyttöoikeus kirjoitettu **Python**, yritä toiminta lisätään miltei mitä tahansa yksinkertaistamaan yritetään yleinen kirjastossa.](https://pypi.python.org/pypi/retrying)
