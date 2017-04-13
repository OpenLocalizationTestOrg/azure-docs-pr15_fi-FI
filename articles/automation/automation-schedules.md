<properties 
   pageTitle="Azure automaatio aikatauluja | Microsoft Azure"
   description="Automaatio aikataulujen avulla voidaan ajoittaa Azure automaatio käynnistymään automaattisesti runbooks. Kerrotaan, miten voit luoda ja hallita aikataulun niin, että voit aloittaa runbookin automaattisesti tiettynä ajankohtana tai toistuva aikataulu."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Azure automaatio-runbookin järjestäminen

Jos haluat järjestää runbookin Azure työkalujen käynnistämiseen määritettynä ajankohtana, linkität sen yhden tai useamman aikataulun. Aikataulun voi määrittää Suorita kerran tai ratkea tunteina tai päivittäinen aikataulu runbooks Azure perinteinen portaalissa ja runbooks Azure-portaalissa, voit lisäksi ajoittaa niitä, viikoittain, kuukausittain tietyn päivän, viikon tai kuukauden päivän ja kuukauden tiettynä päivänä.  Runbookin voidaan linkittää useita aikatauluja ja aikataulun voi olla useita runbooks linkitetyn.

>[AZURE.NOTE]  Aikataulujen tällä hetkellä tue Azure automaatio DSC määrityksiä.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShellin cmdlet-komennot

Seuraavassa taulukossa Cmdlet-komentoja käytetään luominen ja hallinta Windows PowerShellin Azure automaatio aikatauluja. Toimitus [Azure PowerShell-moduulin](../powershell-install-configure.md)osana.

|Cmdlet-komennot|Kuvaus|
|:---|:---|
|**Azure resurssien hallinnan cmdlet-komennot**||
|[Hae AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Hakee aikataulun.|
|[Uusi AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Luo uuden aikataulun.|
|[Poista AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Poistaa aikataulun.|
|[Määritä AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|Määrittää suoritettavan aikataulua ominaisuudet.|
|[Hae AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Hakee varattu runbooks.|
|[Rekisteröi AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Liittää runbookin aikataulun.|
|[Unregister AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Dissociates runbookin aikataulusta.|
|**Azure hallinnan cmdlet-komennot**||
|[Hae AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Hakee aikataulun.|
|[Uusi AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Luo uuden aikataulun.|
|[Poista AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Poistaa aikataulun.|
|[Määritä AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|Määrittää suoritettavan aikataulua ominaisuudet.|
|[Hae AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Hakee varattu runbooks.|
|[Rekisteröi AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Liittää runbookin aikataulun.|
|[Unregister AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Dissociates runbookin aikataulusta.|

## <a name="creating-a-schedule"></a>Aikataulun luominen

Voit luoda uuden aikataulun runbooks Azure portaalissa perinteinen-portaalista tai Windows PowerShellin avulla. Voit myös halutessasi uuden aikataulun luominen, kun linkität runbookin aikataulun Azure perinteinen tai Azure-portaalissa.

>[AZURE.NOTE] Kun liität aikataulun runbookin, automaatio moduulit nykyiset versiot tallentaa tilisi ja linkit, aikataulu.  Tämä tarkoittaa, että jos kun olet luonut aikataulun ja Päivitä moduulin versiosta 2.0-versiolla 1.0 moduuli-tilisi, aikataulun jatkaa 1.0.  Jotta voit käyttää päivitettyä moduuli, sinun on luotava uuden aikataulun. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Voit luoda uuden aikataulun Azure-portaalissa

1. Azure-portaalissa automaatio-tililtä valitsemalla Avaa **kalusto** -sivu napsauttamalla **kalusto** -ruutua.
2. Napsauta Avaa **aikatauluja** sivu **aikatauluja** -ruutua.
3. Valitse **Lisää aikataulun** yläreunaan sivu.
4. Kirjoita **nimi** ja halutessasi **kuvaus** uuden aikataulun **uuden aikataulun** -sivu.
5. Valitse onko aikataulun suoritetaan yhden kerran tai löytävä aikataulun valitsemalla **kerran** tai **Toistuva tapahtuma**.  Jos valitset **kerran** Määritä **alkamisaika** ja valitse sitten **Luo**.  Jos valitset **Toistuminen**, Määritä **alkamisaika** - ja taajuus, kuinka usein haluat toistaa runbookin - **Tunti**, **päivän**, **viikon**tai **kuukauden**mukaan.  Jos valitset **viikon** tai **kuukauden** avattavasta luettelosta, **Toistuvuus-vaihtoehto** tulee näkyviin sivu valinnan yhteydessä **Toistuminen-asetus** -sivu avautuu ja voit valita päivä, jos olet valinnut **viikon**.  Jos valitsit **kuukausi**, voit valita **viikonpäivien** tai tiettyjen päivien kalenterin kuukauden ja toimi lopuksi suorittamalla sen kuukauden viimeisen päivän vai ei ja valitse sitten **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Voit luoda uuden aikataulun Azure perinteinen-portaalissa

1. Azure perinteinen-portaalissa Valitse automaatio ja valitse sitten automaatio-tilin nimi.
1. Valitse **resurssit** -välilehti.
1. Ikkunan alareunassa valitsemalla **Lisää**.
1. Valitse **Lisää aikatauluun**.
1. **Nimi** ja halutessasi **kuvaus** schedule.your uuden aikataulun suoritetaan **Kerran**, **tunnin välein**, **Päivittäin**, **Viikoittain**tai **kuukausittain**.
1. Määritä **Alkamisaika** ja muita asetuksia, jotka valitsit aikataulun tyypin mukaan.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Voit luoda uuden aikataulun Windows PowerShellin avulla

Voit käyttää [Uusi AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet-komento, voit luoda uuden aikataulun Azure automaatio for perinteinen runbooks tai [Uusi AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet-komento, runbooks Azure-portaalissa. Alkamisajan määrittäminen aikataulun ja suorittaa korkojakso on määritettävä.

Esimerkki seuraavista komennoista näyttää, miten kuukausittain Azure resurssien hallinnan cmdlet-komennolla 30 ja 15 päivää aikataulun luominen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Esimerkki seuraavista komennoista näyttää, miten voit luoda uuden aikataulun, joka suoritetaan aina päivän 3:30 PM Azure hallinnan cmdlet-20. tammikuussa 2015 alkaen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Aikataulun linkittäminen runbookin

Runbookin voidaan linkittää useita aikatauluja ja aikataulun voi olla useita runbooks linkitetyn. Jos runbookin on parametreja, voit kirjoittaa arvoja niiden. Anna arvot pakollinen parametreja ja voidaan säätää mitä tahansa valinnaisten parametrien arvot.  Nämä arvot käytetään aina, kun n runbookin aloitetaan tämän aikataulun mukaan.  Voit samaan runbookin liittäminen toisesta aikataulun ja määritä eri parametriarvot.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Voit linkittää aikataulun runbookin Azure-portaalissa

1. Azure-portaalissa automaatio-tililtä napsauta Avaa **Runbooks** sivu **Runbooks** -ruutua.
2. Valitse ajoittaminen runbookin nimi.
3. Jos n runbookin ei ole linkitetty aikatauluun, valitse sinulle annetaan asetus, jos haluat luoda uuden aikataulun tai aikataulun linkki.  
4. Jos n runbookin on parametreja, voit valita asetuksen, **Muokkaa, suorita asetukset (oletus: Azure)** ja **Parametrit** -sivu on esitetty mihin voi lisätä tiedot vastaavasti.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Voit linkittää aikataulun runbookin Azure perinteinen-portaalissa

1. Azure perinteinen-portaalissa Valitse **automaatio** ja valitse sitten automaatio-tilin nimi.
2. Valitse **Runbooks** -välilehti.
3. Valitse ajoittaminen runbookin nimi.
4. Valitse **aikataulu** -välilehti.
5. Jos n runbookin ei ole linkitetty aikatauluun, valitse voit kiinnitetään vaihtoehto **uuden aikataulun** tai **linkittää aikataulun**.  Jos n runbookin on linkitetty aikataulun, napsauttamalla **linkkiä** ikkunan alareunassa voit käyttää näitä valintoja.
6. Jos n runbookin on parametreja, voit pyydetään niiden arvojen.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Voit linkittää aikataulun runbookin Windows PowerShellin avulla

[Rekisteröi AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) avulla voit linkittää aikataulun perinteinen runbookin tai [Rekisteröi AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet-komentojen käytöstä runbooks Azure-portaalissa.  Voit määrittää n runbookin parametrien arvot parametrit-parametrin. Saat lisätietoja määrittämisestä parametriarvot [Azure automaatio-Runbookin alkaen](automation-starting-a-runbook.md) .

Esimerkki seuraavista komennoista näyttää, miten voit linkittää aikataulun runbookin, Azure resurssien hallinnan cmdlet-komennon käyttäminen parametrit.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Esimerkki seuraavista komennoista näyttää, miten voit linkittää aikataulun Azure hallinnan cmdlet-komennon parametrit.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Aikataulun poistaminen käytöstä

Kun poistat aikataulun, kaikki siihen linkitetyt runbooks ei enää toimi, aikataulussa. Voit aikataulun poistaminen käytöstä tai määrittäminen aikatauluja niin usein vanheneminen-ajaksi, kun luot ne manuaalisesti. Kun päättymisaika saavutetaan, aikataulun eivät ole käytettävissä.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure-portaalista aikataulun poistaminen käytöstä

1. Azure-portaalissa automaatio-tililtä valitsemalla Avaa **kalusto** -sivu napsauttamalla **kalusto** -ruutua.
2. Napsauta Avaa **aikatauluja** sivu **aikatauluja** -ruutua.
2. Valitse Avaa tiedot-sivu aikataulun nimi.
3. Siirry **ei** **käytössä** .

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Perinteinen Azure-portaalista aikataulun poistaminen käytöstä

Voit poistaa käytöstä aikataulun Azure perinteinen portaalissa aikataulun aikataulutiedot-sivulla.

1. Azure perinteinen-portaalissa Valitse automaatio ja valitse sitten automaatio-tilin nimi.
1. Valitse resurssit-välilehti.
1. Valitse Avaa sen tietosivu aikataulun nimi.
2. Siirry **ei** **käytössä** .

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Windows PowerShellin aikataulun poistaminen käytöstä

[Määritä AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet-komennon avulla voit perinteinen runbookin tai [Aseta AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet-komentojen käytöstä runbooks Azure-portaalissa aikataulun ominaisuuksien muuttaminen. Käytöstä aikataulun, Määritä **Epätosi** **IsEnabled** -parametrin.

Esimerkki seuraavista komennoista Näytä poistamisesta käytöstä aikataulun runbookin, Azure resurssien hallinnan cmdlet-komennolla.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Esimerkki seuraavista komennoista Näytä poistamisesta käytöstä aikataulun Azure hallinnan cmdlet-komennolla.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Seuraavat vaiheet

- Aloittaminen Azure automaatio runbooks, on artikkelissa [Azure automaatio-Runbookin käynnistäminen](automation-starting-a-runbook.md) 