<properties
    pageTitle="Yleisten Azure SQL-tietokantaan yhteyden ongelmien vianmääritys"
    description="Ohjeet ja yhteyden yleisten virheiden ratkaiseminen Azure SQL-tietokantaan."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Yhteyden muodostaminen Azure SQL-tietokantaan ongelmien vianmääritys

Kun Azure SQL-tietokannan yhteys epäonnistuu, näyttöön tulee [virhesanomia](sql-database-develop-error-messages.md). Tässä artikkelissa on keskitetty aihe, jonka avulla voit tehdä Azure SQL-tietokanta-yhteysongelmien vianmääritys. Se esittelee [yhteisen aiheuttaa](#cause) ongelmia, suosittelee [vianmääritystyökalu](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , joka auttaa tunnistetietojen ongelma ja annetaan vianmääritysohjeita [lyhytkestoisia virheitä](#troubleshoot-transient-errors) ja [pysyvä tai muu kuin lyhytaikainen virheitä](#troubleshoot-the-persistent-errors). Lopuksi siinä näkyvät [kaikki Azure SQL-tietokanta-yhteysongelmat asiaa artikkelit](#all-topics-for-azure-sql-database-connection-problems).

Jos käytössä ilmenee ongelmia, yritä vianmääritys, joka on kuvattu tämän artikkelin ohjeita.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Syy

Yhteysongelmien voi johtua seuraavista:

- Virhe sovelletaan parhaiden käytäntöjen ja rakenteen ohjeita sovelluksen suunnitteluprosessin aikana.  Artikkelissa [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md) avulla pääset alkuun.
- Azure SQL-tietokannan kokoonpanon uudelleenmääritys
- Palomuuriasetukset
- Yhteyden aikakatkaisu
- Virheellinen kirjautumistiedot
- Enimmäismäärä on saavutettu-resurssien Azure SQL-tietokanta

Yleensä yhteysongelmien Azure SQL-tietokantaan voidaan luokitella seuraavalla tavalla:

- [Lyhytkestoisia virheitä (lyhytkestoinen tai katkonainen)](#troubleshoot-transient-errors)
- [Jatkuva tai muu kuin lyhytaikainen virheitä (virheitä, säännöllisesti toistuva)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Kokeile Azure SQL-tietokanta-yhteysongelmien vianmääritys

Jos käytössä ilmenee tietyn yhteysvirhe, kokeile [tätä työkalua](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), jonka avulla voit nopeasti käyttäjätiedot ja ratkaista ongelmasi.

## <a name="troubleshoot-transient-errors"></a>Lyhytkestoisia vianmääritys
Jos sovellus on yllä lyhytkestoisia virheitä, tarkista vihjeitä ja vähentää virheet on seuraavissa artikkeleissa:

- [Tietokannan vianmääritys &lt;x&gt; palvelimessa &lt;y&gt; ei ole käytettävissä (Virhe: 40613)](sql-database-troubleshoot-connection.md)
- [Vianmääritys, diagnosointi ja estää SQL yhteyden virheet ja lyhytkestoisia virheet SQL-tietokantaan](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Jatkuva vianmääritys (muu kuin lyhytaikainen virheitä)

Jos sovellus lakkaa pysyvästi Azure SQL-tietokannan yhdistäminen, se ilmaisee yleensä ongelma seuraavasti:

- Palomuurin määrityksen. Azure SQL-tietokanta tai asiakkaan palomuuri estää yhteydet Azure SQL-tietokantaan.
- Verkon kokoonpanon uudelleenmääritys asiakaspuolen: esimerkiksi uusi IP-osoite tai välityspalvelinta.
- Käyttäjä-virhe: kirjoitusvirhe esimerkiksi yhteyden parametrien, kuten yhteysmerkkijonon palvelimen nimi.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Jatkuva yhteysongelmien ratkaisemisesta

1.  [Palomuurisäännöt](sql-database-configure-firewall-settings.md) määritetään sallimaan asiakkaan IP-osoite.
2.  Varmista kaikki palomuurit asiakkaan ja Internetin välillä, että porttia 1433 on auki lähteviä yhteyksiä. Tutustu lisävinkkejä [määrittäminen Windowsin palomuurin Salli SQL Server-käyttö](https://msdn.microsoft.com/library/cc646023.aspx) .
3.  Tarkista yhteysmerkkijono ja muut asetukset. Yhteysmerkkijonon on kohdassa [yhteyden ongelmat aihe](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Tarkista palvelun kunto koontinäytön. Jos on alueellisen käyttökatkosta, katso [käyttökatkosta palauttaminen](sql-database-disaster-recovery.md) ohjeet palauttaa uuden alueen.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Kaikki aiheet Azure SQL-tietokannan yhteys ongelmat

Seuraavassa taulukossa on lueteltu jokaisen yhteyden ongelmaa koskeva artikkeli suoraan Azure SQL-tietokanta-palvelun.


| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 1 | [Yhteyden muodostaminen Azure SQL-tietokantaan ongelmien vianmääritys](sql-database-troubleshoot-common-connection-issues.md) | Tämä on aloitussivu Azure SQL-tietokantaan yhteysongelmien vianmääritystä varten. Se käsitellään tunnistamaan ja ratkaisemaan lyhytkestoisia virheet ja pysyvä tai muu kuin lyhytaikainen virheet Azure SQL-tietokantaan. |
| 2 | [Vianmääritys, diagnosointi ja estää SQL yhteyden virheet ja lyhytkestoisia virheet SQL-tietokantaan](sql-database-connectivity-issues.md) | Opettele liittyviä ongelmia, diagnosointi ja estää SQL-yhteysvirhe tai lyhytkestoisia virhe Azure SQL-tietokantaan. |
| 3 | [Yleiset lyhytkestoisia virheen käsittely-ohjeet](best-practices-retry-general.md) | On yleisiä ohjeita lyhytkestoisia vika käsittely yhdistettäessä Azure SQL-tietokanta. |
| 4 | [Microsoft Azure SQL-tietokanta yhteysongelmien vianmääritys](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Tämä työkalu auttaa tunnistetietojen ongelmasi yhteyden virheiden ratkaiseminen. |
| 5 | [Vianmääritys "tietokannan &lt;x&gt; palvelimessa &lt;y&gt; ei ole tällä hetkellä käytettävissä. Yritä yhteyttä myöhemmin"-Virhe](sql-database-troubleshoot-connection.md) | Tässä artikkelissa käsitellään tunnistamaan ja ratkaisemaan 40613 virhe: "tietokannan &lt;x&gt; palvelimessa &lt;y&gt; ei ole tällä hetkellä käytettävissä. Yritä uudelleen myöhemmin yhteys." |
| 6 | [SQL-virhekoodit SQL-tietokantaan asiakassovellukset: tietokannan yhteysvirhe ja muut ongelmat](sql-database-develop-error-messages.md) | Sisältää tietoja SQL-virhekoodit SQL-tietokanta-asiakassovellusten, kuten yleisiä tietokannan yhteys virheitä, tietokannan kopion ongelmat ja yleisten virheiden. |
| 7 | [Azure SQL-tietokannan suorituskykyä ohjeet yksittäisen tietokantoja](sql-database-performance-guidance.md) | Voit selvittää, mitkä palvelutaso sopii sovelluksen ohjeita. Sisältää myös suosituksia säätäminen sovelluksesi hyödyntäminen tehokkaasti Azure SQL-tietokanta. |
| 8 | [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md) | MALLIKOODEJA eri tekniikoita, joiden avulla voit muodostaa yhteyden ja käsitellä Azure SQL-tietokanta, linkkejä. |
| 9 | Päivitä Azure SQL-tietokanta Vipuventtiili v12 sivu ([Azure portal](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Sisältää ohjeet päivittäminen aiemmin Azure SQL-tietokannan V11 palvelimia ja tietokantoja Azure SQL-tietokannan Vipuventtiili V12 Azure portal tai PowerShellin avulla. |


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure SQL-tietokannan suorituskyvyn vianmääritys](sql-database-troubleshoot-performance.md)
- [Azure SQL-tietokannan käyttöoikeudet ongelmien vianmääritys](sql-database-troubleshoot-permissions.md)
- [Näkyviin kaikki Azure SQL-tietokanta-palvelu](sql-database-index-all-articles.md)
- [Microsoft Azure ohjeiden etsiminen](http://azure.microsoft.com/search/documentation/)
- [Näytä uusimmat päivitykset Azure SQL-tietokanta-palveluun](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokannan kehittäminen yleiskatsaus](sql-database-develop-overview.md)
- [Yleiset lyhytkestoisia virheen käsittely-ohjeet](../best-practices-retry-general.md)
- [SQL-tietokantaan ja SQL Serverin kirjastot](sql-database-libraries.md)
- [Oppimispolku käytettäessä Azure SQL-tietokanta](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Oppimispolku joustavasti tietokannan ominaisuudet ja työkalut](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
