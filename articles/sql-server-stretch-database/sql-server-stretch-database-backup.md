<properties
    pageTitle="Voit varmuuskopioida Venytys käytössä tietokannoissa | Microsoft Azure"
    description="Lue, miten voit varmuuskopioida Venytys\-käytössä tietokannoissa."
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
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Varmuuskopion Venytys käyttävä tietokannat

Tietokannan varmuuskopiointi avulla voit palauttaa useita erityyppisiä epäonnistuu, virheet ja lieventämiseksi.  

-   Voit varmuuskopioida oman Venytys on\-otettu käyttöön SQL Server-tietokannat.  

-   Microsoft Azure Varmuuskopioi automaattisesti remote tiedot, jotka Venytys tietokanta on siirretty Azure SQL Serveristä.  

>    [AZURE.NOTE] Varmuuskopiointi on vain yksi osa valmis suuren käytettävyyden ja liiketoiminnan jatkuvuuden ratkaisu. Saat lisätietoja suuren käytettävyyden [Suuri käytettävyys ratkaisuja](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>SQL Server-tietojen varmuuskopiointi  

Voit varmuuskopioida oman Venytys\-otettu käyttöön SQL Server-tietokannat, voit edelleen käyttää SQL Server varmuuskopion menetelmiä, jota käytät parhaillaan. Saat lisätietoja, [Varmuuskopiointi ja palauttaminen, SQL Server-tietokannat](https://msdn.microsoft.com/library/ms187048.aspx).

Varmuuskopioiden Venytys on otettu käyttöön SQL Server-tietokannan sisältää vain paikalliset tiedot ja tietoja oikeutettuja siirron kohtaan samanaikaisesti, kun varmuuskopiointi suoritetaan. \(Olevalla tiedot on ei ole vielä siirretty, mutta se siirretään Azure taulukot siirto-asetusten tiedot.\) Tämä on nimeltään **Suppea** varmuuskopion ja ei sisällä tietoja jo Siirry Azure.  

## <a name="back-up-your-remote-azure-data"></a>Varmuuskopioida remote Azure tiedot   

Microsoft Azure Varmuuskopioi automaattisesti remote tiedot, jotka Venytys tietokanta on siirretty Azure SQL Serveristä.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure vähentää automaattinen varmuuskopioimalla tietojen menettämisen riskiä  
Azure SQL Server-Venytys tietokanta-palvelun suojaa remote tietokantoja automaattinen tallennustilan tilannevedoksia kanssa vähintään kerran kahdeksan tuntia. Sekä kunkin tilannevedoksen, 7 päivää tarjota alueen mahdollista palauttaminen asioista.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure vähentää kanssa geo tietojen menettämisen riskiä\-redundancy  
Varmuuskopioiden Azure-tietokanta on tallennettu geo\-tarpeettomat Azure-tallennustilan (RA\-GRS) ja on siksi geo\-tarpeettomat oletusarvoisesti. GEO\-ylimääräinen kopioi tietojen toissijainen alue, joka on satoja Mailia ensisijainen alueen ulkopuolella. Ensisijainen ja toissijainen alueilla tiedot on replikoida jokaisen kolme kertaa, erillisessä vika toimialueet ja Päivitä toimialueet. Näin varmistat, että tiedot ovat kestävät jopa kyseessä valmis alueellisen käyttökatkosta tai tietojen, joka hahmontaa jokin Azure alueet eivät ole käytettävissä.

### <a name="stretchRPO"></a>Venytä tietokannan vähentää Azure tiedoillesi tietojen menettämisen riskiä siirretyt rivit säilytetään väliaikaisesti
Kun Venytys tietokannan siirtää olevalla rivien SQL Server Azure-sekä kahdeksan tuntia väliaikaisen taulukossa vähintään ne rivit. Jos palautat varmuuskopion Azure-tietokanta, Venytys tietokannan käyttää tallennettu väliaikaisen taulukossa rivit täsmäyttää SQL Serverin ja Azure-tietokannat.

Kun varmuuskopion Azure tietojen palauttaminen, sinun on käytettävä uudelleen Venytys tallennetun toimintosarjan [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) \-otettu käyttöön SQL Server-tietokantaan Azure etätietokantaan. Kun suoritat **sys.sp_rda_reauthorize_db**, Venytys tietokannan täsmää SQL Serverin ja Azure-tietokannat.

Kun haluat lisätä siirretyt tiedot, jotka Venytys tietokannan säilyttää tilapäisesti väliaikaisen taulukossa tuntimäärä, suorita tallennetun toimintosarjan [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) ja määritä suurempia kuin 8 tuntien määrän. Voit päättää, kuinka paljon tietoja säilytetään, ota huomioon seuraavat seikat:
-   Taajuus automaattisen Azure varmuuskopioinnin (vähintään kerran kahdeksan tuntia).
-   Aika, joka tarvitaan jälkeen ongelma tunnista ongelma ja päättää palauttaminen varmuuskopiosta.
-   Azure palautustoiminto kestoa.

> [AZURE.NOTE] Pakollinen SQL Serverin jäävän lisääntyvien Venytys tietokannan säilyttää tilapäisesti väliaikaisen taulukossa tietojen määrään kasvaa.

Tarkista tiedot, jotka Venytys tietokannan tällä hetkellä säilyttää väliaikaisesti väliaikaisen taulukossa tuntimäärä, suorita tallennetun toimintosarjan [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Katso myös

[Hallinta ja Venytys tietokannan vianmääritys](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Varmuuskopiointi ja palauttaminen SQL Server-tietokannat](https://msdn.microsoft.com/library/ms187048.aspx)
