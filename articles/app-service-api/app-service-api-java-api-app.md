<properties
    pageTitle="Muodosta ja ota käyttöön Java API-sovellus App Azure-palvelussa"
    description="Opettele Java API-sovelluspaketin luominen ja käyttöönotto Azure-sovelluksen palveluun."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Muodosta ja ota käyttöön Java API-sovellus App Azure-palvelussa

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tässä opetusohjelmassa näytetään Java-sovelluksen luominen ja ota se käyttöön Azure Service API sovellukset [Git]avulla. Tässä opetusohjelmassa ohjeita voit seurata sellaisille käyttöjärjestelmille, joka voi suorittaa Java. Tässä opetusohjelmassa koodi on luotu käyttämällä [maven-testi]. [Jax RS] avulla luodaan RESTful palvelua ja luodaan [Swagger] -metatiedot määrityksen [Swagger editorin]perusteella.

## <a name="prerequisites"></a>Edellytykset

1. [Java Developer Kit 8] \(tai uudempi versio)
1. Asennettu kehittäminen [maven-testi]
1. [Git] asennettu kehittäminen
1. [Microsoft Azure] maksettu tai [maksuttoman kokeiluversion] tilausta
1. Jokin HTTP testisovellus, esimerkiksi [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Scaffold Ohjelmointirajapinnan käyttäminen Swagger.IO

Swagger.io online-editorin avulla voit kirjoittaa omaa API rakenteen Swagger JSON tai YAML aktivoitavan. Kun sinulla on suunniteltu API pinta, voit viedä eri alustojen ja kehysten koodi. Seuraavan osion scaffolded koodin muokataan sisältämään mock toimintoja. 

Tässä esittelyssä alkavat Swagger JSON tekstissä, ohjelma liittää swagger.io editoriin, jonka jälkeen voidaan luoda tekeminen JAX RS käyttämään REST API päätepisteen käyttö-koodin. Tämän jälkeen Muokkaa scaffolded koodia mock tietojen palauttaminen simuloimalla laadittuihin aidot tietojen pysyvyyttä järjestelmä REST-Ohjelmointirajapinnalla.  

1. Kopioi seuraava koodi Swagger JSON Leikepöydän:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Siirry [Swagger verkossa editorissa]. Kerran, valitse **Tiedosto -valikosta Liitä JSON** -valikkovaihtoehto.

    ![Kohteen liittäminen JSON-valikko][paste-json]

1. Liitä Yhteystiedot luettelon API Swagger JSON aiemmin kopioitu. 

    ![JSON-koodin liittäminen Swagger][pasted-swagger]

1. Näytä dokumentaatio sivuja ja API yhteenveto palautui editorin. 

    ![Näytä Swagger luodut tiedostot][view-swagger-generated-docs]

1. Valitsemalla **Luo Server-> JAX RS** valikko Muokkaa myöhemmin Lisää mock käyttöönoton palvelinpuolen koodin scaffold. 

    ![Luo koodi-valikkovaihtoehto][generate-code-menu-item]

    Koodin luodaan, kun sinun annettava ZIP-tiedoston lataaminen. Tiedoston nimi sisältää scaffolded Swagger koodin luontitoiminnon avulla koodin ja kaikki liittyvät luodaan komentosarjoja. Pura koko kirjaston kansioon kehittäminen työasemalle. 

## <a name="edit-the-code-to-add-api-implementation"></a>Voit lisätä API käyttöönotto-koodin muokkaaminen

Tässä osassa Swagger luotu koodi palvelinpuolen käyttöönoton tilalle mukautettua koodia. Uusi koodi palauttaa yhteyshenkilön ArrayList kohteiden puheluja asiakkaalle. 

1. Avaa tiedosto *Contact.java* malli, joka sijaitsee *src/gen/java/io/swagger/malli* -kansiossa [Visual Studio-koodin] tai tuttuja tekstieditorissa. 

    ![Avaa yhteystietojen mallitiedosto][open-contact-model-file]

1. Lisää seuraavat konstruktoria **yhteyshenkilö** -luokka. 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Avaa *ContactsApiServiceImpl.java* palvelun käyttöönoton-tiedosto, joka sijaitsee *src/pää/java/io/swagger/api/toteutuksen* -kansiossa [Visual Studio-koodin] tai tuttuja tekstieditorissa.

    ![Avaa yhteystietojen koodin Palvelutiedosto][open-contact-service-code-file]

1. Korvaa tiedoston koodin lisääminen mock käyttöönotto palvelukoodi uusi koodi. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Avaa komentorivi-ikkuna ja muuta kansion pääkansion sovelluksesi.

1. Suorita seuraava maven-testi komento, muodosta koodi ja suorita sen jota app-palvelin paikallisesti. 

        mvn package jetty:run

1. Raportissa pitäisi näkyä ulkoasu, jota on aloittanut koodisi porttiin 8080 komentorivi-ikkuna. 

    ![Avaa yhteystietojen koodin Palvelutiedosto][run-jetty-war]

1. [Postman] avulla voit tehdä pyyntö "Saat kaikki yhteystiedot" 8080/api/yhteystiedot API-menetelmää.

    ![Kutsu yhteyshenkilöt Ohjelmointirajapinta][calling-contacts-api]

1. [Postman] avulla voit tehdä pyyntö "get tietyn yhteyshenkilön"-Ohjelmointirajapinnan menetelmää osoitteessa 8080/api/yhteystiedot/2.

    ![Kutsu yhteyshenkilöt Ohjelmointirajapinta][calling-specific-contact-api]

1. Luoda lopuksi suorittamalla seuraavan komennon maven-testi konsoli Java SODAN (Web-arkisto)-tiedosto. 

        mvn package war:war

1. Kun SOTA-tiedosto on luotu, se sijoitetaan **kohde** -kansioon. Siirry kansioon, **kohde** ja SODAN tiedoston nimeksi **ROOT.war**. (Varmista, että kirjainkoon vastaa tässä muodossa).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Suorita seuraavat komennot-sovelluksen **käyttöönotto** kansio ottaa käyttöön SODAN tiedoston Azure pääkansiosta. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Tulosteen julkaiseminen App Azure-palvelu

Tässä osassa artikkelissa Azure-portaalissa uuden API-sovelluksen luominen, isännöinnin Java-sovellusten, API-sovelluksen valmisteleminen ja luomasi SODAN tiedoston käyttöön Azure App palvelu suorittamaan uusi API-sovellus. 

1. [Azure portal]API uuden sovelluksen luominen napsauttamalla **Uusi -> Web + Mobile -> API sovelluksen** -valikkovaihtoehto, sovellusten tietojen syöttäminen ja valitsemalla sitten **Luo**.

    ![Uuden API-sovelluksen luominen][create-api-app]

1. Kun API-sovellus on luotu, Avaa sinua sovelluksen **asetukset** -sivu ja valitse sitten **asetukset** -valikkovaihtoehto. Java uusinta versiota Valitse käytettävissä olevista vaihtoehdoista, ja valitse sitten uusimmat Tomcat **Web säilö** -valikon ja valitse sitten **Tallenna**.

    ![Määritä Java API sovellus-sivu][set-up-java]

1. Valitse **käyttöönoton tunnistetiedot** asetukset-valikkovaihtoehto ja Anna käyttäjänimi ja salasana, jotka haluat käyttää tiedostojen julkaiseminen API-sovelluksen. 

    ![Määritä käyttöönoton tunnistetiedot][deployment-credentials]

1. Valitse **käyttöönoton lähteen** asetukset-valikkovaihtoehto. Kerran ja sitten **Valitse lähde** -painiketta, valitse **Paikallinen Git säilöön** -vaihtoehto ja valitse sitten **OK**. Tämä luo Git säilön Azure-, jolle on määritetty API-sovelluksen käytössä. Aina, kun Git-säilöön, *master* haaraa Vahvista koodi koodisi julkaistaan oman live API App suoritettavan kyselyjä. 

    ![Uuden paikallisen Git säilön määrittäminen][select-git-repo]

1. Kopioi uusi Git säilöön URL-osoite leikepöytääsi. Tallenna, kun se on tärkeää hetken. 

    ![Uuden sovelluksen Git säilö määrittäminen][copy-git-repo-url]

1. Git push SODAN tiedostoa online säilöön. Voit tehdä tämän Siirry aiemmin luomasi niin, että voit helposti Vahvista koodi sovelluksen-palvelu käytössä säilöön ylöspäin **käyttöönotto** -kansioon. Kun olet konsoli-ikkunassa ja ymmärtää webapps-kansion sisältävässä kansioon, antaa seuraavat Git komentoja, Kyselysäännön käyttöönoton käytöstä ja Käynnistä prosessi. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Kun lähetät **push** -pyyntö, sinua pyydetään käyttöönoton tunnistetiedon aiemmin luomasi salasana. Kun olet kirjannut tunnistetiedot, pitäisi näkyä portaalin näyttää päivityksen on otettu käyttöön.

1. Jos käytät Postman, kun taas osumien äskettäin käyttöön-Ohjelmointirajapinnan sovellus App Azure-palvelu käytössä, näet toiminnan ovat yhdenmukaisia ja että se nyt palauttaa yhteyshenkilön tiedot oikein ja käyttämällä Yksinkertainen koodin muutokset Swagger.io scaffolded Java-koodia. 

    ![Java yhteystiedot REST-Ohjelmointirajapinnalla Liven käyttäminen Azure-tietokannassa][postman-calling-azure-contacts]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa ehdit Aloita Swagger JSON-tiedostosta ja jotkin scaffolded saatua Swagger.io editorin Java-koodia. Sieltä yksinkertaisia muutoksia ja Git käyttöön prosessin tuloksena on kirjoitettu Java-toimintojen API-sovellus. Seuraava opetusohjelman näyttää kuinka tarjoaman [API-sovelluksia JavaScript-asiakkaita, käyttämällä CORS][App Service API CORS]. Sarjan myöhemmin opetusohjelmat Näytä todennus- ja ottamisesta käyttöön.

Voit luoda tämän mallin, voit lukea lisää säilyttää JSON-BLOB [Storage SDK Java] . Tai voit käyttää [Asiakirjan DB Java SDK] yhteyshenkilön tietojen tallentamiseen Azure asiakirjan DB. 

Lisätietoja Azure Java käyttämisestä on artikkelissa [Java Developer Center].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portal]: https://portal.azure.com/
[Asiakirjan DB Java SDK-paketissa]: ../documentdb/documentdb-java-application.md
[maksuton kokeiluversio]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Java-kehityskeskus]: /develop/java/
[Java Developer Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[JAX RS]: https://jax-rs-spec.java.net/
[Maven-testi]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger-editori]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Tallennustilan SDK Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger-editori]: http://editor.swagger.io/
[Visual Studio-koodin.]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
