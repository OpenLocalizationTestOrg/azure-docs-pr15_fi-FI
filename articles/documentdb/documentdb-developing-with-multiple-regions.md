<properties
   pageTitle="Useiden alueiden DocumentDB kehittämisen | Microsoft Azure"
   description="Opettele käyttää useita alueilla tietojen Azure DocumentDB täysin hallitun NoSQL tietokannan palvelu."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Kehittäminen monille DocumentDB tilien kanssa

> [AZURE.NOTE] Yleinen jakautumisen DocumentDB tietokantoja on yleisesti saatavilla ja automaattisesti käyttöön juuri luomasi DocumentDB kaikki tilit. Yritämme yleinen käyttöönottaminen kaikkien asiakkaiden, mutta sillä välin voit halutessasi Yleinen osoitteisto käytössä tililläsi, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja Microsoft ota se puolestasi nyt.

Jotta voit hyödyntää [Yleinen osoitteisto](documentdb-distribute-data-globally.md)-asiakassovelluksissa määrittää järjestetyn preference luettelon alueista, jota käytetään suorittaa asiakirjan. Tämä voidaan tehdä määrittämällä yhteyden käytännön. Azure DocumentDB tilin määrittäminen, tavoitettavuuden Aluekohtaiset ja määritetty asetusluettelo perusteella eniten optimaalisen päätepisteen valitaan SDK suorittamiseen Kirjoita ja lue toimintojen avulla. 

Tämä asetusluettelo määritetään, kun valmisteltaessa yhteyttä DocumentDB asiakkaan SDK: T. Hyväksy SDK: T valinnaisten parametrien "PreferredLocations", joka on järjestetty luettelo Azure alueiden.

SDK automaattisesti lähettää kaikki kirjoituksia nykyiseen kirjoittaa alue. 

Kaikki lukee lähetetään PreferredLocations-luettelosta ensimmäinen käytettävissä alue. Jos kutsu epäonnistuu, asiakkaan epäonnistua seuraavan alueen luetteloa alaspäin ja niin edelleen. 

Asiakkaan SDK: T vain yrittää lukea alueiden määritetty PreferredLocations. Niin esimerkiksi jos tietokanta-tili on käytettävissä kolme alueilla, mutta asiakkaan vain määrittää kaksi kirjoitus alueiden PreferredLocations, sitten ei ole lukuja olla served ulos kirjoitus-alue, vaikka kyseessä automaattisesti.

Sovelluksen voit tarkistaa nykyisen kirjoittaminen päätepisteen ja lukea päätepisteen valitsema SDK valitsemalla WriteEndpoint ja ReadEndpoint käytettävissä SDK versiossa 1.8 ja yllä ominaisuudet. 

Jos PreferredLocations-ominaisuutta ei ole määritetty, kaikki pyynnöt served nykyisen kirjoitus-alueelta. 


## <a name="net-sdk"></a>.NET SDK-PAKETISSA
SDK voidaan ilman koodin muutoksia. Tässä tapauksessa SDK automaattisesti ohjaa sekä lukee ja kirjoittaa nykyisen kirjoitus-alueen. 

Versiossa 1,8 ja myöhemmin .NET SDK DocumentClient konstruktoria ConnectionPolicy-parametri on Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations-ominaisuuden. Tämä ominaisuus on tyypin sivustokokoelman `<string>` ja niissä on alueen nimiluettelon. Merkkijonoarvot muotoillaan [Azure alueiden] alueen nimi-saraketta kohti [ regions] sivun ilman välilyöntejä ennen tai jälkeen ensimmäisen ja viimeisen merkin tarpeen mukaan.

Nykyinen kirjoitus- ja lue päätepisteet ovat käytettävissä DocumentClient.WriteEndpoint ja DocumentClient.ReadEndpoint tarpeen mukaan.

> [AZURE.NOTE] Päätepisteet URL-osoitteita ei pidetä pitkäikäiset vakioita. Palvelun voi päivittää nämä milloin tahansa. SDK käsittelee muutoksen automaattisesti.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS-, JavaScript- ja Python SDK: T
SDK voidaan ilman koodin muutoksia. Tässä tapauksessa SDK ohjaa automaattisesti kirjoittaa lukuja ja kirjoittaa nykyisen alueen. 

Version 1,8 ja uudemmilla kunkin SDK DocumentClient konstruktoria ConnectionPolicy parametrin uuden ominaisuuden nimi DocumentClient.ConnectionPolicy.PreferredLocations. Tämä on parametri on matriisikaava, joka vie alueen nimiluettelon merkkijonoja. Nimet on muotoiltu [Azure alueiden] alueen nimi-saraketta kohti [ regions] sivulle. Voit myös käyttää ennalta määritettyjä vakioita helppokäyttöisyys objektin AzureDocuments.Regions

Nykyinen kirjoitus- ja lue päätepisteet ovat käytettävissä DocumentClient.getWriteEndpoint ja DocumentClient.getReadEndpoint tarpeen mukaan.

> [AZURE.NOTE] Päätepisteet URL-osoitteita ei pidetä pitkäikäiset vakioita. Palvelun voi päivittää nämä milloin tahansa. SDK käsittelee muutoksen automaattisesti.

Alla on esimerkki koodista NodeJS/Javascript. Python ja Java seuraa samoissa.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>MUILLE KÄYTTÄJILLE 
Kun tietokanta-tili on tehty käytettävissä useita alueilla, asiakkaat kyselyn sen käytettävyys suorittamalla GET-pyynnössä seuraavat URI.

    https://{databaseaccount}.documents.azure.com/

Palvelu palauttaa luettelon alueet ja niiden vastaavan DocumentDB päätepiste URI replikoita. Kirjoita nykyisen alueen ilmaistaan vastauksessa. Asiakas voidaan valita kaikki edelleen REST API pyynnöt tarvittavat päätepisteen seuraavasti.

Esimerkki vastaus

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   HYLLYTETTY, Kirjaa ja poista pyynnöt Siirry määritettyihin URI kirjoittaminen
-   Kaikki saa ja muut vain luku-pyynnöt (esimerkiksi kysely) voivat Siirry minkä tahansa päätepisteen asiakkaan valinta

Kirjoita vain luku-alueiden pyynnöt epäonnistuu HTTP-virhekoodi 403 ("kielletty").

Kirjoita alueen muuttuessa asiakkaan alkuperäinen etsiminen vaiheen jälkeen myöhemmin kirjoituksia aiemman kirjoittaa alueen epäonnistuu HTTP-virhekoodin 403 ("kielletty"). Asiakkaan Valitse Hanki luettelon alueista uudelleen, jotta saat päivitetyn kirjoitus-alue.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja jakelutavoista tiedot yleisesti DocumentDB seuraavissa artikkeleissa:

- [Jakaa tietoja yleisesti DocumentDB](documentdb-distribute-data-globally.md)
- [Yhdenmukaisuuden tasot](documentdb-consistency-levels.md)
- [Useiden alueiden kanssa siirtonopeuden toiminta](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Lisää alueita Azure-portaalissa](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
