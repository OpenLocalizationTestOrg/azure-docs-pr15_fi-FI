<properties 
    pageTitle="Johdanto App Serviceen Linux | Microsoft Azure" 
    description="Lisätietoja sovelluksen palvelun Linux." 
    keywords="sovelluksen Azure-palvelu, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Sovelluksen palvelun Linux esittely
App palvelun Linux on parhaillaan julkinen esikatselu ja suorittaminen online tukee grafiikkatiedostomuotoja Linux. 

## <a name="overview"></a>Yleiskatsaus ##
Asiakkaiden avulla sovelluksen palvelun Linux host web Apps-sovellusten grafiikkatiedostomuotoja Linux tuetun sovelluksen pinoa varten. Seuraavat ominaisuudet-osassa on lueteltu tuetut sovelluksen pinoa.

## <a name="features"></a>Ominaisuudet ##
Sovelluksen palvelun Linux tukee tällä hetkellä seuraavia sovelluksen pinoa

- Node.js
- PHP

Asiakkaat voivat ottaa käyttöön niiden sovellusten avulla

- FTP.
- Paikallinen Git.
- GitHub tai BitBucket.

Sovelluksen skaalauksen


- Asiakkaiden skaalata niiden online ylös ja alas muuttamalla niiden sovelluksen-palvelun suunnittelu-taso. 
- Asiakkaiden voi skaalata niiden sovellusten ja suorita niiden app useita kertoja allekirjoitusvarmenteella niiden SKU yli.

Joitakin perustoimintoja Kudu toimivat

- Ympäristössä.
- Asennuksia.
- Tavallinen konsolissa.

## <a name="limitations"></a>Rajoitukset ##

Azure hallinta-portaalin vain Näytä Linux palvelun sovelluksen tuetut ominaisuudet ja piilottaa muut. Tiimimme ottaminen käyttöön enemmän toimintoja kuin olemme säilyttää heijastavan tämä hallinta-portaalissa. Jotkin ominaisuudet, kuten VNET integrointi ja AAD tai kolmannen osapuolen todennuksen tai Kudu sivuston tunnisteet eivät tällä hetkellä toimi. Mutta ne toimivat lähestyessä päivitämme sekä ohjeet ja blogin muutoksista.

Julkinen esikatselu on käytettävissä vain seuraavilla alueilla

-   Länsi US.
-   Länsi Europe.
-   Kaakkoisaasialaiset Aasian.

Web app Linux tuetaan vain varattu sovelluksen palvelun suunnitelmien ja ei ole vapaa tai jaettu taso. Sovelluksen palvelusopimusten vaihtoehdot tavalliseen ja Linux web Apps-sovellusten ovat myös toisensa poissulkevia, joten et voi luoda Linux verkkosovellukseen-Linux sovelluksen-palvelusopimus.

Resurssiryhmä, jotka eivät sisällä-Linux verkkosovelluksissa samalla alueella on luotava Linux Web Appissa.

Web Apps-sovellusten päällekkäisen kierrättäminen puuttuminen vuoksi asiakkaiden tulee odottaa pieni käyttökatkot verkkosovellukseen Jos jonkin käynnistetään uudelleen. 

## <a name="next-steps"></a>Seuraavat vaiheet ##

Noudata seuraavia linkkejä Linux palvelun sovelluksen käytön aloittaminen. Lähettää kysymyksiä ja ongelmia Ota [Microsoftin keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Web Apps-sovellusten luominen Linux sovelluksen-palvelu](./app-service-linux-how-to-create-a-web-app.md)
* [Node.js verkkosovelluksissa Linux PM2 määritysten käyttäminen](./app-service-linux-using-nodejs-pm2.md)

