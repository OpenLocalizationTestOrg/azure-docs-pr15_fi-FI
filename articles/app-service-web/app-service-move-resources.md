<properties
    pageTitle="Siirry toiseen resurssiryhmä Web App-resurssit"
    description="Tässä artikkelissa kuvataan tilanteita, joissa voit siirtää Web-sovellusten ja palveluiden sovelluksen resurssi-ryhmästä toiseen."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Tuetut Siirrä käyttömahdollisuudet

Voit siirtää [ARM siirtää resurssit-ohjelmointirajapinnan](../resource-group-move-resources.md)käyttäminen Azure Web App-resursseja.

Azure online tukee tällä hetkellä Siirrä seuraavissa tilanteissa:

* Resurssiryhmä (web Apps-sovelluksista, sovelluksen palvelusopimusten vaihtoehdot ja varmenteet) koko sisällön siirtäminen toiseen resurssiryhmä 
    * Huomautus: Kohde resurssiryhmä ei voi sisältää tässä skenaariossa Microsoft.Web resursseja
* Yksittäisten web Apps-sovellusten siirtäminen eri resurssiryhmä samalla, kun ne isännöinti edelleen niiden nykyinen sovellus-palvelusopimus (sovelluksen palvelusopimus pysyy Vanha resurssiryhmän)
