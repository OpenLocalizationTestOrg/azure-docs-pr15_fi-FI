<properties
   pageTitle="Käytön aloittaminen toimintojen hallinta Suite suojaus ja valvonta-ratkaisun | Microsoft Azure"
   description="Tämän asiakirjan avulla voit aloittaa toimintojen hallinta Suite suojaus ja valvonta ominaisuudet hybrid cloud seurannassa."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun käytön aloittaminen
Tämän asiakirjan avulla pääset alkuun nopeasti toimintojen hallinta Suite (OMS) suojaus ja valvonta ominaisuudet luotsaa läpi kunkin asetuksen.

## <a name="what-is-oms"></a>Mikä on OMS?
Microsoft toimintojen hallinta Suite (OMS) on Microsoftin pilvipohjainen IT hallintaratkaisu, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuuria. Lisätietoja OMS Lue artikkeli [Toimintojen hallinta Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Koontinäytön OMS suojaus ja valvonta

OMS suojaus ja valvonta-ratkaisu tarjoaa kattavan näkymän organisaation IT suojauksen reagoimisessa huomattavat ongelmat, jotka tärkeintä valmiin hakukyselyt kanssa. **Suojaus ja valvonta** -koontinäyttö on kohteiden kaikki OMS suojauksesta aloitusnäytön. Se on ylätason tietoja tietokoneiden suojaus-tilaan. Se on myös voi tarkastella Viimeiset 24 tuntia, seitsemän päivää kaikki tapahtumia tai mitä tahansa muita mukautettuja aikaväli. Voit käyttää **Suojaus ja valvonta** Raporttinäkymät-ikkunan, toimimalla seuraavasti:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät Raporttinäkymät-ikkunan vasemmasta **asetukset** -ruutu.
2. Valitse **asetukset** -sivu **ratkaisuja** **Suojaus ja valvonta** -vaihtoehto.
3. **Suojaus ja valvonta** raporttinäkymät-ikkuna tulee näkyviin:

    ![Koontinäytön OMS suojaus ja valvonta](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Jos käytät käyttöoikeuspalvelinta ensimmäistä kertaa näiden raporttinäkymien ja sinulla ei ole OMS valvoo laitteita, ruudut ei kaikille agentti saatuja tietoja. Kun olet asentanut agentti, se voi viedä jonkin aikaa täytä, vuoksi mitä näet alun perin puuttuu tietoja, kun ne ovat edelleen lataaminen pilveen.  Tässä tapauksessa on tavallinen Nähdäksesi joitakin ruudut ilman todellista tietoja. Lue lisätietoja OMS agentti asentaminen Windows-järjestelmän ja [muodostaa Linux tietokoneiden OMS](https://technet.microsoft.com/library/mt622052.aspx) lisätietoja siitä, miten voit suorittaa tämän tehtävän Linux-järjestelmän [yhteyden Windows-tietokoneiden suoraan OMS](https://technet.microsoft.com/library/mt484108.aspx) .

> [AZURE.NOTE] Agentti kerää tietoja nykyisen tapahtumat, jotka ovat käytössä, esimerkiksi tietokonenimi ja IP-osoite ja käyttäjän nimen perusteella. Kuitenkin kerätään asiakirjojen ja tiedostojen, tietokannan nimi tai yksityisiä tietoja.   

Logiikan, visualisointi ja tietojen hankintasäännöt, jotka osoitteissa asiakkaiden keskeisiä haasteita kokoelma ratkaisuja. Suojaus ja valvonta on yksi ratkaisu, muiden voidaan lisätä erikseen. Lue lisätietoja siitä, miten voit lisätä uuden ratkaisun artikkelista [Lisää ratkaisuja](https://technet.microsoft.com/library/mt674635.aspx) .

OMS suojaus ja valvonta Raporttinäkymät-ikkunan on järjestetty neljä tärkeintä luokat:

- **Suojaus-toimialueet**: tällä alueella voit voivat edelleen tarkastella suojauksen tietueiden ajan kuluessa, käyttää haittaohjelman arviointi, Päivitä arviointi, verkkosuojauksen, käyttäjätieto- ja käyttää tietoja, tietokoneiden ja suojauksen tapahtumat ja nopeasti käyttää Azure Tietoturvakeskus Raporttinäkymät-ikkunan.
- **Huomattavat ongelmat**: tämän asetuksen avulla voit tunnistaa nopeasti määrä aktiiviset seurantakohteet ja näiden ongelmien vakavuus.
- **Tunnistuksia (ennakkoversio)**: voit tunnistaa hyökkäyksen kuviot havainnollistetaan usein suojausvaroitusten kuin ne asetetaan vastaan resurssit.
- **Uhkien liiketoimintatietojen**: avulla voit tunnistaa hyökkäyksen kuviot kesäolympialaisten visualisointi palvelinten haittaohjelmien IP liikenteen, haittaohjelmien uhkien tyyppi ja kartan, jossa näkyvät mistä nämä IP-osoitteet ovat peräisin kokonaismäärän. 
- **Yleiset suojauksen kyselyt**: Tämä vaihtoehto on luettelo yleisimmistä suojauksen kyselyt, joiden avulla voit valvoa ympäristön. Kun napsautat johonkin näistä kyselyt, se avautuu **Etsi** -sivu kyselyn tuloksiin.

> [AZURE.NOTE] Lisätietoja siitä, miten OMS säilyttää tietosi suojatun lukea miten OMS suojaa tiedot.

## <a name="security-domains"></a>Suojauksen toimialueet

Kun resurssien valvominen on tärkeää voi käyttää nopeasti ympäristön nykyisen tilan. Kuitenkin on myös tärkeää voi takaisin tapahtumia, jotka aiemman, joka aiheuttaa paremman käsityksen mitä tapahtuu ympäristössäsi tiettyyn kohtaan samanaikaisesti. 

> [AZURE.NOTE] tietojen säilytys tapahtuu mukaan OMS hinnat suunnitelma. Saat lisätietoja seuraavasta [Microsoft toimintojen hallinta Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) hinnat sivun.

Tapauksen vastauksen ja forensics tutkimuksen skenaariot suoraan hyötyä käytettävissä **Suojauksen tietueiden ajan kuluessa** ruudun tulokset.

![Suojauksen tietueiden ajan mittaan](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Napsauttaessasi ruudussa **haku** -sivu avautuu, näkyy kyselyn tuloksessa **Suojauksen** tapahtumien (tyyppi = SecurityEvents) tietojen perusteella viimeisten seitsemän päivän aikana, alla kuvatulla tavalla:

![Suojauksen tietueiden ajan mittaan](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Hakutuloksen on jaettu kahteen osaan: vasemmanpuoleisessa ruudussa tutustutaan hajautuksen, löytyi-tietokoneissa, joissa näitä tapahtumia löytyi-tilit, jotka havaittuja näiden tietokoneiden määrä ja toimintojen tyyppeihin suojaustapahtumien määrä. Oikeanpuoleisessa ruudussa on tuloksia yhteensä ja tietokoneen nimi ja tapahtuman aktiviteettiin suojaustapahtumien kronologinen näkymän. Voit myös napsauttaa **Näytä Lisää** voit tarkastella tarkempia tietoja tapahtuman, kuten tapahtumatietoja, Tapahtumatunnuksen ja lähteen.

> [AZURE.NOTE] Lue lisätietoja OMS hakukyselyn [OMS Etsi viittaus](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Antimalware arviointi

Tämän asetuksen avulla voit helposti tunnistaa tietokoneiden riittämätön ja tietokoneissa, joissa haittaohjelman osan mukaan käsiin. Haittaohjelman arviointi tila ja havaittiin uhkien valvottu palvelimissa luetaan ja sitten tiedot lähetetään OMS palvelun pilveen käsittelyä varten. Palvelinten havaita uhkien ja palvelimet eivät riitä käyttöoikeudella näkyvät haittaohjelman arviointi Raporttinäkymät-ikkunan, joka on käytettävissä, kun valitset **Antimalware arviointi** -ruutu. 

![haittaohjelmien arviointi](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Aivan kuin mitä tahansa muita live ruutua OMS Raporttinäkymät-ikkunan käytettävissä, kun napsautat, **haku** -sivu avautuu kyselyn tuloksen. Tämä vaihtoehto, jos valitset **Suojaus-tila**-kohdassa **Ei raportointi** -vaihtoehto sinun on kyselyn tulos, jossa näkyy tämän yksittäisen tiedon, joka sisältää sen tietokoneen nimi ja sen järjestys alla kuvatulla tavalla:

![hakutulokset](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *sija* on sivukomentojen vastaavat suojauksen tilaa riskitekijälle (, käytöstä, päivittää, yms) ja uhkien, jotka löytyvät. Ottaa, jotta koosteet numero auttaa.

Jos valitset sen tietokoneen nimi, sinun on aikajärjestyksessä näkymän tämän tietokoneen suojaus-tila. Tämä on erittäin hyödyllinen skenaariot, jotka sinun on tiedettävä antimalware on asennettu kerran ja se poistettiin vaiheessa.   

### <a name="update-assessment"></a>Päivitä arviointi 

Tämän asetuksen avulla voit nopeasti yleinen näyttäminen suojaukseen liittyviä ongelmia, voit määrittää ja onko tai miten kriittiset päivitykset ovat ympäristössä. OMS suojaus ja valvonta-ratkaisun vain tiettyjen päivitykset visualisoinnin, reaali tiedot tulevat [Järjestelmän päivityksiä ratkaisuja](https://technet.microsoft.com/library/mt484096.aspx), jotka on eri moduuli OMS kuluessa. Tässä on esimerkki päivitykset:

![Järjestelmäpäivitykset](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Lisätietoja päivitykset ratkaisu lukea [Päivitä palvelinten päivityksiä järjestelmä-ratkaisuun](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Tunnistetietojen ja käytön

Käyttäjätietojen pitäisi olla ohjausobjektin tasossa yrityksen yksityisyyden suojaaminen tulisi olla yläreunan prioriteetti. Aiemman on perimeters ympärille organisaatiot ja näiden perimeters on yksi ensisijainen kehittyneen rajat, Internetissä enemmän tietoja ja sovelluksia siirtäminen pilveen käyttäjätietoja tulee uusi ympäröivän. 

> [AZURE.NOTE] Tällä hetkellä tiedot perustuu vain suojauksen tapahtumien kirjaustiedot (tapahtuman tunnus 4624) tulevien Office 365-kirjautumiset ja Azure AD-tiedot, myös sisällytetään.

Käyttäjätietojen toimintojen seuranta osaat ennakoiva toimintoja ennen tapahtuma kestää paikkaan tai uudelleenaktivointi toiminnot lopettavat hyökkäyksen yritys. **Tunnistetietojen ja käytön** Raporttinäkymät-ikkunan sisältää yleiskatsauksen jäsenyyden tilan, mukaan lukien Kirjaudu epäonnistuneiden yritysten määrä, kyseiset yritykset, tilit, jotka on lukittu, tili ja aikana käyttämä käyttäjätiliä muutettu tai salasanan palauttaminen ja tällä hetkellä tilit, jotka olet kirjautunut määrä. 

**Käyttäjätieto- ja Access** -ruutua napsauttaessasi seuraavat raporttinäkymät-ikkuna tulee näkyviin:

![tunnistetietojen ja käytön](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Käytettävissä olevat tämän koontinäytön tiedot heti voi auttaa sinua tunnistaa mahdolliset epäilyttävistä tehtävän. Esimerkiksi ovat 338 yritykset, kirjaudu sisään **järjestelmänvalvojan** ja 100 % nämä yritykset epäonnistui. Tämä voi johtua tämän tilin palvelinten hyökkäystä. Jos valitset tämän tilin Saat lisätietoja, jotka voivat auttaa määrittämään mahdollinen hyökkäys kohde resurssin:

![Etsinnän tulokset](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Yksityiskohtainen raportti on tärkeitä tietoja tapahtuman, mukaan lukien: kohde-tietokonetta, tyyppi (Valitse palvelupyynnön verkon sisäänkirjautumisen) kirjautuminen, tehtävän (tällöin palvelupyynnön 4625) ja jokaisen yrityksen täydellinen aikajana. 

### <a name="computers"></a>Tietokoneiden

Tämä ruutu voidaan käyttää kaikissa tietokoneissa, joissa on aktiivisesti suojauksen tapahtumat. Napsauttaessasi tämä ruutu tulee näkyviin luettelo tietokoneissa, joissa suojaustapahtumien ja tapahtumien määrä kussakin tietokoneessa:

![Tietokoneiden](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Voit jatkaa oman tutkimuksen valitsemalla jokaisessa tietokoneessa ja tarkastella suojauksen tapahtumat, jotka on merkitty.

### <a name="azure-security-center"></a>Azure Tietoturvakeskus

Tämä ruutu on lähinnä pikakuvakkeen käyttämään Azure Tietoturvakeskus Raporttinäkymät-ikkunan. Lue lisätietoja tämän ratkaisun [Azure Tietoturvakeskus käytön aloittaminen](../security-center/security-center-get-started.md) .

## <a name="notable-issues"></a>Huomattavat ongelmat

Tämän ryhmän vaihtoehtojen käyttötarkoitusta on ongelmat, jotka ovat ympäristössäsi mukaan luokitella ne kriittinen, varoitus ja tiedottavat nopeasti näkymää. Visualisoinnin ongelmaan, mutta se on aktiivinen ongelman tyyppi-ruutu ei salli sinun kannattaa tutustua saat tarkempia tietoja, sinun on käytettävä luettelon alaosassa tämä ruutu, jossa on ongelma (nimi) nimi, kuinka monta objektia oli tämä tapahtuu (määrä) ja miten tärkeä se on (vakavuus).

![Huomattavat ongelmat](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Näet ongelmaan oli jo annettu **Suojauksen toimialueet** -ryhmään, joka vahvistaa palveluita tämän näkymän eri alueilla: visualisointi tärkeimmistä ominaisuuksista ympäristössäsi yhdestä paikasta.

## <a name="detections-preview"></a>Tunnistuksia (ennakkoversio)

Tämän asetuksen käyttötarkoitusta on, jotta se tunnistaa nopeasti mahdollisten uhkien ja niiden ympäristön kautta tämän uhkien vakavuus.

![Uhkien Intel](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Tämä vaihtoehto voidaan myös tapauksen vastauksen tutkimuksen aikana arvioiminen ja saada lisätietoja hyökkäystä.

> [AZURE.NOTE] Lisätietoja siitä, miten voit käyttää OMS tapahtumaa vastauksen Katso [miten voit hyödyntää Azure Tietoturvakeskus & Microsoft toimintojen hallinta Suite tapahtumaa vastauksen](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Uhkien liiketoimintatietojen

Uuden uhkien Liiketoimintatieto-osan suojaus ja valvonta-ratkaisun valitseminen puolestaan Visualisoi mahdollinen hyökkäyksen kuviot monin eri tavoin: palvelinten haittaohjelmien IP liikenteen, haittaohjelmien uhkien tyyppi ja kartan, jossa näkyvät mistä nämä IP-osoitteet ovat peräisin kokonaismäärän. Voit käsitellä kartan ja valitse sitten Lisätietoja IP-osoitteet.

Keltainen loitontamista kartassa osoittavat saapuvan tietoliikenteen haittaohjelmien IP-osoitteet. Ei ole usein palvelimet, joilla niitä julkaista Internetissä Nähdäksesi haittaohjelmien saapuvan liikenteen, mutta suosittelemme tarkistaminen nämä yritykset, varmista, että ne onnistui. Ilmaisimet perustuvat IIS-lokeja, WireData ja kirjaa Windowsin palomuuri.  

![Uhkien Intel](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Yleiset suojaus-kyselyt

Yleiset suojaus-kyselyt luettelo voi olla hyötyä, voit avata nopeasti resurssin tiedot ja mukauta sitä ympäristö valmistellaan tarpeiden perusteella. Nämä Yleiset kyselyt ovat seuraavat:

- Kaikki suojaus-toiminnot
- Suojaus toiminnan tietokoneessa "computer01.contoso.com" (korvaa oman tietokonenimi)
- Suojaus toiminnan tietokoneessa "computer01.contoso.com" tilin "Järjestelmänvalvojan" (korvaa oman tietokoneen ja tilin nimet)
- Kirjaudu sisään tehtävän tietokoneella
- Tilit, jotka lopetettu Microsoft antimalware millä tahansa tietokoneella
- Tietokoneissa, joissa Microsoft antimalware prosessi on lopetettu
- Tietokoneissa, joissa "hash.exe" oli suoritettu (korvaa prosessin eri nimellä)
- Kaikki prosessin nimiä, jotka on suoritettu
- Kirjaudu sisään aktiviteetit mukaan tili
- Tilien kuka etäyhteyden kirjautuneena tietokoneessa "computer01.contoso.com" (korvaa oman tietokonenimi)

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa on tuotu OMS suojaus ja valvonta-ratkaisuun. Lisätietoja OMS tietoturvasta on seuraavissa artikkeleissa:

- [Toimintojen hallinta Suite (OMS) yleiskatsaus](operations-management-suite-overview.md)
- [Seuranta ja niihin vastaaminen suojausvaroitusten toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-responding-alerts.md)
- [Resurssien valvominen toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-monitoring-resources.md)
