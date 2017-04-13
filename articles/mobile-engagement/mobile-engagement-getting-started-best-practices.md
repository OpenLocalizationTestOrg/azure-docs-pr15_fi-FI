<properties 
    pageTitle="Oppaan Azure Mobile välitys käytön aloittaminen parhaat käytännöt"
    description="Käytön aloittaminen-opas Azure Mobile välitys ja onboarding parhaat käytännöt" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/04/2016"
    ms.author="wesmc;ricksal"/>

# <a name="azure-mobile-engagement---getting-started-guide-with-best-practices"></a>Azure Mobile välitys - ja parhaita käytäntöjä aloitusopas

## <a name="overview"></a>Yleiskatsaus

**Matkapuhelimen näytön on hyvin tiivis tila:** 2013: ssa tutkimus löydy keskimääräinen mobiililaitteeseen oli 27 asennetut sovellukset. Käyttäjien käytetyt yleensä 30 tunnin kuukaudessa niiden sovellukset. Suurin osa tällä hetkellä kului sosiaalinen verkko-ja pelaaminen (noin 20 tuntia). Android market oli mukaan 2014, valittavana 1,5 miljoonaa sovelluksiin. Apple-kaupasta sisälsi 1.2 miljoonaa sovellukset. Mobiilisovelluksen käytetään edelleen yhä enemmän kuin kehittäjät kilpailemaan tämän kasvava market. 

Keskimääräinen matkapuhelinkäyttäjää asennetaan ja suuri usein mukaan muuttaminen kiinnostuksen kohteet-sovelluksen asennuksen ja sovellus-kohtaa. Määrittää sovelluksen onnistuu tulee tietää, kuinka monta käyttäjää pelkästään asentaa sovelluksen tärkeä. On tärkeää tietää, miten hyödyllinen sovelluksen on, ja jos käyttö, jonka trendi muuttuu. Seuraavat kysymykset ovat tärkeitä:

- Ovat käyttäjien alkuun uninteresting tai vanhentuneen etsiminen sovelluksen? 
- Kuinka monta käyttäjää on pysäytetty-sovelluksen käytön lainkaan? 
- Ovat sovelluskohtaisesta ostot tykkäysten ylös- tai alaspäin?
- Epäonnistuvat käyttäjien suorittamiseen työnkulussa sovelluksen tai puutteen korko-ongelmien vuoksi? 
- Onnistunut voit pitää sovelluksen hyödyllisiä asiaa mukaan valitseminen uusi sisältö käyttäjän kantalukua? 
- Milloin tämä uusi sisältö on sama kaikille käyttäjille tai käyttäjän osia-sovelluksen toiminta perustuu kohdistettu? 
 
Vastauksia kysymyksiin, jotka vastaavat näihin voi auttaa laajentaa eloisuutta ja tuotto-sovellukset. Niiden myös avulla voit määrittää ja säilyttää käyttäjän base. 

Media, jotka liittyvät sovellukset yleensä on suurin säilytys käyttäjien keskuudessa. Syy tähän on ne jatkuvasti tarjoaa käyttäjille uusi sisältö. Hyödyllisiä push-ilmoitukset ohjataan käyttäjän segmenttiin aikainen antamisen yleensä on suuri vaikutus app säilytys. 

Azure Mobile välitys ohjelma on suunniteltu avulla voit laajentaa eloisuutta ja sovelluksen säilytys antamalla tapa kerätä ja analysoida yksityiskohtaiset tiedot sovelluksen käyttöä. Sen avulla voit luokitella perus mukaan toiminta käyttäjän ja luo aktiivisen Kampanjat välittää push-ilmoitukset ja sovelluskohtaisesta viestien tunnistetun käyttäjän osia. Suorituskykyilmaisimet (KPI) mittaa, miten aktiiviset käyttäjät voivat sovelluksen osa. Azure Mobile välitys tarjoaa menetelmiä, sinun on määritettävä nämä KPI: T. Sen avulla voit suurentaa palauttaa tuoton (ROI) antamalla infrastruktuuri, sinun täytyy suurentaa välitys mobile-sovelluksen. 

Jotta saat tehokkaasti Azure Mobile välitys, sinun on hyvin suunnitellut välitys suunnitelman aloittaminen. Palvelupaketin auttaa sinua huomaamaan, sinun on voitava määritetään käyttäjän base hajautetun tiedot. Tämä voi perustua toiminta ja sovellus-kohtaa. Järjestyksessä palvelupakettiisi onnistuu on paras käytäntö on selkeästi määrittää KPI, joka mittaa sovelluksen tavoitteet. Poista suorituskykyilmaisimia määritetty, jossa voit upottaa helposti tarvittavat logiikan kerää tietoa hakuindeksointi voidaan suojata tietoja, joita käytät analysoida ja arvioida oman KPI: T-sovelluksen. Tämä artikkeli koskee parhaiden käytäntöjen opas määrittäminen KPI, jota käytät ja välitys-palvelupaketti. 


## <a name="step-1-define-your-kpis-to-fit-the-bet-model"></a>Vaihe 1: Määritä Sovita VALINTOJEN mallin oman KPI: T


Määrittäminen oikein suorituskykyilmaisimet voi olla vaikeaa tehtävän suorittamiseen. Sovellukset on suunniteltu eri teollisuuden on Omat tiedot ja tavoitteet. Tämä yleensä sekoittaa oman toimintatapa sen vuoksi. Voit välttää tämän tavoitteet ja KPI: T luokitellaan tärkeimmät kolmeen luokkaan: **Business**, **välitys**ja **tekniset**. Tämä on **VALINTOJEN malli**.

Hyvä palvelupaketti on yleensä tavoitteet, joissa mitataan VALINTOJEN mallin seuraavanlaisia suotuisien KPI: N kanssa.


#### <a name="business-kpis"></a>Liiketoiminnan KPI: T

Liiketoiminnan KPI: T on oltava luonnissa helpoin osa. Voit määrittää todennäköisesti näitä joitakin lomakkeen suunnittelun mobile-sovellus. Nämä KPI: T avulla yleensä tuoton mitta ja ROI voit sovelluksen. Seuraavassa luettelossa on joitakin Esimerkki Business KPI: T, jotka voivat auttaa auttaa sinua aikana suorituskyvyn mittarit määrittäminen:

- Media Business KPI: T
    - Active Directory määrä napsautettaessa
    - Sivun numeron käy käyttäjää kohden
    - Nykyinen tilausten määrä
- Gaming Business KPI: T
    - Sovelluskohtaisesta ostot määrä
    - Keskimääräinen tuotto käyttäjäkohtainen (ARPU)
    - Istunnossa aika
    - Ottelu tason toistettujen ja nykyisen päivän
- E Commerce Business KPI: T
    - Päivät-sovelluksessa
    - Keskimääräinen tuotto käyttäjäkohtainen (ARPU)
    - Keskimääräinen summa ostoskorin uloskuittauksen aikana
    - Useimmissa näkymissä ja Ostot tuoteluokka
- Pankin ja vakuutus Business KPI: T
    - Tilien määrä
    - Aktivoitu ominaisuudet
    - Tarjouksen avatut sivut
    - Napsautettaessa tai aktivoitu      



#### <a name="engagement-kpis"></a>Välitys KPI: T

Välitys-KPI on suorituskyky-ilmaisin mitata välitys käyttäjät. Sovelluksen säilyttäminen selvittää trendejä tällä alueella. Tässä on muutama Esimerkki suorituskykyilmaisimia tämäntyyppisen KPI:

- Viimeisen 7 päivän aikana aktiiviset käyttäjät
- Viimeisen seitsemän päivän passiivisen käyttäjän määrä
- Käyttäjät, jotka ole käyttänyt sovelluksen 30 päivää määrä  

Jotkin ilmeisimmät Ulkoiset tekijät voivat vaikuttaa ilmaisimet tällä alueella. Esimerkiksi kannattaa harkita mobiililaitteella on aina käyttäjälle. Tämä voi tai ei voi olla tosi. Gaming-sovellus voi yleensä on suurempi käyttö juhlapäivät, kun pelaajan voi toistaa Lisää aikana työn tai koulun loitontaminen ympärille.   

Hyvin määritelty suorituskykyilmaisimet tähän luokkaan auttaa mittaa sovelluksen ja asiakkaillesi välisen suhteen.



#### <a name="technical-kpis"></a>Tekniset KPI: T

KPI-ilmaisimien tähän luokkaan avulla voit määrittää, jos sovellus on muunnettuna oikein, kaatuu tai kaatuu. Ilmaisimet voit mitata sovelluksen kunto ja selvittää käytettävyys ongelmat, jotka saattavat estää käyttäjiä sovelluksen. Luokan kerättyjä tietoja voi olla myös suorituskykyyn liittyvät tiedot, jotka kiinnostavat markkinoinnin ryhmiä. Tiedot myös voivat olla hyödyllisiä vianmääritys IT ja tuki työryhmät voivat auttaa tunnistamaan ilmoittamatta virheet. 
 
Seuraavassa on esimerkkejä tekniset KPI:

- Käsittelemättömän tai käsitellyt poikkeuksen tiedot ja määrä 
- Viimeinen kaatumisen aikaleima
- Viimeinen painiketta napsautettaessa tai viimeisen sivun avoimelle
- Sovelluksen muistinkäyttö
- Sovelluksen kehyksen korko
- Käyttöjärjestelmäversio, jossa sovellus on käynnissä
- App-versio

Määritä nämä KPI: T mittayksikön sovelluksen suorituskykyä ja pinpoint mahdolliset virheet. Tämä ilmaisimet olisi pakettitiedoston aikaa aikana korjaa asiakkaillesi. Ne voi myös avulla voit määrittää, kuka on ilmennyt tietyn ongelmat käyttäjän segmenttiin. Kyseisen käyttäjän Segmentointi avulla voit luoda Kampanjat aikana käytettävissä korjaukset ja mahdolliset Kampanjat ilmoituksia avulla palauttaa asiakkaiden tyytyväisyyttä. 


#### <a name="playbook-exercise-1-create-your-kpi-dashboard"></a>Playbook Harjoitus 1: KPI-raporttinäkymän luominen

Yrityksen Markkinoinnin määrittäminen määritettäessä oman suorituskykyilmaisimet pitäisi esittää näkymän kunkin tärkeimmät tavoitteet. Niiden on oltava selvästi arvopisteitä, jotta voit kerätä seurannassa sovelluksen ja käyttäjä toimintaa tärkeitä tietoja.

Luo KPI: N koontinäyttö, jossa on alla olevia tietoja

1.  Mitkä ovat KPI-sovellukseen?
2.  Tietoja osoittaa avulla esittää kunkin KPI: N?
3.  Nämä tiedot sijainnin Omat sovelluksen (eli näytön, asetukset, järjestelmä...)
4.  Välitys sarjan toistaa tämän KPI: n

Voit käyttää **KPI Builder** -laskentataulukon Microsoftin [Media Playbook mallin] [ Media Playbook link] esimerkkejä ja ohjeita.



## <a name="step-2-your-engagement-program"></a>Vaihe 2: Välitys ohjelman


Hyvien mobile välitys ohjelma on otettava huomioon avaimen sovelluksen osa. Tämä on ehdottoman sisältää hyvien Tervetuloa ohjelmaan, joka suorittaa käyttäjän sovelluksen käytön ensimmäisen päivän aikana. Tämä yleensä hyvin positiivinen vaikutus välitys ja sovelluksen säilytys. Tutkimukset näkyvän käyttäjien useimpia lopettaa käyttämällä sovelluksen asennuksen jälkeen ensimmäisen muutaman päivän. Haluat tarkoituksena on täytettävä tai ylittää asiakkaan oletuksena ajo korko etuajassa samalla, kun käyttäjä on edelleen kohdistettu sovelluksen. Varmista, että avainarvon ja sovelluksen hyödyt esittää asiakkaillesi. 


![](./media/mobile-engagement-getting-started-best-practices/unsegmented-push-notifications.png)

Push-ilmoitukset ovat paras tapa aikainen sitoumukset mobiililaitteen käyttäjien kanssa. Kuitenkin hyvin varovainen olisi otettava segmentoidessasi käyttäjien push-ilmoitukset. Koska kun käyttäjän tuntuu kuten käyttäjä saa roskapostia tai uninteresting ilmoituksissa, se voi olla vakavia vaikuttaa. Käyttäjä voi poistaa sovelluksen koskaan palauttamaan muutamalla napsautuksella. Käyttäjä olisi vastaanottaminen erittäin mukautettuja sovelluskohtaisesta arvo yleinen roskapostin sijaan.

Kun käyttäjät aktiivisesti osallistut, valitse välitys-ohjelman avulla vaikuttavat sovelluksen muita ominaisuuksia.

Voit esimerkiksi määrittää markkinointikampanjan, joka pyytää luokittelevan sovelluksen aktiiviset käyttäjät. Koska käyttäjän tässä osiossa on eniten aktiivisia ja on eniten kokemusta kanssasi app, todennäköisesti odottanut ne mahdollisimman tarkat luokitus. Kun suuri app luokitus, se voi auttaa aseman pienentämisestä sekä uusi asiakas hankintakustannukset sovelluksen orgaaniset Lataa ylöspäin.



#### <a name="engagement-sequence"></a>Välitys järjestys


Yleinen välitys-ohjelma sisällyttää eri välitys sarjaa. Kunkin sarjan tarkoituksena on useita tavoitteiden saavuttamiseksi.


###### <a name="life-push-sequence"></a>Elämä push järjestys


Elämä push-sarja tavoitteet ovat erilaiset riippuen siitä käyttäjän välitys sovelluksen elinkaari. Tietyn käyttäjän voi olla uusi, passiiviset tai hyvin aktiivinen. Välitys elinkaari eri vaiheissa käyttäjät voi saada ajan tasalla sisältösi vihjeitä tai linkit ohjeissa. 

Esimerkiksi uusi käyttäjä voi on Ohje käytön aloittaminen-ohjelmaan tai ne käynnistämällä sovelluksen ensimmäisen kerran uuteen käyttäjän kannustaa seuraavankaltaiselta hyötyvät...

*"Että olet siirtyvät! Saat vapaa oman 1 kuukausi kirjautuminen muista!"*


###### <a name="behavioral-push-sequence"></a>Käytöspiirteen push järjestys

Käytöspiirteen push jaksossa tarkoituksena on Lisää perustuvat käyttäjän toiminnan kerääminen sovelluksen käyttöä.  

Esimerkiksi Kuvitellun jalkapallo-sovelluksen hyvin aktiivinen käyttäjä voi hyötyä on kytketty seuraavat push-ilmoituksen...

*"John pidät vakavia jalkapallo! Kirjaudu sisään Microsoftin NFL-osassa ja vapaa SuperBowl pääsy win!"*


###### <a name="alerting-push-sequence"></a>Ilmoitat push järjestys

Käyttäjien Arvostamme asiaa kohdistettu niiden edut uutisia. Ilmoitusten push-sarjan parantaa välitys lähettämällä etuja perustuvat hälytykset käyttäjä on selkeästi osoittanut. Tämä voi johtua eksplisiittisten, kun käyttäjä valitsee omia etuja-sovelluksessa. Voi myös määritettävä implisiittisesti käyttäjän toimia sovelluksen aikana kerättyjen tietojen perusteella.

Sovelluksen E Commerce käyttäjän säännöllisesti esimerkiksi ostaa tietyn brändiä, esimerkiksi, jotka on tallennettu liiketoiminnan KPI: N kanssa. Seuraavan ilmoituksen voit parantaa käyttäjän välitys sovelluksen kanssa.
 
*"Suuren Wes, yksi esimerkiksi oman tuttuja merkkisiä otetaan käyttöön myyntiä ensimmäisen viikon 2015 syyskuu 25 %. Arvostamme voit asiakkaaksi ja varmista, että etsimäsi voit ovat tienneet."*

###### <a name="rentention-push-sequence"></a>Rentention push järjestys

Tässä järjestyksessä pyritään käyttäjät käyttävät toistuvien push-ilmoituksen Kampanjat avulla asema on kiinnostavissa sovelluksen säännöllisesti hyvä säilyttää. Tämä voi auttaa parantamaan app säilytys, jos käyttäjä saa vuorovaikutukset. 

Esimerkiksi liittyvät urheilu-sovelluksen käyttäjä voi tulla seuraava ilmoitus push-käyttäjän suosikki ryhmiä viikoittain perusteella:

*"Win 200 pistettä, siirry äänestyksen onko ja New Yorkin Yankees voittaa niiden Ottelu tällä viikolla vastaan Toronto sininen Jays!"*


#### <a name="the-3w-approach"></a>3W lähestymistapa

Hallintaa eri push-sarjaa auttaa sinua osallistuminen loppukäyttäjien kanssa. Kuitenkin yhä haluat käyttää 3W tapaa mukauttaminen ilmoitukset. 3W lähestymistavan olisi osoite, kun kukin ilmoituksen. Jos olet selvästi täyttävät nämä kolme kysymykset voit ilmoitukset pitäisi olla oikein koskivat välitys varten.

![](./media/mobile-engagement-getting-started-best-practices/who-what-when.png)



###### <a name="who-the-user-segment-that-will-receive-messages"></a>Kuka: Käyttäjälle määritetään, jotka saavat viestit

Erittäin luottamuksellisia tietoliikenneyhteyden olisi otettava huomioon valitseminen ilmoitukset käyttäjille. Varmista, että ilmoitukset lähetetään käyttäjän segmenttiin tavoitteena on hyvin rajattu kyseisen käyttäjän segmentin etuja. Väärin reititetty ilmoitus on todennäköisesti on negatiivinen vaikutus käyttäjälle. Ne voivat katsoa poistamisen sovelluksen johtavat roskapostia. 

Käytä sellaisia tekniset ja käytöspiirteen tiettyjen ehtojen käyttäjän osia, jotka saavat ilmoituksia määritettäessä. Esimerkki määrittäminen käyttäjän segmenttiin voi olla samanlainen seuraavan lauseen:

"Kaikki käyttäjät, jotka käynnistetty mobiilisovelluksen ensimmäisen kerran kolme päivää ja olet käynyt Kirjaudu sisään-sivulla kahdesti ilman Viimeistellään todella kirjautumisen".
 
Lause auttaa tunnistamaan joudut aloittamaan tukemaan tietyn tilanne kerää tietoja.


###### <a name="what-the-message-that-you-will-send"></a>Mitä: Viesti, joka lähetetään

**Ääni**

Käytä tekemäsi työt äänimerkkiä, joka sopii oman Segmentoitu käyttäjiä. Tämä on ehdottomasti oman loppukäyttäjien yhteydessä ja sovelluksen käyttäjän kiinnostusta Ylennä erinomainen tapa. 

**Uudelleenohjaus**

Push-ilmoitus voi käyttää useita avaamiseksi sovellus. Jos ilmoitusviesti kontekstissa, kuten uutisia lähetyksen tai tuotteen ylennyksen, tämä ilmoitus voi laaja linkin suoraan sovelluksessa oikean sisällön. Sinun on luotava kertoaksesi hallinta uudelleenohjauksen sovelluksen URL-malli tukee tätä. Kun käsittelet välitys sarjaa, tämä on tärkeä vaihe, joka ei voi poistaa.

Uudelleenohjauksen voidaan hallita myös muista järjestelmistä. Esimerkiksi toiminnon URL-osoite on mahdollista loppukäyttäjien uudelleenohjaamiseen monta muista järjestelmistä, mukaan lukien seuraavasti:

- Sivusto
- Postilaatikko, jossa jo määrittänyt sähköpostin
- SMS-ruutu
- PIN-palvelu
- Suoraan, jos haluat luokitella sovelluksen sovelluksen-kauppa. 

Tämä on monia mahdollisuuksia osallistuminen loppukäyttäjien ja luo automaattisen kyselysääntöjä, jotka parantavat suorituskyky.


**Muoto ja sisältö**

Erityyppisiä ja Push ilmoituksen muodoissa:

1. **Ilmoitukset** : mahdollistaa mainonta viestejä lähettämällä käyttäjille eri aikaa (ulos sovelluksesta-sovelluksessa tai milloin tahansa).
2. **Kyselyiden** : käytössä, voit tietojen kerääminen loppukäyttäjien pääomasijoitusprojekteihin liittyviä kysymyksiä. Näiden vastaukset ovat käytettävissä sitten, kun kohde loppukäyttäjien ehtojen luominen.
3. **Vie tiedot** : Voit lähettää päivittää sovelluksen binaarinen tai base64 datatiedosto. Tietoja tietojen push lähetetään sovelluksen käyttäjien sovelluksen käyttökokemuksen mukauttamiseen. Sovellus on suunniteltu tukemaan tiedot tietojen push.
4. **Ruutujen (vain Windows Phone)** : käytössä Microsoftin Push-ilmoituksen Service (MPNS) avulla voit lähettää alkuperäisen Push-ilmoituksen sisältävän XML-tietojen (tuetut SDK version 0.9.0 jälkeen. Lopulliset tiedot osalta voi olla enintään 32 kilotavua.). Viesti näkyy suoraan yhteyttä taulun-ruutu.
5. **Näkymä** : web-näkymä on ponnahdusikkunoiden sisältävä verkkosisällön. Tämä ponnahdusikkuna tulee näkyviin, kun käyttäjä on napsautettu push-ilmoituksen. Web-näkymässä voit on käyttäjä lisää käsittelemisen.
 
>[AZURE.NOTE] Varmista, että lähetät push-ilmoitukset sisällön noudattaa vastaaviin ympäristö (iOS, Android-, Windows) sovellusten kehittämisestä ja push-ilmoitukset lähetetään.

 


###### <a name="when-the-timing-of-your-campaign"></a>Milloin: Ajoituksen että markkinointikampanja

Kun löydät parhaan mahdollisen ajan aktivoimaan markkinointikampanjan käynnistävä push-ilmoitukset? Pitääkö olla manuaalisesti tai automaattisesti? Olisi se on toistuva? Tilanteisiin tai korkojakso on tärkeää tehdä käyttäjät, joilla on parhaat tulokset. Kunkin välitys sarjan ja skenaarion, sinun on määritettävä milloin päivitetään kannattaa push-ilmoitukset lähetetään. Seuraavassa on joitakin esimerkkejä mahdollisista:

![](./media/mobile-engagement-getting-started-best-practices/campaign-timing-examples.png)

Jos lähetät paljon ilmoitukset päivittäin, sinun on otettava vakavia huomioon, että käyttäjät voivat puoliväliarvo viestintää roskapostiksi. 

Azure Mobile välitys on kaksi tapaa viestintää parhaillaan tasapuolisina roskapostin estämiseksi. Ensin käytät hieno syyt Segmentointi kohdistamista ei samoille käyttäjille. Lisäksi Azure Mobile välitys sisältää "kiintiö"-ominaisuutta. Tämä ominaisuus rajoittaa ilmoitukset lähetetään markkinointikampanjan varten. Esimerkiksi oletusarvo-kiintiön asettaminen 5 viikossa varmistat, kuuluvien markkinointikampanjan käyttäjän segmentin osa saa ilmoituksen enintään 5 kyseisen viikon käyttäjä.





#### <a name="playbook-exercise-2-create-your-engagement-program"></a>Playbook Harjoitus 2: Luo välitys-ohjelma

Silmäile yhteenvetojen tavoitteisiin parhaiten ja määrittämällä odotat toistamiseen käyttämällä tietyn järjestyksiä Kampanjat jonkin aikaa. Varmista, että voit käyttää 3W lähestymistapa ilmoitukset oman kampanjat. 

Käyttää **Välitys ohjelma** taulukkoa [Media Playbook mallin] [ Media Playbook link] esimerkkejä ja ohjeita.


## <a name="step-3-app-integration"></a>Vaihe 3: App integrointi


#### <a name="create-a-tag-plan"></a>Luo tunniste suunnitelma

Tarvitset Azure Mobile välitys integroida sovelluksen Luo tunniste suunnitelma. Tunniste-palvelupaketti on projektin perusta. Se määrittää välisestä yhteydestä markkinoinnin määritykset-sovelluksen työnkulku ja reaali tunnisteen tietoja mitata KPI-sovelluksessa. Se osoittaa, mitä analytics osaat Nähdäksesi portaalissa. Sen avulla voit myös määrittää käyttäjän osia ja Lähetä osallistua oman loppukäyttäjien aktiivisen push-ilmoitukset. Kun olet määritetty, tunnisteen suunnitelman lisääminen integroida sovelluksen koodi on yksinkertainen Azure Mobile välitys SDK: N avulla.

Tunniste-suunnitelma ei olisi tunnisteen kaikki-sovelluksessa. Se sisältää vain tunnisteen tiedot, joka on osa yrityksen mobile välitys määrittäminen. Tämä on todennäköisesti monipuolisen sovellusten välillä. [Media Playbook mallin] [ Media Playbook link] edellyttäen mukaan Azure Mobile välitys auttaa tunnisteen suunnitelman luominen annetun-menetelmällä. Käytä **Tunnisteen suunnitelman** laskentataulukon ohjeena rakentamiseen tunnisteen suunnitelma.

Määritettäessä tunnisteen osan laskentataulukon olla tarkkojen. Tämä on erittäin tärkeää sekaannusten. Tiedot kunkin odotettu tilanne, jossa kutakin lähetetään. Sisältää tehtävän, jossa kutakin on upotettu nimi. Tämä kaikki sisällytetään laskentataulukon **Informative** osana. Tunnisteen suunnitelman laskentataulukon pitäisi olla tärkeimmät viittaus testi vahvistusta varten. 

**Kerätä tieto** -osan kehittäminen ryhmän olisi Etsi tyypit, nimiä, arvot ja tarvittavat kunkin tunnisteen, joka upotetaan sovelluksen ylimääräisiä tiedot avain/arvo-pareina.

On suositeltavaa tarkistaminen tunnisteen suunnitelman kanssa projektiin liittyvät kaikki ryhmät. Tee tarvittavat korjaukset ja Vahvista kaikki ei ole valittuna markkinointi- ja ryhmiä.

**Työn kuvaus** laskentataulukon voidaan projektiin osallistuvat kaikki oppaan avulla.


#### <a name="data-types"></a>Tietotyypit

Nämä ovat tavallisia tietojen tukemaan Azure Mobile välitys.

###### <a name="devices-and-users"></a>Laitteet ja käyttäjät

Azure Mobile välitys tunnistaa käyttäjät luodaan kunkin laitteen yksilöllinen. Tämän tunnisteen kutsutaan laitteen tunnus (tai deviceid). Luo tavalla samalla laitteen kaikkien sovellusten jakaminen on sama laitteen tunnus.

###### <a name="sessions-and-activities"></a>Istunnot ja toiminnot

Istunnon on yksi käyttäjä suoritettavan sovelluksen esiintymä. Istunnon ulottuu käyttäjä aloittaa sovelluksen, kunnes se pysäytetään ajasta.

Tehtävä on looginen ryhmittely joukko sovellus voi tehdä istunnon aikana. Se on yleensä näytön-sovelluksessa, mutta se voi olla sovelluksen logiikan määrittämien mitään. Vähintään olisi tunnisteen, kun sovellus kunkin näytön tai toiminta. Näin tulkita käyttäjä-polku.


###### <a name="events"></a>Tapahtumat

Tapahtumien käytetään raportin sovelluksen käyttäjän toimia. Ne voivat olla pikaviestien toiminnot, kuten sisältöä tai käynnistäminen videon. Tapahtumien tunnisteita antaa sinulle tietojen sivustokokoelmat, jotka osoittavat, miten käyttäjät voivat käsitellä sovelluksen kanssa. 

###### <a name="jobs"></a>Projektit

Töiden käytetään raportin toiminnot, jotka kestävät. Esimerkkejä:

- API suorituksesta
- Näytä Active Directory-aika
- Taustan tehtävien keston 
- Osta prosessin kesto
- Videon katsominen


###### <a name="errors"></a>Virheet

Virheet käytetään raportoida ongelmista havaitsemien sovellus. Esimerkiksi virheellinen käyttäjän tai Ohjelmointirajapinnan kutsu epäonnistuu.

###### <a name="application-information"></a>Hakemuksen tiedot

Hakemuksen tiedot (tiedot sovelluksen) käytetään tunniste käyttöympäristön kuvaus-sovellukseen liittyviä tietoja. Se luodaan käyttäjän vuorovaikutus-sovelluksella. 

Tietyn sovelluksen-info-näppäimen Azure Mobile välitys säilyttää vain niiden viimeisin arvo (ei historiatietoja). Sovelluksen tiedot paljastaa sovelluksen tai oman loppukäyttäjien tila. Esimerkiksi sisään tilan tai käyttäjän tuttuja tuoteryhmän.

###### <a name="crash-data"></a>Kaatumisen tiedot

Kaatua Mobile välitys SDK raporttien sovellusvirheet sovellus ei käsitellä automaattisesti keräämät tiedot. Esimerkki käsittelemättömän poikkeuksen vuoksi, joka esiintyy.


###### <a name="extra-data"></a>Lisätiedot

Tapahtumien, virheitä, tehtävät ja työt voidaan parantaa parametreilla. Tämä on erittäin tiedot kehittäjä voi antaa tietyt tiedot sovelluksen nimellä. Tämä on tärkeää tarkasti rajattuja Segmentointi määrittämistä varten. 

Esimerkiksi "artikkelissa"-tunnisteen arvon sallii segmentin loppukäyttäjien kuka tarkastella tietyn artikkelin perusteella. Kuitenkin, jotka eivät välttämättä ole tarpeeksi. Se saattaa olla parempi, jos saman "artikkelissa" tunnisteen sisältyy myös lisä-tiedot, kuten "news_category" aktiviteetin. Tämä on hyötyä määrittämään dynaamisesti käyttäjän tuttuja luokkia. 

Lisä tiedot ilmoitetaan avain/arvo-pari. Media-sovelluksen esimerkissä "news_category" lisä-tiedot olisi luokan arvon. Esimerkiksi "urheilu", "taloudessa" tai "tutkimuspolitiikasta".





#### <a name="tag-and-sdk-integration"></a>Tunniste- ja SDK-integrointi 

Vaiheittaiset ohjeet Azure Mobile välitys SDK integroiminen sovelluksen noudata [Välitys SDK integrointi](mobile-engagement-windows-store-integrate-engagement.md) ohjeissa Azure-sivustossa. Valitse kohdeympäristö linkit sivun yläreunassa.

On suositeltavaa laadittuihin päälle Azure Mobile välitys kalenteripainikkeilla projektien luominen. Yksi kehittämisen ja testaa väliaikaisen ja toista vaiheet tuotannon. IT-työryhmän voit sitten korottaa testi väliaikaisesta tuotannon kun käyttäjän hyväksymisen testaaminen onnistuu.



#### <a name="user-acceptance-testing-uat"></a>Käyttäjän hyväksyminen testaaminen (UAT)

Käyttäjän hyväksyminen testaaminen (UAT) liittyy varmistetaan, että kaikki toimii suunniteltu. Työnkulussa voi suorittaa ja kerääminen tunnisteen palvelupaketin kaikki tarvittavat tiedot:
 
- Tietoja merkitsemisestä on oltava paikallaan kuvata AZME käsitteitä mukaan
- Kaikki tiedot on kerätty (mukaan lukien ylimääräisiä tiedot arvo-sovelluksen tiedot-arvo)
- Nimikkeistön vastaa mukaan asettelua tunnisteen suunnitteleminen
- Ei kaksoiskappaleiden tunnisteita ei ole lähetetty

Testaa perusteellisesti käyttäytyminen on upotettu sovelluksen tyypit

- Ilmoitukset, kyselyiden, tietojen Vie ulos-sovelluksen ja sovelluksen:
- Teksti-Web-näkymät
- Merkin päivitys-luokat



#### <a name="setup"></a>Asetukset

Azure Mobile välitys määrittäminen on erittäin yksinkertaisia. Kaikki käyttöliittymän liittyvät asiakirjat on käytettävissä Azure Mobile välitys sivustossa, [Voit siirtyä käyttöliittymän](mobile-engagement-user-interface-home.md).

On suositeltavaa, että käynnistät määrittämällä oikean rooleista ja roolien jäsenyys projektin käyttäjille. Näin voit hallita käyttöoikeus platform kaikille käyttäjille. Oman roolit voi olla:

- Järjestelmänvalvojat
- Kehittäjät
- Tarkastelijat 

Tämän jälkeen:
- Voit rekisteröidä oman deviceID Testaa oman laitteessa.
- Tilin asetukset ja määritä haluamasi aikavyöhyke kaavioita ja ilmoituksen toimitusaikaan aikavyöhykkeen määrittäminen.
- Valitse sovelluksen asetukset ja rekisteröi-sovelluksen-tiedot"haluat kohde loppukäyttäjien ulottuvilla.

Lisätietoja siitä, miten voit suorittaa ensimmäisen push-ilmoituksen markkinointikampanjan Tutustu [pikaviestien käyttäminen ja hallinta työntää käyttäjiesi saavuttaminen](mobile-engagement-how-tos.md).



## <a name="conclusion"></a>Tekemistä


Välitys ohjelmat eivät iteratiivinen ja sinun olisi jatkuvasti parantaakseen omasi voit kokeilla mikä toimii parhaiten, kun sovellus. 

Aluksi kehittä strategioita ei yritä muodostaa koko yleinen välitys strategia välitys kokemusta aikana. Ota yhteyttä KPI: T ja miten voit hyödyntää niitä vaihe vaiheelta-vaihtoehto. Välitys strategia on yksilöllinen yksittäisistä sovelluksista.

Kun olet kehittänyt kokemusta kannattaa lisääminen välitys-ohjelmissa seuraavasti:

- Seuranta: Käyttäjät hankkia, ja määrittää tietojen kerääminen lähteistä todennäköisesti. Tietojen kerääminen lähteisiin voidaan linkittää Azure mobile välitys. Sen avulla voit valvoa kunkin toiminnan. Tämä tieto on kiinnostavat Suurenna hankinta sijoitus. 

- A / B testaaminen: Tämä on olennainen osa välitys-ohjelman. Kunkin-sovelluksessa on oma yksityiskohtia. A / B testaaminen, voit parantaa välitys-ohjelmaa.

- GEO-sijainti: Tämä on suuri mahdollisuus-merkkisiä. Tämän ominaisuuden ansiosta voit siirtyä oikeista kohteista ja ajankohtana. Suosittelemme, että olet kerännyt riittävästi käyttäjän toiminnan tietoja ennen kuin alat käyttää geo sijaintia tarkistetaan.

- Tietoja push: tietojen push on näkymätön push. Tietoja push avulla mukauttaminen sovelluksesi peruskäyttäjän toimintatapojesi perusteella. Esimerkiksi jos käyttäjän segmenttiin kuulee usein high-tech tuotteet-sovelluksen omistaja voi lähettää tiedot-push joka mukauttavat hänen kotisivun high-tech sisältöä.



## <a name="next-steps"></a>Seuraavat vaiheet

- [Luo Azure Mobile välitys-tili](mobile-engagement-create.md).
- Käy [Määritä Mobile välitys strategia](mobile-engagement-define-your-mobile-engagement-strategy.md) lisätietoja määrittäminen yrityksen Mobile välitys määrittäminen.



  

<!--Image references-->


<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
