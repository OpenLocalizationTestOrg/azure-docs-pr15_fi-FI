<properties
   pageTitle="5 tietojen tiede kysymykset - tietojen tiede aloittelijoille | Microsoft Azure"
   description="Hae tiedot tiede lyhyt esittely-tietojen tiede aloittelijoille viisi lyhyissä videoissa, jotka alkavat 5 kysymyksiä tietojen tiede vastauksia."
   keywords="Näin tietojen tiede, tietojen tiede aloittelijoille, tietojen tiede aloittelijoille, kysymyksiä, tietojen tiede kysymysten ja tietojen tiede video tyypit"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Tietoja tiede aloittelijoille video 1: 5 tietojen tiede vastauksia kysymyksiin

Pyydä lyhyt esittely tietojen tiede- *Tietojen tiede aloittelijoille* viisi lyhyiden videoiden yläreunan tietojen Tiedemies. Näissä videoissa on basic, mutta se on hyödyllinen, käyttämällä tekevät tietojen tiede ja voit käsitellä tietoja tutkijoiden.

Ensimmäinen Tämä video on kysymyksiä, joihin tiedot tiede voi vastata, millaisia tietoja. Pääset tehokkaasti sarjan katsoa ne kaikki. [Siirry videoiden luettelo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## <a name="other-videos-in-this-series"></a>Muita videoita sarjassa

*Tietoja tiede aloittelijoille* on lyhyt esittely tietojen tiede kestää noin 25 minuuttia yhteensä. Tutustu muihin neljä videot:

  * Video 1: 5 tietojen tiede vastauksia kysymyksiin
  * Video 2: [tiedoissa on valmiina tietojen tiede?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 minuuttia 56 sekuntia)*
  * Video 3: [Kirjoita kysymys voi vastata tiedoilla](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 s)*
  * Video 4: [Ennustaa vastaus yksinkertainen malli](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 s)*
  * Video 5: [Muiden käyttäjien työn alla tietojen tiede kopioiminen](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 minuuttia 18 sekuntia)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Tallenne: 5 tietojen tiede vastauksia kysymyksiin

Moikka! Tervetuloa käyttämään videosarja *Tietojen tiede aloittelijoille*.

Tietojen tiede voi uhkaava, jotta voin oma perusteet tähän ilman kaavoja tai tietokoneen ohjelmointi ammattikieli.

Tämän ensimmäisen videon käsittelemme "5 kysymyksiin tietojen tiede vastauksia."

Tietoja tiede käyttää numeroilla ja nimillä (tunnetaan myös nimellä luokat tai otsikot) ennustaa vastauksia kysymyksiin.

Se saattaa poistoputkeen, mutta *on vain viisi kysymyksiä, tietojen tiede vastauksia*:

  * Onko tämä A ja B?
  * Onko tämä Oudot?
  * Kuinka paljon – tai – kuinka monta?
  * Miten tämä järjestetyt?
  * Mitä teen seuraavaksi?

  Yksitellen näihin kysymyksiin vastataan erillisessä perheen tietokoneen learning tavoista kutsutaan algoritmeista.


Kannattaa ottaa huomioon algoritmin kuin ohjeen ja tietojen olevina. Algoritmin kerrotaan, miten voidaan yhdistää ja sekoitetaan tiedot, jotta saat vastauksen. Tietokoneissa on kuin Blenderissä. Samannäköisinä useimmat algoritmin työtä, ja ne tee melko nopeasti.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Kysymys 1: Tarkoittavat tämän A ja B? käyttää luokitus algoritmit

Aloitetaan kysymykseen: Tämä A ja B on?

![Luokitus algoritmit: Tämä A ja B on?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Tämä perheen algoritmeista kutsutaan kahden luokan luokitus.

Se on hyödyllinen kysymyksen, joka sisältää vain kaksi vastausvaihtoehtoa.

Esimerkki:

  * Tämä tire epäonnistuu-seuraavan 1 000 Mailia: Kyllä tai ei?
  * Joka yhdistää useita asiakkaat: $5 kuponki tai 25 % alennuksen?

Tämä kysymys voi myös rephrased sisältämään enemmän kuin kaksi vaihtoehtoa: on tämän A tai B tai C ja D, jne.?  Tätä kutsutaan multiclass luokittelu ja se on hyödyllinen on useita, tai useita thousand – vastausvaihtoehtoa. Multiclass luokitus valitsee todennäköisesti jokin.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Kysymys 2: Tämä on Oudot? käyttää poikkeavuuksista tunnistus algoritmit

Tietoja tiede voi vastata Seuraava kysymys on: Tämä oudosti on? Perheen algoritmeista kutsutaan poikkeavuuksista tunnistus tämän kysymykseesi.

![Poikkeavuuksista tunnistus algoritmit: Tämä oudosti on?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Jos sinulla on luottokortti, sinun on jo benefitted-poikkeavuuksista tunnistus. Luottokortin analysoi osto-kuvioiden niin, että ne ilmoita sinulle mahdollista ilmoituksen. Kulujen, jotka ovat "Oudot" ehkä osto säilön, johon sinun ei yleensä ostoksia tai ostamalla kuluisi pricey kohteen.

Tämä kysymys voi olla hyötyä monilla tavoilla. Esimerkiksi:

  * Jos sinulla on auton Paineanturit kanssa, haluat ehkä tietää: tämän painemittari lukeminen Normaali on?
  * Jos olet seuranta Internetissä, kannattaa tietää: Tämä sanoma Internetistä on tyypillinen?

Poikkeavuuksista tunnistus merkitsee odottamattomia tai epätavallinen tapahtumia tai toiminta. Vihjeitä antaa where Tarkista ongelmien varalta.



## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Kysymys 3: Kuinka paljon? tai kuinka monta? käyttää regression algoritmit

Koneen learning voi myös ennustaa vastauksen kuinka paljon? tai kuinka monta? Algoritmin perhe, kysymykseen vastataan kutsutaan regression.

![Regressiosuoran algoritmit: kuinka paljon? tai kuinka monta?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Regressiosuoran algoritmit tehdä numeerisia ennusteiden

  * Mikä lämpötila on seuraava tiistai?  
  * Mitä omien neljännen vuosineljänneksen myynti on?

Ne helpottavat minkä tahansa kysymykseen, jossa kysytään, määrälle.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Kysymys 4: Miten tämä järjestetään? käyttää klusterointi algoritmit

Kaksi kysymykset ovat nyt kehittyneempiä vähän.

Joskus haluat tietojoukon - rakenteen tietoja siitä, miten tämä on järjestetty? Kysymyksen sinulla ei ole esimerkkejä, jotka tiedät tulokset.

On useita tapoja tease ulos olevien tietojen rakennetta. Yksi tapa on klusterointi. Sen erottamat tietojen tuominen luonnollinen "clumps," helpompaa tulkinnassa. Asentaminen, on yksi oikean vastausta.

![Klusterointi algoritmit: miten tämä on järjestetty?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Yleisiä esimerkkejä klusteroinnin kysymyksiä:

  * Mitä tarkastelijat näyttää samaan tapaan kuin elokuvat?
  * Tulostimen malleja epäonnistui samalla tavalla?

Ymmärrät, miten tiedot on järjestetty, voit paremmin ymmärtää - ja ennustaa - toiminnan ja tapahtumia.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Kysymys 5: Mitä teen nyt? käyttää raudoitustöiden algoritmit oppiminen

Viimeisin kysymys – Mitä teen nyt? – käyttää perhe kutsutaan raudoitustöiden learning algoritmeista.

Raudoitustöiden learning on tuloksena mukaan, miten etäkäyttötroijalaiset ja ihmisiin laboratoriotutkimukseen vastata rankaiseminen ja edut. Näiden algoritmien tulosten oppiminen ja päättää seuraavan toiminnon.

Yleensä raudoitustöiden oppiminen on hyvin ratkaistavaan automaattinen järjestelmiin, joissa on paljon pieni päätöksiä ilman ihmisten ohjeita.

![Koulujen algoritmit raudoitustöiden: Mitä minun pitäisi tehdä seuraavaksi?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Se on vastauksia kysymyksiin ovat aina, mitä tietoja on otettava - yleensä koneen tai robotti. Esimerkkejä:

  * Jos olen talon lämpötilan ohjausobjektin järjestelmä: Säädä lämpötila tai jätä se on?  
  * Jos olen itse ajo Auto: keltainen valo jarrun tai nopeuttamiseksi?  
  * Saat robotti tyhjiö: pidä vacuuming tai palaa lataamiseen aseman?

Raudoitustöiden learning algoritmit kerätä tietoja, kun ne siirtyä, kannattaa tutustua kokeiluversio ja virheestä.

Jotta siinä - 5 kysymyksiä tietojen tiede voi vastata.



## <a name="next-steps"></a>Seuraavat vaiheet

  * [Kokeile ensimmäisessä tietojen tiede kokeessa koneen Learning Studiossa](machine-learning-create-experiment.md)
  * [Hanki Microsoft Azure koneen Learning esittely](machine-learning-what-is-machine-learning.md)
