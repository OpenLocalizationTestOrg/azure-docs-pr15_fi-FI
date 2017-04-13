
<properties
    pageTitle="Koko VNET Azure RemoteApp tiedot | Microsoft Azure"
    description="Lisätietoja Azure RemoteApp käyttämisestä VNET IP-osoite vaatimuksista"
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



# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Azure RemoteApp VNET tiedot koon muuttaminen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Kun käytät Azure RemoteApp virtual verkon (VNET), RemoteApp käyttää aliverkon IP-osoitteet. RemoteApp-palvelun asteikon mukaan haluat varmistaa, että käytettävissä myös aliverkon ulkopuolella on riittävästi käytettävissä RemoteApp näennäiskoneiden IP-osoitteet. Kun koonmuuttokahvat nämä ohjeet eivät ole täydellisiä miten RemoteApp dynaamisesti pyörii ylös ja alas näennäiskoneiden kokoelmassa pyörii, sen avulla voit arvioida aliverkon alue. Tämä on erityisen tärkeää, koska kun RemoteApp-palvelun sijoitetaan VNET, et voi lisätä aliverkon koon poistamatta RemoteApp.

RemoteApp valikoimien, jonka haluat suorittaa suurin mahdollinen sinun on 100 IP-osoitteet. Jos sinulla on yksi RemoteApp sivustokokoelman Standard järjestelyn ja haluat on enintään 500 käyttäjää, tarvitset 100 IP-osoitteet, että. Vastaavasti tarvitset 100 IP-osoitteiden RemoteApp sivustokokoelman Basic palvelupakettiin, joka on 800 käyttäjää. Jos aiot käyttää vähemmän käyttäjä (pienempi kuin suurin), voit pienentää IP-osoitteita tarvitaan sivustokokoelman kohden. Pienin aliverkon koon vaatimus on 30 IP-osoitteet (/ 27).

Varmista, että VNET tiedot uloskuittaus on määritetty ja että mikrofoni toimii propertly:

- [Siirtää henkilökohtaiset VNET Azure-VNET](remoteapp-migratevnet.md)
- [Vahvista Azure VNET Azure RemoteApp käyttäminen](remoteapp-vnet.md)
