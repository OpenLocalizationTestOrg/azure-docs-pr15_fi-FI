<properties
    pageTitle="Optimoida SQL arviointi-ratkaisuun Log Analytics-ympäristösi | Microsoft Azure"
    description="SQL-arviointi-ratkaisun avulla voit arvioida riski ja palvelin-ympäristöissä kunto säännöllisin väliajoin."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimoi ympäristösi Log Analytics arviointi SQL-ratkaisuun


SQL-arviointi-ratkaisun avulla voit arvioida riski ja palvelin-ympäristöissä kunto säännöllisin väliajoin. Tämän artikkelin avulla voit asentaa ratkaisun niin, että voit tehdä korjaavat toimenpiteet mahdollisia ongelmia.

Tämä ratkaisu on prioriteettiluetteloon suosituksia tietyn käyttöön server-infrastruktuuria. Suosituksia luokitellaan yli kuusi kohdistus alueet, joiden avulla voit nopeasti ymmärretty, ja ota korjaavat toimenpiteet.

Suositukset perustuvat tietämyksen ja kokemus Microsoft insinöörien asiakkaan vierailut tuhansia kohteesta. Kunkin suositus ohjeita siitä, miksi ongelma saattaa merkitystä sinulle ja ottamisesta käyttöön ehdotetut muutokset.

Voit valita kohdistus alueet, jotka ovat tärkeimpiä organisaation ja kohti käytössä riskin maksuttomia ja kunnossa ympäristön edistymisen seuraaminen.

Kun olet lisännyt ratkaisun ja arvio on valmis, yhteenveto kohdistus aihealueiden näkyvät ympäristössäsi infrastruktuurin **SQL arviointi** Raporttinäkymät-ikkunan. Seuraavissa kohdissa kuvataan tiedot käyttämisestä **SQL-arviointi** koontinäyttö, jossa voit tarkastella ja sitten toimintoja suositeltu, SQL server-infrastruktuuria.

![Kuva arviointi SQL-ruutu](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL-arviointi Raporttinäkymät-ikkunan kuva](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
SQL-arviointi toimii kaikki tuetut SQL Server Standard-, Developer- ja Enterprise-versiot-versioiden kanssa.

Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Tekijöiden on oltava asennettuna, joissa on asennettu SQL Server-palvelimiin.
- SQL-arviointi-ratkaisun edellyttää .NET Framework 4 kussakin tietokoneessa, jossa on OMS agentti asennettuina.
- Kun käytät Operations Manager-agentti SQL Assessment, tarvitset Operations Manager Run-As-tilin käyttöä varten. Lisätietoja on kohdassa [Operations Manager Suorita nimellä tilit OMS](#operations-manager-run-as-accounts-for-oms) alapuolella.

    >[AZURE.NOTE] MMA-agentti ei tue Operations Manager Run-As tilit.

- SQL-arviointi-ratkaisun lisääminen OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet. Ei ole tarvitaan lisämäärityksiä.

>[AZURE.NOTE] Kun olet lisännyt ratkaisun, AdvisorAssessment.exe-tiedosto lisätään palvelinten agenttien vuoksi. Määritystietoja lukea ja lähettää sitten OMS palvelun pilveen käsittelyä varten. Logiikan käytetään vastaanotetut tiedot ja pilvipalvelussa tietueiden tietoja.

## <a name="sql-assessment-data-collection-details"></a>SQL-arvioinnin sivustokokoelman tietojen

SQL-arviointi kerää WMI-tietoja, rekisteritietoja, suorituskykytietoja ja SQL Server dynaaminen hallinta Näytä tulokset käyttämällä agenttien vuoksi, jotka on otettu käyttöön.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmiä agenttien vuoksi, onko Operations Manager (SCOM) ja kuinka usein kerää tietoja, agentti.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Kyllä](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Kyllä](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-sql-assessment/oms-bullet-red.png)|    ![Ei](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Kyllä](./media/log-analytics-sql-assessment/oms-bullet-green.png)|   7 päivää|

## <a name="operations-manager-run-as-accounts-for-oms"></a>Suorita nimellä Operations Manager tilien OMS varten

Kirjaudu Analytics OMS käyttää Operations Manager agent- ja ryhmän kerää ja Lähetä tiedot OMS-palvelun. OMS versiot yhteydessä management Pack-paketit työmääriä antamaan arvo-Lisää palveluita. Kunkin työmäärää edellyttää työmäärää kielikohtaiset oikeuksilla management Pack-paketit suojauksen kontekstissa, kuten toimialuetiliä. Haluat tarjota määrittämällä Operations Manager Suorita nimellä-tilin tunnistetiedot.

Seuraavat tiedot avulla voit määrittää Operations Manager Suorita nimellä-tilin SQL arviointia varten.

### <a name="set-the-run-as-account-for-sql-assessment"></a>SQL-arvioinnin Suorita nimellä-tilin määrittäminen

 Jos käytössäsi on jo SQL Server management Pack-paketti, sinun on käytettävä tiliin Suorita nimellä.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>Toiminnot-konsolissa SQL Suorita nimellä-tilin määrittäminen

>[AZURE.NOTE] Jos käytössäsi on suora OMS-agentti SCOM-agentti sijaan, management pack suoritetaan aina paikallisen järjestelmätilin käyttöympäristössä. Ohita vaiheet 1 – 5 mukaisesti ja suorita T-SQL- tai Powershell-malli, lisäämällä NT AUTHORITY\SYSTEM käyttäjänimi.

1. Operations Manager Avaa toiminnot-konsolin ja valitse sitten **hallinta**.

2. Valitse **Suorita määritys nimellä**Valitse **profiilit**ja Avaa **OMS SQL arviointi suorittaa kuin profiilin**.

3. Valitse **Suorita kuin tilit** -sivulla **Lisää**.

4. Valitse Windows Suorita nimellä-tili, joka sisältää SQL Server tarvittavat tunnistetiedot tai valitse **Uusi** , luo sellainen.
    >[AZURE.NOTE] Suorita nimellä-tilin tyyppi on oltava Windows. Suorita nimellä-tili on kuuluttava myös paikalliseen järjestelmänvalvojaryhmään Windows palvelimilta, jossa SQL Server-esiintymät.

5. Valitse **Tallenna**.

6. Muokkaaminen ja suorita seuraava T-SQL-malli voit myöntää tarvitse suorittaa tilille kuin SQL-arvioiminen käyttöoikeusvaatimukset kunkin SQL Server-esiintymän. Kuitenkin ei tarvitse tehdä tämä, jos on Suorita nimellä-tili on jo osa järjestelmänvalvojaryhmään palvelimen SQL Server-esiintymät.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Windows PowerShellin SQL Suorita nimellä-tilin määrittäminen

Avaa PowerShell-ikkuna ja suorita seuraavaa komentosarjaa, kun olet päivittänyt se omilla tiedoillasi:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Tietoja siitä, miten suositukset on ensin

Kaikki tehdyt suositus annetaan painotus-arvo, joka määrittää suositus suhteellinen tärkeys. Vain kymmenen tärkeimmät suositukset ovat näkyvissä.

### <a name="how-weights-are-calculated"></a>Miten leveydet lasketaan

Painotuksia ovat koostearvoja kolme avaimen tekijöiden perusteella:

- *Todennäköisyys* , että havaittu ongelma aiheuttaa ongelmia. Korkeampi todennäköisyys vastaa tehtävässä suositus suurempi yleinen tulos.

- *Vaikutus* organisaatiossa, jos se aiheuttaa ongelmia ongelmaa. Suurempi vaikutus vastaa tehtävässä suositus suurempi yleinen tulos.

- *Työmäärään* pakollinen suositus täytäntöön. Korkeampi työmäärään vastaa tehtävässä suositus pienempi yleinen tulos.

Kunkin toiminnon painotus ilmaistaan käytettävissä kohdistus kullekin alueelle yhteensä kirjainarvosanan prosentteina. Esimerkiksi jos suositus tietosuojaan ja vaatimustenmukaisuuteen kohdistus-alueella on tulos on 5 %, tämä suositus soveltamisesta kasvattaa oman yleinen tietosuojaan ja vaatimustenmukaisuuteen pistemäärän 5 %.

### <a name="focus-areas"></a>Kohdistus-alueet

**Tietosuojaan ja vaatimustenmukaisuuteen** - kohdistus tällä alueella näkyvät suosituksia mahdollisia tietoturvauhkia ja ilmeisesti, yrityksen käytännöt ja tekniset, lakisääteisten määräysten noudattaminen vaatimukset.

**Käytettävyys ja Liiketoiminnan jatkuvuus** - kohdistus tällä alueella näkyvät suosituksia palvelun saatavuus, vikasietoisuudelle infrastruktuuri ja business suojaus.

**Suorituskyky ja skaalattavuus** - kohdistus tällä alueella näkyvät suosituksia, joiden avulla organisaation IT-infrastruktuurin kasvaa, varmista, että IT-ympäristösi vastaa nykyisen suorituskyvyn ja ei voi vastata infrastruktuurin tarpeiden.

Suosituksia, joiden avulla voit päivittää, siirtää ja SQL Serverin käyttöönotto aiemmin infrastruktuuria näyttää **päivityksen, siirtymis- ja** - kohdistus tällä alueella.

**Toimintojen ja seuranta** - kohdistus tällä alueella näkyvät suosituksia, joiden avulla tehostaa IT-toimintoja, toteuttaa Ennaltaehkäis ja parantaa suorituskykyä.

**Muutosten ja määritystenhallintaa** - kohdistus tällä alueella näkyvät suositukset suojaa päivittäinen toimintoja, varmista, että muutokset ei vaikuta infrastruktuurin, muodostaa Muutoksenhallinnan menetelmien, heikentää avulla seurata ja valvonta järjestelmän määritykset.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Olisi tavoitteena sija 100 %: n jokaisen kohdistus-alueella?

Ei välttämättä. Suosituksia perustuvat tietämyksen ja kokemuksen mukaan Microsoft insinöörien kautta tuhansia asiakkaan vierailut kokemukset. Kaksi server ei ole infrastruktuurin ovat samat, ja suosituksia voi olla enemmän tai vähemmän tärkeimpiä. Jotkin suojausta koskevia suosituksia voi olla esimerkiksi vähemmän merkitystä, jos oman näennäiskoneiden eikä niitä julkaista Internetissä. Käytettävyys joitain suosituksia voi olla pienempi kannalta palveluja, jotka tarjoavat pienen tärkeyden luonnoslehtiössä tietojen kerääminen ja raportointi. Ongelmat, jotka ovat tärkeitä kehittynyt business voi olla pienempi tärkeää pysäyttämisestä. Voit tunnistaa kohdistus mitkä alueet olevat oman prioriteetit ja katso, kuinka oman tulosten muuttuvat ajan kuluessa.

Jokaisen suositus sisältää ohjeita siitä, miksi on tärkeää. Nämä ohjeet käytettävä arvioidaan, onko käyttöönoton suositus sopii, IT-palveluiden laatu ja organisaation tarpeisiin.

## <a name="use-assessment-focus-area-recommendations"></a>Käytä arviointi kohdistus alueen suositukset

Ennen kuin voit käyttää arviointi-ratkaisun OMS, sinulla on oltava asennettuna ratkaisu. Lisätietoja asentamisesta ratkaisuja Katso [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md). Kun se on asennettu, voit tarkastella Yhteenveto suosituksista arviointi SQL-ruudun käyttäminen OMS yleiskatsaus-sivulla.

Näytä infrastruktuuri ja sitten Poraudu kyselyjä suosituksia yhteenvedettyinä yhteensopivuus-arvioinnit.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Näytä suositukset kohdistus-alueelle ja korjausta

1. Valitse **sivulla** Napsauta **SQL arviointi** -ruutua.
2. **SQL-arviointi** sivulla Tarkista yhteenvetotiedot jossakin kohdistus alueen näiden ja valitse sitten haluat nähdä kohdistus alueen suosituksia.
3. Millä tahansa kohdistus alue-sivulla voit tarkastella Priorisoidut suositukset-ympäristössä. Valitse suositus **Vaikuttaa objektien** tietoja siitä, miksi suositus tehdään.  
    ![SQL-arviointi suosituksia kuva](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Voit ottaa ehdotetut **Ehdotetut toimenpiteet**korjaavat toimenpiteet. Kun kohde on osoitettu, myöhemmin arvioinnit tietueen, jotka suositeltavat toimenpiteet on otettu ja yhteensopivuuden Pistemäärä kasvaa. Korjatut kohteet näkyvät **Välitetty**objekteina.

## <a name="ignore-recommendations"></a>Ohita suositukset

Jos sinulla on suosituksia, jotka haluat ohittaa, voit luoda tekstitiedosto, joka OMS käytetään estää suosituksia näkymisen arvioinnin tulokset.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Tunnistaa, voit ohittaa suositukset

1.  Työtilan kirjautuminen ja avaa loki haku. Käytä ympäristösi tietokoneet luettelon suositukset, jotka eivät ole läpäisseet seuraava kysely.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Näin näyttökuva Log hakukyselyn: ![epäonnistui suositukset](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.  Valitse suosituksia, jotka haluat ohittaa. Tarvitset arvot seuraavaan vaiheeseen RecommendationId varten.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Voit luoda ja käyttää IgnoreRecommendations.txt-Tekstitiedosto

1.  Luo IgnoreRecommendations.txt-tiedosto.
2.  Liitä tai kirjoita kukin RecommendationId kunkin toiminnon, jonka haluat OMS Ohita sana omalle rivilleen ja Tallenna ja sulje tiedosto.
3.  Viemällä tiedoston seuraavaan kansioon käyttöön kussakin tietokoneessa, johon haluat OMS Ohita suositukset.
    - Tietokoneissa, joissa on (yhteydessä suoraan tai Operations Manager) Microsoft seuranta Agent - *SystemDrive*: Files\Microsoft Agent\Agent seuranta
    - Operations Manager hallinta palvelimessa - *SystemDrive*: Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Voit varmistaa, että suosituksia ohitetaan

1.  Kun seuraava ajoitettu arviointi suoritetaan oletusarvoisesti 7 päivää, määritetty suositukset merkitään ohitettu ja ei näy arviointi koontinäytössä.
2.  Voit käyttää lokiin hakukyselyt ohitetut suositukset-luettelo.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.  Jos päätät myöhemmin, että haluat nähdä ohitetut suositukset, poista IgnoreRecommendations.txt tiedostoja tai voit poistaa ne RecommendationIDs.

## <a name="sql-assessment-solution-faq"></a>SQL-arviointi-ratkaisun usein kysytyt kysymykset

*Kuinka usein arvio suorittaa?*
- Arvioinnin suorittaa 7 päivän välein.

*Onko voi määrittää arvioinnin tiheyttä?*
- Ei tällä hetkellä.

*Jos toiseen palvelimeen havaitaan, kun SQL-arviointi ratkaisu on lisätty, se arvioidaan?*
- Kyllä, kun se on arvioitu-, sitten 7 päivää havaitaan.

*Jos palvelin on poistettu käytöstä, kun se poistetaan arvioinnin?*
- Jos palvelin ei lähettää tietoja 3 viikon aikana, se poistetaan.

*Mikä on prosessin, joka tekee tietojen keräys nimen?*
- AdvisorAssessment.exe

*Kuinka kauan menee tietojen kerätään?*
- Todellisten tietojen keräys palvelimessa kestää noin 1 tunti. Saattaa kestää kauemmin palvelimissa, joissa on runsaasti SQL esiintymiä tai tietokantoihin.

*Tietojen tyypin kerätään?*
- Kerätään seuraavanlaiset tiedot:
    - WMI
    - Rekisterin
    - Suorituskyvyn laskureita
    - SQL-dynaaminen hallinta näkymien (DMV).

*Onko voi määrittää kerätä tietoja?*
- Ei tällä hetkellä.

*Miksi Suorita nimellä-tilin määrittäminen on?*
- SQL Serverin pieneen SQL-kyselyiden suoritetaan. Jotta he voivat suorittaa suorittaa kuin käyttöoikeuksin varustetun tilin VIEW SERVER STATE SQL on käytettävä.  Lisäksi kyselyn WMI, jotta paikallisen järjestelmänvalvojan tunnistetietoja ei tarvita.

*Miksi näkyviin vain top 10-suosituksia?*
- Sijaan suojausvalintaikkuna monipuolisen hankalaa luettelo tehtävistä, on suositeltavaa, että voit keskittyä osoitteiden Priorisoidut suosituksia ensin. Sen jälkeen, kun ne osoite lisäsuosituksia ole käytettävissä. Jos haluat nähdä yksityiskohtainen luettelo, voit tarkastella kaikkien suosituksia OMS log-haun avulla.

*Onko voi ohittaa suositus?*
- Katso Kyllä, [Ohita suosituksia](#ignore-recommendations) edellisen osan.



## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) Tarkastele yksityiskohtaista tietoa sisältävä SQL-arviointi ja suositukset.
