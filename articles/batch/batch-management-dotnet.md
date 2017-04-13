<properties
    pageTitle="Tilin Resurssienhallinta ja erä hallinta .NET | Microsoft Azure"
    description="Luominen, poistaminen ja muokkaaminen Azure erä tilin resurssien erä hallinta .NET-kirjaston kanssa."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Azure erä tilit ja kiintiön kanssa erä hallinta .NET hallinta

> [AZURE.SELECTOR]
- [Azure portal](batch-account-create-portal.md)
- [Erän hallinta .NET](batch-management-dotnet.md)

Voit pienentää ylläpito yleisrasite Azure erä-sovellusten avulla [Erä hallinta .NET] [ api_mgmt_net] kirjastossa voit automatisoida erä tilin luominen, poistaminen tai hallintaa kiintiön etsiminen.

- **Luo ja poista erä tilit** minkä tahansa alueella. Jos riippumattomat toimittajaksi (ISV) esimerkiksi tarjoaa palvelua, johon kunkin on määritetty eri erä asiakkaan laskutuksen tarkoituksiin asiakkaiden, voit lisätä tilin luominen ja poistaminen ominaisuuksia asiakas-portaaliin.
- **Nouda ja muodosta uudelleen sarjanumerot tilin avaimet** ohjelmallisesti jollekin erän tilit. Tämä auttaa noudattamaan suojauskäytäntöjä, jotka edellyttävät säännöllisiä palauttaminen tai sen tilin näppäimet voimassaolon. Kun käytössäsi on useita erä tilejä Azure eri alueilla, automaatio palauttaminen toimintaohjeet kasvaa-ratkaisun tehokkuutta.
- **Tarkista tilin kiintiön** ja ota-kokeiluversio ja virheen vähentää arvailua ulos määritettäessä erä tilit on rajat. Valitsemalla tilin kiintiön ennen kuin aloitat työt jakavat luominen tai lisääminen Laske solmujen, missä voit säätää itse tai kun nämä Laske resurssien luodaan. Voit määrittää tilit edellyttävät kiintiön kasvaa ennen varaa lisäresursseja-tilit.
- Täydet toiminnot sisältävien ohjelmien hallinnan takaamiseksi--käyttämällä erä hallinta .NET [Azure Active Directory] **Ominaisuudet Azure muiden palvelujen yhdistää** [aad_about], ja [Azure Resurssienhallinta] [ resman_overview] yhdessä samassa sovelluksessa. Nämä ominaisuudet ja niiden ohjelmointirajapinnan avulla voit antaa frictionless todennus-versio, voi luoda ja poistaa resurssiryhmät ja ominaisuuksista, joita ovat lopusta loppuun-management-ratkaisun yllä.

> [AZURE.NOTE] Kun tässä artikkelissa keskitytään erä-tilejä, avaimet ja kiintiön ohjelmallisesti hallinnointi, voit suorittaa Monet näistä toiminnoista [Azure portal][azure_portal]. Lisätietoja on artikkelissa [Azure erä-tilin luominen käyttämällä Azure portaalin](batch-account-create-portal.md) ja [kiintiöiden ja Azure erä palvelun rajoitukset](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Luoda ja poistaa Siirtoerän tilit

Kuten edellä, yksi erä hallinta Ohjelmointirajapinnan ensisijaisista ominaisuuksista on luominen ja poistaminen erä tilit Azure alueella. Voit tehdä käyttää [BatchManagementClient.Account.CreateAsync] [ net_create] ja [DeleteAsync][net_delete], tai niiden synkronisen vastineiden.

Seuraavat koodikatkelman Luo tili, hakee juuri luomasi tilin erä-palvelusta ja poistaa sen. Tämä koodikatkelman ja muiden tämän artikkelin `batchManagementClient` on täysin valmisteltu esiintymä [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Erän hallinta .NET-kirjastoon ja sen BatchManagementClient luokan käyttävät sovellukset tarvitsevat **palvelun järjestelmänvalvoja** tai **coadministrator** käyttöoikeudet tilaukseen, joka omistaa tuleeko erä-tili. Lisätietoja on artikkelissa [Azure Active Directory](#azure-active-directory) -kohta ja [AccountManagement] [ acct_mgmt_sample] koodin malli.

## <a name="retrieve-and-regenerate-account-keys"></a>Hae ja luo tili näppäimet

Hanki ensisijainen ja toissijainen tilin näppäimet erä tilien kuluessa tilauksen käyttämällä [ListKeysAsync][net_list_keys]. Voit luoda uudelleen näppäimiä käyttämällä [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Voit luoda virtaviivaistettu yhteyden työnkulun sovellusten hallinta. Hankkimista tiliavain haluat hallita kanssa [ListKeysAsync]erä tilin[net_list_keys]. Käytä tätä näppäintä sitten, kun valmistellaan erä .NET-kirjaston [BatchSharedKeyCredentials] [ net_sharedkeycred] luokka, jota käytetään, kun valmistellaan [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Tarkista Azure tilaus ja erä tilin kiintiön

Azure tilaukset ja yksittäisiä Azure palveluja, kuten kaikki erän on oletusarvoinen kiintiön, joka rajoittaa tiettyjä kohteita ne. Katso Azure tilaukset oletusarvo-kiintiön [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](./../azure-subscription-service-limits.md). Katso erä palvelun oletusarvo-kiintiön [kiintiöiden ja Azure erä palvelun rajoitukset](batch-quota-limit.md). Erän hallinta .NET-kirjastossa käyttämällä voit tarkistaa näiden kiintiöiden sovellukset. Näin voit kohdistus päätösten ennen Lisää tilejä tai laskea esimerkiksi jakavat ja Laske solmujen.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Tarkista Azure tilauksen erä tilin kiintiön

Ennen erän tilin luominen alueen, voit tarkistaa tilauksen Azure nähdäksesi, oletko voi lisätä tiliä alueen.

Valitse alla koodikatkelman Käytämme ensin [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] saat kaikki erän-tilit, jotka ovat tilauksen kokoelma. Kun olemme olet saanut tätä sivustokokoelman, määrittäminen kuinka moneen tiliin on kohde-alueella. Valitse Käytämme [BatchManagementClient.Subscriptions] [ net_mgmt_subscriptions] erä tilin kiintiön hankkiminen ja määrittää, kuinka moneen tiliin (jos saatavilla) voidaan luoda alueen.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

Valitse yllä koodikatkelman `creds` on esiintymä [TokenCloudCredentials][azure_tokencreds]. Esimerkki objektin luomisesta on ohjeartikkelissa [AccountManagement] [ acct_mgmt_sample] GitHub-koodi-malli.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Tarkista Laske palvelinresurssien kiintiön erä-tili

Ennen lisääntyvien Laske resurssien erä-ratkaisussa, voit tarkistaa, jotta haluat varata resursseja ei ylittää asiakkaan kiintiön. Alla olevassa koodikatkelman olemme tulostaa erän tili nimeltä kiintiötiedot `mybatchaccount`. Oman sovelluksessa voi määrittää, voiko tilin voit käsitellä luoda lisäresursseja näiden tietojen avulla.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Internetissä on myös oletus kiintiön Azure tilaukset ja palveluja, monia nämä raja-arvot voidaan nostaa tekemällä [Azure portal]-pyynnön[azure_portal]. Esimerkiksi näkyviin [kiintiöiden ja Azure erä palvelun rajoitukset](batch-quota-limit.md) lisääntyvien erä tili-kiintiön ohjeita.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Erän hallinta .NET-Azure AD- ja resurssien hallinta

Käsiteltäessä erä hallinta .NET-kirjasto tavanomaiseen avulla myös [Azure Active Directory] [ aad_about] (Azure AD) ja [Azure Resurssienhallinta][resman_overview]. Esimerkki projektin käsitellyt alla käyttää Azure Active Directory- ja Resurssienhallinta samalla, kun se osoittaa erän hallinta .NET-Ohjelmointirajapinnan.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure itse käyttää Azure AD sen asiakkaiden, palvelun Järjestelmänvalvojat ja organisaation käyttäjien todentamiseen. Erän hallinta .NET kontekstissa Käytä Azure AD tarkistamiseen vain tilaus tai muiden järjestelmänvalvojat. Näin hallinta kirjaston kyselyn erä-palvelun ja suorittaa tässä artikkelissa kuvatut toimet.

Esimerkki projektin käsitellyt alla Azure [Active Directory käyttöoikeuksien kirjaston] [ aad_adal] (ADAL) käytetään Microsoft käyttöoikeutensa käyttäjältä. Palvelun järjestelmänvalvoja tai coadministrator tunnistetiedot on annettu, kun sovellus voi kyselyn Azure tilaukset – luettelo ja voit luoda ja poistaa resurssiryhmiä ja erä tilit.

### <a name="resource-manager"></a>Resurssien hallinta

Kun luot erän tilit erä hallinta .NET-kirjaston kanssa, voit tavallisesti luovatko ne [resurssiryhmä][resman_overview]. Voit luoda resurssiryhmän ohjelmallisesti käyttämällä [ResourceManagementClient] [ resman_client] [Resurssienhallinta .NET] luokan[ resman_api] kirjastoon. Tai voit lisätä tili, jonka loit aiemmin käyttämällä [Azure portal]resurssiryhmä[azure_portal].

## <a name="sample-project-on-github"></a>Esimerkki projektin GitHub

Nähdäksesi erä hallinta .NET-toiminto kuittaa ulos [AccountManagment] [ acct_mgmt_sample] otoksen projektin GitHub. Tämä Konsolisovellus näkyy luomista ja [BatchManagementClient] käyttö[ net_mgmt_client] ja [ResourceManagementClient][resman_client]. Esimerkissä Azure [Active Directory käyttöoikeuksien kirjaston] käyttö myös[ aad_adal] (ADAL), joita tarvitaan sekä asiakkaat.

Malli-sovelluksen käyttämiseen onnistuneesti, sinun on ensin rekisteröitävä se Azure AD mukaan Azure-portaalissa. Ohjeiden [lisääminen sovelluksen](../active-directory/active-directory-integrating-applications.md#adding-an-application) [integrointi sovellusten Azure Active Directory] -osassa[ aad_integrate] rekisteröidä sovelluksen oman tilin malli on oletusarvoinen kansio. Valitse sovelluksen tyypin **Alkuperäisen asiakassovellus** , ja voit määrittää, mikä tahansa kelvollinen URI (kuten `http://myaccountmanagementsample`) **Uudelleenohjata URI**--sen ei tarvitse olla todellisia päätepiste.

Kun olet lisännyt sovelluksen, sovelluksen asetukset-portaalissa *Windows Azure Service Management API* -sovelluksen **Käytön Azure hallinnan kuin organisaation** oikeutta delegoida:

![Palvelusovelluksen käyttöoikeuksien Azure-portaalissa][2]

> [AZURE.TIP] Jos **Windows Azure Service Management API** ei näy *muiden sovellusten*käyttöoikeudet, valitse **Lisää sovellus**, valitse **Windows Azure Service Management API**ja valitse sitten valintaruutupainiketta. Valitse edustajan oikeudet edellä määritetyllä tavalla.

Kun olet lisännyt sovelluksen yllä olevien ohjeiden mukaisesti, Päivitä `Program.cs` - [AccountManagment] [ acct_mgmt_sample] otoksen projektin sovelluksen uudelleenohjata URI ja asiakkaan ID-tunnuksellasi. Löydät nämä arvot-sovelluksen **määrittäminen** -välilehdessä:

![Sovelluksen määritys Azure-portaalissa][3]

[AccountManagment] [ acct_mgmt_sample] sovelluksen malli esittelee seuraavat toimenpiteet:

1. Suojaustunnus Azure AD-hankkia käyttämällä [ADAL][aad_adal]. Jos käyttäjä on ole jo kirjautunut sisään, heitä pyydetään Azure tunnistetietoja.
2. Azure AD saatu suojaustunnus käyttämällä luoda [SubscriptionClient] [ resman_subclient] kyselyyn Azure tiliin liittyvät tilaukset luettelo. Näin valinta, jos useita tilauksen löytyy.
3. Luo valitun-tilaukseen liittyvää tunnistetiedot objekti.
4. Luo [ResourceManagementClient] [ resman_client] käyttöoikeuksilla.
5. Käytä [ResourceManagementClient] [ resman_client] resurssiryhmä luomiseen.
6. Käytä [BatchManagementClient] [ net_mgmt_client] suorittamaan useita erä tilin toimintoja:
  - Luoda erää tilin uusi resurssiryhmä.
  - Uuden tilin hankkiminen erä-palvelusta.
  - Tulosta tilin näppäimet uutta tiliä varten.
  - Luo uusi perusavain tilin.
  - Tulosta kiintiötiedot tilin.
  - Tulosta kiintiötiedot tilaukseen.
  - Tulosta kaikki tilit tilauksen piiriin kuuluvien.
  - Voit poistaa juuri luomasi tilin.
7. Poista resurssiryhmän.

Ennen kuin poistat juuri luomasi erä tilin ja resurssien ryhmä, Tarkasta molemmat [Azure portal][azure_portal]:

![Azure portal resurssiryhmä ja erä tilin näyttäminen][1]
<br />
*Azure portal uusi resurssiryhmä ja erä tilin näyttäminen*

[aad_about]: ../active-directory/active-directory-whatis.md "Azure Active Directory-ominaisuudet"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD todennus tilanteita, joissa"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Azure Active Directory-integraation sovellukset"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
