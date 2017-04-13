<properties
    pageTitle="Aloita Azure haun Java | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Miten voit luoda Azure käyttäen Java oman ohjelmointikieli isännöityä cloud-haku-sovellus."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Java-haun Azure käytön aloittaminen
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Opettele Luo mukautettu Java Etsintäsovellus, joka käyttää Azure etsiminen sen hakukokemusta. Tässä opetusohjelmassa käyttää [Azure haun palvelun REST API](https://msdn.microsoft.com/library/dn798935.aspx) muodostaa objektit ja käyttää tässä toimintoja.

Jos haluat suorittaa tämän mallin on oltava Azure Search-palvelun, joka Kirjaudu käyttäjäksi [Azure-portaalissa](https://portal.azure.com). Katso vaiheittaiset ohjeet [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

On käytetty seuraavat ohjelmat voivat laatia ja testaa tässä esimerkissä:

- [Pimennys IDE Java ss kehittäjille](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Muista ladata ss-version. Vahvistus seuraavien vaiheittaisten ohjeiden edellyttää ominaisuus, joka löytyy vain tämä versio.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Tietoja

Esimerkkisovellus käyttää tietoja [Yhdysvallat geologiset Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), valitse-tilaan, Rhode saari tietojoukko koon pienentämiseksi suodatettujen. Microsoft search-sovellus, joka palauttaa maamerkki rakennuksia, kuten sairaaloita ja koulut sekä geologiset ominaisuuksia, kuten virtaa, järviä ja huippukokouksissa luominen näitä tietoja käytetään.

Tämän sovelluksen **SearchServlet.java** ohjelma muodostaa ja lataa indeksin avulla [indeksointitoiminto](https://msdn.microsoft.com/library/azure/dn798918.aspx) -rakennetta, suodatettu USGS tietojoukko noutaminen julkisen Azure SQL-tietokantaan. Ennalta määritetyt tunnistetiedot ja online-tietolähteen yhteystiedot on tarkoitettu ohjelmakoodi. Tietojen käyttö ei edellytä mitään muita määrityksiä.

> [AZURE.NOTE] Suodatin on käytössä tämän tietojoukko yhteydenpitoon vapaa hinnoittelu taso 10 000 asiakirjan raja-kohdassa. Jos käytät vakio taso, tämä rajoitus ei koske ja voit muokata käyttämään isommaksi tietojoukko koodi. Lisätietoja hinnoittelu kunkin tason kapasiteetti on artikkelissa [rajat ja rajoitukset](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Tietoja ohjelmatiedostot

Seuraavassa luettelossa kuvataan tiedostot, jotka liittyvät tässä esimerkissä.

- Search.jsp: Tarjoaa käyttöliittymä
- SearchServlet.java: Tarjoaa menetelmiä (muistuttaa MVC-ohjain)
- SearchServiceClient.java: Käsittelee HTTP-pyynnöt
- SearchServiceHelper.java: Helper luokka, joka sisältää staattiset menetelmät
- Document.Java: Sisältää tietomallin
- Config.Properties: määrittää Search-palvelun URL-osoite ja ohjelmointirajapinnan avain
- Pom.XML: Maven-testi riippuvuus

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Etsi palvelunimi ja Azure-hakupalvelun ohjelmointirajapinnan avain

Kaikki Azure haun REST API-kutsut edellyttävät, että annat palvelun URL-osoite ja ohjelmointirajapinnan avain. 

1. Kirjautuminen [Azure Portal](https://portal.azure.com).
2. Napsauta tekstin-palkin **Hakupalvelun** luettelon kaikista valmisteltu tilauksen Azure-Etsi-palveluita.
3. Valitse palvelu, jota haluat käyttää.
4. Palvelun koontinäytössä näet ruutuja varten tärkeitä tietoja ja käyttää järjestelmänvalvojan näppäimet Avainkuvake.

    ![][3]

5. Kopioi URL-osoite ja järjestelmänvalvoja-näppäintä. Tarvitset niitä myöhemmin, kun lisäät ne **config.properties** -tiedostoon.

## <a name="download-the-sample-files"></a>Tiedostojen lataaminen

1. Siirry [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) Github.

2. Valitse **Lataa ZIP-**, Tallenna .zip-tiedosto levylle ja Pura se sisältää kaikki tiedostot. Harkitse tiedostojen puretaan tiedostojasi on helpompi löytää projektin myöhemmin Java työtilaasi.

3. Esimerkkitiedostot ovat vain luku-tilassa. Napsauta kansion ominaisuudet ja Poista vain luku-määrite.

Kaikki myöhemmin tiedoston muutoksia ja suorita lauseet tehdään vastaan tämän kansion tiedostot.  

## <a name="import-project"></a>Projektin tuominen

1. Valitse Pimennys, **Tiedosto** > **Tuo** > **Yleiset** > **Aiemmin projektien työtilaan**.

    ![][4]

2. **Valitse pääkansioon**Selaa kansioon, joka sisältää mallitiedostojen. Valitse kansio, joka sisältää .project-kansio. Projektin pitäisi näkyä **projektien** luettelosta valitun kohteen.

    ![][12]

3. Valitse **Valmis**.

4. **Project Explorer** avulla voit tarkastella ja muokata tiedostoja. Jos jo ei ole avoinna, valitse **ikkunan** > **Näytä** > **Project Explorer** tai käytä pikakuvaketta, avaa se.

## <a name="configure-the-service-url-and-api-key"></a>Palvelun URL-osoite ja -ohjelmointirajapinnan avain määrittäminen

1. Kaksoisnapsauta **Project Explorer** **config.properties** Muokkaa määritysasetukset, joka sisältää palvelimen nimi ja ohjelmointirajapinnan avain.

2. Lisätietoja on tämän artikkelin löytänyt missä [Azure-portaalissa](https://portal.azure.com), hakee arvot nyt kirjoittaa **config.properties**palvelun URL-osoite ja ohjelmointirajapinnan avain vaiheet.

3. Korvaa-ohjelmointirajapinnan avain" **config.properties**-palvelun api-näppäintä. Seuraavaksi palvelunimi (URL-http://servicename.search.windows.net ensimmäinen osa) korvaa "palvelunimi" samaan tiedostoon.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Määritä projektin, muodosta ja runtime-Ympäristöt

1. Napsauta hiiren kakkospainikkeella Pimennys Project Explorer-Projekti > **Ominaisuudet** > **Projektin rakenteita**.

2. Valitse **Dynaaminen WWW-moduulin**ja **JavaScript** **Java**.

    ![][6]

3. Valitse **Käytä**.

4. Valitse **ikkuna** > **asetukset** > **palvelimen** > **Runtime ympäristöissä** > **Lisää.**.

5. Laajenna Apache ja valitse aiemmin asennettu Apache Tomcat server-version. -Järjestelmään on asennettu versio 8.

    ![][7]

6. Valitse seuraavalla sivulla Määritä Tomcat asennus-hakemisto. Windows-tietokoneessa tämä on todennäköisesti C:\Program Files\Apache ohjelmiston Foundation\Tomcat *versio*.

6. Valitse **Valmis**.

7. Valitse **ikkuna** > **asetukset** > **Java** > **asennettu JREs** > **Lisää**.

8. Valitse **Lisää JRE** **Vakio AM**.

10. Valitse **Seuraava**.

11. Valitse JRE määritelmää JRE koti- **kansio**.

12. Siirry **Program Files** > **Java** ja valitse JDK asensit aiemmin. On tärkeää valita JDK JRE.

13. Valitse asennettu JREs **JDK**. Asetusten pitäisi muistuttaa seuraavista näyttökuvan.

    ![][9]

14. Voit myös valita **ikkunan** > **Selaimen** > **Internet Explorer** Avaa sovelluksen ulkoisen selainikkunassa. Ulkoinen selaimessa avulla voit tehostaa Web-sovelluksen käyttökokemusta.

    ![][8]

Määritystehtävät on nyt valmis. Seuraavaksi muodosta ja Suorita projekti.

## <a name="build-the-project"></a>Projektin luominen

1. Valitse Project Explorer projektin nimeä hiiren kakkospainikkeella ja valitse **Suorita nimellä** > määrittäminen projektin**maven-testi luominen...** .

    ![][10]

8. Muokkaa määrityksessä-tavoitteet Kirjoita "puhtaasti asentaa" ja valitse sitten **Suorita**.

Tilasanomat tulostetaan console-ikkunaan. Raportissa pitäisi näkyä ilmaisee, että projektin sisäisten virheettömät luominen onnistui.

## <a name="run-the-app"></a>Suorita sovellus

Tässä viimeisessä vaiheessa sovelluksen suorittamiseen paikallisen palvelimen runtime-ympäristössä.

Jos et ole vielä määrittänyt server runtime-ympäristössä Pimennys, tarvitset tekemään se ensin.

1. Laajenna **WebContent**Project Explorer.

5. Napsauta hiiren kakkospainikkeella **Search.jsp** > **suorittaa** > **suoritetaan palvelimessa**. Valitse Apache Tomcat-palvelin ja valitse sitten **Suorita**.

> [AZURE.TIP] Jos muun kuin oletusarvoisen työtilan avulla voit tallentaa projektin, ne on muokattava siten, että projektin sijainti, johon haluat välttää pysäyttämisestä palvelinvirhe **Suorita määritys** . Napsauta hiiren kakkospainikkeella Project Explorer **Search.jsp** > **Suorita nimellä** > **Suorittaa määrityksiä**. Valitse Apache Tomcat palvelin. Valitse **argumentit**. Valitse Määritä kansio, jossa on projektin **työtilan** tai **Tiedostojärjestelmässä** .

Kun käynnistät sovelluksen, selainikkunassa, pitäisi näkyä antamisen hakukenttä työnkulun ehdot.

Odota noin minuutin, ennen kuin valitset **haun** avulla voit luoda ja ladata indeksiä service-aika. Saat HTTP 404-virhesanoma, jos haluat vain odottamaan hieman enää ennen kuin yrität uudelleen.

## <a name="search-on-usgs-data"></a>Etsi USGS tietoihin

USGS tietojoukon sisältää tietueet, jotka liittyvät, tila on Rhode saari. Jos valitset **Etsi** tyhjä hakuruutuun, näet yläreunan 50 merkinnät-mikä on oletusarvo.

Kirjoittamalla hakusana avulla hakukonetta jotain Siirry. Kirjoita alueellisen nimi. "Pekka Williams" on Rhode saari ensimmäisen säädin. Monia puistojen, rakennuksia ja koulut ovat nimeltään hänelle jälkeen.

![][11]

Voi myös kokeilla jotakin näistä ehdoista:

- Pawtucket
- Pembroke
- hanhi + Kap

## <a name="next-steps"></a>Seuraavat vaiheet

Tämä on ensimmäinen Azure haun opetusohjelman Java ja USGS tietojoukko perusteella. Ajan myötä on laajentaa Tässä opetusohjelmassa osoittamaan mukautettujen ratkaisujen saatat haluta käyttää muita hakuominaisuudet.

Jos sinulla on jo joitakin taustan Azure haun, voit tässä esimerkissä springboard kuin kokeissa, esimerkiksi augmenting [haun sivulle](search-pagination-page-layout.md)tai käyttöönoton [kohdistettua siirtymistä varten](search-faceted-navigation.md). Voit parantaa yhteydessä hakutulossivulla lisäämällä määrät ja jonottaminen asiakirjoja, jotta käyttäjät voivat selata tulokset.

Uusi Azure haun? On suositeltavaa kokeilujakson muiden opetusohjelmat kehittää ymmärtämisen voit luoda. Lisätietoja saat lisätietoja Microsoftin [asiakirjat-sivu](https://azure.microsoft.com/documentation/services/search/) . Voit tarkastella linkkejä myös sekä [videon ja sovelluksen luettelon](search-video-demo-tutorial-list.md) enemmän tietoja.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
