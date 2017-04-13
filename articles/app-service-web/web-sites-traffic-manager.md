<properties
    pageTitle="Hallinta Azure web app-liikenteen Azure liikenteen hallinta"
    description="Tässä artikkelissa on yhteenvetotiedot Azure liikenteen hallinta, miten se liittyy Azure verkkosovelluksissa."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Hallinta Azure web app-liikenteen Azure liikenteen hallinta

> [AZURE.NOTE] Tässä artikkelissa on yhteenvetotiedot for Microsoft Azure liikenteen hallinta, miten se liittyy Azure palvelun Web sovellukset. Lisätietoja Azure liikenne Managerin itse löytyy ohjesisältöä linkkejä tämän artikkelin lopussa.

## <a name="introduction"></a>Johdanto
Voit määrittää, miten verkkosovellukset pyynnöt jaetaan web Apps-sovellusten Azure App palvelun Azure liikenteen hallinta. Kun web app päätepisteet lisätään Azure liikenteen hallinta profiilin, Azure liikenteen hallinta pitää kirjaa siitä tila (käynnissä, pysäytetty tai poistetut) web sovellus niin, että se päättää, mitä ne päätepisteet tietoliikenteen vastaanottamiseen.

## <a name="load-balancing-methods"></a>Lataa Vastatilin menetelmät
Azure liikenteen hallinta käyttää eri kuormituksen Vastatilin kolmella. Nämä on kuvattu seuraavassa luettelossa, kuin ne koskevat Azure verkkosovelluksissa.

* **Automaattisesti**: Jos sinulla on web app kloonit eri alueilla, voit käyttää tätä menetelmää vastaamisessa kaikki web-asiakasohjelman liikenne yhden web App-sovelluksen määrittäminen ja toiseen web App-sovelluksen määrittäminen toisella alueella vastaamisessa liikenne siltä varalta, että ensimmäinen web app ei ole käytettävissä.

* **PYÖRISTÄ-funktiota Mikko**: Jos web app kloonit ovat eri alueilla, voit Tämä menetelmä Jaa liikenne tasaisesti eri alueilla verkkosovelluksissa.

* **Suorituskyvyn**: suorituskyvyn tapa jakaa liikenne asiakkaille lyhyimmän kiertoaika perusteella. Suorituskyky-menetelmää voi käyttää web Apps-sovellusten samalla alueella tai eri alueilla.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web-sovellusten ja liikenteen hallinta-profiileista
Voit määrittää web app-liikenteen hallinta profiilin luominen Azure liikenteen hallinta, käyttää jokin kolmesta ladata Vastatilin kuvatuilla tavoilla ja lisää sitten päätepisteet (Tässä tapauksessa verkkosovelluksissa), jonka haluat ohjata liikenne profiiliin. Web-sovelluksen tila (on lopetettu tai poistaa) säännöllisesti tiedoksi profiiliin, niin, että Azure liikenteen hallinta ohjata liikenne vastaavasti.

Kun käytät Azure liikenteen hallinta Azure kanssa, ota huomioon seuraavat seikat:

* Web Apps-sovelluksista on jo automaattisesti ja pyöreän pöydän toimintoja riippumatta siitä, missä web app-tilassa web app vain käyttöönotoissa samalla alueella.

* Käyttöönotoissa sama alue, joka käyttää Web Apps-sovellusten kanssa toisessa Azure pilvipalvelussa voit yhdistää molempien tietotyyppien päätepisteet käyttöön hybrid skenaarioita.

* Voit määrittää profiilin vain yksi web app-päätepiste alueittain. Kun valitset yhden alueen päätepiste web app-alueella olevat jäljellä web Apps-sovellusten käytettävistä profiilin valinta.

* Web app-päätepisteet, jotka määrität Azure liikenteen hallinta profiilin profiilin web-sovelluksen määrittäminen-sivulla **Toimialuenimet** -osassa näkyvät, mutta eivät voi muuttaa siellä.

* Kun olet lisännyt verkkosovellukseen profiiliin, web app-portaalisivu koontinäytössä **Sivuston URL-osoite** näkyy Mukautettu toimialueen URL-osoite web Appin, jos on määritetty jokin. Muussa tapauksessa näyttää liikenteen hallinta profiilin URL-osoite (esimerkiksi `contoso.trafficmgr.com`). Molemmat suoraan toimialuenimi web app- ja liikenteen hallinta URL-Osoitteen näkevät web-sovelluksen määrittäminen-sivulla **Toimialuenimet** -osassa.

* Mukautettuja toimialuenimiä toimii odotetulla tavalla, mutta lisäksi lisäämällä web Apps-sovellusten, sinun on määritettävä DNS-kartan osoittamaan liikenteen hallinta URL-osoite. Lisätietoja Azure web-sovelluksen mukautetun toimialueen määrittämisestä on artikkelissa [määrittäminen Azure sivustossa mukautettua toimialuenimeä](web-sites-custom-domain-name.md).

* Voit lisätä vain web-sovellukset, jotka ovat vakiotilassa Azure liikenteen hallinta-profiiliin.

## <a name="next-steps"></a>Seuraavat vaiheet

Käsitteellisiä ja tekninen yleiskatsaus Azure liikenteen hallinta-artikkelissa [Liikenteen hallinta yleiskatsaus](../traffic-manager/traffic-manager-overview.md).

Saat lisätietoja liikenteen hallinta käyttäminen Web Apps-blogimerkinnät [Käyttämällä Azure liikenteen hallinta Azure Web-sivustojen](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) ja [Azure liikenteen hallinta nyt integroida Azure Web-sivustoja](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
