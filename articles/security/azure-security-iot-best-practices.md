<properties
   pageTitle="Asioita suojauksen parhaiden käytäntöjen Internet | Microsoft Azure"
   description="Tässä artikkelissa Microsoft Internet asioita suojauksen parhaiden käytäntöjen ja Yleiset suositusten curated luettelo."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Asioita suojauksen parhaiden käytäntöjen Internet

Suojaaminen asioita Internet (IoT) infrastruktuuri on kriittinen yrityksen kaikille liittyvistä IoT ratkaisuja. Laitteiden yhdistävää määrää ja seuraaviin laitteisiin hajautettu laatu-suojauksen tapahtuman liittyvät havaittu miljoonia IoT laitteita, vaikutus on ei trivial ja voi olla juuri vaikutus.

Tästä syystä IoT suojaus on suojauksen syvyys-vaihtoehto. Tietojen on oltava suojatun pilveen ja siirtyy yksityisten ja julkisten verkoissa. Menetelmien on oltava paikallaan valmistelu suojatusti IoT laitteet itse. Kunkin kerroksen, millä laitteella, verkkoon cloud taustatietokannan on vahva suojaus vahvistukset.

IoT parhaita käytäntöjä voidaan luokitella seuraavalla tavalla:

- IoT laitteen valmistajan tai integraattori
- IoT ratkaisun kehittäjä
- IoT ratkaisu deployer
- IoT ratkaisu operaattori

Tässä artikkelissa on yhteenveto [Internet, asioita suojauksen parhaista käytännöistä](../iot-suite/iot-security-best-practices.md). Katso lisätietoja artikkelin.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IoT laitteen valmistajan tai integraattori

Noudata alla parhaita käytäntöjä, jos olet IoT-laitteen valmistus tai laitteiston integraattori:

- **Laajuus laitteiston vähimmäisvaatimukset**: laitteiston rakenne projektityypin pienin ominaisuuksia, joita tarvitaan toiminnan laitteiston ja mitään Lisää. 
- **Tee laitteiston pystyisivät kuitti**: Muodosta järjestelmiä esiintyvien fyysinen luvattoman muokkaamisen laitteita, kuten avata laite-kansi poistaminen osa laitteen jne. 
- **Luo suojatun laitteiston ympärille**: Jos [myytyjen tuotteiden kustannusten](https://en.wikipedia.org/wiki/Cost_of_goods_sold) sallivat luominen suojausominaisuuksia, kuten suojattua ja salattua tallennustilan ja luotetut Platform Module TPM pohjaisissa toimintoja.
- **Tee päivitykset suojatun**: päivittäminen laitteisto laitteen elinkaaren aikana on väistämätöntä.

## <a name="iot-solution-developer"></a>IoT ratkaisun kehittäjä

Noudata alla parhaita käytäntöjä, jos olet IoT-ratkaisun kehittäjä:

- **Seuraa suojatun software development kehitysmenetelmistä**: suojatun ohjelmiston kehittäminen edellyttää maasta ylöspäin Pohdi suojauksen projektin alusta aivan sen käyttöönoton, testaus ja käyttöönottoa.
- **Valitse Avaa lähde-ohjelmisto, varoen**: Avaa lähde-ohjelmiston tarjoaa mahdollisuuden kehittää nopeasti ratkaisuja.
- **Tarkkaan integroida**: monia ohjelmiston suojaus-virheet olemassa kirjastoihin ja ohjelmointirajapinnan reunaa. 

## <a name="iot-solution-deployer"></a>IoT ratkaisu deployer

Noudata alla olevia parhaita käytäntöjä, jos olet IoT-ratkaisun deployer:

- **Laitteiston käyttöönotto suojatusti**: IoT ominaisuuksissa saattaa edellyttää laitteiston suojaamaton sijainneissa, kuten julkisten välilyöntejä tai varoitukseksi kielet käyttöön.
- **Lukemaan todennus näppäimet**: käyttöönoton aikana kunkin laitteen edellyttää, että laitteen tunnukset ja siihen todennus näppäimet luoma pilvipalvelussa. Lukemaan painettavat näppäimet fyysisesti jopa käyttöönoton jälkeen. Mikä tahansa selvittämäänsä avainta voi käyttää haittaohjelmien laitteen naamioitua aiemmin luotu tiedosto.

## <a name="iot-solution-operator"></a>IoT ratkaisu operaattori

Noudata alla parhaita käytäntöjä, jos olet IoT ratkaisu operaattori:

- **Järjestelmien pitäminen ajan tasalla**: varmistaa laitteen käyttöjärjestelmät ja kaikki laiteohjaimet päivitetään uusinta versiota. 
- **Suojaa vastaan haittaohjelmien toiminta**: Jos käyttöjärjestelmän sallimassa sijoittaa uusimman virustentorjunta- ja haittaohjelmien ominaisuuksia kunkin laitteen-käyttöjärjestelmässä. 
- **Usein valvonta**: valvonta IoT infrastruktuurin tietoturva ongelmista on avain vastattaessa suojauksen tapaukset.
- **Suojaa fyysisesti IoT infrastruktuuri**: huonoin suojauksen kalastelu, vastaan IoT infrastruktuurin käynnistää fyysinen Accessilla laitteisiin.
- **Suojaa cloud tunnistetiedot**: määrittäminen ja toimivien IoT käyttöönoton cloud todennus tunnistetiedot ovat mahdollisesti helpoin tapa päästä ja IoT järjestelmän. 
