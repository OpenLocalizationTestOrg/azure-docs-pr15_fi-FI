<properties
    pageTitle="Azure AD-Java aloittaminen | Microsoft Azure"
    description="Miten voit luoda Java komentorivi-sovellus, joka kirjautuu käyttäjiä käyttämään Ohjelmointirajapinnan."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Java komentorivi-sovelluksen käyttäminen ja Azure AD Ohjelmointirajapinta

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD on yksinkertainen ja selkeä ulkoistaa web-sovelluksen jäsenyyksien hallinta tarjoaa yhden Kirjaudu sisään ja kirjaudu ulos vain muutaman viivoilla koodin.  Java web Apps-sovelluksissa voit tehdä tämän käyttämällä Microsoftin soveltaminen yhteisön perustuva ADAL4J.

  Käytämme tähän ADAL4J avulla.
- Kirjaudu käyttäjän sovellukseen käyttämällä Azure AD tunnistetietojen toimittaja.
- Näyttää käyttäjän tietoja.
- Kirjaudu ulos sovelluksesta käyttäjä.

Jotta voit tehdä tämän, sinun on:

1. Voit rekisteröidä sovelluksen Azure AD
2. Määrittää sovelluksen ADAL4J-kirjasto.
3. Kirjaudu sisään ja kirjaudu ulos pyynnöt antamaan Azure AD ADAL4J kirjaston avulla.
4. Tulostaa käyttäjän tietoja.

Jos haluat aloittaa, [Lataa sovelluksen rakenne](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Sinun on myös Azure AD-vuokraajan, jolla Rekisteröi sovelluksen.  Jos sinulla ei sitä vielä ole, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. rekisteröidä Azure AD sovellus
Käyttäjien todentamiseen sovelluksen käyttöön ensin tarvitset uusi sovellus rekisteröidään alihallintaan.

- Kirjaudu sisään Azure hallinta-portaalin.
- Valitse vasemmalla olevasta siirtymisruudussa **Active Directorysta**.
- Valitse missä haluat rekisteröidä sovellus vuokraaja.
- Valitse **sovellukset** -välilehti ja valitse Lisää ala-laatikon.
- Seuraa ohjeita ja luo uusi **Web-sovelluksen ja/tai WebAPI**.
    - Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    - **Sign-On URL** on sovelluksen pääkansion URL-osoite.  Oletusarvo rakenne on `http://localhost:8080/adal4jsample/`.
    - **Sovelluksen tunnus URI** on yksilöllinen sovelluksen.  Konferenssi on käyttää `https://<tenant-domain>/<app-name>`, kuten`http://localhost:8080/adal4jsample/`
- Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se määritys-välilehti.

Kerran, kun sovellus portal- **Sovellus salaisuus** sovelluksen luominen ja kopioiminen alaspäin.  Tarvitset sitä pian.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. määrittää sovelluksen ADAL4J kirjasto- ja maven-testi käytön edellytykset
Tässä on määrittää ADAL4J OpenID yhteyden todennus-protokollaa.  ADAL4J käytetään antaa Kirjaudu sisään ja kirjaudu ulos pyyntöjä ja hallita käyttäjän istunnon käyttäjän, muun muassa tietoja.

-   Projektin pääkansiossa avaaminen/luominen `pom.xml` ja Etsi `// TODO: provide dependencies for Maven` ja korvata seuraavasti:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. java PublicClient-tiedoston luominen

Edellä kuvatulla on käyttää Graph-Ohjelmointirajapinnan saadaksesi kirjautuneena sisään käyttäjän tietoja. Tämän ominaisuuden helppokäyttöiseksi us olemme luo tiedoston edustavan **Hakemisto-objekti** ja edustavan **käyttäjän** niin, että Java OO rakenteessa voidaan yksittäisinä tiedostoina.

1. Luo tiedosto nimeltä `DirectoryObject.java` joka Käytämme tallentamiseen perustietojen minkä tahansa DirectoryObject (jonka voit vapaasti tämä myöhempää käyttöä varten muiden Graph kyselyt voi tehdä). Voit voit Leikkaa ja Liitä tämä alla seuraavasti:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Käännä ja suorita otosten

Muuta, pääkansio ja suorittamalla seuraavan komennon luonnissa otosten vain valitseminen ehtorivien `maven`. Tämä käyttää `pom.xml` tiedoston kirjoittaneen riippuvuuksien.

`$ mvn package`

Pitäisi nyt olla `adal4jsample.war` -tiedoston lisääminen `/targets` hakemisto. Voi ottaa, Tomcat säilö ja käy URL-osoite 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
On hyvin helppoa ottamaan SODAN uusimman Tomcat-palvelimiin. Siirry yksinkertaisesti `http://localhost:8080/manager/` noudattamalla näyttöön tulevia ohjeita ladataan oman "adal4jsample.war" tiedosto. Se autodeploy puolestasi oikea päätepisteen kanssa.

##<a name="next-steps"></a>Seuraavat vaiheet

Onnittelen! Nyt käytössäsi on toimiva Java-sovellus, jossa on voi todentaa käyttäjät, suojatusti Soita verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0, ja poistaa käyttäjän perustietoja.  Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa.

Viitteen Esimerkki valmiista (ilman määritysarvot) [tähän .zip taulukkona](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), tai voit voit käytetään sen GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

