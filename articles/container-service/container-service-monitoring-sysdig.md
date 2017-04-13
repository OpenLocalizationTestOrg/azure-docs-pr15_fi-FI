<properties
   pageTitle="Valvoa Azure säilö Service-klusterin kanssa Sysdig | Microsoft Azure"
   description="Seurata Azure säilö Service-klusterin Sysdig kanssa."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Säilöjen Ohjauskoneen/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Näytön Azure säilö Service-klusterin Sysdig kanssa

Tässä artikkelissa on otetaan käyttöön Sysdig käyttäjäagenttien kaikki agentti Azure säilö Service-klusterin solmut. Tarvitset tilillä Sysdig määritysten. 

## <a name="prerequisites"></a>Edellytykset 

[Ota käyttöön](container-service-deployment.md) ja määritetty Azure säilö palvelun klusteri [yhteyden](container-service-connect.md) . Tutustu [Marathon Käyttöliittymän](container-service-mesos-marathon-ui.md). Siirry [http://app.sysdigcloud.com](http://app.sysdigcloud.com) Sysdig cloud-tilin määrittäminen. 

## <a name="sysdig"></a>Sysdig

Sysdig on seuranta-palvelu, jonka avulla voit valvoa oman säilöjen yhteyttä klusterin kuluessa. Sysdig tiedetään apua vianmäärityksessä, mutta siinä on oman basic seurannan arvot myös suorittimen, verkko, muistin ja i/o. Sysdig on helppo nähdä säilöt käsittelevät hardest tai olennaisesti eniten muistin ja suorittimen avulla. Tämä näkymä on tällä hetkellä beta "Yhteenveto"-osassa. 

![Sysdig Käyttöliittymä](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Määritä Marathon Sysdig käyttöönotto

Näitä ohjeita noudattamalla määrittämisestä ja Sysdig sovellusten yhteyttä klusterin Marathon kanssa. 

Käyttää Ohjauskoneen/OS-Käyttöliittymän välityksellä [http://localhost:80 /](http://localhost:80/) kerran Ohjauskoneen/OS-Käyttöliittymän Siirry "Universe", joka on vasemmassa alareunassa ja Hae "Sysdig."

![Toimialueen Ohjauskoneen/OS universe Sysdig](./media/container-service-monitoring-sysdig/sysdig1.png)

Viimeistele määritys tarvitset nyt Sysdig cloud-tili tai maksuton kokeiluversio-tili. Kun olet kirjautunut sisään Sysdig cloud-sivustoon, valitse käyttäjänimi ja -sivulla näkyy "Käyttää avainta." 

![Sysdig-Ohjelmointirajapinnan avain](./media/container-service-monitoring-sysdig/sysdig2.png) 

Kirjoita seuraava sisällä Ohjauskoneen/OS Universe Sysdig-määritys Access-näppäintä. 

![Toimialueen Ohjauskoneen/OS universe Sysdig määritys](./media/container-service-monitoring-sysdig/sysdig3.png)

Nyt määrittäminen esiintymät 10000000 aina, kun uusi solmu lisätään klusterin Sysdig pystyvät automaattisesti käyttöön agentti uuden solmun. Tämä on väliaikainen ratkaisu varmistaaksesi, että Sysdig otetaan käyttöön kaikkia uusia käyttäjäagenttien klusterin kuluessa. 

![Toimialueen Ohjauskoneen, OS Universe-esiintymät Sysdig määrittäminen](./media/container-service-monitoring-sysdig/sysdig4.png)

Kun olet asentanut paketin siirtyä takaisin Sysdig Käyttöliittymä ja pääset aloittamaan voit tarkastella säilöjen yhteyttä klusterin sisällä eri käyttö mittaukset. 