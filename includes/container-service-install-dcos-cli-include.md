<properties
   pageTitle="Asenna Ohjauskoneen/Käyttöjärjestelmä CLI | Microsoft Azure"
   description="Asenna Ohjauskoneen/Käyttöjärjestelmä CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Säilöjen, mikro-palveluja, Ohjauskoneen/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Tämä on Ohjauskoneen/OS-pohjainen ACS klustereiden käsittelyyn. Ei ole tarpeen toiminto Swarm perustuva ACS klustereiden.

Ensin [Ohjauskoneen/OS-pohjainen ACS klusteri yhdistäminen](../articles/container-service/container-service-connect.md). Kun olet tehnyt tämän, voit asentaa Ohjauskoneen/OS CLI asiakaskoneeseen alla olevia komentoja:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Jos käytät Python vanhaa versiota, saatat huomata joitakin "InsecurePlatformWarnings". Voit turvallisesti ohittaa ne.

Aloita käynnistämättä oman käyttöliittymän, jotta voit suorittaa:

```bash
source ~/.bashrc
```

Tämä vaihe ei ole tarpeen, kun aloitat uuden liittymät.

Nyt voit vahvistaa, että CLI on asennettu:

```bash
dcos --help
```