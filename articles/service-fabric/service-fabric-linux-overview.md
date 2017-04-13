<properties
   pageTitle="Azure palvelun kangasta Linux | Microsoft Azure"
   description="Palvelun kangasta klustereiden tukevat Linux ja Java, mikä tarkoittaa voit voi ottaa käyttöön ja isännöidä palvelun kangasta sovelluksia kirjoitettu Linux Java- ja C#."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Palvelun kangasta Linux

Esikatselu-palvelun kangasta Linux mahdollistaa luominen, ottaminen käyttöön ja hallita helposti saatavilla, erittäin skaalattava sovelluksia Linux samalla tavalla kuin Windows. Palvelun kangasta kehysten (luotettavia palveluja ja luotettava toimijoiden) ovat käytettävissä Java Linux lisäksi C# (.NET Core).  Voit myös luoda [Vieras suoritettavien palveluiden](service-fabric-deploy-existing-app.md) kielen tai framework. Lisäksi esikatselu tukee myös orchestrating Docker säilöjen. Docker säilöjen suorittamisen Vieras suoritettavat tai alkuperäisen palvelun kangasta palvelujen, jotka käyttävät palvelun kangasta kehysten.

Palvelun kangasta Linux vastaa käsitteellisesti palvelun kangasta Windows (lukuun ottamatta OS yksityiskohtia ja ohjelmointi kielituki). Näin ollen sekä [aiemmin luoduissa asiakirjoissa](http://aka.ms/servicefabricdocs) useimmat koskee auttavat tutustuminen tekniikka.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Tuetut käyttöjärjestelmät ja ohjelmoinnin kielet

Rajoitettu preview tukee yhden kehittäminen klustereiden lisäksi usean kuormitusryhmälle klustereiden luomisen käynnissä Ubuntu palvelimen 16.04 Azure-tietokannassa. Esikatselu tukee Java-ja C# luotettava toimijat ja luotettavia tilattomien palveluja kehysten Vieras suoritettavat ja orchestrating Docker säilöjen lisäksi.  

>[AZURE.NOTE] Luotettavan sivustokokoelmat ei tue Linux vielä. Tuelle yksin klustereiden ei tueta joko - vain yksi ruutu ja Azure Linux usean kuormitusryhmälle klustereiden tuetaan esikatselussa.

## <a name="supported-tooling"></a>Tuetut sillä

Esikatselu tukee Azure CLI klusterissa käsittelemisen. Java-kehittäjille Pimennys ja Yeoman annetaan Pimennys tuetut Linux ja OS x. OS x-integroinnin käyttää Linux-AM kautta vagrant Näytä lisäasetukset. C#-kehittäjille Yeoman integrointi annetaan Luo sovellusmallit.

## <a name="next-steps"></a>Seuraavat vaiheet


1. Tutustu [Luotettava toimijat](service-fabric-reliable-actors-introduction.md) ja [Luotettavia palveluja](service-fabric-reliable-services-introduction.md) ohjelmoinnin puitteissa.

2. [Linux kehittäminen lisätietoja ympäristön valmistelemisesta](service-fabric-get-started-linux.md)

3. [OS x-lisätietoja kehitysympäristön valmistelemisesta](service-fabric-get-started-mac.md)

4. [Linux ensimmäisen palvelun kangasta Java-sovelluksen luominen](service-fabric-create-your-first-linux-application-with-java.md)
