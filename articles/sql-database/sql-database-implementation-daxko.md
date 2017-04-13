<properties
   pageTitle="Azure SQL-tietokannan Azure Esimerkkitapaus - Daxko/CSI | Microsoft Azure"
   description="Tietoja siitä, miten Daxko/CSI käyttää SQL-tietokantaan nopeuttamiseksi sen kehittämisen aikana ja asiakkaan palvelujen ja suorituskyvyn parantamiseksi"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/08/2016"
   ms.author="carlrab"/>
   
# <a name="daxkocsi-used-azure-to-accelerate-its-development-cycle-and-to-enhance-its-customer-services-and-performance"></a>Daxko/CSI käytetään Azure nopeuttaa sen kehittämisen aikana ja asiakkaan palvelujen ja suorituskyvyn parantamiseksi

![Daxko/CSI-Logo](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI ohjelmiston järjestelmässä hankala: kunnon ja vapaa-keskukset asiakkaan sen kantaluku on kasvava ansiosta sen täydellinen enterprise-ohjelmisto-ratkaisun onnistuu nopeasti, mutta on ajan tasalla IT-infrastruktuurin kasvava asiakkaan tarpeiden perus testaaminen yrityksen IT-henkilöstön. Yrityksen on yhä rajoitettu mukaan ylä toimintojen katseltavan erityisen sen kasvava tietokantojen hallintaa varten. Huonoin-toimintoja, katseltavan on leikkaaminen kyselyjä kehittäminen resurssit uusi hankkeita, kuten yrityksen-ohjelmiston uudet mobility ominaisuudet.

Azure mukana CSI ohjelmiston mukaan David Molina johtaja, tuotekehitys, Daxko/CSI ympäristö nimellä--palvelun (PaaS)-mallin tarvittavat yksinkertaistaa tietokannan hallinta, Suurenna skaalattavuus ja vapauttaa resursseja keskittyminen ohjelmiston ops sijaan. "Azure SQL-tietokanta on hyvä vaihtoehto, us. Ei tarvitse huolehtia ylläpito SQL Serverin automaattisesti klusterin ja kaikki muut infrastruktuuri tarvitsee ei us erinomaisesti."

Koska siirtämisestä Azure CSI ohjelmisto on toiminnot-henkilökunnan kahden, yli 600 asiakkaan tietokantojen hallinta. Yritys käyttää Azure SQL-tietokanta joustavasti jakavat siirrettävä asiakkaan tietokantojen koon perusteella ja on.

Jatkuu Molina "asiakkaidemme huopa muutokset heti. Ennen joustavasti jakavat toisinaan ovat samat aikakatkaisu ja muiden ongelmien purskeen aikoina. Azure joustavasti jakavat, jossa ne burst tarpeen mukaan ja käyttää ohjelmistoa ongelmaton."

Asiakkaiden suorituskyvyn parannusten Azure Lisää joustavasti tietokannan jakavat vapauttanut CSI ohjelmiston resurssien keskittyminen kehittä uusia palveluja ja ominaisuuksista, sen sijaan, että käsittely toiminnot ja hallinta. Näiden IT-resurssit auttoi CSI ohjelmiston yrityksen ohjelmaa tarjoaa SpectrumNG, jotta osallistuminen gym jäsenet, henkilöstön tehokkuuden parantamiseksi ja antaa henkilökunnan ja jäseniä mobiilikäytön vuorovaikutteinen tehtävien ja reaaliaikaiset ilmoitukset.

Azure on myös auttoi CSI ohjelmiston nopeuttamiseksi ja parantaa kehittäminen ja laatu-assurance (kysymysten ja vastausten) jakson ottamalla Automaattiset asetukset. Yrityksen Azure toteutukseen muodosta johtajat voit pakata osat napsauttamalla hiiren napsautuksella. Kun kuvaa Molina "osana julkaisuprosessin kysymysten ja vastausten on nyt voit ottaa käyttöön testiympäristössä Azure-tietokannassa, johon muistuttava tiiviisti Microsoftin tuotannon pino. Olemme välitön käyttöönotto versiot sekä keskihajonta-ympäristöön vastaanottavat muutokset. Tämä johtuu suuri win varten meille, Microsoft ei ole välistä eroa testikäyttöön ennen,."

## <a name="offloading-to-the-cloud"></a>Purkaminen pilveen

Ennen siirtymistä pilveen CSI ohjelmiston oli onnistuneesti rakennettu omassa multitenant infrastruktuuri-Houston paikallisen joten. Yrityksen laajennettuina järjestelmässä kasvava kasvava pains ostaminen, valmistelu ja ylläpito kaikki laitteiston ja asiakkaiden tukemaan ohjelmistot. IT henkilökunta käsittelemään toimintojen tuli toiseen pullonkaula, johtanut heikentyneen valmistelu uudet resurssit ja toteuttaa uusia palveluita asiakkaille.

CSI ohjelmiston etsitään kyselyjä poistamisesta kyseisen katseltavan cloud asetukset niin, että sen koodin sijaan toimintojaan voi keskittyä. Yrityksen havaitsi, että monia yläreunan cloud-palveluntarjoajat tarjoavat vain infrastruktuuri-muodossa--palvelun (IaaS)-ratkaisuja, jotka edellyttävät edelleen suuri IT-henkilöstön IaaS pino hallinta. Lopuksi CSI ohjelmiston määritetään Azure PaaS ratkaisu on varattu sopivimmat sen tarpeisiin. Molina kerrotaan "Azure saa laitteisto ja järjestelmän ohjelmisto pois näkyvistä, jolloin on keskittyä Microsoftin ohjelmiston palveluja, kun pienentämisestä Microsoftin IT-katseltavan."

## <a name="making-the-transition-to-azure"></a>Jolloin siirtyminen Azure

Kun olet valinnut Azure sen PaaS-ratkaisun, CSI ohjelmiston alkamisen sen Taustajärjestelmä infrastruktuuri ja tietokantojen siirtämisestä pilveen. Azure-ennen SpectrumNG asiakkaiden tarvittavat asiakassovellus, joka ilmoittaa asentaminen Windows Communication Foundation (WCF)-palvelussa, valitse uudelleen. Mukaan Molina "vaikka asiakkaita isännöidään kaikkea omia palvelinkeskusten on suunniteltu tuote on multitenant. Olemme isännöidään kaikki-Houston, joten käyttäen SQL Server tietovaraston.

"Ojentamassa myös tuotteen mukana jäsenen osoittava web-portaali (kirjoitettu ASP.net), joka on suunniteltu vastaamaan asiakkaan web tavoitettavuus- ja SOAP-Ohjelmointirajapinnan tukemaan online-sivut ja kolmannen osapuolen integraatio valkoinen, jossa."

Siirron pilveen eivät tule pitkä käyttöympäristön. Mukaan Molina, "Useimpia käsitellyt muokkaaminen, lue config tietoja, keskitetty yhteysmerkkijono-muokkausta ja automatisointi pakkaus, lataamisesta ja Microsoftin versioiden käyttöönoton työmäärään."

Kehittää muodosta automaatio CSI ohjelmiston insinöörien käyttää PowerShellin Azure ja REST API luomaan pakettien ja lataaminen väliaikaisen käyttöympäristön release kunkin yöllä.
Yleinen siirtyminen Azure pilvipohjainen käyttöönoton tapahtui nopeasti ja sujuvasti CSI ohjelmiston IT-ryhmän. Molina kerrotaan "kaikki, emme oli beeta-ympäristön pilveen kolme tai neljä viikon ottaen projektin kuluessa. Joka oli surprising win meille,."

Kun määrittäminen ja testaus ympäristön, CSI ohjelmiston alkamisen siirtyminen asiakkaille. Käytätkö jo CSI ohjelmiston isännöinnin asiakkaiden siirtymän oli lähes saumattomasti. Siirtyminen: paikallisen ympäristön asiakkaiden pilveen siirron muita ajan, mutta on edelleen ensisijaisesti kipua vapaa-asiakkaille sekä CSI ohjelmiston.

Uusi-asiakkaiden CSI ohjelmisto on IT-henkilöstön junassa seuraavat prosessin niiden avulla Azure:

1.  Azure PowerShell-komentosarjojen käytetään asettamasi uuden tietokannan asiakkaan; kaikkien asiakkaiden aloittaa jotta tarpeeksi alkuperäinen siirtonopeuden siirtymän premium taso.
2.  Jos mahdollista, CSI ohjelmisto käyttää ohjattua Azure SQL-siirron olemassa olevia tietoja Siirry Azure SQL-tietokanta-esiintymä.
3.  Microsoft SQL Server Integration Services (SSIS) käytetään lopuksi täsmäyttää tietojen ristiriitoja tai suorittaa minkä tahansa tietojen uudelleenjärjestäminen halutulla tavalla.

Nykyään noin 99 prosenttia CSI ohjelmiston asiakkaiden isännöidään Azure-tietokannassa, neljä alueellisen palvelinkeskusten (pohjoinen keskitetyn, Etelä Keski-Itä- ja Länsi) kautta. Pyytämällä palvelinkeskusten kunkin asiakkaan maantieteellisen alueen viive on käytettävissä vähintään.

## <a name="azure-elastic-database-pools-free-up-it-resources"></a>Azure joustavasti tietokannan jakavat vapauttaminen IT-resurssit

Azure useita ominaisuuksia on voitu CSI ohjelmiston VAIHTO seuraavilta infrastruktuuri ja toiminnot, jotka koskivat siitä ominaisuus ja koskivat kehittäminen. Ehkäpä suurimmista etu on ollut joustavasti tietokannan jakavat.

CSI ohjelmisto on tällä hetkellä noin 550 tietokantojen asiakkaille. Ennen joustavasti jakavat on vaikea taso rakenteessa monta tietokantojen hallinta. OPS valvojat oli määrittää suorituskyvyn tasoa asiakkaiden, jotka tarvitaan merkittäviä IT-resurssi yleisrasite purskeen tarpeiden mukaan. Joustavasti tietokannan jakavat kanssa valvojat voit määrittää alihallinnat premium tai standard resurssivarantoon tarvittaessa ja Siirrä koon perusteella asiakkaat ja on. Asiakkaiden huopa joustavasti tietokannan jakavat tehosteita miltei heti. ennen joustavasti jakavat asiakkaiden oli aikakatkaisu ja muiden ongelmien purskeen käytön aikana, mutta kanssa joustavasti jakavat asiakkaat voivat kohdata tehtävän bursts tarpeen mukaan ja he voivat edelleen käyttää SpectrumNG ongelmaton.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Azure Active Geo-replikoinnin nopeuttaa raportointi

Useita CSI ohjelmiston asiakkaita myös kestää advantage ja Azure Active Geo-replikoinnin. Aktiivinen Geo-toistot, enintään neljä luettavissa toissijainen tietokantojen voi määrittää samassa tai eri palvelinkeskuksen alueilla. CSI ohjelmisto on aktiivinen Geo-replikointi käyttää kahdella tavalla: ensin toissijainen tietokantoja on käytettävissä palvelinkeskuksen käyttökatkosta tai ei voi muodostaa ensisijainen tietokantaan. ja toinen toissijainen tietokannat ovat luettavissa ja voidaan käyttää vain luku-toiminnoista, kuten raportoinnin työt purku. CSI ohjelmiston asiakkaita tämän edun avulla voit nopeuttaa raportoinnin työnkulut.

## <a name="csi-software-application-logic-and-architecture"></a>CSI ohjelmiston sovelluksen logiikkaa ja arkkitehtuuri

SpectrumNG käyttää web roolit. Sovellus on usean vuokraajan, koska WCF-palvelua käytetään yhteyden pyynnön asiakkaiden käsittelemiseen. Kun Molina kertoo, "pyynnön tunnistaa kunkin asiakkaan, jonka avulla us luominen yhteysmerkkijonon, voit tehdä riippumatta siitä, mitä voit tehdä annettava tietokantansa."

Palvelussa web-tason CSI ohjelmiston hyödyntää Azure Automaattinen skaalaus-perusteella ja -aika. Käytettävissä olevat resurssit ovat kasvattaa automaattisesti sopimaan suurempi käyttö työaikana, kunkin alueellisen palvelinkeskuksen aikavyöhykkeen mukaan. Resurssien määritetään myös skaalata viikonloppuisin, kun asiakkaan tarpeiden ovat pienempiä.

     
![Daxko/CSI-arkkitehtuuri](./media/sql-database-implementation-daxko/figure1.png)

Kuva 1. Cloud services-Työntekijä-roolin piirtää jäsenneltyjen tietojen Azure SQL-tietokanta ja puolirakenteisia tietoja taulukkotallennus. SpectrumNG käyttäjät käsitellä tietoja pilvestä kautta services web rooli.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Käyttämällä web Apps-sovellusten ja web-suunnitelma taso for mobile-sovellukset

Mukautetun API, ylläpidettävä Azure web Apps-sovellusten käyttöön uuden hankkeita, kuten valmis mobiilisovellusten ympäristön Azure SQL-tietokanta vapauttanut CSI ohjelmistojen resurssien avulla perusteella. Ympäristön avulla gym jäsenet ja henkilökunnan mobiililaitteiden avulla voit tarkistaa aikatauluja, kirjan luokat ja vastaanottaa viestejä.

Ympäristön käyttää palvelun aloittaminen arkkitehtuuri (SOA) suorittavan yhteen osaan – myyntipistejärjestelmän (POS) tai myynnin järjestelmän, kuten – siirtää sen toiseen web-suunnitelma suoraan selaimessa ja sitten asettamasi palvelu tukemaan kyseisen osan jättäen monipuolisista alkuperäisen web osalta. Tämä sallii CSI ohjelmiston sisältää joustavasti ja se auttaa pitämään kustannukset.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure avulla CSI ohjelmiston kehittäjät kohdistus sovelluksia ja palveluja

Azure SQL-tietokanta ei ole SpectrumNG asiakkaille, jotka Nauti nopea ja luotettava-palvelun mahdotonta, se on myös suuri win CSI-ohjelmiston IT-henkilöstöön ja kehittäjille. CSI ohjelmiston purkaminen ops Azure pilvipalvelussa, vähentää resurssien ja infrastruktuurin sen katseltavan, vähentää huomattavasti nopeutettu micromanage tietokantojen ja suorituskyvyn parantaminen sen omistajien sen kehittäminen jaksot ja enää tarpeisiin.

## <a name="more-information"></a>Lisätietoja

- Lisätietoja Azure joustavasti tietokannan jakavat on artikkelissa [joustavasti tietokannan jakavat](sql-database-elastic-pool.md).

- Lisätietoja Tietokantatyökalut ja joustavasti Skaalaus-kohdassa [joustavasti Tietokantatyökalut ja joustavasti skaalaus](sql-database-elastic-scale-get-started.md).

- Katso lisätietoja SQL Server-tietokantaan siirtyminen-kohdassa [Ohjattu Azure SQL-siirron](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md).

- Tietoja Active Geo-replikoinnin, artikkelissa [Aktiivinen Geo-replikoinnin](sql-database-geo-replication-overview.md).

- Lisätietoja verkko-roolit ja työntekijä roolit on artikkelissa [Työntekijä roolit](../fundamentals-introduction-to-azure.md#compute). 

- Lisätietoja Azure-palvelu Bus on artikkelissa [Azure-palvelu Bus](https://azure.microsoft.com/services/service-bus/).

- Lisätietoja Automaattinen skaalaus-kohdassa [Skaalaus pilvipalveluihin](../cloud-services/cloud-services-how-to-scale.md).
