<properties
   pageTitle="Määritä SQL server palomuuri-yleiskatsaus | Microsoft Azure"
   description="Opettele määrittämään SQL-tietokantaan palomuurin kanssa palvelintason ja tietokannan tason palomuurisäännöt voit hallita käyttöoikeuksia."
   keywords="tietokannan palomuurin"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Määritä Azure SQL-tietokantaan palomuurisäännöt \- yleiskatsaus


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShellin](sql-database-configure-firewall-settings-powershell.md)
- [REST-OHJELMOINTIRAJAPINNALLA](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-tietokanta on relaatiotietokannasta palvelu Azure ja Internet-pohjaisia sovelluksia. Jos haluat ehkäistä tietojen palomuurit Estä kaikki käyttöä tietokantapalvelin, ennen kuin määrität tietokoneet, joilla on käyttöoikeudet. Palomuuri antaa tietokantoja sivupyynnön alkuperäiseen IP-osoitteen perusteella.

Määrittää palomuurin, voit luoda palomuurisäännöt, jotka määrittävät alueiden hyväksyttävä IP-osoitteet. Voit luoda palomuurisäännöt tasoilla palvelin ja tietokanta.

- **Palvelintason palomuurisäännöt:** Sääntöjen avulla asiakkaat voivat käyttää koko Azure SQL-palvelimeen, kaikki tietokannat saman loogisen Serverissä. Säännöt on tallennettu **tietokannasta** . Palvelintason palomuurisäännöt voi määrittää portaalissa tai käyttämällä Transact-SQL-lauseita.
- **Tietokannan tason palomuurisäännöt:** Sääntöjen avulla asiakkaat voivat käyttää yksittäisiä tietokannoista Azure SQL-tietokanta-palvelin. Voit luoda sääntöjen kunkin tietokannan, ja ne on tallennettu yksittäiset tietokannat. (Voit luoda **perusmuodon** tietokannan tietokannan tason palomuurisäännöt.) Sääntöjen voi olla hyötyä käytön rajoittaminen tiettyihin (Suojaus) tietokantoihin saman loogisen Serverissä. Tietokannan tason palomuurisäännöt voidaan määrittää vain käyttämällä Transact-SQL-lauseita.

**Suositus:** Microsoft suosittelee käyttämällä tietokannan tason palomuurisäännöt aina, kun se on mahdollista, tietoturvan ja tehdä tietokannan paremmin siirrettäviä. Käytä palvelintason palomuurisäännöt järjestelmänvalvojille ja, kun useita tietokantoja, joissa on samat access-vaatimukset ja joita et halua käyttää aikaa määrittäminen tietokantojen yksitellen.


## <a name="firewall-overview"></a>Palomuurin yleiskatsaus

Aluksi palomuuri estää kaikki Transact-SQL-access Azure SQL-palvelimeen. Kun haluat käyttää Azure SQL-palvelimeen, siirry Azure-portaaliin ja määritä vähintään yksi palvelintason palomuurisäännöt, jotka mahdollistavat access Azure SQL-palvelimeen. Palomuurisäännöt avulla voit määrittää, mitkä IP-osoitealueet Internetistä sallitaan ja onko Azure sovellukset voivat yrittää Azure SQL-palvelimeen.

Vain yksi Azure SQL server-tietokannan käyttöoikeuden myöntäminen valikoivasti, sinun on luotava tarvittavat tietokannan tietokannan tason säännön. Määritä IP-osoitealueita tietokantaan palomuurin säännön, joka on määritetty palvelintason palomuurin säännön IP-osoitealueita lisäksi ja varmista, että asiakkaan IP-osoite kuuluu tietokannan tason säännön määritetyn alueen.

Yhteyden yritykset Internetistä ja Azure on ensin monen palomuurin läpi ne saapuvat Azure SQL server- tai SQL-tietokantaan, kuten seuraavassa kaaviossa on esitetty.

   ![Kaavio, joka kuvaa palomuurin määrityksen.][1]

## <a name="connecting-from-the-internet"></a>Internet-yhteyden

Kun tietokone yrittää muodostaa Internet-tietokantapalvelin, palomuurin tarkistaa ensin alkuperäiseen pyynnön vastaan täydellisen luettelon palomuurisäännöt IP-osoite:

- Jos pyynnön IP-osoite on yksi palvelintason palomuurisäännöt määritettyjä alueita, yhteys on myönnetty Azure SQL-tietokanta-palvelimeen.

- Jos pyynnön IP-osoite ei ole johonkin palvelintason palomuurin säännössä määritetyn alueita, tietokannan tason palomuurisäännöt ovat valittuina. Jos pyynnön IP-osoite on yksi tietokanta tason palomuurisäännöt-parametrissa alueet, yhteys myönnetään vain tietokanta, joka sisältää vastaavat tietokannan tason säännön.

- Jos pyynnön IP-osoite ei ole määritetty kaikista palvelintason tai tietokannan tason palomuurisäännöt-yhteyden pyynnön epäonnistuu alueet.

> [AZURE.NOTE] Jos haluat käyttää Azure SQL-tietokanta paikallisesta tietokoneesta, varmistaa verkon ja paikallisen tietokoneen palomuuri antaa lähtevän tietoliikenteen TCP-porttia 1433.


## <a name="connecting-from-azure"></a>Azure yhdistäminen

Kun azuren sovellus yrittää muodostaa tietokantapalvelin, palomuurin tarkistaa, että Azure yhteydet sallitaan. Palomuurin asetus alkamis- ja 0.0.0.0 sama osoite osoittaa asiakasyhteydet sallitaan. Jos yhteyden muodostaminen ei sallita, pyyntö ei ulotu Azure SQL-tietokantapalvelimeen.

> [AZURE.IMPORTANT] Tämä asetus määrittää palomuurin siten, että kaikki yhteydet Azure mukaan lukien yhteydet muiden asiakkaiden tilaukset. Kun valitset tämän vaihtoehdon, varmista, että kirjautumisen ja käyttöoikeuksien rajoittaminen vain valtuutettujen käyttäjien pääsyn.

Voit ottaa Azure yhteydet kahdella tavalla:

- [Microsoft Azure-portaalissa](https://portal.azure.com/)Valitse **Salli azure services, access-palvelimeen** -valintaruutu, kun luot uuden palvelimen.

- Valitse [perinteisessä portal](http://go.microsoft.com/fwlink/p/?LinkID=161793)-palvelimessa, **Sallittu palvelut** -kohdassa **Määritä** -välilehdestä Valitse **Microsoft Azure**-palveluiden **Kyllä** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Ensimmäinen palvelintason palomuurisäännön luominen

Ensimmäinen palvelintason palomuuriasetus voidaan luoda [Azure portal](https://portal.azure.com/) tai REST API tai PowerShellin Azure ohjelmallisesti käyttämällä. Myöhemmille palvelintason palomuurisäännöt voidaan luoda ja hallita käyttämällä seuraavista tavoista kuin Transact-SQL kautta. Voit parantaa suorituskykyä palvelintason palomuurisäännöt väliaikaisesti välimuistin tietokannan tasolla. Välimuistin päivittäminen on artikkelissa [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Katso lisätietoja palvelintason palomuurisäännöt [Toimintaohje: määrittää Azure SQL server-palomuurin Azure-portaalissa](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Tietokannan tason Palomuurisääntöjen luominen

Kun olet määrittänyt ensimmäisen palvelintason palomuurin, haluat ehkä käytön rajoittaminen tiettyihin tietokannat. Jos määrität IP-osoitealueita tietokannan tason palomuurisääntö, joka on määritetty palvelintason palomuurin säännön alueen ulkopuolella, vain asiakkaat, joilla IP-osoitteiden tietokannan tason alueen käyttää tietokantaa. Käytössä voi olla enintään 128 tietokannan tason palomuurisäännöt tietokantaan. Tietokannan tason palomuurisäännöt tietokantojen perustyyli ja käyttäjä voi luoda ja Transact-SQL hallitaan. Lisätietoja tietokannan tason palomuurisäännöt määrittämisestä on artikkelissa [sp_set_database_firewall_rule (Azure SQL-tietokannat)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Hallinta ohjelmallisesti palomuurisäännöt

Lisäksi Azure-portaaliin palomuurisäännöt voi hallita ohjelmallisesti käyttämällä Transact-SQL REST API ja PowerShellin Azure. Seuraavissa taulukoissa kuvataan olevat komennot palauttamista varten.


### <a name="transact-sql"></a>Transact-SQL

| Luettelo-näkymä tai tallennettu toimintosarja                                                           | Taso     | Kuvaus                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Palvelin    | Näyttää nykyisen palvelintason palomuurisäännöt     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Palvelin    | Luo tai päivittää palvelintason palomuurisäännöt       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Palvelin    | Poistaa palvelintason palomuurisäännöt                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Tietokannan  | Näyttää nykyisen tietokannan tason palomuurisäännöt   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Tietokannan  | Luo tai päivittää tietokannan tason palomuurisäännöt |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Tietokannat | Poistaa tietokannan tason palomuurisäännöt                |

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

| OHJELMOINTIRAJAPINTA                  | Taso  | Kuvaus                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Luettelon palomuurisäännöt](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Palvelin | Näyttää nykyisen palvelintason palomuurisäännöt                 |
| [Palomuurisäännön luominen](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Palvelin | Luo tai päivittää palvelintason palomuurisäännöt                   |
| [Palomuurin sääntöjoukon](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Palvelin | Päivittää nykyisen palvelintason palomuurisäännön ominaisuudet |
| [Palomuurisäännön poistaminen](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Palvelin | Poistaa palvelintason palomuurisäännöt                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet-komento                                                                                              | Taso  | Kuvaus                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Hae AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Palvelin | Palauttaa nykyisen palvelintason palomuurisäännöt                  |
| [Uusi AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Palvelin | Luo uusi palvelintason palomuurisääntö                         |
| [Määritä AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Palvelin | Päivittää nykyisen palvelintason palomuurisäännön ominaisuudet |
| [Poista AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Palvelin | Poistaa palvelintason palomuurisäännöt                              |

> [AZURE.NOTE] Voi olla määrittäminen mahdollisimman paljon viiden minuutin viive muutokset tulevat voimaan palomuuriasetukset.

## <a name="troubleshooting-the-database-firewall"></a>Tietokannan palomuurin vianmääritys

Kun käyttämään Microsoft Azure SQL-tietokanta-palvelua ei toimi odotetusti, ota huomioon seuraavat seikat:

- **Paikallisen palomuurin:** Ennen kuin tietokoneen voi käyttää Azure SQL-tietokantaan, saatat joutua Luo palomuuripoikkeus TCP-portin 1433 tietokoneeseen. Jos teet Azure cloud reunan sisällä yhteyksiä, saatat joutua Avaa muita portit. Lisätietoja on artikkelissa **Vipuventtiili V12 SQL-tietokantaan: ulkopuolella ja** [portit lisäksi 1433 ADO.NET 4.5 ja SQL-tietokannan Vipuventtiili V12](sql-database-develop-direct-route-ports-adonet-v12.md)-osassa.

- **Verkko-osoitteiden muuntaminen (NAT):** Vuoksi NAT-IP-osoite, tietokoneen käyttämä Azure SQL-tietokannan muodostaminen voi olla eri kuin tietokoneen IP-määritysasetukset näkyvät IP-osoite. Voit tarkastella tietokoneen käyttämän muodostaa Azure IP-osoite, kirjaudu sisään-portaaliin ja siirry tietokannan isännöivän palvelimen **määritys** -välilehti. **Sallitut IP-osoitteet** -kohdassa näkyy **Nykyisen asiakkaan IP-osoite** . Valitse **Lisää** **Sallitut IP-osoitteet** , voit sallia tämän tietokoneen yhteyden palvelimeen.

- **Tehosteen ole tehnyt muutokset sallittujen vielä:** Ehkä mahdollisimman paljon viiden minuutin viive, muutokset tulevat voimaan Azure SQL-tietokantaan palomuurin-määrityksiin.

- **Kirjautuminen ei ole oikeuksia tai väärän salasanan käytettiin:** Jos kirjautumistunnus ei ole oikeuksia Azure SQL-tietokantapalvelinta tai salasana, jolla on virheellinen, Azure SQL-tietokantapalvelin yhteyden on estetty. Asiakkaat, joilla mahdollisuuden yrittää yhteyden palvelimeen, sisältää vain luominen palomuuriasetus Kukin asiakas on määritettävä tarvittavat suojausvaltuudet. Saat lisätietoja valmistellaan kirjautumiset tietokantojen hallinta ja kirjautumiset käyttäjien Azure SQL-tietokantaan.

- **Dynaaminen IP-osoite:** Jos sinulla on Internet-yhteys, jonka dynaaminen IP-osoitteiden ja sinulla on ongelmia palomuurin läpi, voi jokin seuraavista toimista:

 - Pyydä Internet-palveluntarjoajan (ISP), joka käyttää Azure SQL-tietokantapalvelinta asiakastietokoneiden myönnetyt IP-osoitealueita ja lisäämällä sitten IP-osoitealueita palomuurisäännön.

 - Hanki staattinen IP-osoitteista sen sijaan asiakastietokoneiden ja lisää sitten palomuurisäännöt IP-osoitteet.

## <a name="next-steps"></a>Seuraavat vaiheet

Miten artikkeleita palvelintason ja tietokannan tason palomuurisäännöt luomisesta on artikkelissa:

- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt Azure-portaalissa](sql-database-configure-firewall-settings.md)
- [Määritä käyttämällä T-SQL Azure SQL-tietokanta palvelintason ja tietokannan tason palomuurisäännöt](sql-database-configure-firewall-settings-tsql.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt PowerShellin avulla](sql-database-configure-firewall-settings-powershell.md)
- [Määritä Azure SQL-tietokanta palvelintason palomuurisäännöt REST-Ohjelmointirajapinnan käyttäminen](sql-database-configure-firewall-settings-rest.md)

Opetusohjelmaan-tietokannan luominen-kohdassa [luominen Azure-portaalissa minuutteina SQL-tietokantaan](sql-database-get-started.md).
Lisätietoja yhteyden muodostamisesta Azure SQL-tietokanta Avaa lähde- tai kolmansien osapuolien sovellukset on [asiakkaan Pika-aloitus MALLIKOODEJA SQL-tietokantaan](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Siirtyminen tietokantoja on artikkelissa [tietokannan käytön ja kirjaudu sisään tietoturvan hallintaa](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Lisäresursseja

- [Tietokannan suojaaminen](sql-database-security.md)
- [Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
