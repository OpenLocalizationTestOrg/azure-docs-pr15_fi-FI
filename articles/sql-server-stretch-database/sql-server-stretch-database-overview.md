<properties
    pageTitle="Venytä tietokannan yleiskatsaus | Microsoft Azure"
    description="Katso, miten Venytys tietokannan siirtää kylmän tietojen läpinäkyvä ja suojatusti Microsoft Azure pilveen."
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
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Venytä tietokannan yleiskatsaus

Venytä tietokannan siirtää kylmän tiedot läpinäkyvä ja suojatusti Microsoft Azure pilveen.

Jos haluat aloittaa saman tien Venytys tietokannan kanssa, katso [Aloita suorittamalla Venytys Ohjattu tietokannan käyttöön](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Mitä hyötyä Venytys tietokannan?
Venytä tietokanta sisältää seuraavat edut:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Kustannus on\-tehokkaan kylmän tietojen käytettävyys
Venytä lämpimän ja kylmän tapahtumien tiedot dynaamisesti SQL Serveristä Microsoft Azure SQL Server-Venytys tietokannan kanssa. Toisin kuin tavallinen kylmän tietosäilö tietosi ovat aina verkkoyhteydessä ja tavoitettavissa kysely. Voit antaa pidempi tietojen säilytys aikajanojen ilman purkaa suurten taulukoiden, kuten asiakkaan Tilaushistoria pankin. Hyötyä vähäisen Azure sijaan skaalaus kallista, valitse\-toimitilojen tallennustilan. Valitse hinnoittelu taso ja asetusten määrittäminen Azure-portaalissa voit hallita kustannukset. Skaalaa ylös tai alas tarpeen mukaan. Käy [SQL Server Venytys tietokannan hinnoittelu](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) -sivulla, jotta tiedot.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Ei edellytä kyselyt tai sovellusten tehdyt muutokset
Käytä SQL Serverin tietoja saumattomasti huolimatta siitä, onko se sijaitsee\-toimitilojen tai sitä venytetään pilveen.  Voit määrittää käytännön, joka määrittää, jossa tiedot on tallennettu ja SQL Server käsittelee tietojen siirto taustalla. Koko taulukko on aina online- ja kyseltäväksi. Ja Venytys tietokantaan ei edellytä kyselyt tai sovellusten muutosten – tietojen sijainti on täysin läpinäkyvä-sovellukseen.

### <a name="streamlines-on-premises-data-maintenance"></a>Valitse virtaviivaistaa\-toimitilojen tietojen hallinta
Vähentää\-toimitilojen ylläpito ja tallennustilaa tiedoillesi. Varmuuskopioiden, että-\-paikallisen tietojen nopeuttaa ja ylläpito-ikkunassa Valmis. Cloud-osaan, tietojen varmuuskopioinnin suoritetaan automaattisesti. Oman-\-paikallisen tallennustilan tarpeiden vähentää huomattavasti. Azure-tallennustilan voi olla pienempi kuin käytössä lisääminen kallista 80 %\-toimitilojen Suoritettaessa.

### <a name="keeps-your-data-secure-even-during-migration"></a>Tietoja suojatun säilyttää myös siirron aikana
Nauti lisäkäyttöoikeuksia voidaan hankkia, kuten venyttää tärkeimmät sovellustesi turvallisesti pilvipalveluun. SQL Server aina salattu salaa tiedoillesi liikkein. Käsitellä Venytys tietokannan tietojen suojaustapoja myös rivin tasolle Suojaustoiminnon ja muut Lisäasetukset SQL Server-suojaustoiminnot.

## <a name="what-does-stretch-database-do"></a>Mitä Venytys tietokannan tarkoittaa?
Kun olet ottanut käyttöön Venytys tietokannan SQL Server-esiintymän, tietokannan ja vähintään yhden taulukon, Venytys tietokannan alkaa äänettömästi kylmän tietojen siirtäminen Azure.

-   Jos tallennat kylmän tiedot erilliseen taulukkoon, voit siirtää koko taulukon.

-   Jos taulukko sisältää sekä kuuman ja kylmän tietoja, voit määrittää suodatinfunktion, valitse Siirrä rivit.

**Sinun ei tarvitse muuttaa kyselyt ja asiakassovellukset.** Voit jatkaa on saumaton sekä paikallisen ja remote tiedot, myös tietojen siirron aikana. Ei viiveen remote kyselyjen vähän, mutta kohtaat tämän viive vain, kun testaat kylmän tiedot.

**Venytä tietokannan varmistaa, että tietoja ei häviä** , virheen ilmetessä siirron aikana. Siinä on myös uudelleen logiikan käsittelemään yhteysongelmien, joita voi ilmetä siirron aikana. Dynaaminen hallinta-näkymässä on siirron tila.

**Voit keskeyttää tietojen siirtäminen** vianmääritys paikallisessa palvelimessa tai suurenna käytettävissä kaistanleveys.

![Venytä tietokannan yleiskatsaus][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Onko Venytys tietokannan puolestasi?
Jos teet seuraavista väittämistä, Venytys tietokannan voi auttaa arvioimaan ja ongelmien ratkaiseminen.

|Jos ole päätös-maker|Jos ole DBA|
|------------------------------|-------------------|
|Minun pitää tapahtumatietojen projektipäällikkönä pitkään.|Taulukon koon on hakeminen ohjausobjektin ulos.|
|Minulla on joskus kylmän tietojen kyselyyn.|Omat käyttäjät ilmoittavat, että he haluavat kylmän tietojen käytön, mutta ne vain harvoin käyttää sitä.|
|Minulla on sovelluksia, mukaan lukien vanhojen sovellusten, et halua päivittää.|Minun pitää lisääminen Lisää tallennustilaa ja ostaminen.|
|Haluan Etsi säästät raha-tallennustilan.|Voin ei varmuuskopiointi ja palauttaminen sisältämiä SLA esimerkiksi suuria taulukoita.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Millaisia tietokannat ja taulukot ovat ehdokkaiden Venytys tietokannan?
Venytä tietokannan kohteiden tapahtumien tietokantojen suuria tietomääriä kylmän, tavallisesti tallennettu pieni useita taulukoita. Seuraavassa taulukossa voi olla enintään miljardin rivit.

Jos käytät SQL Server 2016: n ajallinen taulukko-ominaisuutta, siirtää tai sen osien oleviin kustannuksiin liittyvän historiataulukossa Venytys tietokannan avulla\-tehokkaan Azure-tallennustilan. Saat lisätietoja, [Hallita säilytys historiallinen järjestelmän versiotietoja ajallinen taulukoiden tietojen](https://msdn.microsoft.com/library/mt637341.aspx).

Määritä tietokannat ja taulukot Venytys tietokannan käyttämällä Venytys tietokannan Advisor, SQL Server 2016 päivittäminen Advisor-ominaisuus. Saat lisätietoja, [Anna tietokantojen ja Venytys tietokannan taulukoiden](sql-server-stretch-database-identify-databases.md). Lisätietoja eston ongelmia, katso [Venytys tietokannan koskevat rajoitukset](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Kokeile Venytys tietokanta
**Kokeile Venytys tietokannan AdventureWorks mallitietokannassa.** Saat AdventureWorks mallitietokannan lataamalla vähintään tietokantatiedoston ja näytteiden ja komentosarjat-tiedoston [täältä](https://www.microsoft.com/download/details.aspx?id=49502). Kun mallitietokannan palauttaa SQL Server 2016 esiintymään, unzip objektit-tiedosto ja Venytys DB objektit-tiedoston avaaminen Venytys DB-kansioon. Suorita komentosarjat tässä tiedostossa, voit tarkistaa ennen tietojen käyttämän tilan ja sen jälkeen, kun otat käyttöön Venytys-tietokannan tietojen siirtäminen etenemisen seuranta ja varmista, että voit halutessasi jatkaa kyselyn aiemmin luotuja tietoja ja lisää uudet tiedot sekä aikana tietojen siirtämisen jälkeen.

## <a name="next-step"></a>Seuraava vaihe
**Määritä tietokannat ja taulukot, jotka ovat ehdokkaiden Venytys tietokantaan.** Lataa SQL Server 2016 päivittäminen Advisor ja suorita Venytys tietokannan Advisor tunnistavan tietokannat ja taulukot, jotka ovat ehdokkaiden Venytys tietokantaan. Venytä tietokannan Advisor etuliitteenä estävät ongelmat. Saat lisätietoja, [Anna tietokantojen ja Venytys tietokannan taulukoiden](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
