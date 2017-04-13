<properties
    pageTitle="Luominen tai tuominen Azure automaatio-runbookin"
    description="Tässä artikkelissa käsitellään uuden runbookin luominen Azure automaatio tai tuo kuva tiedostosta."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Luominen tai tuominen Azure automaatio-runbookin

Voit lisätä runbookin Azure automaatio joko [luomalla uuden](#creating-a-new-runbook) tai tuomalla aiemmin runbookin tiedostosta tai [Runbookin valikoima](automation-runbook-gallery.md). Tässä artikkelissa on tietoja luomisesta ja runbooks tuominen tiedostosta.  Saat kaikki tiedot yhteisön runbooks ja [Runbookin ja moduulin valikoimat Azure automatisointiin](automation-runbook-gallery.md)moduulit käyttäminen.

## <a name="creating-a-new-runbook"></a>Uuden runbookin luominen

Voit luoda uuden runbookin jollakin Azure portaaleihin tai Windows PowerShellin Azure automaatio. Kun n runbookin on luotu, voit muokata sen [Learning PowerShell työnkulun](automation-powershell-workflow.md) ja [graafiset yhteiskäyttö Azure automaatio](automation-graphical-authoring-intro.md)tietoja.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Voit luoda uuden Azure automaatio-runbookin Azure perinteinen-portaalissa

Voit käsitellä [PowerShell työnkulun runbooks](automation-runbook-types.md#powershell-workflow-runbooks) vain Azure-portaalissa.

1. Azure-perinteinen-portaalissa valitsemalla **Uusi** **sovellus Services** **Automation**, **Runbookin**, **Nopea luominen**.
2. Kirjoita tarvittavat tiedot ja valitse sitten **Luo**. Runbookin nimi on alettava kirjaimella, ja se voi olla kirjaimia, numeroita tai alaviivoja katkoviivat.
3. Jos haluat muokata: n runbookin nyt ja valitse sitten **Muokkaa Runbookin**. Muussa tapauksessa valitse **OK**.
4. Uusi runbookin näkyy **Runbooks** -välilehdessä.


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Voit luoda uuden Azure automaatio-runbookin Azure-portaalissa

1. Avaa Azure-portaaliin, automaatio-tilillesi.
2. Valitse Avaa runbooks luettelo **Runbooks** -ruutu.
3. Valitse **Lisää runbookin** -painiketta ja sitten **Luo uusi runbookin**.
2. : N runbookin **nimi** ja valitse sen [tyyppiä](automation-runbook-types.md). Runbookin nimi on alettava kirjaimella, ja se voi olla kirjaimia, numeroita tai alaviivoja katkoviivat.
3. Valitse **Luo** Luo: n runbookin ja avaa editorin.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Voit luoda uuden Azure automaatio-runbookin Windows PowerShellin avulla

[Uusi AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) cmdlet-komennon avulla voit luoda tyhjän [PowerShell työnkulun runbookin](automation-runbook-types.md#powershell-workflow-runbooks). Voit joko määrittää Luo tyhjä runbookin, joita voi muokata myöhemmin **Name** -parametri tai voit määrittää **Path** -parametrin runbookin-tiedoston tuonnin. **Kirjoita** parametrin myös otetaan määrittämään runbookin tyyppejä.

Esimerkki seuraavista komennoista näyttää, miten voit luoda uuden tyhjän runbookin.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Tuominen tiedostosta runbookin Azure automaatio

Voit luoda uuden runbookin Azure automaatio tuomalla PowerShell-komentosarjaa tai PowerShell-työnkulku (.ps1-tunniste) tai viedyn graafinen runbookin (.graphrunbook).  Sinun on määritettävä [runbookin tyyppi](automation-runbook-types.md) , joka luodaan ottaen huomioon seuraavat seikat tuonnista.

- Uusi [Graafinen runbookin](automation-runbook-types.md#graphical-runbooks)vain voi tuoda .graphrunbook tiedoston ja graafiset runbooks voi luoda vain .graphrunbook-tiedostosta.
- PowerShell-työnkulun sisältävän .ps1-tiedoston voi tuoda vain [PowerShell työnkulun runbookin](automation-runbook-types.md#powershell-workflow-runbooks).  Jos tiedostossa on useita PowerShell työnkulkuja, tuonti epäonnistuu. Täytyy tallentaa jokaiselle työnkululle oma tiedostoon ja tuoda kunkin erikseen.
- .Ps1 tiedosto, joka ei sisällä työnkulun voi tuoda [PowerShell runbookin](automation-runbook-types.md#powershell-runbooks) tai [PowerShell työnkulun runbookin](automation-runbook-types.md#powershell-workflow-runbooks).  Jos se tuodaan PowerShell työnkulun runbookin, valitse se muuntuvat työnkulun ja kommentit sisällytetään runbookin, joka määrittää tehdyt muutokset.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Tuo tiedostosta Azure perinteinen-portaalissa runbookin
Voit komentosarjatiedoston tuominen Azure automaatio seuraavasti.  Huomaa, että voit tuoda .ps1 tiedoston vain tässä portaalissa työnkulun PowerShell-runbookin.  Sinun on käytettävä Azure portaalin muiden apuohjelmatyyppien.

1. Azure hallinta-portaalissa Valitse **automaatio** ja valitse sitten automaatio-tiliä.
2. Valitse **Tuo**.
3. Etsi **Tiedosto** ja Etsi tuotava komentosarjatiedosto.
4. Jos haluat muokata: n runbookin nyt ja valitse sitten **Muokkaa Runbookin**. Muussa tapauksessa valitse OK.
5. Uusi runbookin **Runbooks** -välilehdessä näkyvät automaatio-tilille.
6. Sinun täytyy [julkaista: n runbookin](#publishing-a-runbook) ennen sen suorittamista.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Tuo tiedostosta Azure-portaalissa runbookin
Voit komentosarjatiedoston tuominen Azure automaatio seuraavasti.  

>[AZURE.NOTE] Huomaa, että voit tuoda .ps1 tiedoston vain työnkulun PowerShell-runbookin-portaalissa.

1. Avaa Azure-portaaliin, automaatio-tilillesi.
2. Valitse Avaa runbooks luettelo **Runbooks** -ruutu.
3. Valitse **Lisää runbookin** -painiketta ja valitse sitten **Tuo**.
4. Valitse tuotava tiedosto **Runbookin-tiedosto**
2. Jos **nimi** -kentässä on otettu käyttöön, sinun on asetus, jos haluat muuttaa.  Runbookin nimi on alettava kirjaimella, ja se voi olla kirjaimia, numeroita tai alaviivoja katkoviivat.
3. [Runbookin tyyppi](automation-runbook-types.md) valitaan automaattisesti, mutta voit muuttaa tyyppiä ottaen huomioon sovellettavan rajoitukset. 
3. Uusi runbookin näkyvät runbooks luettelo automaatio-tilille.
4. Sinun täytyy [julkaista: n runbookin](#publishing-a-runbook) ennen sen suorittamista.

>[AZURE.NOTE] Kun olet tuonut graafinen runbookin tai graafisen PowerShell-työnkulun runbookin, sinulla asetus, jos haluat muuntaa toisen tyypin, jos. Et voi muuntaa tekstimuodossa.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Komentosarjatiedosto Windows PowerShellin avulla runbookin tuominen

[Tuo AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet-komennon avulla voit tuoda komentosarjatiedosto luonnokseksi PowerShell työnkulun runbookin. Jos n runbookin on jo olemassa, tuonti epäonnistuu, ellet käytä *-voimassa* parametria. 

Esimerkki seuraavista komennoista Näytä komentosarjatiedoston tuomisesta runbookin.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Julkaiseminen runbookin

Luo tai tuo uusi runbookin, kun se on julkaistava ennen sen suorittamista.  Automaatio kunkin runbookin on luonnos ja julkaistu versio. Vain julkaistu versio on saatavana suorittaminen ja luonnos-version voi muokata. Julkaistu versio ei vaikuta luonnos-versioon tehdyt muutokset. Kun luonnos-versio on annettava saataville, sitten voit julkaista sen, joka korvaa julkaistu versio luonnos-versiolla.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Jos haluat julkaista runbookin, Azure perinteinen-portaalissa

1. Avaa: n runbookin Azure perinteinen-portaalissa.
1. Valitse näytön yläreunassa **Tekijä**.
1. Näytön alareunassa napsauttamalla vahvistusviesti **Julkaise** ja valitse sitten **Kyllä** .

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Jos haluat julkaista runbookin, Azure-portaalissa

1. Avaa: n runbookin Azure-portaalissa.
1. Valitse **Muokkaa** -painiketta.
1. Valitse **Julkaise** -painiketta ja valitse **Kyllä** vahvistusviesti.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Jos haluat julkaista runbookin, Windows PowerShellin avulla

[Julkaise AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) cmdlet-komennon avulla voit julkaista runbookin Windows PowerShellin avulla. Esimerkki seuraavista komennoista näyttää, miten voit julkaista otoksen runbookin.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja siitä, miten voit hyötyä Runbookin ja PowerShell-moduulin valikoima on artikkelissa [Azure automatisointiin Runbookin ja moduulin valikoimat](automation-runbook-gallery.md)
- Lisätietoja muokkaaminen PowerShellistä ja PowerShell työnkulun runbooks tekstiä editorin avulla, katso [muokkaaminen tekstiä runbooks Azure automaatio-](automation-edit-textual-runbook.md)
- Lisätietoja graafinen runbookin yhtä aikaa muiden kanssa on artikkelissa [graafiset yhteiskäyttö Azure automaatio](automation-graphical-authoring-intro.md)
