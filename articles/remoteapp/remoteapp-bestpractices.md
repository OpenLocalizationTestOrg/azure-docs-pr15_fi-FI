<properties
    pageTitle="Azure RemoteApp parhaita käytäntöjä | Microsoft Azure"
    description="Parhaat käytännöt määrittäminen ja käyttäminen Azure RemoteApp."
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

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Määrittäminen ja käyttäminen Azure RemoteApp parhaat käytännöt

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Seuraavat tiedot auttavat määrittäminen ja käyttäminen Azure RemoteApp toimintaa.

## <a name="connectivity"></a>Yhteys


- Käytä aina asiakkaan uusimpaan versioon. Vanhempien käyttäminen saattaa aiheuttaa yhteysongelmien ja muut eivät toimi oikein kokemukset. Automaattinen sovelluksen laitteeseesi päivitysten ottaminen käyttöön varmistat, että uusin asiakkaan aina asennettuna.
- Käytä aina eniten vakaata ja luotettavat internet-yhteyden käytettävissäsi.  
- Käytä tuettu vain välityspalvelimen yhteydet optimaalisen yhteyksien suorituskykyä.  SOCKS-välityspalvelimen ei tueta.

## <a name="applications"></a>Sovellukset


- Tallenna ja sulje RemoteApp-sovellukset, kun olet valmis-sovelluksella. Sovelluksen sulkeminen ei voi aiheuttaa tietojen menettämisen.
- Tarkista ennen niiden käyttämistä Azure RemoteApp mukautettuihin sovelluksiin. Tämä sisältää varmistaa, että ne käyttää moni-ympäristön ja ei tarjoaman tarpeettomat resurssit, kuten muistin ja suorittimen, joka voi jättää liian-vähän suoritinaikaa saman sivustokokoelman toinen käyttäjä. Tietoja Lataa ja tarkista [Yhteensopivuus sovelluksen Etätyöpöytäpalvelut parhaita käytäntöjä](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Määrittäminen ja hallinta


- Sivustomallien kuvia ajan tasalla, ohjelmistopäivitykset ja muut tärkeät päivitykset asentaminen tarpeen mukaan. Näin varmistat, että kun Azure RemoteApp automaattinen-asteikkoa täyttävän oman kapasiteettia, jokaiselle esiintymälle on asennettu.  
- Varmista, että Active Directory Federation Services (AD FS) käyttöönoton on suojattu ja luotettavia. Muussa tapauksessa asiakkaan välityspalvelimia epäonnistumiseen, estämisestä Azure RemoteApp.
- Määritä sivustomallien kuvia asennetut sovellukset, rooleja tai ominaisuudet siten, että ne ovat tilattomien. Hän ei kannata kaikki esiintymät näennäiskoneiden on jatkuva tilaan RemoteApp-palvelussa.
    - Tallentaa kaikki käyttäjätiedot käyttäjäprofiilien tai ulkoisen palveluun tallennustilan muihin sijainteihin, kuten paikallisen tiedoston osakkeet tai OneDrive.
    - Tallentaa jaettujen tietojen tallennuspaikkojen ulkoisen-palveluun, kuten paikallisen tiedoston osakkeet tai OneDrive.
    - Määritä järjestelmää asetukset-suunnittelumallin kuva sijaan yksittäisiä näennäiskoneiden palveluun.
    - Automaattinen ohjelmistopäivitykset sovelluksiin käytöstä - sijaan käyttää niitä manuaalisesti suunnittelumallin kuva ja testaa ne ennen niiden käyttöönottoa mallista.
