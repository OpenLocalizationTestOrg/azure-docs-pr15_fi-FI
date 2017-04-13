<properties
    pageTitle="Active Directory replikoinnin tila-ratkaisun Log Analytics | Microsoft Azure"
    description="Active Directory-replikoinnin tila-ratkaisun pack valvoo replikoinnin mahdollisia virheitä Active Directory-ympäristösi säännöllisesti ja raportoi tulokset OMS koontinäytössä."
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

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory replikoinnin tila-ratkaisun Log Analytics

Active Directory on avaimen osa yrityksen IT-ympäristössä. Suuri käytettävyys ja korkean suorituskyvyn varmistamiseksi kunkin toimialueen ohjauskoneen on oma kopio Active Directory-tietokannasta. Toimialueen ohjaimet replikoida toistensa kanssa, jotta Välitä muutokset koko yrityksessä. Virheiden replikoinnin tämä prosessi voi aiheuttaa ongelmia erilaisia koko yrityksessä.

AD replikoinnin tila-ratkaisun pack valvoo replikoinnin mahdollisia virheitä Active Directory-ympäristösi säännöllisesti ja raportoi tulokset OMS koontinäytössä.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Tekijöiden on oltava asennettuna, toimialueen ohjauskoneen, jotka ovat toimialueen jäseniä arvioidaan tai määritetty AD replikoinnin tietojen lähettäminen OMS jäsen-palvelimiin. Windows-tietokoneiden yhdistämisestä OMS on artikkelissa [yhteyden Windows-tietokoneiden lokin Analytics](log-analytics-windows-agents.md). Jos toimialueen ohjauskoneen kuuluu jo aiemmin, johon haluat muodostaa yhteyden OMS System Center Operations Manager-ympäristössä, katso [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md).
- Active Directory-replikoinnin tila-ratkaisun lisääminen OMS työtilan käyttämällä [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.


## <a name="ad-replication-status-data-collection-details"></a>AD-replikoinnin tila sivustokokoelman tietojen

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään AD replikoinnin tila.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Kyllä](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Kyllä](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ei](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ei](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Kyllä](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| 5 päivän välein.|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Vaihtoehtoisesti voit ottaa käyttöön ei toimialueen ohjauskoneen AD tietojen lähettäminen OMS
Jos et halua liittää jokin toimialueen ohjaimet suoraan OMS, voit tehdä OMS yhdistetty tietokoneelta toimialueesi Pack AD replikoinnin tila-ratkaisun tietojen kerääminen ja se lähettää tiedot.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Jos haluat ottaa käyttöön ei toimialueen ohjauskoneen AD tietojen lähettäminen OMS
1.  Varmista, että tietokone on toimialueelle, jota haluat seurata AD replikoinnin tila-ratkaisun avulla.
2.  [Yhdistä Windows-tietokoneen OMS](log-analytics-windows-agents.md) tai [Yhdistä se aiemmin Operations Manager-ympäristön avulla OMS avulla](log-analytics-om-agents.md), jos se ei ole jo liitetty.
3.  Tietokoneessa Määritä seuraava rekisteriavain:
    - Avain: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management ryhmien\<ManagementGroupName > \Solutions\ADReplication**
    - Arvo: **IsTarge**
    - Arvon tiedot: **Tosi**

    >[AZURE.NOTE]Nämä muutokset ei voimaan vasta yhteyttä uudelleen Microsoft seuranta Agent-palvelu (HealthService.exe).

## <a name="understanding-replication-errors"></a>Tietoja replikoinnin virheet
Kun AD replikoinnin tilapäivämäärää OMS lähetetään, näet seuraavankaltaiselta ruutua, joka ilmaisee, kuinka monta replikoinnin virheitä asennetun OMS koontinäytössä.  
![AD replikoinnin tila-ruutu](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kriittinen replikoinnin** -virheitä, jotka ovat tai niiden yläpuolella Active Directory-metsää [poiston elinaika](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) 75 %.

Kun valitset ruudun, näet lisätietoja virheet.
![AD replikoinnin tila Raporttinäkymät-ikkunan](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Kohde palvelimeen ja tietolähteen palvelimen tila
Nämä lavat Näytä tila kohde-palvelimia ja lähde-palvelimien, joka esiintyy replikoinnin virheitä. Luku, kun kunkin toimialueen ohjauskoneen nimi näkyy kyseisen toimialueen ohjauskoneen replikoinnin virheiden määrä.

Kohde-palvelimia ja lähde-palvelimet virheet ovat näkyvissä, koska joitakin ongelmia on helpompi vianmääritys lähde palvelimen perspektiivi ja muiden kohde palvelimen näkökulmasta.

Tässä esimerkissä näet moni kohde-palvelin on suunnilleen sama määrä virheitä, mutta on yksi lähdepalvelin (ADDC35), jossa on monia kaikki muut kuin virheitä. On todennäköistä, että, joka aiheuttaa se voi epäonnistua tietojen lähettäminen sen toistoa kumppaneiden ADDC35 on ongelmia. Valitse ADDC35 ongelmien todennäköisesti ratkaisee monia virheitä, jotka näkyvät kohde server-sivu.

### <a name="replication-error-types"></a>Replikoinnin virhetyypit
Tämä sivu antaa havaita yrityksessä Virhetyyppien tietoja. Jokainen virhe on yksilöivä numeerinen koodi ja viestin, jotka auttavat selvittämään pääkansion aiheuttaa virheen.

Rengas yläreunassa avulla voit verrata, mitä virheitä esiintyä ja kovin usein käyttäjät.

Tämä voidaan näyttää, kun useita toimialueen ohjaimet kohdata saman replikoinnin virheen. Tässä tapauksessa voit ehkä tuttuihin tunnistaa ratkaista yhden toimialueen ohjauskoneen ja sitten toista muiden toimialueen ohjauskoneen, mihin sama virhe.

### <a name="tombstone-lifetime"></a>Poiston elinaika
Poiston elinkaaren määrittää, kuinka kauan poistetun objektin, poiston nimitystä, Active Directory-tietokannan säilytetään. Kun poistetun objektin kulkee poiston elinaika-tarpeettomien kohteiden poistamisprosessi poistaa sen automaattisesti Active Directory-tietokanta.

Oletusarvon poiston elinaika on uusimmat Windows-versioiden 180 päivää, mutta se on 60 päivää vanhemmat versiot ja se voi muuttaa erikseen Active Directory-järjestelmänvalvoja.

On tärkeää tietää, jos sinulla on replikoinnin virheistä, jotka ovat lähellä tai poiston elinaika on. Jos kahden toimialueen ohjauskoneen replikoinnin-virhe, joka jatkuu aiempia poiston elinkaaren, replikointi eivät ole käytettävissä nämä kaksi toimialueen ohjainten välille vaikka replikoinnin virhe korjataan.

Poiston elinaika-sivu auttaa tunnistamaan merkit, jos se on asetuksen nimiä. Jokainen virhe **yli 100 % TSL** luokan edustaa osio, joka on ei replikoida sen lähde- ja palvelimen välillä vähintään poiston elinaika metsää varten.

Tässä tilanteessa korjaaminen yksinkertaisesti replikoinnin virheen eivät ole tarpeeksi. Vähintään sinun on manuaalisesti tutkia tunnistamiseen ja SIIVOA jäävät objektit ennen voit käynnistää replikoinnin. Haluat ehkä myös toimialueen ohjauskoneen poistetaan käytöstä.

Lisäksi tunnistaminen replikoinnin virheitä, joka on samanlainen aiempia poiston elinkaaren myös haluat kiinnittää huomiota **50 75 % TSL** tai **75 100 %: n TSL** luokkiin kuuluvia virheitä.

Nämä ovat virheitä, jotka ovat selvästi jäävät, ei lyhytkestoisia, joten he tarvitsevat luultavasti omaa toimia ratkaisemiseksi. Hyviä uutisia on, että ne ei ole vielä saavuttanut poiston elinaika. Jos näiden ongelmien korjaaminen välittömästi, *ennen kuin* he saavuttaa poiston elinaika-replikoinnin voi käynnistää mahdollisimman vähän manuaalisia toimia.

Aiemmin mainittuja AD replikoinnin tila-ratkaisun dashboard-ruutu näkyy määrä *Kriittinen* replikoinnin virheitä ympäristössä, joka on määritetty virheinä, jotka ovat yli 75 % poiston elinaika (mukaan lukien virheitä, jotka ovat yli 100 % TSL). Pyri pitämään tämä numero 0.

>[AZURE.NOTE] Kaikki poiston elinaika prosentti laskutoimitukset perustuvat todellinen poiston elinkaaren, Active Directory-metsää, jotta voit luottaa näiden prosenttiosuudet ovat oikein, vaikka sinulla olisi mukautetun poiston elinaika-kohdan arvoksi.

### <a name="ad-replication-status-details"></a>AD-replikoinnin tilatiedot
Kun napsautat jossakin kohtaa, näet lisätietoja se Log-haun avulla. Tulokset on suodatettu näyttämään vain kyseisen kohteen liittyvien virheiden. Esimerkiksi jos valitset ensimmäinen **Kohde palvelintila (ADDC02)**-kohdasta toimialueen ohjauskoneen, näet etsinnän tulokset suodatettuna Näytä virheet kyseisen toimialueen ohjauskoneen peruutuksesta kohdepalvelimeen:

![AD replikoinnin tila virheitä hakutuloksissa](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Tässä näkymässä suodattaa edelleen, Muokkaa hakukyselyn ja niin edelleen. Saat lisätietoja Log-haun avulla [Log haut](log-analytics-log-searches.md).

**HelpLink** -kentässä on TechNet-sivulla, että tiettyä virhettä koskevien lisätietojen ja URL-osoite. Voit kopioida ja liittää tämän linkin tietoja vianmäärityksestä ja säätämisessä virheen selaimen osoiteriville.

Voit myös napsauttaa **Vie** tulokset viedään Exceliin. Voit havainnollistaa replikoinnin virhetietojen minkäänlaista haluat.

![viedyt AD replikoinnin tila virheitä Excelissä](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD replikoinnin tila usein kysytyt kysymykset
**K: miten usein on päivitetty AD replikoinnin tilapäivämäärää?**
V: tiedot päivittyvät 5 päivän välein.

**K: onko voi määrittää tietojen päivittämisväliä?**
A: ei tällä hetkellä.

**K: minun täytyy lisätä kaikki toimialue-ohjaimet OMS-työtilan näkyviin replikoinnin tila?**
A: ei, saa lisätä vain yhden toimialueen ohjauskoneen. Jos sinulla on useita toimialueen ohjaimet OMS-työtilassa, ne kaikki tiedot lähetetään OMS.

**Kysymys: en halua lisätä minkä tahansa toimialueen ohjaimet OMS-työtilassa. Voit edelleen käyttää AD replikoinnin tila-ratkaisun?**
V: Kyllä. Voit määrittää voit ottaa tämän rekisteriavaimen arvo. Ohjeaiheessa [ei toimialueen ohjauskoneen lähettää OMS AD tietojen käyttöön](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**K: mikä on prosessin, joka tekee tietojen keräys nimen?**
A: AdvisorAssessment.exe

**K: miten paljon kuluu aikaa tietojen kerätään?**
V: tiedot sivustokokoelman aika määräytyy Active Directory-ympäristössä kokoa, mutta yleensä on pienempi kuin 15 minuuttia.

**K: mitä tyyppisiä tietoja kerätään?**
A: replikoinnin tiedot kerätään LDAP kautta.

**K: onko voi määrittää kerätä tietoja?**
A: ei tällä hetkellä.

**K: mitä oikeuksia pitääkö kerää tietoja?**
A: Active Directory Normaali käyttöoikeudet ovat yleensä riittävät.

## <a name="troubleshoot-data-collection-problems"></a>Tietojen kerääminen ongelmien vianmääritys
Tietojen kerääminen AD replikoinnin tila-ratkaisun pack edellyttää vähintään yksi yhdistetty OMS lisääminen työtilaan toimialueesta. Ennen tämän tekemistä, näyttöön tulee sanoma, joka ilmaisee, että **tiedot kerätään edelleen**.

Jos tarvitset apua yhteyden jokin toimialueen ohjaimet, voit tarkastella [yhteyden Windows-tietokoneiden lokin Analytics](log-analytics-windows-agents.md)-palvelussa. Voit myös jos toimialueen ohjauskoneen on jo muodostettu käytössä System Center Operations Manager-ympäristö, voit tarkastella ohjeet [Yhteyden System Center Operations Manager, loki Analytics](log-analytics-om-agents.md)-palvelussa.

Jos et halua muodostaa tahansa toimialueen ohjaimet suoraan OMS tai SCOM, ohjeaiheessa [ei toimialueen ohjauskoneen lähettää OMS AD tietojen käyttöön](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia Active Directory-replikoinnin tilapäivämäärää.
