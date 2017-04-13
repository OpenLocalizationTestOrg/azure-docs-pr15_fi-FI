<properties
   pageTitle="Valvoa Azure säilö Service-klusterin kanssa Datadog | Microsoft Azure"
   description="Seurata Azure säilö Service-klusterin Datadog kanssa. Toimialueen Ohjauskoneen/OS web-Käyttöliittymä avulla voit ottaa yhteyttä klusterin Datadog agenttien vuoksi."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Säilöjen Ohjauskoneen/OS, Docker Swarm Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Näytön Azure säilö Service-klusterin Datadog kanssa

Tässä artikkelissa on otetaan käyttöön Datadog agenttien vuoksi kaikki agentti Azure säilö Service-klusterin solmut. Sinun on ensin tili Datadog määritysten. 

## <a name="prerequisites"></a>Edellytykset 

[Ota käyttöön](container-service-deployment.md) ja määritetty Azure säilö palvelun klusteri [yhteyden](container-service-connect.md) . Tutustu [Marathon Käyttöliittymän](container-service-mesos-marathon-ui.md). Siirry [http://datadoghq.com](http://datadoghq.com) Datadog-tilin määrittäminen. 

## <a name="datadog"></a>Datadog 

Datadog on seuranta-palvelu, joka kokoaa seurantatiedot oman säilöjen Azure säilö Service-klusterin kuluessa. Datadog on Docker integrointi Raporttinäkymät-ikkunan kohtaa näet tietyn arvot oman säilöjen kuluessa. Oman säilöjen kerättyjen arvot on järjestetty suorittimen ja muistin, verkon i/o. Datadog jakautuu arvot säilöjä ja kuvia. Alla on esimerkki siitä, miltä Käyttöliittymä näyttää suorittimen käytön.

![Datadog Käyttöliittymä](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Määritä Marathon Datadog käyttöönotto

Näitä ohjeita noudattamalla määrittämisestä ja Datadog sovellusten yhteyttä klusterin Marathon kanssa. 

Käyttää Ohjauskoneen/OS-Käyttöliittymän välityksellä [http://localhost:80 /](http://localhost:80/). Kerran Ohjauskoneen/OS-Käyttöliittymän Siirry "-Universe" vasemmassa alareunassa eli ja Hae "Datadog" ja valitse "Asenna".

![Toimialueen Ohjauskoneen/OS Universe kuluessa paketin Datadog](./media/container-service-monitoring/datadog1.png)

Nyt Viimeistele määritys tarvitset Datadog- tai maksuttoman kokeiluversion tili. Kun olet kirjautunut sisään Datadog sivuston ulkoasun vasemmalle ja siirry integroinnit -> Valitse API. 

![Datadog-Ohjelmointirajapinnan avain](./media/container-service-monitoring/datadog2.png)

Kirjoita seuraavaksi Ohjelmointirajapinnan avain sisällä Ohjauskoneen/OS Universe Datadog-määritys. 

![Toimialueen Ohjauskoneen/OS universe Datadog määritys](./media/container-service-monitoring/datadog3.png) 

Yllä määrityksessä esiintymät määritetään 10000000 niin aina, kun uusi solmu lisätään klusterin Datadog otetaan automaattisesti käyttöön agentti solmun. Tämä on väliaikainen ratkaisu. Kun olet asentanut paketin pitäisi palata Datadog-sivustoon ja Etsi "Koontinäytöt." Sieltä näet mukautettu ja integrointi raporttinäkymiä. Docker integrointi Raporttinäkymät-ikkunan on kaikki yhteyttä klusterin seurantaa varten tarvitset säilö arvot. 
