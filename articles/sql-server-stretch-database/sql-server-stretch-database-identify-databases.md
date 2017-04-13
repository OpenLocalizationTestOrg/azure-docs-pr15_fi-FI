<properties
    pageTitle="Määritä tietokannat ja taulukot suorittamalla Venytys tietokannan Advisor Venytys tietokannan | Microsoft Azure"
    description="Opettele tunnistaa tietokannat ja taulukot, jotka ovat ehdokkaiden Venytys tietokantaan."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Tunnistaa tietokannat ja taulukot suorittamalla Venytys tietokannan Advisor Venytys tietokantaan

Tietokannat ja taulukot, jotka ovat ehdokkaiden Venytys tietokannan tunnistavan Lataa SQL Server 2016 päivittäminen Advisor ja suorita Venytys tietokannan Advisor. Venytä tietokannan Advisor etuliitteenä estävät ongelmat.

## <a name="download-and-install-upgrade-advisor"></a>Lataa ja asenna Upgrade Advisor
Lataa ja asenna Upgrade Advisor [täältä](http://go.microsoft.com/fwlink/?LinkID=613421). Tämä työkalu ei sisälly SQL Serverin asennustietovälineen.

## <a name="run-the-stretch-database-advisor"></a>Suorita Venytys tietokannan tukipalvelu

1.  Suorita päivityksen tukipalvelu.

2.  Valitse **skenaariot**, ja valitse sitten **Suorita VENYTYS TIETOKANNAN ADVISOR**.

3.  Valitse **Suorita Venytys tietokannan Advisor** -sivu **Valitsemalla TIETOKANTOJEN analyysi**.

4.  **Valitse tietokantojen** -sivu, anna tai valitse palvelimen nimi ja todennus-tiedot. Valitse **Muodosta yhteys**.

5.  Näkyviin tulee luettelo tietokantojen valitussa palvelimessa. Valitse tietokannoissa, jotka haluat analysoida. Valitse **Valitse**.

6.  Valitse **Suorita Venytys tietokannan Advisor** -sivu valitsemalla **Suorita**.  Analyysin suoritetaan.

## <a name="review-the-results"></a>Tulosten tarkasteleminen

1.  Kun analyysi on valmis, valitse **Analyzed tietokantojen** -sivu valitsemalla jokin tietokannoissa, jotka voit analysoida näyttää **analyysitulokset** -sivu.

    Suositeltu valitun tietokannan taulukoiden oletusarvon suositus ehtoja vastaavat luettelo **analyysitulokset** -sivu.

2.  Valitse taulukot **analyysitulokset** -sivu-luettelosta jokin suositellut taulukot Näytä **taulukko tulokset** -sivu.

    Jos siellä estävät ongelmat, **taulukon tulokset** -sivu näyttää valitun taulukon estävät ongelmat. Lisätietoja Venytys tietokannan Advisor havaitsemien eston ongelmista on artikkelissa [rajoitukset Venytys tietokannan](sql-server-stretch-database-limitations.md).

3.  Estävät ongelmat **taulukon tulokset** -sivu-luettelossa, valitse Näytä lisätietoja valittu ongelma ongelmatilanteisiin liittyviä ja ehdottaa rajoituksen vaiheet. Toteuta ehdotettu rajoituksen ohjeita, jos haluat määrittää valitun taulukon Venytys tietokantaan.

## <a name="next-step"></a>Seuraava vaihe
Ota käyttöön Venytys tietokannan.

-   **Tietokannan**Venytys tietokannan käyttöön on artikkelissa [Tietokannan Venytys tietokannan käyttöön](sql-server-stretch-database-enable-database.md).

-   Venytä tietokannan käyttöön toisessa **taulukossa**, kun Venytys on jo käytössä tietokanta, katso [Käyttöön Venytys tietokannan taulukkoon](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Katso myös

[Venytä tietokannan koskevat rajoitukset](sql-server-stretch-database-limitations.md)

[Ota käyttöön Venytys tietokannan tietokantaan](sql-server-stretch-database-enable-database.md)

[Venytä tietokannan käyttöön taulukon](sql-server-stretch-database-enable-table.md)

[Kaikki aiheet Azure SQL Server Venytys-tietokanta-palveluun](sql-server-stretch-database-index-all-articles.md)
