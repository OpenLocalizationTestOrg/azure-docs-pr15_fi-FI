<properties
    pageTitle="Julkaise sovelluksen Azure RemoteApp | Microsoft Azure"
    description="Lue, miten voit julkaista Azure RemoteApp sovellukset ja resurssit."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Julkaiseminen sovelluksen RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Kun olet luonut RemoteApp-sivustokokoelman, sinun täytyy julkaista sovelluksia tai resursseja, jonka haluat käyttäjien käytettävissä. Sivustomallien kuvia, tilauksen mukana on vain muutaman sovellukset, jotka on julkaistu oletusarvoisesti - jakaminen muista sovelluksista, sinun pitää julkaista ne.

> [AZURE.NOTE] Tarvitseeko voi päivittää? Sinun on päivitettävä [kuvan](remoteapp-update.md) ensin.

Valitse **Julkaise** -välilehdestä-portaalissa, **Julkaise**. Voit lisätä sovelluksen suunnittelumallin kuva **Käynnistä** -valikosta tai polku, jossa sovellus on asennettu suunnittelumallin kuva. Jos haluat lisätä **Käynnistä** -valikko, valitse poistettava sovellus luettelosta julkaisemaan. Jos valitset sovellukseen polun, kirjoita nimi sovelluksen ja polku-sovellukseen. Käytä sen sijaan, että muuttujat polun – esimerkiksi "%SYSTEMDRIVE%\Program" "c:\".

> [AZURE.NOTE] Jos haluat lisätä sovelluksen **Käynnistä** -valikosta, tarvitset *lisätään kyseiseen sovellus * *Käynnistä* * suunnittelumallin Kuva-valikon.* Muussa tapauksessa RemoteApp näkevät vain mitä *on* **Käynnistä** -valikon ja voi. 

>Varmista, että sovelluksesi sijaitsee **Käynnistä** -valikko, sijoittaa pikakuvaketiedosto - **.lnk** - %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs-kansioon.

> Jos olet unohtanut-sovelluksen lisääminen **Käynnistä** -valikko, kun olet luonut mallia, valitse polku lisääminen sovellukseen. (Tai luo suunnittelumallin kuva, mutta tämä on aivan hieman enemmän työtä.)


 