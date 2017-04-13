<properties 
    pageTitle="Välitys API käyttämisestä Windows Phone Silverlight" 
    description="Välitys API käyttämisestä Windows Phone Silverlight"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Välitys API käyttämisestä Windows Phone Silverlight

Tämä asiakirja on lisäosaa, [miten voit integroida Mobile Windows Phone Silverlight-sovelluksen välitys](mobile-engagement-windows-phone-integrate-engagement.md)-asiakirjaan. Se sisältää syvyys tietojen välitys Ohjelmointirajapinnan käyttäminen ja raportoi sovellusta tilastotiedot.

Jos haluat vain varauksia raportin sovelluksen istunnot, aktiviteetteja, kaatuu ja teknisiä tietoja ja valitse helpoin tapa on kaikkien oman `PhoneApplicationPage` aliraportti luokat perivät `EngagementPage` luokka.

Jos haluat käyttää lisätoimintoja, esimerkiksi jos sinun on ilmoitettava sovelluksen tiettyjä tapahtumia, virheet ja töitä, tai jos tietokoneeseesi on ilmoittamaan sovelluksen toimintoja eri tavalla kuin se käytössä `EngagementPage` luokkien, sinun on käytettävä välitys Ohjelmointirajapinnan.

Välitys Ohjelmointirajapinnan tarjoaa `EngagementAgent` luokka. Voit käyttää näitä menetelmiä kautta `EngagementAgent.Instance`.

Vaikka agent-moduulia ei ole käynnistetty, jokainen Ohjelmointirajapinnan kutsu on lykätty ja suoritetaan uudelleen, kun agentti ei ole käytettävissä.

##<a name="engagement-concepts"></a>Välitys käsitteitä

Seuraavien osien Tarkenna Mobile välitys käsitteitä Windows Phone-ympäristössä.

### <a name="session-and-activity"></a>`Session`ja`Activity`

*Tehtävän* liittyy yleensä yhden sivun reunukset, toisin sanoen *tehtävä* alkaa, kun sivu tulee näkyviin ja lopettaa sivun ollessa suljettuna: Tämä on sen jälkeen tapausta, kun välitys SDK on integroitu avulla `EngagementPage` luokka.

Mutta *toimintoja* voidaan ohjata myös manuaalisesti välitys Ohjelmointirajapinnan avulla. Näin voit jakaa useita sub-osia, saat lisätietoja tämän sivun (esimerkiksi kuinka usein tunnetut ja kuinka kauan valintaikkunat käytetään tämän sivun kohtaa) käyttö tietyn sivulle.

##<a name="reporting-activities"></a>Raportoinnin toiminnot

### <a name="user-starts-a-new-activity"></a>Käyttäjä aloittaa uusi tehtävä

#### <a name="reference"></a>Viittaus

            void StartActivity(string name, Dictionary<object, object> extras = null)

Soita `StartActivity()` aina, kun käyttäjän toiminnan muutokset. Tämän funktion ensimmäinen kutsu aloittaa uuden käyttäjäistunnon.

> [AZURE.IMPORTANT] SDK kutsua EndActivity menetelmää automaattisesti, kun sovellus suljetaan. Näin ollen on erittäin suositeltavaa Soita StartActivity menetelmä aina, kun tehtävän käyttäjän muutoksen ja ei koskaan kutsua EndActivity-menetelmää, koska tämä kutsumista pakottaa päättää istunnon.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Käyttäjän päättyy hetkinen toiminta

#### <a name="reference"></a>Viittaus

            void EndActivity()

Soita `EndActivity()` vähintään kerran, kun käyttäjä loppuu viimeisen toimintaansa. Näin tiedät, että käyttäjä ei tällä hetkellä käyttämättömänä ja että käyttäjäistunto on suljettava kerran istunnon aikakatkaisun päättyy välitys SDK (Jos soitat `StartActivity()` istunnon aikakatkaisun vanhenee, istunnon yksinkertaisesti jatkuu).

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Töiden raportointi

### <a name="start-a-job"></a>Työn aloittaminen

#### <a name="reference"></a>Viittaus

            void StartJob(string name, Dictionary<object, object> extras = null)

Työn avulla voit seurata sisältää tehtäviä ajan kuluessa.

#### <a name="example"></a>Esimerkki

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Lopeta työ

#### <a name="reference"></a>Viittaus

            void EndJob(string name)

Kun jäljittää työn tehtävä on päättynyt, voit soittaa EndJob-menetelmä tälle projektille toimittamalla työn nimi.

#### <a name="example"></a>Esimerkki

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Tapahtumien raportointi

On kolmenlaisia tapahtumia:

-   Erillisestä tapahtumat
-   Istunnon tapahtumat
-   Työn tapahtumat

### <a name="standalone-events"></a>Erillisestä tapahtumat

#### <a name="reference"></a>Viittaus

            void SendEvent(string name, Dictionary<object, object> extras = null)

Erillisestä tapahtumat voi ilmetä ulkopuolella istunnon yhteydessä.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Istunnon tapahtumat

#### <a name="reference"></a>Viittaus

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Istunnon tapahtumat käytetään yleensä raportin hänen istunnon aikana käyttäjän toimintoja.

#### <a name="example"></a>Esimerkki

**Ilman tietoja:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Tietoja:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Työn tapahtumat

#### <a name="reference"></a>Viittaus

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Työn tapahtumien käytetään yleensä raportin toiminnot suoritetaan käyttäjän projektin aikana.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Virheiden raportoiminen

On kolmenlaisia virheitä:

-   Erillisestä virheet
-   Istunnon virheet
-   Työn virheet

### <a name="standalone-errors"></a>Erillisestä virheet

#### <a name="reference"></a>Viittaus

            void SendError(string name, Dictionary<object, object> extras = null)

Toisin kuin istunnon virheet erillinen virheitä voi ilmetä ulkopuolella istunnon yhteydessä.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Istunnon virheet

#### <a name="reference"></a>Viittaus

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Istunnon virheitä käytetään yleensä vaikuttavat käyttäjän hänen istunnon aikana virheiden raportoiminen.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Työn virheet

#### <a name="reference"></a>Viittaus

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Käynnissä olevan projektin sijaan, että liittyvät käyttäjäistunnon voi liittyä virheitä.

#### <a name="example"></a>Esimerkki

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Raportoinnin kaatuu

Agentti on kaksi tapaa käsitellä kaatuu.

### <a name="send-an-exception"></a>Lähetä poikkeuksen

#### <a name="reference"></a>Viittaus

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Esimerkki

Voit lähettää poikkeuksen milloin tahansa soittamalla:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Voit myös lopettaa samaan aikaan kuin lähettäminen kaatumisen välitys-istunnon valinnaisten parametrien. Voit tehdä Soita:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Jos teet näin, istunnon ja töiden suljetaan lähettämisestä kaatumisen.

### <a name="send-an-unhandled-exception"></a>Lähetä käsittelemättömän poikkeuksen vuoksi

#### <a name="reference"></a>Viittaus

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Välitys mahdollistaa myös lähettää käsittelemättömän poikkeukset. Tämä on hyötyä erityisesti silloin, kun käytetään sulkeiden sisällä silverlight UnhandledException tapahtumakäsittelijä.

Tämä menetelmä on **aina** lopettaa välitys istunnon ja töiden jälkeen kutsutaan.

#### <a name="example"></a>Esimerkki

Sen avulla voidaan toteuttaa oman UnhandledException käsittely (varsinkin, jos olet poistanut automaattisen kaatumisen raportointi-toiminnon välitys). Esimerkiksi `Application_UnhandledException` menetelmää `App.xaml.cs` tiedosto:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Viittaus

            void OnActivated(ActivatedEventArgs e)

Kun käyttäjä siirtyy eteenpäin, poissa-sovelluksen aktivointi poistetaan tapahtuma käynnistyy, kun käyttöjärjestelmä yrittää siirtää sovelluksen passiiviset vaiheeseen. Valitse sovellus on Tombstoning. Tässä prosessissa sovellus on päättynyt, mutta jotkin sovelluksen ja sovelluksen yksittäisiä sivuja tilan tietoja säilytetään.

Voit lisätä on `EngagementAgent.Instance.OnActivated(e)` - `Application_Activated` menetelmä vaihtamaan välitys-agentti, kun sovellus on Tombstoned App.xaml.cs-tiedostosta.

### <a name="example"></a>Esimerkki

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Laitteen tunnus

            String GetDeviceId()

Saat välitys laitteen tunnus soittamalla tätä menetelmää.

##<a name="extras-parameters"></a>Lisäominaisuudet-parametrit

Satunnaisia tietoja voidaan liittää tapahtuman, virhe, tehtävän tai projektin. Nämä tiedot on jäsennetty sanastoa. Avaimet ja arvot voi olla mikä tahansa.

Lisäominaisuudet tiedot ovat muuntaa sarjaksi, joten jos haluat lisätä oman lisäominaisuudet sinun on lisättävä tällaista sopimuksen tiedot.

### <a name="example"></a>Esimerkki

Luodaan uusi luokka "Henkilö".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Valitse, lisäämme `Person` ylimääräinen esiintymä.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Jos lisäät muita objekteja, varmista, että niiden ToString() menetelmää on toteutettu ihmisten luettavissa merkkijono.

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Jokainen objektin avain on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

**Vähintään 1 024** merkkiä puhelun kokorajoituksena lisäominaisuudet.

##<a name="reporting-application-information"></a>Sovelluksen raportointiin

### <a name="reference"></a>Viittaus

            void SendAppInfo(Dictionary<object, object> appInfos)

Voit raportoida manuaalisesti seurannan tiedot (tai muita sovelluksen tiettyjä tietoja) SendAppInfo() käyttämällä funktiota.

Huomaa, että nämä tiedot voidaan lähettää vaiheittainen: laitteen säilytetään vain tietyn avaimen uusimman arvo. Tapahtuman lisäominaisuudet, kuten sanastoa käyttämällä\<objektia, objektin\> liitettävä informations.

### <a name="example"></a>Esimerkki

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Jokainen objektin avain on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

Hakemuksen tiedot on oikeutettu enintään **1 024** merkkiä puhelun.

Edellisen esimerkin palvelimeen lähetetään JSON on 44 merkkiä:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Lokiin kirjaaminen
###<a name="enable-logging"></a>Kirjaamisen ottaminen käyttöön

SDK voi määrittää tuottamaan testi lokit IDE konsolissa.
Lokitiedostot ovat ei oletusarvoisesti. Voit mukauttaa tätä Päivitä ominaisuus `EngagementAgent.Instance.TestLogEnabled` johonkin käytettävissä arvo `EngagementTestLogLevel` luettelointi, esimerkiksi:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
