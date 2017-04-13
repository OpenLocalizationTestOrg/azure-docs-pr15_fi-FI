<properties 
   pageTitle="Microsoft seuranta tuotteen vertailu | Microsoft Azure"
   description="Microsoft toimintojen hallinta Suite (OMS) on Microsoft cloud perusteella IT management-ratkaisun, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuurin.  Tässä artikkelissa on eri palvelut sisältyvät OMS ja linkkejä yksityiskohtaiset niiden sisältöön."
   services="operations-management-suite"
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft seurantaa tuotteen vertailu

Tässä artikkelissa on System Center Operations Manager (SCOM) ja kirjaudu Analytics toimintojen hallinta Suite (OMS) vertailu niiden arkkitehtuuri, miten resurssit valvoa ja kuinka ne kerääminen tietojen analysointi suorittaa logiikan.  Tämä on Anna niiden erot ja suhteellinen vahvuuksien keskeisiä tietoja.  

## <a name="basic-architecture"></a>Perus-arkkitehtuuri
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Kaikki SCOM osat on asennettu data Centerissä.  Windows- ja Linux tietokoneissa, joihin hallitaan SCOM [agenttien vuoksi on asennettu](http://technet.microsoft.com/library/hh551142.aspx) .  Agenttien vuoksi muodostaa yhteyden [Hallinta-palvelimien](https://technet.microsoft.com/library/hh301922.aspx) jotka yhteydenpito SCOM tietokannan ja tietojen varasto.  Tekijöiden luottavat toimialueen käyttöoikeuksien hallinta-palvelimiin.  Sellaisten luotettuun toimialueeseen voit suorittaa todennus tai liittäminen [Yhdyskäytävän palvelimeen](https://technet.microsoft.com/library/hh212823.aspx).

SCOM edellyttää kahta SQL-tietokantoja, yksi toiminnallisia tiedot ja toisen tietovarasto tukemaan raportointi ja tietojen analysointi.  [Raportoinnin palvelimen](https://technet.microsoft.com/library/hh298611.aspx) suoritetaan SQL Reporting Services-tietovarasto tietoja raportointia. 

SCOM voit valvoa cloud resurssien käytön tuotteissa, kuten [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365: ssä](https://www.microsoft.com/download/details.aspx?id=43708)ja [sisältyy AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html)management Pack-paketit.  Nämä management Pack-paketit Määritä vähintään yksi paikallisen agenttien vuoksi välityspalvelimet etsintää cloud resurssien ja käynnissä olevat työnkulut mitata niiden suorituskykyä ja käytettävyys.  Välityspalvelimen tekijöiden käytetään myös [näytön verkkolaitteet](https://technet.microsoft.com/library/hh212935.aspx) ja muut Ulkoiset resurssit.

Toiminnot-konsolin on Windows-sovellus, joka yhdistää hallinta-palvelimia ja avulla järjestelmänvalvoja voi tarkastella ja kerättyjen tietojen analysointia ja määritä SCOM-ympäristössä.  WWW-konsolin voidaan ylläpitää minkä tahansa IIS-palvelin ja tarjoaa tietojen analysointi-selaimen kautta.

![SCOM arkkitehtuuri](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Lokitiedoston Analytics

Useimmat OMS osat ovat Azure pilveen, jotta voit ottaa käyttöön ja hallita mahdollisimman vähän kustannukset ja järjestelmänvalvojan työmäärään.  Kaikki lokin Analytics keräämät tiedot tallennetaan OMS säilö.

Lokitiedoston Analytics voi kerätä tietoja yhdestä kolmeen lähteet:

- Fyysinen ja näennäiskoneiden Windowsin ja [Microsoft seuranta agentti (MMA)](https://technet.microsoft.com/library/mt484108.aspx) tai Linux ja [Toimintojen hallinta Suite agentti Linux](https://technet.microsoft.com/library/mt622052.aspx).  Nämä koneet voi olla paikallisen tai näennäiskoneiden Azure tai toiseen pilveen.
- Azure-tallennustilan tilin keräämiä Azure työntekijän rooli, web-roolin tai virtuaalikoneen [Azure diagnostiikka](../cloud-services/cloud-services-dotnet-diagnostics.md) tiedoilla.
- [Yhteyden SCOM hallinta-ryhmälle](https://technet.microsoft.com/library/mt484104.aspx).  Tässä määrityksessä niistä yhteyttä SCOM hallinta-palvelimien jotka toimittavat tiedot SCOM tietokantaan kohtaa, johon se on sitten toimitettu OMS tietovaraston.
Järjestelmänvalvojat kerättyjen tietojen analysointia ja määritä lokin Analytics OMS-portaalissa, joita isännöidään Azure ja niitä voi käyttää kaikissa selaimissa.  Voit käyttää näitä tietoja mobiilisovelluksia on tarjolla vakio ympäristöjen.

![Lokitiedoston Analytics-arkkitehtuuri](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Paikallisen ympäristön integroinnissa SCOM ja lokin analysoinnin

Kun SCOM käytetään tietolähteenä lokin analysoinnissa avulla voidaan hyödyntää hybrid, seuranta ympäristössä sekä tuotteet ominaisuuksia.  Voit määrittää aiemmin SCOM tekijöiden hallinnoi OMS, lisäksi siirtymistä management Pack-paketit pitäminen SCOM toiminnot-konsolin kautta.  
Tietoja yhdistetyn SCOM hallinta-ryhmästä toimitetaan Log Analytics neljä tavoilla:

- Tapahtuma- ja suorituskykytietoja agentti keräämiä ja SCOM toimitettu.  Hallintapalvelimet SCOM pitää tiedot sitten Log Analytics.
- Jotkin tapahtumia, kuten IIS-lokit ja suojauksen tapahtumien edelleen toimitetaan suoraan Log Analytics-agentti.
- Jotkin ratkaisut sisällytetään pitää agentti LISÄOHJELMISTOA tai edellyttää, että ohjelmisto on asennettu voi kerätä tietoja.  Nämä tiedot yleensä lähetetään suoraan Log Analytics.
- Jotkin ratkaisut kerää tietoja suoraan SCOM hallinta-palvelimet, joilla ei ole peräisin agentti.  Esimerkiksi [ilmoitusten hallintaratkaisu](https://technet.microsoft.com/library/mt484092.aspx) kerää ilmoitusten SCOM sen jälkeen, kun ne on luotu.

## <a name="monitoring-logic"></a>Logiikan seuranta
SCOM ja lokin analysoinnin samalla agenttien vuoksi kerättyjen tietojen käsitteleminen, mutta on siitä, miten ne määrittää ja toteuttaa niiden logiikan tietojen kerääminen ja kuinka ne analysoida tietoja, jotka he kerääminen keskeisiä eroja.

### <a name="operations-manager"></a>Operations Manager
Seurannan logiikan SCOM on toteutettu [management Pack-paketit](https://technet.microsoft.com/library/hh457558.aspx) , jotka sisältävät logiikan osat voivat valvoa, tutustu mittaaminen kuntotietojen kyseisten komponenttien ja tietojen analysointia varten.  Tietojen valvominen voi olla pelkästään kerääminen tapahtuma tai suorituskyvyn laskuri tai se voi käyttää monimutkaisia logiikan komentosarjan käytössä.  Hallinta-paketteja, jotka sisältävät valmis seuranta ovat käytettävissä [Microsoftin ja kolmansien osapuolten sovellusten](http://go.microsoft.com/fwlink/?LinkId=82105) erilaisia laitteiston ja verkon lisäksi.  Voit mukautettujen sovellusten [tekijän oman management Pack-paketit](http://aka.ms/mpauthor) .

Management Pack-paketit sisältävät useita työnkulkuja, että jokainen suorittaa joitakin eri seurantaa funktiota, kuten näyte suorituskyvyn laskuri, palvelun tilan tarkistaminen tai komentosarjan suorittaminen.  Työnkulku suoritetaan itsenäisesti ja määrittää omassa tuloksia, kuten tietokannan se kirjoittaa ja se luoda ilmoituksen. 

Voit ohittaa tiedot työnkulun, kuten raja-arvon, jossa pitävät virheen suorittaa, korkojakso ja hän luo ilmoituksen vakavuus.  Voit myös lisätä lisätoimintoja lisäämällä omia työnkulkuja.

![Ohittaa](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management Pack-paketit asennetut Operations Manager-tietokannan ja jakaa automaattisesti hallinta-palvelimet agenttien vuoksi.  Kunkin agentti Lataa management Pack-paketit automaattisesti ja ladata asianmukaiset käyttäjät ovat asentaneet sovellusten työnkulkuja.  Agentti keräämät tiedot toimitetaan hallinta palvelimeen SCOM tietokannan ja tietojen varastoon lisäämistä varten.  Toiminnot-konsolin avulla voit tarkastella ja analysoida avulla mukautettuja näkymiä, management pack sisällyttää raporttien ja raporttinäkymien.

Seuraavassa kaaviossa on kuvattu management Pack jakauman.

![Management pack-työnkulku](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Lokitiedoston Analytics
#### <a name="event-and-performance-collection"></a>Tapahtuma- ja suorituskyvyn sivustokokoelman
Lokitiedoston Analytics kerää tapahtumien ja suorituskyvyn laskureita agentti järjestelmistä lähteistä, kuten Windowsin tapahtumalokiin, IIS-lokit ja Syslog avulla.  Voit määrittää ehdot, jonka tiedot on koottu palvelun Log Analytics-portaalissa ja luo sitten Log kyselyiden kerätään tietojen analysointia varten.  Vakio ehtojen määritetään, kun luot OMS työtilan ja voit määrittää tietyn sovelluksista lisätiedot. 

![Log Analytics tapahtumalokien määrittäminen](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM ei useita yksityiskohtaiset työnkulkuja, jotka määrittävät yleensä tietojen ja toiminnon, joka suoritetaan vastauksena ehtoja, loki Analytics on yleisiä ehtoja tietojen keräämistä varten.  Log-kyselyiden ja ratkaisut on Lisää kohdennettujen ehtoja analysointiin ja sen jälkeen, kun se kerätään tietyt tiedot pilvipalveluun toimivat.

#### <a name="solutions"></a>Ratkaisut
Ratkaisujen on muita logiikan tietojen kerääminen ja analysointia varten.  Voit valita ratkaisuja OMS tilaukseen lisääminen ratkaisuvalikoimaan.

![Ratkaisuvalikoimaan](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Ratkaisujen Suorita ensisijaisesti antamisen analyysi tapahtuma-ja suorituskyvyn laskureita OMS säilöön tallennettuja pilveen.  Ne voivat määritellä myös kerätään lisätiedot, jotka voidaan analysoida Log kyselyjen tai muita käyttöliittymän myöntämä OMS koontinäytön ratkaisu. 

Esimerkiksi [muutosten jäljityksen ratkaisu](https://technet.microsoft.com/library/mt484099.aspx) tunnistaa määritysmuutoksia agentti järjestelmiin, ja kirjoittaa tapahtumia, jotka voidaan analysoida graafinen näkymiä, jotka sisältävät säilöön OMS havainnut muutoksia.  Voit siirtyä alaspäin tiivistetty näkymästä kyselyjä log kyselyt, jotka näyttävät ratkaisun keräämät yksityiskohtaiset tiedot.


Kun valitset ratkaisujen lisääminen tilaukseen, sinulla ei ole tällä hetkellä annetaan oikeudet luoda omia ratkaisuja.  Voit valita tapahtuma- ja suorituskyvyn laskureita voit kerätä ja luoda mukautettuja näkymiä log-kyselyjen perusteella.

Seuraavassa kaaviossa yhteenveto seurannan logiikan lokin analysoinnissa.

![Lokitiedoston Analytics-ratkaisun työnkulku](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Kunnon seuranta
### <a name="operations-manager"></a>Operations Manager
SCOM voidaan mallintaa sovelluksen eri osat ja anna reaaliaikainen kunto kunkin.  Näin paitsi havaita Näytä virheet ja suorituskyvyn ajan kuluessa, mutta Vahvista todellinen sovelluksen tai järjestelmän kunto ja sen osat annetun milloin tahansa.  Koska se ymmärtää aikajaksot, sovellus on käytettävissä, SCOM kunto-ohjelma tukee myös palvelussa toimeenpano (SLA) joka analysoida ja raportoinnissa käytettävyyttä sovelluksen ajan kuluessa.

Alla olevassa näkymässä näkyy esimerkiksi SQL-tietokannan ohjelmien SCOM valvoo reaaliaikainen kunto.  Kunkin tietokantoja johonkin tietokanta-ohjelmien kunto näkyy alareunassa näkymän.

![Näytä tila](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Kunto-Explorer johonkin tietokanta-ohjelmien alla, joita käytetään sen yleinen terveydentilasta näytössä.  Näiden näytön määritetään SQL management Pack ja Suorita kaikki SQL-tietokannan ohjelmien löytää SCOM vastaan.

![Kuntotietojen explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Useiden järjestelmien osia voidaan yhdistää mitata jaetun sovelluksen kunto.  Tämä voi olla hyötyä yrityssovellussarjan, joka sisältää useita osia, eri aikajaksoille.  Voit luoda mallin, joka mittaa jokaisen osan kuntoa, rollup käytettävyys sovelluksen kyselyjä.

Active Directory on esimerkki yhden hallintapaketin, joka sisältää mallin sen hajautettu osien analysointia varten.  Esimerkki seuraavassa kuvassa näkyy yleinen ympäristön ja metsien, toimialueet ja toimialueen ohjaimet suhteen kunto.  Nämä osat sisältää alikomponentit ja yllä SQL-syntaksia usean näytön avulla.

![SCOM kaavionäkymä](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Lokitiedoston Analytics
OMS ei sisällä yleisiä ohjelma mallin sovellusten tai mittaa niiden reaaliaikainen kunto.  Yksittäisten ratkaisuja arvioida kerättyjen tietojen perusteella palveluun yleinen kunto ja niitä voi asentaa mukautetun logiikan reaaliaikainen analysoinnissa agentti.  Ratkaisujen suorittaa Accessissa Pilvipalvelun OMS säilöön, koska ne on usein myyntilukujen kuin tehdään tavallisesti management Pack-paketit. 

Esimerkiksi [AD arviointi, ja SQL-ratkaisujen](https://technet.microsoft.com/library/mt484102.aspx) kerättyjen tietojen analysointia ja anna ympäristön eri ominaisuuksia luokitus.  Se sisältää suosituksia, jotka voidaan tehdä käytettävyys ja ympäristön suorituskykyä on parannettu.

![AD arviointi ratkaisu](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Tietojen analysointi
SCOM ja lokin Analytics kunkin sisältävät eri ominaisuuksia kerättyjen tietojen analysointia varten.  SCOM on näkymiä ja raporttinäkymien erilaisia muotoiluja ja raporttien esittäminen tiedot taulukkomuodossa tietovarasto viimeisimmät tietojen analysointiin toiminnot-konsolissa.  Lokitiedoston Analytics on valmis log-kyselykieltä ja käyttöliittymän OMS säilössä tietojen analysoimista varten.  Kun SCOM käytetään tietolähteenä Log Analytics-säilö sisältää keräämät SCOM, joten Log Analytics-työkalujen avulla voidaan sekä järjestelmistä tietojen analysointia varten.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Näkymät
Toiminnot-konsolissa näkymien avulla voit tarkastella eri tietotyyppien keräämiä SCOM eri muodoissa, yleensä taulukkomuotoinen tapahtumia, ilmoituksia ja tila-tiedot- ja viivakaaviot suorituskykytietoja varten.  Näkymien suorittaa mahdollisimman vähän analyysin tai koonnin tiedot, mutta avulla voit suodattaa tietyn ehtojen mukaan. 

![Näkymät](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Management Pack-paketit yleensä toimi tukemalla sovelluksen tai se valvoo järjestelmän useita näkymiä.  Tämä voi olla eri objektit, jotka management pack löytää ilmoitusten näkymät havaitut ongelmat ja suorituskyvyn näkymiä laskureita tilan näkymiä.

Näkymät ovat erityisen sopii analysoiminen nykyisen tilan mukaan lukien Avaa ilmoitukset ja kuntotietojen valvottu järjestelmien ja objekteja.  Voit siirtyä alaspäin yksityiskohtaiset tapahtuma tai suorituskykytietoja tukevat tietyn ilmoitus, jos haluat selvittää pääkansion sen syy. Voit vastaavasti tarkastella suorituskykyä ja sovelluksen arvioida sen nykyinen kunto eri osien kunto.

#### <a name="dashboards"></a>Raporttinäkymien
Raporttinäkymien toimintojen konsolissa käsitellä samat tiedot ensisijaisesti näkymät ovat Lisää mukautettavissa mutta ja voi olla tehokkaan visualisointeja.  Vakio raporttinäkymien joukko ovat käytettävissä, voit helposti mukauttaa oman tarkoituksiin.  Voit käyttää myös PowerShell-pienoissovelluksella, jotka voivat näyttää PowerShell kysely palauttaa tietoja.

![Raporttinäkymät-ikkunan](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Kehittäjät voivat mukautettujen osien lisääminen niiden management Pack sisällyttää ne raporttinäkymiä.  Nämä saattaa olla erittäin erilaisia tietyn sovellukseen, kuten alla olevassa SQL management Pack Raporttinäkymät-ikkunan.  Tämä koontinäyttö voi käyttää myös mallina mukautetun versiot.

![SQL-koontinäyttö](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Raportit
Raporttien SCOM analysoida tietoja tietovarasto taulukkomuodossa.  Ne voidaan tulostaa ja suunniteltu automaattinen toimitus eri tiedostomuodossa, kuten PDF, CSV ja Word.  Raporttien käsittelemään tietovarasto tiedot, jotta erityisesti soveltuvat pitkällä aikavälillä trendien analysointia varten.

Management Pack-paketit tarjoavat mukautettuja raportteja yleensä tietyn-sovelluksen.  Voit myös valita valikoiman yleinen raportteja, jotka voit mukauttaa oman sovellusten tai tekemistä analysoimista varten.

Seuraavassa on Active Directory Management Pack keräämät tiedot näkyvät esimerkkiraportti suorituskykyä.

![Raportti](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Lokitiedoston Analytics
Lokitiedoston Analytics on [kyselykieltä](https://technet.microsoft.com/library/mt484120.aspx) , joiden avulla voit analysoida tietoja useiden sovelluksista ilman, että voit luoda mukautetun näkymän tai raportin kaikissa.  Koska OMS on toteutettu pilveen, kyselyjen ja tietojen analysointi suorituskyvyn eivät ole laitteiston rajoitettu käyttöoikeus ja kyselyt, mukaan lukien miljoonia tietueet nopeasti voit analysoida. 

Log Analytics kyselyissä on myös muut toiminnot peruste.  Voit tallentaa kyselyn, sen tulokset viedään Exceliin tai sen automaattisesti säännöllisin väliajoin ja luoda ilmoituksen, jos sen tuloksia tietyn ehtoja vastaavat.  

![Lokitiedoston kyselyn työnkulku](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Alla on esimerkki Log Analytics-kysely.  Tässä esimerkissä kaikki tapahtumat, joiden "aloittaminen" nimi on palautettu perusteella ja ryhmitelty tapahtuman tunnus.  Käyttäjän yksinkertaisesti tarjoaa kysely ja Log Analytics muodostaa dynaamisesti käyttöliittymän analyysin suorittamiseen.  Valitsemalla kohteen luettelosta palauttaa yksityiskohtaiset tapahtumatietoja.

![Log-kysely](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Lisäksi antamisen analysoimista Log Analytics kyselyissä voidaan tallentaa myöhempää käyttöä varten ja lisätään myös [OMS Raporttinäkymät-ikkunan](http://technet.microsoft.com/library/mt484090.aspx) seuraavan esimerkin mukaisesti.

![OMS Raporttinäkymät-ikkunan](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Ota käyttöön [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
- Tilaa [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics).  
