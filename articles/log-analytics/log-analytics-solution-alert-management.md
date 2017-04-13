<properties
   pageTitle="Ilmoita hallintaratkaisu toimintojen hallinta Suite (OMS) | Microsoft Azure"
   description="Lokitiedoston Analytics ilmoituksen Management-ratkaisun avulla voit analysoida kaikki ilmoitukset-ympäristössä.  Lisäksi ilmoitusten OMS luodaan, se tuodaan ilmoitusten yhdistetyn System Center Operations Manager (SCOM) hallinta-ryhmistä Log Analytics."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Ilmoitusten hallintaratkaisu toimintojen hallinta Suite (OMS)

![Ilmoitusten hallinta-kuvake](media/log-analytics-solution-alert-management/icon.png) Ilmoitusten Management-ratkaisun avulla voit analysoida kaikki ilmoitukset-ympäristössä.  Lisäksi ilmoitusten OMS luodaan, se tuodaan ilmoitusten yhdistetyn System Center Operations Manager (SCOM) hallinta-ryhmistä Log Analytics.  Ilmoitusten Management-ratkaisun tarjoaa ympäristöissä useita hallinta-ryhmiin, kaikki hallinta ryhmien kootut Näytä ilmoitukset.

## <a name="prerequisites"></a>Edellytykset

- Tuominen SCOM ilmoituksia, tämä ratkaisu edellyttää yhteyttä OMS-työtilan ja SCOM hallinta ryhmän puolesta [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md)kuvatut toimet.  


## <a name="configuration"></a>Määritys

Lisää ilmoitusten Management-ratkaisun OMS työtilan käyttämällä [Lisää ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.

## <a name="management-packs"></a>Management Pack-paketit

Jos SCOM hallinta-ryhmä on yhdistetty OMS työtilan, valitse seuraavat management Pack-paketit asennetaan SCOM lisättäessä tämä ratkaisu.  Ei ole määritysten tai ylläpito näiden management Pack-pakettien pakollinen.  

- Microsoft System Center Advisor ilmoitusten hallinta (Microsoft.IntelligencePacks.AlertManagement)

Lisätietoja siitä, miten ratkaisu management Pack-paketit päivitetään on artikkelissa [Yhteyden Operations Manager Log Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Tietojen kerääminen

### <a name="agents"></a>Agenttien vuoksi

Seuraavassa taulukossa on kuvattu yhdistettyjen lähteiden, jotka tukevat tätä ratkaisua.

| Yhdistettyjen lähde | Tuki | Kuvaus |
|:--|:--|:--|
| [Windowsin agenttien vuoksi](log-analytics-windows-agents.md) | Ei | Suoraan Windows tekijöiden Luo SCOM ilmoitukset. |
| [Linux agenttien vuoksi](log-analytics-linux-agents.md) | Ei | Suora Linux tekijöiden Luo SCOM ilmoitukset. |
| [SCOM hallinta-ryhmä](log-analytics-om-agents.md) | Kyllä | Ilmoituksia, jotka on luotu SCOM tekijöiden toimitettu hallinta-ryhmä ja siirtää sitten Log Analytics.<br><br>Suora yhteyden Log Analytics SCOM-agentti ei ole pakollinen. Hallinta-ryhmästä lähettää ilmoituksen tiedot OMS säilöön. |
| [Azure-tallennustilan tilin](log-analytics-azure-storage.md) | Ei | Azure-tallennustilan tilin ei tallenneta SCOM ilmoitukset. |

### <a name="collection-frequency"></a>Sivustokokoelman korkojakso

Luodaan OMS ilmoitukset ovat käytettävissä ratkaisun heti.  Ilmoitusten tiedot lähetetään SCOM hallinta-ryhmästä Log Analytics 3 minuutin välein.  

## <a name="using-the-solution"></a>Ratkaisun avulla

Kun lisäät ilmoituksen Management-ratkaisun OMS lisääminen työtilaan, **Ilmoitusten hallinta** -ruutu lisätään OMS Raporttinäkymät-ikkunan.  Tässä ruudussa näkyy Laske- ja graafinen kuvaus aktiivisena viimeisen 24 tunnin kuluessa luodut hälytykset määrä.  Et voi muuttaa tähän aikaväli.

![Ilmoitusten hallinta-ruutu](media/log-analytics-solution-alert-management/tile.png)

Valitse Avaa **Ilmoitusten hallinta** Raporttinäkymät-ikkunan **Ilmoitusten hallinta** -ruutu.  Koontinäytön sisältää sarakkeet seuraavassa taulukossa.  Kunkin sarakkeessa yläreunan kymmenen ilmoituksia kyseisen sarakkeen ehdoissa määritetyn alueen ja aikavälin määrän mukaan.  Voit suorittaa haun lokitiedoston, joka sisältää koko luettelon napsauttamalla sarakkeen alareunassa **näet kaikki** tai napsauttamalla sarakkeen otsikkoa.

| Sarakkeen| Kuvaus |
|:--|:--|
| Kriittinen ilmoitukset | Kaikki ilmoitukset ja ilmoitusten nimen mukaan ryhmiteltynä kriittinen vakavuus.  Napsauta ilmoituksen nimeä, suorita log haku palauttaa kaikki tietueet ilmoituksen. |
| Varoitus-ilmoitukset | Kaikki ilmoitukset ja ilmoitusten nimen mukaan ryhmiteltynä varoitus vakavuus.  Napsauta ilmoituksen nimeä, suorita log haku palauttaa kaikki tietueet ilmoituksen. |
| Aktiivinen SCOM ilmoitukset | Kaikki SCOM ilmoitukset avulla state muu kuin *Suljettu* ilmoituksen luoneen lähteen Ryhmittelyperuste. |
| Kaikki aktiivinen ilmoitukset | Kaikki ilmoitukset, ja kaikki vakavuus ilmoitusten nimen mukaan ryhmiteltynä. Sisältää vain SCOM ilmoitukset ja minkä tahansa muun kuin *Suljettu*.|

Jos siirryt oikealle, koontinäyttö luettelo Yleiset kyselyt, voit napsauttaa painiketta ilmoituksen tietojen [log haun](log-analytics-log-searches.md) tekemiseen.

![Ilmoitusten hallinta-koontinäyttö](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Laajuus ja kellonajat

Oletusarvon mukaan analysoida ilmoitusten Management-ratkaisun ilmoituksia alue on luotu viimeisen seitsemän päivän ajalta kaikki yhdistetyn hallinta-ryhmistä.  

![Ilmoitusten hallinta laajuus](media/log-analytics-solution-alert-management/scope.png)

- Voit muuttaa hallinta-ryhmien mukaan analyysiin valitsemalla **laajuus** Raporttinäkymät-ikkunan yläosassa.  Voit valita kaikkien yhdistettyjen hallinta ryhmien **Yleinen** tai **Management Group** , jos haluat valita yksittäisen hallinta-ryhmä.

- Voit muuttaa aikavälin ilmoituksia valitsemalla Raporttinäkymät-ikkunan yläreunassa **tietojen perusteella** .  Voit valita ilmoitusten luodaan 1 päivä tai 6 tuntia viimeisen seitsemän päivän sisällä.  Tai voit valita **mukautetun** ja Määritä mukautettu päivämääräväli.

## <a name="log-analytics-records"></a>Kirjaudu Analytics-tietueet

Ilmoitusten Management-ratkaisun analysoi tietueisiin, jonka tyyppi on **ilmoitus**.  Se ilmoitusten tuominen SCOM ja luoda **ilmoituksen** ja SourceSystem **OpsManager**, joissa jokaisessa vastaavaa tietuetta.  Nämä tietueet on määrittämäsi ominaisuudet seuraavan taulukon.  

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi | *Ilmoitus* |
| SourceSystem | *OpsManager* |
| AlertContext | Tieto-osa, joka aiheuttaa ilmoituksen XML-muodossa luotavan tiedot. |
| AlertDescription | Ilmoituksen yksityiskohtainen kuvaus. |
| AlertId | Ilmoituksen GUID-tunnus. |
| AlertName | Ilmoituksen nimeä. |
| AlertPriority | Ilmoituksen prioriteettitaso. |
| AlertSeverity | Ilmoituksen vakavuustaso. |
| AlertState | Ilmoituksen uusimman tarkkuus tila. |
| Viimeksi muokannut | Ilmoituksen viimeksi muokanneen käyttäjän nimi. |
| ManagementGroupName | Hallinta-ryhmä, jossa ilmoituksen luotiin nimi. |
| RepeatCount | Sama ilmoitus luotiin samaan ajan määrän seurattava objektin jälkeen on selvitetty. |
| ResolvedBy | Ratkaistu ilmoituksen sen käyttäjän nimi. Jos ilmoitusta ei vielä ole ratkaistu tyhjä. |
| SourceDisplayName | Seurantaa objekti, joka on luotu ilmoituksen näyttönimi. |
| SourceFullName | Koko nimi seurantaa objektia, jonka luoda ilmoituksen. |
| TicketId | Ilmoitus, jos SCOM-ympäristö on integroitu prosessi, jossa liput ilmoitusten määrittäminen lipun tunnus.  Tyhjä ei ole lipun tunnus on määritetty. |
| TimeGenerated | Päivämäärä ja aika, joka on luotu ilmoituksen. |
| TimeLastModified | Päivämäärä ja kellonaika, ilmoituksen on viimeksi muutettu. |
| TimeRaised | Päivämäärä ja aika, joka on luotu ilmoituksen. |
| TimeResolved | Päivämäärä ja aika, Ilmoita ongelma. Jos ilmoitusta ei vielä ole ratkaistu tyhjä. |

## <a name="sample-log-searches"></a>Esimerkki log haut

Seuraavassa taulukossa on otoksen log etsii ilmoitusten tietueiden keräämiä tämä ratkaisu.  

| Kyselyn | Kuvaus |
|:--|:--|
| Tyyppi = ilmoitusten SourceSystem = OpsManager AlertSeverity = virhe TimeRaised > nyt 24 tunnin | Kriittinen ilmoitusten korotettuna Viimeiset 24 tunnin aikana |
| Tyyppi = ilmoitusten AlertSeverity = varoitus TimeRaised > nyt 24 tunnin | Varoitus ilmoitusten korotettuna Viimeiset 24 tunnin aikana  |
| Tyyppi = ilmoitusten SourceSystem = OpsManager AlertState! = suljettu TimeRaised > nyt 24 tunnin & #124; mittaa count() määränä SourceDisplayName mukaan | Tietolähteiden korotettuna Viimeiset 24 tunnin aikana aktiiviset ilmoitusten avulla |
| Tyyppi = ilmoitusten SourceSystem = OpsManager AlertSeverity = virhe TimeRaised > nyt 24 tunnin AlertState! = suljettu | Kriittinen ilmoitusten käynnistyy, jotka ovat edelleen Viimeiset 24 tunnin aikana |
| Tyyppi = ilmoitusten SourceSystem = OpsManager TimeRaised > nyt 24 tunnin AlertState = suljettu | Ilmoitukset, joka käynnistyy, jotka ovat nyt sulkea Viimeiset 24 tunnin aikana |
| Tyyppi = ilmoitusten SourceSystem = OpsManager TimeRaised > nyt - 1 päivä & #124; mittaa count() määränä AlertSeverity mukaan | Ilmoitusten korotettuna niiden vakavuus ryhmitelty edellisten 1 päivän aikana |
| Tyyppi = ilmoitusten SourceSystem = OpsManager TimeRaised > nyt - 1 päivä & #124; RepeatCount laskeva lajitteleminen | Ilmoitusten korotettuna toista Laske niiden arvon lajiteltuina edellisten 1 päivän aikana |

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [ilmoitusten Log Analytics](log-analytics-alerts.md) lisätietoja hälytysten tekemisen Log Analytics.
