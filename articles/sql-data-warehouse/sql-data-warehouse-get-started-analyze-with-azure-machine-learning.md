<properties
   pageTitle="Azure koneen Learning tietojen analysoiminen | Microsoft Azure"
   description="Azure Konepohjaisten Oppimistekniikoiden avulla voit luoda ennakoivan konepohjaisten oppimistekniikoiden malliin perustuvan Azure SQL-tietovarasto tallennettuja tietoja."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Azure koneen Learning tietojen analysoiminen

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure koneen oppiminen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Tässä opetusohjelmassa käyttää Azure Konepohjaisten Oppimistekniikoiden luonnissa ennakoivan konepohjaisten oppimistekniikoiden malliin perustuvan Azure SQL-tietovarasto tallennettuja tietoja. Tarkemmin sanottuna tämä muodostaa kohdistetun markkinointikampanjan Adventure Works-polkupyörien, kahvilassa mukaan korrelaatio, jos asiakas on luultavasti Osta polkupyörien vai ei.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Edellytykset
Jos haluat askel Tässä opetusohjelmassa on:

- SQL-tietovarasto ladata etukäteen AdventureWorksDW mallitiedot. Valmistelu tämä [Luo SQL-tietovarasto][] ja valitse Lataa mallitiedot. Jos sinulla on jo tietovarasto, mutta ei ole mallitiedot, voit [ladata mallitiedot manuaalisesti][].

## <a name="1-get-data"></a>1. Nouda tiedot
Tiedot AdventureWorksDW tietokannan dbo.vTargetMail-näkymässä. Voit lukea näitä tietoja seuraavasti:

1. Kirjaudu [Azure koneen Learning studio][] ja valitse sitten oma kokeissa.
2. Valitse **+ Uusi** ja valitse **Tyhjä kokeen**.
3. Oman kokeen nimi: kohdistettu markkinoinnin.
4. Vedä **lukija** -moduulin moduulit-ruudusta alusta.
5. Määritä ominaisuudet-ruudussa tietovarasto SQL-tietokannan tiedot.
6. Määritä **kyselyn** lukea halutut tiedot.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Suorita kokeen napsauttamalla kokeen alusta-kohdassa **Suorita** .
![Suorita kokeen][1]


Kun kokeilla on tehty suorittaminen onnistui, tuloste portti lukija-moduulin alareunassa ja valitse **Visualisoi** tuodut tiedot.
![Tuotujen tietojen tarkasteleminen][3]


## <a name="2-clean-the-data"></a>2. Puhdista tiedot
Puhdista tiedot pudottaa sarakkeita, jotka eivät ole ajankohtaisia mallin. Toiminto:

1. Vedä **Projektin sarakkeet** -moduulin alusta.
2. Valitse **Käynnistä sarakevalitsin** ominaisuudet-ruudussa voit määrittää haluat Poista sarakkeet.
![Project-sarakkeet][4]

3. Kaksi sarakkeiden jättäminen pois: CustomerAlternateKey ja GeographyKey.
![Tarpeettomien sarakkeiden poistaminen][5]


## <a name="3-build-the-model"></a>3. mallin luominen
Olemme jakaa tietoja 80 – 20: 80 % kouluttaminen konepohjaisten oppimistekniikoiden malli ja testaa mallin 20 %. Microsoft tekee käyttää "Luokan kaksi" algoritmeista binaarinen luokitus tämän ongelman.

1. Vedä **Jaa** -moduulin alusta.
2. Kirjoita 0,8 nimittäjä rivejä ensimmäisen tulosteen tietojoukko, ominaisuudet-ruudussa.
![Tietojen jakaminen koulutus ja testi][6]
3. Vedä **Kahden luokan tehosti Päätöspuun** moduulin alusta.
4. Vedä **Junassa malli** -moduulin alusta ja määritä syötteiden. Valitse **Käynnistä sarakevalitsin** ominaisuudet-ruudussa.
      - Lisää ensin: ML algoritmin.
      - Lisää toinen: kouluttaminen algoritmin tiedot.
![Yhdistä junassa malli-moduuli][7]
5. Valitse sarakkeen ennustaa **Pyöränostaja** -sarakkeen.
![Valitse sarake ennustaa][8]


## <a name="4-score-the-model"></a>4. sija malli
On nyt, Testaa kuinka mallin suorittaa testitiedot. Olemme vertaa algoritmin valinta, näet, joka suorittaa paremmin eri algoritmin kanssa.

1. Vedä **Pistemäärän mallin** moduulin alusta.
    Lisää ensin: mallin toisen syötteen koulutus: testitiedot ![sija malli][9]
2. Vedä **Kahden luokan Bayes pisteen koneen** kokeen alusta. Olemme vertaa miten tämän algoritmin suorittaa kaksi luokan tehosti Päätöspuun verrattuna.
3. Kopioi ja liitä moduulit junassa mallin ja tulos mallin alusta.
4. Vedä **Arvioi malli** -moduulin haluat verrata kahta algoritmit alusta.
5. **Suorita** kokeen.
![Suorita kokeen][10]
6. Valitse tulostusportin arvioi malli-moduulin alareunassa ja valitse Visualisoi.
![Visualisoi arvioinnin tulokset][11]

Annettu arvot ovat OHJETIEDOSTO käyrä, tarkkuus peruuttaminen kaavio- ja hissin käyrän. Katsomalla nämä arvot näkyvissä ensimmäisen mallin suorittaa parempi vaihtoehto toinen tehtävä. Jotta voit tarkastella mikä ennustaa ensimmäisen mallin output portti tulos-mallin Valitse ja valitse Visualisoi.
![Tulos tulokset visualisointi][12]

Näyttöön tulee kaksi testi-tietojoukko lisätään sarakkeita.

- Tulos todennäköisyys liittyy: todennäköisyys, että asiakas on polkupyörien ostajan.
- Tulos: otsikoiden luokittelu tekemän mallin polkupyörien ostajan (1) vai ei (0). Tämä todennäköisyys raja otsikoita on määritetty 50 %, ja se voidaan säätää.

Vertailu sarakkeen Pyöränostaja (todellinen) ja tulos-otsikot (tekstinsyöttö), näet miten hyvin malli on suoritettu. Seuraavat vaiheet voit voit käyttää tätä mallia puhelujen ennusteiden uusien asiakkaiden ja julkaista tämän mallin web-palvelu ja kirjoittaa tulokset takaisin SQL-tietovarasto.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja ennakoivan konepohjaisten oppimistekniikoiden mallien rakentaminen viitata [Konepohjaisten Oppimistekniikoiden Azure-esittely][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure koneen Learning studio]:https://studio.azureml.net/
[Koneen Learning Azure-esittely]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Lataa mallitiedot manuaalisesti]: sql-data-warehouse-load-sample-databases.md
[SQL-tietovarasto luominen]: sql-data-warehouse-get-started-provision.md
