<properties
   pageTitle="Palvelun kangasta paikallisen klusterin asetusten vianmääritys | Microsoft Azure"
   description="Tässä artikkelissa käsitellään joukko ehdotuksia paikallista kehittämistä klusterin vianmääritys"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Paikallista kehittämistä klusterin-asennuksen vianmääritys

Jos ongelma tulla vastaan käyttäminen paikallisen Azure palvelun kangasta kehittäminen-klusterin, tarkista seuraavat ehdotukset mahdollisia ratkaisuja.

## <a name="cluster-setup-failures"></a>Klusterin asennus epäonnistuu

### <a name="cannot-clean-up-service-fabric-logs"></a>Et voi tyhjentää palvelun kangasta lokit

#### <a name="problem"></a>Ongelma

Esitettävän DevClusterSetup komentosarja näet virhesanoman tältä:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Ratkaisu

Sulje nykyinen PowerShell-ikkuna ja Avaa uusi PowerShell-ikkuna järjestelmänvalvojan oikeuksilla. Voit nyt pitäisi käyttää komentosarja on suoritettu onnistuneesti.

## <a name="cluster-connection-failures"></a>Klusterin epäonnistuneita

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Palvelun kangasta PowerShellin cmdlet-komennot eivät tunnista PowerShellin Azure

#### <a name="problem"></a>Ongelma

Jos yrität suorittaa jonkin palvelun kangasta PowerShell cmdlet-komentoja, kuten `Connect-ServiceFabricCluster` PowerShellin Azure-ikkunassa se epäonnistuu, jossa sanotaan, että cmdlet ei tunnisteta. Tämä on PowerShellin Azure käyttää 32-bittinen versio Windows PowerShellin (myös niissä 64-bittinen käyttöjärjestelmä), kun taas palvelun kangasta cmdlet-komennot toimivat vain 64-bittinen ympäristöissä.

#### <a name="solution"></a>Ratkaisu

Palvelun kangasta cmdlet suoritetaan aina suoraan Windows PowerShell.

>[AZURE.NOTE] PowerShellin Azure uusimman version niin tämä suoritetaan ei enää ole määräten pikakuvakkeen luominen.

### <a name="type-initialization-exception"></a>Kirjoita alustus poikkeuksen

#### <a name="problem"></a>Ongelma

Kun olet muodostamassa yhteyttä klusterin powershellissä, näkyviin tulee virhe TypeInitializationException System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Ratkaisu

Path-muuttujaan ei ole määritetty oikein asennuksen aikana. Kirjaudu ulos Windows ja kirjaudu takaisin sisään. Tämä täysin päivitys path.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Klusterin yhteys epäonnistuu "Objekti on suljettu"

#### <a name="problem"></a>Ongelma

Yhdistä ServiceFabricCluster kutsu epäonnistuu, ja järjestelmä antaa virheilmoituksen tältä:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Ratkaisu

Sulje nykyinen PowerShell-ikkuna ja Avaa uusi PowerShell-ikkuna järjestelmänvalvojan oikeuksilla. Tiedostojen pitäisi nyt näkyä muodostaa.

### <a name="fabric-connection-denied-exception"></a>Kangasta yhteyden estetty poikkeuksen

#### <a name="problem"></a>Ongelma

Jos virheenkorjaus Visual Studio, näyttöön tulee FabricConnectionDeniedException-virhe.

#### <a name="solution"></a>Ratkaisu

Tämä virhe ilmenee yleensä käsitellessäsi yritä yritä Aloita palvelun host manuaalisesti sen sijaan, että salliminen palvelun kangasta runtime Käynnistä se puolestasi.

Varmista, että sinulla ei ole palvelun projekteja käynnistys projektit-ratkaisun määrittäminen. Vain palvelun kangasta sovelluksen projekteja on asetettava käynnistys projekteja.

>[AZURE.TIP] Jos seuraavat asetukset paikallisen klusterin alkaa toimivat normaalisti, voit palauttaa sen paikallisen klusterin hallinta järjestelmän ilmaisinalueen-sovelluksen avulla. Tämä poistaa aikaisemmin luodun klusterin ja määritä uuden. Huomaa, että kaikki sovellukset käyttöön ja niihin liittyvät tiedot poistetaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietoja ja järjestelmän kunto-raportteja sisältäviä yhteyttä klusterin vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Visualisoi yhteyttä klusterin kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md)
