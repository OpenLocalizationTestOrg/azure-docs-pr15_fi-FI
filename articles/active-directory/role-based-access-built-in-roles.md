<properties
    pageTitle="RBAC: Valmiit roolit | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan myös sisäinen roolit Roolipohjainen käyttöoikeuksien valvonta (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Valmiit roolit

Azure Roolipohjainen käytön hallinta (RBAC) sisältää seuraavat valmiit roolit, joilla voi määrittää käyttäjät, ryhmät ja palveluja. Et voi muokata valmiita roolien määritykset. Voit luoda kuitenkin [Mukautettu roolien Azure RBAC](role-based-access-control-custom-roles.md) tietyn organisaation tarpeita.

## <a name="roles-in-azure"></a>Azure-roolien

Seuraavassa taulukossa on valmiita roolit lyhyet kuvaukset. Napsauttamalla sen nimeä Nähdäksesi yksityiskohtainen luettelo **toiminnoista** ja **notactions** oikeuksia. **Toiminnot** -ominaisuus määrittää sallitut toiminnot Azure resurssit. Toiminnon merkkijonoja käyttämällä yleismerkkejä. **Notactions** -ominaisuus määrittää toiminnot, jotka eivät sisälly sallitut toiminnot.

>[AZURE.NOTE] Azure roolimäärityksiä jatkuvasti kehittyville. Tässä artikkelissa on ajan tasalla niin kuin on mahdollista, mutta löytyvät aina uusimmat roolit määritelmiä PowerShellin Azure. Käytä Cmdlet-komentoja `(get-azurermroledefinition "<role name>").actions` tai `(get-azurermroledefinition "<role name>").notactions` tarvittaessa.

| Roolinimi | Kuvaus |
| --------- | ----------- |
| [API Management-palvelun avustaja](#api-management-service-contributor) | Voit hallita API hallintapalvelut |
| [Hakemuksen tiedot osan osallistujan](#application-insights-component-contributor) | Voit hallita sovelluksen tiedot-osat |
| [Automaatio-operaattori](#automation-operator) | Voit aloittaa, lopettaa, keskeyttää ja jatkaa töitä |
| [BizTalk avustaja](#biztalk-contributor) | Voit hallita BizTalk palvelut |
| [ClearDB MySQL DB avustaja](#cleardb-mysql-db-contributor) | Voit hallita ClearDB MySQL-tietokannat |
| [Avustaja](#contributor) | Voit hallita kaikki muu paitsi access. |
| [Tietojen Factory osallistuja](#data-factory-contributor) | Voit luoda ja hallita tietojen tehtaan ja alatason resurssien niiden välillä. |
| [DevTest harjoituksia käyttäjä](#devtest-labs-user) | Voit tarkastella kaikkea ja yhteyden, Käynnistä uudelleen ja Sammuta virtual koneet |
| [DNS-vyöhyke avustaja](#dns-zone-contributor) | Voit hallita DNS-vyöhykkeet ja tietueet |
| [DocumentDB tilin avustaja](#documentdb-account-contributor) | Voit hallita DocumentDB tilit |
| [Älykäs järjestelmien tilin avustaja](#intelligent-systems-account-contributor) | Voit hallita älykkäitä järjestelmiä-tileistä |
| [Verkon avustaja](#network-contributor) | Voit hallita kaikkia verkkoresursseja |
| [Uusi Relic APM tilin avustaja](#new-relic-apm-account-contributor) | Voit hallita uuden Relic sovellusten suorituskykyä hallinta asiakkaat ja sovellukset |
| [Omistaja](#owner) | Voit hallita kohteiden kaikki, mukaan lukien käyttö |
| [Lukija](#reader) | Voit tarkastella kaikkea, mutta ei voi tehdä muutoksia |
| [Redis välimuistin avustaja](#redis-cache-contributor) | Voit hallita Redis.txt tallentaa |
| [Ajoituksen työn sivustokokoelmat avustaja](#scheduler-job-collections-contributor) | Voit hallita ajoituksen työn sivustokokoelmat |
| [Search-palvelun avustaja](#search-service-contributor) | Voit hallita Etsi palveluita |
| [Suojauksen hallinta](#security-manager) | Voit hallita suojauksen osat, suojauskäytäntöjä ja näennäiskoneiden |
| [SQL DB avustaja](#sql-db-contributor) | Voit hallita SQL-tietokantoja, mutta ei niiden tietoturvaan liittyvät käytännöt |
| [SQL-suojauksen hallinta](#sql-security-manager) | Voit hallita tietoturvaan liittyvät käytännöt ja SQL Server-tietokannat |
| [SQL Server avustaja](#sql-server-contributor) | Voit hallita SQL Server- ja tietokantoja, mutta ei niiden tietoturvaan liittyvät käytännöt |
| [Perinteinen tallennustilan tilin avustaja](#classic-storage-account-contributor) | Voit hallita perinteinen tallennustilan tilit |
| [Tallennustilan tilin avustaja](#storage-account-contributor) | Voit hallita tallennustilan tilit |
| [Käyttäjän Access-järjestelmänvalvoja](#user-access-administrator) | Voit hallita Azure resurssien käytön |
| [Perinteinen virtuaalikoneen avustaja](#classic-virtual-machine-contributor) | Voit hallita perinteinen näennäiskoneiden, mutta ei virtual verkko- tai tili, johon ne on kytketty |
| [Virtuaalikoneen avustaja](#virtual-machine-contributor) | Voit hallita näennäiskoneiden, mutta ei virtual verkko- tai tili, johon ne on kytketty |
| [Perinteinen verkon avustaja](#classic-network-contributor) | Voit hallita perinteinen virtual verkkojen ja varattu IP-osoitteet |
| [Web-suunnitelma avustaja](#web-plan-contributor) | Voit hallita web-Palvelupaketit |
| [Sivuston avustaja](#website-contributor) | Voit hallita sivustot, mutta ei web-palvelupakettia, johon ne on kytketty |

## <a name="role-permissions"></a>Roolin oikeudet
Seuraavassa taulukossa kuvataan rooleille tietyt käyttöoikeudet. Tämä voi sisältää **Toiminnot**, jotka antavat käyttöoikeudet, ja **NotActions**, joka rajoittaa niitä.

### <a name="api-management-service-contributor"></a>API Management-palvelun avustaja
Voit hallita API hallintapalvelut

| **Toiminnot** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Voit luoda ja hallita API hallintapalvelut |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue roolit ja roolimäärityksiä |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="application-insights-component-contributor"></a>Hakemuksen tiedot osan osallistujan
Voit hallita sovelluksen tiedot-osat

| **Toiminnot** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolimäärityksiä |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.Insights/components/* | Voit luoda ja hallita havainnollistamisen osat |
| Microsoft.Insights/webtests/* | Voit luoda ja hallita web-testiä |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="automation-operator"></a>Automaatio-operaattori
Voit aloittaa, lopettaa, keskeyttää ja jatkaa töitä

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolimäärityksiä |
| Microsoft.Automation/automationAccounts/jobs/read | Lue automaatio tilin töitä |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Automaatio-tilin työn jatkaminen |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Lopeta automaatio-tilin työ |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Lue automaatio tilin työn virtaa |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Keskeyttää automaatio-tilin työ |
| Microsoft.Automation/automationAccounts/jobs/write | Kirjoita automaatio tilin töitä |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Lue automaatio raporttimallin työ |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Lue automaatio raporttimallin työ |
| Microsoft.Automation/automationAccounts/read | Lue automaatio-tilit |
| Microsoft.Automation/automationAccounts/runbooks/read | Lue automaatio runbooks |
| Microsoft.Automation/automationAccounts/schedules/read | Lue automaatio raporttimalleja |
| Microsoft.Automation/automationAccounts/schedules/write | Kirjoita automaatio raporttimalleja |
| Microsoft.Insights/components/* | Voit luoda ja hallita havainnollistamisen osat |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="biztalk-contributor"></a>BizTalk avustaja
Voit hallita BizTalk palvelut

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolimäärityksiä |
| Microsoft.BizTalkServices/BizTalk/* | Voit luoda ja hallita BizTalk palvelut |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB avustaja
Voit hallita ClearDB MySQL-tietokannat

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolimäärityksiä |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän käyttöönotoissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |
| successbricks.cleardb/Databases/* | Voit luoda ja hallita ClearDB MySQL-tietokannat |

### <a name="contributor"></a>Avustaja
Voit hallita kaikki muu paitsi käyttö

| **Toiminnot** ||
| ------- | ------ |
| * | Voit luoda ja hallita kaikenlaisten resurssit |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Ei voi poistaa roolit ja roolimäärityksiä |  
| Microsoft.Authorization/*/Write | Voi luoda roolit ja roolimäärityksiä |

### <a name="data-factory-contributor"></a>Tietojen Factory osallistuja
Luoda ja hallita tietojen tehtaan ja alatason resurssien niiden välillä.

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.DataFactory/dataFactories/* | Luoda ja hallita tietojen tehtaan ja alatason resurssien niiden välillä. |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="devtest-labs-user"></a>DevTest harjoituksia käyttäjä
Voit tarkastella kaikkea ja yhteyden, Käynnistä uudelleen ja Sammuta virtual koneet

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Compute/availabilitySets/read | Lue käytettävyys joukot ominaisuudet |
| Microsoft.Compute/virtualMachines/*/read | Lue ominaisuuksien virtual machine (AM koot, suorituksenaikainen tila, AM tunnisteet jne.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Poista varaus näennäiskoneiden |
| Microsoft.Compute/virtualMachines/read | Lue virtual machine ominaisuudet |
| Microsoft.Compute/virtualMachines/restart/action | Käynnistä näennäiskoneiden |
| Microsoft.Compute/virtualMachines/start/action | Käynnistä näennäiskoneiden |
| Microsoft.DevTestLab/*/read | Lue kurssin ominaisuudet |
| Microsoft.DevTestLab/labs/createEnvironment/action | Luoda |
| Microsoft.DevTestLab/labs/formulas/delete | Poista kaavat |
| Microsoft.DevTestLab/labs/formulas/read | Lue kaavat |
| Microsoft.DevTestLab/labs/formulas/write | Lisätä tai muokata kaavoja |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Kurssin käytännöt arvioiminen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Liittyminen kuormituksen tasauspalvelun Taustajärjestelmä osoite resurssivarantoon |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Liittyminen kuormituksen tasauspalvelun liikenteen NAT sääntö |
| Microsoft.Network/networkInterfaces/*/read | Lue verkon-liittymän (esimerkiksi kaikki kuormituksen tasoitusmääritykset, verkkoliittymän kuuluu) ominaisuudet |
| Microsoft.Network/networkInterfaces/join/action | Liittyminen Virtual Machine verkko-käyttöliittymä |
| Microsoft.Network/networkInterfaces/read | Lue verkon liityntäkohdat |
| Microsoft.Network/networkInterfaces/write | Kirjoita verkon liityntäkohdat |
| Microsoft.Network/publicIPAddresses/*/read | Lue julkisen IP-osoitteen ominaisuudet |
| Microsoft.Network/publicIPAddresses/join/action | Liittyminen julkiseen IP-osoite |
| Microsoft.Network/publicIPAddresses/read | Lue verkon julkiseen IP-osoitteet |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtuaalinen verkkoon |
| Microsoft.Resources/deployments/operations/read | Lue toimintojen käyttöönotto |
| Microsoft.Resources/deployments/read | Lue käyttöönotoissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Storage/storageAccounts/listKeys/action | Luettelon tallennustilan tilin näppäimet |

### <a name="dns-zone-contributor"></a>DNS-vyöhyke avustaja
Voit hallita DNS-vyöhykkeet ja tietueet.

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/\*/lukeminen | Lue roolit ja roolimäärityksiä |
| Microsoft.Insights/alertRules/\* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.Network/dnsZones/\* | Luo ja hallitse DNS-vyöhykkeet ja tietueet |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue resurssit kunto |
| Microsoft.Resources/deployments/\* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/\* | Voit luoda ja hallita tukipyyntöjä |


### <a name="documentdb-account-contributor"></a>DocumentDB tilin avustaja
Voit hallita DocumentDB tilit

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.DocumentDb/databaseAccounts/* | Voit luoda ja hallita DocumentDB tilit |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="intelligent-systems-account-contributor"></a>Älykäs järjestelmien tilin avustaja
Voit hallita älykkäitä järjestelmiä-tileistä

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.IntelligentSystems/accounts/* | Voit luoda ja hallita älykkäitä järjestelmiä-tileistä |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="network-contributor"></a>Verkon avustaja
Voit hallita kaikkia verkkoresursseja

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.Network/* | Voit luoda ja hallita verkot |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="new-relic-apm-account-contributor"></a>Uusi Relic APM tilin avustaja
Voit hallita uuden Relic sovellusten suorituskykyä hallinta asiakkaat ja sovellukset

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |
| NewRelic.APM/accounts/* | Voit luoda ja hallita uuden Relic sovelluksen suorituskyvyn hallinta-tilit |

### <a name="owner"></a>Omistaja
Voit hallita kohteiden kaikki, mukaan lukien käyttö

| **Toiminnot** ||
| ------- | ------ |
| * | Voit luoda ja hallita kaikenlaisten resurssit |

### <a name="reader"></a>Lukija
Voit tarkastella kaikkea, mutta ei voi tehdä muutoksia

| **Toiminnot** ||
| ------- | ------ |
| * / lukeminen | Lue resurssien kaikenlaisten tietoja lukuun ottamatta. |

### <a name="redis-cache-contributor"></a>Redis välimuistin avustaja
Voit hallita Redis.txt tallentaa

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Cache/redis/* | Voit luoda ja hallita Redis.txt tallentaa |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="scheduler-job-collections-contributor"></a>Ajoituksen työn sivustokokoelmat avustaja
Voit hallita ajoituksen työn sivustokokoelmat

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Scheduler/jobcollections/* | Voit luoda ja hallita työn sivustokokoelmat |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä  |

### <a name="search-service-contributor"></a>Search-palvelun avustaja
Voit hallita Etsi palveluita

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Search/searchServices/* | Voit luoda ja hallita Etsi palveluita |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä  |

### <a name="security-manager"></a>Suojauksen hallinta
Voit hallita suojauksen osat, suojauskäytäntöjä ja näennäiskoneiden

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.ClassicCompute/*/read | Lue kokoonpanotietoja perinteinen Laske näennäiskoneiden |
| Microsoft.ClassicCompute/virtualMachines/*/write | Kirjoita näennäiskoneiden määritys |
| Microsoft.ClassicNetwork/*/read | Lue perinteinen verkon määritysten tiedot  |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Security/* | Voit luoda ja hallita suojausosat ja käytännöistä |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä  |

### <a name="sql-db-contributor"></a>SQL DB avustaja
Voit hallita SQL-tietokantoja, mutta ei niiden tietoturvaan liittyvät käytännöt

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue roolit ja roolin tehtävät |
| Microsoft.Insights/alertRules/* | Luoda ja hallita sääntöjä ilmoitus |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Sql/servers/databases/* | Voit luoda ja hallita SQL-tietokannat |
| Microsoft.Sql/servers/read | Lue SQL-palvelimet |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Ei voi muokata valvontakäytännöt |
| Microsoft.Sql/servers/databases/auditingSettings/* | Ei voi muokata valvonta-asetukset |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ei voi lukea valvontatiedot |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Ei voi muokata yhteyden käytännöt |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Masking käytännöt tietoja ei voi muokata |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Ilmoitusten suojauskäytäntöjä muokkaaminen ei onnistu |
| Microsoft.Sql/servers/databases/securityMetrics/* | Ei voi muokata suojauksen arvot |

### <a name="sql-security-manager"></a>SQL-suojauksen hallinta
Voit hallita tietoturvaan liittyvät käytännöt ja SQL Server-tietokannat

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue Microsoft-todennus |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Sql/servers/auditingPolicies/* | Voit luoda ja hallita SQL Serverin valvonta käytännöt |
| Microsoft.Sql/servers/auditingSettings/* | Voit luoda ja hallita SQL Serverin valvonta-asetus |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Voit luoda ja hallita SQL Serverin tietokantaan valvonta käytännöt |
| Microsoft.Sql/servers/databases/auditingSettings/* | Voit luoda ja hallita SQL Serverin tietokantaan valvonta-asetuksiin |
| Microsoft.Sql/servers/databases/auditRecords/read | Lue valvontatiedot |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Voit luoda ja hallita SQL Serverin tietokantaan yhteyden käytännöt |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Voit luoda ja hallita SQL server-tietokannan tietoihin masking käytännöt |
| Microsoft.Sql/servers/databases/read | Lue SQL-tietokannat |
| Microsoft.Sql/servers/databases/schemas/read | Luku SQL server-tietokannan rakenteita |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lue SQL Serverin tietokantaan taulukon sarakkeet |
| Microsoft.Sql/servers/databases/schemas/tables/read | Luku SQL server-tietokannan taulukoihin |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Voit luoda ja hallita SQL Serverin tietokantaan ilmoitusten suojauskäytäntöjä |
| Microsoft.Sql/servers/databases/securityMetrics/* | Voit luoda ja hallita SQL Serverin tietokantaan suojauksen arvot |
| Microsoft.Sql/servers/read | Lue SQL-palvelimet |
| Microsoft.Sql/servers/securityAlertPolicies/* | Voit luoda ja hallita SQL server ilmoitusten käytännöt |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="sql-server-contributor"></a>SQL Server avustaja
Voit hallita SQL-palvelimet ja tietokantoja, mutta ei tietoturvaan liittyvät käytännöt

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus|
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Sql/servers/* | Voit luoda ja hallita SQL-palvelimet |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server valvonta käytännöt muokkaaminen ei onnistu |
| Microsoft.Sql/servers/auditingSettings/* | SQL server valvonta-asetuksia ei voi muokata. |
| Microsoft.Sql/servers/databases/auditingPolicies/* | SQL Serverin tietokantaan valvonta käytännöt muokkaaminen ei onnistu |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL Serverin tietokantaan valvonta-asetuksia ei voi muokata. |
| Microsoft.Sql/servers/databases/auditRecords/read  | Ei voi lukea valvontatiedot |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL Serverin tietokantaan yhteyden käytännöt muokkaaminen ei onnistu |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-tietokannan tietoihin masking käytännöt muokkaaminen ei onnistu |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | SQL Serverin tietokantaan ilmoitusten suojauskäytäntöjä muokkaaminen ei onnistu |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL Serverin tietokantaan suojauksen arvot muokkaaminen ei onnistu |
| Microsoft.Sql/servers/securityAlertPolicies/* | SQL server ilmoituksen suojauskäytäntöjä muokkaaminen ei onnistu |

### <a name="classic-storage-account-contributor"></a>Perinteinen tallennustilan tilin avustaja
Voit hallita perinteinen tallennustilan tilit

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.ClassicStorage/storageAccounts/* | Luo ja tallennustilaa tilien hallinta |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="storage-account-contributor"></a>Tallennustilan tilin avustaja
Voit tallennustilan tilien hallinta, mutta niitä ei käsitellä.

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lue kaikki todennus |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.Insights/diagnosticSettings/* | Diagnostiikan asetusten hallinta |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Storage/storageAccounts/* | Luo ja tallennustilaa tilien hallinta |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="user-access-administrator"></a>Käyttäjän Access-järjestelmänvalvoja
Voit hallita Azure resurssien käytön

| **Toiminnot** ||
| ------- | ------ |
| * / lukeminen | Lue resurssien kaikenlaisten tietoja lukuun ottamatta. |
| Microsoft.Authorization/* | Käyttöoikeuksien hallinta |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="classic-virtual-machine-contributor"></a>Perinteinen virtuaalikoneen avustaja
Voit hallita perinteinen näennäiskoneiden, mutta ei virtual verkko- tai tili, johon ne on kytketty

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.ClassicCompute/domainNames/* | Voit luoda ja hallita perinteinen Laske toimialuenimet |
| Microsoft.ClassicCompute/virtualMachines/* | Voit luoda ja hallita näennäiskoneiden |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Liittyminen verkon käyttöoikeusryhmät |
| Microsoft.ClassicNetwork/reservedIps/link/action | Linkki varattu IP-osoitteet |
| Microsoft.ClassicNetwork/reservedIps/read | Lue varattu IP-osoitteet |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Liittyminen virtual verkot |
| Microsoft.ClassicNetwork/virtualNetworks/read | Lue virtual verkot |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Lue tallennustilan tilin levyjen |
| Microsoft.ClassicStorage/storageAccounts/images/read | Lue tallennustilan tilin kuvat |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Luettelon tallennustilan tilin näppäimet |
| Microsoft.ClassicStorage/storageAccounts/read | Lue perinteinen tallennustilan tilit |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="virtual-machine-contributor"></a>Virtuaalikoneen avustaja
Voit hallita näennäiskoneiden, mutta ei virtual verkko- tai tili, johon ne on kytketty

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.Compute/availabilitySets/* | Luominen ja suorittaminen käytettävyys joukkojen hallinta |
| Microsoft.Compute/locations/* | Luominen ja suorittaminen sijaintien hallinta |
| Microsoft.Compute/virtualMachines/* | Voit luoda ja hallita näennäiskoneiden |
| Microsoft.Compute/virtualMachineScaleSets/* | Voit luoda ja hallita virtuaalikoneen asteikko joukot |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verkon yhdyskäytävän Taustajärjestelmä osoite sovellussarjojen liittyminen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Kuormituksen tasauspalvelun Taustajärjestelmä osoite jakavat liittyminen |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Liittyminen kuormituksen saapuva NAT jakavat |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Liittyminen kuormituksen saapuvan liikenteen säännöt NAT |
| Microsoft.Network/loadBalancers/read | Lue kuormituksen tasoitusmääritykset |
| Microsoft.Network/locations/* | Voit luoda ja hallita verkkosijainneissa |
| Microsoft.Network/networkInterfaces/* | Voit luoda ja hallita verkon liityntäkohdat |
| Microsoft.Network/networkSecurityGroups/join/action | Liittyminen verkon käyttöoikeusryhmät |
| Microsoft.Network/networkSecurityGroups/read | Lue verkon käyttöoikeusryhmät |
| Microsoft.Network/publicIPAddresses/join/action | Liittyminen verkon julkiseen IP-osoitteet |
| Microsoft.Network/publicIPAddresses/read | Lue verkon julkiseen IP-osoitteet |
| Microsoft.Network/virtualNetworks/read | Lue virtual verkot |
| Microsoft.Network/virtualNetworks/subnets/join/action | Liittyminen VPN-aliverkosta |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Storage/storageAccounts/listKeys/action | Luettelon tallennustilan tilin näppäimet |
| Microsoft.Storage/storageAccounts/read | Lue tallennustilan tilit |
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="classic-network-contributor"></a>Perinteinen verkon avustaja
Voit hallita perinteinen virtual verkkojen ja varattu IP-osoitteet

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.ClassicNetwork/* | Voit luoda ja hallita perinteinen verkot |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |

### <a name="web-plan-contributor"></a>Web-suunnitelma avustaja
Voit hallita web-Palvelupaketit

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |
| Microsoft.Web/serverFarms/* | Voit luoda ja hallita palvelimen klustereihin |

### <a name="website-contributor"></a>Sivuston avustaja
Voit hallita sivustot, mutta ei web-palvelupakettia, johon ne on kytketty

| **Toiminnot** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Luku-todennus |
| Microsoft.Insights/alertRules/* | Voit luoda ja hallita havainnollistamisen ilmoitusten säännöt |
| Microsoft.Insights/components/* | Voit luoda ja hallita havainnollistamisen osat |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lue kunto resurssit |
| Microsoft.Resources/deployments/* | Voit luoda ja hallita resurssin ryhmän ominaisuuksissa |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lue resurssiryhmät |  
| Microsoft.Support/* | Voit luoda ja hallita tukipyyntöjä |
| Microsoft.Web/certificates/* | Voit luoda ja hallita sivuston varmenteet |
| Microsoft.Web/listSitesAssignedToHostName/read | Isäntänimi määritetty luku-sivustot |
| Microsoft.Web/serverFarms/join/action | Liittyminen palvelimen klustereihin |
| Microsoft.Web/serverFarms/read | Lue palvelimen klustereihin |
| Microsoft.Web/sites/* | Voit luoda ja hallita sivustot |

## <a name="see-also"></a>Katso myös
- [Roolipohjainen käyttöoikeuksien valvonta](role-based-access-control-configure.md): Aloita RBAC Azure-portaalissa.
- [Mukautettu roolien Azure RBAC](role-based-access-control-custom-roles.md): Lue, miten voit luoda mukautettuja rooleja access tarpeitasi.
- [Luo access muuttaa historia-kaavioraportin](role-based-access-control-access-change-history-report.md): seurantaan RBAC muuttaminen roolimäärityksiä.
- [Roolipohjainen käyttöoikeuksien valvonta vianmääritys](role-based-access-control-troubleshooting.md): Hae ehdotuksia yleisten ongelmien korjaaminen.
