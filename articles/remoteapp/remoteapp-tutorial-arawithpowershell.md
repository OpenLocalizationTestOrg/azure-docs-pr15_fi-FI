<properties
   pageTitle="PowerShell cmdlet-komentojen käyttäminen Azure RemoteApp | Microsoft Azure"
   description="Opi käyttämään Windows PowerShellin cmdlet-komennot Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Windows PowerShellin cmdlet-komentojen käyttäminen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

 Voit hallita ja säilyttää oman sivustokokoelmat Azure RemoteApp PowerShell cmdlet-komentoja. Seuraavat tiedot avulla pääset alkuun.

## <a name="get-the-cmdlets"></a>Hae cmdlet-komennot 
-------------
Lataa ensin Azure PowerShellin cmdlet-komennot [tähän](http://go.microsoft.com/?linkid=9811175)RemoteApp sisältyvät cmdlet-komennot. 

Tutustu [Azure RemoteApp cmdlet-komennon avulla](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Määritä Azure cmdlet-komentojen käyttäminen tilauksen
------------------
Noudata [Tässä oppaassa](../powershell-install-configure.md) , jotta voit käyttää Cmdlet-komentoja vastaan Azure tilauksen.

Voit käyttää näiden ohjeiden avulla pääset nopeasti alkuun:

1.  Lataa ja asenna [Azure PowerShellin cmdlet-komennot](http://go.microsoft.com/?linkid=9811175).
2.  Käynnistä Microsoft Azure PowerShell.
3.  Suorita **Lisää AzureAccount** tarkistamiseen Azure-tilaukseen. Kirjoita pyydettäessä saman käyttäjänimen ja salasanan, jonka avulla Kirjaudu Azure-portaaliin.  
4.  Suorittamalla **Get-AzureSubscription** käyttäjätili liittyvät tilaukset-luettelo. 
5.  **Valitse AzureSubscription** Suorita ja määritä tilauksen nimi tai tunnus PowerShell console käyttäminen.

Onnittelut, PowerShellin Azure-konsolin on määritetty ja käyttövalmis. Huomaa, että sinun on repeate vaiheet 2 – 5 aina, kun käynnistät PowerShellin Azure-konsolin.  

## <a name="create-a-cloud-collection"></a>Cloud sivustokokoelman luominen
--------------------
Se on helppoa, suorita seuraava komento:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Yllä olevan komennon julkaisee automaattisesti Microsoft Office 365-sovellukset (Excel, OneNote, Outlook, PowerPoint, Visio ja Word).

Sivustokokoelman luominen voi kestää 30 minuuttia tai kauemmin. Tämän vuoksi tämä komento palauttaa Seurantatunnus, jota voit käyttää seuraavasti:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Kun kokoelma on valmis, voit lisätä käyttäjiä sivustokokoelman seuraavalla komennolla:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Ja olet valmis! Käyttäjän pitäisi yhteys löydy Azure RemoteApp-asiakas-sovellukseen [tähän](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Käytettävissä olevat cmdlet-komennot
On muut komennot, jotka on useita, ne ohjeissa tulossa pian:

Tavallinen RemoteApp sivustokokoelman cmdlet-komennot: 

- Uusi AzureRemoteAppCollection
- Hae AzureRemoteAppCollection
- Määritä AzureRemoteAppCollection
- Päivitä AzureRemoteAppCollection
- Poista AzureRemoteAppCollection
- Lisää AzureRemoteAppUser
- Hae AzureRemoteAppUser
- Poista AzureRemoteAppUser
- Hae AzureRemoteAppSession
- Katkaise yhteys AzureRemoteAppSession
- Käynnistää AzureRemoteAppSessionLogoff
- Lähetä AzureRemoteAppSessionMessage
- Hae AzureRemoteAppProgram
- Hae AzureRemoteAppStartMenuProgram
- Julkaise AzureRemoteAppProgram
- Julkaisun AzureRemoteAppProgram
- Hae AzureRemoteAppCollectionUsageDetails
- Hae AzureRemoteAppCollectionUsageSummary
- Hae AzureRemoteAppPlan

RemoteApp VPN-cmdlet-komennot:

- Uusi AzureRemoteAppVNet
- Hae AzureRemoteAppVNet
- Määritä AzureRemoteAppVNet
- Poista AzureRemoteAppVNet
- Hae AzureRemoteAppVpnDevice
- Hae--AzureRemoteAppVpnDeviceConfigScript
- Palauta AzureRemoteAppVpnSharedKey

RemoteApp mallin kuva cmdlet-komennot:

- Uusi AzureRemoteAppTemplateImage
- Hae AzureRemoteAppTemplateImage
- Nimeä AzureRemoteAppTemplateImage
- Poista AzureRemoteAppTemplateImage

Muut RemoteApp-cmdlet-komennot:

- Hae AzureRemoteAppLocation
- Hae AzureRemoteAppWorkspace
- Määritä AzureRemoteAppWorkspace
- Hae AzureRemoteAppOperationResult
 
