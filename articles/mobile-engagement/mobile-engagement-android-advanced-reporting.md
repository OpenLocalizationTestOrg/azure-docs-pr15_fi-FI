<properties
    pageTitle="Azure Mobile välitys Android SDK raportoinnin Lisäasetukset"
    description="Tässä artikkelissa kuvataan Lisäasetukset raportoinnin sieppaaminen analytics Azure Mobile välitys Android SDK varten"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Laajennettu raportointi Android-välitys

> [AZURE.SELECTOR]
- [Yleinen Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-advanced-reporting.md)

Tässä ohjeaiheessa kerrotaan Lisää raportoinnin skenaarioita Android-sovelluksessa. Voit käyttää näitä asetuksia luotu [Käytön aloittaminen](mobile-engagement-android-get-started.md) -opetusohjelma-sovellukseen.

## <a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Olet suorittanut opetusohjelman oli tahallisesti suoraan ja yksinkertainen, mutta voit valita asetukset ovat Lisäasetukset.

## <a name="modifying-your-activity-classes"></a>Muokkaaminen oman `Activity` luokat

[Opetusohjelman aloittaminen](mobile-engagement-android-get-started.md)kaikki oli tehtävä on Tee oman `*Activity` aliluokkien perivät vastaavan `Engagement*Activity` luokat. Jos vanha tehtävän laajennettu esimerkiksi `ListActivity`, se laajentaa teet `EngagementListActivity`.

> [AZURE.IMPORTANT] Käytettäessä `EngagementListActivity` tai `EngagementExpandableListActivity`, varmista, että jokin kutsu `requestWindowFeature(...);` tehdään ennen kutsun `super.onCreate(...);`, muuten kaatumisen tapahtuu.

Näitä luokkia voit etsiä `src` -kansiossa ja kopioida ne projektiin. Luokkia on myös **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `startActivity()` ja `endActivity()` manuaalisesti

Jos et halua ylikuormittaa tai ei onnistu oman `Activity` luokkia, voit aloittaa ja lopettaa toimintojen soittamalla sen sijaan `EngagementAgent`'s menetelmiä suoraan.

> [AZURE.IMPORTANT] Android SDK koskaan soittaa `endActivity()` menetelmä myös silloin, kun sovellus suljetaan (Android-sovellukset on aina suljettu). Joten se on *erittäin* suositeltavaa Soita `startActivity()` menetelmä `onResume` takaisinsoitto *All* toimintosi ja `endActivity()` menetelmä `onPause()` takaisinsoitto *All* toimintojen. Tämä on ainoa tapa toiseen ja varmistamalla, että istunnot ei vuotamaan. Jos istunnon vuotamaan, välitys-palvelun katkaisee välitys Taustajärjestelmä koskaan (koska palvelun edelleen liitettynä, kunhan istunnon odottaa).

Tässä on esimerkki:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Tässä esimerkissä on samanlainen kuin `EngagementActivity` luokan ja sen muunnokset, joiden lähdekoodi on säädetty `src` kansio.

## <a name="using-applicationoncreate"></a>Application.onCreate() käyttäminen

Voit lisätä koodia `Application.onCreate()` ja muiden sovellusten takaisinkutsuja Suorita kaikki sovelluksesi kannalta, mukaan lukien välitys-palvelun. Se voi olla ei-toivottuja puoli tehosteita, kuten tarpeettomat muistin kohdistukset ja viestiketjuissa siirtyminen välitys prosessin tai kaksoiskappaleiden lähetyksen vastaanottajia tai palvelut.

Jos voit ohittaa `Application.onCreate()`, suosittelemme seuraavia koodikatkelman lisääminen alussa oman `Application.onCreate()` funktiota:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Voit tehdä saman myös `Application.onTerminate()`, `Application.onLowMemory()`, ja `Application.onConfigurationChanged(...)`.

Voit myös laajentaa `EngagementApplication` sijaan laajentaminen `Application`: takaisinkutsu `Application.onCreate()` onko prosessi-valintaruutu ja puhelut `Application.onApplicationProcessCreate()` vain, jos nykyinen prosessi ei ole välitys isännöintipalvelu yksi, muut takaisinkutsuja sovelletaan samoja sääntöjä.

## <a name="tags-in-the-androidmanifestxml-file"></a>Tunnisteiden AndroidManifest.xml-tiedoston avulla

Palvelun tunnisteen AndroidManifest.xml-tiedostossa `android:label` määritteen avulla voit valita välitys-palvelun nimi sellaisena kuin se näkyy käyttäjille "Servicesiä" näytössä puhelimeensa. Suosittelemme tämän määritteen asettaminen `"<Your application name>Service"` (esimerkiksi `"AcmeFunGameService"`).

Määrittämällä `android:process` määrite varmistaa, että välitys-palvelun suorittaa omassa prosessissa (käynnissä samoja ohjeita välitys, kun sovellus on pää/Käyttöliittymän viestiketjun mahdollisesti vähemmän vastaa).

## <a name="building-with-proguard"></a>Rakennuksen ProGuard kanssa

Voit luoda oman sovelluspaketin ProGuard kanssa, jos haluat säilyttää jotkin luokat. Voit käyttää seuraavia määritysten koodikatkelman:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
