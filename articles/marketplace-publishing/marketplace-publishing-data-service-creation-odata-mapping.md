<properties
   pageTitle="Ohje tietojen palvelun luomiseen Marketplacen | Microsoft Azure"
   description="Lisätietoja siitä, miten luominen, varmentaminen ja ottaa käyttöön tietojen palvelun hankkia Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Yhdistäminen aiemmin WWW-palvelun kautta CSDL OData

>[AZURE.IMPORTANT] **Tällä hetkellä onboarding olemme eivät ole enää mitään uusi tietopalvelu julkaisijat. Uusi dataservices ei Hae hyväksyä luettelo.** Jos haluat julkaista AppSource SaaS yrityssovelluksen löydät lisätietoja [tähän](https://appsource.microsoft.com/partners). Jos sinulla IaaS sovellusten tai haluat julkaista Azure Marketplace-Kehitystyökalut-palvelun löytyy lisätietoja [tästä](https://azure.microsoft.com/marketplace/programs/certified/).

Tässä artikkelissa on yleiskatsaus siitä, miten voit yhdistää aiemmin luodun palvelun yhteensopiva OData-palvelu CSDL avulla. Tässä artikkelissa kerrotaan, miten Luo yhdistäminen-asiakirja (CSDL), joka muuntaa syötteen pyynnön asiakaskoneesta palvelun puhelun kautta ja tulos (tiedot) takaisin asiakkaalle kautta on yhteensopiva OData-syötteen. Microsoft Azure Marketplacesta paljastaa palvelut loppukäyttäjien OData-protokollan avulla. Palvelut, jotka ovat näyttämiä sisältöpalveluista (tietojen omistajat) niitä julkaista eri tavoilla, kuten muille käyttäjille, SOAP jne.

## <a name="what-is-a-csdl-and-its-structure"></a>Mikä CSDL ja sen rakennetta?
CSDL (käsitteellisiä rakenteen Definition Language) on määrittämisestä kuvaamaan web tai tietokannan palvelua yleisiä XML työpaikkailmoituksille tyypillisiä sanamuotoja, Azure Marketplace-määritys.

Yksinkertaisen-yleistä **pyytää työnkulku:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Tiedonkulun** on toisin päin:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Kuva 1** kaaviot, miten on jokin muu sähköpostiohjelma hankkia tietojen sisällöntuottaja (palvelun) tarkistamalla Azure Marketplacesta.  CSDL käytetään yhdistäminen/muunnos-komponentin käsittelemään pyynnön ja tiedot läpäisseet sisältötoimittajan palveluihin ja pyytävä asiakkaan välillä.

*Kuva 1: Yksityiskohtaiset kulkuun pyytävä asiakaskoneelta sisältötoimittajan kautta Azure Marketplacesta*

  ![piirustuksen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Taustatietoa Atom Atom kirja ja OData-protokolla, joihin luominen Azure Marketplace-laajennukset, tarkista: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Linkki yläpuolelta ote:     *"(jäljempänä OData) Open Data protocol tarkoituksena on säätää REST-protokollaksi vastaan paljastaa tietopalvelujen resurssien CRUD-tyylin toimintoja (luominen, lukeminen, päivitys ja poista). "Tiedot"palvelu on päätepisteen ei peräisin vähintään yksi"-kunkin jossa nolla tai" ", jotka koostuvat tarjoamia tietoja kirjoitettu nimi-arvo-pareina. OData julkaistaan Microsoft kohdassa OASIS (organisaation siirtymisen, jäsennettyjä tietoja standardeja) standardit niin, että kuka tahansa, jolla haluaa kehittää palvelimissa, asiakkaat tai työkaluja ilman rojaltit tai rajoituksia."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Kolmen tärkeän laitteita, joilla CSDL määrittämää ovat seuraavat:

- **Päätepisteen** , palvelun tarjoajan Web osoite (URI)-palvelun
- **Tietoja parametrien** välitettävä syötteeksi palveluntarjoaja parametrit määritelmät on lähetetty sisältötoimittajan palveluun tietotyypin alaspäin.
- Palautetut pyytää palvelun toimitettavan sisältötoimittajan palvelua, mukaan lukien säilö, sivustokokoelmat ja taulukot, muuttujat/sarakkeiden ja tietotyypit tietojen rakenne tietojen **rakennetta** .

Seuraavassa kaaviossa on esitetty yleiskatsaus jossa asiakkaan kirjoittama OData-lause (sisältötoimittajan verkkopalvelun kutsu) tuloksia ja tietojen hakeminen takaisin kulusta.

  ![piirustuksen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Ohjeita:

1. Asiakkaan lähettää pyynnön kautta Palvelukutsu Azure Marketplacesta XML-määritysten syöteparametrit täyttäminen
2. Vahvista Palvelukutsu käytetään CSDL.
    - Muotoiltu palvelun Soita sitten lähettää sisällön tarjoajan palveluun Azure Marketplacesta
3. Verkkopalvelu suorittaa ja preforms toiminnon Http verbin (eli HAE) palauttaa Azure Marketplace missä pyydetyt tiedot (jos saatavilla) näyttää XML-muodossa, jotta asiakas CSDL määritelty yhdistämistä käyttäen.
4. Asiakkaan lähetetään tiedot (jos saatavilla) XML- tai JSON-muodossa

## <a name="definitions"></a>Määritelmät

### <a name="odata-atom-pub"></a>OData-ATOM kirja

ATOM-julkaisu, jossa kukin merkintä edustaa yhden rivin tuloksen laajennus määrittäminen. Tapahtuma sisällön osaa on parannettu sisältävän rivin – kuin avainarvon paria arvot. Lisätietoja on tässä luettelossa: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - käsitteellinen rakenteen Definition Language

Sallii määrittäminen Funktiot (SPROCs) ja kohteet, joka on määritetty tietokannan kautta. Lisätietoja on täällä: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] **Muut versiot** -valikko ja valitse versio, jos et näe artikkelin.

### <a name="edm---entry-data-model"></a>EDM - tapahtuma-tietomalli
- Yleistä: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Esikatselu: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Tietotyyppien: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Seuraavassa on esitetty yksityiskohtainen vasemmalta oikealle flow-kohtaa, johon asiakas kirjoittaa OData-lause (sisältötoimittajan verkkopalvelun kutsu), tulokset ja tietojen hakeminen takaisin:

  ![piirustuksen](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>CSDL perusteet

CSDL (käsitteellinen rakenteen Definition Language) on määrittämisestä kuvaamaan web tai tietokannan palvelua yleisiä XML työpaikkailmoituksille tyypillisiä sanamuotoja, Azure Marketplace-määritys. CSDL kuvataan tärkeästä kappaletta, **mahdollistaa tietojen kulkee tietolähteen Azure Marketplace.** Tärkeimmät osat on kuvattu seuraavassa:

- LIP-tiedot, jotka kuvaavat kaikki yleisesti saatavilla Funktiot (FunctionImport solmu)
- Kaikkien viestin requests(input) ja viestin responses(outputs) (EntityContainer/EntitySet/entiteettityypin solmujen) tiedot
- Sidonta tietoja transport protocol on käytetty (ylätunniste-solmu)
- Osoitteen tietojen etsimiseen määritetyn palvelun (BaseURI määrite)

Sanottuna CSDL edustaa ympäristö ja kielestä riippumaton sopimuksen palvelun pyytäjän ja palvelun välillä. Käytä CSDL, asiakkaan Etsi/tietokannan verkkopalvelun ja käynnistää jonkin yleisesti saatavilla tehtäviensä.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Tietokannan tai kokoelma CSDL liittyvät
**CSDL-määritys**

CSDL on XML kieliopin kuvaava verkkopalvelun. Itse määritys on jaettu 4 tärkeimmät osat: EntitySet, FunctionImport; Nimitilan ja entiteettityypin.

Jotta tämä otetaan helpommin ymmärrettäviä avulla välisten suhteiden CSDL taulukkoon.

Muista;

  CSDL edustaa ympäristö ja kielestä riippumaton sopimuksen **palvelun pyytäjän** ja **palveluntarjoajan**välillä. Käytä CSDL, **asiakkaan** voit etsiä **palvelu/tietokannan verkkopalvelun** ja käynnistää sen yleiseen käyttöön yhtään **Funktiot.**

Tietoja palvelun CSDL neljä osaa voidaan ajatella tietokannan, taulukon, sarakkeen ja kaupan ohjeiden mukaisesti.

Liittyvät nämä tiedot palvelua varten seuraavasti:

- EntityContainer ~ = tietokanta
- EntitySet ~ = taulukko
- Entiteettityypin ~ = sarakkeet
- FunctionImport ~ = tallennettu toimintosarja

**HTTP-käyttö sallittu**
- HAE – palauttaa arvot db (palauttaa kokoelman)
- Kirjaa – käytetään välittämään tietoja ja valinnainen palautusarvojen db (Luo uusi merkintä sivustokokoelman, palaa tunnus tai URI)
- Poista – poistaa tietoja DB (poistaa kokoelma)
- SIJOITA – tietojen päivittäminen tietokantaan (korvaa kokoelma tai luoda uuden)

## <a name="metadatamapping-document"></a>Metatietojen/yhdistäminen asiakirjaan

Metatietojen/yhdistäminen asiakirjan käytetään Yhdistä sisällön toimittaja aiemmin verkkopalvelut niin, että se voi olla näyttämiä OData-web-palvelu Azure Marketplace-järjestelmän. Se perustuu CSDL ja toteuttaa muutaman laajennuksia CSDL muiden tarpeiden perusteella verkkopalvelut tarjoamia kautta Azure Marketplacesta. Tunnisteet löytyvät [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) nimitilan.

Esimerkki CSDL seuraavasti: (kopioida ja liittää seuraavassa esimerkissä CSDL XML-editoriin ja muuta vastaamaan palvelun.  Liitä CSDL yhdistäminen DataService välilehden luotaessa palvelun [Azure Marketplace-Julkaisemisportaali](https://publish.windowsazure.com)).

**Ehdot:** Liittyvät CSDL ehtojen [Julkaisemisportaali](https://publish.windowsazure.com) Käyttöliittymä (PPUI) ehtoja.
- Tarjouksen "Otsikko" PPUI liittyvät MyWebOffer
- Yritys-PPUI liittyvät **Publisher näyttönimi** [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) Käyttöliittymä
- Oman API liittyvät verkossa tai tietopalvelu (PPUI suunnitelma)

**Hierarkian:** 
 yrityksen (sisällön toimittaja) omistaa Offer(s), joilla on suunnitelman tai ilmoitetut suunnitelmat, eli service(s), mitkä rivi ylöspäin Ohjelmointirajapinnan kanssa.

### <a name="webservice-csdl-example"></a>Verkkopalvelu CSDL Esimerkki

Muodostaa yhteyden palvelu, joka paljastaa web application päätepisteen (esimerkiksi C# sovellus)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Näytä CSDL verkkopalvelun esimerkkejä artikkelissa [Esimerkkejä yhdistäminen muodostaminen OData-CSDLs nykyinen web-palvelu](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>DataService CSDL Esimerkki

Muodostaa yhteyden palvelu, joka on paljastaa tietokannan taulukon tai näkymän, kuten päätepisteen seuraavassa esimerkissä näkyy kaksi ohjelmointirajapinnan perus tietojen perusteella API CSDL (käyttää näkymiä taulukoiden sijaan).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Katso myös
- Jos olet kiinnostunut liittyviä tietoja tietyn solmut ja niiden parametrit määritykset ja selityksiä sekä esimerkkejä [Tietojen palvelun OData yhdistäminen solmujen](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) tässä artikkelissa ja käyttää kirjainkoon kontekstissa.
- Jos olet kiinnostunut tarkistaminen esimerkkejä, lue tämän artikkelin [Tiedot palvelun OData yhdistäminen esimerkkejä](marketplace-publishing-data-service-creation-odata-mapping-examples.md) otoksen koodi ja ymmärtää kenttäkoodin syntaksi ja kontekstia.
- Palaa määrätyn polun tietojen palvelun julkaiseminen Azure Marketplace-artikkeli [Tietoja palvelun julkaisun opas](marketplace-publishing-data-service-creation.md).
