<properties 
    pageTitle="Hakemuksen tiedot sovelluksen karttaan | Microsoft Azure" 
    description="Sovelluksen osia väliset riippuvuudet visuaalisen selityksen, jossa on KPI: T ja ilmoitukset." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Sovelluksen kartta-sovelluksen tiedot

[Visual Studio sovelluksen tiedot](app-insights-overview.md)-sovelluksen-määritys on ulkoasun riippuvuussuhteet sovelluksen osia. Kukin osa näyttää KPI: T kuten kuormituksen, suorituskyvyn, virheet ja ilmoitukset-auttaa löytämään osan suorituskykyyn liittyvä ongelma tai virhe. Voit valita kautta mitä tahansa osasta yksityiskohtaisempia diagnostiikka-molempiin sovelluksen tiedot - ja jos sovellus käyttää Azure-palvelut - Azure diagnostiikan asetuksia, kuten SQL-tietokannan Advisor suositukset.

Muut kaaviot, kuten voit kiinnittää sovelluksen-kartan Azure koontinäyttö, jossa on täysin. 

## <a name="open-the-application-map"></a>Avaa sovellus kartta

Avaa kartan sovelluksen yhteenveto-sivu:

![Avaa app kartta](./media/app-insights-app-map/01.png)

![sovelluksen kartta](./media/app-insights-app-map/02.png)

Kartan näkyy:

* Käytettävyys testit
* Asiakkaan puoli osan (seurattava JavaScript-SDK)
* Reunan osa
* Asiakkaan ja palvelimen osat riippuvuudet

Voit laajentaa ja tiivistää riippuvuuden linkki ryhmät:

![Tiivistä käyttäjäruutu-painike](./media/app-insights-app-map/03.png)
 
Jos sinulla on runsaasti riippuvuudet yhdessä tyyppi (SQL, HTTP jne.), ne saattavat näkyä ryhmitelty. 


![ryhmitelty riippuvuudet](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Ongelmia

Kukin solmu on asiaa suorituskykyilmaisimia, kuten osan kuormituksen, suorituskykyä ja virheen korvaukset. 

Varoitus kuvakkeet Korosta mahdolliset ongelmat. Oranssi varoitus tarkoittaa, että virheiden pyyntöjä riippuvuuden puhelujen tai sivun näkymät. Punainen tarkoittaa virheen korvaus yli 5 %.


![Virhe kuvakkeet](./media/app-insights-app-map/04.png)

 
Aktiivinen ilmoitusten myös Näytä määrittäminen: 


![aktiivinen ilmoitukset](./media/app-insights-app-map/05.png)
 
Jos käytät SQL Azure-kuvake, joka ilmoittaa, milloin on on suosituksia, miten voit parantaa suorituskykyä. 


![Azure suositus](./media/app-insights-app-map/06.png)

Napsauta mitä tahansa tarkempia kuvaketta:


![Azure suositus](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnostiikan napsauttamalla kautta

Kunkin kartassa solmut on kohdistettu napsauttamalla kautta Diagnostiikka. Vaihtoehdot määräytyvät solmun tyypin mukaan.

![palvelimen asetukset](./media/app-insights-app-map/09.png)

 
Osien, joita isännöidään Azure-asetuksia ovat suoraan linkkejä.


## <a name="filters-and-time-range"></a>Suodattimet ja aika-alue

Oletusarvon mukaan kartan on yhteenveto kaikki tiedot, jotka ovat käytettävissä valittu aikaväli. Mutta voit suodattaa sitä sisältämään vain tietyn toiminnon nimiä tai riippuvuudet.

* Toiminnon nimi: Tämä vaihtoehto sisältää sivun näkymiä ja puoli pyynnön palvelintyypit. Tällä asetuksella kartasta näkyy KPI: N server-asiakas puoli solmu vain valittuihin toimintoihin. Se näyttää riippuvuudet, valitse kyseiset tiettyjen toimintojen yhteydessä.
* Riippuvuus kannan nimi: Tämä vaihtoehto sisältää AJAX selaimen Palvelinpuolen riippuvuuksia ja palvelinpuolen riippuvuuksia. Jos ilmoitat Mukautettu riippuvuus telemetriatietojen TrackDependency Ohjelmointirajapinnan kanssa, ne näkyvät myös tässä. Voit valita riippuvuudet, jos haluat näyttää kartassa. Huomaa, että tällä hetkellä tämän ei Suodata palvelimen puoli ominaisuuksia tai asiakkaan puolen sivun näkymät.


![Määritä suodattimet](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Tallenna suodattimet

Tallentamiseen käyttämäsi suodattimet Kiinnitä suodatettua näkymää [raporttinäkymät-ikkunan](app-insights-dashboards.md)päälle.


![Raporttinäkymät-ikkunan kiinnittäminen](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Palaute

Ota [palautteen portaalin palaute-vaihtoehdon avulla](app-insights-get-dev-support.md).


![MapLink-1-kuva](./media/app-insights-app-map/13.png)


