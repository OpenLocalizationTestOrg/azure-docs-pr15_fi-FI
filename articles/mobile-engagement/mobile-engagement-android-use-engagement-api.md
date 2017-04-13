<properties
    pageTitle="Välitys API käyttämisestä Android-laitteeseen"
    description="Uusimman Android SDK - välitys API käyttämisestä Android-laitteeseen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Välitys API käyttämisestä Android-laitteeseen

Tämä asiakirja on lisäosaa asiakirjaan [Android Mobile välitys SDK Advanced raportoinnin asetukset](mobile-engagement-android-advanced-reporting.md). Se sisältää syvyys tietojen välitys Ohjelmointirajapinnan käyttäminen ja raportoi sovellusta tilastotiedot.

Ota huomioon, jos haluat vain varauksia raportin sovelluksen istunnot, aktiviteetteja, kaatuu ja teknisiä tietoja, valitse helpoin tapa on kaikkien oman `Activity` aliraportti luokat perivät vastaavan `EngagementActivity` luokan.

Jos haluat käyttää lisätoimintoja, esimerkiksi jos sinun on ilmoitettava sovelluksen tiettyjä tapahtumia, virheet ja töitä, tai jos tietokoneeseesi on ilmoittamaan sovelluksen toimintoja eri tavalla kuin se käytössä `EngagementActivity` luokkien, sinun on käytettävä välitys Ohjelmointirajapinnan.

Välitys Ohjelmointirajapinnan tarjoaa `EngagementAgent` luokka. Tämä luokan esiintymä voi hakea soittamalla `EngagementAgent.getInstance(Context)` staattinen menetelmä (Huomaa, että `EngagementAgent` palautti objekti on Yksiarvoinen).

##<a name="engagement-concepts"></a>Välitys käsitteitä

Seuraavien osien Tarkenna yleisiä [Mobile välitys käsitteistä](mobile-engagement-concepts.md), Android-ympäristöön.

### <a name="session-and-activity"></a>`Session`ja`Activity`

Jos käyttäjä pysyy käyttämättömänä kahdella *toimintojen*välinen yli muutaman sekunnin, hänen *toimenpiteiden* järjestys on jaettu kaksi eri *istuntoja*. Nämä hetkinen kutsutaan "istunnon aikakatkaisu".

*Tehtävän* liittyy yleensä yhden näytön sovelluksen, toisin sanoen *tehtävän* käynnistyy, kun näyttöön tulee näkyviin ja lopettaa näytön ollessa suljettuna: Tämä on sen jälkeen tapausta, kun välitys SDK on integroitu avulla `EngagementActivity` luokat.

Mutta *toimintoja* voidaan ohjata myös manuaalisesti välitys Ohjelmointirajapinnan avulla. Näin voit jakaa useita sub osia tarkempia tietoja (esimerkiksi, kuinka usein tunnetut ja kuinka kauan valintaikkunat käytetään tämän näytön sisällä) tämän näytön käyttö tietyn näyttöön.

##<a name="reporting-activities"></a>Raportoinnin toiminnot

> [AZURE.IMPORTANT] Sinun ei tarvitse tehdä tässä osassa kuvatut, jos käytössäsi on toimintoja, kuten raportin `EngagementActivity` luokan ja sen muunnosten Android asiakirjan miten voit integroida välitys esitetyllä.

### <a name="user-starts-a-new-activity"></a>Käyttäjä aloittaa uusi tehtävä

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Soita `startActivity()` aina, kun käyttäjän toiminnan muutokset. Tämän funktion ensimmäinen kutsu aloittaa uuden käyttäjäistunnon.

Hyvä Soita Tämä funktio on kunkin tehtävän `onResume` takaisinsoitto.

### <a name="user-ends-his-current-activity"></a>Käyttäjän päättyy hetkinen toiminta

            EngagementAgent.getInstance(this).endActivity();

Soita `endActivity()` vähintään kerran, kun käyttäjä loppuu viimeisen toimintaansa. Näin tiedät, että käyttäjä ei tällä hetkellä käyttämättömänä ja että käyttäjäistunto on suljettava kerran istunnon aikakatkaisun päättyy välitys SDK (Jos soitat `startActivity()` istunnon aikakatkaisun vanhenee, istunnon jatketaan yksinkertaisesti).

Hyvä Soita Tämä funktio on kunkin tehtävän `onPause` takaisinsoitto.

##<a name="reporting-events"></a>Tapahtumien raportointi

### <a name="session-events"></a>Istunnon tapahtumat

Istunnon tapahtumat käytetään yleensä raportin hänen istunnon aikana käyttäjän toimintoja.

**Esimerkki ilman ylimääräisiä tietoja:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Esimerkki, jossa lisätiedot:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Erillisestä tapahtumat

Toisin kuin istunnon tapahtumat erillinen tapahtumat voi ilmetä ulkopuolella istunnon yhteydessä.

**Esimerkki:**

Oletetaan, että raportin tapahtumien lähetyksen vastaanotin käynnistyy, kun haluat:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Virheiden raportoiminen

### <a name="session-errors"></a>Istunnon virheet

Istunnon virheitä käytetään yleensä vaikuttavat käyttäjän hänen istunnon aikana virheiden raportoiminen.

**Esimerkki:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Erillisestä virheet

Toisin kuin istunnon virheet erillinen virheitä voi ilmetä ulkopuolella istunnon yhteydessä.

**Esimerkki:**

Seuraavassa esimerkissä esitetään, miten ilmoittaa virheestä, kun muistin tulee pieni puhelimen sovelluksen prosessi on käynnissä.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Töiden raportointi

### <a name="example"></a>Esimerkki

Oletetaan, että haluat ilmoittaa kirjautuminen prosessin kestoa:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Projektin aikana virheistä

Käynnissä olevan projektin sijaan, että liittyvät käyttäjäistunnon voi liittyä virheitä.

**Esimerkki:**

Oletetaan, että raportoitava virheen aikana voit kirjautua sisään prosessi:

[...] julkinen void kirjautumisikkuna (konteksti konteksti,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Raportoinnin tapahtumia projektin aikana

Käynnissä olevan projektin sijaan, että liittyvät käyttäjäistunnon voi liittyä tapahtumat.

**Esimerkki:**

Oletetaan, että olemme yhteisöpalvelun ja Käytämme raporttiin työn kokonaisaika, jona käyttäjä on yhteydessä palvelimeen. Käyttäjä voi pitää yhteyttä jopa taustalla kun hän on käytössä toisessa sovelluksessa tai puhelimen Nukkuva, jotta istuntoja ei ole.

Käyttäjä voi vastaanottaa viestejä hänen ystävien, työ-tapahtuma.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Ylimääräisen parametrit

Satunnaisia tietoja voidaan liittää tapahtumia, virheitä, tehtävät ja työt.

Nämä tiedot on järjestetty, se käyttää Android's Pikaoppaista luokan (Kyllä, se toimii kuten Android sovituksen ylimääräisiä parametrien). Huomaa, että Pikaoppaista voi olla matriiseja tai toisen Pikaoppaista esiintymät.

> [AZURE.IMPORTANT] Jos lisäät parcelable tai voi sarjoittaa parametreja, varmista, että niiden `toString()` menetelmää on toteutettu luettavassa merkkijono. Voi sarjoittaa luokat, jotka sisältävät ei lyhytkestoisia kentät, jotka eivät voi sarjoittaa tekee Android kaatumisen, kun aiot soittaa`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Ylimääräisen parametrien lyhyet Matriisit eivät ole tuettuja, eli se ei voi muuntaa sarjaksi matriisina. Sinun pitäisi muuntaa ne vakio matriiseja ennen kuin käytät sitä ylimääräiset parametrit.

### <a name="example"></a>Esimerkki

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Kunkin avain `Bundle` on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

**Vähintään 1 024** merkkiä (kerran koodattu JSON välitys-palvelu) puhelun kokorajoituksena lisäominaisuudet.

Edellisen esimerkin palvelimeen lähetetään JSON on 58 merkkiä:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Sovelluksen raportointiin

Voit raportoida manuaalisesti käyttämällä seurannan tiedot (tai muut sovelluksen tietyt tiedot) `sendAppInfo()` funktio.

Huomaa, että nämä tiedot voidaan lähettää vaiheittainen: laitteen säilytetään vain tietyn avaimen uusimman arvo.

Tapahtuman lisäominaisuudet, kuten Pikaoppaista luokan käytetään abstrakti hakemuksen tiedot, Huomaa, että matriiseja tai aliraportti nippujen käsitellään tasainen merkkijonot (joko JSON Sarjatoiminto).

### <a name="example"></a>Esimerkki

Lähettäminen käyttäjän sukupuoli- ja Syntymäpv-kenttien koodin otoksen näin:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Rajoitukset

#### <a name="keys"></a>Näppäimet

Kunkin avain `Bundle` on vastattava seuraavalla lausekkeella:

`^[a-zA-Z][a-zA-Z_0-9]*`

Se tarkoittaa sitä, että näppäimet alussa on oltava vähintään yksi kirjain, kirjaimia, numeroita ja alaviivoja (\_).

#### <a name="size"></a>Kokoa

Hakemuksen tiedot on oikeutettu enintään **1 024** merkkiä (kerran koodattu JSON välitys-palvelu) puhelun.

Edellisen esimerkin palvelimeen lähetetään JSON on 44 merkkiä:

            {"expiration":"2016-12-07","status":"premium"}
