<properties
   pageTitle="Määritä palvelun kangasta sovelluksen päivitys | Microsoft Azure"
   description="Opettele määrittämään asetuksia päivittämiseen palvelun kangasta-sovelluksen avulla Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Määritä Service kangasta sovelluksen päivitys Visual Studiossa

Visual Studio työkalut Azure palvelun kangasta tukevat päivityksen paikallisessa tai etätietokoneessa klustereiden julkaiseminen. On kaksi päivittäminen uudempaan versioon sijaan korvaaminen sovellusta aikana testaus- ja sovelluksen etuja:

- Sovelluksen tietoja ei menetetä päivityksen aikana.
- Käytettävyys säilyy suuri, joten ei ole mitään palvelukatkos päivityksen aikana Jos ei riitä palveluesiintymiä levitä päivityksen toimialueilla.

Testit voidaan suorittaa vastaan sovelluksen samalla, kun se päivitetään.

## <a name="parameters-needed-to-upgrade"></a>Parametreja tarvitaan päivittäminen

Voit valita kahdentyyppisiä käyttöönoton: Normaali tai päivitetään. Tavallinen käyttöönoton poistaa kaikki edellisen asennustiedot ja tietojen klusterin, samalla, kun päivityksen käyttöönoton säilyttää sen. Kun päivität palvelun kangasta-sovelluksen Visual Studiossa, sinun täytyy antaa sovelluksen päivityksen parametrit- ja kuntotietojen tarkistaminen käytännöt. Sovelluksen päivitys parametrit auttaa päivityksen aikana kuntotietojen tarkistus käytännöt määrittävät päivitys onnistui. Katso [palvelun kangasta sovelluksen päivitys: parametrien päivitystä](service-fabric-application-upgrade-parameters.md) lisätietoja.

On päivityksen kolmessa: *Tarkkailtavat*, *UnmonitoredAuto*ja *UnmonitoredManual*.

  - Tarkkailtavat päivityksen automatisoi päivitys ja sovelluksen kuntotietojen tarkistus.

  - UnmonitoredAuto päivityksen automatisoi päivitys, mutta ohittaa sovelluksen kuntotietojen tarkistus.

  - Kun teet UnmonitoredManual päivityksen, sinun täytyy päivittää manuaalisesti jokaisen päivityksen toimialueen.

Kunkin päivitystilan edellyttää erilaisia arvojoukkoja parametrit. Katso lisätietoja päivityksen vaihtoehtoihin [sovelluksen päivittäminen parametrit](service-fabric-application-upgrade-parameters.md) .

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Päivittää palvelun kangasta sovelluksen Visual Studiossa

Jos haluat päivittää palvelun kangasta sovelluksen Visual Studio palvelun kangasta Työkalut avulla, voit määrittää julkaisuprosessin on päivityksen säännöllisesti käyttöönoton sijaan valitsemalla **Päivitä sovellus** -valintaruutu.

### <a name="to-configure-the-upgrade-parameters"></a>Päivityksen parametrien määrittäminen

1. Valitse **asetukset** -painikkeen vieressä oleva valintaruutu. **Muokkaa päivittäminen parametrit** -valintaikkuna tulee näkyviin. **Muokkaa päivittäminen parametrit** -valintaikkunasta tukee Tarkkailtavat, UnmonitoredAuto ja UnmonitoredManual päivityksen tila.

2. Valitse Päivitä tila, jota haluat käyttää ja täytä sitten parametrin ruudukon.

    Kunkin parametrin on oletusarvot. Valinnaisten parametrien *DefaultServiceTypeHealthPolicy* kestää hash-taulukon tietoja. Tässä on esimerkki *DefaultServiceTypeHealthPolicy*muodosta hash taulukko-syötteen:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* on toisen valinnaisten parametrien, joka vie muodossa hash-taulukon tietoja:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Seuraavassa on esimerkki käytännön:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Jos valitset UnmonitoredManual päivitystilan, sinun on käynnistettävä manuaalisesti PowerShell console, voit jatkaa ja valmis päivitysprosessin. Viitata [palvelun kangasta sovelluksen päivitys: advanced aiheet](service-fabric-application-upgrade-advanced.md) kerrotaan, miten manuaalinen päivitys toimii.

## <a name="upgrade-an-application-by-using-powershell"></a>Sovelluksen päivittäminen PowerShell-toiminnolla

Voit päivittää palvelun kangasta sovelluksen PowerShellin cmdlet-komennot. [Päivitä opetusohjelma palvelun kangasta-sovellus](service-fabric-application-upgrade-tutorial.md) ja [Käynnistä ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) saat yksityiskohtaiset tiedot.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Määritä kuntotietojen tarkistus-käytännön luominen tiedostojen sovelluksen-tiedostoon

Jokaisella palvelulla palvelun kangasta-sovelluksessa voi olla oma kunto parametrit, jotka ohittavat oletusarvot. Voit kirjoittaa sovelluksen luettelon tiedoston kyseiset parametriarvot.

Seuraavassa esimerkissä esitetään, miten voit määrittää yksilöllinen kuntotietojen tarkistus käytännön sovelluksen luettelo kunkin palvelun.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja sovelluksen käyttöönotto [käyttöönotto aiemmin luodun sovelluksen Azure palvelun kangasta](service-fabric-deploy-existing-app.md).
