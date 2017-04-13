<properties
    pageTitle="Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta | Microsoft Azure"
    description="Lokitiedoston Analytics-ratkaisuja, jotka logiikan, visualisointi ja tietojen hankkiminen kokoelma säännöt, jotka tarjoavat arvot poimia ympärillä on tietty ongelma-alue."
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

# <a name="add-log-analytics-solutions-from-the-solutions-gallery"></a>Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta

Lokitiedoston Analytics ratkaisuja on kokoelma **logiikan**, **visualisointi** ja **tietojen hankkiminen säännöt** , jotka sisältävät arvot poimia ympärillä on tietty ongelma-alue. Tässä artikkelissa luettelot-ratkaisujen tuetut Log Analytics ja kerrotaan, kuinka voit lisätä ja poistaa niitä käyttämällä ratkaisuvalikoiman.

Ratkaisujen Salli tarkempaa tietoja, voit:

- tutkia ja toiminnallisia ongelmien ratkaiseminen nopeammin
- kerää ja yhdistää eri tietotyyppeihin on tietokoneen
- auttaa sinua ennakoiva toimintoja, kuten kapasiteetin suunnittelu, korjaustiedoston tilan raportointi ja suojauksen valvonnan kanssa.


>[AZURE.NOTE] Log Analytics sisältää Log hakutoimintoa, joten sinun ei tarvitse asentaa ratkaisu, jos haluat ottaa sen käyttöön. Voit kuitenkin avata tietojen visualisoinnit ja hakuehdotukset tietoja lisäämällä ratkaisuja ratkaisuvalikoimaan.

Kun olet lisännyt ratkaisun, kerätään-infrastruktuurin palvelimiin ja lähetetään OMS-palveluun. Käsittelyä OMS palvelun yleensä kestää muutaman minuutin tunneiksi. Kun palvelu käsittelee tiedot, voit tarkastella sitä OMS.

Voit helposti poistaa ratkaisun, kun sitä ei enää tarvita. Kun poistat ratkaisun, sen tietoja ei lähetetä OMS, mikä voi pienentää käyttävät päivittäin kiintiön, tietojen määrää, jos käytössäsi on jokin.


## <a name="solutions-supported-by-the-microsoft-monitoring-agent"></a>Microsoft Agent seuranta tukemat ratkaisut

Tällä hetkellä palvelimissa, jotka ovat yhteydessä OMS käyttämällä Microsoft Agent seuranta voit käyttää useimpia saatavilla, mukaan lukien ratkaisuja:

- Active Directory-arviointi
- Ilmoitusten hallinta (ilman SCOM ilmoitukset)
- Antimalware
- Muutosten jäljityksen
- Tietoturva
- SQL-arviointi
- Järjestelmäpäivitykset

Seuraavat ratkaisut on kuitenkin *ei* tueta Microsoft Agent seuranta ja vaatia System Center Operations Manager (SCOM) agentti.

- Ilmoitusten hallinta (mukaan lukien SCOM ilmoitukset)
- Kapasiteetin hallinta
- Kokoonpanon arviointi

Viittaavat [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md) Lisätietoja yhteyden SCOM-agentti Log Analytics.

### <a name="to-add-a-solution-using-the-solutions-gallery"></a>Jos haluat lisätä käyttämällä ratkaisuvalikoiman ratkaisu

1. Napsauta OMS yleiskatsaus-sivulla **Ratkaisuvalikoimaan** -ruutua.    
    ![ratkaisuvalikoimaan](./media/log-analytics-add-solutions/sol-gallery.png)
2. Lisätietoja OMS ratkaisuvalikoiman sivulla kunkin käytettävissä ratkaisu. Napsauta nimeä, jonka haluat lisätä OMS-ratkaisun.
3. Ratkaisun, jonka valitsit sivulla näkyy ratkaisun yksityiskohtaisia tietoja. Valitse **Lisää**.
4. Uusi ruutu ratkaisun, jonka lisäsit näkyy sivun OMS ja voit kokeilla sitä käytännössä jälkeen OMS-palvelun käsittelee tietojen yhteenveto.

## <a name="to-configure-solutions"></a>Voit määrittää ratkaisut
1. Sinun on määritettävä joitakin ratkaisuja. Sinun on esimerkiksi määrittäminen automaatio, Azure palauttaminen ja varmuuskopiointi, ennen kuin voit käyttää niitä.
2. Jokin luomiasi sovelluksia Valitse kanavan ruudun yleiskatsaus-sivulla.  
    ![Määritä ratkaisu](./media/log-analytics-add-solutions/configure-additional.png)
3. Valitse-ratkaisun määrittäminen tarvittavat tiedot ja valitse sitten **Tallenna**.  
    ![Määritä ratkaisu](./media/log-analytics-add-solutions/configure.png)

### <a name="to-remove-a-solution-using-the-solutions-gallery"></a>Jos haluat poistaa ratkaisun käyttämällä ratkaisuvalikoiman

1. Valitse OMS yleiskatsaus-sivulla **asetukset** -ruutu.
2. Valitse asetukset-sivun ratkaisut-välilehden kohdasta **Poista** ratkaisun, jonka haluat poistaa.
3. Valitse vahvistusvalintaikkunassa **Kyllä** Poista ratkaisu.

## <a name="data-collection-details-for-oms-features-and-solutions"></a>Sivustokokoelman tietojen OMS ominaisuudet ja ratkaisut

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään OMS ominaisuudet ja ratkaisut. Suora tekijöiden ja SCOM edustajat eivät käytettävä samoja, mutta suora agentti sisältää lisätoimintoja, jotta se muodostaa yhteyden OMS-työtilan ja reitittää välityspalvelimen kautta. Jos käytät SCOM-agentti, on voidaan kohdistaa OMS pitää yhteyttä OMS edustajana. Tässä taulukossa tekijöiden SCOM ovat OMS agenttien vuoksi, jotka ovat yhteydessä SCOM. Saat lisätietoja OMS aiemmin SCOM ympäristön [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md) .

>[AZURE.NOTE] Agentti, jota käytetään tyyppi määrittää, kuinka tiedot lähetetään OMS, seuraavat vaatimukset:

- Voit käyttää joko suoraan edustajan tai SCOM liitetty OMS-agentti.
- Kun SCOM on tehtävä, SCOM agentti tiedot ratkaisun aina lähetetään OMS käyttämällä SCOM hallinta-ryhmä. Lisäksi SCOM on tehtävä, kun SCOM-agentti käytetään ratkaisu.
- Kun taulukossa näkyy, että SCOM agentti tiedot lähetetään OMS käyttämällä hallinta-ryhmässä SCOM ei tarvita, valitse SCOM agentti tiedot aina lähetetään OMS hallinta ryhmien käyttäminen. Suora tekijöiden ohittaa hallinta-ryhmä ja lähettää tietonsa suoraan OMS.
- Kun SCOM agentti tietoja ei lähetetä käyttämällä hallinta-ryhmässä, valitse tiedot lähetetään suoraan OMS – ohittaminen hallinta-ryhmä.


|tietotyyppi| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|---|
|AD-arviointi|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|  7 päivää|
|AD replikoinnin tila|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 päivää|
|Ilmoitusten (Nagios)|Linux|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|saapuessa|
|Ilmoitusten (Zabbix)|Linux|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 minuutti|
|Ilmoitusten (Operations Manager)|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 minuuttia|
|Antimalware|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| HOURLY|
|Kapasiteetin hallinta|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| HOURLY|
|Muutosten jäljityksen|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| HOURLY|
|Muutosten jäljityksen|Linux|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|HOURLY|
|Kokoonpanon arviointi (vanha Advisor)|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| kahdesti päivässä|
|TAPAHTUMIEN SEURANTA|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuuttia|
|IIS-lokit|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuuttia|
|Tärkeimmät Vaults|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuutin|
|Verkko-sovelluksen yhdyskäytävät|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuutin|
|Verkon käyttöoikeusryhmät|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 minuutin|
|Office 365-palveluun|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|ilmoituksen|
|Suorituskyvyn laskureita|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|aikataulussa, vähintään 10 sekuntia|
|Suorituskyvyn laskureita|Linux|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|aikataulussa, vähintään 10 sekuntia|
|Palvelun kangasta|Windows|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 minuuttia|
|SQL-arviointi|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| 7 päivää|
|SurfaceHub|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|saapuessa|
|Syslog|Linux|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|Azure säilöstä: 10 minuutin ajan; -agentti: saapuessa|
|Järjestelmäpäivitykset|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| vähintään 2 kerrotaan luvulla per päivä- ja 15 minuutin päivityksen asentamisen jälkeen|
|Windows-Suojauksen tapahtumalokien|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| Azure tallennuksessa: 10 min; Saat agentti: saapuessa|
|Windowsin palomuurin lokit|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| saapuessa|
|Windowsin tapahtumalokien|Windows|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)| Azure tallennuksessa: 1 min; Saat agentti: saapuessa|
|Wire tiedot|Windows (2012 R2 / 8.1 tai uudempi)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Kyllä](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ei](./media/log-analytics-add-solutions/oms-bullet-red.png)| 1 minuutin välein|

## <a name="log-analytics-preview-solutions-and-features"></a>Kirjaudu Analytics esikatselu ratkaisuja ja toiminnot

Suorittamalla palvelu ja noudattamalla devops käytäntöjä emme voi kumppanin ominaisuudet ja ratkaisujen kehittämiseen asiakkaiden kanssa.

Yksityinen esikatselun aikana on antaa asiakkaiden Accessin pienelle aikainen soveltaminen ominaisuus tai ratkaisu parantamiseksi ja saada palautetta. Tämä aikainen toteutus on mahdollisimman vähän ominaisuuksia, toiminnassa.

Tavoitteenamme on kokeilla asiat nopeasti, jotta löydämme mikä toimii ja mikä ei toimi. Olemme käydä läpi tämän prosessin, kunnes yksityinen esikatselu-asiakkaiden palautetta ilmoittaa meille, että olemme ryhtyä julkisen esikatselu.

Julkinen esikatselun aikana olemme saataville ominaisuus tai ratkaisu kaikkien käyttäjien Lisää palautteen saaminen ja vahvista Microsoftin skaalaus ja tehokkuutta. Tässä vaiheessa:

- Esikatselutoiminnot tulee näkyviin asetukset-välilehti ja kaikki käyttäjät voivat ottaa käyttöön
- Esikatselu-ratkaisuja voi lisätä valikoimaan tai julkaistun komentosarjan avulla

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Mitä esikatselu ominaisuuksista ja ratkaisujen tulee tietää?

Olemme kiinnostunut uusista ominaisuuksista ja ratkaisut ja perinteisen Wordin kanssa voit kehittää ne käsitteleminen.

Esikatselu-ominaisuuksia ja ratkaisut eivät ole oikean kaikille, niin pyytää liittyä yksityinen esikatselu ja julkisen esikatselun ottaminen käyttöön Varmista ennen työskentelyn OK jollakin kehittäminen annettuun.

Kun otat käyttöön esikatselu-toimintoa voi yritysportaalin kautta muistuttaa, että ominaisuus on esikatselu varoitusta.

#### <a name="for-both-private-and-public-preview"></a>*Yksityinen* - ja *julkinen* esikatselua varten

Seuraavat koskee sekä julkisista ja yksityisistä esikatselut:

- Asiat eivät ehkä aina toimi oikein.
  - Ongelmat alueen seuraavilta aliversiot häiritsevyyden jokin eivät toimi lainkaan kautta
- Seurata esikatselussa on negatiivinen vaikutus järjestelmien / ympäristössä
  - Yritämme välttämiseksi tapahtuu järjestelmien käytät OMS kanssa, mutta joskus odottamattomia asioita ilmetä negatiivinen asioita
- Tietojen menetyksen / saattaa vioittua
- Emme voi pyytää kerää vianmäärityslokit tai muita tietoja vianmäärityksessä
- Ominaisuus tai ratkaisu saatetaan poistaa (väliaikaisesti tai lopullisesti)
  - Tutustu learnings perusteella saatat haluta Vapauta ominaisuus tai ratkaisu esikatselun aikana
- Esikatselukuvat eivät välttämättä toimi tai saattaa olla testattu kaikki määritykset ja voi rajoittaa:
  - Käyttöjärjestelmät, joita voidaan käyttää (kuten ominaisuutta voi koskea ainoastaan Linux esikatselussa)
  - Agentti (MMA, SCOM), joita voidaan käyttää tyyppi (kuten ominaisuus ei ehkä toimi SCOM esikatselussa)  
- Esikatselu-ratkaisuihin ja ominaisuuksia ei koske taso-palvelusopimus
- Esikatselutoiminnot käyttö selvittää käyttökustannukset
- Ominaisuuksia tai ominaisuuksia, sinun on haluamasi toiminnon / voi olla hyötyä ratkaisu saattaa olla puuttuvat tai virheelliset
- Ominaisuuksien / ratkaisut eivät välttämättä ole käytettävissä kaikilla alueilla
- Ominaisuuksien / ratkaisuja ei ole välttämättä lokalisoitu
- Ominaisuuksien / ratkaisuja voi olla rajoitukset asiakkaiden tai sen käyttävät laitteet
- Voit joutua komentosarjojen avulla, voit tehdä määrityksen ja ratkaisu/toiminnon käyttöön ottaminen
- Käyttöliittymän (UI) ei voi suorittaa loppuun ja voivat muuttua päivittäin
- Julkinen esikatselut ei ehkä sovellu oman tuotannon / kriittiset järjestelmät

#### <a name="for-private-preview"></a>*Yksityinen* esikatselua varten

Yllä kohteiden lisäksi seuraavat on yksityinen esikatselut:

- Odotamme voit antaa meille palautetta käyttökokemuksen, jotta voimme kehittää ominaisuus tai ratkaisu parempi
- Emme voi ottaa sinuun palautetta, kyselyt, puhelut tai sähköpostin käyttäminen
- Kohteita ei aina toimi oikein
- Emme voi vaatia ei paljastaminen sopimuksen (määräysten) osallistua tai luottamuksellista sisältöä
  - Ennen blogin, tweeting tai muussa yhteydenpito kolmansien osapuolten Tarkista kanssa Järjestelmänhallinnan vastuussa esikatselu ymmärtää paljastaminen koskevat rajoitukset
- Ei suoriteta tuotannon / kriittiset järjestelmät


### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Miten voin hankkia yksityinen esikatselu-ominaisuuksien ja ratkaisujen käyttöoikeudet?

Olemme kutsu asiakkaita yksityinen esikatselut usealla eri tavalla sen mukaan, esikatselun välityksellä.

- Kuukausittainen asiakkaan kyselyn vastaaminen ja antaa oikeuden kanssasi seuranta parantaa mahdollisuuksia kutsuttavalle yksityinen esikatselu.
- Microsoft-tiliä työryhmän voit nimetä voit.
- Voit hankkia sen tiedot, jotka on lähetetty twitter [msopsmgmt](https://twitter.com/msopsmgmt) perusteella
- Voit hankkia sen mukaan tietoja jaetun yhteisön tapahtumat – Etsi us osoitteessa täyttävät ups, kokoukset ja online-yhteisöjen.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) ratkaisuja keräämien yksityiskohtaiset tiedot.
