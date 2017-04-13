
<properties
    pageTitle="Vianmääritys RemoteApp cloud sivustokokoelmat - luominen | Microsoft Azure"
    description="Opettele RemoteApp cloud sivustokokoelman luominen virheiden vianmääritys"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Vianmääritys luominen RemoteApp cloud sivustokokoelmat

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Jos cloud sivustokokoelman luomisesta on ongelmia, Katso seuraavat tiedot.

## <a name="your-image-is-invalid"></a>Kuva ei kelpaa ##
Jos näet viestin, kuten "GoldImageInvalid" kun odotat Azure valmistelu kokoelmaa, se tarkoittaa, että suunnittelumallin kuva ei täytä [määritetty kuva vaatimukset](remoteapp-imagereqs.md). Siirry, lue näitä [vaatimuksia](remoteapp-imagereqs.md), korjaa kuvan ja yritä luoda sivustokokoelman uudelleen.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Azure hallinta-portaalin nähdä yleisten virheiden korjaaminen

    DNS server could not be reached
    ProvisioningTimeout

Cloud sivustokokoelmat usein virheiden vuoksi voit luonnin aikana käyttävät mukautetut kuvia.  Jos näet jommankumman edellä mainituista virheistä ja käytössäsi on mukautettu kuva Luo kokoelma, tarkista seuraavat kohdat:

- Varmista, että olet ladannut mukautetun kuvan täyttää kuva vaatimuksia.
- Useimmin yleinen ongelma on, että kuva ei ole oikein Sysprep.  
- Tarkista kuvan voit käynnistää Hyper-V kuluessa tai yritä luominen IAAS AM suoraan Azure tilauksen käyttämällä kuvaa. Jos AM epäonnistuu käynnistys ja ei käynnisty, valitse tämä tarkoittaa yleensä, että mukautettu kuva ei ollut valmisteltu oikein.  Tarkista mukautettu kuva on luotu seuraavan miten voit luoda mukautetun mallin kuva RemoteApp

Jos käytössäsi on jokin tilaukseen sisältyy Microsoft kuvista, yritä luoda sivustokokoelman uudelleen. Jos ongelma jatkuu sitten Ota yhteyttä Microsoftin tuotetukeen.

    PlatformImageTrialModeOnly

Jos saat virheilmoituksen Tämä yleensä tarkoittaa, joka on päivitetty maksettu tiliin, mutta yrität käyttää Microsoftin tarjoamista kuva, joka on voimassa vain palvelun kokeiluversion tilan aikana. Tässä tapauksessa yritä luoda cloud sivustokokoelman uudelleen, mutta muista Määritä oikea kuva.
