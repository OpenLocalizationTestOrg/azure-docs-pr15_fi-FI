<properties
    pageTitle="Cortana liiketoimintatietojen ratkaisu mallin Playbook demand ennusteen sekä energiajärjestelmien | Microsoft Azure"
    description="Ratkaisu-malli, jonka Microsoft Cortana liiketoimintatietojen, joka auttaa ennuste demand energiajärjestelmien apuohjelman yritykselle."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana liiketoimintatietojen ratkaisu mallin Playbook Demand ennusteen sekä energiajärjestelmien  

## <a name="executive-summary"></a>Johdon yhteenveto  

Aikaisempien muutaman vuoden asioita Internet (IoT), Vaihtoehtoinen energiajärjestelmien lähteiden ja big datasta yhdistämisen jälkeen voit luoda suuria myyntimahdollisuuksia apuohjelma ja energiajärjestelmän toimialueen. Samanaikaisesti apuohjelma ja koko energiajärjestelmien alan tullut kulutus litistämistä uudempaan parempia tapoja hallita niiden käyttöä energiajärjestelmien kuluttajien kanssa. Näin ollen smart ruudukon yritykset ja apuohjelman ovat erinomainen hyödyntämisestä ja uusia itsensä sinun tarvitsee. Lisäksi monia power ja apuohjelman ruudukko on tulossa vanhentuneet ja hyvin kallista säilyttää ja hallita. Edellisen vuoden ryhmän on työstänyt sitoumukset energiajärjestelmien toimialueen määrä. Nämä työt aikana olemme on havaitsi missä apuohjelmia tai valmistajille (riippumaton ohjelmistotoimittajat) on ollut näyttöä kyselyjä ennusteiden tekeminen tulevien energiajärjestelmien demand usein. Nämä ennusteiden toistaminen tärkeä nykyisten ja tulevien käyttävät yritykset ja on tullut foundation eri Käytä tapauksissa. Näitä ovat lyhyen ja pitkän ajan power kuormituksen ennuste, myynti, kuormituksen tasaamisen, ruudukon optimointi jne. Big datasta ja Advanced Analytics (AA) menetelmiä esimerkiksi tietokoneen Learning (ML) ovat tärkeimmät käyttöönottajia tuottavat tarkkoja ja luotettavia ennusteiden.  

Tämä playbook olemme koonneet työ- ja analytical onnistuneen kehittäminen tarvittavat ohjeet ja energiajärjestelmien demand käyttöönoton ennuste ratkaisu. Ehdotettu ohjeita auttaa apuohjelmia, tietojen tutkijoiden ja tietojen insinöörien vahvistamiseksi täysin operationalized, pilvipohjainen, demand ennusteiden ratkaisuja. Yrityksille, joilla vain mahdollisesti niiden big datasta ja kehittyneen analyysin matkan tällainen ratkaisu kuvaa alkuperäinen lähde-niiden pitkään smart ruudukon strategia.

>[AZURE.TIP] Lataa kaavio, jossa esitetään yleiskatsaus arkkitehtuuri tätä mallia, on artikkelissa [Cortana liiketoimintatietojen ratkaisu mallin arkkitehtuuri demand ennusteen sekä energiajärjestelmien](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Yleiskatsaus  

Tässä asiakirjassa kerrotaan business, tiedot ja teknisessä käytön Cortana liiketoimintatietojen ja -tietyn Azure koneen Learning (Sovellustietoliikenneliittymät) käyttöönoton sekä energiajärjestelmien ennusteiden ratkaisujen käyttöönoton. Asiakirja sisältää kolme pääosaa:  

1. Yrityksen tietoja  
2. Tietojen ymmärtäminen  
3. Tekniset

**Yrityksen tietoja** -osan ympärille yhden on tietoja ja ota huomioon ennen tekeminen sijoitus päätös business kuvasuhde. Kuvataan, kuinka voin business ongelma kätevästi varmistaaksesi, että ennakoivan analyysin ja konepohjaisten oppiminen ovat varmasti voimassa ja käytettävissä. Asiakirjan tarkemmin tässä artikkelissa kerrotaan koneen oppiminen ja miten niitä käytetään energiajärjestelmien ennusteiden ongelmien korjaamiseksi. Sen ympärille edellytykset ja Käyttötapaus pätevyys ehdot. Jotkin otoksen käyttää tapauksissa ja kokonaisuuden tilanteita, joissa on myös annettu.

Tiedot ovat tärkeimmät ainesosan, minkä tahansa konepohjaisten oppimistekniikoiden ratkaisu. Tämän artikkelin **Tiedot tietoja** -osassa kerrotaan tietyiltä tärkeitä tietoja. Sen ympärille sen tyyppistä tietoa, jota tarvitaan energiajärjestelmien ennusteiden, tietojen laadun mukaan ja tietolähteiden yleensä olemassa. Kerromme myös perustietoja käyttötavan valmisteleminen tietoihin liittyviä toimintoja, jotka vaikuttavat todella mallinnus-osa.

Asiakirjan kolmas osa koskee ratkaista **Tekniset** näkökohdat. Ominaisuuden tekniikka ja mallinnus ovat tärkeä osa tietojen tiede prosessi ja näin ollen käsitellään joitakin yksityiskohtaiset tiedot. Se peittää WWW-palvelut, jotka ovat tärkeitä ajoneuvon, ennakoivan analytics-ratkaisujen käyttöönoton cloud käsite. On myös jäsentää tyypillinen arkkitehtuuri operationalized lopusta loppuun-ratkaisun.

Asiakirja sisältää lisäksi aineistoa, joiden avulla voit saada toimialueen ja tekniikka.

On tärkeää muistaa, että olemme et aio kattaa tässä asiakirjassa tarkempaa tietojen tiede prosessin matemaattisia ja tekninen sen ominaisuuksia. Nämä tiedot löytyvät [Azure ML asiakirjat](http://azure.microsoft.com/services/machine-learning/) ja [blogit](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Kohdeyleisön   
Tämän asiakirjan kohdeyleisön on business- ja tekninen henkilöt, joille haluat tietämyksen ja tietoja tietokoneen opiskelun ratkaisuja ja miten ne käytetään erityisesti energiajärjestelmien ennusteiden toimialueen sisällä perusteella.

Tietoja tutkijoiden myös voi olla hyötyä lukemista tämä asiakirja ja myönnä paremman käsityksen siitä, korkean tason prosessi, joka ohjaa energiajärjestelmien ennusteiden ratkaisun käyttöönotto. Tässä yhteydessä se voidaan myös muodostaa hyvä perusaikataulun ja lisää aloituspiste yksityiskohtaiset ja advanced materiaali.

### <a name="industry-trends"></a>Teollisuuden trendejä  
Aikaisempien muutaman vuoden IoT, Vaihtoehtoinen energiajärjestelmien lähteiden ja big datasta yhdistämisen jälkeen voit luoda suuria myyntimahdollisuuksia apuohjelma-ja energiajärjestelmän. Samanaikaisesti apuohjelma ja koko energiajärjestelmien sektorit tullut kulutus litistämistä uudempaan parempia tapoja hallita niiden käyttöä energiajärjestelmien kuluttajien kanssa.

Monta smart energiajärjestelmien yritykset ja apuohjelma on on pioneeritutkimus [smart ruudukon](https://en.wikipedia.org/wiki/Smart_grid) ottamalla käyttöön useita tapauksia, jotka helpottavat käyttäminen ruudukon luomien tietojen käyttöä. Käytä usein työnkulkuihin sähköntuotannon ominaispiirteet: se et voi kumulatiivisen eikä tallennettujen varaa varaston. Näin on mikä on valmistettu on käytettävä. Apuohjelmat, jotka haluat luoda tehokkaampia täytyy ennuste virrankulutuksen yksinkertaisesti, joka antaa sen, että ne on suurempi mahdollisuus **Saldo toimituksen ja tarpeen**, koska näin estää energiajärjestelmien ainehävikkiä, **vähentää kasvihuonekaasujen kaasu päästöjen**ja ohjausobjektin kustannukset.

Kun puhelu kustannusten, käytössäsi on toisen projektinhallintaa, joka on hinta. Uusien ominaisuuksien kaupan välillä apuohjelmien power olet tuonut hyvien tarvitse **tulevat tarvittaessa ja tulevien hinta sähkö**. Tämä voi auttaa yritykset määrittää niiden tuotannon asemat.

Kun Käytämme sanan "Automaattinen", on todella viitata ruudukon, voit tietoja ja tee sitten ennusteiden. Se voi ennakoida kulutus sekä **ennustaa tilapäinen liikaa tilanteissa ja muuttaa automaattisesti sen**kausiluonteisista muutokset. Mukaan etäyhteyden sääteleviä kulutus (nämä smart metriä avulla), voidaan käsitellä lokalisoitu liikaa tilanteissa. **Korrelaatio ensin ja sitten visuaalisessa**ruudukon tekee itse entistä tehokkaammin ajan kuluessa.

Tämän asiakirjan tiettyyn perhe, jotka kattavat koskevia tulevaisuudessa, käytä palvelupyyntöjen keskitytään muiden lyhyt termi ja pitkään energiajärjestelmien tarvittaessa. Olemme olet käyttänyt muutaman kuukautta alueilla ja on saatu joitakin ja taidot, joka mahdollistaa us alan arvosana tulokset. Käyttää muissa tapauksissa voidaan kattaa myös asiakirjan lähitulevaisuudessa.

## <a name="business-understanding"></a>Yrityksen tietoja

### <a name="business-goals"></a>Tavoitteet
**Energiajärjestelmien esittely** lähinnä osoittamaan tyypillinen ennakoivan analyysin ja konepohjaisten oppimistekniikoiden ratkaisu, joka voidaan ottaa käyttöön hyvin lyhyt aika kehyksessä. Tarkemmin sanottuna Microsoftin nykyisen keskitytään energiajärjestelmien demand ennusteen ratkaisujen ottaminen käyttöön niin, että sen liiketoimintatulokset voi nopeasti toteutunut ja hyödyntää yhteydessä. Tämä playbook tiedot auttavat asiakkaan, joiden avulla voit suorittaa seuraavat tavoitteet:
-   Lyhyt aika-arvon tietokoneen opiskelun perusteella ratkaisu
-   Mahdollisuus Laajenna koekäytön Käyttötapaus käytössä muissa tapauksissa tai laajempi käyttöalue niiden tarpeellinen perusteella
-   Päästä nopeasti Cortana liiketoimintatietojen Suite tuotteen tiedot

Nämä mielessä tavoitteista ja tämän playbook pyritään business ja teknisen tietämyksen, jotka auttavat saavuttaa nämä tavoitteet.

### <a name="power-load-and-demand-forecasting"></a>Power ladataan ja tarvittaessa ennusteiden
Energiajärjestelmien alalla voi olla mitä demand-ennusteiden auttaa kriittinen liiketoiminnan ongelmien monella tavalla. Itse asiassa demand ennusteiden voidaan pitää core käyttäminen usein alan perusta. Yleensä kahdentyyppisiä energiajärjestelmien demand ennusteiden oletetaan: lyhyt termi ja pitkällä aikavälillä. Kullekin voivat toimia eri tarkoitukseen ja hyödyntää sisäkkäistä. Tärkein kahden välinen ero on Ennustaminen horisontti, eli ajanjakson, jossa on ennuste tulevaisuuteen.

#### <a name="short-term-load-forecasting"></a>Lyhyt termin kuormituksen Ennustaminen
Energiajärjestelmien demand puitteissa lyhyt termin ladata ennusteiden (STLF) on määritetty koostetun kuormituksen, joka on ennustettu lähitulevaisuudessa eri osia ruudukon (tai ruudukossa kokonaisuutena). Tässä yhteydessä lyhyt termi on määritetty solualueessa 1 tunti 24 tunnin ajanjakso. Joissakin tapauksissa horisontti 48 tuntia on myös mahdollista. STLF on hyvin yleisiä toiminnallisia Käyttötapaus ruudukon. Seuraavassa on esimerkkejä STLF perustuva Käytä tapauksissa:
-   Toimituksen ja tarpeen tasaamisen
-   Power kaupan tuki
-   Market tekeminen (asetus power hinta)
-   Ruudukon toiminnallisia optimointi
-   [Tarvittaessa vastaus](https://en.wikipedia.org/wiki/Demand_response)
-   Huippu demand Ennustaminen
-   Puolen kysynnänhallinta
-   Kuormituksen tasaaminen ja ylikuormittaa estäminen
-   Pitkäkestoisen kuormituksen Ennustaminen
-   Vika ja poikkeavuuksista tunnistus
-   Huippu supistaminen ja tasaaminen 

STLF malliin perustuvat enimmäkseen lähelle aiemman (viimeinen päivä tai viikko) tiedot ja käytä ennustettu lämpötilan kuin tärkeä predictor. Tarkka lämpötila seuraava tunnin ennuste hankkiminen ja määrittäminen 24 tuntia on jatkossa pienempi hankala nyt päivää. Nämä ovat vähemmän luottamuksellisia kausiluonteisista kuvioiden tai pitkään kulutus trendejä.

SLTF ratkaisut myös todennäköisesti Luo tekstinsyöttö puhelut (palvelupyyntöjä) määrää, koska ne on kutsuttu tunnissa ja joissakin tapauksissa jopa suurempi korkojakso kanssa. Kannattaa myös hyvin yleisiä näet nämä missä kunkin yksittäisen ala-asema tai muuntotoiminnon esitetään erillinen mallina ja näin ollen tekstinsyöttö pyynnöt äänenvoimakkuuden ovat vieläkin toimenpiteet.

#### <a name="long-term-load-forecasting"></a>Pitkäkestoisen kuormituksen ennusteiden
Tavoitteena on pitkä termin kuormituksen ennusteiden (LTLF) on ennuste power demand ja ajanjakso, väliltä 1 viikko useita kuukautta (tai joissakin tapauksissa vuosien määrä). Tämä horisontti alue on lähinnä sovellu suunnittelu ja sijoitus käyttötapauksiin.

Pitkään tilanteessa on tärkeää laadukkaita tietoja, joka kattaa aikaväli useita vuoden (vähintään kolme vuotta). Näitä malleja yleensä poimia kausivaihtelu kuviot historiatiedoissa ja käyttää ulkoisia predicators, kuten sää- ja siitä kuviot nimellä.

On tärkeää, että sitä kauemmin Ennustaminen horisontti on, mitä vähemmän tarkkoja ennuste voidaan selkeyttää. Tämän vuoksi on tärkeää tuottaa todellinen ennuste, joka mahdollistaa ihmisiin mahdollista variaation niiden suunnittelun siirtymistä ottaa huomioon sekä joitakin LUOTTAMUSVÄLI väliajoin.

Koska LTLF kulutus skenaario enimmäkseen suunnittelu, Odotamme paljon pienempi tekstinsyöttö asemat (verrattuna STLF). Olemme yleensä nähdä nämä ennusteiden upottaa visualisoinnin työkaluja, kuten Excel- tai PowerBI ja käynnistää manuaalisesti käyttäjän.

### <a name="short-term-vs-long-term-prediction"></a>Lyhyt termi ja pitkä termin Ennustaminen
Seuraavassa taulukossa on Vertailtu STLF ja LTLF osin tärkeimmät ominaisuudet:

|Määrite|Lyhyen aikavälin kuormituksen ennuste|Pitkäkestoisen kuormituksen ennuste|
|---|---|---|
|Ennuste horisontti|Valitse 1 tunti 48 tuntia|1 vähintään 6 kuukautta|
|Tietoja rakeisuuden|Tunnin välein|HOURLY tai päivittäin|
|Tavallisesti tapauksissa|<ul><li>Tarvittaessa/toimituksen tasaamisen</li><li>Valitse tunti Ennustaminen</li><li>Tarvittaessa vastaus</li></ul>|<ul><li>Pitkäkestoisen suunnittelu</li><li>Ruudukon Resurssien suunnittelu</li><li>Resurssisuunnittelu</li></ul>|
|Tyypillinen predictors|<ul><li>Päivä- tai viikkonäkymässä</li><li>Kellonaika</li><li>Tunnin välein lämpötila</li></ul>|<ul><li>Vuoden kuukausi</li><li>Kuukauden päivä</li><li>Pitkällä aikavälillä lämpötilan ja siitä</li></ul>|
|Historiallinen tietoalue|Kahden tai kolmen vuoden eurolla tiedot|5-10 vuotta eurolla tiedot|
|Tyypillinen tarkkuus|MAPE * 5 % tai pienempi|MAPE * 25 % tai pienempi|
|Ennuste korkojakso|Tunnin tai 24 tunnin välein|Kun kuukausittain tai neljännesvuosittain vuosittain valmistettu|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – keskiarvo keskimääräinen prosentti virhe

Kun näkevät tämän taulukon, on aivan tärkeää erottaa lyhyt ja ennusteiden skenaariot ne edustavat eri yrityksen tarpeet ja voi olla eri käyttöönotto- ja kulutus kuviot pitkällä aikavälillä.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Esimerkki Käyttötapaus 1: eSmart Systems – liikaa optimointi
[Smart ruudukon](https://en.wikipedia.org/wiki/Smart_grid) tärkeä tehtävä on dynaamisesti jatkuvasti optimointi ja säädä muuttaminen kulutus kuviot. Virrankulutuksen voi heiketä lyhytkestoinen muutokset, jotka aiheutuvat pääasiassa lämpötilan vaihteluita (*kuten*Lisää power käytetään ilma ehdon tai lämmitys). Yhtä aikaa virrankulutuksen myös vaikuttaa pitkään trendejä. Näitä ovat kausivaihtelu tehosteita, kansallisia juhlapäivät, pitkään kulutus kasvu ja jopa taloudellisen tekijöistä, kuten kuluttaja indeksin, oil hinnan ja BKT.

Käytä tässä tapauksessa [eSmart](http://www.esmartsystems.com/) määrittää pilvipohjainen ratkaisu, joka mahdollistaa ennakoimista liikaa tilanne minkä tahansa määritetyn ala-asema ruudukon vientikapasiteetti käyttöönotto. Erityisesti eSmart haluaisit käyttää tunnistavan syöttöasemien, jotka todennäköisesti ylikuormituksen seuraavan tunnin kuluessa, joten heti toiminnon voitaisiin ryhtyä välttää ja ratkaista tilanne.

Tarkkoja ja suorittamiseen nopeasti tekstinsyöttö edellyttää kolmen ennakoivan tietomallin soveltaminen:
-   Termin malli, jonka avulla Ennustaminen virrankulutuksen kunkin ala-asema, seuraavan muutaman viikon tai kuukauden aikana
-   Lyhyen aikavälin malli, jonka avulla liikaa tilanne annetun ala-aseman seuraavaan tunnin aikana Ennustaminen
-   Lämpötila-malli, joka sisältää ennusteiden tulevien lämpötilan päälle useita skenaarioita

Pitkään mallin tavoitteena on Järjestä syöttöasemille niiden vientikapasiteetti ylikuormituksen (annettu niiden power siirto kapasiteetti) aikana seuraavan viikon tai kuukauden mukaan. Näin lyhyen luettelon syöttöasemien, jota käyttää lyhytkestoinen tekstinsyöttö syöte luomista. Lämpötila on tärkeää predictor, pitkään mallin, ei tarvitse jatkuvasti tuottaa usean skenaarion lämpötilan ennusteiden ja syötteen ne syötteeksi pitkään mallin. Lyhyen aikavälin ennuste sitten käynnistää ennustaa mitä ala-asema todennäköisesti ylikuormituksen seuraavaan tunnin päälle.

Lyhytkestoinen ja pitkään-malleissa on otettu käyttöön yksitellen kohti kunkin ala-asema. Vuoksi näitä malleja käytännön suorittaminen edellyttää laaja tiedonsiirron. Saada tekstinsyöttö tarkempiin lyhyessä eritellympiä malli on omistautunut kunkin vuorokauden tuntia. Näiden mallien suoritetaan tunnissa ja valmis suorittamisen riittävästi aikaa toimia ehkäisevät tarvittaessa ja vastata muutaman minuutin kuluessa. Tämän sivustokokoelman mallien päivitetään jatkuvasti käyttämällä säännöllisiä uudelleenkoulutuksen uusimpia tietoja.

Lisätietoja tämän Käyttötapaus löytyy [tähän](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Palvelupyynnön pätevyys ehdoilla – edellytykset
Cortana yritystieto ensisijainen voimakkuuden on tehokas mahdollisuus käyttöönotto ja Skaalaa koneen learning keskitettyä ratkaisuja. Se on suunniteltu ennusteiden, joka suoritetaan samanaikaisesti tuhat-tukeen. Se Skaalaa automaattisesti muuttaminen kulutus kuvion mukaiseksi. Ratkaisussa vuoksi keskitytään tarkkuutta ja laskennallinen suorituskykyä. Esimerkiksi yrityksen apuohjelma on kiinnostunut tuottavat tarkkoja energiajärjestelmien demand ennuste seuraavaan tunnin, sekä kunkin vuorokauden tuntia. Toisaalta, emme vähemmän kiinnostunut vastaaminen kysymystä siitä, miksi tarve ennustettujen on sellaisena kuin se on (mallin itse huolehtia,).

Näin ollen tärkeää, että kaikki käyttötapauksiin ja yrityksen ongelmat voidaan tehokkaasti ratkaista koneen learning avulla.

Cortana liiketoimintatietojen ja tietokoneen oppiminen voi olla erittäin tehokkaasti ratkaiseminen annetun business, kun seuraavat ehdot täyttyvät:
-   Käsi business-ongelma on **ennakoivan** laatu. Ennakoivan Käytä palvelupyynnön esimerkki on apuohjelman yritys, joka haluat ennustaa power kuormitus annetun ala-aseman seuraavaan tunnin aikana. Toisaalta analysointia ja luokittelu historiallisten demand ohjaimet olisi **kuvaava** laatu ja näin ollen vähemmän käytettävissä.
-   Ei poista **toiminto polun** otettava, kun tekstinsyöttö eivät ole käytettävissä. Korrelaatio-ala-asema liikaa seuraavaan tunnin aikana voit esimerkiksi käynnistää ennakoiva toiminnon pienentämisestä kuormituksen, joka liittyy kyseiseen ala-asema ja estää näin mahdollisesti liikaa.
-   Käyttötapaus edustaa **tyypillinen ongelman tyyppi** esimerkiksi, että kun ratkaista se voi ensin ratkaiseminen muiden samalla käyttötapauksiin.
-   Asiakkaan määrittää **määrää ja laatua koskevat tavoitteet** osoittamaan onnistuneen ratkaisun käyttöönotto. Esimerkiksi hyvä määrän tavoitteen energiajärjestelmien demand ennuste olisi tarkkuutta raja-arvon (*kuten*enintään 5 % virhe sallitaan) tai kun ennakoimista ala-asema ylikuormituksen sitten tarkkuus (tosi positiivisten kurssi) ja peruuttaminen (tosi positiivisten laajuus) on oltava annetun kynnysarvo. Nämä tavoitteet olisi johdettu asiakkaan tavoitteet.
-   Ei poista **integrointi skenaarion** yrityksen business työnkulussa. Esimerkiksi ala-asema kuormituksen ennuste voidaan yhdistää, jotta liikaa estäminen toimintoja ruudukko-hallintakeskus.
-   Asiakkaan on valmis käyttämään **tietoja ja riittävä laatu** tukea Käyttötapaus (Katso Lisää-kohdassa seuraava, **Tietojen laadun**tämän playbook).
-   Asiakkaan yleisstrategian cloud keskitettyä tietojen arkkitehtuuri tai **koneen pilvipohjainen Koulujen**, mukaan lukien Azure ML ja Cortana liiketoimintatietojen Suite muita osia.
-   Asiakas on valmis muodostaa **lopusta loppuun-Tietovuo** kyseisen tilat tietojen toimittamisen pilveen kanssa jatkuvasti, kyselyjä ja on ovat, **operationalize** ratkaisu.
-   Asiakas on valmis **tälle resurssien** kuka aktiivisesti käytetään alkuperäisen kokeilun aikana niin, että tietämyksen ja ratkaisu omistajuuden voidaan siirtää asiakkaan onnistumiseen yhteydessä.
-   Asiakkaan resurssi on oltava **taitoja tietojen professional**-mieluiten tietojen Tiedemies.

Yllä ehtojen perusteella Käyttötapaus pätevyys merkittävästi parantaa Käyttötapaus onnistumista myönnettävän ja muodostaa hyvä beachhead myöhempää käyttöä varten tapauksissa toteuttamisesta.

### <a name="cloud-based-solutions"></a>Pilvipohjainen ratkaisut
Azure Cortana liiketoimintatietojen Suite on integroitu ympäristössä, joka sijaitsee pilveen. Cloud-ympäristössä kehittyneen analyysin-ratkaisun käyttöönotto pitää merkittäviä etuja yrityksille ja samanaikaisesti saattaa tarkoittaa suuri muuta paikallinen IT-ratkaisujen silti käyttävät yritykset. Energiajärjestelmien alalla ei poista trendi pilveen toimintoja asteittain siirron. Tämä suuntaus siirtyy valmiina valmiina smart ruudukon kehittämistä sekä kuin edellä kuvatun **Alan trendejä**. Kun tämä playbook on kohdistettu energiajärjestelmien toimialueen pilvipohjainen ratkaisu, on tärkeää kerrotaan, mitä etuja sekä muita huomioon otettavia seikkoja on pilvipohjainen-ratkaisun käyttöönotto.

Pilvipohjainen ratkaisu suurimmista etuna on esimerkiksi kustannukset. Ratkaisuksi käyttää cloud käyttöön osia ei ole jaettu kustannukset tai myytyjen tuotteiden kustannusten (myytyjen tuotteiden kustannukset)-osan kustannukset liittyy. Tämä tarkoittaa sijoittamaan laitteiston ja ohjelmiston IT-ylläpito ei tarvita, ja näin ollen ei vähentämään liiketoiminnan riskejä.

Toinen tärkeitä etu on pilvipohjainen ratkaisuja tukiyhteyksiä kustannukset rakenne. Pilvipohjainen palvelimiin selvitetään tai tallennustilan voidaan ottaa käyttöön ja skaalata juuri-tarvittaessa välein. Tämä vastaa kustannukset tehokkuuden etuna pilvipohjainen ratkaisu.

Lopuksi ei tarvetta investointien IT ylläpito tai tulevien infrastruktuurin kehitystä, kun kaikki tämä on pilvipohjainen tarjoaa osa. Sekä Cortana liiketoimintatietojen Suite sisältää parhaan luokan Services-palveluissa ja sen suunnittelu säilyttää kehittyville. Uusia ominaisuuksia, osien ja ominaisuuksia esitellään jatkuvasti ja kehityttävä.

Yritys, joka käynnistyy heti sen siirtymän pilvipalveluun emme on erittäin suositellaan tulevat asteittain lähestymistavan mukaan käyttöönoton cloud-siirron suunnittelu. Microsoft uskoo, että apuohjelmien ja energiajärjestelmän toimialueen yritykset, käytä palvelupyynnöt, jotka käsitellään tämän playbook edustavat erinomainen mahdollisuuden pilottivaiheisiin ennakoivan analytics-ratkaisujen pilveen.

#### <a name="business-case-justification-considerations"></a>Yrityksen palvelupyynnön tasaus huomioon otettavia seikkoja
Monissa tapauksissa asiakkaan voi olla kiinnostuneita tekemään business peruste annetun käyttötapaus, jossa pilvipohjainen ratkaisu ja tietokoneen Learning ovat tärkeitä osia. Toisin kuin paikallinen ratkaisu, jos kyseessä on pilvipohjainen ratkaisu jaettu kustannukset-komponentti on pienin ja suurin osa kustannukset-osat liittyvät todelliset käyttö. Sen tullessa käyttöönotto energiajärjestelmien ennusteiden ratkaisu Cortana liiketoimintatietojen Suite palveluihin voi integroida yksittäisen yleiset kustannukset rakenteen kanssa. Esimerkiksi tietokantoja (*kuten*SQL Azure) avulla voidaan tallentaa perustietoja ja sitten todellinen ennusteiden Azure ML käytetään isännöimiseen Ennustaminen palvelut. Tässä esimerkissä rakenteen kustannukset voivat olla tallennustilan ja komponentit.

Toisaalta yksi pitäisi olla ymmärtää toimii ennusteiden (lyhyemmällä tai pidemmällä termin) energiajärjestelmien-demand liiketoiminta-arvoa. Itse asiassa on tärkeää kunkin ennusteen toiminnon liiketoiminta-arvoa. Esimerkiksi tarkasti ennusteiden power ladataan seuraavan 24 tunnin estää overproduction tai voit estää Osastollasi ruudukon ja tämä on niiden kannalta taloudellisten säästöjen päivittäin.

Tavallisen kaavan laskemisen demand taloudellisen tilanteen etuna ennuste ratkaisu olisi: ![Basic kaavan laskeminen demand taloudellisen hyödyn ennuste ratkaisu](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Koska Cortana liiketoimintatietojen Suite sisältää tukiyhteyksiä hinnoittelu mallin, ei ole tarpeen, ettei kiinteät kustannukset-osan seuraava kaava. Tämä kaava voidaan laskea päivittäin, kuukausittain tai vuosittain välein.

Nykyinen Cortana liiketoimintatietojen ohjelmistopaketissa ja Azure ML hinnat suunnitelmien löytyy [tähän](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Ratkaisuprosessin kehittäminen
Ennusteiden ratkaisu liittyy yleensä 4 vaiheiden, mikä on Tee energiajärjestelmien-tarvittaessa development-jakson käyttö pilvipohjainen Technologies-tuote ja palveluja Cortana Liiketoimintatieto-ohjelmiston.

Seuraavassa kaaviossa on esitetty näin:

![Smart ruudukon kehä](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Kohta on kuvattu tämän 4 vaihetta:

1.  **Tietojen kerääminen** – kehittyneen analyysin mukaan kaikki ratkaisu perustuu tietojen (katso **Tietoja tietoja**). Erityisesti jos ennakoivan analytics ja ennusteiden, emme luottavat meneillään, dynaaminen tietojen kulun. Kyseessä energiajärjestelmien demand ennusteiden, näitä tietoja voi Orpotermit suoraan smart metriä tai on jo koostettava paikallinen tietokanta. Olemme luottavat myös muista ulkoisista lähteistä, kuten sää- ja lämpötilan tietoja. Tämän jatkuvaa vuon tietojen on oltava koordinoitu, ajoitettu, ja tallennettu. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (SYÖTTÖ) on Microsoftin tärkeimmät workhorse, joiden avulla voit suorittaa tämän tehtävän.
2.  **Mallintaminen** – tarkkoja ja luotettavia, forecasts, yksi on kehittää (junassa) ja ylläpitää hyvien mallin tekee historiatiedoissa käyttö ja purkaa tietojen merkityksellinen ja ennakoivan kuviot. Alueen koneen Learning (ML) on on kasvava nopeasti säännöllisesti kehittämisen aikana monimutkaisemman algoritmien kanssa. Azure ML Studio on hyvä käyttäjäkokemus, jonka avulla voit hyödyntää mahdollisimman Lisäasetukset ML algoritmeista, työ-työnkulun sisällä. Työnkulun on esitelty intuitiivisen vuokaavio, ja se sisältää tietojen valmisteleminen, toiminto poiminnan, mallinnus ja mallin arviointi. Käyttäjä voi tuoda satoja erilaisia malleja, jotka sisältyvät tässä ympäristössä. Tässä vaiheessa loppuun mennessä tietojen Tiedemies on työskentelymallia, joka on täysin arvioitu ja valmis käyttöönottoa varten.

    Seuraavassa kaaviossa on tyypillinen työnkulun Esimerkki:

    ![Työnkulun mallinnus](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Käyttöönoton** – työskentelyä mallin käytettävissä, seuraava vaihe on käyttöönotto. Tätä mallia muunnetaan verkkopalveluun, joka paljastaa RESTful-Ohjelmointirajapinta, joka voidaan samanaikaisesti kutsua eri kulutus asiakkaiden Internetin välityksellä. Azure ML on helppo tapa otat käyttöön mallin suoraan Azure ML Studio yhdellä napsautuksella hiiren napsautuksella. Koko käyttöönottoprosessin tapahtuu Näytä lisäasetukset. Tämä ratkaisu automaattisesti skaalata tarvittavat kulutus mukaiseksi.

4.  **Kulutus** – tämä vaihe on todella Tee avulla Ennustaminen mallin tuottaa ennusteiden. Kulutus voidaan perustuva käyttäjä-ohjelmasta (*esimerkiksi*, raporttinäkymät-ikkunan) tai suoraan toiminnallisia järjestelmästä, kuten vaatimus/toimituksen tasaamisen järjestelmän tai ruudukon optimointi ratkaista. Käytä useita tapauksia voidaan perustuva yhden mallista.

## <a name="data-understanding"></a>Tietojen ymmärtäminen
Jälkeen kattava business näkökohdat (katso **Yrityksen tietoja**) ennusteiden ratkaisu energiajärjestelmien-demand on nyt Keskustele tieto-osaa. Mikä tahansa ennakoivan analytics-ratkaisun on riippuvainen luotettavat tiedot. Ennusteen energiajärjestelmien demand, emme luottavat historiallisten kulutus tietojen eri tasojen määrää. Historiallinen tietoja käytetään raaka. Se tehdään varovainen analyysin, jossa tiedot Tiedemies tunnistaa predictors (tunnetaan myös nimellä ominaisuudet), joka voidaan lisätä malliin, joka ilmestyy luo tarvittavat ennusteiden.

Tässä osassa loput emme kuvataan eri vaiheet ja huomioon otettavia seikkoja tietoja tiedot ja miten voit noutaa sen käytettävissä olevaa lomaketta.

### <a name="the-model-development-cycle"></a>Mallin kehittäminen kehä
Tuottavat hyvä mallien edellyttää, että jotkin varovainen valmistelu ennustamista varten ja suunnittelu. Useita vaiheita prosessi mallinnus jakamiseen ja keskitytään vaihe kerrallaan voi parantaa huomattavasti koko prosessin tulos.

Seuraavassa kaaviossa on kuvattu, miten mallinnus-prosessi voi voidaan jakaa useita vaiheita:

![Mallin kehittäminen kehä](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Kun näkevät jakson koostuu kuusi:
-   Ongelma muodostamisessa
-   Tietoja nieltynä ja tietojen tarkasteluun
-   Tietojen valmisteleminen ja ominaisuus-tekniikka
-   Mallinnus
-   Mallin arviointi
-   Kehittäminen

Tässä osassa loput on kuvaavat yksittäisiä vaiheita ja kohteiden huomioitavia kussakin vaiheessa.

### <a name="problem-formulation"></a>Ongelma muodostamisessa
Emme voi harkitse kannalta vaiheessa yksi on otettava ennen minkä tahansa ennakoivan analytics-ratkaisun toteuttaminen ongelma-muodostamisessa. Seuraavassa on muuntaminen business ongelma ja Hajota sen tiettyjä osia, joita käyttämällä tietoja ja menetelmiä mallinnus voidaan ratkaista. Se on hyvä muodostetaan ongelma kysymyksiä, vastata haluamme sarjana. Seuraavassa on mahdollista joihinkin kysymyksiin, joita voi olla laajuus sekä energiajärjestelmien demand ennusteiden:
-   Yksittäisen ala-asema odotettu kuormitus ominaisuudet seuraavan tunti tai päivä?
-   Mitä milloin päivän oma ruudukon kohdata piikin demand?
-   Miten todennäköisesti on oma ruudukon ylläpitää odotettu huippu-kuormituksen?
-   Kuinka paljon power power aseman Luo kullekin vuorokauden tuntia aikana?

Näihin kysymyksiin koskevan pystyy keskittyminen oikean tietojen hakeminen ja toteuttaminen ratkaisun, joka on täysin kohdistettu business ongelman mukaan. Lisäksi on voit määrittää joitakin keskeisiä arvot, jotta arvioida mallin. Esimerkiksi kuinka tarkkoja ennuste olisi ja mikä on solualue, joka on yhä voi hyväksyä yrityksen?

### <a name="data-sources"></a>Tietolähteet
Nykyaikainen smart ruudukon kerää tietoja eri osat ja -osien ruudukon. Nämä tiedot edustaa toimintoa eri ominaisuuksia ja käyttö power ruudukon. Ennuste energiajärjestelmien demand alueessa emme ovat rajoittaminen tietolähteitä, jotka kuvastavat todellinen tarve keskusteluun. Tärkeää tietolähteen energiajärjestelmien kulutus ovat smart metriä. Apuohjelmien maapallon ympärille nopeasti käyttöön smart metriä niiden kuluttajille. Smart metriä tallentaminen todellinen virrankulutuksen ja välittää jatkuvasti takaisin apuohjelman yrityksen tiedoista. Tietojen kerääminen ja lähettää takaisin kiinteällä aikavälillä, 5 minuutin välein väliltä 1 tunti. Monimutkaisemman smart metriä on ohjelmoitu etäyhteyden ja saldo toteutuneen sisällä koti. Smart mittarin tiedot suhteellisen luotettava ja sisältää aikaleiman. Joka on tärkeää ainesosan ennuste tarvittaessa. Mittarin tiedot voidaan koota (yhteen ylöspäin) tasolla ruudukon topologian: *muuntotoiminnon, ala-asema, alue*. Olemme Valitse luominen Ennustaminen mallin tarvittavat koostamistaso. Esimerkiksi jos apuohjelman yrityksen haluat ennusteiden tulevien kuormituksen kunkin sen ruudukon syöttöasemien sitten kaikki metriä tiedot voidaan yhdistetään kunkin yksittäisen ala-asema ja käyttää syötteeksi Ennustaminen mallin. Smart metriä viitataan sisäinen tietolähteeksi.

Luotettavan energiajärjestelmien demand ennusteen ovat riippuvaisia myös muita ulkoisiin tietolähteisiin. Tärkeä tekijä, joka vaikuttaa virrankulutuksen on sään tai tarkemmin lämpötila. Historiatietoja näyttää vahva korrelaatio ulkopuolelta lämpötilan ja virrankulutuksen. Kuuma kesä päivän aikana kuluttajille tehdä käyttää niiden ilmastointilaitteet ja talvi virta lämmitys-järjestelmiä aikana. Historiallinen lämpötilojen ruudukon sijainnissa luotettavasta lähteestä siis avain. Lisäksi on myös luottavat tarkkoja ennuste lämpötilan predictor, virrankulutuksen nimellä.

Muut ulkoisiin tietolähteisiin avulla voi myös muodostaminen energiajärjestelmien demand ennusteen mallit. Nämä voivat sisältää pitkällä aikavälillä siitä muutokset ja taloudellisia indeksit (*kuten*BKT). Tässä asiakirjassa on eivät sisällä näitä muista tietolähteistä.

### <a name="data-structure"></a>Tietorakenne
Haluamme jälkeen tunnistaminen tarvittavat tietolähteitä, voit varmistaa, että tietoja, jotka on kerätty sisältää oikeat tiedot-ominaisuuksia. Luonnissa luotettava demand ennusteen malli on on varmistettava, että kerättyjä tietoja sisältää tietoja elementtejä, jotka auttavat ennustaa tulevat tarvittaessa. Seuraavassa on joitakin basic perustietoja tietorakenne (rakenne) koskevat vaatimukset.

Perustietoja koostuu rivejä ja sarakkeita. Kunkin mitta esitetään lukuna yhden tietorivin. Kukin rivi sisältää useita sarakkeita (tunnetaan myös nimellä ominaisuuksia tai kentät).

1.  **Aikaleiman** – aikaleima-kenttä vastaa kun mitta tallennettiin ajan. Se on noudatettava yleisiä pvm. / klo-tiedostomuodoissa. Päivämäärän ja kellonajan osat tulisi olla mukana. Useimmissa tapauksissa ei ole tarpeen, jos sisäänkirjautuminen on tallennettava rakeisuuden toisen tason tähän päivään. On tärkeää määrittää aikavyöhykkeen, jossa tiedot on tallennettu.
2.  **Mittarin tunnus** - kentän tunnistaa mittarin tai mitta-laitteessa. Se on categorical muuttuja ja voivat olla numeroiden ja merkkien yhdistelmä.
3.  **Kulutus arvon** – tämä on toteutuneen annetun päivämäärä ja kellonaika-palvelussa. Kulutus voidaan mitata kWh (kilowatt-hour) tai jokin muu ensisijaiset yksiköt. On tärkeää muistaa, että mittayksikkö täytyy pysyä yhdenmukaisia koko kaikki tiedot mitat. Joissakin tapauksissa kulutus voit toimitetun yli 3 power vaiheet. Tässä tapauksessa emme on kerätä riippumaton kulutus vaiheet.
4.  **Lämpötila** – lämpötila kerätään yleensä riippumaton lähteestä. Olisi kuitenkin olla yhteensopiva kulutustiedot. Sisällytettävä aikaleima yllä olevien ohjeiden mukaisesti, joka sallii synkronoitavaksi toteutuneen tiedoilla. Lämpötila arvo voidaan määrittää Celsius-astetta tai Fahrenheit, mutta kannattaa pitää yhdenmukaisia yli kaikki mitat.
5.  **Sijainnissa:** Sijainti-kenttään liittyy yleensä paikka, johon lämpötilan tiedot on kerätty. Se voi esittää lukuna postinumero ja leveys-/ pituusasteet (lat tai pitkä)-muodossa.

Seuraavissa taulukoissa on hyvä kulutus ja lämpötilan tietomuoto Esimerkkejä:

|**Päivämäärä**|**Aika**|**Mittarin tunnus**|**Vaihe 1**|**Vaihe 2**|**Vaihe 3**|
|--------|--------|------------|-----------|-----------|-----------|
|7/1/2015|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|7/1/2015|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|7/1/2015|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Päivämäärä**|**Aika**|**Sijainti**|**Lämpötila**|
|--------|--------|-------------|---------------|
|7/1/2015|10:00:00|11242        |24.4           |
|7/1/2015|10:00:01|11242        |24.4           |
|7/1/2015|10:00:02|11242        |24.5           |

Näkevät yläpuolella, kuten tässä esimerkissä on 3 eri arvot käyttöön liittyvän 3 power vaiheet. Huomaa, että päivämäärä ja kellonaika-kentät on erotettu, mutta ne myös voidaan yhdistää yhteen sarakkeeseen. Tässä tapauksessa sijainti-sarakkeessa esitetään 5-numeroinen postinumero-muodossa ja lämpötilan celsiusaste-muodossa.

### <a name="data-format"></a>Tietojen muoto
Cortana liiketoimintatietojen Suite tukee yleisimmät tietojen muodoissa, kuten *CSV TSV, JSON*. On tärkeää, että tietomuoto pysyy yhdenmukaisia koko projektin elinkaaren.

### <a name="data-ingestion"></a>Tietoja nieltynä
Koska energiajärjestelmien demand ennuste on jatkuvasti ja usein ennustaa olemme on varmistettava, että perustietoja juoksutus tasainen luotettavaa tietoa nieltynä prosessin avulla. Nieltynä prosessi on takaa, että raaka tiedot ovat käytettävissä Ennustaminen prosessin tarvittavan ajan. Joka tarkoittaa, että tietojen nieltynä korkojakso on oltava suurempi kuin Ennustaminen korkojakso.

Esimerkki: Jos Microsoftin demand ennusteiden ratkaisu Luo uusi ennusteen 8:00 Suomen päivittäin sitten annettava varmistaa, että kaikki tiedot, jotka on kerätty viimeisen 24 tunnin aikana on poistettu kokonaan nautittuina tähän päivään kyseiseen pisteeseen ja jopa on sisältävät tiedot edellisen tunnin.

Jotta voit tehdä tämän Cortana liiketoimintatietojen Suite on tukemaan luotettavat tiedot nieltynä prosessin eri tavoin. Tämä on edelleen lisätietoja tämän artikkelin **käyttöönotto** -osasta.

### <a name="data-quality"></a>Tietojen laadun
Raaka tietolähde, jota tarvitaan luotettava ja tarkka demand ennusteiden tekemistä varten on täytettävä joitakin perustiedot laatukriteerit. Lisäasetukset tilastollisten menetelmien avulla voidaan joidenkin tietojen laadun ongelman korjaamiseksi, emme on varmistaa, että olemme ovat leikkaavat joitakin perustiedot laadun raja-arvon, kun ingesting uudet tiedot. Tässä on joitakin tapoja tietoja laatuun:
-   **Arvo puuttuu** – tämä tilanne viittaa, kun tarkan allekirjoitukseen ei. Tähän perusvaatimukset on, että puuttuu arvo korko ei voi olla suurempi kuin 10 prosenttia tahansa tiettynä aikajaksona. Tapauksessa, että yksittäisen arvon puuttuu se ilmoitetaan käyttämällä valmiiksi määritetyn arvon (esimerkiksi: "9999") ja not "0" joka voi olla kelpaa.
-   **Mitta-tarkkuutta** – todellinen arvo kulutus ja -lämpötilan merkitään tarkasti. Epätarkkoja mitat tuottaa epätarkkoja ennusteiden. Yleensä Mittausvirhe on pienempi kuin 1 % suhteessa arvon true.
-   **Mittayksikön muuttaminen aika** – on pakollinen todellinen aikaleima tietojen kerääminen saa olla yli 10 sekuntia suhteessa todellinen mitta tosi aika.
-   **Synkronoinnin** – kun useista tietolähteistä käytetään (*kuten*, kulutus ja lämpötilan) on varmistamme, joka ei ole ajan synkronoinnin ongelmat niiden välillä on. Tämä tarkoittaa minkä tahansa kahden riippumattoman tietolähteistä kerättyjen aikaleima ajan erotuksen korkeintaan yli 10 sekuntia.
-   **Viive** - kuin edellä kuvatun **Tietojen nieltynä**olemme ovat riippuvaisia luotettavat tiedot kulun sekä nieltynä prosessi. Voit hallita, joka on varmistamme olemme valitsemilla tietojen viive. Tämä on määritetty aika, joka todellinen mitta on tehty ja aika, jolloin se on ladattu Cortana liiketoimintatietojen Suite varastoon ja on valmis käytettäväksi ajan erotuksen. Lyhyen aikavälin kuormituksen ennusteiden yhteensä viive ei saa olla yli 30 minuuttia. Pitkällä aikavälillä kuormituksen ennusteiden yhteensä viive ei saa olla yli 1 päivä.

### <a name="data-preparation-and-feature-engineering"></a>Tietojen valmisteleminen ja ominaisuus-tekniikka
Kun perustietoja on nieltäväksi (katso **Tietoja nieltynä**) ja on tallennettu turvallisesti, on valmis käsiteltäväksi. Tietoja valmisteluvaihe on lähinnä tekeminen perustietoja ja muuntaminen (muodonmuutoksen, uudelleen muotoilemalla) sen mallinnus vaiheen lomakkeeseen. Jotka voivat sisältää yksinkertaiset toiminnot, kuten käyttämällä raaka tietosarake on mitattu arvo oikeasti, standardoitu arvoja, monimutkaisia toimintoja, kuten [aika lagging](https://en.wikipedia.org/wiki/Lag_operator)ja muiden. Juuri luomasi tietosarakkeiden kutsutaan tietoihin liittyviä toimintoja ja luontia nämä kutsutaan ominaisuus tekniikka. Tämä toimenpide lopussa on on uuden tietojoukon, joka perustietoja johdettu ja mallinnus voidaan. Tietoja valmisteluvaihe on huolehtia puuttuvat arvot (katso **Tietojen laadun**) ja niiden. Joissakin tilanteissa myös annettava varmistaaksesi, että kaikki arvot esitetään samaa asteikkoa tietojen normalisointi.

Tässä osassa on luettelo joitakin yleisiä tietoja ominaisuuksia, jotka sisältyvät energiajärjestelmien demand ennusteen mallit.

**Aika perustuva ominaisuudet:** Nämä ominaisuudet ovat peräisin päivämäärän ja aikaleiman tiedoista. Nämä ovat purettu ja muunnettu categorical ominaisuuksia, kuten:
-   Aika päivän – tämä on tunnin päivämäärän, joka kestää arvot väliltä 0 – 23
-   Päivä viikon – tämä edustaa viikonpäivä ja kestää arvot väliltä 1 (sunnuntai) – 7 (lauantai)
-   Päivä kuukauden – tämä on todellinen päivämäärä ja niihin arvot väliltä 1 – 31
-   Kuukauden vuoden – tämä edustaa kuukauden ja kestää arvot väliltä 1 (tammikuu)-12 (joulukuu)
-   Viikonlopun – tämä on binaarinen arvo-ominaisuus, joka suoritetaan viikonloppu arvot 0, 1 tai viikonpäivät
-   Loma - tämä on binaarinen arvo-ominaisuus, joka suoritetaan vapaapäivä arvot 0, 1 tai Normaali päivä
-   Termejä Fourier – termejä Fourier ovat leveydet, joka johdetaan aikaleiman ja niitä käytetään siepata kausivaihtelu (jaksot) tiedot. Vaikka emme voi olla useita Vuodenajat tietojamme tarvitsemme Fourier useita ehtoja. Esimerkiksi demand arvoja voi olla vuosittain-, viikko- ja päivittäisten Vuodenajat/jaksot joka johtaa 3 Fourier ehdot.

**Riippumaton mitta ominaisuudet:** Riippumattomien ominaisuuksia ovat esimerkiksi kaikki tiedot elementtejä, jotka haluamme predictors Microsoftin mallissa käytetään. Seuraavassa on jättää riippuvaiset toiminto, jolla annettava ennustaa.
-   Viive-ominaisuus – nämä ovat ajan ajassa todellinen tarve arvot. Esimerkiksi viive 1 ominaisuuksia painettuna demand-arvon edellisen tunnin (olettaen että tunnin välein tietojen) suhteessa nykyisen aikaleimaa. Olemme vastaavasti Lisää viive 2, 3, viiveen *jne*. Viive-ominaisuuksia, joita käytetään todellinen yhdistelmä määrittää mallinnus vaiheessa mallin tulosten arviointi.
-   Pitkäkestoisen trending – tämä toiminto on demand vuotta lineaarinen kasvu.

**Riippuvaiset ominaisuus:** Riippuvaiset ominaisuus on tietosarake, jotka haluamme ennustaa Microsoftin mallia. Kanssa [valvonnan alaisena konepohjaisten oppimistekniikoiden](https://en.wikipedia.org/wiki/Supervised_learning)annettava kouluttaminen ensimmäistä riippuvainen ominaisuuksilla mallin (joka kutsutaan myös otsikoina). Näin saat riippuvaiset ominaisuus liittyviä tietoja kuviot malli. Ennuste energiajärjestelmien demand-yleensä haluamme ennustaa todellinen tarve ja näin ollen Käytämme se riippuvaiset ominaisuutena.

**Puuttuvat arvot käsittely:** Tiedot-valmisteluvaihe aikana annettava puuttuvat arvot käsitellään parhaita määrittäminen. Eri tilastollisten [tietojen huomioon ottaminen menetelmiä](https://en.wikipedia.org/wiki/Imputation_(statistics))enimmäkseen valmis. Kyseessä energiajärjestelmien demand ennusteiden on yleensä hankintapäivän sijaan puuttuvat arvot käyttämällä liukuva keskiarvo-edellisen käytettävissä arvopisteitä.

**Tietojen normalisointi:** Tietojen normalisointi on muu muunnos, jolla voidaan tuoda kaikki numeeriset tiedot, kuten vaatimus ennuste samalla asteikko. Tämä yleensä auttaa parantamaan mallin tarkkuutta ja tarkkuus. Olemme toimet tehdään yleensä todellinen arvo jakamalla tulostettava tietoalue.
Tämä skaalautuvat alkuperäisen arvon pienempi solualueen yleensä väliltä-1 – 1.

## <a name="modeling"></a>Mallinnus
Mallinnus vaihe on, jos mallin tiedot muuntaminen tapahtuu. Tämä toimenpide core ovat Lisäasetukset algoritmeista, joka tarkistaa historiatiedoissa (koulutustietoja), purkaa kuviot ja mallin luominen. Mallin voidaan myöhemmin ennustaa uusia tietoja, joka ei ole käytetty luonnissa mallin käyttöön.

Kun on toimiva luotettava malli, emme voi käyttää sitä sija uudet tiedot, jotka on järjestetty tarvittavat toiminnot (X). Voit julkaista tulosmalli prosessin pysyvän (objektin koulutus-vaihe)-mallin avulla ja ennustaa kohde-muuttuja, joka on merkitty Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Tarvittaessa ennusteiden mallinnus tekniikat
Kyseessä demand ennusteiden Microsoft käyttää historiatiedot, jotka on järjestetty ajan mukaan. Yleensä viitataan tietoja, jotka sisältävät aikadimension kuin [aikasarjalle](https://en.wikipedia.org/wiki/Time_series). Aika sarjan mallinnus tavoitteena on löytää aikaa liittyvät trendejä, kausivaihtelu, automaattinen-korrelaatio (ajan kuluessa korrelaatio), ja antaa käyttäjille malliin.

Viime vuosina Lisäasetukset algoritmit on kehitetty sopimaan aikasarjan ennusteen ja Ennustaminen tarkkuuden parantamiseksi. Käsiteltävät aiheet lyhyesti joitakin ne tähän.

> [AZURE.NOTE] Tässä osassa ei ole tarkoitettu käytettävän tietokoneen oppiminen ja Ennustaminen yleiskatsaus mutta mieluummin kuin lyhyt katsauksen mallinnus demand ennusteen yleisimmin käytettyjä tekniikoita. Lisätietoja ja oppilaitoksille materiaali tietoja aikasarjan ennusteen online kirjan on erittäin suositeltavaa [Ennustaminen: periaatteiden ja käytäntö](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**Liukuva keskiarvo (liukuva keskiarvo)**](https://www.otexts.org/fpp/6/2)
Liukuva keskiarvo on yksi ensimmäisen analytical tekniikoita, joka on käytetty aikasarjan ennusteen ja se on vielä yksi yleisimmin käytettyjä menetelmiä tänään. Kannattaa myös perusta kehittyneempiä ennusteiden avulla. Kanssa liukuva keskiarvo on ovat ennusteiden seuraava arvopiste K viimeisimmän pisteet, jossa K ilmaisee liukuvan keskiarvon järjestyksen kautta aluetoimistojen.

Liukuva keskiarvo tekniikka vaikutus tasoitus ennuste on, ja näin ollen saattaa ei käsittele hyvin suuri haihtuvuus tietojen.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (Eksponentiaalinen tasoitus)**](https://www.otexts.org/fpp/7/5)
Eksponentiaalisen Kolmoistasoituksen (ETS) on perhe, joka käyttää uudempia arvopisteitä painotetun keskiarvon ennustaa seuraavan arvopiste eri tavoilla. Idea on suurempi leveydet varaaminen uudempaan arvot ja pienentää vähitellen tätä vanhemmat arvojen paino. On useita eri tapoja kyseisen perheen kanssa, jotkin niistä Sisällytä kausivaihtelu käsittely tietoja, kuten [Holt Winters Kausiluonteisista menetelmää](https://www.otexts.org/fpp/7/5).

Joitakin näistä menetelmistä huomioon myös tiedot kausivaihtelu.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automaattinen Regression integroitu liukuvan keskiarvon)**](https://www.otexts.org/fpp/8)
Automaattinen Regression integroitu liukuvan keskiarvon (ARIMA) on toisen tason tavoista koon muuttamiseen tarkoitettu yleisesti aikasarjan ennusteen. Se käytännössä liukuva keskiarvo yhdistää automaattinen regression tavoista. Automaattinen regression tavoista käyttää tehokkaasta edellisen kerran sarjojen arvot regression mallien Laske seuraavan päivämäärän kohtaa.. ARIMA menetelmiä koskevat myös differencing menetelmiä, jotka sisältävät arvopisteiden välisen eron laskeminen ja käyttämällä niitä alkuperäisen mitattu arvo sijaan. Lopuksi ARIMA myös tekee käyttäminen liukuva keskiarvo tekniikoita, joiden käsitellään yläpuolella. Kaikki tavat eri tavoin yhdistelmä on mitä rakenteiden perhe ARIMA tavoilla.

ETS ja ARIMA käytetään yleisesti tänään energiajärjestelmien demand ennusteiden ja monia muita Ennustaminen ongelmatilanteita varten. Monissa tapauksissa nämä yhdistetään yhteen toimittamaan hyvin tarkkoja tuloksia.

**Yleinen useita regressio** Regressiosuoran malleja voi olla tärkeimmät mallinnus lähestymistapa, tietokoneen oppiminen ja tilastoja toimialueella. Aikasarjan kontekstissa Käytämme regression ja ennustaa tulevia arvoja (*kuten*vaatimus,). Regressiosuoran kestää lineaarinen yhdistelmä predictors ja tietoja, kyseiset predictors (tunnetaan myös nimellä kertoimet) leveydet koulutus aikana. Tavoitteena on tuottaa regressiosuoran, joka ennustettu Microsoftin ennustettu arvo. Regressiosuoran menetelmät soveltuvat, kun kohde-muuttuja on numeerinen ja näin ollen sopii myös aikasarjan ennusteen. Tällä monien regression kautta kuten hyvin regressio malleja, kuten [Lineaarisen regressiosuoran](https://en.wikipedia.org/wiki/Linear_regression) ja monimutkaisemman tiedoston, kuten päätös puita, [Satunnainen metsien](https://en.wikipedia.org/wiki/Random_forest), [Neural verkkojen](https://en.wikipedia.org/wiki/Artificial_neural_network)ja tehosti päätös puut.

Luomisesta energiajärjestelmien demand ennusteiden tulkitaan ongelmiksi regression antaa us joustavuutta valitsemalla Microsoftin tietojen ominaisuuksia, jotka voidaan yhdistää todellinen tarve aikatietoja sarjan ja ulkoiset tekijöistä, kuten lämpötilan paljon. Lisätietoja valitun ominaisuuksista käsitellään suunnittelu tämän playbook (katso **tietojen valmisteleminen ja ominaisuus suunnitteluryhmät**)-osa-ominaisuutta.

Käyttöönotto-ja energiajärjestelmien demand ennusteiden kokeilla käyttöönoton Microsoftin kokemus-on löytänyt että Lisäasetukset regression mallit, jotka ovat käytettävissä Azure millilitroina yleensä paras tuota ja Microsoft käyttää niitä.

## <a name="model-evaluation"></a>Mallin arviointi
Mallin arviointi on **Mallin kehittäminen jakson**kriittinen rooli. Tässä vaiheessa Odotamme kyselyjä mallin ja todellisia tietoja suorituskykyyn vahvistaminen. Mallinnus vaiheessa Käytämme, koulutus mallin osa käytettävissä olevia tietoja. Laskenta-vaiheessa kestää loput tiedot Testaa mallia. Käytännössä se tarkoittaa sitä, että olemme käyttöä mallin uudet tiedot, jotka on tarkoitus, ja se sisältää samat toiminnot kuin koulutus-tietojoukko. Kuitenkin vahvistus aikana Käytämme mallin ennustaa kohde-muuttuja on käytettävissä kohde muuttujan sijaan. Usein viitataan kuin mallin näkyvissä pistemäärä prosessia. Syy käyttää tosi tavoitearvojen ja vertaillaan budjetoidut niistä. Tähän tavoitteena on mitata ja minimoida tekstinsyöttö-virhe, mikä tarkoittaa, ennusteiden ja tosi arvon välinen ero. Avaimen määrällisesti virhe mitta on, koska haluamme hienosäätää mallin ja tarkista, onko virheen todella pienentäminen. Tarkka määrittäminen mallin voidaan toteuttaa, muokkaamalla mallin parametrit, jotka ohjaavat learning prosessin tai lisäämällä tai poistamalla tietoihin liittyviä toimintoja (jota kutsutaan [Parametrit Puhdista](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Käytännössä siis ehkä annettava käydä välillä modeling-ominaisuus-tekniikka ja mallin arvioinnin vaiheet useita kertoja, kunnes emme voi pienentää virheen vaaditun tason.

On tärkeää, että tekstinsyöttö virheen koskaan on nolla korostus mallin, joka voi ennustaa täysin jokaisen tulos ei koskaan. On kuitenkin tiettyjä suuruus virhe, joka on hyväksyttävä yrityksen. Vahvistuksen aikana, haluamme varmistaa, että mallin tekstinsyöttö virhettä tasolla tai parempi kuin business poikkeama taso. Tämän vuoksi on tärkeää suojaustaso suurin sallittu virhe **Ongelma muodostamisessa** vaiheessa jakson alussa.

### <a name="typical-evaluation-techniques"></a>Tyypillinen arvioinnin tekniikat
Useilla eri tavoilla mitä tekstinsyöttö-virhe määritettävissä ja niiden. Tässä osassa on keskitytään arvioinnin menetelmistä asiaa aikasarjalle ja energiajärjestelmien demand ennuste tietyn keskustelun.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE lyhenne tarkoittaa suora prosentti-virhe. MAPE kanssa on tietojenkäsittely välisen eron kunkin ennustettu piste- ja kyseiseen pisteeseen todellinen arvo. Olemme sitten Muuta kussakin virheen laskemalla osuus suhteessa todellinen arvo erotuksen. Viimeinen vaihe keskimääräinen nämä arvot. MAPE käytettävä matemaattinen kaava on seuraava:

![MAPE kaavan](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*<sub>t</sub> on todellinen arvo, F,<sub>t</sub> on ennustettu arvo ja n on ennusteen horisontti.*

## <a name="deployment"></a>Käyttöönotto
Kun olemme nailed mallinnus vaiheen alaspäin ja vahvistaa emme valmis salliva asetus käyttöönottovaiheen mallin suorituskykyä. Tässä yhteydessä käyttöönoton tarkoittaa, että ottaminen käyttöön asiakkaan tarjoaman mallin suorittamalla todellinen ennusteiden sitä suurten asteikko. Käyttöönoton käsite on Azure millilitroina avainta, koska tärkeimmät Tavoitteenamme on käynnistää jatkuvasti ennusteiden verrattuna hankkiminen tietoja vain tiedoista. Käyttöönottovaihe on osa, jossa olemme käyttöön mallin kulutettu suuri tasolla.

Energiajärjestelmien demand ennusteen puitteissa Microsoftin tavoitteena on ongelma jatkuvan ja jonka ennusteiden ja varmistaa, että ajan tasalla tiedot ovat käytettävissä mallin ja ennustetut tiedot lähetetään takaisin vievää asiakkaalle.

### <a name="web-services-deployment"></a>Web Services-palvelujen käyttöönottaminen
Tärkeimmät Azure millilitroina esikunnista rakenneosan on web-palvelu. Tämä on tehokkaasti kulutus pilveen ennakoivan mallin käyttöön. WWW-palvelun kapseloi mallia ja rivittyy sen [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface) kanssa. Ohjelmointirajapinnan voidaan osana mahdollisesti asiakkaan koodi, kuten seuraavassa kuvassa.

![Olemme palvelun käyttöönottoa ja kulutus](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Kun näkevät, web-palvelu on otettu käyttöön Cortana liiketoimintatietojen Suite pilveen ja voi käynnistää sen näkyviä REST API-päätepisteen kautta. Erityyppisen asiakkaat eri toimialueilla voi käynnistää palvelun kautta verkko-Ohjelmointirajapinnan samanaikaisesti. WWW-palvelun myös skaalata tukemaan tuhansia samanaikaisia puhelut.

### <a name="a-typical-solution-architecture"></a>Tyypillinen ratkaisuarkkitehtuuri
Ennusteiden ratkaisu energiajärjestelmien-demand otettaessa emme kiinnostunut, jotka ulottuvat tekstinsyöttö web-palvelu ja helpottaa koko tiedonkulun lopusta loppuun-ratkaisun käyttöönotto. Olemme käynnistää uusi ennusteen milloin annettava Varmista, että mallin syötetään on ajan tasalla olevat tiedot ominaisuudet. Joka tarkoittaa, että raaka juuri kerättyjen tietojen jatkuvasti nautittuina, käsitellä ja muutetaan tarvittavat ominaisuusjoukko, jossa malli on luotu. Haluamme samanaikaisesti, ja helpottaa näin ennustetut tietojen käytettävissä muissa asiakkaiden loppuun. Esimerkki tietojen työnkulun jakson (tai tietojen myyntijakso) on esitetty seuraavassa kuvassa:

![Energiajärjestelmien Demand ennuste pääty tiedonkulun](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Seuraavassa on kuvattu vaiheet, jotka asetetaan energiajärjestelmien demand ennusteen jakson osana:
1.  Käyttöön tietojen metriä miljoonia jatkuvasti luodaan power tiedot reaaliajassa.
2.  Tämä tieto on kerätty ja ladata kyselyjä cloud-tietovarasto (*kuten*Azure-Blob).
3.  Ennen käsittelyn perustietoja koostetaan ala-asema tai alueellisen tason, yrityksen määritysten mukaisesti.
4.  Ominaisuuden käsittely (katso **tietojen valmisteleminen ja ominaisuus käsittely**) Valitse tapahtuu ja tuottaa tiedot, jotka tarvitaan malli koulutus tai näkyvissä pistemäärä – ominaisuuden määrittäminen tiedot on tallennettu tietokantaan (*kuten*SQL Azure).
5.  Uudelleen koulutusta palvelu käynnistyy ja kouluttaminen uudelleen Ennustaminen mallin kyseisen mallin päivitetty versio on samanlainen niin, että se voidaan käyttää tulosmalli WWW-palvelun.
6.  Tulosmalli web-palvelu käynnistyy, joka sopii tarvittavat ennusteen korkojakso aikataulun.
7.  Ennustetut tiedot on tallennettu tietokantaan, jota voi käyttää Lopeta kulutus asiakas.
8.  Kulutus asiakkaan noutaa ennusteiden, ottaa sen takaisin ruudukkoa ja siinä käytetään tarvittavat Käyttötapaus mukaisesti.

On tärkeää muistaa, että koko jakson täysin automaattinen ja suorittaa aikataulun. Tietoja jakson koko tiedonsiirron voidaan toteuttaa työkaluilla, kuten [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Lopusta loppuun-käyttöönotto-arkkitehtuuri
Jotta käytännössä käyttöön energiajärjestelmien demand ennusteen ratkaisun Cortana Liiketoimintatieto-, annettava tarvittavat osat ovat luotu ja määritetty oikein.

Seuraavassa kaaviossa on kuvattu tyypillinen mukaan Cortana Liiketoimintatieto-arkkitehtuuri, joka toteuttaa orchestrates tietojen työnkulun jakson, joka on yllä:

![Lopusta loppuun-käyttöönotto-arkkitehtuuri](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Jos haluat lisätietoja eri osat ja koko-arkkitehtuuri tutustumaan energiajärjestelmien ratkaisu mallia.
