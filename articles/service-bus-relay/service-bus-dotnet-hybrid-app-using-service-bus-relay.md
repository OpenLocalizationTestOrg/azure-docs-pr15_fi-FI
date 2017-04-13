<properties
    pageTitle="Hybrid--paikallisen/cloud sovelluksen (.NET) | Microsoft Azure"
    description="Opettele luomaan .NET--paikallisen/cloud hybrid-sovellus, joka käyttää Azure palvelun Bus välitys."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET--paikallisen/cloud hybrid-sovelluksesta valitsemalla Azure palvelun Bus välitys

## <a name="introduction"></a>Johdanto

Tässä artikkelissa kuvataan, miten voit luoda hybrid cloud-sovellusta, jonka Microsoft Azure ja Visual Studio. Opetusohjelman oletetaan, että ei ole edellisen kokemus Azure avulla. Käytössäsi on useita Azure resurssien määrittäminen käyttävä sovellus ja suorittamalla pilveen, alle puolessa tunnissa.

Opit seuraavat asiat:

-   Voit luoda tai mukauttaa olemassa olevan WWW-palvelun käyttöön web-ratkaisulla.
-   Opit käyttämään Azure palvelun Bus välityspalvelu jakaminen Azure sovelluksen ja verkkopalvelun tietojen pitäminen muualla.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Kuinka palvelun Bus välitys auttaa hybrid ratkaisut

Yritysratkaisut koostuvat yleensä yhdistelmän mukautettua koodia kirjoitettu aloittamaan uuden ja ainutkertainen liiketoiminnan vaatimukset ja ratkaisut ja järjestelmät, jotka on jo olemassa toimintoja.

Ratkaisu architects mahdollisesti pilveen käytettävät helpompaa käsittely asteikko vaatimukset ja pienempi toiminnallisia kustannukset. Näin ne Etsi että haluavat hyödyntää kuin ehdotetaan niihin ratkaisuja rakenneosien yrityksen palomuuri sisään ja ulos helposti aiemmin service-resurssien mukaan cloud-ratkaisun saavuttaa käytön. Monia sisäinen palveluita ei sisäisten tai ylläpidettävä siten, että ne voivat olla helposti tarjoamia yritysverkon reunan.

Palvelun Bus välitys on suunniteltu tehtäväksi aiemmin Windows Communication Foundation (WCF)-verkkopalvelut Käyttötapaus ja että näistä palveluista suojatusti ratkaisuja, jotka sijaitsevat ulkopuolella yrityksen ympäröivän tarvitsematta yrityksen verkkoinfrastruktuuria tunkeutuva muutokset. Palvelun Bus välitys näiden palvelujen ylläpidetään edelleen sisällä käytössä-ympäristö, mutta ne delegoida Kuuntele saapuvat istunnot ja cloud isännöimä palvelun Bus pyynnöt. Palvelun Bus suojaa myös luvattomasti näistä palveluista [Jaettu Access allekirjoitus](../service-bus-messaging/service-bus-sas-overview.md) (SAS) todennuksen avulla.

## <a name="solution-scenario"></a>Skenaario

Tässä opetusohjelmassa Luo ASP.NET-sivustoon, jonka avulla voit tarkastella tuoteluettelosta tuotteen varaston sivulla.

![][0]

Opetusohjelman olettaa on tuotetiedot paikallisen-järjestelmässä ja käyttää palvelun Bus välitys saavuttamiseksi kyseiseen järjestelmään. Tämä on Simuloitu web-palvelu, joka suorittaa yksinkertaisen console-sovelluksessa ja tuotteiden määrittää siihen ladatun varmuuskopioidaan. Osaat tämän konsolisovelluksen käyttämiseen omaan tietokoneeseesi ja asentaa web-roolin Azure. Näin näet miten käynnissä Azure joten web-roolin varmasti soittaa tietokoneeseen, vaikka tietokoneesi liittyville lähes varmasti vähintään yhden palomuurin ja verkon osoite muuntaminen (NAT) kerroksen takana.

Seuraavassa on näyttökuva valmiin web-sovelluksen aloitussivu.

![][1]

## <a name="set-up-the-development-environment"></a>Määritä kehitysympäristö

Ennen kuin aloitat Azure sovellusten kehittämiseen, Hanki työkalut ja kehitysympäristön määritys.

1.  Asenna Azure SDK for .NET [Hae työkalut ja SDK][] -sivulta.

2.  Valitse **Asenna SDK** -versiota käytät Visual Studio. Tässä opetusohjelmassa käytetään Visual Studio 2015.

4.  Kun sinua pyydetään suorittamaan tai tallentamaan asennusohjelma, valitse **Suorita**.

5.  **WWW-ympäristö asennusohjelma**valitsemalla **Asenna** ja jatkaa asennusta.

6.  Kun asennus on valmis, sinulla on kaikki tarvittavat voit kehittää sovellusta. SDK sisältää työkaluja, joiden avulla voit helposti kehittää Azure sovellusten Visual Studiossa. Jos sinulla ei ole asennettu Visual Studiossa, SDK asentaa myös ilmainen Visual Studio Express.

## <a name="create-a-namespace"></a>Luo nimitila

Aloita Azure-palvelu Bus toimintoja käyttämällä, sinun on luotava palvelun nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Luo paikalliseen Serveriin

Ensin luot (mock) paikallisen tuotteen luettelon järjestelmän. Se on melko helppoa. Näet tämän kuin todellinen paikallisen tuotteen luettelon kanssa valmis palvelun pinta, joka on haluat integroida järjestelmää edustava.

Tämä projekti on Visual Studio console-sovellus ja käyttää [Azure palvelun Bus NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) sisältää palvelun Bus kirjastoja ja asetukset.

### <a name="create-the-project"></a>Projektin luominen

1.  Käytä järjestelmänvalvojan oikeuksia, Käynnistä Microsoft Visual Studio. Käynnistä Visual Studio ja järjestelmänvalvojan oikeudet, **Visual Studio** -ohjelmakuvaketta hiiren kakkospainikkeella ja valitse sitten **Suorita järjestelmänvalvojana**.

2.  Visual Studiossa, valitse **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.

3.  Valitse **Asennetut mallit**- **Visual C#**- **Konsolisovelluksen**. Kirjoita **nimi** -ruutuun nimi **ProductsServer**:

    ![][11]

4.  Valitse **OK** **ProductsServer** projektin luominen.

7.  Jos olet jo asentanut Visual Studio NuGet paketin hallintaa, siirry seuraavaan vaiheeseen. Muussa tapauksessa käy [NuGet][] ja valitse [Asenna NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Kehotteiden mukaisesti asentaa NuGet pakettien hallinta ja valitse sitten Aloita uudelleen Visual Studio.

7.  Napsauta ratkaisunhallinnassa **ProductsServer** projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

8.  **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Valitse **Asenna**ja hyväksy käyttöehdot.

    ![][13]

    Huomaa, että tarvittavat asiakkaan kokoonpanon viitataan nyt.

9.  Lisää uusi luokka tuotteen sopimuksen. Napsauta ratkaisunhallinnassa **ProductsServer** projektin hiiren kakkospainikkeella ja valitse **Lisää**ja valitse sitten **luokan**.

10. Kirjoita **nimi** -ruutuun nimi **ProductsContract.cs**. Valitse **Lisää**.

11. Korvaa nimitilan määritys **ProductsContract.cs**seuraava koodi, joka määrittää sopimus-palvelun kanssa.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Korvaa nimitilan määritys Program.cs, seuraava koodi, joka lisää profiili-palvelu ja sen host.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Napsauta ratkaisunhallinnassa Kaksoisnapsauta Visual Studio-editorin avaaminen **App.config** -tiedostoa. Alareunassa ** &lt;järjestelmän. ServiceModel&gt; ** elementin (mutta edelleen toimintoa &lt;järjestelmän. ServiceModel&gt;), Lisää seuraava XML-koodi. Varmista *yourServiceNamespace* korvaaminen nimi nimitilan ja *yourKey* SAS avaimella, voit hakea aiemmin-portaalista:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Edelleen-App.config, valitse ** &lt;appSettings&gt; ** elementti, korvaa yhteyden merkkijonoarvo aiemmin portaalin saadut yhteysmerkkijonon kanssa. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Painamalla **Ctrl + Vaihto + B** tai **luoda** -valikosta valitsemalla **Muodosta ratkaisu** muodosta sovellus ja tarkista työsi tarkkuudella tähän mennessä.

## <a name="create-an-aspnet-application"></a>ASP.NET-sovelluksen luominen

Tämän osion luot yksinkertaisen ASP.NET-sovelluksessa, jossa näkyy tuotteen-palvelun tiedot.

### <a name="create-the-project"></a>Projektin luominen

1.  Varmista, että Visual Studio on käynnissä ja järjestelmänvalvojan oikeudet.

2.  Visual Studiossa, valitse **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.

3.  **Asennetut mallit**Valitse **Visual C#**-kohdassa **ASP.NET Web-sovelluksen**. Projektin **ProductsPortal**nimi. Valitse **OK**.

    ![][15]

4.  Valitse **Valitse malli** -luettelosta **MVC**. 

6.  Valitse valintaruutu isännän **pilveen**.

    ![][16]

5. Valitse **Muuta** Authentication. **Muuta todennus** -valintaikkunassa **Ei todennus**ja valitse sitten **OK**. Tässä opetusohjelmassa, olet käyttöönotto sovelluksen käyttäjän kirjautumistunnus silloin.

    ![][18]

6.  **Uusi ASP.NET-projekti** -valintaikkunassa **Microsoft Azure** -osassa Varmista, että **isännöidä pilveen** on valittuna ja että **App palvelu** on valittuna avattavasta luettelosta.

    ![][19]

7. Valitse **OK**. 

8. Sinun on määritettävä Azure resurssien uusi web-sovelluksen. Noudattamalla [määrittäminen Azure resurssit uuden online](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app)-osan ohjeita. Tässä opetusohjelmassa palaa ja siirry seuraavaan vaiheeseen.

5.  Napsauta ratkaisunhallinnassa **Mallit** ja sitten **Lisää**hiiren kakkospainikkeella **luokka**. Kirjoita **nimi** -ruutuun nimi **Product.cs**. Valitse **Lisää**.

    ![][17]

### <a name="modify-the-web-application"></a>Web-sovelluksen muokkaaminen

1.  Visual Studio Product.cs-tiedostossa korvaa olemassa olevan nimitilan määrityksen seuraava koodi.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Napsauta ratkaisunhallinnassa Laajenna **ohjaimet** -kansio ja kaksoisnapsauta **HomeController.cs** -tiedoston avaaminen Visual Studio.

3. Korvaa olemassa olevan nimitilan määrityksen **HomeController.cs**seuraava koodi.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Napsauta ratkaisunhallinnassa Laajenna Views\Shared-kansio ja kaksoisnapsauta **_Layout.cshtml** Visual Studio-editorin avaaminen.

5.  Voit muuttaa **Omat ASP.NET-sovelluksen** kaikki esiintymät **LITWARE päivän**tuotteille.

6. Poista **Aloitus**, **tietoja**ja **yhteyshenkilön** linkit. Seuraavassa esimerkissä Poista korostetun koodi.

    ![][41]

7.  Napsauta ratkaisunhallinnassa Laajenna Views\Home-kansio ja kaksoisnapsauta **Index.cshtml** Visual Studio-editorin avaaminen.
    Korvaa tiedoston koko sisällön seuraava koodi.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Työn oikeellisuuden mennessä voit painaa **Ctrl + Vaihto + B** projektin luonnissa.


### <a name="run-the-app-locally"></a>Suorita sovellus paikallisesti

Voit varmistaa, että se toimii sovelluksen käyttämiseen.

1.  Varmista, että **ProductsPortal** aktiivisen projektin. Napsauta ratkaisunhallinnassa projektin nimeä hiiren kakkospainikkeella ja valitse **Määritä kuin käynnistys projekti**.
2.  Visual Studiossa painamalla F5-näppäintä.
3.  Sovelluksen pitäisi näkyä selaimessa.

    ![][21]

## <a name="put-the-pieces-together"></a>Tehdä osat

Seuraavaksi voit yhdistää ASP.NET-sovelluksen kanssa on paikallinen tuotteet-palvelin.

1.  Jos osoite ei ole jo Avaa, Visual Studiossa uudelleen Avaa **ProductsPortal** projektin loit [ASP.NET-sovelluksen luominen](#create-an-aspnet-application) -osassa.

2.  Vastaa vaiheeseen "Luominen käytössä paikallinen palvelin"-osassa Lisää NuGet paketti projektiviitteet. Napsauta ratkaisunhallinnassa **ProductsPortal** projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

3.  Etsi "Palvelun Bus" ja **Microsoft Azure palvelun Bus** kohteen valitseminen. Valitse Viimeistele asennus ja sulje valintaikkuna.

4.  Napsauta ratkaisunhallinnassa **ProductsPortal** projektin hiiren kakkospainikkeella ja valitse **Lisää**ja sitten **Olemassa olevan kohteen**.

5.  Siirry **ProductsContract.cs** tiedoston **ProductsServer** konsolin projektista. Voit korostaa ProductsContract.cs. Valitse **Lisää**-kohdan vieressä olevaa alanuolta ja valitse sitten **Lisää linkki**.

    ![][24]

6.  Nyt **HomeController.cs** -tiedoston avaaminen Visual Studio-editorin ja korvaa nimitilan määritelmän seuraava koodi. Muista korvata *yourServiceNamespace* palvelun nimitila ja *yourKey* nimellä SAS-näppäintä. Tämä ottaa käyttöön asiakkaan Soita puhelu tulos, joka palauttaa paikallisen-palveluun.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Napsauta ratkaisunhallinnassa Napsauta hiiren kakkospainikkeella **ProductsPortal** ratkaisu (Varmista, että ei ole project-ratkaisun hiiren kakkospainikkeella). Valitse **Lisää**ja valitse sitten **Olemassa olevaan projektiin**.

8.  Siirry **ProductsServer** -projekti ja valitse lisättävä se **ProductsServer.csproj** ratkaisu-tiedostoa.

9.  **ProductsServer** on oltava käynnissä, jotta voit näyttää tiedot **ProductsPortal**. Napsauta ratkaisunhallinnassa **ProductsPortal** -ratkaisun hiiren kakkospainikkeella ja valitse **Ominaisuudet**. **Ominaisuussivujen** -valintaikkuna tulee näkyviin.

10. Valitse vasemmalla puolella **Projektin aloitus**. Valitse oikealla puolella **useita käynnistys projekteja**. Varmista, että **ProductsServer** ja **ProductsPortal** näkyvät, tässä järjestyksessä, **Aloita** sekä toiminnon määrittäminen.

      ![][25]

11. **Ominaisuudet** -valintaikkunan Napsauta **Projektiriippuvuudet** vasemmassa reunassa.

12. Valitse **Projektit** -luettelosta **ProductsServer**. Varmista, että **ProductsPortal** ei **ole** valittuna.

14. Valitse **Projektit** -luettelosta **ProductsPortal**. Varmista, että **ProductsServer** on valittuna. 

    ![][26]

15. Valitse **Ominaisuus-sivut** -valintaikkunassa **OK** .

## <a name="run-the-project-locally"></a>Suorita projektin paikallisesti

Voit esikatsella sovelluksen paikallisesti-Visual Studio painamalla **F5-näppäintä**. Paikallisen palvelimen (**ProductsServer**) alkamisajan ensimmäisen ja valitse **ProductsPortal** -sovellusta kannattaa aloittaa selainikkunassa. Näet tällä hetkellä tuotteen varaston luettelo tuotteen service paikallisen järjestelmän tiedot.

![][10]

Paina **Päivitä** **ProductsPortal** -sivu. Aina, kun päivität sivun, näet sanoman näyttäminen palvelimen sovelluksen kun `GetProducts()` - **ProductsServer** kutsutaan.

Sulje molemmat sovellukset ennen seuraavaan vaiheeseen jatkamista.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Ota käyttöön ProductsPortal projekti Azure web app-sovelluksessa

Seuraavaksi voit muuntaa **ProductsPortal** frontend Azure web app-sovelluksessa. Ota ensin **ProductsPortal** -projekti, kaikki [käyttöönotto Internet-projektin Azure online](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app)-osan ohjeita noudattamalla. Kun käyttöönotto on valmis, palaa Tässä opetusohjelmassa ja siirry seuraavaan vaiheeseen.

> [AZURE.NOTE] Näkyviin voi tulla virhesanoma selainikkunassa **ProductsPortal** web-projekti käynnistyy automaattisesti käyttöönoton jälkeen. Tämä on odotettavissa ja ilmenee, koska **ProductsServer** -sovellusta ei ole vielä käynnissä.

Kopioi käyttöön web app-URL-osoite, sillä tarvitset seuraavassa vaiheessa URL-osoite. Voit hankkia tätä URL-Osoitetta myös Visual Studiossa Azure App aktiviteetin-ikkunassa:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Määritä ProductsPortal kuin web app

Ennen kuin suoritat sovelluksen pilveen, voit varmistaa, että **ProductsPortal** on ilmennyt Visual Studion kuin verkkosovellukseen.

1. Visual Studion **ProjectsPortal** projektin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.

3. Valitse vasemmanpuoleisessa sarakkeessa **Web**.

5. **Käynnistä toiminto** -kohdassa **URL-Osoitteen Käynnistä** -painiketta ja kirjoita tekstiruutuun URL-osoite aiemmin käyttöön verkkosovelluksessa; esimerkiksi `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Valitse Visual Studio **Tiedosto** -valikosta **Tallenna kaikki**.

7. Napsauta **Uudelleen ratkaisun**Visual Studiossa muodosta-valikossa.

## <a name="run-the-application"></a>Suorita sovellus

2.  Painamalla F5 voivat laatia ja suorita sovellus. Paikallisen palvelimen ( **ProductsServer** console-sovellus) alkamisajan ensimmäisen ja valitse sitten **ProductsPortal** -sovellus tulee Aloita selainikkunassa, koodin seuraavassa kuvassa esitetyllä tavalla. Huomaa uudelleen tuotteen varaston luettelo tuotteen service paikallisen järjestelmän tiedot ja tuo ne web App-sovelluksessa. Tarkista, varmista, että **ProductsPortal** suoritetaan pilvipalvelussa, kuin Azure web app-URL-osoite. 

    ![][1]

    > [AZURE.IMPORTANT] **ProductsServer** console-sovelluksen on oltava käynnissä ja voivat virransaanti **ProductsPortal** -sovellukseen. Jos selain näyttää virheen, odota muutama **ProductsServer** lataamisen ja näyttää seuraavan sanoman useita sekunteja. Paina **päivittäminen** selaimessa.

    ![][37]

3. Takaisin selaimessa paina **ProductsPortal** sivun **päivittäminen** . Aina, kun päivität sivun, näet sanoman näyttäminen palvelimen sovelluksen kun `GetProducts()` - **ProductsServer** kutsutaan.

    ![][38]

## <a name="next-steps"></a>Seuraavat vaiheet  

Lisätietoja palvelun Bus on seuraavissa resursseissa:  

* [Azure palvelun Bus][sbwacom]  
* [Opi käyttämään palvelua Bus olevien][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Työkalut ja SDK-paketissa]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

