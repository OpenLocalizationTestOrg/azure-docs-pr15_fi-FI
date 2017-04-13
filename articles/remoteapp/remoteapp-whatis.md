<properties 
    pageTitle="Mikä on Azure RemoteApp? | Microsoft Azure" 
    description="Opit jakamaan sovellukset ja resursseja, joiden kautta Azure RemoteApp kaikissa laitteissa." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Mikä on Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp näyttää paikallisen Microsoft RemoteApp-ohjelman, palautettua mukaan Etätyöpöytäpalvelut, Azure toimintoja. Azure RemoteApp avulla voit määrittää suojatun, remote access-sovellukset useista eri käyttäjän laitteista. Azure RemoteApp isännöi tilapäisten terminaalissa Server-istunnot lähinnä pilveen ja voit käyttää niitä ja jakaa niitä käyttäjien kanssa.

Voit jakaa Azure RemoteApp käyttäjille lähes missä tahansa laitteessa sovellukset ja resurssit. Olemme isännöidä sovelluksia pilvipalvelussa, eli Microsoft huolehtia laitteiston ja skaalausta täyttämään käyttäjä. Sinun on tehtävä ei ladata sovellukset, jonka haluat jakaa, ja poistaa sitten käyttäjille kyseisissä sovelluksissa. [Käyttäjät saavat säilyttää oman laitteensa](remoteapp-clients.md), kun hallitset kaikki palvelun Azure-portaalissa. Voit myös halutessasi yrityksen tunnistetiedoillasi mahdollistaa suojauksesta sovellukset ja tiedot.

Lue lisätietoja Azure RemoteApp tai, jos Microsoft on jo ovat vakuuttuneita siitä sinua, [Kokeile suoritetaan nyt](https://azure.microsoft.com/services/remoteapp/).

Sinulla on kysyttävää Azure RemoteApp? Tutustu Microsoftin [usein kysytyt kysymykset](remoteapp-faq.md).

[Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx)Azure RemoteApp kuuluu.

**Uusi!** Haluatko lisätietoja Azure RemoteApp? Tai Haluatko tarkistaa Azure RemoteApp tasolla? Osallistuminen Microsoftin viikoittain [Kysy asiantuntijoita webinaari](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp sivustokokoelmat
[Azure RemoteApp](remoteapp-collections.md)kokoelmien kahdenlaisia:


- **Cloud sivustokokoelman** isännöidään ja -ohjelmien tiedot tallennetaan pilveen. Käyttäjät voivat käyttää sovellusten kirjautumalla sisään Microsoft-tili- tai yrityksen tunnistetiedot synkronoitu tai liitetty Azure Active Directory-hakemistosta.

    Valitse cloud sivustokokoelman kun sovellus, jonka haluat jakaa ei edellytä mitään resurssin yhteyden yrityksen yksityisverkon (esimerkiksi kautta VPN-laite). Jos sovellus käyttää resurssien Internet-, OneDrive-tai Azure-pilvi sivustokokoelman toimii puolestasi. Kannattaa myös nopein luomiseen.

- **Hybrid sivustokokoelman** isännöidään ja tallentaa tiedot Azure cloud, mutta myös antaa käyttäjien access tietojen ja resurssien tallennetun kuin paikalliseen verkkoon kirjauduttaessa. Käyttäjät voivat käyttää sovellusten kirjautumalla sisään yrityksen tunnistetiedot synkronoitu tai liitetty Azure Active Directory-hakemistosta.

    Valitse hybrid kokoelma, jos asetat resurssit yrityksen yksityisverkon yhteyden. Jos sovellus on pääsy jompikumpi seuraavista:

    - Tiedostopalvelimiin, jotka sijaitsevat intranetissä
    - Nopeuttaa
    - Tietokantojen palomuurin takana

    Tämä on yleensä enemmän hyötyä suuret yritykset, joissa on paljon resursseja niiden yksityisiä verkkoja ei voi siirtää pilveen.

Eri sivustokokoelmat usealla eri tavalla, mukaan lukien verkot, niin Selvitä, [mitä sivustokokoelman](remoteapp-collections.md) toimii parhaiten puolestasi. 


### <a name="updating-your-collection"></a>Kokoelma päivittäminen
Yksi hybrid ja pilvi kokoelmien eroista on saatavilla olevat ohjelmistopäivitykset käsittelytavan. Cloud kokoelma, joka käyttää esiasennettu Office 365 ProPlus tai Office 2013-kuva, jossa ei tarvitse huolehtia päivityksiä. Palvelu säilyttää itse ja palauttaa kanssa jatkuvasti, sovellusten ja käyttöjärjestelmän päivityksiä.

Hybrid sivustokokoelmat, sekä cloud sivustokokoelmat, käytä mukautetun mallin kuvaa olet vastuussa kuva ja sovellukset. Toimialueen liittyneet kuvia voit hallita päivitykset työkaluilla, kuten Windows Update, Ryhmäkäytäntö tai System Center.

Kun olet muokannut mukautetun mallin kuvaa, lataa uusi kuva Azure pilveen ja päivitä sitten sivustokokoelman käyttämään uusi kuva. (Tee tämä Azure RemoteApp **Pika-aloitus** -sivulta tai koontinäytön.)

Lisätietoja on kohdassa [Päivitä kokoelmasta](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Tuetut RemoteApp-ohjelmat
Windows-ja Windows RT RemoteApp-asiakassovellukset sekä Microsoft etätyöpöydän sovellusten tuetaan Azure RemoteApp Mac, Live v2.0. Käyttäjille voi käyttää nämä sovellukset niiden Mobile tai laskea laitteiden uusien Azure RemoteApp-ohjelmien käyttäminen.

Lisätietoja asiakkaiden kohdassa [käyttäminen Azure RemoteApp sovelluksia](remoteapp-clients.md) .

## <a name="next-steps"></a>Seuraavat vaiheet
Siirry! Kokeile! Nämä artikkelit avulla pääset alkuun Azure RemoteApp:

- [Millaisia sivustokokoelman tarvitaan Azure RemoteApp?](remoteapp-collections.md)
- [Luo Azure RemoteApp-kuva](remoteapp-imageoptions.md)
- [Azure RemoteApp cloud kokoelma luominen](remoteapp-create-cloud-deployment.md)
- [Azure RemoteApp hybrid kokoelma luominen](remoteapp-create-hybrid-deployment.md)
- [Miten käyttöoikeudet toimii Azure RemoteApp?](remoteapp-licensing.md)
- [Azure RemoteApp parhaat käytännöt](remoteapp-bestpractices.md)
- [Azure RemoteApp usein kysytyt kysymykset](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Auta meitä auttamaan 
Tiesitkö, että lisäksi tässä artikkelissa arviointi ja tehdä kommentit alaspäin alla, voit tehdä muutoksia itse artikkelin? Järjestelmässä ei löydy? Ongelmia? Kirjoittaa jotakin, mitä on vain hankalaa? Vierittää ylöspäin ja valitse **Muokkaa GitHub** tai **Muokkaa** muokkaamiseen – ne toimitetaan US tarkistettavaksi ja sitten, kun olemme kirjautua niitä, näet muutokset ja parannukset täältä.