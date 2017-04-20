# <a name="internet-of-things-security-architecture"></a>Asioita Internet security architecture

Järjestelmää suunniteltaessa on tärkeää ymmärtää järjestelmän mahdollisia uhkia ja lisää näin ollen asianmukainen suojaus järjestelmä on suunniteltu ja arkkitehtuuri. On erityisen tärkeää suunnitella alusta alkaen tuotteen ja huomioida siinä suojaukseen koska ymmärtämään miten hyökkääjä ehkä voi saada järjestelmän auttaa Varmista, että sopiva lieventävät tekijät ovat paikoillaan alusta alkaen. 

## <a name="security-starts-with-a-threat-model"></a>Suojauksen alkaa uhka malli
 
Microsoft pitkään käytetty uhka mallit tuotteitaan ja yrityksen uhka mallinnusprosessi Luettele käytettävissä. Yrityksen kokemus osoittaa, että mallintamiseen odottamattomia etuja ulkopuolella välitön käsitys mitä uhkia ovat eniten koskevat. Esimerkiksi se luo myös avenue avoimeen keskusteluun muiden kanssa ulkopuolella kehitys tiimi, joka voi johtaa uusien ideoiden ja parannuksia tuotteeseen.
  
Uhka mallinnus tavoitteena on ymmärtää, miten hyökkääjä saattaa pystyä murtautua järjestelmään ja varmista, että sopiva lieventävät tekijät ovat paikoillaan. Uhka mallinnus voimat rakenteen tiimin lieventävät tekijät harkita järjestelmä on suunniteltu sijaan jälkeen järjestelmä on otettu käyttöön. Tämä seikka on tärkeää kriittisesti retrofitting tietoturva suojaus, runsaasti laitteiden alalla on mahdotonta, koska virhe voi enää ja jättää asiakkaiden vastuulla.

Monet kehitysryhmät voivat tehdä erinomainen työ, sieppaus, joista hyötyvät asiakkaat järjestelmän toiminnalliset vaatimukset. Tunnistaa-ilmeiset tavalla, että joku voi käyttää väärin järjestelmä on kuitenkin enemmän haastava. Uhka mallinnuksen avulla kehitysryhmät voivat ymmärtää, mitä hyökkääjä voi tehdä ja miksi. Uhka mallinnus on rakenteellinen prosessi, joka luo turvallisuutta koskevan keskustelun järjestelmän suunnitteluun sekä muutokset, jotka tehdään varrella vakuuden vaikutus rakenteeseen. Uhka malli on yksinkertaisesti asiakirja, kun näitä ohjeita edustaa myös ihanteellisen keinon varmistaa jatkuvuuden tietämyksen, kokemusten säilyttäminen opitut asiat ja ohjeen uuden tiimin siirtyvät nopeasti. Lopuksi, jotta voit ottaa huomioon muita näkökohtia suojausta, kuten mitä haluat tarjota asiakkaillesi suojaus-sitoumukset on uhka mallinnus tulos. Näiden sitoumusten uhka mallinnusta yhdessä ilmoittaa ja asema asioita Internet (IoT)-ratkaisun testaus.
 

### <a name="when-to-threat-model"></a>Kun uhka malli

[Uhka mallinnus](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) tarjoaa suurimman arvon, jos on sisällytetty suunnitteluvaiheessa. Kun suunnittelet, on korkein muutokset uhkien poistamiseksi. Haluttu lopputulos on tarkoituksellista uhkien poistamiseksi. On paljon helpompaa kuin lisäämällä lieventävät tekijät, testaamaan niitä ja varmistaa ne ajan tasalla ja lisäksi tällaiseen hävittämiseen ei ole aina mahdollista. Se on yhä vaikeampaa uhkien poistamiseksi tuote tulee kehittyneempään ja puolestaan viime kädessä edellyttää enemmän työtä ja paljon vaikeampi kuin mallinnus aikaisessa vaiheessa kehityksen uhka puolensa.

### <a name="what-to-threat-model"></a>Mitä malli

Olisi säiettä malli ratkaisua kokonaisuutena ja keskittyä myös seuraavilla aloilla:

- Ja yksityisyyden suojausominaisuuksista
- Jonka virheet ovat asianmukaisen suojauksen ominaisuudet
- Ominaisuuksia, joita luottamus rajan kosketus 

### <a name="who-threat-models"></a>Joka uhkaa mallit

Uhka mallinnus on prosessi, samoin kuin muut.  Se on hyvä käsitellä ratkaisun muiden osien tavoin uhka-mallitiedosto ja vahvista se. Monet kehitysryhmät voivat tehdä erinomainen työ, sieppaus, joista hyötyvät asiakkaat järjestelmän toiminnalliset vaatimukset. Tunnistaa-ilmeiset tavalla, että joku voi käyttää väärin järjestelmä on kuitenkin enemmän haastava. Uhka mallinnuksen avulla kehitysryhmät voivat ymmärtää, mitä hyökkääjä voi tehdä ja miksi.

### <a name="how-to-threat-model"></a>Miten malli

Mallinnusprosessi uhka muodostuu neljästä vaiheesta; vaiheet ovat:

- Hakemuksen malli
- Luetteloi uhat
- Uhkien lieventämiseksi
- Vahvista lieventävät tekijät

#### <a name="the-process-steps"></a>Prosessin eri vaiheiden

Kolme saatossa huomattavasti viisastunut näissä pitää mielessä, kun uhka rakentamiseen:

1. Voit luoda kaavion ulkopuolella reference Architecture-arkkitehtuurin. 
2. Aloita kehitysyhteisön-first. Yleiskuvan ja ymmärtää järjestelmän kokonaisuudessaan, ennen kuin syvä Sukeltava.  Näin voidaan varmistaa, että olet ohjaamme ovat oikeissa paikoissa.
3. Prosessin Drive, älä anna sinun asema prosessi. Jos löytää ongelman vaiheessa mallinnus ja haluavat tutkia sitä, mene sen!  Ei mielestäsi ei tarvitse noudattaa näitä vaiheita slavishly.  

#### <a name="threats"></a>Uhat

Neljä core uhka mallin elementit ovat:

- Prosessit (web services, Win32 services * nix daemonit jne. Huomaa, että joitakin monimutkaisia kohteiden (esim. kenttä yhdyskäytävät ja tunnistimet) erotettuja prosessi, kun tekninen Poraudu alaspäin näillä alueilla ei ole mahdollista.
- Tiedot tallennetaan (missä tiedot on tallennettu, kuten tiedoston tai tietokannan)
- Tietovuo (Jos tiedot siirretään sovelluksen muiden osien välillä)
- Ulkopuolisille yksiköille (mitään vuorovaikutuksessa järjestelmän kanssa, mutta se ei soveltamisen valvonnassa, esimerkkejä ovat käyttäjät ja satelliitin syötteet)

Arkkitehtoninen kaavio kaikki elementit ovat erilaisia uhkia; Käytämme ASKEL Muistikas. Lue [Uhka mallinnus uudelleen, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) tietää enemmän STRIDE-elementtejä.

Sovelluksen kaavion eri osia sovelletaan ASKEL tietyt uhat:

- Prosessit ovat ASKEL
- Tietovirrat ovat TID
- Tietovarastot ovat TID ja joskus R-Jos lokitiedostojen tietoja tallennetuista tiedoista.
- SRD ulkoiset tahot ovat

## <a name="security-in-iot"></a>IoT suojaus

ERIKOISKÄYTTÖÖN yhdistetyistä laitteista on merkittävä määrä mahdollisia toimia pinta-alat ja vuorovaikutuksen kuviot, jotka on otettava huomioon tarjoamaan puitteet digitaalisen käytön kyseisiin laitteisiin. Termi "digital access" käytetään tässä erottaa toiminnot, jotka suoritetaan suoraan laitteen vuorovaikutuksen kautta kun Accessin suojauksista tarjotaan fyysinen käytönvalvonta. Esimerkiksi liikkeelle laitteen lukittava huone, ovi. Kun fyysinen ei voi olla käyttö on estetty ohjelmistot ja laitteistot, käyttää fyysisesti estää johtavien järjestelmän häiriöt voidaan toteuttaa toimenpiteitä. 

Tutustumme vuorovaikutus kuvioita, kuten olemme näyttää "Laitehallinta" ja "laitteen tiedot" yhtä paljon huomiota. "Laitehallinta" voitaisiin luokitella sellaisia tietoja, joita laitteeseen osapuoli mukana toimitetaan tavoitteen muuttaminen tai metsän käyttäytymistään tilaansa tai sen ympäristön tilasta. "Laitteen tiedot" voitaisiin luokitella sellaisia tietoja, joita laite tietokoneesta kuuluu äänimerkki tilaansa ja sen ympäristön tilasta havaittujen toiselle osapuolelle.
   
Jotta voidaan optimoida suojauksen parhaita käytäntöjä, on suositeltavaa, että tyypillinen IoT-arkkitehtuuri voidaan jakaa vyöhykkeisiin useita osa/osana mallinnus harjoituksessa uhka. Nämä kuvaukset ovat tämän osan koko ja sisällyttää:

-   Laitteen,
-   Kentän yhdyskäytävä
-   Cloud yhdyskäytäviä, ja
-   Palvelut.

Vyöhykkeet ovat laajat tavalla segmentoidessasi ratkaisun; kummallakin alueella on usein Omat tiedot ja todennus- ja vaatimuksia. Alueet myös voidaan käyttää yksinään vahinkoa ja rajoittaa vaikutus suurempi luottamus alueet vyöhykkeisiin alhainen luottamus.

Kullekin vyöhykkeelle on erotettu luota rajan, jossa huomattava jäljempänä esitetyssä kaaviossa punainen pisteviiva. Se edustaa siirtymää, tietoja yhdestä lähteestä toiseen. Tietoja voi tämän siirtymisen aikana tietojen väärentämisen, aiheuttaa uudelleenaktivointitarpeen, hylkäämistä, palveluneston, tietojen paljastumisen muille käyttäjille ja korotusta on oikeus (ASKEL).

![IoT suojausvyöhykkeistä](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Jokainen rajan sisällä käytetyt osat testataan myös ASKEL, otetaan käyttöön täydet 360 mallinnuksen ratkaisun Näytä uhka. Seuraavissa osissa tarkentamiseksi, kaikki osat ja erityisiä turvatoimiin liittyviä ongelmia ja ratkaisuja, jotka täytyy sijoittaa takaisin paikalleen.

Osaa, joka seuraa keskustelee vakio osat, joita löytyy yleensä näillä alueilla.

### <a name="the-device-zone"></a>Laite alueella

Laitteen ympäristö ei heti fyysisen laitteen ympärillä, jossa fyysinen käyttö ja/tai "lähiverkko" peer to peer digitaalinen laite on toteutettavissa. "Paikallisen verkon" oletetaan olevan verkon, joka on erillinen ja eristetty-– mutta mahdollisesti Silloitettu Internetiin – julkinen ja sisältää kaikki lyhyen langaton radio tekniikka, joka sallii peer-to-peer viestinnän laitteiden. Se *ei* Sisällytä kaikki verkon virtualisointitekniikka pintakuvion paikallisen verkon luominen ja myös ei sisällä mitään kaksi laitetta välittää tietoja yli julkisen verkon tilaa ne olisivat peer-to-peer viestinnän suhde Anna vaativia verkkoja julkisen toimijan.

### <a name="the-field-gateway-zone"></a>Kentän yhdyskäytävä vyöhyke

Kentän yhdyskäytävä on laitteen tai laitteen tai joidenkin yleinen tietokone Palvelinohjelmisto, joka toimii nimellä viestintä-käyttöönotto-ohjelma ja mahdollisesti laitteen tietojen käsittely keskittimeen ja laitteen ohjausjärjestelmä. Kentän yhdyskäytävä vyöhyke sisältää kentän yhdyskäytävä itse ja kaikki laitteet, jotka on liitetty siihen. Kuten nimikin viittaa, yhdyskäytävien kentän ulkopuolella kiinteä tietojenkäsittelyn laitteet toimivat, ovat yleensä sidottu paikkaan, ovat mahdollisesti fyysisen tunkeutumisen ja on rajoitettu toiminnallisten redundanssin. Kaikki kentän yhdyskäytävä on usein asia sanoa, yksi kosketus ja sabotage samalla tietäen sen funktio on. 

Kentän yhdyskäytävä on erilainen kuin pelkkä liikenteen reitittimen, Tietovuo-, eli se on hakemuksen vastaanottavan yksikön ja verkkoyhteys tai terminal istunto ja se on ollut aktiivinen rooli käyttöoikeuksien hallinta. NAT-laite tai palomuuri, sen sijaan ei ollakseen kentän yhdyskäytäviä, koska ne eivät ole eksplisiittinen yhteys tai istunnon päätteiden vaan pikemminkin reitin (tai lohkon) yhteyksien kautta ne tapahtumapaikan tai esiintyjän kuva. Kentän yhdyskäytävä on kaksi erillistä pinta-alat. Yksi suunnattu siihen liitettyjä laitteita ja edustaa alueen sisäpuolella ja toinen suunnattu kaikille ulkoisille osapuolille ja on alueen reunaan.   

### <a name="the-cloud-gateway-zone"></a>Cloud gateway-vyöhyke

Cloud gateway on järjestelmä, joka mahdollistaa etäyhteyksien ja laitteiden tai kentän yhdyskäytävien useiden eri sivustojen kautta julkisen verkon tilaa pilvipohjainen valvontaa ja tietojen analyysi järjestelmän federation, tällaiset järjestelmät yleensä kohti. Joissakin tapauksissa cloud yhdyskäytävä voi heti helpotetaan erikoiskäyttöön laitteisiin varastoalueilta kuten tabletit tai puhelimet. Käsitellä tässä yhteydessä "pilvi" on tarkoitettu viittaamaan kiinteä atk-järjestelmän, joka ei ole sidottu samaan saittiin kytketyt laitteet tai kentän yhdyskäytävä. Myös Cloud vyöhykkeellä operatiiviset toimenpiteet estävät kohdennettuja fyysisen ja ei välttämättä saastu "Julkinen pilvi-infrastruktuurin.  

Cloud yhdyskäytävä voi mahdollisesti yhdistää verkon virtualisointi-kerros, vesi-ja cloud-yhdyskäytävä ja kaikki sen kytketyt laitteet tai kentän yhdyskäytävien verkko-liikenteen avulla. Cloud-gateway itse on ole laitteen ohjausjärjestelmä eikä käsittelyn tai varastoihin, laitteen tietojen; Näissä tiloissa yhdessä cloud-yhdyskäytävän. Cloud gateway-vyöhykkeeseen kuuluu pilvi yhdyskäytävän itse kaikki kentän yhdyskäytävät ja suoraan tai epäsuorasti liitetyt laitteet. Alueen reuna on erillinen pinta, jossa kaikki mahdollisuuksiisi kommunikoida kautta.

### <a name="the-services-zone"></a>Palvelut zone

"Palvelu" on määritelty mitään ohjelmiston osaa tai moduuli, joka on vuorovaikutuksessa kanssa laitteiden kentän ja pilvi yhdyskäytävän kautta tietojen keruuta ja analysointia sekä ohjaus- ja tässä yhteydessä.  Palvelut ovat lupia myöntävien luvanhakijoiden. Ne toimivat mukaisesti henkilöllisyytensä yhdyskäytävät ja muihin osajärjestelmiin, tallentaa ja analysoida tietoja, itsenäisesti antaa komentoja laitteita koskevia tietoja tai aikataulujen perusteella ja paljastaa tietoja ja ohjata valtuutettujen loppukäyttäjille ominaisuuksia.

### <a name="information-devices-vs-special-purpose-devices"></a>Vs. erikoiskäyttöön laitteiden tiedot-laitteet

Tietokoneet, puhelimet ja tabletit ovat pääasiassa vuorovaikutteisia tietoja laitteita. Puhelimet ja tabletit erikseen optimoida ympärille suurentaminen Akun käyttöajan. He mieluummin käytöstä osittain käsitteleminen ei heti henkilö tai tarjoa palveluja, kuten musiikin tai niiden omistaja käsittää yritystietokannan tiettyyn paikkaan. Järjestelmien näkökulmasta nämä tiedot teknologian laitteet ovat pääasiassa toimii välityspalvelimia ihmisiä kohti. Ne ovat "ihmiset toimilaitteita" "ihmiset anturit" input kerääminen ja toimien ehdottaminen. 

ERIKOISKÄYTTÖÖN laitteet lämpötilan yksinkertainen tunnistimien monimutkaisia tehtaan tuotantolinjojen osien sisällä niitä, tuhansia kanssa ovat erilaiset. Nämä laitteet ovat paljon mitoitettu käytön ja vaikka ne tarjoavat joitakin käyttöliittymän, ne ovat pitkälti mitoitettu joilla satamarakenteen turvatoimet sovitetaan tai integroida hyödykkeet fyysisessä maailmassa. Ne mitataan raportin ympäristöoloissa, venttiilit ottaa, hallita servos, sound hälytykset, vaihtaa valot ja suorittaa muita tehtäviä. Ne auttavat toimimaan tiedot-laite on liian yleinen, liian kallis, liian suuri tai liian hauras. Konkreettinen tarkoitus määrittää välittömästi niiden teknisen suunnittelun kuin hyvin saatavilla rahaliiton talousarvio niiden tuotannon ja työvaiheen suunnitellun käyttöiän ajan. Näiden kahden avaimen tekijöiden yhdistelmä-rajoittaman käytettävissä toiminnallinen energia budjetti, fyysinen kosketuspinnan ja näin ollen käytettävissä olevan tallennustilan compute ja ominaisuudet.  

Jos jotain "suuntaisrakeisten sähköteknisten levyvalmisteiden väärä" Automaattinen tai etätietokoneen hallittavien laitteiden kanssa, esimerkiksi fyysisiä vikoja tai -ohjauslogiikkakaavio, vikojen ja luvattoman tunkeutumisen willful. Tuotannon erät on hävitettävä rakennukset voidaan looted tai poltettu ja ihmiset ehkä loukkaantuu tai jopa die. Tämä on tietysti kokonaan eri luokkaa kuin joku maxing ulos varastetun luottokortin raja vahingon. Suojaus-palkin laitteille, jotka tekevät siirtää ja myös tunnistimen tietojen, joka lopulta johtaa komentoja, jotka aiheuttavat asiat siirretään, on oltava suurempi kuin sähköisen kaupankäynnin tai pankkitoiminnan skenaario. 


### <a name="device-control-and-device-data-interactions"></a>Laitehallinta ja laitteen tiedot vuorovaikutukset

ERIKOISKÄYTTÖÖN yhdistetyistä laitteista on merkittävä määrä mahdollisia toimia pinta-alat ja vuorovaikutuksen kuviot, jotka on otettava huomioon tarjoamaan puitteet digitaalisen käytön kyseisiin laitteisiin. Termi "digital access" käytetään tässä erottaa toiminnot, jotka suoritetaan suoraan laitteen vuorovaikutuksen kautta kun Accessin suojauksista tarjotaan fyysinen käytönvalvonta. Esimerkiksi liikkeelle laitteen lukittava huone, ovi. Kun fyysinen ei voi olla käyttö on estetty ohjelmistot ja laitteistot, käyttää fyysisesti estää johtavien järjestelmän häiriöt voidaan toteuttaa toimenpiteitä. 
 
Tutustumme vuorovaikutus kuvioita, kuten olemme näyttää "Laitehallinta" ja "laitteen tiedot" huomiota uhan mallinnuksen aikana samalla tasolla. "Laitehallinta" voitaisiin luokitella sellaisia tietoja, joita laitteeseen osapuoli mukana toimitetaan tavoitteen muuttaminen tai metsän käyttäytymistään tilaansa tai sen ympäristön tilasta. "Laitteen tiedot" voitaisiin luokitella sellaisia tietoja, joita laite tietokoneesta kuuluu äänimerkki tilaansa ja sen ympäristön tilasta havaittujen toiselle osapuolelle. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Modeling Azure IoT reference Architecture-arkkitehtuurin uhka

Microsoft käyttää mallintamista varten Azure IoT uhka edellä esitettyjä puitteissa. Osassa sen vuoksi Käytämme Azure IoT Reference Architecture-arkkitehtuurin konkreettinen Esimerkki osoittamaan uhka mallintaminen IoT on mietittävä, miten ja miten tunnistaa uhkat osoite. Meidän tapauksessa voimme tunnistaa kohdistus neljä osa-aluetta:

-   Laitteiden ja tietolähteiden,
-   Tietojen siirto
-   Laite- ja käsittely- ja
-   Esitys

![Uhka Azure IoT mallinnus](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Oheinen kaavio on yksinkertaistettu näkymä Microsoftin IoT arkkitehtuurin Tietovuokaavio-malli, jota käytetään Microsoft Threat mallinnus työkalu:

![Uhka Azure IoT MS uhka mallinnuksen avulla mallinnus](media/iot-security-architecture/iot-security-architecture-fig3.png)

On tärkeää huomata, että arkkitehtuurin erottaa laitteen ja gateway-ominaisuuksia. Näin käyttäjä voi hyödyntää gateway-laitteet, jotka ovat turvallisempia: he pystyvät cloud yhdyskäytävän joka vaatii yleensä suurempi käsittelyn yleiskustannus, että alkuperäinen laite - termostaatin - kuten voi tarjota itse suojattu protokollien avulla yhteydenpito. Azure-palveluiden alueella oletetaan Cloud-yhdyskäytävä edustaa Azure IoT Hub-palvelu.

### <a name="device-and-data-sourcesdata-transport"></a>Laitteen ja tietojen tietolähteet/tietojen siirto

Tässä osassa käsitellään edellä esitettyjä uhka mallinnus linssin läpi arkkitehtuuri ja antaa yhteenvedon siitä, miten olemme täyttävät joitakin luontaisia koskee. Keskitytään ydinosat uhka-malli:

- Prosessien (jotka meidän valvonta ja ulkoisten kohteiden)
- Viestintä (lyhenne sanoista tietovirrat)
- Varastointi (kutsutaan myös Microsoft Data)

#### <a name="processes"></a>Prosessit

Kunkin luokan esitettyjen Azure IoT arkkitehtuuri, pyrimme pienentämään eri uhkien määrä on tietoja eri vaiheiden välillä: prosessi-, viestintä- ja varastointi. Alla on yhteenveto yleisimpiä "prosessi"-luokan perässä yleiskuvan miten nämä voi olla paras, joiden lievity antaa: 

**Tietojen väärentämisen (S)**: hyökkääjä voi purkaa salauksen avainten luontimateriaalin laitteesta, joko ohjelmiston tai laitteiston tasolla, ja myöhemmin toinen fyysinen tai virtuaalinen laite käyttäjätiedoilla järjestelmän avainten luontimateriaalin laitteen käyttö on tehty siitä. Hyvää kuvassa on kaukosäätimissä, joka voi ottaa minkä tahansa TV ja jotka ovat suosittuja prankster työkalut.

**Denial of Service (D)**: laite voi muodostaa kykenemätön toimintaan tai kommunikointiin häiritsemällä radiotaajuuksien tai leikkaamalla johdot. Esimerkiksi valvonta-kamera, joka oli tarkoituksella knocked sen virta- tai yhteys ei raportoi tietoja lainkaan.

**Käsittelyltä (T)**: hyökkääjä osittain tai kokonaan korvata ohjelmiston laitteeseen, joten mahdollisesti korvattu ohjelmiston voit hyödyntää laitteen tunnistetiedot avainten luontimateriaalin tai pitämällä tärkeimmät materiaalit cryptographic tilat olisivat käytettävissä laittomaan ohjelma. Hyökkääjä voi hyödyntää esimerkiksi puretut avainten luontimateriaalin kaapata ja estä viestintä polku on laitteen tiedot ja korvata vääriä tietoja, jotka on todentanut varastettu avainten luontimateriaalin.

**(I) tietojen paljastumisen muille käyttäjille**: Jos laite on käynnissä käsitellä ohjelmiston, käsitellä ohjelmistoon mahdollisesti voitu paljastaa tietojen luvattoman. Hyökkääjä voi hyödyntää esimerkiksi puretut avainten luontimateriaalin lisätäkseen itse laitteen ja ohjaimen tai kentän yhdyskäytävä tai cloud-yhdyskäytävän käytöstä tietoja siphon välillä viestintä-polkunimen.

**Korkeus on oikeus (E)**: laite, joka toteuttaa tiettyä toimintoa voidaan pakottaa tekemään jotain muuta. Jotka on ohjelmoitu puoli tapa avata venttiili voi esimerkiksi tricked Avaa aivan.

| **Osa** | **Uhka** | **Mitätöinti**                                                                                                                                                | **Riski**                                                                                                                                                                                                    | **Toteutus**                                                                                                                                                                                                                                                                                                                                     |
|---------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Laitteen        | S          | Laitteen tunnistetiedot määritteleminen ja todentaa laitetta                                                                                                | Korvaa laite tai laitteen osa muun laitteen kanssa. Miten voimme tietää, olemme puhumisen oikea väline?                                                                                           | Todentaa laitetta, Transport Layer Security (TLS)- tai IP-suojauksen avulla. Infrastruktuuri olisi tuettava käyttämällä ennalta jaettua avainta (PSK) sellaisissa laitteissa, joita ei voi käsitellä koko epäsymmetrinen. Suorituskykykertoimen Azure AD [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt)                             |
|               | TRID       | Koskevat mekanismit tamperproof laitteen esimerkiksi tekemällä se erittäin vaikea avaimet ja muun materiaalin salauksen purkaminen laite mahdotonta. | Riski on, jos joku on lastin laite (fyysinen häiriöitä). On siitä, miten voimme varmistaa, että laite ei koskettu.                                                                                 | Tehokkain vähentäminen on TPM TPM-turvapiiri ominaisuus, joka mahdollistaa erityisen on-chip-piirit, joista avaimet voi lukea, mutta voidaan käyttää vain salaustoimintoja, joka käyttää avain mutta ei koskaan paljastaa avain avaimet tallennetaan. Laitteen muisti salaus. Avainten hallinta laitteelle. Koodin allekirjoitus. |
|               | E          | Ottaa hallintalaitteen käytön. Luvan malli.                                                                                                    | Jos laite sallii yksittäisille toimenpiteille, jotka suoritetaan komentoja ulkoisesta lähteestä hankittavaa tai jopa tartunnan saaneen anturit perusteella on sallittua hyökkäys, toimintojen suorittaminen ei muuten voi käyttää. | Laitteen malli luvan ottaa                                                                                                                                                                                                                                                                                                             |
| Kentän yhdyskäytävä | S          | Todennuksen kentän yhdyskäytävä Cloud yhdyskäytävään (cert mukaan PSK-vaatimuksen mukaan..)                                                                           | Jos joku voi väärentää kentän yhdyskäytävä, sitten se pystyy esittämään kuin laitteeseen.                                                                                                                               | TLS RSA/PSK IPSe, [RFC 4279](https://tools.ietf.org/html/rfc4279). Samat – yleensä laitteita avainten säilytys- ja todistus koskee Paras tapaus on käyttää TPM-Turvapiiriä. IPSec tukee Wireless Sensor verkkojen (WSN) 6LowPAN tunniste.                                                                                                              |
|               | TRID       | Suojaa kentän yhdyskäytävä luvattoman (TPM)?                                                                                                            | Väärentämällä, houkutella cloud gateway-ajattelua se puhuu kentän yhdyskäytävä saattaa aiheuttaa tietojen paljastumisen muille käyttäjille ja tietojen väärentämisen                                                             | Muistin salaus, TPM-Turvapiirin 's-todennusta.                                                                                                                                                                                                                                                                                                              |
|               | E          | Ohjausmekanismi Access Gateway kenttä                                                                                                                    |                                                                                                                                                                                                             |                                                                                                                                                                                                                                                                                                                                                        |

Seuraavassa on joitakin esimerkkejä tämän luokan uhat:

Tunnistetietojen väärennys: Hyökkääjä voi purkaa salauksen avainten luontimateriaalin laite joko ohjelmiston tai laitteiston taso, ja myöhemmin käyttää käyttäjätiedolla toinen fyysinen tai virtuaalinen laite järjestelmän laitteen avainten luontimateriaalin on haettu.

**Denial of Service**: laite voi muodostaa kykenemätön toimintaan tai kommunikointiin häiritsemällä radiotaajuuksien tai leikkaamalla johdot. Esimerkiksi valvonta-kamera, joka oli tarkoituksella knocked sen virta- tai yhteys ei raportoi tietoja lainkaan.

**Aiheuttaa uudelleenaktivointitarpeen**: hyökkääjä osittain tai kokonaan korvata ohjelmiston laitteeseen, joten mahdollisesti korvattu ohjelmiston voit hyödyntää laitteen tunnistetiedot avainten luontimateriaalin tai pitämällä tärkeimmät materiaalit cryptographic tilat olisivat käytettävissä laittomaan ohjelma.

**Aiheuttaa uudelleenaktivointitarpeen**: valvonta kamera, joka näkyy tyhjä hallway kuvan näkyvän spektrin pyritään saattaa, tällainen hallway valokuva. Savun tai palon anturi voi raportoida joku vaaleampi sen tilalla. Kummassakin tapauksessa laite saattaa olla teknisesti täysin luotettava järjestelmä kohti, mutta se raportoi käsitellä tietoja.

**Aiheuttaa uudelleenaktivointitarpeen**: hyökkääjä saattaa hyödyntää puretut avainten luontimateriaalin kaapata ja estä viestintä polku on laitteen tiedot ja korvata vääriä tietoja, jotka on todentanut varastettu avainten luontimateriaalin.

**Aiheuttaa uudelleenaktivointitarpeen**: hyökkääjä voi saada kokonaan tai osittain korvata laitteesta Palvelinohjelmiston joten mahdollisesti korvattu ohjelmiston voit hyödyntää laitteen tunnistetiedot avainten luontimateriaalin tai pitämällä tärkeimmät materiaalit cryptographic tilat olisivat käytettävissä laittomaan ohjelma.
   
**Tietojen paljastuminen muille käyttäjille**: Jos laite on käynnissä käsitellä ohjelmiston, käsitellä ohjelmistoon mahdollisesti voitu paljastaa tietojen luvattoman.

**Tietojen paljastuminen muille käyttäjille**: hyökkääjä voi hyödyntää lisätäkseen itse laitteen ja ohjaimen tai kentän yhdyskäytävä tai cloud-yhdyskäytävän käytöstä tietoja siphon välillä viestintä-polkunimen puretut materiaalia.

**Denial of Service**: laite on sammutettu tai kytketty tilaan jossa viestintä ei ole mahdollista (jossa on tahallista monet teollisuuden koneet).

**Aiheuttaa uudelleenaktivointitarpeen**: laite määritettävä uudelleen Tuntematon valvontajärjestelmä (ulkopuolella tunnettu kalibrointiparametreja) tilassa ja näin määrittää tietoja, jotka on väärin

**Käyttöoikeuksien laajentamisen**: laite, joka toteuttaa tiettyä toimintoa voidaan pakottaa tekemään jotain muuta. Jotka on ohjelmoitu puoli tapa avata venttiili voi esimerkiksi tricked Avaa aivan.

**Denial of Service**: laitteeseen on kytketty tilaan, jossa tiedonsiirto ei ole mahdollista.

**Aiheuttaa uudelleenaktivointitarpeen**: laite määritettävä uudelleen Tuntematon valvontajärjestelmä (ulkopuolella tunnettu kalibrointiparametreja) tilassa ja näin määrittää tietoja, jotka voivat olla väärin.
 
**Tietojen väärentämisen ja aiheuttaa uudelleenaktivointitarpeen tai hylkäämistä**: Jos ei ole suojattu (mikä tapahtuu harvoin tapauksessa kuluttajan kaukosäätimet) hyökkääjä voi käsitellä laitteen tilan anonyymisti. Hyvää kuvassa on kaukosäätimissä, joka voi ottaa minkä tahansa TV ja jotka ovat suosittuja prankster työkalut.

#### <a name="communication"></a>Viestintä

Laitteet, laitteet ja kentän yhdyskäytävä yhdyskäytävä laite ja cloud tiedonannon polun ympärillä uhat. Seuraavassa taulukossa on joitakin ohjeita ympäri avoinna olevia vastakkeita-laitteeseen tai näennäiseen Yksityisverkkoon:

| **Osa**               | **Uhka** | **Mitätöinti**                                      | **Riski**                                                                                                      | **Toteutus**                                                                                                                                                                                                                                                                                                                                                               |
|-----------------------------|------------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| IoT Hub laite              | TID        | (D) TLS (PSK/RSA) liikenteen salaaminen             | Salakuuntelulta tai haittaa laitteen ja yhdyskäytävän välisessä                             | Suojaus, protokolla tasolla. Meidän on kuva siitä, miten voit suojata ne kanssa määritettäessä mukautettuja protokollia. Useimmissa tapauksissa tiedonsiirto tapahtuu laitteesta IoT keskittimeen (laite Avaa yhteyden).                                                                                                                                                                 |
| Laite-laite               | TID        | (D) TLS (PSK/RSA) tietoliikenteen salaamiseen.            | Luetaan tietoja siirron laitteiden välillä. Tietojen luvaton käsittely. Ylikuormituksen laitteen kanssa uusia yhteyksiä | Suojaus, protokolla tasolla (MQTT/AMQP/HTTP/CoAP. Meidän on kuva siitä, miten voit suojata ne kanssa määritettäessä mukautettuja protokollia. DoS-hyökkäyksiltä Poistotapa on vertaiskone laitteet pilven tai kentän yhdyskäytävän kautta ja ne vain act asiakkaina kohti verkkoa. Peering saattaa johtaa suoran yhteyden jälkeen ollut liittymillä yhdyskäytävän vertaiskoneiden välillä |
| Laitteen ulkoinen yksikkö      | TID        | Laitteen ulkoinen yksikkö vahva laiteparin muodostaminen | Salakuuntelulta yhteyden laitteeseen. Viestinnän laite häiritse                     | Laiteparin muodostaminen turvallisesti ulkoista LE NFC/Bluetooth-laitteeseen. Toiminnallinen paneeli (inventointi)-laitteen hallinta                                                                                                                                                                                                                                                  |
| Kentän yhdyskäytävä Cloud Gateway | TID        | TLS (PSK/RSA) tietoliikenteen salaamiseen.               | Salakuuntelulta tai haittaa laitteen ja yhdyskäytävän välisessä                             | Suojaus, protokolla tasolla (MQTT/AMQP/HTTP/CoAP). Meidän on kuva siitä, miten voit suojata ne kanssa määritettäessä mukautettuja protokollia.                                                                                                                                                                                                                                                       |
| Yhdyskäytävä laite Cloud        | TID        | TLS (PSK/RSA) tietoliikenteen salaamiseen.               | Salakuuntelulta tai haittaa laitteen ja yhdyskäytävän välisessä                             | Suojaus, protokolla tasolla (MQTT/AMQP/HTTP/CoAP). Meidän on kuva siitä, miten voit suojata ne kanssa määritettäessä mukautettuja protokollia.                                                                                                                                                                                                                                                       |

Seuraavassa on joitakin esimerkkejä tämän luokan uhat:

**Denial of Service**: rajoitettuihin laitteet ovat yleensä DoS uhkaavat kun he aktiivisesti kuunnella saapuvat yhteydet tai ei-toivottujen datagrammeja verkon, koska hyökkääjä voi avata useita yhteyksiä samanaikaisesti eikä ne palvelun tai palvelun hyvin hitaasti tai laite voi olla ylivuotavan luvattoman liikenteen kanssa. Kummassakin tapauksessa laite voi tosiasiallisesti tulla käyttökelvoton verkossa.

**Tietojen väärentämisen, tietojen paljastumisen**: rajoitettu ja erikoiskäyttöön laitteet on usein one for all security tilat, kuten salasana tai PIN-koodi suojaus tai kokonaan pääsemästä verkkoon, eli myöntäessään pääsy tietoon, kun laite on samassa verkossa ja verkon jaetun avaimen on usein vain suojattu luottaminen. Tämä tarkoittaa sitä, että kun laitteen tai verkon jaetun salaisuuden esitetään, on mahdollista määrittää laite ja noudata peräisin laitteen tiedot.  

**Tietojen väärentämisen**: hyökkääjä voi kaapata tai osittain korvata lähetyksen ja väärentää aloittaja (mies keskellä)

**Aiheuttaa uudelleenaktivointitarpeen**: hyökkääjä voi kaapata tai osittain korvata lähetyksen ja väärien tietojen lähettäminen 

**Tietojen paljastumisen:** hyökkääjä voi eavesdrop lähetys- ja saada tietoja ilman lupaa **Denial of Service:** hyökkääjä saa lähetyksen signaali lukkiutumista ja estää tietojen jakelu

#### <a name="storage"></a>Varastointi

Jokaisen laitteen ja kentän yhdyskäytävä on jonkinlainen varastointi (väliaikainen Message Queuing-sovelluksessa tiedot, os kuva varastointi).

| **Osa**                            | **Uhka** | **Mitätöinti**                       | **Riski**                                                                                                                                                                                                                                                                                                                | **Toteutus**                                                                                                                                                     |
|------------------------------------------|------------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Device storage                           | TRID       | Varastointi salauksen allekirjoitus lokit | Luetaan tietoja varastosta (PII) tiedot, kaukomittaus tietojen peukaloinnin. Jonossa tai komennon ohjausobjektin tiedot välimuistiin. Määritykset tai laitteisto-ohjelmiston päivityksen paketit häirintä-tilassa tai välimuistin avulla toteutettu tilanvaihto jonossa paikallisesti voi johtaa tartunnan OS ja/tai järjestelmän osat                                         | Salausta message authentication code (MAC) tai digitaalinen allekirjoitus. Jos vahva voi käyttää resurssien hallinnan ohjaavat (ACL) ja käyttöoikeudet. |
| Laitteen OS kuva                          | TRID       |                                      | Häirintä OS / Käyttöjärjestelmäosien korvaaminen                                                                                                                                                                                                                                                                         | Vain luku-OS osio allekirjoitettu OS image-salaus                                                                                                                    |
| Kentän yhdyskäytävä varastointi (queuing tiedot) | TRID       | Varastointi salauksen allekirjoitus lokit | Luetaan tietoja varastosta (PII) tiedot, kaukomittaus tietojen luvaton käsittely jonossa tai komennon ohjausobjektin tiedot välimuistiin. Määritykset tai laitteisto-ohjelmisto (joka on tarkoitettu laitteiden tai kentän yhdyskäytävä) päivityspaketit häirintä-tilassa tai välimuistin avulla toteutettu tilanvaihto jonossa paikallisesti voi johtaa tartunnan OS ja/tai järjestelmän osat | BitLocker                                                                                                                                                              |
| Kentän yhdyskäytävä OS kuva                   | TRID       |                                      | Häirintä OS / Käyttöjärjestelmäosien korvaaminen                                                                                                                                                                                                                                                                          | Vain luku-OS osio allekirjoitettu OS image-salaus                                                                                                                    |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Laite- ja tapahtuma käsittely/cloud gateway vyöhyke

Cloud yhdyskäytävä on järjestelmä, joka sallii etäyhteyksien ja laitteiden tai kentän yhdyskäytävien useiden eri sivustojen kautta julkisen verkon tilaa pilvipohjainen valvontaa ja tietojen analyysi järjestelmän federation, tällaiset järjestelmät yleensä kohti. Joissakin tapauksissa cloud yhdyskäytävä voi heti helpotetaan erikoiskäyttöön laitteisiin varastoalueilta kuten tabletit tai puhelimet. Käsitellä tässä yhteydessä viittaamaan kiinteä atk-järjestelmän, joka ei ole sidottu samaan saittiin kytketyt laitteet tai kentän yhdyskäytävä on tarkoitettu "pilvi" ja jossa operatiiviset toimenpiteet estä kohdistetut käyttää fyysisesti mutta ei välttämättä "Julkinen pilvi-infrastruktuurin avulla.  Cloud yhdyskäytävä voi mahdollisesti yhdistää verkon virtualisointi-kerros, vesi-ja cloud-yhdyskäytävä ja kaikki sen kytketyt laitteet tai kentän yhdyskäytävien verkko-liikenteen avulla. Cloud-gateway itse on ole laitteen ohjausjärjestelmä eikä käsittelyn tai varastoihin, laitteen tietojen; Näissä tiloissa yhdessä cloud-yhdyskäytävän. Cloud gateway-vyöhykkeeseen kuuluu pilvi yhdyskäytävän itse kaikki kentän yhdyskäytävät ja suoraan tai epäsuorasti liitetyt laitteet.

Cloud yhdyskäytävä on enimmäkseen mukautetun rakennettu pala ohjelmisto palveluna jossa alttiina päätepisteiden, joihin kentän yhdyskäytävä ja laitteet muodostavat. Sellaisenaan se on suunniteltava ja huomioida siinä suojaukseen. Noudata [SDL](http://www.microsoft.com/sdl) prosessin suunnitteluun ja rakentamiseen tämän palvelun. 

#### <a name="services-zone"></a>Palvelut zone

Ohjausjärjestelmä (tai ohjain) on Ohjelmistoratkaisu, joka on liityntäkohtia, laite tai kentän yhdyskäytävä tai cloud-yhdyskäytävän valvomiseksi yhden tai useita laitteita ja/tai kerätä tai tallentaa tai analysoida tietoja laitteen esitys tai myöhemmin tarkastusta varten. Valvontajärjestelmät ovat vain kohteiden soveltamisalaan, tätä keskustelua, joka voi välittömästi helpottaa vuorovaikutusta ihmisten kanssa. Poikkeuksena ovat keskitason fyysinen ohjainpintojen laitteissa, kuten kytkin, jonka avulla voit poistaa laitteen käytöstä tai muuttaa muita ominaisuuksia henkilö ja joilla on ei ole toiminnallinen vastine, jota voi käsitellä digitaalisesti. 

Keskitason fyysinen valvonta ovat ne, jossa kaikki tällaiset koskevat logiikka-rajoittaman fyysinen valvonta pinnan funktiota siten, että vastaavaa funktiota voidaan aloittaa etäyhteyden kautta tai etäyhteyden input syöte ristiriidassa voidaan välttää – tällaisen intermediated ohjainpintojen ovat käsitteellisesti liitetty paikalliseen valvontajärjestelmää, joka hyödyntää pohjana olevan samalla tavalla kuin mitä tahansa muita kaukosäätöjärjestelmä, laite voidaan liittää rinnakkain. TOP uhkien cloud computing [Cloud Security Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) -sivulla voi lukea.


## <a name="additional-resources"></a>Lisää resursseja

Lue lisätietoja seuraavista artikkeleista:

- [SDL uhka mallinnus työkalu](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
- [Microsoftin Azure IoT reference Architecture-arkkitehtuurin](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
 
