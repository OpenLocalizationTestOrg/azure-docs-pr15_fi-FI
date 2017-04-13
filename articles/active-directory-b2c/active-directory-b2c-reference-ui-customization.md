<properties
    pageTitle="Azure Active Directory-B2C: Käyttäjän käyttöliittymän mukauttaminen | Microsoft Azure"
    description="Käyttäjä-käyttöliittymän mukautusominaisuudet Azure Active Directory-B2C aiheesta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory-B2C: Azure AD B2C käyttöliittymän (UI) mukauttaminen

Käyttäjäkokemus on ensiarvoisen kuluttaja osoittava-sovelluksessa. Se on hyvä sovelluksen ja hyvien yksi sekä välillä vain aktiiviset käyttäjät ja todella kytketty tiedoston erotus. Azure Active Directory (Azure AD) B2C avulla voit mukauttaa kuluttaja kirjautuminen, kirjaudu sisään (*Katso alla oleva huomautus*)-profiilin muokkaaminen ja salasanan kuvapisteen piirtää ohjausobjektin sivuilla.

> [AZURE.NOTE]
Tällä hetkellä paikallisen tilin kirjautumisen sivuja, accompaning salasana Palauta sivut ja todentaminen sähköpostit voi mukauttaa vain käyttämällä [yrityksen yrityksen tunnuksen ominaisuus](../active-directory/active-directory-add-company-branding.md) ja järjestelmiä tämän artikkelin ohjeiden mukaan.

Tässä artikkelissa voit lukea tietoja:

- Sivun käyttäjän käyttöliittymän mukauttaminen-ominaisuus.
- Työkalu, jonka avulla voit testata sivun Käyttöliittymän mukauttaminen-ominaisuus käyttämällä Microsoftin mallin sisältöä.
- Core Käyttöliittymän osat kummankin sivun.
- Parhaita käytäntöjä, kun tämä toiminto.

## <a name="the-page-ui-customization-feature"></a>Sivun Käyttöliittymän mukauttaminen-ominaisuus

Sivun Käyttöliittymän mukauttaminen-ominaisuudella voit mukauttaa kuluttajien kirjautumisen, kirjaudu sisään ulkoasun, salasanan ja profiilin muokkaaminen sivut (määrittämällä [käytännöt](active-directory-b2c-reference-policies.md)). Lukijoille saa saumaton kokemukset, kun sovellus ja sivujen välillä siirtyminen served Azure AD B2C mukaan.

Toisin kuin muiden palvelujen Jos Käyttöliittymän asetusten kokorajoituksena tai ovat vain ohjelmointirajapinnan kautta Azure AD B2C käyttää Moderni (ja yksinkertaisempi) lähestymistavan sivun Käyttöliittymän mukauttaminen.

Näin toiminta: Azure AD B2C suorittaa koodin lisääminen kuluttaja selaimessa ja käyttää Moderni lähestymistapa, kutsutaan [Rajat Origin resurssien jakaminen (CORS)](http://www.w3.org/TR/cors/) Lataa sisältö käytännön määrität URL-osoite. Voit määrittää eri URL-osoitteita eri sivuille. Koodin yhdistää Azure AD B2C käyttöliittymäelementit ja URL-osoitteesi ladata sisältöä, ja näyttää sivun oman asiakkaaksi. Kaikki sinun on suoritettava on:

1. Luo muotoiltu HTML5: n sisällöstä `<div id="api"></div>` elementin (on oltava tyhjä elementti) sijaitsevat toiseen sijaintiin `<body>`. Tämä elementti merkit, jossa Azure AD B2C sisältö on lisätty.
2. Ylläpitää HTTPS päätepisteen sisältöä (kun CORS sallittu).
3. Tyylin Käyttöliittymän osat, jotka Azure AD B2C Lisää.

## <a name="test-out-the-ui-customization-feature"></a>Kokeile Käyttöliittymän mukauttaminen-ominaisuus

Jos haluat tutustua Käyttöliittymän mukauttaminen-ominaisuuden avulla sekä otoksen HTML ja CSS-sisältöä, on määritetty voit [Yksinkertainen avustaja-työkalua](active-directory-b2c-reference-ui-customization-helper-tool.md) , joka latauksia ja määrittää mallin sisältöä Azure-Blob-säiliö.

> [AZURE.NOTE]
Voit ylläpitää Käyttöliittymän sisältöä missä tahansa: web-palvelimissa, Sisältöverkot, sisältyy AWS S3, tiedoston jakamisen järjestelmien jne. Kun sisällön isännöidään yleiseen käyttöön HTTPS-päätepisteen (ja CORS sallittu), on hyvä huomioida missä. Azure-Blob-säiliö on käytössä vain epäsuotuisista varten.

### <a name="the-most-basic-html-content-for-testing"></a>Yleisin HTML-sisällön testikäyttöön

Alla on yleisin HTML sisällön, voit testata tätä ominaisuutta. Edellä ladataan ja sisällön määrittämistä Azure-Blob-säiliö saman avustaja-työkalun avulla. Olet varmistanut basic, tyyliteltyjä painikkeet ja lomakekentät kullakin sivulla on näkyvissä ja toimi sitten.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Kirjoita jokaisen sivun core Käyttöliittymän osat

Seuraavissa osissa, saat näkyviin esimerkkejä HTML5-osat, joka yhdistää Azure AD B2C `<div id="api"></div>` elementti sijaitsee sisältöä. **Älä lisää nämä Vaillinaiset lauseet HTML 5 sisällöstä.** Azure AD B2C palvelun lisää ne suorituksen aikana. Näissä esimerkeissä avulla voit suunnitella omia tyylisivuja.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD-B2C sisältöä, jotka on lisätty "tunnistetietojen toimittaja valinnan sivulle"

Tämä sivu sisältää luettelon tunnistetietojen toimittajat, käyttäjä voi valita aikana kirjautuminen tai Kirjaudu sisään. Nämä ovat sosiaalisen tunnistetietojen toimittajat, kuten Facebook- ja Google + tai paikalliset tilit (sähköpostin osoite tai käyttäjän nimen perusteella).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C sisältöä, jotka on lisätty "paikallisen tilin kirjautumissivulle"

Tämä sivu sisältää ilmoittautumislomake käyttäjän on paikallinen tili, joka perustuu sähköpostiosoitteen tai käyttäjänimen kirjautuessasi täyttää. Lomakkeen voi olla eri syötteen ohjausobjekteja, kuten tekstiä tekstiruudussa, tapahtuma salasanakenttä, valintanappi, yksi Valitse avattavista ja valittavia valintaruudut.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C sisältöä, jotka on lisätty "" sosiaalisen tilin kirjautumissivulle"

Tämä sivu sisältää ilmoittautumislomake kuluttaja täytä kirjautuessasi käyttämällä käytössä olevan tilin sosiaalisen tunnistetietojen tarjoajalta, kuten Facebookissa tai Google + sisältävän. Tämä sivu muistuttaa paikallisen tilin kirjautumissivulle (kuten edellisessä osassa) salasanan hakukentistä lukuun ottamatta.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD-B2C sisältöä, jotka on lisätty "yhdistetyssä kirjautuminen tai Kirjaudu sisään sivulle"

Tällä sivulla käsittelee sekä kirjautuminen ja kirjaudu sisään kuluttajien, jotka voivat käyttää sosiaalisen tunnistetietojen toimittajat, kuten Facebookissa tai Google + tai paikalliset tilit.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD-B2C sisältöä, jotka on lisätty "monimenetelmäisen todentamisen sivulle"

Tällä sivulla käyttäjiä, voit tarkistaa niiden puhelinnumerot (joko tekstin tai äänen) aikana kirjautuminen tai Kirjaudu sisään.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD-B2C sisältöä, jotka on lisätty "-Virhe sivulle"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Muistettavaa omalla sisällölläsi luotaessa

Jos aiot käyttää sivun Käyttöliittymän mukauttaminen-toimintoa, tarkista seuraavat parhaat käytännöt:

- Ei kopioida Azure AD B2C oletussisältö ja yrität muokata sitä. On suositeltavaa luominen alusta alkaen HTML5 sisältöä ja käyttää oletussisältö viittauksena.
- Kaikki sivut (lukuun ottamatta virhesivujen) served-kirjautuminen, kirjautuminen ja profiilin muokkaaminen käytäntöjä, tyylisivut, jonka annat on korvaavat oletusarvon tyylisivuja, jotka on lisää nämä sivut <head> Vaillinaiset lauseet. Kaikki served kirjautuminen tai Kirjaudu sisään ja nollaa salasanakäytännöt ja kaikkien käytäntöjen virhesivujen sivuilla, sinun on annettava kaikki tyylejä itse.
- Tietoturvasyistä on Älä salli sisällyttää kaikki JavaScript-sisältöä. Sinun on yleensä pitäisi olla käytettävissä oleva ruutu. Muussa tapauksessa [Käyttäjän Voice](http://feedback.azure.com/forums/169401-azure-active-directory) avulla voit pyytää uusia toimintoja.
- Tuetut selainversiot:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (rajoitettu)
    - Internet Explorer 8 (rajoitettu)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
