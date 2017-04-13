<properties
    pageTitle="Facebook-todennus App Services-sovelluksen määrittäminen"
    description="Opettele määrittämään Facebook-todennus App Services-sovelluksen."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Voit määrittää sovelluksen palvelusovelluksen käyttämään Facebook-kirjautuminen

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Tässä ohjeaiheessa esitellään Azure App palvelun käyttämään Facebook käyttöoikeustarkistuspalvelun asetusten määrittämisestä.

Noudattamalla tämän artikkelin ohjeita, tarvitset Facebook-tili, jolla on vahvistettu sähköpostiosoite ja matkapuhelinnumero. Jos haluat luoda uuden Facebook-tilin, siirry [facebook.com].

## <a name="register"> </a>Rekisteröidä sovelluksen Facebookiin

1. Kirjaudu sisään [Azure portal]ja siirry sovelluksen. Kopioi **URL-osoite**. Tämä käytetään määrittäminen Facebook-sovelluksessa.

2. Toisessa selainikkunassa Siirry [Kehittäjät Facebook] -sivustossa ja kirjaudu sisään Facebook-tilin tunnistetiedot.

3. (Valinnainen) Jos et ole jo rekisteröitynyt, valitse **sovellukset** > **Rekisteröi kehittäjä**, sitten Hyväksy käytäntö ja rekisteröinti ohjeiden mukaisesti.

4. Valitse **Omat sovellukset** > **Lisää uuden sovelluksen** > **sivuston** > **ohittaa ja luoda Sovellustunnus**. 

5. Kirjoita **Näyttönimi**-sovelluksen yksilöivä nimi, kirjoita **Yhteyshenkilön sähköpostiosoite**, valitse **luokka** , kun sovellus sitten napsauttamalla **Luo Sovellustunnus** ja määrittämällä suojaus-valintaruutu. Tämä avaa uuden Facebook-sovelluksen kehittäjän raporttinäkymät-ikkuna.

6. Valitse "Facebook-kirjautuminen," **Pääset alkuun**. Lisää sovelluksen **Uudelleenohjata URI** **kelvollinen OAuth**uudelleenohjaaminen URI ja valitse sitten **Tallenna muutokset**. 

    > [AZURE.NOTE] Uudelleenohjauksen URI on lisätty path _/.auth/login/facebook/callback_sovelluksen URL-osoite. Esimerkiksi `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Varmista, että käytät HTTPS-malli.

6. Valitse vasemmassa siirtymisruudussa **asetukset**. **Sovelluksen toiminta** -kentän valitsemalla **Näytä**-pyydetty Valitse muistiin **Sovellustunnus** ja **Sovelluksen salaisuus**arvoja, Anna salasana. Voit käyttää näitä myöhemmin määrittäminen sovelluksen Azure-tietokannassa.

    > [AZURE.IMPORTANT] Sovelluksen toiminta perustuu tärkeän tunnistetiedon. Älä Jaa tämä salaisuus kenen kanssa tai jakaa sen asiakas-sovelluksesta.

7. Facebook-tili, jota käytettiin rekisteröidä sovellus on sovelluksen järjestelmänvalvoja. Tässä vaiheessa vain järjestelmänvalvoja voi kirjautua tämän sovelluksen. Todentamiseen muut Facebook-tilit, valitse **Sovelluksen Tarkista** ja **< - sovelluksen-nimesi > Tee julkiseksi** Facebook-todennusta Yleiset julkinen-käytön käyttöön ottaminen käyttöön.

## <a name="secrets"> </a>Lisätä Facebook-tiedot-sovellukseen

1. Siirry takaisin sisään [Azure portal]-sovellus. Valitse **asetukset** > **todennusta / luvan**, ja varmista, että **Sovelluksen todentaminen** on **otettu käyttöön**.

2. Valitse **Facebook**, Sovellustunnus ja sovelluksen salaisen arvot, jotka olet hankkinut aiemmin sijoittaminen, käyttöön myös kaikki alueet sovelluksen tarvitsemia ja valitse sitten **OK**.

    ![][0]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

3. (Valinnainen) Käytön rajoittaminen vain Facebook-todennetut käyttäjät sivustoon, määrittää **toiminnon, joka suoritetaan, kun pyyntö ei ole todennettu** **Facebookiin**. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Facebookiin todennusta varten.

4. Kun valmis määrittäminen todennusta, valitse **Tallenna**.

Olet nyt valmiina käyttämään Facebook-sovelluksen todennusta varten.

## <a name="related-content"> </a>Liittyvät sisältö

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-kehittäjille]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
