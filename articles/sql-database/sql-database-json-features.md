<properties
    pageTitle="Azure SQL-tietokannan JSON ominaisuudet | Microsoft Azure"
    description="Azure SQL-tietokantaan voit jäsennä, kyselyjen ja tietojen muotoileminen JavaScript Object Notation (JSON)-merkintätapaa."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Käytön aloittamista käsittelevät JSON Azure SQL-tietokantaan

Azure SQL-tietokantaan voit jäsentää ja kyselyn tiedot JavaScript Object Notation [(JSON)](http://www.json.org/) -muodossa ja relaatio tietojen vieminen JSON tekstinä.

JSON on käytettävä Moderni web ja mobile-sovelluksissa tietojen vaihtaminen suosittujen tietomuoto. JSON käytetään myös projektiin puolirakenteisia tietoja lokitiedostoihin tai NoSQL tietokannoissa, kuten [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Monet muut verkkopalvelut palauttavat tulokset JSON tekstiksi muotoillut tai hyväksy tiedot muotoiltu JSON. Useimmat Azure palveluihin [Azure haku](https://azure.microsoft.com/services/search/), [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/)ja [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on muille käyttäjille, jotka palauttavat tai tarjoaman JSON päätepisteet.

Azure SQL-tietokantaan voit käsitellä JSON tietoja helposti ja tietokantasi integrointi nykyaikaista palvelut.

## <a name="overview"></a>Yleiskatsaus

Azure SQL-tietokanta on JSON tietojen käsitteleminen seuraavat toiminnot:

![JSON-Funktiot](./media/sql-database-json-features/image_1.png)

Jos sinulla on JSON tekstin, voit Poimi tiedot JSON tai varmista, että JSON on muotoiltu oikein [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)ja [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx)valmiin funktioiden avulla. [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) -funktion avulla voit päivittää JSON tekstin sisällä arvoa. Varten kehittyneempiä kyselyt ja analyysi- [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) -funktiota voit muuntaa matriisin JSON objektien rivisarjaa. Palautetut tulosjoukko voidaan suorittaa minkä tahansa SQL-kysely. Lopuksi on [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) -lause, jonka avulla voit muotoilla relaatio taulukoiden JSON tekstiksi tallennettuja tietoja.

## <a name="formatting-relational-data-in-json-format"></a>Muotoilun relaatiotietoja JSON-muodossa
Jos sinulla on web-palvelu, tulevat tiedot tietokannasta kerroksen sekä annetaan JSON-vastauksen muotoileminen tai asiakkaan JavaScript-kehysten ja kirjastoja, joissa Hyväksy tiedot muotoiltuna JSON, voit muotoilla tietokannan sisältöä JSON kuin suoraan SQL-kyselyssä. Voit ei enää tarvitse kirjoittaa sovelluksen koodi, joka muotoilee Azure SQL-tietokannan olevan muu kuin JSON tulokset tai sisältää joitakin JSON Sarjatoiminto kirjastossa voit muuntaa taulukkomuotoinen kyselytulokset ja onnistu objektien JSON-muotoon. Sen sijaan voit muotoilla SQL-kyselytulokset JSON Azure SQL-tietokantaan ja suoraan sovelluksessa FOR JSON-lause.

Seuraavassa esimerkissä Sales.Customer taulukon rivit on muotoiltu JSON FOR JSON-lauseen avulla:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

JSON POLKU-lauseen muotoilee kyselyn tulosten JSON teksti. Sarakkeiden nimet käytetään näppäimet, kun solujen arvot muunnetaan JSON arvot:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Tulosjoukko on muotoiltu JSON-matriisi, jossa kullakin rivillä on muotoiltu erillinen JSON-objekti.

POLKU ilmaisee, että voit mukauttaa Siirtomuoto JSON tuloksen käyttämällä sarakkeen aliases piste-merkintätapaa. Seuraava kysely JSON muodossa "CustomerName"-näppäintä nimi muuttuu, ja siirtää Puhelin-ja "Yhteyshenkilön" osa objektin:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Kyselyn tulos näyttää tältä:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Tässä esimerkissä on palautettu JSON yhtenä objektina sen sijaan, että matriisi määrittämällä [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) -vaihtoehto. Voit käyttää tämä vaihtoehto, jos tiedät, että ovat palauttamista kyselyn tuloksena yhtenä objektina.

FOR JSON-lause tärkeimmät arvo on, että sen avulla voit palauttaa monimutkaisia hierarkkisia tietoja tietokannastasi muotoiltuna sisäkkäiset JSON objektit tai matriiseja. Seuraavassa esimerkissä esitetään, miten Sisällytä tilaukset, jotka kuuluvat asiakkaan tilaukset sisäkkäisiä matriisina:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Sen sijaan, että erota asiakkaan tietojen noutaminen kyselyjen ja sitten hakeaksesi liittyvät tilaukset luettelo saat kaikki tarvittavat tiedot yhteen kyselyn lähettämisestä seuraava näyte tulos esitetyllä tavalla:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON-tietojen käsitteleminen

Jos sinulla ei ole täysin jäsenneltyjen tietojen, jos sinulla on monimutkaisia aliraportti objekteja, matriiseja tai hierarkkisten tietojen tai tietojen rakenteet kehityttävä ajan kuluessa, JSON-muoto auttavat minkä tahansa monimutkaisten tietojen rakennetta.

JSON on tekstimuotoinen muoto, jota voidaan käyttää kuten mikä tahansa merkkijono muu Azure SQL-tietokantaan. Voit lähettää tai tallentaa vakio NVARCHAR JSON tiedot:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Tässä esimerkissä käytetään JSON tiedot esitetään NVARCHAR(MAX) käyttäen. JSON voidaan lisätty tämä taulukko tai annettu argumenttina tallennetun toimintosarjan käyttämällä vakio Transact-SQL-syntaksi, kuten seuraavassa esimerkissä:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Asiakkaan kieli- tai kirjasto, joka toimii Azure SQL-tietokannan merkkijonotietojen toimii myös JSON tiedot. JSON voidaan tallentaa mitä tahansa taulukon, joka tukee NVARCHAR tyyppi, kuten muistin optimoitu taulukoksi tai päinvastoin järjestelmän versiotietoja. JSON sisältämiin minkä tahansa rajoituksen asiakaspuolen koodin tai tietokannan kerros.

## <a name="querying-json-data"></a>JSON tietojen kyselyt

Jos sinulla on muotoiltu JSON Azure SQL-taulukoihin tallennetut tiedot, JSON-funktioiden avulla voit käyttää näitä tietoja minkä tahansa SQL-kysely.

JSON-toiminnot, jotka ovat käytettävissä Azure SQL-tietokannan Salli Käsittele JSON kaikki muut SQL tietotyypiksi muotoiltuna tiedot. Voit helposti Poimi arvot JSON-tekstin ja JSON tietojen käyttäminen kyselyn:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

JSON_VALUE-funktio poimii arvon JSON tekstin tietosarakkeen tallennetaan. Tämä funktio käyttää JavaScriptiä kaltaisessa polku viittaamaan JSON-teksti, joka purkaa arvon. Puretun arvon voidaan käyttää minkä tahansa SQL-kysely-osassa.

JSON_QUERY-funktio on samankaltainen kuin JSON_VALUE. Toisin kuin JSON_VALUE Tämä funktio poimii monimutkaisia aliraportti objektin esimerkiksi matriiseja tai objekteja, jotka ovat käytettävissä JSON teksti.

JSON_MODIFY-funktion avulla voit määrittää arvon polun JSON teksti, joka on päivitettävä sekä uusi arvo, joka korvaa vanhan. Näin voit helposti päivittää JSON-teksti ilman reparsing rakennetta.

Koska JSON on tallennettu Vakioteksti, mikään ei ole tallennettu palstojen arvot on muotoiltu oikein. Voit tarkistaa, että teksti tallennettu JSON-sarakkeessa on muotoiltu oikein vakio Azure SQL-tietokanta-valintaruudun rajoitukset ja ISJSON-funktion avulla:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Jos Lisää teksti on muotoiltu oikein JSON-ISJSON-funktio palauttaa arvon 1. Joka lisää tai päivitä JSON sarakkeen tämä rajoitus tarkistaa, että uuden tekstiarvo ei ole Vääränlainen JSON.

## <a name="transforming-json-into-tabular-format"></a>JSON muodonmuutoksen taulukkomuotoinen-muotoon

Azure SQL-tietokantaan voit myös muuntaa JSON sivustokokoelmat taulukkomuotoinen muoto ja lataa tai kyselyn JSON tietoihin.

OPENJSON on taulukon arvo-funktio, joka jäsentää JSON tekstiä, määrittää matriisin JSON objekteja, iteroivaa matriisin osasta toiseen ja palauttaa yhden rivin jokainen matriisin osa tulosteen tulos.

![JSON taulukkomuotoinen](./media/sql-database-json-features/image_2.png)

Yllä olevassa esimerkissä on voit määrittää, Etsi JSON-matriisi, joka on avattava ($. Tilaukset-polun), mitkä sarakkeet palautetaan tuloksen ja Etsi JSON-arvoja, jotka palautetaan soluina.

Olemme transform JSON-matriisin @orders muuttujan rivit-joukkoon analysoida Tämä tulosjoukko tai rivien lisääminen taulukkoon Vakio:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Tilausten sivustokokoelman muotoiltu JSON matriisina ja joka tallennetun toimintosarjan parametri voidaan jäsentää ja Tilaukset-taulukkoon.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja JSON integroida sovelluksesi, Tutustu näihin resursseihin:

- [TechNet-blogi](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [MSDN-asiakirjat](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanavan video 9](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Lisätietoja eri tilanteita, joissa JSON integroiminen sovelluksen, katso tämä [video kanavan 9](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) esittelyt tai Etsi skenaario, joka vastaa oman käyttötapaus- [JSON blogimerkinnät](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).
