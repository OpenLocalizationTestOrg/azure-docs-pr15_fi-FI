<properties
    pageTitle="Välitys Ohjelmointirajapinnan käyttämisestä iOS"
    description="Uusimman iOS SDK - Ohjelmointirajapinnan välitys käyttämisestä iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Välitys Ohjelmointirajapinnan käyttämisestä iOS

Tämä asiakirja on lisäosaa tiedostoa voit integroida välitys iOS: se sisältää syvyys tietojen välitys Ohjelmointirajapinnan käyttäminen ja raportoi sovellusta tilastotiedot.

Ota huomioon, että jos haluat vain varauksia raportin sovelluksen istunnot, aktiviteetteja, kaatuu ja teknisiä tietoja, valitse helpoin tapa on kaikki mukautetun `UIViewController` perivät vastaavasta `EngagementViewController` luokka.

Jos haluat käyttää lisätoimintoja, esimerkiksi jos sinun on ilmoitettava sovelluksen tiettyjä tapahtumia, virheet ja töitä, tai jos tietokoneeseesi on ilmoittamaan sovelluksen toimintoja eri tavalla kuin se käytössä `EngagementViewController` luokkien, sinun on käytettävä välitys Ohjelmointirajapinnan.

Välitys Ohjelmointirajapinnan tarjoaa `EngagementAgent` luokka. Tämä luokan esiintymä voi hakea soittamalla `[EngagementAgent shared]` staattinen menetelmä (Huomaa, että `EngagementAgent` palautti objekti on Yksiarvoinen).

Ohjelmointirajapinta, puhelujen ennen `EngagementAgent` objektia on valmisteltu menetelmällä`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Välitys käsitteitä

Seuraavien osien Tarkenna yhteiset [Mobile välitys käsitteitä](mobile-engagement-concepts.md) iOS-ympäristössä.

### <a name="session-and-activity"></a>`Session`ja`Activity`

*Tehtävän* liittyy yleensä yhden näytön sovelluksen, toisin sanoen *tehtävän* käynnistyy, kun näyttöön tulee näkyviin ja lopettaa näytön ollessa suljettuna: Tämä on sen jälkeen tapausta, kun välitys SDK on integroitu avulla `EngagementViewController` luokat.

Mutta *toimintoja* voidaan ohjata myös manuaalisesti välitys Ohjelmointirajapinnan avulla. Näin voit jakaa useita sub osia tarkempia tietoja (esimerkiksi, kuinka usein tunnetut ja kuinka kauan valintaikkunat käytetään tämän näytön sisällä) tämän näytön käyttö tietyn näyttöön.

##<a name="reporting-activities"></a>Raportoinnin toiminnot

### <a name="user-starts-a-new-activity"></a>Käyttäjä aloittaa uusi tehtävä

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Soita `startActivity()` aina, kun käyttäjän toiminnan muutokset. Tämän funktion ensimmäinen kutsu aloittaa uuden käyttäjäistunnon.

### <a name="user-ends-his-current-activity"></a>Käyttäjän päättyy hetkinen toiminta

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Sinun tulee **ei koskaan** kutsua tämän funktion itse, paitsi, jos haluat jakaa yhden jakaa useita istuntoja sovelluksen käyttäminen: tämän funktion kutsu lopettaa istunnon välittömästi, niin, seuraavien puhelun `startActivity()` aloittaa uuden istunnon. SDK nimi Tämä funktio automaattisesti, kun sovellus suljetaan.

##<a name="reporting-events"></a>Tapahtumien raportointi

### <a name="session-events"></a>Istunnon tapahtumat

Istunnon tapahtumat käytetään yleensä raportin hänen istunnon aikana käyttäjän toimintoja.

**Esimerkki ilman ylimääräisiä tietoja:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Esimerkki, jossa lisätiedot:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Erillisestä tapahtumat

Toisin kuin istunnon tapahtumat erillinen tapahtumat voidaan ulkopuolella istunnon yhteydessä.

**Esimerkki:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Virheiden raportoiminen

### <a name="session-errors"></a>Istunnon virheet

Istunnon virheitä käytetään yleensä vaikuttavat käyttäjän hänen istunnon aikana virheiden raportoiminen.

**Esimerkki:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Erillisestä virheet

Toisin kuin istunnon virheet erillinen virheet voidaan ulkopuolella istunnon yhteydessä.

**Esimerkki:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Töiden raportointi

**Esimerkki:**

Oletetaan, että haluat ilmoittaa kirjautuminen prosessin kestoa:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Projektin aikana virheistä

Käynnissä olevan projektin sijaan, että liittyvät käyttäjäistunnon voi liittyä virheitä.

**Esimerkki:**

Oletetaan, että haluat ilmoittaa virheestä kirjautuminen prosessin aikana:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Tapahtumien projektin aikana

Käynnissä olevan projektin sijaan, että liittyvät käyttäjäistunnon voi liittyä tapahtumat.

**Esimerkki:**

Oletetaan, että olemme yhteisöpalvelun ja Käytämme raporttiin työn kokonaisaika, jona käyttäjä on yhteydessä palvelimeen. Käyttäjä voi vastaanottaa viestejä hänen ystävien, työ-tapahtuma.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Ylimääräisen parametrit

Satunnaisia tietoja voidaan liittää tapahtumia, virheitä, tehtävät ja työt.

Nämä tiedot on järjestetty, se käyttää iOS's NSDictionary luokka.

Huomaa, että lisäominaisuudet voi olla `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` tai muita `NSDictionary` esiintymät.

> [AZURE.NOTE] Muuntaa sarjaksi ylimääräistä parametria, JSON-muodossa. Jos haluat siirtää eri kuin edellä kuvatun niistä objekteja, omaa luokkaasi käyttöön seuraavasti:
>
             -(NSString*)JSONRepresentation;
>
> Menetelmän pitää palauttaa objektin JSON esittäminen.

### <a name="example"></a>Esimerkki

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Kunkin avain `NSDictionary` on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

**Vähintään 1 024** merkkiä (kerran koodattu JSON-agentti välitys) puhelun kokorajoituksena lisäominaisuudet.

Edellisen esimerkin palvelimeen lähetetään JSON on 58 merkkiä:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Sovelluksen raportointiin

Voit raportoida manuaalisesti käyttämällä seurannan tiedot (tai muut sovelluksen tietyt tiedot) `sendAppInfo:` funktio.

Huomaa, että nämä tiedot voidaan lähettää vaiheittainen: laitteen säilytetään vain tietyn avaimen uusimman arvo.

Tapahtuman lisäominaisuudet, kuten `NSDictionary` luokan käytetään abstrakti hakemuksen tiedot, Huomaa, että matriiseja tai aliraportti sanastot käsitellään tasainen merkkijonot (joko JSON Sarjatoiminto).

**Esimerkki:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Kunkin avain `NSDictionary` on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

Hakemuksen tiedot on oikeutettu enintään **1 024** merkkiä (kerran koodattu JSON-agentti välitys) puhelun.

Edellisen esimerkin palvelimeen lähetetään JSON on 44 merkkiä:

    {"birthdate":"1983-12-07","gender":"female"}
