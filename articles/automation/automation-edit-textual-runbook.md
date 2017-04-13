<properties 
    pageTitle="Tekstimuotoinen runbooks Azure automaatio-muokkaaminen"
    description="Tässä artikkelissa on eri tavoista käsittelyyn PowerShellistä ja PowerShell työnkulun runbooks Azure automaatio-tekstiä editorilla."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure automaatio-tekstiä runbooks muokkaaminen

Azure automaatio tekstimuotoinen editorin avulla voidaan muokata [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) ja [PowerShell työnkulun runbooks](automation-runbook-types.md#powershell-workflow-runbooks). Tämä on muita koodin editors kuten intellisense- ja värikoodaus on erityisen lisäominaisuuksia voit helpottaa käytettäessä resurssien yhteiset runbooks tyypillisiä ominaisuuksia.  Tässä artikkelissa on lisätietoja kohdasta eri Funktiot, joiden tämän editorin tekemistä varten.

Tekstiä editorin sisältää toiminnon, voit lisätä koodin tehtävät, resurssit ja alatason runbooks runbookin. Sen sijaan, että kirjoittamalla koodin itse, voit valita luettelosta käytettävissä olevien resurssien ja on lisätty: n runbookin koodi.

Azure automaatio kunkin runbookin on kaksi versiota, luonnos ja julkaistu. Muokkaa: n runbookin luonnos-versiota ja julkaista sen, jotta se voidaan suorittaa. Julkaistu versio ei voi muokata. Lisätietoja on kohdassa [julkaiseminen runbookin](automation-creating-importing-runbook.md#publishing-a-runbook) .

Käsitellä [Graafinen Runbooks](automation-runbook-types.md#graphical-runbooks), on artikkelissa [Azure automaatio yhteiskäyttö graafiset](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Jos haluat muokata runbookin Azure-portaalissa

Seuraavien ohjeiden avulla voit avata runbookin tekstiä editorissa muokkaamista varten.

1. Valitse Azure-portaaliin, automaatio-tilillesi.
2. Napsauta Avaa runbooks luettelo **Runbooks** -ruutua.
3. Valitse Muokkaa ja valitse sitten **Muokkaa** -painikkeen runbookin nimi.
6. Suorita pakolliset muokkaamista.
7. Kun muutokset ovat valmiit, valitse **Tallenna** .
8. Valitse **Julkaise** , jos haluat julkaista runbookin luonnos uusin.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Jos haluat lisätä cmdlet-komento, runbookin

2. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa-cmdlet-komennolla.
3. Laajenna kirjaston ohjausobjektin **cmdlet** -solmu. 
3. Laajenna moduuli, joka sisältää cmdlet-komento, jota haluat käyttää.
4. Napsauta hiiren kakkospainikkeella ja valitse **Lisää Kuvapohjan**cmdlet-komento.  Jos cmdlet-komento on useampi kuin yksi parametrin määrittäminen, oletusarvoiset lisätään.  Voit myös laajentaa-cmdlet-komennolla voit valita eri parametria.
4. Cmdlet koodi on lisätty sen koko luettelon parametrien avulla.
5. Anna tekstin näyttäminen aaltosulkeet <> varten tarvittavat parametrit uhkaavat tietotyypin sopiva arvo.  Poista parametreja, joita et tarvitse.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Jos haluat lisätä lapsen runbookin koodi runbookin

2. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa [lapsen runbookin](automation-child-runbooks.md)koodi.
3. Laajenna **Runbooks** solmu kirjasto-ohjausobjektissa. 
3. Napsauta hiiren kakkospainikkeella ja valitse **Lisää Kuvapohjan**runbookin.
4. Lapsen runbookin koodi on lisätty paikkamerkkien runbookin kaikki parametrien kanssa.
5. Paikkamerkkien tilalle tarvittavat kunkin parametrin arvot.

### <a name="to-insert-an-asset-into-a-runbook"></a>Jos haluat lisätä sijoituksen runbookin

2. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa lapsen runbookin koodi.
3. Laajenna kirjaston ohjausobjektin **kalusto** -solmu. 
4. Laajenna haluamasi resurssi tyypin solmu.
3. Napsauta hiiren kakkospainikkeella ja valitse **Lisää Kuvapohjan**resurssi.  [Muuttujan resurssit](automation-variables.md)Valitse **Lisää "Hae muuttujan" Kuvapohjan** tai **lisätä "määrittäminen muuttujan" Kuvapohjan** sen mukaan, haluatko saada tai määrittää muuttujan.
4. Kohteen koodi on lisätty: n runbookin.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Jos haluat muokata runbookin Azure-portaalissa

Seuraavien ohjeiden avulla voit avata runbookin tekstiä editorissa muokkaamista varten.

1. Azure-portaalissa Valitse **Automaattiset** ja valitse sitten automaatio-tilin nimi.
2. Valitse **Runbooks** -välilehti.
3. Valitse Muokkaa ja valitse sitten **Tekijä** -välilehti: n runbookin nimi.
5. Napsauta näytön alareunassa **Muokkaa** -painiketta.
6. Suorita pakolliset muokkaamista.
7. Kun muutokset ovat valmiit, valitse **Tallenna** .
8. Valitse **Julkaise** , jos haluat julkaista runbookin luonnos uusin.

### <a name="to-insert-an-activity-into-a-runbook"></a>Voit lisätä toiminnon Runbookin

1. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa tehtävän.
1. Valitse näytön alareunassa **Lisää** ja valitse sitten **tehtävä**.
1. Valitse tehtävän sisältävä moduuli **Integrointi moduuli** -sarakkeessa.
1. Valitse tehtävän **aktiviteetin** -ruudussa.
1. Huomioi tehtävän kuvaus **kuvaus** -sarakkeessa. Vaihtoehtoisesti voit napsauttaa Näytä yksityiskohtaisia ohjeita käynnistää Ohjeen tehtävän selaimessa.
1. Napsauta sitten oikealle osoittavaa nuolta.  Jos tehtävä on parametreja, ne näkyvät tiedot.
1. Valitse Tarkista-painiketta.  Suorittaa tehtävän koodia lisätty: n runbookin.
1. Jos tehtävän vaatii parametreja, anna tekstin näyttäminen aaltosulkeet <> uhkaavat tietotyypin sopiva arvo.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Jos haluat lisätä lapsen runbookin koodi runbookin

1. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa [lapsen runbookin](automation-child-runbooks.md).
2. Valitse näytön alareunassa **Lisää** ja sitten **Runbookin**.
3. Valitse runbookin center-sarake ja sitten oikealle osoittavaa nuolta.
4. Jos n runbookin on parametreja, ne näkyvät tiedot.
5. Valitse Tarkista-painiketta.  Suorittaa valitun runbookin koodia lisätty nykyisen runbookin.
7. Jos n runbookin vaatii parametreja, antaa tekstin näyttäminen aaltosulkeet <> uhkaavat tietotyypin sopiva arvo.

### <a name="to-insert-an-asset-into-a-runbook"></a>Jos haluat lisätä sijoituksen runbookin

1. Tekstimuotoinen editorin piirtoalusta Aseta kohdistin kohtaa, johon haluat sijoittaa tehtävän hakemiseen kohteen.
1. Valitse näytön alareunassa **Lisää** ja valitse sitten **asetus**.
1. **Asetus-toiminto** -sarakkeeseen ja valitse haluamasi toiminto.
1. Valitse käytettävissä olevat varat center-sarakkeessa.
1. Valitse Tarkista-painiketta.  Koodin hankkiminen tai kohteen lisätty: n runbookin.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Voit muokata Windows PowerShellin Azure automaatio-runbookin

Jos haluat muokata runbookin Windows PowerShellin avulla, editorilla valittua ja tallenna se .ps1 tiedostoon. [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) cmdlet-komennon avulla voit hakea runbookin ja sitten [Aseta AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) cmdlet-komento, voit korvata aiemmin luonnos runbookin muokattu sisällön.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Hakea sisältöä Runbookin, Windows PowerShellin avulla

Esimerkki seuraavista komennoista näyttää, miten noutaa komentosarjan runbookin ja tallenna se komentosarja-tiedostoon. Tässä esimerkissä luonnos-versio on noudettu. On myös voi hakea: n runbookin julkaistu-version, vaikka tämä versio ei voi muuttaa.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Jos haluat muuttaa sisällön Runbookin, Windows PowerShellin avulla

Esimerkki seuraavista komennoista näyttää, miten voit korvata olemassa olevan sisällön runbookin komentosarjatiedoston sisällön. Huomaa, että tämä on sama otoksen menettely, kuten [Tuo runbookin komentosarja-tiedostosta Windows PowerShellin avulla](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Luominen tai tuominen Azure automaatio-runbookin](automation-creating-importing-runbook.md)
- [Oppimiskeskuksen PowerShell-työnkulku](automation-powershell-workflow.md)
- [Graafisen Azure automaatio yhteiskäyttö](automation-graphical-authoring-intro.md)
- [Varmenteet](automation-certificates.md)
- [Yhteydet](automation-connections.md)
- [Tunnistetiedot](automation-credentials.md)
- [Aikatauluja](automation-schedules.md)
- [Muuttujat](automation-variables.md)