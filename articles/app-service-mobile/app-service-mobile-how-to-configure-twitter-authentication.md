<properties
    pageTitle="Twitter-todennus App Services-sovelluksen määrittäminen"
    description="Opettele määrittämään Twitter-todennus App Services-sovelluksen."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Sovelluksen palvelusovelluksen Twitter-kirjautuminen käyttämään määrittäminen

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Tässä ohjeaiheessa esitellään Azure App palvelun käytettävä Twitter käyttöoikeustarkistuspalvelun asetusten määrittämisestä.

Noudattamalla tämän artikkelin ohjeita, tarvitset Twitter-tili, jolla on vahvistettu sähköpostin sähköpostiosoitteen ja puhelinnumeron lisääminen. Jos haluat luoda uuden Twitter-tilin, siirry <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Rekisteröidä sovelluksen Twitteriin


1. Kirjaudu sisään [Azure portal]ja siirry sovelluksen. Kopioi **URL-osoite**. Tämä käytetään Twitter-sovelluksen määrittäminen.

2. Siirry sivustoon [Twitter-kehittäjille] , kirjaudu sisään Twitter-tilin tunnistetiedot ja sitten **Luo uusi sovellus**.

3. Kirjoita **nimi** ja **kuvaus** , kun uusi sovellus. Liitä **sivuston** arvon sovelluksen **URL-osoite** . Liitä sitten **Takaisinsoitto URL-osoite**aiemmin kopioitu **Takaisinsoitto URL-osoite** . Täältä Mobile-sovelluksen polulla _/.auth/login/twitter/callback_perään. Esimerkiksi `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Varmista, että käytät HTTPS-malli.

3.  Sivun alalaidassa Lue ja hyväksy tilaussivun ehdot. Valitse **Luo Twitter-sovellusta**. Tämä Rekisteröi sovelluksen näyttää hakemuksen tiedot.

4. Valitse **asetukset** -välilehti, valitse **Salli tämän sovelluksen Twitter sisäänkirjautumiseen käytettävän**ja valitse sitten **Päivitysasetukset**.

5. Valitse **näppäimet ja tunnusten Access** -välilehti. Pane merkille **Kuluttaja avainta (API)** ja **kuluttaja salaisuus (API salaisuus)**-arvot.

    > [AZURE.NOTE] Kuluttaja toiminta perustuu tärkeän tunnistetiedon. Älä Jaa tämä salaisuus kenen kanssa tai jakaa sen sovelluksen.


## <a name="secrets"> </a>Sovelluksen lisääminen Twitter-tiedot

13. Siirry takaisin sisään [Azure portal]-sovellus. Valitse **asetukset**, ja valitse sitten **todennus / luvan**.

14. Jos todennus / todennus-ominaisuus ei ole käytössä, ota **Valitse**Siirry.

15. Valitse **Twitter**. Liitä Sovellustunnus ja sovelluksen salaisuus arvoja, jotka olet hankkinut aiemmin. Valitse **OK**.

    ![][1]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

17. (Valinnainen) Käytön rajoittaminen vain Twitter-todennetut käyttäjät sivustoon, **Twitter-** **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** arvoksi. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Twitter todennusta varten.

17. Valitse **Tallenna**.

Olet nyt valmis käytettäväksi Twitter-sovelluksen todennusta varten.

## <a name="related-content"> </a>Liittyvät sisältö

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter-kehittäjille]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
