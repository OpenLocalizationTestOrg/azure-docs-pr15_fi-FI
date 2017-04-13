<properties
    pageTitle="Optimoi ympäristössäsi Active Directory-arviointi-ratkaisuun Log Analytics | Microsoft Azure"
    description="Active Directory-arviointi-ratkaisun avulla voit arvioida riski ja palvelin-ympäristöissä kunto säännöllisin väliajoin."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimoi ympäristössäsi Active Directory-arviointi-ratkaisuun Log Analytics

Active Directory-arviointi-ratkaisun avulla voit arvioida riski ja palvelin-ympäristöissä kunto säännöllisin väliajoin. Tämän artikkelin avulla voit asentaa ja käyttää ratkaisun, jotta voit tehdä korjaavat toimenpiteet mahdollisia ongelmia.

Tämä ratkaisu on prioriteettiluetteloon suosituksia tietyn käyttöön server-infrastruktuuria. Suosituksia luokitellaan yli neljä kohdistus alueet, joiden avulla voit nopeasti ymmärretty, ja toimi.

Suosituksia perustuvat tietämyksen ja kokemus Microsoft insinöörien asiakkaan vierailut tuhansia kohteesta. Kunkin suositus ohjeita siitä, miksi ongelma saattaa merkitystä sinulle ja ottamisesta käyttöön ehdotetut muutokset.

Voit valita kohdistus alueet, jotka ovat tärkeimpiä organisaation ja kohti käytössä riskin maksuttomia ja kunnossa ympäristön edistymisen seuraaminen.

Kun olet lisännyt ratkaisun ja arvio on valmis, yhteenveto kohdistus aihealueiden näkyvät ympäristössäsi infrastruktuurin **AD arviointi** Raporttinäkymät-ikkunan. Seuraavissa kohdissa kuvataan, kuinka pystyt käyttämään tietoja **AD arviointi** koontinäyttö, jossa voit tarkastella ja sitten toimintoja suositeltu Active Directory-palvelimen infrastruktuurin.

![Kuva arviointi SQL-ruutu](./media/log-analytics-ad-assessment/ad-tile.png)

![SQL-arviointi Raporttinäkymät-ikkunan kuva](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisuja.

- Toimialueen ohjauskoneen, jotka ovat toimialueen jäseniä arvioidaan on oltava asennettuna agenttien vuoksi.
- Active Directory-arviointi-ratkaisun edellyttää .NET Framework 4 kussakin tietokoneessa, jossa on OMS agentti asennettuina.
- Active Directory-arviointi-ratkaisun lisääminen OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.

    >[AZURE.NOTE] Kun olet lisännyt ratkaisun, AdvisorAssessment.exe-tiedosto lisätään palvelinten agenttien vuoksi. Määritystietoja lukea ja lähettää sitten OMS palvelun pilveen käsittelyä varten. Logiikan käytetään vastaanotetut tiedot ja pilvipalvelussa tietueiden tietoja.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory arvioinnin sivustokokoelman tietojen

Active Directory-arviointi kerää WMI tietoja, tiedot ja suorituskykytietoja käyttämällä agenttien vuoksi, jotka on otettu käyttöön.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmiä agenttien vuoksi, onko Operations Manager (SCOM) ja kuinka usein kerää tietoja, agentti.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Kyllä](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Kyllä](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ei](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Ei](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Kyllä](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 päivää|


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

Suosituksia, joiden avulla voit päivittää, siirtää ja ota käyttöön Active Directory olemassa olevan infrastruktuurin näyttää **päivityksen, siirtymis- ja** - kohdistus tällä alueella.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Olisi tavoitteena sija 100 %: n jokaisen kohdistus-alueella?

Ei välttämättä. Suosituksia perustuvat tietämyksen ja kokemuksen mukaan Microsoft insinöörien kautta tuhansia asiakkaan vierailut kokemukset. Kaksi palvelin ei ole infrastruktuurin ovat samat, ja suosituksia voi olla enemmän tai vähemmän tärkeimpiä. Jotkin suojausta koskevia suosituksia voi olla esimerkiksi vähemmän merkitystä, jos oman näennäiskoneiden eikä niitä julkaista Internetissä. Käytettävyys joitain suosituksia voi olla pienempi kannalta palveluja, jotka tarjoavat pienen tärkeyden luonnoslehtiössä tietojen kerääminen ja raportointi. Ongelmat, jotka ovat tärkeitä kehittynyt business voi olla pienempi tärkeää pysäyttämisestä. Voit tunnistaa kohdistus mitkä alueet olevat oman prioriteetit ja katso, kuinka oman tulosten muuttuvat ajan kuluessa.

Jokaisen suositus sisältää ohjeita siitä, miksi on tärkeää. Nämä ohjeet käytettävä arvioidaan, onko käyttöönoton suositus sopii, IT-palveluiden laatu ja organisaation tarpeisiin.

## <a name="use-assessment-focus-area-recommendations"></a>Käytä arviointi kohdistus alueen suositukset

Ennen kuin voit käyttää arviointi-ratkaisun OMS, sinulla on oltava asennettuna ratkaisu. Lisätietoja asentamisesta ratkaisuja Katso [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md). Kun se on asennettu, voit tarkastella Yhteenveto suosituksista arviointi-ruutua käyttämällä OMS yleiskatsaus-sivulla.

Näytä infrastruktuuri ja sitten Poraudu kyselyjä suosituksia yhteenvedettyinä yhteensopivuus-arvioinnit.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Näytä suositukset kohdistus-alueelle ja korjausta

1. Valitse **Yleiskatsaus** -sivulla server-julkaisuinfrastruktuuri **arviointi** -ruutu.
2. **Arvioinnin** sivulla Tarkista yhteenvetotiedot jossakin kohdistus alueen näiden ja valitse sitten haluat nähdä kohdistus alueen suosituksia.
3. Millä tahansa kohdistus alue-sivulla voit tarkastella Priorisoidut suositukset-ympäristössä. Valitse suositus **Vaikuttaa objektien** tietoja siitä, miksi suositus tehdään.  
    ![Kuva arviointi suositukset](./media/log-analytics-ad-assessment/ad-focus.png)
4. Voit ottaa ehdotetut **Ehdotetut toimenpiteet**korjaavat toimenpiteet. Kun kohde on osoitettu, myöhemmin arvioinnit tietueen, jotka suositeltavat toimenpiteet on otettu ja yhteensopivuuden Pistemäärä kasvaa. Korjatut kohteet näkyvät **Välitetty**objekteina.

## <a name="ignore-recommendations"></a>Ohita suositukset

Jos sinulla on suosituksia, jotka haluat ohittaa, voit luoda tekstitiedosto, joka OMS käytetään estää suosituksia näkymisen arvioinnin tulokset.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Tunnistaa, voit ohittaa suositukset

1.  Työtilan kirjautuminen ja avaa loki haku. Käytä ympäristösi tietokoneet luettelon suositukset, jotka eivät ole läpäisseet seuraava kysely.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Näin näyttökuva Log hakukyselyn: ![epäonnistui suositukset](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Valitse suosituksia, jotka haluat ohittaa. Tarvitset arvot seuraavaan vaiheeseen RecommendationId varten.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Voit luoda ja käyttää IgnoreRecommendations.txt-Tekstitiedosto

1.  Luo IgnoreRecommendations.txt-tiedosto.
2.  Liitä tai kirjoita kukin RecommendationId kunkin toiminnon, jonka haluat Log Analytics Ohita sana omalle rivilleen ja Tallenna ja sulje tiedosto.
3.  Viemällä tiedoston seuraavaan kansioon käyttöön kussakin tietokoneessa, johon haluat OMS Ohita suositukset.
    - Tietokoneissa, joissa on (yhteydessä suoraan tai Operations Manager) Microsoft seuranta Agent - *SystemDrive*: Files\Microsoft Agent\Agent seuranta
    - Operations Manager hallinta palvelimessa - *SystemDrive*: Files\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Voit varmistaa, että suosituksia ohitetaan

Kun seuraava ajoitettu arviointi suoritetaan oletusarvoisesti 7 päivää, määritetty suositukset merkitään *ohitettu* ja ei näy arviointi koontinäytössä.

1. Voit käyttää lokiin hakukyselyt ohitetut suositukset-luettelo.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Jos päätät myöhemmin, että haluat nähdä ohitetut suositukset, poista IgnoreRecommendations.txt tiedostoja tai voit poistaa ne RecommendationIDs.

## <a name="ad-assessment-solutions-faq"></a>AD arviointi ratkaisuja usein kysytyt kysymykset

*Kuinka usein arvio suorittaa?*
- Arvioinnin suorittaa 7 päivän välein.

*Onko voi määrittää arvioinnin tiheyttä?*
- Ei tällä hetkellä.

*Jos toiseen palvelimeen havaitaan, kun Olen lisännyt arviointi-ratkaisun, se arvioidaan?*
- Kyllä, kun se on arvioitu-, sitten 7 päivää havaitaan.

*Jos palvelin on poistettu käytöstä, kun se poistetaan arvioinnin?*
- Jos palvelin ei lähettää tietoja 3 viikon aikana, se poistetaan.

*Mikä on prosessin, joka tekee tietojen keräys nimen?*
- AdvisorAssessment.exe

*Kuinka kauan menee tietojen kerätään?*
- Todellisten tietojen keräys palvelimessa kestää noin 1 tunti. Saattaa kestää kauemmin palvelimissa, joissa on runsaasti Active Directory-palvelimiin.

*Tietojen tyypin kerätään?*
- Kerätään seuraavanlaiset tiedot:
    - WMI
    - Rekisterin
    - Suorituskyvyn laskureita

*Onko voi määrittää kerätä tietoja?*
- Ei tällä hetkellä.

*Miksi näkyviin vain top 10-suosituksia?*
- Sijaan suojausvalintaikkuna monipuolisen hankalaa luettelo tehtävistä, on suositeltavaa, että voit keskittyä osoitteiden Priorisoidut suosituksia ensin. Sen jälkeen, kun ne osoite lisäsuosituksia ole käytettävissä. Jos haluat nähdä yksityiskohtainen luettelo, voit tarkastella suositukset Log-haun avulla.

*Onko voi ohittaa suositus?*
- Katso Kyllä, [Ohita suosituksia](#ignore-recommendations) edellisen osan.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia AD arviointi tiedot ja suositukset.
