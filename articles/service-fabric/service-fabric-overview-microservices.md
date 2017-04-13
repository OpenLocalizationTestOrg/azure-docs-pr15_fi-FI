<properties
   pageTitle="Tietoja microservices | Microsoft Azure"
   description="Miksi rakennuksen cloud microservices lähestymistavan sovellusten yleiskatsaus on tärkeää Moderni sovellusten kehittämisen ja miten Azure palvelun kangasta on tätä ympäristö"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="mfussell"/>

# <a name="why-a-microservices-approach-to-building-applications"></a>Miksi microservices lähestymistapa sovellusten?
Kuin ohjelmistokehittäjät ei ole mitään uudet miten olemme huomioon otettavia asioita käyttötilejä sovelluksen osan osiin. Objektin suunta, ohjelmiston vedenoton ja componentization keskitetyssä koaksiaalikaapeliin on. Nykyään tämän factorization yleensä tulevat luokat ja liittymät jaettuja kirjastoja ja tekniikka kerrokset välillä. Yleensä Porrastettu lähestymistapa otetaan taustatietokantaan kaupan, keskimmäisen liiketoimintalogiikan ja edusta Käyttöliittymän. Mikä *on* muuttunut viimeisen muutaman vuoden on, että esimerkissä-kehittäjille kuin luodaan pilvipalveluun, yrityksen perustuva välimuisti.

Muuttuvien liiketoimintatarpeiden ovat seuraavat:

- Muodosta ja toimivat asteikko palvelusta päästäkseen asiakkaiden uusi maantieteellisillä alueilla (esimerkiksi).
- Nopeampi lähettämisen ominaisuuksia ja toimintoja, jotka voivat vastata asiakkaan vaatimukset joustava tavalla.
- Parannettu resurssien käytön pienentämiseksi.

Nämä liiketoimintatarpeiden vaikuttavat *miten* on luoda sovelluksia.

Lisätietoja Azure's hallintatavan microservices [Microservices: pilveen tarjoaa sovelluksen-vallankumous](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monoliittiset ja microservice rakenne lähestymistapa
Kaikki sovellukset kehityttävä ajan kuluessa. Onnistuneiden sovellusten kehityttävä ole hyödyllisiä henkilöille. Epäonnistuneen sovellukset ei kehittävät ja tekstin on poistettu. Kysymyksen muuttuu: kuinka paljon tiedät tietoja tarpeen tänään, ja mitä ne päivitetään myöhemmin? Esimerkiksi oletetaan luotavaa raportoinnin sovelluksen osaston. Olet varma sovelluksen pysyy puitteissa yrityksesi ja olla lyhytkestoinen raportit. Valintasi lähestymistapa on erilainen kuin, Yammer-palvelu välittää videosisältöä asiakkaiden miljoonia kymmenlukuun luomisesta. Joskus käytön jotakin kohdassa ovi osoituksena, joka on ajo kerroin siten, että sovellus on suunniteltu myöhemmin. On pieni pisteen suunnittelu perusteettomasti jotain, joka saa käyttänyt. On tavallista suunnitteluryhmät trade-off. Toisaalta, kun yritysten keskustella pilveen rakennuksen, se kasvu ja käyttö. Ongelma on kasvu ja skaalausta ovat odottamattomat. Haluamme voivat prototyypin nopeasti aikana tietää myös, että olemme tulevien success käsitellä reitillä. Tämä on lean käynnistys toimintatapa sen vuoksi: luoda mitata tietoja, käytöstä.

Asiakas-palvelin, ajanjakson aikana olemme hoideta keskittyä Porrastettu sovellusten luominen käyttämällä tietyn technologies kunkin tason. Termin "Monoliittiset"-sovellus on kulutusluottomuotoja ‑ominaisuuksia varten. Liityntäkohdat hoideta että tasojen välissä olevan ja useita kytketty tiiviisti rakenne käytetään kunkin tason osia välillä. Kehittäjät suunniteltu ja monimenetelmäinen luokat kirjastot käännetään muutaman suoritettavat ja dll linkittää toisiinsa. Liittyy etuja Monoliittiset rakenne-menetelmän. Se on usein yksinkertaisempi suunnitteluun ja on nopeampaa puhelut osien, koska nämä kutsut ovat usein IPC päälle. Myös kaikkien Testaa yhden tuote, joka on Lisää henkilöitä-resurssien tehokkaan yleensä. Entä huonot puolet on Porrastettu kerrokset tulokset ja välinen tiivis kytkentä ei skaalaudu yksittäisiä osia. Jos haluat suorittaa korjauksia tai päivityksiä, sinun tarvitse odottaa muiden Viimeistele niiden testaaminen. On vaikeaa joustava.

Microservices osoite nämä downsides ja lisää tasata tarkasti edellisen business vaatimusten, mutta ne on myös upsides ja downsides. Microservices eduista on, että kukin kapseloi yleensä yksinkertaisempi business-toiminto, jota skaalata ylös tai alas-testi, ottaminen käyttöön ja hallita erikseen. Yksi tärkeitä microservice menetelmän etuna on se, että ryhmät ovat perustuva enemmän kuin liiketoimintatilanteet tekniikka, joka tehostaa Porrastettu lähestymistapa mukaan. Käytännössä pienempi ryhmiä kehittää microservice, asiakkaan skenaariossa käyttämällä synkronointitekniikoiden he perusteella. Toisin sanoen organisaation ei tarvitse yhdenmukaistaa tech ylläpitää Monoliittiset sovelluksia. Yksittäisten ryhmistä, joilla on omien palveluiden voit tehdä, mikä on järkevää niiden perusteella työryhmän osaamisalueet tai ominaisuudet sopii parhaiten ongelma voidaan ratkaista. Käytännössä ottaa suositellut tekniikat esimerkiksi tietyn NoSQL kaupan tai web application framework on suositeltavampaa.

Entä huonot puolet microservices, tasaaminen hallinta erilliset yksiköt kasvaa määrän ja lisää monimutkaisempiin ja versiointi käsittely. Verkkoliikenteen välillä microservices kasvaa sekä vastaava verkon viiveet suurempia. On paljon chatty, hajautetun services on ohjeen suorituskyvyn nightmare varten. Näytä työkaluja ilman riippuvuuksia, voi olla vaikea "on" koko järjestelmään. Mitä tehdä microservice lähestymistapa työpaikan siitä siitä, miten voit viestiä ja että varmatoimisempia vain-palvelusta on toimintoihin sijaan kiinteä vaatimuksia sopimuksia. On tärkeää määrittää yhteyshenkilöillä etukäteen suunnittelun, jälkeen services ulkopuolisista keskenään. Uusi kuvaus coined kanssa microservices lähestymistavan suunnittelemisesta on "tarkasti rajattuja SOA."


***Milloin sen helpoin microservices rakenne-vaihtoehto on erilliset federation Services, kukin riippumaton muutokset ja sovittuja standardit viestintään tietoja.***


Kun cloud sovelluksia on valmistettu, henkilöiden huomaat, että tämä hajotus yleinen sovelluksen riippumaton, keskittyvässä palvelujen tuominen ei kahdeksalla vaihtoehto.

## <a name="comparison-between-application-development-approaches"></a>Sovellusten kehittämisen tavoista vertailu

![Palvelun kangasta ympäristön sovellusten kehittämisen][Image1]

1. Yksiosaiset app sisältää toimialue-kohtaisia toimintoja ja toimintojen kerrokset, kuten web tai business tavallisesti jaettuna.

2. Kloonaamalla useita palvelimia/VMs/säilöt-Monoliittiset sovelluksen skaalattuja.

3. Microservice sovelluksen erottaa erillisiä pienempi palveluita toimintoja.

4. Tämän menetelmän Skaalaa ottamalla kunkin palvelun itsenäisesti luominen palveluista esiintymät palvelinten/VMs/säilöjen yli.


Suunnittelemisesta microservice kanssa lähestymistapa ei ole panacea kaikissa projekteissa, mutta se tasata tarkemmin ja edempänä business tavoitteet. Monoliittiset lähestymistavan alkaen voi olla hyväksyttävä, jos tiedät, että myöhemmin sinulla ei ole mahdollisuus Uudista koodin microservice rakenne, tarvittaessa. Yleensä Monoliittiset sovelluksen alkavat ja hitaasti rahaa sen vaiheittain, toiminta-alueet, jotka on oltava useampi skaalattava tai joustava alkaen.

Yhteenveto microservice-vaihtoehto on Kirjoita sovelluksesi monia pienempi palveluita.  Palvelut toimivat säilöjen koneet klusterin otetaan käyttöön. Pienempi ryhmiä kehittää palvelu, joka ohjeessa on tilanne ja itsenäisesti esikatsella-versio, ottaminen käyttöön ja skaalata kunkin palvelun niin, että sovellus kokonaisuudessaan voit kehityttävä.

## <a name="what-is-a-microservice"></a>Mikä microservice?

On microservices eri määritelmät ja etsiminen Internetistä tarjoaa useita hyvä resursseja, jotka tarjoavat omia tähtäyksellä ja määritelmät. Useimmat microservices seuraavat ominaisuudet sopivat kuitenkin laajasti yhteydessä:

- Kapseloida asiakas- tai business-skenaarion. Voit ratkaista ongelman kuvaus
- Kehittämä pienen suunnitteluryhmät ryhmän.
- Kirjoittaa millä tahansa ohjelmointikielellä ja käyttää mitä tahansa framework.
- Koodin koostuvat ja (valinnainen) tilan, jotka molemmat ovat itsenäisesti versiotietoja käyttöön ja skaalata.
- Käyttää muita microservices hyvin määritetyn liittymät ja protokollat.
- On yksilöllinen nimi (URL) haluamaasi kohtaan ratkaista avulla.
- Ovat yhdenmukaisia ja käytettävissä kanssa virheet.

Voit tehdä yhteenvedon kyselyjä seuraavia ominaisuuksia:

***Microservice sovellusten muodostuvat pieni, itsenäisesti versiotietoja ja skaalattava asiakkaan keskittyvässä palvelut, jotka keskenään vakio protokollat hyvin määritetyn liittymät päälle.***


Olemme kattaa on edellisen osion ensimmäistä kahta pistettä ja nyt voimme Laajenna ja selventää muihin sarakkeisiin.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Kirjoittaa millä tahansa ohjelmointikielellä ja käyttää mitä tahansa framework
Kuin kehittäjille emme voi vapaasti valita jostakin kielen tai framework haluamme, Microsoftin taidot tai palvelun tarpeiden mukaan. Joissakin palveluissa saattaa arvo suorituskyvyn C++ kaikkea muita etuja. Muissa palveluissa hallitun laatiminen C#- tai Java helpottaminen ehkä tärkeimpiä. Joissakin tapauksissa joudut ehkä kolmannen osapuolen kirjastojen, tietojen tallennustilan tekniikka tai keskiarvoja paljastaa palvelun asiakkaille.

Kun olet valinnut tekniikka, sinun tulee toiminnallisia tai elinkaari-hallintaan ja skaalaus-palvelun.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Sallii koodi ja tila on itsenäisesti versiotietoja käyttöön ja skaalata  

Kuitenkin haluat kirjoittaa oman microservices, koodi ja halutessasi tila olisi erikseen käyttöön, päivittäminen ja skaalata. Tämä on todella yksi näyttävyyttä ongelmien ratkaisemiseen jälkeen, kun yrität tekniikoita on. Saat skaalauksen, käsityksen siitä, miten osio (tai shard) koodi ja tila on hankalaa. Kun koodi- ja tilan erillisiä tekniikoita (joka tänään ne yleensä tehdä), että microservice käyttöönotto-komentosarjat on voi siten skaalaus molemmat. Tämä on myös joustavuutta ja määrityksen joustavuutta, jotta voit päivittää osa microservices päivittämättä ne kaikki kerralla.
Palaaminen Monoliittiset ja microservice lähestymistavan hetken, seuraavassa kaaviossa näkyvät tallentaminen tilan vaiheelta erot.

#### <a name="state-storage-between-application-styles"></a>Tilan tallennustilan sovelluksen tyylien väliin
![Palvelun kangasta ympäristö tilan tallennustila][Image2]

***Vasemmalla on Monoliittiset lähestymistapa, yhteen tietokantaan ja tiettyjen toimintojen tasoa.***

***Oikealla on microservices toimintatapa sen vuoksi, yhdistetty microservices, jossa tila on yleensä määritetty microservice ja eri tekniikoita käytetään-kaaviota.***

Monoliittiset lähestymistavan mukaan yleensä ei sovellus käyttää yhden tietokannan. Etuna on se, että se on yhteen paikkaan, mikä helpottaa käyttöönotto. Kukin osa voi olla yksittäinen taulukkoa tallentamaan tilaan. Ryhmien on oltava tarkka olevan vaiheeseen, joka on hankala. On väistämättä temptations uuden sarakkeen lisääminen aiemmin luotuun Asiakas-taulukkoon, tee taulukoiden välille ja luoda riippuvuussuhteita tallennustilan tasolla. Näin tapahtuu, kun yksittäisiä osia ei voi skaalata. Microservices lähestymistavan mukaan kunkin palvelun hallitsee ja tallentaa oman. Jokaisen palvelun on vastuussa skaalauksen koodi ja yhdessä täyttämään-palvelun tila. Entä huonot puolet on, että kun ei tarvitse luoda näkymät tai kyselyt, sovelluksen tietojen tarvitset kysely erillisiä tilan stores. Tämä on yleensä selviä on erillinen microservice, jota käytetään kaikissa microservices kokoelma näkymän. Jos haluat suorittaa itsenäisen kyselyjen tiedot, kunkin microservice huomioon sen kirjoittaminen tietovaraston offline-tilassa analytics-palvelun kyselyjä.

Versiotiedot on otettu käyttöön microservice-version niin, että useita eri versioiden ottaminen käyttöön ja suorita rinnakkain. Versiotietojen korjaa käyttötavoista kohtaa, johon microservice uudempaan versioon epäonnistuu päivityksen aikana ja tarvitsee palauttamaan aiempi versio. Toinen skenaario versiotiedoissa suorittaman A ja B-tyyppiseksi testaus, jossa eri käyttäjille kohdata palvelun eri versioita. Esimerkiksi on yleisiä päivittäminen microservice tietyn joukon asiakkaiden Testaa ennen juoksevan saat paljon uusia toimintoja. Microservices elinkaaren hallinta, kun tämän nyt tuo us välisen ne.


### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Toimii yhdessä muiden microservices kanssa hyvin määritetyn liittymät ja protokollat

Tässä aiheessa on vähän huomiota tähän, koska se on laaja julkaisuista-palvelun aloittaminen arkkitehtuuri julkaistu kuvaava viestintä kuviot viimeiset 10 vuotta. Yleensä palvelun tietoliikenne käytetään REST-menetelmän HTTP- ja TCP protokollia ja XML- tai JSON Sarjatoiminto-muodossa. LIP-näkökulmasta on tietoja käsittää web rakenne-vaihtoehto. Mutta ei ole enää mitään pysäyttäminen voit käyttää binaarinen protokollat tai oman tietomuotojen. Valmisteltava kuuluvien henkilöiden on näyttävyyttä ajan käyttämällä omaa microservices, jos ne ovat käytettävissä lisensoitu.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>On käytettävä ratkaista paikkaan yksilöllinen nimi (URL)

Muista, kuinka Microsoft säilyttää ajattelevat microservice tapa muistuttaa verkossa? Verkossa, kuten oman microservice on oltava käytettävissä aina, kun se suoritetaan. Jos kumpi toimii tiettyyn microservice ovat pohtivat koneet, kohteita voi siirtyä virheelliset nopeasti. DNS ratkaisee tietyn URL-Osoitteen tietyn tietokoneeseen samalla tavalla että microservice on oltava yksilöivä nimi, niin, että sen nykyinen sijainti on löydettävissä. Microservices on käytettävissä, jotta ne riippumaton infrastruktuuri käynnissä olevat nimet. Tämä tarkoittaa sitä, että on vuorovaikutuksen miten palvelussa on otettu käyttöön ja miten havaitaan, koska järjestelmässä on oltava palvelun rekisterin välillä. Samalla tavalla, kun kone epäonnistuu rekisterin palvelu on kertoa, jossa palvelu on nyt käytössä. Tämä Lisää us seuraavan aiheen: toimintakykyyn ja yhdenmukaisuuden.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Pysyy yhdenmukaisia ja käytettävissä kanssa virheet

Myyminen odottamattomia virheitä on vaikein ongelmien ratkaisemiseen erityisesti hajautettu järjestelmä. Paljon tunnus, jolla on kirjoittaa kuin kehittäjät käsittelee poikkeukset ja tämä on myös missä kuluvaa aikaa eniten testauksessa. Mutta ongelma on enemmän aikaa kuin koodin virheiden kirjoittamista. Mitä tapahtuu, kun tietokoneeseen, jossa on käynnissä microservice epäonnistuu? Paitsi sinun tarvitsee tunnistaa microservice tämän virheen (Kova ongelman, itsenäisesti), mutta sinun on myös jotain käynnistämään oman microservice. Microservice täytyy joustavat virheet ja Käynnistä uudelleen usein käytettävyys syistä toisessa tietokoneessa. Tämä on myös alaspäin mitä tila on tallennettu microservice puolesta, jossa se palauttaa tämä tila ja onko se pystyy käynnistämään onnistuneesti. Toisin sanoen järjestelmässä on oltava Laske (prosessi käynnistyy) toimintakykyyn sekä toimintakykyyn tilaan tai tiedot (ei ole tietojen menettämisen ja tiedot pysyvät yhtenäisinä).

Vikasietoisuudelle ongelmia ovat lasketaan aikana muissa tilanteissa, esimerkiksi kun virheiden käydä sovelluksen päivityksen aikana. Microservice käyttäminen yhdistelmäympäristössä, järjestelmän ei tarvitse palauttaa. Se on myös sen jälkeen päätät, onko se jatkaa siirtää eteenpäin uudempaan versioon tai peruuttaa sen sijaan säilyttää yhdenmukaisen tilan aiemman version. Kysymyksiin kuten liittyykö koneet riittävästi käytettävissä pitämään siirtäminen eteen ja niiden microservice on otettava huomioon aiempien versioiden palauttaminen. Tämä edellyttää tulostaminen kunto tiedot voivat tehdä näitä päätöksiä microservice.

### <a name="reports-health-and-diagnostics"></a>Raporttien terveys- ja vianmääritys

Saattaa näyttää selvää, ja se on usein tekemättä jäänyt, mutta on tärkeää, että microservice ilmoittaa terveys- ja Diagnostiikka. Muussa tapauksessa on pieni tietoja toimintojen näkökulmasta. Diagnostiikan tapahtumat hajautettuna joukko riippumaton palveluita yli ja käsittely tietokoneen kellon viistottaa tai koko kuvan selventämään tapahtuma-tilaus on hankalaa. Samalla tavalla, voit käsitellä microservice sovittuja protokollat ja tietomuotojen esiin tarve-kirjautuminen terveys- ja diagnostiikan tapahtumia, jotka kädessä lopettaa tapahtuma-säilössä, kyselyt ja tarkastelemista varten. Microservices lähestymistavan mukaan se on avain, joka eri ryhmiä sopivat yhteen kirjaaminen-muodossa.  On on oltava yhtenäinen lähestymistapa diagnostiikan tapahtumien tarkasteleminen sovelluksessa kokonaisuudessaan.

Kuntotietojen poikkeaa Diagnostiikka. Kunto on tarvittavat toimintoja nykyisestä tilasta raportointi microservice. Hyvä esimerkki toimii varustettuja päivitys- ja säilyttää käytettävyyttä. Palvelun saattaa virheelliseksi tällä hetkellä prosessin kaatumisen vuoksi tai tietokoneen uudelleenkäynnistys, mutta edelleen toiminnassa. Tarvitset viimeiseksi on saavat tämän huonompi suorittamalla päivityksen. Paras tapa on tehtävä tutkimuksen ensin tai palauttaa microservice aikaa. Kuntotietojen tapahtumia microservice Salli us päätösten tekemisessä ja avulla, Luo itsekorjautumisominaisuudet palvelut.

## <a name="service-fabric-as-a-microservices-platform"></a>Palvelun kangasta kuin microservices-ympäristössä

Azure palvelun kangasta kulutusluottomuotoja Microsoftin siirtymisen välittää ruutuun tuotteita, joka on yleensä Monoliittiset tyyli-palveluiden toimittaminen ulos. Kokemukset rakennuksen ja liiketoiminnan suuri palvelut, kuten Azure SQL-tietokannat ja DocumentDB-muotoinen palvelun kangasta. Ympäristö kehittynyt ajan kuluessa, Lisää ja lisää se hyväksytty. Lisäksi palvelun kangasta oli suorittaa missä tahansa: et pelkästään Azure, mutta myös erillinen Windows Server käyttöönotoissa.

***Palvelun kangasta tarkoituksena on vaikea ratkaiseminen luominen ja suorittaminen palvelu ja hyödyntää infrastruktuurin resursseja tehokkaasti niin, että ryhmiä käyttämällä microservices lähestymistavan business-ongelmat voidaan ratkaista.***

Palvelun kangasta sisältää laajan alueisiin, voit täydentää microservices lähestymistavan sovellusten:

- Ympäristössä, joka sisältää järjestelmäpalvelut otetaan käyttöön, päivittäminen, tunnistaa ja Käynnistä epäonnistui services, tutustu palvelun sijainti, hallinta valtion ja kunnon valvonta. Järjestelmän palveluista käyttöön voimassa monia microservices edellä kuvatut ominaisuudet.

-  Ohjelmoinnin ohjelmointirajapinnan tai kehysten, voit täydentää sovellusten kuin microservices: [luotettava toimijat ja luotettavia palveluja](service-fabric-choose-framework.md). Voit luoda oman microservice valittua koodi. Mutta näitä API Tee työn Lisää helppoa, ja ne integroi ympäristö tarkempaa tasolla. Näin saat terveys-ja diagnostiikka tai valmiin suuren käytettävyyden voit hyödyntää esimerkiksi.

***Palvelun kangasta on agnostic, miten voit luoda palvelun ja voit käyttää mitä tahansa tekniikka. Se kuitenkin säätää valmiin ohjelmoinnin API, jotka helpottavat microservices luominen.***

### <a name="are-microservices-right-for-my-application"></a>Ovat microservices sovellu oman sovelluksen?

Ehkäpä. On ilmennyt on, että Lisää ja lisää ryhmiä Microsoft alkamisen liiketoimintaa Pilvipalvelun luominen, kun niistä monta toteutunut edut ottaen microservice kaltaisessa lähestymistapa. Bing-esimerkiksi on kehittänyt microservices hakutoiminnolla vuotta. Muiden ryhmien microservices lähestymistapa on uusi. Ne löydy, käytettävissä on vaikea ongelmien ratkaisemiseen voimakkuus core niiden alueiden ulkopuolella. Tämän vuoksi palvelun kangasta kokemukset veto valinta etsimisen services tekniikka.

Palvelun kangasta tavoitteena on vähentää muodostamisen microservice lähestymistavan sovellusten monimutkaisia, niin, että sinun ei tarvitse käydä läpi kuin monet kallista redesigns. Käynnistä pieni, skaalata tarvittaessa käytöstä palveluja ja lisää uusia asiakkaan käytön--, joka on lähestymistapa myötä. Olemme tunnettiin aiemmin että todellisuudessa on monia muita ongelmia vielä, jos haluat tehdä microservices Lisää approachable useimmat kehittäjille ongelmaan. Säilöjen ja toimija ohjelmoinnin malli on esimerkkejä vähän kerrallaan kyseiseen suuntaan ja emme ole varma, että Lisää innovaatiot päätyvät helpompi.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja:
    * [Palvelun kangasta termejä yleiskatsaus](service-fabric-technical-overview.md)
    * [Microservices:-Sovelluksen vallankumous tarjoaa pilveen](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
