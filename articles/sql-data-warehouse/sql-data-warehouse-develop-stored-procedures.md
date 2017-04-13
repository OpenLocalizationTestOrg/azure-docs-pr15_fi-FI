<properties
   pageTitle="Tallennettujen toimintosarjojen SQL-tietovarasto | Microsoft Azure"
   description="Vihjeitä tallennettuja toimintosarjoja Azure SQL-tietovarasto, ratkaisujen kehittämiseen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>SQL-tietovarasto tallennettuja toimintosarjoja.

SQL-tietovarasto tukee monia SQL Server-kohdan Transact-SQL-ominaisuuksia. Tärkeintä on pois tiettyjä ominaisuuksia, jotka haluamme hyödyntää voi parantaa suorituskykyä, että asteikko.

Jos haluat säilyttää asteikko ja suorituskyvyn, SQL-tietovarasto on kuitenkin myös joitakin ominaisuuksia ja toimintoja, jotka on käytöspiirteen erot ja muut käyttäjät, joita ei tueta.

Tässä artikkelissa kerrotaan, miten voit toteuttaa tallennettujen toimintosarjojen SQL-tietovarasto kuluessa.

## <a name="introducing-stored-procedures"></a>Tallennettujen toimintosarjojen esittely
Tallennettujen toimintosarjojen on erinomainen tapa, jolla encapsulating SQL-koodi; tallentaminen lähellä tietovarasto tietoja. Encapsulating koodin kyselyjä helpommin hallittaviin yksiköt tallennettujen toimintosarjojen auttaa modularize ehdotetaan niihin ratkaisuja; kehittäjät helpottaa suurempi uusiokäytettävyyttä koodin. Kunkin toimintosarjan myös hyväksyä, jotta ne myös joustavammat parametrit.

SQL-tietovarasto on yksinkertaistettu ja virtaviivaistettu tallennetun toimintosarjan käyttöönotto. SQL Server verrattuna suurimmista ero on tallennettu toimintosarja ei ole valmiiksi käännetty koodi. Tietoja varastossa emme yleensä vähemmän kyseisen kääntäminen ajan. On tärkeää tallennettu toimintosarja-koodi on oikein optimoitava toimivien vastaan suuria tietomääriä. Tavoitteena on raportin tallennus tunnit, minuutit ja sekunnit ei millisekuntia. Näin ollen Lisää kannattaa ajatella tallennettujen toimintosarjojen SQL logiikan säilöjä.     

Kun SQL-tietovarasto suorittaa tallennetun toimintosarjan SQL-lauseet on jäsennetty, käännetty ja optimoitu suorituksen aikana. Vaihdon kunkin lauseen muunnetaan hajautettu kyselyt. SQL-koodi, joka on todella kohdistaa tiedot ei ole lähetetty kyselyyn.

## <a name="nesting-stored-procedures"></a>Sisäkkäisiä tallennettujen toimintosarjojen
Kun tallennettujen toimintosarjojen Kutsu muita tallennettujen toimintosarjojen tai suorita dynaamisen sql, sitten sisemmän tallennetun toimintosarjan tai koodin kutsu sanotaan olla ylemmän tason.

SQL-tietovarasto tukevat enintään 8 sisäkkäisiä tasoja. Tämä on hieman erilainen SQL Server-tietokantaan. Sisäkkäiset SQL Server on 32.

Ylimmän tason tallennetun toimintosarjan puhelun vastaa tehtävässä sisäkkäisiä taso 1

```sql
EXEC prc_nesting
```
Jos tallennettu toimintosarja on myös toisen johto Soita sitten tämä kasvattaa sisäkkäiset tason 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Jos toinen suorittaa joitakin dynaamisen sql sitten tämä kasvattaa sisäkkäiset tason 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Huomautus SQL Data Warehouse ei tue tällä hetkellä @@NESTLEVEL. Haluat pitää kirjaa sisäkkäiset tason itse. On epätodennäköistä, valitset 8 sisäkkäiset tason rajoitus, mutta jos tarvitset "Litistä" sitä ja toimi uudelleen niin, että se on verkkoisännöintialustan rajoitusta.

## <a name="insertexecute"></a>LISÄÄ... SUORITA
SQL-tietovarasto ei salli tarjoaman tallennetun toimintosarjan Lisää lauseella tulosjoukon. On kuitenkin vaihtoehtoinen toimintatavan voit käyttää.

Lisätietoja on seuraavassa artikkelissa on esimerkki siitä, miten voit tehdä tämän [väliaikaisia taulukoita] .

## <a name="limitations"></a>Rajoitukset

On tallennettu Transact-SQL-toimenpiteitä, joita ei ole otettu käyttöön SQL-tietovarasto joitakin ominaisuuksia.

Ne:

- tilapäisten tallennettujen toimintosarjojen
- numeroidun tallennettujen toimintosarjojen
- laajennettujen tallennettujen toimintosarjojen
- CLR tallennettujen toimintosarjojen
- salaus-asetusta
- replikoinnin-vaihtoehto
- taulukkoarvoiset parametrit
- vain luku-parametrit
- oletusparametrit
- suorittamisen kontekstit
- Palauttaa lause

## <a name="next-steps"></a>Seuraavat vaiheet
Artikkelissa kehitys Lisävihjeitä [kehittäminen yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[Väliaikaiset taulukot]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[kehittäminen yleiskatsaus]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
