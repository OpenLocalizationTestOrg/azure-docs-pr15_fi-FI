<properties
    pageTitle="Voit siirtää RemoteApp VNET Azure-VNET | Microsoft Azure"
    description="Opi siirtämään RemoteApp VNET Azure-VNET"
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



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Voit siirtää hybrid sivustokokoelman RemoteApp VNET Azure-VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Hyviä uutisia! On ottanut voit ottaa käyttöön hybrid RemoteApp sivustokokoelmien suoraan aiemmin Azure virtual lopettaminen (VNETs) sijaan RemoteApp-kohtaisia VNETs. Näillä oikeuksilla voit hyödyntää uusimmista VNET ominaisuuksista (kuten ExpressRoute) ja antaa suoraan verkkokäyttö muiden Azure palveluihin ja ottaa käyttöön kyseisen VNET näennäiskoneiden hybrid sivustokokoelmat.  (Tämä saa voit parantaa suorituskykyä ja kuin VNET VNET määrityksiä helpompaa määritys).


Oletetaan, että olet jo luonut hybrid RemoteApp sivustokokoelman kutsutaan *OriginalCollection* RemoteApp VNET kutsutaan *RemoteAppVNET*. Näin voit siirtää sen uuteen Azure-VNET kutsutaan *AzureVNET*.

1.  Luo VNET, kutsutaan *AzureVNET*, käyttäen samaan sijaintiin, DNS-määrityksiä ja osoitetilaa (varten vähintään yksi *AzureVNET* aliverkosta) voit käyttää *RemoteAppVNET* [hallinta-portaalin](http://manage.windowsazure.com/) **verkot** -välilehti.
2.  Määritä *AzureVNET* joko isännöidä tai on verkkoyhteys Active Directory-käyttöönoton *OriginalCollection* on liitetty toimialueeseen.
3.  Luo *Uusi*kerääminen uuden RemoteApp kokoelman **RemoteAppsin hallinta: ssa** -välilehdessä. (Anna **Luo VNET kanssa** -vaihtoehto, **Nopea luominen**.)
3.  Määritä *NewCollection* aliverkon *AzureVNET*-käyttöön.
4.  Määritä *NewCollection* käytettävä voit käyttää *OriginalCollection*saman kuvan ja toimialueen liittymistiedot.
5.  *NewCollection* näkyy muutaman tunnin jälkeen sivustokokoelman luettelon aktiivisen tilan kanssa.

Nyt, jos et tarvitse siirtää kaikki käyttäjätiedot alkuperäisen sivustokokoelman uuden kokoelman, toimi seuraavien ohjeiden mukaisesti seuraava:

6.  Poista *OriginalCollection*.
7.  Poista *RemoteAppVNET*.

Ja olet valmis!

Vaihtoehtoisesti, jos haluat siirtää käyttäjätiedot alkuperäisen sivustokokoelman uuden kokoelman, toimi seuraavien ohjeiden mukaisesti seuraava:

6.  Lähetä sähköpostia [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) Azure Tilaustunnus, alkuperäiseen nimi ja uusi kokoelmaa nimi ja kysy, jos haluat siirtää käyttäjätietoja.
7.  2 business päivän kuluessa RemoteApp-ryhmän siirtyvät käyttäjä access-luettelo ja kaikki käyttäjän tiedostot ja käyttäjäasetusten alkuperäisen kokoelmasta uuden kokoelman.
8.  Poista *OriginalCollection*.
9.  Poista *RemoteAppVNET*.

Ja -valmis!

Jos kysymykset tarvitse erityistä tukea, ota sähköpostin [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
