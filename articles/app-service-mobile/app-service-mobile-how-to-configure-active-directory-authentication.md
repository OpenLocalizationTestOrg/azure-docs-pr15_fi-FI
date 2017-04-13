<properties
    pageTitle="Azure Active Directory-todennus App Services-sovelluksen määrittäminen"
    description="Opettele määrittämään Azure Active Directory-todennus App Services-sovelluksen."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Sovelluksen palvelusovelluksen Azure Active Directory-kirjautuminen käyttämään määrittäminen

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Tämän artikkelin avulla voit käyttää Azure Active Directory käyttöoikeustarkistuspalvelun Azure-sovelluksen palvelujen määrittäminen.

## <a name="express"> </a>Määrittäminen Azure Active Directory express asetusten avulla

13. Siirry [Azure portal]-sovellus. Valitse **asetukset**ja sitten **Todennus ja luvan**.

14. Jos todennus / todennus-ominaisuus ei ole käytössä, ota **Valitse**Siirry.

15. Valitse **Azure Active Directory**ja valitse sitten **Hallinta**-tilassa **Express** .

16. Valitse sovellus rekisteröidään Azure Active Directory **OK** . Tämä vaihtoehto Luo uusi rekisteröinti. Jos haluat valita aiemmin rekisteröinti sen sijaan, valitse **aiemmin luotu sovelluksen** ja Hae aiemmin luodun rekisteröinti sisällä alihallintaan nimi.
Valitse rekisteröinnin napsauttamalla sitä ja valitse **OK**. Valitse Azure Active Directory-asetukset-sivu valitsemalla **OK** .

    ![][0]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

17. (Valinnainen) Käytön rajoittaminen vain Azure Active Directory-todennetut käyttäjät sivustoon, määrittää **toiminnon, joka suoritetaan, kun pyyntö ei ole todennettu** kirjautuaksesi **sisään Azure Active Directory**. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Azure Active Directory todennusta varten.

17. Valitse **Tallenna**.

Olet nyt valmis käytettäväksi Azure Active Directory-sovelluksen todennusta varten.

## <a name="advanced"> </a>(Vaihtoehtoinen menetelmä) Azure Active Directory ja lisäasetusten määrittäminen manuaalisesti
Voit valita myös antaa asetukset manuaalisesti. Tämä on ensisijainen ratkaisu, jos haluat käyttää AAD vuokraajan eroaa vuokraajan, jolla voit kirjautua Azure. Viimeistele määritys, sinun on luotava rekisteröinti Azure Active Directoryn ja sitten on annettava joitakin rekisteröinnin tiedot sovelluksen-palveluun.

### <a name="register"> </a>Rekisteröidä sovelluksen Azure Active Directory-hakemistosta

1. Kirjaudu sisään [Azure portal]ja siirry sovelluksen. Kopioi **URL-osoite**. Tämä käytetään Azure Active Directory-sovelluksen määrittäminen.

3. Kirjautuminen [Azure perinteinen portaaliin] ja siirry **Active Directory**.

    ![][2]

4. Valitse hakemisto ja valitse sitten yläreunassa **sovellukset** -välilehti. Valitse Luo uusi sovelluksen rekisteröinti alaosassa **Lisää** .

5. Valitse **Lisää sovelluksen kehittää oman organisaation ulkopuolelta**.

6. Ohjatun sovelluksen lisäämisen Kirjoita sovelluksesi **nimi** ja valitse **Web Application ja/tai verkko-Ohjelmointirajapinnan** tyyppi. Valitse Jatka.

7. Liitä aiemmin kopioimasi sovelluksen URL-osoite **KIRJAUDU edelleen URL** -ruutuun. Kirjoita **Sovelluksen tunnus URI** -ruutuun, että URL-Osoitetta. Valitse Jatka.

8. Kun sovellus on lisätty, valitse **Määritä** -välilehti. Muokkaa **kertakirjautumisen** on lisätty path _/.auth/login/aad/callback_sovelluksen URL-osoite-kohdassa **Vastaa URL-osoite** . Esimerkiksi `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Varmista, että käytät HTTPS-malli.

    ![][3]

9. Valitse **Tallenna**. Kopioi **Asiakastunnus** -sovellukseen. Voit määrittää myöhempää käyttöä tämän sovelluksen.

10. Alas-painikkeita **Näkymän päätepisteet**, ja kopioi **Federation metatietojen tiedoston** URL-osoite ja Lataa asiakirja tai sen Siirry selaimessa.

11. **EntityDescriptor** pääelementti sisällä pitäisi olla **tunnus** -määritteen lomakkeen `https://sts.windows.net/` perään tietyn GUID-asiakasympäristöön (eli "vuokraajan tunnus"). Kopioi tämä arvo – se toimivat **Myöntäjä URL-osoite**. Voit määrittää myöhempää käyttöä tämän sovelluksen.

### <a name="secrets"> </a>Lisää Azure Active Directory-tiedot-sovellukseen

13. Siirry takaisin sisään [Azure portal]-sovellus. Valitse **asetukset**ja sitten **Todennus ja luvan**.

14. Jos todennus/todennus-ominaisuus ei ole käytössä, ota **Valitse**Siirry.

15. Valitse **Azure Active Directory**, ja valitse sitten **Lisäasetukset** - **Tilan hallinta**-kohdassa. Liitä Asiakastunnus ja myöntäjä URL-Osoitteen arvo, jonka olet hankkinut aiemmin. Valitse **OK**.

    ![][1]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

17. (Valinnainen) Käytön rajoittaminen vain Azure Active Directory-todennetut käyttäjät sivustoon, määrittää **toiminnon, joka suoritetaan, kun pyyntö ei ole todennettu** kirjautuaksesi **sisään Azure Active Directory**. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Azure Active Directory todennusta varten.

17. Valitse **Tallenna**.

Olet nyt valmis käytettäväksi Azure Active Directory-sovelluksen todennusta varten.

## <a name="optional-configure-a-native-client-application"></a>(Valinnainen) Native client-sovelluksen kokoonpanon määrittäminen

Azure Active Directory myös voit rekisteröidä alkuperäisiä, joka sisältää tarkemmin yhdistäminen käyttöoikeudet. Tämä on, jos haluat suorittaa kirjautumiset käyttämällä kirjastoon, kuten **Active Directory käyttöoikeuksien kirjastoon**.

1. Siirry [Azure perinteinen portal] **Active Directorysta** .

2. Valitse hakemisto ja valitse sitten yläreunassa **sovellukset** -välilehti. Valitse Luo uusi sovelluksen rekisteröinti alaosassa **Lisää** .

3. Valitse **Lisää sovelluksen kehittää oman organisaation ulkopuolelta**.

4. Valitse ohjatussa sovelluksen lisääminen Kirjoita sovelluksesi **nimi** ja valitse **Native Client sovelluksen** tyyppi. Valitse Jatka.

5. Kirjoita **Uudelleenohjata URI** -ruutuun sivuston _/.auth/login/done_ päätepisteen HTTPS-mallin avulla. Tämän arvon pitäisi olla samanlainen _https://contoso.azurewebsites.net/.auth/login/done_. Jos luot Windows-sovellus, käytä [paketin SUOJAUSTUNNUS](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) sen sijaan URI.

6. Kun alkuperäisen sovellus on lisätty, valitse **Määritä** -välilehti. Etsi **Asiakastunnus** ja Merkitse muistiin tätä arvoa.

7. Vieritä sivua alaspäin **muiden sovellusten käyttöoikeudet** -kohdassa ja valitse **Lisää sovellus**.

8. Etsi web-sovelluksen rekisteröityjen aiempi ja napsauta plusmerkkiä. Valitse Tarkista, sulje valintaikkuna. Jos web-sovelluksen ei löydy, siirry sen rekisteröinti ja Lisää uusi vastaa URL-osoite (esimerkiksi HTTP version nykyinen URL-osoite), valitse Tallenna ja toista sitten nämä vaiheet - sovelluksen näytetään luettelossa.

9. Juuri lisäämäsi uuden tapahtuman Avaa **Valtuutetun käyttöoikeudet** -valikko ja valitse **Access (appName)**. Valitse **Tallenna**.

On nyt määritetty alkuperäisen asiakassovellus, jotka voivat käyttää sovelluksen palvelusovelluksen.

## <a name="related-content"> </a>Liittyvät sisältö

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure perinteinen portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
