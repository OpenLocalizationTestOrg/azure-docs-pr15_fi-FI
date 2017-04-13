<properties 
    pageTitle="Azure RemoteApp vianmääritys - käynnistys- ja yhteyden sovellusvirheet | Microsoft Azure" 
    description="Opettele liittyviä alkamis- ja yhteyden muodostaminen Azure RemoteApp-sovelluksia." 
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



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Vianmääritys Azure RemoteApp - sovellusten käynnistys- ja yhteyden virheistä 

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Sovellusten ylläpidettävä Azure RemoteApp voi epäonnistua käynnistää muutamalla eri syistä. Tässä artikkelissa kerrotaan syitä ja virhesanomat käyttäjien saattaa tulla näkyviin, kun yrität käynnistää sovelluksia. Se myös puheen epäonnistuneita tietoja. (Mutta tässä artikkelissa kuvata ongelmia kirjauduttaessa Azure RemoteApp-asiakasohjelman.)  

Lukea lisätietoja Yleiset virhesanomat sovelluksen käynnistys- ja yhteyden virheiden vuoksi.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Olemme tulee näyttöön, voit määrittää... Yritä uudelleen 10 minuuttia.

Tämä virhe tarkoittaa Azure RemoteApp on skaalaus kapasiteettitarve käyttäjät täyttävät. Taustalla Azure RemoteApp AM esiintymiä luodaan käsittelemään kapasiteetin käyttäjien tarpeisiin. Yleensä tämä kestää noin viisi minuuttia, mutta voi kestää jopa 10 minuuttia. Joskus tämä ei tapahdu, tarpeeksi nopeasti ja resursseja tarvitaan välittömästi. Esimerkki 9 AM skenaario, jossa monta käyttäjää on sovelluksen käyttämisestä Azure RemoteAppn samanaikaisesti. Jos näin käy Microsoft käyttöön uudelleen **kapasiteetin tilassa** . Voit tehdä tämän ja avaa Azure tuki lippu tai sähköpostia osoitteeseen [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Olla tiettyjä sisällytettävien tilauksen tunnus pyynnön.  

![Olemme jää määrittäminen](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Onnistunut automaattinen – muodosta, sovelluksiin uudelleen Käynnistä sovellus  

Jos käytät Azure RemoteApp ja ladata Tietokoneesi lepotilaan kauemmin kuin 4 tuntia ja herätti sitten tietokoneeseen ja Azure RemoteApp-asiakas yritä automaattinen on usein nähnyt tämä virhesanoma tulee näyttöön, yhdistää ja aikakatkaisu ylitettiin.  Kehota käyttäjiä, voit palata sovellus ja yritä avata Azure RemoteApp-asiakasohjelmassa.

![Onnistunut automaattinen-muodosta sovelluksiin](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Temp profiiliin liittyviä ongelmia 

Tämä virhe ilmenee, kun käyttäjäprofiilin (käyttäjän profiiliin levy) ei voitu ottaminen käyttöön ja käyttäjän saanut tilapäistä profiilia.  Järjestelmänvalvojat olisi Siirry Azure-portaalissa sivustokokoelman ja sitten Siirry **istunnot** -välilehti ja yritä **Kirjaudu ulos** käyttäjä. Tämä pakottaa koko lokia luettelosta käyttäjän - istunnon sitten on käyttäjä yrittää käynnistää sovelluksen uudelleen. Jos ongelma jatkuu Azure tuelta ja sähköpostia osoitteeseen [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp lakkasi toimimasta

Tämä virhesanoma tarkoittaa Azure RemoteApp-asiakasohjelman ongelma aiheuttaa ja on käynnistettävä. Opasta käyttäjiä sulkemaan: Valitse **Sulje ohjelma** ja käynnistä sitten Azure RemoteApp-asiakasohjelman uudelleen.  Jos ongelma edelleen Avaa ja Azure tuki lippujen ja sähköpostia osoitteeseen [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp lakkasi toimimasta](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Etätyöpöytäyhteys on käyttäminen tämän resurssin ilmeni virhe. Muodostaa yhteyden uudelleen tai ota yhteyttä järjestelmänvalvojaan

Tämä on yleinen virhesanoma – Azure tuelta ja sähköpostia osoitteeseen [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) , jotta voimme tutkia. 

![Yleinen Azure RemoteApp-viesti](./media/remoteapp-apptrouble/ra-apptrouble4.png) 