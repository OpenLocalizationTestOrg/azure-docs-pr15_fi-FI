<properties
    pageTitle="Azure Cordova/Phonegap for Mobile välitys käytön aloittaminen"
    description="Opettele käyttämään Azure Mobile välitys Cordova/Phonegap sovellusten Analytics ja Push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Azure Cordova/Phonegap for Mobile välitys käytön aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa esitellään, miten ymmärtää sovelluksen käytön ja lähettää push-ilmoitukset Segmentoitu käyttäjille mobile-sovelluksen kanssa Cordova kehittänyt Azure Mobile välitys avulla.

Tässä opetusohjelmassa on tyhjä Cordova-sovelluksen käyttäminen Mac luodaan ja integroida Mobile välitys SDK. Se kerää basic analytics-tiedot ja vastaanottaa käyttämällä Apple Push ilmoituksen järjestelmän (APN) iOS-ja Google Cloud Messaging (GCM) androidille push-ilmoitukset. Olemme otetaan käyttöön iOS-tai Android-laitteen testikäyttöön. 

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ XCode, jossa voit asentaa Mac App Storesta (käyttöönotossa iOS)
+ [Android SDK & emulaattorin](http://developer.android.com/sdk/installing/index.html) (Saat käyttöönotossa Android)
+ Push-ilmoitus-varmenne (.p12), jonka voit saada APN Apple keskihajonta Centeristä
+ GCM projektin numero, jonka voit saada GCM Google Kehitystyökalut-konsolin
+ [Mobiili välitys Cordova laajennus](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Voit etsiä [Github](https://github.com/Azure/azure-mobile-engagement-cordova) Cordova-laajennuksen lähdekoodin ja Lueminut-tiedosto

##<a id="setup-azme"></a>Määritä Cordova-sovelluksen Mobile välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen muodostamisesta Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi" on välttämättömät määrittää tietojen kerääminen ja Lähetä push-ilmoitus. 

Luodaan basic sovelluksen Cordova osoittamaan integroinnin kanssa:

###<a name="create-a-new-cordova-project"></a>Cordova uuden projektin luominen

1. Käynnistä *Pääte* -ikkunassa Mac-tietokoneeseen ja kirjoita joka luo uuden Cordova projektin oletusarvo-mallista. Varmista, että käytät myöhemmin iOS-sovelluksen käyttöönotto julkaisun profiilin käyttää 'com.mycompany.myapp' App ID-tunnuksellasi. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Suorita projektin **iOS** ja suorittaa sen iOS Simulator seuraavasti:

        $ cordova platform add ios 
        $ cordova run ios

3. Suorita projektin määrittäminen **Android-** ja suorita se Android emulaattorin seuraavasti. Varmista, että Android SDK emulaattorin asetuksia kohde nimellä Google-ohjelmointirajapinnan (Google Inc.) Suoritin / ABI Google API ARM nimellä.  

        $ cordova platform add android
        $ cordova run android

4.  Lisää Cordova Console-laajennus. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Asenna Azure Mobile välitys Cordova laajennus käyttämisessä määrittäminen laajennus muuttujien arvoihin:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android saavuttaa kuvake* : on oltava ilman tunnistetta eikä drawable etuliite resurssin nimi (ex: mynotificationicon), ja kuvaketiedosto on kopioitava projektiisi android (ympäristöjen/android/tarkkuus/drawable)

*iOS saavuttaa kuvake* : on oltava tiedostotunnisteen resurssin nimi (ex: mynotificationicon.png), ja kuvaketiedosto on lisättävä iOS-projektiisi XCode (joko lisää tiedostoja-valikko) kanssa

##<a id="monitor"></a>Reaaliaikaisen seurannan ottaminen käyttöön

1. Muokkaa Cordova project - **www/js/index.js** Lisää Mobile varauksia määritellä uusi tehtävä kerran *deviceReady* tapahtuman puhelu vastaanotetaan.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Suorita sovellus:
        
    - **IOS**
    
        Valitse `Terminal` ikkunan Käynnistä sovelluksen Simulator uuden esiintymän suorittamalla seuraavat:

            cordova run ios

    - **Androidille**
        
        Valitse `Terminal` ikkunan Käynnistä sovelluksen emulaattorin uuden esiintymän suorittamalla seuraavat:

            cordova run android

3. Näet console-lokit seuraavasti:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-ilmoitukset ja sovelluskohtaisesta messaging ottaminen käyttöön

Mobile välitys voit käsitellä käyttäjien Push-ilmoitukset ja sovelluksen messaging Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa määritys sovelluksen vastaanottamaan ne.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Push-tunnistetietojen määrittäminen Mobile välitys

Anna Mobile varauksia Push-ilmoitukset lähetetään puolestasi, haluat myöntää käyttöoikeuden Apple iOS varmenteen tai GCM palvelimen Ohjelmointirajapinnan avain. 
    
1. Siirry Mobile välitys-portaalin. Varmista olet on käyttämäsi projekti ja valitse sitten alareunassa **Engage** -painikkeen-sovelluksessa:
    
    ![][1]
    
2. Power View-asetukset-sivun välitys-portaalissa. Valitse siellä **Alkuperäisen Push** -osassa:
    
    ![][2]

3. Määritä iOS todistus/GCM palvelimen Ohjelmointirajapinnan avain

    **[iOS]**

    a. Valitse oman .p12, lataa se ja kirjoita salasanasi:
    
    ![][3]

    **[Android]**

    a. Valitse **Ohjelmointirajapinnan avain** GCM asetukset-kohdassa ja valikko, joka näkyy Liitä GCM Server-näppäintä ja valitse **OK**edessä olevaa muokkauskuvaketta. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>Ota käyttöön push-ilmoitukset Cordova-sovelluksessa

Muokkaa **www/js/index.js** Lisää Mobile varauksia pyytää push-ilmoitukset ja määritellä käsittelytoiminto puhelu:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Suorita sovellus

**[iOS]**

1. Käytämme XCode muodosta ja ota käyttöön sovellus Testaa push-ilmoitukset, koska iOS sallii vain push-ilmoitukset todellinen laitteeseen laitteeseen. Siirry sijaintiin, johon Cordova projektin luodaan ja siirry **...\platforms\ios** sijaintiin. Avaa XCode alkuperäisen .xcodeproj-tiedoston. 
    
2. Muodosta ja ota käyttöön Cordova-sovellus iOS-laitteeseen, käyttämällä tiliä valmistelu profiilin sisältävä juuri varmenteen ladataan Mobile välitys-portaaliin ja sovelluksen tunnus on mikä vastaa se antamaasi Cordova sovelluksen luomisen aikana. Voit tarkistaa *Pikaoppaista tunnus* -että **resurssien\*-info.plist** tiedoston XCode vastaamaan. 

3. Näet vakio iOS-Ponnahdusvalikko laitteen, jossa sanotaan, että sovellus pyytää oikeutta lähettää ilmoitukset. Voit myöntää käyttöoikeuden. 

**[Android]**

Voit vain toimivat Android-sovelluksen Android-emulointiohjelma tuetaan GCM ilmoitukset emulaattori. 

    cordova run android

##<a id="send"></a>Lähettää ilmoituksen sovelluksen

Luodaan nyt yksinkertainen Push-ilmoitus-markkinointikampanjan, joka lähettää push käynnissä laitteeseen sovelluksen:

1. Siirry Mobile välitys-portaalin **yhteys** -välilehti

2. Valitse **Uusi ilmoitus** push-markkinointikampanjan luominen

    ![][6]

3. Anna syötteiden luominen **[Android]** että markkinointikampanja
    
    - Anna **nimi** , että markkinointikampanja. 
    - Valitse *ilmoitus* *Yksinkertainen* **Toimituksen tyyppi**
    - Valitse **toimitusaikaan** *"mikä tahansa aika"*
    - Antavat ilmoitus joka painallus ensimmäisellä rivillä on **otsikko** .
    - Anna **viestin** ilmoitus, jotka toimivat viestin tekstiosaan. 

    ![][11]

4. Anna syötteiden luomiseen että markkinointikampanja **[iOS]**

    - Anna **nimi** , että markkinointikampanja. 
    - Valitse **toimitusaikaan** *"ulos sovelluksesta vain"*
    - Antavat ilmoitus joka painallus ensimmäisellä rivillä on **otsikko** .
    - Anna **viestin** ilmoitus, jotka toimivat viestin tekstiosaan. 
 
    ![][12]

5. Vieritä alas ja valitse sisältö, valitse **vain-ilmoitus**

    ![][8]

6. [Valinnainen] Voit myös kirjoittaa toiminnon URL-osoite. Varmista, että se käyttää URL-malli laajennuksen määritettäessä annettu **AZME\_UUDELLEENOHJATA\_URL-Osoitteen** muuttujan esimerkiksi *myapp://test*.  

7. Olet valmis määrittäminen yleisin markkinointikampanjan mahdollista. Nyt uudelleen alaspäin ja valitse Tallenna että markkinointikampanja **Luo** -painiketta.

8. Lopuksi **Aktivoi** että markkinointikampanja
    
    ![][10]

9. Push-ilmoituksen pitäisi tulla näkyviin laitteessa tai emulaattorin tämän markkinointikampanjan osana. 

##<a id="next-steps"></a>Seuraavat vaiheet
[Yleiskatsaus käytettävissä Cordova Mobile välitys SDK tavoilla](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

