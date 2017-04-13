<properties 
    pageTitle="Logiikan sovellusten hinnat mallin | Microsoft Azure" 
    description="Lisätietoja hinnat toiminta logiikan-sovelluksissa" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logiikan sovellusten hinnat malli

Azure logiikan-sovellusten avulla voit skaalata ja suorita työnkulkuja integroinnin pilveen.  Alla on tietoja laskutus ja palvelupaketin hinnoittelua logiikan sovellusten.

## <a name="consumption-pricing"></a>Kulutus hinnat

Vastaluotu logiikan sovellusten käyttää kulutus suunnitelma. Logiikan sovellusten kulutus hinnoittelu mallia maksat vain käyttötarkoitus.  Logiikan sovelluksia ei ole rajoittanut, kulutus suunnitelma käytettäessä.
Kaikki toiminnot suoritetaan Suorita logiikkaa esiintymän sovelluksen käytön mukaan laskutettavat.

### <a name="what-are-action-executions"></a>Mitä toiminto suorituskerran?

Jokaisen vaiheen logiikan sovelluksen määrityksessä on toiminto.  Tämä sisältää käynnistimien, ohjausobjektin työnkulun vaiheet kuin ehdot, kunkin silmukoita vaikutusalueet, tee silmukoita puhelut yhdysviivat ja kutsujen alkuperäisen toiminnot asti.
Käynnistimien ovat vain erityisiä toimintoja, jotka on suunniteltu vahvistaa logiikan sovelluksen uuden esiintymän tietyn tapahtuman yhteydessä.  On useita erilaisia toimintamalleja käynnistimien, jotka voivat vaikuttaa miten logiikan-sovellus on käytön mukaan laskutettavat.

-   Päätepisteen **Kyselyt käynnistimen** – käynnistimen jatkuvasti tekee kyselyn, ennen kuin se vastaanottanut viestin, joka täyttää ehdot logiikan sovelluksen uuden esiintymän luomiseen.  Kyselyväliä voi määrittää suunnittelussa logiikan sovellusten käynnistin.  Kysely sivupyynnön, vaikka ei luoda uuden esiintymän logiikan-sovelluksessa saavat arvon toiminnon suorittaminen.

-   **Webhook käynnistimen** – Tämä käynnistin odottaa asiakas voi lähettää sen tiettyyn päätepisteen pyyntö.  Kunkin webhook päätepisteen lähetetään pyyntö saa arvon toiminnon suorittaminen. Pyynnön ja HTTP Webhook käynnistin ovat molemmat webhook käynnistimien.

-   **Toistuminen käynnistimen** – Tämä käynnistin Luo uuden esiintymän logiikan sovelluksen käynnistin määritetyissä toistovälin perusteella.  Esimerkiksi toistuminen käynnistimen määritetty käyttämiseen 3 päivän välein tai jopa joka minuutti.

Käynnistimen suorituskerran näkevät käynnistimen historia-osassa logiikan sovellusten resurssi-sivu.

Kaikki toiminnot, jotka on suoritettu, onko siirretyt onnistuneesta tai epäonnistui on käytön mukaan laskutettavat kuin toiminnon suorittaminen.  Toiminnot, jotka on ohitettu vuoksi ehto ei täyty tai toiminnot, joita ei voi suorittaa, koska logiikan sovellus lopetettu ennen valmistumista ei lasketa toiminnon suorituskerran nimellä.

Toiminnot suoritetaan silmukoita lasketaan iteraation silmukan kohden.  Esimerkiksi yksi toiminto-saat kunkin silmukan läpikäyminen 10 kohteiden luettelo lasketaan luettelon (10) tapahtuu (1) toimintojen määrä kerrottuna määränä plus yksi aloittamisesta silmukan joka tässä esimerkissä olisi (10 * 1) + 1 = 11 toiminnon suorituskerran.

Logiikan sovellukset, jotka eivät ole käytettävissä voi olla uusia esiintymiä vahvistaa ja sen vuoksi, ne eivät ole käytettävissä tänä aikana Hae peritä.  Olla tietoisia, kun logiikan-sovellus on poistettu käytöstä saattaa kestää hieman aikaa esiintymät pysäytystilaan ennen poistetaan kokonaan käytöstä.

## <a name="app-service-plans"></a>Sovelluksen palvelusopimusten vaihtoehdot

Sovelluksen palvelun suunnitelmien ei enää tarvitse logiikan sovelluksen luominen.  Voit luoda viittauksen-sovelluksen palvelun suunnitteleminen logiikan-sovelluksella.  Logiikan-sovellukset, jotka on aiemmin luotu sovellus-palvelusopimus säilyvät toimivat kuin ennen, suunnitelman mukaan valittu saada rajoittanut päivittäinen suorituskerran määrä on ylitetty ja ei laskuteta avulla toiminto suorittamisen jälkeen.

Sovelluksen palvelun suunnitelmien ja niiden päivittäinen sallitun toiminnon suorituskerran:

| |Vapaa/jaetut/Basic|Vakio|Premium|
|---|---|---|---|
|Toiminnon suorituskerran päivässä| 200|10 000|50 000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Sovelluksen palvelun suunnitteleminen hinnat kulutus muuntaminen

Viittaus-sovelluksen palvelun suunnitteleminen kulutus logiikan sovelluksen, voit yksinkertaisesti [Suorita PowerShell-komentosarjaa alla](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Varmista, että sinun on asennettu [PowerShellin Azure-Työkalut](https://github.com/Azure/azure-powershell) .

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Siirtyminen App palvelun suunnitteleminen hinnat kulutus

Jos haluat muuttaa logiikan-sovellusta, joka on-sovelluksen palvelun suunnitteleminen kulutus malliin liittyy poista sovellus-palvelun suunnittelu viittaus logiikan sovelluksen määrityksessä.  Tämän voi tehdä PowerShell-cmdlet-kutsun kanssa:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Hinnat

Hinnat tiedot on [Logiikan sovellusten hinnat](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Logiikan sovellusten yleiskatsaus][whatis]
- [Ensimmäinen logiikan-sovelluksen luominen][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

