<properties
    pageTitle="Käyttäjän lisääminen Azure RemoteApp-sivustokokoelman | Microsoft Azure"
    description="Lisätietoja käyttäjien lisäämisestä Azure RemoteApp-kokoelmaan"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Käyttäjän lisääminen Azure RemoteApp-kokoelmaan

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Ennen kuin käyttäjät voi tarkastella ja käyttää sovelluksia Azure RemoteApp, sinun on kokoelma käyttöoikeuden myöntäminen käyttäjille. Tämä on helppo osa: Valitse **Käyttöoikeudet** -välilehdessä Anna käyttäjän tilitiedot ja valitse sitten valintamerkkiä.

Käytettävän tilin tiedot tarvitaan? Sivustokokoelman tyypin riippuu luodessasi (cloud tai hybrid) ja onko käytössäsi on Office 365 ProPlus kyseistä kokoelmaa.

## <a name="supported-user-identities"></a>Tuetut käyttäjätietojen

Eri sivustokokoelman tyypit (cloud ja hybrid) tukevat eri käyttäjätietojen käyttäminen access-sovellukset.  

RemoteApp hybrid kokoelma sinun on Active Directory-toimialueen infrastruktuurin määrittäminen paikallisen ja Azure Active Directory-vuokraajan Directory-integrointi (ja halutessasi yksittäinen Sign). Lisäksi on luotava joitakin Active Directory-objekteja paikallisesta hakemistosta.  

Varten RemoteApp cloud kokoelma kuka tahansa käyttäjä, jolla jäsenyyksiä Azure Active Directory on voidaan myöntää käyttöoikeuksia RemoteApp sisältämään Microsoft Accounts.  Katso alla oleva taulukko.

Office 365-käyttäjät voivat Azure Active Directory-käyttäjiä. Jos heillä on hybrid Azure Active Directory-hakemiston synkronoida tilit, ne voidaan myöntää käytön RemoteApp-yhdistelmäympäristössä.   

Voit käyttää tämän taulukon, jonka jäsenyyden voi käyttää kokoelmaa ja Active Directory-vaatimukset ovat pikaohje.

|Käyttäjätilien |Cloud   |Hybrid|
|--------------|--------|------|
|Microsoft-tili|     Kyllä|    Ei|
|Azure Active Directory (Azure AD)| | |
|Vain Azure AD-pilvi    |Kyllä    |Ei |
|ADsync ja salasanan synkronointi  |Kyllä    |Kyllä    |
|ADsync ilman salasanan synkronointi|  Kyllä |Ei |
|ADsync AD FS  |Kyllä    |Kyllä    |
|[3rd osapuolen Azure tuettu tunnistetietojen toimittajat](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (Esimerkki Ping) |Kyllä    |Kyllä|
|Monimenetelmäisen todentamisen    |Kyllä    |Kyllä    |

Katso [Lisätietoja](remoteapp-ad.md) määrittämisestä Active Directory RemoteApp varten.


> [AZURE.NOTE] Azure Active Directory-käyttäjiä on oltava alihallintaa, johon on liitetty tilaukseesi. (Voit tarkastella ja muokata tilauksen portaalissa **asetukset** -välilehdessä. Lisätietoja on kohdassa [Muuta Azure Active Directory-vuokraajan käyttämä RemoteApp](remoteapp-changetenant.md) .)

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlus käyttäjätilitiedot
Jos käytät Office 365 ProPlus suunnittelumallin kuva sivustokokoelman *tai* , jos olet luonut mukautetun kuvan, joka käyttää Office 365: ssä, sinun on sallittua vain Lisää Azure Active Directory-käyttäjät, joilla on Office 365-tilausta varten oletustoimialueen tilauksen. Lisätietoja on kohdassa [käyttämällä Office 365: n Azure RemoteApp](remoteapp-o365.md) .
