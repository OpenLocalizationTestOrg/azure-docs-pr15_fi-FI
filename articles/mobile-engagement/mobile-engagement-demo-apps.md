<properties
    pageTitle="Azure Mobile välitys esittely-sovellus | Microsoft Azure"
    description="Tässä artikkelissa kuvataan mistä voit ladata, käyttäminen ja Azure Mobile välitys esittely-sovelluksen käytön etuja"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Sovelluksen Azure Mobile välitys-esittely

Olemme olet julkaissut Azure Mobile välitys esittely-sovellus **iOS**, **Android**-ja **Windows** -ympäristöissä avulla voit etsiä hyödyllisiä resursseja ja lisätietoja Mobile välitys.

Sovelluksen avulla voit:

- Etsi helposti hyödyllisiä linkkejä Mobile välitys resurssit, kuten videoita, asiakirjat, tuki-keskustelupalstalla ja mistä nostaa ehdottaa ominaisuuksia.
- Kokemus otoksen ilmoitukset on Mobile välitys saat ideoita mobile sovellusten tukemat.
- Viite-käyttöönoton avulla voit tutkia Mobile välitys ottamisesta käyttöön oman Appiin. Voit opetella:

    - Kerää analytics-tietoja.
    - Ota käyttöön tyypit, kuten *koko näytön kudosten* tai *ponnahdusikkunoiden*Lisäasetukset ilmoituksen skenaarioita.
    - Ota käyttöön, kyselyt ja kyselyiden.
    - Ota käyttöön push hiljainen tiedot ja push skenaarioita.   

## <a name="app-installation"></a>Sovelluksen asennuksen
Sovellus on käytettävissä seuraavassa app tallentaa:

- **Windowsin yleinen esittely-sovelluksen**:

    - Lataa sovelluksen [Windows-sovellus tallentaa](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Sovellus on kehittänyt Windows 10 yleinen sovelluksen nimellä. Lähdekoodin on käytettävissä [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS esittely sovelluksen**:

    - Lataa [Apple tallentaa](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090)sovelluksen.
    - Sovellus on kehittänyt iOS Swift. Lähdekoodin on käytettävissä [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Esittely android-sovelluksen**:

    - Lataa sovellus [Google Play tallentaa](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement)osoitteessa.
    - Lähdekoodin on käytettävissä [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Windowsin yleinen esittely-sovellus][1]

![iOS esittely sovelluksen][2]
![esittely Android-sovelluksen][3]


## <a name="usage"></a>Käyttö

Voit käyttää sovelluksen seuraavilla tavoilla:

**Lataa sovellus laitteessasi sovelluksesta tallentaa linkkejä (edellä):**

>[AZURE.IMPORTANT] Sinun ei tarvitse Azure-tili tai sovelluksen yhdistäminen taustatietokannaksi ei tarvitse. Sovellus toimii itsenäisesti.

- Kun olet sovellus laitteessasi, voit siirtyä linkkien Etsi tietoja Mobile välitys ohjeista vasemman reunan-valikon kautta.
- Olemme lisänneet [palvelun RSS-syötteen](https://aka.ms/azmerssfeed) tämän sovellukseen niin, että olet päivitetään aina uusimmat tuotepäivitysten.
- Voit myös siirtyä otoksen ilmoituksen tilanteita, kohtaat ilmoitukset, joita tuetaan eri ympäristöissä Mobile välitys tyyppi. Ilmoitukset voi olla listan paikallisesti--, joka on, voit napsauttaa näytöissä, Näytä ilmoitukset-toiminto, joka on sama kuin ilmoitusten lähettäminen Mobile välitys-ympäristö-painikkeita.

![Windowsin sovellusvalikko][4]

![IOS sovellusvalikko][5]
![sovellusvalikko androidille][6]

**Lataa lähdekoodin GitHub linkkien (edellä):**

- Kun olet ladannut lähdekoodin, avaa se vastaaviin kehitysympäristö--iOS-Android Studio Android-ja Visual Studio Windows XCode.
- Noudata Microsoftin [SDK-integroinnin perusvaiheet](mobile-engagement-windows-store-dotnet-get-started.md) Seuraava niin, että olet voi muodostaa sovelluksen oma Mobile välitys taustatietokantaan esiintymä.
    - Sinun täytyy määrittää yhteysmerkkijonon-sovelluksessa.
    - Haluat myös sovelluksen push-ilmoituksen ympäristön määrittäminen.
- Huomaat, että sovelluksessa instrumented Mobile välitys kanssa. Tämän vuoksi, kun avaat sovelluksen jälkeen yhteyden taustatietokantaan, voit voi **Näyttö** -välilehdessä näkyvien käyttäjän istuntoon, toimintoja, tapahtumia ja niin edelleen.
- Voit myös voi käyttää Lähetä ilmoitukset sovelluksen Mobile välitys oman esiintymästä sijaan paikallisen ilmoitukset.
    - Tässä voit lisätä laitteen testi laitteen avulla **saat laitteen tunnus** -valikkovaihtoehto-sovelluksessa. Näin voit laitteen ID, jolla voit sitten Rekisteröi testilaitteen ympäristö taustatietokantaan-esiintymää.

    ![Windows-laitteen tunnus][7]

    ![IOS-laitteen tunnus][8]
    ![Android-laitteen tunnus][9]

## <a name="key-features-of-the-demo-app"></a>Esittely-sovelluksen keskeisiä ominaisuuksia

- Kuten edellä mainittiin, tämän sovelluksen kanssa on tärkeiden resurssien for Mobile välitys käden. Linkkien avulla voit siirtyä vasemmalla olevasta valikosta.

- Sovelluksen ilmoitukset eri ympäristöissä voi ilmetä. Ilmoitukset toimitetaan **vain ilmoitus**, jossa valitsemalla ilmoituksen yksinkertaisesti avataan sovelluksen alkuperäisen näyttö ylöspäin (avulla **laaja linkittäminen**)--tai **Web-ilmoitus**, jossa voit näyttää Lisää HTML-sisällön Mobile välitys takaisin päässä voi näyttää ilmoituksen napsautettaessa.

    ![Sovellusten ilmoitukset][29]

- IOS-sinun on suljettava sovelluksen sovelluksen tai järjestelmän push-ilmoitukset. Voi tarkastella käyttöönoton tähän lisäämiseen **toimintopainikkeiden**ilmestyä, jotka lisätään tämän sovelluksen ilmoituksen *palaute* ja *jakaminen* (niin, että käyttäjä voi suorittaa toiminnon oikealle ilmoituksesta itse).

    ![IOS-sovelluksen ilmoitukset][11] ![IOS näytön sovelluksen ilmoitus][14]

- Android-asetukset, joita tuetaan lisäämässä Monirivinen teksti (**Suuri teksti**) tai ilmoituksessa kuva (**Ylemmän tason**) ilmoituksen sekä **Toimintopainikkeet** (kuten tukemat iOS).

    ![Android-sovelluksen ilmoitukset][12] ![Android-sovelluksen ilmoitus-näyttö][15]

- Windows 10: ssä näet, miltä ilmoitukset tietokoneen. Tämä ilmoitus näyttää myös Windows 10- **Ilmoituksen Center**. **Toimintopainikkeet** lisäämiseen Windows SDK hetkellä ei tueta.

    ![Windows-sovellus ilmoitukset][10] ![Windows-sovellus näyttö][13]

- Oletus "sovelluskohtaisesta"-ilmoitukset eri ympäristöissä voi ilmetä. Tämä on kaksivaiheinen kokemus, jolloin **ilmoitus** -ikkunassa näkyy ensin. Kun painiketta napsautetaan, se avautuu koko näytön **ilmoitus**, sellaisina kuin ne näkyvät seuraavassa näyttökuvassa. Tämä ilmoitus sisältö tulee Mobile välitys taustatietokantaan-esiintymä. SDK on malleja, ilmoituksia ja ilmoitukset. Voit mukauttaa ne helposti, johon on lisätty logo ja ohjeet kohdasta esittely sovelluksen esitetyllä tavalla.  

    ![Windows-sovellusten ilmoitukset][16]

    ![IOS-sovelluksen ilmoitukset][17]  ![Android-sovelluksen-ilmoitukset][18]

    **iOS**- **Android**

- Voit myös oletus-käyttötuntuman Mobile välitys **luokka** -ominaisuus. Esittely-sovelluksessa on on osoittanut kaksi tapaa muuttaa ilmoitukset käyttökokemusta. Huomaa, että luokka-ominaisuus ei vielä tueta Windows SDK.

    **Koko näytön kudosten:**

    ![Sovelluksen-ilmoitus - kudosten luokka][30]

    ![IOS kudosten luokka][21]  ![Android-kudosten luokka][22]

    **Ponnahdusilmoituksen:**

    ![Sovelluksen-ilmoitus - ponnahdusikkunoiden luokka][31]

    ![IOS ponnahdusilmoitus][19]   ![Android-ponnahdusilmoitus][20]

**iOS**- **Android**

- Mobile välitys tukee myös sovelluskohtaisesta ilmoituksen kutsutaan **kyselyjä**erityinen tyypin. Voit lähettää nopeasti kyselyt Segmentoitu sovelluksen käyttäjille. Voit lisätä kysymyksiä ja kysymyksen, kuten seuraavassa näyttökuvan asetukset. Tämä sitten Hae näkyy muodossa-sovelluksen ilmoituksella sovelluksen käyttäjälle.   

    ![Kysely-ilmoitukset][32]

    ![Kyselyn Windows][26]

    ![Kyselyn iOS][27]   ![Kyselyn Android-laitteeseen][28]

**iOS**- **Android**

- Mobile välitys tukee myös hiljainen **Tietojen Push** -ilmoitukset lähetetään. Ilmoitukset, jonka voit lähettää tiedot-palvelun (kuten JSON tietoja seuraavassa esimerkissä), jossa voit käsitellä sovelluksen ja jotkin toiminnon. Esimerkki on, miten Microsoft haluat vaihtaa kohteen hinta valikoivasti käyttämällä tietojen push-ilmoitukset.

    ![Tietoja push-ilmoitus][33]

    ![Windowsin tietojen push ilmoitus][23]

    ![Tietoja push-ilmoituksen iOS][24]  ![Tietoja push-ilmoituksen Android-laitteeseen][25]

**iOS**- **Android**

> [AZURE.NOTE] Voit tarkastella yksityiskohtaiset ohjeet minkään ilmoitukset valitsemalla minkä tahansa otoksen ilmoituksen näytön **napsauttamalla tätä ohjeita siitä, miten voit lähettää nämä Mobile välitys ympäristö ilmoituksia** .


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
