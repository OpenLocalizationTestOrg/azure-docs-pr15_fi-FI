<properties 
    pageTitle="Seurata ja hallita Azure Data Factory putkistot" 
    description="Opettele käyttämään Azure-portaaliin ja PowerShellin Azure ja Azure tietojen tehtaan ja olet luonut putkistot hallinta." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>


# <a name="monitor-and-manage-azure-data-factory-pipelines"></a>Seurata ja hallita Azure Data Factory putkistot
> [AZURE.SELECTOR]
- [Azure portal/Azure PowerShellin avulla](data-factory-monitor-manage-pipelines.md)
- [Käyttämällä seuranta ja hallinta-sovellus](data-factory-monitor-manage-app.md)

Data Factory-palvelu sisältää luotettava ja valmis näkymän tallennustilan ja käsittelyn siirto palvelujen. Palvelun tarjoaa seurantaa Raporttinäkymät-ikkunan avulla, joiden avulla voit suorittaa seuraavia tehtäviä: 

- Arvioi nopeasti lopusta loppuun-tietojen myyntijakso kunto.
- Tunnistaa ongelmista ja tutustu korjaavat toimenpiteet tarvittaessa. 
- Seuraa tiedot hierarkiatasolla. 
- Voit seurata yhteydet yli jonkin omista lähteistä tietojen välillä.
- Näytä koko historiallinen kirjanpidon suorittaminen ja järjestelmän kunnon riippuvuuksien.

Tässä artikkelissa käsitellään seurata ja hallita oman putkistot virheenkorjaus. On myös tietoja siitä, miten voit luoda ilmoituksia ja saat ilmoituksen virheet.

## <a name="understand-pipelines-and-activity-states"></a>Putkistot ja tehtävän tilan
Azure portaalin avulla voit:

- Tarkastele tietoja factory kaaviona
- Valitse putkijohto toimintojen tarkasteleminen
- Tarkastele syöttö- ja tietojoukkoja
- ja paljon muuta. 

Tässä osassa on myös siitä, miten sektoria siirtymät tilasta toiseen tilaan.   

### <a name="navigate-to-your-data-factory"></a>Siirry tiedot-factory
1.  Kirjautuminen [Azure portal](https://portal.azure.com).
2.  Valitse **tietojen tehtaan** vasemmasta valikosta. Jos ei ole näkyvissä, valitse **Lisää palveluja >** ja valitse **tietoja tehtaan** **liiketoimintatietojen + ANALYTICS** -luokassa. 

    ![Selaa kaikki -> tietojen tehtaan](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    Raportissa pitäisi näkyä **tietojen tehtaan** sivu kaikki tiedot tehtaan. 
4. Valitse kiinnostavan tietojen factory tehtaan tiedot-sivu.

    ![tietojen valitseminen factory](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)  
5.  ja näyttöön tulee tietoja factory kotisivu (**Data factory** -sivu).

    ![Tietoja factory-sivu](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Verkkokaavio-näkymän tiedot factory.
Tietoja factory kaavionäkymään tarjoaa yhden laidassa, kun haluat seurata ja hallita tietoja factory ja sen varat.

Saat tietoja factory kaavionäkymään valitsemalla **kaavion** tiedot factory aloitussivulla.

![Kaavionäkymä](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Voit lähentää, loitontaa, Zoomaa Sovita, zoomaus 100 %, Lukitse kaavioon asettelun ja sijoita automaattisesti putkistot ja taulukot. Näet myös hierarkiatasolla tiedot (Näytä valitut kohteet ja alaspäin kohteet).
 

### <a name="activities-inside-a-pipeline"></a>Toimintojen putkijohto sisällä 
1. Napsauta putkijohto ja valitse **Avaa myyntijakso** Nähdäksesi putkijohto syöttö- ja tietojoukkoja toiminnasta sekä kaikki toiminnot. Tämä ominaisuus on hyödyllinen, kun yhteyttä myyntijakso sisältää useamman kuin yhden tehtävän ja haluat ymmärtää yksi putkijohto toiminnallisia hierarkiatasolla.

    ![Avaa myyntijakso-valikko](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. Seuraavassa esimerkissä on kaksi toimintojen putkijohto ja niiden syötteiden ja tulostaa. Liittyvä **JoinData** HDInsight rakenne tehtävän tyyppi ja **EgressDataAzure** tyypin kopioi tehtävän aktiviteetin ovat tämän otoksen myyntijakso. 
    
    ![Toimintojen putkijohto sisällä](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. Voit siirtyä takaisin Data Factory kotisivulle napsauttamalla factory tietoyhteyden linkkipolussa vasemmassa yläkulmassa.

    ![Siirry takaisin tietojen factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-state-of-each-activity-inside-a-pipeline"></a>Näytä tila tehtävätyypin putkijohto sisällä
Voit tarkastella tehtävän nykyisen tilan mukaan, minkä tahansa tehtävän tuottamien tietojoukkoja tilan tarkasteleminen. 

Esimerkki: Seuraavassa esimerkissä **BlobPartitionHiveActivity** suoritettiin onnistuneesti ja tuottaa tietojoukko nimeltä **PartitionedProductsUsageTable**, joka on **Valmis** -tilaan.

![Myyntijakso tila](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Toinen tehtävä on suoritettu sisällä putkijohto tuottamat sektorit kaksoisnapsauttamalla **PartitionedProductsUsageTable** kaavionäkymään esitellään. Näet, että **BlobPartitionHiveActivity** suoritettiin onnistuneesti joka kuukausi, viimeksi kahdeksan kuukauden ja tuottaa sektorit **Valmis** -tilaan.

Tietoja factory tietojoukko sektorit voi olla jokin seuraavista tiloista:

<table>
<tr>
    <th align="left">Tila</th><th align="left">Yhdistelmätilaan</th><th align="left">Kuvaus</th>
</tr>
<tr>
    <td rowspan="8">Odottaa</td><td>ScheduleTime</td><td>Suorita sektoria on tule aika.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Seuraavat riippuvuudet eivät ole valmis.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Laske-resurssit eivät ole käytettävissä.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Tehtävän esiintymät on varattu muiden sektoreiden käynnissä.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Tehtävä on keskeytetty ja voi suorittaa sektorit, kunnes se on sen käyttöä jatketaan.</td>
</tr>
<tr>
<td>Yritä uudelleen</td><td>Tehtävän suorittaminen yritetään.</td>
</tr>
<tr>
<td>Kelpoisuustarkistus</td><td>Kelpoisuuden tarkistaminen ei ole vielä aloitettu.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Odotetaan kelpoisuuden yritettävä.</td>
</tr>
<tr>
<tr
<td rowspan="2">Aktiivisuustila</td><td>Vahvistaminen</td><td>Vahvistus on käynnissä.</td>
</tr>
<td></td>
<td>Sektoria käsitellään.</td>
</tr>
<tr>
<td rowspan="4">Epäonnistui</td><td>Aikakatkaisu</td><td>Suorittamisen kulunut kauemmin kuin, joka on tehtävä.</td>
</tr>
<tr>
<td>Peruuttaa</td><td>Peruuttaa käyttäjältä-toiminto.</td>
</tr>
<tr>
<td>Kelpoisuustarkistus</td><td>Vahvistus epäonnistuu.</td>
</tr>
<tr>
<td></td><td>Luo ja/tai Vahvista sektoria epäonnistui.</td>
</tr>
<td>Valmis</td><td></td><td>Sektoria on valmis käytettäväksi.</td>
</tr>
<tr>
<td>Ohitettu</td><td></td><td>Sektoria ei käsitellä.</td>
</tr>
<tr>
<td>Ei mitään</td><td></td><td>Sektorin käytetään eri tila liitettyinä, mutta se on vaihdettu.</td>
</tr>
</table>



Voit tarkastella muutoksen tietoja sektoria sektoria-merkintä **Viimeksi päivitetty sektorit** -sivu napsauttamalla.

![Sektorin tiedot](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
Jos sektoria on suoritettu useita kertoja, näet useita rivejä **tehtävän suoritetaan** -luettelossa. Voit tarkastella tietoja tehtävän suorittaminen napsauttamalla Suorita tapahtuma **tehtävän suoritetaan** -luettelossa. Luettelossa näkyvät kaikki virhesanoma, jos sellainen on sekä lokitiedostot. Tämä ominaisuus on hyödyllinen, voit tarkastella ja korjata lokit poistumatta tietojen factory.

![Tehtävän suorittaminen tiedot](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Jos sektoria ei ole **Valmis** -tilaan, näet seuraavat sektorit, jotka eivät ole valmis ja estävät nykyinen sektori suorittaminen **ylöspäin sektoreiden, joita ei ole valmis** -luettelossa. Tämä ominaisuus on hyödyllinen, kun yhteyttä sektoria on **odottaa** -tilassa ja haluat ymmärtää seuraavat riippuvuudet, joina sektoria odottaa.

![Seuraavat sektoreiden ei ole valmis](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Tietojoukon tilan kaavio
Kun otat tietojen factory ja putkistot on kelvollinen aktiivisen kauden, dataset sektoria siirtymän tilasta toiseen. Tällä hetkellä sektoria tila tapahtuu seuraavassa kaaviossa tila:

![Tilakaavioille.](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Tietoja factory tietojoukko tilan siirtymä-työnkulku: odottaa -> Valitse käynnissä/käynnissä (Validating) -> valmis/epäonnistui

Sektorien Aloita vanhat ehtojen on täytyttävä, ennen kuin suoritat **odottaa** -tilassa. Sitten tehtävän suoritus alkaa ja sektoria siirtyy **Meneillään** tilassa. Tehtävän suorittaminen onnistuu tai epäonnistuu. Sektoria merkitään **valmiiksi**' tai suorittamisen tuloksen perusteella **epäonnistui** . 

Voit palauttaa sektoria voit siirtyä takaisin **Valmis** -tai **epäonnistui** **odottaa** -tilassa. Voit myös merkitä **Ohita**, joka estää tehtävän suorittaminen sektoria valtion ja käsittele sektoria.


## <a name="manage-pipelines"></a>Putkistot hallinta
Voit hallita oman putkistot Azure PowerShellin avulla. Voit esimerkiksi keskeyttäminen ja jatkaminen putkistot suorittamalla Azure PowerShellin cmdlet-komennot. 

### <a name="pause-and-resume-pipelines"></a>Keskeyttäminen ja jatkaminen putkistot
Voit Keskeytä tai keskeyttää putkistot **Keskeytys AzureRmDataFactoryPipeline** Powershell-cmdlet-komennolla. Tämä cmdlet-komento on hyödyllinen, jos et halua suorittaa oman putkistot vasta sitten, kun ongelma on kiinteä.

Esimerkki: seuraavassa ruudussa koodin, on havaittu ongelma **PartitionProductsUsagePipeline** **productrecgamalbox1dev** tiedot factory-kanssa ja haluat keskeyttää putkijohto.

![Myyntijakso keskeytetään](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Voit keskeyttää putkijohto suorittamalla seuraavan PowerShell-komennon:

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Esimerkki:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Kun ongelma on korjattu **PartitionProductsUsagePipeline**, keskeytetty myyntijakso voidaan jatkaa suorittamalla seuraavan PowerShell-komennon:

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Esimerkki:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## <a name="debug-pipelines"></a>Virheenkorjaus putkistot
Azure Data Factory tarjoaa monipuolisia Azure yritysportaalin ja korjata virheet ja vianmääritys putkistot PowerShellin Azure kautta.

### <a name="find-errors-in-a-pipeline"></a>Virheiden etsiminen putkijohto
Jos tehtävän suorittaminen epäonnistuu putkijohto, putkijohto tuottamien tietojoukko on virhetilassa virheen vuoksi. Voit korjata ja Azure Data Factory käyttäen seuraavat vianmääritys.

#### <a name="use-azure-portal-to-debug-an-error"></a>Azure yritysportaalin avulla voit korjata virheen:

3.  Valitse **taulukko** -sivu ongelma sektoria **tilan** määrittäminen **epäonnistui**.

    ![Taulukon-sivu, josta ongelma sektori](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.  Valitse **Tiedot-SEKTORI** -sivu tehtävän suorittaminen epäonnistui.
    
    ![Dataslice virheen vuoksi](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.  **Tehtävän suorittaminen tiedot** -sivu voit ladata HDInsight-käsittely liittyvät tiedostot. Valitse tila ja stderr lataamaan lokitiedoston, joka sisältää virheistä Lataa.

    ![Tehtävän tiedot-sivu näyttää virheen](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### <a name="use-the-powershell-to-debug-an-error"></a>Korjaa virhe PowerShellin avulla
1.  Käynnistä **Azure PowerShell**.
3.  Suorittamalla **Get-AzureRmDataFactorySlice** komento sektorit ja niiden tiloja. Raportissa pitäisi näkyä sektoria, jonka tila: **epäonnistui**.       

            Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Esimerkki:
        
            Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Korvaa **StartDateTime** StartDateTime Set-AzureRmDataFactoryPipelineActivePeriod määritetyn arvon.
4. Nyt suorittaa **Get-AzureRmDataFactoryRun** cmdlet-komennon sektoria suorittaa tehtävän tietoja.

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    Esimerkki:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    StartDateTime arvo ei ole mainittu edellisessä vaiheessa-virhe tai ongelma sektoria aloitusaika. Päivämäärän ja kellonajan pitäisi olla lainausmerkkeihin.
5.  Tulos (muistuttaa seuraavankaltaiselta) virheestä yksityiskohdilla pitäisi näkyä seuraavat:

            Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
            ResourceGroupName       : ADF
            DataFactoryName         : LogProcessingFactory3
            TableName               : EnrichedGameEventsTable
            ProcessingStartTime     : 10/10/2014 3:04:52 AM
            ProcessingEndTime       : 10/10/2014 3:06:49 AM
            PercentComplete         : 0
            DataSliceStart          : 5/5/2014 12:00:00 AM
            DataSliceEnd            : 5/6/2014 12:00:00 AM
            Status                  : FailedExecution
            Timestamp               : 10/10/2014 3:04:52 AM
            RetryAttempt            : 0
            Properties              : {}
            ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                            Jobs/841b77c9-d56c-48d1-99a3-
                        8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                        more details.
            ActivityName            : PigEnrichLogs
            PipelineName            : EnrichGameLogsPipeline
            Type                    :
    
    
6.  Voit suorittaa **Tallenna AzureRmDataFactoryLog** cmdlet-komennon tunnus-arvolla näkyviin tulosteesta ja lataa käyttämällä cmdlet **-DownloadLogsoption** lokitiedostot.

            Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## <a name="rerun-failures-in-a-pipeline"></a>Suorita virheiden putkijohto

### <a name="using-azure-portal"></a>Azure-portaalissa

Vianmääritys ja putkijohto virheiden korjaaminen, kun olet suorittamalla virheet siirtyminen virhe sektoria ja napsauttamalla komentopalkista **Suorita** -painiketta.

![Suorita on epäonnistunut sektori](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Siltä varalta, että sektoria on ei läpäissyt käytännön virheen vuoksi (for ex: tiedot eivät ole käytettävissä), voit korjata virheen ja vahvista osoittamalla komentopalkin **kelpoisuuden tarkistus** -painiketta uudelleen.
![Korjaa virheitä ja vahvista](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="using-azure-powershell"></a>Azure PowerShellin avulla

Voit uudelleen virheiden määrittäminen AzureRmDataFactorySliceStatus cmdlet-komennolla. Katso [Määrittäminen AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) aiheen syntaksin ja muita tietoja-cmdlet-komennolla. 

**Esimerkki:** Seuraavassa esimerkissä määrittää kaikki sektorit taulukon tilan 'DAWikiAggregatedData' odottaa-Azure data factory "WikiADF".

UpdateType on määritetty UpstreamInPipeline, mikä tarkoittaa, että kunkin taulukon sektoria ja seuraajasoluja (ylöspäin) taulukoiden tilat määritetään-odotetaan." Muut mahdolliset parametrin arvo on "Yksilön."

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## <a name="create-alerts"></a>Luo ilmoituksia
Azure kirjaa käyttäjän tapahtumat, kun Azure resurssi (esimerkiksi tietoja factory) luodaan, päivittää tai poistetaan. Voit luoda ilmoituksia näitä tapahtumia. Tietoja Factory avulla voit kerätä eri arvot ja luo ilmoituksia arvojen mukaisesti. On suositeltavaa käyttäminen tapahtumien reaaliaikaista seurantaa ja arvot tiedoksi. 

### <a name="alerts-on-events"></a>Tapahtumien ilmoitukset
Azure tapahtumat on hyödyllisiä tietoja kyselyjä Azure resurssien tapahtumat. Azure kirjaa käyttäjän tapahtumat, kun Azure resurssi (esimerkiksi tietoja factory) on luotu, päivittää tai poistaa. Azure Data Factory käytettäessä tapahtumia luodaan kun:

- Azure Data Factory on luotu tai päivittää ja poistaa.
- Tietojen käsittely (jota kutsutaan nimellä suoritetaan) on aloitettu ja valmis.
- Tarvittaessa-HDInsight-klusterin luodaan ja poistetaan.

Voit luoda ilmoituksia käyttäjän näitä tapahtumia ja määrittää niitä lähettämään sähköposti-ilmoitusten järjestelmänvalvoja- ja apuyhteyshenkilöiden Tilauksen. Lisäksi voit määrittää ylimääräisten sähköpostiosoitteiden käyttäjistä, joille haluat saada sähköposti-ilmoituksen, kun ehdot täyttyvät. Tämä ominaisuus on hyödyllinen, kun haluat saada ilmoituksen virheet etkä halua seurata jatkuvasti tietoja factory.

> [AZURE.NOTE] Portaalin ei tällä hetkellä Näytä ilmoitukset tapahtumista. Nähdä kaikki ilmoitukset [Seuranta ja hallinta-sovelluksen](data-factory-monitor-manage-app.md) avulla.

#### <a name="specifying-an-alert-definition"></a>Ilmoitusten määritys, joka määrittää:
Voit määrittää ilmoitusten määrityksen, voit luoda JSON tiedoston toiminnoista, joita haluat saada ilmoituksen joka kuvaa. Seuraavassa esimerkissä ilmoituksen lähettää sähköposti-ilmoituksen RunFinished-toiminnon. Johonkin tiettyyn sähköposti-ilmoituksen lähetetään, kun tietojen factory-asennus on valmis ja suorita epäonnistui (tilan = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

JSON-määrityksestä **alitila** voi poistaa, jos et halua saada ilmoituksen tietyn epäonnistui.

Tässä esimerkissä määrittää kaikki tiedot tehtaan tilaukseesi ilmoitusta. Ilmoitus on määritetty tietyn tietojen factory halutessasi voit määrittää tietojen factory **resourceUri** **tietolähde**:

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Seuraavassa taulukossa on luettelo käytettävissä olevat toiminnot ja tilat (ja aliraportti tilat).

Toiminnon nimi | Tila | Sub-tila
-------------- | ------ | ----------
RunStarted | Käytön aloittaminen | Aloittaminen
RunFinished | Epäonnistui / onnistui | FailedResourceAllocation<br/><br/>Onnistui<br/><br/>FailedExecution<br/><br/>Aikakatkaisu<br/><br/>< peruutettu<br/><br/>FailedValidation<br/><br/>Ilman ylläpitäjää
OnDemandClusterCreateStarted | Käytön aloittaminen
OnDemandClusterCreateSuccessful | Onnistui
OnDemandClusterDeleted | Onnistui

Saat [Ilmoituksen säännön luominen](https://msdn.microsoft.com/library/azure/dn510366.aspx) JSON-elementit, joita käytetään esimerkissä tietoja. 

#### <a name="deploying-the-alert"></a>Käyttöönotto ilmoitus 
Ottaa käyttöön ilmoituksen Azure-PowerShell-cmdlet-komennolla: **Uusi AzureRmResourceGroupDeployment**, kuten seuraavassa esimerkissä:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Kun resurssien ryhmä-käyttöönoton on suoritettu onnistuneesti, näet seuraavista ilmoituksista:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [AZURE.NOTE] Voit [Luoda ilmoituksen säännön](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST-Ohjelmointirajapinnalla ilmoitusten säännön luominen. JSON sisältö on JSON esimerkin.  

#### <a name="retrieving-the-list-of-azure-resource-group-deployments"></a>Haetaan Azure resurssin ryhmän ominaisuuksissa luettelo
Noutaa luettelon käyttöön Azure resurssiryhmä-käyttöönotoissa, käytä komentosovelmaa: **Get-AzureRmResourceGroupDeployment**, kuten seuraavassa esimerkissä:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### <a name="troubleshooting-user-events"></a>Käyttäjän tapahtumien vianmääritys


1. Näet kaikki tapahtumat, jotka on luotu jälkeen napsauttamalla **arvot ja toiminnot** -ruutua.

    ![Arvot ja toiminnot-ruutu](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)

2. Valitse **tapahtumat** -ruutu, näet tapahtumat. 

    ![Tapahtumat-ruutu](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. **Tapahtumat** -sivu voit nähdä lisätietoja tapahtumista, tapahtumien suodattaminen ja niin edelleen. 

    ![Tapahtumat-sivu](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Valitse **toiminto** , joka aiheuttaa virheen toiminnot-luettelossa.
    
    ![Valitse toiminto](./media/data-factory-monitor-manage-pipelines/select-operation.png) 
5. Valitse **virheen** tapahtuman voit tarkastella virheen tietoja.

    ![Tapahtumavirhe](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)
  

Katso [Azure tietoja cmdlet](https://msdn.microsoft.com/library/mt282452.aspx) -artikkelissa PowerShellin cmdlet-komennot, joiden avulla voit lisätä tai Hae/Poista ilmoitukset. Seuraavassa on muutamia esimerkkejä **Get-AlertRule** cmdlet-komennolla: 


    PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
        
            Properties :
            Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
            Condition   :
            DataSource :
            EventName             :
            Category              :
            Level                 :
            OperationName         : RunFinished
            ResourceGroupName     :
            ResourceProviderName  :
            ResourceId            :
            Status                : Failed
            SubStatus             : FailedExecution
            Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
            Condition   :
            Description : One or more of the data slices for the Azure Data Factory has failed processing.
            Status      : Enabled
            Name:       : ADFAlertsSlice
            Tags       :
            $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
            Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
            Location   : West US
            Name       : ADFAlertsSlice
    
    PS C:\> Get-AlertRule -res $resourceGroup

            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
            Location   : West US
            Name       : FailedExecutionRunsWest3

    PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
    
            Properties : Microsoft.Azure.Management.Insights.Models.Rule
            Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
            Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
            Location   : West US
            Name       : FailedExecutionRunsWest0

Suorita seuraavat ohjeiden etsiminen komennot, voit tarkastella tietoja ja esimerkkejä Get-AlertRule cmdlet-komento. 

    get-help Get-AlertRule -detailed 
    get-help Get-AlertRule -examples


- Jos näet hälytysten tapahtumat portaalin sivu, mutta et saa sähköposti-ilmoitusten, tarkista onko määritettyyn sähköpostiosoitteeseen on määritetty vastaanottamaan sähköpostia ulkopuolisilta lähettäjiltä. Sähköpostiasetukset on poistettu käytöstä ilmoitusten sähköpostiviestit.

### <a name="alerts-on-metrics"></a>Ilmoitukset-arvojen mukaisesti
Tietoja Factory avulla voit kerätä eri arvot ja luo ilmoituksia arvojen mukaisesti. Voit seurata sekä luoda ilmoituksia sektorit seuraavat mittaukset tiedot factory.
 
- Epäonnistuneiden suoritetaan
- Onnistuneiden suoritetaan

Nämä arvot ovat käteviä, kun ja niiden ansiosta saat yleiskatsauksen yleinen epäonnistui ja onnistuneen lohkot niiden tiedot factory. Arvot ovat lähettämän aina, kun on sektoria Suorita. Päälle tunnin nämä arvot koostetaan ja miten tallennustilan-tiliisi. Siksi, jotta arvot tallennustilan tilin määrittäminen.


#### <a name="enabling-metrics"></a>Ottaminen käyttöön arvot:
Arvot, valitse Seuraava Data Factory-sivu:

**Seurannan** -> **metrijärjestelmä** -> **Diagnostiikan asetusten** -> **Diagnostiikan**

![Diagnostiikka-linkki](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

Valitse **Diagnostiikan** sivu **napsauttamalla** ja tallennustilaa-tili ja Tallenna.

![Diagnostiikka-sivu](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Kun tallennettu, voi kestää jopa tunnin arvot olevan näkyvissä seuranta-sivu, koska mittarit kooste tapahtuu kerran tunnissa.


### <a name="setting-up-alert-on-metrics"></a>Arvojen mukaisesti ilmoituksen määrittäminen:

Valitse **Data Factory arvot** -sivu: 

![Tietoja factory arvot-ruutu](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Valitse **+ Lisää ilmoitus** työkalurivin **metrijärjestelmä** -sivu. 
![Tietoja factory metrisillä sivu - Lisää ilmoitus](./media/data-factory-monitor-manage-pipelines/add-alert.png)

**Ilmoitusten säännön lisääminen** -sivulla seuraavat toimet ja valitse **OK**.
 
- Ilmoituksen nimi (Esimerkki: epäonnistui ilmoitus).
- Kirjoita kuvaus ilmoituksen (Esimerkki: lähettää sähköpostia, kun tapahtuu virhe).
- Valitse arvo (epäonnistunut käynnistyy ja onnistuneen suoritetaan).
- Määritä ehto ja raja-arvo.   
- Määritä ajanjakso. 
- Määrittää, onko omistajat, osallistujat ja lukijat lähetetään sähköpostitse.
- ja paljon muuta. 

![Tietoja factory metrisillä sivu - Lisää ilmoitus](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Kun hälytyksen on lisätty, sivu sulkeutuu ja **metrijärjestelmän** sivulla näkyy uusi ilmoitus. 

![Tietoja factory metrisillä sivu - Lisää ilmoitus](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Ilmoitusten määrää pitäisi näkyä myös **ilmoitukset** -ruutu. Valitse **ilmoitukset** -ruutu.

![Tietoja factory metrisillä-sivu - ilmoitusten säännöt](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

Näet kaikki tallennetut hälytykset **ilmoitukset** -sivu. Jos haluat lisätä ilmoituksen, työkaluriviltä **Lisää ilmoitus** .

![Ilmoitusten sääntöjä-sivu](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Ilmoitukset:
Kun hälytyksen vastaa ehtoa, hanki ilmoitusten aktivoitu sähköpostiviestin. Kun ongelma on ratkaistu ja ilmoitusten ehto ei vastaa mitään muuta, näyttöön tulee ilmoitus ratkaistu sähköposti.

Toiminta on erilainen kuin tapahtumat jossa ilmoitus lähetetään jokaisen virheen mitä ilmoitusten säännön saa.

### <a name="deploying-alerts-using-powershell"></a>Käyttöönotto ilmoitukset PowerShellin avulla
Voit ottaa arvot ilmoitusten samalla tavalla kuin tapahtumat. 

**Ilmoitusten määritys:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
Korvaa subscriptionId, resourceGroupName ja dataFactoryName otoksessa tarvittavat arvot.

nyt saavuttaman *metricName* tukee kaksi arvoa:
- FailedRuns
- SuccessfulRuns

**Käyttöönotto ilmoitus:**

Ottaa käyttöön ilmoituksen Azure-PowerShell-cmdlet-komennolla: **Uusi AzureRmResourceGroupDeployment**, kuten seuraavassa esimerkissä:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Pitäisi näkyä seuraavan viestin onnistuneen käyttöönoton jälkeen:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


Voit käyttää myös **Lisää AlertRule** cmdlet-komento ilmoitusten säännön käyttöönotto. Saat lisätietoja ja esimerkkejä [Lisää AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) aiheessa.  

## <a name="move-data-factory-to-a-different-resource-group-or-subscription"></a>Siirrä tiedot factory eri resurssiryhmä tai -Tilaustani?
Voit siirtää tietoja factory eri resurssiryhmä tai eri tilauksen käyttämällä **siirtäminen** komentopainike palkin tietojen factory aloitussivulla. 

![Siirrä tiedot factory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Voit myös siirtää kaikki liittyvät resurssit (esimerkiksi tietoja factory liittyvät ilmoitukset) sekä tiedot valmiiksi.

![Siirrä resurssit-valintaikkuna](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
