<properties
    pageTitle="Tietojen siirtäminen ja Azure säilöstä | Microsoft Azure"
    description="Tässä artikkelissa on yleisiä tietoja eri tavoista, tietojen siirtäminen ja Azure-tallennustilan."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Tietojen siirtäminen ja Azuren tallennustilaan

Jos haluat siirtää paikalliset tiedot Azuren tallennustilaan (tai päinvastoin), sinun on useilla eri tavoilla toiminto. Tapa, joka sinulle parhaiten riippuu käyttämässäsi skenaariossa. Tässä artikkelissa tarjoaa nopean yleiskuvan eri skenaarioissa ja sopiva palveluja kullekin.

## <a name="building-applications"></a>Sovellusten

Jos luot sovelluksen, kehittäminen REST-Ohjelmointirajapinnalla tai useita Microsoftin asiakas-kirjastoihin on ansiosta voit siirtää tietoja ja sieltä pois Azure-tallennustilan.

Azure-tallennustilan tarjoaa rich client-kirjastojen .NET, iOS, Java, Android, yleinen Windows Platform (UWP), Xamarin, C++, Node.JS, PHP, Ruby ja Python. Asiakas-kirjastojen tarjouksen lisäasetusten ominaisuudet, kuten uudelleen logiikan, kirjaaminen ja rinnakkaisia lataukset. Voit myös kehittää suoraan vastaan REST-Ohjelmointirajapinta, joita voidaan kutsua kielen, joka tekee HTTP/HTTPS-pyyntöjen perusteella.

Katso lisätietoja [Azure-Blob-säiliö käytön aloittaminen](storage-dotnet-how-to-use-blobs.md) .

Lisäksi myös Tarjoamme [Azure tallennustilan tietojen siirtämistä kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) , joka on tarkoitettu tietojen kopioiminen tehokas ja sieltä pois Azure kirjastoon. Katso lisätietoja Microsoftin tietojen siirto-kirjaston [asiakirjat](https://github.com/Azure/azure-storage-net-data-movement) . 

## <a name="quickly-viewinginteracting-with-your-data"></a>Nopeasti tarkasteleminen ja käsitteleminen tiedot

Jos haluat tarkastella Azuren tallennustilaan tietoja ja ottaa myös tietojen lataaminen onedriveen ja mahdollisuus helposti, harkitse sitten Azure-tallennustilan Resurssienhallinnan avulla.

Tutustu luettelossamme lisätietoja [Azure-tallennustilan hallinnasta](storage-explorers.md) .

## <a name="system-administration"></a>Järjestelmän hallinta

Jos vaadi tai ovat sujuvammin komentorivin apuohjelmalla (kuten järjestelmänvalvojat), seuraavat on useita vaihtoehtoja, jotka on otettava huomioon:

### <a name="azcopy"></a>AzCopy

AzCopy on tarkoitettu tietojen kopioiminen tehokas ja sieltä pois Azuren tallennustilaan Windows komentorivin apuohjelma. Voit myös kopioida tietoja tallennustilan tilin tai tallennustilan eri tilien välillä.

Katso lisätietoja [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell on moduuli, joka sisältää palveluiden Azure cmdlet-komennot. On tehtävä-pohjainen komentorivin runko ja suunniteltu erityisesti järjestelmänvalvontaan kieltä.

Katso lisätietoja [Azure PowerShellin Azure-tallennustilan kanssa](storage-powershell-guide-full.md) .

### <a name="azure-cli"></a>Azure CLI

Azure CLI on joukko Avaa lähde Office kaikissa ympäristöissä komentoja, joiden toimimasta Azure-palvelujen kanssa. Azure CLI on saatavana Windows OS x ja Linux.

Katso lisätietoja [Azure CLI Azuren tallennustilaan kanssa käyttämällä](storage-azure-cli.md) .

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Siirtäminen suurten tietomäärien ja verkkoyhteys on hidas

Yhden liittyvän suuria tietomääriä suurimmista haasteita on Siirtoaika. Jos haluat hakea tietoja ja ulkoisesta Azuren tallennustilaan ilman katkeamisesta verkkojen kustannukset tai kirjoittaa koodin, Azure Tuo/Vie on sopivan ratkaisun.

Katso lisätietoja [Azure Tuo/Vie](storage-import-export-service.md) .

## <a name="backing-up-your-data"></a>Tietojen varmuuskopiointi

Jos haluat ainoastaan Azure-tallennustilan tietojen varmuuskopioinnin tapa siirtyä on Azure varmuuskopion. Tämä on tehokas ratkaisu paikalliset tiedot ja Azure VMs varmuuskopioiminen.

Katso lisätietoja [Azure varmuuskopion](../backup/backup-introduction-to-azure-backup.md) .

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Oman tietoja paikallisen käyttäminen ja pilvestä

Jos tarvitset ratkaista käyttämiseen oman tietoja paikallisen ja pilvestä, valitse Ota huomioon Azure's hybrid cloud tallennustilan ratkaisu, StorSimple avulla. Tämä ratkaisu koostuu fyysistä StorSimple laitetta älykkäästi stores usein käytetyt tiedot SSD, että toisinaan käyttää tietojen HDDs ja passiiviset/varmuuskopiointia tai arkistointia tietojen näyttämisen Azuren tallennustilaan.

Katso lisätietoja [StorSimple](../storsimple/storsimple-overview.md) .

## <a name="recovering-your-data"></a>Tietojen palauttaminen

Kun käytössä on paikallisen toiminnoista ja sovelluksia, sinun on ratkaisu, joka sallii yrityksesi Jatka Jos huono. Azure palauttaminen käsittelee replikoinnin automaattisesti ja näennäiskoneiden ja fyysiset palvelimet. Replikoitua tiedot tallennetaan Azuren tallennustilaan, jonka avulla voi poistaa toissijaisia paikalla palvelinkeskuksen edellyttää.

Katso lisätietoja [Azure palauttaminen](../site-recovery/site-recovery-overview.md) .
