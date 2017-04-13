<properties
    pageTitle="Palvelimen tietokanta ei ole käytettävissä, yhteyden muodostaminen SQL-tietokantaan | Microsoft Azure"
    description="Vianmääritys tietokanta palvelin ei ole tällä hetkellä käytettävissä-virhe, kun sovellus muodostaa yhteyden muodostaminen SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="palvelimen tietokanta ei ole käytettävissä, yhteyden muodostaminen sql-tietokantaan"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Virhe "Palvelimen tietokanta ei ole tällä hetkellä käytettävissä" yhdistettäessä sql-tietokantaan
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Kun sovellus muodostaa yhteyden Azure SQL-tietokantaan, näyttöön tulee seuraava virhesanoma:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Tämä virheilmoitus ei yleensä lyhytkestoisia (lyhytkestoinen).

Tämä virhe ilmenee, kun Azure-tietokanta on siirretty (tai uudelleen) ja sovelluksen yhteys SQL-tietokantaan katkeaa. SQL-tietokannan kokoonpanon uudelleenmääritys tapahtumat aiheutuu suunnitellun tapahtuma (esimerkiksi ohjelmistopäivitys) tai suunnittelematon tapahtuman (esimerkiksi prosessin kaatumisen, tai kuormituksen tasaamisen). Useimmat kokoonpanon uudelleenmääritys tapahtumat ovat yleensä lyhytkestoinen ja olisi enintään suoritettava alle 60 sekunnin kuluttua. Kuitenkin näitä tapahtumia toisinaan saattaa toimia hitaasti on valmis, esimerkiksi kun suuren tapahtumassa aiheuttaa pitkään suoritettavien palautuksen.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Voit ratkaista lyhytkestoisia yhteysongelmat vaiheet
1.  Tarkista tunnetut katkokset, jolta virheet raportoidut sovellus aikana on tapahtunut [Microsoft Azure palvelun Raporttinäkymät-ikkunan](https://azure.microsoft.com/status) .
2. Sovellukset, jotka yhdistävät pilvipalveluun, kuten Azure SQL-tietokanta kannattaa toiminta säännöllisiä kokoonpanon uudelleenmääritys tapahtumia ja toteuttaa uudelleen logiikan käsittelemään virheet sijaan pinnoituksen ne sovellusvirheet käyttäjille. Tarkista [lyhytkestoisia virheet](sql-database-connectivity-issues.md) -kohta ja parhaita käytäntöjä ja ohjeiden etsiminen [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md) lisätietoja ja Yleiset uudelleen strategioita rakenne. Tarkista MALLIKOODEJA [Kirjastot SQL-tietokantaan ja SQL Server](sql-database-libraries.md) on lisätietoja.
3.  Tietokannan lähestyessä resurssien rajoitukset voi olla lyhytkestoisia connectivity ongelmaa. Katso [suorituskykyongelmien vianmääritys](sql-database-troubleshoot-performance.md).
4.  Jos yhteysongelmien edelleen, tai jos, jonka virhe ilmenee sovelluksen kesto on yli 60 sekuntia tai useita kertoja virheen kohdassa tiettynä päivänä, tiedoston Azure tukipyyntö valitsemalla **Hae tukevat** [Azure tuki](https://azure.microsoft.com/support/options) -sivustossa.

## <a name="next-steps"></a>Seuraavat vaiheet
- Jos näyttöön tulee eri virhesanoma, arvioi [virhesanoma](sql-database-develop-error-messages.md) vihjeitä syy.
- Jos ongelma on jatkuva, siirry ohjeet [vianmääritys yleisiä yhteysongelmien Azure SQL-tietokantaan](sql-database-troubleshoot-common-connection-issues.md).
