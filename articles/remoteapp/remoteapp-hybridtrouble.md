
<properties
    pageTitle="Vianmääritys luominen RemoteApp hybrid sivustokokoelmat | Microsoft Azure"
    description="Opettele RemoteApp hybrid sivustokokoelman luominen virheiden vianmääritys"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Sivustokokoelmien luominen Azure RemoteApp-hybrid vianmääritys

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Hybrid sivustokokoelman isännöidään ja tallentaa tiedot Azure cloud, mutta myös antaa käyttäjien access tietojen ja resurssien tallennetun kuin paikalliseen verkkoon kirjauduttaessa. Käyttäjät voivat käyttää sovellusten kirjautumalla sisään yrityksen tunnistetiedot synkronoitu tai liitetty Azure Active Directory-hakemistosta. Voit ottaa käyttöön hybrid kokoelma, joka käyttää aiemmin Azure Virtual verkkoon tai voit luoda uuden virtual verkon. Suosittelemme, että luot tai CIDR suuren alueen virtual verkko-osoitteiden käyttäminen odotettu tulevien kasvun Azure RemoteApp.

Sivustokokoelman vielä ole luonut? Katso [Luo hybrid sivustokokoelman](remoteapp-create-hybrid-deployment.md) ohjeita.

Jos sinulla on vaikeuksia luominen kokoelmaa tai jos kokoelma ei toimi tapaa, jolla arvelet sen pitäisi, Katso seuraavat tiedot.

## <a name="your-image-is-invalid"></a>Kuva ei kelpaa ##
Jos näet viestin, kuten "GoldImageInvalid" kun odotat Azure valmistelu kokoelmaa, se tarkoittaa, että suunnittelumallin kuva ei täytä [määritetty kuva vaatimukset](remoteapp-imagereqs.md). Siirry, lue näitä [vaatimuksia](remoteapp-imagereqs.md), korjaa kuvan ja yritä luoda sivustokokoelman uudelleen.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Onko oman VNET verkon käyttöoikeusryhmät, jotka on määritetty? ##
Jos sinulla on määritetty kokoelmaa käytät aliverkon verkon käyttöoikeusryhmät, varmista, että nämä [URL-osoitteista ja porteista](remoteapp-ports.md) ovat käytettävissä aliverkon ulkopuolella.

Voit lisätä muita verkon käyttöoikeusryhmät-parannettu hallinta aliverkon käyttämät VMs.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Käytät DNS-palvelimet? Ja ne ovat käytettävissä olevat VNET aliverkon ulkopuolella? ##
>[AZURE.NOTE] Varmista, että VNET DNS-palvelimet ovat aina ylöspäin ja aina yrittää ratkaista ylläpidettävä VNET näennäiskoneiden on. Älä käytä Google DNS tämän.


Voit käyttää omia DNS-palvelimet hybrid loppu. Voit määrittää ne verkon määritys-rakenteessa tai palvelun hallinta-portaalissa virtual verkon luodessasi. DNS-palvelimet käytetään siinä järjestyksessä, että ne on määritetty automaattisesti tavalla (eikä PYÖRISTÄ-funktiota Mikko).  
Tutustu [Nimenselvitys VMs ja roolin esiintymät](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) , varmista, että DNS-palvelimet on määritetty correcly.

Varmista, että DNS-palvelimet kokoelmaa ovat käytettävissä ja käytettävissä VNET aliverkosta määrittämäsi tähän kokoelmaan.

Esimerkki:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Määritä DNS-tietoja](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Käytät Active Directory-toimialueen ohjauskoneen kokoelmasta? ##
Tällä hetkellä vain yhden Active Directory-toimialueen voidaan liittää Azure RemoteApp. Hybrid sivustokokoelman tukee vain Azure Active Directory-tilit, jotka on synkronoitu Windows Server Active Directory-käyttöönotosta; DirSync-työkalun avulla tarkemmin sanottuna joko synkronoitu salasanojen synkronoinnin-vaihtoehto tai Active Directory Federation Services (AD FS) federation määritetty synkronoitu. Haluat luoda mukautetun toimialueen, joka vastaa täydellisen Käyttäjätunnuksen jälkiliitteen paikallisen toimialueen ja määrittää hakemistointegrointi.

Lisätietoja on kohdassa [Määrittäminen Active Directory Azure RemoteApp](remoteapp-ad.md) .

Varmista, että annettu toimialueen tiedot ovat kelvollisia ja toimialueen ohjauskoneen on luotu Azure Remote-sovellus käyttää aliverkon AM käytettävissä. Varmista myös palvelun tili valtuudet on oikeudet lisätä tietokoneita annettu toimialueen ja annettujen Mainos nimi voi selvittää DNS VNET säädetty.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Mitä toimialuenimen määrititkö kokoelmaa luonnin yhteydessä? ##

Luotu tai lisätty toimialuenimi on oltava sisäisen toimialuenimen (ei Azure AD toimialuenimi), ja se on oltava ratkaistava DNS-muodossa (contoso.local). Esimerkiksi Active Directory sisäinen nimi (contoso.local) ja Active Directory-UPN (contoso.com) – sinun on käytettävä sisäinen nimi, kun luot kokoelmasta.
