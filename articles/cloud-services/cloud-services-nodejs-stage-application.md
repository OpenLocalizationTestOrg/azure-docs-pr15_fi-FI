<properties 
    pageTitle="Vaiheen cloud palvelun käyttöönoton (Node.js) | Microsoft Azure" 
    description="Opettele väliaikaisen ympäristöön Azure sovelluksen käyttöönotto ja sitten käyttöönotto tuotantoympäristössä käyttämisestä Virtual IP (VIP) Vaihda." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Väliaikaisen sovelluksen Azure-tietokannassa

Pakattu sovelluksen voidaan ottaa käyttöön ennen sen Siirry tuotantoympäristössä, jossa sovellus on käytettävissä Internetissä testataan Azure väliaikaisesta ympäristössä. Väliaikaisen ympäristö ei täsmälleen kuten tuotantoympäristössä, sillä erotuksella, että voit käyttää vain vaiheistettu obfuscated URL-Osoitteen, joka on luonut Azure-sovellukseen. Kun olet varmistanut, että sovelluksesi toimii oikein, se voidaan ottaa käyttöön tuotantoympäristössä suorittamalla Virtual IP (VIP) Vaihda.

> [AZURE.NOTE] Tässä artikkelissa kuvattuja koskevat vain solmu sovellusten isännöidään kuin Azure-pilvipalvelussa.

## <a name="step-1-stage-an-application"></a>Vaihe 1: Vaiheen sovelluksen

Tämän tehtävän käsitellään sovelluksen vaiheittaisesti **Microsoft Azure PowerShell**-toiminnolla.

1.  Kun julkaiset palvelu, riittää, että välittää **-paikka** parametri **Julkaise AzureServiceProject** cmdlet-komento.

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Kirjaudu sisään [Azure perinteinen portaaliin] ja valitse **Pilvipalveluihin**. Kun pilvipalvelussa sijaitsevaan luodaan ja **väliaikaisen** sarakkeen tila on päivitetty **käynnissä**, napsauta palvelunimi.

    ![portaalin näytetään käytössä palvelu][cloud-service]

3.  Valitse **raporttinäkymät-ikkunan**ja valitse sitten **vaiheet**.

    ![cloud palvelun koontinäyttö][cloud-service-dashboard]

4. Huomautus oikealle **Sivuston URL-osoite** -merkinnän arvo. DNS-nimi on obfuscated sisäinen ID, jolla Azure luodaan.

    ![sivuston URL-osoite][cloud-service-staging-url]

Nyt voit varmistaa, että sovellus toimii oikein väliaikaisen ympäristössä käyttämällä väliaikaisen sivuston URL-osoite.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Vaihe 2: Tuotannon mukaan muunsivat VIPs sovelluksen päivittäminen

Kun olet varmistanut sovelluksen väliaikaisen ympäristössä päivitetyn version, voit nopeasti määrittää sen käytettäväksi tuotannon mukaan virtual IP-osoitteet (VIPs) testaus- ja ympäristöjen vaihtaminen.

> [AZURE.NOTE] Tässä vaiheessa oletetaan, että olet jo käyttöön sovelluksen tuotannon ja vaiheistettu sovelluksen päivitetyn version.

1.  Kirjaudu sisään [Azure perinteinen portal], valitse **Pilvipalveluihin** ja valitse sitten palvelunimi.

2.  **Raporttinäkymät-ikkunan**Valitse **väliaikaisen**ja valitse sitten **Vaihda** sivun alareunassa. VIP Swap-valintaikkuna avautuu.

    ![swap VIP-valintaikkuna][vip-swap-dialog]

3.  Tarkista tiedot ja valitse sitten **OK**. Kaksi ominaisuuksissa alkaa väliaikaisen käyttöönoton siirtyy tuotannon ja tuotannon käyttöönoton siirtyy väliaikaisen päivitetään.

Olet vaiheistettu käyttöönoton ja päivittää tuotannon käyttöönoton vaihtaminen VIPs väliaikaisen käyttöönoton kanssa.

## <a name="additional-resources"></a>Lisäresursseja

- [Ottamisesta käyttöön Palvelupäivitys tuotannon mukaan VIPs Azure vaihtaminen]

[Azure perinteinen portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Ottamisesta käyttöön Palvelupäivitys tuotannon mukaan VIPs Azure vaihtaminen]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
