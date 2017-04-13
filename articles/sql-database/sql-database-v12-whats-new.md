<properties
    pageTitle="Mitä uusia ominaisuuksia SQL-tietokannan Vipuventtiili V12 | Microsoft Azure"
    description="Tässä artikkelissa kuvataan, miksi järjestelmien, jotka käyttävät Azure SQL-tietokanta pilveen hyötyä päivittämällä versioon Vipuventtiili V12 nyt."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>SQL-tietokannan Vipuventtiili V12 uudet ominaisuudet


Tässä ohjeaiheessa kerrotaan monia etuja, jossa uusi Vipuventtiili V12 versio Azure SQL-tietokanta on version V11 päälle.


Microsoft jatkaa toimintojen lisääminen Vipuventtiili V12. Jotta on suositeltavaa Siirry Azure varten Palvelupäivityksistä Microsoftin WWW- ja sen suodattimien avulla:


- Suodatettu [SQL-tietokanta-palvelun](https://azure.microsoft.com/updates/?service=sql-database).
- Suodatettuna SQL-tietokannan ominaisuuksien yleiseen käyttöön [(GA) ilmoitukset](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) .


Milloin on kuvattu uusimpia tietoja resurssien rajoitukset SQL-tietokantaan:<br/>[Azure SQL-tietokannan resurssien rajoitukset](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Parannettu yhteensopivuuden SQL Serverin kanssa


SQL-tietokannan Vipuventtiili V12 avaimen tavoitteen oli parantaakseen Microsoft SQL Server 2014 yhteensopivuus ja säilyttää yhteensopivuus, kun uusia versioita SQL Server on julkaistu. Vipuventtiili V12 kertyy ohjeikkunan muiden alueiden välistä eroa ohjelmoitavuus tärkeitä alueella SQL Serverin kanssa. Esimerkki:

- [Sisäinen JSON-tuki](https://msdn.microsoft.com/library/dn921897.aspx)

- [Ikkunan Funktiot](http://msdn.microsoft.com/library/ms189798.aspx), jossa [PÄÄLLE](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-indeksit](http://msdn.microsoft.com/library/bb934097.aspx) ja [Valikoiva XML-indeksit](http://msdn.microsoft.com/library/jj670104.aspx)

- [Muutosten jäljityksen](http://msdn.microsoft.com/library/bb933875.aspx)

- [VALITSE... TUOMINEN](http://msdn.microsoft.com/library/ms188029.aspx)

- [Teksti-haku](http://msdn.microsoft.com/library/ms142571.aspx)

- [Muuta TIETOKANNAN SUODATETUT kokoonpano (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Katso [tähän](sql-database-transact-sql-information.md) pieni joukko toimintoja ei vielä tueta SQL-tietokantaan.


### <a name="compatibility-level-130"></a>Yhteensopivuuden tason 130


> [AZURE.IMPORTANT] **Kesäkuussa**2016 alkaen *juuri* luotujen tietokantojen Azure SQL-tietokannan Vipuventtiili V12 on niiden yhteensopivuus-tason aloittamalla 130, joka vastaa Microsoft SQL Server 2016 GA.
> 
> Voit käyttää `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` halutessasi.
> 
> Tietokantoja, jotka on luotu ennen kesäkuussa 2016 ei ole niiden yhteensopivuuden tason muuttaa muutoksen oletus. Eikä muuttaa päivittämällä se V11 Vipuventtiili V12 tietokannan taso.



Katso, miten voit verrata välisen Edellinen yhteensopivuuden taso ja uusimmat tärkeimmät kyselyjen selostus:

- [Parantaa kyselyn suorituskykyä yhteensopivuuden viereiset 130 Azure SQL-tietokantaan](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Lisää premium suorituskyvyn uusi suorituskyvyn tasot


Vipuventtiili V12 on lisätty kohdistettu Premium suorituskyvyn tasojen 25 % ilman lisäkustannuksia tietokannan tapahtuman yksiköt (DTUs). Vieläkin suorituskyvyn voitot onnistuu uusia ominaisuuksia,:


- Ladatun [columnstore indeksit](http://msdn.microsoft.com/library/gg492153.aspx)tuki.
- [Taulukon jakaminen riveittäin](http://msdn.microsoft.com/library/ms187802.aspx) [Katkaise](http://msdn.microsoft.com/library/ms177570.aspx)taulukkoon liittyvät parannukset kanssa.
- Dynaaminen hallinta käytettävyyttä näkee [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) avulla seurata ja suorituskyvyn parantamisesta.


### <a name="reliable-performance"></a>Luotettavan suorituskyky


Jos asiakasohjelman muodostaa yhteyden SQL-tietokannan Vipuventtiili V12 samalla, kun asiakkaan suoritetaan Azure virtuaalikoneen (AM), sinun on avattava AM seuraavia portti-alueita:

- 11000 11999
- 14000 14999


Napsauta [tätä](sql-database-develop-direct-route-ports-adonet-v12.md) tietoa SQL-tietokannan Vipuventtiili V12 portit. Portit tarvitaan suorituskykyparannukset-SQL-tietokannan Vipuventtiili V12 mukaan.


## <a name="better-support-for-cloud-saas-vendors"></a>Parannettu tuki cloud SaaS toimittajat


Vain Vipuventtiili V12, valitse on julkaissut uuden normaalin suorituskyvyn tason S3 ja [joustavasti tietokannan jakavat](sql-database-elastic-pool.md)julkisen esikatselu. Joustavasti tietokannan jakavat on ratkaista suunniteltu cloud SaaS toimittajia.  Joustavasti tietokannan jakavat avulla voit:


- Voit jakaa DTUs tietokantojen ja vähennä tietokantojen paljon kustannuksia kesken.
- Suorita [joustavasti tietokannan työt](sql-database-elastic-jobs-overview.md) tasolla tietokantojen hallinta.


## <a name="security-enhancements"></a>Suojauksen parannukset


Suojaus on ensisijainen huolenaiheita, kuka tahansa, joka suoritetaan käyttävät yritykset pilveen. Uusimmat tietoturvaominaisuudet julkaistu Vipuventtiili V12 ovat:


- [Rivin käyttäjätason suojauksen](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dynamic Data naamioiminen](sql-database-dynamic-data-masking-get-started.md)
- [Sisältämät tietokannat](http://msdn.microsoft.com/library/ff929188.aspx)
- [Sovelluksen roolit](http://msdn.microsoft.com/library/ms190998.aspx) hallitut ja myönnä, estä, KUMOTA
- [Läpinäkyvä tiedon salaus](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Yhteyden muodostaminen SQL-tietokantaan Azure Active Directory käyttöoikeuksien avulla](sql-database-aad-authentication.md)
 - SQL-tietokanta tukee nyt Azure Active Directory käyttöoikeuksien puitteissa yhteyden muodostaminen SQL-tietokantaan avulla käyttäjätietojen Azure Active Directory (Azure AD). Azure Active Directory-todennuksen avulla voit hallita keskitetysti käyttäjätietojen tietokanta käyttäjien ja muiden Microsoftin palvelujen yhdessä keskitetyssä paikassa.
- [Salattu aina](https://msdn.microsoft.com/library/mt163865.aspx) (ennakkoversio) mahdollistaa salauksen läpinäkyvä sovelluksia ja sallii asiakkaiden salata asiakassovelluksissa sisällä arkaluontoisia tietoja ilman salausavaimet jakamisen SQL-tietokantaan.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Liiketoiminnan jatkuvuus kasvaa, kun palautus tarvita


Vipuventtiili V12 on parannettu palautus pisteen tavoitteet (RPOs) ja arvioitu palautus aina (ERTs):


| Liiketoiminnan jatkuvuus-ominaisuus | Aiemman version tiedostomuodossa | VIPUVENTTIILI V12 |
| :-- | :-- | :-- |
| GEO palauttaminen | • RPO < 24 tuntia.<br/>• NNA < 12 tuntia. | • RPO < 1 tunti.<br/>• NNA < 12 tuntia. |
| Aktiivinen Geo-replikointi | • RPO < 5 minuuttia.<br/>• NNA < 1 tunti. | • RPO < 5 sekuntia.<br/>• NNA < 30 sekunnin välein. |


Lisätietoja on [SQL-tietokantaan Liiketoiminnan jatkuvuus](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Lisää syitä nyt päivittäminen


Liittyy monia hyviä syitä, miksi asiakkaat pitäisi päivittää nyt Azure SQL-tietokannan Vipuventtiili V12 V11:


- SQL-tietokannan Vipuventtiili V12 on paljon toimintojen lisäksi V11 ominaisuuksia.
- Olemme jatkaa ja lisätä uusia ominaisuuksia Vipuventtiili V12, mutta V11 lisätään uusia ominaisuuksia ei.
- Useimmat uusia ominaisuuksia julkaistaan SQL-tietokannan Vipuventtiili V12, ennen kuin ne julkaistaan Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Käytät Vipuventtiili V12 jo?


Tarkista, ovatko tietokantaan tai aiemmassa versiossa SQL-tietokanta-palvelu käytössä looginen palvelimeen yhden helposti on seuraavasti:


1. Siirry [Azure Portal](https://portal.azure.com/).
2. Valitse **Selaa**.
3. Valitse **SQL-palvelimiin**.
4. Tarinan ilmoittaa, ettei palvelimen tai tietokannan vieressä olevaa kuvaketta:
 - ![Kuvakkeen Vipuventtiili v12 palvelinta](./media/sql-database-v12-whats-new/v12_icon.png) **Vipuventtiili V12 looginen server**
 - ![Aiemman version palvelimen kuvake](./media/sql-database-v12-whats-new/earlier_icon.png) **aiemman version looginen server**


Toinen tapa varmistaa, että versio kannattaa suorittaa `SELECT @@version;` lauseen tietokannassa ja tarkastella kaltaisilta tulokset:


- **12**.0.2000.10 &nbsp; *(versio Vipuventtiili V12)*
- **11**.0.9228.18 &nbsp; *(V11 versio)*


Vain Vipuventtiili V12 looginen server pystyy ylläpitämään Vipuventtiili V12 tietokannan. Ja Vipuventtiili V12 palvelimeen isännöidä vain Vipuventtiili V12 tietokantoja.


Jos käytössäsi ei ole vielä Vipuventtiili V12 käyttöön, voit päivittää looginen palvelimellesi noudattamalla seuraavia ohjeita [päivittää SQL tietokanta Vipuventtiili V12 paikassa](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Yleinen käytettävyys-alueet


- 31 heinäkuussa 2015 mukaan kaikkien alueiden oli muunnettu, yleinen käytettävyys (GA).
- Vipuventtiili V12 julkaistiin joulukuussa 2014, mutta vain esikatselu tilan osoitteessa.

[Täydentäviä Microsoft Azure esikatselut käyttöehdot](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
