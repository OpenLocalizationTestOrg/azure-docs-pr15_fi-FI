<properties
   pageTitle="Yleistä SQL-tietokannan suojauksesta"
   description="Lisätietoja Azure SQL-tietokantaan ja SQL Serverin suojaus, mukaan lukien pilveen erot ja SQL Server-ympäristöön sen tullessa todennus, todennus, yhteyden suojaus, salaus ja yhteensopivuuden."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>SQL-tietokannan suojaaminen

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa käydään läpi perusteet suojaaminen Azure SQL-tietokanta-sovellus tietojen taso. Erityisesti tämän artikkelin pääset alkuun resurssit rajoittaminen access-tietojen suojaaminen ja valvonta aktiviteetteja tietokannan luonut [SQL-tietokantaan opetusohjelman käytön aloittaminen](sql-database-get-started.md). Katso yleiskuvaus suojausominaisuuksia, jotka ovat käytettävissä kaikissa flavors SQL- [Tietoturvakeskus SQL Server-tietokantamoduuli ja Azure SQL-tietokanta](https://msdn.microsoft.com/library/bb510589). Lisätietoja on myös käytettävissä [Suojaus ja Azure SQL-tietokanta tekninen julkaisu](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Yhteyden suojaus

Yhteyden suojaus viittaa siitä, miten voit rajata ja palomuurisäännöt ja yhteyden salaus tietokannan suojattuja yhteyksiä.

Palvelin ja tietokanta käytetään palomuurisäännöt IP-osoitteista, joita ei ole nimenomaisesti whitelisted yhteyden yritykset Hylkää. Jotta sovelluksesi tai asiakkaan tietokoneen julkiseen IP-osoite, yritä muodostaa yhteyden uuteen tietokantaan, sinun on luotava palvelintason palomuurisäännön Azure perinteinen Portal, REST API tai PowerShellin avulla. Paras käytäntö tulee rajoita sallittu server-palomuurin läpi mahdollisimman paljon IP-osoitealueet. Lisätietoja on artikkelissa [Azure SQL-tietokantaan palomuurin](https://msdn.microsoft.com/library/ee621782).

Kaikkien yhteyksien Azure SQL-tietokanta salata (SSL/TLS) on aina samalla, kun tiedot on "siirrossa" ja tietokannasta. Sovelluksen yhteysmerkkijonossa, sinun on määritettävä parametrit salaa yhteys ja *ei* luottaminen palvelinvarmennetta (tämä on valmiiksi Jos kopioit yhteysmerkkijono Azure perinteinen portaalin ulos), muutoin yhteyden eivät Vahvista palvelimen henkilöllisyys ja ne ovat alttiita "mies---keskiosan" kalastelu. ADO.NET-ohjainta, esimerkiksi nämä yhteyden kyselyparametri ovat **Salaa = True** ja **TrustServerCertificate = False**. Lisätietoja on artikkelissa [Azure SQL-tietokannan yhteys-salaus ja varmenne](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Todennus

Todennus viittaa siitä, miten voit todistaa henkilöllisyytesi, kun tietokantayhteyden muodostamisessa. SQL-tietokanta tukee kahdentyyppisiä todennus:

 - **SQL-todennusta**, joka käyttää käyttäjänimi ja salasana
 - **Azure Active Directory-todennuksen**, joka käyttää käyttäjätietojen hallitsee Azure Active Directory ja hallittujen ja integroitu tuetaan

Luodessasi looginen palvelimen tietokannan määrittämäsi "palvelimen järjestelmänvalvojan" kirjautumisen käyttäjänimellä ja salasanalla. Käytä näitä tunnistetietoja, voi todentaa minkä tahansa tietokannan palvelimen tietokannan omistaja tai "dbo." Jos haluat käyttää Azure Active Directory-todennus, sinun on luotava toisen palvelimen järjestelmänvalvojan nimeltä "Azure AD järjestelmänvalvoja," mikä on sallittua hallinnoimaan Azure AD-käyttäjät ja ryhmät. Tämä järjestelmänvalvoja voi tehdä myös kaikki toiminnot, jotka säännöllisesti palvelimen järjestelmänvalvoja voi. [Yhteyden muodostaminen SQL tietokanta mukaan käyttämällä Azure Active Directory-todennusta](sql-database-aad-authentication.md) saat luominen Azure AD-järjestelmänvalvojan Azure Active Directory-todennus käyttöön vaiheista.

Paras käytäntö sovelluksen pitäisi käyttää eri tilillä tarkistamiseen – tällä tavalla voit rajoittaa sovelluksen käyttöoikeudet ja vähentää haittaohjelmien tehtävän riskit, jos sovelluksen koodin ei koske SQL-lisäämisen hyökkäyksen. Suositellaan on luotava [sisälsi tietokannan käyttäjä](https://msdn.microsoft.com/library/ff929188), joka sallii sovelluksen todentaa suoraan yksittäiseen tietokantaan. Voit luoda suorittamalla seuraava T-SQL-komento yhdistettynä käyttäjän tietokannan kuin palvelimen järjestelmänvalvojan kirjautuminen SQL-todennusta käyttävä sisältämät tietokannan käyttäjä:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Jos olet luonut Azure AD-järjestelmänvalvoja, voit luoda sisältämät tietokannan-käyttäjä, joka käyttää Azure Active Directory käyttöoikeuksien suorittamalla seuraava T-SQL-komento yhdistettynä käyttäjän tietokannan Azure AD-järjestelmänvalvoja:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

Kummassakin tapauksessa sovelluksesi yhteysmerkkijonon kannattaa määrittää nämä käyttäjätietoja palvelimen järjestelmänvalvojan kirjautuminen, muodosta yhteys tietokantaan sijaan.

Saat lisätietoja todennustapa SQL-tietokantaan, [hallinta ja kirjautumiset Azure SQL-tietokantaan](sql-database-manage-logins.md).


## <a name="authorization"></a>Todennus
Luvan viittaa toiminnot-Azure SQL-tietokanta ja tämä määräytyy käyttäjätilisi roolien jäsenyys ja käyttöoikeudet. Paras käytäntö tulee Myönnä käyttäjille vähintään tarvittavat oikeudet. Azure SQL-tietokanta on tämä T-SQL-roolien hallinta on helppoa:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Palvelimen järjestelmänvalvoja-tili, johon muodostat yhteyden kuuluu db_owner, jolla on oikeus tehdä mitään tietokannasta. Tallenna rakenne päivitykset ja muut hallintatoiminnot käyttöönoton tähän tiliin. Sovelluksen tarvitsemia vähimmäiskäyttöoikeuksin yhteydessä tietokantaan sovelluksestasi Lisää rajoitetuilla käyttöoikeuksilla "ApplicationUser"-tilin avulla.

Usealla tavalla voit rajata mitä käyttäjä voi tehdä Azure SQL-tietokantaan:

* [Tietokannan rooleja](https://msdn.microsoft.com/library/ms189121) kuin db_datareader ja db_datawriter voidaan luoda tehokkaampia sovelluksen käyttäjätilit tai vähemmän tehokas hallinta-tilit.
* Hajautetun [käyttöoikeuksien](https://msdn.microsoft.com/library/ms191291) avulla voit määrittää, mitkä toiminnot Tee yksittäiset sarakkeet, taulukoiden, näkymien, menettelyt ja muihin tietokannan objekteihin.
* [Tekeytyminen](https://msdn.microsoft.com/library/vstudio/bb669087) ja [moduulin allekirjoituksessa](https://msdn.microsoft.com/library/bb669102) voidaan turvallisesti käyttöoikeuksien laajentamista tilapäisesti.
* [Rivin käyttäjätason suojauksen](https://msdn.microsoft.com/library/dn765131) voidaan raja, jonka rivit käyttäjän käyttöoikeus.
* [Tietojen Masking](sql-database-dynamic-data-masking-get-started.md) voidaan rajoittaa luottamuksellisten tietojen näyttäminen.
* [Tallennettu toimintosarja](https://msdn.microsoft.com/library/ms190782) voidaan rajoittaa toimintoja, jotka voidaan ottaa käyttöön tietokantaa.

Tietokantojen ja looginen palvelinten hallinta perinteinen Azure-portaalista tai Azure Resurssienhallinta-Ohjelmointirajapinnan käyttäminen ohjataan portaalin käyttäjätilin roolimäärityksiä. Lisätietoja tämän artikkelin kohdassa [Roolipohjainen käyttöoikeuksien valvonta Azure-portaalissa](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Salaus

Azure SQL-tietokantaan auttaa suojaamaan tietosi salaamalla tiedot, kun se on "muut" tai tallennettu tietokantatiedostoja ja varmuuskopioiden, [Läpinäkyvä salauksen](http://go.microsoft.com/fwlink/?LinkId=526242)avulla. Tietokannan salaaminen, tietokannan omistajan yhdistää ja suorittaa:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Muita tapoja salaamaan tiedot tietoja Ota huomioon:

* [Solun tason salaus](https://msdn.microsoft.com/library/ms179331.aspx) tietyt sarakkeet tai jopa solujen tietojen kanssa eri salausavaimet.
* Jos tarvitset laitteiston suojauksen moduuli tai keskitetyn hallinnan avaimen salaus-hierarkian, kannattaa käyttää [Azure avaimen säilö SQL Server Azure-AM kanssa](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Salattu aina](https://msdn.microsoft.com/library/mt163865.aspx) (ennakkoversio) mahdollistaa salauksen läpinäkyvä sovelluksia ja sallii asiakkaiden salata asiakassovelluksissa sisällä arkaluontoisia tietoja ilman salausavaimet jakamisen SQL-tietokantaan.

## <a name="auditing"></a>Valvonta

Valvonta- ja tietokannan tapahtumien seuranta auttaa sinua säilyttää säädösten noudattamisen ja epäilyttävistä tehtävän määritys. SQL-tietokannan tarkistaminen avulla voit kirjata tapahtumia tarkastus tietokannan Kirjaudu tilisi Azure-tallennustilan. Microsoft Power BI helpottamiseksi alirakenteen raportteja ja analyyseja myös SQL-tietokannan tarkistaminen integroitu. Lisätietoja on artikkelissa [valvonta SQL-tietokannan käytön aloittaminen](sql-database-auditing-get-started.md).

SQL-tietokannan uhkien tunnistus antaa suojauksen valvonta päälle. Sen avulla voit vastata uhkien ilmenee tarjoamalla suojausvaroitusten erheellisiin toimiin. Lisätietoja on artikkelissa [SQL-tietokannan uhkien tunnistus käytön aloittaminen](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Yhteensopivuus

Lisäksi edellä ominaisuuksia ja toimintoja, jotka auttavat täyttävät eri suojaus-yhteensopivuusvaatimukset, Azure SQL-tietokanta myös sovelluksen osallistuu säännöllisesti auditointeja ja varmennettu vastaan yhteensopivuusstandardeja määrä. Lisätietoja on artikkelissa [Microsoft Azure-Valvontakeskus](https://azure.microsoft.com/support/trust-center/), josta löydät [SQL-tietokantaan yhteensopivuuden kaltaisilta](https://azure.microsoft.com/support/trust-center/services/)uusimmat luettelo.
