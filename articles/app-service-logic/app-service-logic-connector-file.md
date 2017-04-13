<properties
    pageTitle="Tiedoston Connectorin avulla logiikan sovelluksissa | Sovelluksen Microsoft Azure-palvelu"
    description="Voit luoda ja tiedoston yhdistimen tai API-sovelluksen määrittäminen ja käyttäminen logiikan-sovellus App Azure-palvelussa"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Tiedoston yhdistimen käytön aloittaminen ja lisää se logiikan-sovellukseen
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2014 – 12-01 – esikatselu rakenteen versio.

Muodosta yhteys tiedostojärjestelmässä, jos haluat ladata, lataa ja lisää tiedostojen host tietokoneeseen. Logiikan sovellukset voivat aiheuttaa erilaisia tietolähteitä ja voit hakea ja käsitellä tietoja yhdistimet tarjouksen perusteella. Voit lisätä tiedoston yhdistimen yrityksen työnkulku ja prosessin tietoja tämän työnkulun logiikan-sovelluksen osana. 

Tiedoston yhdistimen käyttää Hybrid Yhteyksienhallinnan hybrid yhteys Host (isäntä)-tiedostojärjestelmässä.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Tiedoston yhdistimen logiikan-sovelluksen luominen ##
Tiedoston yhdistimen käyttämään sinun on ensin tiedosto yhdistimen API-sovelluksen luominen. Voit tehdä tämän seuraavasti:

1.  Avaa käyttämällä [+ uusi Azure Marketplace vaihtoehto vasemmassa reunassa Azure-portaalin.
2.  Etsi "tiedoston yhdistimen".
3.  Valitse **Tiedosto-yhdistin (ennakkoversio)** hakutuloksista.
4.  Valitse **Luo** -painike
5.  Määritä tiedoston yhdistimen seuraavasti:  
![][1]

    - **Nimi** - antaa tiedoston yhdistimen nimi
    - **Pakettiasetukset**
        - **Pääkansio** - pääkansion polku Määritä Host (isäntä)-tietokoneessa. . D:\FileConnectorTest
        - **Palvelun Bus yhteysmerkkijonon** : Anna palvelun Bus yhteysmerkkijonon. Varmista, että palvelun bus nimitila on vakio-tyypin ja Basic palvelun Bus releitä käyttöä varten.  Palvelun Bus välitys avulla luodaan yhteys Hybrid Yhteyksienhallinnan.
    - **Sovelluksen palvelusopimus** - tai Luo sovelluksen palvelusopimus
    - **Hinnat taso** - Valitse Connectorin hinnoittelu taso
    - **Resurssiryhmä** - tai luo resurssiryhmä, joissa yhdistimen olisi sijaitsevat
    - **Tilauksen** - tilaus haluat luoda Tämä yhdistin
    - **Sijainti** - kohtaa, johon haluat yhdistimen käyttöön maantieteellisen sijainnin valitseminen

4. Valitse Luo. Luodaan uusi tiedosto-yhdistin

## <a name="configure-hybrid-connection-manager"></a>Hybrid Yhteyksienhallinnan määrittäminen ##
Kun API sovelluksen esiintymää on luotu, siirry sen Raporttinäkymät-ikkunan.  Voit tehdä tämän valitsemalla Selaa > API sovellukset > Valitse tiedosto-yhdistin API-sovellus.  Tässä Hybrid Yhteyksienhallinnan varten on määritettävä.
Katso lisätietoja määrittäminen ja vianmääritys Hybrid Yhteyksienhallinnan [Hybrid yhteyden hallinnan avulla].

## <a name="using-the-file-connector-in-your-logic-app"></a>Tiedoston Connectorin avulla logiikan-sovelluksessa ##
Kun API-sovellus on luotu, voit nyt käyttää tiedoston yhdistimen toiminnon logiikan-sovelluksen. Voit tehdä tämän on:

1.  Logiikan uuden sovelluksen luominen ja valitse saman resurssiryhmän on tiedoston yhdistin. Luo [uuden logiikan sovelluksen]ohjeiden.

2.  Avaa "Käynnistimet ja toiminnot" luotu logiikan sovelluksen, voit avata logiikan sovellukset suunnittelu ja määrittää oman työnkulun.

3.  Tiedoston yhdistimen näkyy "API tämän resurssiryhmä-sovellusten"-osassa oikealla puolella-valikoimassa.

4.  Voit poistaa tiedoston yhdistimen API-sovelluksen editoriin valitsemalla "tiedosto"-yhdistin. tiedoston yhdistimen paljastaa yksi käynnistin ja 4 toiminnot:  
![][5]

6.  Näiden yksitellen paljastaa tiettyjä ominaisuuksia. Alla olevassa kuvassa luetellaan käynnistin ominaisuudet ja poistaa tiedoston toiminto:  
![][6]

7. Kun ne on määritetty, käynnistin ja toiminto voidaan oman työnkulku. Vastaavasti muita toimintoja voidaan määrittää myös.

> [AZURE.NOTE] Tiedoston käynnistin poistaa tiedoston, kun se on onnistuneesti luku-kansiosta.

## <a name="file-connector-rest-apis"></a>Tiedoston yhdistimen REST API ##
Jos haluat käyttää yhdistimen logiikan-sovelluksen ulkopuolella, voit hyödyntää yhdistimen näyttämiä REST API. Voit voit Näytä tämän API määritelmät käyttämällä Selaa -> Api App -> tiedoston yhdistin. Napsauta API määritelmä linssin voit tarkastella kaikkia Tämä yhdistin näyttämiä ohjelmointirajapinnan yhteenveto-osassa:  
![][7]

Tietoja API tukikäytännöistä [tiedoston yhdistimen API-määritys].

## <a name="do-more-with-your-connector"></a>Älä lisää ja sitten yhdistimen
Yhdistimen on luotu, voit lisätä business työnkulkuun logiikan sovellusta käytettäessä. Katso [mitä logiikan sovelluksia?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Jos haluat aloittaa Azure logiikan sovellukset ennen rekisteröimässä Azure-tili, siirry [Yritä logiikan sovellus](https://tryappservice.azure.com/?appservice=logic), jossa lyhytkestoinen starter logiikan-sovelluksen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

Tarkastele Swagger REST API-viittaus [yhdistimiä](http://go.microsoft.com/fwlink/p/?LinkId=529766)ja API sovellusten viittaus.

Voit myös tarkastella suorituskyvyn Tilasto- ja tietoturva yhdistin. Katso [hallinta ja valvonta valmiin API-sovellusten ja yhdistin](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Logiikan uuden sovelluksen luominen]: app-service-logic-create-a-logic-app.md
[Tiedoston yhdistimen API määritys]: https://msdn.microsoft.com/library/dn936296.aspx
[Hybrid yhteyden hallinnan avulla]: app-service-logic-hybrid-connection-manager.md
