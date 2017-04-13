<properties
    pageTitle="Uusi web-palveluun käyttämällä tietokoneen Learning hallinta PowerShellin cmdlet-komennot suuntaamista | Microsoft Azure"
    description="Opettele ohjelmallisesti suuntaamista mallin ja Päivitä juuri koulutetun mallin käyttäminen Azure koneen Learning koneen Learning hallinta PowerShellin cmdlet-komentojen käyttäminen WWW-palvelun."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Suuntaamista uuden web-palvelun avulla tietokoneen Learning hallinta PowerShellin cmdlet-komennot

Kun suuntaamista uusi web-palvelu, voit päivittää viittaamaan uuden koulutetun mallin ennakoivan web service-määritys.  

## <a name="prerequisites"></a>Edellytykset

On määritettävä koulutus kokeessa ja ennakoivan kokeen mukaisesti [suuntaamista koneen Learning mallit ohjelmallisesti](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Ennakoivan kokeen on otettava käyttöön Azure Resurssienhallinta (uusi) perusteella konepohjaisten oppimistekniikoiden verkkopalvelun nimellä. 
 
Saat lisätietoja käyttöönotto-verkkopalvelut [käyttöönotto Azure koneen Learning web-palveluun](machine-learning-publish-a-machine-learning-web-service.md).

Tämä prosessi edellyttää, että olet asentanut Azure koneen Learning cmdlet-komennot. Asentaminen tietokoneeseen Learning cmdlet-komennot lisätietoja [Azure koneen Learning cmdlet](https://msdn.microsoft.com/library/azure/mt767952.aspx) -viittaus MSDN-sivuston.

Kopioi uudelleenkoulutuksen tulosteen seuraavat tiedot:

* BaseLocation
* RelativeLocation

Vaiheet ovat seuraavat:

1.  Kirjaudu tiliisi Azure Resurssienhallinta.
2.  Saat WWW-palvelun määritys
3.  Vie JSON WWW-palvelun määritys
4.  Päivitä JSON ilearner-blob viittaus.
5.  JSON tuominen Web-palvelu-määritys
6.  Päivitä uuden WWW-palvelumäärityksen web-palvelu

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Azure Resurssienhallinta-tiliisi kirjautuminen

Sinun on kirjauduttava Azure tiliisi PowerShell-ympäristössä, [Lisää AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komennolla.

## <a name="get-the-web-service-definition"></a>Saat WWW-palvelun määritys

Saat WWW-palvelun seuraavaksi soittamalla [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komento. Web-palvelu määritelmä on WWW-palvelun koulutetun mallin sisäinen esittäminen ja ei voi muokata suoraan. Varmista, että haet WWW-palvelun määritys ennakoivan kokeessa ja ei koulutus kokeen.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Aiemmin luodun WWW-palvelun resurssin ryhmän nimen määrittämiseen suorittamalla Get-AzureRmMlWebService cmdlet-komento ilman WWW-palvelut näkyvät tilauksen parametreja. Etsi web-palvelu ja katso sen WWW-palvelun tunnus. Resurssiryhmän nimi on tunnus, neljännen osan vain *resourceGroups* elementin jälkeen. Seuraavassa esimerkissä resurssin ryhmänimi on oletusarvo-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Vaihtoehtoisesti voit määrittää aiemmin luodun WWW-palvelun resurssiryhmän nimi, kirjaudu Microsoft Azure koneen Learning Web Services-portaaliin. Valitse web-palvelu. Resurssin ryhmänimi on viidennen osan WWW-palvelun URL-osoitteen vain *resourceGroups* elementin jälkeen. Seuraavassa esimerkissä resurssin ryhmänimi on oletusarvo-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Vie JSON WWW-palvelun määritys

Voit muokata käyttämään juuri koulutetun koulutetun mallin määritys mallin, sinun on ensin käytettävä [Vie AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet-komento vieminen JSON-tiedostomuodossa.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Päivitä JSON ilearner-blob viittaus.

Kohteiden Etsi [koulutetun malli], Päivitä ilearner Blob-objektien URI *locationInfo* solmun *uri* -arvo. URI luodaan yhdistämällä *BaseLocation* ja *RelativeLocation* uudelleenkoulutusta puhelun BES tulosteesta.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>JSON tuominen Web-palvelu-määritys

Sinun on käytettävä [Tuo AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet-komento muuntaa takaisin Web-palvelu määritys, joiden avulla voit päivitettävä Predicative kokeen muokattu JSON-tiedosto.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Päivitä uuden WWW-palvelumäärityksen web-palvelu

Lopuksi voit Päivitä ennakoivan kokeen [Päivityksen AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet-komennon avulla.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Yhteenveto

Käytä koneen Learning PowerShell hallinnan cmdlet-komennot, voit päivittää koulutetun mallin mukainen ennakoivan verkkopalvelun ottaminen käyttöön skenaarioiden, kuten:

* Säännöllisiä malli uudelleenkoulutusta uusilla tiedoilla.
* Mallin asiakkaille jakauma ja jonka tavoitteena muokkaamista suuntaamista käyttämällä omia tietoja malliin.
