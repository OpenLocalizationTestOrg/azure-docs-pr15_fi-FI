<properties
    pageTitle="Microsoft Account todennus sovelluksen palvelut-sovelluksen määrittäminen"
    description="Opettele määrittämään Microsoft Account todennus sovelluksen palvelut-sovelluksen."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Sovelluksen palvelusovelluksen käyttämään Microsoft Account käyttäjätunnus määrittäminen

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Tässä ohjeaiheessa esitellään Azure App palvelun käyttämään Microsoft Account tarjoajan käyttöoikeuksien määrittämisestä. 

## <a name="register-microsoft-account"> </a>Rekisteröidä sovelluksen Microsoft-tilillä

1. Kirjaudu sisään [Azure portal]ja siirry sovelluksen. Kopioi **URL-osoite**, jossa myöhemmin voit määrittää sovelluksen Microsoft Account.

2. Microsoft Account Developer Center [Omat sovellukset] -sivulle ja kirjaudu sisään Microsoft-tilisi kanssa, tarvittaessa.

3. Valitse **Lisää sovellus**ja valitse Kirjoita sovelluksen nimi ja valitse **Luo sovellus**.

4. Muistiin **Tunnus**, kun tarvitset sitä myöhemmässä vaiheessa. 

5. Valitse "Ympäristöjen" **Lisää ympäristö** ja valitse "WWW".

6. "Uudelleenohjata URI-kohdassa Anna sovelluksen päätepiste ja valitse sitten **Tallenna**. 
 
    >[AZURE.NOTE]Uudelleenohjauksen URI on lisätty path _/.auth/login/microsoftaccount/callback_sovelluksen URL-osoite. Esimerkiksi `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Varmista, että käytät HTTPS-malli.

7. Valitse **Luo uusi salasana**-sovelluksen tietoja,". Kirjoita muistiin arvo, joka tulee näkyviin. Kun poistut sivusta, sitä ei näytetä uudelleen.


    > [AZURE.IMPORTANT] Salasana on tärkeän tunnistetiedon. Älä salasana jakaminen kuka tahansa tai jakaa sen asiakas-sovelluksesta.

## <a name="secrets"> </a>Sovellusten palvelusovellus Lisää Microsoft-tilin tiedot

1. Siirry [Azure portal]-sovellus, valitse **asetukset**uudelleen > **todennusta / luvan**.

2. Jos todennus / todennus-ominaisuus ei ole käytössä, ota **käyttöön**.

3. Valitse **Microsoft-tili**. Tunnus ja salasana arvot, jotka olet hankkinut aiemmin sijoittaminen ja käyttöön myös kaikki alueet sovellus edellyttää. Valitse **OK**.

    ![][1]

    Oletusarvoisesti App palvelun tarjoaa todennuksen, mutta rajoita sivuston sisältö ja ohjelmointirajapinnan valtuutettujen käyttöoikeutta. Käyttäjien on sallittava sovelluksen-koodisi.

4. (Valinnainen) Käytön rajoittaminen sivustoon vain Microsoft-tilin käyttäjille, määrittää **toiminnon, joka suoritetaan, kun pyyntö ei ole todennettu** **Microsoft**-tilille. Tämä edellyttää, että kaikki palvelupyynnöt todennus ja kaikki Todentamattomille pyynnöt ohjataan Microsoft-tilille todennusta varten.

5. Valitse **Tallenna**.

Olet nyt valmiina käyttämään Microsoft Account sovelluksen todennusta varten.

## <a name="related-content"> </a>Liittyvät sisältö

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Omat sovellukset]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
