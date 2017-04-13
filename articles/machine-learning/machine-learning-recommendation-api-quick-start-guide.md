<properties 
    pageTitle="Pikaopas: tietokoneen Learning suosituksia API | Microsoft Azure" 
    description="Azure Konepohjaisten Oppimistekniikoiden suositukset - pikaopas" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Koneen Learning suositukset-Ohjelmointirajapinnan pika-aloitusopas

>[AZURE.NOTE]Käynnistä suosituksia API kognitiiviset Service-Esityspalvelua käyttämällä tämän version sijaan. Kognitiiviset suositukset-palvelu korvaaminen palvelua ja kaikkien uusien ominaisuuksien kehittämisestä siellä. Siinä on uusia ominaisuuksia, kuten jonottaminen tuki, paremmin API Explorer, selkeämpi API pinta, yhdenmukaisempi kirjautuminen/Laskutus kokemus ja niin edelleen.
> Lisätietoja [uuden kognitiiviset palvelun siirtyminen](http://aka.ms/recomigrate)


Tässä asiakirjassa kerrotaan kuinka, määrän palvelun tai sovelluksen käyttämään Microsoft Azure koneen Learning suosituksia. Löydät lisätietoja suositukset-Ohjelmointirajapinnan [valikoima](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Yleisiä tietoja

Jos haluat käyttää Azure koneen Learning suositukset, seuraavasti:

* Luo malli - malli on säilö käyttötietojen, luettelotiedot ja suositus-malli.
* Luettelon tietojen - luettelo sisältää kohteiden metatietoja. 
* Tuo käyttötiedot - käyttötietoja voi ladata 2 tavoilla (tai molemmat):
    * Lataamalla käyttötietojen sisältävän tiedoston.
    * Lähettämällä tietojen hankkiminen tapahtumat.
    Yleensä käyttö tiedoston lataaminen, jos haluat, että voit luoda ensimmäisen suositus-malli (käynnistyksen) ja käyttää sitä, kunnes järjestelmä kerää riittävästi tietoja käyttämällä hankintaa tietomuoto.
* Suositus mallin luominen – Tämä on asynkroninen toiminto, jossa suositus järjestelmä hakee käyttötiedot ja luo suositus malli. Tämä toiminto voi kestää useita minuutteja tai useita tunteja tietoja ja muodosta määritysten parametrit koon mukaan. Kun käynnistävä Luo saavat on muodosta. Sen avulla voit tarkistaa, milloin muodosta prosessi on päättynyt ennen kuin aloitat tarjoaman suosituksia.
* Kuluttavat suositukset - Get suosituksia tietyn kohteen tai kohteiden luettelo.

Kaikki edellä mainitut vaiheet on tehty Azure koneen Learning suosituksia API-Liittymän kautta.  Voit ladata sovelluksen malli, joka sisältää kaikki nämä vaiheet [sekä valikoima.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Rajoitukset

* Mallien tilauskohtaisten enimmäismäärä on 10.
* Kohteet, jotka voivat sisältää luettelon enimmäismäärä on 100 000.
* Käyttö pisteet, joita pidetään enimmäismäärä on noin 5,000,000. Vanhin poistetaan, jos uusia ladattu tai raportoi.
* Tiedot, jotka voidaan lähettää viestiin (kuten luettelon tietojen tuominen-valintaikkunassa Tuo käyttötiedot) enimmäiskoko on 200 Megatavua.
* Suositus mallin muodosta, joka ei ole aktiivinen sekunnissa tapahtumien määrä on noin 2TPS. Suositus mallin muodosta, joka on aktiivinen voidaan säilyttää, 20TPS.

##<a name="integration"></a>Integrointi

###<a name="authentication"></a>Todennus
Microsoftin Azure Marketplacesta tukee Basic- tai OAuth todentamismenetelmän. Löydät helposti tilin näppäimet siirtymällä näppäimet Marketplacesta [tilin asetukset](https://datamarket.azure.com/account/keys)-kohdassa. 
####<a name="basic-authentication"></a>Perustodentamista
Lisää Authorization-otsikko:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Muuntaa Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Muuntaa Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Palvelun URI
Palvelun pääkansio URI Azure koneen Learning suositukset-ohjelmointirajapinnan on [tähän.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Täyden palvelun URI ilmaistaan käyttämällä OData-määrityksen osat.

###<a name="api-version"></a>API-versio
Kunkin API-kutsu on, lopuksi kyselyparametri kutsutaan apiVersion, joka on asetettava "1.0".

###<a name="ids-are-case-sensitive"></a>Tunnukset ovat kirjainkoko on merkitsevä
Tunnukset, jokin ohjelmointirajapinnan palauttama kirjainkoko on merkitsevä ja tulisi käyttää sellaisenaan, kun välitetty parametreiksi API seuraavat suosikkiryhmän puhelut. Esimerkiksi mallin tunnukset ja luettelon tunnukset ovat kirjainkoko on merkitsevä.

###<a name="create-a-model"></a>Mallin luominen
Luodaan "Luo malli-pyyntö:

| HTTP-menetelmä | URI |
|:--------|:--------|
|KIRJAA     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
|   modelName   |   Vain kirjaimia (A-Ö-a-ö), numerot (0-9) yhdysmerkit (--) ja alaviivaa (_) on sallittu.<br>Enimmäispituus: 20 |
|   apiVersion      | 1.0 |
|||
| Pyydä tekstissä | EI MITÄÄN |


**Vastaus**:

HTTP-tilakoodi: 200

- `feed/entry/content/properties/id`-Sisältää mallin ID-tunnuksellasi.
**Huomautus**: Mallitunnus on kirjainkoko on merkitsevä.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Luettelon tietojen tuominen

Jos useita luettelotiedostojen lataaminen useita soitettaessa samaan tietomalliin, emme Lisää vain uudet luettelokohteet. Muiden kohteiden säilyy alkuperäisessä arvoilla.

| HTTP-menetelmä | URI |
|:--------|:--------|
|KIRJAA     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
|   modelId |   Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli  |
| Tiedostonimi | Luettelon tekstiä tunnus.<br>Vain kirjaimia (A-Ö-a-ö), numerot (0-9) yhdysmerkit (--) ja alaviivaa (_) on sallittu.<br>Enimmäispituus: 50 |
|   apiVersion      | 1.0 |
|||
| Pyydä tekstissä | Luettelon tiedot. Muoto:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Nimi</th><th>Pakollinen</th><th>Tyyppi</th><th>Kuvaus</th></tr><tr><td>Kohteen tunnus</td><td>Kyllä</td><td>Aakkosnumeerinen, enintään 50 pituus</td><td>Kohteen yksilöivä tunnus</td></tr><tr><td>Kohteen nimi</td><td>Kyllä</td><td>Aakkosnumeerinen, enintään 255 pituus</td><td>Kohteen nimi</td></tr><tr><td>Kohteen luokka</td><td>Kyllä</td><td>Aakkosnumeerinen, enintään 255 pituus</td><td>Luokka, johon kohdetta kuuluu (esimerkiksi ruoanlaitto kirjat, näytelmiä...)</td></tr><tr><td>Kuvaus</td><td>Ei</td><td>Aakkosnumeerinen, enintään pituus 4000</td><td>Kohteen kuvaus</td></tr></table><br>Suurin sallittu tiedostokoko on 200 Megatavua.<br><br>Esimerkki:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan osoitteisto<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, unohtaminen huoneen: Fiction (Byzantium kirja), Varaa<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework-osoitteisto<br>552a1940-21e4-4399-82bb-594b46d7ed54, Beasts, kirjan rajoitus</pre> |


**Vastaus**:

HTTP-tilakoodi: 200

- `Feed\entry\content\properties\LineCount`-Hyväksytty rivien määrä.
- `Feed\entry\content\properties\ErrorCount`-, Joka ei ole lisätty virheen vuoksi rivien määrä.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Tietojen tuominen käyttö

####<a name="uploading-a-file"></a>Tiedoston lataaminen
Tässä osassa esitellään lataaminen käyttötiedot-tiedoston avulla. Voit kutsua tämän Ohjelmointirajapinnan käyttötietojen kanssa useita kertoja. Kaikki käyttötiedot tallennetaan kaikki puheluihin.

| HTTP-menetelmä | URI |
|:--------|:--------|
|KIRJAA     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
|   modelId |   Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli |
| Tiedostonimi | Luettelon tekstiä tunnus.<br>Vain kirjaimia (A-Ö-a-ö), numerot (0-9) yhdysmerkit (--) ja alaviivaa (_) on sallittu.<br>Enimmäispituus: 50 |
|   apiVersion      | 1.0 |
|||
| Pyydä tekstissä | Käyttötiedot. Muoto:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Nimi</th><th>Pakollinen</th><th>Tyyppi</th><th>Kuvaus</th></tr><tr><td>Käyttäjätunnus</td><td>Kyllä</td><td>Aakkosnumeerinen</td><td>Käyttäjän yksilöllinen tunnus</td></tr><tr><td>Kohteen tunnus</td><td>Kyllä</td><td>Aakkosnumeerinen, enintään 50 pituus</td><td>Kohteen yksilöivä tunnus</td></tr><tr><td>Aika</td><td>Ei</td><td>Päivämäärä-muodossa: VVVV/KK/ppTHH (kuten 2013/06/20T10:00:00)</td><td>Tiedot-aika</td></tr><tr><td>Tapahtuman</td><td>Ei, jos annettu on myös sijoitettava päivämäärä</td><td>Jokin seuraavista toimista:<br>• Napsauttamalla<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Hankkiminen</td><td></td></tr></table><br>Suurin sallittu tiedostokoko on 200 Megatavua.<br><br>Esimerkki:<br><pre>149452, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2 84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2 84e4-414c-bb38-be9cf461c347</pre> |

**Vastaus**:

HTTP-tilakoodi: 200

- `Feed\entry\content\properties\LineCount`-Hyväksytty rivien määrä.
- `Feed\entry\content\properties\ErrorCount`-, Joka ei ole lisätty virheen vuoksi rivien määrä.
- `Feed\entry\content\properties\FileId`-Tiedoston tunnus.


OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Käyttämällä tietojen hankkiminen
Tässä osassa esitellään lähettäminen tapahtumia reaaliajassa Azure koneen Learning suositukset, yleensä verkkosivustosta.

| HTTP-menetelmä | URI |
|:--------|:--------|
|KIRJAA     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Pyydä tekstissä| Tietojen tapahtuma kuhunkin tapahtumaan, jonka haluat lähettää. Lähetä sama käyttäjä tai selaimen istunnon sama tunnus tunnus-kenttään. (Katso esimerkki tapahtuman leipätekstin alla).|


- Esimerkki tapahtuman 'Sitten':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esimerkki tapahtuman 'RecommendationClick':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esimerkki tapahtuman 'AddShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esimerkki tapahtuman 'RemoveShopCart':

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Esimerkki tapahtuman "Hankkiminen":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Esimerkki 2 tapahtumia, 'Sitten' ja 'AddShopCart' lähettäminen:

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Vastaus**: HTTP-tilakoodi: 200

###<a name="build-a-recommendation-model"></a>Suositus mallin luominen

| HTTP-menetelmä | URI |
|:--------|:--------|
|KIRJAA     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
| modelId | Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli  |
| userDescription | Luettelon tekstiä tunnus. Huomaa, että jos käytössäsi on välilyöntejä sinun täytyy koodata se 20 % sen sijaan. Katso edellä olevassa esimerkissä.<br>Enimmäispituus: 50 |
| apiVersion | 1.0 |
|||
| Pyydä tekstissä | EI MITÄÄN |

**Vastaus**:

HTTP-tilakoodi: 200

Tämä on asynkroninen API. Näkyviin tulee muodosta tunnus vastaukseksi. Tietää, kun Luo on päättynyt, olisi "Hae muodostaa tila, mallin" Ohjelmointirajapinnan kutsu ja Etsi muodosta tunnus vastauksessa. Huomaa, että muodosta voi kestää minuuteista tunteja tiedot koon mukaan.

Ei voi käyttää suosituksia tähän päivään luo päättyy.

Kelvollinen muodosta tila:

- Luo – malli on luotu.
- Jonossa – mallin muodosta käynnistetty, ja se on jonossa.
- Rakennuksen – mallin muodostanut.
- Success – muodosta päättyi onnistuneesti.
- Virhe – muodosta, joka on päättynyt kanssa epäonnistui.
- Peruutettu – muodosta on peruutettu.
- Peruuttamisesta – muodosta peruutetaan.


Huomautus muodosta tunnus löytyy kohdasta seuraava polku:`Feed\entry\content\properties\Id`

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Muodosta tilan malli

| HTTP-menetelmä | URI |
|:--------|:--------|
|HAE     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
|   modelId         |   Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli    |
|   onlyLastBuild   |   Osoittaa palauttaa kaikki muodosta mallin historia- tai vain viimeisimmän muodosta tila. |
|   apiVersion      |   1.0                                 |


**Vastaus**:

HTTP-tilakoodi: 200

Vastaus on yksi tapahtuma muodosta. Jokaiselle merkinnälle on seuraavat tiedot:

- `feed/entry/content/properties/UserName`– Käyttäjän nimi.
- `feed/entry/content/properties/ModelName`– Mallin nimi.
- `feed/entry/content/properties/ModelId`-Mallin yksilöllinen.
- `feed/entry/content/properties/IsDeployed`– Onko Luo otetaan käyttöön (tietovälinettä aktiivinen koontiversio).
- `feed/entry/content/properties/BuildId`– Muodosta yksilöllinen.
- `feed/entry/content/properties/BuildType`– Luo tyyppi.
- `feed/entry/content/properties/Status`– Muodosta tila. Voi olla jokin seuraavista: virhe, rakennus, jonossa, Cancelling, peruutettu, onnistui
- `feed/entry/content/properties/StatusMessage`– (Koskee vain tiettyjä hyötyä) yksityiskohtaisia tila-sanoma.
- `feed/entry/content/properties/Progress`– Muodosta edistyminen (%).
- `feed/entry/content/properties/StartTime`– Muodosta aloitusaika.
- `feed/entry/content/properties/EndTime`– Muodosta päättymisaika.
- `feed/entry/content/properties/ExecutionTime`– Muodosta kesto.
- `feed/entry/content/properties/ProgressStep`– Muodosta käynnissä oleva nykyisen vaiheen tiedot.

Kelvollinen muodosta tila:
- Luotu – muodosta pyynnön tapahtuma on luotu.
- Jonossa – muodosta pyyntö on aiheutunut, ja se on jonossa.
- Rakennuksen – muodosta on käynnissä.
- Success – muodosta päättyi onnistuneesti.
- Virhe – muodosta, joka on päättynyt kanssa epäonnistui.
- Peruutettu – muodosta on peruutettu.
- Peruuttamisesta – muodosta peruutetaan.

Muodosta tyyppi kelvollisia arvoja:
- Järjestys - järjestyksen muodosta. (Arvon luoda tiedot, lue lisätietoja "Machine Learning suositus API dokumentaatio" asiakirja.)
- Suositus - suositus muodosta.
- Muuttaa FBT-maksuerien - ostanut yhdessä usein muodosta.

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Nouda suositukset

| HTTP-menetelmä | URI |
|:--------|:--------|
|HAE     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
| modelId | Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli |
| että ItemId-tunnukset | CSV-suositella kohteiden luettelo.<br>Enimmäispituus: vähintään 1 024 |
| numberOfResults | Pakollinen tuloksia |
| includeMetatadata | Myöhempää käyttöä varten, aina false |
| apiVersion | 1.0 |

**Vastaus:**

HTTP-tilakoodi: 200

Vastaus on yksi tapahtuma suositeltu kohteen. Tekstien on seuraavat tiedot:

- `Feed\entry\content\properties\Id`-Suositellut kohteen tunnus.
- `Feed\entry\content\properties\Name`-Kohteen nimi.
- `Feed\entry\content\properties\Rating`-Luokitus suositus; suurempi arvo tarkoittaa suurempi LUOTTAMUSVÄLI.
- `Feed\entry\content\properties\Reasoning`-Suositus perustelut (kuten suositus kuvaukset).

OData-XML

Alla olevassa esimerkissä vastaus sisältää 10 suositellut kohteet:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Päivitä malli
Voit päivittää mallin kuvaus tai active muodosta ID-tunnuksellasi.
*Aktiivinen muodosta tunnus* - jokaisen muodosta, jokainen malli on on muodosta. Aktiivinen muodosta tunnus on jokaisen uuden mallin ensimmäisen onnistuneen muodosta. Kun sinulla on aktiivinen muodosta-tunnus ja tehdä muita muodostaa samaan tietomalliin, joudut määrittämään sen oletusarvo muodosta tunnisteeksi erikseen, jos haluat. Kun tarjoaman suositukset, jos et määritä muodosta tunnus, jota haluat käyttää, oletusarvon mukaan käytetään automaattisesti.

Tämä järjestelmä mahdollistaa – kun suositus malli on tuotannon – Luo uusia malleja ja testaa ne ennen niiden edistää tuotannon.

| HTTP-menetelmä | URI |
|:--------|:--------|
|VALITSEMINEN     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Esimerkki:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Parametrin nimi  |   Kelvollisia arvoja                        |
|:--------          |:--------                              |
| tunnus | Yksilöllinen tunnus (kirjainkoko on merkitsevä) malli |
| apiVersion | 1.0 |
|||
| Pyydä tekstissä | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Huomaa, että XML-tunnisteita, kuvaus ja ActiveBuildId ovat valinnaisia. Jos et halua määrittää kuvaus tai ActiveBuildId, poista koko tunniste. |

**Vastaus**:

HTTP-tilakoodi: 200

OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Oikeudellinen
Tämä asiakirja toimitetaan "nimellä-on". Tiedot ja näkymät, joka on tämän asiakirjan, mukaan lukien URL-osoite ja muut viittaukset Internet-sivustossa, voivat muuttua ilman erillistä ilmoitusta. Esimerkkejä mainitut on tarkoitettu vain esimerkiksi ja ovat kuvitteellinen. Yhteyden eikä reaali liitos on tarkoitettu tai edusta. Tässä asiakirjassa ei antaa sinulle mitään oikeuksia Microsoftin tuotteisiin omistusoikeuksia. Oikeus kopioida ja käyttää tätä tiedostoa sisäisiin käyttötarkoituksiin. © 2014 Microsoft. Note 
 
