<properties 
    pageTitle="Millaisia sivustokokoelman tarvitaan Azure RemoteApp? | Microsoft Azure" 
    description="Tiedostotyypit, jotka olivat käytettävissä Azure RemoteApp tietoja." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Millaisia sivustokokoelman tarvitaan Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp avulla voit jakaa käyttäjille millä tahansa laitteella sovellukset ja resurssit. Olemme tehdä tämän luomalla sivustokokoelmat pitoon sovellukset ja resurssit ja sitten näiden kokoelmien jakaa käyttäjille. 2 eri sivustokokoelman asetukset-eri verkko-ja todennusasetukset - joka itsellesi on?

Oletetaan, että käy läpi eri asioita ja haluat saada mahdollisimman paljon irti Azure RemoteApp-sivustokokoelman valinnat. 


## <a name="quick-differences-between-the-collection-types"></a>Sivustokokoelman nopeasti erot

|           | Cloud | Hybrid |
|-----------|-------|--------|
|Käytä aiemmin VNET| Kyllä| Kyllä|
|Edellyttää AD yhdistetyt tilit (DirSync)| Ei| Kyllä|
|Edellyttää toimialueen liitos| Ei| Kyllä|
|Edellyttää toimialueen ohjauskoneen VNET käytettävissä| Ei| Kyllä|

## <a name="cloud-collections"></a>Cloud sivustokokoelmat
- Nopea luominen - kokoelmaan on nopeasti valmisteltu, eli sovelluksia Hanki käyttäjille nopeammin.
- Tuoda Omat sovellukset tai jakaa stävällisin. Voit käyttää mukautetun kuvan (muodostetaan Azure-AM) tai jokin tilaukseen sisältyy kuvista.
- Ei tarvitse määrittää kokoelmaa ja paikallisen toimialueen välinen yhteys.
- Mutta voit käyttää omaa Azure VNET voit myös antaa paikalliseen ympäristöön tietojen jakamiseen tai käyttämään-Windows todennusta kyselyjä resurssit, kuten SQL Server (joko tietokannan todennustiedot).


OK miten jokin luoda?

- Cloud? Luo-portaalin **Nopea luominen** -vaihtoehto.
- Cloud + VNET? Luoda käyttämällä Luo **VNET kanssa** , mutta älä valitse liittyä toimialueelle.

## <a name="hybrid-collections"></a>Hybrid sivustokokoelmat
- Anna paikalliseen verkkoon + Azure VNET täydet oikeudet.
- Myös toimialueen liity käyttö sovellukset ja tiedot. Sovellusten voit paikallisen Active Directorysta todennus - he voivat käyttää toimialueen resursseja.
- Ota käyttöön Lisäasetukset valvonnan ja hallinnan kanssa System Center ratkaisuja ja Windows-ryhmäkäytännöt (kautta Windows Server 2012 R2 rakennettu mukautetun kuvan)
- Tue [ExpressRoute](https://azure.microsoft.com/services/expressroute/) Azure VNET muodostaminen paikalliseen VNET.

Luoda käyttämällä **Luo VNET kanssa** -vaihtoehto ja valitse liity toimialueeseen.

## <a name="authentication-options"></a>Todennusasetukset
Azure RemoteApp tukee Microsoftin tili- ja Azure Active Directory-tilit, mutta ei kaikkiin sivustokokoelmat tukevat kaikki menetelmät. 

| Valitse tilin tyyppi                      |                                                             | Cloud | Cloud + VNET | Hybrid |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-tili                 |                                                             | Kyllä   | Kyllä          | Ei     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure AD                                               | Kyllä   | Kyllä          | Ei     |
|                                   | AD Connect ja salasanan synkronointi                               | Kyllä   | Kyllä          | Kyllä    |
|                                   | AD Connect ilman salasanan synkronointi                            | Kyllä   | Kyllä          | Ei     |
|                                   | AD Connect AD FS                                       | Kyllä   | Kyllä          | Kyllä    |
|                                   | 3rd osapuolen tukemaan Azure-tunnistetietojen toimittajat (kuten Ping) | Kyllä   | Kyllä          | Kyllä    |
| Monimenetelmäisen todentamisen       |                                                             | Kyllä   | Kyllä          | Kyllä    |



### <a name="cloud-and-cloud--vnet"></a>Cloud ja Cloud + VNET 
Cloud sivustokokoelmat voit käyttää Microsoft-tilit, Azure AD-tilien tai kahden yhdistelmä. Käytä tilit, jotka toimivat parhaiten käyttäjiä.

On ei tiettyä käyttövaatimukset Microsoft-tilit. 

Jos haluat käyttää Azure AD-tilejä, haluat varmistaa, että Azure AD-vuokraajan vastaa jonkin-tilaukseen liittyvää. Kun olet luonut Azure RemoteApp-tilauksen, jota käytit Azure AD-vuokraajan on liitetty automaattisesti tilauksen. Kuka tahansa Azure AD-käyttäjä, voit antaa oikeuden on oltava sama alihallintaan. Tarvittaessa voit [muuttaa Azure AD-vuokraajan](remoteapp-changetenant.md) -tilaukseen liittyvää.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybridi (tai cloud + Azure AD + AD)

Käyttämällä Azure AD + paikallisen Active Directory on kokoelma hybrid. Sinun on käytettävä AD Connect integroiminen kaksi kansioita. Mutta on joitakin valinta, jos haluat siitä, miten voit määrittää AD Connect. 

Yhdistä skenaariot - salasanan synkronointi tai käyttämisen AD federation on 2 AD. Tutustu [AD Connect tiedot](../active-directory/active-directory-aadconnect.md) selvittää, mitä nämä toimii parhaiten soveltuu.

Voit myös käyttää Azure AD + AD cloud kokoelman kanssa. Varmista, että sama määrittäminen ohjeita noudattamalla.

Tutustu [Azure AD + Azure RemoteApp Active Directory-vaatimukset](remoteapp-ad.md) määritettävä Azure AD-ohjeet ja Active Directorysta.

## <a name="go-create-your-collection"></a>Valitse Luo kokoelmaa!
OK luulen olemme olet toimivuutta se nyt, joten vain kuitenkaan vasemmalle, jos haluat tehdä - Luo ensimmäinen Azure RemoteApp-sivustokokoelman.

[Cloud sivustokokoelman luominen](remoteapp-create-cloud-deployment.md) tai [luoda hybrid kokoelman](remoteapp-create-hybrid-deployment.md) - Hae vain luominen.
