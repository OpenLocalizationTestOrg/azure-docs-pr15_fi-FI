<properties
    pageTitle="Azure RemoteApp käyttöoikeudet | Microsoft Azure"
    description="Katso, miten käyttöoikeudet tapahtuu Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Miten käyttöoikeudet toimii Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Niin, että olet määrittänyt Azure RemoteApp-palvelussa, luoda malleja, ja olet valmis julkaisemaan sovelluksia käyttäjille. Mutta on edelleen viimeisen kuitenkaan laskemisesta: käyttöoikeudet. Vain mistä RemoteApp ja RemoteApp jaettujen sovellusten käyttöoikeuksien myöntämistä työn?

RemoteApp edellytä Windows-käyttöoikeuksia tai Remote Desktop CALs. Tilauksesi on huolellisesti RemoteApp puolen itse. (Katso tietoja [hinnoittelua suunnitelmien](https://azure.microsoft.com/pricing/details/remoteapp).)

Jos käytät jotakin kuvista, joka sisältyy tilauksen, voit jakaa sovelluksia asennettuihin kyseiseen kuvaan, eikä hänen nimeään tarvitse erillistä lisenssiä. Esimerkiksi jos käytät Windows Server 2012 R2-suunnittelumallin kuva luonnissa kokoelma, voit jakaa System Center Endpoint Protection käyttäjien kanssa. Sääntöön poikkeuksia on Office 365 ProPlus, jossa pyydetään tilattu ja Office 2013, joka ei voi jakaa tuotannon kokoelman.

Jos haluat käyttää Office 365-suunnittelumallin kuva, joka sisältää RemoteApp, sinulla on *Office 365 ProPlus suunnitelman* . Sama koskee kaikkia Office 365-sovelluksia, voit julkaista mukautetun mallin avulla. Sinun on aktivoitava oman tilauksen sovellukset. Tämä pätee kokeiluversio- ja maksullisia tilaukset. Jos haluat käyttää Office 365-suunnittelumallin kuva aikana kokeiluversio *ja sinulla ei vielä ole tilauksen*, siirry Office 365-sivun kokeiluversioon [Rekisteröidy](https://go.microsoft.com/fwlink/p/?LinkID=403802) . Katso [miten RemoteApp ja Officen toimivat yhdessä?](remoteapp-o365.md) lisätietoja.

Jos kokeilujakson aikana et halua halutun Office 365-kokeilutilauksen, voit käyttää Office 2013 Professional Plus mallin kuvaa, joka sisältyy RemoteApp. Tämän mallin kuvaa voi käyttää vain 30 päivän ja ei voi muuntaa maksettu sivustokokoelman.

Muista sovelluksista sinun on varmistaaksesi, että sinulla on käyttöoikeus sovelluksen jakaminen.

Joka on järkevää, oikea? Voit julkaista sovellusta, joka on lain mukaan oikeutettu jakaminen. Eikä sinun tarvitse Varmista, että todella ovat oikeutettuja ohjelmien jakaminen.

Huomaa, että et voi käyttää käyttö- tai volyymikäyttöoikeus sopimuksen cloud-sivustokokoelman. Äänenvoimakkuuden käyttöoikeussopimus avulla *Voit* käyttää Aktivoi hybrid-sivustokokoelman (lukuun ottamatta Office)-sovelluksia. Haluat asentaa ne mallin kuva volyymikäyttöoikeus-tietovälineestä. Noudata tiedot sovelluksen asentamiseksi käyttöoikeuksien etätyöpöydän ympäristössä toimittajalta.
