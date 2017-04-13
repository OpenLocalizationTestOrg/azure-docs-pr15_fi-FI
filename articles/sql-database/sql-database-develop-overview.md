<properties
    pageTitle="SQL-tietokantaan kehittää yleiskatsaus | Microsoft Azure"
    description="Lisätietoja käytettävissä connectivity kirjastoja ja sovelluksia yhteyden muodostaminen SQL-tietokantaan parhaat käytännöt."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>SQL-tietokannan kehittäminen yleiskatsaus
Tässä artikkelissa käydään läpi basic seikat, kehittäjä olisi otettava huomioon, kun kirjoitat koodia Azure SQL-tietokantayhteyden muodostamisessa.

## <a name="language-and-platform"></a>Kieli- ja ympäristö
MALLIKOODEJA ovat käytettävissä eri ohjelmoinnin kielet ja ympäristöissä. Voit tarkistaa MALLIKOODEJA linkkejä: 

* Lisätietoja: [yhteyden kirjastojen SQL-tietokantaan ja SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Resurssien rajoitukset
Azure SQL-tietokantaan hallitsee tietokanta käyttämällä kahta eri järjestelmiä resursseista: resurssien hallinnon ja rajoitukset käyttäminen.

* Lisätietoja: [Azure SQL-tietokantaan resurssien rajoitukset](sql-database-resource-limits.md)

## <a name="security"></a>Tietoturva
Azure SQL-tietokanta on resurssien rajoittaminen access-tietojen suojaaminen ja valvonta aktiviteetteja SQL-tietokantaan.

* Lisätietoja: [SQL-tietokannan suojaaminen](sql-database-security.md)

## <a name="authentication"></a>Todennus
* Azure SQL-tietokanta tukee sekä SQL Server-todennus käyttäjillä ja kirjautumiset-sekä [Azure Active Directory käyttöoikeuksien](sql-database-aad-authentication.md) käyttäjiä sekä kirjautumiset.
* Haluat määrittää tietyn tietokannan, sen sijaan, että siirrytään *perusmuodon* tietokanta.
* Et voi käyttää Transact-SQL **käyttää myDatabaseName;** -lause SQL-tietokantaan voit siirtyä toiseen tietokantaan.
* Lisätietoja: [SQL-tietokannan suojaus: tietokannan käytön ja kirjaudu sisään tietoturvan hallintaa](sql-database-manage-logins.md)

## <a name="resiliency"></a>Vikasietoisuudelle
Kun lyhytkestoisia virhe ilmenee muodostettaessa yhteyttä SQL-tietokantaan, koodisi tulee uudelleen puhelun.  On suositeltavaa, yritä logiikan Käytä backoff logiikan niin, että se ei ole ärsyttävä liikaa täyttää SQL-tietokanta yritetään samanaikaisesti useita asiakkaiden kanssa.

* Koodin näytteiden: MALLIKOODEJA, jotka kuvaavat uudelleen logiikan, lue näytteiden kielen valittua: [yhteyden kirjastojen SQL-tietokantaan ja SQL Server](sql-database-libraries.md)
* Lisätietoja: [SQL-tietokanta-asiakasohjelmien virhesanomat](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Yhteyksien hallinta
* Asiakas-yhteyden logiikan ohittaa aikakatkaisun oletusarvo on 30 sekuntia.  15 sekunnin oletusarvo on liian lyhenne internet riippuvat yhteydet.
* Jos käytät [yhteysvarannon](http://msdn.microsoft.com/library/8xx3tyca.aspx), muista sulkea yhteyden kokouksen ohjelmasi ei käytä sitä aktiivisesti ja valmistelee ei voi käyttää sitä uudelleen.

## <a name="network-considerations"></a>Verkon huomioon otettavia seikkoja
* Tietokone, jossa asiakas-ohjelmaa Tarkista palomuurin sallii lähtevät TCP-tietoliikenteen porttia 1433.  Lisätietoja: [Määritä Azure SQL-tietokantaan palomuurin](sql-database-configure-firewall-settings.md)
* Jos asiakasohjelman muodostaa yhteyden SQL-tietokannan Vipuventtiili V12 samalla, kun asiakkaan suoritetaan Azure virtuaalikoneen (AM), sinun on avattava AM portin tiettyjä alueita. Lisätietoja: [portit 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12 lisäksi](sql-database-develop-direct-route-ports-adonet-v12.md)
* Joskus Azure SQL-tietokannan Vipuventtiili V12 asiakasyhteydet ohittaa välityspalvelimen ja käsitellä tietokannan. Portit kuin 1433 ole tärkeä. Lisätietoja: [portit 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12 lisäksi](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Tietoja Sharding ja joustavasti asteikko
Joustavasti asteikko yksinkertaistaa skaalaus ulos (ja -). 

* [Rakenne-kuvioiden määrittäminen usean vuokraajan SaaS sovellusten Azure SQL-tietokanta](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Tietoja riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md)
* [Azure SQL-tietokannan joustavasti asteikko esikatselu käytön aloittaminen](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Seuraavat vaiheet

Selaa kaikkia [ominaisuuksia SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/).
