<properties
    pageTitle="Ottamisesta käyttöön rajat-sovelluksen SSO iOS käyttämällä ADAL | Microsoft Azure"
    description="Miten ADAL SDK ominaisuuksien avulla voit ottaa käyttöön kertakirjautumisen sovellusten välillä. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>IOS käyttämällä ADAL SSO rajat-sovelluksen ottaminen käyttöön


Antamalla Single Sign-On (SSO) niin, että käyttäjillä on vain syöttää tunnistetietoja kerran ja tunnistetiedot on automaattisesti työskentelet yli sovellusten odotetaan nyt asiakkaiden mukaan. Ongelmia ovat antamalla käyttäjänimi ja salasana ja pieneen näyttöön, usein kertaa yhdistettynä puhelun tai texted koodi, kuten muita kerroin (2FA) tulokset nopeasti tyytymättömyyden, jos käyttäjä on voit tehdä tämän tuotteen useammin kuin kerran.

Lisäksi voit hyödyntää identity-ympäristö, kuten Microsoft Accounts tai työpaikan Office 365-voi käyttää muissa sovelluksissa, jos asiakkaiden odottaa, että ne tunnistetiedot käytettävissä paikasta kaikista verkkosovelluksistaan riippumatta siitä, toimittajan.

Microsoft Identity-ympäristö, tutustu Microsoft Identity SDK: T, sekä tekee tämän työtä, ja voit ominaisuuden, jonka avulla voit ilahduttaa asiakkaisiin ja Kertakirjautuminen joko oman-ohjelmistopaketin tai -broker ominaisuuksien ja todentaja sovellusten välillä koko laite.

Tämä vaiheittainen kuvaus kertoo Microsoftin tarjota tämä etu asiakkaisiin sovelluksessa SDK määrittämisestä.

Tätä vaiheittaista koskee:

* Azure Active Directory
* Azure Active Directory-B2C
* Azure Active Directory-B2B
* Azure Active Directory ehdollisen käyttöoikeuden


Huomaa, että alla asiakirjan olettaa voit tietävän siitä, miten [säännöstä sovellusten aiempien versioiden Azure Active Directory-portaalissa](./develop/active-directory-how-to-integrate.md) sekä sovelluksesi on integroitu [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>SSO käsitteiden Microsoft Identity-ympäristössä

### <a name="microsoft-identity-brokers"></a>Microsoft Identity vakuutuksenvälittäjän

Microsoft tarjoaa jokaisen mobiilisovellusten ympäristön sovelluksia, jotka sallivat tunnistetiedot korjaamaan sovellusten eri toimittajilta sekä erityisiä ominaisuuksia, jotka edellyttävät yksittäisen turvallisessa paikassa mistä vahvistamiseen tunnistetietojen avulla. Olemme Soita nämä **vakuutuksenvälittäjän**. IOS- ja Android-nämä ovat käytettävissä ladattavan sovellukset, että asiakkaiden Asenna itsenäisesti tai voivat sijaita yritys, joka hallinnoi jotkin tai kaikki laitteen niiden työntekijän laite. Nämä vakuutuksenvälittäjän tuki hallinta suojauksen vain sovelluksia tai koko laite IT-järjestelmänvalvojien haluavat perusteella. Windows-toiminto tarjoaa muodostettu käyttöjärjestelmä tunnetut kuin Web-todennus Broker tarkalleen ottaen huomioon-valitsin.

Ymmärtää, kuinka Käytämme näitä vakuutuksenvälittäjän ja miten asiakkaille, voivat tarkastella niitä niiden kirjautuminen-työnkulussa, saat lisätietoja Microsoft Identity-ympäristön.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Kuvioiden määrittäminen mobiililaitteissa kirjautuminen

Tunnistetietojen laitteissa pääsy noudattamalla kaksi basic kuviot Microsoft Identity-ympäristöön:

* Muut broker maksullista kirjautumiset
* Broker maksullista kirjautumiset

#### <a name="non-broker-assisted-logins"></a>Muut broker maksullista kirjautumiset

Muut broker maksullista kirjautumiset ovat kirjautuminen kokemukset käydä sovelluksen tekstiosassa ja käyttää paikallisen säilön sovelluksen laitteeseen. Tämä voi jakaa muiden sovellusten, mutta tunnistetiedot ovat tiiviisti sidottu sen sovelluksen tai sovellusten avulla, että credential suite. Tämä on kokemus, olet todennäköisesti kokenut monet mobile-sovellukset, johon kirjoitat käyttäjänimen ja salasanan sovelluksesta.

Nämä kirjautumiset on seuraavat edut:

-  Käyttäjäkokemus on täysin-sovelluksesta.
-  Tunnistetietojen voidaan jakaa sovelluksia, jotka on allekirjoitettu parantamiseen yksittäisen Sign avulla voit olevien sovellusten varmennetta.
-  Ohjausobjektin ympärille kokemukset kirjautumisesta on annettu ennen ja jälkeen-kirjautuminen-sovellukseen.

Nämä kirjautumiset ovat seuraavat haitoista:

- Käyttäjä ei voi ilmetä single-etumerkin yli kaikki sovellukset, jotka käyttävät Microsoft-Identity vain ne, jotka ovat sovelluksen omistaa ja määrittänyt Microsoft Identities kautta.
- Sovellus ei voi käyttää monimutkaisemman business-ominaisuuksia, kuten ehdollisen käyttöoikeuden tai käytä tuotteiden InTune-ohjelmiston.
- Sovellus ei tue pohjainen todennus yrityskäyttäjille.

Miten Microsoft Identity SDK: T suoritetaan sovellusten SSO käyttöön jaettua tallennustilan esittäminen näin:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Broker maksullista kirjautumiset

Broker avustaa kirjautumiset ovat kirjautuminen kokemukset, ilmenee broker-sovelluksesta ja jaa tunnistetiedot laitteeseen kaikissa sovelluksissa, jotka käyttävät Microsoft Identity-ympäristö, tallentamista ja välittäjän suojaus avulla. Tämä tarkoittaa, että sovelluksesi ovat riippuvaisia välittäjän kirjautuminen käyttäjille. IOS- ja Android-ne ovat käytettävissä ladattavan sovellukset, että asiakkaiden Asenna itsenäisesti tai voivat sijaita laitteeseen yritys, joka hallinnoi niiden käyttäjän laite. Esimerkki tällaista sovellus on iOS Azure todentaja-sovellus. Windows-toiminto tarjoaa muodostettu käyttöjärjestelmä tunnetut kuin Web-todennus Broker tarkalleen ottaen huomioon-valitsin.
Kokemus vaihtelee ympäristössä ja joskus voi olla häiritä käyttäjien jollei hallitun oikein. Olet todennäköisesti eniten käyttänyt tätä mallia, jos tietokoneeseen on asennettu Facebook-sovelluksen ja Facebook-kirjautuminen toimintojen käyttäminen toisessa sovelluksessa. Microsoft Identity-ympäristö hyödyntää samoissa.

IOS "siirtymätehosteen" johtaa animaatio, jossa sovelluksen lähetetään Azure todentaja sovellukset ja tausta on edustaa käyttäjä voi valita mitä ne haluat kirjautumiseen tiliä.  

Android-ja Windows tilin valitseminen näkyy sovelluksesi, joka on pienempi häiritä käyttäjän päällä.

#### <a name="how-the-broker-gets-invoked"></a>Miten välittäjän saa kutsua

Jos yhteensopiva broker asennetaan laitteeseen Azure todentaja-sovelluksessa, kuten Microsoft Identity SDK: T tehdä automaattisesti käynnistettäessä broker puolestasi, kun käyttäjä ilmaisee he haluavat Kirjaudu sisään käyttämällä Microsoft Identity-ympäristön kaikki tilin työmäärä. Tämä voi johtua omalla Microsoft-Account, on työpaikan tai oppilaitoksen tilin tai tilin, joka saadaan ja isännän käyttäminen B2C ja B2B tuotteitamme Azure-tietokannassa. Erittäin suojatun algoritmit ja salauksen avulla varmistamme tunnistetiedot ovat pyytänyt ja toimitettu takaisin sovelluksen turvallisesti. Menetelmien tarkat tekniset tiedot ei ole julkaistu, mutta se on kehitetty ja yhteistyön Apple ja Google.

**Kehittäjä on valinta, jos Microsoft Identity SDK kutsuu välittäjän tai käyttää-broker maksullista työnkulku.** Jos kehittäjä valitsee Käytä broker avustaa kulun ne menettää kuitenkin etua hyödyntäminen SSO-tunnistetiedot, että käyttäjä voi jo lisännyt laitteeseen sekä estää niiden sovelluksen ominaisuuksien Microsoft toimittaa ehdollisen käyttöoikeuden Intune hallintatoiminnot, kuten asiakkaiden kanssa käytössä ja varmenteeseen perustuva todentaminen.

Nämä kirjautumiset on seuraavat edut:

-  Käyttäjän ilmenee SSO paikasta kaikista verkkosovelluksistaan riippumatta siitä, toimittaja.
-  Sovelluksen voit hyödyntää muut business-ominaisuudet, kuten ehdollisen käyttöoikeuden tai tuotteiden InTune-ohjelmiston.
-  Sovellus tukee pohjainen todennus yrityskäyttäjille.
- Paljon suojattu kirjautuminen sovelluksen ja käyttäjän tunnistetietoja kuin tarkistetaan broker-sovelluksen lisäsuojauksen algoritmit ja salausta.

Nämä kirjautumiset ovat seuraavat haitoista:

- IOS-käyttäjä on transitioned sovelluksen kokemus ulos samalla, kun tunnistetiedot valitaan.
- Voi hallita asiakkaiden sovelluksessa kirjautuminen käyttökokemusta menetyksiä.



Tässä on kuvaus siitä, miten Microsoft Identity SDK: T käyttöön SSO broker-sovellusten käyttäminen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Tietoturvariskien sinun pitäisi ymmärtää ja toteuttaa SSO käyttämällä Microsoft Identity-ympäristön ja SDK: T sovelluksessa paremmin taustan näillä tiedoilla.


## <a name="enabling-cross-app-sso-using-adal"></a>Usean sovelluksen Kertakirjautuminen ADAL ottaminen käyttöön

Tähän Käytämme ADAL iOS SDK avulla:

- Ota käyttöön-broker maksullista SSO sovellukset-3fa7
- Broker avustaa SSO tuen ottaminen käyttöön

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Palauttaa kertakirjautumisen ei broker avustaa SSO

Broker sovellusten maksullista SSO Microsoft Identity SDK: T hallinta paljon SSO monimutkaisuutta puolestasi. Tämä sisältää etsiminen oikean käyttäjän välimuistin ja ylläpito kirjautuneena sisään käyttäjien kysely-luettelo.

Jotta Kertakirjautumista sovellusten omistat haluat seuraavasti:

1. Varmista kaikki sovellukset-käyttäjän samaan Ostajantunnus tai sovelluksen tunnus.
* Varmista, että kaikki sovelluksesi jakaa saman allekirjoitusvarmenteen Applelta niin, että voit jakaa keychains
* Pyydä saman avainnipun oikeuden, kunkin sovellukset.
* Kerro Microsoft Identity SDK: T jaetusta avainnipusta haluat käyttää.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Käyttämällä samaa Ostajantunnus / sovelluksen tunnus olevien sovellusten kaikki sovellukset

Jotta Microsoft Identity-ympäristössä, jotta tiedät, että se on oikeus Jaa tunnusten sovellustesi kaikkien sovellustesi on jaettava samaan Ostajantunnus tai sovelluksen tunnus. Tämä on yksilöllinen, antamien tietojen ensimmäisen sovelluksen rekisteröity portaalissa.

Mutta miten tunnistaa eri sovellusten Microsoft Identity-palveluun, jos se on käytössä sama sovelluksen tunnus Vastaus on **Uudelleenohjata URI**. Jokainen sovellus voi olla useita uudelleenohjata URI rekisteröity onboarding-portaalissa. Kunkin sovelluksen avulla, on eri uudelleenohjaus URI. Alla on esimerkki siitä, miten tämä näyttää:

App1 uudelleenohjaus URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 uudelleenohjaus URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 uudelleenohjaus URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Nämä ovat sisäkkäisiä saman Asiakastunnus-kohdassa / Sovellustunnus ja etsitään uudelleenohjaus URI palaat us SDK-kokoonpanoa perusteella.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Huomautus nämä ohjata URI muoto on selitetty alla. Vähimmäiskeskiarvovaatimuksen ohjata kaikki URI paitsi, jos haluat tukevat broker, jolloin niitä on näyttää suunnilleen edellä*



#### <a name="create-keychain-sharing-between-applications"></a>Luo avainnipun jakaminen sovellusten välillä

Avainnipun jakaminen ottaminen käyttöön on tämän asiakirjan laajemmin ja piiriin Apple-asiakirjan [Ominaisuuksien lisääminen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Mikä on tärkeää on päättää odotusten avainnipussasi kutsuttava ja lisää kyseisen ominaisuus kaikkien sovellusten välillä.

Kun sinulla on oikeudet oikein voit määrittää pitäisi näkyä tiedoston oikeutettu projektin hakemistossa `entitlements.plist` , joka sisältää jotain, joka näyttää seuraavalta:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Kun sinulla on käytössä olevista sovellustesi avainnipun oikeuden ja haluat käyttää SSO, kerro Microsoft Identity SDK avainnipussasi seuraava asetus käyttämällä omaa `ADAuthenticationSettings` seuraavat asetukset:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Avainnipun jakaminen sovellustesi jokin sovellus voit käyttäjien poistaminen tai poistaa kaikki tunnusten huonoin sovelluksen kautta. Tämä on erityisen suuria, jos sinulla on sovelluksia, jotka käyttävät tunnuksia, tausta työmäärä. Avainnipun jakaminen tarkoittaa, että sinun on oltava-kaikista Älä poista toimintojen – Microsoft Identity SDK: T.

Joka on tämä! Microsoft Identity SDK nyt jakaa tunnistetiedot kaikki sovellukset. Käyttäjä-luettelo myös jakaa Palvelusovellusten esiintymät.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Palauttaa kertakirjautumisen broker avustaa SSO

Sovelluksen käyttämään broker, johon on asennettu laitteessa on **oletusarvoisesti pois käytöstä**. Jotta voit käyttää sovelluksen välittäjän tekemään joitakin lisämäärityksiä ja Lisää koodia sovelluksen.

Toimet, joiden on:

1. Sovelluksen-koodin kutsussa MS SDK broker-tila käyttöön.
2. Hae uusi uudelleenohjaus URI verovapautta toimittamalla, sovelluksen ja sovelluksen rekisteröinti.
3. Rekisteröi URL-malli.
4. iOS9 tuki: Lisää käyttöoikeus info.plist tiedostoon.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Vaihe 1: Broker tila-sovelluksen käyttöön
Mahdollisuus käyttää välittäjän sovelluksen on otettu käyttöön, kun luot ensimmäisen määrityskerran todennus-objektin tai "yhteydessä". Voit tehdä tämän määrittämällä tunnistetiedot tyyppi koodi:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Asetus mahdollistaa Microsoft Identity SDK yrittää broker, vastaanottaja `AD_CREDENTIALS_EMBEDDED` Microsoft Identity SDK estää soitat välittäjän.

#### <a name="step-2-registering-a-url-scheme"></a>Vaihe 2: Rekisteröiminen URL-malli
Microsoft Identity-ympäristö käyttää URL-osoitteet kutsua välittäjän ja palata sitten takaisin sovelluksen ohjausobjektiin. Viimeistele kierron, sinun on rekisteröity, Microsoft Identity-ympäristö Lisätietoja sovelluksen URL-malli. Tämä on lisäksi kaikki muut sovelluksen mallit ehkä olet aikaisemmin rekisteröinyt sovelluksen.

> [AZURE.WARNING]
Suosittelemme, että URL-malli varsin yksilöllinen Pienennä toisessa sovelluksessa käyttämällä samaa URL-malli mahdollisuuksia. Apple Pakota yksilöllisyyden URL-mallit, joka on rekisteröity app Storessa.

Alla on esimerkki ulkoasun tämä project-määrittäminen. Voit ehkä tehdä saman myös XCode sekä:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Vaihe 3: Muodostaa uusi uudelleenohjaus URI kanssa URL-malli

Jotta voit varmistaa, että aina palautettavien tunnistetiedon tunnusten oikein, annettava varmistaaksesi, että olemme soittaa takaisin sovelluksen niin, että iOS-käyttöjärjestelmässä voit tarkistaa. IOS-käyttöjärjestelmän raportit Microsoft broker sovellusten kutsumista sen sovelluksen Pikaoppaista tunnus. Tämä et voi väärentää päivittämättömien-sovelluksessa. Tämän vuoksi olemme hyödyntää tämän sekä varmistaa, että paikkamerkkejä palautetaan oikein broker Microsoftin sovelluksen URI. Olemme edellyttää, että muodostamalla tämän yksilöllinen uudelleenohjaus URI sekä sovelluksessa ja Aseta uudelleenohjata URI, tutustu developer-portaalissa.

Uudelleenohjauksen URI edellyttää, että erisnimen muodossa:

`<app-scheme>://<your.bundle.id>`

ex: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Tämä uudelleenohjata URI on määritettävä [Azure perinteinen portal](https://manage.windowsazure.com/)-sovellus rekisteröinti. Saat lisätietoja Azure AD-sovelluksen rekisteröinnin [integrointi Azure Active Directory-hakemistosta](./develop/active-directory-how-to-integrate.md).


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Vaihe 3 a: Lisää uudelleenohjaus URI-sovellus ja keskihajonta-portaalissa tukemaan pohjainen todennus

Varmenteen tukemaan perustuva todentaminen toisen "msauth" on oltava rekisteröity sovelluksesi ja [Azure perinteinen portal](https://manage.windowsazure.com/) käsittelemään todennus, jos haluat lisätä sovelluksen, joka tukee.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

ex: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Vaihe 4: iOS9: sovelluksen kokoonpanon parametrin lisääminen

ADAL käyttää – canOpenURL: tarkistaa, jos välittäjän asennetaan laitteeseen. IOS-9 Apple lukittu mitä kalastelulta sovellus tehdä kyselyn. Sinun on lisättävä "msauth" LSApplicationQueriesSchemes-osaan tekemäsi `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Olet määrittänyt SSO!

Nyt Microsoft Identity SDK automaattisesti sekä jakaa tunnistetiedot sovellustesi ja käynnistää välittäjän, jos se ei sisällä tietoja niiden laitteessa.
