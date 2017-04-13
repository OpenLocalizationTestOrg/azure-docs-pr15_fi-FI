<properties
   pageTitle="Power BI käyttäminen SQL-tietovarasto | Microsoft Azure"
   description="Power BI käyttäminen Azure SQL-tietovarasto, ratkaisujen kehittämiseen liittyviä vinkkejä."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI käyttäminen SQL-tietovarasto
Kuin Azure SQL-tietokanta, jossa on SQL tietojen varasto suoran yhteyden avulla käyttäjä voi hyödyntää tehokkaita looginen kopioiminen palvelimesta analyyttisten ominaisuuksien Power BI: n rinnalla.  Kanssa suoran yhteyden kyselyt lähetetään takaisin Azure SQL Data Warehouse reaaliajassa, kuten voit tarkastella tietoja.  Tämän yhdistämällä SQL-tietovarasto avulla käyttäjät voivat dynaaminen raporttien luominen minuuttia vastaan teratavua tietojen asteikon.  Lisäksi Power BI-painike avaa johdanto avulla käyttäjät voivat muodostaa suoraan Power BI niiden SQL-tietovarasto ilman keräämällä tietoja muista Azure-osista.

Kun suoran yhteyden Ota Huomautus:

+ Määritä palvelimen täydellinen nimi muodostettaessa (Katso alla lisätietoja)
+ Varmistettava palomuurisäännöt tietokanta on määritetty "Salli käyttöoikeus Azure-palveluihin".
+ Jokaisen toiminnon, kuten sarakkeen valitseminen ja lisääminen suodattimen suoraan pyytää tietovarasto
+ Ruutujen päivitetään noin 15 minuutin välein (Päivitä ei tarvitse voidaan ajoittaa)
+ Kysymysosio ei ole käytettävissä suoraan yhteyden tietojoukkoja
+ Rakennemuutokset ovat ei noudettu automaattisesti
+ Kaikki suora Yhdistä kyselyt on aikakatkaisu 2 minuutin kuluttua

Näiden rajoitusten ja huomautukset voivat muuttua Pyrimme jatkuvasti sen parantamiseen. Muodostaa vaiheet on kuvattu alla.  

## <a name="using-the-open-in-power-bi-button"></a>"Avaa Power BI-painikkeen avulla
Helpoin tapa siirtyä SQL-tietovarasto ja Power BI on Avaa Power BI-painikkeella. Tätä painiketta, voit aloittaa saumattomasti uusi raporttinäkymien luominen Power BI.  

1.  Aloita Siirry SQL-tietovarasto-esiintymän perinteinen Azure-portaalissa.
2.  Napsauta "Avaa Power BI-painiketta.
3.  Jos emme voi rekisteröidä suoraan tai jos sinulla ei ole Power BI-tilin, sinun on sisään.  
4.  Sinut ohjataan SQL-tietovarasto yhteys-sivulle, että SQL-tietovarasto esitäytetty tiedoilla.
5.  Kun olet lisännyt tunnistetiedot on täysin yhteys SQL-tietovarasto.

## <a name="connecting-through-the-power-bi-portal"></a>Yhteyden muodostaminen palvelun Power BI-portaalissa
Lisäksi Avaa Power BI-painikkeella, käyttäjät voivat myös muodostaa niiden SQL-tietovarasto palvelun Power BI-portaalissa.

1.  Valitse "Nouda tiedot' siirtymisruudun alareunassa.
2.  Valitse "Tietokannat".
3.  Valitse kerran 'Azure SQL-tietovarasto' tietokantojen-sivulla ja valitse sitten "Yhdistä".
4.  Kirjoita tarvittavat yhteystiedot.  Palvelimen nimi sekä tietokannan nimi löytyy Azure-portaalissa.
5.  Ohjataan takaisin Power BI pääsivulta ja kun yhteys on tehty "Tietojoukkoja"-kohdassa uusi merkintä näkyy oman esiintymän nimi.  
6.   Voit valita uuden tietojoukon kannattaa tutustua kaikki taulukot ja näkymät tietokannan. Sarakkeen valitseminen lähettää kyselyn takaisin tietolähteen luominen dynaamisesti esilajitella visualisoinnin. Näiden tehosteiden uuden raportin tallentaminen ja kiinnitetty tehtävä koontinäyttöön.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
