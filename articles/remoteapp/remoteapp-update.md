<properties
   pageTitle="Päivitä Azure RemoteApp-sivustokokoelman | Microsoft Azure"
   description="Opi päivittämään Azure RemoteApp-kokoelmaan"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Azure RemoteApp sivustokokoelman päivitys

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Tulossa väistämättä, kun haluat päivittää sovellukset tai -kuvan Azure RemoteApp-sivustokokoelman aika. Jos käytössäsi on jokin Azure RemoteApp-tilaukseen cloud- tai hybrid kokoelman kuuluva kuvat kaikista päivitykset hoitaa Azure RemoteApp itse, niin pidät helposti.

Jos käytössäsi on mukautettu kuva (joko laadittuihin alusta alkaen tai muokkaamalla Microsoftin kuvia luomasi), sinun on kuitenkin vastaava kuva ja sovellukset. Jos haluat päivittää kuvan tai sovelluksia sisällä, joudut luominen kuvan uusi, päivitetyn version ja korvaa kokoelmasta kuvaan uusi päivitetty kuvassa.

Näkyy kuinka käymään päivittämisessä kokoelmaa? On melko helppoa:

1. Päivitä kuva, jota käytit kokoelmasta. Käytä mitä tahansa korjaukset ja tarvittavat päivitykset ja tallentaa sen uudella nimellä.
2. [Ladata](remoteapp-uploadimage.md) tai [tuoda](remoteapp-image-on-azurevm.md) , joka kuvaa, niin RemoteApp.
3. Nyt sivustokokoelman sivulla, valitse **Päivitä**.
4. Valitse uusi kuva **Suunnittelumallin kuva** -luettelosta.
4. Seuraavassa on vaikeaa osa - sinun on määritettävä voit käsitellä kaikki käyttäjät, jotka on käytössä sovelluksen kokoelmasta. Sinulla on seuraavat vaihtoehdot:
    - **Myönnä käyttäjille 60 minuuttia päivityksen jälkeen**. Kun päivitys on valmis, Azure RemoteApp näyttää viestin aktiivinen käyttäjiä yrittäville Tallenna työssään ja kirjaudu ulos ja kirjaudu takaisin sisään. 60 minuutin kuluttua aktiiviset käyttäjät, joilla ei ole kirjautunut ulos kirjataan automaattisesti. Käyttäjät voivat kirjautua välittömästi takaisin sisään.
    - **Kirjaudu ulos käyttäjät heti**. Kun päivitys on valmis, kirjaudu ulos automaattisesti ilman varoituksia kaikille käyttäjille. Jos valitset tämän vaihtoehdon, käyttäjät saattavat menettää tietoja. Kuitenkin ne voit muodostaa yhteyden sovellus heti.

1. Napsauta Aloita päivitys valintamerkkiä.
