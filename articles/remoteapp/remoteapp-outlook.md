<properties
    pageTitle="Outlookin käyttäminen Azure RemoteApp | Microsoft Azure" 
    description="Opettele määrittäminen ja käyttäminen Outlook Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Azure RemoteApp Microsoft Outlookin avulla

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp tukee Microsoft Outlookin O365. Lue lisää siitä, miten [Office toimii Azure RemoteApp](remoteapp-officesubscription.md). On joitakin Outlook, kun sitä käytetään Azure RemoteApp suositellut asetukset.

## <a name="cached-mode"></a>Synkronointitila
Synkronointitila on suositellut määritykset, kun Azure RemoteApp Outlookin avulla. Kun määrität Outlook 2013-tili, jota käytät Exchange-synkronointitilaa, Outlook 2013: n toimii paikallisen kopion käyttäjän Microsoft Exchange-postilaatikon, joka on tallennettu offline-tilassa datatiedostoon (OST-tiedosto) käyttäjän tietokonetta, ja offline-tilassa osoite osoitteiston (OAB). Välimuistiin tallennetut postilaatikon ja OAB päivitetään säännöllisesti O365-palvelusta. Lisätietoja on artikkelissa [välimuistin ja online-tilassa välisiä eroja](https://technet.microsoft.com/library/jj683103.aspx).

Käyttäjä voi valita **Exchange-synkronointitilaa** tai **Online-tilassa** , tilin asennuksen aikana tai muuttamalla tiliasetukset. Voit myös ottaa tilassa tai toisen Office Customization Tool (OCT) tai ryhmäkäytännön avulla.  

Lue [Vaiheittaiset ohjeet synkronointitila ottamisesta käyttöön](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Haku
Azure RemoteApp hakutoiminnolla Outlookissa on rajoitukset. Azure RemoteApp käyttää liittyminen VMs sopimaan käyttäjäistuntojen. Haun indeksoinnista määräytyy machine-tunnus, joka on erilainen eri VMs. Se on mahdollista, että aina, kun käyttäjä kirjautuu sisään Azure RemoteApp, ne ohjataan uuteen AM. Joka tarkoittaa, jos on paikallinen haku käyttöön indeksointitoiminto suoritetaan aina, kun tietokoneen tunnus muuttuu (kun käyttäjä on eri AM). Sen mukaan, kokoa. OST-tiedoston indeksointitoiminto saattaa kestää kauan, suorittaa ja muiden sovellusten tarvittavien resurssien määrittäminen. Haku ei vain ole hidas, mutta ei voi tuottaa tuloksia. Online-tilassa-tilin profiilin avulla kiertää ongelman, mutta suorituskykyyn haittaohjelman paikallisen välimuistin puutteen vuoksi (Katso lisätietoja välimuistin ja online-tilassa välisen eron yläpuolella olevaa linkkiä). Valitettavasti indeksoitu/paikallinen haku ei voi poistaa käytöstä ja online-haku ei voi ottaa käyttöön oletusarvoisesti Outlook 2013: ssa.
