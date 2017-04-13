<properties
    pageTitle="Azure Mobile välitys iOS SDK integrointi | Microsoft Azure"
    description="Uusimmat päivitykset ja ohjeet iOS SDK Azure Mobile välitys"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Voit integroida välitys iOS

> [AZURE.SELECTOR]
- [Windowsin Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md)

Tässä kuvataan helpoin tapa Aktivoi välitys 's Analytics seurantaan ja valvontaan Funktiot iOS-sovelluksessa.

Välitys SDK edellyttää iOS6 + ja Xcode 8: käyttöönoton kohde sovelluksen on oltava vähintään iOS 6.

> [AZURE.NOTE]
> Jos todella XCode 7 määräytyvät voi käyttää [iOS välitys SDK v3.2.4](https://aka.ms/r6oouh). Ei tunnettuja ohjelmavirhe tätä aiemman version Reach moduuli samalla, kun käynnissä iOS 10-laitteissa on [saatavilla moduulin integroinnin](mobile-engagement-ios-integrate-engagement-reach.md) lisätietoja. Jos haluat käyttää SDK-v3.2.4 sitten Ohita vain `UserNotifications.framework` Tuo seuraavassa vaiheessa.

Seuraavat vaiheet ovat riittävän Aktivoi lokit tarpeen laskea tilastoja kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja Technicals raportti. Lokit tarpeen laskea muiden tilastotietoja, kuten tapahtumia, virheitä ja töiden raportti on tehtävä manuaalisesti välitys-Ohjelmointirajapinnan käyttäminen (katso [käyttämisestä Lisäasetukset Mobile-välitys tunnisteita Ohjelmointirajapinnan iOS-sovellukseen](mobile-engagement-ios-use-engagement-api.md) , koska nämä tilasto on sovellusta riippuvaisten.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>Välitys SDK upotat iOS-projektiin

- Lataa iOS SDK [täältä](http://aka.ms/qk2rnj).

- Välitys SDK lisääminen projektiin iOS: Xcode, valitse projektin hiiren kakkospainikkeella ja valitsemalla **"Lisää tiedostoja..."** ja valitse `EngagementSDK` kansio.

- Välitys tarvitaan muita kehysten toimimaan: project explorer Avaa project-ruudussa ja valitse oikea kohde. Avaa **"Muodosta vaiheiden"** -välilehti ja **"binaarinen kanssa kirjastojen"** -valikosta Lisää nämä kehysten:

    -   `UserNotifications.framework`– Määritä linkki nimellä`Optional`
    -   `AdSupport.framework`– Määritä linkki nimellä`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] AdSupport framework voidaan poistaa. Välitys on tässä yhteydessä voi kerätä IDFA. Kuitenkin IDFA sivustokokoelman käytöstä \<ios-sdk-välitys-idfa\> noudattamisen uusi Apple-käytäntö koskeva tämän ID-tunnuksellasi.

##<a name="initialize-the-engagement-sdk"></a>Välitys SDK alustaminen

Saatat joutua muokkaamaan edustajallasi sovelluksen:

-   Käyttöönotto-tiedoston yläosassa tuoda välitys-agentti seuraavasti:

        [...]
        #import "EngagementAgent.h"

-   Alusta välitys sisällä menetelmä '**applicationDidFinishLaunching:**"tai"**sovelluksen: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Tavallinen raportointi

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Suositeltu tapa: ylikuormituksen oman `UIViewController` luokat

Kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja teknisistä tiedoista laskemiseen välitys vaatii lokit raportin aktivoimista yksinkertaisesti tehdä kaikki oman `UIViewController` aliraportti luokat perivät `EngagementViewController` luokkien (sama sääntö `UITableViewController`  - \> `EngagementTableViewController`).

**Ilman välitys:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Välitys: kanssa**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `startActivity()` manuaalisesti

Jos et voi tai et halua ylikuormituksen oman `UIViewController` luokkia, voit aloittaa toimintojen sijaan soittamalla `EngagementAgent`'s menetelmiä suoraan.

> [AZURE.IMPORTANT] SDK automaattisesti soittaa iOS `endActivity()` silloin, kun sovellus suljetaan. Näin, se on *erittäin* suositeltavaa Soita `startActivity` menetelmä aina, kun tehtävä käyttäjän muuttaminen ja *ei koskaan* Soita `endActivity` menetelmää, koska tämä kutsumista pakottaa päättää istunnon.

##<a name="location-reporting"></a>Sijainti raportointi

Apple palveluehdot Älä salli sovellusten käyttää vain tilastotiedot tarkoitukseen seuranta sijainti. Näin ollen suositellaan sijainti raportit käyttöön vain, jos sovellus käyttää myös sijainti seuranta muusta syystä.

Alkaen iOS 8, sinun on annettava kuvaus siitä, miten sovelluksen käyttää määrittämällä merkkijonon avaimen [NSLocationWhenInUseUsageDescription] tai [NSLocationAlwaysUsageDescription] sinua sovelluksen Info.plist tiedoston sijainti palveluja. Jos haluat raportin kohtaan välitys taustan, Lisää avain NSLocationAlwaysUsageDescription. Kaikissa muissa tapauksissa Lisää avain NSLocationWhenInUseUsageDescription.

### <a name="lazy-area-location-reporting"></a>Kuitata alueen sijainti raportointi

Kuitata alueen sijainti reporting avulla voit raportoida maa-, alue- ja laitteisiin liittyvät paikka. Tämän tyyppistä sijainti raportoinnin käyttää vain verkkosijainnit (perustuu solun tunnus tai WIFI). Laitteen-alueella ilmoitetaan enintään kerran istunnossa. GPS on käyttänyt ja näin ollen tällaista sijainti raportti on hyvin vähän (ei haluat sanoa ei) varauksen vaikutus.

Raportoidun alueet käytetään laskemiseen maantieteelliset tilastotietoja käyttäjille, istunnot, tapahtumia ja virheet. Niitä voi käyttää myös Reach Kampanjat ehto. Viimeinen tunnetut alueen raportoidusta laitteen voi hakea [Laitteen API]alkuun.

Käyttöön kuitata alueen sijainti, Lisää seuraava rivi jälkeen valmistellaan välitys-agentti:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Reaaliaikainen sijainti raportointi

Reaaliaikainen sijainti raportoinnin avulla raportoimaan leveyttä ja laitteisiin liittyvät pituutta. Oletusarvon mukaan tällaista sijainti raportoinnin käyttää vain verkkosijainnit (perustuu solun tunnus tai WIFI) ja raportoinnin on käytettävissä vain, kun sovellus suoritetaan edustalla (eli istunnossa).

Reaaliaikainen sijainnit eivät *ole* käyttää laskemiseen tilastotiedot. Vain niiden tarkoitus on sallittua käyttää reaaliajassa geo-aitaus \<Reach osallistujat-geofencing\> Reach Kampanjat ehto.

Käyttöön reaaliaikainen sijainti, Lisää seuraava rivi jälkeen valmistellaan välitys-agentti:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS perusteella raportointi

Käyttää oletusarvon mukaan reaaliaikainen sijainti raportointi vain mukaan verkkosijainnit. Voit ottaa mukaan GPS sijainneista (jotka ovat paljon tarkemmin), Lisää:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Taustan raportointi

Oletusarvon mukaan reaaliaikainen sijainti raportointi on aktiivinen vain, kun sovellus suoritetaan edustalla (eli istunnossa). Käyttöön myös taustalla, Lisää:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Kun sovellus toimii taustalla, vain verkon perusteella sijainnit raportoidaan, vaikka GPS käytössä.

Tämä funktio soveltaminen kutsuu [startMonitoringSignificantLocationChanges] , kun sovellus siirtyy taustan. Huomaa, että se automaattisesti käynnistää uudelleen jakaa sovelluksen taustan Jos uuden sijainnin tapahtuman saapuu.

##<a name="advanced-reporting"></a>Laajennettu raportointi

Voit myös halutessasi sovelluksen tiettyjä tapahtumia, virheet ja töiden raportoimaan sinun on käytettävä välitys Ohjelmointirajapinnan kautta menetelmät `EngagementAgent` luokka. Objektin tämän luokan voi hakea soittamalla `[EngagementAgent shared]` staattinen menetelmä.

Välitys Ohjelmointirajapinnan avulla voit käyttää kaikkia välitys 's lisäominaisuuksia ja miten yksityiskohtaisesti välitys Ohjelmointirajapinnan käyttäminen iOS (sekä teknisten Valitse `EngagementAgent` luokan).

##<a name="disable-idfa-collection"></a>IDFA sivustokokoelman poistaminen käytöstä

Oletusarvon mukaan välitys käyttämällä [IDFA] yksilöivät käyttäjän. Mutta jos et käytä mainonta muualla-sovelluksessa, olet ehkä hylännyt App Store Tarkista prosessin. IDFA sivustokokoelman voidaan poistaa käytöstä lisäämällä Esikäsittelijän makron `ENGAGEMENT_DISABLE_IDFA` pch-tiedostossa (tai `Build Settings` sovelluksesi). Näin varmistat, että ei ole viittauksia `ASIdentifierManager`, `advertisingIdentifier` tai `isAdvertisingTrackingEnabled` sovelluksen-versiossa.

Integrointi **prefix.pch** tiedostossa:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Voit varmistaa, että IDFA sivustokokoelman oikein lakkautetaan sovelluksessa valitsemalla välitys testi lokit. Katso integrointi testata\<ios-sdk-välitys-testi-idfa\> lisätietojen ohjeissa.

##<a name="disable-log-reporting"></a>Lokitiedoston raportoinnin poistaminen käytöstä

### <a name="using-a-method-call"></a>Menetelmä puhelun

Jos halua enää halua lähettää lokit välitys, voit soittaa:

    [[EngagementAgent shared] setEnabled:NO];

Tämä kutsu on jatkuva: se käyttää `NSUserDefaults` tietojen tallentamista varten.

Voit ottaa lokin raportoinnin uudelleen soittamalla kanssa samaan toimintoon `YES`.

### <a name="integration-in-your-settings-bundle"></a>Asetukset-pikaoppaista integrointi

Sen sijaan, että puhelut tätä toimintoa, voit myös integroida tämän asetuksen suoraan-nykyinen `Settings.bundle` tiedosto. Merkkijonon `engagement_agent_enabled` on käytettävä asetus-tunnus ja sen on oltava liitetty vaihtopainikkeen (`PSToggleSwitchSpecifier`).

Seuraavassa esimerkissä `Settings.bundle` näyttää, miten se:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Laitteen Ohjelmointirajapinta]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
