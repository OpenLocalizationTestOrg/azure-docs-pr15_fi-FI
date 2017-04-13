<properties
   pageTitle="Azure koneen Learning käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Opetusohjelma Azure SQL-tietovarasto Azure koneen Learning käyttöä varten, ratkaisujen kehittämiseen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>SQL-tietovarasto Azure koneen Oppimiskeskuksen käyttäminen

Azure koneen oppiminen on täysin hallitun ennakoivan analytics-palvelu, joka luo SQL-tietovarasto ennakoivan mallien vastaan tiedot ja sitten Julkaise valmis-,-tarjoaman verkkopalvelut. Voit lukea ennakoivan analyysin ja konepohjaisten oppimistekniikoiden lukemalla [Johdanto Konepohjaisten Oppimistekniikoiden Azure-][]perusteet.  Voit luoda kouluttaminen pisteet ja testaa tietokoneen learning mallin [luominen kokeilla opetusohjelma][]sitten saat.

Tässä artikkelissa kerrotaan, miten seuraavasti [Azure koneen Learning Studio][]:

- Lue tietoja tietokannastasi, jos haluat luoda, kouluttaminen ja sija ennakoivan malli
- Tietokannan tietojen kirjoittaminen


## <a name="read-data-from-sql-data-warehouse"></a>Tietojen lukeminen SQL-tietovarasto

Olemme lukea tietoja tuotteen AdventureWorksDW tietokannan taulukkoon.

### <a name="step-1"></a>Vaihe 1

Aloita uusi koe valitsemalla + Uusi koneen Learning Studio-ikkunan alareunassa Valitse kokeessa ja valitse sitten tyhjä kokeilla. Kokeen oletusnimi alusta yläreunaan ja vaihda sen jotakin merkityksellinen, esimerkiksi polkupyörän hinta tekstinsyöttö.

### <a name="step-2"></a>Vaihe 2

Etsi tietojoukkoja valikoimasta lukija-moduulin ja moduulit vasemmalla puolella kokeen alusta. Vedä moduuli kokeen alusta.
![][drag_reader]

### <a name="step-3"></a>Vaihe 3

Valitse lukija-moduulin ja täytä ominaisuudet-ruudussa.

1. Valitse tietolähteenä Azure SQL-tietokanta.
2. Tietokantapalvelimen nimi: Kirjoita palvelimen nimi. Voit käyttää [Azure portal][] haettava kohde.

![][server_name]

3. Tietokannan nimi: Kirjoita tietokannan nimi juuri määritetty palvelimeen.
4. Palvelimen käyttäjätilisi nimi: Kirjoita käyttäjänimi ja tilille, jolla on tietokannan käyttöoikeudet.
5. Palvelimen käyttäjätilin salasana: antaa määritetyn käyttäjän salasana.
6. Hyväksy kaikki palvelinvarmennetta: Käytä asetusta (heikompi suojaus), jos haluat ohittaa tarkistaminen sivuston sertifikaatin, ennen kuin voit lukea tietoja.
7. Tietokantakyselyn: Kirjoita SQL-lause, joka kuvaa haluat lukea. Tässä tapauksessa emme lukee tiedot tuote-taulukosta seuraava kysely.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Vaihe 4

1. Suorita kokeen napsauttamalla Suorita kohdassa kokeen alusta.
2. Kokeen päätyttyä lukija-moduuli on vihreä valintamerkki osoittaa, että se on suoritettu onnistuneesti. Huomaa myös tila käytössä oikeasta yläkulmasta valmis.

![][run]

3. Saat näkyviin tuodut tiedot, valitse tulosteen portti autojen tietojoukko alareunassa ja valitse Visualisoi.


## <a name="create-train-and-score-a-model"></a>Luoda kouluttaminen ja sija malli

Nyt voit käyttää tätä dataset:

- Mallin luominen: käsitellä tietoja ja voit määrittää ominaisuuksia
- Mallin kouluttaminen: valitseminen ja ottaminen käyttöön learning algoritmin
- Pisteet ja testaa mallia: ennustaa uusi polkupyörän hinta


![][model]

Lisätietoja siitä, miten voit luoda kouluttaminen, pisteet ja testaa konepohjaisten oppimistekniikoiden mallin käyttäminen [Luo kokeilla opetusohjelma][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Tietojen kirjoittaminen Azure SQL-tietovarasto

Olemme kirjoittaa tulosjoukon ProductPriceForecast AdventureWorksDW tietokannan taulukkoon.

### <a name="step-1"></a>Vaihe 1

Etsi tietojoukkoja valikoimasta Writer-moduulin ja moduulit vasemmalla puolella kokeen alusta. Vedä moduuli kokeen alusta.

![][drag_writer]

### <a name="step-2"></a>Vaihe 2

Valitse Writer-moduulin ja täytä ominaisuudet-ruudussa.

1. Valitse Azure SQL-tietokannan tietojen kohteeksi.
2. Tietokantapalvelimen nimi: Kirjoita palvelimen nimi. Voit käyttää [Azure portal][] haettava kohde.
3. Tietokannan nimi: Kirjoita tietokannan nimi juuri määritetty palvelimeen.
4. Palvelimen käyttäjätilisi nimi: Kirjoita käyttäjänimi ja tilille, jolla on tietokannan kirjoitusoikeudet.
5. Palvelimen käyttäjätilin salasana: antaa määritetyn käyttäjän salasana.
6. Hyväksy kaikki palvelinvarmennetta (turvallinen): Valitse tämä vaihtoehto, jos et halua tarkastella.
7. Pilkuilla erotettu luettelo sarakkeiden tallentaa: on viestiä koskevaa tulos tai tietojoukon sarakkeet, jotka haluat siirtää.
8. Taulukkonimi: taulukon nimi.
9. Pilkuilla erotettu luettelo arvotaulukko sarakkeiden: Määritä, jos haluat käyttää uuden taulukon sarakkeiden nimet. Sarakkeiden nimet voi olla eri kuin olevia lähde-tietojoukko, mutta on luettelo sama määrä sarakkeita tähän, voit määrittää tulostusalue.
10. Kirjoitettu SQL Azure-toimintoa kohden rivien määrä: Voit määrittää, jotka on kirjoitettu yhdellä kertaa SQL-tietokantaan rivien määrä.

![][writer_properties]

### <a name="step-3"></a>Vaihe 3

1. Suorita kokeen napsauttamalla Suorita kohdassa kokeen alusta.
2. Kokeen päätyttyä kaikki moduulit on vihreä valintamerkki osoittamaan, että ne suoritettu onnistuneesti.

## <a name="next-steps"></a>Seuraavat vaiheet

Artikkelissa kehitys Lisävihjeitä [SQL-tietovarasto kehittäminen yleiskatsaus][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL-tietovarasto kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md
[Luo koe-opetusohjelma]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Koneen learning Azure-esittely]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure koneen Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
