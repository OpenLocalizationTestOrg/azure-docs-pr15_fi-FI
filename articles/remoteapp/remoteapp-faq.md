<properties 
    pageTitle="Azure RemoteApp usein kysytyt kysymykset | Microsoft Azure" 
    description="Lue vastauksia usein kysyttyjä kysymyksiä Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp usein kysytyt kysymykset

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Usein kysyttyihin Azure RemoteApp seuraaviin kysymyksiin. On muiden? Tutustu [RemoteApp keskustelupalstat](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) ja kerro meille, mitä haluat tietää tai avattava kommentin alapuolella.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Etkö löydä etsimääsi? On muita kysymyksiä, emme ei ole puheluun?
Jos et löydä tietoja tarvitset tai sinun on muita kysymyksiä, jotka on ei ole kattava tähän, siirry [Azure RemoteApp-keskustelupalstalla](http://aka.ms/araforum) ja lähetä kysymys siellä. Lisätietoja on seuraavassa artikkelissa aina lisätään.

## <a name="what-is-azure-remoteapp"></a>Mikä on Azure RemoteApp? ##


- **Mikä on Azure RemoteApp?** RemoteApp on Azure-palvelun avulla voit antaa suojatun, remote sovellusten useista eri käyttäjän laitteista. Lisätietoja on artikkelissa [Azure RemoteApp](remoteapp-whatis.md).
- **Mitkä ovat asennusvaihtoehdot?** RemoteApp kokoelmien kahdenlaisia: cloud ja hybrid. Sinun on yhtä määräytyy useista tekijöistä, kuten Tarvitsetko toimialueen liity. Käsittelemme kaikki päätökset [tähän](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Pikavihjeet Azure RemoteApp käyttämiseen ##
- **Kuinka kauan, kunnes I 'M irti? Kuinka kauan voin voidaan käyttämättömänä ennen kuin voit tarkastella minä käynnistyksen?** 4 tuntia. Jos sinulle tai käyttäjät on ollut käyttämättömänä 4 tuntia, jonka sinut vastedes automaattisesti Azure RemoteApp ulos. Tutustu muiden oletusasetusten [Azure tilaus ja palvelun rajoitukset, kiintiöiden](../azure-subscription-service-limits.md)ja rajoitukset.
- **Voinko kokeilla palvelua maksutta?** Kyllä. 30 päivää on käytettävissä maksuttoman kokeiluversion käyttäjäksi. Kokeiluversion päättyy jälkeen voi siirtyä maksettu-tilille (jonka avulla voit tuotannon) tai palvelu. Aloita siirtymällä [portal.azure.com](http://portal.azure.com) maksuton kokeiluversio - Luo RemoteApp uuden esiintymän. Maksuttoman kokeiluversion avulla voit luoda RemoteApp 2 esiintymät 10 käyttäjää kohden esiintymän kanssa. Muista, että tämä kokeiluversion sijaitsee vain 30 päivää.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp tilauksen tiedot ##

- **Mitkä ovat palvelun rajoituksia?** Voit opetella oletusasetukset ja palvelun rajojen Azure RemoteApp [Azure tilaus ja palvelun rajoitukset, kiintiöiden](../azure-subscription-service-limits.md)ja rajoitukset. Kerro meille, jos sinulla on muita kysymyksiä.
- **Kuinka monta käyttäjää minulla on?** On vähintään 20 käyttäjää. Haluan toista on huipputehokas Tyhjennä - pienin arvo on 20. Sinua laskuteta 20 varten. 
- **Kuinka paljon RemoteApp kustannukset?** Tutustu [Azure RemoteApp hinnat tiedot ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Toisentyyppiseen sivustokokoelman maksaa yli toiseen?** Kyllä, se voidaan sivustokokoelman tarpeen mukaan. Hybrid sivustokokoelman vaatii yhteyden Azure RemoteApp paikalliseen verkkoon. Jos käytät aiemmin luotua VNET/Express-reittiä, ei ilman lisäkustannuksia. Mutta jos käytät uusi Azure-VNET ja yhdyskäytävää tai Express reitin, voit veloitetaanko [VPN-yhdyskäytävän](https://azure.microsoft.com/pricing/details/vpn-gateway) tai [Express reitityksen](https://azure.microsoft.com/pricing/details/expressroute/). (Yksityiskohtainen linkit) kustannus on päälle kuukausittain Azure-RemoteApp kustannus.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Sivustokokoelmat - tuetut toiminnot, jotka kannattaa valita ja muille
- **Mukautettu-liiketoiminta (LOB) sovellusten tukee?** Kyllä. Mukautetun sovelluksen käyttäminen Azure RemoteApp, luoda [mukautetun mallin kuvaa](remoteapp-create-custom-image.md)ja lataaminen RemoteApp-sivustokokoelman.
- **Mukautettu LOB-sovellus toimii Azure RemoteApp?** Kuva, joka, testaa se on paras tapa. Tutustu [RD yhteensopivuuden keskelle](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Käyttöönottomenetelmä (cloud tai hybrid) parhaiten organisaatiossani?** Hybrid sivustokokoelmat antaa täydellisen kokemus, jos haluat, että koko kertakirjautuminen (SSO) integrointi ja suojatun paikallisen verkon yhteys. Cloud sivustokokoelmat ovat joustava ja helppo tapa eristää käyttöönoton käyttämällä useita todennustavat. Lisätietoja [asennusvaihtoehdot](remoteapp-whatis.md).
- **On SQL- tai toisen tietokannan joko paikallisessa tai Azure-tietokannassa. Mitä käyttöönottotapa käytetään?** Riippuu siitä, missä SQL- tai Taustajärjestelmä tietokannan. Jos tietokanta on yksityisverkko, käytä hybrid-sivustokokoelman. Jos tietokanta on määritetty Internet ja muodostaa siihen yhteyden sallii, voit käyttää cloud-sivustokokoelman.
- **Entä aseman yhdistämistä, USB ja sarja, Leikepöydän jakaminen ja tulostimen uudelleenohjaus?** Kaikki näitä ominaisuuksia tuetaan Azure RemoteApp. Leikepöydän jakaminen ja tulostimen uudelleenohjaus on käytössä oletusarvoisesti. Voit lukea lisää uudelleenohjaus [tähän](remoteapp-redirection.md). 


## <a name="template-images"></a>Sivustomallien kuvia
- **Voinko käyttää cloud tai aiemmin virtuaalikoneen mallina RemoteApp-keräämistä varten?** Kyllä! Voit luoda Azure-AM perusteella kuvan, jollakin tilaukseen sisältyy kuvat tai luoda mukautetun kuvan. Tutustu [RemoteApp kuva-asetukset](remoteapp-imageoptions.md).


## <a name="network-options"></a>Verkkoasetukset
- **Hybrid sivustokokoelman edellyttää VNET. Microsoft käyttää Microsoftin aiemmin VNET?** Voit tehdä aiemmin VNET onko Azure-VNET. Katso "Vaihe 1: Määritä virtual verkko" Lisätietoja [hybrid sivustokokoelman ohjeita](remoteapp-create-hybrid-deployment.md) .
- **VNET käyttää cloud kokoelman kanssa** Todellakin voit. Tutustu [cloud sivustokokoelman luominen](remoteapp-create-cloud-deployment.md), etenkin vaiheessa 1, saat lisätietoja.

## <a name="authentication-options"></a>Todennusasetukset



- **Miten käyttöoikeuksien? Mitä menetelmiä voi käyttää?** Cloud sivustokokoelman tukee Microsoft ja Azure Active Directory-tilit, jotka ovat myös Office 365-tilien. Hybrid sivustokokoelman tukee vain Azure Active Directory-tilit, jotka on synkronoitu (kuten [Azure Active Directory-synkronointi](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)-työkalun avulla) Windows Server Active Directory-käyttöönotosta; tarkemmin sanottuna joko synkronoitu salasanojen synkronoinnin-vaihtoehto tai Active Directory Federation Services (AD FS) federation määritetty synkronoitu. Voit myös määrittää [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Azure Active Directory-käyttäjiä on oltava alihallintaa, johon on liitetty tilaukseesi. (Voit tarkastella ja muokata tilauksen portaalissa **asetukset** -välilehdessä. Lisätietoja on kohdassa [Muuta Azure Active Directory-vuokraajan käyttämä RemoteApp](remoteapp-changetenant.md) .)

- **Miksi antaa omat Azure Active Directory käyttöoikeuksien?** Azure Active Directory-käyttäjien on oltava hakemiston, johon on liitetty tilauksen. Voit tarkastella tai muokata kansion asetukset-välilehdessä portaalissa. Lisätietoja on kohdassa [Muuta Azure Active Directory-vuokraajan käyttämä RemoteApp](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Asiakkaat - mitä laitetta voi käyttää Azure RemoteApp käytetään?
Löydät hyvä asiakkaan tiedot, mukaan lukien ohjeet asentamisesta eri asiakkaiden etsiminen [käyttäminen Azure RemoteApp sovelluksia](remoteapp-clients.md).

- **Mitkä laitteet ja käyttöjärjestelmät asiakassovellukset tukevat?**
Ensimmäinen, tietokoneita ja taulutietokoneita: 
    - Windows 10: ssä (Preview-versio)
    - Windows 8.1 ja Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Android-tableteille
    - iPad-sovellukset ja puhelimille:
    - iPhone
    - Android-puhelimeen
    - Windows Phone
 
    [Lataa](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) nyt RemoteApp asiakas.
- **Tukeeko Azure RemoteApp ohuen asiakkaat?** Seuraavat Windows upotetun ohuen asiakkaat tuetaan:
    - Windowsin upotettu Standard 7
    - Windows 8 Standard upotettu
    - Windows 8.1 alan upotettu Pro
    - Windows 10 IoT yrityksen

- **Windows Server-version tuetaan varten Remote Desktop istunnon Host (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Tuki ja palaute


- **Mikä on RemoteApp tuki suunnitteleminen?** Tuen laskutus ja hallintaa varten on annettu ilmaiseksi. Tekninen tuki on saatavana [Azure palvelusopimusten vaihtoehdot](https://azure.microsoft.com/support/plans/). Saat myös maksuttomia yhteisön tukea Microsoftin [Azure tuoteluettelon](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)avulla. 
- **Miten voin antaa palautetta?** Käy [palautetta keskustelupalstalle](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Kuka lisätietoja Azure RemoteApp voit keskustella?** Lisäksi Microsoftin [tuoteluettelon](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), joka on kätevä, jos haluat lähettää kysymyksiä, voit liittyä viikoittain [Kysy asiantuntijoiden webinaari](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), jossa käsittelemme asioista RemoteApp.
- **Mitä tietoja RemoteApp asiakirjat?** Olemme niin että sinua. Lisäksi yritysportaalin Ohje-laatikon ohjesisältö (Napsauta **** ? millä tahansa sivulla portaalissa) on seuraavissa artikkeleissa on käytettävissä opeta voit RemoteApp tietoja:
    - **Pääset alkuun:**
        - [Mikä on RemoteApp?](remoteapp-whatis.md)
        - [Mikä on RemoteApp sivustomallien kuvia?](remoteapp-images.md)
        - [Miten käyttöoikeudet toimii?](remoteapp-licensing.md)
        - [Miten RemoteApp ja Officen toimivat yhdessä?](remoteapp-o365.md)
        - [Miten uudelleenohjaus toimii RemoteApp](remoteapp-redirection.md)?
    - **Ottaa käyttöön:**
        - [Luo mukautetun mallin kuva](remoteapp-create-custom-image.md)
        - [Hybrid sivustokokoelman luominen](remoteapp-create-hybrid-deployment.md)
        - [Cloud sivustokokoelman luominen](remoteapp-create-cloud-deployment.md)
        - [Määrittää Azure Active Directory RemoteApp](remoteapp-ad.md)
        - [Julkaise sovelluksen RemoteApp](remoteapp-publish.md)
    - **Hallinta:**
        - [Käyttäjien lisääminen](remoteapp-user.md)
        - [Määrittäminen ja käyttäminen RemoteApp parhaat käytännöt](remoteapp-bestpractices.md)  

    Videot! On myös useita RemoteApp videoita. Joitakin on esittely ([Azure RemoteApp esittely](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), kun muiden opastusta käyttöönoton ([Cloud käyttöönotto](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) ja [yhdistelmäympäristö](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Katso niitä.

 
### <a name="help-us-help-you"></a>Auta meitä auttamaan 
Tiesitkö, että lisäksi tässä artikkelissa arviointi ja tehdä kommentit alaspäin alla, voit tehdä muutoksia itse artikkelin? Järjestelmässä ei löydy? Ongelmia? Kirjoittaa jotakin, mitä on vain hankalaa? Vierittää ylöspäin ja valitse muokkaamiseen **GitHub muokkaaminen** - ne toimitetaan US tarkastelua varten ja sitten, kun olemme kirjautua niitä, näet muutokset ja parannukset täältä.
