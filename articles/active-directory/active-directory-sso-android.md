<properties
    pageTitle="Ottamisesta käyttöön rajat-sovelluksen SSO käyttämällä ADAL Android | Microsoft Azure"
    description="Miten ADAL SDK ominaisuuksien avulla voit ottaa käyttöön kertakirjautumisen sovellusten välillä. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Rajat-sovelluksen SSO Android-käyttämällä ADAL ottaminen käyttöön


Antamalla Single Sign-On (SSO) niin, että käyttäjillä on vain syöttää tunnistetietoja kerran ja tunnistetiedot on automaattisesti työskentelet yli sovellusten odotetaan nyt asiakkaiden mukaan. Ongelmia ovat antamalla käyttäjänimi ja salasana ja pieneen näyttöön, usein kertaa yhdistettynä puhelun tai texted koodi, kuten muita kerroin (2FA) tulokset nopeasti tyytymättömyyden, jos käyttäjä on voit tehdä tämän tuotteen useammin kuin kerran.

Lisäksi voit hyödyntää identity-ympäristö, kuten Microsoft Accounts tai työpaikan Office 365-voi käyttää muissa sovelluksissa, jos asiakkaiden odottaa, että ne tunnistetiedot käytettävissä paikasta kaikista verkkosovelluksistaan riippumatta siitä, toimittajan.

Microsoft Identity-ympäristö, tutustu Microsoft Identity SDK: T, sekä tekee tämän työtä, ja voit ominaisuuden, jonka avulla voit ilahduttaa SSO asiakkaiden joko oman-ohjelmistopaketin tai -broker ominaisuuksien ja todentaja sovellusten välillä koko laite.

Tätä vaiheittaista kertoo Microsoftin tarjota tämä etu asiakkaillesi sovelluksessa SDK määrittämisestä.

Tätä vaiheittaista koskee:

* Azure Active Directory
* Azure Active Directory-B2C
* Azure Active Directory-B2B
* Azure Active Directory ehdollisen käyttöoikeuden

Huomaa, että alla asiakirjan olettaa voit tietävän siitä, miten [säännöstä sovellusten aiempien versioiden Azure Active Directory-portaalissa](./develop/active-directory-how-to-integrate.md) sekä sovelluksesi on integroitu [Microsoft Identity Android SDK: ssa](https://github.com/AzureAD/azure-activedirectory-library-for-android).

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

Broker avustaa kirjautumiset ovat kirjautuminen kokemukset, ilmenee broker-sovelluksesta ja jaa tunnistetiedot laitteeseen kaikissa sovelluksissa, jotka käyttävät Microsoft Identity-ympäristö, tallentamista ja suojauksen välittäjän avulla. Tämä tarkoittaa, että sovelluksesi ovat riippuvaisia välittäjän kirjautuminen käyttäjille. IOS- ja Android-ne ovat käytettävissä ladattavan sovellukset, että asiakkaiden Asenna itsenäisesti tai voivat sijaita laitteeseen yritys, joka hallinnoi niiden käyttäjän laite. Esimerkki tällaista sovellus on iOS Azure todentaja-sovellus. Windows-toiminto tarjoaa muodostettu käyttöjärjestelmä tunnetut kuin Web-todennus Broker tarkalleen ottaen huomioon-valitsin.
Kokemus vaihtelee ympäristössä ja joskus voi olla häiritä käyttäjien jollei hallitun oikein. Olet todennäköisesti eniten käyttänyt tätä mallia, jos tietokoneeseen on asennettu Facebook-sovelluksen ja Facebook-kirjautuminen toimintojen käyttäminen toisessa sovelluksessa. Microsoft Identity-ympäristö hyödyntää samoissa.

IOS "siirtymätehosteen" johtaa animaatio, jossa sovelluksen lähetetään Azure todentaja sovellukset ja tausta on edustaa käyttäjä voi valita mitä ne haluat kirjautumiseen tiliä.  

Android-ja Windows tilin valitseminen näkyy sovelluksesi, joka on pienempi häiritä käyttäjän päällä.

#### <a name="how-the-broker-gets-invoked"></a>Miten välittäjän saa kutsua

Jos yhteensopiva broker asennetaan laitteeseen Azure todentaja-sovelluksessa, kuten Microsoft Identity SDK: T tehdä automaattisesti käynnistettäessä broker puolestasi, kun käyttäjä ilmaisee he haluavat Kirjaudu sisään käyttämällä Microsoft Identity-ympäristön kaikki tilin työmäärä. Tämä voi johtua omalla Microsoft-Account, on työpaikan tai oppilaitoksen tilille tai tiliä helposti antaa ja isännöi Azure käyttämällä B2C ja B2B tuotteitamme. Erittäin suojatun algoritmit ja salauksen avulla varmistamme tunnistetiedot ovat pyytänyt ja toimitettu takaisin sovelluksen turvallisesti. Menetelmien tarkat tekniset tiedot ei ole julkaistu, mutta se on kehitetty ja yhteistyön Apple ja Google.

**Kehittäjä on valinta, jos Microsoft Identity SDK soittaa välittäjän tai käyttää-broker maksullista työnkulku.** Jos kehittäjä valitsee Käytä broker avustaa kulun ne menettää kuitenkin etua hyödyntäminen SSO-tunnistetiedot, että käyttäjä voi jo lisännyt laitteeseen sekä estää niiden sovelluksen ominaisuuksien Microsoft toimittaa ehdollisen käyttöoikeuden Intune hallintatoiminnot, kuten asiakkaiden kanssa käytössä ja varmenteeseen perustuva todentaminen.

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

Käyttämällä tätä ADAL Android SDK-paketissa, voit:

- Ota käyttöön-broker maksullista SSO sovellukset-3fa7
- Broker avustaa SSO tuen ottaminen käyttöön


### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Palauttaa kertakirjautumisen ei broker avustaa SSO

Broker sovellusten maksullista SSO Microsoft Identity SDK: T hallinta paljon SSO monimutkaisuutta puolestasi. Tämä sisältää etsiminen oikean käyttäjän välimuistin ja ylläpito kirjautuneena sisään käyttäjien kysely-luettelo.

Jotta Kertakirjautumista sovellusten omistat haluat seuraavasti:

1. Varmista kaikki sovellukset-käyttäjän samaan Ostajantunnus tai sovelluksen tunnus.
* Varmista kaikkien sovellusten SharedUserID samat.
* Varmista, että kaikki sovelluksesi jakaa saman allekirjoitusvarmenteen Google Play-kaupasta niin, että voit jakaa tallennustilan.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Vaihe 1: Käyttämällä samaa Ostajantunnus / sovelluksen tunnus olevien sovellusten kaikki sovellukset

Jotta Microsoft Identity-ympäristössä, jotta tiedät, että se on oikeus Jaa tunnusten sovellustesi kaikkien sovellustesi on jaettava samaan Ostajantunnus tai sovelluksen tunnus. Tämä on yksilöllinen, antamien tietojen ensimmäisen sovelluksen rekisteröity portaalissa.

Mutta miten tunnistaa eri sovellusten Microsoft Identity-palveluun, jos se on käytössä sama sovelluksen tunnus Vastaus on **Uudelleenohjata URI**. Jokainen sovellus voi olla useita uudelleenohjata URI rekisteröity onboarding-portaalissa. Kunkin sovelluksen avulla, on eri uudelleenohjaus URI. Alla on esimerkki siitä, miten tämä näyttää:

App1 uudelleenohjaus URI:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 uudelleenohjaus URI:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 uudelleenohjaus URI:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

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


#### <a name="step-2-configuring-shared-storage-in-android"></a>Vaihe 2: Määrittäminen jaettua tallennustilan Android

Asetus `SharedUserID` ei käsitellä tämän asiakirjan, mutta voit opitut asiat lukemalla Google Android ohjeet [luettelon](http://developer.android.com/guide/topics/manifest/manifest-element.html). Mikä on tärkeää on, että voit päättää, mitä oman sharedUserID kutsutaan ja käyttää sitä kaikkien sovellusten välillä.

Kun sinulla `SharedUserID` kaikissa sovelluksissa ovat käyttövalmiita SSO.

> [AZURE.WARNING]
Kun jaat tallennustilan sovellusten välillä jokin sovellus voit käyttäjien poistaminen tai poistaa huonoin sovelluksen kautta tunnusten. Tämä on erityisen suuria, jos sinulla on sovelluksia, jotka käyttävät tunnuksia, tausta työmäärä. Tallennustilan jakaminen tarkoittaa, että sinun on oltava hyvin varovainen kaikista Poista-toiminnot – Microsoft Identity SDK: T.

Joka on tämä! Microsoft Identity SDK nyt jakaa tunnistetiedot kaikki sovellukset. Käyttäjä-luettelo myös jakaa Palvelusovellusten esiintymät.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Palauttaa kertakirjautumisen broker avustaa SSO

Sovelluksen käyttämään broker, johon on asennettu laitteessa on **oletusarvoisesti pois käytöstä**. Jotta voit käyttää sovelluksen välittäjän tekemään joitakin lisämäärityksiä ja Lisää koodia sovelluksen.

Toimet, joiden on:

1. Sovelluksen-koodin kutsussa MS SDK broker-tila käyttöön
2. Hae uusi uudelleenohjaus URI verovapautta toimittamalla, sovelluksen ja sovelluksen rekisteröinti
3. Android-luettelo on riittävät käyttöoikeudet on määritetty


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Vaihe 1: Broker tila-sovelluksen käyttöön
Mahdollisuus käyttää välittäjän sovelluksen on otettu käyttöön, kun luot "asetukset"- tai ensimmäisen määrityskerran todennus-esiintymä. Voit tehdä tämän määrittämällä koodissa ApplicationSettings tyyppi:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Vaihe 2: Muodostaa uusi uudelleenohjaus URI kanssa URL-malli

Jotta voit varmistaa, että aina palautettavien tunnistetiedon tunnusten oikein, annettava Varmista, että olemme soittaa takaisin sovelluksen niin, että Android käyttöjärjestelmä, jota voit tarkistaa. Android-käyttöjärjestelmän käyttää varmenteen hash Google Play-kaupasta. Tämä et voi väärentää päivittämättömien-sovelluksessa. Tämän vuoksi olemme hyödyntää tämän sekä varmistaa, että paikkamerkkejä palautetaan oikein broker Microsoftin sovelluksen URI. Olemme edellyttää, että muodostamalla tämän yksilöllinen uudelleenohjaus URI sekä sovelluksessa ja Aseta uudelleenohjata URI, tutustu developer-portaalissa.

Uudelleenohjauksen URI edellyttää, että erisnimen muodossa:

`msauth://packagename/Base64UrlencodedSignature`

ex: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Tämä uudelleenohjata URI on määritettävä [Azure perinteinen portal](https://manage.windowsazure.com/)-sovellus rekisteröinti. Saat lisätietoja Azure AD-sovelluksen rekisteröinnin [integrointi Azure Active Directory-hakemistosta](./develop/active-directory-how-to-integrate.md).


#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Vaihe 3: Määritä sovelluksen asianmukaisia käyttöoikeuksia

Microsoftin broker-sovelluksen Android-sovellusten tunnistetietojen hallinta Android OS Tilienhallinta-toiminnon avulla. Jotta voit käyttää välittäjän Android sovelluksen luettelo on oltava oikeudet käyttää AccountManager tilit. Tämä kerrotaan yksityiskohtaisesti asiakirjoissa [Google Account Manager tähän] (http://developer.android.com/reference/android/accounts/AccountManager.html)

Erityisesti ovat seuraavat oikeudet:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Olet määrittänyt SSO!

Nyt Microsoft Identity SDK automaattisesti sekä jakaa tunnistetiedot sovellustesi ja käynnistää välittäjän, jos se ei sisällä tietoja niiden laitteessa.
