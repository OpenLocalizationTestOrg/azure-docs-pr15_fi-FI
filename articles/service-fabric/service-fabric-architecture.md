<properties
   pageTitle="Palvelun kangasta arkkitehtuuri | Microsoft Azure"
   description="Palvelun kangasta on jaettu järjestelmien käytettävä pilveen skaalattava, luotettavaa ja helposti hallitun-sovelluksia. Tässä artikkelissa kerrotaan palvelun kangasta arkkitehtuuri."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Palvelun kangasta arkkitehtuuri

Palvelun kangasta on luotu kerroksittain alijärjestelmien. Nämä alijärjestelmien avulla voit kirjoittaa sovelluksia, jotka ovat:

* Erittäin käytettävissä
* Skaalattavissa
* Helpommin hallittaviin
* Testable

Seuraavassa kaaviossa on esitetty palvelun kangasta pää alijärjestelmien.

![Palvelun kangasta arkkitehtuuri kaavio](media/service-fabric-architecture/service-fabric-architecture.png)

Hajautettu järjestelmä mahdollisuuden pitää yhteyttä suojatusti klusterin solmujen välillä on tärkeää. On kantaluku pino transport-alirakenne, joka on suojattu tietoliikenne solmujen välillä. Edellä kuljetus alirakenne on liitetty viestintä-alirakenne, jossa klusterit eri solmujen yhden kohteen (nimeltään klustereiden), niin, että palvelun kangasta voit tunnistaa virheet, suorittaa Täytemerkki osavaltioiden ja anna yhdenmukaisia reititys. Luotettavuuden-alirakenne, tasoilla päälle federation-alirakenne on vastuussa luotettavuutta palvelun kangasta-palveluista, kuten replikoinnin, Resurssienhallinta ja automaattisesti menetelmien avulla. Liitetty viestintä-alirakenne pohjana myös isännöinti ja aktivoinnin-alirakenne, joka hallitsee elinkaari yksittäinen solmu-sovellukseen. Hallinta-alirakenne hallitsee elinkaari sovelluksia ja palveluja. Testattavuus-alirakenne avulla sovelluskehittäjät testaaminen niiden palveluihin Simuloitu virheitä ennen ja jälkeen-sovellusten ja palveluiden käyttöönotto tuotannon ympäristöt. Palvelun kangasta mahdollistaa ratkaisemiseksi palvelun sijainnit sen viestintä-alirakenne kautta. Kehittäjät käsiksi sovelluksen ohjelmoinnin mallit ovat tasoilla sekä sovelluksen mallin käyttöön sillä nämä alijärjestelmien päälle.

## <a name="transport-subsystem"></a>Transport alirakenne
Transport-alirakenne toteuttaa pisteestä pisteeseen-datagram tietoliikenneyhteyden. Kanavan käytetään sisällä palvelun kangasta klustereiden viestintää ja palvelun kangasta klusterin ja asiakkaiden välisen. Tukee yksisuuntainen ja pyynnön vastaus viestintä kuviot, jossa pohjan soveltamisesta lähetys- ja moni Federation kerros. Transport-alirakenne suojaa viestintä käyttämällä X509 varmenteet ja Windowsin käyttöoikeusryhmät. Tämä alirakenne käytetään sisäisesti palvelun kangasta ja ei voi käyttää suoraan application programming kehittäjille.

## <a name="federation-subsystem"></a>Liitetty viestintä alirakenne
Syy tietoja solmujen joukko hajautettu järjestelmässä, jotta tarvitset järjestelmän perusteella. Liitetty viestintä-alirakenne käyttää transport alirakenne myöntämä viestintä perusalkioiden ja stitches eri solmuissa yhdistetty klusteriin, se syy tietoja. Se on muiden alijärjestelmien - virhe tunnistus, Täytemerkki osavaltioiden ja yhtenäinen reititys tarvitsemia hajautettu järjestelmien perusalkioiden. Liitetty viestintä-alirakenne rakentuu hajautettu hash-taulukoita, joissa 128 bittistä suojaustunnuksen välilyönti. Alirakenne Luo soi topologian solmujen, kukin solmu varataan alijoukkoa suojaustunnuksen tilaa omistajuus Soita. Virhe tunnistaminen kerros käyttää kesto järjestelmä sydän beating ja välimiesmenettely perusteella. Liitetty viestintä-alirakenne takaa myös monimutkaisia liity ja lähtevien protokollat, jotka on luotu vain tunnusta ainoa omistaja, milloin tahansa. Tämä on Täytemerkki osavaltioiden ja yhtenäinen reititys oikeudet.

## <a name="reliability-subsystem"></a>Luotettavuuden alirakenne
Luotettavuuden alirakenne tarjoaa voit asettaa palvelun kangasta-palvelun tilan erittäin _Monistus_, _Automaattisesti hallinta_ja _Resurssin tasaustoiminto_.

* Replikointitoiminto varmistaa, että se ensisijainen palvelun tilan muutokset automaattisesti replikoida toissijainen replikoihin ylläpito yhdenmukaisuuden palvelun replikajoukon ensisijainen ja toissijainen replikoiden välillä. Replikointitoiminto on vastuussa quorum hallinta replikajoukon kopioiden välillä. Se toimii automaattisesti yksikkö replikointia toimintojen luettelo kanssa ja kokoonpanon uudelleenmääritys agentti antaa sen replikajoukon määrityksissä. Kyseisen määritys osoittaa, mitkä toiminnot voi replikoida on replikoita. Palvelun kangasta on oletusarvo-Monistus kutsutaan kangasta Monistus, jotka voidaan käyttää ohjelmoinnin mallin Ohjelmointirajapinnan tekemään palvelun tilan erittäin käytettävissä ja luotettavia.
* Automaattisesti hallinta varmistaa, että kun solmut on lisätty tai poistettu klusterista, lataa automaattisesti jaetaan käytettävissä solmujen välillä. Jos klusterin solmu epäonnistuu, klusterin automaattisesti uudelleen palvelun replikat pitämään käytettävyys.
* Resurssienhallinta sijoittaa palvelun replikoiden yli virheen toimialueen klusterin ja varmistaa, että kaikki automaattisesti yksiköt ovat toiminnassa. Resurssienhallinta saldojen palvelun resurssien myös pohjana Jaetun resurssivarannon klusterin solmujen saavuttamiseksi optimaalisen yhtenäisen kuormituksen jakauman yli.

## <a name="management-subsystem"></a>Hallinta: n alijärjestelmää
Hallinta-alirakenne on lopusta loppuun-palvelun ja sovelluksen elinkaaren hallinta. PowerShellin cmdlet-komennot ja järjestelmänvalvojan ohjelmointirajapinnan, joiden avulla voit valmistella, ottaa käyttöön, korjaustiedoston, päivittäminen ja valmistella varaustiedoista ilman tappio sovellusten käytettävyyden. Hallinta-alirakenne suorittaa tämä läpi seuraavat palvelut.

* **Klusterin hallinta**: Tämä on ensisijainen palvelu, joka toimii kanssa automaattisesti esimies-luotettavuutta sovellukset sijoittaminen solmut palvelun sijainti rajoitusten perusteella. Resurssienhallinta-automaattisesti alirakenne varmistaa, että rajoitukset ovat aina katkennut. Klusterin hallinta hallitsee sovellukset elinkaari säännöstä varaustiedoista valmistelu. Tämä integroituu kunto esimiehen Varmista, että sovelluksen käytettävyys ei ole menettää semanttinen kunto näkökulmasta päivitysten aikana.
* **Kuntotietojen hallinta**: tämän palvelun avulla kunnon valvonta sovellusten ja palvelujen klusterin kohteita. Klusterin kohteiden (kuten solmujen, osiot ja replikoiden) raportoida kunto tiedot, jotka yhdistetään sitten keskitetty kunto säilöön. Kuntotietojen tiedot on yleinen-ajankohta kunto tilannevedoksen palvelut ja solmujen niille päiville, useita solmujen klusterin, voit toteuttaa kaikki tarvittavat korjaavat toimenpiteet. Kunto-alirakenne raportoitu kunto kyselyn ohjelmointirajapinnan mahdollistavat kyselyn kunto-tapahtumat. Kunto-kysely API palauttaa raaka kunto-tiedot tallennetaan kunto-kaupasta tai koostetun, tulkita kunto tietojen tiettyyn klusteri kohteen.
* **Kuva kaupan**: tämän palvelun tarjoaa säilytystä ja sovelluksen binaaritiedostot jakelua. Tämä palvelu on yksinkertainen jaettua tiedostoa kaupan missä sovellukset ladataan ja ladataan.


## <a name="hosting-subsystem"></a>Isännöinnin alirakenne
Klusterin esimies ilmoittaa isännöintipalvelu alirakenne (käytössä kunkin solmun) palveluiden tarvitaan tietyn solmu hallintaa. Valitse isännöintipalvelu alirakenne hallitsee solmun ohjelman elinkaari. Se toimii luotettavasti ja terveyteen osat varmistaa, että on asetettu oikein ja on kunnossa.

## <a name="communication-subsystem"></a>Viestintä-alirakenne
Tämä alirakenne on luotettava messaging sisällä klusterin ja palvelun löytäminen Naming-palvelun kautta. Naming palvelun selvittää palveluiden nimet klusterin sijaintiin, ja käyttäjät voivat hallita palveluiden nimet ja ominaisuudet. Naming Service-esityspalvelua käyttämällä asiakkaat voivat turvallisesti viestiä minkä tahansa solmu ratkaista palvelunimi ja Nouda palvelun metatietojen klusterin. Käytä yksinkertaista Naming asiakkaan API, palvelun kangasta käyttäjien voit kehittää palvelut ja asiakkaiden voi ratkaiseminen nykyinen verkkosijainti solmu tehokkuuden huolimatta tai uudelleen koon muuttaminen klusterin.

## <a name="testability-subsystem"></a>Testattavuus alirakenne
Testattavuus on suunniteltu erityisesti testikäyttöön services-palvelun kangasta rakennettu Työkalut kuuluu. Työkalujen avulla kehittäjä helposti aiheuttaa kuvaava virheitä ja Suorita testi tilanteita, joissa on syytä olla ja vahvista monia: ssa tai siirtymät, jotka palvelu ilmetä koko aikana, kaikki valvottu ja turvallisten tavalla. Testattavuus sisältää myös järjestelmä suorittaa pidempi testejä, joiden avulla voit käydä läpi eri mahdolliset virheet menettämättä käytettävyys. Tämä voi testi tuotannon-ympäristössä.
