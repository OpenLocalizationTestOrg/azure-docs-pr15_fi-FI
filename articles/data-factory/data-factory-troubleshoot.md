<properties 
    pageTitle="Azure Data Factory-ongelmien vianmääritys" 
    description="Lue, miten käyttämällä Azure Data Factory ongelmien vianmääritystä." 
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
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Data Factory-ongelmien vianmääritys
Tässä artikkelissa on vianmääritysvihjeitä ongelmien Azure Data Factory käytettäessä. Tässä artikkelissa ei Näytä kaikki mahdolliset ongelmat-palvelun avulla, mutta se käsitellään joitakin ja Yleiset vianmääritysvihjeitä.   

## <a name="troubleshooting-tips"></a>Vianmääritysvihjeitä

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Virhe: Tilaus ei ole rekisteröity käyttämään nimitilan 'Microsoft.DataFactory'
Jos näyttöön tulee seuraava virheilmoitus, Azure Data Factory resurssin palvelu ei ole rekisteröity tietokoneeseesi. Toimi seuraavasti: 

1. Käynnistä Azure PowerShell. 
2. Kirjaudu sisään käyttämällä seuraavaa komentoa Azure-tiliisi.
        Kirjaudu sisään AzureRmAccount 
3. Suorita seuraava komento rekisteröi Azure Data Factory-palvelu.
        Rekisteröi AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Ongelma: Luvattoman virhe, kun Data Factory cmdlet-komennon suorittaminen
Ei ehkä ole käytät oikealle Azure-tili tai tilauksen Azure-PowerShellin avulla. Valitse oikea Azure-tili ja tilauksen PowerShellin Azure käytettäväksi seuraavat cmdlet-komennot avulla. 

1. Kirjaudu sisään-AzureRmAccount - käyttö oikea Käyttäjätunnus ja salasana
2. Get-AzureRmSubscription - tarkastella kaikkien tilausten tilin. 
3. Valitse AzureRmSubscription &lt;tilauksen nimi&gt; -oikean tilaus. Käytä samoja jokin voit luoda tietojen factory Azure-portaalissa.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Ongelma: Käynnistä Azure-portaalista Data Management Gatewayn Express asennus ei onnistu
Data Management Gateway Express asetusten tarvitaan Internet Explorerin tai Microsoft ClickOnce yhteensopiva selain. Jos pika-asennus ei käynnisty, toimi seuraavasti: 

- Käytä Internet Explorer tai Microsoft ClickOnce yhteensopiva selain.

    Jos käytät Chrome, siirry [Chrome-web-säilön](https://chrome.google.com/webstore/), Etsi "ClickOnce-avainsanalla, valitse jokin ClickOnce-laajennukset ja asenna se. 
    
    Toimi samoin Firefox (Asenna laajennus). Avaa valikko-painiketta (kolme vaakaviivat oikeassa alakulmassa)-työkalurivin lisäosat, Etsi "ClickOnce-avainsanalla, jokin ClickOnce-laajennukset ja asenna se. 

- Käytä samaa sivu-portaalissa näkyvä **Manuaalinen määritys** -linkkiä. Tämän menetelmän avulla voit ladata asennustiedosto ja suorittaa sen myös manuaalisesti. Kun asennus on valmis, näyttöön tulee Data Management Gateway Configuration-valintaikkunan. Kopioi **avaimen** portaalin näytön ja käyttää sitä hallintatoiminnon manuaalisesti Rekisteröi yhdyskäytävä-palvelussa.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Ongelma: Yhteyden muodostaminen paikalliseen SQL Server-tietokantaan 
Käynnistä yhdyskäytävän tietokoneen **Data Management Gatewayn määritysten hallinta** ja Testaa yhteys SQL Server yhdyskäytävän tietokoneesta **vianmääritys** -välilehden avulla. Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Ongelma: Syötteen sektoreiden ovat odottaa, että joskus tila

Sektorien voitu **odottaa** -tilassa eri syistä. Yksi tavallisimmista syistä on, että **ulkoinen** ominaisuutta ei ole määritetty **Tosi**. Minkä tahansa tietojoukko, joka on valmistettu kuulu Azure Data Factory merkitään **ulkoisen** ominaisuus. Tämä ominaisuus osoittaa, että tiedot ovat ulkoisista ja varmuuskopioidut ei sisällä tietoja factory minkä tahansa putkistot mukaan. Tietojen sektorit merkitään **valmiiksi** , kun tiedot on saatavilla vastaaviin Storessa. 

Katso **ulkoisen** ominaisuuden seuraavassa esimerkissä käyttöä varten. Voit halutessasi määrittää **externalData*** kun määrität ulkoisen tosi.

Katso lisätietoja tämän ominaisuuden [tietojoukkoja](data-factory-create-datasets.md) -artikkelissa.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Virheiden korjaamiseksi **ulkoisen** -ominaisuus ja valinnainen **externalData** -osan lisääminen syötetaulukko JSON määrityksessä ja Luo taulukko. 

### <a name="problem-hybrid-copy-operation-fails"></a>Ongelma: Hybrid kopiointi epäonnistuu
Katso [yhdyskäytävän ongelmien vianmääritystä](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) ohjeita / kaupasta paikallisen tiedot käyttämällä Data Management Gateway kopioiminen ongelmien vianmääritystä. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Ongelma: Tarvittaessa HDInsight epäonnistuu valmistelu
Linkitetyn palvelua tyypin HDInsightOnDemand käytettäessä sinun täytyy määrittää linkedServiceName, joka osoittaa Azure-Blob-objektien tallennustilaan. Tietojen Factory-palvelu käyttää tätä tallennuspaikkaa lokit ja tarvittaessa HDInsight-klusterin tukitiedostot tallentamiseen.  Joskus valmistelu tarvittaessa-HDInsight-klusterin epäonnistuu ja seuraava virhesanoma:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Tämä virhe ilmaisee yleensä määritetyn linkedServiceName-tallennustilan tilin sijainti ei paikallaan tietojen center missä HDInsight valmistelu tapahtuu. Esimerkki: Jos tietojen factory on Länsi US ja Azure-tallennustila on Itä Yhdysvalloissa, tarvittaessa valmistelu epäonnistuu Länsi Yhdysvalloissa.

Lisäksi on toinen JSON-ominaisuuden additionalLinkedServiceNames missä lisätallennustilaa tilejä voi erikseen tarvittaessa Hdinsightista. Linkitetyn lisätallennustilaa tilit on oltava samassa sijainnissa kuin HDInsight-klusterin tai saman virheen epäonnistuu.

### <a name="problem-custom-net-activity-fails"></a>Ongelma: Mukautettu .NET tehtävän epäonnistuu
Saat lisätietoja kohdasta [Virheenkorjaus myyntijakso, jossa on mukautetut aktiviteetit](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Azure-portaalin käyttäminen vianmääritys 

### <a name="using-portal-blades"></a>Portaalin lavat käyttäminen
Katso [näytön myyntijakso](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) vaiheet. 

### <a name="using-monitor-and-manage-app"></a>Valvonnan käyttäminen ja hallinta-sovelluksella
Katso [näyttö ja hallinta tietojen factory putkistot näyttö ja hallita sovelluksen](data-factory-monitor-manage-app.md) lisätietoja. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Azure PowerShellin vianmääritys

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Azure PowerShellin virheen vianmääritys  
[Näytön Data Factory putkistot PowerShellin Azure](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) lisätietoja. 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 