<properties
   pageTitle="Ota käyttöön SQL Server Azure Venytys tietokantaan läpinäkyvä salauksen (TDE) | Microsoft Azure"
   description="Ota käyttöön SQL Server Azure Venytys tietokantaan läpinäkyvä salauksen (TDE)"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Läpinäkyvä salauksen (TDE) käyttöön Venytys Azure-tietokantaan
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Läpinäkyvä tietojen salaus (TDE) auttaa suojaamaan tietokonetta haittaohjelmien toiminnan suorittamalla tarvitsematta sovelluksen muutokset reaaliaikainen salausta ja salauksen tietokannan, liitetty varmuuskopiot ja tapahtuman lokitiedostojen rest-palvelussa.

TDE salaa koko tietokannan tallennustilaan kutsutaan tietokannan salausavaimen symmetrisen avaimen avulla. Tietokannan salausavaimen on suojattu valmiin palvelinvarmennetta. Valmiin palvelinvarmennetta on yksilöllinen kullekin Azure palvelimelle. Microsoft kiertää nämä varmenteet automaattisesti vähintään kerran 90 päivää. Katso yleiskuvaus TDE, [Läpinäkyvä tietojen salaus (TDE)].

##<a name="enabling-encryption"></a>Salauksen ottaminen käyttöön

Ottaa käyttöön Azure TDE tietokantaan, joka on tallentaminen tiedot uuteen Venytys on otettu käyttöön SQL Server-tietokannasta, seuraavilla tavoilla:

1. Avaa tietokanta [Azure portal](https://portal.azure.com)
2. Valitse tietokanta-sivu **asetukset** -painike
3. Valitse **Läpinäkyvä salauksen** -vaihtoehto![][1]
4. Valitse **Valitse** ja valitse sitten **Tallenna**
![][2]


##<a name="disabling-encryption"></a>Salauksen poistaminen käytöstä

TDE käytöstä Azure tietokannassa, jossa on tallentaminen tiedot uuteen Venytys on otettu käyttöön SQL Server-tietokannasta, seuraavilla tavoilla:

1. Avaa tietokanta [Azure portal](https://portal.azure.com)
2. Valitse tietokanta-sivu **asetukset** -painike
3. Valitse **Läpinäkyvä salauksen** -vaihtoehto
4. Valitse **käytöstä** -asetus ja valitse sitten **Tallenna**




<!--Anchors-->
[Läpinäkyvä salauksen (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
