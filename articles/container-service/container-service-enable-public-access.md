<properties
   pageTitle="Ota käyttöön yleisön ACS-sovellukseen | Microsoft Azure"
   description="Miten voit ottaa käyttöön julkinen access Azure säilö palveluihin."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Julkinen käyttöönottaminen Azure säilö Service-sovellukseen

Minkä tahansa toimialueen Ohjauskoneen/OS säilö ACS [julkisen agentti resurssivarantoon](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automaattisesti näkyvät Internet. Oletusarvon, portit **80**ja **443** **8080** avataan ja kuuntele portit tahansa (julkisten) säilöön ovat käytettävissä. Tässä artikkelissa kerrotaan Lisää porttien sovellusten avaaminen Azure säilö-palvelussa.

## <a name="open-a-port-portal"></a>Avaa portti (portaalin) 

Ensin annettava Avaa haluamme portti.

1. Kirjaudu sisään portaaliin.
2. Etsi resurssiryhmä joka Azure säilö palvelun käyttöön.
3. Valitse edustaja kuormituksen (joka on nimeltään samalla **XXXX-agentti-g-XXXX**).

    ![Azure säilö palvelun kuormituksen](media/container-service-dcos-agents/agent-load-balancer.png)

4. Valitse **Probes** ja sitten **Lisää**.

    ![Azure säilö palvelun kuormituksen tasauspalvelun keräysputkien](media/container-service-dcos-agents/add-probe.png)

5. Näytteenottimen-lomakkeen täyttäminen ja valitse **OK**.

  	| Kenttä | Kuvaus |
  	| ----- | ----------- |
  	| Nimi  | Näytteenottimen kuvaava nimi. |
  	| Port (portti)  | Voit testata säilö portti. |
  	| Polku  | (Kun HTTP-tilassa) Suhteellinen sivuston polku tutkia. HTTPS ei tueta. |
  	| Väli | Näytteenottimen välisen ajan yrittää sekunnin kuluttua. |
  	| Perusasemassa kynnysarvo | Ennen kuin harkitset säilö perusasemassa yrittää määrä peräkkäisiä näytteenottimen. | 
    

6. Takaisin agentti kuormituksen ominaisuudet, valitse **Lataa Vastatilin säännöt** ja valitse sitten **Lisää**.

    ![Azure säilö palvelun kuormituksen tasauspalvelun säännöt](media/container-service-dcos-agents/add-balancer-rule.png)

7. Kuormituksen tasauspalvelun-lomakkeen täyttäminen ja valitse **OK**.

  	| Kenttä | Kuvaus |
  	| ----- | ----------- |
  	| Nimi  | Kuormituksen kuvaava nimi. |
  	| Port (portti)  | Julkinen saapuvien portti. |
  	| Taustajärjestelmä portti | Sisäinen Julkinen portti reitittää liikenteen säilöstä. |
  	| Taustajärjestelmä resurssivarantoon | Kannan säilöt ovat tämän kuormituksen kohde. |
  	| Tutkia | Voit selvittää, onko kohde **Taustajärjestelmä resurssivarantoon** käytetyn näyteyksikön on kunnossa. |
  	| Istunnon pysyvyys | Määrittää, miten liikenne asiakaskoneesta käsitellään istunnon ajaksi.<br><br>**Ei mitään**: saman asiakkaan peräkkäiset pyynnöt voit käsitellä tahansa säilöön.<br>**Asiakkaan IP**: saman asiakkaan IP-peräkkäiset pyynnöt käsittelee samaa säilö.<br>**Asiakkaan IP- ja protokolla**: samaa asiakkaan IP- ja protokolla yhdistelmää peräkkäiset pyynnöt käsittelee samaa säilö. |
  	| Aikakatkaisun | (Vain TCP) Minuutteina aika, joka säilyttää TCP/HTTP-asiakas Avaa ilman, että *säilyttämisen* viestejä. |

## <a name="add-a-security-rule-portal"></a>Lisää suojauksen sääntö (portaalin)

Seuraavaksi annettava lisätä suojauksen säännön, joka reitittää liikenteen Microsoftin avattu porttiin palomuurin läpi.

1. Kirjaudu sisään-portaaliin.
2. Etsi resurssiryhmä joka Azure säilö palvelun käyttöön.
3. **Julkinen** agentti verkon suojauksen ryhmän valitseminen (joka on nimeltään samalla **XXXX-agentti-julkinen-nsg-XXXX**)

    ![Azure säilö palvelun verkko-käyttöoikeusryhmän](media/container-service-dcos-agents/agent-nsg.png)

4. Valitse **Saapuvan liikenteen säännöt** ja valitse sitten **Lisää**.

    ![Azure säilö palvelun verkon ryhmän suojaussäännöt](media/container-service-dcos-agents/add-firewall-rule.png)

5. Täytä palomuurisäännön Salli Julkinen portti ja valitse **OK**.

  	| Kenttä | Kuvaus |
  	| ----- | ----------- |
  	| Nimi  | Palomuurin säännön kuvaava nimi. |
  	| Priority (prioriteetti) | Prioriteetti luvun säännön. Mitä pienempi luku suuremman prioriteetti. |
  	| Lähde | Rajoita saapuvien IP-osoitealueita haluat sallia tai estää tämän säännön perusteella. Käyttää **mitään** Määritä rajoitus. |
  	| Palvelun | Valitse ennalta määritettyjä palvelujen suojauksen tämä sääntö koskee. Muussa tapauksessa luo oma **Mukautettu** avulla. |
  	| Protokolla | Rajoita liikenne **TCP** tai **UDP**. Käyttää **mitään** Määritä rajoitus. |
  	| Port ()-porttialue | Kun **palvelu** on **Mukautettu**, määrittää portit, tämä sääntö koskee. Voit käyttää yhden portin, kuten **80**tai alueen, kuten **1 024 1500**. |
  	| Toiminto | Salli tai estä liikenteestä, joka täyttää ehdot. |

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [julkisista ja yksityisistä Ohjauskoneen/OS käyttäjäagenttien](container-service-dcos-agents.md)välinen erotus.

Lue lisätietoja [toimialueen Ohjauskoneen/OS säilöjen hallinta](container-service-mesos-marathon-ui.md).