<properties
    pageTitle="Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla | Microsoft Azure"
    description="Azure Active Directory automaattisesti voit valmistella käyttäjät ja ryhmät, verkkopalvelun fronted liittymällä, jotka on määritelty SCIM protokolla-tiedot sovelluksen tai käyttäjätiedot säilöön"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Ota käyttöön automaattinen valmisteleminen käyttäjien ja ryhmien Azure Active Directorysta sovellusten SCIM avulla

##<a name="overview"></a>Yleiskatsaus

Azure Active Directory voit valmistella automaattisesti käyttäjät ja ryhmät, verkkopalvelun fronted liittymällä, jotka on määritelty [SCIM 2.0-protokollan määrityksen](https://tools.ietf.org/html/draft-ietf-scim-api-19)sovelluksen tai käyttäjätiedot säilöön. Azure Active Directory lähettää luominen, muokkaaminen ja poistaminen määritetty käyttäjille ja ryhmille verkkopalvelun, jonka voit sitten kääntää nämä pyynnöt toimintojen yhteydessä kohde identity-kaupan pyynnöt. 

![][1]
*Kuva: Valmistelu Azure Active Directorysta identity-kaupan kautta Web-palvelu*

Tätä ominaisuutta voidaan yhdessä "[tuoda Omat sovellus](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)"-ominaisuuksien kanssa Azure AD-ottaa käyttöön kertakirjautumisen ja automaattinen käyttäjän valmistelu sovelluksissa, jotka on tai SCIM web-palvelu fronted.

Azure Active Directoryn SCIM Käytä tapauksista on:

* **Käyttäjät ja ryhmät-sovellukset tukevat SCIM valmistelu** - sovellukset, jotka tukevat SCIM 2.0 ja käyttää OAuth haltijan tunnusten todentamiseen toimii Azure AD-ruudun.

* **Luoda omia sovelluksia, jotka tukevat muiden API-pohjaisen valmistelu valmistelu ratkaisu** --SCIM sovelluksia, voit luoda SCIM-päätepisteen käännettävä Azure AD SCIM päätepisteen ja välillä riippumatta siitä, mitä API sovellus tukee käyttäjän valmisteluun.  Helpota SCIM päätepisteen kehittämistä, annamme CLI kirjastoihin sekä MALLIKOODEJA, kuinka voit antaa SCIM päätepiste ja kääntää SCIM viestejä.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Käyttäjät ja ryhmät-sovelluksia, jotka tukevat SCIM valmistelu

Azure Active Directory voi määrittää automaattisesti määritetty käyttäjien ja ryhmien sovelluksia, jotka toteuttavat [toimialueelta jäsenyyksien hallinta 2 (SCIM)-järjestelmän](https://tools.ietf.org/html/draft-ietf-scim-api-19) sivuston palvelun ja hyväksy OAuth haltijan-tunnukset todennusta varten. SCIM 2.0-määrityksen sisällä sovellukset on täytettävä nämä vaatimukset:

* Tukee luomista käyttäjät ja ryhmät-osassa 3.3 SCIM-protokollan mukaan.  

* Tukee käyttäjien tai ryhmien korjaustiedoston pyynnöt, kuten SCIM-protokollan 3.5.2 olevassa muokkaaminen.  

* Tukee tunnetut resurssi, kuten SCIM-protokollan 3.4.1 olevassa noudetaan.  

*  Tukee kyselyt käyttäjät ja ryhmät-osassa 3.4.2 SCIM-protokollan mukaan.  Oletusarvon mukaan käyttäjiltä pyydetään externalId mukaan ja ryhmien pyydetään mukaan näyttönimi.  

* Tukee käyttäjän tunnuksen mukaan ja hallinta, kuten SCIM-protokollan 3.4.2 olevassa kyselyt.  

* Tukee kyselyt ryhmiä tunnuksen mukaan jäsenen osan 3.4.2 SCIM-protokollan mukaan.  

* Hyväksyy OAuth haltijan-tunnukset lupaa, kuten 2.1 SCIM-protokollan.

Tarkista sovelluksen-palveluntarjoajan tai näitä vaatimuksia yhteensopivuus lauseet sovelluksen kehittäjän ohjeissa.
 
###<a name="getting-started"></a>Käytön aloittaminen

Yllä olevien ohjeiden SCIM profiilin tukevat sovellukset voidaan yhdistää Azure Active Directory Azure AD-sovelluksen valikoiman "Mukautettu" sovellus-toiminnolla. Kun yhteys on muodostettu, suorittaa Azure AD-synkronoinnin joka 5 kohtaa, johon se kyselyt sovelluksen SCIM päätepisteen määritetyt käyttäjät ja ryhmät- ja luo tai muokkaa niitä mukaan tehtävän tiedot.

**Voit liittää sovellus, joka tukee SCIM seuraavasti:**

1.  Web-selaimessa Käynnistä Azure hallinta-portaalissa osoitteessa https://manage.windowsazure.com.
2.  Siirry **Active Directory > hakemisto > [Your hakemisto] > sovellukset**, ja valitse **Lisää > sovelluksen lisääminen valikoimasta**.
3.  Valitse vasemman reunan **Mukautettu** -välilehti, sovelluksesi nimi ja valitse sovellus-objektin luominen valintamerkki-kuvake.

![][2]

4.  Valitse tuloksena näytön toinen **Määritä tili valmistelu** -painike.
5.  Kirjoita **Valmistelu päätepisteen URL-osoite** -kenttään sovelluksen SCIM päätepisteen URL-osoite.
6.  Jos SCIM päätepisteen edellyttää OAuth-haltijatunnukseen kuin Azure AD myöntäjältä, kopioi OAuth-haltijatunnukseen **Todennus suojaustunnuksen (valinnainen)** -kenttään. On tämä kenttä on tyhjä, ja sitten Azure AD sisällytetään OAuth-haltijatunnukseen, myöntämä Azure AD sivupyynnön kanssa. Sovellukset, jotka käyttää Azure AD idenity tarjoajan voit tarkistaa tämän Azure AD-annettu tunnus.
7.  Valitse **Seuraava**ja valitse sitten **Aloita testi** -painiketta, jolloin Azure Active Directory muodostat yhteyden SCIM endpoint. Jos yritykset, vianmääritystiedot näkyvät.  
8.  Jos yritykset muodostaa sovelluksen onnistui, valitse **Seuraava** jäljellä olevat näyttöihin ja valitse **Valmis** , sulje valintaikkuna.
9.  Valitse tuloksena näytön kolmannen **Määrittää tilit** -painiketta. Tuloksena olevat käyttäjät ja ryhmät-osassa määrittää käyttäjät tai ryhmät säännöstä sovellukseen.
10. Kun käyttäjille ja ryhmille määritetään, valitse näytön yläreunassa **määritys** -välilehti.
11. Valitse **Tilin valmistelu**Varmista, että tila on käytössä. 
12. Valitse **Työkalut**- **uudelleen tilin valmistelu** tehoa aivoriihipalavereihin valmistelu prosessi.

Huomaa, että 5-10 minuutin saattaa käyttämättömänä valmistelu prosessi alkaa lähettää pyyntöjä SCIM päätepiste.  Yhteyksien yhteenveto on käytettävissä sovelluksen koontinäyttö-välilehti ja raportin valmistelu toiminnan ja valmistelu virheitä voi ladata kansion raportit-välilehti.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Rakentaminen oman valmistelu sovelluksen ratkaisu

Luomalla SCIM verkkopalveluun, joka liittymät Azure Active Directory, voit ottaa Sign- ja automaattinen kertakäyttäjä valmistelu miltei mistä tahansa sovellus, joka sisältää valmistelu API REST- tai SOAP käyttäjä.

Näin sen toiminnasta:

1.  Azure AD tarjoaa yleisiä kielen infrastruktuurin kirjaston nimeltä [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Järjestelmän muutetaan ja kehittäjät voivat käyttää tämän kirjaston luominen ja käyttöönotto SCIM-pohjainen web palvelupäätepiste voi yhteyden Azure AD minkä tahansa sovelluksen tunnistetietojen säilöön.
2.  WWW-palvelun avulla standardoitu käyttäjän rakenteen yhdistäminen käyttäjän rakenteen ja sovelluksen vaatii protokolla yhdistämismäärityksiä otettu käyttöön.
3.  Päätepisteen URL-osoite on rekisteröity Azure AD sovelluksen valikoiman mukautetun sovelluksen osana.
4.  Käyttäjille ja ryhmille määritetään Azure AD-sovellukselle. Tehtävän, kun niitä liikkeelle jonon kohdesovelluksen synkronoitavaksi. Käsittely jonossa synkronointiprosessia suorittaa 5 minuutin välein.

###<a name="code-samples"></a>MALLIKOODEJA

Prosessia helpottavaa joukko [näytteiden koodi](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) on kuvaus, joka luo SCIM web palvelupäätepiste kuvaavat Automaattinen valmisteleminen. Yksi näyte on palvelu, joka ylläpitää pilkuilla erotetut arvot edustavat käyttäjät ja ryhmät rivejä on tiedoston.  Toinen on palvelu, joka toimii Amazon Web Services tunnistetietojen ja käyttöoikeushallinta-palvelusta.  

**Edellytykset**

* Visual Studio 2013 tai uudempi versio
* [.NET Azure SDK](https://azure.microsoft.com/downloads/)
* Windowsin tietokoneeseen, joka tukee ASP.NET-framework 4.5, jota käytetään SCIM päätepiste. Tässä tietokoneessa on oltava käytettävissä pilvestä
* [Azure tilauksen Azure AD Premium kokeiluversio tai käyttöoikeus version kanssa](https://azure.microsoft.com/services/active-directory/)
* Amazon sisältyy AWS otosten edellyttää [Sisältyy AWS työkalujen Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)kirjastoihin. Katso lisätietoja otosten mukana Lueminut-tiedosto

###<a name="getting-started"></a>Käytön aloittaminen

Helpoin tapa toteuttaa SCIM päätepisteen, jotka voivat hyväksyä valmistelu Azure AD-pyynnöt on Muodosta ja ota käyttöön koodin otoksen, joka siirtää valmistellun käyttäjät CSV (CSV)-tiedostoon.

**Luo malli SCIM päätepiste:**

1.  Lataa [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) koodin malli-paketissa
2.  Paketin purkaminen ja sijoita se sijainnissa, kuten C:\AzureAD-BYOA-Provisioning-Samples\ Windows-tietokoneessa.
3.  Käynnistä FileProvisioningAgent-ratkaisun käyttöön Visual Studiossa tähän kansioon.
4.  Valitse **Työkalut > kirjasto pakettien hallinta > paketti hallinta-konsolin**, ja suorita komennot alla FileProvisioningAgent projektin ratkaisemaan ratkaisu viittaukset:

    Asenna paketti Microsoft.SystemForCrossDomainIdentityManagement Asenna-paketin Microsoft.IdentityModel.Clients.ActiveDirectory Asenna-paketin Microsoft.Owin.Diagnostics Asenna-paketin Microsoft.Owin.Host.SystemWeb

5.  Luoda FileProvisioningAgent projektin.
6.  Käynnistä Windows komentokehote-sovelluksen (järjestelmänvalvojana) ja **cd** -komennon avulla voit muuttaa kansion **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** -kansioon.
7.  Suorita-komento alla < ip-osoite > tilalle Windows-tietokoneen IP- tai toimialueen nimi.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  Windows-kohdassa **Windows-asetukset > verkko ja Internet-asetukset**, valitse **Windowsin palomuuri > Lisäasetukset**, ja luo **Saapuvan liikenteen sääntö** , joka sallii saapuvan portin 9000 käyttö.
9.  Jos Windows-tietokoneeseen on reitittimen takana, reitittimen on määritetty tekemään verkon käytön käännös välillä sen portti 9000, joka on määritetty Internet- ja portin 9000 Windows-tietokoneessa. Tämä on pakollinen Azure AD, voit käyttää tätä päätepisteen pilveen.


**Voit rekisteröidä otoksen SCIM päätepisteen Azure AD:**

1.  Web-selaimessa Käynnistä Azure hallinta-portaalissa osoitteessa https://manage.windowsazure.com.
2.  Siirry **Active Directory > hakemisto > [Your hakemisto] > sovellukset**, ja valitse **Lisää > sovelluksen lisääminen valikoimasta**.
3.  Valitse **Mukautettu** -välilehdessä vasemmalla, Anna nimi, esimerkiksi "SCIM testi sovellus" ja valitse ja Luo objekti app valintamerkki-kuvake. Huomaa, että sovellusobjektin luotu aio sovellukselle kohde jonka voi valmisteleminen ja kertakirjautumisen varten ja eikä vain SCIM päätepiste.

![][2]

4.  Valitse tuloksena näytön toinen **Määritä tili valmistelu** -painike.
5.  Kirjoita ‑valintaikkunan internet tarjoamia URL-Osoitteen ja portin SCIM päätepisteen. Tämä näyttää jotakuinkin esimerkiksi http://testmachine.contoso.com:9000 tai http://<ip-address>:9000/, missä < ip-osoite > Internetin tarjoamia IP address.  
6.  Valitse **Seuraava**ja valitse sitten **Aloita testi** -painiketta, jolloin Azure Active Directory muodostat yhteyden SCIM endpoint. Jos yritykset, vianmääritystiedot näkyvät.  
7.  Jos yritykset muodostaa WWW-palvelun onnistu, valitse **Seuraava** näyttöjen ja valitse **Valmis** , sulje valintaikkuna.
8.  Valitse tuloksena näytön kolmannen **Määrittää tilit** -painiketta. Tuloksena olevat käyttäjät ja ryhmät-osassa määrittää käyttäjät tai ryhmät säännöstä sovellukseen.
9.  Kun käyttäjille ja ryhmille määritetään, valitse näytön yläreunassa **määritys** -välilehti.
10. Valitse **Tilin valmistelu**Varmista, että tila on käytössä. 
11. Valitse **Työkalut**- **uudelleen tilin valmistelu** tehoa aivoriihipalavereihin valmistelu prosessi.

Huomaa, että 5-10 minuutin saattaa käyttämättömänä valmistelu prosessi alkaa lähettää pyyntöjä SCIM päätepiste.  Yhteyksien yhteenveto on käytettävissä sovelluksen koontinäyttö-välilehti ja raportin valmistelu toiminnan ja valmistelu virheitä voi ladata kansion raportit-välilehti.

Tarkistetaan otosten viimeinen vaihe on TargetFile.csv-tiedoston avaaminen \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug-kansio Windows-tietokoneessa. Kun valmistelu prosessin suorittamista, tiedosto näyttää kaikki tiedot määritetty ja valmisteltu käyttäjät ja ryhmät.

###<a name="development-libraries"></a>Kehittäminen kirjastot

Voit tehdä omia verkkopalveluun, joka SCIM-standardin mukainen ensin valintanauhaan tutustuminen seuraavat kirjastot Microsoftin nopeuttamiseksi kehittäminen prosessin avulla: 

**1:**  Yleiset kielen infrastruktuurin kirjastot tarjotaan käytettäväksi kieliä infrastruktuurin, kuten C# perusteella.  Näiden kirjastoihin [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)ilmoittaa liittymän Microsoft.SystemForCrossDomainIdentityManagement.IProvider, alla olevassa kuvassa.  Kehittäjä kirjastojen avulla Toteuta kyseisen liittymän luokka, joka saattaa yhdistyvät toisten tietokantajärjestelmien, palveluntarjoaja kuin käsiteltäväksi, kanssa.  Kirjastojen käyttöön developer verkkopalveluun, joka noudattaa käyttöön helposti SCIM määrityksen, joko isännöi Internet Information Services tai suoritettavan yleisiä kielen infrastruktuurin kokoonpanon.  Web-palveluun pyynnöt käännetään tarjoajan menetelmiä, jotka ohjelmoitu joitakin identity-säilö toimivat kehittäjä puhelut.    

![][3]

**2:** [Express reitin tiedostokäsittelijöitä](http://expressjs.com/guide/routing.html) ovat käytettävissä jäsentämiseen node.js pyynnön objektien puhelut (määritelty SCIM määrityksen mukaan), joka edustaa tehdyt node.js Web-palveluun.     

###<a name="building-a-custom-scim-endpoint"></a>Mukautetun SCIM päätepisteen luominen

Kirjastojen, yllä olevien ohjeiden avulla, kyseiset kirjastojen avulla kehittäjät ylläpitää niiden palveluiden suoritettavan yleisiä kielen infrastruktuurin kokoonpanon tai Internet Information Services kuluessa.  Tässä on esimerkki koodi isännöintipalvelu suoritettavan kokoonpanossa, osoitteessa http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

On tärkeää muistaa, että palvelu on oltava HTTP ja palvelimen todennusvarmennetta, jonka päämyöntäjä on jokin seuraavista: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Palvelimen todennusvarmenne voidaan sitoa portin verkon shell-apuohjelmalla Windows-isännän seuraavalla tavalla: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Tässä kohdassa annettu varmenteen hajautusarvo-argumentin arvo on varmenteen allekirjoitus silloin, kun annettu sovelluksen tunnus-argumentin arvo on haluamaansa yleisesti yksilöllinen tunnus.  

Isännöimiseen palvelun kuluessa Internet Information Services kehittäjä muodostaa yleisiä kielen infrastruktuurin koodin kirjasto-kokoonpanon käynnistys-nimisen kokoonpanon oletusnimitila luokka.  Tässä on esimerkki näiden luokan: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Päätepisteen todennus käsittely

Azure Active Directory-pyynnöt ovat OAuth 2.0-haltijatunnukseen.   Minkä tahansa palvelu saa pyynnön olisi todentaa siten, että Azure Active Directory puolesta odotettu Azure Active Directory-vuokraajan, Azure Active Directory Graph WWW-palvelun käytön myöntäjä.  Tunnistetaan tunnuksen, myöntäjä iss-ryhmän, kuten "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Tässä esimerkissä perusosoitteen vaatimus-arvon https://sts.windows.net, tunnistaa Azure Active Directory kuin myöntäjä, kun suhteellinen osoite-osiossa cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, on yksilöllinen Azure Active Directory-vuokraajan puolesta, jonka tunnus on annettu.  Jos Azure Active Directoryn kaavio-WWW-palvelun käyttöä varten on annettu tunnus, kyseisen palvelun 00000002-0000-0000-c000-000000000000-tunnisteen pitäisi olla tunnuksen aud varaa arvon.  

Kehittäjät Microsoftin toimittaman etsimisen SCIM palvelu yleisiä kielen infrastruktuuri-kirjastojen avulla voi todentaa pyynnöt Azure Active Directory Microsoft.Owin.Security.ActiveDirectory-paketin avulla toimimalla seuraavasti: 

**1:**  Valitse palveluntarjoaja toteuta se palauttaa tapa nimi on aina, kun palvelu käynnistetään Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior-ominaisuus: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Lisää tämä menetelmä mihin tahansa palvelun päätepisteet todennettu varustettu tunnuksen, joka on myöntänyt Azure Active Directory määritetyn vuokraajan, Azure Active Directory-kaavio-WWW-palvelun käytön puolesta pyyntö on seuraava koodi: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Käyttäjän ja ryhmän rakenne

Azure Active Directory voit valmistella kaksi resurssityyppiä SCIM Web-palveluihin.  Nämä resurssit ovat käyttäjät ja ryhmät.  

Käyttäjän resurssien tunnistetaan rakenne-tunniste, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, joka sisältyy protokolla-määritys: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Käyttäjien Azure Active Directoryn urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurssien määritteet attribuuteista oletusarvo-määritys on annettu taulukossa 1 alla.  

Resurssit ryhmästä tunnistetaan rakenne-tunniste, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Taulukon 2 alla näkyy ryhmät Azure Active Directoryn määritteet http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resurssien määritteiden oletusarvon määritystä.  

###<a name="table-1-default-user-attribute-mapping"></a>Taulukko 1: Oletusarvon käyttäjän määrite yhdistäminen

| Azure Active Directory-käyttäjän | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktiivinen |
| Näyttönimi | Näyttönimi |
| Telekopiota TelephoneNumber | phoneNumbers [tyyppi eq "Faksi"] .value |
| givenName | name.givenName |
| Asema | otsikko |
| sähköposti | sähköpostien [tyyppi eq "työ"] .value |
| mailNickname | externalId |
| hallinta | hallinta |
| Nuolet | phoneNumbers [tyyppi eq "mobile"] .value |
| Objektitunnus | tunnus |
| Postinumero | osoitteiden [tyyppi eq "työ"] .postalCode |
| välityspalvelimen osoitteet | sähköpostien [Kirjoita eq "muut"]. Arvo |
| Fyysinen toimitus-OfficeName | [Kirjoita eq "muut"]-osoitteet. Muotoiltu |
| streetAddress | osoitteiden [tyyppi eq "työ"] .streetAddress |
| Sukunimi | name.familyName |
| Puhelinnumero | phoneNumbers [tyyppi eq "työ"] .value |
| käyttäjän PrincipalName | Käyttäjänimi |


###<a name="table-2-default-group-attribute-mapping"></a>Taulukko 2: Oletus määritettä määritys

| Azure Active Directory-ryhmä | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| Näyttönimi | externalId |
| sähköposti | sähköpostien [tyyppi eq "työ"] .value |
| mailNickname | Näyttönimi |
| jäsenet | jäsenet |
| Objektitunnus | tunnus |
| proxyAddresses | sähköpostien [Kirjoita eq "muut"]. Arvo |


##<a name="user-provisioning-and-de-provisioning"></a>Käyttäjän valmistelu ja sen valmistelu

Kuvassa näkyy alla, että Azure Active Directory lähettää SCIM palveluun hallittavan käyttäjätiedot käyttäjän elinkaari viestit tallennetaan.  Kaavio näyttää myös siitä, miten käyttämällä Microsoftin toimittaman etsimisen näiden palvelujen yleisiä kielen infrastruktuurin kirjastojen SCIM palvelu kääntää nämä pyynnöt palveluntarjoaja menetelmiä puhelut.  

![][4]
*Kuva: Käyttäjän valmistelu ja valmistelu varaustiedoista järjestys*

**1:**  Azure Active Directory kyselyn käyttäjän palvelun externalId-määritteen, vastaavat mailNickname määritteen Azure Active Directory-käyttäjän kanssa.  Kyselyn valuuttamäärän Hypertext Transfer Protocol pyyntö katsomalla, jossa jyoung on esimerkki mailNickname Azure Active Directory-käyttäjän: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Jos palvelu on luotu Microsoftin toimittaman soveltamisesta SCIM services yleisiä kielen infrastruktuuri-kirjastojen avulla, sitten pyynnön muunnetaan kutsu palveluntarjoajan kysely-menetelmää.  Tämä menetelmä allekirjoituksen näin: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Näin Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters-liittymän määritys: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Kyselyn käyttäjän määritetyn arvon externalId-määritteen edellä otoksen kyseessä arvot kyselyn menetelmä argumentit on seuraavasti: 

* parametrit. AlternateFilters.Count: 1
* parametrit. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* parametrit. AlternateFilters.ElementAt(0). Vertailuoperaattori: ComparisonOperator.Equals
* parametrit. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Pyyntötunnus"] 

**2:**  Jos kyselyn palveluun externalId määritteen arvo Azure Active Directory-käyttäjän mailNickname-määritteen arvo vastaa käyttäjän vastausta ei palauta kaikki käyttäjät, valitse Azure Active Directory pyytää, että palvelun valmistelu vastaavat Azure Active Directory-käyttäjän.  Tässä on esimerkki pyynnön: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

Microsoftin toimittaman soveltamisesta SCIM services yleisiä kielen infrastruktuurin kirjastoja kääntäminen pyynnön palveluntarjoajan Create-menetelmä puhelun.  Luo menetelmässä on allekirjoitusta: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Pyyntö valmistelu käyttäjä resurssi-argumentin arvo on Microsoft.SystemForCrossDomainIdentityManagement esiintymä. Core2EnterpriseUser luokka, joka on määritetty Microsoft.SystemForCrossDomainIdentityManagement.Schemas-kirjastossa.  Jos valmistelu käyttäjän pyynnön onnistuu ja valitse menetelmä soveltaminen odotetaan palauttavan esiintymä Microsoft.SystemForCrossDomainIdentityManagement. Luokan Core2EnterpriseUser arvoksi juuri valmisteltu käyttäjän yksilöllinen tunnus-omaisuus arvolla.  

**3:**  Päivitettävä löytääkseen olemassa mukaan SCIM fronted tunnistetietojen säilössä käyttäjän Azure Active Directory etenee pyytämällä kyseisen käyttäjän nykyinen tila-palvelusta pyyntöä seuraavanlainen: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

-Palvelun avulla Microsoftin toimittaman soveltamisesta SCIM services yleisiä kielen infrastruktuuri-kirjastoja pyynnön muunnetaan kutsu palveluntarjoajan Nouda-menetelmää.  Näin Nouda menetelmän allekirjoitus: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Kyseessä pyyntö hakea nykyisen tilan käyttäjän edellä olevassa esimerkissä parametrit-argumentin arvo taulukkona objektin ominaisuuksien arvot ovat seuraavat: 

* Tunnus: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Jos viittaus-määrite on päivitettävä, sitten Azure Active Directory pyytää määrittääksesi, onko identity-kaupan viittaus-määritteen nykyisen arvon fronted palvelun jo palvelun Azure Active Directory-määritteen arvo vastaa.  Käyttäjien, jossa kysely nykyisen arvon tällä tavalla vain-määrite on hallinta-määrite.  Tässä on esimerkki pyyntö määrittääksesi, onko tietyn käyttäjän objektin hallinta-määrite on jokin tietty arvo: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Arvon määritteet kysely-parametrin tunnus osoittaa, että kyseessä, jos käyttäjät-objekti, joka täyttää suodattimen kysely-parametrin arvo-taulukkona lauseke ja valitse palvelu on tarkoitus urn: ietf:params:scim:schemas:core:2.0:User tai urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurssin, mukaan lukien vain kyseiselle resurssille id-määritteen arvo vastaa.  Tunnus-määritteen arvon tiedetään pyytäjän – se sisältyy suodatinta kyselyn; parametrin arvo pyytävä sen tarkoituksena on todella pyytää resurssin mahdollisimman vähän esittäminen, jotka täyttävät siitä, onko tällainen objekti on luotu suodatin-lausekkeen.   

Jos palvelu on luotu Microsoftin toimittaman soveltamisesta SCIM services yleisiä kielen infrastruktuuri-kirjastojen avulla, sitten pyynnön muunnetaan kutsu palveluntarjoajan kysely-menetelmää.  Parametrit-argumentin arvo taulukkona objektin ominaisuudet arvo on seuraavasti: 

* parametrit. AlternateFilters.Count: 2
* parametrit. AlternateFilters.ElementAt(x). AttributePath: "tunnus"
* parametrit. AlternateFilters.ElementAt(x). Vertailuoperaattori: ComparisonOperator.Equals
* parametrit. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* parametrit. AlternateFilters.ElementAt(y). AttributePath: "hallinta"
* parametrit. AlternateFilters.ElementAt(y). Vertailuoperaattori: ComparisonOperator.Equals
* parametrit. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* parametrit. RequestedAttributePaths.ElementAt(0): "tunnus"
* parametrit. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Tässä indeksin x arvo voi olla 0 ja indeksi y-arvo voi olla 1 tai x arvo voi olla 1 ja y-arvo voi olla 0, suodattimen kyselyn parametri lausekkeiden järjestyksen mukaan.   

**5:**  Tässä on esimerkki pyyntö Azure Active Directorysta SCIM palveluihin päivittää käyttäjän: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

Soveltamisesta SCIM palveluiden Microsoft yleisiä kielen infrastruktuuri-kirjastot kääntää pyynnön kutsu palveluntarjoajan Update-menetelmää.  Tämä menetelmä allekirjoituksen näin: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Päivittää käyttäjän pyyntö edellä olevassa esimerkissä objektin taulukkona korjaus-argumentin arvo on tämän ominaisuuden: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest PatchRequest2 nimellä). Operations.Count: 1
* (PatchRequest PatchRequest2 nimellä). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest PatchRequest2 nimellä). Operations.ElementAt(0). Path.AttributePath: "hallinta"
* (PatchRequest PatchRequest2 nimellä). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest PatchRequest2 nimellä). Operations.ElementAt(0). Value.ElementAt(0). Viite: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest PatchRequest2 nimellä). Operations.ElementAt(0). Value.ElementAt(0). Arvo: 2819c223-7f76-453a-919d-413861904646

**6:**  Poista valmistelu käyttäjän fronted SCIM-palvelun identity-kaupasta, Azure Active Directory lähetetään pyyntö seuraavanlainen: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Jos palvelu on luotu Microsoftin toimittaman soveltamisesta SCIM services yleisiä kielen infrastruktuuri-kirjastojen avulla, pyyntö käännettävä palveluntarjoajan Delete-menetelmä puhelun mukaisesti.   Tämä menetelmä on allekirjoitusta: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Objektin taulukkona resourceIdentifier-argumentin arvo on kyseessä pyyntö valmistelu Poista käyttäjä edellä Esimerkki ominaisuuden arvot: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Ryhmän valmistelu ja sen valmistelu

Kuva alapuolella näkyy, että Azure Active Directory lähettää SCIM palveluun hallittavan ryhmän toisen käyttäjätiedot elinkaari viestit tallennetaan.  Viestit poiketa viestit liittyvät käyttäjien kolmella tavalla: 

* Ryhmän resurssin rakenteen oletuskentiksi http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Ryhmien hakemiseen pyynnöt säätää, että jäsenet-määrite on minkä tahansa resurssin pyynnön vastaus säädetty ulkopuolelle.  
* Pyynnöt määrittää, onko viittaus-määrite on tietty arvo on pyynnöt tietoja jäsenet-määrite.  

![][5]
*Kuva: Ryhmän valmistelu ja valmistelu varaustiedoista järjestys*

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Automatisoida käyttäjän valmistelu ja valmistelun poistaminen SaaS sovelluksiin](active-directory-saas-app-provisioning.md)
- [Määritteiden yhdistämismääritykset käyttäjän valmisteluun mukauttaminen](active-directory-saas-customizing-attribute-mappings.md)
- [Määritteiden yhdistämismääritykset lausekkeiden kirjoittamiseen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Vaikutusalueen määrittäminen suodattimet käyttäjän valmistelu](active-directory-saas-scoping-filters.md)
- [Tilin valmistelu ilmoitukset](active-directory-saas-account-provisioning-notifications.md)
- [Luettelo siitä, miten voit integroida SaaS sovellusten opetusohjelmat](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
