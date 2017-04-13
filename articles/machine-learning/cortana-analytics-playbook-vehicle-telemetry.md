<properties 
    pageTitle="Ajoneuvon telemetriatietojen analytics ratkaisu playbook | Microsoft Azure" 
    description="Cortana yritystieto-ominaisuuksien käyttäminen saada reaaliaikaisia ja ennakoivan oivallusten luomiseen ajoneuvon terveys- ja ajo-käyttöä." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Ajoneuvon telemetriatietojen analytics ratkaisu playbook

Tässä **valikossa** linkkejä tämän playbook luvut. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Yleiskatsaus
Erittäin tietokoneissa on siirretty pois testiympäristössä ja ovat nyt säilytyksessä Microsoftin autotallin! Nämä kehittyneet autot sisältää runsaasti anturit, hänelle voivat seurata ja seurata tapahtumien miljoonia sekunnin välein. Odotamme, että mukaan 2020, useimmat nämä autoista on on yhteydessä Internetiin. Kuvitellaan napauttamalla tätä runsaasti antamaan parhaiten luokan turvallisuus, luotettavuutta ja ajo kokemusta tietojen tuominen! Microsoft on tehnyt tämän unta reality Cortana liiketoimintatietojen kanssa.

Microsoftin Cortana liiketoimintatietojen on täysin hallitun big datasta ja kehittyneen analyysin suite, jonka avulla voit muuntaa tietoja älykkäät toiminto. Haluat esitellään Cortana liiketoimintatietojen ajoneuvon Telemetriatietojen Analytics-ratkaisun-malliin. Tämä ratkaisu esitellään, miten auton dealerships, autojen valmistajat ja vakuutusyhtiöiden voi käyttää Cortana liiketoimintatietojen ominaisuuksia saada reaaliaikaisia ja ennakoivan havainnollistamisen ajoneuvon terveys- ja ajo-käyttöä. 

Ratkaisu on toteutettu [lambda arkkitehtuuri kuvion](https://en.wikipedia.org/wiki/Lambda_architecture) täydellinen näkyy mahdollisten Cortanan Liiketoimintatieto-ympäristö, reaaliaikainen ja eräkäsittely. Ratkaisu: 

- sisältää ajoneuvon telemaattiset simulator
- Tapahtuman keskittimet hyödyntää varten ingesting Simuloitu ajoneuvon telemetriatietojen tapahtumien miljoonia Azure tuominen 
- Hanki reaaliaikaisia tietoja ajoneuvon terveyteen Stream Analytics avulla
-  jatkuu tiedot pitkään varastoon tehokkaan erä analysoinnissa. 
- hyödyntää koneen Learning poikkeavuuksista tunnistaminen reaaliaikainen ja erän käsittelyn ja myönnä ennakoivan tiedot.
- hyödyntää HDInsight muuntamiseen tietoja asteikko ja Data Factory käsittelemään tiedonsiirron, järjestäminen, Resurssienhallinta ja eräkäsittely putkijohto seuranta 
- Tämä ratkaisu monipuolisia Raporttinäkymät-ikkunan antaa reaaliaikaiset tiedot ja ennakoivan analytics-visualisointeja käyttämällä Power BI

## <a name="architecture"></a>Arkkitehtuuri

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Kuva 1 – ajoneuvon Telemetriatietojen Analytics ratkaisu-arkkitehtuuri*

Tämä ratkaisu sisältää seuraavat **Cortana Liiketoimintatieto-osia** ja esitellään niiden lopusta loppuun-integrointi


- **Tapahtuman keskittimet** ingesting ajoneuvon telemetriatietojen tapahtumien miljoonia Azure kyselyjä.
- **Virta Analytics** kasvun ajoneuvon terveyteen reaaliaikaiset tiedot ja tietoja jatkuu pitkään varastoon tehokkaan erä analysoinnissa.
- **Koneen Learning** poikkeavuuksista tunnistaminen reaaliaikaisesti ja eräkäsittely ja myönnä ennakoivan tiedot.
- **Hdinsightista** on hyödyntää tasolla tietojen muuntamiseen
- **Data Factory** käsittelee tiedonsiirron, aikataulutus, Resurssienhallinta ja eräkäsittely putkijohto seuranta.
- **Power BI** antaa tämän ratkaisun monipuolisia Raporttinäkymät-ikkunan reaaliaikaiset tiedot ja ennakoivan analytics visualisointeja.

Tämä ratkaisu käyttää kahta eri **tietolähteisiin**: 

- **Simuloitu ajoneuvon signaalit ja diagnostiikka**: ajoneuvon telemaattiset-simulator tietokoneesta kuuluu äänimerkki vianmääritystiedot ja signaalit, jotka vastaavat tilaan ajoneuvon ja ajo kuvio tiettynä ajankohtana. 
- **Ajoneuvon luettelon**: viittaus tietojoukon sisältävä VIN mallin määritys.
