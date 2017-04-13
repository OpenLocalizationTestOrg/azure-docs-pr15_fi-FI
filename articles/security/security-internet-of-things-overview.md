<properties
   pageTitle="Yleistä asioita Internet-suojauksesta | Microsoft Azure"
   description=" Azure internet asioita (IoT) palvelujen tarjoavat laajan valikoiman ominaisuuksia. Tässä artikkelissa neuvotaan, kuinka voit suojata IoT ratkaisujen Azure-tietokannassa. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Yleistä asioita Internet-suojauksesta

Azure internet asioita (IoT) palvelujen tarjoavat laajan valikoiman ominaisuuksia. Yrityksen arvosana palveluista, joiden avulla voit:

- Tietojen kerääminen laitteilla
- Analysoi tietoja virtaa liikkeessä
- Tallentaa ja suurten tietojoukkojen kysely
- Reaaliaikainen- ja historiallisten tietojen visualisointi
- Integroi takaisin office-järjestelmiin

Pitää näitä ominaisuuksia, Azure IoT Suite pakettien yhdessä Azure palveluihin on mukautettu kuin esimääritetyt ratkaisut. Esimääritetyt ratkaisut ovat Yleiset IoT ratkaisu mallit, jotka voit vähentää ajan tallentumaan IoT ratkaisuja perusosoitteen käyttöotot. Käytä IoT software development Kit, voi mukauttaa ja laajentaa vastaamaan omia tarpeita seuraavia ratkaisuja. Voit käyttää myös seuraavia ratkaisuja esimerkkejä tai malleja kun kehittävät uusia IoT ratkaisuja.

Azure IoT suite on tehokas ratkaisu IoT tarpeisiin. On kuitenkin upmost tärkeyden IoT ratkaisujen on suunniteltu alusta mielessä suojausta. IoT laitteiden jo pelkkä viestien määrää, koska tahansa suojaus-tapahtumaa voi olla nopeasti juuri tapahtumaa, jonka merkittäviä vaikutukset.

Voi auttaa sinua ymmärtämään suojaamisesta IoT ratkaisujen on seuraavat tiedot.

## <a name="security-architecture"></a>Suojausarkkitehtuuri

Kun suunnittelet järjestelmää, on tärkeää ymmärtää mahdollisten uhkien järjestelmän ja lisää haluamasi suojaus näin ollen järjestelmä on suunniteltu ja arkkitehtuuri. On erittäin tärkeää suunnittelemaan alusta tuotteen mielessä suojausta, koska tietoja siitä, miten luvaton välttämättä voi saada järjestelmän avulla, varmista, että tarvittavat ongelman pienentämistavat ovat paikoillaan alusta.

Saat tietoja IoT suojausarkkitehtuuri lukemalla [Asioita suojauksen arkkitehtuuri Internet](../iot-suite/iot-security-architecture.md).

Tässä artikkelissa käsitellään seuraavia aiheita:

- [Suojauksen malli alkaa](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [IoT suojaus](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Uhkien mallinnus Azure IoT viittaus-arkkitehtuuri](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Suojaus on alusta alkaen

IoT aiheuttaa yksilöllinen tietoturva, tietosuoja ja vaatimustenmukaisuus haasteita yrityksille kaikkialla maailmassa. Toisin kuin missä ongelmaan työnkulkuihin ohjelmistojen ja miten se otetaan käyttöön perinteinen cyber teknologian IoT koskee, mitä tapahtuu, kun cyber ja fyysinen maailman todennäköisyyden. IoT ratkaisuja suojaaminen edellyttää varmistaa, että suojatun valmistelu eri laitteisiin, suojattu seuraaviin laitteisiin ja cloud ja suojattu tietojen suojaaminen pilveen aikana käsittely- ja väliset yhteydet. Toimi näiden toimintoja vastaan, on kuitenkin resurssin rajoitettu laitteiden ja ominaisuuksissa maantieteellinen jakauma laitteiden ratkaisussa suuri määrä.

Lue, miten käsittelemään alueilla suojaus lukemalla [asioita Internet suojaus on alusta alkaen](../iot-suite/securing-iot-ground-up.md).

Tässä artikkelissa käsitellään seuraavia aiheita:

- [Suojattu infrastruktuuri on alusta alkaen](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure – suojattu yrityksesi IoT infrastruktuuri](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Parhaat käytännöt

Tiukka suojauksen syvyys strategia IoT infrastruktuuri suojaaminen edellyttää. Lähtien pilvipalvelussa, voit suojaaminen tietojen eheys aikana julkisen Internetin kautta, joka tarjoaa mahdollisuuden valmistelu suojatusti laitteet-tietojen suojaaminen kerroksen muodostaa suurempi suojauksen assurance-yleistä infrastruktuuri.

Saat asioita Internet-suojauksen parhaista käytännöistä lukemalla [asioita Internet-suojauksen parhaiden käytäntöjen](../iot-suite/iot-security-best-practices.md).

Tässä artikkelissa käsitellään seuraavia aiheita:

- [IoT laitteen valmistajan/integraattori](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [IoT ratkaisun kehittäjä](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [IoT ratkaisu deployer](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [IoT ratkaisu operaattori](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
