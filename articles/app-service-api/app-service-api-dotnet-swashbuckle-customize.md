
<properties 
    pageTitle="Mukauta Swashbuckle luodut API määritelmät" 
    description="Opettele mukauttamaan Swashbuckle avulla luotujen Azure App Service API sovelluksen Swagger API-määrityksiä." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Mukauta Swashbuckle luodut API määritelmät 

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit mukauttaa Swashbuckle käsittelemään Yleisiä tilanteita, joissa, jossa voit muuttaa oletustoiminnan:

* Swashbuckle Luo Osastollasi ohjauskoneen tavoista kaksoiskappaleiden toiminnon tunnisteet
* Swashbuckle oletetaan, että menetelmän vain vastaus on HTTP 200 (OK). 
 
## <a name="customize-operation-identifier-generation"></a>Toiminnon tunnus luontia mukauttaminen

Swashbuckle Luo Swagger toiminnon tunnisteet ja menetelmä nimi ohjauskoneen ketjuttamalla. Tämä kaava luo ongelma, kun sinulla on useita Osastollasi menetelmää: Swashbuckle Luo kaksoiskappaleiden toiminnon tunnukset, joka on virheellinen Swagger JSON.

Esimerkiksi seuraava koodi ohjauskoneen aiheuttaa Swashbuckle luo kolme Contact_Get toiminnon tunnukset.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Voit ratkaista ongelman manuaalisesti antamalla menetelmiä yksilöllisesti, kuten tässä esimerkissä seuraavasti:

* Hae
* GetById
* GetPage

Laajenna Swashbuckle, jotta se luo automaattisesti yksilöllisen toiminnon tunnukset ei-toivottuja.

Seuraavissa vaiheissa kuvataan mukauttamisesta Swashbuckle, joka sisältyy projektin Visual Studio API sovellusten esikatselu projektimalli *SwaggerConfig.cs* -tiedoston avulla.  Voit myös mukauttaa Swashbuckle verkko-Ohjelmointirajapinnan projekteissa, jotka määrität kuin API-sovelluksen käyttöönottoa varten.

1. Luoda mukautetun `IOperationFilter` käyttöönotto 

    `IOperationFilter` Laajennettavuus-kohta on Swashbuckle käyttäjille, jotka haluat mukauttaa Swagger metatietojen prosessin eri ominaisuuksia. Seuraava koodi esitellään yksi tapa toiminnon-tunnus-luontia toiminnan muuttaminen. Koodin Lisää parametrinimet toiminnon tunnusnimi.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. *App_Start\SwaggerConfig.cs* -tiedostossa, soita `OperationFilter` menetelmä aiheuttaa Swashbuckle, jos haluat käyttää uutta `IOperationFilter` käyttöönotto.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    *SwaggerConfig.cs* tiedosto poistetaan Swashbuckle NuGet-paketti sisältää useita Kommentoitu ulos esimerkkejä laajennettavuus asioista. Lisää kommentit eivät näy tässä. 

    Kun olet tehnyt tämän muutoksen oman `IOperationFilter` käyttöönoton käytetään ja aiheuttaa yksilöllinen toiminnon tunnukset luodaan.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Salli vastauksen koodit kuin 200

Oletusarvon mukaan Swashbuckle olettaa, että HTTP 200 (OK)-vastauksen verkko-Ohjelmointirajapinnan menetelmän *vain* sellaisia vastaus. Haluat ehkä palauttaa vastauksen muita koodeja ilman asiakkaan nostaa poikkeuksen vuoksi.  Esimerkiksi seuraava verkko-Ohjelmointirajapinnan koodi esitellään tilanne, jossa haluat ehkä asiakkaan Hyväksy 200 tai 404 kuin kelvollinen vastaukset.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

Tässä skenaariossa Swagger, joka Swashbuckle luo oletusarvoisesti määrittää vain yhden oikeutetut HTTP-tilakoodin, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Visual Studio käytetään Swagger API-määritys luo asiakkaan koodi, koska se luo asiakas-koodi, joka aiheuttaa poikkeuksen huomautuksiin kuin HTTP-200. Seuraava koodi on esimerkki verkko-Ohjelmointirajapinnan menetelmä luo C#-asiakasohjelma.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle on kaksi tapaa mukauttaa odotettu HTTP-vastauksen koodit, jonka se luo luettelo XML kommentteja tai `SwaggerResponse` määrite. Määrite on helppoa, mutta se on vain käytettävissä Swashbuckle 5.1.5:ssä tai uudempi versio. Visual Studio 2013 API sovellusten esikatselu uusi projekti-malli sisältää Swashbuckle versio 5.0.0, joten jos käyttää mallia etkä halua päivittää Swashbuckle, ainoana vaihtoehtona on käyttää XML-kommentteja. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Odotettu Vastauskoodit XML kommentteja mukauttaminen

Käytä tätä menetelmää voit määrittää vastauksen koodit, jos Swashbuckle-versio on aiempi kuin 5.1.5:ssä.

1. Lisää XML-dokumentaation kommenttien haluat määrittää HTTP vastauksen koodimerkkijonon menetelmistä päälle. Ottaen otosten verkko-Ohjelmointirajapinnan toiminnon yllä ja XML-dokumentointia soveltaminen johtaa koodi, kuten seuraavassa esimerkissä. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Lisää ohjeita *SwaggerConfig.cs* tiedostoa, johon haluat ohjata Swashbuckle, jos haluat käyttää XML-asiakirjat-tiedosto.

    * Avaa *SwaggerConfig.cs* ja luo menetelmän *SwaggerConfig* luokan polun asiakirjat XML-tiedostoon. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Vierittää alaspäin *SwaggerConfig.cs* tiedoston kunnes näet koodin yksinkertaisia alla näyttökuvan rivin Kommentoitu-kohtaa. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Kommentointi rivin käyttöön XML-kommentteja käsittelyn Swagger luonnin aikana. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Luo asiakirjat XML-tiedoston, jotta salliva asetus projektin ominaisuudet ja XML-asiakirjat määritystiedoston, kuten alla olevassa näyttökuvassa. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Kun olet tehnyt nämä vaiheet, luomien Swashbuckle Swagger JSON vaikuttavat HTTP-vastauksen koodit, jonka olet määrittänyt XML-kommentit. Seuraavassa näyttökuvassa esitetään tämän uuden JSON-paketti. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Kun käytät Visual Studio uudelleen asiakas-koodin REST API-C#-koodin hyväksyy HTTP OK sekä ei löydy tilakoodin ilman nostaminen poikkeuksen salliminen vievää koodi päätösten siitä, miten voit käsitellä palauttamista null Contact-tietue. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Tässä esittelyssä koodi löytyy [GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse)-säilössä. Sekä verkko-Ohjelmointirajapinnan on project XML-asiakirjat kommentteja merkitty Konsolisovellus-projekti, joka sisältää luodun asiakkaan Tämä API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Mukauta odotettu Vastauskoodit SwaggerResponse-määritteen avulla

[SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) -määrite on käytettävissä Swashbuckle 5.1.5:ssä ja uudempi versio. Siltä varalta, että projektin on aiempi versio, tämän osan käynnistää kuvaaminen Päivitä Swashbuckle NuGet-paketti, jotta voit määrittää tämän määritteen.

1. **Ratkaisunhallinnassa**verkko-Ohjelmointirajapinnan projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Napsauta vieressä *Swashbuckle* NuGet paketin *Päivitä* -painiketta. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Lisää verkko-Ohjelmointirajapinnan toiminnon menetelmiä, jonka haluat määrittää kelvollinen HTTP-vastauksen koodit *SwaggerResponse* määritteet. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Lisää `using` määritteen nimitilan tietosuojatiedot:

        using Swashbuckle.Swagger.Annotations;
        
1. Siirry projektin */swagger/docs/v1* URL-osoite ja eri HTTP-vastauksen koodit on näkyvissä Swagger JSON-muodossa. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Tässä esittelyssä koodi löytyy [GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes)-säilössä. Verkko-Ohjelmointirajapinnan sekä projektin koristellusta *SwaggerResponse* -määrite on konsolisovellus-projekti, joka sisältää luodun asiakkaan Tämä API. 

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on osoittanut voit mukauttaa Swashbuckle Luo toiminnon tunnukset ja kelvollinen Vastauskoodit. Lisätietoja on artikkelissa [Swashbuckle-GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
