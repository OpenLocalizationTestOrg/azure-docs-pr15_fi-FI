#<a name="azure-service-bus"></a>Azure palvelun Bus

Onko sovelluksen tai palvelun suoritetaan pilveen tai paikallinen, se on usein käsitellä muita sovelluksia ja palveluja. Azure on antamaan laajasti käteviä, voit tehdä tämän palvelun Bus. Tässä artikkelissa kerrotaan, mitä ja miksi voit käyttää sitä teknologian katsaus.

## <a name="service-bus-fundamentals"></a>Palvelun Bus perusteet
Eri tilanteissa Ota viestintä tyylit. Joskus auttaa muita sovelluksia – yksinkertainen jonon viestien lähettäminen ja vastaanottaminen on paras mahdollinen ratkaisu. Muissa tilanteissa tavallisen jonossa ei riitä; jono ja Julkaise ja tilaa järjestelmä on paremmin. Ja joissakin tapauksissa kaikki todella tarvittavan on sovellusten & #151 välinen yhteys; olevien eivät ole pakollisia. Palvelun Bus on kaikki kolme vaihtoehtoa auttaa muita sovellustesi käyttää useilla eri tavoilla.

Palvelun Bus on usean vuokraajan pilvipalveluun, mikä tarkoittaa, että palvelu on jaettu useat käyttäjät. Kunkin käyttäjän kohdalla sovelluskehittäjän, kuten Luo *nimitilan*ja määrittää viestintä-järjestelmiä, hänen on kyseisen nimitilan. [Kuva 1](#Fig1) näkyy, miltä tämä näyttää.

<a name="Fig1"></a>![Azure palvelun Bus kaavio][svc-bus]
 
**Kuva 1: Palvelun Bus on usean vuokraajan palvelun yhteyden sovellusten pilven kautta.**

Nimitilan voit käyttää vähintään yhden esiintymät neljä eri viestintä-järjestelmiä, joista jokaisella muodostaa sovellusten eri tavalla. Vaihtoehdot:

- *Olevien*, jotka mahdollistavat yksi yhteys. Kunkin jonon toimii (kutsutaan joskus *broker*) edustaja, joka tallentaa lähetettyjä viestejä, kunnes ne vastaanotetaan. Yksittäisen vastaanottajan on vastaanottanut viestin.
- *Aiheista*, joissa on yksi suunta tietoliikenteen käyttämällä *tilaukset*-ohjeaiheen voi olla useita tilauksia. Jono, kuten aihe toimii broker, mutta jokaisen tilauksen voit myös käyttää suodatinta vain tietyt ehdot täyttävät viestien.
- *Releitä*, jotka tarjoavat kaksisuuntaisen. Toisin kuin olevien ja ohjeaiheissa välitys ei tallenna tapahtumakartoitus viestit-it's ei broker. Sen sijaan se vain välittää niitä voin kohdesovellus.
- *Tapahtuman keskittimet*, jotka tarjoavat tapahtuma- ja telemetriatietojen tunkeutumisen valtaviin tasolla pilveen pieni viive ja suuri luotettavuutta.

Kun luot jonossa, aiheen, välitys tai tapahtumaa-toiminnossa, voit antaa sille nimen. Yhdistetty riippumatta voit kutsua oman nimitilan nimi Luo objektin yksilöllinen. Sovellusten voit nimetä palvelun Bus sitten toisiinsa, jonossa, aiheen, välitys tai tapahtumaa-toiminnossa avulla. 

Jos haluat käyttää näitä objekteja, Windows-sovellukset voivat käyttää Windows Communication Foundation (WCF). Olevien, aiheet ja tapahtuman keskittimet Windows sovellukset voivat käyttää palvelun Bus määrittämät tekstiviesti API. Nämä objektit-Windows sovellusten käyttäminen helpompaa, Microsoft toimittaa SDK: T Java, Node.js ja muilla kielillä. Voit myös käyttää olevien, aiheet ja tapahtuman keskittimet over HTTP REST API avulla. 

On tärkeää ymmärtää, vaikka palvelun Bus itse suorittaa pilvipalvelussa (eli Microsoft Azure palvelinkeskusten-)-sovelluksia, jotka käyttävät sitä voi käyttää missä tahansa. Voit yhdistää sovellusten Azure, esimerkiksi tai oman palvelinkeskuksen sisällä sovellusten palvelun Bus. Sen avulla voit myös muodostaa sovelluksen Azure tai toiseen käynnissä cloud-ympäristössä paikallisen sovelluksen tai taulutietokoneisiin ja matkapuhelimiin. On myös mahdollista muodostaa kotitalouden laitteet, anturit ja muiden laitteiden keskitetyn sovelluksen tai johonkin muiden. Palvelun Bus on yleinen viestintä järjestelmä pilvipalvelussa, joita voit käyttää melko paljon missä tahansa. Ohjeet käytät määräytyy sovellustesi on suoritettava.


## <a name="queues"></a>Olevien

Oletetaan, että haluat yhdistää kaksi sovellusten palvelun Bus jonossa. [Kuva 2](#Fig2) on kuvattu tämän tilannettasi.

<a name="Fig2"></a>![Palvelun Bus olevien kaavio][queues]
 
**Kuva 2: Palvelun Bus olevien on yksisuuntainen asynkroninen queuing.**

Prosessi on helppoa: lähettäjä lähettää viestin palvelun Bus jonossa ja vastaanotin vastataan viestin joitakin myöhemmin. Jono voi olla vain yksi vastaanotin, [Kuva 2](#Fig2) näkyy tai useita sovelluksia voit lukea saman jonon. Jälkimmäisessä tilanteessa jokaisen viestin lukenut yhden vastaanottaja-usean cast palvelun käytettävä aiheen sijaan.

Viesti on kaksi osaa: ominaisuuksia, joka kunkin avain/arvo-pari ja binaarinen viestin tekstiosaan. Miten niitä käytetään määräytyy sen mukaan, mitä sovellus yrittää tehdä. Esimerkiksi sovelluksen viimeisimmät myyntiä tietoja viestin lähettämistä saattavat sisältää ominaisuuksia *Myyjä = "Käyt"* ja *Summa = 10000*. Viestin tekstiosaan saattaa sisältää skannatusta kuvasta myyntiin allekirjoitetun sopimuksen tai, jos sellaista ei ole, ovat vain tyhjä.

Vastaanotin voit lukea viestin palvelun Bus jonon kahdella eri tavalla. Ensimmäinen vaihtoehto, jota kutsutaan *ReceiveAndDelete*poistaa viestin jonossa ja poistaa sen heti. Tämä on yksinkertaista, mutta jos vastaanotin kaatuu, ennen kuin se päättyy viestin käsittelyn, viestin, menetetään. Koska se on poistettu jonossa, muut vastaanotin voit käyttää sitä. 

Toinen vaihtoehto *PeekLock*tarkoitus on tämän ongelman korjaamiseksi. ReceiveAndDelete, kuten PeekLock luku poistaa viestin jonossa. Se ei poista viestin sijaan. Sen sijaan lukitsee viestin, jolloin näkymätön muita vastaanottajia sitten jokin seuraavista kolmesta tapahtumien odottaa:

- Jos vastaanottaja käsittelee viestin onnistuneesti, kutsuu *valmiina*ja jonossa poistaa viestin. 
- Jos vastaanottaja päättää, että se ei voi käsitellä viestiä onnistuneesti, kutsuu *Hylkää*. Valitse jonossa lukituksen poistaa viestin ja työnkulkuna muita vastaanottajia.
- Jos vastaanottaja soittaa kumpaakaan näistä määritettäviä ajanjakson (oletusarvoisesti 60 sekuntia), jonossa olettaa vastaanottaja on epäonnistunut. Tässä tapauksessa se toimii aivan kuin vastaanottaja oli nimeltään Abandon, tehdä viestin muita vastaanottajia.

Huomaa, mitä voi tapahtua tähän: saman viestin voi toimitetaan kahdesti, esimerkiksi kaksi eri vastaanottajia. Tämä on valmisteltava palvelun Bus olevien sovellusten. Voit helpottaa kaksoiskappaleiden, viesti on yksilöllinen MessageID-ominaisuus, joka oletusarvoisesti pysyy muuttumattomana riippumatta siitä, kuinka monta kertaa viesti on luettu jonon. 

Olevien on hyötyä aivan vähän tilanteissa. Niiden avulla sovellukset viestiä jopa kun molemmat ei ole käynnissä yhtä aikaa, jotain, joka on erityisen hyödyllinen Siirtoerän ja mobile-sovellusten kanssa. Jono, jolla useita vastaanottajia sisältää myös Automaattinen kuormituksen tasaamisen, koska nämä vastaanottajia yli leviävät lähetetyt viestit.


## <a name="topics"></a>Ohjeaiheet

Hyödyllisiä sellaisina kuin ne ovat olevien ei aina oikea ratkaisu. Joskus palvelun Bus ohjeaiheet ovat paremmin. [Kuva 3](#Fig3) on kuvattu tämän verrata.

<a name="Fig3"></a>![Kaavio palvelun Bus aiheet ja tilaukset][topics-subs]
 
**Kuva 3: Perustuvan suodattimen subscribing sovellus määrittää, se vastaanottaa jotkin tai kaikki palvelun Bus aiheen lähetettyihin viesteihin.**

Aiheen eroaa jonon useilla tavoilla. Lähettäjien Lähetä viestejä samalla tavalla kuin he lähettävät viestit jonossa ja viestit aiheen näy samanlaisina olevien kanssa. Suuri ero on ohjeaiheissa antaa kunkin vastaanottava sovellus luo oma tilauksen määrittämällä *Suodatin*. Valitse tilaajan tulee näkyviin vain ne viestit, jotka vastaavat suodatin. [Kuva 3](#Fig3) näkyy esimerkiksi lähettäjä ja aihe ja kolme tilaajille jokaisella on oma suodatin:

- Tilaajan 1 vastaanottaa vain ne viestit, jotka sisältävät ominaisuuden *Myyjä = "Käyt"*.
- Tilaajan 2 vastaanottaa viestit, jotka sisältävät ominaisuuden *Myyjä = "Ruby"* ja/tai sisältää *Summa* -ominaisuuden, jonka arvo on suurempi kuin 100 000. Ehkäpä Ruby on Myyntipäällikkö ja siten hän haluaa nähdä oman myynti- ja kaikki suuri myynti riippumatta siitä, joka tekee niistä.
- Tilaajan 3 on määrittänyt sen suodattimen *Tosi*, mikä tarkoittaa, että se saa kaikkien viestien. Esimerkiksi tämä sovellus voi olla vastaavaa kirjausketju ja näin ollen se on kaikki viestit.

Olevien, jossa aiheen tilaajat voivat lukea viestiä, joissa ReceiveAndDelete tai PeekLock. Toisin kuin olevien kuitenkin yhden viestin aiheen lähetetään voi vastaanottaa useita tilaajille. Tämän menetelmän tunnetaan yleisesti nimellä *Julkaise ja tilaa*, kannattaa käyttää aina, kun useita sovelluksia voivat olla kiinnostuneita samat viestit. Määrittämällä oikean suodattimen kunkin tilaajan napauttamalla heti, että se tarvitsee nähdä viestin virta osaan.


## <a name="relays"></a>Releitä

Olevien ja ohjeaiheissa on yksisuuntainen asynkroninen tietoliikenne broker kautta. Liikenne jatkuu vain yhteen suuntaan ja ei ole suoraa yhteyttä lähettäjien ja vastaanottajien välillä. Mutta Entäpä jos et halua tätä? Oletetaan, että sovellukset on sekä lähettää ja vastaanottaa viestejä, tai ehkä haluat linkin toiseen ja broker viestien tallentamiseen ei tarvita. Osoite, kuten skenaarioiden palvelun Bus tarjoaa releitä, kuten [kuvassa 4](#Fig4) näkyy.

<a name="Fig4"></a>![Palvelun Bus välitys kaavio][relay]
 
**Kuva 4: Palvelun Bus välitys on synkronoitu, kaksisuuntainen viestintä sovellusten välillä.**

Tämä on ilmeisimmät releitä kysyä kysymyksen: Miksi käyttää? Vaikka voin tarvitse olevien, miksi viestiä Pilvipalvelun kautta eikä vain vuorovaikutuksessa suoraan sovellusten tehdä? Vastaus on, että puhelu suoraan voi olla näyttävyyttä kuin mielestäsi ehkä.

Oletetaan, että haluat muodostaa kahden paikallisen sovelluksen, molemmat käynnissä yrityksen palvelinkeskusten sisällä. Palomuurin takana kunkin nämä sovellukset, ja jokainen palvelinkeskuksen käyttää todennäköisesti NAT (NAT). Palomuuri estää kaikki mutta muutaman porttien saapuvat tiedot ja NAT tarkoittaa, että jokainen sovellus on käynnissä tietokoneessa ei ole kiinteä, voit siirtyä suoraan ulkopuolella sen palvelinkeskuksen IP-osoite. Jotkin lisäapua ilman yhteyden nämä sovellukset julkisen Internetin aiheuttaa ongelmia.

Palvelun Bus välitys on tämän ohjeen. Tiedonvälitys bi-sijainnin välitys, kunkin sovelluksen lähtevän TCP-yhteyden kanssa palvelun Bus sitten pitää avoinna. Kaikki viestintä sovellusten välisen siirtyvät nämä yhteyksillä. Koska kukin yhteys muodostettiin sisällä palvelinkeskukseen, palomuurin Salli saapuvan liikenteen kunkin sovelluksen avaamatta uusi portit. Tämän menetelmän saa myös NAT-ongelman, koska Jokaisella sovelluksella on yhtenäinen päätepisteen pilveen koko tietoliikenne. Välityksen tietojen vaihtamalla sovellukset voivat välttää ongelmista, jotka muuten vaikeuttaa viestintä. 

Käyttämään palvelua Bus releitä sovellukset käyttävät Windows Communication Foundation (WCF). Palvelun Bus on WCF sidontojen, joiden ansiosta yksinkertaista Windows-sovellusten Käsittele releitä kautta. WCF jo käyttävät sovellukset voivat yleensä vain jonkin näistä sidontojen, ja puhua toisiinsa kautta välitys. Toisin kuin olevien ja ohjeita käyttämällä-Windows sovelluksessa, kun mahdollista, releitä edellyttää kuitenkin joitakin ohjelmoinnin työmäärään; ei ole vakio kirjastojen toimitetaan.

Toisin kuin olevien ja ohjeita sovellusten ei tarvitse erikseen luoda releitä. Sen sijaan, kun sovellus, joka vastaanottaa viestejä haluaa muodostaa palvelun Bus TCP-yhteyden, välitys luodaan automaattisesti. Kun yhteys on katkaistu, välityksen poistetaan. Etsi tietty kuuntelija luoma välitys sovelluksen kertoaksesi palvelun Bus on rekisterin, jonka avulla voit etsiä tietyn välitys nimelläsi sovelluksia.

Releitä ovat oikean ratkaisun, kun tarvitset suora viestintä sovellusten välillä. Esimerkkinä lentoyhtiön varaus järjestelmän paikalliseen-palvelinkeskukseen, joita käytetään sisäänkuittauksen kioskien, mobiililaitteet ja muiden tietokoneiden käytössä. Kaikki järjestelmät sovellusten voi luottaa pilveen viestintä-palvelun Bus releitä kaikkialla, missä ne voivat olla käynnissä.

## <a name="event-hubs"></a>Tapahtuman keskittimet

Tapahtuman keskittimet on erittäin skaalattava nieltynä järjestelmä, jossa voit käsitellä tapahtumia sekunnissa miljoonia, ottaminen käyttöön sovellusta käsitellä ja analysoida yhdistetyn laitteita ja sovelluksia tuottamat tiedot mahdutettavia. Tapahtuma-toiminnossa avulla voit esimerkiksi live ohjelma suorituskykytietoja kerätään laivaston autoista. Kun kerätä tapahtuman keskittimet, voit transform ja tallentaa tiedot reaaliaikainen analytics-palveluntarjoajan tai tallennustilan klusterin. Saat lisätietoja tapahtuman keskittimet [tapahtuman keskittimet yleiskatsaus][].

## <a name="summary"></a>Yhteenveto

Yhteyden sovellukset on aina valmiina ratkaisujen osa ja skenaariot, jotka tarvitsevat sovellusten ja palveluiden toistensa kanssa kommunikoimiseen alue on määritetty niin, että lisää sovelluksia ja laitteet on yhteydessä Internetiin. Antamalla pilvipohjainen technologies saavuttamiseksi tämä kautta olevien, aiheet tai releitä tapahtuman keskittimet palvelun Bus pyritään avulla olennaiset Tämä funktio on helpompaa toteuttamisesta ja laajemmin käytettävissä.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Tapahtuman keskittimet yleiskatsaus]: https://msdn.microsoft.com/library/azure/dn836025.aspx
