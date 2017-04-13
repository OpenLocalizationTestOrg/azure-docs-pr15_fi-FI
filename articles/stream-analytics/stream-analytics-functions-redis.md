<properties
    pageTitle="Siirtää Azure Redis välimuisti-Azure-funktioilla Azure Stream Analytics | Microsoft Azure"
    description="Lue, miten Azure-funktiota käytetään yhdistetty palvelun Bus jonossa, täytä Azure Redis välimuistin Stream Analytics työn tulosteesta."
    keywords="tietovirta, Redis.txt välimuisti-palvelun bus jonossa"
    services="stream-analytics"
    authors="ryancrawcour"
    manager="jhubbard"
    documentationCenter=""
    />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/26/2016"
    ms.author="ryancraw"/>

# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Azure-funktioiden käyttäminen Azure Redis välimuistissa Azure Stream Analytics tietojen tallentaminen

Azure virta-analyysin avulla voit nopeasti kehittää ja edullinen ratkaisujen saada reaaliaikaisia tietoja laitteet, anturit, infrastruktuuri- ja sovellusten tai minkä tahansa stream tietojen käyttöön. Ottaa käyttöön eri käyttäminen tapauksissa, kuten reaaliaikaisia hallinta ja seuranta, komento ja ohjausobjektin, petoksilta tunnistus, yhdistetyt autot ja monista muista. Näiden skenaarioita haluat ehkä outputted mukaan Azure Stream Analytics jaettujen tietojen säilöön, kuten Azure Redis välimuistin tietojen tallentamiseen.

Oletetaan, että olet telealan yrityksen osassa. Yrität tunnistaa SIM ilmoituksen, jossa useita tunnistetiedot, samaan tulevat puhelut aikaa, mutta eri maantieteellisesti sijainnit. Sinun on luovat tallentaminen kaikki mahdolliset vilpilliseen puhelut Azure Redis välimuistin. Tämä blogiin Annamme ohjeet, miten voit helposti tehtävä on valmis. 

## <a name="prerequisites"></a>Edellytykset
Viimeistele [Reaaliaikaisen petoksen tunnistus] [ fraud-detection] ASA hallintapaketteihin

## <a name="architecture-overview"></a>Arkkitehtuuri yleiskatsaus
![Näyttökuva arkkitehtuuri](./media/stream-analytics-functions-redis/architecture-overview.png)

Edeltävässä kuvassa esitetyllä virta-analyysin avulla streaming syöttötiedot kysely ja lähettää tulos. Perusteella tulosteen Azure-funktiot voivat sitten käynnistää jonkin tapahtuman. 

Tässä blogissa on keskittyä tämän myyntijakso Azure-Funktiot-osa- tai tarkemmin tapahtuman, joka tallentaa tiedot vilpilliseen välimuistiin käynnistävä.
Kun olet suorittanut [Reaaliaikaisen petoksen tunnistus] [ fraud-detection] opetusohjelmassa sinulla arvon (tapahtumaa-toiminnossa), kyselyn, ja tulos (blob storage) on jo määritetty ja käynnissä. Tässä blogissa muutamme käyttämään palvelua Bus jonon tulos. Tämän jälkeen muodostetaan jonon Azure-funktiota. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Luo ja Yhdistä palvelun Bus jonon tulostus
Voit luoda palvelun Bus jonon noudattamalla vaiheet 1 ja 2- [Palvelun Bus olevien aloittaminen].NET-osassa[servicebus-getstarted].
Nyt jonossa yhdistäminen japanin Stream Analytics työ, joka on luotu aiemmassa petoksen tunnistus hallintapaketteihin.



1. Azure-portaalissa Siirry työtäsi **tulostus** -sivu ja valitse **Lisää** sivun yläreunassa.

    ![Tulostaa lisääminen](./media/stream-analytics-functions-redis/adding-outputs.png)

2. Valitse **Palvelun Bus jonon** kuin **allas** ja noudattamalla näytön ohjeita. Valitse luomasi [Aloittaminen palvelun Bus olevien]palvelun Bus jonossa nimitilan[servicebus-getstarted]. Napsauta "oikea"-painiketta, kun olet valmis.
3. Määritä seuraavat arvot:
    - **Tapahtuman Sarjatoiminto muoto**: JSON
    - **Koodaus**: UTF8
    - **Muoto**: erotettu rivi

4. Napsauta **Luo** -painiketta, voit lisätä tämän lähteen ja tarkista, että Stream Analytics voit muodostaa tallennustilan tilin.

5. Valitse **kysely** -välilehdessä korvaa nykyisen kyselyn seuraavasti. Korvaa Tulostenimi, jonka loit vaiheessa 3 *[YOUR palvelun BUS nimi]* . 

    ```    

        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2

        INTO [YOUR SERVICE BUS NAME]
    
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
    
        WHERE CS1.SwitchNum != CS2.SwitchNum
    
    ```

## <a name="create-an-azure-redis-cache"></a>Luo Azure Redis.txt välimuisti
Luo Azure Redis välimuistin seuraavalla .NET osa, [miten voit käyttää Azure Redis välimuistin] [ use-rediscache] kunnes osa nimeltä ***välimuisti-asiakkaiden määrittäminen***.
Kun valmiina, sinulla on uusi Redis välimuisti. Valitse **kaikki asetukset**, **pikanäppäimet** ja Huomautus ***ensisijainen yhteysmerkkijonon***alaspäin.

![Näyttökuva arkkitehtuuri](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Azure funktion luominen
Noudata [ensimmäinen Azure-toiminto luo] [ functions-getstarted] opetusohjelma, jotta Azure-funktioiden käytön aloittaminen. Jos sinulla on jo Azure koostefunktiolla haluat käyttää, siirry eteenpäin [Redis välimuistin kirjoittaminen](#Writing-to-Redis-Cache)

1. Portaalissa Valitse sovelluksen palvelut vasemmanpuoleisessa siirtymisruudussa ja valitse sitten Azure funktion app nimesi siirtyä toiminnon sovelluksen sivustoon.
    ![Näyttökuva sovelluksen services toimintoluettelo](./media/stream-analytics-functions-redis/app-services-function-list.png)

2. Valitse **uudesta funktiosta > ServiceBusQueueTrigger – C#**. Seuraaviin kenttiin noudata seuraavia ohjeita:
    - **Jonon nimi**: sama nimi kuin luodessasi jonossa- [Palvelun Bus olevien aloittaminen] kirjoittamasi nimi[ servicebus-getstarted] (ei palvelun bus nimeä). Varmista, että käytät jono, joka on yhdistetty muodossa Analytics-tuloste.
    - **Palvelun Bus yhteyden**: Valitse **Lisää yhteysmerkkijonon**. Etsi yhteysmerkkijono-perinteinen-portaaliin, valitse **Palvelun Bus**, luomasi palvelun bus ja näytön alareunassa **Yhteystiedot** . Varmista, että olet päänäyttöä tällä sivulla. Kopioi ja liitä yhteysmerkkijono. Vapaasti minkä tahansa yhteyden nimi.
    
        ![Näyttökuva bus yhteyttä](./media/stream-analytics-functions-redis/servicebus-connection.png)
    - **AccessRights**: Valitse **hallinta**


3. Valitse **Luo**

## <a name="writing-to-redis-cache"></a>Välimuistin Redis kirjoitettaessa
Azure-funktio, joka lukee palvelun Bus jonon nyt olet luonut. Kaikki haluamasi hahmottaa on tämän funktion avulla voit kirjoittaa tiedot Redis välimuistiin. 

1. Valitse juuri luomasi **ServiceBusQueueTrigger**ja valitse oikeassa yläkulmassa **funktion sovelluksen asetukset** . Valitse **sovelluksen palvelun asetukset > Asetukset > sovellusasetukset**

2. Luo nimi **nimi** -osan yhteys merkkijonoja-osa. Liitä ensisijainen yhteysmerkkijonon löytämäsi **Luo Redis välimuistin** vaiheessa **arvo** -osaan. Valitse **Mukautettu** , jossa lukee **SQL-tietokantaan**.

3. Valitse yläreunassa **Tallenna** .

    ![Näyttökuva bus yhteyttä](./media/stream-analytics-functions-redis/function-connection-string.png)

4. Palaa sovelluksen Palveluasetukset ja valitse nyt **Työkalut > App Service-editori (ennakkoversio) > Valitse > Siirry**.

    ![Näyttökuva bus yhteyttä](./media/stream-analytics-functions-redis/app-service-editor.png)

5. Editorissa valittua Luo JSON-tiedosto nimeltä **project.json** seuraavien ja tallenna se paikalliselle kiintolevylle.

        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }

6. Tiedoston lataaminen (ei WWWROOT)-funktion pääkansion kyselyjä. Raportissa pitäisi näkyä näkyvät tiedoston nimeltä **project.lock.json** automaattisesti varmistamalla, Nuget paketteja "StackExchange.Redis" ja "Newtonsoft.Json" on tuotu.

7. Korvaa valmiiksi luodun koodin **run.csx** -tiedosto seuraava koodi. LazyConnection-funktion korvaa "Yhteys nimi" **myymälätietojen Redis.txt välimuistiin**vaiheessa 2 luomasi nimi.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;
    
    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");
    
        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-the-stream-analytics-job"></a>Aloita Stream Analytics-työ

1. Telcodatagen.exe sovelluksen käynnistäminen. Käyttö on seuraavanlainen:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````

2. Napsauta sivun yläosassa **Aloita** portaalissa Stream Analytics työ-sivu.

    ![Näyttökuva työn aloitus](./media/stream-analytics-functions-redis/starting-job.png)

3. - **Aloita työ** sivu, joka tulee näkyviin Valitse **nyt** ja valitse sitten näytön alareunassa **Käynnistä** -painiketta. Työ-tilamuutosten aloitus ja sen jälkeen suorittaminen aika koskeviin muutoksiin.
 
    ![Näyttökuva Käynnistä työn aika valinta](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Suorita ratkaisu ja Tarkista tulokset
Palaaminen **ServiceBusQueueTrigger** sivun pitäisi tulla näkyviin lokin lauseita. Lokitiedostot Näytä, että käytössä jotain palvelun Bus jonossa, sijoita se-tietokantaan ja hakemisen, käytössä aika avaimeksi!

Voit varmistaa, että tiedot ovat Redis.txt välimuistin, siirry uudessa portaalissa Redis.txt välimuisti-sivulle (katso [luominen Azure Redis välimuistin](#Create-an-Azure-Redis-Cache) edellisessä vaiheessa) ja valitse konsoli.

Nyt voit kirjoittaa Redis.txt komentoja, varmista, että tiedot on todella välimuistin.

![Näyttökuva Redis.txt konsoli](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Seuraavat vaiheet
Olemme kiinnostunut uusia asioita Azure Funktiot Stream analytics voit tehdä yhdessä ja Toivottavasti tämä lukitus mahdollisuuksia uusi puolestasi. Jos sinulla palautetta mitä haluat seuraavaksi, vapaasti [Azure UserVoice-sivuston](https://feedback.azure.com/forums/270577-stream-analytics)käyttäminen.

Jos olet uusi Microsoft Azure, emme kutsu voit kokeilla sitä kirjautumalla [ilmainen kokeiluversio Azure tili](https://azure.microsoft.com/pricing/free-trial/). Jos ole ennen käyttänyt Stream Analytics, valitse Microsoft kutsua [Luo ensimmäinen Stream Analytics-työ](stream-analytics-create-a-job.md).

Jos tarvitset jokin Ohje tai kysymyksiisi, Kirjaa [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) - tai [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) keskustelupalstalle. 

Näet myös on seuraavissa resursseissa:

- [Azure Funktiot Sovelluskehittäjän opas](../azure-functions/functions-reference.md)
- [Azure Funktiot C# Sovelluskehittäjän opas](../azure-functions/functions-reference-csharp.md)
- [Azure Funktiot F # Sovelluskehittäjän opas](../azure-functions/functions-reference-fsharp.md)
- [Azure Funktiot NodeJS Sovelluskehittäjän opas](../azure-functions/functions-reference.md)
- [Azure Funktiot käynnistimien ja sidontojen](../azure-functions/functions-triggers-bindings.md)
- [Voit valvoa Azure Redis välimuisti](../redis-cache/cache-how-to-monitor.md)

Pysyt ajan tasalla kaikissa uutisia ja ominaisuuksia, noudata [@AzureStreaming](https://twitter.com/AzureStreaming) Twitter.


[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
