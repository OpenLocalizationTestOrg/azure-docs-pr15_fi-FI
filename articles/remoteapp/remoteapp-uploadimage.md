
<properties
    pageTitle="Mukautetun kuvan ladata Azure RemoteApp | Microsoft Azure"
    description="Mukautetun kuvan lataaminen Azure RemoteApp varten"
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
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Mukautetun kuvan lataaminen Azure RemoteApp varten

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Nyt kun olet luonut mukautetun mallin kuva tai päivittänyt muutosten kanssa, sinun on ladattava kyseiseen kuvaan Azure RemoteApp kuvakirjastossa. Näiden ohjeiden avulla.


## <a name="before-you-start"></a>Ennen aloittamista

1.      Tarkista, mukautettu kuva täyttää [Kuva-vaatimukset](remoteapp-imagereqs.md) ja [sovelluksen vaatimukset](remoteapp-appreqs.md).
2.      [PowerShellin Azure-moduulin](../powershell-install-configure.md)asentaminen.

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Step by step-mukautetun kuvan lataaminen

1.      Avaa Azure hallinta-portaalin ja RemoteApp-sivulle.
2.      Valitse **sivustomallien kuvia** -välilehdessä **Lataa** sivun alareunassa.
4.      Kirjoita kutsumanimi kuva ja määritä tili tallennuspaikka. Varmista sijainti on RemoteApp-sivustokokoelman samaan sijaintiin tai sijainti, johon haluat luoda.
5.      Kun sinulta kysytään, Lataa komentosarja paikalliseen tietokoneeseen.
6.      Kopioi komennon parametrit tekstiruutuun Leikepöydän.
7.      Avaa järjestelmänvalvojana Windows PowerShell-ikkuna.
8.      Siirry samaan kansioon, johon olet ladannut komentosarja järjestelmänvalvojana Windows PowerShell-ikkunassa.
9.      Liitä kopioitu komento ja paina **Enter**-näppäintä.

    Lataus alkaa ja keston saattavat riippua monet eri tekijät, kuten verkon nopeuden ja kuvan koko

11.    Jos lataus ei onnistu vuoksi verkon keskeyttäminen tai kohteita kuin, voit palauttaa aina kun aloitit lataus. Jatka lataus suorittamalla komentosarja uudelleen käyttämällä samalle riville.

> [AZURE.WARNING] Muokkaa koskaan Lataa komentosarja. Varmista, että kuva täyttää kuva-vaatimukset ja sovelluksen vaatimukset on toteutettu tiettyjä tarkistuksia.

## <a name="common-problems"></a>Yleisiä ongelmia

- Varmista, että käytät Windows PowerShellin Azure PowerShell. Sinun täytyy asentaa PowerShellin Azure-moduulissa, koska tietyt moduulit tarvitaan latauksen aikana.
- Muuta ei koskaan komentosarja-vahvistukset onko käytön helpottamiseksi.
- Jos Näennäiskiintolevytiedosto ei pääse kirjautumaan sisään lataamisen aikana, kopioi tiedosto tai siirtää sen uuteen sijaintiin ja yritys Lataa uudelleen. Voi olla jokin Windows-prosessi, joka estää Lataa.  
