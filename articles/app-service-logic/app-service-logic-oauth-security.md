<properties
    pageTitle="OAUTH-suojauksen SaaS yhdistimien ja API-sovelluksissa | Azure"
    description="Lue lisätietoja OAUTH-suojauksen yhdistimiä ja API-sovelluksissa Azure-sovelluksen palvelun; microservices arkkitehtuuri; SAAS"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Opi OAUTH tietoturvasta SaaS yhdistimet

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Monet ohjelmistoon Service (SaaS) yhdistimiä, kuten Facebookiin, Twitteriin, DropBox ja niin edelleen pakolliseksi tarkistamiseen OAUTH-protokollan avulla.  Kun nämä SaaS yhdistimiä logiikan sovelluksista, annamme yksinkertaistettu käyttäjäkokemus, jossa napsauttamalla "Hyväksy" logiikan sovellusten suunnittelussa. Kun olet **Hyväksy**, sinua pyydetään Kirjaudu tässä (jos se ei vielä ole) ja anna lupaa muodostaa puolestasi SaaS-palveluun. Kun Anna suostumus ja määritä, logiikan sovelluksia jälkeen voit käyttää SaaS palveluista.

## <a name="create-your-own-saas-app"></a>SaaS-sovelluksen luominen
Tämä yksinkertaistettu ongelma on mahdollista, koska kopiointia aiemmin luotu ja rekisteröity tämän sovelluksen SaaS palveluista.  Tietyissä tapauksissa haluat ehkä rekisteröi ja käytä omaa sovellus.  Tämä on tarpeen, esimerkiksi kun haluat käyttää näitä SaaS yhdistimet mukautettuihin sovelluksiin. Tässä esimerkissä käytetään DropBox-yhdistin, mutta prosessi on sama kaikille yhdistimiä, jotka ovat riippuvaisia OAUTH.

Myös logiikan sovellusten yhteydessä, voit käyttää omia sovelluksen oletussovellus, annamme sijaan. Jos "Hyväksy"-painike ei onnistu, voit yrittää luoda omia sovelluksen. Seuraavassa on luettelo Twitter-yhdistimen seuraavasti:

1. Avaa Twitter-yhdistin Azure esikatselu-portaalissa. Valitse **Selaa** > **API-sovellukset**. Valitse Twitter-yhdistin:  
    ![][1]

2. Valitse **asetukset** > **todennus**:  
    ![][2]

3. Kopioi **Uudelleenohjata URI** -arvoa.  
    ![][3]

4. Siirry [Twitter-](http://apps.twitter.com) ja **uuden sovelluksen luominen**. Liitä kopioitu Twitter-yhdistin **Uudelleenohjata URI** -arvoa **Takaisinsoitto URL** -ominaisuus: ![][4]  
5. Kun Twitter-sovellus on luotu, valitse **avain ja Access-tunnukset**. Kopioi seuraavat arvot.
6. Twitter yhdistimen käyttöoikeuksien, asetusten Liitä nämä arvot **Asiakastunnus** ja **Asiakkaan salaisen** ominaisuudet:   
    ![][5]  
7. Tallenna yhdistimen asetukset.  

Sinun pitäisi nyt, käytä sitten yhdistimen logiikan sovelluksista. Kun tämä yhdistin logiikan sovelluksista, se käyttää sovelluksen oletussovellus sijaan.  

> [AZURE.NOTE] Jos sovellus on sallittua aiemmin, voit joutua reauthorize sovellus.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
