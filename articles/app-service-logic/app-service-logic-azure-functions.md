<properties
   pageTitle="Azure funktioilla logiikan sovelluksilla | Microsoft Azure"
   description="Opettele käyttämään logiikan sovelluksilla Azure-Funktiot"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Azure funktioilla logiikan sovelluksilla

Voit suorittaa mukautetun katkelmat C# tai node.js Azure funktioiden käytön muutamassa logiikan-sovellus.  [Azure-Funktiot](../azure-functions/functions-overview.md) on palvelimen vapaa tietojenkäsittely Microsoft Azure. Tästä on hyötyä suorittaa seuraavia tehtäviä:

* Mukautettu muotoilu tai kenttiä logiikan sovelluksen suorittaminen
* Työnkulun laskutoimitusten suorittaminen
* Logiikan sovellusten toimintojen kanssa funktioita, joita tuetaan C#- tai node.js laajentaminen

## <a name="create-a-function-for-logic-apps"></a>Logiikan sovellusten funktion luominen

On suositeltavaa luoda uutta funktiota Azure-Funktiot-portaalissa **Yleinen Webhook - solmu** tai **Yleinen Webhook - C#** -mallien avulla. Tämä automaattinen-Lisää malli, jota voidaan käyttää `application/json` logiikan-sovelluksesta.  Jos valitset **liittää** -välilehden Azure-funktioissa tunnisteen pitäisi olla **tilan** asettaminen **Webhook** ja **Yleinen JSON** **Webhook tyyppi** .  Funktiot, jotka käyttävät mallit havaitsi ja logiikka sovellusten suunnittelussa luettelossa automaattisesti **Azure Funktiot aikavyöhykkeistä.**

Webhook Funktiot hyväksyä pyynnön ja siirtää sitä kautta menetelmä `data` muuttuja. Voit käyttää omaa salataan ominaisuudet käyttämällä piste-merkintätapaa, kuten `data.foo`.  Esimerkiksi yksinkertaisen JavaScript-funktio, joka muuntaa tekstimerkkijonon päivämäärän DateTime-arvo näyttää seuraavalta:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Puhelun Azure-Funktiot logiikan-sovelluksesta

Suunnittelussa, jos valitset **Toiminnot ja** voit valita **Aikavyöhykkeistä Azure-Funktiot**.  Tämä näyttää säilöt-tilaukseesi ja avulla voit käyttää toimintoa, johon haluat soittaa.  

Kun valitset funktion, ohjelma pyytää määrittämään syötteen paketti-objekti. Tämä on viesti, joka logiikan app lähettää funktio ja sen on oltava JSON-objekti. Esimerkiksi jos haluat siirtää **Viimeisin** muokkauspäivämäärä Salesforce-käynnistimestä, funktio paketti saattaa näyttää tältä:

![Viimeinen modfied päivämäärä][1]

## <a name="trigger-logic-apps-from-a-function"></a>Funktion käynnistimen logiikan sovellukset

On myös mahdollista käynnistettävän logiikan-sovelluksen sisällyttäminen funktioon.  Voit tehdä tämän luoda logiikan app yksinkertaisesti manuaalinen käynnistäminen. Lisätietoja on kohdassa [logiikan kutsuttava päätepisteet nimellä](app-service-logic-http-endpoint.md).  Luo sitten-funktion sisällä HTTP-viestin, paketti, jonka haluat lähettää logiikan-sovelluksen manuaalinen käynnistäminen URL-osoite.

### <a name="create-a-function-from-the-designer"></a>Luo suunnittelija-funktio

Voit myös luoda node.js webhook funktion-suunnittelussa. Ensimmäisen kerran Valitse **Azure Funktiot aikavyöhykkeistä,** ja valitse sitten funktion säilö.  Jos et ole vielä tallentanut säilön, haluat luoda [Azure-Funktiot-portaalissa](https://functions.azure.com/signin). Valitse sitten **Luo uusi**.  

Voit luoda malleja tiedot, jotka haluat laskea perusteella, Määritä kontekstiobjekti, jota aiot siirtää funktiota. Tämä on oltava JSON-objekti. Esimerkiksi jos tiedoston sisällön FTP-toiminto, konteksti-paketti näyttää tältä:

![Konteksti-paketti][2]

>[AZURE.NOTE] Koska tätä objektia ei joka merkkijonona, sisältö lisätään suoraan JSON-paketti. Se on kuitenkin virheen, jos se ei ole JSON-tunnus (eli merkkijono tai JSON objektin/matriisi). Voit pakottaa merkkijonona, lisää lainausmerkit tämän artikkelin ensimmäisen kuvassa esitetyllä tavalla.

Suunnittelija luo sitten funktio-malli voit luoda tekstiin. Muuttujat valmiiksi luodaan, jota aiot välittää funktiolle kontekstin mukaan.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
