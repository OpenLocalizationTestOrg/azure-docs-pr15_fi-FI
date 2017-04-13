<properties
    pageTitle="Päivitä Management-ratkaisun OMS | Microsoft Azure"
    description="Tämä artikkeli on tarkoitettu, kuinka voit käyttää tätä ratkaisua, Windowsin ja Linux tietokoneiden päivitysten hallinta."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Päivitä Management-ratkaisun OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Päivitä Management-ratkaisun OMS

OMS Update Management-ratkaisun avulla voit hallita päivityksiä Windows- ja Linux-tietokoneiden.  Voit nopeasti tilan saatavilla olevat päivitykset agentti kaikkiin tietokoneisiin arvioi ja aloittaa asentamisessa palvelinten tarvittavat päivitykset. 

## <a name="prerequisites"></a>Edellytykset

-   Windowsin tekijöiden joko on oltava määritettynä on pääsy Microsoft Update ja Windows Server Update Services (WSUS)-palvelimen yhteyttä.  

    >[AZURE.NOTE] Windows-agentti ei voi hallita samanaikaisesti System Center määritysten hallinta.  
  
-   Linux tekijöiden on oltava access päivitys-säilöön.  Linux OMS-agentti voi ladata [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Määritys

Seuraavien toimien lisätä Update Management-ratkaisun OMS lisääminen työtilaan ja lisää Linux agenttien vuoksi.  Windowsin tekijöiden lisätään automaattisesti lisämäärityksiä.

1.  Lisää päivityksen Management-ratkaisun OMS työtilan käyttämisestä ratkaisuvalikoimasta [Lisää OMS ratkaisuja](../log-analytics/log-analytics-add-solutions.md) kuvatut toimet.  
2.  Valitse OMS-portaaliin, **asetukset** ja sitten **Yhdistetty lähteistä**.  Huomautus **Työtila-tunnus** ja joko **perusavain** tai **toissijaisen avaimen**.
3.  Seuraavien toimien Linux jokaisessa tietokoneessa.

    a.  Linux OMS-agentti uusimman version asentamisesta suorittamalla seuraavat komennot.  Korvaa <Workspace ID> työtilan tunnuksella ja <Key> joko ensisijaisen tai toissijaisen-näppäimen kanssa.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Jos haluat poistaa agentti, suorita seuraava komento.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Management Pack-paketit

Jos System Center Operations Manager hallinta-ryhmä on yhdistetty OMS työtilan, valitse seuraavat management Pack-paketit asennetaan Operations Manager lisättäessä tämä ratkaisu. Ei ole määritysten tai ylläpito näiden management Pack-pakettien pakollinen. 

-   Microsoft System Center Advisor päivityksen arviointi Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Päivitä käyttöönoton MP

Lisätietoja siitä, miten ratkaisu management Pack-paketit päivitetään on artikkelissa [Yhteyden Operations Manager Log Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Tietojen kerääminen

### <a name="supported-agents"></a>Tuetut agenttien vuoksi

Seuraavassa taulukossa on kuvattu yhdistettyjen lähteiden, jotka tukevat tätä ratkaisua.

Yhdistettyjen lähde | Tuettu | Kuvaus|
----------|----------|----------|
Windowsin agenttien vuoksi | Kyllä | Ratkaisun kerää tietoja Järjestelmäpäivitykset Windows tekijöiden ja aloittaa asennettuun tarvittavat päivitykset.|
Linux agenttien vuoksi | Kyllä | Ratkaisu kerää tietoja Järjestelmäpäivitykset Linux agenttien vuoksi.|
Operations Manager hallinta-ryhmässä | Kyllä | Ratkaisu kerää tietoja Järjestelmäpäivitykset tekijöiden yhdistetyn hallinta-ryhmässä.<br>Suora yhteyden Log Analytics Operations Manager-agentti ei ole pakollinen. Tietojen hallinta-ryhmästä välitetään OMS säilöön.|
Azure-tallennustilan tilin | Ei | Azure-tallennustilan ei sisällä tietoja Järjestelmäpäivitykset.|  

### <a name="collection-frequency"></a>Sivustokokoelman korkojakso

Kunkin hallitun Windows-tietokoneen tarkistus suoritetaan kahdesti päivässä.  Kun päivitys on asennettu, sen tiedot päivitetään 15 minuutin kuluessa.  

Kunkin hallitun Linux tietokoneen tarkistus suoritetaan 3 tunnin välein.  

## <a name="using-the-solution"></a>Ratkaisun avulla

Kun lisäät päivityksen Management-ratkaisun OMS työtilan- **Päivitysten hallinta** -ruutu lisätään OMS Raporttinäkymät-ikkunan. Tässä ruudussa näkyy Laske- ja tietokoneiden määrä graafinen esitys ympäristössäsi edellyttävän tällä hetkellä Järjestelmäpäivitykset.<br><br>
![Päivitä hallinnan yhteenveto-ruutu](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Päivitä arvioinnit tarkasteleminen

Valitse Avaa **Päivitysten hallinta** Raporttinäkymät-ikkunan **Päivitysten hallinta** -ruutu. Koontinäytön sisältää sarakkeet seuraavassa taulukossa. Kukin sarake sisältää enintään kymmenen kohteet, jotka vastaavat määritettyä aluetta ja aikavälin kyseisen sarakkeen ehdot. Voit suorittaa haun lokitiedoston, joka palauttaa kaikki tietueet, **näet kaikki** sarakkeen alareunassa valitsemalla tai napsauttamalla sarakkeen otsikkoa.

Sarakkeen | Kuvaus|
----------|----------|
**Tietokoneiden puuttuvat päivitykset** ||
Kriittinen tai päivitykset | Kymmenen tietokoneissa, joihin ei näy päivittää yläreunassa lajiteltu päivitysten määrästä luetteloiden ne ovat puuttuu. Valitse Suorita kaikki tietokoneen tietueiden päivittäminen, joka palauttaa log haun tietokonenimi.|
Kriittinen tai 30 päivää vanhemmat päivitykset| Tunnistaa tietokoneita, jotka ovat tärkeitä puuttuu tai on turvapäivitykset, jotka on ryhmitelty pituutta päivityksen julkaisemisen jälkeen. Valitse jokin tapahtumista suorittaa log haku palauttaa kaikki puuttuu ja kriittiset päivitykset.|
**Pakollinen puuttuu päivitykset**||
Kriittinen tai päivitykset | Luettelo vaihtelee päivitykset, että tietokoneisiin ei näy puuttuvat päivitykset luokassa tietokoneiden määrän mukaan lajiteltuina. Valitse Suorita kaikki Päivitä käyttäjäryhmälle tietueet, joka palauttaa log haun luokitus.|
**Päivitä ominaisuuksissa**||
Päivitä ominaisuuksissa | Tällä hetkellä ajoitettu päivitys ominaisuuksissa ja keston vasta seuraavan ajoitettua tehtävää määrä.  Valitse aikataulujen tarkasteleminen, käynnissä ja valmis päivitykset tai uusi käyttöönotto-ruutu.|  
<br>  
![Päivitä hallinnan yhteenveto-koontinäytössä](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Päivitä hallinta Raporttinäkymät-ikkunan tietokoneen näkymä](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Päivitä hallinta Raporttinäkymät-ikkunan paketti-näkymässä](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Päivitysten asentaminen

Kun päivitykset on arvioitu kaikkien ympäristössäsi Windows-tietokoneissa, voit on tarvittavat päivitykset luomalla *Päivityksen käyttöönotto*.  Update-käyttöönoton on ajoitettu asennettuun tarvittavat päivitykset Windows-tietokoneille.  Voit määrittää päivämäärän ja kellonajan tietokoneen tai ryhmä tietokoneita, jotka sisällytetään lisäksi käyttöönottoa varten.  

Päivitykset on asennettu Azure automaatio runbooks mukaan.  Et voi tällä hetkellä tarkastella näitä runbooks ja ne ei edellytä mitään määritys.  Päivitä käyttöönoton luomisen jälkeen luomassaan aikataulun, alkaa perusmuodon päivityksen runbookin määritettynä ajankohtana mukana tietokoneissa.  Tämä perusmuodon runbookin aloittaa lapsen runbookin kunkin Windows-agentti, joka suorittaa tarvittavat päivitykset.  

### <a name="viewing-update-deployments"></a>Päivitä ominaisuuksissa tarkasteleminen

Valitse aiemmin luotu päivityksen ominaisuuksissa luettelon tarkasteleminen **Päivityksen käyttöönotto** -ruutu.  Ne on ryhmitelty tilan – **ajoitettu**, **käynnissä**ja **Valmis**.<br><br> ![Päivitä ominaisuuksissa ajoitus-sivulla](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

Seuraavassa taulukossa on kuvattu näkyvät kunkin käyttöön ominaisuudet.

Ominaisuus | Kuvaus|
----------|----------|
Nimi | Update-käyttöönoton nimi.|
Aikataulu | Aikataulun tyyppi.  *OneTime* ei tällä hetkellä vain mahdollinen arvo.|
Aloitusaika|Päivämäärä ja aika, päivitys-käyttöönoton on ajoitettu alkavaksi.|
Kesto | Update-käyttöönoton voi suorittaa minuuttimäärä.  Jos kaikki päivitykset eivät asennu tämän ajan kuluessa, jäljellä olevat päivitykset on odotettava seuraavan päivityksen käyttöönottoon asti.|
Palvelinten | Käyttöönoton päivitys vaikuttaa tietokoneiden määrä.|
Tila | Update-käyttöönoton nykyinen tila.<br><br>Mahdolliset arvot ovat:<br>-Ei ole aloitettu<br>-Käytössä<br>-Valmis|  

Valitse Update-ympäristöön, voit tarkastella sen tiedot-näyttö, jossa sisältää sarakkeet seuraavassa taulukossa.  Näiden sarakkeiden täytetään ei, jos päivitys-käyttöönoton ei ole vielä aloitettu.<br>

Sarakkeen | Kuvaus|
----------|----------|
**Tietokoneen tulokset**||
Onnistui | Näyttää päivityksen käyttöönoton tietokoneiden määrä tilan mukaan.  Valitse tila, jonka haluat suorittaa log haku palauttaa kaikki Päivitä tilan tietueiden päivittäminen käyttöönottoa varten.|
Tietokoneen asennustilan| Näyttää päivityksen käyttöönottoa ja päivitykset, jotka asennettiin onnistuneesti prosenttiosuus tietokoneet. Valitse jokin tapahtumista suorittaa log haku palauttaa kaikki puuttuu ja kriittiset päivitykset.|
**Päivitä esiintymän tulokset**||
Esiintymän asennustilan | Luettelo vaihtelee päivitykset, että tietokoneisiin ei näy puuttuvat päivitykset luokassa tietokoneiden määrän mukaan lajiteltuina. Valitse tietokone suorittaa log haku palauttaa kaikki tietueiden tietokoneen päivittäminen.|  
<br><br> ![Yleistä päivityksen käyttöönoton tulokset](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Update-käyttöönoton luominen

Luo uusi päivitys-käyttöönoton valitsemalla Avaa **Uuden päivityksen käyttöönottoa** sivu näytön yläreunassa olevaa **Lisää** -painiketta.  Anna ominaisuudet seuraavan taulukon arvot.

Ominaisuus | Kuvaus|
----------|----------|
Nimi | Update-käyttöönoton yksilöllinen nimi.|
Aikavyöhyke | Käytettävä alkamisajan aikavyöhyke.|
Aloitusaika | Aloita päivitys-käyttöönotto päivämäärä ja kellonaika.|
Kesto | Update-käyttöönoton voi suorittaa minuuttimäärä.  Jos kaikki päivitykset eivät asennu tämän ajan kuluessa, jäljellä olevat päivitykset on odotettava seuraavan päivityksen käyttöönottoon asti.|
Tietokoneiden | Tietokoneiden tai ryhmien nimet, tietokoneen sisällytettävien päivityksen käyttöönotto.  Valitse vähintään yksi tapahtumat avattavasta luettelosta.|
<br><br> ![Uusi päivitys käyttöönottoa sivu](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Aika-alue

Oletusarvon mukaan Update Management-ratkaisun analysoida tietoja alue on 1 viimeisenä luodaan kaikki yhdistetyn hallinta-ryhmistä. 

Voit muuttaa tietojen aikavälin valitsemalla **tietojen perusteella** Raporttinäkymät-ikkunan yläosassa. Voit valita tietueet luodaan tai päivitetään 1 päivä tai 6 tuntia viimeisen 7 päivän kuluessa. Tai voit valita **mukautetun** ja Määritä mukautettu päivämääräväli.<br><br> ![Mukautettu aikaväli-vaihtoehto](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Kirjaudu Analytics-tietueet

Päivitä Management-ratkaisun Luo kahdentyyppisiä tietueita OMS säilö.

### <a name="update-records"></a>Tietueiden päivittäminen

Tietueen tyyppi, **Päivitä** luodaan jokaisen päivityksen, joka on asennettu tai tarvittavat kunkin tietokoneessa. Päivittää tietueita on määrittämäsi ominaisuudet seuraavan taulukon.

Ominaisuus | Kuvaus|
----------|----------|
Tyyppi | *Päivitys*|
SourceSystem | Tietolähde, jota hyväksytty-päivityksen.<br>Mahdolliset arvot ovat:<br>-Microsoft päivitys<br>-Windows päivitys<br>-SCCM<br>-Linux palvelimet (hakemisen paketin valvojien)|
Hyväksytty | Määrittää, onko päivitys hyväksytty asennusta varten.<br> Linux-palvelinten tämä on tällä hetkellä valinnainen kuin OMS korjausta ei ole käytössä.|
Windowsin luokitus | Päivityksen luokitus.<br>Mahdolliset arvot ovat:<br>-Sovellukset<br>-Kriittiset päivitykset<br>-Määritelmä päivitykset<br>-Pakkaa ominaisuus<br>-Suojaus päivitykset<br>-Palvelupaketit<br>-Update Rollup<br>-Päivitykset|
Linux luokitus | Päivityksen cassification.<br>Mahdolliset arvot ovat:<br>-Kriittiset päivitykset<br>-Suojaus päivitykset<br>-Muut päivitykset|
Tietokoneen | Tietokoneen nimi.|
InstallTimeAvailable | Määrittää, onko asennukseen kuluvaa aikaa käytettävissä muiden tekijöiden, joka on asennettuna sama päivitys.|
InstallTimePredictionSeconds | Arvioitu asennuksen sekunteina muiden tekijöiden, sama päivityksen asennuksen perusteella.|
KBID | Knowledge Base-artikkeli, joka kuvaa päivitys tunnus.|
ManagementGroupName | SCOM tekijöiden hallinta-ryhmän nimi.  Muiden tekijöiden tämä on AOI -<workspace ID>.|
MSRCBulletinID | Päivityksen kuvaava Microsoftin security tunnus.|
MSRCSeverity | Microsoftin security vakavuus.<br>Mahdolliset arvot ovat:<br>-Kriittinen<br>– Tärkeää<br>-Valvonta|
Valinnainen | Määrittää, onko päivitys on valinnainen.|
Tuotteen | Päivitys on tuotteen nimi.  Valitse **Näytä** artikkelin avaaminen selaimessa.|
PackageSeverity | Tämä päivitys korjaa ilmoittaa Linux distro toimittajien haavoittuvuuden vakavuus. | 
PublishDate | Päivämäärä ja aika, päivitys on asennettu.|
RebootBehavior | Määrittää, jos päivitys pakottaa uudelleen.<br>Mahdolliset arvot ovat:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Päivityksen versionumero.|
SourceComputerId | Yksilöivä tietokoneen GUID-tunnus.|
TimeGenerated | Päivämäärä ja aika, tietue on päivitetty.|
Otsikko | Päivityksen otsikko.|
UpdateID | GUID-tunnus yksilöllisesti päivitys.|
UpdateState | Määrittää, onko päivitys asennettu tässä tietokoneessa.<br>Mahdolliset arvot ovat:<br>-Asennettu - päivitys on asennettu tässä tietokoneessa.<br>-Tarvitaan - päivitys ei ole asennettu ja tarvitaan tässä tietokoneessa.|  

<br>
Kun suoritat log etsinnän, joka palauttaa tietueet, joiden **päivityksen** tyyppi voit valita **päivitykset** näkymän joka näkyy ruutujoukossa yhteenvetojen haun palauttamat päivitykset. Voit valita tapahtumien **puuttuu ja käytetään päivitykset** ja **pakolliset ja valinnaiset päivitykset** ruutujen lisääminen laajuus joukosta päivittää näkymän. Valitse yksittäiset tietueet palauttaa **luettelo** tai **taulukko** -näkymä.<br> 

![Etsi päivitys Näytä loki tietuetyypin päivityksessä](./media/oms-solution-update-management/update-la-view-updates.png)  

**Taulukkonäkymässä** Napsauta **KBID** minkä tahansa tietueen Avaa selaimessa Knowledge Base-artikkelista. Voit nopeasti lisätietoja tietyn päivityksen tietoja.<br> 

![Ruutujen tietuetyypin muutoksia log haun taulukkonäkymässä](./media/oms-solution-update-management/update-la-view-table.png)

Valitse **luettelonäkymässä** Avaa Knowledge Base-artikkelista KBID **näkymän** valinnan.<br>

![Lokitiedoston haun luettelonäkymän ruudut tietuetyypin muutoksia](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary tietueet

Tietueen, jonka tyyppi on **UpdateSummary** luodaan Windows-agentti jokaisessa tietokoneessa. Tämä tietue päivittyy aina, kun päivitykset tietokoneeseen tarkistetaan. **UpdateSummary** tietueella on ominaisuudet seuraavan taulukon.

Ominaisuus | Kuvaus|
----------|----------|
Tyyppi | UpdateSummary|
SourceSystem | OpsManager |
Tietokoneen | Tietokoneen nimi.|
CriticalUpdatesMissing | Tärkeitä päivityksiä ei löydy tietokoneessa määrä.|
ManagementGroupName | SCOM tekijöiden hallinta-ryhmän nimi. Muiden tekijöiden tämä on AOI -<workspace ID>.|
NETRuntimeVersion | .NET-runtime asennettuna tietokoneeseen versio.|
OldestMissingSecurityUpdateBucket | Aikajakson luokitella ajan jälkeen vanhimmasta puuttuu suojauksen tässä tietokoneessa on julkaistu.<br>Mahdolliset arvot ovat:<br>-Vanhemmat<br>-180 päivää sitten<br>-150 päivää sitten<br>-120 päivää sitten<br>-90 päivää<br>– 60 päivää sitten<br>-Siirry 30 päivää<br>-Viimeisimmät|
OldestMissingSecurityUpdateInDays | Päivien jälkeen vanhimmasta puuttuu suojauksen tässä tietokoneessa on julkaistu.|
OsVersion | Tietokoneeseen asennetun käyttöjärjestelmän versio.|
OtherUpdatesMissing | Muiden puuttuu tietokoneen päivitysten määrä.|
SecurityUpdatesMissing | Puuttuvat tietokoneen päivitysten määrä.|
SourceComputerId | Yksilöivä tietokoneen GUID-tunnus.|
TimeGenerated | Päivämäärä ja aika, tietue on päivitetty.|
TotalUpdatesMissing |Puuttuvat tietokoneen päivitysten määrä.|
WindowsUpdateAgentVersion | Versionumero tietokoneen Windows Update-agentti.|
WindowsUpdateSetting | Miten tietokoneen asentaa tärkeät päivitykset-asetusta.<br>Mahdolliset arvot ovat:<br>– Ei käytössä<br>-Ilmoittaa ennen asennusta<br>-Ajoitettu asennus|
WSUSServer | Jos tietokoneessa on määritetty käyttämään WSUS URL-osoite palvelimen.|  

## <a name="sample-log-searches"></a>Esimerkki log haut

Seuraavassa taulukossa on esimerkki log etsii päivittää tietueita keräämiä tämä ratkaisu. 

Kyselyn | Kuvaus|
----------|----------|
Kaikkien tietokoneissa, joissa puuttuvat päivitykset | Tyyppi = päivityksen UpdateState = tarvitaan valinnainen = false & #124; Valitse tietokone, otsikon, KBID, luokittelu, UpdateSeverity, PublishedDate|
Puuttuvat päivitykset tietokoneen "COMPUTER01.contoso.com" (korvaa oman tietokonenimi) | Tyyppi = päivityksen UpdateState = tarvitaan valinnainen = false tietokoneen = "COMPUTER01.contoso.com" & #124; Valitse tietokone, otsikon, KBID, tuote, UpdateSeverity, PublishedDate|
Kaikkien tietokoneissa, joissa ei löydy kriittinen tai päivitykset | Tyyppi = päivityksen UpdateState = tarvitaan valinnainen epätosi (luokitus = "Turvapäivitykset" tai luokitus = "Kriittiset päivitykset")|
Kriittinen tai suojauksen päivitykset manuaalisesti käytettyjen koneet tarvitsemia päivitykset | Tyyppi = päivityksen UpdateState = tarvitaan valinnainen epätosi (luokitus = "Turvapäivitykset" tai luokitus = "Kriittiset päivitykset") tietokone tuumaa {tyyppi = UpdateSummary WindowsUpdateSetting = manuaalinen & #124; Eri tietokoneen} & #124; Eri KBID|
Virhe tapahtumien tietokoneissa, joissa on puuttuu kriittinen tai suojaukseen liittyviä tarvittavat päivitykset | Tyyppi = tapahtuman EventLevelName virhe tietokone tuumaa = {tyyppi = päivitys (luokitus = "Turvapäivitykset" tai luokitus = "tärkeät päivitykset-) UpdateState = tarvitaan valinnainen = false & #124; Eri tietokoneen}|
Kaikissa tietokoneissa, josta puuttuvat update Rollup | Tyyppi = päivityksen valinnainen = false luokitus = "Update Rollup" UpdateState = tarvitaan & #124; Valitse tietokone, otsikon, KBID, luokitus, UpdateSeverity, PublishedDate|
Eri tietokoneissa puuttuvat päivitykset | Tyyppi = päivityksen UpdateState = tarvitaan valinnainen = false & #124; Eri osaston|
WSUS tietokoneen jäsenyys | Tyyppi = UpdateSummary & #124; mittaa count() WSUSServer mukaan|
Automaattisten päivitysten määrittäminen | Tyyppi = UpdateSummary & #124; mittaa count() WindowsUpdateSetting mukaan|
Tietokoneissa, joissa automaattinen päivitys on poistettu käytöstä | Tyyppi = UpdateSummary WindowsUpdateSetting = manuaalinen|  
Kaikki Linux tietokoneissa, joissa on paketin päivitys saatavilla luettelo | Tyyppi = päivitys ja OSType = Linux ja UpdateState! = "Ei tarvita" & #124; mittaa count() tietokoneella|
Kaikki Linux tietokoneissa, joissa on paketin päivitys saatavilla luettelossa, joka korjaa kriittinen tai suojauksen haavoittuvuuden | Tyyppi = päivitys ja OSType = Linux ja UpdateState! "Ei tarvita" = ja (luokitus = "tärkeät päivitykset-tai luokitus ="Turvapäivitykset") & #124; mittaa count() tietokoneella|
Luettelon kaikki paketteja, jotka on saatavana päivitys | Tyyppi = päivitys ja OSType = Linux ja UpdateState! = "Ei ole tarpeen"|
Kaikki paketteja, jotka on saatavana päivitys luettelossa, joka korjaa kriittinen tai suojauksen haavoittuvuuden | Tyyppi = päivitys ja OSType = Linux ja UpdateState! "Ei tarvita" = ja (luokitus = "tärkeät päivitykset-tai luokitus ="Turvapäivitykset")|
Kaikissa "Ubuntu" käytettävissä minkä tahansa päivityksessä tietokoneissa luettelo | Tyyppi = päivitys ja OSType = Linux ja OSName = Ubuntu & #124; mittaa count() tietokoneella|

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lokitiedoston Analytics](../log-analytics/log-analytics-log-searches.md) Log hakujen avulla voit tarkastella yksityiskohtaisia Päivitä tiedot.

- [Luo omia raporttinäkymiä](../log-analytics/log-analytics-dashboards.md) näyttäminen yhteensopivuuden hallitun tietokoneissa.

- [Luo ilmoituksia](../log-analytics/log-analytics-alerts.md) , kun päivityksiä tunnistetaan puuttuvat tietokoneille tai tietokone on poistettu käytöstä Automaattiset päivitykset.  




