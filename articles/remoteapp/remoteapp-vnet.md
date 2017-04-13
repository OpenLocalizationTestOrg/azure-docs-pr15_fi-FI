
<properties
    pageTitle="Vahvista Azure RemoteApp käytettäväksi Azure-VNET | Microsoft Azure"
    description="Opettele Varmista Azure VNET on valmis käytettäväksi Azure RemoteApp"
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



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Vahvista Azure VNET Azure RemoteApp käyttäminen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Ennen kuin voit käyttää Azure-VNET Azure RemoteApp, voit tarkistaa VNET. Tämä auttaa estämään ongelmat yhteyttä.

Vahvista Azure VNET, toimi seuraavasti:

1. Luo Azure virtuaalikoneen, Azure VNET, jota haluat käyttää Azure RemoteApp aliverkon sisällä.

2. Muodosta yhteys valitsemalla **Yhdistä** -vaihtoehdon hallinta-portaalissa, AM.
3. Liity virtuaalikoneen samaa toimialuetta, jota haluat käyttää Azure RemoteApp. Jos olet luomassa hybrid kokoelma, joka yhdistää paikalliseen verkkoon, liittää virtuaalikoneen paikallisen toimialueen.

Jos tämä onnistuu, Azure-VNET on valmis käytettäväksi RemoteApp.

Lisätietoja lopusta loppuun hybrid sivustokokoelman työnkulun seuraavissa artikkeleissa:

- [Voit suunnitella virtual verkon Azure RemoteApp](remoteapp-planvnet.md)
- [Hybrid sivustokokoelman luominen](remoteapp-create-hybrid-deployment.md)
- [Ota käyttöön Azure RemoteApp sivustokokoelman Azure-Virtual Network (ja ExpressRoute tuki)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
