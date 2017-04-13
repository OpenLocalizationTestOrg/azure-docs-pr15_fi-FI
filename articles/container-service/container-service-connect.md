<properties
   pageTitle="Yhteyden muodostaminen Azure säilö Service-klusterin | Microsoft Azure"
   description="Muodosta yhteys Azure säilö Service-klusterin SSH tunnelin käyttämällä."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Ohjauskoneen/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Yhteyden muodostaminen Azure säilö Service-klusterin

Toimialueen Ohjauskoneen/OS ja Docker Swarm klustereiden, jotka on otettu Azure säilö palvelun Näytä muut päätepisteet. Nämä päätepisteet ei kuitenkaan Avaa muille. Jotta voit hallita näitä päätepisteet, sinun on luotava tunnelin suojattu runko (SSH). SSH jälkeen tunnelin on muodostettu, voit suorittaa komennot klusterin päätepisteet ja tarkastella oman järjestelmän klusterin Käyttöliittymän selaimen kautta. Tämän asiakirjan vaiheittaiset luomiseen SSH tunnelin Linux, OS x: ssä ja Windows.

>[AZURE.NOTE] Voit luoda klusterin hallintajärjestelmän SSH-istunto. Ei kuitenkaan Suosittelemme tämän. Parissa suoraan hallintajärjestelmän paljastaa riski tahattoman määritysmuutoksia.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Luo SSH tunnelin Linux tai OS X

Huomaa, jotka voit tehdä, kun luot SSH tunnelin Linux tai OS X ei etsi kuormituksen perustyylit julkinen DNS-nimi. Voit tehdä tämän Laajenna resurssiryhmän niin, että kullekin resurssille näytetään. Etsi ja valitse perusmuodon julkiseen IP-osoite. Tämä avaa sivu, joka sisältää tietoja julkiseen IP-osoite, joka sisältää DNS-nimen määrittäminen. Tallenna nimi myöhempää käyttöä varten. <br />


![Julkinen DNS-nimi](media/pubdns.png)

Avaa on nyt ja suorittamalla seuraavan komennon jossa:

**Portti** on päätepiste, jonka haluat näyttää portti. Tämä on Swarm, 2375. Käytä Ohjauskoneen/OS portti 80.  
**Käyttäjänimi** on käyttäjänimi, joka on annettu klusterin on otettu käyttöön.  
**DNSPREFIX** on DNS-etuliite, jonka ilmoitit, kun klusterin käyttöön.  
**Alue** on alue, jossa resurssiryhmän sijaitsee.  
**PATH_TO_PRIVATE_KEY** [Valinnainen] on yksityinen avain, joka vastaa antamasi luodessasi säilö Service-klusterin julkisella avaimella polku. Käytä tätä valitsinta -i merkki.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH yhteyden portti on 2200--ei tavallinen portti 22.

## <a name="dcos-tunnel"></a>Toimialueen Ohjauskoneen/OS tunnelin

Avaa tunnelin Ohjauskoneen/OS-ryhmän päätepisteet, suorita komento, joka on seuraavankaltaiselta:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Voit nyt käyttää Ohjauskoneen/OS-ryhmän päätepisteet osoitteessa:

- TOIMIALUEEN OHJAUSKONEEN/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Vastaavasti pääset rest API kunkin sovelluksen tämän tunnelin kautta.

## <a name="swarm-tunnel"></a>Swarm tunnelin

Avaa Swarm päätepisteen tunnelin, suorita komento, joka näyttää seuraavankaltaiselta:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Nyt voit määrittää DOCKER_HOST ympäristömuuttuja seuraavasti. Voit edelleen käyttää Docker komentorivivalitsimet interface (CLI) normaalisti.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Luo SSH tunnelin Windows

On useita vaihtoehtoja SSH tunneleita luomisesta Windows. Tässä asiakirjassa kerrotaan käyttämisestä painovärit, muste toiminto.

Lataa Windows-järjestelmän painovärit, muste ja suorita sovellus.

Kirjoita isäntänimi, joka koostuu klusterin järjestelmänvalvojan käyttäjänimeä ja klusterin ensimmäistä perustyyliä julkinen DNS-nimi. **Isäntänimi** näyttää tältä: `adminuser@PublicDNS`. Kirjoita 2200 **portin**.

![Painovärit, muste määritysten 1](media/putty1.png)

Valitse **SSH** ja **todennusta**. Lisää yksityisen avaimen tiedosto todennusta varten.

![Painovärit, muste määritys 2](media/putty2.png)

Valitse **tunneleita** ja määritä välittää portit seuraavasti:
- **Lähdeportti:** Haluamallasi--Ohjauskoneen/OS 80 käyttäminen tai 2375 Swarm.
- **Kohde:** Käyttää localhost:80 Ohjauskoneen/OS tai localhost:2375 Swarm.

Seuraavassa esimerkissä on määritetty Ohjauskoneen/OS, mutta näyttää samalla Docker Swarm varten.

>[AZURE.NOTE] Portti 80 ei saa olla käytössä, kun luot tätä tunnelin.

![Painovärit, muste määritysten 3](media/putty3.png)

Kun olet valmis, Tallenna yhteysmäärityksen ja Yhdistä painovärit, muste istunto. Kun muodostat yhteyden, näet painovärit, muste tapahtumalokiin portin-määrityksiä.

![Painovärit, muste tapahtumaloki](media/putty4.png)

Kun olet määrittänyt tunnelin Ohjauskoneen/OS varten, voit käyttää liittyvät päätepiste kohteessa:

- TOIMIALUEEN OHJAUSKONEEN/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Kun olet määrittänyt tunnelin Docker Swarm varten, voit käyttää Swarm-klusterin Docker CLI kautta. Sinun on ensin määrittäminen Windows-ympäristömuuttuja nimeltä `DOCKER_HOST` arvot ` :2375`.

## <a name="next-steps"></a>Seuraavat vaiheet

Ottaa käyttöön ja hallita säilöjen Ohjauskoneen/OS tai Swarm:

- [Azure säilö palvelun ja Ohjauskoneen/Käyttöjärjestelmä](container-service-mesos-marathon-rest.md)
- [Azure säilö palvelu ja Docker Swarm](container-service-docker-swarm.md)
