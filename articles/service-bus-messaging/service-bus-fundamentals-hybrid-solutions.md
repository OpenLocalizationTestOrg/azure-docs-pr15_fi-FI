<properties 
    pageTitle="Azure palvelun Bus | Microsoft Azure" 
    description="Johdanto yhteyden muiden ohjelmien Azure sovellusten palvelun Bus avulla." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure palvelun Bus

Onko sovelluksen tai palvelun suoritetaan pilveen tai paikallinen, se on usein käsitellä muita sovelluksia ja palveluja. Microsoft Azure on antamaan laajasti käteviä, voit tehdä tämän palvelun Bus. Tässä artikkelissa kerrotaan, mitä ja miksi voit käyttää sitä teknologian katsaus.

## <a name="service-bus-fundamentals"></a>Palvelun Bus perusteet

Eri tilanteissa Ota viestintä tyylit. Joskus auttaa muita sovelluksia – yksinkertainen jonon viestien lähettäminen ja vastaanottaminen on paras mahdollinen ratkaisu. Muissa tilanteissa tavallisen jonossa ei riitä; jono ja Julkaise ja tilaa järjestelmä on paremmin. Joissakin tapauksissa kaikki todella tarvittavan on sovellusten; välinen yhteys olevien eivät ole pakollisia. Palvelun Bus on kaikki kolme vaihtoehtoa ottaminen käyttöön sovellukset voivat olla vuorovaikutuksessa useilla eri tavoilla.

Palvelun Bus on usean vuokraajan pilvipalveluun, mikä tarkoittaa, että palvelu on jaettu useat käyttäjät. Kunkin käyttäjän kohdalla sovelluskehittäjän, kuten Luo *nimitilan*ja määrittää viestintä-järjestelmiä, hänen on kyseisen nimitilan. Kuva 1 näkyy, miltä tämä näyttää.

![][1]
 
**Kuva 1: Palvelun Bus on usean vuokraajan palvelun yhteyden sovellusten pilven kautta.**

Nimitilan voit käyttää vähintään yhden esiintymät neljä eri viestintä-järjestelmiä, joista jokaisella muodostaa sovellusten eri tavalla. Vaihtoehdot:

- *Olevien*, jotka mahdollistavat yksi yhteys. Kunkin jonon toimii (kutsutaan joskus *broker*) edustaja, joka tallentaa lähetettyjä viestejä, kunnes ne vastaanotetaan. Yksittäisen vastaanottajan on vastaanottanut viestin.
- *Aiheista*, joissa on yksi suunta tietoliikenteen käyttämällä *tilaukset*-ohjeaiheen voi olla useita tilauksia. Jono, kuten aihe toimii broker, mutta jokaisen tilauksen voit myös käyttää suodatinta vain tietyt ehdot täyttävät viestien.
- *Releitä*, jotka tarjoavat kaksisuuntaisen. Toisin kuin olevien ja ohjeaiheissa välitys ei tapahtumakartoitus viestien; tallentaminen se ei ole broker. Sen sijaan se vain välittää niitä voin kohdesovellus.

Kun luot jonossa, aiheen tai välitys, voit antaa sille nimen. Yhdistetty riippumatta voit kutsua oman nimitilan nimi Luo objektin yksilöllinen. Sovellusten voit nimetä palvelun Bus sitten toisiinsa jonossa, aiheen tai välitys avulla. 

Jos haluat käyttää näitä objekteja välitys skenaariota, Windows-sovellukset voivat käyttää Windows Communication Foundation (WCF). Windows-sovellusten käyttää olevien ja ohjeita palvelun Bus määrittämät tekstiviesti API. Nämä objektit-Windows sovellusten käyttäminen helpompaa, Microsoft toimittaa SDK: T Java, Node.js ja muilla kielillä. Voit myös käyttää olevien ja aiheet REST API over HTTP (s) avulla. 

On tärkeää ymmärtää, vaikka palvelun Bus itse suorittaa pilvipalvelussa (eli Microsoft Azure palvelinkeskusten-)-sovelluksia, jotka käyttävät sitä voi käyttää missä tahansa. Voit yhdistää sovellusten Azure, esimerkiksi tai oman palvelinkeskuksen sisällä sovellusten palvelun Bus. Sen avulla voit myös muodostaa sovelluksen Azure tai toiseen käynnissä cloud-ympäristössä paikallisen sovelluksen tai taulutietokoneisiin ja matkapuhelimiin. On myös mahdollista muodostaa kotitalouden laitteet, anturit ja muiden laitteiden keskitetyn sovelluksen tai johonkin muiden. Palvelun Bus on communication järjestelmä pilvipalvelussa, joita voit käyttää melko paljon missä tahansa. Ohjeet käytät määräytyy sovellustesi on suoritettava.

## <a name="queues"></a>Olevien

Oletetaan, että haluat yhdistää kaksi sovellusten palvelun Bus jonossa. Kuva 2 on kuvattu tämän tilannettasi.

![][2]
 
**Kuva 2: Palvelun Bus olevien on yksisuuntainen asynkroninen queuing.**

Prosessi on helppoa: lähettäjä lähettää viestin palvelun Bus jonossa ja vastaanotin vastataan viestin joitakin myöhemmin. Jono voi olla vain yksi vastaanottaja-, kuten kuvassa 2 on. Tai useita sovelluksia voit lukea saman jonon. Jälkimmäisessä tilanteessa viesti on lukea yhden vastaanottajan. Usean cast palvelun käytettävä aiheen sijaan.

Viesti on kaksi osaa: ominaisuuksia, joka kunkin avain/arvo-pari ja binaarinen viestin tekstiosaan. Miten niitä käytetään määräytyy sen mukaan, mitä sovellus yrittää tehdä. Esimerkiksi sovelluksen viimeisimmät myyntiä tietoja viestin lähettämistä saattavat sisältää ominaisuuksia *Myyjä = "Käyt"* ja *Summa = 10000*. Viestin tekstiosaan saattaa sisältää skannatusta kuvasta myyntiin allekirjoitetun sopimuksen tai, jos sellaista ei ole, ovat vain tyhjä.

Vastaanotin voit lukea viestin palvelun Bus jonon kahdella eri tavalla. Ensimmäinen vaihtoehto, jota kutsutaan *ReceiveAndDelete*poistaa viestin jonossa ja poistaa sen heti. Tämä on yksinkertaista, mutta jos vastaanotin kaatuu, ennen kuin se päättyy viestin käsittelyn, viestin, menetetään. Koska se on poistettu jonossa, muut vastaanotin voit käyttää sitä. 

Toinen vaihtoehto *PeekLock*tarkoitus on tämän ongelman korjaamiseksi. **ReceiveAndDelete**, kuten **PeekLock** luku poistaa viestin jonossa. Se ei poista viestin sijaan. Sen sijaan lukitsee viestin, jolloin näkymätön muita vastaanottajia sitten jokin seuraavista kolmesta tapahtumien odottaa:

- Jos vastaanottaja käsittelee viestin onnistuneesti, kutsuu **valmiina**ja jonossa poistaa viestin. 
- Jos vastaanottaja päättää, että se ei voi käsitellä viestiä onnistuneesti, kutsuu **Hylkää**. Valitse jonossa lukituksen poistaa viestin ja työnkulkuna muita vastaanottajia.
- Jos vastaanottaja soittaa kumpaakaan näistä määritettäviä ajanjakson (oletusarvoisesti 60 sekuntia), jonossa olettaa vastaanottaja on epäonnistunut. Tässä tapauksessa se toimii aivan kuin vastaanottaja oli nimeltään **Hylkää**, tehdä viestin muita vastaanottajia.

Huomaa, mitä voi tapahtua tähän: saman viestin voi toimitetaan kahdesti, esimerkiksi kaksi eri vastaanottajia. Tämä on valmisteltava palvelun Bus olevien sovellusten. Voit helpottaa kaksoiskappaleiden, viesti on yksilöllinen **MessageID** -ominaisuus, joka oletusarvoisesti pysyy muuttumattomana riippumatta siitä, kuinka monta kertaa viesti on luettu jonon. 

Olevien on hyötyä aivan vähän tilanteissa. Niiden avulla kommunikoida myös kun molemmat ei ole käynnissä yhtä aikaa, jotain, joka on erityisen hyödyllinen Siirtoerän ja mobile-sovellusten kanssa. Jono, jolla useita vastaanottajia sisältää myös Automaattinen kuormituksen tasaamisen, koska nämä vastaanottajia yli leviävät lähetetyt viestit.

## <a name="topics"></a>Ohjeaiheet

Hyödyllisiä sellaisina kuin ne ovat olevien ei aina oikea ratkaisu. Joskus palvelun Bus ohjeaiheet ovat paremmin. Kuva 3 on kuvattu tämän verrata.

![][3]
 
**Kuva 3: Perustuvan suodattimen subscribing sovellus määrittää, se vastaanottaa jotkin tai kaikki palvelun Bus aiheen lähetettyihin viesteihin.**

*Aiheen* eroaa jonon useilla tavoilla. Lähettäjien Lähetä viestejä samalla tavalla kuin he lähettävät viestit jonossa ja viestit aiheen näy samanlaisina olevien kanssa. Suuri ero on ohjeaiheissa käyttöön kunkin vastaanottava sovellus luominen omassa *tilauksen* määrittämällä *Suodatin*. Valitse tilaajan tulee näkyviin vain ne viestit, jotka vastaavat suodatin. Kuva 3 näkyy esimerkiksi lähettäjä ja aihe ja kolme tilaajille jokaisella on oma suodatin:

- Tilaajan 1 vastaanottaa vain ne viestit, jotka sisältävät ominaisuuden *Myyjä = "Käyt"*.
- Tilaajan 2 vastaanottaa viestit, jotka sisältävät ominaisuuden *Myyjä = "Ruby"* ja/tai sisältää *Summa* -ominaisuuden, jonka arvo on suurempi kuin 100 000. Ruby on ehkä myyntipäällikkö, joten hän haluaa nähdä oman myynti- ja kaikki suuri myynti riippumatta siitä, joka tekee niistä.
- Tilaajan 3 on määrittänyt sen suodattimen *Tosi*, mikä tarkoittaa, että se saa kaikkien viestien. Esimerkiksi tämä sovellus voi olla vastuussa ylläpito kirjausketju ja näin ollen se on kaikki viestit.

Olevien, jossa aiheen tilaajat voivat lukea viestejä tai **ReceiveAndDelete** **PeekLock**avulla. Toisin kuin olevien mutta yksittäisen viestin aiheen lähetetään voi vastaanottaa useita tilauksia. Tämän menetelmän tunnetaan yleisesti nimellä *Julkaise ja tilaa* (tai *kirja ja sub*), kannattaa käyttää aina, kun useita sovelluksia ovat kiinnostuneita samat viestit. Määrittämällä oikean suodattimen kunkin tilaajan voit Hyödynnä säilytettävä osa, että se tarvitsee nähdä viestin-muodossa.

## <a name="relays"></a>Releitä

Olevien ja ohjeaiheissa on yksisuuntainen asynkroninen tietoliikenne broker kautta. Liikenne jatkuu vain yhteen suuntaan ja ei ole suoraa yhteyttä lähettäjien ja vastaanottajien välillä. Mutta Entäpä jos et halua tätä? Oletetaan, että sovellukset on sekä lähettää ja vastaanottaa viestejä, tai ehkä haluat linkin toiseen ja broker viestien tallentamiseen ei tarvita. Osoite, kuten skenaarioiden palvelun Bus tarjoaa *välittää*, kuten kuvassa 4 näkyy.

![][4]
 
**Kuva 4: Palvelun Bus välitys on synkronoitu, kaksisuuntainen viestintä sovellusten välillä.**

Tämä on ilmeisimmät releitä kysyä kysymyksen: Miksi käyttää? Vaikka voin tarvitse olevien, miksi viestiä Pilvipalvelun kautta eikä vain vuorovaikutuksessa suoraan sovellusten tehdä? Vastaus on, että puhelu suoraan voi olla näyttävyyttä kuin mielestäsi ehkä.

Oletetaan, että haluat muodostaa kahden paikallisen sovelluksen, molemmat käynnissä yrityksen palvelinkeskusten sisällä. Palomuurin takana kunkin nämä sovellukset, ja jokainen palvelinkeskuksen käyttää todennäköisesti NAT (NAT). Palomuuri estää kaikki mutta muutaman porttien saapuvat tiedot ja NAT tarkoittaa, että jokainen sovellus on käynnissä tietokoneessa ei ole kiinteä IP-osoite, voit siirtyä suoraan sen palvelinkeskuksen ulkopuolella. Jotkin lisäapua ilman yhteyden nämä sovellukset julkisen Internetin aiheuttaa ongelmia.

Palvelun Bus välitys avulla. Tiedonvälitys bi-sijainnin välitys, kunkin sovelluksen lähtevän TCP-yhteyden kanssa palvelun Bus sitten pitää avoinna. Kaikki viestintä sovellusten välisen kulkee nämä yhteyksillä. Koska kukin yhteys muodostettiin sisällä palvelinkeskukseen, palomuuri antaa saapuvan liikenteen kunkin sovelluksen avaamatta uusi portit. Tämän menetelmän saa myös NAT-ongelman, koska Jokaisella sovelluksella on yhtenäinen päätepisteen pilveen koko tietoliikenne. Välityksen tietojen vaihtamalla sovellukset voivat välttää ongelmista, jotka muuten vaikeuttaa viestintä. 

Käyttämään palvelua Bus releitä sovellukset käyttävät Windows Communication Foundation (WCF) käyttöön. Palvelun Bus on WCF sidontojen, joiden ansiosta yksinkertaista Windows-sovellusten Käsittele releitä kautta. WCF jo käyttävät sovellukset voivat yleensä vain jonkin näistä sidontojen, ja puhua toisiinsa kautta välitys. Toisin kuin olevien ja ohjeita käyttämällä-Windows sovelluksessa, kun mahdollista, releitä edellyttää kuitenkin joitakin ohjelmoinnin työmäärään; ei ole vakio kirjastojen toimitetaan.

Toisin kuin olevien ja ohjeita sovellusten ei tarvitse erikseen luoda releitä. Sen sijaan, kun sovellus, joka vastaanottaa viestejä haluaa muodostaa palvelun Bus TCP-yhteyden, välitys luodaan automaattisesti. Kun yhteys on katkaistu, välityksen poistetaan. Etsi tietty kuuntelija luoma välitys sovelluksen käyttöön palvelun Bus on rekisterin, jonka avulla voit etsiä tietyn välitys nimelläsi sovelluksia.

Releitä ovat oikean ratkaisun, kun tarvitset suora viestintä sovellusten välillä. Esimerkkinä lentoyhtiön varaus järjestelmän paikalliseen-palvelinkeskukseen, joita käytetään sisäänkuittauksen kioskien, mobiililaitteet ja muiden tietokoneiden käytössä. Kaikki järjestelmät sovellusten voi luottaa pilveen viestintä-palvelun Bus releitä kaikkialla, missä ne voivat olla käynnissä.

## <a name="summary"></a>Yhteenveto

Yhteyden sovellukset on aina valmiina ratkaisujen osa ja skenaariot, jotka tarvitsevat keskenään sovellusten ja palveluiden välillä on määritetty niin, että lisää sovelluksia ja laitteet on yhteydessä Internetiin. Palvelun Bus tarkoituksena tarjoamalla pilvipohjainen technologies saavuttamiseksi tämä olevien, aiheet ja releitä avulla olennaiset Tämä funktio on helpompaa toteuttamisesta ja laajemmin käytettävissä.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut Azure palvelun Bus perusteet, noudata näitä linkkejä lisätietoja.

- Opi käyttämään [palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
- Opi käyttämään [palvelua Bus aiheita](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- Opi käyttämään [palvelua Bus välitys](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Palvelun Bus objektit](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
