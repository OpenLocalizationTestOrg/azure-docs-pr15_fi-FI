<properties
   pageTitle="Luo WWW-edusta, käyttämällä ASP.NET Core sovelluksen | Microsoft Azure"
   description="Näytä Web-palvelu kangasta sovelluksen ASP.NET Core verkko-Ohjelmointirajapinnan projektin ja ServiceProxy väliset palvelun viestintä."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Web palvelun eturivin käyttämällä ASP.NET Core sovelluksen luominen

Oletusarvon mukaan Azure palvelun kangasta palvelut eivät tarjoa julkinen Internet-käyttöliittymä. Voit näyttää sovelluksen toimintoja HTTP-asiakkaille, sinun on web-projekti, jonka edustajana aloituskohtaa ja sitten toimittaa sieltä yksittäisten palvelujen luomiseen.

Tässä opetusohjelmassa on kohtaa, johon on viimeksi jäit [luominen Visual Studiossa ensimmäisen sovelluksen](service-fabric-create-your-first-application-in-visual-studio.md) opetusohjelmassa ja lisää verkkopalvelun tilallisten laskuri-palvelun eteen. Jos et ole vielä tehnyt voit siirtyä takaisin ja askel kyseisen opetusohjelma ensin.

## <a name="add-an-aspnet-core-service-to-your-application"></a>ASP.NET-Core-palvelun lisääminen sovellukseen

ASP.NET-Core on kevyt, Office kaikissa ympäristöissä web kehittäminen kehys, joiden avulla voit luoda Moderni sivuston Käyttöliittymä ja verkko-ohjelmointirajapinnan. ASP.NET-verkko-Ohjelmointirajapinnan projektin lisääminen japanin aiemmin-sovellusta.

>[AZURE.NOTE] Viimeistele Tässä opetusohjelmassa, sinun on [asennettava .NET Core 1.0][dotnetcore-install].

1. Kakkospainikkeella **palveluiden** sovelluksen projektin ratkaisunhallinnassa, ja valitse **Lisää > uusi palvelu kangasta**.

    ![Uusi palvelu lisääminen sovellukseen][vs-add-new-service]

2. Valitse **Luo palvelu** -sivulla **ASP.NET Core** ja anna sille nimi.

    ![ASP.NET web-palvelu valitseminen uusi palvelu-valintaikkuna][vs-new-service-dialog]

3. Seuraavalla sivulla on joukko ASP.NET Core projektimallit. Huomaa, että ne ovat saman mallit, joka näkyy Jos olet luonut ASP.NET Core-projektin palvelun kangasta-sovelluksen ulkopuolella. Tässä opetusohjelmassa on valita **Verkko-Ohjelmointirajapinnan**. Voit kuitenkin käyttää samaa menetelmää rakentamiseen koko web-sovelluksen.

    ![ASP.NET-projektityypin valitseminen][vs-new-aspnet-project-dialog]

    Kun verkko-Ohjelmointirajapinnan projektin on luotu, sovelluksessa on kaksi palvelut. Kun jatkat sovelluksen luomiseen, Lisää Lisää palveluja täsmälleen samalla tavalla. Jokainen voi olla itsenäisesti versiotietoja ja päivitetty.

>[AZURE.TIP] Lue lisätietoja luomisesta ASP.NET Core services on [ASP.NET Core ohjeissa](https://docs.asp.net).

## <a name="run-the-application"></a>Suorita sovellus

Oletetaan, että voit käsityksen siitä, että olet valmis, uuden sovelluksen käyttöönotto ja tutustumme oletustoiminnon, joka sisältää ASP.NET Core verkko-Ohjelmointirajapinnan-malli.

1. Painamalla F5 Visual Studiossa korjaamisessa sovellus.

2. Käyttöönoton päätyttyä Visual Studio Käynnistä ASP.NET-verkko-Ohjelmointirajapinnan-palvelun--julkaiseminen http://localhost:33003 kuten ylimmällä selaimen. Porttinumero satunnaisesti määritetään ja hän voi olla eri tietokoneessa. ASP.NET Core verkko-Ohjelmointirajapinnan-malli ei tarjoa oletustoiminnan pääkansio, niin näet virheen selaimessa.

3. Lisää `/api/values` selaimessa haluamaasi kohtaan. Tämä käynnistää `Get` verkko-Ohjelmointirajapinnan mallin ValuesController menetelmää. Palauttaa Oletusvastaus, joka tarjoaa mallin--JSON-taulukko, joka sisältää kaksi merkkijonoa:

    ![Palauttaa ASP.NET Core verkko-Ohjelmointirajapinnan mallista oletusarvot][browser-aspnet-template-values]

    Opetusohjelman lopussa on korvanneet oletusarvoja viimeisimmän laskuri-arvon Microsoftin tilallisten-palvelusta.


## <a name="connect-the-services"></a>Yhdistä palvelut

Palvelun kangasta joustavasti valmis kuinka luotettava palvelujen yhteydessä. Yhden sovelluksen voit joutua palvelut, jotka ovat käytettävissä kautta TCP, muut palvelut, jotka ovat käytettävissä HTTP REST-Ohjelmointirajapinnalla kautta tai edelleen muista palveluista, jotka ovat käytettävissä web sockets kautta. Saat käytettävissä olevat vaihtoehdot ja yhdistävää kompromissien tausta- [Communicating-palvelujen kanssa](service-fabric-connect-and-communicate-with-services.md). Tässä opetusohjelmassa on seuraa yksinkertaisempi tavoilla ja käytä `ServiceProxy` / `ServiceRemotingListener` luokat, jotka ovat SDK: ssa.

- `ServiceProxy` Vaihtoehto (mallintaa etäproseduurikutsu puhelut tai RPC-kutsujen), voit määrittää rajapinta-palvelun julkisen sopimuksen edustajana. Valitse kyseisen liittymän käyttää luomiseen käyttäminen palvelun välityspalvelimen luokka.


### <a name="create-the-interface"></a>Luo käyttöliittymä

Olemme alkaa luomalla käyttöliittymän tilallisten palvelu ja sen asiakkaiden, mukaan lukien ASP.NET Core projektin sopimuksen edustajana.

1. Napsauta ratkaisunhallinnassa-ratkaisu hiiren kakkospainikkeella ja valitse **Lisää** > **Uusi projekti**.

2. Valitse vasemmassa siirtymisruudussa **Visual C#** -tapahtuma ja valitse sitten **Luokkakirjasto** -malli. Varmista, että .NET Framework-versio on määritetty **4.5.2**.

    ![Käyttöliittymän projektin tilallisten palvelun luominen][vs-add-class-library-project]

3. Käyttöliittymään on käytettävissä myös `ServiceProxy`, sen on oltava johdettu IService käyttöliittymästä. Tämän liittymän sisältyy jotakin palvelun kangasta NuGet. Lisää paketti, uuden luokan kirjaston projektin hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

4. Etsi **Microsoft.ServiceFabric.Services** -paketti ja asenna se.

    ![Lisäämällä palveluja NuGet-paketti][vs-services-nuget-package]

5. Luo käyttöliittymään luokkakirjasto, yksi tapa `GetCountAsync`, ja käyttöliittymän ulottuvat IService.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Toteuta liittymää tilallisten-palvelussa

Nyt kun liittymän määritimme annettava Toteuta tilallisten-palvelussa.

1. Lisää luokan kirjasto-projekti, joka sisältää liittymän viittaa tilallisten-palvelussa.

    ![Lisäämällä luokan kirjaston projektin viittaa tilallisten-palvelussa][vs-add-class-library-reference]

2. Etsi luokan, jotka perivät `StatefulService`, kuten `MyStatefulService`, ja laajentaa toteuttamisesta `ICounter` käyttöliittymän.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Toteuta nyt yksi tapa, joka on määritetty `ICounter` -käyttöliittymä, `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Näyttää tilallisten palvelun Etäpalvelujen kuuntelija-palveluun

Ja `ICounter` käyttöliittymän toteutettu, viimeinen vaihe on kutsuttava muiden palveluiden tilallisten palvelun ottaminen käyttöön on avata viestintä kanavan. Tilallisten services-palvelun kangasta tarjoaa korvattavaa menetelmää kutsutaan `CreateServiceReplicaListeners`. Tätä menetelmää voit määrittää yhden tai usean tyyppisiä, jotka haluat ottaa käyttöön palvelun perusteella viestintä kuuntelijoita.

>[AZURE.NOTE] Tietoliikenneyhteyden tilattomien palveluihin avaamista vastaavaa menetelmää kutsutaan `CreateServiceInstanceListeners`.

Tässä tapauksessa emme korvaa olemassa olevat `CreateServiceReplicaListeners` menetelmä ja anna esiintymän `ServiceRemotingListener`, joka luo RPC päätepisteen, joka on kutsuttava asiakkaiden kautta `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>ServiceProxy-luokan avulla voit käsitellä palvelun

Tilallisten palveluun on nyt valmis vastaanottamaan tietoliikennettä muista palveluista. Kaikki niin, että pysyy lisätään koodin pitää yhteyttä se ASP.NET web-palvelusta.

1. ASP.NET-projektin Lisää luokan kirjastoon, joka sisältää viite `ICounter` käyttöliittymän.

2. **Luo** -valikosta Avaa **Määritysten hallinta**. Näyttöön tulee seuraavankaltaiselta:

    ![Määritysten hallinnan näyttäminen luokkakirjasto AnyCPU nimellä][vs-configuration-manager]

    Huomaa, että luokan kirjaston projektin **MyStatefulService.Interface**on määritetty luonnissa tahansa suorittimen. Jos haluat käsitellä palvelun kangasta oikein, se on erikseen suunnataan x64. Ympäristö-valikko ja valitsemalla **Uusi**ja valitse Luo x64 ympäristön määritys.

    ![Luodaan uusi ympäristön luokkakirjasto][vs-create-platform]

3. Lisää Microsoft.ServiceFabric.Services paketti ASP.NET-projektin samalla tavalla kuin luokan kirjaston projektin aiemmin. Tällöin `ServiceProxy` luokka.

4. Avaa **ohjaimet** -kansiossa `ValuesController` luokka. Huomaa, että `Get` menetelmä palauttaa tällä hetkellä vain-koodatun merkkijonon matriisin "arvo1" ja "arvo2"--joka vastaa Microsoft tuli aiemmin selaimessa. Korvaa tämän toteutuksen seuraava koodi:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Ensimmäisen rivin koodi näkyy avaimen. Luo ICounter välityspalvelimen tilallisten-palveluun, sinun täytyy antaa kaksi kappaletta tietojen: osion tunnuksen ja palvelun nimi.

    Voit käyttää jakaminen asteikko tilallisten palveluihin jakamalla niiden tilaan eri uudelleenkäytettävät kyselyjä, avainta, joka määrittää, kuten Asiakastunnus- tai postinumeron mukaan. Microsoftin trivial sovelluksen tilallisten palvelu on vain yksi osio, joten avain ei ole väliä. Avainta, joka saadaan johtaa samaan osioon. Lisätietoja jakaminen services, katso, [miten palvelun kangasta luotettava palvelut-osioon](service-fabric-concepts-partitioning.md).

    Palvelunimi ei, lomakkeen kangasta URI: /&lt;sovelluksen_nimi&gt;/&lt;palvelun nimi&gt;.

    Ja nämä kaksi tietojen palvelun kangasta yksilöivät tietokoneen, joka lähetetään pyynnöt. `ServiceProxy` Luokan käsittelee saumattomasti myös silloin, kun tilallisten service-osion isännöivän tietokoneen epäonnistuu ja toiseen tietokoneeseen on ylennetyt tulevat sijaintia. Tämä otetaan on merkittävästi yksinkertaisempi muiden palvelujen käsitellä asiakas-koodin kirjoittamista.

    Kun olemme välityspalvelimen, emme riittää, että ongelma `GetCountAsync` menetelmää ja palauttaa tuloksen.

5. Painamalla F5 uudelleen muokattu sovelluksen käyttämiseen. Nimellä, Visual Studio käynnistää automaattisesti web-projekti ylimmällä selaimen. Lisää "api/arvojen-polku ja näkyviin tulee nykyinen arvo laskuri-funktio palauttaa.

    ![Tilallisten laskuri-arvo näkyy selaimessa][browser-aspnet-counter-value]

    Päivitä säännöllisesti, jos haluat nähdä laskuri-arvo, Päivitä selain.


>[AZURE.WARNING] Mallin nimeltään Kestrel, säädetty ASP.NET Core verkkopalvelin on [käsittelyyn suoraan internet-liikenne ei tällä hetkellä tueta](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Tuotannon tilanteessa, harkitse isännöinnin ASP.NET Core-päätepisteet takana [API hallinta] [ api-management-landing-page] tai toisen Internetiin yhteydessä oleva yhdyskäytävän. Huomaa, että palvelun kangasta ei tueta IIS: n käyttöönottoa varten.


## <a name="what-about-actors"></a>Mihin ulkopuoliset toimijat?

Tässä opetusohjelmassa kohdistettu lisääminen web edusta, tilallisten palvelun kanssa. Voit kuitenkin noudattaa muistuttaa mallin toimijoiden jutella. Itse asiassa on melko helppoa.

Kun luot toimija projektin, Visual Studio Luo käyttöliittymän projektin automaattisesti. Kyseisen liittymän avulla voit luoda toimija-välityspalvelimen pitää yhteyttä toimija web-projekti. Viestintä-kanavan annetaan automaattisesti. Jotta sinun ei tarvitse tehdä mitään, joka vastaa vahvistamiseksi `ServiceRemotingListener` kuin kirjoitit tilallisten palvelun Tässä opetusohjelmassa.

## <a name="how-web-services-work-on-your-local-cluster"></a>Verkkopalvelut toiminnasta paikallisen klusterin

Voit yleisesti täsmälleen usean kuormitusryhmälle klusteriin, joka paikallisen klusterin otettu käyttöön sama palvelun kangasta sovelluksen käyttöönotto ja olla erittäin varma, että se toimii haluamallasi tavalla. Tämä johtuu siitä paikallisen klusterin yksinkertaisesti viisi solmun kokoonpanossa, jossa on tiivistetty yhteen tietokoneeseen.

Jos Internet-palveluihin, on kuitenkin yksi tärkeimmistä nuance. Kun yhteyttä klusterin takana kuormituksen, kuin se tekee Azure, sinun on varmistettava, että WWW-palvelut on otettu käyttöön jokaiseen tietokoneeseen jälkeen kuormituksen on yksinkertaisesti pyöreän pöydän liikenne tietokoneissa. Voit tehdä tämän määrittämällä `InstanceCount` palvelun määräten arvoksi "1".

Sen sijaan, kun olet suorittanut verkkopalvelun paikallisesti, sinun on varmistettava, että vain yhden esiintymän palvelu on käynnissä. Muussa tapauksessa kohtaat ristiriidat-useita prosesseja, jotka kuuntelevat saman polku ja portin. Tuloksena "1" paikallisen käyttöönotoissa on asetettava web-palvelun esiintymän määrä.

Lisätietoja eri arvot toiseen ympäristöön määrittämisestä on artikkelissa [hallinta sovelluksen parametrit useita ympäristössä](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Klusterin luominen Azure pilveen sovelluksen käyttöönotto](service-fabric-cluster-creation-via-portal.md)
- [Lisätietoja palvelujen yhteydessä](service-fabric-connect-and-communicate-with-services.md)
- [Lisätietoja jakaminen tilallisten palvelut](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
