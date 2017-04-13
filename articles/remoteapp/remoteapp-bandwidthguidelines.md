<properties 
    pageTitle="Azure RemoteApp-kaistanleveys - yleisiä ohjeita | Microsoft Azure"
    description="Tietoja joitakin perusverkko kaistanleveyden ohjeita Azure RemoteApp sivustokokoelmat ja sovellukset."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp-kaistanleveys - yleisiä ohjeita (Jos et voi testata oman)

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Jos sinulla ei ole aika tai [verkon kaistanleveyden testien](remoteapp-bandwidthtests.md) suorittaminen Azure RemoteApp ominaisuuksien, seuraavassa on joitakin melko yleinen ohjeita, joiden avulla voit arvioida kaistanleveys käyttäjää kohden.

Jos sinulla on mix tilanteisiin, ei ole suositeltavaa mitään pienempi kuin (tai yhtä suuri kuin) 10 Mt/s kuin pienin kaistanleveys Moderni Internet-yhteys-sovellusten remote-ympäristössä. (Vaikka, kuten edellä, näin takaa paremmin kuin keskimääräinen käyttäjäkokemuksen.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Monimutkainen PowerPointissa yksinkertaisen PowerPoint

Azure RemoteApp toimii parhaiten Lähiverkon 100 Megatavua. 10 Mt/s profiilin verkko-synkronointivirheiden 120 ms yläpuolella on enemmän kuin 5 %, kun käyttäjä näe keskimääräinen kokemus. 1 Mt/s eri on glaring – esimerkiksi diaesityksen käyttäjän eivät ehkä näy animoitu siirtymät lainkaan koska kehysten ohitetaan.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorerissa sekoitus PDF-, PDF-, teksti

10 Mt/s Verkkoprofiilin on lähellä Lähiverkon useimmat ominaisuuksia. 1 Mt/s päivitystavasta OK sisäänrakennetun, vaikka ehkä joitakin synkronointivirheiden kun käyttäjä vierittää samalla, kun näytössä ei kuvia.

## <a name="flash-video-youtube"></a>Flash-video (YouTube)

100 Mt/s Lähiverkon on parhaiten, kun 10 Mt/s on hyväksyttävä (merkitys Microsoft kehyksen korko pysy, mutta synkronointivirheiden kasvaa). 1 Mt/s-synkronointivirheiden on erittäin suuri ja huomattavia.

## <a name="word-typing-word-remote-input"></a>Wordin kirjoittaminen (Word remote syöttö)
Tämä on pieni kaistanleveyden käytön skenaario. 256 kt/s annamme yhtä hyvä kokemusta Lähiverkon nimellä.

## <a name="learn-more"></a>Opi lisää
- [Azure RemoteApp verkon kaistanleveyden käytön arviointi](remoteapp-bandwidth.md)

- [Azure RemoteApp - miten kaistanleveys ja laatu ilmene työn samassa paikassa?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting verkon kaistanleveyden käytön joitakin yleisiä tilanteita, joissa kanssa](remoteapp-bandwidthtests.md)