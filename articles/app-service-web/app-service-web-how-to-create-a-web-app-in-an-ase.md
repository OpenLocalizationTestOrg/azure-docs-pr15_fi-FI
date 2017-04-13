<properties
    pageTitle="Web-sovelluksen luominen sovelluksen Service-ympäristössä"
    description="Web-sovellusten ja sovelluksen palvelusopimusten vaihtoehdot luominen sovelluksen Service-ympäristössä"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Web-sovelluksen luominen sovelluksen Service-ympäristössä

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään luomisesta web Apps-sovellusten ja sovelluksen palvelusopimusten vaihtoehdot [App Service-ympäristössä](app-service-app-service-environment-intro.md) (Ietokannan). 

> [AZURE.NOTE] Jos haluat lisätietoja web-sovelluksen luominen, mutta ei tarvitse tehdä App Service-ympäristössä, katso [.NET-web-sovelluksen luominen](web-sites-dotnet-get-started.md) tai jokin muiden kielten ja kehysten liittyvät opetusohjelmat.

## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa oletetaan, että olet luonut sovelluksen Service-ympäristössä. Jos sinulla ole vielä, katso [luominen sovelluksen Service-ympäristössä](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Web-sovelluksen luominen

1. Valitse [Azure Portal](https://portal.azure.com/)- **Uusi > Web + Mobile > Web Appin**. 

    ![][1]

2. Valitse tilauksen.  

    Jos sinulla on useita tilauksia Huomaa, että luomaan-sovellus App ympäristössäsi sinun on käytettävä samaa tilaus, jota käytit luodessasi ympäristön. 

3. Valitse tai luo resurssiryhmä.

    *Resurssiryhmät* , joiden avulla voit hallita liittyvät Azure resurssit yksikkönä ja on hyötyä, kun sovellus säännöistä *Roolipohjainen käyttöoikeuksien valvonta* (RBAC). Lisätietoja on artikkelissa [Azure resurssien hallinta][ResourceGroups]. 

4. Valitse tai Luo sovelluksen palvelusopimus.

    *Sovelluksen palvelusopimusten vaihtoehdot* hallitaan joukot web Apps-sovelluksista.  Yleensä kun valitset hinnat, hinta, jotka veloitetaan otetaan käyttöön App palvelusopimus sijaan yksittäisiä sovellukset. Ietokannan maksat Laske esiintymät Ietokannan varattu eikä mikä on lueteltu ASP.  Voit skaalata määrittäminen sovelluksen palvelun esiintymät verkkosovellukseen esiintymien määrän suunnitelma ja se vaikuttaa kaikkiin web Apps-sovellusten kyseiseen järjestelyyn.  Jotkin ominaisuudet, kuten sivuston paikkojen tai VNET integrointi on myös määrä rajoitusten sisällä suunnitelma.  Lisätietoja on artikkelissa [Azure App palvelun suunnitelmien yleiskatsaus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Tunnistat oman Ietokannan App palvelusopimusten vaihtoehdot katsomalla sijaintiin, jossa tilauksen nimen kohdassa merkille.  

    ![][5]

    Jos haluat käyttää sovelluksen Service-palvelupaketti, joka on jo App ympäristössäsi, valitse haluamasi suunnitelma. Jos haluat luoda uuden sovelluksen palvelun suunnitelman, on seuraavassa osassa tässä opetusohjelmassa, [Luo-sovelluksen palvelusopimus App Service-ympäristössä](#createplan).

5. Web-sovelluksen nimi ja valitse sitten **Luo**. 

    Jos yhteyttä Ietokannan käyttää ulkoisen VIP Ietokannan-sovelluksen URL-osoite on: [*nimi*]. [*App palvelun ympäristön nimi*]. [*nimi*] sijaan p.azurewebsites.net. azurewebsites.net
    
    Jos yhteyttä Ietokannan käyttää sisäinen VIP sitten sovelluksen URL-osoite siten, että Ietokannan: [*nimi*]. [*alitoimialueen määritetty Ietokannan luonnin aikana*]   
    Kun olet valinnut ASP Ietokannan luonnin aikana näet alitoimialueen **nimen** alapuolella päivittäminen

## <a name="createplan"></a>Luo sovellus-palvelusopimus

Kun luot sovelluksen palvelusopimus App Service-ympäristössä, työntekijä-vaihtoehdot ovat eri, kuin Ietokannan ei ole jaettu työntekijöiden.  Sinun on käytettävä työntekijät ovat oikeat, joka on varattu Ietokannan sinua.  Tämä tarkoittaa, että voit luoda uuden suunnitelman, tarvitset lisää työntekijöiden kohdistettu Ietokannan työntekijä-ryhmään kuin esiintymät kokonaismäärän kaikissa suunnitelmia jo työntekijä kyseisen sarjassa.  Jos sinulla ei ole tarpeeksi työntekijöiden Ietokannan työntekijä-varannon luo suunnitelma, tarvitset Ietokannan järjestelmänvalvojaa ne lisätään saa käyttöä varten.

Sovelluksen palvelun palvelupakettien App Service-ympäristössä ylläpitämä toiseen ero on puuttuminen hinnoittelua valinta.  Kun sovellus Service-ympäristössä maksat järjestelmä käyttää Laske resurssien ja ei ole lisätty kulujen suunnitelmat, ympäristössä.  Kun luot sovelluksen-palvelusopimus voit valita tavallisesti hinnoittelu palvelupaketti, joka määrittää laskutukseen.  Sovelluksen Service-ympäristössä on käytettävä yksityinen sijainti, jossa voi luoda erilaisen sisällön.  Voit maksaa ympäristön ja ei, jos haluat hallita sisällön.

Seuraavien ohjeiden näyttää, miten voit luoda sovelluksen palvelusopimus samalla, kun luot verkkosovellukseen opetusohjelman edellisessä kohdassa kuvatulla.

1. Valitse **Luo uusi** suunnitelma valintaan Käyttöliittymä ja anna palvelupakettiisi samalla tavalla kuin Ietokannan ulkopuolella normaalisti.

2. Valitse Ietokannan, jota haluat käyttää sijainti-valitsimesta.

    Koska olennaisesti yksityinen käyttöönoton sijainti on sovelluksen Service-ympäristössä, se näyttää Valitse sijainti. 

    ![][2]

    Sijainti-valitsin Ietokannan valinnan, kun sovellus palvelun suunnitelman luominen Käyttöliittymän päivittää.  Sijainti näkyy nyt Ietokannan järjestelmän ja alueen nimi, se on ja hinnoittelu suunnitelman valitsin korvaa työntekijä resurssivarantoon-valitsin.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Työntekijän resurssivarantoon valitseminen

Tavallisesti Azure-sovelluksen palvelun ja sen ulkopuolisten App Service-ympäristössä, on 3 Laske koot, jotka ovat käytettävissä ja erillinen hinta suunnitelman valinta.  Samalla tavalla Ietokannan, voit määrittää 3 jakavat työntekijöiden ja määritä Laske koko, jota käytetään työntekijän kyseiseen resurssivarantoon.  Mitä tämä tarkoittaa sitä Ietokannan alihallinnat on sen sijaan, että valitseminen hinnoittelu suunnitelma ja Laske koon App palvelusopimus, voit valita, mitä kutsutaan *Työntekijä resurssivarantoon*.  

Työntekijän resurssivarantoon valinnan Käyttöliittymä näyttää käytettäviä kyseiseen työntekijä resurssivarantoon nimen alapuolella Laske koon.  Käytettävissä oleva määrä viittaa esiintymät ovat käytettävissä kyseiseen resurssivarantoon määrän laskemiseen.  Yhteensä resurssivarantoon voi sisältyä esiintymiä näin monta, mutta tämä arvo viittaa yksinkertaisesti montako eivät ole käytössä.  Jos haluat säätää App Service-ympäristössä, voit lisätä Lisää Laske resurssien ohjeaiheessa [Sovelluksen Service-ympäristössä](app-service-web-configure-an-app-service-environment.md).

![][4]

Tässä esimerkissä näet vain kaksi työntekijä-jakavat. Tämä johtuu Ietokannan järjestelmänvalvojan varattu isäntien vain nämä kaksi työntekijä jakavat kyselyjä.  Kolmas näkyviin, kun on varattu siihen VMs.  

## <a name="after-web-app-creation"></a>Web-sovelluksen luomisen jälkeen

On muutama huomioon otettavia seikkoja käynnissä web Apps-sovellusten ja sovelluksen palvelusopimusten vaihtoehdot-Ietokannan, jotka on otettava huomioon hallintaa varten.  

Aiemmin mainittuja Ietokannan omistaja on vastuussa järjestelmän koon ja tuloksena ne ovat myös vastuussa varmistetaan, että on riittävä kapasiteetti isännöimiseen haluamasi sovellus palvelusopimusten vaihtoehdot. Jos sovellusta ei ole käytettävissä, voit voi luoda sovelluksen palvelusopimus.  Tämä on myös tosi web Appin skaalaus.  Jos tarvitset esiintymiä tarvitse Hanki sovellus palvelun ympäristön järjestelmänvalvojaa lisäämään useita työntekijöiden.

Kun olet luonut online ja sovelluksen palvelusopimus on hyvä skaalata.  Ietokannan aina täytyy olla vähintään 2 esiintymää App Service-palvelupaketin vikasietoa antamaan sovelluksia.  Skaalauksen sovelluksen-palvelusopimus Ietokannan on sama kuin tavallinen App palvelusopimus Käyttöliittymän välityksellä.  Lisätietoja skaalauksen, [miten verkkosovellukseen App Service-ympäristössä](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
