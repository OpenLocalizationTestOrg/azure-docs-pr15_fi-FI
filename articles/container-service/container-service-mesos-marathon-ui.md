<properties
   pageTitle="Azure säilö palvelun säilö hallinnan web-Käyttöliittymää | Microsoft Azure"
   description="Asentavat säilöjen Azure säilö palvelun klusterin palveluihin Marathon web-Käyttöliittymä."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Säilön hallinnan web-Käyttöliittymä

Toimialueen Ohjauskoneen/OS on ympäristö käyttöönotto ja skaalausta liitetty toiminnoista, kun abstracting pohjana olevan laitteen. Toimialueen Ohjauskoneen/OS päälle on kehys, joka hallitsee ajoitus ja Laske työmääriä suorittamista.

Vaikka puitteissa ovat käytettävissä monet Suositut toiminnoista, tässä asiakirjassa kerrotaan siitä, miten voit luoda ja skaalata säilön käyttöönotto uudella Marathon. Ennen kuin käytät näitä esimerkkejä kautta, sinun on Ohjauskoneen/OS klusteriin, joka on määritetty Azure säilö-palvelussa. Tarvitset myös etäyhteyksien tähän klusteriin. Lisätietoja näistä kohteista on seuraavissa artikkeleissa:

- [Azure säilö Service-klusterin käyttöönotto](container-service-deployment.md)
- [Yhteyden muodostaminen Azure säilö Service-klusterin](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Tutustu Ohjauskoneen/OS Käyttöliittymä

Suojattu runko (SSH) tunnelin muodostettu, ja siirry http://localhost/. Lataa Ohjauskoneen/OS web-Käyttöliittymä, ja näyttää tietoja klusterin, kuten käytetään resursseja, aktiivinen tekijöiden ja palveluista.

![TOIMIALUEEN OHJAUSKONEEN/OS KÄYTTÖLIITTYMÄ](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Tutustu Marathon Käyttöliittymä

Jos haluat nähdä Marathon-Käyttöliittymä, siirry http://localhost/Marathon. Tässä näytössä voit aloittaa uuden säilön tai muuhun sovellukseen Azure säilö palvelun Ohjauskoneen/OS-klusterin. Näet myös tietoja säilöjä ja sovellukset.  

![Marathon Käyttöliittymä](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Docker muotoiltujen säilön käyttöönotto

Asentavat uuden säilön Marathon, valitse **Luo sovellus** -painike ja lisää lomakkeeseen seuraavat tiedot:

Kenttä           | Arvo
----------------|-----------
TUNNUS              | nginx
Kuva           | nginx
Verkon         | Silloitettu
Isäntä portti       | 80
Protokolla        | TCP

![Uuden sovelluksen Käyttöliittymän--Yleiset](media/dcos/dcos4.png)

![Uuden sovelluksen Käyttöliittymän--Docker säilö](media/dcos/dcos5.png)

![Uuden sovelluksen Käyttöliittymän--portit ja palvelun etsiminen](media/dcos/dcos6.png)

Jos haluat yhdistää nimipalvelimet säilö portin agentti porttiin, tarvitset JSON-tilaa. Voit tehdä Vaihda ohjatun uuden sovelluksen **JSON tilaan** käyttämällä sivulla. Kirjoita-kohdan vaihtoehdoista `portMappings` Sovellusmäärityksen-osassa. Tässä esimerkissä sitoo säilö portti 80 Ohjauskoneen/OS-agentti portti 80. Voit liikkua JSON tilasta ohjatun toiminnon, kun olet tehnyt tämän muutoksen.

```none
"hostPort": 80,
```

![Uuden sovelluksen Käyttöliittymän--portti 80 Esimerkki](media/dcos/dcos13.png)

Toimialueen Ohjauskoneen/OS klusterin on otettu käyttöön ja määrittää yksityisten ja julkisten käyttäjäagenttien. Klusterin voi käyttää sovelluksia Internetin kautta sinun on sovellusten julkisen edustajalle. Tee uuden sovelluksen ohjatun toiminnon **Valinnainen** -välilehti ja lisää **slave_public** **Hyväksynyt resurssien roolien**.

![Uuden sovelluksen Käyttöliittymän--julkisen edustajan määrittäminen](media/dcos/dcos14.png)

Takaisin käyttöön Marathon pääsivulta näet käyttöönoton tilan säilö.

![Marathon pääsivulta Käyttöliittymän--säilö Käyttöönottotila](media/dcos/dcos7.png)

Kun siirryt takaisin toimialueen Ohjauskoneen/OS web-Käyttöliittymä (http://localhost/), näet, että tehtävä (Tässä tapauksessa Docker muotoiltujen säilön) suoritetaan Ohjauskoneen/OS-klusterin.

![Toimialueen Ohjauskoneen/OS WWW-Käyttöliittymän--klusterin suoritettava tehtävä](media/dcos/dcos8.png)

Näet myös tehtävän käyttävä klusterisolmu.

![Toimialueen Ohjauskoneen/OS WWW-Käyttöliittymän--tehtävän Klusterisolmu](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Skaalaa oman säilöt

Voit käyttää Marathon-Käyttöliittymän skaalata säilön esiintymän määrää. Voit tehdä **Marathon** sille sivulle, valitse säilö, jonka haluat skaalata ja **Skaalaus** -painiketta. Kirjoita **Asteikko-sovelluksen** -valintaikkunassa säilö kerrat, jonka haluat ja valitse **Asteikko-sovellus**.

![Marathon Käyttöliittymän--asteikko-sovelluksen-valintaikkuna](media/dcos/dcos10.png)

Kun asteikko-toiminto on valmis, näyttöön tulee eri Ohjauskoneen/OS käyttäjäagenttien samaan tehtävään useita kertoja.

![Toimialueen Ohjauskoneen/OS web Käyttöliittymän Raporttinäkymät-ikkunan--tehtävän levitä käyttäjäagenttien välillä](media/dcos/dcos11.png)

![Toimialueen Ohjauskoneen/OS WWW-Käyttöliittymän--solmujen](media/dcos/dcos12.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Toimialueen Ohjauskoneen/OS ja Marathon Ohjelmointirajapinnan käyttäminen](container-service-mesos-marathon-rest.md)

Perinpohjaisesti käsittelevään artikkeliin kanssa Mesos Azure säilö-palvelusta

> [AZURE. VIDEON] azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]
