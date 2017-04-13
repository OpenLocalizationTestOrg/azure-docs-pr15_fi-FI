
<properties 
    pageTitle="Azure RemoteApp verkon kaistanleveyden käytön arviointi | Microsoft Azure"
    description="Lisätietoja verkon kaistanleveyden edellytyksistä Azure RemoteApp sivustokokoelmat ja sovellukset."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Azure RemoteApp verkon kaistanleveyden käytön arviointi 

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp käyttää Remote Desktop Protocol (RDP) vaihtamaan Azure pilveen ja käyttäjien sovellusten välillä. Tässä artikkelissa on joitakin perusohjeita avulla voit arvioida, verkon käyttö ja arvioi mahdollisesti verkon kaistanleveyden käytön Azure RemoteApp käyttäjää kohden.

Käyttäjäkohtainen kaistanleveyden käytön arviointi on monimutkainen ja edellyttää suorittaminen useita sovelluksia samanaikaisesti tietoja tilanteissa missä sovellukset saattavat vaikuttaa toistensa niiden tarve kaistanleveys perusteella. Vaikka etätyöpöydän asiakkaan (kuten Mac-asiakas ja HTML5: n asiakkaan) tyypin voi aiheuttaa eri kaistanleveystestin tulokset. Auttaa käydä läpi nämä monimutkaisuuden, emme Jaa käytön skenaariot useita yleisiä luokkia replikoida tosielämän skenaarioita. (Jos tosielämän skenaariota, on useita luokkia ja käyttäjän eroaa.)

Huomaa, että oletetaan RDP avulla helppo erinomainen ratkaisun useimmissa käyttö skenaarioissa verkoissa viive alapuolella 120 ms ja kaistanleveyden yli 5 MBs - perustuu käyttäjän RDP mahdollisuus säädetään dynaamisesti käytettävissä kaistanleveys ja sovelluksen arvioitu-kaistanleveyden käytön on on edelleen - Siirry. Tässä artikkelissa käsitellään niitä muutakin kuin "useimmat käyttötapoja" reuna, jossa skenaariot alkaa unwind ja käyttäjäkokemuksen alkaa heikentää tarkasteltavan.

Tutustu nyt seuraavissa artikkeleissa on tiedot, kuten huomioon otettavia seikkoja, perusaikataulun suosituksia ja mikä on eivät kuulu Microsoftin arvioiden.

- [Miten kaistanleveys ja laatu ilmene työn samassa paikassa?](remoteapp-bandwidthexperience.md)
- [Verkon kaistanleveyden käytön havainnollistetaan Yleisiä tilanteita, jossa testaaminen](remoteapp-bandwidthtests.md)
- [Jos sinulla ei ole ajan tai mahdollisuus testata nopeasti ohjeet](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Mikä on Microsoft lukuun ottamatta?

Kun olet tarkistanut ehdotettu testejä ja tutustu Yleinen (ja admittedly yleinen) suosituksia, huomioon, että on, joka on ei ota huomioon seuraavat seikat. Esimerkiksi käyttäjä-toiminto monimutkaisuuden Lataa julkiseen laatu myöntämä Lataa kaistanleveyden. Useimmat Wi-Fi-verkkoja julkiseen laatu lisäksi vaikuttaa suorituskykyyn ja käyttäjän kokemus vaikutelmaa. Vuorovaikutteinen skenaarioissa edeltävät liikenne ehkä ensin pienempi kuin ylöspäin, jotka voivat suurentaa menettää videon tai äänen kehysten ja saattaa heikentyä käyttäjän vaikutelmaa streaming käyttökokemuksesta. Voit suorittaa oman kokeet, mitä on hyvä tietyn Käyttötapaus ja verkon.

Vaikka käsiteltävät aiheet laitteen uudelleenohjaus, on oteta huomioon aiheutuvat liitettyjen laitteiden, kuten tallennuskiintiön, tulostimet, skannerit, web kamerat ja muiden USB-laitteiden verkkoliikenteen kaistanleveyden vaikutus. Yleensä kyseiset laitteet vaikutus piikkejä kaistanleveyden tarpeiden tilapäisesti ja häviää näkyvistä, kun tehtävä on suoritettu. Mutta tehnyt usein, että kaistanleveys demand voi olla aivan huomattavia.

Myös ei käsittelemme miten käyttäjä voi vaikuttaa samassa verkossa oleville muille käyttäjille. Yksittäisen käyttäjän muissa 4K video 100 Mt/s verkossa merkittävästi esimerkiksi mahdollisista vaikutuksista muille käyttäjille, että samassa verkossa yritetään suorittaa saman tehtävän. Valitettavasti saa asteittain näyttävyyttä samanaikainen käyttö antaa yleisiä tai kaikki osana suositus siitä, miten järjestelmä suorittaa milloin kooste vaikutus määrittämiseen. Kaikki Microsoft sanoa on protokolla tekniikkaan tekee käytettävissä kaistanleveys käyttäminen, mutta se on rajoituksia.