<properties
    pageTitle="Pikaopas: tietokoneen Learning suosituksia API | Microsoft Azure"
    description="Azure Konepohjaisten Oppimistekniikoiden suositukset - pikaopas"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Kognitiiviset Services suositukset-Ohjelmointirajapinnan pika-aloitusopas

Tässä asiakirjassa kerrotaan kuinka, määrän palvelun tai sovelluksen käyttämään [Suosituksia API](http://go.microsoft.com/fwlink/?LinkId=759710).
Voit etsiä lisätietoja suositukset-Ohjelmointirajapinnan ja kognitiiviset muiden [tähän](http://go.microsoft.com/fwlink/?LinkId=759709). Tässä oppaassa kannattaa myös tutustua [Suosituksia API viittaus](http://go.microsoft.com/fwlink/?LinkId=759348) kätevää.


<a name="Overview"></a>
## <a name="general-overview"></a>Yleisiä tietoja

Tämä asiakirja on vaiheittaiset ohjeet. Microsoftin tavoitteena on opastusta kouluttaminen malli ja osoita resursseja, jotta voit tarjoaman tuotannon-ympäristöstä mallin tarvittavat vaiheet.

Tämä Harjoitus kestää noin 30 minuuttia.

Käyttämään [Suositukset-Ohjelmointirajapinnan](http://go.microsoft.com/fwlink/?LinkId=759710)on seuraavasti:

1. Luo malli - malli on säilö käyttötietojen, luettelotiedot ja suositus-malli.
1. Luettelon tietojen - luettelo sisältää metatietojen tietoja.
1. Tuo käyttötiedot - käyttötietoja voi ladata 2 tavoilla (tai molemmat):
  -  Lataamalla käyttötietojen sisältävän tiedoston.
  -  Lähettäminen tietojen hankkiminen tapahtumat.
  Yleensä käyttö tiedoston lataaminen, jos haluat, että voit luoda ensimmäisen suositus-malli (käynnistyksen) ja käyttää sitä, kunnes järjestelmä kerää riittävästi tietoja käyttämällä hankintaa tietomuoto.
1. Suositus mallin luominen – Tämä on asynkroninen toiminto, jossa suositus järjestelmä hakee käyttötiedot ja luo suositus malli. Tämä toiminto voi kestää useita minuutteja tai useita tunteja tietoja ja muodosta määritysten parametrit koon mukaan. Kun käynnistävä Luo saavat on muodosta. Sen avulla voit tarkistaa, milloin muodosta prosessi on päättynyt ennen kuin aloitat tarjoaman suosituksia.
1. Kuluttavat suositukset - Get suosituksia tietyn kohteen tai kohteiden luettelo.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Aloitetaan.

Voit aloittaa rakentamiseen suosituksia. Valitse Microsoft auttaa sinua käyttämisestä power suosituksia e commerce sivuston mallin tuloksista.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Tehtävä 1 - allekirjoitetun sinua suositukset-Ohjelmointirajapinnan käyttäjäksi ####

Tässä tehtävässä rekisteröityä suosituksia API-palveluun ja suositukset-mallin luominen.

1. Siirry **Azure-portaalissa** osoitteessa [http://portal.azure.com/](http://portal.azure.com/) ja kirjaudu sisään Azure-tili.

1.  Valitse **+ Uusi**.

1. Valitse **Liiketoimintatieto** -vaihtoehto.

1. Valitse **Kognitiiviset Services API** -tuote.
Tämän tuotteen avulla voit aloittaa tilauksen kognitiiviset palveluja API (hymiö, tekstin Analytics, tietokoneen näkö jne.). Tänään keskitytään suositukset-Ohjelmointirajapinnan.

1. Kirjoita **tilin nimi** kognitiiviset Services API-aloitussivu suositukset-tilauksen. (Saat instace: "MyRecommendations"). Tämä nimi ei saa olla välilyöntejä sitä.

1. Valitse **API tyyppi**, **suositukset**.

1. Valitse **hinnoittelu taso**-suunnitelma. Voit valita 10 000 tapahtumat/kuukauden **vapaa** -taso. Tämä on vapaa suunnitelma, joten se on hyvä tapa aloittaa kokeilujakson järjestelmä. Kun siirryt tuotannon, on suositeltavaa harkita pyynnön äänenvoimakkuutta ja suunnitelman tyyppi muuttuvat vastaavasti. (Huomautus: voi olla vain yksi kerrallaan vapaa taso tilaus)

1. Valitse **Resurssiryhmä**tai luoda uuden, jos sitä ei vielä ole.

1. Voit muuttaa muiden osien tasaamiseen Luo-valintaikkunan. On osoitettava että resurssin tarjoajaan tänään on tuettu vain Yhdysvaltojen tiedot keskikohdan mukaan.

1. Kun olet tehnyt valinnat, valitse **Luo**.

1. Odota muutama minuutti resurssin käyttöön.
Kun se on otettu käyttöön, voit siirtyä **näppäimet** -osassa- **asetukset** -sivu, jossa annettu ensisijaisen ja toissijaisen avaimen Ohjelmointirajapinnan käyttäminen.  Kopioi perusavaimen, sillä sinun on sen ensimmäinen malli luotaessa.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Tehtävä 2 - asentaminen liittävät tiedot? ####

Suositukset-Ohjelmointirajapinnan tuntea luettelon ja tapahtumien on hyvä tuotteen suosituksia. Tämä tarkoittaa sitä, sinun täytyy syötteen hyvä tiedot (Microsoft kutsua tämän **luettelon** tiedoston) tuotteiden ja joukko tapahtumat riittävän suuri, jotta se voi löytää kiinnostavia kuviot kulutus (olemme kutsua tämän **käyttö**).

1. Yleensä kyselyn tapahtumat tietokannan nämä tiedot kappaletta.
Siltä varalta, sinulla ei ole ne käteviä, on annettu mallitietoja Microsoft Storeen tapahtumatietojen perusteella.

 Voit ladata tiedot [täältä](http://aka.ms/RecoSampleData). Kopioi ja purkaminen MsStoreData.Zip paikallisessa tietokoneessa kansioon.

 > **Huomautus:** Esimerkki-tunnus, jolla haluat ladata ja suorita tehtävä 3 on jo upotettu sisällä--, joten tämä onkin vapaaehtoinen vaihe mallitiedot.  Toisaalta, tämän tehtävän ansiosta voit ladata realistisesta tietojoukot ja Salli ymmärtää syötteiden paremmin suositukset-Ohjelmointirajapinnan kyselyjä.

1.  Nyt voit tarkastella luetteloa-tiedostosta. Siirry sijaintiin, johon kopioit tiedot.
 Avaa luettelotiedosto **Muistiossa**.

 Huomaat, että luettelo on melko helppoa. Seuraava kaava on`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32/64 E34 Online DwnLd, Office <br>
 > AAA-04303, OfficeLangPack 2013 32/64 ET Online DwnLd, Office  <br>
 > C9F 00168, KRUSELL Kiruna Käännä kansi for Nokia Lumia 635 - kamelin, Apuohjelmat

 On osoitettava, että luettelotiedosto voi olla paljon monipuolisempi, esimerkiksi voit lisätä metatietojen tuotteista (olemme Soita nämä *kohteen ominaisuudet*). Lisätietoja API-viittaus [luettelon muoto](http://go.microsoft.com/fwlink/?LinkID=760716) -kohdassa pitäisi näkyä luettelon muotoilu.

1. Oletetaan, että Tee sama käyttötietojen kanssa. Huomaat, että käyttö päivämäärä on muodon `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, osta 0003BFFDD4C2148C 297833400, 2015/08/04T11:02:50, osta 0003BFFDD4C2118D 297833300, 2015/08/04T11:02:40, osta 00030000D16C4237 297833300, 2015/08/04T11:02:37, osta 0003BFFDD4C20B63 297833400, 2015/08/04T11:02:12, osta 00037FFEC8567FB8 297833400, 2015/08/04T11:02:04 hankkiminen

Huomaa, että ensin kolmesta elementistä on pakollinen. Tapahtumatyyppi on valinnainen. Voit tarkistaa [käyttö Muotoile](http://go.microsoft.com/fwlink/?LinkID=760712) Lisätietoja aiheesta.

 > **Kuinka paljon tietoja on?**
 <p>
Todella riippuu itse käyttötiedot. Järjestelmä oppii, kun käyttäjä ostaa eri kohteille. Jotkin versiot, kuten muuttaa FBT-Maksuerien on tärkeää tietää, mitkä kohteet ostetaan samat tapahtumat. (Emme Soita tämän *muiden esiintymien*). Hyvä peruslähtökohta on useimpien kohteiden 20 tapahtumia tai enemmän, joten Suosittelemme, että sinulla on vähintään 20 kertaa määrä tapahtumien tai noin 200,000 tapahtumat Jos haluat käyttää luettelon 10 000 kohdetta,.
Uudelleen Tämä on hyvä peruslähtökohta. Haluat kokeilla tiedot.
</p>

<a name="Ex1Task3"></a>
####Tehtävä 3 - suositukset-mallin luominen####

Nyt kun olet määrittänyt tilin ja tiedot, Luo ensimmäinen malli.

Tässä tehtävässä ensimmäisen mallin luominen malli-sovelluksen avulla.

1. Ensin sinun tulee tietää [Suosituksia API viittaus](http://go.microsoft.com/fwlink/?LinkId=759348).

1. Lataa [Sovelluksen malli](http://go.microsoft.com/fwlink/?LinkID=759344) paikalliseen kansioon.

1. Avaa Visual Studiossa **RecommendationsSample.sln** -ratkaisun **C#** -kansiossa.

1. Avaa **SampleApp.cs** -tiedosto. Huomaa, että seuraavat vaiheet tiedosto:
 + Mallin luominen
 + Luettelotiedoston lisääminen
 + Käyttö-tiedoston lisääminen
 + Käynnistimen mallin luominen
 + Hae kohteita kahdet perusteella suositus
<p></p>

1. Korvaa **AccountKey** -kentän arvo tehtävän 1-näppäintä.

1. Vaihe ratkaisussa, ja näet, kuinka luodaan mallin.

1. Kokeile lataamaasi vain uuden mallin luominen Microsoft Storeen tai kirjan suosituksia luettelon ja käyttö tiedostojen korvaaminen. Tarvitset sekä mallinimi ja, jonka pyydät suosituksia kohteet.

1. Mallin luomisen jälkeen Ota seuraavat seikat huomioon **Mallitunnus** , kun tarvitset sitä kun pyydetään suosituksia tuotantoympäristössä.

>  Lue lisätietoja muodosta tyypit ja niiden laskeminen versiot laadun [tähän](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Laajennettujen mallin tuotannon! ###

Nyt kun osaat mallin luominen ja tarjoaman suositukset, seuraava vaihe on sijoitetaan sivuston-mobiilisovelluksen tuotannon tai integroida CRM tai ERP-järjestelmässä.
Selvästi kukin näistä olisi eri. Koska suositukset-Ohjelmointirajapinnan on pyydetty web-palvelu, sinun pitäisi Soita jossakin näistä eri ympäristöissä helposti.

**Tärkeä:**  Jos haluat näyttää suosituksia julkisen asiakaskoneesta (esimerkiksi sähköiseen kaupankäyntiin sivuston), sinun on luotava antamaan suositukset välityspalvelinta. Tämä on tärkeää, jotta Ohjelmointirajapinnan avain ei näy ulkoisessa (mahdollisesti luotettu).

Seuraavassa on muutamia ehdotuksia sijainneista, jossa käytät suositukset:

### <a name="product-page"></a>Product-sivu

**Kohteen suositukset**
<p>Jos malli on koulutus hankkiminen tietoihin, saat *tietää, jotka todennäköisesti kiinnostavat henkilöille, jotka on ostettu lähde tuotteiden*asiakkaalle on sallittua.</p>
<p>Jos malli on koulutus Valitse tiedot, se sallii asiakkaalle saat *tietää, tuotteiden, jotka todennäköisesti avoimelle henkilöt, jotka olet käynyt lähde*. Tämän mallin tyypin voi palauttaa samankaltaiset kohteet.</p>

**Usein ostanut yhdessä suositukset**
<p>Usein ostanut yhdessä muodosta A voi olla koulutus, jotta pääset kohdejoukkojen todennäköisesti ovat maksullisia yhdessä kohdetta.</p>

### <a name="check-out-page"></a>Tutustu sivu

**Kohteen suositukset**
<p>Suositukset, mallin voi toteuttaa syötteen kohteiden luettelo. Siirtää kaikki kohteet niin koriin syötteeksi Nouda suositukset.
Tässä tapauksessa mallin antaa suositukset, jotka kiinnostavat annettu koriin kaikki kohteet.
</p>

### <a name="landing-page"></a>Aloitussivu

**Käyttäjän suositukset**
<p>
Suositukset, voit ottaa mallin syötteenä käyttäjätunnus.  Tämä mukautettuja suosituksia antaa käyttäjän määrittämä käyttämällä tapahtumat, joita käyttäjä historia.
</p>

Tutustu [Hae kohteen suosituksia ohjeissa](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Mitä seuraavaksi?
Onnittelut, jos olet tehnyt sitä tämä reunassa! Saat enemmän pääset käsiksi [Suosituksia API viittaus](http://go.microsoft.com/fwlink/?LinkId=759348) , jos sinulla on kysymyksiä etsimääsi, ota meihin yhteyttä-palvelussamlapi@microsoft.com
