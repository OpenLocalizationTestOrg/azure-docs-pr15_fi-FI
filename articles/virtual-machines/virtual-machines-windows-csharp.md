<properties
    pageTitle="Ottaa käyttöön Azure resurssien C# | Microsoft Azure"
    description="Lue luominen Microsoft Azure resurssien C#- ja Azure resurssien hallinnan avulla."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Ottaa käyttöön käyttämällä C Azure-resurssit# 

Tässä artikkelissa näytetään, miten voit luoda Azure resurssien C#.

Sinun on ensin varmistaaksesi, että olet lisännyt tehtäviä:

- Asenna [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) - tai [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) asennuksen tarkistaminen
- Hae [todennus tunnus](../resource-group-authenticate-service-principal.md)

Kestää noin 30 minuuttia, toimi seuraavien ohjeiden mukaisesti.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Vaihe 1: Projektin luominen Visual Studio ja asenna kirjastot

NuGet paketit ovat helpoin tapa asentaa kirjastot, jotka haluat Tässä opetusohjelmassa loppuun. Saat kirjastot, jotka sinulla on oltava Visual Studiossa, toimi seuraavasti:

1. Valitse **Tiedosto** > **uuden** > **projektin**.

2. **Mallien** > **Visual C#**- **Console**-sovellus, nimen ja sijainnin projektin ja valitse sitten **OK**.

3. Projektin nimeä Solution Explorerissa hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

4. *Active Directory* -hakuruutuun valitsemalla **Asenna** Active Directory käyttöoikeuksien kirjaston paketin ja noudata asennusohjeita paketin.

5. Valitse **Sisällytä Prerelease**sivun yläreunassa. Haku-ruutuun tyyppi *Microsoft.Azure.Management.Compute* Valitse Laske .NET-kirjastojen **Asenna** ja noudata asennusohjeita paketin.

6. Haku-ruutuun tyyppi *Microsoft.Azure.Management.Network* valitsemalla **Asenna** verkon .NET-kirjastojen ja noudata asennusohjeita paketin.

7. Haku-ruutuun tyyppi *Microsoft.Azure.Management.Storage* tallennustilan .NET-kirjastojen valitsemalla **Asenna** ja noudata asennusohjeita paketin.

8. Kirjoita *Microsoft.Azure.Management.ResourceManager* hakuruutuun, valitse resurssi hallinta kirjastojen **asentaminen** .

Nyt voit ryhtyä toimeen kirjastojen avulla voit luoda sovelluksen.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Vaihe 2: Luo tarkistamiseen pyynnöt käytettävät tunnistetiedot

Nyt voit muotoilla, jonka loit aiemmin tunnistetiedot, joita käytetään tarkistamiseen pyynnöt Azure resurssien hallinnan siirtäminen hakemuksen tiedot.

1. Avaa projekti, jonka loit Program.cs-tiedosto ja lisää ne lauseet tiedoston ylhäältä:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. Luo tunnuksen, jota tarvitaan lisäämällä tätä menetelmää ohjelma luokkaan:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Asiakastunnus} korvaaminen Azure Active Directory tunnus sovelluksen, {asiakas-salaisuus} AD-sovelluksen ja {vuokraajan id} access-näppäimen kanssa tilauksen vuokraajan tunnuksessa on virhe. Löydät vuokraajan tunnus suorittamalla Get-AzureRmSubscription. Pikanäppäin voit etsiä Azure-portaalissa.

3. Soita menetelmä, jota olet aiemmin lisännyt, lisää koodi Program.cs tiedoston Main-menetelmää:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Tallenna tiedosto Program.cs.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>Vaihe 3: Rekisteröi resurssi-palveluntarjoajat ja luo resursseja

### <a name="register-the-providers-and-create-a-resource-group"></a>Rekisteröi palvelut ja Resurssiryhmän luominen

Kaikki resurssit täytyy sisältyä resurssiryhmä. Ennen kuin voit lisätä resurssien ryhmä-tilauksesi on rekisteröitävä resurssin tarjoajien palvelua.

1. Lisää muuttujat ohjelma luokan, jota haluat käyttää resurssien nimien määrittäminen Main-menetelmää:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    Korvaa kaikki muuttujien arvoihin nimet ja tunniste, jota haluat käyttää. Löydät tilauksen tunnus suorittamalla Get-AzureRmSubscription.

2. Resurssiryhmän luominen ja rekisteröi palvelut-ohjelma-luokan lisääminen tätä menetelmää:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Soita menetelmä, jota olet aiemmin lisännyt, lisää koodi Main-menetelmää:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

[Tallennustilan tilin](../storage/storage-create-storage-account.md) tarvitaan virtual kiintolevyn tiedostolle, joka on luotu virtuaalikoneen tallentamiseen.

1. Luo tallennustilan tilin lisääminen ohjelman luokan tätä menetelmää:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Soita menetelmä, jota olet aiemmin lisännyt lisää koodi-ohjelma-luokan Main-menetelmää:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>Luo julkinen IP-osoite

Yhteydenpito virtuaalikoneen tarvitaan julkiseen IP-osoite.

1. Luo virtuaalikoneen julkiseen IP-osoite, Lisää tämä menetelmä ohjelma-luokan:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Soita menetelmä, jota olet aiemmin lisännyt lisää koodi-ohjelma-luokan Main-menetelmää:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>Luo virtuaalisia verkko

Virtual machine, jotka on luotu resurssien hallinnan käyttöönottomalli on oltava virtual verkkoon.

1. Luo aliverkon ja virtual verkon lisäämällä tätä menetelmää ohjelma luokkaan:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Soita menetelmä, jota olet aiemmin lisännyt lisää koodi-ohjelma-luokan Main-menetelmää:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>Luo verkkokäyttöliittymä

Virtual machine on verkkoliittymän vaihtamaan virtual verkossa.

1. Liittymää verkko-ohjelma-luokan lisääminen tätä menetelmää:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Soita menetelmä, jota olet aiemmin lisännyt lisää koodi-ohjelma-luokan Main-menetelmää:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Käytettävyys-joukon luominen

Käytettävyys joukot helpottavat sovelluksen käyttämä näennäiskoneiden ylläpito hallinta.

1. Luo joukko käytettävyys-ohjelma-luokan lisääminen tätä menetelmää:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Soita menetelmä, jota olet aiemmin lisännyt lisää koodi-ohjelma-luokan Main-menetelmää:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Luo virtual machine

Tukitiedostojen resursseja on luotu, voit luoda virtual machine.

1. Luo virtuaalikoneen lisäämällä tätä menetelmää ohjelma luokkaan:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Tässä opetusohjelmassa Luo virtual kone Windows Server-käyttöjärjestelmän versio. Lisätietoja valitsemalla muut kuvat-kohdassa [Siirry ja valitse Azure virtuaalikoneen kuvat ja Windows PowerShellin Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).

2. Soita menetelmä, jota olet aiemmin lisännyt, lisää koodi Main-menetelmää:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Vaihe 4: Poista resurssit

Perittävän Azure käytettävien resurssien, koska se on aina hyvä Poista resursseja, joita ei enää tarvita. Jos haluat poistaa näennäiskoneiden ja hyödyt resurssit, sinun tarvitsee on resurssiryhmän poistaminen.

1.  Poista resurssiryhmän lisääminen ohjelman luokan tätä menetelmää:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Soita menetelmä, jota olet aiemmin lisännyt, lisää koodi Main-menetelmää:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>Vaihe 5: Console-sovelluksen käyttämiseen

1. Console-sovelluksen käyttämiseen valitsemalla **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

2. Kun kunkin Tilakoodi palautetaan Luo kullekin resurssille, paina **Enter** -näppäintä. Kun virtuaalikoneen on luotu, muuta seuraavaan vaiheeseen ennen painamalla Enter, jos haluat poistaa kaikki resurssit.

    Olisi kestää noin viisi minuuttia konsolin sovelluksen suorittamiseen täysin alusta loppuun. Ennen kuin painat Enter aloittaa poistaminen resursseja, voi kestää muutaman minuutin kuluttua vahvistamiseksi luomisen resursseista Azure-portaalissa, ennen kuin poistat ne.

3. Resurssien tilan tarkasteleminen Siirry Azure-portaalissa valvontalokien:

    ![Selaa valvontalokien Azure-portaalissa](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Seuraavat vaiheet

- Hyödynnä mallin avulla voit luoda virtual machine edellä mainittujen tietojen avulla [käyttöönotto Azure-virtuaalikoneen](virtual-machines-windows-csharp-template.md)käyttämällä C#- ja Resurssienhallinta-malli.
- Opi hallitsemaan virtuaalikoneen, jonka loit tarkastelemalla [hallinta näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-csharp-manage.md).
