<properties
   pageTitle="Asenna Visual Studio ja SSDT SQL-tietovarasto | Microsoft Azure"
   description="Asenna Visual Studio ja SQL Server Development Tools (SSDT) Azure SQL-tietovarasto"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Asenna Visual Studio 2015 ja SSDT SQL-tietovarasto

Kehittää sovelluksia SQL-tietovarasto, on suositeltavaa Visual Studio 2015 käyttäminen viimeisin versio SQL Server Data Tools (SSDT).  Visual Studio 2013: n päivitys 5 kanssa SSDT tuetaan myös yhteensopivuus aiempien versioiden kanssa.  

Visual Studio käyttäminen SSDT avulla voit visuaalisesti tutkiminen taulukoiden, näkymien, tallennettujen toimintosarjojen ja SQL-tietovarasto monta useita objekteja sekä suorittaa kyselyjä SQL Server objektin Resurssienhallinnan avulla.

> [AZURE.NOTE] SQL Data Warehouse ei tue vielä Visual Studio-tietokannan projektit.  Tämä ominaisuus lisätään tulevissa versioissa.

## <a name="step-1-install-visual-studio-2015"></a>Vaihe 1: Asenna Visual Studio 2015

Lataa ja asenna Visual Studio 2015 seuraavista linkeistä. Jos sinulla on jo Visual Studio 2013: n tai 2015 asennettu, voit siirtyä vaiheeseen 2, asenna SSDT.

1. [Lataa Visual Studio 2015][].
2. Noudata [Asentaminen Visual Studio][] -opas MSDN-sivuston ja valitse Oletus-määrityksiä.

## <a name="step-2-install-ssdt"></a>Vaihe 2: Asenna SSDT

Asenna Visual Studio SSDT yksinkertaisesti Tarkista SSDT-päivityksen Visual Studion toimimalla seuraavasti.

1. Valitse Visual Studio **Työkalut** / **tunnisteet ja päivitykset...**  /  **Päivitykset**
2. Valitse **Tuotepäivityksiä** ja Etsi **tietokannan Microsoft SQL Server-päivitys tooling**

Jos päivitys ei löydy, on oltava asennettuna uusimpaan versioon.  Vahvista SSDT on asennettu, valitse **Ohje** / **Tietoja Microsoft Visual Studio** ja Etsi SQL Server Data Tools-luettelossa.  SSDT uusin versio on 14.0.60525.0.  Jos voi asentaa ei ole käytettävissä Visual Studio, voit myös napsauttaa pääset käsiksi [SSDT][] lataussivulta ladattava ja asennettava SSDT manuaalisesti.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut SSDT uusimman version, sinun on valmis [muodostamaan][] SQL-tietovarasto.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[yhdistäminen]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Lataa Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Visual Studio asentaminen]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT lataaminen]: https://msdn.microsoft.com/library/mt204009.aspx
