
<properties
    pageTitle="Azure RemoteApp kuva vaatimukset | Microsoft Azure"
    description="Lisätietoja kuvien luomiseen, jota käytetään Azure RemoteApp vaatimukset"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Azure RemoteApp kuvia vaatimukset

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp käyttää Windows Server 2012 R2-kuva isännöimiseen kaikki ohjelmat, jotka haluat jakaa käyttäjien kanssa. Voit luoda mukautetun kuvan, voit aloittaa olemassa olevan kuvan tai [luoda uuden](remoteapp-create-custom-image.md).

> [AZURE.TIP] Tiesitkö, että Azure RemoteApp-tilauksesi avulla voit käyttää Windows Server 2012 R2-kuva, jonka avulla voit luoda oman mallin kuva Azure AM valikoiman? [Tarkista asia](remoteapp-image-on-azurevm.md).  


Kuva, joka voi ladata ja käyttää siinä Azure RemoteApp vaatimukset ovat seuraavat:


- Mukautetut sovellukset Älä tallenna tiedot paikallisesti kuva. Nämä kuvat ovat tilattomien ja saa sisältää vain sovellukset.
- Kuva ei sisällä tietoja, jotka voidaan menettää.
- Kuvan koko on oltava MBs kerrannaiseen. Jos yrität ladata kuvan, joka ei ole on jaollinen, lataus epäonnistuu.
- Kuvan koko on oltava 127 gt tai Pienennä.
- Sen on oltava .vhd-tiedosto (VHDX tiedostoja ei tällä hetkellä tueta).
- Näennäiskiintolevyn ei saa olla luonti 2 virtual koneen.
- Näennäiskiintolevyn voi olla kiinteä tai dynaamisesti laajennettavassa. Dynaamisesti laajennettavassa Näennäiskiintolevyn on suositeltavaa, sillä kuluu vähemmän aikaa, voit ladata Azure kuin kiinteä Näennäiskiintolevytiedosto.
- Levy on valmistellaan käyttämällä perustyyli käynnistyksen tietueen (Pääkäynnistystietue) jakaminen tyyli. GUID-tunnus osion (GPT) osion Taulukkotyyli ei tueta.
- Näennäiskiintolevyn on oltava yksittäinen Windows Server 2012 R2-asennus. Se voi olla useita asemat, mutta vain yksi, joka sisältää Windowsin asennuksen.
- Remote Desktop istunnon Host (RDSH)-roolin ja Desktop Experience-toiminto on oltava asennettuna.
- Remote Desktop yhteyksien välittäjä rooli on *ei* voitu asentaa.
- Salaaminen File System (EFS) on poistettava käytöstä.
- Kuvan täytyy olla käyttäen parametreja Sysprep **/oobe / generalize/shutdown** (Älä käytä **/mode:vm** -parametri).
- Ladata tilannevedoksen yhdistettyjen oman Näennäiskiintolevyn ei tueta.

Katso lisätietoja kuvien luominen Azure RemoteApp [luominen Azure RemoteApp-kuva](remoteapp-imageoptions.md) .
