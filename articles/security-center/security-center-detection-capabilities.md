<properties
   pageTitle="Azure Tietoturvakeskus ominaisuuksien tunnistus | Microsoft Azure"
   description="Tämän asiakirjan avulla voit ymmärtää, kuinka Azure Tietoturvakeskus tunnistusominaisuudet toimivat."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Azure Tietoturvakeskus tunnistusominaisuudet
Tässä asiakirjassa kerrotaan Azure Tietoturvakeskus Lisäasetukset tunnistus ominaisuuksia, joiden avulla voit tunnistaa kohdistamisen resurssien Microsoft Azure active uhkien ja avulla voit nopeasti vastata tarvittavat tiedot.

> [AZURE.NOTE] Tunnistuksia Lisäasetukset ovat käytettävissä Standard taso ja Azure Tietoturvakeskus. 90 päivän maksuton kokeiluversio on käytettävissä. Voit päivittää [Suojauskäytäntö](security-center-policies.md)hinnoittelua taso-valinnasta. Lisätietoja saat lisätietoja hinnoittelua [Tietoturvakeskus sivun](https://azure.microsoft.com/pricing/details/security-center/) . 


## <a name="responding-to-todays-threats"></a>Tämän päivän uhkien vastaaminen
On tehty merkittäviä muutoksia uhkien vaaka-20 edellisen vuoden aikana. Aiemmin yritykset yleensä vain oli huolehtia sivuston muokkaamishyökkäys yksittäisiä hyökkääjät, jotka olivat lähinnä kiinnostunut pelkkiä "mitä käyttäjä voi tehdä". Tämän päivän hyökkääjät ovat paljon kehittyneitä ja järjestyksessä. Heillä on usein tiettyä rahoitus- ja strategisia tavoitteita. Heillä on lisää resursseja, jotka ovat käytettävissä myös silloin, kun ne on ehkä rahoittaja nation tilan tai järjestäytyneen rikollisuuden.

Tämän menetelmän johtanut voi saada järjestää arvot ammattitaidon ennennäkemättömät taso. Enää ovat ne kiinnostuneita sivuston muokkaamishyökkäys. Nykyään ne ovat kiinnostuneita varastamasta tietoja, Kirjanpito ja yksityiset tiedot – kaikki, jonka he voivat käyttää luomiseen kassavirrasta Avaa markkinoille tai hyödyntää liiketoiminnan, käyttämällä- tai sijainti. Entistä enemmän koskevat kuin ne hyökkääjät taloudellisen tilanteen tavoitteen kanssa on hyökkääjät joka rikkonut verkkojen haittaa infrastruktuuri- ja henkilöt.

Vastauksen organisaatiot Ota usein eri piste-ratkaisuja, jotka keskittyä puolustaa yrityksen rajojen tai päätepisteet hakemalla tunnetut hyökkäyksen allekirjoitukset. Luo pienen tarkkuuden ilmoituksia, jotka edellyttävät suojaus-henkilön kiireellisyysjärjestys ja tutkia määrää yleensä seuraavia ratkaisuja. Useimmat organisaatiot ei ole aikaa ja ammattitaidon pakollinen vastata äänimerkkien – niin paljon Siirry unaddressed.  Tänä aikana hyökkääjät on kehittynyt niiden menetelmiä subvert monta allekirjoitus-pohjainen suojaus ja [mukauttaa cloud-ympäristössä](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Uusi tavoista on entistä nopeammin tunnistaa uusia uhkien ja nopeuttamiseksi tunnistaminen ja vastaus. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Miten Azure Tietoturvakeskus tunnistaa ja uhkien reagoi

Microsoftin security tutkijoiden ovat jatkuvasti uhkien Etsi. Heillä on pääsy telemetriatietojen Microsoftin yleinen tavoitettavuuden cloud ja paikallisen saatu laajojen joukko. Tämä tietojoukkoja wide kurottavat ja monipuolisen kokoelma avulla Microsoft voi löytää uuden hyökkäyksen toistuvien tulosten ja trendien yli paikallisen kuluttaja- ja enterprise tuotteiden sekä online-palveluihin. Tietoturvakeskuksessa voit seurauksena päivittää sen tunnistus algoritmit nopeasti kuin hyökkääjät Vapauta uudet ja entistä enemmän kehittyneitä hyödyntämän. Tämän menetelmän auttaa pysymään ajan nopea siirtäminen uhkien-ympäristössä. 

Tietoturvakeskus uhkien havaitseminen tapahtuu keräämällä automaattisesti suojaustiedot Azure resursseja, verkon ja yhdistetyn kumppaniratkaisuihin. Se analysoi nämä tiedot hajautettuna usein tietoja useista lähteistä tunnistavan uhkien. Suojausvaroitusten ensin Tietoturvakeskuksessa sekä suosituksia riskien uhalla muutoin parantaminen.

![Tietoturvakeskus tietojen kerääminen ja esitys](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Tietoturvakeskus käytetään laajennettu analytics, jossa paljon muutakin kuin allekirjoitus-pohjainen tavoista. Todellisuutta big datasta ja-tekniikoiden [konepohjaisten oppimistekniikoiden](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) ovat hyödyntää arvioida tapahtumien koko cloud kangasta – tunnistaminen uhkien, joka on mahdotonta tunnistaa käyttämällä manuaalinen lähestyessä ja ennakoimista kalastelu projektinhallintajärjestelmien yli. Nämä suojauksen analytics ovat seuraavat: 

- **Integroitu uhkien liiketoimintatietojen**: etsii tunnetut bad toimijoiden mukaan Microsoft-tuotteiden ja -palveluista, Microsoft digitaalisen rikokset yksikkö (DCU), Microsoftin Security vastauksen Center (MSRC) ja ulkoisen syötteet yleisen uhkien liiketoimintatietojen hyödyntäminen.
- **Käytöspiirteen analytics**: tunnetut kuviot tuttuihin haittaohjelmien ongelma koskee. 
- **Poikkeavuuksista tunnistus**: käyttää luonnissa historiallisten perusaikataulun tilastollinen profilointi. Se ilmoittaa muodostettu perusaikataulujen, jotka noudattavat mahdolliset hyökkäystavan poikkeamia.


### <a name="threat-intelligence"></a>Uhkien liiketoimintatietojen
Microsoft on yleinen uhkien liiketoimintatietojen erittäin määrä. Telemetriatietojen jatkuu useista lähteistä, kuten Azure, Office 365: ssä, Microsoft CRM online, KIELIASETUS, outlook.com, MSN.com, Microsoft digitaalisen rikokset yksikkö (DCU) ja Microsoftin Security vastauksen Center (MSRC). Tutkijoiden tulla uhkien liiketoimintatiedot, jotka on jaettu kesken pää cloud palveluntarjoajien ja uhkien Liiketoimintatieto-syötteitä kolmansien osapuolten tilaa. Azure Tietoturvakeskuksessa voit ilmoittaa tunnetut bad toimijoiden uhkien tämän toiminnon avulla. Esimerkkejä:

- **Haittaohjelmien IP-osoite lähtevän tietoliikenteen**: lähtevän tietoliikenteen tunnetut botnet tai darknet todennäköisesti ilmaisee, että resurssi on ongelma luvaton se yritetään suorittaa komennot järjestelmän tai exfiltrate tietoja. Azure Tietoturvakeskus vertaa verkkoliikennettä Microsoftin yleinen uhkien tietokantaan, ja ilmoittaa, jos se havaitsee tietoliikenne haittaohjelmien IP-osoite.

## <a name="behavioral-analytics"></a>Käytöspiirteen analytics

Käytöspiirteen analytics on hyvä, analysoi ja vertaa tietoja tunnetut kuviot-apuohjelmista. Nämä kuvioita ei kuitenkaan yksinkertaisia allekirjoituksia. Ne määräytyvät monimutkaisia konepohjaisten oppimistekniikoiden algoritmeista, joita käytetään valtaviin tietojoukkoja kautta. Ne myös määräytyvät haittaohjelmien toiminnan varovainen analyysin asiantuntija analyytikot. Azure Tietoturvakeskus käyttää käytöspiirteen analytics tunnistavan vaarantuneen resurssien mukaan virtuaalikoneen lokit, VPN-laitteen lokit, kangasta lokit, kaatumisvedokset ja muista lähteistä. 

Lisäksi on korrelaatio ja muut signaalit tarkistavan tukevaa juuri markkinointikampanjan näyttöä. Tämän korrelaatiokertoimen voit tunnistaa tapahtumia, jotka ovat yhdenmukaisia vuotamiseen muodostettu ilmaisimet. Esimerkkejä:

- **Suspicious prosessin suorittamisen**: hyökkääjät käyttää useilla eri tavoilla suorittamaan haittaohjelmilta ilman tunnistus. Esimerkiksi luvaton saattaa antaa haittaohjelman on sama nimi kuin oikeutetut järjestelmätiedostoja, mutta nämä tiedostot sijoitetaan sijaintipaikkaan, käyttämällä nimeä, joka muistuttaa ole haittaa tiedostoon tai peite tiedoston tosi tunnisteen. Tietoturvakeskus mallit prosessit toiminnan ja näyttöjen käsitteleminen suorituskerran harha esimerkiksi seuraavia vianmäärityksen avulla.  
- **Piilotettu haittaohjelmia ja hyödyntämistä yrittää**: kehittyneitä haittaohjelman pystyy sopimattomin perinteinen antimalware tuotteet koskaan levylle kirjoittamista tai salaamaan ohjelmisto-osia, jotka on tallennettu levylle.  Kuitenkin tällaisten haittaohjelman voidaan havaita muistin analyysillä haittaohjelman on lähtevien jäljittää muistissa toimivat. Ohjelmiston kaatuu, kun kaatumisvedostiedosto sieppaa muistin osa kaatumisen aikaan.  Analysointi kaatumisvedos muistin mukaan Azure Tietoturvakeskus tunnistaa tekniikoita avulla voit hyödyntää ohjelmiston heikkouksien, luottamuksellisia tietoja ja kanssa-: ssä vaivihkaa jatkuvat vaarantuneen koneen ilman suorituskykyyn käyttämääsi laitteeseen.
- **Radan viereen varattu siirto ja sisäinen reconnaissance**: säilyttää vaarantuneen verkko- ja Etsi/korjuun arvokkaita tietojen hyökkääjät usein yrität siirtyä sivusuunnassa vaarantuneen tietokoneesta muiden samassa verkossa. Tietoturvakeskus valvoo prosessi ja kirjaudu sisään toimintoja jotta Tutustu yrittää laajentaminen verkkoon, kuten komennon suorituksen verkko-tutkitaan, ja tilin luettelointi hänen foothold.
- **Haittaohjelmien PowerShell-komentosarjojen**: PowerShell käytetään hyökkääjät suorittaa haitallisen koodin kohde näennäiskoneiden tarkoituksiin erilaisille. Tietoturvakeskus tarkastaa näyttöä epäilyttävistä tehtävän aktiviteetin PowerShell. 
- **Lähtevien kalastelu**: hyökkääjät kohteen usein cloud resursseja tavoitteena muita kalastelu käyttöön näiden resurssien avulla. Vaarantuneen näennäiskoneiden esimerkiksi ehkä voi käyttää Käynnistä palvelinten kalastelu muiden näennäiskoneiden vastaan, Lähetä ROSKAPOSTIA ja silmäily avoimet portit ja muissa Internetissä. Lisäämällä koneen learning verkkoliikennettä Tietoturvakeskus voi tunnistaa, kun lähtevä tietoliikenne ylittää sääntöön. ROSKAPOSTI, kyseessä Tietoturvakeskus myös numeroidulla epätavallisia sähköpostin liikenne kanssa Liiketoimintatieto-Office 365: ssä, onko postin todennäköisesti nefarious tai välillä markkinointikampanjan tulos.  

### <a name="anomaly-detection"></a>Poikkeavuuksista tunnistus

Azure Tietoturvakeskus käyttää myös poikkeavuuksista tunnistus uhkien tunnistamiseen. Käytöspiirteen analytics (joka määräytyy tunnetut kuviot suurten tietojoukkojen johdettu) vastoin poikkeavuuksista tunnistaminen on enemmän "muokattu" ja ohjeessa on perusaikataulujen, jotka liittyvät käyttöönoton. Koneen learning käytetään määrittämään käyttöönoton Normaali tehtävä ja sitten säännöt luodaan voit määrittää poikkeavien arvojen ehdot, jotka voivat aiheuttaa Suojaustapahtuma. Tässä on esimerkki:

- **Saapuva RDP/SSH palvelinten pakottaa kalastelu**: käyttöönoton voi olla varattu näennäiskoneiden, joissa on paljon kirjautumiset kunkin päivän ja näennäiskoneiden, joissa on hyvin vähän tai minkä tahansa kirjautumiset. Azure Tietoturvakeskuksessa voit määrittää nämä näennäiskoneiden perusaikataulun kirjautuminen tehtävän ja konepohjaisten oppimistekniikoiden voit määrittää, mikä on Normaali kirjautuminen tehtävän ulkopuolella. Jos kirjautumiset määrä tai kellonaika kirjautumiset ja josta kirjautumiset haluttuun paikkaan tai muut kirjautuminen liittyvät ominaisuudet eroavat merkittävästi perusaikataulun-ilmoitus voi johtua. Koneen learning määrittää uudelleen, mikä merkittäviä.

## <a name="continuous-threat-intelligence-monitoring"></a>Jatkuva uhkien liiketoimintatietojen seuranta

Azure Tietoturvakeskus toimii suojauksen oheis- ja tietojen tiede ryhmistä, joilla on jatkuvasti valvoa uhkien vaaka muutokset. Tämä vaihtoehto sisältää seuraavat hankkeita:

- **Uhkien liiketoimintatietojen seuranta**: Aikatietojen sisältää järjestelmiä ja ilmaisimet, vaikutukset suoritettavia neuvojen tai tulevan uhkien uhkien. Nämä tiedot jaetaan suojaus-yhteisön ja Microsoft valvoo jatkuvasti uhkien Liiketoimintatieto-syötteitä sisäisistä ja ulkoisista lähteistä.
- **Signaalin jakaminen**: tiedot-suojauksen ryhmät Microsoftin laaja portfolion cloud ja paikallisen palvelujen ja asiakkaan palvelinten päätepisteen laitteiden jaettu ja analysoidaan. 
- **Microsoftin security asiantuntijoiden**: jatkuva välitys Microsoftin ryhmistä, joilla on erityinen suojaus-kentät, kuten forensics toimii ja WWW-hyökkäyksen tunnistus kanssa.
- **Tunnistus säätäminen**: algoritmit suoritetaan vastaan todellisia asiakkaiden tietojoukot ja suojauksen tutkijoiden työskenteleminen vahvistamiseen tulokset asiakkaiden kanssa. TOSI ja EPÄTOSI positiivisten käytetään tarkentaa koneen learning algoritmeista.

Nämä yhdistetyn tehokkuutta culminate uudet ja parannetut tunnistuksia, jossa voit hyötyä välittömästi – ei voi tehdä tarvittavat toimenpiteet.

## <a name="see-also"></a>Katso myös
Tämän asiakirjan opit, kuinka Azure Tietoturvakeskus tunnistus ominaisuuksia käyttää. Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskus suunnittelu- ja käyttöoppaan](security-center-planning-and-operations-guide.md)
- [Hallinta ja Azure Tietoturvakeskuksessa suojausvaroitusten vastaaminen](security-center-managing-and-responding-alerts.md)
- [Suojausvaroitusten Azure Tietoturvakeskus tyypin mukaan](security-center-alerts-type.md)
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen kirjoituksia.
