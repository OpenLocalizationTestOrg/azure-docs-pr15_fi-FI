<properties
   pageTitle="Yhteyden muodostaminen SQL-tietokanta tai SQL-tietovarasto Azure Active Directory-todennuksen avulla | Microsoft Azure"
   description="Opettele muodostamaan yhteys SQL-tietokantaan Azure Active Directory-todennuksen avulla."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Yhteyden muodostaminen SQL-tietokanta tai SQL-tietovarasto Azure Active Directory-todennuksen avulla

Azure Active Directory-todennus on puitteissa yhteyden muodostaminen Microsoft Azure SQL-tietokantaan ja [SQL-tietovarasto](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) avulla käyttäjätietojen Azure Active Directory (Azure AD). Azure Active Directory-todennuksen avulla voit hallita keskitetysti tietokannan käyttäjien käyttäjätietojen ja muiden Microsoftin palvelujen yhdessä keskitetyssä paikassa. Tunnus keskitetysti keskitetysti hallita tietokannan käyttäjiä ja yksinkertaistaa käyttöoikeuksien hallinta. Etuja ovat seuraavat:

- Se on SQL Server-todennus löytämiseen.
- Auttaa lopettaa käyttäjätietojen koskevia tietokannan palvelinten.
- Yhteen paikkaan kierto salasanan avulla
- Asiakkaat voivat hallita tietokannan käyttöoikeuksia ulkoisen (AAD)-ryhmien käyttäminen.
- Se voi poistaa tallentamista salasanat ottamalla Windows-todennusta ja muilta Azure Active Directory tukemista.
- Azure Active Directory-todennusta käyttävä sisälsi tietokannan käyttäjien todentamiseen käyttäjätietojen tietokanta tasolla.
- Azure Active Directory tukee tunnuksen todennuksen yhteyden muodostaminen SQL-tietokantaan.
- Azure Active Directory-todennus tukee ADFS (toimialueliittäminen) tai alkuperäisen käyttäjänimen todennus paikallinen Azure Active Directory ilman toimialueen synkronointi.  
- Azure Active Directory tukee SQL Server Management Studiossa yhteydet, joka käyttää Active Directory yleinen todennusta, joka sisältää multi-factor Authentication (MFA).  MFA sisältää vahva todennus helposti vahvistus asetukset-alue, puhelu, tekstiviesti, älykortit PIN-tunnus tai mobiilisovelluksessa ilmoitus. Lisätietoja on artikkelissa [Azure AD-MFA SQL-tietokantaan ja SQL-tietovarasto tuki SSMS](sql-database-ssms-mfa-authentication.md).

Määritysvaiheet ovat määrittäminen ja käyttäminen Azure Active Directory käyttöoikeuksien noudattamalla seuraavia ohjeita.

1. Luo ja täytä Azure Active Directory.
2. Varmista tietokannan on Azure SQL-tietokannan Vipuventtiili V12. (Ei tarvita SQL-tietovarasto.)
3. Valinnainen: Liitä tai muuta active directory, joka on liitetty Azure-tilauksen.
4. Azure Active Directory-järjestelmänvalvojan luominen Azure SQL Serverin tai [SQL Azure-tietovarasto](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Määritä asiakastietokoneiden.
6. Luo sisältämät tietokannan käyttäjien käyttäjätietojen Azure AD määritetty tietokannan.
7. Muodosta yhteys tietokantaan käyttämällä Azure AD-käyttäjätietoja.


## <a name="trust-architecture"></a>Luota arkkitehtuuri

Ylätason seuraavassa kaaviossa on yhteenveto ratkaisuarkkitehtuuri Azure AD-todennuksen avulla Azure SQL-tietokantaan. SQL-tietovarasto käyttää samaa menetelmää. Tukemaan Azure Active Directory alkuperäisen käyttäjän salasanan pidetään vain Cloud-osan ja Azure AD/Azure SQL-tietokanta. Organisaation ulkopuolisten todennus (tai/salasanaa Windows-tunnistetiedot) ADFS palkilla tietoliikenne. Nuolet osoittavat viestintä kulkeutumisreittien.

![aad auth kaavio][1]

Seuraavassa kaaviossa ilmaisee federaatio, luota ja isännöintipalvelu yhteyksiä, joiden avulla voit muodostaa yhteyden tietokantaan lähettämällä tunnusta asiakas. Tunnuksen voi todentaa Azure AD ja luottaa tietokantaan. Asiakkaan 1 kuvaa Azure Active Directory alkuperäisen käyttäjien kanssa tai Azure Active Directory liitetyt käyttäjien kanssa. Asiakkaan 2 on mahdollinen ratkaisu, mukaan lukien tuodut käyttäjille. Tässä esimerkissä on tulossa liitetty Azure Active Directorysta ADFS Azure Active Directorysta synkronoinnin aikana. On tärkeää ymmärtää, että tietokanta käyttämällä Azure AD-todennus käyttö edellyttää, että isännöintipalvelu tilaukseen on liitetty Azure Active Directory. Luo SQL Server Azure SQL-tietokanta tai SQL-tietovarasto isännöivän on käytettävä saman tilauksen.

![tilauksen suhde][2]

## <a name="administrator-structure"></a>Järjestelmänvalvoja-rakenne

Käytettäessä Azure AD-todennusta on kaksi Järjestelmänvalvojatileille SQL-tietokantapalvelimen; alkuperäinen SQL Server-järjestelmänvalvoja ja Azure AD-järjestelmänvalvojaan. SQL-tietovarasto käyttää samaa menetelmää. Ainoastaan Azure AD-asiakkaaseen perustuva järjestelmänvalvoja, voit luoda ensimmäisen sisälsi Azure AD-tietokannan käyttäjä käyttäjän tietokannan. Kirjautuminen Azure AD-järjestelmänvalvoja voi olla Azure AD-käyttäjä tai Azure AD-ryhmä. Kun järjestelmänvalvoja on ryhmätilin, se voidaan käyttää minkä tahansa ryhmäjäsen, ottaminen käyttöön useita Azure AD-järjestelmänvalvojia SQL Server-esiintymän. Ryhmän tilillä järjestelmänvalvojana parantaa hallittavuuden sallimalla keskitetysti lisätä ja poistaa jäseniä Azure AD muuttamatta käyttäjät ja käyttöoikeudet SQL-tietokantaan. Vain yksi Azure AD-järjestelmänvalvoja (käyttäjän tai ryhmän) voi määrittää milloin tahansa.

![Järjestelmänvalvoja-rakenne][3]

## <a name="permissions"></a>Käyttöoikeudet

Jos haluat luoda uusia käyttäjiä, sinulla on oltava `ALTER ANY USER` tietokannan käyttöoikeudet. `ALTER ANY USER` Tietokannan kaikki käyttäjät voidaan myöntää käyttöoikeuksia. `ALTER ANY USER` Käyttöoikeudet pidetään myös palvelimen Järjestelmänvalvojatileille ja tietokannan käyttäjien kanssa `CONTROL ON DATABASE` tai `ALTER ON DATABASE` käyttöoikeudet tietokannan ja jäsenet `db_owner` tietokannan rooli.

Luo sisältämät tietokannan käyttäjä Azure SQL-tietokanta tai SQL-tietovarasto, yhdistäminen tietokantaan Azure AD-tunnistetietojen avulla. Luo ensimmäinen sisältämät tietokannan käyttäjä on muodostaa tietokannan Azure Active Directory-järjestelmänvalvoja (joka on tietokannan omistaja) avulla. Tämä on selitetty vaiheet 4 ja 5 ohjeita. Minkä tahansa Azure Active Directory-todennus voi vain, jos Azure Active Directory-järjestelmänvalvoja on luonut Azure SQL-tietokanta tai tietovarasto SQL Serverin. Jos Azure Active Directory-järjestelmänvalvoja on poistanut palvelimesta, aiemmin luotu Azure Active Directory-käyttäjien aiemmin sisällä SQL Server ei enää muodostaa yhteyden tietokantaan avulla Azure Active Directory-tunnistetietoja.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-ominaisuudet ja rajoitukset

Azure Active Directory seuraavat jäsenet voivat valmisteltava Azure SQL Server- tai SQL-tietovarasto:

- Alkuperäisen jäsenet: luotu Azure AD hallitun toimialueen tai asiakkaan toimialueen jäsen. Lisätietoja on artikkelissa [Azure AD oman toimialuenimi](../active-directory/active-directory-add-domain.md).

- Liitetty toimialueen jäsenet: Azure AD kanssa liitetyssä toimialueessa luotu jäsen. Lisätietoja on artikkelissa [Microsoft Azure nyt tukee Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Tuodut jäsenet muiden Azure Active kansioista alkuperäisen tai liitetyt toimialuejäsenten käyttävien.

- Käyttöoikeusryhmät luodaan Active Directory-ryhmä.

Microsoft-tilit (esimerkiksi outlook.com, hotmail.com, live.com) tai muut vieraiden tilit (esimerkiksi gmail.com, Yahoo.comia) eivät ole tuettuja. Voit kirjautua [https://login.live.com](https://login.live.com) käyttämällä tili ja salasana, jos käyttämäsi Microsoft-tiliä, jota ei tueta Azure SQL-tietokanta tai SQL Azure-tietovarasto Azure AD-todennusta varten.

### <a name="additional-considerations"></a>Muita huomioon otettavia seikkoja

- Hallittavuuden parantamiseksi on suositeltavaa valmistella erillinen Azure Active Directory-ryhmän järjestelmänvalvojan oikeuksilla.
- Vain yksi Azure AD-järjestelmänvalvoja (käyttäjän tai ryhmän) voidaan määrittää Azure SQL Server- tai SQL Azure-tietovarasto milloin tahansa.
- Vain SQL Server Azure Active Directory-järjestelmänvalvojan alun perin muodostaa yhteyden Azure SQL Server- tai Azure SQL-tietovarasto Azure Active Directory-tilin avulla. Active Directory-järjestelmänvalvoja voi määrittää seuraavat Azure Active Directory-tietokannan käyttäjät.
- On suositeltavaa yhteyden aikakatkaisu asettaminen 30 sekuntia.
- SQL Server 2016 Management Studiossa ja SQL Server Data Tools for Visual Studio 2015 (2016 tai uudempi versio 14.0.60311.1April) tuetaan Azure Active Directory-todentamista. (Azure Active Directory-todennus on tuettu **SQL Server .NET Framework-tietopalvelu**; osoitteessa vähintään versio .NET Framework 4.6). Tämän vuoksi työkaluilla ja (DAC ja .bacpac) tietojen tason sovellusten uusimmat versiot käyttää Azure Active Directory-todennusta.
- [ODBC-version 13.1](https://www.microsoft.com/download/details.aspx?id=53339) tukee Azure Active Directory käyttöoikeuksien `bcp.exe` ei voi muodostaa yhteyden Azure Active Directory käyttöoikeuksien, koska se käyttää vanhoja ODBC-palvelua.
- `sqlcmd`tukee Azure Active Directory käyttöoikeuksien alkaa sanalla versio 13.1 voi [Download Centeristä](http://go.microsoft.com/fwlink/?LinkID=825643).  
- SQL Server Data Tools for Visual Studio 2015 edellyttää vähintään Datatyökalut (versio 14.0.60311.1) huhtikuun 2016-versiota. Tällä hetkellä, Azure Active Directory-käyttäjät eivät näy SSDT Object Explorer. Voit kiertää tämän ongelman tarkastella käyttäjät [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [Microsoft SQL Server JDBC ohjaimen 6.0](https://www.microsoft.com/en-us/download/details.aspx?id=11774) tukee Azure Active Directory-todennusta. Katso myös [yhteyden ominaisuuksien määrittäminen](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase ei voi todentaa Azure Active Directory-todennuksen avulla.
- Jotkin työkalut BI ja Excelin kaltaisten ei tueta.
- Azure portaalin **Tietokannan tuominen** ja **Vieminen tietokannan** näiden tue Azure Active Directory käyttöoikeuksien SQL-tietokantaan. Tuo ja vie Azure Active Directory-todennusta tuetaan myös PowerShell-komentoa.


## <a name="1-create-and-populate-an-azure-ad"></a>1. luominen ja täytä Azure AD

Luo Azure Active Directory ja Lisää käyttäjät ja ryhmät. Azure Active Directory voi olla hallitun alustavaksi toimialueeksi Azure AD-toimialue. Azure Active Directory voi olla myös paikallisen Active Directory ‑toimialueen palveluihin, joka on liitetty Azure Active Directory-hakemistosta.

Lisätietoja [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory/active-directory-aadconnect.md), [Lisää oman toimialuenimen, Azure AD](../active-directory/active-directory-add-domain.md), [tukee nyt Microsoft Azure Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [hallinta Azure AD-kansio](https://msdn.microsoft.com/library/azure/hh967611.aspx)ja [hallinta Windows PowerShellin Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. Varmista versio 12 on SQL-tietokantaan

Uusimman SQL-tietokannan V12 tuetaan Azure Active Directory-todennusta. SQL-tietokannan Vipuventtiili V12 ja katso, onko kyseessä käytettävissä alueellasi lisätietoja [uusimman SQL-tietokannan päivitys V12 uudet ominaisuudet](sql-database-v12-whats-new.md). Tämä vaihe ei ole tarpeen Azure SQL-tietovarasto, koska SQL-tietovarasto on käytettävissä Vipuventtiili V12 vain.

Jos sinulla on aiemmin luotuun tietokantaan, varmista, että sitä isännöidään-SQL-tietokannan Vipuventtiili V12 yhdistämällä tietokantaan (esimerkiksi käyttäen SQL Server Management Studiossa) ja `SELECT @@VERSION;`. Odotettu tulos-SQL-tietokannan Vipuventtiili V12 tietokantaan on vähintään **Microsoft SQL Azure (RTM) - 12.0**. Jos tietokantasi isännöidään ei ole SQL-tietokannan Vipuventtiili V12, katso lisätietoja artikkelista [suunnitteleminen ja Valmistautuminen päivittämään SQL-tietokannan Vipuventtiili V12](sql-database-v12-plan-prepare-upgrade.md), ja siirry Azure perinteinen portaalin tietokannan siirtäminen SQL-tietokannan Vipuventtiili V12.

Vaihtoehtoisesti voit luoda uuden tietokannan SQL-tietokannan Vipuventtiili V12 [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md)lueteltuja vaiheita noudattamalla. **Vihje**: Lue seuraavaksi, ennen kuin valitset tilauksen uuden tietokannan.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. Valinnainen: Liitä tai muuta, joka on liitetty tilauksen Azure active directory

Tietokannan liittäminen organisaation Azure AD-kansio, varmista hakemiston luotettujen directory Azure tilaukseen, jossa tietokanta. Lisätietoja on artikkelissa [miten Azure tilaukset liittyvät Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Seuraavat tiedot:** Jokaisen Azure tilauksella on luottamussuhde Azure AD-esiintymää. Tämä tarkoittaa, että se voi luottaa kansion tarkistamiseen käyttäjien ja palvelujen laitteet. Useita tilauksia luottaa samaan kansioon, mutta tilauksen luottaa vain yhden kansion. Näet, mitkä directory luottaa tilauksen osoitteessa [https://manage.windowsazure.com/](https://manage.windowsazure.com/) **asetukset** -välilehdessä. Tämä luottamussuhde, joka tilauksen on hakemiston poikkeaa suhteen, josta tilaus on kaikki muut Azure (esimerkiksi sivustot ja tietokannat), resurssit, jotka muistuttavat lapsen resurssien tilauksen kanssa. Jos tilaus vanhenee, kyseiset tilaukseen liittyvää resurssien käytön myös pysähtyy. Mutta hakemisto pysyy Azure ja voit yhdistää toiseen tilaukseen kyseiseen kansioon ja jatkaa directory käyttäjien. Lisätietoja resursseja on artikkelissa [tietoja resurssin Azure-tietokannassa](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Ohjeiden on vaiheittaiset ohjeet liittyvät hakemiston tietyn tilauksen muuttamisesta.

1. Muodosta [Azure perinteinen Portal](https://manage.windowsazure.com/) käyttämällä Azure tilauksen järjestelmänvalvoja.
2. Valitse vasemman-palkin **asetukset**.
3. Tilausten näy asetukset-näytössä. Jos haluamasi tilaus ei näy, valitsemalla sen yläreunassa **tilaukset** , **SUODATTIMEN HAKEMISTO** -luetteloruudusta ja valitse kansio, joka sisältää tilauksistasi ja valitse sitten **Käytä**.

    ![Valitse tilaus][4]
4. Valitse **asetukset** -sarakkeessa tilauksen ja valitse sitten **Muokkaa DIRECTORY** sivun alareunassa.

    ![AD-asetukset-portaalissa][5]
5. **Muokkaa DIRECTORY** -ruutuun Valitse Azure Active Directory, joka on liitetty SQL Server- tai SQL-tietovarasto ja valitse sitten Seuraava nuolta.

    ![Muokkaa hakemisto-valinta][6]
6. Varmista, että **VAHVISTA** directory määritys-valintaikkunassa "**poistetaan kaikki apuyhteyshenkilöiden.**"

    ![kansion Muokkaa Vahvista][7]
7. Valitse valintaruutu lataa portaalin.

> [AZURE.NOTE] Kun muutat hakemiston apuyhteyshenkilöiden, Azure AD-käyttäjät ja ryhmät- ja hakemisto palautettua resurssin käyttäjien poistetaan ja heillä on käyttöoikeus tähän tilaukseen tai sen resursseja ei enää. Vain-service-järjestelmänvalvojana voit määrittää access ansaitun perusteella uusi kansio. Tämä muutos saattaa kestää merkittäviin aikamäärä leviäminen kaikkiin resursseihin. Hakemiston, vaihtaminen muuttuu Azure AD-järjestelmänvalvojan SQL-tietokantaan ja SQL-tietovarasto myös ja estää tietokannan käytön aiemmin luoduille Azure AD-käyttäjille. Azure AD-järjestelmänvalvoja on oltava Palauta (kuten jäljempänä) ja uusi Azure AD-käyttäjiä on luotava.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. Azure SQL Server Azure AD-järjestelmänvalvojan luominen

Kunkin Azure SQL Server (joka isännöi SQL-tietokanta tai SQL-tietovarasto) käynnistyy yhden palvelimen järjestelmänvalvojan tilillä, joka on koko Azure SQL-palvelimen järjestelmänvalvoja. Toinen SQL Server-järjestelmänvalvojan on luotava, joka on Azure AD-tili. Tämä lyhennys luodaan perusmuodon tietokannan sisältämät tietokannan käyttäjäksi. Järjestelmänvalvoja palvelimen järjestelmänvalvojatilejä on jokaisen käyttäjän tietokannan **db_owner** -roolin jäsen ja kirjoita kunkin käyttäjän tietokannassa **dbo** -käyttäjänä. Saat lisätietoja palvelimen Järjestelmänvalvojatileille [tietokantojen hallinta ja kirjautumiset Azure SQL-tietokantaan](sql-database-manage-logins.md) ja **Kirjautumiset ja käyttäjien** osan [Azure SQL tietokanta suojauksen ohjeita ja rajoituksia](sql-database-security-guidelines.md).

Käytettäessä Azure Active Directory toistot Geo Azure Active Directory-järjestelmänvalvojan on määritettävä ensisijainen ja toissijainen palvelimiin. Jos palvelin ei ole Azure Active Directory-järjestelmänvalvoja, valitse Azure Active Directory-kirjautumiset ja käyttäjät saavat "ei voi muodostaa yhteyttä" palvelinvirhe.

> [AZURE.NOTE] Käyttäjät, jotka perustuvat ei Azure AD-tilin (kuten järjestelmänvalvoja Azure SQL Server-tili), Azure AD-pohjaisen käyttäjille, ei voi luoda, koska niitä ei ole oikeutta Vahvista ehdotettu tietokannan käyttäjät, joilla on Azure AD.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Valmistele Azure-SQL Server Azure Active Directory-järjestelmänvalvojan mukaan Azure-portaalissa

1. Valitse yhteys avattava luettelo mahdollista aktiivinen kansioiden [Azure portal](https://portal.azure.com/)-oikeassa yläkulmassa. Valitse oikea Active Directory Azure AD-oletukseksi. Tässä vaiheessa linkit tilauksen liitoksen Active Directory varmistetaan, että molemmille käytetään saman tilauksen Azure SQL Server Azure AD- ja SQL Server. (Azure SQL Server voi olla isännöidä, Azure SQL-tietokanta tai SQL Azure-tietovarasto.)

    ![Valitse ad][8]
2. Vasen ilmoituspalkissa Valitse **SQL-palvelimet**, valitse **SQL server**-ja valitse sitten **SQL Server** -sivu yläosassa **asetukset**.

    ![AD-asetukset][9]
3. Valitse **asetukset** -sivu ** Active Directory-järjestelmänvalvoja.
4. **Active Directory-järjestelmänvalvoja** , sivu- **Active Directory-järjestelmänvalvoja**ja valitse sitten yläreunassa, **Määritä järjestelmänvalvoja**.
5. Valitse **Lisää järjestelmänvalvoja** -sivu, Etsi käyttäjä-käyttäjän tai ryhmän järjestelmänvalvojan ja valitse sitten **Valitse**. (Active Directory-järjestelmänvalvoja-sivu näyttää kaikki jäsenet ja ryhmiin Active Directoryn. Käyttäjien tai ryhmien, joka on himmennetty, ei voi valita, koska ne eivät ole tuettuja Azure AD-järjestelmänvalvojille. (Katso **Azure AD-ominaisuudet ja rajoitukset** yllä tuetut järjestelmänvalvojien luettelosta.) Roolipohjainen käyttöoikeuksien valvonta (RBAC) koskee vain portaalin ja on välitetty ei SQL Server.
6. Yläreunassa olevalla **Active Directory-järjestelmänvalvoja** -sivu valitsemalla **Tallenna**.
    ![Valitse järjestelmänvalvoja][10]

    Muuttamisessa järjestelmänvalvoja voi kestää useita minuutteja. Valitse uusi järjestelmänvalvoja näkyy **Active Directory-järjestelmänvalvoja** -ruudussa.

> [AZURE.NOTE] Kun määrität Azure AD-järjestelmänvalvoja, uusi järjestelmänvalvojan nimi (käyttäjä tai ryhmä) jo voi virtual perusmuodon tietokannassa SQL Server-todennus-käyttäjänä. Jos sellainen Azure AD-järjestelmänvalvojan asennus epäonnistuu, peruutetaan sen luomisen ja osoittaa, että tällaisten järjestelmänvalvoja (nimi) jo olemassa. Esimerkiksi SQL Server todennus käyttäjä ei ole Azure AD, minkä tahansa työmäärään Azure AD-todennusta palvelimeen epäonnistuu.

Jos haluat myöhemmin poistaa järjestelmänvalvoja yläreunaan **Active Directory-järjestelmänvalvoja** -sivu, valitse **Poista järjestelmänvalvoja**ja valitse sitten **Tallenna**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Valmistele Azure SQL Server Azure AD-järjestelmänvalvojan PowerShell-toiminnolla

Jos haluat suorittaa PowerShell-cmdlet-komentoja on asennettu ja käytössä PowerShellin Azure. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

Valmistelu Azure AD-järjestelmänvalvoja, suorita seuraavat PowerShellin Azure-komennot:

- Lisää AzureRmAccount
- Valitse AzureRmSubscription


Cmdlet-komentojen avulla valmistelu ja hallita Azure AD-järjestelmänvalvoja:

| Cmdlet-nimi                                       | Kuvaus                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Määritä AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Azure Active Directory-järjestelmänvalvoja valmistelee Azure SQL Serverin tai SQL Azure-tietovarasto. (On oltava nykyisen tilauksesi.) |
| [Poista AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Azure Active Directory-järjestelmänvalvoja poistaa Azure SQL Serverin tai SQL Azure-tietovarasto. |
| [Hae AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Palauttaa Azure Active Directory-järjestelmänvalvojan määritetyt Azure SQL Server- tai SQL Azure-tietovarasto tietoja. |

Katso lisätietoja kullekin komentoja, esimerkiksi komento PowerShell-ohjeita avulla ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Seuraavien komentosarjan säännösten Azure AD-järjestelmänvalvojien ryhmään nimeltä **DBA_Group** (objektitunnus `40b79501-b343-44ed-9ce7-da4c8cc7353f`), **demo_server** resurssiryhmä-palvelimen nimi **ryhmän 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

**Näyttönimi** syöteparametria hyväksyy Azure AD-näyttönimen tai täydellinen käyttäjätunnus. Esimerkiksi ``DisplayName="John Smith"`` ja ``DisplayName="johns@contoso.com"``. Azure AD-ryhmien Azure AD tuetaan näyttönimi.

> [AZURE.NOTE] PowerShellin Azure-komento ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` estä valmistelu Azure AD-järjestelmänvalvojien ei tueta käyttäjille. Ei tueta käyttäjä voi olla valmistelun yhteydessä, mutta tietokantaan ei voi muodostaa yhteyttä. (Katso **Azure AD-ominaisuudet ja rajoitukset** yllä tuetut järjestelmänvalvojien luettelosta.)

Seuraavassa esimerkissä on valinnainen **objektitunnus**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Azure AD- **objektitunnus** tarvitaan, kun **Näyttönimi** eivät ole yksilöllisiä. **Objektitunnus** ja **Näyttönimi** -arvojen noutamiseen perinteinen portaalin Azure Active Directory-osaan ja käyttäjän tai ryhmän ominaisuudet.

Seuraava esimerkki palauttaa nykyisen tietoja Azure SQL Server Azure AD-järjestelmänvalvoja:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Seuraavassa esimerkissä poistaa Azure AD-järjestelmänvalvoja:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Voit myös valmistella Azure Active Directory-järjestelmänvalvoja, REST API avulla. Lisätietoja on artikkelissa [Service Management REST API-viittaus ja Azure SQL-tietokantoja toimintoja Azure SQL-tietokantoja toiminnot](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. asiakastietokoneiden määrittäminen

Valitse kaikki asiakaskoneiden, josta sovellus tai muodostaa yhteyden Azure SQL-tietokanta tai Azure SQL-tietovarasto Azure AD-käyttäjätietojen käyttämisestä Asenna seuraavat ohjelmistot:

- .NET framework 4.6 tai uudempi kuin [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory käyttöoikeuksien kirjaston SQL Server (**ADALSQL. DLL**) on käytettävissä useita kieliä (x86 ja amd64) download Centeristä [Microsoft Active Directory käyttöoikeuksien kirjaston Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742)-palvelussa.

### <a name="tools"></a>Työkalut

- .NET Framework 4.6 vaatimus asennettaessa [SQL Server 2016 Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) tai [SQL Server Data Tools for Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) täyttää.
- **ADALSQL SSMS asennuksia x86 versio. DLL**.
- SSDT asentaa **ADALSQL amd64-version. DLL**.
- - [Visual Studio Lataa](https://www.visualstudio.com/downloads/download-visual-studio-vs) uusin Visual Studio .NET Framework 4.6 vaatimuksia, mutta eivät asennu **ADALSQL tarvittavat amd64-version. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. sisältämät tietokannan käyttäjien luominen Azure AD-jäsenyyksiä yhdistää tietokannan

### <a name="about-contained-database-users"></a>Sisältämät tietokannan käyttäjistä

Azure Active Directory-todennus edellyttää tietokannan käyttäjiä luodaan sisältämät tietokannan käyttäjiksi. Azure AD-jäsenyyden perustuvan sisältämät tietokannan käyttäjä on tietokannan käyttäjä, joka ei ole kirjautumisen perusmuodon tietokannan ja joka yhdistää jäsenyyden Azure AD-kansiossa, joka on liitetty tietokanta. Azure AD-tunnistetietojen voi olla yksittäinen käyttäjätili tai ryhmästä. Saat lisätietoja sisältämät tietokannan käyttäjiä [Sisälsi tietokannan käyttäjiä tekeminen tietokannan Your Portable](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Tietokannan käyttäjiä (lukuun ottamatta järjestelmänvalvojat) ei voi luoda portaalissa. RBAC roolit välittyvät ei SQL Server, SQL-tietokanta tai SQL-tietovarasto. Azure RBAC roolit käytetään Azure resurssien hallinta ja eivät vaikuta tietokannan käyttöoikeudet. Esimerkiksi **SQL Server osallistujan** rooli ei anna yhteyden muodostaminen SQL-tietokanta tai SQL-tietovarasto. Käyttöoikeudet on myönnettävä suoraan Transact-SQL-lauseita käyttäen tietokannassa.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Yhdistäminen käyttäjän tietokannasta tai varastoon SQL Server Management Studiossa tai SQL Server Data Tools

Vahvista Azure AD-järjestelmänvalvoja on määrittänyt oikein, Azure AD-järjestelmänvalvojatilin avulla **pää** -tietokantayhteyden muodostamisessa.
Valmistelu Azure AD-pohjaisen sisälsi tietokannan käyttäjä (muut kuin palvelimen järjestelmänvalvoja, joka omistaa tietokannan), muodosta yhteys tietokantaan, jolla on pääsy tietokantaan Azure AD-käyttäjätietojen kanssa.

> [AZURE.IMPORTANT] Azure Active Directory-todentamisen tuki on käytettävissä [SQL Server 2016 Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx) ja Visual Studio 2015 [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) . SSMS elokuussa 2016-versio sisältää myös Active Directory yleinen todennusta, joka avulla järjestelmänvalvojat voivat edellyttävät Monimenetelmäisen todentamisen avulla, puhelu, tekstiviesti-PIN-tunnus tai mobiilisovelluksessa ilmoituksen älykorttien tuki.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Muodostaa yhteyden Active Directory-todennusta

Käytä tätä tapaa, jos olet kirjautunut Windows Azure Active Directory-tunnistetiedot: llä liitetyssä toimialueessa.

1. Käynnistä Management Studiossa tai Datatyökalut ja valitse **Muodosta yhteys palvelimeen** (tai **Yhdistä Database Engine**)-valintaikkunassa **todennusta** -vaihtoehto **Active Directory-todennusta**. Salasanaa tarvita tai voidaan kirjoittaa, koska yhteyden esitetään aiemmin tunnistetiedot.
    ![Valitse AD todennusta][11]

2. Napsauta **asetukset** -painiketta ja kirjoita **Yhteyden ominaisuudet** -sivulla **tietokantayhteyden muodostamisessa** -ruutuun haluat muodostaa yhteyden käyttäjän tietokannan nimi.
    ![Valitse tietokannan nimi][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Muodostaa yhteyden Active Directory-salasanan tarkistus

Tätä menetelmää kannattaa käyttää silloin, kun yhteyden käyttämällä Azure AD Azure AD pääasiallista nimellä hallittu toimialueen. Sitä voi käyttää myös ilman mahdollisuutta toimialueen, esimerkiksi silloin, kun Etätyö liitetyt tilin.

Käytä tätä tapaa, jos olet kirjautunut Windowsiin tunnistetiedoilla toimialueelta, joka on ole äänipuheluita Azure tai käyttämällä Azure AD Azure AD-todennusta alkuperäisen tai asiakkaan toimialueen perusteella.

1. Käynnistä Management Studiossa tai Datatyökalut ja valitse **Muodosta yhteys palvelimeen** (tai **Yhdistä Database Engine**)-valintaikkunassa **todennusta** -vaihtoehto **Active Directory salasanan todennusta**.
2. Kirjoita **käyttäjänimi** -ruutuun käyttäjänimi Azure Active Directory-muodossa **username@domain.com**. Tämä on oltava Azure Active Directory-tilin tai tilin toimialueelta järjestäjäorganisaatiota Azure Active Directory-hakemistosta.
3. Kirjoita **salasana** -ruutuun käyttäjän salasanan Azure Active Directory-tilin tai liitetty toimialue.
    ![Valitse AD salasana-todennus][12]

4. Napsauta **asetukset** -painiketta ja kirjoita **Yhteyden ominaisuudet** -sivulla **tietokantayhteyden muodostamisessa** -ruutuun haluat muodostaa yhteyden käyttäjän tietokannan nimi. (Katso kuvaa edelliseen vaihtoehtoon.)


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Luoda sisälsi Azure AD-tietokannan käyttäjä käyttäjätietokantaan

Luo Azure AD-pohjaisen sisälsi tietokannan käyttäjä (muut kuin palvelimen järjestelmänvalvoja, joka omistaa tietokannan), Yhdistä tietokantaan Azure AD-jäsenyyden käyttäjänä, jolla on vähintään **muuttaa minkä tahansa** käyttöoikeudet. Käytä seuraavaa Transact-SQL-syntaksia:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* voi olla pääasiallista käyttäjänimi ja Azure AD-käyttäjä tai näyttönimen muuttaminen Azure AD-ryhmä.

**Esimerkkejä:** Sisältämät tietokannan luomiseen käyttäjä, joka edustaa Azure AD liitetty tai hallittu toimialuekäyttäjä:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Luo edustava Azure AD tai liitetty toimialueryhmän sisältämät tietokannan käyttäjä, Ilmoita käyttöoikeusryhmän näyttönimi:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Luo sovellus, joka käyttää Azure AD-tunnuksen edustava sisältämät tietokannan käyttäjä:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Saat lisätietoja sisältämät tietokannan käyttäjien perusteella Azure Active Directory-käyttäjätietojen luominen [Luo käyttäjä (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Azure AD-todennus kuka tahansa käyttäjä Azure SQL Server Azure Active Directory-järjestelmänvalvojan poistaminen estää muodostamasta yhteyttä palvelimeen. Tarvittaessa käyttökelvoton Azure AD-käyttäjät voi vetää ja pudottaa manuaalisesti SQL-tietokannan valvoja.

Kun luot tietokannan käyttäjä, kyseisen käyttäjän vastaanottaa **Yhdistä** -käyttöoikeudet ja muodostaa yhteyden tietokantaan **julkisen** roolin jäsen. Alun perin käyttäjän käytettävissä vain käyttöoikeudet ovat **julkisia** roolin käyttöoikeudet tai minkä tahansa Windows-ryhmiin, jotka he ovat jäsen käyttöoikeudet. Kun valmistelu Azure AD-pohjaisen sisälsi tietokannan käyttäjä, voit myöntää käyttäjä lisää käyttöoikeuksia samalla tavalla kuin voit myöntää minkä tahansa käyttäjän. Yleensä käyttöoikeuksia tietokannan rooleja ja käyttäjien lisääminen roolit. Lisätietoja on artikkelissa [Tietokannan ohjelma käyttöoikeuksia perusteet](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Saat lisätietoja erityisen SQL-tietokanta-roolien [tietokantojen hallinta ja kirjautumiset Azure SQL-tietokantaan](sql-database-manage-logins.md).
Liitetyssä toimialueessa käyttäjä, jolla on tuotu Hallitse toimialuetta, on käytettävä hallittu toimialueen tunnistetiedot.

> [AZURE.NOTE] Azure AD-käyttäjiä on merkitty tietokannan metatiedoissa sisältötyyppi E (EXTERNAL_USER) ja ryhmien sisältötyyppi X (EXTERNAL_GROUPS). Lisätietoja on artikkelissa [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7. yhteyden Azure AD-käyttäjätietojen avulla

Azure Active Directory-todennus tukee tietokanta käyttämällä Azure AD-käyttäjätietojen muodostaa seuraavista tavoista:

- Windows-todennusta käyttäminen
- Lainan Azure AD-nimen ja salasanan avulla
- Sovelluksen suojaustunnuksen todennusta käyttämällä

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Yhteyden muodostaminen todennusta (Windows)

Integroitu Windows-todennus käyttämään toimialueen Active Directory on liitetty Azure Active Directory-hakemistosta. Asiakassovellus (tai palvelun) tietokantayhteyden muodostamisessa on oltava käynnissä käyttäjän toimialueen tunnistetiedot kohdassa toimialueen liittyneet koneeseen.

Muodosta yhteys tietokanta käyttämällä todennusta ja Azure AD-jäsenyyden tietokannan yhteysmerkkijonon todennus-avainsana on määritettävä Active Directoryn integroitu. Seuraavassa C# koodin esimerkissä käytetään ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Huomautus yhteyden merkkijono avainsanan ``Integrated Security=True`` muodostamisesta Azure SQL-tietokantaan ei tueta.
Huomaa, että ODBC-yhteyttä muodostettaessa tarvitset välilyöntejä ja todennuksen "ActiveDirectoryIntegrated".

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Yhdistämällä pääasiallista Azure AD-nimi ja salasana
Muodosta yhteys tietokanta käyttämällä todennusta ja Azure AD-jäsenyyden todennus-avainsana on määritettävä Active Directory-salasana. Yhteysmerkkijonon on oltava käyttäjän tunnus/UID ja salasanan/PWD avainsanoja ja arvot. Seuraavassa C# koodin esimerkissä käytetään ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Lisätietoja Azure AD-todennusmenetelmien käyttämällä käytettävissä esittely MALLIKOODEJA [Azure AD-todennuksen GitHub esittely](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 yhdistämällä Azure AD-tunnus
Tämä todennusmenetelmä avulla keskimmäisen palveluiden yhdistäminen Azure SQL-tietokanta tai SQL Azure-tietovarasto hankkimalla tunnusta from Azure Active Directory (AAD). Ottaa käyttöön myös varmenteen kehittyneitä skenaarioita. Sinun on suoritettava neljästä vaiheesta Azure AD-tunnuksen todennus käyttämään:

1. Rekisteröi sovelluksen Azure Active Directory-hakemistosta ja hankkia Asiakastunnus-koodisi. 
2. Luo sovelluksen edustava tietokannan käyttäjä. (Valmis edellä vaiheessa 6).
3. Luo varmenteen asiakkaan tietokone toimii sovelluksen.
4. Lisää varmenteen avaimen sovelluksen.

Esimerkki yhteysmerkkijonon:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Lisätietoja on [SQL Server](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)-suojauksen blogimerkinnässä.

### <a name="connecting-with-sqlcmd"></a>Yhdistämällä sqlcmd  
Seuraavista väittämistä muodostaa yhteyden sqlcmd, 13.1 versio, joka ei ole saatavilla [Download Centeristä](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Katso myös

[Tietokantojen ja Azure SQL-tietokanta-kirjautumiset hallinta](sql-database-manage-logins.md)

[Sisältämät tietokannan käyttäjiä](https://msdn.microsoft.com/library/ff929071.aspx)

[Luo käyttäjä (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

