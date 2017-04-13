<properties 
    pageTitle="Todentamismenetelmä Mobile välitys REST API - Manuaalinen määritys"
    description="Tässä artikkelissa kuvataan määrittäminen manuaalisesti Mobile välitys REST API-todennus" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Todentamismenetelmä Mobile välitys REST API - Manuaalinen määritys

Tämä on lisäys-ohjeista todentaa [Mobile välitys REST API kanssa](mobile-engagement-api-authentication.md). Varmista, että voit lukea sen ensimmäisen kerran, jos haluat saada yhteydessä. Tässä artikkelissa on vaihtoehtoinen tapa tehdä for Mobile välitys-REST API Azure-portaalissa käyttöoikeuksien määrittämisestä erikseen asetukset. 

>[AZURE.NOTE] Alla olevia ohjeita perustuvat [Active Directory-opas](../resource-group-create-service-principal-portal.md) ja mukautetut mitä vaaditaan Mobile välitys ohjelmointirajapinnan todennusta varten. Niin siihen viitataan halutessasi ymmärtää alla olevat vaiheet yksityiskohtaisesti. 

1. Kirjaudu sisään – [perinteinen portal](https://manage.windowsazure.com/)Azure-tiliisi.

2. Valitse vasemmanpuoleisessa ruudussa **Active Directorysta** .

     ![Valitse Active Directory][1]

3. Valitse **Oletus Active Directory** Azure-portaalin. 

     ![Valitse kansio][2]

    >[AZURE.IMPORTANT] Tämä menetelmä toimii vain, kun käsittelet oletusarvo Active Directory-tilisi ja ei toimi, jos teet tämän Active Directoryssa, jotka olet luonut tilissäsi. 

4. Tarkastelemaan sovellukset-kansiossa, valitse **sovellukset**.

     ![Näytä sovellukset][3]

5. Valitse **Lisää**. 

     ![sovelluksen lisääminen][4]

6. Valitse **oman organisaation kehittää sovelluksen lisääminen**

     ![uusi sovellus][5]

6. Kirjoita sovelluksen nimi ja valitse sovelluksen tyypin **WEB APPLICATION ja/tai verkko-Ohjelmointirajapinnan** ja napsauttamalla Seuraava-painiketta.

     ![sovelluksen nimi][6]

7. Voit antaa tyhjä URL-osoitteet **KIRJAUDU edelleen URL-osoite** ja **Sovelluksen tunnus URI**. Niitä ei käytetä tässä tapauksessa ja URL-osoitteet itse ei tarkisteta.  

     ![sovelluksen ominaisuudet][7]

8. Tämä lopussa on AAD-sovelluksessa, jossa seuraavalta aiemmin annettu nimi. Tämä on oman **AD\_Sovelluksen\_nimi** ja kirjoita se muistiin.  

     ![sovelluksen nimi][8]

9. Napsauta apuohjelman nimeä ja valitse sitten **Määritä**.

     ![App-sovelluksen määrittäminen][9]

10. Huomioi, jota käytetään Asiakastunnus **asiakkaan\_tunnus** oman API puhelujen. 

     ![App-sovelluksen määrittäminen][10]

11. Siirry **näppäimet** -osioon ja Lisää avain ajallisesta mieluiten kahden vuoden (voimassaolon) ja valitse **Tallenna**. 

     ![App-sovelluksen määrittäminen][11]


12. Kopioi arvo, joka näkyy näppäimen sellaisena kuin se näkyy vain nyt ja ei ole tallennettu, jotta ei näytetä koskaan uudelleen heti. Jos kadotat se tarvitse Luo uusi avain. Tämä on **CLIENT_SECRET** API puheluiden. 

     ![App-sovelluksen määrittäminen][12]

    >[AZURE.IMPORTANT] Tätä näppäintä päättyy, joka on määritetty, joten varmista, että uusia sen tullessa aika muuten API-todennus ei enää toimi keston loppuun. Voit myös poistaa ja luo avain uudelleen, jos arvelet, että se on ongelma.
 
13. Valitsemalla **Näytä PÄÄTEPISTEET** painikkeen nyt joka avautuu **Sovelluksen päätepisteet** -valintaikkuna. 

    ![][13]

14. Sovelluksen päätepisteet-valintaikkunassa kopioi **OAUTH 2.0 SUOJAUSTUNNUKSEN PÄÄTEPISTE**. 

    ![][14]

15. Tämä päätepiste on URL-osoitteen GUID on oman **TENANT_ID** niin Pane merkille sen seuraavassa muodossa: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Nyt on jatkuu sovelluksen käyttöoikeuksien määrittämistä varten. Tämä on avata [Azure portal](https://portal.azure.com). 

17. Valitse **Resurssi-ryhmät** ja Etsi **Mobile välitys** resurssiryhmä.  

    ![][15]

18. Valitse **Mobile välitys** resurssiryhmä ja siirry tähän **asetukset** -sivu. 

    ![][16]

19. Valitse **käyttäjät** -asetukset-sivu ja valitse **Lisää** kunkin käyttäjän lisäämiseen. 

    ![][17]

20. Valitse **Valitse rooli**

    ![][18]

21. Valitse **omistaja**

    ![][19]

22. Etsi sovelluksesi nimi **AD\_Sovelluksen\_nimi** hakuruutuun. Et näe tätä tähän oletusarvoisesti. Kun löydät, napsauttamalla sitä ja valitse sitten sivu alareunassa **Valitse** . 

    ![][20]

23. **Lisää Access** -sivu, se näkyy **1 käyttäjä**, 0 ryhmät. Valitse tämä sivu Vahvista muutokset valitsemalla **OK** . 

    ![][21]

On nyt valmis AAD vaadittu ja kaikki määritettyjen Soita API. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



