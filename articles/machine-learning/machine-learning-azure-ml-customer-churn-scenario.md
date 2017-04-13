<properties
    pageTitle="Analysoiminen asiakkaan Churn käyttämällä koneen Learning | Microsoft Azure"
    description="Esimerkkitapaus kehittäminen integroitu mallin, analysointiin ja näkyvissä asiakkaan churn pistemäärä"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Asiakkaan Churn analysoiminen käyttämällä Azure koneen oppiminen

##<a name="overview"></a>Yleiskatsaus
Tässä ohjeartikkelissa esitellään asiakkaan churn analyysi projektin, jotka on luotu Azure koneen Learning Studiossa viittaus soveltaminen. Tässä artikkelissa liittyvät Yleiset mallit holistically teollisuuden asiakkaan churn ongelman ratkaisemiseen. Olemme mitataan tarkkuutta, mallit, jotka on luotu käyttämällä tietokoneen oppiminen ja on arvioitava kehittäminen ohjeita.  

### <a name="acknowledgements"></a>Kuittaussanomien

Tämän kokeen on kehitetty ja testattu Serge Berger, lyhennys tietojen Tiedemies Microsoftin ja Pekka Barga aiemmin tuotteen Manager for Microsoft Azure koneen Learning. Azure asiakirjat-ryhmän gratefully hyväksyy sen niiden osaamisalueet ja Kiitos jakaminen tiedotteessa.

>[AZURE.NOTE] Tämän kokeen käytettäviä tietoja ei ole yleiseen käyttöön. Esimerkki muodostaminen tietokoneen learning mallin churn analysointia varten, katso: [Cortana Liiketoimintatieto](http://gallery.cortanaintelligence.com/) -valikoiman [Telco churn malli-malli](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Asiakkaan churn ongelma
Yritysten kuluttaja markkinoilla ja kaikki yrityksen alat on käsitellä churn. Joskus churn on liikaa ja päätösten vaikuttaa. Perinteinen ratkaisu on suuri vientikapasiteetti churners ennustaa ja niiden tarpeita, markkinointi kampanjat, concierge-palvelun kautta tai lisäämällä määräten erivapautta osoite. ‑Ominaisuuksia voivat vaihdella alan teollisuuden ja jopa tietyn kuluttaja-klusterin toiseen yhden alan (esimerkiksi telecommunications).

Yhteinen tekijä on, että yritysten Pienennä nämä määräten asiakkaan säilytys tehokkuutta. Näin ollen luonnollinen kehitysmenetelmistä olisi sija kaikkien asiakkaiden kanssa churn todennäköisyyden ja osoitteen ylimmät N niistä. Parhaat asiakkaat voivat olla voitollisimpien niistä; esimerkiksi kehittyneempiä tilanteissa, tuotto-funktio työskentelee määräten erivapautta hakevien valinnan aikana. Seuraavat seikat huomioon on kuitenkin churn myyminen kokonaisvaltaiseksi strategia osa. Yritysten on myös huomioon tilin riski (ja liittyvät risk poikkeama) taso ja toimia ja luottamuksellisuudelle asiakkaan Segmentointi kustannuksiksi.  

##<a name="industry-outlook-and-approaches"></a>Teollisuuden Outlookin ja tavoista
Tyylikkään käsittely churn on kehittynyt teollisuuden merkki. Perinteinen esimerkki on televiestinnän alan, jossa tilaajille tiedetään siirtyminen usein yksi toimittaja toiseen. Tämä vapaaehtoinen churn on ensisijainen huolenaiheita. Lisäksi tarjoajat on kertynyt merkittäviä tietosi *churn ohjaimet*, jotka ovat tekijöiden siirtyä asema, että asiakkaat.

Esimerkiksi luuri tai laitteen valinta on tunnetun ohjain, churn matkapuhelimen Business-palvelussa. Suositut käytäntö on tuloksena tukea maahuolintaan liittyvää luuri hinta uusi tilaajille ja charging koko hinnan päivitys asiakkaille. Käytäntö on perinteisesti johtanut asiakkaille hopping yhden tarjoajalta toiseen saadaksesi uuden alennuksen, joka puolestaan on pyytää tarjoajat tarkentaa niiden strategioita.

Suuri haihtuvuus kuuloke-versioiden on kerroin, joka peruuttaa nopeasti churn mallit, jotka perustuvat nykyisen luuri mallit. Lisäksi matkapuhelimet eivät ole vain tietoliikenteen laitteita; ne ovat myös sellaisten lausekkeita (harkitse iPhonen) ja nämä sosiaalisen predictors ulkopuoliselle säännöllisesti televiestinnän tietojoukot aluetta.

Mallinnus nettonykyarvon tulos on, että et voi suunnittelemaan äänen käytännön yksinkertaisesti poistamalla churn tunnetut syistä. Itse asiassa jatkuva mallinnus strategia, mukaan lukien perinteinen tietomalleja, joissa categorical muuttujat (kuten päätös puita), muuta on **pakollinen**.

Käytät suurten tietojoukkojen asiakkaidensa, organisaatiot suorittamassa big datasta analytics (erityisesti churn tunnistus suuri tietojen perusteella) kuin tehokkaita toimintatavan ongelma. Voit etsiä lisätietoja big datasta lähestymistapa ETL osan suositukset churn ongelman.  

##<a name="methodology-to-model-customer-churn"></a>Mallin asiakkaan churn menetelmät
Yleisten ongelmien ratkaisemiseen prosessin asiakkaan churn ratkaisemiseen on edellisessä luvut 1 – 3:  

1.  Riski-mallin avulla voit ottaa huomioon, miten toiminnot vaikuttavat todennäköisyys ja risk.
2.  Toimia-mallin avulla voit ottaa huomioon, miten toimia taso-syötteestä voi vaikuttaa churn ja asiakkaan määrän todennäköisyyden elinaika-arvon (CLV).
3.  Tämä analyysi kohdealueella laatua koskevan analyysin, joka on siirretty ennakoiva markkinointikampanjan, joka sopii aikana optimaalisen tarjous asiakkaalle osia.  

![][1]

Välitä näköisen tämä vaihtoehto on paras tapa käsitellä churn, mutta se sisältää monimutkaisuus: usean mallin archetype ja mallien väliset riippuvuudet jäljitys on. Vuorovaikutuksen kesken malleja voi EPS seuraavassa kaaviossa esitetyllä tavalla:  

![][2]

*Kuva 4: Yhdistetty usean mallin archetype*  

Mallien välisen vuorovaikutuksen on avain, jos emme pitää kokonaisvaltaiseksi lähestymistavan asiakkaan säilytys. Jokainen malli heikentää välttämättä ajan myötä; arkkitehtuuri siksi implisiittisesti silmukan (muistuttaa archetype määrittämiä TERÄVÄ DM tietojen louhinta standardia, [***3***]).  

Riskin päätös-kaupan Segmentointi/hajotus yleinen jakson on edelleen generalized rakenteeseen, joka on käytettävissä useita yrityksen ongelmat. Churn analyysi on vain tämän ryhmän ongelmia vahva edustaja, koska se poistettu käytöstä käytetään kaikkien monimutkaista liiketoiminta-ongelman, joka ei salli yksinkertaistettu ennakoivan ratkaisu piirteitä. Nykyaikainen hallintatavan churn sosiaalisia ominaisuuksia ei erityisen korostetut lähestymistapa, mutta sosiaalisia ominaisuuksia EPS-mallinnus, archetype, kun ne on kaikki mallin.  

Tähän kiinnostavat lisäys on big datasta analytics. Kuluvan päivän tietoliikenteen ja jälleenmyynti yritysten kerätä monipuolisen niiden asiakkaiden tietoja ja olemme ennustaa helposti edellyttää moniarvoista mallin yhteyksien tulee yleisiä trendin, annettu tulevia trendejä kuten asioita Internet- ja laajasti käytetty laitteita, jotka mahdollistavat business käyttää smart ratkaisujen kerrosta.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Käyttöönoton mallinnus archetype Machine Learning Studiossa
Valita vain kuvattu ongelma, mikä on paras tapa integroitu mallinnus ja näkyvissä pistemäärä lähestymistapa? Tässä osassa on esitetty, miten Microsoft kirjautumalla tämä Azure koneen Learning Studiossa.  

Usean mallin vaihtoehto on on suunniteltaessa yleinen archetype churn varten. Vaikka tulosmalli (ennakoivan) osa lähestymistapa pitäisi olla usean malli.  

Seuraavassa kaaviossa on esitetty prototyypin luomaamme, joka käyttää tietokoneen Learning Studiossa ennustaa churn neljä tulosmalli algoritmeista. Syy käyttämiseen usean mallin lähestymistapa ei ole vain, jos haluat luoda ensemble-valitsin parantaa tarkkuutta, mutta myös suorittaa liiallinen asennusvaatimukset ja parantaa ominaisuuksien ominaisuudet.  

![][3]

*Kuva 5: Prototyypin, churn, mallinnus lähestymistapa*  

Seuraavissa osissa on lisätietoja näkyvissä pistemäärä malli, joka on toteutettu koneen Learning Studiossa prototyypin.  

###<a name="data-selection-and-preparation"></a>Tietojen valitseminen ja valmistelu
Käyttää mallien luominen ja tulos asiakkaiden saatiin CRM pystysuunnassa-ratkaisun, salatut asiakkaan turvaavia tiedoilla. Tiedot sisältävät tietoja 8 000 tilaukset USA: ssa ja se yhdistää kolme lähteet: valmistelu tiedot (metatiedot tilaus), tehtävätietojen (järjestelmä käyttö) ja tukea asiakastiedot. Tietoja ei ole mitään business liittyviä tietoja asiakkaista; esimerkiksi se ei ole kanta metatiedot tai luottokortin tulosten.  

Yksinkertaisuuden ETL ja tietojen puhdistus prosessit ovat alueen ulkopuolella koska oletetaan, että tietojen valmisteleminen on jo suoritettu muualla.   

Ominaisuudet mallinnus perustuu alustava tarkkuus lasketaan predictors, prosessi, joka käyttää satunnaisia metsää moduulin sisältyy joukko. Koneen Learning Studiossa toteutuksen on laskettu keskiarvo, Mediaani ja tulostusalueet edustavan ominaisuuksia. Esimerkiksi on lisätty laadun tietoja, kuten vähimmäis- ja arvot käyttäjän koosteet.    

On myös siepata ajallinen viimeisimmän kuuden kuukauden tiedot. Olemme analysoida tietoja vuosi ja on muodostettu, vaikka käytettävissä on tilastollinen merkittäviä trendien, churn vaikutus huomattavasti heikkenee kuuden kuukauden jälkeen.  

Tärkeimmät piste on, että koko prosessin ETL-ominaisuudet, mukaan lukien ja mallinnus on toteutettu koneen Learning Studiossa käyttämällä Microsoft Azure tietolähteet.   

Seuraavissa kuvissa esitetään tiedot, joita on käytetty.  

![][4]

*Kuva 6: Ote tietolähteen (salatut)*  

![][5]


*Kuva 7: Ominaisuuksia poimittujen tietolähde*
 
> Huomaa, että tiedot on yksityinen ja sen vuoksi mallin ja tietoja ei voi jakaa.
> Kuitenkin samalla mallin julkisesti saatavilla olevien tietojen avulla, katso tässä esimerkissä kokeilla [Cortana liiketoimintatietojen valikoima](http://gallery.cortanaintelligence.com/): [Telco asiakkaan Churn](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Lisätietoja siitä, miten voit toteuttaa käyttämällä Cortana liiketoimintatietojen Suite churn analysis-mallin, on suositeltavaa myös [Tässä videossa](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) vanhempi Järjestelmänhallinnan Wee Hyong Tok mukaan. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Valitse prototyypin algoritmeista

Neljä koneen learning-algoritmien avulla luoda prototyypin (ei mukauttaminen):  

1.  Logistista regressiota (LR)
2.  Tehosti Päätöspuun (BT)
3.  Pisteiden perceptron (AP)
4.  Tuen vector machine (SVM)  


Seuraavassa kaaviossa on kuvattu kokeen suunnitteluosaan, joka ilmaisee järjestyksen, jossa mallit on luotu osa:  

![][6]  


*Kuva 8: Luominen mallien koneen Learning Studiossa*  

###<a name="scoring-methods"></a>Näkyvissä menetelmiä pistemäärä
Olemme tulos neljä mallien avulla nimetty koulutus tietojoukko.  

On myös lähettää tulosmalli tietojoukko vastaavaa mallia käyttämällä SAS yrityksen tietojenhakutoiminnon 12 paikallisen version. Olemme mitattuna tarkkuudella SAS-malli ja kaikki neljä koneen Learning Studio mallit.  

##<a name="results"></a>Tulokset
Tässä osassa on esitä Microsoftin päätelmien tietoja mallien perusteella tulosmalli tietojoukko tarkkuudella.  

###<a name="accuracy-and-precision-of-scoring"></a>Tarkkuutta ja tarkkuus näkyvissä pistemäärä
Yleensä Azure koneen Learning käyttöönoton on takana SAS tarkkuutta noin 10-15 % (alue-kohdassa kaari- ja AUC).  

Tärkeimmät metrijärjestelmä-churn on kuitenkin luokitteluvirhe korko: eli, ylimmät N churners kuin budjetoidut valitsimen mukaan, minkä niistä todella asentaminen **ei** churn ja vielä vastaanotettu erityinen käsittely? Seuraavassa kaaviossa vertaa kaikki mallien luokitteluvirhe tämä kurssi:  

![][7]


*Kuva 9: Passau prototyypin pinta käyrä*

###<a name="using-auc-to-compare-results"></a>AUC avulla voit verrata tuloksia
Alueen kohdassa käyrän (AUC) on arvo, joka edustaa yleinen mittayksikkö *Erotettavuus* positiivisia ja negatiivisia populaatio tulosten jaot välillä. On samanlainen kuin perinteisen vastaanotin operaattori säännössä käytettävän ominaisuuden (OHJETIEDOSTO)-kaavio, mutta yksi tärkeä ero on, että AUC metrijärjestelmä ei edellytä sopivat raja-arvo. Sen sijaan se luo yhteenvedon tulokset **kaikki** mahdolliset vaihtoehdot päälle. Sen sijaan perinteinen OHJETIEDOSTO kaavio näyttää positiiviset korko pystyakselin ja EPÄTOSI positiivinen korko vaaka-akselilla ja luokittelu raja-arvo vaihtelee.   

AUC käytetään yleensä mittaa sitä, eri algoritmit (tai eri järjestelmien), koska se sallii mallit, jota verrataan AUC-arvojen avulla. Tämä on Suositut lähestymistapa erityisesti sääolojen seuranta ja biosciences. Näin ollen AUC edustaa Suositut työkalu, jolla arvioidaan valitsimen suorituskykyä.  

###<a name="comparing-misclassification-rates"></a>Vertailu luokitteluvirhe vaihtelevasti.
Olemme verrattuna poistettavaan tietojoukko luokitteluvirhe-kurssit noin 8 000 tilauksista CRM-tietojen avulla.  

-   SAS luokitteluvirhe korko on 10 – 15 %.
-   Koneen Learning Studio luokitteluvirhe oli ensimmäiset 200 – 300 churners 15-20 %.  

Televiestinnän, alaan on tärkeää osoite vain asiakkaat, joilla on suurin riski churn tarjoamalla ne concierge-palvelun tai muu erityinen käsittely. Suhteessa koneen Learning Studio käyttöönoton kertyy tulokset on par with SAS-malli.  

Tarkkuus on tulostusjälkeä tärkeämpi tarkkuus on samoja token koska emme pääosin kiinnostunut luokittelussa oikein mahdollisten churners.  

Wikipedia-seuraavassa kaaviossa on esitetty muotoiluja sisältävässä muodossa, helppoa ymmärtää kuvan suhteen:  

![][8]

*Kuva 10: Sopivat tarkkuutta ja tarkkuus*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Tehosti päätös puun mallin tarkkuutta ja tarkkuus tulokset  

Seuraavassa kaaviossa näkyvät näkyvissä pistemäärä koneen Learning prototyypin käytön tehosti päätös puun mallista, joka eniten vaati neljä mallien välillä tapahtuu raaka tuloksista:  

![][9]

*Kuva 11: Tehosti päätös puun mallin ominaisuudet*

##<a name="performance-comparison"></a>Suorituskyvyn vertailu
Olemme verrattuna nopeutta, jolla tiedot on tulos koneen Learning Studio-malleja ja vastaa mallin luotu SAS yrityksen tietojenhakutoiminnon 12.1 paikallisen version avulla.  

Seuraavassa taulukossa on yhteenveto algoritmit suorituskyvyn:  

*Taulukko 1. Yleiset suorituskykyyn (tarkkuus) algoritmeista*

| LR|BT|AP|SVM|
|---|---|---|---|
|Keskiarvo-malli|Paras malli|Myynti alittanut tavoitetason|Keskiarvo-malli|

Koneen Learning Studio outperformed SAS 15-25 % nopeuden suorittamisen, mutta tarkkuutta oli pääosin nimellisarvo ylläpidettävä mallit.  

##<a name="discussion-and-recommendations"></a>Keskustelun ja suositukset
Televiestinnän, alaan useita käytännöt on kulutusluottomuotoja analysointiin churn, mukaan lukien:  

-   Johdettu mittaukset neljä ratkaisevan luokat:
    -   **Kohde (esimerkiksi tilauksen)**. Valmistele perustietoja tilaus ja/tai asiakkaan, joka on churn aihe.
    -   **Tehtävän**. Hanki kaikki mahdolliset käyttötietoja, jotka liittyvät kohteen, esimerkiksi kirjautumiset määrä.
    -   **Asiakastukeen**. Sadonkorjuun tiedot asiakkaan tuki lokien osoittamaan tilaukseen, joka oli ongelmia tai aktiviteettien asiakastukeen.
    -   **Kilpailuun ja yrityksen tiedot**. Hankkia tietoja mahdollinen asiakas (esimerkiksi voi olla vaikea seurata tai pois käytöstä).
-   Käytä tärkeä asema ominaisuudet. Tämä tarkoittaa sitä, että tehosti päätös puun malli on aina antamalla lupauksia lähestymistapa.  

Neljä luokkiin käyttö luo vaikutelma, jotka yksinkertainen *deterministic* lähestymistavan mukaan indeksit muotoiltu suositeltavaa kerralla tekijät kohti luokan perusteella pitäisi riittää asiakkaiden churn vastuullasi tunnistamiseen. Vaikka tämä käsite vaikuttaa luottamuksellisuudelle, se on valitettavasti EPÄTOSI tietoja. Syy on, että churn on ajallinen tehoste ja tekijöitä, jotka vaikuttavat churn ovat yleensä lyhytkestoisia hyötyä. Mitä liidit asiakkaan kannattaa jättää tänään voivat olla erilaisia huomenna, ja se varmasti päivitetään eri kuusi kuukautta päästä. Tämän vuoksi *probabilistic* malli on välttämätöntä.  

Tämä tärkeä huomautus usein tekemättä jäänyt business-palvelua, johon liiketoimintatietojen aloittaminen toimintatavan haluaa yleensä tunnin, lähinnä, koska se on helppokäyttöisempi myydä voi osallistua vain yksinkertaisia automaatio.  

Omatoiminen analytics Konepohjaisten Oppimistekniikoiden Studiossa lupauksen on kuitenkin neljä tietoluokkien, osasto, joita luokiteltava muuttuvat koneen liittyviä churn arvokkaita lähde.  

Toisen mielenkiintoinen ominaisuus Azure koneen Learning tulossa on mahdollista lisätä mukautetun moduuli käytettävissä olevat ennalta määritetyt moduulit säilöön. Tätä ominaisuutta, Luo mahdollisuus valita kirjastojen ja luoda malleja pystysuora markkina-alueet. Markkinoilla Azure koneen opiskelun tärkeää differentiator on.  

Toivottavasti myöhemmin, jatka tämän ohjeaiheen liittyvät erityisesti big datasta analytics.
  
##<a name="conclusion"></a>Tekemistä
Tässä asiakirjassa kerrotaan järkevää hallintatavan tasa asiakkaan churn yleinen ongelma käyttämällä yleinen kehys. Syy pidettäviä prototyypin näkyvissä pistemäärä mallit ja toteutettu Azure koneen Learning avulla. Lopuksi on arvioitava tarkkuutta ja suorituskyvyn prototyypin ratkaisun SAS vastaa algoritmit osalta.  

**Lisätietoja:**  

Tässä asiakirjassa avulla voit? Anna meille palautetta. Kerro 1 (heikko) asteikolla 5 (erinomainen), miten voit arvostella tässä asiakirjassa ja miksi voit annetut se luokitus? Esimerkki:  

-   Ovat voit arviointi sen vuoksi on hyvä esimerkkejä, koulutuskurssien näyttökuvien, poista kirjoittaminen tai muusta syystä hyvin?
-   Ovat voit arviointi sen pienen vuoksi heikko esimerkkejä, epäselvältä näyttökuvia tai epäselvä kirjoittamista?  

Palaute auttaa meitä parantamaan laatua, Vapauta on kokoelma.   

[Lähetä palautetta](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Viittaukset
[1] ennakoivan Analytics: lisäksi ennusteiden, W. McKnight tietojen hallinta-heinäkuussa/elokuun 2011 p.18 – 20.  

[2] Wikipedia-artikkelista: [tarkkuutta ja tarkkuus](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [TERÄVÄ-DM 1.0: vaiheittaiset ohjeet tietojen louhinta opas](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big datasta markkinointi: osallistuminen asiakkaille tehokkaammin ja arvoa](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco churn mallin mallin](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana Liiketoimintatieto](http://gallery.cortanaintelligence.com/) -valikoimassa 
 
##<a name="appendix"></a>Lisäys

![][10]

*Kuva 12: Esityksen churn prototyypin tilannevedoksen*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
