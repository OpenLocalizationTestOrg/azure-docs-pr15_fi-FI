<properties 
    pageTitle="Seurata ja hallita Stream Analytics töitä PowerShellin | Microsoft Azure" 
    description="Opettele käyttämään PowerShellin Azure ja cmdlet-komennot ja Stream Analytics töiden hallinta." 
    keywords="Azure powershell, azure PowerShellin cmdlet-komennot, powershell-komento powershell-komentosarjojen käyttäminen" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Seurata ja hallita Stream Analytics töiden Azure PowerShellin cmdlet-komennot

Lue, miten voit valvoa ja hallita Azure PowerShellin cmdlet-komennot ja powershell-komentosarjojen Stream Analytics resursseja, jotka suorittavat Stream Analytics perustoiminnot.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Edellytyksistä käynnissä Stream Analytics Azure PowerShellin cmdlet-komennot

 - Luo Azure-resurssiryhmä-tilaukseesi. Seuraavassa on esimerkki Azure PowerShell-komentosarjaa. PowerShellin Azure Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Stream Analytics työt luotu ohjelmallisesti ei ole käytössä oletusarvoisesti seuranta.  Voit ottaa manuaalisesti Azure-portaalissa valvoo projektin näytön sivulle siirtyminen ja ota käyttöön-vaihtoehdon tai voit tehdä tämän ohjelmallisesti osoitteessa [Azure Stream Analytics - näytön Stream Analytics työt ohjelmallisesti](stream-analytics-monitor-jobs.md)ohjeiden mukaisesti.

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Virta Analytics Azure PowerShell cmdlet-komennot
Seuraavat Azure PowerShell cmdlet-komennot avulla voidaan valvoa ja Azure Stream Analytics töiden hallinta. Huomaa, että PowerShellin Azure on eri versioita. 
**Olevissa esimerkeissä ensimmäinen komento on PowerShellin Azure 0.9.8, toista-komento on Azure PowerShell 1.0.** Azure PowerShell 1.0-komennot on aina "AzureRM" komento.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Hae AzureStreamAnalyticsJob | Hae AzureRMStreamAnalyticsJob
Näyttää kaikki Stream Analytics työt, jotka on määritelty Azure-tilauksen tai resurssiryhmä tai saa resurssin ryhmän tietyn työn projektin tietoja.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

PowerShell-komento palauttaa tietoja Stream Analytics projekteista Azure tilaus.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

PowerShell-komento palauttaa tietoja Stream Analytics projekteista resurssiryhmä StreamAnalytics-oletusarvo-Keski-US.

**Esimerkki 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

PowerShell-komento palauttaa tietoja Stream Analytics-työstä StreamingJob resurssiryhmä StreamAnalytics-oletusarvo-Keski-US.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Hae AzureStreamAnalyticsInput | Hae AzureRMStreamAnalyticsInput
Kaikkien syötteiden, jotka on määritetty määritetyn Stream Analytics-työn tai saa tietyn syötteen tietoja.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

PowerShell-komento palauttaa kaikkien määritetty työn StreamingJob syötteiden tietoja.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

PowerShell-komento palauttaa nimeltä EntryStream määritetty työn StreamingJob syötteen tietoja.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Hae AzureStreamAnalyticsOutput | Hae AzureRMStreamAnalyticsOutput
Luettelo kaikista tulostus, jotka on määritetty määritetyn Stream Analytics-työn tai saa tietyn tulosteen tietoja.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

PowerShell-komento palauttaa tietoja työn StreamingJob määritelty tulostaa.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

PowerShell-komento palauttaa nimeltä tulosteen määritetty työn StreamingJob tulosteen tietoja.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Hae AzureStreamAnalyticsQuota | Hae AzureRMStreamAnalyticsQuota
Saa streaming yksiköt määritetyllä alueella kiintiön tietoja.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

PowerShell-komento palauttaa tietoja kiintiön ja streaming yksiköiden käyttö Yhdysvaltojen Keski-alueella.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Hae AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Hakee tietyn muunnoksen määritetty Stream Analytics projektin tietoja.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

PowerShell-komento palauttaa tietoja eli StreamingJob työn StreamingJob muunnos.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Uusi AzureStreamAnalyticsInput | Uusi AzureRMStreamAnalyticsInput
Luo uusi input Stream Analytics projektissa tai päivittää aiemmin määritetyn arvon.
  
Syötteen nimi voidaan määrittää .json tiedostossa tai komentorivillä. Jos molemmat on määritetty, komentorivillä nimen on oltava sama kuin tiedoston.

Jos syöte, joka on jo ja määritä voimassa – parametrin cmdlet kysyy korvaa aiemmin luotu syötteen vai ei.

Jos määrität – voimassa parametrin ja määrittää syötteen korvataan ilman vahvistusta aiemmin luodun syötteen nimi.

Lisätietoja JSON tiedostorakenteen ja sisällön viitata [Luominen syötteen (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] osan [Stream Analytics REST API viittaus kirjaston][stream.analytics.rest.api.reference].

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

PowerShell-toiminto luo uusi input tiedoston Input.json. Jos aiemmin syöte, jonka nimi on määritetty syötteen määritystiedostossa on jo määritetty, cmdlet kysyy korvata vai ei.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

PowerShell-komentoa Luo uusi input kutsutaan EntryStream työn. Jos aiemmin syöte, jonka nimi on jo määritetty, cmdlet kysyy korvata vai ei.

**Esimerkki 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

PowerShell-komentoa korvaa määritelmän nimeltä EntryStream määritys tiedostosta olemassa lähde.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Uusi AzureStreamAnalyticsJob | Uusi AzureRMStreamAnalyticsJob
Luo uusi Stream Analytics-työ Microsoft Azure tai päivittää aiemmin määritetyn työn määritys.

Työn nimi voidaan määrittää .json tiedostossa tai komentorivillä. Jos molemmat on määritetty, komentorivillä nimen on oltava sama kuin tiedoston.

Jos projektin nimi, joka on jo ja määritä voimassa – parametrin cmdlet kysyy korvaa olemassa olevan projektin vai ei.

Jos määrität – voimassa parametrin ja määrittää aiemmin luodun projektin nimi projektin määritelmän korvataan ilman vahvistusta.

Lisätietoja JSON tiedostorakenteen ja sisällön viitata [Luominen Stream Analytics työn] [ msdn-rest-api-create-stream-analytics-job] osan [Stream Analytics REST API viittaus kirjaston][stream.analytics.rest.api.reference].

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

PowerShell-toiminto luo uuden projektin JobDefinition.json määritelmän. Jos aiemmin luodun työn, jonka nimi on määritetty työn määritystiedostossa on jo määritetty, cmdlet kysyy korvata vai ei.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

PowerShell-komentoa korvaa StreamingJob työn määritelmä.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Uusi AzureStreamAnalyticsOutput | Uusi AzureRMStreamAnalyticsOutput
Luo uusi tulosteen Stream Analytics projektissa tai päivittää aiemmin tulos.  

Tulosteen nimi voidaan määrittää .json tiedostossa tai komentorivillä. Jos molemmat on määritetty, komentorivillä nimen on oltava sama kuin tiedoston.

Jos määrität tulos, joka on jo ja määritä voimassa – parametrin cmdlet kysyy korvaa aiemmin luotu tulosteen vai ei.

Jos määrität – voimassa parametrin ja määrittää aiemmin luodun tulosteen nimi, tulos korvataan ilman vahvistusta.

Lisätietoja JSON tiedostorakenteen ja sisällön viitata [Luominen tulos (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] osan [Stream Analytics REST API viittaus kirjaston][stream.analytics.rest.api.reference].

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

PowerShell-toiminto luo uuden tulosteen, kutsutaan "tulosteen" työn StreamingJob. Jos aiemmin tulosteen, jonka nimi on jo määritetty, cmdlet kysyy korvata vai ei.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

PowerShell-komentoa korvaa "tulosteen" määrittelyn työn StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Uusi AzureStreamAnalyticsTransformation | Uusi AzureRMStreamAnalyticsTransformation
Luo uusi muunnos Stream Analytics projektissa tai päivittää aiemmin muunnos.
  
Muunnoksen nimi voidaan määrittää .json tiedostossa tai komentorivillä. Jos molemmat on määritetty, komentorivillä nimen on oltava sama kuin tiedoston.

Jos määrität muunnoksen, joka on jo ja määritä voimassa – parametrin cmdlet kysyy korvaa aiemmin luotu muunnos vai ei.

Jos määrität – voimassa parametrin ja määrittää aiemmin luodun muunnos nimeä muunnoksen korvataan ilman vahvistusta.

Lisätietoja JSON tiedostorakenteen ja sisällön viitata [Luominen-muunnos (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] osan [Stream Analytics REST API viittaus kirjaston][stream.analytics.rest.api.reference].

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

PowerShell-toiminto luo uusi muunnos, eli StreamingJobTransform työn StreamingJob. Jos aiemmin muunnos on jo määritetty tämännimistä, cmdlet kysyy korvata vai ei.

**Esimerkki 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 PowerShell-komentoa korvaa StreamingJobTransform määritelmän työn StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Poista AzureStreamAnalyticsInput | Poista AzureRMStreamAnalyticsInput
Poistaa tietyn syötteen asynkronisesti Stream Analytics Microsoft Azure-työstä.  
Jos määrität voimassa – parametrin syöte poistetaan ilman vahvistusta.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

PowerShell-toiminto poistaa syötteen EventStream työn StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Poista AzureStreamAnalyticsJob | Poista AzureRMStreamAnalyticsJob
Poistaa tietyn Stream Analytics työn Microsoft Azure-asynkronisesti.  
Jos määrität voimassa – parametrin työn poistetaan ilman vahvistusta.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

PowerShell-toiminto poistaa työn StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Poista AzureStreamAnalyticsOutput | Poista AzureRMStreamAnalyticsOutput
Poistaa tietyn tulosteen asynkronisesti Stream Analytics Microsoft Azure-työstä.  
Jos määrität voimassa – parametrin tulosteen poistetaan ilman vahvistusta.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Tämä PowerShell komento poistaa tulosteen tulosteen työn StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Käynnistä AzureStreamAnalyticsJob | Käynnistä AzureRMStreamAnalyticsJob
Ottaa käyttöön asynkronisesti ja aloittaa Stream Analytics työn Microsoft Azure.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

PowerShell-komento aloittaa StreamingJob mukautettu tulostus alkamisaika ja määritä 12. joulukuuta 2012 12:12:12 työn UTC-aika.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Lopeta AzureStreamAnalyticsJob | Lopeta AzureRMStreamAnalyticsJob
Asynkronisesti lopettaa Stream Analytics projektin suorittaminen Microsoft Azure ja poista Varaa resurssit, jotka olivat, joka on käytössä. Työn määrityksen ja metatietojen ne ovat käytettävissä kuluessa tilauksen kautta Azure portal ja hallinnan API siten, että työ voidaan muokata ja käynnistetään uudelleen. Sinun ei peri projektin pysäytetty-tilaan.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

PowerShell-komentoa lopettaa työn StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Testaa AzureStreamAnalyticsInput | Testaa AzureRMStreamAnalyticsInput
Testaa Stream Analytics mahdollisuus yhdistää määritetyn syöte.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

PowerShell-komento on testannut yhteyden tilan syötteen EntryStream StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Testaa AzureStreamAnalyticsOutput | Testaa AzureRMStreamAnalyticsOutput
Testaa Stream Analytics mahdollisuus yhdistää määritetyn tulos.

**Esimerkki 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Tämä PowerShell komento testien tulos yhteyden tilan tulosteen StreamingJob.  

## <a name="get-support"></a>Käytä tukipalveluita
Saat lisäohjeita voit kokeilla Microsoftin [Azure Stream Analytics-keskustelupalstalla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
