<properties
   pageTitle="Liiketoiminnan jatkuvuus cloud - poistetun tietokannan - SQL-tietokannan palauttaminen | Microsoft Azure"
   description="Lisätietoja ajankohta palauttaminen, jonka avulla voit palata Azure SQL-tietokanta edellisessä kohdassa senhetkinen (enintään 35 päivää)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Palauttaa Azure SQL-tietokantaan, käyttämällä automaattista tietokannan varmuuskopiointi

SQL-tietokanta on kolmella eri tietokannan palauttaminen käyttämällä [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md). Voit palauttaa tietokannan palvelun käynnistämä-varmuuskopioista niiden [säilytysaika](sql-database-service-tiers.md) :

- Palauttaa tietyn ajan kuluessa säilytysaika-samaan looginen palvelimeen uuden tietokannan. 
- Tietokanta palauttaa poistetun tietokannan poistaminen ajan samassa looginen palvelimessa.
- Uuden tietokannan alueessa, geo replikoida blob storage (RA GRS) uusimman päivittäinen varmuuskopioiden palauttaa loogisen minkä tahansa palvelimessa.

Voit käyttää [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md) luoda [tietokannan kopioi](sql-database-copy.md) kaikki loogiset palvelimeen alueessa, joka vastaa muulla tavoin nykyisen SQL-tietokantaan. Voit käyttää tietokannan kopiota [BACPAC vieminen](sql-database-export.md) ja arkistoida pitkään tallennustilan säilytys päättymisen jälkeen tietokannan muulla tavoin yhdenmukaisia kopion tai siirtää paikallisen kopion tietokannan tai SQL Server Azure AM esiintymä.

## <a name="recovery-time"></a>Palautumisaika

Palauta tietokanta käyttämällä automaattista tietokannan varmuuskopioita palauttamisaika vaikuttaa useista tekijöistä: 
 - Tietokannan koko
 - Tietokannan suorituskykytaso
 - Tapahtumalokit yhdistävää määrä
 - Tehtävän sieppaamaa palauttaa Palauta kohtaan, jossa on määrä
 - Jos palautuksen toisella alueella kaistanleveys 
 - Samanaikainen palauttaminen käsittelemien kohde alueen pyyntöjen määrä. 
 
 Erittäin suuri ja/tai active tietokannan palauttaminen voi kestää useita tunteja. Jos alue on pitkäaikaisesta käyttökatkosta, on mahdollista, että hakutuloksissa näkyy paljon Geo palauttaminen pyynnöt käsittelemien muilla alueilla. Jos määritettynä on suuri määrä pyynnöt Tämä saattaa kasvattaa tietokantojen alueella olevat palauttamisaika. Tietokannan useimpia palauttaa valmis 12 tunnin kuluessa.

 Ei ole sisäistä toimintoa voit joukkomuokata palauttaminen. [Azure SQL-tietokanta: koko palvelimen palautus](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) komentosarja on esimerkki yksi tapa, joiden avulla voit suorittaa tämän tehtävän.

> [AZURE.IMPORTANT] Palauttaaksesi käyttämällä automaattisen varmuuskopioinnin tilaus SQL Server avustaja-roolin jäsen tai tilauksen omistaja. Voit palauttaa Azure portaalissa PowerShell tai REST-Ohjelmointirajapinnalla. Et voi käyttää Transact-SQL. 

## <a name="point-in-time-restore"></a>Ajankohta palauttaminen

Ajankohta: Palauta voit palauttaa aiemmin luodun tietokannan uuden tietokannan aikaisempaan samanaikaisesti käyttämällä [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md)samassa looginen palvelimessa. Et voi korvata aiemmin luotuun tietokantaan. Voit palauttaa aikaisempaan [Azure portal](sql-database-point-in-time-restore-portal.md)- sekä [PowerShell](sql-database-point-in-time-restore-powershell.md) -ja [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx)samanaikaisesti.

> [AZURE.SELECTOR]
- [Ajankohta palauttaminen: Azure portal](sql-database-point-in-time-restore-portal.md)
- [Ajankohta: Palauta: PowerShell](sql-database-point-in-time-restore-powershell.md)

Tietokannan voi palauttaa suorituskyky ja Lisää joustavasti resurssivarantoon. Haluat varmistaa, että sinulla on riittävät DTU kiintiön looginen palvelimessa tai joustavasti resurssivarantoon. Ota huomioon, että Palauta Luo uuden tietokannan ja palautettu tietokanta palvelun taso ja suorituskyvyn taso voi olla eri kuin live tietokannan nykyisen tilan. Kun valmiina, palautettu tietokanta on Normaali käytettävissä online tietokanta veloitetaan normaali tahdissa service tason ja suorituskyvyn tason perusteella. Kulujen maksamaan ei, kunnes tietokannan palautus on valmis.

Tietokannan palauttaminen yleensä earler kohtaan palauttamiseen. Kun teet näin, voit käsitellä palautettu tietokanta korvaa alkuperäinen tietokanta tai hakea tietoja ja päivitä sitten alkuperäisen tietokannan. 

- ***Tietokannan korvaavan:*** Jos palautettu tietokanta on tarkoitettu korvaavan alkuperäisen tietokannan, Tarkista suorituskyky ja/tai palvelutaso sopivat ja skaalata tietokannan tarvittaessa. Voit nimetä uudelleen alkuperäisen tietokannan ja antaa palautetun tietokannan alkuperäinen nimi T SQL muuta TIETOKANTAA-komennolla. 
- ***Tietojen palauttaminen:*** Jos haluat hakea tietoja palautettu tietokannan palauttaminen käyttäjä tai sovellus-virhe, sinun on kirjoittaa ja suorittaa jostakin tietojen palauttaminen komentosarjoja haluat Poimi tiedot palautettu tietokannasta alkuperäisen tietokannan erikseen. Vaikka palautustoiminto saattaa kestää kauan suorittamiseen, palauttaminen tietokantaan on näkyvissä koko tietokanta-luettelossa. Jos poistat tietokannan palauttamisen aikana, se peruuttaa toiminnon ja olet ei peri tietokantaan, joka ei onnistunut Palauta. 

Lisätietoja käyttäjän ja sovellusvirheet palauttaminen ajankohta palauttaminen avulla, katso [-ajankohta palauttaminen](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Poistetun tietokannan palauttaminen

Poistetun tietokannan palauttaminen avulla voit palauttaa poistetun tietokannan poistetun tietokannan samaan looginen palvelimeen käyttämällä [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md)poistaminen ajan. 

> [AZURE.IMPORTANT] Jos poistat erillisen server Azure SQL-tietokanta, kaikki sen tietokantojen poistetaan eikä sitä voi palauttaa. Palauttaa poistetun palvelimen tällä hetkellä ei tueta.

Voit käyttää samaa tai palautetun tietokannan uuden tietokannan nimi. Voit käyttää [Azure portal](sql-database-restore-deleted-database-portal.md)- [PowerShell](sql-database-restore-deleted-database-powershell.md) tai [muiden (createMode = palauttaminen)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Tietokannan palauttaminen poistetut: Azure portal](sql-database-restore-deleted-database-portal.md)
- [Tietokannan palauttaminen poistetut: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>GEO palauttaminen

GEO Palauta voit palauttaa uusin geo replikoida [automaattisen varmuuskopioinnin](sql-database-automated-backups.md)missä tahansa alueessa, minkä tahansa Azure SQL-tietokanta. GEO palauttaminen käyttää geo tarpeettomat varmuuskopion lähteenä ja avulla voidaan palauttaa tietokannan vaikka tietokanta tai palvelinkeskuksen ei ole käytettävissä käyttökatkosta vuoksi. Voit käyttää [Azure portal](sql-database-geo-restore-portal.md)- [PowerShell](sql-database-geo-restore-powershell.md)- tai [muiden (createMode = palautus)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [GEO-Palauta: Azure portal](sql-database-geo-restore-portal.md)
- [GEO palauttaminen: PowerShell](sql-database-geo-restore-powershell.md)

GEO palautus on oletusarvoinen palautus-asetus, kun tietokanta ei ole käytettävissä vuoksi tapahtuma alueen, jossa tietokanta sijaitsee. Jos suuret tapaukseen alueen johtaa siitä, tietokanta-sovelluksen, voit palauttaa tietokannan viimeisimmän varmuuskopiosta muiden alueen palvelimeen Geo palauttaminen. Kaikki varmuuskopiot ovat geo replikoida ja voi olla viive Jos varmuuskopioinnin on otettava ja geo replikoida, Azure blob toisella alueella. Tämän viiveen voi olla enintään tunnin, joten jos huono voi olla määrittäminen 1 tunti tietojen menettämisen, eli RPO on enintään 1 tunti. Seuraavassa näkyy tietokannan palauttaminen viimeisen päivän varmuuskopiosta.

![GEO palauttaminen](./media/sql-database-geo-restore/geo-restore-2.png)

Yksityiskohtaisia tietoja käyttökatkosta palauttaminen Geo palauttaminen avulla on artikkelissa [käyttökatkosta palauttaminen](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Kun Geo palautus on käytettävissä kaikissa palvelutasot, on yleisin tietojen palauttaminen ratkaisuja käytettävissä SQL-tietokantaan pisimmän RPO ja arvion palautus aika (Lisää). Perustietoja tietokantojen kanssa enimmäiskoko 2 Gigatavua Geo-Palauta on suositeltavaa kerralla DR-ratkaisussa, jossa NNA 12 tuntia. Suurempi vakio- tai Premium tietokantoja, jos merkittävästi lyhentää palautus ajat ovat toivottuja tai vähentää tietojen menettämisen todennäköisyys kannattaa harkita aktiivinen Geo-replikoinnin. Aktiivinen Geo-replikoinnin tarjoaa huomattavasti RPO ja NNA, kun se on vain edellyttää aloittaa jatkuvasti replikoitua toissijainen siirtyy automaattisesti. Lisätietoja on artikkelissa [Aktiivinen Geo-replikoinnin](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Suorittamiseen ohjelmallisesti käyttämällä automaattisen varmuuskopioinnin palauttaminen

Kun edellä kuvatun addiition Azure-portaaliin, tietokannan palauttaminen voi suorittaa PowerShellin Azure ja REST-Ohjelmointirajapinnalla programmically avulla. Seuraavissa taulukoissa kuvataan olevat komennot.

### <a name="powershell"></a>PowerShellin

|Cmdlet-komento|Kuvaus|
|------|-----------|
|[Hae AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Saa yksi tai useampi tietokanta.|
|[Hae AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Hakee, voit palauttaa poistetun tietokannan.|
|[Hae AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Saa tietokannan geo tarpeettomat varmuuskopion.|
|[Palauta AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Palauttaa SQL-tietokantaan.|
||||

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

|OHJELMOINTIRAJAPINTA|Kuvaus|
|---|-----------|
|[MUUT (createMode = palauttaminen)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Palauttaa tietokannan|
|[Hae luominen tai päivittäminen tietokannan tila](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Palauttaa tilan palautustyön aikana|
||||



## <a name="summary"></a>Yhteenveto

Automaattisen varmuuskopioinnin suojaa tietokantoja käyttäjän ja sovellusvirheet, vahingossa tietokannan poistaminen ja pitkäaikaisesta katkokset. Nolla-kustannus nolla-järjestelmänvalvojan ratkaisu on käytettävissä kaikkien SQL-tietokannat. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
- Lisätietoja Azure SQL-tietokanta automaattisen varmuuskopioinnin on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin suunnittelu](sql-database-automated-backups.md)
- Lisätietoja nopeampaa palautusasetukset on artikkelissa [Aktiivinen-Geo-replikoinnin](sql-database-geo-replication-overview.md)  
- Lisätietoja käyttämisestä automaattisen varmuuskopioinnin arkistointia varten, tutustu artikkeliin [tietokannan kopioiminen](sql-database-copy.md)
