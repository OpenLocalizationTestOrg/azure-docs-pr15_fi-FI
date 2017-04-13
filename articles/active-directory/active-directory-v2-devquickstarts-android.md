<properties
    pageTitle="Azure Active Directory v2.0 Android-sovelluksen | Microsoft Azure"
    description="Miten voit luoda Android-sovelluksen, joka kirjautuu käyttäjät, joilla on sekä henkilökohtainen Microsoft-tili ja työpaikan tai oppilaitoksen tilit ja kutsujen Graph-Ohjelmointirajapinnan kolmannen osapuolen kirjastojen avulla."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Kirjaudu sisään Android-sovelluksen käyttäminen Graph-Ohjelmointirajapinnan käyttäminen v2.0 päätepisteen kolmannen osapuolen kirjaston lisääminen

Microsoft identity-ympäristössä käyttää avoimia standardeja, kuten OAuth2 ja OpenID yhteyden. Kehittäjät voivat käyttää kaikissa kirjastoissa he haluavat integroida palveluiden kanssa. Jotta sovelluskehittäjät Microsoftin ympäristö käyttäminen muita kirjastoja, että olet kirjoittanut muutama vaihe vaiheelta, kuten näytettyjä osoittamaan määrittäminen kolmannen osapuolen kirjastot, jos haluat muodostaa yhteyden Microsoft identity-ympäristössä. Useimmat kirjastoissa, jotka toteuttavat [RFC6749 OAuth2 määritys](https://tools.ietf.org/html/rfc6749) muodostaa yhteyden Microsoft identity-ympäristössä.

Käyttäjät voivat, joka luo tätä vaiheittaista sovelluksessa organisaation kirjautuminen ja Hae itse organisaation Graph-Ohjelmointirajapinnan käyttämällä.

Jos ole ennen käyttänyt OAuth2 tai OpenID muodosta, paljon otoksen määritysten saattaa ei tulkita sinulle. On suositeltavaa lukea [2.0 protokollat - OAuth 2.0 luvan koodin Flow](active-directory-v2-protocols-oauth-code.md) taustan.

> [AZURE.NOTE] Microsoftin ympäristö ominaisuuksia, joilla lausekkeen OAuth2 tai OpenID muodosta standardeja, kuten ehdollisen käyttöoikeuden ja Intune hallinnassa edellyttää Microsoftin avoimen lähteen Microsoft Azure tunnistetietojen kirjastojen avulla.

V2.0 päätepiste ei tue kaikkia Azure Active Directory-skenaariot ja toiminnot.

> [AZURE.NOTE] Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Lataa koodin GitHub
Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) tai Kloonaa rakenne:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Voit ladata yksinkertaisesti otosten ja aloittaa saman tien:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Uuden sovelluksen luominen [sovelluksen rekisteröinnin portaalissa](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)osoitteessa tai [rekisteröimään sovelluksen v2.0 päätepisteissä](active-directory-v2-app-registration.md)yksityiskohtaiset noudattamalla.  Varmista, että:

- Kopioi **Tunnus** , jolla määritetään sovelluksen, sillä tarvitset sitä pian.
- Lisää, kun sovellus **Mobile** -ympäristössä.

> Huomautus: Sovelluksen rekisteröinti-portaalissa on **Uudelleenohjata URI** -arvoa. Tässä esimerkissä on avulla kuitenkin oletusarvon `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Lataa kolmannen osapuolen NXOAuth2-kirjaston ja työtilan luominen

Näiden vaiheiden käytetään OIDCAndroidLib-GitHub on OpenID yhteyden koodi Google OAuth2-kirjastoon. Se toteuttaa alkuperäisen sovelluksen profiilin ja tukee käyttäjän luvan päätepisteelle. Nämä ovat kaikki asioista, jotka sinun täytyy integroida Microsoft identity-ympäristössä.

Kloonaa OIDCAndroidLib repo tietokoneeseen.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Android Studiossa määrittäminen

1. Android Studio uuden projektin luominen ja hyväksy oletusarvot ohjatun toiminnon.

    ![Luo uusi projekti Android Studiossa](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Kohteen Android-laitteille](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Tehtävän lisääminen mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Määrittämään projektin moduulit siirtää kloonatun repo projektin sijainti. Voit myös luoda projektin ja Kloonaa se suoraan projektin haluamaasi kohtaan.

    ![Project-moduulit](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Avaa project-moduulit asetukset käyttämällä pikavalikon tai Ctrl + Alt + Maj + S-pikanäppäimellä.

    ![Projektin moduulit asetukset](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Poistaa oletusarvon app moduuli, koska haluat projektin säilö-asetukset.

    ![Oletus-sovelluksen moduuli](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Tuo kloonatun repo moduulit nykyiseen projektiin.

    ![Projektin tuominen gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![Luo uusi moduuli-sivu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Toista nämä vaiheet `oidlib-sample` moduuli.

7. Tarkasta oidclib riippuvuudet `oidlib-sample` moduuli.

    ![oidclib riippuvuuksia oidlib otoksen-moduuli](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Valitse **OK** ja odota gradle synkronointia.

    Oman settings.gradle pitäisi näyttää samalta kuin:

    ![Näyttökuva settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Kehittää varmistaa, että malli-sovelluksen malli toimimasta oikein.

    Et voi käyttää tätä Azure Active Directory vielä. Microsoft on joitakin päätepisteitä määrittää ensin. Tämä on varmistaa ei ole Android Studio ongelmat ennen aluksi otoksen sovelluksen mukauttaminen.

10. Luoda ja suorittaa `oidlib-sample` Android Studiossa kohteena.

    ![Etenemisen oidlib otoksen muodosta](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Poista `app ` kansio, joka jätettiin, kun olet poistanut moduulin projektin, koska Android Studio ei poista turvallisuutta.

    ![Tiedoston rakennetta, joka sisältää sovelluskirjaston](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Poista Suorita määrityksistä, jotka jätettiin myös poistamisen moduulin projektin **Käyttömahdollisuudet Muokkaa** -valikon avaaminen

    ![Muokkaa-valikko määrityksiä](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![suorittaa sovelluksen määrittäminen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Määritä otosten päätepisteet

Nyt kun olet luonut `oidlib-sample` suorittaminen onnistui muokkaaminen joitakin päätepisteitä toimiminen Azure Active Directory-hakemistosta.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Määrittää asiakkaan oidc_clientconf.xml-tiedoston muokkaaminen

1. Kun käytät OAuth2 kulkee vain voit hakea tunnuksen ja soita Graph-Ohjelmointirajapinta, Määritä tekemään OAuth2 vain asiakas. OIDC toimitetaan myöhemmin esimerkissä.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Määritä asiakas-Tunnuksesi, joita olet saanut rekisteröinti-portaalista.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Määrittää uudelleenohjauksen URI sitä alla.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Määritä oman käyttöalueen voit koota Graph-Ohjelmointirajapinnan käyttäminen.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

`User.Read` Arvo `oidc_scopes` avulla voit lukea perustiedot profiilin allekirjoitetut käyttäjä.
Voit lukea lisää kaikki käytettävissä olevat alueet, [Microsoft Graph käyttöoikeuksien käyttöalueet](https://graph.microsoft.io/docs/authorization/permission_scopes).

Jos haluat lisätietoja selitykset `openid` tai `offline_access` kuin käyttöalueet OpenID muodostaa, katso [2.0 protokollat - OAuth 2.0 luvan koodin Flow](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Määrittää asiakkaan päätepisteet oidc_endpoints.xml-tiedoston muokkaaminen

- Avaa `oidc_endpoints.xml` tiedoston ja tee seuraavat muutokset:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Nämä päätepisteet vaihdetaan koskaan, jos käytät OAuth2 oman protokolla.

> [AZURE.NOTE]
Päätepisteet `userInfoEndpoint` ja `revocationEndpoint` ei tällä hetkellä tue Azure Active Directory. Jos jätät ne example.com oletusarvoa, näyttöön tulee virheilmoitus, että ne eivät ole käytettävissä otoksessa :-)


## <a name="configure-a-graph-api-call"></a>Määritä Graph API-kutsu

- Avaa `HomeActivity.java` tiedoston ja tee seuraavat muutokset:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Tätä yksinkertaisen kaavion API-kutsu palauttaa Microsoftin tiedot.

Siinä ovat kaikki muutokset, jotka sinun on suoritettava. Suorita `oidlib-sample` sovellus, ja valitse **Kirjaudu sisään**.

Sen jälkeen, kun olet onnistuneesti todennettu, valitse Testaa Graph-Ohjelmointirajapinnan puhelu **Pyytää suojattu resurssi** -painike.

## <a name="get-security-updates-for-our-product"></a>Päivitysten hakeminen tuote

On suositeltavaa suojauksen tapaukset koskevien ilmoitusten saaminen ohjesisältöä [Suojauksen TechCenter](https://technet.microsoft.com/security/dd252948) ja neuvoa suojausvaroitusten tilaaminen.
