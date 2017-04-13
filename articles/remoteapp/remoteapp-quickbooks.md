<properties 
    pageTitle="Ottaa käyttöön QuickBooks Azure RemoteApp | Microsoft Azure" 
    description="Opettele QuickBooks jakaminen Azure RemoteApp." 
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



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Miten voit ottaa QuickBooks Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Seuraavat tiedot avulla voit jakaa QuickBooks Azure RemoteApp-sovelluksen.


Voit jakaa QuickBooks 2015 yrityksen Azure RemoteApp hybrid tai pilvestä kokoelmasta. Yritys-tiedosto on sijaittava AM, jossa on käytössä QuickBooks-tietokantapalvelin, joka on erillinen Azure RemoteApp-palvelimiin. Ei koskaan Azure RemoteApp kuva yrityksen tiedoston tallennuspaikka – tietojen menettämisen odotetaan, jos teet tämän. Vain QuickBooks yrityksen tukee isännöinnin QuickBooks-tietokantapalvelin, johon käytettävissä standard Windowsin verkkotoiminnot kautta ulkoisen jakamisen QuickBooks-tiedoston.   

> [AZURE.IMPORTANT] QuickBooks-tietokantapalvelin, joka isännöi yrityksen tiedoston on sijaittava eri AM saman VNET kuin Azure RemoteApp-sivustokokoelman sisällä.  

## <a name="steps-to-deploy-quickbooks"></a>Ottaa käyttöön QuickBooks vaiheet

1. Luo Azure-AM ja asenna QuickBooks-QuickBooks-tietokantapalvelin, ja sijoita Azure AM yrityksen-tiedosto.  Varmista, että voit määrittää oikein palomuurisäännöt.
2. Asenna QuickBooks [Mukautetun kuvan](remoteapp-imageoptions.md) ja luoda aiemmin [Azure RemoteApp sivustokokoelman](remoteapp-collections.md), cloud tai hybrid sisällä täsmälleen saman VNET AM isännöidä QuickBooks-tietokantapalvelin yrityksen tiedostojen sijainti. 
3.  [Julkaiseminen](remoteapp-publish.md) QuickBooks-sovelluksen käyttäjille
4.  Käynnistä Azure RemoteApp isännöimä QuickBooks-asiakas, siirry käytön isännöintipalvelu QuickBooks-tietokantapalvelin AM vakio Windows verkon ja yritys-tiedoston avaaminen. 

## <a name="documentation-references"></a>Ohjeet viittaukset

- QuickBooks [tuettu käyttömahdollisuudet](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [Käyttöönottoasetukset](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Voit myös tarkistaa Ignite esityksen, [Fundamentals, Microsoft Azure RemoteApp hallintaan liittyvistä](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - eteenpäin, voit siirtyä QuickBooks-osa 1:02:45 siirtyminen.

## <a name="deployment-architecture"></a>Käyttöönotto-arkkitehtuuri

![QuickBooks + Azure RemoteApp käyttöönotto](./media/remoteapp-quickbooks/ra-quickbooks.png)