<properties
    pageTitle="Azure RemoteApp-sovelluksilla App V | Microsoft Azure"
    description="Opettele käyttämään Azure RemoteApp App V sovellukset."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Sovelluksen V-sovellusten käyttäminen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Voit käyttää sovelluksen V sovellusten Azure RemoteApp hybrid kokoelma, joka edellyttää toimialueen liity.

Ennen kuin aloitat, varmista, että voit asentaa sovelluksen V 5.1 asiakkaan uusimmat päivitykset. Haluat luoda [Mukautetun kuvan](remoteapp-create-custom-image.md) , joka sisältää sovelluksen V-asiakas.  

On helppo käyttää olemassa olevan sovelluksen V-infrastruktuurin Azure RemoteApp. Koska hybrid sivustokokoelman on otettu käyttöön, jolla on pääsy toimialueen ohjauskoneen Azure-VNET kyselyjä ja VMs on liitetty toimialueeseen, voidaan hyödyntää oman sovelluksen v infrastruktuuri- ja menetelmiä easyily host Azure RemoteApp App V-sovellukseen. Tässä on joitakin tapoja, sinun olisi otettava huomioon App V käyttöönoton asennetun lajin perusteella:

| Kokoonpanoasetusten määrittäminen |                       | Positiivinen                                                               | Negatiivinen                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Toimitustapa       | Streaming (tarvittaessa) | Sovellus on aina uusimmat ja tuore                                     | Ensimmäisen kerran viive                                                                                    |
|                       | Otettu käyttöön               | Nopein; sovellus on jo olemassa AM                              | Koon suureneminen - tulevat kuva tilaa (127 Gigatavun rajoitus)                                                           |
| App sijainti-säilö  | Jaettu sisältö        | Sovelluksen suorittaa muistissa Azure RemoteApp esiintymän                         | Eats muistin ja hyvä yhteyden streaming (tiedosto)-palvelimeen, jossa sovellus sijaitsee                      |
|                       | Levyn (välimuistissa)         | Nopea suorittamisen. Sovellus ei ole riippuvainen sisältölähteen saatavuus | Koon suureneminen - tulevat kuva tilaa (127 Gigatavun rajoitus)                                                           |
| Kohdistaminen             | Käyttäjän                  | Edellyttää, että koko erillinen App V infrastruktuuri                          |                                                                                                       |
|                       | Yleinen (tietokone)      |  Valmiiksi julkaiseminen tai kohteen käyttämällä julkaiseminen palvelimeen                         |  On päivitettävä Azure kuva, jos haluat päivittää sovelluksen (erittäin suuri). Vie kuva tilaa. |

 Kun olet luonut mukautetun kuvan ja hybrid sivustokokoelman, julkaista sovelluksesi, määrittää käyttäjiä ja Nauti ylläpidettävä Azure RemoteApp toimitettu mihin tahansa laitteeseen App V sovellukset.
