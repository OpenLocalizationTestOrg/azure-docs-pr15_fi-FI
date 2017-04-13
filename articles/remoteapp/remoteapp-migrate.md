
<properties
    pageTitle="Siirtää käyttäjätiedot Azure RemoteApp | Microsoft Azure"
    description="Opi siirtämään Azure RemoteApp-ja uloskirjautuminen käyttäjätiedot."
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



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Tietojen siirtäminen sisään ja ulos Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Voit käyttää useita eri työkaluja ja menetelmiä [Käyttäjätietojen](remoteapp-upd.md) siirtäminen sisään ja ulos Azure RemoteApp. Seuraavassa on joitakin tapoja:

- Kopioiminen ja sijoittaminen käyttämällä Leikepöytää jakaminen
- Kopioi tiedostot ja tiedot tiedostopalvelimeen
- Tiedostojen kopioiminen OneDrive for Business-selaimen kautta
- Kopioi tiedostot uudelleenohjaus

>[AZURE.NOTE] 
> Et voi ottaa käyttöön OneDrive for Business- tai kuluttaja synkronointi tekijöiden - ne Azure RemoteApp [ei tueta](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopioimalla ja liittämällä Resurssienhallinnassa

Kopioi ja liitä käyttämällä Leikepöytää on käytössä RemoteApp ominaisuuksissa [oletusarvoisesti](remoteapp-redirection.md). Näillä oikeuksilla käyttäjät kopioida tiedostoja niiden paikallisen tietokoneen ja RemoteApp-sovellusten välillä. Usein – tavanomaiseen-sovellusten käyttäminen RemoteApp-käyttäjien tallennettuja tiedostoja niiden UPDs - siirtäminen pois RemoteApp tietoja on helppo:

1. RemoteApp kokoelman [Julkaiseminen sovelluksena Resurssienhallinnassa](remoteapp-publish.md) . (Huomaa, että se hallintatehtävän.)
2. Suora käyttäjiä Käynnistä julkaisit Resurssienhallinnan avulla, kopioida ja liittää tiedostoja sekä niiden UPD ja siitä pois.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Lataa tiedostot ja tiedot tiedostopalvelimeen käyttämällä vakio verkon tiedostojen kopioiminen

Usein organisaatiot tallentaa yleisiä tietoja tiedostopalvelimien avulla. Jos tiedät palvelimen nimeä tai sijaintia, käyttäjien voit selata palvelimen lähiverkossa ja kopioi sinne, niiden tiedostoja samalla tavalla kuin samalla yläpuolella. Haluat uudelleen Resurssienhallinnan julkaiseminen RemoteApp ja jaa se sitten käyttäjien kanssa.

>[AZURE.NOTE] 
> Tiedostopalvelin on oltava reititettävä verkossa, RemoteApp on otettu käyttöön kyselyjä.

## <a name="copy-files-to-onedrive-for-business"></a>Tiedostojen kopioiminen OneDrive for Business
Vaikka et voi ottaa käyttöön OneDrive for Businessin synkronointi agentti RemoteApp, voit edelleen tiedostoja kopioidaan oman UPD onedrive for Business-selaimen kautta. 

1. Julkaise Resurssienhallinnassa RemoteApp ja kerro käyttäjät voivat käyttää tiedostoja, sovelluksen kautta. 
2. Se on helpointa tiedostojen siirtäminen Jos ne on pakattu, joten käyttäjiä kannattaa luoda .zip-tiedosto, joka sisältää kaikki tiedostot siirretään onedrive for Business.
3. Pyydä käyttäjiä, siirry Office 365-portaaliin ja siirry OneDrive ja lataa .zip-tiedosto.

## <a name="copy-files-by-using-drive-redirection"></a>Tiedostojen kopioiminen asema uudelleenohjaus avulla

Jos olet ottanut [asema uudelleenohjausta](remoteapp-redirection.md), olet jo luonut käyttäjien jaettuun verkkoasemaan. Tässä tapauksessa ne zip-tiedostonsa uudelleenohjatun aseman ja tallentaa ne sitten niiden paikalliseen tietokoneeseen.