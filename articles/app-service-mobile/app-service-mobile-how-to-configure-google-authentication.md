<properties
    pageTitle="Google-todennus App Services-sovelluksen määrittäminen"
    description="Opettele määrittämään sovelluksen Services-sovelluksen todennus Google."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Google-kirjautuminen käyttämään App palvelusovelluksen määrittäminen

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Tässä ohjeaiheessa esitellään Azure App palvelun käyttää Google käyttöoikeustarkistuspalvelun asetusten määrittämisestä.

Noudattamalla tämän artikkelin ohjeita, tarvitset Google-tili, jolla on vahvistettu sähköpostiosoite. Jos haluat luoda uuden Google-tilin, siirry [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Rekisteröidä sovelluksen Google

1. Kirjaudu sisään [Azure portal]ja siirry sovelluksen. Kopioi **URL-osoite**, jossa voit määrittää Google-sovelluksen myöhemmin käyttämällä.

2. Siirry sivustoon [Google-ohjelmointirajapinnan](http://go.microsoft.com/fwlink/p/?LinkId=268303) , kirjaudu sisään Google-tilin tunnistetiedot, valitse **Luo projekti**, **projektinimi**ja valitse sitten **Luo**.

3. Valitse **Sosiaalisten ohjelmointirajapinnan** **Google + API** ja **Ota käyttöön**.

4. Valitse vasemmassa siirtymisruudussa **tunnistetiedot** > **OAuth suostumus näytön**, sitten Valitse **sähköpostiosoite**, **Tuotteen nimi**ja valitse **Tallenna**.

5. Valitse **tunnistetiedot** -välilehdessä **Luo tunnistetiedot** > **OAuth Asiakastunnus**ja sitten Valitse **WWW-sovelluksessa**.

6. Liitä aiemmin kopioimasi **Valtuutettuja JavaScript alkuperän**App palvelun **URL-osoite** ja liitä URI uudelleenohjauksen **Valtuutettuja uudelleenohjata URI**. Uudelleenohjauksen URI on lisätty path _/.auth/login/google/callback_sovelluksen URL-osoite. Esimerkiksi `https://contoso.azurewebsites.net/.auth/login/google/callback`. Varmista, että käytät HTTPS-malli. Valitse **Luo**.

7. Valitse seuraavassa näytössä Asiakastunnus- ja asiakkaan salaisuus arvot muistiin.


    > [AZURE.IMPORTANT]
    Asiakkaan toiminta perustuu tärkeän tunnistetiedon. Älä Jaa tämä salaisuus kenen kanssa tai jakaa sen asiakas-sovelluksesta.


## <a name="secrets"> </a>Sovelluksen lisääminen Google-tiedot

8. Siirry takaisin sisään [Azure portal]-sovellus. Valitse **asetukset**, ja valitse sitten **todennus / luvan**.

9. Jos todennus / todennus-ominaisuus ei ole käytössä, ota **Valitse**Siirry.

10. Valitse **Google**. Sovellustunnus ja sovelluksen salaisen arvot, jotka olet hankkinut aiemmin sijoittaminen ja käyttöön myös kaikki alueet sovellus edellyttää. Valitse **OK**.

    ![][1]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

17. (Valinnainen) Käytön rajoittaminen vain Google-todennetut käyttäjät sivustoon, määritetty **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** **Google**. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Google todennusta varten.

12. Valitse **Tallenna**.

Olet nyt valmis käytettäväksi Google-sovelluksen todennusta varten.

## <a name="related-content"> </a>Liittyvät sisältö

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

