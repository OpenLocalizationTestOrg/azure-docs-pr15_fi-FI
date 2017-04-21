Tietokannan tapahtuman yksikkö (DTU) on SQL-tietokantaan, joka vastaa tosielämän mitta perustuvien tietokantojen suhteellinen power mittayksikön: tietokannan tapahtuma. Olemme noudatit joukko toimintoja, jotka ovat tyypillisiä online tapahtuman (OLTP)-pyynnön ja mitataan kuinka monta tapahtumaa onnistunut sekunnissa, täysin ladattu ehdot (eli tiivistettynä, voit lukea luokan tiedot [ensisijainen yleiskatsaus](../articles/sql-database/sql-database-benchmark-overview.md)). 

Esimerkiksi Premium P11 tietokanta on 1750 DTUs sisältää Lisää DTU x 350 Laske power kuin tavallinen tietokanta on 5 DTUs. 

![Johdanto SQL-tietokantaan: yksi tietokannan DTUs taso ja taso.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Jos olet siirtymässä aiemmin luotuun SQL Server-tietokantaan, voit kolmannen osapuolen-työkalun [Azure SQL tietokanta DTU Laskimen](http://dtucalculator.azurewebsites.net/), voit hakea arvio suorituskyky ja palvelun taso tietokannan voi edellyttää Azure SQL-tietokantaan.

### <a name="dtu-vs-edtu"></a>DTU ja eDTU

Yksittäisen tietokantojen DTU vastaa suoraan joustavasti tietokantojen eDTU. Esimerkiksi Basic joustavasti tietokannan resurssivarantoon tietokannan on enintään 5 eDTUs. Tämä on sama suorituskyky yhden Basic tietokantana. Ero on joustavasti tietokantaan ei tarjoaman minkä tahansa eDTUs olevilla, kunnes se on. 

![Johdanto SQL-tietokantaan: joustavasti jakavat tason mukaan.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Esimerkki avulla. Ota perus joustavasti tietokannan resurssivarantoon 1000 DTUs kanssa ja pudota se 800 tietokannat. Kun vain 200 800 tietokantojen käytetään milloin tahansa aika (5 DTU X 200 = 1 000), ei valitset kapasiteetin altaan ja tietokannan suorituskykyä ei heikentää. Tässä esimerkissä on yksinkertaistettu selkeyttä varten. Todellinen matemaattiset on hieman enemmän aikaa. Portaalin tekee laskutoimitukset puolestasi ja tekee suositus tietokanta käyttö perusteella. Katso [hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan resurssivarantoon](../articles/sql-database/sql-database-elastic-pool-guidance.md) lisätietoja toiminta suosituksia tai laskemaan itse. 
