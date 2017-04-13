<properties
    pageTitle="Palvelun Bus muiden opetusohjelman käyttämällä välittäminen messaging | Microsoft Azure"
    description="Luoda yksinkertaisen palvelun Bus välitys Host (isäntä)-sovelluksen, joka paljastaa REST-pohjaisen käyttöliittymän."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Palvelun Bus välitys REST-opetusohjelma

Tässä opetusohjelmassa kuvataan, miten voit luoda yksinkertaisen palvelun Bus isäntäsovellus, joka paljastaa REST-pohjaisen käyttöliittymän. MUUT mahdollistaa web asiakkaan-web-selaimessa, kuten palvelun Bus-ohjelmointirajapinnan käyttäminen pyyntöjen.

Tässä opetusohjelmassa käyttää Windows Communication Foundation (WCF) muiden ohjelmointi mallin muodostaa REST-palvelun käyttöön palvelun Bus. Katso lisätietoja, [WCF muiden ohjelmointi malli](https://msdn.microsoft.com/library/bb412169.aspx) ja [suunnittelemisesta ja toteuttaminen palvelujen](https://msdn.microsoft.com/library/ms729746.aspx) WCF-ohjeessa.

## <a name="step-1-create-a-service-namespace"></a>Vaihe 1: Luo palvelun nimitila

Ensimmäiseksi on, voit luoda nimitilan ja jakaa Access allekirjoitus (SAS) Key-tunnuksen. Nimitilan on sovelluksen rajan kunkin sovelluksen tarjoamia palvelun Bus kautta. Suojaussidosten avaimen luodaan automaattisesti järjestelmä palvelun nimitila luomisen yhteydessä. Palvelun nimitila ja Suojaussidosten avaimen yhdistelmä on tunnistetietojen palvelun Bus tarkistamiseen access-sovellukseen.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Vaihe 2: Määritä REST-pohjainen WCF sopimus palvelun Bus käyttäminen

Kuin muiden Service Bus-palvelujen kanssa luodessasi REST-tyyppiseksi palvelun, sinun on määritettävä sopimuksen. Sopimuksen määrittää, mitä toimintoja isäntä tukee. Service-toimintoa Voit ajatella WWW-palvelun-menetelmää. Sopimuksia luodaan määrittämällä C++, C# tai Visual Basic-käyttöliittymän. Kummassakin menetelmässä käyttöliittymässä vastaa tietyn palvelun-toiminto. [ServiceContractAttribute-kohdetta](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) -määrite on otettava käyttöön kunkin käyttöliittymän ja [ServiceOperationInfo-kohteessa määritetystä](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) -määrite on otettava käyttöön kunkin toiminnon. Jos menetelmän käyttöliittymä, jossa on [ServiceContractAttribute-kohdetta](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) ei ole [ServiceOperationInfo-kohteessa määritetystä](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), tämä menetelmä ei näkyvät. Näiden tehtävien käytettävä koodi näkyy esimerkin ohjeiden mukaisesti.

Tärkein basic palvelun Bus-sopimus ja REST-tyyppiseksi sopimuksen välinen ero on [ServiceOperationInfo-kohteessa määritetystä](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)ominaisuus: [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Tämän ominaisuuden avulla voit yhdistää käyttöliittymän menetelmän menetelmän liittymän muiden reunassa. Tässä tapauksessa Käytämme HTTP GET menetelmän linkittäminen [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) . Näin palvelun Bus tarkasti noutaa ja tulkita liittymään lähetetyt komennot.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Voit luoda liittymällä Service Bus-palvelusopimus

1. Avaa Visual Studio järjestelmänvalvojana: Napsauta hiiren kakkospainikkeella **Käynnistä** -valikon ohjelma ja valitse sitten **Suorita järjestelmänvalvojana**.

2. Luo uusi konsoli sovelluksen projekti. Valitse **Tiedosto** -valikosta ja valitse **Uusi**ja valitse **projektin**. **Uusi projekti** -valintaikkunassa valitsemalla **Visual C#**, valitse **Console-sovelluksen** malli ja anna sille nimi **ImageListener**. Käytä oletusarvoista **sijainti**. Valitse **OK** , jos haluat luoda projektin.

3. C# projektin Visual Studio Luo `Program.cs` tiedosto. Tähän luokkaan sisältää tyhjän `Main()` -menetelmä pakollinen konsolin sovelluksen projektin luonnissa oikein.

4. Lisää viitteet palvelun Bus ja **System.ServiceModel.dll** projektin asentamalla palvelun Bus NuGet-paketti. Tämän paketin lisää automaattisesti palvelun Bus kirjastoista sekä WCF **System.ServiceModel**viittauksia. Napsauta ratkaisunhallinnassa **ImageListener** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**. **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Valitse **Asenna**ja hyväksy käyttöehdot.

5. Sinun on lisättävä **System.ServiceModel.Web.dll** viittaa erikseen projektin:

    a. Napsauta ratkaisunhallinnassa Napsauta project-kansion **viittaukset** -kansiota hiiren kakkospainikkeella ja valitse sitten **Lisää viittaus**.

    b. Valitse **Lisää viittaus** -valintaikkuna **Framework** -välilehden vasemmalla puolella ja **hakuruutuun** , kirjoita **System.ServiceModel.Web**. Valitse **System.ServiceModel.Web** -valintaruutu ja valitse sitten **OK**.

6. Lisää seuraava `using` lauseet Program.cs tiedoston yläosassa.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) on nimitilan, joka mahdollistaa WCF perusominaisuudet ohjelmallisesti käytön. Palvelun Bus määrittää monia objektien ja määritteiden WCF avulla palvelun sopimuksia. Voit käyttää tätä nimitilaa palvelun Bus välitys-sovelluksia useimmissa. Vastaavasti [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) avulla määrittää kanavan, jossa on objekti, jonka kautta, jonka kanssa viestit palvelun Bus ja asiakas-selain. Lopuksi [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) sisältää tiedostotyypit, joiden avulla voit luoda web-sovelluksissa.

7. Nimeä uudelleen `ImageListener` nimitilan **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Suoraan jälkeen avaava aaltosulje nimitilan-ilmoituksen määrittäminen uuden käyttöliittymän nimeltä **IImageContract** ja käytä **ServiceContractAttribute-kohdetta** -määritteen arvo-liittymällä `http://samples.microsoft.com/ServiceModel/Relay/`. Nimitilan poikkeaa nimitilan, jota käytetään koko koodi aluetta. Nimitilan arvo käytetään yksilöllinen tämä sopimus ja pitäisi olla versiotiedot. Lisätietoja on artikkelissa [Palvelun versiotiedot](http://go.microsoft.com/fwlink/?LinkID=180498). Nimitilan, joka määrittää erikseen estää nimitilan oletusarvo lisätään sopimuksen nimeä.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Sisällä `IImageContract` liittymän, määritellä laskentatapaa yhdellä kertaa `IImageContract` sopimuksen paljastaa käyttöliittymässä ja käytä `OperationContractAttribute` määrite menetelmä, jonka haluat näyttää julkisen palvelun Bus sopimuksen osana.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Lisää **OperationContract** -määritteen **WebGet** arvo.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Tällöin mahdollistaa palvelun Bus reitin HTTP GET pyynnöt `GetImage`, ja kääntää, palautusarvojen `GetImage` HTTP GETRESPONSE-vastaukseen. Myöhemmin opetusohjelman, voit käyttää selaimen voivat käyttää tätä tapaa ja näyttää kuvan selaimessa.

11. Suoraan jälkeen `IImageContract` määritys on kanavalla, molemmat perii määritellä `IImageContract` ja `IClientChannel` liittymät.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanavan on WCF-objekti, jonka kautta palvelu ja asiakkaan välittää tiedot toisiinsa. Luo myöhemmin kanavan Host (isäntä)-sovelluksessa. Palvelun Bus käyttää kanavan HTTP GET-pyyntöjen siirtää **GetImage** käyttöympäristösi selaimessa. Palvelun Bus käyttää myös kanavan **GetImage** palautusarvon ja kääntäminen HTTP-GETRESPONSE varten selaimeen.

12. Valitse **Muodosta** -valikossa **Luominen ratkaisu** vahvistat työsi tähän mennessä.

### <a name="example"></a>Esimerkki

Seuraava koodi näkyy basic käyttöliittymä, joka määrittää Service Bus-palvelusopimus.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Vaihe 3: Käytä palvelu Bus käyttämään REST-pohjainen WCF palvelusopimusta

REST-tyyppiseksi palvelun Bus palvelun luominen edellyttää, että luot ensin sopimuksen, jonka avulla määritetään. Seuraavaksi toteuta liittymää. Tämä koskee nimeltä **ImageService** , joka sisältää käyttäjän määrittämät **IImageContract** käyttöliittymän luokan luominen. Kun otat sopimuksen, määrität sitten käyttöliittymän App.config-tiedoston avulla. Määritystiedosto sisältää tarvittavat tiedot sovelluksen, kuten palvelun nimi ja sopimus ja protokolla, jota käytetään pitää yhteyttä palvelun Bus nimiä. Näiden tehtävien käytettävä koodi on annettu esimerkin ohjeiden mukaisesti.

Edellä kuvatut vaiheet, jossa on vain vähän ero REST-tyyli-sopimus ja basic Service Bus-palvelusopimus.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Toteuttamisesta REST-tyyppiseksi palvelun Bus-palvelusopimus

1. Luo uusi luokka nimeltä **ImageService** suoraan **IImageContract** -liittymän määritelmään jälkeen. **ImageService** luokan toteuttaa **IImageContract** -liittymän.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Käyttöliittymän muiden käyttöotot tapaan määritelmä voit toteuttaa toiseen. Kuitenkin tässä opetusohjelmassa käyttöönoton näkyy liitännän määrityksen samaa tiedostoa ja `Main()` menetelmää.

2. Käytä [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) -määritteen osoittaa, että luokka on toteutus WCF-sopimus **IImageService** -luokka.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Kuten edellä mainittiin, tämä nimitila ei ole perinteinen nimitila. Sen sijaan se on osa, joka ilmaisee sopimuksen WCF-arkkitehtuuri. Saat lisätietoja WCF asiakirjoissa ohjeaiheessa [Tietojen palvelusopimuksen nimiä](https://msdn.microsoft.com/library/ms731045.aspx) .

3. .Jpg-kuvan lisääminen projektiin.  

    Tämä on kuva, joka palvelun näyttää vastaanottamisessa selaimessa. Projektin hiiren kakkospainikkeella ja valitse sitten **Lisää**. Valitse **Olemassa olevan kohteen**. Etsi haluamasi .jpg **Lisää aiemmin luotu kohde** -valintaikkunan avulla ja valitse sitten **Lisää**.

    Kun lisäät tiedoston, varmista, että **Kaikki tiedostot** on valittuna avattavan luettelon vieressä **tiedostonimi:** kentän. Jäljempänä tässä opetusohjelmassa oletetaan, että kuva on "kuva.jpg". Jos sinulla on jokin muu tiedosto, sinun on kuvan nimetä uudelleen tai muuttaa koodin hyvittää.

4. Voit varmistaa, että käynnissä palvelun löytää kuvatiedosto, napsauta **Ratkaisunhallinnassa** kuvatiedostoa hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**. Määritä **Ominaisuudet** -ruudussa **Kopioi tulosteen kansioon** kopioitavat **Jos uudempaan**.

5. Lisää **System.Drawing.dll** kokoonpanon viittaus projektiin ja lisää myös seuraavia liittyvät `using` lauseita.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Lisää seuraavat konstruktoria, joka lataa bittikartta ja lähetä se selaimeen valmistelee **ImageService** -luokka.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Suoraan edellisen koodin jälkeen Lisää palauttaa HTTP-sanoman, joka sisältää kuvan **ImageService** -luokan **GetImage** seuraavasti.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Tämä toteutus käyttää **MemoryStream** noutaa kuvan ja valmisteleminen lähettäväksi toistamisen selaimessa. Se käynnistyy nollasta virta-sijainti, ilmoittaa jpeg-muodossa-sisällön ja virtauttaa tiedot.

8. Napsauta **Muodosta** -valikosta **Muodosta ratkaisu**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Voit määrittää WWW-palvelun käytössä palvelun Bus määrityskohde

1. Kaksoisnapsauta **Ratkaisunhallinnassa** **App.config** Visual Studio-editorin avaaminen.

    **App.config** -tiedostoa muistuttaa WCF-kokoonpanotiedosto ja sisältää palvelunimi, päätepisteen (eli palvelun Bus paljastaa asiakkaat ja isännät keskenään sijainti) ja sidonta (protokolla, jota käytetään vaihtamaan tyyppi). Tärkein ero on määritetty Palvelupäätepisteen viittaa [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) sidonta, joka ei kuulu .NET Framework.

2. `<system.serviceModel>` XML-elementti on WCF-elementin, joka määrittää vähintään yhden palvelut. Tässä sitä käytetään palvelunimi ja päätepisteen määrittämiseen. Alareunassa `<system.serviceModel>` elementin (mutta edelleen toimintoa `<system.serviceModel>`), Lisää `<bindings>` elementti, joka sisältää seuraavan sisällön. Tämä määrittää käytössä sovelluksessa sidontojen. Voit määrittää useita sidontojen, mutta opetusohjelmassa määrität vain yhden.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Tässä vaiheessa määrittää palvelun Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) sidonta kanssa **relayClientAuthenticationType** asetuksena on **ei mitään**. Tämä asetus ilmaisee päätepisteen avulla tämän sidonnan ei edellytä asiakkaan tunnistetieto.

3. Sen jälkeen `<bindings>` elementti, Lisää `<services>` elementti. Kaltaisilta sidontojen palveluihin voit määrittää yksittäisen määritystiedostossa. Kuitenkin tässä opetusohjelmassa määritetään vain yksi.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Tässä vaiheessa määrittää palvelu, joka käyttää aiemmin määritettyä oletusarvon **webHttpRelayBinding**. Tiedostossa käytetään myös oletusarvoinen **sbTokenProvider**, joka on määritetty seuraavaan vaiheeseen.

4. Sen jälkeen `<services>` elementti, luominen `<behaviors>` elementin seuraavat sisällöllä "SAS_KEY" tilalle saatu [Azure portal][]aiemmin *Jaettu Access allekirjoitus* (SAS)-näppäintä.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Edelleen-App.config, valitse `<appSettings>` elementti, korvaa koko yhteyden merkkijonoarvo aiemmin portaalin saadut yhteysmerkkijonon kanssa. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Valitse **Muodosta** -valikossa **Luominen ratkaisu** Rakenna koko ratkaisu.

### <a name="example"></a>Esimerkki

Seuraava koodi näkyy sopimus ja palvelun käyttöönoton REST-palvelun, joka suoritetaan palvelun Bus **WebHttpRelayBinding** sidontaa käyttämällä.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Seuraavassa esimerkissä esitetään-palveluun liitetyn App.config-tiedostoa.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Vaihe 4: Isännöidä käyttämään palvelua Bus REST-pohjainen WCF-palvelu

Tässä vaiheessa käsitellään suorittamalla console-sovelluksen käyttäminen palvelun Bus verkkopalvelun. Tässä vaiheessa kirjoitetun koodin täydellinen luettelo on annettu esimerkin ohjeiden mukaisesti.

### <a name="to-create-a-base-address-for-the-service"></a>Perusosoitteen palvelun luominen

1. Valitse `Main()` funktio ilmoitus, luo muuttujan tallentamiseen palvelun Bus projektin nimitilan. Varmista, että korvaa `yourNamespace` aiemmin luotu palvelun nimitilan nimi.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Palvelun Bus käytetään luomaan yksilöllinen URI oman nimitilan nimi.

2. Luo `Uri` palvelu, joka perustuu nimitilan perusosoitteen esiintymä.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Voit luoda ja määrittää WWW-palvelun host

- Luo web palvelun host URI osoitteella aiemmin luotu sisältö.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Service-isäntä WCF-objekti, joka muodostaa isäntäsovellus. Tässä esimerkissä välittää host luotavan ( **ImageService**) ja osoite, jossa haluat näyttää isäntäsovellus.

### <a name="to-run-the-web-service-host"></a>Jos haluat suorittaa web-palvelu isäntään

1. Avaa palvelu.

    ```
    host.Open();
    ```
    Palvelu on nyt käynnissä.

2. Näyttää sanoman, että palvelu on käynnissä ja kuinka voit lopettaa palvelun.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Kun olet valmis, sulje palvelun isäntä.

    ```
    host.Close();
    ```

## <a name="example"></a>Esimerkki

Seuraavassa esimerkissä sisältää sopimus ja käyttöönoton aiemmissa vaiheissa opetusohjelman ja isännöi palvelua console-sovelluksessa. Käännä seuraava koodi suoritettavan nimeltä ImageListener.exe kyselyjä.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Koodin käännös

Jälkeen rakentaminen ratkaisu sovelluksen käyttämiseen seuraavasti:

1. Painamalla **F5-näppäintä**tai selaamalla suoritettavan tiedostosijainti (ImageListener\bin\Debug\ImageListener.exe)-palvelun suorittamiseen. Ottaa sovelluksen käytössä, kun se tarvitaan suorittaa seuraavan vaiheen.

2. Kopioi ja liitä osoite komentokehotteen selaimessa, voit tarkastella kuvaa.

3. Kun olet valmis, paina **Enter** komentokehote-ikkunassa sovelluksen sulkeutumaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet aiemmin luonut palvelun Bus välityspalvelu käyttävä sovellus, katso lisätietoja välittäminen viestintä on seuraavissa artikkeleissa:

- [Azure palvelun Bus arkkitehtuuri yleiskatsaus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Opi käyttämään Bus välitys palvelun](service-bus-dotnet-how-to-use-relay.md)

[Azure portal]: https://portal.azure.com