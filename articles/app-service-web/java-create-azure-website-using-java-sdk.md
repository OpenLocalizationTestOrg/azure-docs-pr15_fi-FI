<properties 
    pageTitle="Web-sovelluksen luominen Azure App palvelu Azure SDK Java" 
    description="Opettele luomaan verkkosovellukseen Azure App palvelun käyttäminen ohjelmallisesti Java Azure SDK-paketissa." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Web-sovelluksen luominen Azure App palvelu Azure SDK Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Yleiskatsaus

Näiden vaiheiden avulla voit luoda Azure-SDK Java-sovelluksen, joka luo verkkosovellukseen [Azure App palvelun][]sitten siihen-sovelluksen ottaminen käyttöön. Se koostuu kahdesta osasta:

- Osa 1 esitellään, miten voit luoda Java-sovelluksella, joka luo verkkosovellukseen.
- Osa 2 kerrotaan, miten voit luoda yksinkertaisen JSP "Hei-maailman" sovelluksen sitten Käytä FTP-asiakasohjelman käyttöön koodi sovelluksen-palveluun.


## <a name="prerequisites"></a>Edellytykset

### <a name="software-installations"></a>Ohjelmiston asentaminen

Tämän artikkelin AzureWebDemo sovelluksen-koodi on kirjoitettu Azure Java SDK 0.7.0, josta voit asentaa [WWW-ympäristö asennusohjelma][] (WebPI) avulla. Varmista lisäksi, että käyttämään [Azure työkalujen varten Pimennys][]uusimman version. Kun olet asentanut SDK, Päivitä Pimennys projektin riippuvuudet suorittamalla **Päivitä hakemisto** **Maven-testi**säilöjen tietoihin ja valitse Lisää pakkauksesta **riippuvuudet** -ikkunassa uusimman version. Voit tarkistaa asennettu ohjelmistosi Pimennys versio valitsemalla **Ohje > asennustiedot**; on oltava vähintään seuraavia versioita:

- Microsoft Azure-kirjastoissa Java 0.7.0.20150309 pakkaaminen
- Pimennys IDE Java ss kehittäjille 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Luominen ja määrittäminen Azure Cloud resurssit

Ennen kuin aloitat, sinun täytyy on aktiivinen Azure-tilaus ja oletusarvojen Active Directory (AD) Azure.


### <a name="create-an-active-directory-ad-in-azure"></a>Azure Active Directory (AD) luominen

Jos sinulla ei ole vielä Active Directory (AD) Azure-tilauksen, kirjaudu sisään Microsoft-tilillä [Azure perinteinen portal][] . Jos sinulla on useita tilauksia, valitse **tilaukset** ja valitse oletuskansion, jota haluat käyttää projektin tilauksen. Valitse **Käytä** tilauksen, että näkymä.

1. Valitse **Active Directory** -valikosta vasemmalla. **Valitsemalla Uusi > kansio > Luo mukautettu**.

2. **Lisää kansio**Valitse **Luo uusi kansio**.

3. Kirjoita **nimi**-kenttään kansionimi.

4. **Toimialueen**Kirjoita toimialuenimi. Tämä on basic toimialuenimi, joka sisältyy oletusarvoisesti hakemistossa; se on lomakkeen `<domain_name>.onmicrosoft.com`. Voit nimetä sen kansionimi tai toinen verkkotunnus, jotka omistat perusteella. Voit lisätä myöhemmin toinen verkkotunnus jo organisaation käyttämät.

5. **Maa tai alue**Valitse kieli.

Lisätietoja AD on artikkelissa [Mikä on Azure AD-kansio][]?


### <a name="create-a-management-certificate-for-azure"></a>Hallinta-varmenteen luominen Azure varten

Java Azure-SDK käyttää hallinta sertifikaatteja todentamismenetelmä Azure tilaukset. Nämä varmenteet X.509 v3 todennetaan asiakassovellus, joka käyttää Service Management API tilauksen omistajan tilauksen resurssien avulla.

Tässä toimintosarjassa koodi käyttää itse allekirjoitettua varmennetta Azure todentamismenetelmä. Nämä toimet sinun on varmenteen luominen ja lataa se [Azure perinteinen portal][] etukäteen. Etenee vaiheittain seuraavasti:

- Luo PFX-tiedosto, joka edustaa asiakkaan varmenne ja tallenna se paikallisesti.
- Luo hallinta-varmenteen (CER-tiedosto) PFX-tiedostosta.
- CER-tiedoston lataaminen Azure tilauksen.
- PFX-tiedoston muuntaminen JKS, koska Java käyttää muodossa todennuksen varmenteet.
- Kirjoita sovelluksen todennuskoodi, joka viittaa JKS paikallinen tiedosto.

Kun olet tehnyt nämä vaiheet, CER varmenteen liittyville Azure-tilaukseesi ja JKS varmenne sijaitsevat paikalliseen asemaan. Lisätietoja hallinta varmenteet-kohdassa [Luo ja lataa Azure hallinta varmennetta][].


#### <a name="create-a-certificate"></a>Varmenteen luominen

Jos haluat luoda oman itse allekirjoitetun varmenteen, Avaa Komentokonsolin käyttöjärjestelmän ja suorittamalla seuraavat komennot.

> **Huomautus:**  Tietokone, jossa suoritat tämän komennon on oltava asennettuna JDK. Lisäksi keytool polku määräytyy sijainti, johon olet asentanut JDK. Lisätietoja on artikkelissa [avain ja varmenteen hallintatyökalun (keytool)][] Java online-tiedostoissa.

Voit luoda .pfx-tiedosto:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

.Cer-tiedoston luominen

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Jos:

- `<java-install-dir>`on kansio, johon asensit Java polku.
- `<keystore-id>`keystore tapahtuma-tunniste (esimerkiksi `AzureRemoteAccess`).
- `<cert-store-dir>`kansio, johon haluat tallentaa varmenteet polku (esimerkiksi `C:/Certificates`).
- `<cert-file-name>`sertifikaattitiedosto nimi (esimerkiksi `AzureWebDemoCert`).
- `<password>`Varmenteen; suojaat salasana se on oltava vähintään 6 merkkiä. Voit kirjoittaa ilman salasanaa, vaikka se ei ole suositeltavaa.
- `<dname>`on liitettävä alias X.500 DN-nimi ja käytetään itse allekirjoitetun varmenteen myöntäjä ja aihe kentät.

Lisätietoja on artikkelissa [Luo ja lataa Azure hallinta varmennetta][].


#### <a name="upload-the-certificate"></a>Lataa sertifikaatti

Voit ladata itse allekirjoitetun varmenteen Azure- **perinteinen portaalin asetussivulla** ja valitse sitten **Hallinta varmenteet** -välilehti. Valitse sivun alareunassa **lataaminen** ja siirry luomasi CER-tiedoston sijainti.


#### <a name="convert-the-pfx-file-into-jks"></a>PFX-tiedoston muuntaminen JKS

Valitse Windows komentokehote (käynnissä järjestelmänvalvojaksi), cd-kansion sisältävä varmenteet ja suorittamalla seuraavan komennon kohtaa, johon `<java-install-dir>` on kansio, johon asensit Java tietokoneeseen:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Kirjoita pyydettäessä kohde keystore salasanaa. Tämä on JKS tiedostolle salasana.

2. Kirjoita pyydettäessä lähteen keystore salasanaa. Tämä on määritetty PFX tiedostolle salasana.

Kaksi salasanaa ei tarvitse olla sama. Voit kirjoittaa ilman salasanaa, vaikka se ei ole suositeltavaa.


## <a name="build-a-web-app-creation-application"></a>Web-sovelluksen luominen-sovelluksen luominen

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Pimennys työtilan ja maven-testi projektin luominen

Tässä osassa voit luoda työtilan ja maven-testi-project web app luonti-sovelluksen, nimeltä AzureWebDemo.

1. Luo uusi projekti maven-testi. Valitse **Tiedosto > Uusi > maven-testi projektin**. Valitse **Uuden maven-testi projektin** **yksinkertaisen projektin luominen** ja **käyttäminen työtilan oletussijainti**.

2. Määritä **Uuden maven-testi projektin**toisella sivulla seuraavat tiedot:

    - Ryhmätunnus:`com.<username>.azure.webdemo`
    - Palvelutietojen ID: AzureWebDemo
    - Versio: 0.0.1-SNAPSHOT
    - Pakkaamista: purkki
    - Nimi: AzureWebDemo

    Valitse **Valmis**.

3. Avaa uusi projekti pom.xml tiedosto Project Explorer. Valitse **riippuvuudet** -välilehti. Tämä on uusi projekti, ei ole pakettien näkyvät vielä.

4. Avaa maven-testi säilöjen tietoihin. **Napsauttamalla ikkunan > Näytä > muut > maven-testi > maven-testi säilöjen tietoihin** ja valitse **OK**. **Maven-testi säilöjen tietoihin** -näkymässä näkyvät IDE alareunassa.

5. Avaa **Yleiset säilöjen tietoihin**, **keskus** hiiren kakkospainikkeella ja valitse **Muodosta uudelleen indeksi**.

    ![][1]
    
    Tämä vaihe voi kestää useita minuutteja yhteyden nopeudesta riippuen. Kun indeksi muodostaa uudelleen, näkyy **maven-testi keskus** Microsoft Azure-paketit.

6. **Riippuvuudet**Valitse **Lisää**. Kirjoita **Enter ryhmän tunnus...** `azure-management`. Valitse Perus hallinta ja sovelluksen palvelun Web Apps-sovellusten hallinta:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Huomautus:** Jos päivität uuteen versioon julkaisemisen jälkeen riippuvuudet, tarvitset lisää kunkin riippuvuudet tässä luettelossa.
    > Kun valitse **Lisää** ja valitse kukin riippuvuuden, se näkyy uuden version numeron **riippuvuuksien** luettelossa.

Valitse **OK**. Azure pakettien näkyviin **riippuvuuksien** luettelossa.


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Web-sovelluksen luominen Azure SDK soittamalla Java-koodin kirjoittaminen

Kirjoita seuraavaksi tunnus, jolla tallenteita ohjelmointirajapinnan Azure SDK Java App Service-verkkosovelluksen luomiseen.

1. Java-luokan sisältämään Päähakusana piste-koodin luominen Valitse Project Explorer projektin solmun hiiren kakkospainikkeella ja valitse **Uusi > luokan**.

2. **Uusi Java-luokan**nimeä luokan `WebCreator` ja **julkinen staattinen void main** -valintaruutu. Valinnat tulisi olla seuraavanlainen:

    ![][2]

3. Valitse **Valmis**. WebCreator.java tiedosto näkyy Project Explorer.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Soittavan Azure Ohjelmointirajapinnan App Service-Web-sovelluksen luominen


#### <a name="add-necessary-imports"></a>Lisää tarvittavat tuonti

WebCreator.java Lisää seuraavat tuonnin; tuonnin tiettyjen luokkien hallinta kirjastojen käyttäjäprofiilipalvelun Azure-ohjelmointirajapinnan käyttö:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>Määritä Päähakusana-kohdassa luokka

Koska AzureWebDemo-sovellus on sovelluksen palvelun Web-sovelluksen luominen, nimeäminen tärkeimmät luokan tämän sovelluksen `WebAppCreator`. Tämä luokka on Päähakusana-kohdan koodi, joka kutsuu Azure Service Management Ohjelmointirajapinnan web-sovelluksen luominen.

Lisää seuraavat parametrin web app- ja webspace määritykset. Tarvitset oman Azure tilauksen tunnus ja varmenteen tietoja.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Jos:

- `<subscription-id>`on Azure Tilaustunnus, johon haluat luoda resurssi.
- `<certificate-store-path>`on polku ja tiedostonimi paikallisen varmenteen kaupan hakemistossa JKS-tiedostoon. Esimerkiksi `C:/Certificates/CertificateName.jks` Linux varten ja `C:\Certificates\CertificateName.jks` for Windowsissa.
- `<certificate-password>`on määritetty luodessasi JKS sertifikaatin salasana.
- `webAppName`voi olla mikä tahansa nimi, valitse; Tässä menetelmässä nimi `WebDemoWebApp`. Täydellinen toimialuenimi on `webAppName` kanssa `domainName` perään, joten tässä tapauksessa koko toimialue on `webdemowebapp.azurewebsites.net`.
- `domainName`on määritettävä yllä esitetyllä tavalla.
- `webSpaceName`on oltava [WebSpaceNames][] luokan määritetyt arvot.
- `appServicePlanName`on määritettävä yllä esitetyllä tavalla.

> **Huomautus:** Aina, kun tämän sovelluksen käyttämiseen, sinun on muutettava arvo `webAppName` ja `appServicePlanName` (tai web-sovelluksen Azure-portaalissa) ennen kuin suoritat sovellus uudelleen. Muussa tapauksessa suorittamisen epäonnistuu, koska sama resurssi on jo Azure.


#### <a name="define-the-web-creation-method"></a>Määritä web luominen

Määritä seuraavaksi menetelmän web-sovelluksen luominen. Tämä menetelmä `createWebApp`-web-sovelluksen ja webspace parametrit. Luo myös sekä määrittää sovelluksen palvelun Web Apps-sovellusten hallinta-asiakasohjelma, joka on määritetty [WebSiteManagementClient][] -objekti. Management-asiakas on luominen Web Apps-näppäintä. Se sisältää RESTful verkkopalvelut, joiden avulla voit hallita soittamalla service management API web Apps-sovellusten (suorittamiseen toimintoja, kuten luominen-tilaan, Päivitä ja poista) sovellukset.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

Koodin siirtää onnistumisesta tai epäonnistumisesta vastauksen HTTP-tila ja, jos onnistunut tulosteen luotu web-sovelluksen nimi.


#### <a name="define-the-main-method"></a>Määritä main()

Anna main() menetelmä-koodi, joka kutsuu createWebApp() web-sovelluksen luominen.

Lopuksi Soita `createWebApp` - `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Suorita sovellus ja tarkista web-sovelluksen luominen

Varmista, että sovelluksesi toimii, valitsemalla **Suorita > Suorita**. Kun sovellus on käynnissä valmis, pitäisi näkyä seuraava tulos Pimennys konsolissa:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Kirjaudu sisään Azure perinteinen portaalin ja sitten **Verkkosovelluksissa**. Uusi web-sovelluksen pitäisi näkyä Web Apps-sovellusten luettelosta muutaman minuutin kuluessa.


## <a name="deploying-an-application-to-the-web-app"></a>Web App sovelluksen käyttöönotto

Olet suorittaa AzureWebDemo ja luoda uuden online-perinteinen portaalin Kirjaudu, valitse **Web Apps -sovellusten**ja valitse **WebDemoWebApp** **Web Apps** -luettelosta. Valitse web Appissa dashboard-sivulla **Selaa** (tai valitse URL-Osoitteen `webdemowebapp.azurewebsites.net`) siirtyminen. Näkyviin tulee tyhjä paikkamerkki-sivulla, koska sisältöä ei ole web App-sovellukseen ole vielä julkaistu.

Seuraavaksi luodaan "Hei-maailman"-sovellus ja käyttöönotto web App-sovellukseen.


### <a name="create-a-jsp-hello-world-application"></a>JSP Hei maailma-sovelluksen luominen

#### <a name="create-the-application"></a>Sovelluksen luominen

Näytetään, miten web-sovelluksen ottaminen käyttöön, jotta seuraavien ohjeiden avulla voit luoda yksinkertaisen "Hei-maailman" Java-sovelluksen ja lataa se App Service-verkkosovelluksen tietokenttiä sovelluksen luotu.

1. Valitse **Tiedosto > Uusi > dynaaminen Web projektin**. Anna sille nimi `JSPHello`. Sinun ei tarvitse muuttaa mitään asetuksia tässä valintaikkunassa. Valitse **Valmis**.

    ![][3]

2. Valitse Project Explorer Laajenna **JSPHello** projekti, **WebContent**hiiren kakkospainikkeella ja valitse sitten **Uusi > JSP tiedoston**. Uusi JSP-valintaikkunassa nimi uuden tiedoston `index.jsp`. Valitse **Seuraava**.

3. **Valitse JSP malli** -valintaikkunassa valitse **Uusi JSP-tiedosto (html)** ja valitse **Valmis**.

4. Index.jsp, Lisää seuraavat koodin `<head>` ja `<body>` tunnisteen osat:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>Hei maailma sovellus suoritetaan localhost

Tämän sovelluksen käyttämiseen, tarvitset, voit määrittää joitakin ominaisuuksia.

1. Napsauta **JSPHello** -projekti ja valitse **Ominaisuudet**.

2. **Ominaisuudet** -valintaikkunassa: Valitse **Java luominen polku**, valitse **tilaus ja vie** -välilehti, tarkista **JRE järjestelmän kirjasto**ja valitse sitten **ylöspäin** Siirry luettelon alkuun.

    ![][4]

3. Myös **Ominaisuudet** -valintaikkunassa: **Kohdistettu CRT Runtime** ja valitse **Seuraava**.

4. Valitse **Uusi Server Runtime-ympäristössä** -valintaikkunan Valitse palvelin, kuten **Apache Tomcat v7.0** ja valitse **Seuraava**. Määritä **Tomcat palvelimen** -valintaikkuna **nimen** `Apache Tomcat v7.0`, ja määritä **Tomcat asennuksen Directory** kansioon, johon haluat käyttää Tomcat palvelimen versio on asennettu.

    ![][5]

    Valitse **Valmis**.

5. Siirry takaisin **Ominaisuudet** -valintaikkunan **Kohdistettu CRT Runtime** -sivulle. Valitse **Apache Tomcat v7.0**ja valitse sitten **OK**.

    ![][6]

6. Pimennys **Suorita** -valikko Valitse **Suorita**. Valitse **Suorita nimellä** -valintaikkunassa **suoritetaan palvelimessa**. Valitse **Tomcat v7.0 palvelimen** **suoritetaan palvelimessa** -valintaikkunassa:

    ![][7]

    Valitse **Valmis**.

7. Sovelluksen suoritettaessa pitäisi näkyä Pimennys localhost ikkunassa näkyvät **JSPHello** sivun (`http://localhost:8080/JSPHello/`), näet seuraavan viestin:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Vie sovellus SODAN

Vie web-projektitiedostoihin web-arkistotiedostoja (SODAN) niin, että voit ottaa sen käyttöön web-sovelluksen. Web project-tiedostot sijaitsevat WebContent-kansio:

    META-INF
    WEB-INF
    index.jsp

1. Napsauta WebContent-kansiota hiiren kakkospainikkeella ja valitse **Vie**.

2. Valitse **Valitse Vie** -valintaikkunan **Web > SODAN** tiedosto ja valitse sitten **Seuraava**.

3. Valitse **SODAN Vie** -valintaikkunasta nykyisessä projektissa src-kansio ja Sisällytä lopussa SOTA-tiedoston nimi. Esimerkki:

    `<project-path>/JSPHello/src/JSPHello.war`

Lisätietoja SODAN tiedostot ottamisesta käyttöön on artikkelissa [Java-sovelluksen Azure palvelun Web sovellukset](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Hei maailma-sovelluksesta valitsemalla FTP käyttöönotto

Valitse kolmannen osapuolen FTP-asiakas, voit julkaista sovellus. Tässä kuvataan on kaksi vaihtoehtoa: Azure; sisäänrakennettu Kudu konsoli ja FileZilla, Suositut työkalu, jonka helposti, graafisessa Käyttöliittymässä.

> **Huomautus:** Azure-Työkalut, Pimennys tukee tallennustilan tilit ja pilvipalveluihin käyttöönottoa, mutta ei tällä hetkellä tue web Apps-sovellusten käyttöönoton. Voit ottaa tallennustilan tilit ja pilvipalveluihin käyttämällä Azure käyttöönotto-projektin kuvatulla tavalla [luomalla Hei maailma sovellus Pimennys Azure varten](http://msdn.microsoft.com/library/azure/hh690944.aspx), mutta ei verkkosovelluksissa. Muilla menetelmillä kuten FTP tai GitHub voivat lähettää toisilleen tiedostoja web App-sovellukseen.

> **Huomautus:** Emme suosittele, FTP: n Windows-komentokehotteen (komentorivin FTP.EXE apuohjelma, mukana Windows). FTP-asiakkaille, jotka käyttävät active FTP, kuten FTP.EXE epäonnistua usein pidemmän palomuurit. Aktiivinen FTP määrittää sisäinen Lähiverkon perustuva osoite, johon FTP-palvelimen epäonnistuu todennäköisesti muodostaa.

Lisätietoja käyttöönoton web App-palvelu-sovellukseen FTP: N avulla on seuraavissa artikkeleissa:

- [Ottaa käyttöön FTP-apuohjelman avulla](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Määritä käyttöönoton tunnistetiedot

Varmista, että olet suorittanut **AzureWebDemo** sovelluksen web-sovelluksen luominen. Voit siirtää tiedostot tähän sijaintiin.

1. Kirjaudu sisään perinteinen portaalin ja sitten **Verkkosovelluksissa**. Varmista, että **WebDemoWebApp** näkyy luettelossa web Apps-sovelluksista ja varmista, että se on käynnissä. Valitse **WebDemoWebApp** Avaa sen **Dashboard** -sivu.

2. **Dashboard** -sivun **Quick Glance**, valitse **Määritä käyttöönotto-tunnistetiedot** (Jos sinulla on jo käyttöönoton tunnistetiedot, tämä lukee **käyttöönoton tunnistetietojen nollaaminen**).

    Käyttöönoton tunnistetiedot liittyy Microsoft-tili. Sinun täytyy määrittää käyttäjänimi ja salasana, jonka avulla voit ottaa käyttöön Git ja FTP. Voit käyttää näitä tunnistetietoja kaikki Microsoft-tilisi Azure tilaukset-web-sovelluksen käyttöön. Anna Git ja FTP käyttöönoton tunnistetiedot-valintaikkunassa ja tallentaa käyttäjänimi ja salasana myöhempää käyttöä varten.


#### <a name="get-ftp-connection-information"></a>FTP-yhteystiedot

FTP avulla voit ottaa käyttöön sovelluksen tiedostot juuri luomasi web App-sovellukseen, haluat saada yhteyden tietoja. Voit hankkia yhteyden tietoja kahdella eri tavalla. Yksi tapa on käy web-sovelluksen **Dashboard** -sivulle. muulla tavalla on verkossa Lataa sovelluksen Julkaise profiili. Julkaise profiili on XML-tiedostoon, joka sisältää tietoja, kuten FTP isännän nimi ja kirjautumistiedot tunnistetiedot web Apps-sovellukset App Azure-palvelussa. Voit käyttää kaikkien tilausten Azure ei vain yhden-tiliin liittyviä-web-sovelluksen käyttöön käyttäjänimi ja salasana.

Saada FTP yhteystiedot [Azure Portal][]web app-sivu:

1. Etsi-kohdassa **Essentials**- ja kopioi **FTP-isäntänimi**. Tämä on URI, joka on samanlainen `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Etsi-kohdassa **Essentials**- ja kopioi **FTP/käyttöönoton käyttäjänimi**. Tämä on muodossa *webappname\deployment käyttäjänimi*; esimerkiksi `WebDemoWebApp\deployer77`.

Voit hankkia Julkaise profiilista FTP yhteyden tietoja:

1. Valitse **Hae Julkaise profiili**web app-sivu. Tämä Lataa .publishsettings tiedoston paikalliseen asemaan.

2. Avaa XML-editoriin tai tekstieditorissa .publishsettings-tiedosto ja Etsi `<publishProfile>` elementti, joka sisältää `publishMethod="FTP"`. Pitäisi näyttää seuraavalta:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Huomaa, että web-sovelluksen `publishProfile` asetukset kartan FileZilla sivuston hallinta-asetukset toimimalla seuraavasti:

- `publishUrl`on sama kuin **FTP-isäntänimi**, voit määrittää **Host**arvo.
- `publishMethod="FTP"`tarkoittaa, että määrität **protokolla** **FTP - File Transfer Protocol**ja **salauksen** , jota **käytetään tavallista FTP**.
- `userName`ja `userPWD` näppäimet todellinen käyttäjänimi ja salasana määritetyt kun käyttöönoton tunnistetiedot palauttaa arvot ovat. `userName`on sama kuin **käyttöönottoa / FTP-käyttäjän**. Ne Yhdistä **käyttäjä** - ja FileZilla **salasana** .
- `ftpPassiveMode="True"`tarkoittaa, että FTP-sivusto käyttää passiivinen FTP-siirto; Valitse **Passiivinen** **Siirtää asetukset** -välilehti.


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Java-sovelluksen isännöimiseen Web App-sovelluksen määrittäminen

Ennen kuin julkaiset sovelluksen, sinun on muutettava joitakin asetukset niin, että web-sovelluksen ylläpitää Java-sovellus.

1. Perinteinen-portaalissa Siirry web-sovelluksen **Dashboard** -sivulle ja valitse **Määritä**. Määritä **määrittäminen** -sivulla seuraavat asetukset.

2. **Java** -versiossa oletusarvo on **käytöstä**; Valitse Java-versio sovelluksen-kohteet; esimerkiksi 1.7.0_51. Kun teet näin, varmista myös, että **Web säilö** on asetettu Tomcat palvelimen versio.

3. **Oletusarvoinen asiakirjoja**Lisää index.jsp ja Siirrä ylös luettelossa. (Oletusarvoisesti tiedoston web Apps-sovelluksista on hostingstart.html.)

4. Valitse **Tallenna**.


#### <a name="publish-your-application-using-kudu"></a>Julkaise sovelluksesi Kudu käyttäminen

Yksi tapa julkaista sovelluksen on käyttämään Azure sisäänrakennettu Kudu virheenkorjaus-konsolin. Kudu kutsutaan vakaata ja täsmällisiä palvelun Web sovellukset ja Tomcat palvelimen kanssa. Voit käyttää web app-konsolin selaamalla URL-osoite seuraavassa muodossa:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Tämä toiminto Kudu konsoli sijaitsee seuraavassa; URL-osoitteessa Siirry tähän sijaintiin:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Valitse yläreunan-valikosta **Virheenkorjaus konsolin > CMD**.

3. Siirry console-komentoriviltä `/site/wwwroot` (tai valitse `site`, valitse `wwwroot` sivun yläreunassa hakemisto-näkymässä):

    `cd /site/wwwroot`

4. Kun olet määrittänyt **Java-versio**, Tomcat server Luo webapps-kansio. Siirry console-komentoriviltä webapps-kansio:

    `mkdir webapps`

    `cd webapps`

5. Vedä JSPHello.war- `<project-path>/JSPHello/src/` ja pudota se Kudu directory näkymän `/site/wwwroot/webapps`. Ei vetämällä sen "Vedä tähän ladataan ja zip-alueelle, koska Tomcat Pura se.

  ![][8]

Ensimmäisen JSPHello.war osoitteessa näkyy erillisenä hakemisto-alueessa:

  ![][9]

Lyhyt aika (luultavasti pienempi kuin 5 minuuttia) Tomcat Server Pura SODAN tiedoston purettu JSPHello-kansioon. Valitse pääkansion onko index.jsp purettuna ja kopioida siellä. Jos näin on, siirry takaisin webapps hakemiston näet, onko purettu JSPHello-kansio on luotu. Jos kohteita ei ole näkyvissä, odota ja toista.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Julkaise sovelluksesi käyttämällä FileZilla (valinnainen)

Muuta työkalua, voit julkaista sovellus on FileZilla, Suositut kolmannen osapuolen FTP asiakkaan kanssa helposti, graafisessa Käyttöliittymässä. Voit ladata ja asentaa FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) , jos sinulla ei vielä ole sitä. Lisätietoja käyttävä näy [FileZilla asiakirjat](https://wiki.filezilla-project.org/Documentation) ja tämän blogimerkinnän [FTP-asiakkaille - osa 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Valitse FileZilla, **Tiedosto > sivuston valvojaa**.
2. Valitse **Sivuston hallinta** -valintaikkunan **Uusi sivusto**. Uusi tyhjä FTP-sivusto tulee näkyviin **Valitsemalla tapahtuma** kannattaa antaa nimi. Tämä toiminto, anna sille nimi `AzureWebDemo-FTP`.

    Valitse **Yleiset** -välilehden Määritä seuraavat asetukset:
    - **Host (isäntä):** Kirjoita **FTP-isäntänimi** , jonka kopioit koontinäyttö.
    - **Portti:** (Jätä tämä kenttä on tyhjä, tämä on passiivinen siirto ja palvelimen määritetään käyttämään portti.)
    - **Protokolla:** FTP File Transfer Protocol
    - **Salaus:** Käytä tavallista FTP
    - **Kirjautuminen tyyppi:** Normaali
    - **Käyttäjä:** Kirjoita käyttöönottoa / FTP-käyttäjä, jonka kopioit koontinäyttö. Tämä on koko FTP käyttäjänimi, jossa on lomake *webappname\username*.
    - **Salasana:** Kirjoita salasana, jonka olet määrittänyt käyttöönoton tunnistetietojen määrittäminen.

    Valitse **Siirrä asetukset** -välilehdessä **Passiivinen**.

3. Valitse **Muodosta yhteys**. Jos onnistuu, FileZilla's konsoliin tulee näkyviin `Status: Connected` viestin ja ongelma `LIST` luettelon kansion sisältö-komennon.

4. **Paikallisen** sivuston-ruudussa Valitse lähdekansio, jossa JSPHello.war tiedosto sijaitsee; polku on seuraavanlainen:

    `<project-path>/JSPHello/src/`

5. **Remote** sivuston-ruudussa Valitse kansio. Otetaan käyttöön SODAN tiedoston `webapps` hakemiston pääkansio web app-kohdassa. Siirry `/site/wwwroot`, napsauta hiiren kakkospainikkeella `wwwroot`, ja valitse **Luo kansio**. Kansion nimen `webapps` ja kirjoita kyseiseen kansioon.

6. Siirtää JSPHello.war, `/site/wwwroot/webapps`. Valitse JSPHello.war **paikallisen** tiedostoluettelossa, napsauttamalla sitä hiiren kakkospainikkeella ja valitse **Lataa**. Näkyviin tulee se näkyvät `/site/wwwroot/webapps`.

7. Kun olet kopioinut JSPHello.war webapps hakemiston, Tomcat palvelin automaattisesti Pura (Pura) SODAN tiedoston tiedostot. Vaikka Tomcat palvelimen alkaa purkamisen lähes välittömästi, voi kestää pitkän ajan (mahdollisesti tuntia), tiedostot näkyvät FTP-asiakas.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Hei maailma-sovelluksen käyttämiseen Web Appissa

1. Kun olet SOTA-tiedosto on ladattu ja varmistaa, että Tomcat palvelin on luonut purettu `JSPHello` hakemistosta, Selaa `http://webdemowebapp.azurewebsites.net/JSPHello` sovelluksen käyttämiseen.

    > **Huomautus:** Jos perinteinen-portaalista valitsemalla **Selaa** voit saada oletusarvon verkkosivulle, ajattelevat "tämän Java-pohjainen web-sovelluksen onnistui." Voit joutua Päivitä WWW-sivu, jotta voit tarkastella sovelluksen tulosteen oletusarvon WWW-sivun sijaan.

2. Kun sovellus suoritetaan pitäisi näkyä seuraava tulos web-sivua:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Puhdista Azure resurssit

Tämä menetelmä luo sovelluksen palvelun web app-sovelluksessa. Voit laskutetaan resurssin, kunhan se on olemassa. Jos haluat jatkaa web App-sovelluksen käytön kokeilua tai kehittämistä, kannattaa pysäyttäminen tai sen poistaminen. Web-sovellusta, joka on pysäytetty edelleen selvittää pieni kulu, mutta voi käynnistää uudelleen milloin tahansa. Web-sovelluksen poistaminen poistaa kaikki siihen lataamasi tiedot.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=529714
[WWW-ympäristö asennusohjelma]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure työkalujen Pimennys varten]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure perinteinen portal]: https://manage.windowsazure.com
[Azure AD-kansion ominaisuudet]: http://technet.microsoft.com/library/jj573650.aspx
[Luo ja lataa hallinta-varmenteen Azure varten]: ../cloud-services/cloud-services-certs-create.md
[Avain ja varmenteen hallintatyökalun (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure Portal]: https://portal.azure.com
