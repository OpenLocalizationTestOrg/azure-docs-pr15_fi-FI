<properties
   pageTitle="Azure-sovellusten palauttaminen | Microsoft Azure"
   description="Teknisiä tietoja ja tarkempia tietoja palauttaminen Microsoft Azure-sovellusten suunnitteleminen."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="disaster-recovery-for-applications-built-on-microsoft-azure"></a>Microsoft Azure-sovellusten palauttaminen

Suuren käytettävyyden olisi tilapäisen virheen hallinnasta palauttaminen (DR) on kriittinen menettämisen uhkasta sovelluksen toimintoja. Esimerkkinä tilanne, jossa alueen siirtyy alaspäin. Tässä tapauksessa sinun täytyy olla suunnitelma suorittamaan sovelluksen tai käyttää tietoja Azure alueen ulkopuolella. Suunnitelman suorittamisen liittyy henkilöt, prosessit ja Salli funktion järjestelmän tukevat sovellukset. Liiketoiminnan ja tekniikka omistajat, joka määrittää järjestelmän toiminnan tila huono myös määrittää toimintojen palvelun huono aikana. Toimintojen voi kestää muutaman lomakkeiden: käytöstä kokonaan, osittain käytettävissä (heikentynyt toimintoja tai lykätä käsittely) tai täysin käytettävissä.

##<a name="azure-disaster-recovery-features"></a>Azure tietojen palautus-toiminnot

Käytettävyys huomioon otettavia seikkoja kanssa Azure on [vikasietoisuudelle tekniset ohjeet](./resiliency-technical-guidance.md) , joka on suunniteltu tukemaan palauttaminen. Välillä joitakin Azure ja tietojen palauttaminen käytettävyys-ominaisuuksia on myös yhteyden. Esimerkiksi vika toimialueilla roolien hallinta kasvaa sovelluksen käytettävyyttä. Käsittelemättömän laitteistovirhe tulee "tietojen" tilanne ilman, että hallinta. Jotta käytettävyys ominaisuuksia ja strategioita oikea sovellus on tärkeä osa tietojen tarkistustyökalut sovelluksen. Kuitenkin tämän artikkelin ulottuvat Yleiset käytettävyysongelmia Lisää vakavia (ja harvoissa) tietojen tapahtumia.

##<a name="multiple-datacenter-regions"></a>Useita palvelinkeskuksen alueita

Azure ylläpitää palvelinkeskusten monta alueilla eri puolilla maailmaa. Tämä infrastruktuuri tukee useita tietojen palauttaminen skenaarioita, kuten järjestelmän mukana geo-replikointi Azure-tallennustilan toissijainen alueille. Se tarkoittaa sitä, että voit helposti ja edullisesti ottaa käyttöön pilvipalveluun useisiin sijainteihin eri puolilla maailmaa. Vertaa tätä kustannus- ja/tai vaikeuksiin suorittaa oman palvelinkeskusten alueille. Tietojen ja palvelujen käyttöönotto alueille auttaa suojaamaan sovelluksen pää katkokset yhden alueen.

##<a name="azure-traffic-manager"></a>Azure liikenteen hallinta

Kun aluekohtainen virhe ilmenee, sinun on luotava uudelleenohjaus liikenne Services-palveluiden vai ominaisuuksissa toisen alueen. Tee Reititys manuaalisesti, mutta se on tehokkaampaa automatisoidun prosessin avulla. Azure liikenteen hallinta on suunniteltu tämän tehtävän. Sen avulla voit hallita käyttäjien tietoliikenne toisen alueen vikasietotilaa automaattisesti siltä varalta, että ensisijainen alueen epäonnistuu. Koska liikenteen hallinta on tärkeä osa kokonaisvaltainen strategia, on tärkeää ymmärtää perusteet liikenteen hallinta.

Seuraavassa kaaviossa käyttäjät muodostavat yhteyden URL-Osoitetta, joka on määritetty liikenteen hallinta (__http://myATMURL.trafficmanager.net__) ja että tiivistelmiä todellisen sivuston URL-osoitteet (__http://app1URL.cloudapp.net__ ja __http://app2URL.cloudapp.net__). Perusteella siitä, miten voit määrittää ehdot ohjaavat käyttäjät, käyttäjien lähetetään oikein todellista sivuston Kun käytäntö määrää. Käytännön asetukset ovat pyöreän pöydän, suorituskykyä tai automaattisesti. Tässä artikkelissa vuoksi on on kyseisen automaattisesti-vaihtoehto.

![Reititys kautta Azure liikenteen hallinta](./media/resiliency-disaster-recovery-azure-applications/routing-using-azure-traffic-manager.png)

Kun olet määrittänyt liikenteen hallinta, anna uusi liikenteen hallinta DNS-etuliite. Tämä on URL-etuliitteen, sinun annettava käyttäjille muodostaaksesi yhteyden palveluun. Liikenteen hallinta käsittelee nyt kuormituksen yhden tason verran ylös- ja ei alueen tasolla. Liikenteen hallinta DNS yhdistää kaikki käyttöönotoissa, joka hallitsee CNAME.

Sisällä liikenteen hallinta-Määritä prioriteetti käyttöönotoissa, jotka käyttäjät reititetään, kun ongelma ilmenee. Liikenteen hallinta valvoo päätepisteet ominaisuuksissa ja muistiinpanoja, kun ensisijainen käyttöönotto epäonnistuu. Virhe-liikenteen hallinta analysoi ominaisuuksissa prioriteettiluetteloon ja ohjaavat käyttäjät seuraavan luettelossa.

Vaikka liikenteen hallinta päättää, miten jatkaa automaattisesti, voit päättää, onko automaattisesti toimialueen passiiviset vai active, kun et ole automaattisesti-tilassa. Vastaavia toimintoja ei tehdä Azure liikenteen hallinta. Liikenteen hallinta havaitsee virheen ensisijainen sivuston ja palauttaa automaattisesti-sivustoon. Liikenteen hallinta palauttaa riippumatta siitä, onko sivusto tällä hetkellä toimiva käyttäjien vai ei.

Lisätietoja Azure liikenteen hallinta toiminnasta on lisätietoja:

 * [Liikenne hallinnassa: yleiskatsaus](../traffic-manager/traffic-manager-overview.md)
 * [Määritä automaattisesti reititys menetelmä](../traffic-manager/traffic-manager-configure-failover-routing-method.md)

##<a name="azure-disaster-scenarios"></a>Azure tietojen skenaariot

Seuraavissa osissa kattaa useita erityyppisiä tietojen skenaarioita. Alueen laajuinen palveluiden keskeytyksiä eivät ole sovelluksen yleisten virheiden vain syy. Heikko suunnittelu ja hallinta virheitä voi myös johtaa katkokset. On tärkeä ottaa huomioon virheen mahdollisia syitä rakenne ja palautus-palvelupaketin testauksen vaiheiden aikana. Hyvä suunnitelman hyödyntää Azure ominaisuuksia ja augments ne sovelluksen strategiat kanssa. Valitun vastauksen määräytyy sovelluksen tärkeyden mukaan palautus-kohdan tavoite (RPO) ja palautus aika tavoitteen (RTO).

###<a name="application-failure"></a>Sovelluksen virhe

Azure liikenteen hallinta käsittelee automaattisesti virheet, jotka aiheutuvat host virtuaalikoneen pohjana laitteistosta ja käyttöjärjestelmästä ohjelmisto. Azure Luo uuden esiintymän roolin toimii palvelimessa ja lisää se kuormituksen kiertoa. Jos roolin kerrat on enemmän kuin yksi, Azure siirtää käsittely muiden käynnissä roolin esiintymien korvaaminen epäonnistui solmu aikana.

On vakavia sovellusvirheet, jotka käynnistyvät ulkopuolisista laitteiston tai käyttöjärjestelmä ilmenevät virheet. Sovellus voi epäonnistua kriittinen poikkeukset virheelliset logiikan tai tietojen eheyteen liittyvien ongelmien vuoksi. Tarpeeksi telemetriatietojen on liittää koodin niin, että valvonta järjestelmän voit havaita virheen ja ilmoita sovelluksen järjestelmänvalvoja. Tuntee täysin tietojen palauttaminen prosesseista ottava järjestelmänvalvoja, voit tehdä päätös käynnistää automaattisesti prosessi. Voit myös järjestelmänvalvoja yksinkertaisesti hyväksyä käytettävyys-käyttökatkosta kriittisten virheiden korjaamiseksi.

###<a name="data-corruption"></a>Tietovirheitä

Azure automaattisesti tallentaa Azure SQL-tietokanta ja Azure-tallennustilan kolme kertaa toisena eri vika toimialueiden saman alueen sisällä. Jos käytät geo replikoinnin, tiedot tallennetaan muita kolmesti toisella alueella. Kuitenkin käyttäjille tai sovelluksen vioittaa ensisijainen kopioi tiedot, jos tiedot nopeasti kopioi muut kappaleet. Valitettavasti tuloksena on vioittunut tietojen kolme kopiota.

Hallittavan tietojen mahdolliset vioittumisen on kaksi vaihtoehtoa. Voit hallita ensin mukautettu Varmuuskopioinnin toimintamallin. Voit tallentaa varmuuskopiot Azure tai paikalliseen, business vaatimukset tai hallinnon asetusten mukaan. Toinen vaihtoehto on käytettävä SQL-tietokannan palauttaminen uusi ajankohta Palauta-vaihtoehto. Lisätietoja on kohdassa [tietojen palauttaminen strategioita](#data-strategies-for-disaster-recovery)käyttöön.

###<a name="network-outage"></a>Verkon käyttökatkosta

Azure verkko-osat eivät ole käytettävissä, et voi olla pääsevät sovelluksen tai tiedot. Jos vähintään yksi rooli esiintymät eivät ole käytettävissä verkko-ongelmien vuoksi, Azure käyttää sovelluksen jäljellä olevan käytettävissä olevat esiintymät. Jos sovellusta voi käyttää tietonsa Azure verkko-käyttökatkosta vuoksi, voit mahdollisesti suorittaa eivät toimi oikein tilassa paikallisesti käyttämällä välimuistiin tallennetut tiedot. Sinun täytyy arkkitehti tietojen palauttaminen strategia käynnissä heikentää tilassa-sovelluksessa. Joidenkin sovellusten tämä ehkä käytännön.

Toinen vaihtoehto on tiedot tallennetaan sijaintipaikkaan, kunnes yhteys on palautettu. Eivät toimi oikein tila ei ole vaihtoehto, jos muut asetukset ovat sovelluksen käyttökatkot tai vaihtoehtoinen alueen automaattisesti. Sovelluksen käynnissä heikentää tilassa rakenne on mahdollisimman paljon business päätös tekniset yksi nimellä. Sitä käsitellään edelleen [heikentynyt sovelluksen toiminnot](#degraded-application-functionality)-osiossa.

###<a name="failure-of-a-dependent-service"></a>Riippuvainen virhe

Azure on monta palvelut, joita voit kohdata säännöllisiä käyttökatkot. Harkitse [Azure Redis välimuistin](https://azure.microsoft.com/services/cache/) esimerkkinä. Usean vuokraajan-palvelu sisältää välimuistiin-sovellukseen. On tärkeä ottaa huomioon, mitä tapahtuu sovelluksesi, jos riippuvainen palvelu ei ole käytettävissä. Monella tavalla Tämä skenaario muistuttaa verkon käyttökatkosta skenaariota. Lähitulevaisuudessa kuitenkin kunkin palvelun johtaa itsenäisesti mahdollisia parannuksia yleinen suunnitelma.

Azure Redis välimuistin on välimuistiin sovelluksen sisällä cloud palvelun käyttöönoton, jossa on tietojen palauttaminen etuja. Palvelun suoritetaan ensin nyt roolit, jotka ovat paikallisia käyttöönoton. Tämän vuoksi on paremmin voivat valvoa ja hallita välimuistin tilaa cloud-palveluun yleisen hallinnan prosesseja osana. Tämän tyyppistä välimuistiin paljastaa myös uusia ominaisuuksia. Yksi uusista ominaisuuksista on suuri käytettävyys välimuistiin tallennetut tiedot. Näin voit säilyttää välimuistiin tallennetut tiedot, jos yksi solmu epäonnistuu yllä kopioiden muissa solmuissa.

Huomaa, että suuren käytettävyyden siirtonopeuden pienentää ja suurentaa viive vuoksi toissijainen kopioi päivittäminen-kirjoituksia. Se myös kaksinkertaistuu muistin, jota käytetään kunkin kohteen, suunnitteleminen niin. Tietyn tässä esimerkissä näytetään, kunkin riippuvaiset palvelun voi olla ominaisuuksia, jotka parantavat yleinen käytettävyys ja kriittinen epäonnistuu, ja tiedostojen.

Kunkin riippuvaiset palvelussa pitäisi ymmärtää keskeytetty vaikutukset. Välimuistiin esimerkissä ehkä voi käyttää tietoja suoraan tietokannan palauttaminen välimuistin. Tämä olisi eivät toimi oikein tilan suorituskykyä, mutta antaa täydellisesti tietojen osalta.

###<a name="region-wide-service-disruption"></a>Alueen laajuinen-palvelu

Edellisessä toistunut ensisijaisesti on virheitä, joita voidaan hallita samalla Azure alueella. Sinun täytyy laatia myös, että on keskeytetty koko alueen mahdollisuudesta. Jos alueen laajuinen keskeytetty, tietojen paikallisesti tarpeettomat kopioita eivät ole käytettävissä. Jos geo replikoinnin on otettu käyttöön, sinun on kolme kopioita BLOB-objektit ja taulukot toisella alueella. Jos Microsoft ilmoittaa hävitty alueen Azure määrittää tehdyt uudelleen kaikki DNS-arvojen geo replikoida-alueelle.

>[AZURE.NOTE] Huomioon, että sinulla ei ole mitään päättää, tämä toimenpide ja se tapahtuu vain alueen laajuinen palvelu varten. Tästä syystä riippuvaisia muiden sovelluksen kielikohtaiset varmuuskopion strategioita käytettävyys ylimmän tason saavuttamiseksi. Lisätietoja on kohdassa [tietojen palauttaminen strategioita](#data-strategies-for-disaster-recovery)käyttöön.

###<a name="azure-wide-service-disruption"></a>Azure laajuisen-palvelu

Tietojen suunnittelussa, ota huomioon mahdollista lieventämiseksi koko alue. Yksi eniten vakavia palveluiden keskeytyksiä aiheuttaa kaikkien Azure alueiden samanaikaisesti. Samalla tavalla kuin muiden palveluiden keskeytyksiä, voit päättää tilapäinen käyttökatkot riskiä tehdä tässä tapauksessa. Juuri palveluiden keskeytyksiä, jotka ulottuvat alueiden on oltava paljon harvoissa kuin erillään palveluiden keskeytyksiä, joihin sisältyy riippuvaisia palveluita tai yksittäisen alueet.

Kuitenkin joitakin kriittisten sovellusten voit päättää, että tämä skenaario varmuuskopion suunnitteleminen on oltava. Tapahtuman suunnitelma voivat kuulua muun muassa kirjautumatta [vaihtoehtoisia cloud](#alternative-cloud) tai [hybrid paikallisen ja ratkaisu cloud](#hybrid-on-premises-and-cloud-solution)Services-palvelujen päälle.

###<a name="degraded-application-functionality"></a>Eivät toimi oikein sovelluksen ominaisuudet

Tyylikkäitä sovellus käyttää yleensä sivustokokoelman moduuleista, jotka ovat yhteydessä toisiinsa vaikka löyhästi kytkettyjä tiedot-interchange kuviot soveltaminen. DR soveltuvia-sovellus edellyttää erottaminen tehtävien moduulin tasolla. Tämä on riippuvainen häiriöt estää joka tuo koko sovelluksen alaspäin. Harkitse commerce verkkosovelluksen esimerkiksi yrityksen Y. Seuraavat moduulit saattaa olla sovelluksen:

 * __Tuoteluettelon__ avulla käyttäjät voivat selata tuotteet.
 * __Ostoskori__ avulla käyttäjät voivat lisätä tai poista niiden ostoskori-tuotteita.
 * __Tilauksen tila__ näkyy käyttäjän tilaukset toimituksen tila.
 * __Tilauksen lähettämisen__ Viimeistelee tietokantatietojen ostoskoriin istunnon lähettämällä järjestyksessä, jossa maksu.
 * __Tilauksen käsitteleminen__ tarkistaa tilauksen tietojen eheyden ja suorittaa määrä käytettävyys-valintaruutu.

Kun tämän sovelluksen moduulin riippuvainen menee, miten moduulin toimi, ennen kuin sen osan palauttaa? Hyvin architected järjestelmän toteuttaa eristystaso rajat erottaminen tehtävät sekä suunnitteluaikaisen suorituksen kautta. Voit luokitella jokaisen virheen palautettavissa ja käsittelylle. Ei palautettavissa virheitä vievät moduulin alaspäin, mutta voit vähentää ratkaistavissa oleva virhe, vaihtoehtojen avulla. Kuten edellä suuren käytettävyyden-osassa, voit piilottaa ongelmia käyttäjiltä käsittely virheitä ja ottamalla vaihtoehtoinen toiminnot. Lisää vakavia keskeytetty aikana sovelluksen ehkä ole täysin käytettävissä. Kolmas vaihtoehto on kuitenkin jatkaa ylläpidon käyttäjät eivät toimi oikein-tilassa.

Esimerkiksi jos tietokannan isännöinnin tilaukset menee, Tilauksen käsitteleminen-moduulin menettää käsitellä myyntitapahtumien mahdollisuus. Sen mukaan, arkkitehtuurista voi olla vaikeaa tai mahdotonta sovelluksen jatkaa lähettämistä ja Tilauksen käsitteleminen-osia. Jos sovellusta ei ole suunniteltu tätä skenaariota, koko sovellus voi siirtyä offline-tilaan.

Saman tässä skenaariossa on kuitenkin mahdollista, että tuotteen tiedot on tallennettu eri sijaintiin. Tässä tapauksessa tuoteluettelon moduulin edelleen voidaan käyttää tuotteiden tarkasteluun. Eivät toimi oikein tilassa sovelluksen säilyy käytettävissä toimintoja, kuten tarkasteleminen tuoteluettelon käyttäjien käytettävissä. Sovelluksen muihin osiin on kuitenkin ei ole käytettävissä, kuten järjestys tai varaston kyselyt.

Toinen muunnelma eivät toimi oikein moodin keskittää suorituskyvyn ominaisuuksia sijaan. Esimerkkinä tilanne, jossa tuoteluettelon tallennetaan välimuistiin Azure Redis välimuistin kautta. Jos välimuisti ei ole käytettävissä, sovellus voi siirtyä suoraan palvelimen tallennustilassa tuotteen luettelotietojen hakemiseen. Mutta tämä access saattaa kestää kauemmin kuin välimuistiin tallennettu versio. Tästä syystä sovelluksen suorituskyky on heikentynyt, kunnes välimuistiin palvelu on palautettu kokonaan.

Päätät, kuinka paljon sovelluksen säilyvät eivät toimi oikein tilassa-funktio on business päätös ja teknisten päätösten. Sovellus on myös päättää, miten väliaikaisia ongelmia käyttäjille. Tässä esimerkissä sovellus saattaa antaa tarkasteleminen tuotteet ja lisäämällä ne myös ostoskärryyn. Kun käyttäjä yrittää hankintaan, sovellus ilmoittaa käyttäjälle että myynnin moduuli ei ole tilapäisesti käytettävissä. Ei erinomaisesti asiakas, mutta se ei estä sovelluksen laajuinen keskeytetty.

##<a name="data-strategies-for-disaster-recovery"></a>Strategioita tietojen palauttaminen

Tietojen käsitteleminen oikein on oikea vaikein alueen tietojen palauttaminen palvelupaketti, valitse. Tietojen palauttaminen on myös palautusprosessi, joka on yleensä osa eniten aikaa. Eri vaihtoehtoja heikkeneminen tilat johtaa vaikeaa haasteisiin, virhe ja yhdenmukaisuuden tietojen palauttaminen virheen jälkeen.

Jokin tekijöiden ei palauta tai säilyttää kopion sovelluksen tietoja ei tarvitse. Näitä tietoja käytetään viittaus ja toissijainen sivustossa tapahtumien varten. Paikallisen määrittäminen edellyttää kallista ja pitkiä suunnittelu-prosessin toteuttamisesta useiden alueiden tietojen palauttaminen strategia. Kätevästi cloud useimmat palveluntarjoajat, mukaan lukien Azure-Salli helposti alueille sovellusten käyttöönoton. Näiden alueiden jaetaan maantieteellisesti siten, että useiden alueiden palvelu pitäisi olla erittäin harvinaisissa. Strategia tietojen käsittely eri alueilla on maksuvelvollisen tekijät palvelupaketti palauttamisen tietojen mukaisesti.

Seuraavissa osissa käsitellään tietojen palauttaminen tekniikoita liittyvien tietojen varmuuskopioinnin, viitetietojen ja tapahtumatietojen.

###<a name="backup-and-restore"></a>Varmuuskopiointi ja palauttaminen

Säännöllisen varmuuskopioinnin suunnittelu sovelluksen tietojen tukee joitakin tietojen palauttaminen skenaarioita. Eri tallennustilan resurssien edellyttävät erilaisia menetelmiä.

Basic, Vakio ja Premium SQL-tietokanta-tasoa, voit hyödyntää ajankohta palautuksen palauttaa tietokannan. Lisätietoja on artikkelissa [yleiskatsaus: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokannan](../sql-database/sql-database-business-continuity.md). Toinen vaihtoehto on käyttää Active Geo-replikoinnin SQL-tietokantaan. Tietokannan muutokset kopioi automaattisesti toissijaisen tietokantojen samalla Azure alueella tai jopa toiseen Azure-alueelle. Tämä on mahdollinen vaihtoehtoinen joitakin Lisää manuaalisen tietojen synkronointi tapoja, joilla esitettävä sisältö. Lisätietoja on artikkelissa [yleiskatsaus: SQL, tietokanta, aktiivinen Geo-replikoinnin](../sql-database/sql-database-geo-replication-overview.md).

Voit myös käyttää Lisää manuaalisen tapaa varmuuskopiointi ja palauttaminen. TIETOKANNAN kopioi-komennon avulla voit luoda kopio tietokannasta. Tällä komennolla on varmuuskopion sisältämistä tapahtumien yhdenmukaisuuden. Voit käyttää myös Azure SQL-tietokanta Tuo/Vie-palvelun. Tämä tukee vientiä tietokantojen BACPAC Azure-Blob-objektien tallennustilaan tallennettuja tiedostoja.

Azuren tallennustilaan valmiin redundancy Luo replikat varmuuskopiotiedoston samalla alueella. Kuitenkin taajuus käynnissä varmuuskopiointia määrittää oman RPO eli voi hävitä tietojen tilanteissa tietojen määrää. Oletetaan esimerkiksi, että varmuuskopiointi tunnin yläreunaan ja huono ilmenee kaksi minuuttia ennen tunnin alkuun. Menetät tiedot, jotka on tapahtunut sen jälkeen, kun viimeisimmän varmuuskopioinnin 58 minuuttia. Lisäksi voit suojautua alueen laajuinen keskeytetty olisi kopioi BACPAC tiedostot vaihtoehtoinen alueen. Voit sitten halutessasi palauttaminen varmuuskopioinnin vaihtoehtoinen alueen. Lisätietoja on artikkelissa [yleiskatsaus: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokannan](../sql-database/sql-database-business-continuity.md).

Azure-tallennustilan voit kehittää omia mukautettuja varmuuskopiointia tai jollakin useita kolmannen osapuolen varmuuskopion työkaluja. Huomaa, että useimmilla sovelluksen rakenteen muita monimutkaisia missä tallennustilan resurssien viittaavat toisiinsa. Esimerkkinä SQL-tietokantaan, joka on sarake, joka linkittyy blob Azuren tallennustilaan. Varmuuskopioista ei suoriteta samanaikaisesti, jos tietokanta on ehkä Blob-objektien, joka on ei varmuuskopioida ennen virheen osoitinta. Sovelluksen tai tietojen palauttaminen palvelupaketti on pantava prosessit voivat käsitellä tämän ristiriidassa palautuksen jälkeen.

###<a name="reference-data-pattern-for-disaster-recovery"></a>Tiedot viittauksen palauttaminen hakuperuste

Viitetiedot on vain luku-tietoja, joka tukee toiminnoista. Yleensä eivät muutu usein. Varmuuskopiointi ja palauttaminen on yksi tapa käsitellä alueen laajuinen palveluiden keskeytyksiä, mutta RTO on suhteellisen pitkää. Kun asennat sovelluksen toissijainen alue, jotkin strategioita voi parantaa RTO viitetiedot varten.

Koska muutoksia tietoihin viitata harvoin, voit parantaa RTO ylläpito toissijainen alueen viitetiedot pysyvän kopion. Näin varmuuskopioista palauttaminen Jos huono tarvittava aika. Useiden alueiden tietojen palauttaminen vaatimusten täyttämiseksi täytyy ottaa käyttöön sovelluksen ja viitetiedot yhdessä useita alueilla. [Viittaus tietojen](./resiliency-high-availability-azure-applications.md#reference-data-pattern-for-high-availability)mallissa suuren mainittu viitetiedot voit ottaa käyttöön roolin itse ulkoisille tai yhdistelmä molempia.

Viittaus käyttöönoton tietomallin sisällä Laske solmujen implisiittisesti täyttävät tietojen palauttaminen. Viittaus tietojen käyttöönoton SQL-tietokantaan edellyttää viitetiedot kopio käyttöön kunkin alueen. Saman strategia koskee Azure-tallennustilan. Viittaus tiedot, jotka on tallennettu Azuren tallennustilaan ensisijainen ja toissijainen alueiden kopio on otettava käyttöön.

![Ensisijainen ja toissijainen alueiden viittaus tietojen julkaisu](./media/resiliency-disaster-recovery-azure-applications/reference-data-publication-to-both-primary-and-secondary-regions.png)

Oman sovelluksen kielikohtaiset kaikki tiedot, mukaan lukien viitetiedot varmuuskopion rutiinit on pantava täytäntöön. Kopioi GEO replikoida eri alueilla käytetään vain alueen laajuinen keskeytetty. Jotta laajennettu käyttökatkot käyttöön toissijainen alueen sovelluksen tietoja kriittisten osat. Katso tämä topologian esimerkki [Aktiivinen passiivinen mallia](#active-passive).

###<a name="transactional-data-pattern-for-disaster-recovery"></a>Tapahtumatietojen kuvio palauttaminen

Täysin tietojen tilassa strategia toteuttaminen edellyttää asynkroninen replikoinnin toissijainen alueen tapahtumien tiedot. Käytännön ajan kuluessa replikointi voi ilmetä windows määräytyy sovelluksen RPO ominaisuudet. Ehkä silti palauttaa tiedot, jotka on katkennut ensisijainen alueelta aikana replikoinnin-ikkuna. Voit myös ehkä käyttää yhdistää toissijainen alueen myöhemmin.

Arkkitehtuuri seuraavasti Anna tehdä käsittelystä tapahtumatietojen automaattisesti tilanne eri tavalla. On tärkeää muistaa, että esimerkit eivät ole täydellisiä. Keskitason tallennuspaikkojen, kuten olevien voi esimerkiksi korvataan Azure SQL-tietokantaan. Itsensä olevien ehkä Azuren tallennustilaan tai Azure palvelun Bus olevien (katso [Azure olevien ja palvelun Bus olevien--verrattuna ja asiaan](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)). Palvelimen tallennustilan kohteet voivat myös vaihdella kuten Azure taulukoita SQL-tietokannan sijaan. Lisäksi työntekijä roolit voidaan lisätä välittäjiä eri tavalla kuin. Tärkeintä on ei voit emuloida nämä arkkitehtuureihin täsmälleen, mutta kannattaa tapahtumatietojen ja Aiheeseen liittyvät moduulit palautus-eri vaihtoehtoja.

####<a name="replication-of-transactional-data-in-preparation-for-disaster-recovery"></a>Tapahtumatietojen valmistelussa palauttaminen sallittuja

Harkitse Azuren tallennustilaan olevien pitoon tapahtumatietojen käyttävä sovellus. Näin työntekijä roolit käsittelemään tapahtumien tiedot erilliset arkkitehtuuri server-tietokantaan. Tämä edellyttää joitakin lomakkeen tilapäinen välimuistiin tallentamisen edusta roolit vaativatko heti kyselyn tietojen tapahtumat. Tietojen menetyksen poikkeama tason mukaan voit esimerkiksi toistaa olevien, tietokannan tai kaikki tallennustilan resurssit. Vain tietokannan toistot, jos ensisijainen alueen siirtyy alaspäin, voit silti palauttaa olevien tietojen ensisijainen alueen tullessa takaisin.

Seuraavassa kaaviossa näkyvät arkkitehtuuri, jossa server-tietokantaan on synkronoitu eri alueilla.

![Tapahtumatietojen valmistelussa palauttaminen sallittuja](./media/resiliency-disaster-recovery-azure-applications/replicate-transactional-data-in-preparation-for-disaster-recovery.png)

Tämä arkkitehtuuri toteuttamisesta suurimmista haasteellista on replikoinnin strategia alueiden välillä. Azure SQL-tietojen synkronointi-palvelun avulla tällaista replikoinnin. Kuitenkin palvelu on yhä esikatselu ja ei ole suositeltavaa tuotannon ympäristössä. Lisätietoja on artikkelissa [yleiskatsaus: Cloud liiketoiminnan jatkuvuuden ja tietokannan tietojen palauttaminen SQL-tietokannan](../sql-database/sql-database-business-continuity.md). Tuotannon sovellusten sijoittamaan kolmannen osapuolen-ratkaisussa tai Luo omia replikoinnin logiikka koodissa. Kopioitavien arkkitehtuuri replikointi välttämättä kaksisuuntainen, joka on myös monimutkaisia.

Yksi mahdollinen käyttöönotto voi olla käyttö keskitason jonossa edellisessä esimerkissä. Työntekijän rooli, joka käsittelee lopullinen tallennustilan kohteeseen tietoja voi tehdä muutoksen ensisijainen alue-ja toissijainen alue. Ei trivial tehtävät ja valmis ohjeet replikoinnin koodi on laajemmin tässä artikkelissa. Tärkeää-kohta on paljon aikaa ja testaaminen kannattaa keskittyä miten tietojen replikoida toissijainen alueen. Lisää käsittelyä ja testaaminen auttaa varmistamaan automaattisesti ja palautus-prosessien käsittele oikein minkä tahansa tietojen epäyhtenäisyydet tai kaksoiskappaleiden tapahtumia.

>[AZURE.NOTE] Useimmat tässä asiakirjassa keskitytään ympäristö palveluna (PaaS). Replikoinnin ja käytettävyyden lisäasetusten määrittäminen hybrid sovellusten kuitenkin käyttää Azuren näennäiskoneiden. Hybrid nämä sovellukset Määritä julkaisuinfrastruktuuri palvelu (IaaS) isännöimiseen SQL Server-näennäiskoneiden Azure-tietokannassa. Näin perinteinen käytettävyys tavoista SQL Serveristä, kuten AlwaysOn käytettävyys ryhmät tai lokin toimitus. Joitakin menettelytapoja, kuten AlwaysOn, toimi vain paikallisen SQL Server-esiintymät ja Azure-virtuaalikoneissa välillä. Lisätietoja on artikkelissa [suuren käytettävyyden ja SQL Server Azuren näennäiskoneiden palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

####<a name="degraded-application-mode-for-transaction-capture"></a>Tapahtuman sieppaaminen tila heikentää sovelluksen

Harkitse toisen arkkitehtuuri, joka toimii eivät toimi oikein-tilassa. Toissijainen alueen sovelluksen poistaa käytöstä kaikki toiminnot, kuten raportointia liiketoimintatietojen (BI) tai tyhjennys olevien. Se hyväksyy tärkeimmät tietyntyyppisiä tapahtumien työnkulut-määrityksen mukaisesti business vaatimuksia. Järjestelmä sieppaa tapahtumia ja kirjoittaa ne olevien. Järjestelmä lykätä tietojenkäsittely keskeytetty alkuvaihe aikana. Jos järjestelmä ensisijainen alueen aktivoidaan ajasta keskusteluikkunasta käsin, ensisijainen alueen työntekijä-roolien voit Tyhjennä olevien. Tämä toimenpide ei tarvita tietokannan yhdistämistä varten. Jos ensisijainen alueen keskeytetty ulottuvat suurin sallittu ikkunan-sovelluksen käynnistää käsittely olevien.

Tässä skenaariossa toissijaisen tietokanta sisältää vaiheittainen tapahtumatietojen, jotka haluat liittää sen jälkeen, kun ensisijainen aktivoidaan. Seuraavassa kaaviossa näkyvät tässä strategia tallentaminen tapahtumatietojen tilapäisesti, kunnes ensisijainen alueen palautetaan.

![Tapahtuman sieppaaminen tila heikentää sovelluksen](./media/resiliency-disaster-recovery-azure-applications/degraded-application-mode-for-transaction-capture.png)

Saat enemmän tietoja hallintamenetelmän joustavat Azure sovellusten keskustelun [Failsafe: joustavat Cloud arkkitehtuureihin ohjeet](https://channel9.msdn.com/Series/FailSafe).

##<a name="deployment-topologies-for-disaster-recovery"></a>Käyttöönoton topologioissa varten palauttaminen

Sinun täytyy laatia kriittisten sovellusten alueen laajuinen keskeytetty mahdollisuudesta. Voit tehdä tämän käyttämällä useiden alueiden käyttöönottostrategian on sisällytetty toiminnallisia suunnittelu.

Useiden alueiden käyttöönotoissa voi liittyä IT-pro prosessit sovellus- ja viitefunktiot tietojen julkaiseminen toissijainen alueen huono. Jos sovellus edellyttää pikaviestien automaattisesti, käyttöönottoprosessin voi liittyä Aktiivinen/passiivinen-asennus tai aktiivista-asetukset. Tällaista käyttöönoton on käynnissä vaihtoehtoinen alueen sovelluksen esiintymät. Reititys kuten Azure liikenteen hallinta-työkalun avulla kuormituksen tasaamisen palvelujen DNS-tasolla. Voit tunnistaa palveluiden keskeytyksiä ja reitittää käyttäjien eri alueiden tarvittaessa.

Onnistuneiden Azure palauttaminen osa on suunnittelee kyseisen palautus alusta ratkaisuksi. Pilveen on lisäasetuksia siitä aikana huono virheet, jotka eivät ole käytettävissä perinteinen isännöintipalvelussa. Tarkemmin sanottuna dynaamisesti ja nopeasti resursseja voi kohdistaa eri alueeseen. Tämän vuoksi ei maksat usein käyttämättömänä resurssien Odotellessasi voit, ilmenee virhe.

Seuraavissa osissa kansilehden eri käyttöönoton topologioissa tietojen palauttamista varten. Yleensä on tarjoa parantavat kustannukset tai muita käytettävyyden monimutkaisuuden.

###<a name="single-region-deployment"></a>Yhden alueen käyttöönotto

Yhden alueen käyttöönoton ei ole todella tietojen palauttaminen-topologian, mutta tarkoitus on kontrastia muihin arkkitehtuureihin kanssa. Yhden alueen ominaisuuksissa ovat yleisiä sovellusten Azure-tietokannassa. Tällaista käyttöönoton ei kuitenkaan vakavia haastaja tietojen palauttaminen suunnitelman.

Seuraavassa kaaviossa on esitetty sovelluksen yksittäisen Azure alueen. Azure liikenteen hallinta ja vika ja päivitys-toimialueiden käyttöä Suurenna käytettävyyttä sovelluksen alueella.

![Yhden alueen käyttöönotto](./media/resiliency-disaster-recovery-azure-applications/single-region-deployment.png)

Tässä on näennäinen tietokanta on yhden kohdan virhe. Vaikka Azure kopioi tiedot sisäisten replikoihin toimialueilla eri vika, tämä kaikki tapahtuu samalla alueella. Sovellus ei kestävät kriittinen virhe. Jos alue siirtyy alaspäin kaikki vika toimialueet siirtyy alaspäin--mukaan lukien kaikki palvelun esiintymät ja tallennustilaa resurssit.

Kaikkien mutta vähintään kriittisten sovellusten on suunnittelemaan perussuunnitelman ottamaan sovellustesi useiden alueiden välillä. Kannattaa myös harkitse RTO ja kustannukset-ottaen huomioon mitä käyttöönoton topologian käyttämään rajoitukset.

Voit nyt tarkastella tietyn kuviot tukemaan eri alueiden automaattisesti. Kaikissa näissä esimerkeissä käytetään kahta aluetta kuvaamaan prosessi.

###<a name="redeployment-to-a-secondary-azure-region"></a>Lukea toissijainen Azure alue

Toissijainen alueen lukea kohdalla ensisijainen alue on sovellukset ja tietokannat ovat käynnissä. Toissijainen alue ei ole määritetty, automaattisesti. Niin huono yhteydessä, sinun on asettamasi uuden alueen palvelun osat. Tämä sisältää pilvipalveluun lataaminen Azure-käyttöönotto pilvipalvelussa, tietojen palauttaminen ja haluat reitittää liikenteen uudelleen DNS muuttaminen.

Tämä on useita alue-vaihtoehdot edullinen eniten, mutta se on huonoin RTO ominaisuuksia. Tämän mallin palvelun paketin ja tietokannan varmuuskopiot tallennetaan joko paikallisessa tai Azure Blob storage esiintymän toissijainen alueen. On kuitenkin käyttöön uusi palvelu ja palauttaa tiedot, ennen kuin se säilyy toiminto. Vaikka täysin automaattisen varmuuskopioinnin säilöstä tiedonsiirron, uusi tietokanta-ympäristön luomiseen pyörivä siinä käytetään aikaa. Tietojen siirtämistä varmuuskopiolevy säilöstä tyhjän tietokannan toissijainen alueella on eniten kallista palautus-osa. Toimi samoin, mutta voit tuoda uuden tietokannan toiminnan tilassa, koska se ei ole replikoida.

Paras tapa on tallentaa service-paketit Blob-objektien tallennustilaan toissijainen alueen. Tämä ei tarvitse paketin lataaminen Azure, eli, mitä tapahtuu, kun otat paikallisen kehittäminen tietokoneesta. Voit nopeasti ottaa service-paketit uusi pilvipalveluun Blob-objektien tallennustilaan PowerShell-komentosarjojen avulla.

Tämä asetus on hyödyllinen vain vähäinen sovelluksia, jotka hyväksyt hyvin RTO varten. Esimerkiksi Tämä saattaa toimia sovellus, joka voi olla useita tunteja alaspäin, mutta on oltava käynnissä uudelleen 24 tunnin kuluessa.

![Lukea toissijainen Azure alue](./media/resiliency-disaster-recovery-azure-applications/redeploy-to-a-secondary-azure-region.png)

###<a name="active-passive"></a>Aktiivinen-passiivinen

Aktiivinen-passiivinen kuvio on valinta, jotka Monet yritykset Web-sivuston yhteensopivaksi. Tämä kaava sisältää parannuksia RTO suhteellisen pieni korotus kustannus-ja lukea mallia nähden.
Tässä skenaariossa on uudelleen ensisijaisen ja toissijaisen Azure alue. Kaikki liikenne siirtyy aktiivisen käyttöönoton ensisijainen alueen. Toissijainen alueen paremmin valmistautua palauttaminen, koska tietokannan käytössä molemmat alueet. Lisäksi synkronointi-järjestelmä on määritetty, niiden välillä. Tämän valmius menetelmän voi sisältyä kaksi variaatiot: vain tietokanta-vaihtoehto tai valmis käyttöönoton toissijainen alueen.

####<a name="database-only"></a>Vain tietokannan

Aktiivinen-passiivinen kuvion ensimmäisen variaation ensisijainen alue on otettu käyttöön cloud palvelusovelluksen. Kuitenkin lukea kohdalla, toisin kuin molemmat alueet synkronoidaan ja tietokannan sisällön. (Lisätietoja on kohdassa Valitse [tapahtumatietojen kuvio palauttaminen](#transactional-data-pattern-for-disaster-recovery).) Huono toteutuessa on vähemmän aktivoinnin vaatimukset. Käynnistä sovellus toissijainen alueen, muuta yhteysmerkkijonon uuden tietokannan ja muuttaa tapaa tietoliikenteen DNS-arvoja.

Lukea kohdalla, kuten olisi olet jo tallentanut service-paketit Azure Blob-objektien tallennustilaan toissijainen alueen nopeampaa käyttöönottoa varten. Toisin kuin lukea, kuvio ei maksamaan katseltavan, joka edellyttää tietokannan palautustoiminto useimpia. Tietokanta on valmis ja käytössä. Toiminto tallentaa merkittäviä ajanjakson ajan, että edullinen DR-kuvion. Kannattaa myös Suosituimmat DR kuvio.

![Vain aktiivinen-passiivinen-tietokanta](./media/resiliency-disaster-recovery-azure-applications/active-passive-database-only.png)

####<a name="full-replica"></a>Kokonaisen replikan

Ensisijainen alue ja toissijainen alue on aktiivinen passiivinen kuvion toisen variaation koko käyttöönottoa. Käyttöönottoon sisältää pilvipalveluun ja synkronoidun tietokannan. Ensisijainen alue on kuitenkin aktiivisesti käsitteleminen verkon pyyntöihin käyttäjiltä. Toissijainen alueen tulee aktiivinen vain silloin, kun ensisijainen alueen ilmenee keskeytetty. Siinä tapauksessa kaikki uudet etätietokone reitittää toissijainen alue. Azure liikenteen hallinta voi hallita tätä automaattisesti automaattisesti.

Automaattisesti ilmenee vain tietokannan variaation nopeammin, koska palveluja on jo otettu käyttöön. Tämä kaava sisältää erittäin pienen RTO. Toissijainen automaattisesti-alue on oltava valmis lähetettäväksi heti ensisijainen alueen virheen jälkeen.

Vastauksen nopeammin ajan, sekä tätä mallia valmiiksi varaamalla ja käyttöönotto varmuuskopion services etuna on. Sinun ei tarvitse huolehtia ilman väli, jos haluat varata uusia esiintymiä huono-alueen. Tämä on tärkeää, jos toissijaisen Azure alueellasi on lähestyvä kapasiteetti. Ei ei takaa (service tason sopimus), että välittömästi osaat käyttöön uuden pilvipalveluihin minkä tahansa alueella määrä.

Nopein vastaus kerran tämän mallin avulla on oltava samalla asteikko (rooli kerrat) ensisijainen ja toissijainen alueilla. Huolimatta on hyötyä maksaa käyttämättömät Laske esiintymien on kallista ja ei ehkä ole useimmat järkevä taloudellisen tilanteen valinta. Tästä syystä on yleisimpiä pilvipalveluihin hieman skaalata alaspäin version käyttämisestä toissijainen alue. Sitten voit nopeasti epäonnistua päälle ja skaalata ulos toissijainen käyttöönoton tarvittaessa. Sinun pitäisi automatisoida automaattisesti niin, että ensisijainen alue ei ole käytettävissä, kun olet aktivoinut uusia esiintymiä kuormituksen mukaan. Tämä saattaa edellyttää automaattisen skaalauksen poistaminen-järjestelmä, kuten [virtuaalikoneen skaalaus-vaihtoehdon](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)käyttö.

Seuraavassa kaaviossa on esitetty malli, jossa ensisijaisen ja toissijaisen alueiden sisältävät täysin käyttöön pilvipalvelussa Aktiivinen-passiivinen-mallissa.

![Aktiivinen-passiivinen, koko replika](./media/resiliency-disaster-recovery-azure-applications/active-passive-full-replica.png)

###<a name="active-active"></a>Aktiivinen aktiivinen

Nyt, jotka sinun on todennäköisesti käyttäminen kuviot kehitys: pienenevillä RTO kasvaa kustannuksia ja monimutkaisuus. Aktiivinen aktiivinen-ratkaisun katkaisee todella tämän taipumusta tarttua toisiinsa ottaen huomioon kustannukset.

Aktiivinen aktiivinen-mallissa pilvipalveluihin ja tietokanta on täysin otettu käyttöön molemmat alueet. Toisin kuin aktiivinen-passiivinen-mallin molemmat alueet saavat käyttäjien tietoliikenne. Tämä vaihtoehto tuottaa nopein palauttamisaika. Palvelujen on jo skaalattu käsittelemään kuormituksen osoitteessa kunkin alueen osa. DNS on jo käytössä toissijainen alue. Määrittää, miten käyttäjät reitittämiseen haluamasi alue on muita monimutkaisuutta. Pyöreän pöydän ajoituksen voi olla mahdollista. On yleinen tietyille käyttäjille käyttäisi tietyn alueen, jossa ensisijaisen kopio, niiden tiedot sijaitsevat.

Automaattisesti, kun käytöstä DNS ainoastaan ensisijainen alueen. Tämä reitittää liikenteen kaikki toissijainen alue.

Vaikka tämä malli on joitakin variaatiot. Esimerkiksi seuraavassa kaaviossa näkyvät mallin jossa ensisijaisen alueen omistaa master-kopio tietokannasta. Ensisijainen tietokannan kirjoittaa molemmat alueilla pilvipalveluihin. Toissijainen käyttöönotto voi lukea ensisijainen tai replikoitua tietokantaa. Tässä esimerkissä Replikointi tapahtuu yksi tapa.

![Aktiivinen aktiivinen](./media/resiliency-disaster-recovery-azure-applications/active-active.png)

Tällä Entä huonot puolet, voit edellä olevassa kaaviossa aktiivinen aktiivinen-arkkitehtuuri. Toisesta on käyttää ensimmäisen alueen tietokannan, koska perustyylin kopioiminen sijaitsee siellä. Suorituskyvyn pudottaa merkittävästi käytöstä, kun käytät tietojen alueen ulkopuolella. Rajat-alueen tietokannan kutsujen Ota huomioon jonkin jonottaminen strategia puhelut suorituskyvyn parantamiseksi. Lisätietoja on artikkelissa [käyttämisestä jonottaminen SQL-tietokantaan sovelluksen suorituskyvyn parantamiseksi](../sql-database/sql-database-use-batching-to-improve-performance.md).

Vaihtoehtoinen arkkitehtuuri voi liittyä kunkin alueen omassa tietokannan käytön suoraan. Mallin jonkin kaksisuuntainen replikoinnin tarvitaan synkronoida kunkin alueen tietokannat.

Aktiivinen aktiivinen rakenteessa et ehkä tarvitse niin monta esiintymät ensisijainen alueen Aktiivinen passiivinen rakenteessa tapaan. Jos sinulla 10 esiintymät Aktiivinen-passiivinen-arkkitehtuuri ensisijainen aluetta, sinun on ehkä vain aktiivinen aktiivinen-arkkitehtuuri kunkin alueen 5. Molemmat alueet jakaa Lataa nyt. Voi olla kustannukset säästöjen päälle Aktiivinen passiivinen mallia, jos pidät kiinnostunut valmiustilassa passiivinen alueen automaattisesti odotetaan 10 esiintymät.

Huomaat, että ennen kuin voit palauttaa ensisijainen alue, toissijainen alueen voi tulla äkillinen ylijännitesuojan uusien käyttäjien. Jos määritettynä on 10 000 käyttäjää jokaisessa palvelimessa, kun ensisijainen alueen ilmenee keskeytetty, toissijainen alueella on yhtäkkiä käsittelemään 20 000 käyttäjät. Seuranta säännöt toissijainen alue on tunnistettava kasvaa ja kaksinkertainen esiintymät toissijainen alueen. Lisätietoja tästä on kohdassa [virheen](#failure-detection)tunnistus.

##<a name="hybrid-on-premises-and-cloud-solution"></a>Hybrid paikallisen ja cloud ratkaisu

Yhden palauttaminen muita strategia kannattaa arkkitehti hybrid-sovellus, joka suoritetaan paikallisen ja pilveen. Sovelluksen ensisijainen alueen voi joko sijainti. Harkitse edellisen arkkitehtuureihin ja Kuvitellaan ensisijaisen tai toissijaisen alueen paikallisen sijainniksi.

Nämä hybrid arkkitehtuureihin on joitakin haasteita. Useimmat tässä artikkelissa on ensin osoitettu PaaS arkkitehtuuri kuviot. Tyypillinen Azure PaaS sovellukset ovat riippuvaisia Azure-kohtaisia rakenteita, kuten roolit, pilvipalveluihin ja liikenteen hallinta. Voit luoda paikallisen ratkaisun tämäntyyppisen PaaS sovelluksen edellyttäisi merkittävästi arkkitehtuuri. Tämä ei ehkä ole mahdollista hallinta tai kustannukset näkökulmasta.

Tietojen palauttaminen hybrid ratkaisu on kuitenkin vähemmän haasteisiin, perinteinen arkkitehtuureihin, joka on siirretty yksinkertaisesti pilveen. Tämä on totta koskien arkkitehtuureihin, jotka käyttävät IaaS. IaaS sovellukset käyttää näennäiskoneiden pilvipalvelussa, joka voi olla suora paikallisen vastineet. Voit käyttää myös virtual verkkojen yhdistämään paikallisen verkkoresursseja koneet pilveen. Tämä avaa useita mahdollisuuksia, joita ei ole mahdollista vain PaaS-sovellusten kanssa. Esimerkiksi SQL Server voit hyödyntää tietojen palauttaminen ratkaisuja, kuten AlwaysOn käytettävyys ryhmiä ja tietokantapeilausta. Lisätietoja on artikkelissa [suuren käytettävyyden ja SQL Server Azure-virtuaalikoneissa palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md).

IaaS ratkaisut tarjoavat myös helpompaa polun, käytä Azure automaattisesti-asetus paikallisen sovellusten. Voit joutua toimiva sovelluksen paikallisen-alueella. Mutta Entäpä jos sinulla ei ole pitämään automaattisesti maantieteellisesti erilliset alueen resurssit? Voit päättää käyttäminen näennäiskoneiden ja virtual verkot sovelluksesi Azure käynnissä. Määritä siinä tapauksessa prosesseja, jotka synkronoidaan tiedot pilvipalveluun. Azure käyttöönoton jälkeen tulee toissijainen alue, jos haluat käyttää automaattisesti. Ensisijainen alue pysyy paikallisen-sovellus. Lisätietoja IaaS arkkitehtuureihin ja ominaisuudet on [näennäiskoneiden ohjeissa](https://azure.microsoft.com/documentation/services/virtual-machines/).

##<a name="alternative-cloud"></a>Vaihtoehtoinen pilveen

Tilanteita, joissa myös Microsoft Cloud vakauden ei riitä sisäinen yhteensopivuuden sääntöjä tai käytäntöjä, jotka organisaatio edellyttää. Myös parhaan valmistelu ja rakenteen toteuttamisesta varmuuskopion järjestelmien aikana huono odotuksia, onko yleinen keskeytetty cloud-palveluntarjoajan.

Haluat verrata käytettävyys tarpeet ja kustannus- ja monimutkaisuudesta parannettu käytettävyys. Riskin analysoinnissa ja määritä RTO ja RPO ratkaisu. Jos sovellus ei hyväksy kaikki käyttökatkot, se voi tulkita sinun kannattaa käyttää cloud ratkaisu. Ellei koko Internetiin menee cloud ratkaisu edelleen ole käytettävissä, jos Azure tulee yleisesti ei ole käytettävissä.

Hybrid tilanne, jossa Edellinen tietojen palauttaminen arkkitehtuureihin automaattisesti-ympäristöissä voi esiintyä myös toisessa cloud-ratkaisussa. Vaihtoehtoinen cloud DR sivustojen käytetään vain ratkaisuja, joiden RTO sallii hyvin vähän mahdollisesti käyttökatkot. Huomaa, että ratkaisuun, joka käyttää DR sivuston ulkopuolella Azure edellyttää enemmän työtä, kehittää, ottaminen käyttöön ja säilyttää. On myös vaikeaa toteuttavien parhaita käytäntöjä rajat cloud-arkkitehtuuri. Vaikka cloud ympäristöjen on samanlainen korkean tason käsitteitä, ohjelmointirajapinnan ja arkkitehtuureihin ovat erit.

Jos päätät jakaa oman DR eri ympäristöjen välillä, kokoelmasta kannattaa arkkitehti otetaan kerrokset ratkaisua rakennenäkymässä. Jos teet tämän, ei tarvitse kehittää ja ylläpitää kaksi eri versioita samassa sovelluksen eri cloud ympäristöjen järjestelmän tietojen varalta. Kuin hybrid skenaario Azuren näennäiskoneiden tai Azure säilö palvelun voi olla helpompaa tällaisissa tapauksissa cloud kielikohtaiset PaaS rakenteet käyttämistä.

##<a name="automation"></a>Automaatio

Jotkin kuviot, vain kerrottu edellyttävät offline-tilassa ominaisuuksissa nopeasti aktivointi ja tiettyjen osien järjestelmän palauttamiseen. Automaatio tai komentosarjan, tukee voi aktivoida resurssien pyydettäessä ja ratkaisujen nopeasti käyttöön. Tässä artikkelissa DR liittyvät automaatio on saama [Azure PowerShellin](https://msdn.microsoft.com/library/azure/jj156055.aspx)avulla, mutta [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) on myös haluamasi vaihtoehto.

Kehittäminen komentosarjojen avulla voit hallita, Azure ei käsittele läpinäkyvä DR osat. Tämä on etu tuottavat yhtenäiset tulokset aina, kun, joka minimoi ihmisten virhe mahdollisuutta. Ottaa ennalta DR komentosarjojen myös vähentää uudelleen järjestelmän ja sen osat huono in the midst of aikaa. Et halua yritä selvittää manuaalisesti, sivuston palauttaminen aikana alaspäin ja hävinnyt rahaa minuutin välein.

Kun olet luonut komentosarjat, Testaa ne toistuvasti alusta loppuun. Kun olet varmistanut niiden perustoimintoja, varmista, että kannattaa testata- [tietojen simuloinnissa](#disaster-simulation). Näin paljastaa komentosarjoja tai prosessit virheet.

Parhaiden käytäntöjen kanssa automaatio kannattaa luoda säilön PowerShell-komentosarjojen tai Azure palauttaminen komentosarjat käyttöliittymä (CLI). Merkitse selkeästi ja luokitella ne helposti haussa. Määrittää yhden henkilön säilöön ja komentosarjat versiotietojen hallinta. Hyvin selitykset parametrit ja esimerkkejä komentosarjan käyttäminen sisältävä asiakirja ne. Varmista myös, että voit säilyttää dokumentaatio synkronoinnissa Azure asennuksia. Tämä alaviivoja tarkoituksena on yksi Vastuuhenkilö tietovarasto kaikista osista.

##<a name="failure-detection"></a>Virhe tunnistus

Käsittelee oikein käytettävyys ja palauttaminen liittyviä ongelmia, sinun on voitava tunnistaa ja virheet vianmäärityksen. Muuta Lisäasetukset-palvelimen nimen ja käyttöönoton seuranta, jotta voit nopeasti näkevät, milloin järjestelmän tai sen osat ovat yhtäkkiä alaspäin. Valvontatyökalut, tarkista käytössäsi ovat sekä riippuvaa yleinen kunto voi suorittaa osa on. Yksi Microsoft-työkalusta on [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/). Kolmannen osapuolen työkalut tarjoavat myös valvontaan. Useimmat seurantaa ratkaisuja seurata suorituskyvyn laskureita ja palvelun saatavuus.

Vaikka työkaluja ovat tärkeitä, ne eivät korvaa suunnitteleminen vika tunnistaminen ja raportoinnin sisällä pilvipalveluun ei tarvitse. On aiot käyttää oikein Azure Diagnostiikka. Mukautetun suorituskyvyn laskureita tai tapahtuman lokimerkintöjä voidaan myös kokonaisvaltainen strategia osa. Tämä sisältää enemmän tietoja aikana virheet nopeasti vianmäärityksessä ja palauttaa koko ominaisuuksia. Se sisältää myös muita tietoja, joista valvontatyökaluja käyttää sovelluksen terveydentilasta. Lisätietoja on artikkelissa [Ottaminen käyttöön Azure diagnostiikan Azure Cloud Services-palveluissa](../cloud-services/cloud-services-dotnet-diagnostics.md). Lisätietoja siitä, miten yleinen "kunto mallin" suunnitteleminen-kohdassa [Failsafe: joustavat Cloud arkkitehtuureihin ohjeet](https://channel9.msdn.com/Series/FailSafe).

##<a name="disaster-simulation"></a>Tietojen simuloinnissa

Simuloinnissa testaaminen liittyy luominen small käytännön tilanteissa seurata, miten ryhmän jäsenet keskustelevat työ-kerroksen. Kuinka tehokkaasti ratkaisuja ovat palauttamista suunnitelman simulaatioita näyttää. Suorita simulaatioita siten, että luotu skenaariot ei häiritä todellinen business aikana edelleen tiedostoja, kuten reaali tilanteissa.

Harkitse suunnittelee "valikkonäyttö-sovelluksessa voit simuloida manuaalisesti käytettävyysongelmia tyyppi. Esimerkiksi – Pehmeät valitsin käynnistää hyödyntämällä aiheuttaa sen virheellisesti tietokannan käytön poikkeukset järjestäminen moduuli. Voi kestää samalla kevyt tavoista muiden moduuleissa verkon käyttöliittymän tasolla.

Sen korostaa ongelmat, jotka on osoitettu koskevissa. Simuloitu käyttötavoista on oltava kokonaan hallittavissa. Tämä tarkoittaa, että myös silloin, kun palautus-suunnitelma vaikuttaa olla viallinen, voit palauttaa tilanne takaisin normaaliksi ilman aiheuttaa merkittäviä vahinkojen. On myös tärkeää ilmoittaa siitä, milloin ja miten simuloinnissa harjoitusten suoritetaan ylemmän tason hallinta. Tätä palvelupakettia sisällyttävä tiedot aika- tai resurssit, jotka saattavat muuttua unproductive, simuloinnissa testi on käynnissä. Kun olet laajennetaan virhetilanteiden testi, on myös tärkeää määrittää, miten success mitataan.

On useita muita menetelmiä, joiden avulla voit testata tietojen. Useimmat ne ovat kuitenkin yksinkertaisesti muutettu versiot basic seuraavilla tavoilla. Tärkeimmät motive takana testi on arvioida miten mahdollista ja miten toimiva palautus-palvelupaketti on. Tietojen palauttaminen testaaminen ohjeessa on tietoja tuttuihin aukkoja basic palautus-suunnitelma.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [palauttaminen ja suuren käytettävyyden sovellusten Microsoft Azure rakennettu](./resiliency-disaster-recovery-high-availability-azure-applications.md)artikkelisarja osa. Edellinen artikkeli sarjassa on [suuri käytettävyys sovellusten Microsoft Azure rakennettu](./resiliency-high-availability-azure-applications.md).
