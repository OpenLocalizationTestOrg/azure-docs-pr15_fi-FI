<properties
    pageTitle="Mitä uusia ominaisuuksia, Pimennys Azure Työkalut"
    description="Lisätietoja saat Pimennys Azure-työkalujen uusimmat ominaisuudet."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Mitä uusia ominaisuuksia, Pimennys Azure Työkalut

## <a name="azure-toolkit-for-eclipse-releases"></a>Azure työkalujen Pimennys julkaisuissa

Tässä artikkelissa on tietoja eri versioista ja uusimmat päivitykset, Pimennys Azure-työkalut.

> [AZURE.NOTE] On myös Azure-työkalujen IntelliJ IDE varten. Lisätietoja on artikkelissa [Azure työkalujen IntelliJ varten].

### <a name="august-26-2016"></a>26 elokuussa 2016

Azure-työkalujen Pimennys - elokuussa 2016-versiossa, sisältää seuraavat parannukset:

* **Mukautetun JDK jaot**. Azure-työkalujen Pimennys, tukee nyt määrittäminen ja käyttöönotto Azure Web App-sovelluksen säilöön haluamaansa JDK-versio:
  - Lisäksi JDKs, Azure varten voit valita myös valituista saataville Azure-Azul järjestelmien Zulu OpenJDK versiot.
  - Voit myös määrittää omia JDK jakauman, jos lataat sen ZIP-tiedostona tallennustilan tilin.
* **Azure Resurssienhallintanäkymä parannukset**:
  - Virtuaalikoneen hallinta käyttämällä Azure's uuden Resurssienhallinta mallin tuki: Voit luettelo, luominen ja poistaminen resurssien hallinta-pohjainen näennäiskoneiden IDE poistumatta.
  - Tallennustilan blob tilinhallinta Azure's resurssien hallinnan, joka täydentää "perinteinen" tallennustilan tilit aiemmin toiminnot tuki.
* **Microsoft JDBC Driver 6.0 SQL Serveriä varten**. Tämä päivitys sisältää JDBC ohjain Microsoft SQL Server (6.0), joka on nyt sisältyvät kirjastoon, jossa voit helposti lisätä Java projektien, jolloin korvaaminen vanhemman version.

### <a name="june-29-2016"></a>29 kesäkuussa 2016

Azure-työkalujen Pimennys - kesäkuussa 2016-versiossa, sisältää seuraavat parannukset:

* **Java 8 vaatimus**. Azure-Työkalut, Pimennys edellyttää Java 8, vaikka tämä vaatimus koskee ainoastaan työkalujen - sovellusten edelleen käyttää kaikkien Java versioiden on Azure tukemat.
* **Uusimman Java-JDKs tuki**. Java-JDKs uusinta versiota tuetaan nyt varten Pimennys Azure-työkalut.
* **Azure SDK v2.9.1 tuki**. Azure-SDK uusin versio on nyt pienin vanhat tarvittavat varten, Pimennys Azure-työkalut.
* **Integroitu objektit**. Saat Pimennys Azure-Työkalut-välilehdessä nyt useita otoksen sovellusten sovelluskehittäjät Aloita.
* **Integrointi HDInsight-työkalu**. Azure-Työkalut, Pimennys nyt yhdistetty Azure's HDInsight-työkalut. Lisätietoja on artikkelissa [HDInsight Työkalut ‑laajennuksen Pimennys].
* **Remote virheenkorjaus Java Web Apps-sovelluksista**. Azure-työkalujen Pimennys, tukee nyt Azure App palvelun Java web Apps-sovellusten remote virheenkorjaus.
* **Tuki Pimennys Luna-versiossa.** Uusi tarvitaan vähintään Pimennys IDE versio on Luna.

### <a name="april-12-2016"></a>2016: n huhtikuun 12

Azure-työkalujen Pimennys - huhtikuun 2016-versiossa, sisältää seuraavat parannukset:

* **Azure SDK v2.9.0 tuki**. Azure-SDK uusin versio on nyt pienin vanhat tarvittavat varten, Pimennys Azure-työkalut.
* **Muut käytettävyyttä, vastausajan ja suorituskyvyn parannuksia liittyvät Azure Web App-tukea**. Suorituskyvyn optimointi-miten työkalujen yhteydessä Azure tuloksen määrä Lisää vastaa käyttöliittymässä.
* **Mahdollisuus poistaa Azure alueella Pimennys aiemmin verkkosovelluksen-säilö**. Saat Pimennys Azure-työkalujen avulla voit poistaa aiemmin Azure-Web-säilön poistumatta Pimennys nyt.

### <a name="march-7-2016"></a>7 maaliskuussa 2016

Azure-työkalujen Pimennys - maaliskuussa 2016-versiossa, sisältää seuraavat parannukset:

* **Nopea kevyt Java-sovellusten käyttöönoton tuki**. Pimennys varten Azure-työkalujen tukee nyt kevyt Java sovellukset Azure Web App säilöjen nopeaa käyttöönottoa, joten Java-sovellusten käyttöönotto nyt kestää sekuntia minuutin sijaan.
* **Web-sovellusten hallinta käyttämällä Azure Resurssienhallintanäkymä tuki**. Azure Resurssienhallintanäkymä työkalujen tukee nyt päällä, käynnistäminen ja lopettaminen Azure verkkosovelluksissa.
* **Päivitetty Tomcat, jota, ja Zulu OpenJDK jaot**. Azure-Työkalut, Pimennys tuki päivitettyjen versioiden Tomcat, jota ja Zulu OpenJDK Java-versioiden Azure pilvipalveluihin kyselyjä.

### <a name="january-4-2016"></a>4 tammikuussa 2016

Azure-työkalujen Pimennys - tammikuussa 2016-versiossa, sisältää seuraavat parannukset:

* **Tuki Zulu OpenJDK päivitykset**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Päivitetty Tomcat ja jota jaot**. Jota ja Tomcat jaot, jotka ovat käytettävissä Microsoft Azure-Pimennys Azure-työkalujen käyttäminen on päivitetty.
* **Ominaisuuden välistä eroa Pimennys ja IntelliJ työkalujen avulla Azure välillä**. Azure-työkalujen Pimennys ja [Azure Työkalut, IntelliJ] tukevat samoja ominaisuuksia.

### <a name="september-1-2015"></a>1 syyskuussa 2015

Azure-Työkalut, Pimennys - syyskuu 2015 release sisältää seuraavat parannukset:

* **Tuki Zulu OpenJDK päivitykset**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Päivitetty Tomcat ja jota jaot**. Jota ja Tomcat jaot, jotka ovat käytettävissä Microsoft Azure-Pimennys Azure-työkalujen käyttäminen on päivitetty. (Nämä jaot kehittäjät voivat luoda nopeasti kehittämisen ja testaa projektien varten Pimennys Azure-työkalut.
* **Automaattisesti päivittyvät viittaukset Tomcat ja jota tuki**. Lisäksi versiot Tomcat ja jota jotka ovat käytettävissä Azure-kehittäjät voivat nyt viitata nimitystä jakauman "uusimman (automaattinen päivitetty)", jotka päivittyvät automaattisesti uusimman jakautumisen kunkin pääversion jota tai Tomcat seuraavan kerran roolin esiintymät ovat kuluessa. (Kierrättäminen tapahtuu automaattisesti, mutta kehittäjät voivat manuaalisesti käynnistää Roskakorin palvelun Azure-portaalissa.) Uuden toiminnon tarkoittaa kehittäjät tarvitse Ota soveltaminen voivat päivittää niiden server-ohjelmiston uudelleen. (
*  Tämä toiminto on tällä hetkellä tarkoitettu vain kehittäminen ja testaa tarkoituksiin tai toiminta-ajatuksen tärkeät sovellukset, ja ei ole suositeltavaa tuotannon.)
* **Resurssienhallintanäkymä azure BLOB-objektit ja olevien taulukoiden Azure-tallennustilan**. Näin kehittäjät voivat suorittaa yleisiä tehtäviä ja tallennustilaa niiden palvelutiedot joukko suoraan Pimennys IDE. Esimerkki: poistaminen, lataus kokonaan BLOB-objektit.

### <a name="august-1-2015"></a>1 elokuussa 2015

Azure-Työkalut, Pimennys - elokuussa 2015 release sisältää seuraavat parannukset:

* **Hakemuksen tiedot Instrumentation hallintaa**. Voit hankkia, luoda ja hallita avaimien instrumentation hakemuksen tiedot suoraan Pimennys IDE päivitys.
* **Microsoft JDBC Driver 4.1 SQL Serveriä varten**. Tämä päivitys sisältää Microsoft SQL Server-ohjain JDBC tuki.
* **Azure SDK 2.7 versio**. Tämän uusimman päivityksen Azure SDK on uusi vanhat tarvittavat työkalut asennetaan Windows varten. (Huomaa tämä ei tarvita,-Windows käyttöjärjestelmissä.)
* **Päivitä Zulu OpenJDK v7 tuki**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].

### <a name="may-1-2015"></a>1. toukokuu 2015

Azure-Työkalut, Pimennys – toukokuu 2015 release sisältää seuraavat parannukset:

* **Parannettu palvelimen valinta UI**. Tässä versiossa yksinkertaistaa-Windows käyttöjärjestelmissä työkalujen käyttäminen.
* **Tuki maven-testi projektit**. Tämä versio tukee maven-testi projektien sovelluksina, jotka työkalujen voit ottaa käyttöön Azure ja määrittää sovelluksen tiedot.
* **Version 2.6 Azure SDK-paketissa**. Tämän uusimman päivityksen Azure SDK on uusi vanhat tarvittavat työkalut asennetaan Windows varten. (Huomaa tämä ei tarvita,-Windows käyttöjärjestelmissä.)
* **Käyttöönoton päivityksen sijaan julkaista sen uudelleen**. Jos käyttöönottoprojektin julkaistavia kun aiemmasta versiosta on jo julkaistu, työkalujen käyttää nyt Azure's käyttöönoton päivitystoimintoa edellisen käyttöönoton suljetaan ja uudelleen alusta alkaen aiemman samalta kuin sijaan. Tämä ottaa käyttöön oman pilvipalvelussa suorittamaan keskeytyksettä mahdollisuuksien tukea saavuttamiseksi suuren käytettävyyden myös päivityksen aikana ja nopeuttaa uudelleen julkaisuprosessin.
* **Tuki saat uusimmat Zulu OpenJDK - v8 update 40**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].

### <a name="march-9-2015"></a>9. maaliskuu 2015

Azure-Työkalut, Pimennys - maaliskuu 2015 release sisältää seuraavat parannukset:

* **Mac, Ubuntu ja muita Linux flavors tuki**. Tässä versiossa, Pimennys Azure-työkalujen Lisää tuki Mac OS- ja useita Unix-ympäristöissä, jotta sovelluskehittäjät asentaa työkalut, voit luoda ja määrittää Java projektien julkaiseminen Azure pilvipalveluihin (PaaS) Pimennys käynnissä kuin Windows-käyttöjärjestelmissä.

>[AZURE.NOTE] Tämä ominaisuus on esikatselu ja sitä ei suositella käytettäväksi tuotantoympäristössä. Asiakas ei tueta palvelussa palvelutasosopimusta (SLA), mutta kaikki palaute säteilyannokseen ja suositellaan.

* **Uuden sovelluksen tiedot-laajennus**. Kehittäjät voivat nyt voi määrittää Automaattinen palvelimen telemetriatietojen käyttäminen Azure hakemuksen tiedot.
* **Mukaan-pohjainen komentoriviltä käyttöönoton automaatio**. Tämän ominaisuuden avulla voit automatisoida julkaisu niiden ominaisuuksissa käyttämällä Ant ulkopuolella Pimennys uudempia versioita kehittäjät. Valmiiksi luodun komentosarjan määritetään automaattisesti projektin, kun ensimmäisen kerran se otetaan käyttöön Pimennys- ja myöhemmissä käyttöönotoissa komentosarjan avulla voidaan automatisoida täysin ominaisuuksissa komentoriviltä vain kautta.
* **Tomcat ja jota saatavuudesta Azure yksinkertaisempi, nopeammin käyttöönottoa varten**. Kehittäjät voivat nyt viitata eri Tomcat ja jota versiot, jotka ovat käytettävissä Azure suoraan sen sijaan, että sinun tarvitsee ladata Java-palvelimen niiden tileihin (tai työkalujen kautta), niin ei ole tarpeen Java palvelimen skenaarioissa pika-aloitusopas lataaminen.
* **Pikavalikon julkaisun yhteyttä Azure pilvipalveluihin Java verkkosovelluksissa**. Voit pienentää yksinkertaisia kehittäminen ja testaa skenaarioita learning käyrä kehittäjät nyt julkaista Java-sovellusten siirtyä Azure. Sen sijaan, että käsitellään luominen ja määrittäminen Azure-käyttöönotto-projektin loppuun, sovellusten otetaan käyttöön oletusarvoinen esiintymässä Tomcat v8 ja Zulu JVM (OpenJDK).

### <a name="january-30-2015"></a>30. tammikuussa 2015

Azure-Työkalut, Pimennys - tammikuussa 2015 release sisältää seuraavat parannukset:

* **IBM® WebSphere® sovelluksen Server Liberty Core tuki**. Tässä versiossa Lisää IBM WebSphere sovelluksen Server Liberty Core tuetun sovelluksen palvelinten, josta työkalujen ei voi ottaa käyttöön Azure luettelon. Tämä uusin lisäys laajentaa sovelluksen palvelimissa, jotka ovat tuettuja nykyinen luettelo &quot;ulos-valmiin&quot; mukaan työkalujen eri versiot Tomcat, jota, JBoss ja GlassFish joka jo mukana.
* **Hakemuksen tiedot SDK sisällyttämistä**. Äskettäin julkaissut asiakkaan API kirjaston (v0.9.0) on nyt Azure kirjastojen Java pakkaaminen osa.
* **Päivitetty paketti Java Azure-kirjastoja**. Tämä päivitys sisältää Azure kirjastojen Java v0.7.0 ja tallennustilaa Client API v2.0.0 sekä v0.9.0 sovelluksen havainnollistamisen SDK äskettäin julkaistu.

### <a name="november-12-2014"></a>12. marraskuussa 2014

Azure-Työkalut, Pimennys - marraskuussa 2014: n julkaisu sisältää seuraavat parannukset:

* **Azure SDK 2,5 tuki**. Tämän uusimman päivityksen Azure SDK on uusi vanhat tarvittavat työkalujen varten.
* **Tuki päivitetyn version Zulu OpenJDK v1.8, v1.7 ja v1.6 paketit**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Uuden normaalin D koon pilvipalveluihin tuki**, jotka antavat parempi suorituskyky ja lisää muistiresurssit. Lisätietoja on artikkelissa [virtuaalikoneen ja Azure Cloud palvelun koot].

### <a name="october-17-2014"></a>17. lokakuussa 2014

Azure-Työkalut, Pimennys - lokakuussa 2014: n julkaisu sisältää seuraavat parannukset:

* **Suorituskykyä on parannettu: n Julkaise Cloud skenaarioiden**. Tilaustiedot lataaminen on paljon aiempaa nopeammin, kun käyttäjillä on useita tilauksia ja tallennustilaa tilit.
* **Tuki Zulu OpenJDK v1.8 paketin päivitetyn version**. Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Kuinka 3 osapuolen JDKs vanhempien versioiden tuki**. Vanhentuneen JDK pakettien ei enää näytetä uudet käyttöönoton projektit-lokikirjaus ‑valikosta. Viittaavan poistetuista JDK pakettien säilyvät onnistuu kerran käytössä, mutta se on suositeltavaa hankkeet riippuvaisia viimeisimmän päivityksen voit tehdä aiemmin projektia.
* **Päivitetty versio Azure-kirjastoja Java asiakkaan API-kirjasto**. Lisätietoja on artikkelissa [Microsoft Azure Client API].
* **Virheenkorjauksia.** Tässä versiossa on useita muita virheenkorjauksia on perustuu käyttäjän raportteja ja testaus.

### <a name="august-5-2014"></a>5. elokuussa 2014

Azure-Työkalut, Pimennys - elokuussa 2014: n julkaisu sisältää seuraavat parannukset

* **Azure SDK 2.4 tuki.** Pimennys työkalujen vanhempia versioita ei käsitellä äskettäin SDK.
* **Päivitetyt versiot Zulu OpenJDK, v1.6 1.7 ja v1.8 paketit.** Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Java asiakkaan API kirjaston Azure kirjastojen pakkaaminen päivitetyn version.** Lisätietoja on artikkelissa [Microsoft Azure Client API].
* **Viimeisimmät tuki julkaista asetukset-tiedostomuodossa.** Version 2.0 Julkaisuasetukset-tiedostomuodon tuki on lisätty.
* **Julkaise Cloud-toiminto arkkitehtuuri muutoksia.** Työkalujen on nyt äskettäin Microsoft Azure asiakkaan Ohjelmointirajapinnan käyttäminen Java sen julkaista pilvipalveluun tukea.
* **Virheenkorjauksia.** Tässä versiossa on useita käyttäjä pyytänyt virheenkorjauksia.

### <a name="june-12-2014"></a>2014 12 kesäkuussa

Azure-Työkalut, Pimennys - kesäkuussa 2014: n julkaisu on pieni ylläpidon päivitys, joka sisältää seuraavat parannukset:

* **Zulu OpenJDK paketin v1.8 tuki.** Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Päivitetty Zulu OpenJDK v1.6 ja 1.7 pakettien versiot.** Lisätietoja on artikkelissa [Zulu OpenJDK Azul järjestelmien web-sivulle].
* **Java asiakkaan API kirjaston Azure kirjastojen pakkaaminen päivitetyn version.** Lisätietoja on artikkelissa [Microsoft Azure Client API].
* **Virheenkorjauksia.** Tässä versiossa on useita käyttäjä pyytänyt virheenkorjauksia.

### <a name="april-4-2014"></a>4. huhtikuussa 2014

Pimennys - huhtikuussa 2014: n julkaisu Azure ‑laajennuksen on julkaissut. Tämä on mukana 2.3 Azure SDK-paketissa, joka on ennen tarvittavat ja ladataan automaattisesti, kun asennat laajennuksen release päivitys. Tässä päivityksessä on uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen helmikuussa 2014 esikatselu:

* **Azure SDK 2.3 julkaisemisen tuki.** Pimennys - huhtikuussa 2014: n julkaisu Azure ‑laajennuksen edellyttää Azure SDK 2.3. Kun käytät uusi laajennus, jos sinulla ei ole jo Azure SDK 2.3, jonka pyydetään sallimaan sen asennuksen. Älä käytä Azure SDK 2.3 laajennus aiempien versioiden kanssa.
* **Päivittäminen sovellusten ilman valmis paketin käyttöönotto.** Otettaessa Java-sovelluksia, jotka ovat osa projektin laajennus nyt automaattisesti lataa ne valitun tallennustilan tilille niin, että voit päivittää sen ja Roskakorin ottamaan uusimman sovelluksen bittien eikä sinun tarvitse uudelleen ja ota uudelleen koko pakkauksen rooli-esiintymät.
* **Tomcat 8: ssa on nyt tunnistettavassa sovelluspalvelin.** Jos valitset Tomcat 8 asennuksen kansio tietokoneessa **Azure käyttöönoton projekti** -valintaikkunasta **palvelin** -välilehti, laajennus nyt automaattisesti tunnistaa sen ja voivat ottaa Tomcat 8 automaattinen tavalla, kuin vanhempien versioiden Tomcat jo luettelossa.
* **Azul Zulu OpenJDK paketin päivitykset: v1.7 päivitys 51 ja v1.6 Päivitä 47.** Tehokas tässä versiossa, Azul järjestelmän Zulu Avaa JDK v7 paketin 51 on saatavana päivitys. Zulu Avaa JDK v6 paketteja, käynnistää on käytettävissä, Päivitä 47 alkaen. Nämä päivitykset, jotka ovat saatavilla ollutta Zulu Avaa JDK v7 paketin lisäksi päivittää 45 tai päivitä 40 ja Päivitä 25.
* **A8 ja A9 Microsoft Azure Virtual Machine koko tuki.** Voit nyt ottaa pilvipalveluun hyvin muistin A8 ja A9 virtuaalikoneen koot. Lisätietoja näiden AM koot artikkeleissa [virtuaalikoneen ja Azure Cloud palvelun koot].
* **Automaattinen uudelleenohjaus HTTP-HTTPS SSL käytössä roolien.** Kun cloud-palvelu sisältää vain HTTPS rooleihin, jos käyttäjän pyynnöt määrittää HTTP, se ohjaa automaattisesti HTTPS. Ei tarvitse luoda erilliset roolin HTTP-pyyntöjen.
* **Voit ilmaista emulaattorin paikallisen emuloinnin käytetään.** Azure Express emulaattorin käytetään nyt emulaattori kun virheenkorjaus sovellustesi paikallisesti.
* **Azure uusi nimi on Microsoft Azure nimellä.** Käyttöliittymän näyttöihin vastaavat nyt, että Azure uusi nimi ja kutsua enää Azure.

### <a name="february-6-2014"></a>6. helmikuussa 2014

Azure-laajennus Pimennys - helmikuussa 2014 esikatselu on julkaissut. Päivitys sisältää uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen lokakuussa 2013 esikatselu:

* **Tuki SSL purkaminen.** Suojatun purkaminen (Secure Sockets LAYER) on lisätty ominaisuutena, joilla voit helposti tuen ottaminen käyttöön Hypertext Transfer Protocol Secure (HTTPS) Java-ympäristön Azure, valitse tarvitsematta määrittäminen Java sovelluspalvelin SSL. Tämä sopii erityisen istunnon affiniteetti ja/tai todennettu viestintä skenaarioita. Esimerkiksi silloin, kun Access ohjausobjektin Service (ACS) suodattimen, joka tukee jo työkalujen avulla. Lisätietoja on artikkelissa [SSL purkaminen] ja [niiden käyttöä SSL purkaminen].
* **GlassFish 4 on nyt tunnistettavassa sovelluspalvelin.** Jos valitset GlassFish 4 asennuksen kansio tietokoneessa **Azure käyttöönoton projekti** -valintaikkunasta **palvelin** -välilehti, laajennus nyt automaattisesti tunnistaa sen ja voivat ottaa GlassFish OSE 4 automaattinen tavalla, kuin jo luettelossa GlassFish OSE 3-versio.
* **Azul Zulu OpenJDK paketin päivitys 45.** Tehokas tässä versiossa, Azul järjestelmän Zulu (Avaa JDK v7 pakkaus) päivitys 45 on nyt käytettävissä. Tämä on saatavilla ollutta päivityksen lisäksi 40 ja Päivitä 25.
* **Tuen automaattisesti, jos yksityinen päätepisteen portit.** Voit määrittää yksityinen portin automaattinen syötteen päätepisteet ja sisäinen päätepisteet kertoaksesi Azure määrittää portin kyseisen päätepisteen automaattisesti. Aiemmin voi liittää vain tietyn portin numeroa.
* **Tuki mukauttaminen varmenteen nimi (CN) itse allekirjoitetun varmenteen luominen Käyttöliittymää.** Aiemmin koodattu nimi on käytetty kaikkien uusien varmenteiden; Voit nyt määrittää oman varmenteen nimi erotat kesken useasta varmenteesta eri tarkoituksiin Azure-portaalissa.
* **Azure työkalurivin:** Azure työkalurivin on päivitetty seuraavat muutokset: 
    * ![][ic710876]Tämä on lisätty **Uusi Azure käyttöönoton projekti**.
    * ![][ic710877]Tämä on lisätty pikanäppäimeksi itse allekirjoitetun varmenteen luominen-valintaikkuna.
* **A5 Azure virtuaalikoneen koon tuki.** Voit nyt ottaa pilvipalveluun hyvin muistiin A5 virtuaalikoneen kokoa. Saat lisätietoja tämän AM koon [virtuaalikoneen ja Azure Cloud palvelun koot].
* **Microsoft Windows Server 2012 R2-tuki.** Voit valita Windows Server 2012 R2 nyt kuin cloud-käyttöjärjestelmä.

### <a name="october-22-2013"></a>22. lokakuussa 2013

Azure-laajennus Pimennys - lokakuussa 2013 Preview on julkaissut. Päivitys sisältää uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen syyskuu 2013 esikatselu:

* **Azure SDK 2.2 julkaisemisen tuki.** Pimennys - Azure ‑laajennuksen lokakuussa 2013 esikatsella tukee Azure SDK 2.2. Laajennus toimivat edelleen Azure SDK 2.1 ja automaattisesti asentaa Azure SDK 2.2, jos sinulla ei ole jo ainakin Azure SDK 2.1 asennettuna.
* **Azul Zulu OpenJDK paketin päivitys 40.** Kuten syyskuu 2013: n esikatselu-laajennuksen nyt mahdollistaa käyttämällä antamalla kolmannen osapuolen JDK suoraan Azure-tarvitsematta ladata oman JDK. Lokakuussa 2013-versiossa Azul järjestelmän Zulu (Avaa JDK v7 pakkaus) päivitys 40 on nyt käytettävissä. Tämä on alun perin julkaistun päivityksen 25 lisäksi.
* **Cloud käyttöönoton linkki tehtävän lokiin.** Sisällä Azure tehtävän lokiin, kun käyttöönoton tilana on **julkaistu**, voit valita **julkaistu** koska se on nyt käyttöönoton; linkki käyttöönoton avataan selaimessa. ( **Julkaistu** tila on aiemmin lukee **käynnissä**.)
* **Kohteen OS valinnan osoitteessa julkaista aika.** **Julkaise Azure** -valintaikkunan sisältää uuden kentän **Kohde OS**, joiden avulla voit määrittää kohde-käyttöjärjestelmän auttaa.
* **Automaattinen korvaa edellisen käyttöönotto.** **Julkaise Azure** -valintaikkunassa on uusi **Korvaa edellisen käyttöönoton**valintaruutu. Jos tämä asetus on valittuna, kun uusi käyttöönoton julkaistaan automaattisesti korvaa edellisen käyttöönoton; kohtaat ei &quot;409 ristiriidan&quot; ongelmat, kun julkaiseminen entisellä paikallaan ilman julkaisun edellisen käyttöönotto.
* **Jota 9 on nyt tunnistettavassa sovelluspalvelin.** Jos valitset jota 9 asennuksen kansio tietokoneessa **Azure käyttöönoton projekti** -valintaikkunasta **palvelin** -välilehti, laajennus nyt automaattisesti tunnistaa sen ja voivat ottaa jota 9 automaattinen tavalla, kuin vanhempien versioiden jota jo luettelossa.
* **Lisää rooli projektin pikavalikosta.** **Azure** -projektin pikavalikko sisältää nyt uuden valikon-vaihtoehdon, **Lisää rooli**, joka sisältää nopeampaa ja havaittavissa tapa, jolla voit lisätä uuden roolin Azure projektiin.
* **Java-kirjaston Azure-kirjastojen pakkaaminen päivitys.** Tämä perustuu [Microsoft Azure Client API]0.4.6 versio.

### <a name="september-25-2013"></a>Syyskuu 25, 2013

Azure-laajennus Pimennys - syyskuu 2013 Preview on julkaissut. Päivitys sisältää uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen elokuussa 2013 esikatselu:

* **Mahdollisuus käytettävissä Azure Azul Zulu OpenJDK paketin käyttöönotto.** Muotoilujen käyttäminen Azure käyttöönoton JDK on lisätty uusi asetus. Tällä toiminnolla voit ottaa kolmannen osapuolen JDK paketin suoraan Azure pilvipalveluun, valitse eikä sinun tarvitse lataa omia. Azul järjestelmien tarjoaa ensimmäisen kuten pakata kutsuttu Zulu, OpenJDK, joka otetaan käyttöön nyt tämä vaihtoehto perusteella.
* **Java-kirjaston Azure-kirjastojen pakkaaminen päivitys.** Tämä perustuu [Microsoft Azure Client API]0.4.5 versio.

### <a name="august-1-2013"></a>1 elokuussa 2013

Azure-laajennus Pimennys - elokuussa 2013 Preview on julkaissut. Tämä on mukana Azure SDK 2.1, joka on ennen tarvittavat ja ladataan automaattisesti, kun asennat laajennuksen release päivitys. Päivitys sisältää uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen heinäkuussa 2013 esikatselu:

* **Mahdollisuus jättää paikallisen JDK ja paikallisen sovelluspalvelin asennuspaketin poistaminen.** Lataaminen JDK ja pilvipalvelusta käyttöönoton aikana sovelluspalvelin on suositeltavampaa komponentit upottaminen pakkauksen, koska lataamalla kohteet tulokset käyttöönotto-paketin pienemmäksi, käyttöönoton nopeammin ja helpommin ylläpito. Tämän vuoksi vaihtoehto, jos haluat sisällyttää JDK ja sovelluspalvelin-asennuspaketin on poistettu. Olemassa olevan projekteja, jotka on määritetty käyttämään paikallisen JDK ja paikallisen sovelluspalvelin kuin asennuspaketin osa muunnetaan automaattisesti automaattisen-Lataa JDK ja sovelluksen palvelimen pilvitallennustilaa.
* **Azure SDK 2.1 julkaisemisen tuki.** Azure-laajennus Pimennys - elokuussa 2013 Preview edellyttää Azure SDK 2.1. Älä käytä elokuussa 2013 Preview'ssa Azure SDK aiempien versioiden kanssa ja käytä Azure SDK 2.1 Azure-laajennus aiempien versioiden kanssa Pimennys.
* **Tuki Pimennys Kepler-versiossa.** Tähän liittyviä, uusi pienin pakollinen Pimennys IDE versio on Indigo. Pimennys Azure ‑laajennuksen testataan enää virallisesti Helios.

### <a name="july-3-2013"></a>3. heinäkuussa 2013

Azure-laajennus Pimennys - heinäkuussa 2013 Preview on julkaissut. Tässä päivityksessä on uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen toukokuussa 2013 esikatselu:

* **Mahdollisuus luoda uuden tallennustilan tilin.** **Lisää tallennustilaa tili** -valintaikkunassa on lisätty **Uusi** -painike. Voit luoda tallennustilan tilin sisällä Pimennys-laajennus tarvitsematta Azure hallinta-portaalin kirjautuminen. (Jo tarvitset Azure tilaus, voit käyttää tätä toimintoa.) Saat lisätietoja tallennustilan uuden tilin luominen [Luo uusi tallennustilan-tili].
* **Uusi &quot;(automaattinen)&quot; tallennustilan tilin automaattinen käyttöönoton JDK ja palvelimen ja välimuistiin tallentaminen vaihtoehto.** Käytettäessä **ladata automaattisesti** -vaihtoehto JDK ja sovelluspalvelin, voit määrittää URL-osoite ja tallennustilaa tilin Kun lataus JDK **(automaattinen)** ja sovelluspalvelin- tai Azure välimuistiin käytettäessä. Valitse nämä ominaisuudet automaattisesti käyttää saman tallennustilan tilin **Julkaise Azure** -valintaikkunassa valitse haluamasi vaihtoehto. [Luodaan Hei maailma sovellusta varten Pimennys Azure] -opetusohjelma on päivitetty uusien **(automaattinen)** -vaihtoehtoa.
* **Mahdollisuus määrittää Azure Palvelupäätepisteet.** Määritä service päätepisteet, jotka määrittävät, onko sovelluksen on otettu käyttöön ja hallinnassa on käytössä yleinen Azure-ympäristö, Azure 21vianetin ylläpitämä Kiinan tai yksityisen Azure ympäristössä. Lisätietoja on artikkelissa [Azure Palvelupäätepisteet].
* **Suurissa yrityksissä määrittää paikallisen säilön resurssin.** Silloin, kun käyttöönoton on liian suuri sisältyvän approot oletuskansioon, voit nyt määrittää paikallisen säilön resurssin käyttöönoton kohteeksi, että JDK ja sovelluspalvelin. Lisätietoja on artikkelissa [Käyttöönotto suurissa yrityksissä].
* **A6 ja A7 Azure virtuaalikoneen tuki.** Voit nyt ottaa pilvipalveluun hyvin muistin A6 ja A7 virtuaalikoneen koot. Lisätietoja näiden koot artikkeleissa [virtuaalikoneen ja Azure Cloud palvelun koot].
* **Java-kirjaston Azure-kirjastojen pakkaaminen päivitys.** Tämä perustuu [Microsoft Azure Client API]0.4.4 versio.

### <a name="may-1-2013"></a>1. toukokuussa 2013

Pimennys - Azure ‑laajennuksen saattaa 2013 Preview on julkaissut. Tämä on mukana versiosta 2.0 Azure SDK-paketissa, joka on ennen tarvittavat ja ladataan automaattisesti, kun asennat laajennuksen pää päivitys. Tämä julkaisu sisältää uusia ominaisuuksia, virheenkorjauksia ja jotkin palautetta perustuva käytettävyyden parannuksia jälkeen helmikuu 2013 esikatselu:

* **Automaattinen lataaminen JDK ja sovelluksen palvelimen ja käyttöönoton, Azure-tallennustilan.** Uusi vaihtoehto, jolla automaattisesti Lataa määritetyn Azure-tallennustilan tilin valitun JDK ja sovelluspalvelin-tarvittaessa, ja ottaa käyttöön komponentit sieltä asennuspaketin upottaminen tai ottaa käyttäjän lataa sitten manuaalisesti sijaan. Tavallisesti Pyydetty toiminto parantaa huomattavasti helpottaminen käyttöönotto JDK ja palvelimen osat, erityisesti niiden työryhmälle. Katso hallintapaketteihin, joka käyttää näitä asetuksia, [luomalla Hei maailma sovellus Pimennys Azure varten].
* **Keskitetty tallennustilan tilin seuranta ja mahdollisuus viittaus tallennustilan tilit Lisää helposti (avattava luettelo-ohjausobjektin kautta).** Tämä koskee useita ominaisuuksia, jotka perustuvat tallennuspaikkaa, kuten JDK ja palvelimen osan käyttöönotto ja tallentamisesta välimuistiin. Lisätietoja on artikkelissa [Azure-tallennustilan Asiakasluettelo].
* **Julkaise Cloud ohjattuun yksinkertaistettu etäkäyttöpalvelimen asetukset.** Sinun on suoritettava on käyttäjänimi ja salasana Kirjoita käyttöön etäkäyttöpalvelimen tai jätä se tyhjäksi, jos haluat säilyttää etäkäyttöpalvelimen käytöstä.
* **Java-kirjaston Azure-kirjastojen pakkaaminen päivitys.** Tämä perustuu [Microsoft Azure Client API]0.4.2 versio.
* **Windows Server 2012 muistilappuja istuntojen tuki.** Aiemmin muistilappuja istuntojen aiemmin vain Windows Server 2008 R2, valitse nyt molemmat cloud käyttöjärjestelmän kohteiden tuki istunnon affiniteetti.
* **Paketin lataamisen suorituskykyparannukset.** Vaikka JDK ja sovelluspalvelin upotettuihin asennuspaketin, lataa osa käyttöönottoprosessin noin kahdesti voidaan yhtä nopeita verrattuna aiempiin versioihin verrattuna.

### <a name="february-8-2013"></a>8. helmikuu 2013

Azure-laajennus Pimennys - helmikuu 2013 Preview on julkaissut. Tämä on pieni päivitys, joka sisältää virheenkorjauksia, palaute perustuva käytettävyyden parannuksia ja muutamia uusia ominaisuuksia, koska marraskuussa 2012 esikatselu:

* Tuki JDKs-palvelimet sovelluksen käyttöönotto ja haluamaansa muiden osien julkinen tai yksityinen azuren blob-tallennustilan lataukset sen sijaan, että myös ne asennuspaketin pilveen käyttöönoton yhteydessä.
* Mahdollisuus muuttaa järjestystä, jossa rooli käyttäjän määrittämät osat käsitellään, **Siirrä ylös** - ja **Siirrä alas** -painikkeet ** **Azure ominaisuudet**osien** lisäämällä kautta.
* Päivityksen **paketti Azure-kirjastojen Java** -kirjastoon, [Microsoft Azure Client API]0.4.0 version perusteella.

### <a name="november-5-2012"></a>Marraskuussa 5, 2012

Azure-laajennus Pimennys - marraskuussa 2012 esikatselu on julkaissut. Tämä on pää päivitys, joka sisältää useita uusia ominaisuuksia sekä muita virheenkorjauksia ja palautetta perustuva käytettävyyden parannuksia, koska syyskuu 2012 esikatselu:

* Microsoft Windows Server 2012 cloud käyttöjärjestelmänä tuki.
* Azure yhtä sijaitsevat välimuistiin tuki memcached asiakkaiden tuki.
* Sisällyttäminen Apache Qpid JMS asiakkaan kirjastojen Azure AMQP perustuvan viestinnän hyödyntämistä varten.
* Parannettu **Uusi projekti** -ohjattu, ja uuden sivun lopussa, joka tarjoaa käyttäjille mahdollisuuden nopeasti käyttöön useita yleisiä ominaisuuksia niiden Projectissa: muistilappuja istunnot, välimuistin ja remote virheenkorjaus.
* Automaattinen vähennys roolin esiintymien 1 suoritettaessa Laske-emulaattorin sidonta porttiin server esiintymät ristiriitojen välttämiseksi.

### <a name="september-28-2012"></a>28 syyskuussa 2012

Azure-laajennus Pimennys - syyskuu 2012 esikatselu on julkaissut. Palvelun päivitys sisältää useita muita virheenkorjauksia, koska elokuussa 2012 esikatselu sekä joitakin palautetta perustuva käytettävyyden parannuksia aiemmin ominaisuudet:

* Microsoft Windows 8: ssa ja Microsoft Windows Server 2012 kuin kehittäminen-käyttöjärjestelmä, joka esti aiemmin toimimasta oikein kyseisissä käyttöjärjestelmissä laajennus ongelmien ratkaiseminen tuki.
* Parannettu tuki määrittäminen päätepisteen portti alueita.
* Välilyöntejä sisältävien tiedostopolkujen liittyvät virheenkorjauksia.
* Rooli kontekstin valikon parannukset nopeampaa roolin kielikohtaiset asetukset.
* Vähäisiä tarkennukset **Julkaise cloud** ohjattu ja muita virheenkorjauksia määrä.

### <a name="august-28-2012"></a>28. elokuussa 2012

Azure-laajennus Pimennys - elokuussa 2012 esikatselu on julkaissut. Palvelun päivitys sisältää muita virheenkorjauksia, koska heinäkuussa 2012 esikatselu sekä useita palautetta perustuva käytettävyyden parannuksia aiemmin ominaisuudet:

* Sisällä Azure Access ohjausobjektin suodatin-valintaikkunassa:
    * Sovelluksen SODAN tiedosto- **vaihtoehto, jos haluat upottaa allekirjoitusvarmenteen** yksinkertaistaa cloud käyttöönotto.
    * **Itse allekirjoitetun varmenteen luominen** ACS suodattimessa UI. Lisätietoja Azure Access ohjausobjektin Services-suodatin Katso, [miten todennusta Internet-käyttäjille Azure Access ohjausobjektin palvelun käyttämällä Pimennys kanssa].
* Azure-käyttöönotto-projektin ohjatusta (koskee myös on rooli palvelinkokoonpanon ominaisuutta-sivulla):
    * **Automaattinen JDK sijainnin etsiminen** tietokoneesta (joka voi ohittaa tarvittaessa).
    * Kun valitset sovelluksen palvelimen asennuksen hakemiston **automaattinen tunnistaminen palvelimen tyyppi** .

### <a name="july-15-2012"></a>15. heinäkuussa 2012

Azure-laajennus Pimennys - heinäkuussa 2012 esikatselu-osoitteet on suurin prioriteetti-virheet löydy ja/tai ilmoittaa kesäkuussa 2012 julkaisemisen jälkeen käyttäjien määrä, jotka on julkaistu. Tämä on vain palvelun päivitystä, sisältää ole uusia ominaisuuksia.

### <a name="june-7-2012"></a>7. kesäkuussa 2012

Azure laajennuksen Pimennys - kesäkuussa 2012 CTP on julkaissut. Uusia ominaisuuksia:

* **Ohjatun uuden Azure käyttöönottoprojektin:** Voit valita oman JDK Java sovelluspalvelin ja Java-sovellusten suoraan ohjatussa parannettu Käyttöliittymä. Ulos,-valmiilla palvelimen määrityksiä valittavana luettelossa ovat Tomcat 6, Tomcat 7, GlassFish OSE 3, jota 7, jota 8, JBoss 6 ja JBoss 7 (erillinen). Lisäksi voit mukauttaa luettelon palvelimen määrityksiä. Tämä Käyttöliittymän parannus on vaihtoehtoinen vetäminen ja pudottaminen pakatut tiedostot ja kopioi Käynnistys-komentosarjat päälle, joka oli aiemmin tärkein lähestymistavan. Tämä menetelmä edelleen toimii hyvin, mutta todennäköisesti käytetään vain monimutkaisemman skenaarioita.
* **Palvelinkokoonpanon rooli ominaisuutta-sivulla:** Voit siirtyä helposti JDKs Java sovelluksen palvelinten ja sovellusten käyttöönoton liittyvän projektin luomisen jälkeen. Lisätietoja on artikkelissa [Server configuration ominaisuudet].
* **&quot;Cloud julkaiseminen&quot; ohjatun:** on helppo tapa ottaa käyttöön projektin Azure suoraan Pimennys, automatisointi aiemmin manuaalinen näkyvä-poistaminen haetaan tunnistetiedot-kirjautumisessa Azure hallinta-portaalin, ladataan paketti, jne. Esimerkki siitä, miten projektin käyttöön suoraan Azure-kohdassa [luomalla Hei maailma sovellus Pimennys Azure varten].
* **Azure työkalurivin:** Azure työkalurivin on nyt saatavilla Pimennys, jossa on painikkeita, jotka kutsuvat seuraavia ominaisuuksia:
    * ![][ic710879]**Suorita Azure emulaattorin**: käytetään projektin emulaattori.
    * ![][ic710880]**Palauttaa Azure emulaattorin**: Palauttaa emulaattorin.
    * ![][ic710881]**Azure Cloud paketin luominen**: kääntää paketin käyttöönottoa varten.
    * ![][ic710876]**Uuden Azure käyttöönottoprojektin**: Luo uuden käyttöönottoprojektin Azure.
    * ![][ic710882]**Julkaise Azure pilveen**: julkaisee projektin Azure.
    * ![][ic710883]**Poista julkaisu**: poistaa käyttöönoton.
    * Monia näistä Azure työkalurivin painikkeet ovat käytössä [luomalla Hei maailma sovellus Pimennys Azure varten].
* **Azure kirjastojen Java:** Nyt saatavana osana Java kirjastossa Pimennys Azure kirjastojen yksittäisen paketin mukana laajennuksen asennuksen ja joka sisältää kaikki tarvittavat riippuvuudet sekä. Lisää vain yhden viittauksen kirjaston Java projektin ja sinun ei tarvitse tehdä mitään ladata erikseen. Lisätietoja on artikkelissa [asentamista varten Pimennys Azure-Työkalut].
* **Microsoft JDBC ohjaimen 4.0 SQL Serverin käytettävissä laajennuksen asennuksen aikana:** Uuden laajennuksen asennuksen aikana Microsoft SQL Server JDBC ohjain uusin versio voidaan asentaa.
* **Azure Access ohjausobjektin palvelun suodattimen käytettävissä laajennuksen asennuksen aikana:** Uuden osan, sisältyvät Pimennys-kirjasto-työkalujen avulla Java web-sovelluksen saumattomasti hyödyntää Azure Access ohjausobjektin Service (ACS) todennus käyttämällä eri tunnistetietojen toimittajat, kuten Google-, Live.com- ja Yahoo!. Sinun ei tarvitse kirjoittaa todennus logiikan, vain määrittäminen muutamalla eri tavalla ja tee käyttöönottoon käyttäjät voivat kirjautua sisään ACS näkyvä-nosto suodattimen avulla. Voit vain keskittyä kirjoitat koodia, joka antaa käyttöoikeuden lisätoimenpiteisiin jäsenyyden perustuvan resurssien palauttamat sovelluksen suodattimen pyynnön objektin sisällä. Opetusohjelmaan käyttämisestä ACS suodatin- [todennusta ja Azure Access ohjausobjektin palvelun käyttämällä Pimennys käyttäjien]ohjeita.
* **Automaattinen tunnistaminen Azure SDK 1.7 edellytyksenä:** Kun luot uuden Azure käyttöönottoprojektin, Azure SDK 1.7 ladataan automaattisesti, jos se ei ole vielä asennettu.
* **Esiintymän päätepisteet:** Sallii suoran portin päätepisteen käytön viestintään kuormitus tasataan roolin esiintymät. Esiintymän päätepisteet voi lisätä päätepisteet Käyttöliittymä, käytettävissä on [päätepisteet ominaisuudet] -sivulla. Näin käyttöön remote virheenkorjaus ja JMX diagnostiikka tietyille Laske esiintymät tilanteissa kanssa usean pilveen käynnissä-käyttöönotoissa esiintymä. 
* **Käyttöliittymän osat:** On helppo kokeneille käyttäjille yksittäisiä Azure projektin roolit ja muut Ulkoiset resurssit Java sovelluksen hankkeita, kuten väliset Projektiriippuvuudet määrittäminen myös helpottaa niiden käyttöönoton logiikan kuvaus. Lisätietoja on artikkelissa [osien ominaisuuksia].
* **Projektien aiempien versioiden automaattinen päivitys:** Työtila, joka on Azure projektin laajennus aiemmalla versiolla luotu avattaessa vanha projektien näkyvät Pimennys sellaisena kuin se on suljettu, koska projektien aiemmat versiot eivät ole yhteensopivia uusien. Jos yrität avata jonkin näistä vanha projektien päivityksen ohjattu toiminto käynnistyy. Jos hyväksyt päivitys, uusi, projektin **_Upgraded** nimen, luodaan ja päivitetään automaattisesti uusien käyttöä varten. Voit muuttaa uuden projektin tarpeen mukaan. Osana päivityksen alkuperäisen projektin ei voi muokata (ja pysyy suljettu).

### <a name="december-10-2011"></a>10. joulukuu 2011

Azure laajennuksen Pimennys - joulukuu 2011 CTP on julkaissut. Uusia ominaisuuksia:

* **Istunnon affiniteetti (&quot;muistilappuja istunnot&quot;) tukevat:** Tukea, ota käyttöön tilallisen, liitetty Java-sovellukset, joilla on vain yksi valintaruutu. Lisätietoja on artikkelissa [Istunnon affiniteetti].
* **Tehty valmiiksi käynnistys-komentosarjamallit:** Suosituimpien Java palvelimia (Tomcat, jota, JBoss, GlassFish), jotka olet voi vain kopioi ja liitä projektin näytteiden hakemistosta se käynnistyskomentosarjan.
* **Emulaattorin käynnistys output reaaliajassa:** Voit nyt katsoa vaiheiden suorittamisen käynnistys-komentosarjan oma konsoli-ikkunassa, jossa virheet ja edistymistä komentosarjan, kun se suoritetaan Azure.
* **Automaattinen, himmennetty java.exe seuranta:** Kun java.exe suoritus keskeytyy, sisällytetään automaattisesti käyttöönoton lightweight, valmistetut komentosarjan avulla pakottaa roolin Roskakori.
* **Virheiden määritys Käyttöliittymän remote Java-sovellus:** Voit helposti mahdollistaa Pimennys 's remote virheenkorjaus käyttää käynnissä emulaattori tai Azure pilveen, jotta voit käydä läpi ja Java-koodin reaaliajassa virheenkorjaus Java sovelluksen. Lisätietoja on artikkelissa [Azure-sovellusten virheenkorjaus Pimennys].
* **Paikallisen tallennustilan resurssin kokoonpanon Käyttöliittymän:** Näin voit ei enää tarvitse määrittää paikalliset resurssit napsauttamalla käsittelevistä XML suoraan. Tämän ominaisuuden avulla voit käyttää paikallinen resurssi tehokkaita tiedostopolku, kun se on otettu käyttöön ympäristömuuttuja viitataan suoraan käynnistys-komentosarjojen kautta. Lisätietoja on artikkelissa [paikallisen säilön ominaisuudet].
* **Ympäristön muuttujan määritysten Käyttöliittymän:** Näin voit ei enää ole määrittämiseen ympäristömuuttujat kautta Manuaalinen muokkaus XML-määritys. Lisätietoja [ympäristön muuttujat ominaisuudet].
* **JDBC SQL Azure-ohjain:** Saa kautta laajennus on asennettu saumattomasti integroidun Pimennys kirjastoon, helpottaa ohjelmointi vastaan SQL Azure ottaminen käyttöön. 
* **Roolin määrittäminen Käyttöliittymän nopeasti pikavalikko pääsy**: vain rooli-kansiota hiiren kakkospainikkeella ja valitse **Ominaisuudet**.
* **Mukautettu Azure-projekti ja rooli kansiokuvaketta:** Paremman näkyvyyden ja helpommin siirtyminen työtilan ja projektin.

## <a name="see-also"></a>Katso myös ##

Lisätietoja Java IDEs Azure työkalupakettien on seuraavissa linkeissä:

- [Azure työkalujen Pimennys varten]
  - [Azure-työkalut asennetaan Pimennys]
  - [Luo Hei maailma verkkosovellukseen Pimennys Azure]
  - *Mitä uusia ominaisuuksia Azure työkalujen Pimennys (tämä artikkeli) varten*
- [Azure työkalujen IntelliJ varten]
  - [Azure-työkalut asennetaan IntelliJ]
  - [Luo Hei maailma verkkosovellukseen IntelliJ Azure]
  - [Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]

Saat lisätietoja Azure käyttäminen Java [Azure Java Developer Center].

<!-- URL List -->

[Azure työkalujen Pimennys varten]: ./azure-toolkit-for-eclipse.md
[Azure työkalujen IntelliJ varten]: ./azure-toolkit-for-intellij.md
[Luo Hei maailma verkkosovellukseen Pimennys Azure]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Luo Hei maailma verkkosovellukseen IntelliJ Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Azure-työkalut asennetaan Pimennys]: ./azure-toolkit-for-eclipse-installation.md
[Azure-työkalut asennetaan IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Mitä uusia ominaisuuksia Azure työkalujen IntelliJ varten]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[Zulu OpenJDK Azul järjestelmien web-sivu]: http://go.microsoft.com/fwlink/?LinkId=402457
[Azure Palvelupäätepisteet]: http://go.microsoft.com/fwlink/?LinkID=699526
[Azure-tallennustilan tilin luettelo]: http://go.microsoft.com/fwlink/?LinkID=699528
[Osien ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[Pimennys Azure Hei maailma-sovelluksen luominen]: http://go.microsoft.com/fwlink/?LinkID=699533
[Virheenkorjaus Pimennys Azure sovellukset]: http://go.microsoft.com/fwlink/?LinkID=699535
[Suurissa yrityksissä käyttöönotto]: http://go.microsoft.com/fwlink/?LinkID=699536
[Päätepisteet ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[Ympäristön muuttujat ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[HDInsight Työkalut ‑laajennuksen Pimennys]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[Miten verkko-käyttäjät, joilla on Azure Access Control-palvelu Pimennys tarkistamiseen]: http://go.microsoft.com/fwlink/?LinkID=264703
[Opi käyttämään SSL purkaminen]: http://go.microsoft.com/fwlink/?LinkID=699545
[Azure-työkalut asennetaan Pimennys]: http://go.microsoft.com/fwlink/?LinkId=699546
[Paikallinen tallennus-ominaisuudet]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Microsoft Azure asiakkaan Ohjelmointirajapinta]: http://go.microsoft.com/fwlink/?LinkId=280397
[Ominaisuudet: palvelimen asetukset]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Istunnon affiniteetti]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-purkaminen]: http://go.microsoft.com/fwlink/?LinkID=699549
[Luo uusi tallennustilan-tili]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Virtuaalikoneen ja Azure Cloud palvelun koot]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png
