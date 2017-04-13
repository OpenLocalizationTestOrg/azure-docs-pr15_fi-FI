<properties
    pageTitle="Aloita välitys Hybrid yhteydet | Microsoft Azure"
    description="Viestin kirjoittaminen Hybrid yhteyksien solmu console-sovellus"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Välitys Hybrid yhteydet käytön aloittaminen

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Mitä tehdä

Koska Hybrid yhteydet vaatii asiakkaan ja palvelimen osa, tässä opetusohjelmassa luodaan kaksi console-sovellusta. Tee näin:

1. Luo välitys nimitilan Azure-portaalissa.

2. Luo Hybrid yhteys Azure-portaalissa.

3. Kirjoita palvelimen console-sovellus vastaanottaa viestejä.

4. Kirjoita asiakkaan console-sovellus, lähettää viestejä.

## <a name="prerequisites"></a>Edellytykset

1. [Node.js](https://nodejs.org/en/) (Tässä esimerkissä käytetään solmu 7.0).

2. Azure tilaus.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. luominen Azure-portaalissa nimitila

Jos sinulla on jo luotu välitys nimitilan, siirry [Hybrid yhteyden luominen käyttämällä Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) -osassa.

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure-portaalissa Hybrid yhteyden luominen

Jos sinulla on jo luotu Hybrid yhteyden, siirry [palvelin-sovelluksen](#3-create-a-server-application-listener) luominen.

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. server (listener)-sovelluksen luominen

Jos haluat kuunnella ja vastaanottaa viestejä välityksen, emme kirjoittaa Node.js console-sovelluksen.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. luominen asiakassovellus (lähettäjä)

Voit lähettää viestejä välityksen, emme kirjoittaa Node.js console-sovellus.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Suorita sovellukset

1. Palvelinsovelluksen käyttämiseen.

2. Suorita asiakassovellus ja kirjoita tekstiä.

3. Varmista, että palvelimen sovelluksen konsolin tulostaa asiakassovelluksessa kirjoitetun tekstin.

![suorittaminen-sovellukset](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Onnittelut, olet luonut sovelluksen Hybrid yhteydet lopusta loppuun.

## <a name="next-steps"></a>Seuraavat vaiheet:

- [Välitys usein kysytyt kysymykset](relay-faq.md)
- [Luo nimitila](relay-create-namespace-portal.md)
- [.NET käytön aloittaminen](relay-hybrid-connections-dotnet-get-started.md)
- [Solmun käytön aloittaminen](relay-hybrid-connections-node-get-started.md)