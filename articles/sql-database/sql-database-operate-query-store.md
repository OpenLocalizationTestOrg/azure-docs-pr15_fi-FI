<properties
   pageTitle="Liiketoiminnan kyselyn kaupan Azure SQL-tietokantaan"
   description="Lue, miten toimia kyselyn säilön Azure SQL-tietokantaan"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Toimimasta Azure SQL-tietokantaan kysely-säilö 

Kyselyn kaupan Azure-tietokannassa on täysin hallitun tietokanta-ominaisuus, jonka jatkuvasti kerää ja esittää kaikki kyselyt historiallisten yksityiskohtaisia tietoja. Mieti kyselyn kaupan samankaltaisiksi kuin lentokone lento tallennin, merkittävästi yksinkertaistaa kyselyn suorituskyvyn vianmääritykseen sekä cloud ja paikallisen asiakkaille. Tässä artikkelissa kerrotaan toimimasta Azure kyselyn säilössä tiettyjä ominaisuuksia. Käytä ennalta kerättyjen kyselyn tiedot, voit määrittää nopeasti ja ratkaisemaan suorituskykyongelmat ja käyttää näin käyttävät yritykset keskitytään enemmän aikaa. 

Kyselyn säilö on [yleisesti saatavilla](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) Azure SQL-tietokantaan kulunut marraskuun 2015. Kyselyn säilö on suorituskyvyn analysointi ja säätäminen ominaisuudet, kuten [SQL-tietokannan Advisor ja suorituskyvyn koontinäyttö](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)perusta. Tämän artikkelin julkaisemisesta hetkellä kyselyn kaupan on käynnissä yli 200,000 käyttäjän tietokannoissa Azure-tietokannassa, useita kuukautta keskeytyksettä kyselyn liittyvä tietojen kerääminen.

> [AZURE.IMPORTANT] Microsoft on parhaillaan aktivoiminen kyselyn kaupan kaikkia Azure SQL-tietokantoja (aiemmin luoduissa että uusissa). 

## <a name="optimal-query-store-configuration"></a>Paras mahdollinen kyselyn säilön määritys

Tässä osassa kuvataan optimaalisen oletusarvoiksi, jotka on suunniteltu kyselyn tallentaminen ja riippuvaiset ominaisuudet, kuten [SQL-tietokannan Advisor ja suorituskyvyn koontinäyttö](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)luotettavan toiminnan varmistamiseksi. Oletusarvon mukainen määritys on tarkoitettu sivustokokoelman jatkuvaa tietoa, joka on mahdollisimman vähän käytöstä/READ_ONLY tilat-aika.

| Määritys | Kuvaus | Oletusarvo | Kommentti |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Määrittää tietojen paikkaan, jota kysely voi kestää sisällä z asiakkaan tietokannan rajoitus | 100 | Uusien tietokantojen voimassa |
| INTERVAL_LENGTH_MINUTES | Määrittää aikaikkunan aikana kerättyjen runtime tilastotietoja kyselyn suunnitelmien koostetaan ja samanlainen kokoa. Aktiivisen kyselyn jokaisen suunnitelmassa on enintään yhtä riviä ajanjakson määritysten on määritetty | 60   | Uusien tietokantojen voimassa |
| STALE_QUERY_THRESHOLD_DAYS | Aikapohjaisten uudelleenjärjestäminen käytännön, joka ohjaa pysyviä runtime tilastotietoja ja passiiviset kyselyjen säilytysaika | 30 | Uusien tietokantojen ja edellisen oletusarvo (367)-tietokantojen voimassa |
| SIZE_BASED_CLEANUP_MODE | Määrittää, onko automaattisen tietojen uudelleenjärjestäminen tapahtuu, kun kysely kaupan arvopisteiden koko lähestyy rajaa | AUTOMAATTINEN | Kaikkien tietokantojen voimassa |
| QUERY_CAPTURE_MODE | Määrittää, onko kaikki kyselyt tai kyselyt alijoukko seurataan | AUTOMAATTINEN | Kaikkien tietokantojen voimassa |
| FLUSH_INTERVAL_SECONDS | Määrittää, jolta tallennetaan tilastotiedot säilytetään ennen materiaalinottoon levylle muistin runtime | 900 | Uusien tietokantojen voimassa |
||||||

> [AZURE.IMPORTANT] Oletusasetuksia käytetään automaattisesti kaikki Azure SQL-tietokannat (Katso edeltävän tärkeä huomautus) aktivointi kyselyn kaupan viimeisessä vaiheessa. Valo, kun Azure SQL-tietokanta ei voi muuttaminen määritysten arvot asiakkaiden, paitsi, jos ne heikentää ensisijainen työmäärää tai luotettavaa toimintojen kysely-kaupasta.

Jos haluat pitää käyttämällä mukautettuja asetuksia, [Muuta TIETOKANTAA kyselyn Store-vaihtoehtojen avulla](https://msdn.microsoft.com/library/bb522682.aspx) avulla voit palata edelliseen tilaan määritys. Tutustu [Parhaita käytäntöjä kyselyn kaupan](https://msdn.microsoft.com/library/mt604821.aspx) jotta Lue, miten ylhäältä valitsit optimaalisen määrityksiä.

## <a name="next-steps"></a>Seuraavat vaiheet

[SQL-tietokannan suorituskykyä tietoja](sql-database-performance.md)

## <a name="additional-resources"></a>Lisäresursseja

Saat enemmän tietoja uloskuittaus on seuraavissa artikkeleissa:

- [Tietokannan lentojen tallennin](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Suorituskyvyn valvominen kyselyn säilön avulla](https://msdn.microsoft.com/library/dn817826.aspx)

- [Kyselyn kaupan käyttötapoja](https://msdn.microsoft.com/library/mt614796.aspx)

- [Suorituskyvyn valvominen kyselyn säilön avulla](https://msdn.microsoft.com/library/dn817826.aspx) 
