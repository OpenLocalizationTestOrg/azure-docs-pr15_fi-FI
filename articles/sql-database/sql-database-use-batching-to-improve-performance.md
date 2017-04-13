 <properties
    pageTitle="Opi käyttämään Azure SQL-tietokanta-sovelluksen suorituskyvyssä jonottaminen"
    description="Aiheessa näyttöä, batching tietokannan toimintojen huomattavasti imroves nopeuden ja skaalattavuus Azure SQL-tietokanta-sovelluksista. Vaikka batching näistä menetelmistä sovellu minkä tahansa SQL Server-tietokantaan, artikkelissa keskitytään Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Voit käyttää SQL-tietokanta-sovelluksen suorituskyvyssä jonottaminen

Jonottaminen Azure SQL-tietokanta toimintojen merkittävästi parantaa suorituskyky ja skaalattavuus sovellustesi. Jotta ymmärrät etuja, ensimmäinen osa on tämän artikkelin käsitellään joitakin otoksen testitulokset, joka vertaa peräkkäiset ja oletusmäärä pyynnöt SQL-tietokantaan. Artikkelin loput näyttää tekniikoita, skenaariot ja huomioon otettavia seikkoja, jotta voit käyttää jonottaminen onnistuneesti Azure-sovelluksissa.

**Tekijät**: Jason Roth, Silvano Coriani Trent Swanson (asteikon 180 Inc.)

**Tarkistajat**: Conor CUNNINGHAM puheenjohtaja, Esko Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Miksi on jonottaminen tärkeitä SQL-tietokantaan?
Tunnettujen strategia suorituskyky ja skaalattavuus on jonottaminen puhelut remote-palveluun. On korjattu minkä tahansa aktiviteettien remote palvelun, kuten Sarjatoiminto, Verkkosiirto ja sarjoituksen käsittelyn kustannukset. Nämä kustannukset pakkaamista useita eri tapahtumia yksittäisen erään pienentää.

Tässä asiakirjassa haluamme tutkia eri jonottaminen strategioita ja skenaarioiden SQL-tietokantaan. Vaikka nämä strategiat ovat myös tärkeitä paikallisen sovelluksissa, jotka käyttävät SQL Server-on useita syitä korostaminen käyttö jonottaminen SQL-tietokantaan:

- On mahdollisesti suurempi verkon latenssin käyttäminen SQL-tietokantaan, etenkin silloin, kun käytät SQL-tietokannan samaan Microsoft Azure-palvelinkeskuksen ulkopuolella.
- SQL-tietokantaan multitenant ominaispiirteet tarkoittaa, että tietojen käytön kerroksen numeroidulla tietokannan yleinen skaalattavuus tehokkuutta. SQL-tietokanta on mikä tahansa yksittäinen vuokraajan/käyttäjä estää yksin muiden omistajien haittaa tietokannan resursseja. Vastauksena yli ennalta määritettyjä kiintiön käyttö SQL-tietokantaan voit vähentää siirtonopeuden tai vastaa rajoittimen poikkeukset. Ota käyttöön tehokkuus, kuten jonottaminen, sinua tekemään enemmän työtä SQL-tietokanta ennen saapumista nämä raja-arvot. 
- Jonottaminen on myös tehokas arkkitehtuureihin, jotka käyttävät useiden tietokantojen (sharding). Tietokannan kunkin yksikön yhteytesi tehokkuutta on edelleen yleinen skaalattavuus avainasemassa. 

SQL-tietokantaan eduista on, että sinun ei tarvitse hallita tietokannan isännöivät palvelimet. Tämä hallitun infrastruktuurin myös tarkoittaa kuitenkin, että sinulla on mietittävä tietokannan optimointi eri tavalla. Voit tarkistaa enää parantaa tietokannan tai verkko-infrastruktuuria. Microsoft Azure hallitsee näiden ympäristössä. Tärkeimmät alue, jolla voit hallita on, miten sovelluksesi toimii SQL-tietokantaan. Jonottaminen on jokin näistä optimointi. 

Paperin alkuosa käy läpi eri batching tekniikoita .NET-sovelluksissa, jotka käyttävät SQL-tietokantaan. Kaksi osien kansilehden jonottaminen ohjeita ja skenaarioita.

## <a name="batching-strategies"></a>Strategioita jonottaminen

### <a name="note-about-timing-results-in-this-topic"></a>Ajoitus tulokset tämän ohjeaiheen koskeva huomautus
>[AZURE.NOTE] Tulokset eivät ole viitearvoja, mutta se on tarkoitus esittää **suhteellinen suorituskykyä**. Ajoitukset perustuvat keskiarvo, vähintään 10 testi suoritetaan. Toiminnot ovat Lisää tyhjään taulukkoon. Näiden testien oli mitattu pre-Vipuventtiili V12 ja ne eivät välttämättä ole vastaa siirtonopeuden, joita voi ilmetä Vipuventtiili V12 tietokanta käyttämällä uusi [palvelutasot](sql-database-service-tiers.md). Suhteellinen etuna batching tekniikka pitäisi olla samanlainen.

### <a name="transactions"></a>Tapahtumat
Tuntuu outo Aloita lukemalla jonottaminen mukaan keskustella tapahtumat. Mutta asiakkaan tapahtumat käyttö on muotoiltu palvelinpuolen batching vaikutus, joka parantaa suorituskykyä. Ja tapahtumia voidaan lisätä vain muutaman viivoilla koodi, jotta ne ovat nopea tapa peräkkäiset toiminnot suorituskyvyn parantamiseksi.

Ota huomioon seuraavat C# koodi, joka sisältää sarjan lisääminen ja päivittäminen yksinkertaisen taulukon toimintoja.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Seuraava ADO.NET-koodi suorittaa peräkkäin näistä toiminnoista.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Paras tapa optimoida koodi on toteuttamisesta joitakin muodossa asiakkaan jonottaminen, puhelut. Mutta parannettavaa on helppo tapa suurentaa koodi suorituskyvyn rivitys riittää, että puhelut järjestyksessä tapahtuman. Seuraavassa on sama koodi, joka käyttää tapahtuma.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Tapahtumia käytetään todella molempien esimerkit. Ensimmäisessä esimerkissä jokainen yksittäisiä kutsu on implisiittinen tapahtuma. Toinen esimerkki eksplisiittisiä tapahtuman rivittää kaikki kutsut. Kohti ohjeissa [Kirjoita eteenpäin tapahtumaloki](https://msdn.microsoft.com/library/ms186259.aspx)tietueet on tyhjennetty levy, kun tapahtuman vahvistaa. Niin sisällyttämällä tulevat puhelut tapahtuman lokiin kirjoitus voi viivästyä kunnes tapahtuma on vahvistettu. Haluat ottaa käyttöön, saat kirjoittaa palvelimen tapahtumaloki jonottaminen.

Seuraavassa taulukossa on joitakin itsenäisen testauksen tuloksia. Testejä suorittaa saman peräkkäisiä Lisää kanssa ja ilman tapahtumia. Saat enemmän perspektiivi testien ensimmäisiin suoritettiin etäyhteyden kannettavasta Microsoft Azure-tietokantaan. Toisen joukon testejä suoritettiin pilvipalvelussa ja tietokannan sekä asunut sisällä samaa Microsoft Azure-palvelinkeskuksen (Länsi US). Seuraavassa taulukossa keston millisekunteina peräkkäisiä Lisää kanssa ja ilman tapahtumia.

**Paikalliseen Azure avulla**:

| Toiminnot | Tapahtumaa (ms) | Tapahtuman (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1 000 | 128852 | 102917 |


**Azure-Azure (sama palvelinkeskuksen)**:

| Toiminnot | Tapahtumaa (ms) | Tapahtuman (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1 000 | 21479 | 2756 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

Perusteella edellisen testitulokset todella rivitys yhdellä kertaa tapahtuman vähenee suorituskykyä. Mutta kun lisäät yhden tapahtumasta toimintoja, suorituskyvyn parantaminen tulee Lisää merkitty. Suorituskyvyn ero on myös helpommin huomattavia, kaikki toiminnot toteutuessa kuluessa Microsoft Azure-palvelinkeskukseen. Parannettu viive SQL-tietokannan käyttäminen Microsoft Azure-palvelinkeskuksen ulkopuolella overshadows suorituskyky-voitto käytön tapahtumat.

Jos tapahtumat käyttö voi parantaa suorituskykyä, [Noudata tapahtumia ja yhteydet parhaat käytännöt](https://msdn.microsoft.com/library/ms187484.aspx)edelleen. Säilyttää mahdollisimman lyhyt ja sulje tietokantayhteys, kun työ on valmis. Lauseella edellisessä esimerkissä varmistaa, että yhteys on suljettu, kun seuraavat koodin esto on valmis.

Edellisessä esimerkissä, että voit lisätä minkä tahansa ADO.NET-koodia, jossa on kaksi riviä paikallinen tapahtuma. Tapahtumien tarjouksen nopeasti koodi, joka tekee peräkkäisiä Lisää, Päivitä ja poista toimintojen suorituskyvyn parantamiseksi. Kuitenkin nopein suorituskyvyn, harkitse koodin lisäksi asiakkaan jonottaminen, kuten taulukkoarvoisten parametrit hyödyntää muuttamista.

Saat lisätietoja ADO.NET-tapahtumat [Paikallisen ADO.NET-tapahtumat](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Taulukkoarvoiset parametrit
Taulukkoarvoiset parametrit tukevat käyttäjän määrittämän taulukkotyypit parametreiksi Transact-SQL-lauseita, tallennettujen toimintosarjojen ja funktioita. Batching asiakkaan tällä menetelmällä voit lähettää taulukkoarvoisten-parametrin tietojen useita rivejä. Taulukkoarvoiset parametrien käyttäminen ensin määritettävä taulukko. Seuraava Transact-SQL-lause luo taulukon nimeltä **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

Koodi voit luoda **arvotaulukko** täsmälleen samat nimet ja lajien taulukon tyyppi. Välitä tämä **arvotaulukko** tekstin kyselyn parametri tai tallennetun toimintosarjakutsu. Seuraavassa esimerkissä esitetään, tällä menetelmällä:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

Edellisen esimerkin **SqlCommand** -objekti Lisää rivejä taulukkoarvoisten parametrin **@TestTvp**. Aiemmin luodun **arvotaulukko** objekti liitetään parametriä **SqlCommand.Parameters.Add** -menetelmällä. Suorituskyvyn jonottaminen yhden kutsussa Lisää merkittävästi kasvaa peräkkäisiä Lisää päälle.

Voit parantaa edelleen edellisessä esimerkissä, käytä tallennetun toimintosarjan tekstipohjainen-komennon sijaan. Seuraava Transact-SQL-komento luo tallennetun toimintosarjan, **SimpleTestTableType** taulukkoarvoisten-parametri.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Muuta seuraava koodi esimerkkiraportin **SqlCommand** objekti-ilmoitus.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

Useimmissa tapauksissa vastaavat tai parempi suorituskyky kuin muita batching menetelmiä on taulukkoarvoisten parametrit. Taulukkoarvoiset parametrit ovat usein suositeltavampaa, koska ne ovat joustavampi kuin muut asetukset. Muita tapoja, kuten SQL joukkona kopioi sallivat esimerkiksi ainoastaan uusien rivien lisääminen. Mutta taulukkoarvoisten parametreilla voit käyttää logiikan tallennetun toimintosarjan voit selvittää, mitkä rivit ovat päivitykset ja jotka ovat Lisää. Taulun tyypin voi muokata myös oltava "Toiminto"-sarake, joka ilmaisee, onko määritetyn rivin pitäisi olla lisätään, päivitetään tai poistetaan.

Seuraavassa taulukossa ad-hoc taulukkoarvoisten parametrien käytön testitulokset millisekunteina.

| Toiminnot | Paikalliseen, Azure (ms)  | Azure saman palvelinkeskuksen (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1 000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

Suorituskyky-voitto-jonottaminen on heti näennäinen. Edellisen peräkkäisiä kokeen 1000 toiminnot on tapahtunut 129 sekuntia sen palvelinkeskuksen ulkopuolella ja 21 sekuntia palvelinkeskuksen kuluessa. Mutta taulukkoarvoisten parametreilla 1000 toimintojen kestää vain 2.6 sekuntia sen palvelinkeskuksen ulkopuolella ja 0,4 sekuntia palvelinkeskuksen kuluessa.

Lisätietoja taulukkoarvoisten parametrit on artikkelissa [Table-Valued parametrit](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>Kopioi SQL-samalla kertaa
Toinen tapa lisätä suuria tietomääriä kohdetietokannan on SQL joukkona versio. .NET-sovellukset voivat käyttää luokan **SqlBulkCopy** suorittamaan joukkona Lisää toimintoja. **SqlBulkCopy** on samanlainen funktion komentoriviltä, **Bcp.exe**tai **JOUKKONA Lisää**Transact-SQL-lausekkeen. Koodin seuraavassa esimerkissä esitetään voit joukkomuokata kopioi rivit lähteessä **arvotaulukko**-taulukosta, SQL Server-MyTable kohde-taulukkoon.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Joissakin tapauksissa missä joukkona kopioi ensisijainen päälle taulukkoarvoisten parametrit on. Katso taulukosta Taulukkoarvoisten parametrien ja JOUKKONA Lisää toimintoja artikkelissa [Table-Valued parametrit](https://msdn.microsoft.com/library/bb510489.aspx).

Itsenäisen testitulokset Näytä suorituskyvyn jonottaminen kanssa **SqlBulkCopy** millisekunteina.

| Toiminnot | Paikalliseen, Azure (ms)  | Azure saman palvelinkeskuksen (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1 000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

Erän koko on pienempi parametrien käyttäminen taulukkoarvoisten outperformed **SqlBulkCopy** -luokka. Kuitenkin **SqlBulkCopy** suorittaa 12 – 31 % nopeammin taulukkoarvoisten parametrit testejä 1 000 – 10 000 riviä. Taulukkoarvoiset parametreja, kuten **SqlBulkCopy** on hyvä vaihtoehto, oletusmäärä Lisää erityisesti silloin, kun verrattuna suorituskyky-erämuotoinen-toimintoa.

Lisätietoja ADO.NET joukkona kopio on artikkelissa [Joukkona kopiointi SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Usean rivin Parametroitu lisää lauseet
Pienissä erissä yksi vaihtoehto on käyttää suuri Parametroitu Lisää ilmoitus, joka lisää useita rivejä. Seuraava koodi-esimerkissä tällä menetelmällä.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

Tässä esimerkissä on tarkoitus esittää basic käsitteestä. Realistisesta tilanne käydään läpi tarvittavat kohteiden muodostaa kyselymerkkijonon ja haluamasi samanaikaisesti. Sinun on rajoitettu 2100 kyselyparametrit korkeintaan rajoittaa näin voi käsitellä rivien kokonaismäärästä.

Itsenäisen testitulokset Näytä tällaista insert-lauseessa suorituskyvyn millisekunteina.

| Toiminnot | Taulukkoarvoiset parametrit (ms) | Yhden lauseen Lisää (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

Tämän menetelmän voi olla hieman nopeammin erissä, joka on pienempi kuin 100 riviä. Parantaminen on pieni, mutta tällä menetelmällä on jokin muu vaihtoehto, jotka toimi hyvin tietylle sovellukselle käyttämässäsi skenaariossa.

### <a name="dataadapter"></a>DataAdapter
**DataAdapter** -luokan avulla voit muokata **DataSet** -objektia ja Lähetä muutokset, kuten lisätä, päivittää ja poistaa toimintoja. Jos käytät **DataAdapter** tällä tavalla, on tärkeää muistaa erillisessä puhelut tehdään kutakin eri toimintoa. Voit parantaa suorituskykyä toiminnoista, joita olisi erämuotoinen kerrallaan määrän **UpdateBatchSize** -ominaisuuden avulla. Lisätietoja on artikkelissa [Suorittamiseen erä toimintoja käyttämällä DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Kohteen framework
Kohteen Framework ei tällä hetkellä tue jonottaminen. Yhteisön eri kehittäjät yrittänyt kuvaavat kiertää, kuten ohitus **SaveChanges** -menetelmää. Mutta ratkaisuja ovat yleensä monimutkaisia ja mukautetun sovelluksen ja tietomalliin. Kohteen Framework codeplex-projekti sijaitsee keskustelun sivun tämän ominaisuuden pyynnön. Keskustelun on artikkelissa [Suunnittelun kokousmuistiinpanoja – August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Täydellisiä koe, että on tärkeää kuin batching strategia XML-tietoja. XML: n käytön on kuitenkin ei ole muita menetelmiä parempia ja useita haittoja. Tapa muistuttaa taulukkoarvoisten parametrit, mutta sen sijaan, että käyttäjän määrittämän taulukon tallennetun toimintosarjan välitetään XML-tiedosto tai merkkijono. Tallennetun toimintosarjan jäsentää tallennetun toimintosarjan komennot.

On useita tämän menetelmän haitat:

- XML käsitteleminen voi olla hankalaa ja virhe voi enää.
- XML-tietokannan jakaminen voi olla Suoritinta kuormittavaa.
- Tämä menetelmä on useimmissa tapauksissa hitaammin taulukkoarvoisten parametrit.

XML: n käytön erä kyselyjen ei suositella seuraavien syiden takia.

## <a name="batching-considerations"></a>Jonottaminen huomioon otettavia seikkoja
Seuraavissa osissa on lisäohjeita käytön jonottaminen SQL-tietokanta-sovelluksissa.

### <a name="tradeoffs"></a>Kompromissien
Sen mukaan, että arkkitehtuuri jonottaminen voi liittyä sopivat suorituskyky ja vikasietoisuudelle. Esimerkkinä tilanne, jossa tehtäväsi odottamatta siirtyy alaspäin. Jos kadotat yhden tietorivin, vaikutus on pienempi kuin vaikutus menettämättä suuri erän lähettämättömät rivit. Ei suurempi riskin, kun rivejä puskurin ennen lähettämistä määritetyn ajanjakson tietokannan.

Tämä tarjoa vuoksi arvioi, että olet erä tyyppi-toimintoa. Erän aggressiivisemmin (enemmän ja pidempi aika windows), joka on vähemmän tärkeitä tietoja.

### <a name="batch-size"></a>Erän koko
Microsoftin testien ei yleensä ole etuna purkaa suuri erissä kyselyjä pienempi näkyvissä. Itse asiassa jakopisteeseen usein tuloksena hidas suorituskyky kuin yhden suuren erän lähettämistä. Esimerkkinä skenaarion, johon haluat lisätä 1 000 riviä. Seuraavassa taulukossa näkyy, kuinka kauan rekisteröijän taulukkoarvoisten parametrien avulla voit lisätä 1000 rivejä, kun jaettu pienempi erissä.

| Erän koko | Iteraatioita | Taulukkoarvoiset parametrit (ms) |
| -------- | --- | --- |
| 1 000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

Näet, että parhaan suorituskyvyn 1000 rivien on lähettää ne kaikki kerralla. Muut kokeet (ei näy tässä) on pieni suorituskyky-voitto Jaa 10000 rivin erän 5000 kaksi erissä. Mutta testit taulukko rakenne on yksinkertainen suhteellisen, joten olisi Tee testit tietyt tiedot ja erä koot Tarkista seuraavat tulokset.

Toinen tekijä on, että jos yhteensä erän koko kasvaa liian suureksi, SQL-tietokanta saattaa rajoita ja vahvista erän kieltäytyä. Saat parhaan tuloksen Testaa tietyn skenaarion määrittääksesi, onko ihanteellinen erän kokoa. Varmista erän koko määritettäviä suorituksen suorituskyvyn tai virheiden nopeasti muutokset käyttöön.

Lopuksi saldo erän koon jonottaminen liittyvien riskien kanssa. Jos lyhytkestoisia virheitä tai rooli epäonnistuu, harkitse vaikutukset yritetään toimintoa tai menetä osalta.

### <a name="parallel-processing"></a>Rinnakkainen käsittely
Entä jos noudatit tapaa vähentää erän koko mutta käyttää useita viestiketjuissa siirtyminen työn? Microsoftin testien ilmeni uudelleen, että useita pienempi monisäikeisten erissä suorittaa yleensä kuin yhden suurempi erän. Seuraava testi yritetään lisätä vähintään yhden rinnakkaisia erissä 1000 rivejä. Tämän testin näyttää, miten Lisää samanaikaisesti erissä väheni todella suorituskykyä.

| Erän koko [Iteraatioita] | Kaksi viestiketjut (ms) | Neljä viestiketjut (ms) | Kuusi viestiketjut (ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Tulokset eivät ole viitearvoja. Katso [Huomautus ajoitus tulokset tämän ohjeaiheen](#note-about-timing-results-in-this-topic).

On useita syitä mahdollisen suorituskyvyn vuoksi rinnakkaisuus heikkeneminen varten:

- On useita samanaikaisesti verkon kutsuja yhden sijaan.
- Useita toimintoja yksittäisen taulukon voi aiheuttaa ristiriita ja estäminen.
- On liittyvät yleiskulut monisäikeisyys.
- Kulujen avaaminen monta outweighs rinnakkain käsittely etua.

Jos laadit eri taulukoiden tai tietokantoihin, on mahdollista nähdä joitakin suorituskyvyn voitto tämä strategia kanssa. Tietokannan sharding tai federations olisi tämän menetelmän tilanne. Sharding käyttää useita tietokantoja ja reitittää kunkin tietokannan eri tietoja. Jos pieni kunkin erän eri tietokantaan, suorittamalla sitten rinnakkain toiminnot voi olla tehokkaampaa. Suorituskyvyn voitto ei kuitenkaan merkittäviä käyttämään päätös perustana käytettävä tietokanta sharding ratkaisu.

Jotkin rakenteet-pienempi erissä rinnakkain suorittaminen voi aiheuttaa parannettu siirtonopeuden pyyntöjen järjestelmässä kuormitus. Tässä tapauksessa on käyttänyt yhden suurempi erän nopeammin, mutta käsittely useita erissä rinnakkain voi olla tehokkaampaa.

Jos käytät rinnakkaisia suorittamisen, harkitse hallinta työntekijä viestiketjuissa siirtyminen enimmäismäärä. Pienempi luku saattaa johtaa vähemmän ristiriita ja nopeampi suoritusaika. Ota huomioon myös muita kuormituksen, tämä sijoittaa kohdetietokannan sekä yhteydet ja tapahtumat.

### <a name="related-performance-factors"></a>Aiheeseen liittyvät suorituskykytekijät
Tyypillinen ohjeita tietokannan suorituskykyä vaikuttaa myös jonottaminen. Lisää esimerkiksi suorituskyky on vähennetty taulukot, joilla on suuri perusavain tai useita löytyvät indeksejä.

Jos taulukkoarvoisten parametrit tallennetun toimintosarjan, voit käyttää komento **SET NOCOUNT edelleen** menettelyä alussa. Tämä lause vaimentaa palauttamista kuvattuja tarvittavien rivien määrä. Kuitenkin Microsoftin testien **SET NOCOUNT edelleen** käyttää joko oli ei vaikuta tai väheni suorituskykyä. Testaa tallennettu toimintosarja on helppoa yksittäisen **Lisää** komennon taulukkoarvoisten-parametrin. On mahdollista, että monimutkaisempaa tallennettujen toimintosarjojen hyötyä tähän lausekkeeseen. Mutta ei oletetaan, että **SET NOCOUNT edelleen** lisääminen automaattisesti tallennetun toimintosarjan parantaa suorituskykyä. Selvittääksesi tehosteen, Testaa yhteyttä tallennetun toimintosarjan kanssa ja ilman **SET NOCOUNT käytössä** -lause.

## <a name="batching-scenarios"></a>Jonottaminen skenaariot
Seuraavissa kohdissa kuvataan taulukkoarvoisten parametrien käyttämisestä kolme sovelluksen skenaarioita. Ensimmäinen tilanne näyttää, miten puskurointi ja jonottaminen toimii yhdessä. Toinen skenaario tehostaa suorittamalla yhden tallennetun toimintosarjan puhelun perustyyli-tiedot-toimintoja. Lopullinen tilanne näyttää taulukkoarvoisten parametrit käyttämisestä "UPSERT"-toimintoa.

### <a name="buffering"></a>Puskurointi
Vaikka joissakin tilanteissa, jotka ovat ilmeisimmät candidate jonottaminen, on monia tilanteita, joissa voi hyödyntää jonottaminen viivästyneet käsittely mukaan. Viivästyneet käsittely suorittaa kuitenkin myös suurempi riskin tiedot ovat kadonneet Jos tapahtui odottamaton virhe. On tärkeää ymmärtää riskin ja harkitse vaikutukset.

Harkitse esimerkiksi web-sovelluksen, joka seuraa kunkin käyttäjän siirtyminen historia. Sovelluksen tehdä kunkin sivupyynnön puhelu, jos haluat tallentaa käyttäjän sivunäkymän tietokannan. Mutta suurempi suorituskyky ja skaalattavuus onnistuu puskurointi käyttäjien siirtymisruudun tehtävät ja lähettämällä erissä tietokannan tiedot. Voit käynnistää tietokannan päivityksen aikarajan ja/tai puskurikoon mukaan. Säännön voi esimerkiksi määrittää, että erän käsitteleminen 20 sekuntia tai kun puskuri saavuttaa 1000 kohteiden jälkeen.

Seuraava koodiesimerkki käyttää [Uudelleenaktivointi tunnisteet - vastaanotto](https://msdn.microsoft.com/data/gg577609) puskuroitua tapahtumien seuranta luokan aiheutuneet käsittelemiseen. Kun puskuri täyttyy tai aikakatkaisu saavutetaan, käyttäjätiedot erän lähetetään taulukkoarvoisten parametrin tietokantaan.

Seuraavat NavHistoryData luokan mallit siirtyminen käyttäjätiedot. Se sisältää perustiedot, kuten käyttäjätunnus, käyttää URL-osoite ja käyttöaika.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

NavHistoryDataMonitor-luokka on vastuussa käyttäjän Siirtyminen tietojen tietokantaan. Se sisältää menetelmä RecordUserNavigationEntry, joka vastaa nostaminen **OnAdded** tapahtuman. Seuraava koodi näkyy konstruktori logiikan, joka käyttää vastaanotto luominen aiemmin havaittavia sivustokokoelmassa tapahtuman perusteella. Se sitten tilaa tämä havaittavia sivustokokoelman puskurin-menetelmällä. Liikaa määrittää, että puskuri lähetetään jokaisen 20 sekuntia tai 1 000 tapahtumat.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

Käsittelijä muuntaa kaikki puskuroitua kohteet taulukkoarvoisten tyyppi ja välittää tällaista tallennetun toimintosarjan, joka käsittelee erän. Seuraava koodi näkyy valmiina määritelmä NavHistoryDataEventArgs ja NavHistoryDataMonitor luokat.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Sovellus luo käyttämään tämän puskurointia luokan staattista NavHistoryDataMonitor objektia. Aina, kun käyttäjä käyttää sivulla sovellus kutsuu NavHistoryDataMonitor.RecordUserNavigationEntry-menetelmää. Puskurointi logiikan etenee huolehtia nämä tietueet lähettäminen tietokannan erissä.

### <a name="master-detail"></a>Päätason
Taulukkoarvoiset parametrit on hyötyä yksinkertaisia Lisää skenaarioita. Voit kuitenkin olla Lisää haastava erä Lisää, joihin sisältyy useita taulukoita. "Pääkomponentti/tietokomponentti-skenaario on hyvä esimerkki. Päätaulukko tunnistaa ensisijaisen kohteen. Yhden tai useamman tietotaulukoissa tallentaa kohteen enemmän tietoja. Tässä skenaariossa viiteavaimen yhteyksiä Säilytä tiedot yksilöllinen perusmuodon kohteeseen suhde. Harkitse PurchaseOrder ja sen liittyvän OrderDetail taulukon yksinkertaistettu versio. Seuraavat Transact-SQL Luo PurchaseOrder-taulukon, jossa on neljä saraketta: Tilaustunnus, Tilauspäivä, Asiakastunnus ja tila.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Kukin tilaus sisältää vähintään yhden tuotteen ostot. Nämä tiedot kirjataan PurchaseOrderDetail-taulukossa. Seuraavat Transact-SQL Luo PurchaseOrderDetail taulukon, jossa on viisi saraketta: Tilaustunnus, OrderDetailID, tuotetunnus, Yksikköhinta ja OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

Tilaustunnus-sarakkeen PurchaseOrderDetail on viitattava tilauksen PurchaseOrder-taulukosta. Seuraavassa on määritys viiteavain pakottaa tämä rajoitus.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Jotta voit käyttää taulukkoarvoisten parametrit on oltava kunkin kohdetaulukkoon yhden käyttäjän määrittämän taulukon tyyppi.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Määritä tallennetun toimintosarjan, joka hyväksyy tällaisten taulukot. Tämän toiminnon avulla sovelluksen erän paikallisesti tilaukset ja Tilaustiedot yhden puhelun. Seuraavat Transact-SQL on valmis tallennetun toimintosarjan ilmoituksen esimerkin hankkiminen tilauksen.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

Tässä esimerkissä paikallisesti määritettyjä @IdentityLink taulukko sisältää juuri lisättyjen rivien todellinen Tilaustunnus arvot. Tilapäinen Tilaustunnus arvot poikkeavat tilauksen tunnukset @orders ja @details taulukkoarvoisten parametrit. Tästä syystä @IdentityLink taulukko yhdistää Tilaustunnus arvot @orders parametri PurchaseOrder taulukon uusien rivien reaali Tilaustunnus arvot. Kun tämä vaihe @IdentityLink taulukon voi helpottaa lisääminen kanssa, jotka täyttävät viiteavaimen rajoite todellinen Tilaustunnus Tilaustiedot.

Tämä tallennettu toimintosarja voidaan koodi tai muita Transact-SQL-puhelut. Katso tässä asiakirjassa koodin esimerkiksi taulukkoarvoisten parametrit-osio. Seuraavat Transact-SQL esitetään, kuinka voit kutsua sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Tämä ratkaisu avulla kunkin erän käyttämään joukko Tilaustunnus-arvoja, joka alkaa numerosta 1. Tilapäinen Tilaustunnus-arvot kuvaavat erän yhteydet, mutta todellinen Tilaustunnus-arvot määritetään insert INTO aikaan. Voit suorittaa saman lauseet edellisessä esimerkissä toistuvasti ja luo yksilöllinen tilaukset tietokannan. Tästä syystä lisäämällä useita koodin tai tietokannan logiikan, joka estää kaksoiskappaleiden tilaukset, kun käytät tätä jonottaminen tekniikka.

Tässä esimerkissä näytetään, että myös monimutkaisia tietokannan toimintoja pääkomponentti tietokomponentti toimintoja, kuten erämuotoinen taulukkoarvoisten parametreilla.

### <a name="upsert"></a>UPSERT
Toinen batching tilanne koskee päivitetään samanaikaisesti olemassa olevien rivien ja uusien rivien lisääminen. Tämä toiminto kutsutaan joskus "UPSERT" (päivityksen + insert)-toimintoa. Sen sijaan, että erillisessä soittaminen ja päivitä, Yhdistä-lause on parhaiten tämän tehtävän. Yhdistä-lauseen voit suorittaa sekä Lisää ja Päivitä toiminnot yhden puhelun.

Taulukkoarvoisten parametrit voidaan suorittaa päivitykset ja lisää Yhdistä-lauseella. Harkitse esimerkiksi yksinkertaistettu työntekijän taulukko, joka sisältää seuraavat sarakkeet: työntekijätunnus, Etunimi, Sukunimi, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
Tässä esimerkissä voit käyttää SocialSecurityNumber on yksilöllinen useiden työntekijöiden yhdistämisessä. Luo taulukon käyttäjän määrittämä tyyppi:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Seuraavaksi luo tallennetun toimintosarjan tai kirjoittaa koodia, joka käyttää Päivitä ja lisää Yhdistä-lause. Seuraavassa esimerkissä käytetään yhdistäminen-lauseen taulukkoarvoisten parametrin @employees, EmployeeTableType tyyppi. Sisällön @employees taulukon eivät näy tässä.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Lisätietoja on artikkelissa asiakirjat ja Yhdistä-lauseen esimerkkejä. Vaikka samaan työhön voitu suorittaa monivaiheisen, tallennettu toimintosarja soittaminen erota Lisää ja päivityksen toiminnot, Yhdistä-lause on tehokkaampaa. Tietokannan koodi voit myös muodostaa Transact-SQL-puhelut, Yhdistä lauseella suoraan tarvitsematta kaksi tietokannan kutsujen lisääminen ja päivitys.

## <a name="recommendation-summary"></a>Suositus yhteenveto

Seuraavassa luettelossa on tässä aiheessa kuvatut batching suositukset yhteenveto:

- Suurenna suorituskyky ja skaalattavuus SQL-tietokanta-sovellusten puskurointi ja jonottaminen avulla.
- Jonottaminen/puskurointi ja vikasietoisuudelle välillä kompromissien ymmärtäminen. Jos merkittävämpiä kuin rooli, virheen aikana riskiä menettämättä yrityksen tärkeiden tietojen käsittelemättömän erän ehkä aiheutuvat suorituskyvyn etua jonottaminen.
- Yrittää säilyttää kaikki kutsut yksittäisen palvelinkeskuksen tietokantakyselyn vähentää viive.
- Jos valitset yksittäisen batching tekniikka, taulukkoarvoisten parametrit tarjoavat parhaan suorituskyvyn ja joustavuuden.
- Lisää nopein suorituskyvyn noudattamalla seuraavia yleisiä ohjeita mutta testata käyttämässäsi skenaariossa:
    - Käytä < 100 riviä yhteen Parametroitu Lisää-komento.
    - Käytä < 1000 rivien taulukkoarvoisten parametrit.
    - Saat > = 1 000 riviä, käytä SqlBulkCopy.
- Päivittää ja poistaa toimintoja, käytä taulukkoarvoisten parametrit tallennetun toimintosarjan logiikan, joka määrittää taulukon parametrin jokaiselle riville oikea toiminta.
- Erän koon ohjeita:
    - Käytä suurin erä koot, joka katsoo sen perustelluksi sovellus-ja business vaatimuksia.
    - Saldo väliaikaisesti tai kriittinen virheet liittyvien riskien suuri erissä suorituskyvyn voitto. Mikä on uudelleenyritykset tärkeä tai erän tietojen menettämistä? 
    - Testi suurimman erän koko, tarkista, että SQL-tietokanta ei ole hylättävä ne.
    - Luo määritysasetukset kyseisen ohjausobjektin jonottaminen, kuten erän koko tai puskurointia aika-ikkuna. Nämä asetukset on joustavuutta. Voit muuttaa batching toiminnan tuotannon ilman mallirakenteeseen pilvipalvelussa.
- Vältä rinnakkain suorittamisen erissä, jotka toimivat yhden taulukon yhtä tietokantaa. Jos haluat jakaa yhden erän yli useita työntekijä viestiketjuissa siirtyminen, suorita tutkimukset viestiketjuissa siirtyminen ihanteellinen määrän. Määrittämätön raja-arvon, kun useita viestiketjuissa siirtyminen Pienennä suorituskyvyn sijaan kasvattaa sitä.
- Harkitse puskurointi koon ja käyttöönoton jonottaminen Lisää skenaarioissa vielä aikaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa kohdistettu sen mukaan, miten tietokannan rakenteen ja coding jonottaminen liittyvät tekniikat parantaa sovelluksen suorituskyky ja skaalattavuus. Mutta tämä on vain yksi tekijä yleinen strategia. Saat enemmän tapoja parantaa suorituskyky ja skaalattavuus [Azure SQL-tietokannan suorituskykyä ohjeet yksittäisen tietokantoja](sql-database-performance-guidance.md) ja [hinnan ja suorituskyvyn huomioon otettavia seikkoja joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md).
