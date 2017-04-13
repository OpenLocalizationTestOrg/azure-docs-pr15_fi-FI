<properties
    pageTitle="Luo Azure RemoteApp-kuva | Microsoft Azure"
    description="Lisätietoja kuvien luominen Azure RemoteApp asetuksista"
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



# <a name="create-an-azure-remoteapp-image"></a>Luo Azure RemoteApp-kuva

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp käyttää kuvia pitämällä sovellukset, jotka jaettavaksi käyttäjien kanssa. (On otettava kuvan ja luoda VMs -, joka on mitä käyttäjät voivat käyttää, kun ne kirjautua Azure RemoteApp.) Jos haluat luoda Azure RemoteApp-sivustokokoelman valintasi sovellukset, olipa cloud tai hybrid, voit aloittaa luomalla kuvan asennetut sovellukset. Luo sitten kokoelma, joka käyttää kyseiseen kuvaan, käyttäjien määrittäminen kokoelma ja julkaista sovellusten kyseisille käyttäjille.

Sinulla on useita vaihtoehtoja luomisen ja kuvien käyttämisestä. Basic [vaatimus](remoteapp-imagereqs.md) kuva on, että se suorittaa Windows Server 2012 R2 ja on asennettu Remote Desktop istunnon Host (RDSH) rooli. Miten saat, joka on mistä asiat kiinnostavat.

Sinulla on seuraavista vaihtoehdoista, kun kuvat on:

- Voit tuoda ja käyttää [Kuva Azure virtuaalikoneen perusteella](remoteapp-image-on-azurevm.md). Tämä on hyvä liiketoiminta-sovellukset, jotka on käytettävä mukautettuja asetuksia. Voit mukauttaa toimimaan sovelluksen kuva.
- Voit [luoda ja lataa mukautetun kuvan](remoteapp-create-custom-image.md). Tämä on hyvä, jos sinulla on jo kuvaksi, jota käytetään paikallisen Etätyöpöytäpalvelut-käyttöönottoa varten.
- Voit käyttää jokin RemoteApp-tilaukseen sisältyvien [sivustomallien kuvia](remoteapp-images.md) . Kuvien luodaan ja ylläpitämä RemoteApp-ryhmän ja sisältää vakio sovelluksia (kuten Office-ohjelmistopaketti), voit tehdä käytettävissä käyttäjille. Huomaa, että vain Office 365 Pro Plus-kuvaa voi käyttää tuotannon-asetuksen.

Riippumatta siitä, mistä kuva tai miten voit luoda kannattaa Varmista, että tiedät, jotta sovelluksesi toimii hyvin RemoteApp- [sovelluksen vaatimukset](remoteapp-appreqs.md) . Valitse Seuraava vaihe on [cloud](remoteapp-create-cloud-deployment.md) tai [hybrid](remoteapp-create-hybrid-deployment.md) sivustokokoelman luomiseen.
