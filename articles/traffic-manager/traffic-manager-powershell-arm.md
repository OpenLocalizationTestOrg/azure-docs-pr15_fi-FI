<properties
    pageTitle="Azure Resurssienhallinta tuki liikenteen hallinta | Microsoft Azure "
    description="PowerShellin for liikenteen hallinta ja Azure resurssien hallinta"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Resurssienhallinta tuki Azure liikenteen hallinta

Azure Resurssienhallinta on ensisijainen hallintaliittymään palveluiden Azure-tietokannassa. Azure liikenteen hallinta-profiileista voi hallita Azure Resurssienhallinta-pohjainen API ja työkalujen avulla.

## <a name="resource-model"></a>Resurssin malli

Azure liikenteen hallinta on määritetty kerääminen kutsuttuja liikenteen hallinta profiilin asetuksia. Tämän profiilin on DNS-asetukset, tietoliikenteen reititys asetuksia, päätepisteen valvonta-asetukset ja johon liikenne reititetään päätepisteiden luettelo.

Kunkin liikenteen hallinta profiilin edustaa resurssin lajin "TrafficManagerProfiles". REST API tasolla kunkin profiilin URI on seuraavanlainen:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Azure liikenteen hallinta perinteinen Ohjelmointirajapinnan vertailu

Azure Resurssienhallinta-tuki liikenteen hallinta käyttää erilaista terminologiaa kuin perinteinen käyttöönottomalli. Seuraavassa taulukossa on Resurssienhallinta ja perinteinen termejä erot:

| Resurssienhallinta termi | Perinteinen termi |
|-----------------------|--------------|
| Liikenne reititys menetelmä | Kuormituksen tasaamisen menetelmä |
| Prioriteetti-menetelmällä | Automaattisesti menetelmä |
| Painotetun menetelmän | Pyöreän pöydän menetelmä |
| Suorituskyvyn menetelmä | Suorituskyvyn menetelmä |

Asiakaspalautteen perusteella olemme muuttaa parantaa selkeyttä ja vähentää yleisiä väärinkäsityksiä termejä. Ei-toiminto ei ole ero.

## <a name="limitations"></a>Rajoitukset

Viittaat päätepisteen tyyppi 'AzureEndpoints' Web-sovelluksen liikenteen hallinta päätepisteet voi viitata oletusarvo (tuotannon) [Web App-paikka](../app-service-web/web-sites-staged-publishing.md). Mukautetun paikkojen ei tueta. Voit kiertää tämän ongelman mukautetun paikkojen voi määrittää käyttämällä 'ExternalEndpoints' tyyppi.

## <a name="setting-up-azure-powershell"></a>PowerShellin Azure määrittäminen

Microsoft Azure PowerShellin käyttäminen päällekkäisten näitä ohjeita. Seuraavassa artikkelissa kerrotaan, miten voit asentaa ja määrittää PowerShellin Azure.

- [Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)

Tämän artikkelin Esimerkeissä oletetaan, että aiemmin resurssiryhmä. Voit luoda resurssiryhmä käyttämällä seuraava komento:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät sijainti. Tämä sijainti käytetään oletusarvon mukaan luodut resurssin kyseisen ryhmän resurssit. Koska liikenteen hallinta profiilin resurssit ovat yleistä, ei alueellisen, resurssin ryhmän sijainti valinta ei ole vaikutusta Azure liikenteen hallinta.

## <a name="create-a-traffic-manager-profile"></a>Liikenteen hallinta profiilin luominen

Voit luoda liikenteen hallinta profiilin käyttämällä uutta AzureRmTrafficManagerProfile cmdlet-komennon:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Seuraavassa taulukossa on kuvattu parametrit:

| Parametri | Kuvaus |
|-----------|-------------|
| Nimi | Resurssin nimi liikenteen hallinta profiilin resurssin. Saman resurssiryhmä-profiileista on oltava yksilöllinen nimi. Tämä nimi on erillinen DNS-kyselyjen DNS-nimi.|
| ResourceGroupName | Resurssiryhmä, joka sisältää profiilin resurssin nimi.|
| TrafficRoutingMethod | Määrittää liikenne reititys tavan, jolla voit selvittää, mitä päätepisteen palautetaan vastauksessa DNS-kyselyn. Mahdolliset arvot ovat 'Suorituskyvyn', 'Painotettu' tai "Prioriteetti".|
| RelativeDnsName | Määrittää DNS-nimen liikenteen hallinta profiilin myöntämä hostname-osaa. Tämä arvo on yhdistetty DNS toimialuenimi, jota käytetään mukaan Azure liikenteen hallinta lomakkeen profiilin täydellinen toimialuenimi (FQDN). Esimerkiksi "contoso-arvo muuttuu contoso.trafficmanager.net."|
| TTL (ELINAIKA) | Määrittää sekuntia DNS--elinaika (TTL). Tämä TTL ilmoittaa paikalliset DNS resolvers ja kuinka kauan välimuistin DNS-vastauksia liikenteen hallinta profiilin DNS-asiakkaille.|
| MonitorProtocol | Määrittää protokollan avulla päätepisteen kunnon valvonta. Mahdolliset arvot ovat "HTTP" ja "HTTPS".|
| MonitorPort | Määrittää käytettävä päätepisteen kunnon valvonta porttinumeroa.|
| MonitorPath | Määrittää polun suhteessa päätepisteen toimialuenimen määrittäminen päätepisteen tutkia.|

Cmdlet Luo liikenteen hallinta profiilin Azure ja palauttaa vastaavan profiilin objektin PowerShell. Tässä vaiheessa profiilin ei sisällä päätepisteet. Saat lisätietoja lisääminen päätepisteet liikenteen hallinta profiiliin [Lisääminen liikenteen hallinta päätepisteet](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Hae liikenteen hallinta-profiili

Aiemmin luodun liikenteen hallinta profiilin objektin hakemiseen Käytä Get-AzureRmTrafficManagerProfle cmdlet-komento:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Tämä cmdlet-komento palauttaa liikenteen hallinta profiilin objektin.

## <a name="update-a-traffic-manager-profile"></a>Liikenteen hallinta profiilin päivittäminen

3 vaihetta muokkaaminen liikenteen hallinta-profiileista seuraavasti:

1. Nouda käyttämällä Hae AzureRmTrafficManagerProfile profiili tai käytä uusi AzureRmTrafficManagerProfile palauttama profiilin.
2. Muokkaa profiilia. Voit lisätä ja poistaa päätepisteet tai päätepisteen tai profiilin parametrien muuttaminen. Nämä muutokset työvaiheita offline-tilassa. Muutat vain paikallisen objekti, joka edustaa profiilin muistiin.
3. Vahvista muutokset määrittäminen AzureRmTrafficManagerProfile cmdlet-komennolla.

Kaikki Profiiliominaisuuksien voi muuttaa profiilin RelativeDnsName lukuun ottamatta. Jos haluat muuttaa RelativeDnsName, sinun on poistettava profiili ja uusi profiili uudella nimellä.

Seuraavassa esimerkissä kerrotaan, miten voit muuttaa profiilin TTL:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Liittyy liikenteen hallinta päätepisteet kolmenlaisia:

1. **Azure päätepisteet** ovat Azure maksuttomien palveluiden
2. **Ulkoinen päätepisteet** ovat ulkopuolella Azure maksuttomien palveluiden
3. **Sisäkkäiset päätepisteet** avulla voidaan käyttää sisäkkäisiä hierarkiat liikenteen hallinta-profiileista. Sisäkkäiset päätepisteet käyttöön monimutkaisia sovellusten liikenne reititys määritykset Lisäasetukset.

Kaikkien kolmen tapauksissa päätepisteet voi lisätä kahdella tavalla:

1. Voit käyttää 3-vaihetta, joka on kuvattu aiemmin. Tämän menetelmän etuna on se yhteen päivityksen voi tehdä useita päätepisteen muutokset.
2. Uusi AzureRmTrafficManagerEndpoint cmdlet-komennon avulla. Tämä cmdlet-komento lisää päätepisteen aiemmin luodun liikenteen hallinta-profiilin yhdellä kertaa.

## <a name="adding-azure-endpoints"></a>Azure päätepisteet lisääminen

Azure päätepisteet viitata Azure maksuttomien palveluiden. Azure päätepisteet kolmenlaisia tuetaan:

1. Azure Web Apps-sovelluksista
2. Perinteinen' cloud services (joka voi olla PaaS palvelun tai IaaS näennäiskoneiden)
3. Azure PublicIpAddress resurssit (joka voidaan liittää kuormituksen- tai virtual machine NIC). PublicIpAddress on oltava DNS-nimeä vastuuhenkilö, jota käytetään liikenteen hallinta.

Kaikissa tapauksissa:

- Palvelu on määritetty käyttämällä Lisää AzureRmTrafficManagerEndpointConfig tai uusi AzureRmTrafficManagerEndpoint 'targetResourceId'-parametria.
- TargetResourceId implisiittinen "Kohde" ja "EndpointLocation".
- Määrittämällä 'leveyden' on valinnainen. Leveydet käytetään vain, jos profiili on määritetty 'Painotettu' liikenne reititys-menetelmää. Muussa tapauksessa ne ohitetaan. Jos määritetty, arvon on oltava luku väliltä 1 – 1 000. Oletusarvo on '1'.
- Määrittämällä 'prioriteetti-lause on valinnainen. Ensisijaiset tavoitteet käytetään vain, jos profiili on määritetty "prioriteetti-liikenne reititys-menetelmää. Muussa tapauksessa ne ohitetaan. Kelvollisia arvoja on väliltä 1 – 1 000 pienet arvot suuri prioriteetti. Jos määritetty päätepiste, ne on määritettävä kaikki päätepisteiden. Jos jätetään pois, '1' lähtien oletusarvot otetaan käyttöön päätepisteet näkyvät järjestyksessä.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Esimerkki 1: Lisääminen Web App-päätepisteet käyttämällä Lisää AzureRmTrafficManagerEndpointConfig

Tässä esimerkissä liikenteen hallinta profiilin luominen ja lisääminen Web App päätepisteet, Lisää AzureRmTrafficManagerEndpointConfig cmdlet-komennolla.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Esimerkki 2: Lisääminen käyttämällä uusi AzureRmTrafficManagerEndpoint perinteinen"pilven palvelun-päätepiste

Tässä esimerkissä 'perinteinen' pilvipalvelussa päätepisteen lisätään liikenteen hallinta profiilin. Tässä esimerkissä on määritetty profiili profiilin ja resurssien ryhmä-nimien käyttäminen sijaan profiilin objektin kulkevan. Kummassakin tuetaan.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Esimerkki 3: Lisääminen käyttämällä uutta AzureRmTrafficManagerEndpoint publicIpAddress-päätepiste

Tässä esimerkissä resurssille julkiseen IP-osoite on lisätty liikenteen hallinta-profiiliin. Julkinen IP-osoite on oltava määritetty DNS-nimen ja on sidottu AM NIC tai kuormituksen tasauspalvelun.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Ulkoisten päätepisteiden lisääminen

Liikenteen hallinta käyttää ulkoisten päätepisteiden tietueita ulkopuolella Azure-palveluihin liikenne voidaan ohjata. Azure päätepisteet, jossa ulkoisten päätepisteiden voidaan lisätä käyttämällä joko Lisää AzureRmTrafficManagerEndpointConfig perään määrittäminen AzureRmTrafficManagerProfile tai uusi AzureRMTrafficManagerEndpoint.

Kun määrität ulkoisten päätepisteiden:

- Päätepisteen toimialuenimi on määritettävä "Kohde"-parametrin avulla
- Jos 'Suorituskyvyn' liikenne reititys-menetelmää käytetään, "EndpointLocation" ei tarvita. Muussa tapauksessa on valinnainen. Arvon on oltava [kelvollinen Azure alueen nimi](https://azure.microsoft.com/regions/).
- 'Paino' ja 'Prioriteetti' ovat valinnaisia.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Esimerkki 1: Lisääminen Lisää AzureRmTrafficManagerEndpointConfig ja määritä AzureRmTrafficManagerProfile ulkoisten päätepisteiden

Tässä esimerkissä on liikenteen hallinta profiilin luominen, Lisää ulkoisen päätepisteet ja Vahvista muutokset.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Esimerkki 2: Lisääminen käyttämällä uusi AzureRmTrafficManagerEndpoint ulkoisten päätepisteiden

Tässä esimerkissä on ulkoinen päätepiste lisääminen aiemmin luotuun profiiliin. Profiilin määritetään käyttämällä profiilia ja resurssien ryhmien nimet.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>'Sisäkkäiset' päätepisteet lisääminen

Kunkin liikenteen hallinta profiilin määrittää yksittäisen liikenne reititys-menetelmällä. On kuitenkin skenaariot, jotka tarvitsevat kehittyneempiä liikenne reititys kuin reititys myöntämä yhden liikenteen hallinta profiilin. Voit sisällyttää liikenteen hallinta-profiileista, jos haluat yhdistää useamman kuin yhden liikenne reititys menetelmä edut. Sisäkkäiset profiilien avulla voit ohittaa liikenteen hallinta oletustoiminnan suurempi tuki-ja monimutkaisia sovelluksen käyttöönottoa. Katso Lisää esimerkkejä [ylemmän tason liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md).

Sisäkkäiset päätepisteet määritetään ylemmän tason profiilitietoihinsa käyttämällä tietyn päätepisteen tyyppi-"NestedEndpoints". Määritettäessä sisäkkäisiä päätepisteet:

- Päätepisteen määritettävä 'targetResourceId'-parametri
- Jos 'Suorituskyvyn' liikenne reititys-menetelmää käytetään, "EndpointLocation" ei tarvita. Muussa tapauksessa on valinnainen. Arvon on oltava [kelvollinen Azure alueen nimi](http://azure.microsoft.com/regions/).
- 'Paino' ja 'Prioriteetti' ovat valinnaisia kuin Azure päätepisteet.
- "MinChildEndpoints"-parametri on valinnainen. Oletusarvo on '1'. Jos tämä kynnysarvo käytettävissä päätepisteet määrä, ylemmän tason profiilin katsoo lapsen profiilin heikentynyt"ja nimisen-liikenne paikalliseen ylemmän tason profiilin muiden päätepisteet.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Esimerkki 1: Lisääminen sisäkkäisiä päätepisteet Lisää AzureRmTrafficManagerEndpointConfig ja määritä AzureRmTrafficManagerProfile käyttäminen

Tässä esimerkissä on luoda uusi liikenteen hallinta lapsen ja ylemmän tason profiileja, Lisää lapsen sisäkkäisiä päätepisteen ylätason ja Vahvista muutokset.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Tässä esimerkissä niitä emme ole lisännyt muiden päätepisteet ylä- tai -profiileista.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Esimerkki 2: Lisääminen käyttämällä uusi AzureRmTrafficManagerEndpoint sisäkkäisiä päätepisteet

Tässä esimerkissä on Lisää aiemmin luotuun ali-profiiliin sisäkkäisiä päätepisteen pää-profiiliin. Profiilin määritetään käyttämällä profiilia ja resurssien ryhmien nimet.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Päivitä liikenteen hallinta päätepiste

Päivitä aiemmin luotu liikenteen hallinta päätepiste kahdella tavalla:

1. Hae käyttämällä Hae AzureRmTrafficManagerProfile liikenteen hallinta profiilin päivittäminen sisällä profiilin päätepisteen-ominaisuudet ja Vahvista muutokset käyttämällä määrittäminen AzureRmTrafficManagerProfile. Tässä menetelmässä on se etu, että voit päivittää useita päätepisteen yhdellä kertaa.
2. Hae käyttämällä Hae AzureRmTrafficManagerEndpoint liikenteen hallinta päätepisteen, Päivitä päätepisteen ominaisuudet ja Vahvista muutokset käyttämällä määrittäminen AzureRmTrafficManagerEndpoint. Tämä menetelmä on helpompaa, koska se ei edellytä indeksoinnin kyselyjä profiilin päätepisteet-matriisi.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Esimerkki 1: Get-AzureRmTrafficManagerProfile ja määritä AzureRmTrafficManagerProfile päätepisteet päivittäminen

Tässä esimerkissä on Muokkaa aiemmin luotuun profiiliin sisällä päätepisteet-prioriteetin.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Esimerkki 2: Get-AzureRmTrafficManagerEndpoint ja määritä AzureRmTrafficManagerEndpoint päätepisteen päivittäminen

Tässä esimerkissä on Muokkaa aiemmin luotuun profiiliin yhteen päätepiste leveyden.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Ottaminen käyttöön ja poistaminen käytöstä päätepisteet ja profiileihin

Liikenne hallinnan avulla yksittäiset päätepisteet käytössä ja poissa käytöstä sekä ottaminen käyttöön ja poistaminen käytöstä koko-profiileista.
Nämä muutokset voi tehdä käytön/päivittäminen/asetus päätepisteen tai profiilin resurssit. Voi tehostaa näitä yleisiä toimintoja, ne tuetaan myös erillinen cmdlet-komennot kautta.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Esimerkki 1: Ottaminen käyttöön ja poistaminen käytöstä liikenteen hallinta-profiili

Voit ottaa liikenteen hallinta profiilin käyttöön ottaminen käyttöön AzureRmTrafficManagerProfile. Profiilin voidaan määrittää profiili-objekti. Profiili-objekti on välitetty putkijohto kautta vai käytätkö '-TrafficManagerProfile' parametria. Tässä esimerkissä määritetään profiili profiilin ja resurssien ryhmän nimi.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Jos haluat poistaa käytöstä liikenteen hallinta profiilin:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Poista käytöstä AzureRmTrafficManagerProfile cmdlet-komento pyytää vahvistamaan. Tämä kehote voi estää käyttämällä '-voimassa "parametria.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Esimerkki 2: Ottaminen käyttöön ja poistaminen käytöstä liikenteen hallinta päätepiste

Voit ottaa liikenteen hallinta päätepisteen käyttöön ottaminen käyttöön AzureRmTrafficManagerEndpoint. Määritä päätepisteen kahdella tavalla

1. Välitetyn kautta putkijohto TrafficManagerEndpoint-objektin tai käyttämisen '-TrafficManagerEndpoint' parametri
2. Käyttämällä päätepisteen nimi, päätepisteen tyyppi, profiilinimi ja resurssiryhmän nimi:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Vastaavasti liikenteen hallinta päätepisteen poistaminen käytöstä:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Poista käytöstä-AzureRmTrafficManagerProfile, jossa käytöstä AzureRmTrafficManagerEndpoint cmdlet-komento pyytää vahvistamaan. Tämä kehote voi estää käyttämällä '-voimassa "parametria.

## <a name="delete-a-traffic-manager-endpoint"></a>Liikenteen hallinta päätepisteen poistaminen

Jos haluat poistaa yksittäisiä päätepisteet, käytä Poista AzureRmTrafficManagerEndpoint cmdlet-komennon:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Tämä cmdlet-komento pyytää vahvistamaan. Tämä kehote voi estää käyttämällä '-voimassa "parametria.

## <a name="delete-a-traffic-manager-profile"></a>Poista liikenteen hallinta-profiili

Jos haluat poistaa liikenteen hallinta profiilin, käytä Poista AzureRmTrafficManagerProfile cmdlet-komento, profiilin ja resurssien ryhmä-nimien määrittäminen:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Tämä cmdlet-komento pyytää vahvistamaan. Tämä kehote voi estää käyttämällä '-voimassa "parametria.

Profiilin poistetaan voidaan määrittää myös käyttämällä profiilia-objekti:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Tässä järjestyksessä piped myös voit:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Seuraavat vaiheet

[Liikenteen hallinta seuranta](traffic-manager-monitoring.md)

[Liikenteen hallinta suorituskykyyn liittyviä tietoja](traffic-manager-performance-considerations.md)
