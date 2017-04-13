<properties
   pageTitle="Sovellusten tietojen järvi kaupan Java SDK avulla | Microsoft Azure"
   description="Sovellusten kehittämiseen Azure tietojen järvi kaupan Java SDK avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Azure Lake Tietosäilölle käyttämällä Java käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Opettele käyttämään Azure tietojen järvi kaupan Java SDK suorittamaan peruslaskutoimituksia, kuten luoda kansioita, lataa ja lataa datatiedostojen jne. Saat lisätietoja tietojen järvi [Azure järvi tietosäilö](data-lake-store-overview.md).

Voit käyttää Java SDK-Ohjelmointirajapinnan docs Azure tietojen järvi kaupan [Azure tietojen järvi kaupan Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/)-palvelussa.

## <a name="prerequisites"></a>Edellytykset

* Java Development Kit (JDK 7 tai uudempi versio, käyttämällä Java 1.7 tai uudempi versio)
* Azure järvi tietovaraston tili. Ohjeiden etsiminen [Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md).
* [Maven-testi](https://maven.apache.org/install.html). Tässä opetusohjelmassa käyttää Muodosta ja projektin riippuvuuksien maven-testi. Se on mahdollista rakentaa käyttämättä muodosta järjestelmän, kuten maven-testi tai Gradle, mutta näitä systems-merkki on helpompaa riippuvuuksien hallinta.
* (Valinnainen) Ja IDE, kuten [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) tai [Pimennys](https://www.eclipse.org/downloads/) tai samankaltaisia.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Miten Azure Active Directoryn avulla todennetaan?

Tässä opetusohjelmassa Käytämme Azure AD-sovelluksen asiakas-salaisuus hakemiseen Azure Active Directory-tunnuksen (palvelu todennus). Käytämme tunnuksessa järvi tietovaraston asiakkaan objektin, voit suorittaa toimintoja tiedostojen ja kansioiden luomiseen. Ohjeet todentamismenetelmä Azure Lake Tietosäilölle käyttämällä asiakkaan toiminta on seuraavien toimien korkean tason:

1. Azure AD-web-sovelluksen luominen
2. Noutaa Asiakastunnus, asiakkaan salaisuus ja suojaustunnuksen päätepisteen Azure AD-web-sovelluksen.
3. Määritä Azure AD-web-sovelluksen käytön järvi tietovaraston tiedostoa tai kansiota, jota haluat käyttää luot Java-sovelluksesta.

Ohjeita siitä, miten voit suorittaa nämä vaiheet on artikkelissa [Active Directory-sovelluksen luominen](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory muita vaihtoehtoja sekä hakemiseen tunnusta. Voit valita useista eri käyttöoikeuksien järjestelmiä sopivaksi käyttämässäsi skenaariossa esimerkiksi sovelluksen selaimessa, jakaa työpöydän sovelluksena sovelluksen tai paikallisen käynnissä palvelin-sovelluksen tai Azure virtuaalikoneen. Voit myös valita erilaisista tunnistetiedot, kuten salasanoja, varmenteet, 2 monimenetelmäinen todentaminen. Lisäksi Azure Active Directoryn avulla voit synkronoida paikallisen Active Directory-käyttäjiä pilven kautta. Lisätietoja on artikkelissa [Azure Active Directory käyttöoikeuksien tilanteita, joissa](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Java-sovelluksen luominen

Koodin Esimerkki käytettävissä [-GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) kerrotaan läpi luominen tiedostojen kaupan, yhdistämis-tiedostoja, tiedoston lataamisen ja poistamalla tiedostoja Storessa. Tässä osassa on artikkelissa käy läpi koodin tärkeimmät osat.

1. Luo maven-testi projekti-komentorivillä [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) tai IDE käyttämisen. Ohjeet Java projektin käyttämällä IntelliJ luomisesta [tähän](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Ohjeet avulla Pimennys luomisesta [tähän](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Lisää seuraavat riippuvuudet maven-testi **pom.xml** tiedostoon. Lisää tekstiä seuraavat katkelma ** \</version >** tunnisteen ja ** \</Projekti >** tunniste:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Ensimmäinen riippuvuus on käyttää tietojen järvi kaupan SDK (`azure-datalake-store`) maven-testi säilöstä. Toinen riippuvuuden (`slf4j-nop`) voidaan määrittää, mitkä Kirjaaminen framework käyttämään tämän sovelluksen. Tietoja järvi kaupan SDK käyttää [slf4j](http://www.slf4j.org/) kirjaaminen ulkoseinä, jolloin voit valita useita Suositut kirjaaminen kehysten, kuten log4j, Java kirjaaminen logback jne tai ei ole kirjaaminen. Tässä esimerkissä Microsoft poistaa kirjaaminen käytöstä, näin ollen Käytämme **slf4j nop** sidonta. Muut kirjaamisasetusten sovelluksessa on artikkelissa [tähän](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Lisää sovellus-koodi

On kolme pääosaa koodiin.

1. Azure Active Directory-tunnuksen hankkiminen

2. Tunnuksen avulla voit luoda järvi tietovaraston asiakas.

3. Tietosäilö järvi asiakkaan avulla voit suorittaa toimintoja.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Vaihe 1: Hanki Azure Active Directory-tunnuksen.

Tietoja järvi kaupan SDK antaa helposti tavoista, joilla voit hankkia tarvittavat jutella järvi tietovaraston tilin suojausta tunnusten. SDK ei kuitenkaan määrätä näistä tavoista käytetään. Voit käyttää muita tarkoittaa, että tunnuksen sekä, kuten [Azure Active Directory-SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)tai oman mukautetun koodin avulla.

Tietoja järvi kaupan SDK avulla voit hankkia tunnuksen aiemmin luomasi Active Directory-Web-sovelluksen, käytä staattinen esitetyssä `AzureADAuthenticator` luokka. **TÄYTÄ---tähän** tilalle todellisten arvojen Azure Active Directory-Web-sovelluksen.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Vaihe 2: Luo objekti Azure järvi tietovaraston asiakkaan (ADLStoreClient)

[ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) -objektin luominen edellyttää, että määrität järvi tietovaraston tilin nimi ja viimeisessä vaiheessa luotu Azure Active Directory-tunnuksen. Huomaa, että järvi tietovaraston tilin nimen on oltava täydellinen toimialuenimi. Esimerkiksi tilalle **TÄYTÖN---tähän** suunnilleen **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Vaihe 3: Käyttäminen tiedostojen ja kansioiden toimintojen suorittaminen ADLStoreClient

Seuraava koodi on esimerkki katkelmat, jotkin Yleiset toiminnot. Voi tarkastella koko [tietojen järvi kaupan Java SDK-Ohjelmointirajapinnan docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) **ADLStoreClient** objektin näet muita toimintoja.
 
Huomaa, että tiedostot lukea ja kirjoittaa ne käyttämällä vakio Java virtaa. Tämä tarkoittaa, että olet kerroksen jokin Java-virtaa päälle järvi tietovaraston virtaa hyötyvät vakio Java-toiminto (esimerkiksi Tulosta virtaa muotoillun tai jokin pakkausta tai salausta virtaa lisäominaisuuksia ylä, jne.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Vaihe 4: Muodosta ja suorita sovellus

1. Suorittaminen IDE sisällä, Etsi ja valitse **Suorita** -painike. Jos haluat suorittaa maven-testi, käytä [johto: suoritus](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Erillisestä purkki, suorittamalla komentorivin muodostaa tuottamaan purkkiin kaikki riippuvuudet mukana, käyttämällä [maven-testi kokoonpanon laajennuksen](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). [Esimerkki lähdekoodi-github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) pom.xml on esimerkki siitä, miten voit tehdä tämän.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
