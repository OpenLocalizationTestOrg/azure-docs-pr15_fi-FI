<properties
    pageTitle="Hallitse VMs Azure Resurssienhallinta ja C# | Microsoft Azure"
    description="Voit hallita näennäiskoneiden Azure Resurssienhallinta ja C#."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Hallitse Azuren näennäiskoneiden Azure Resurssienhallinta ja C#  

Tämän artikkelin tehtävät näyttää, miten voit hallita näennäiskoneiden, kuten käynnistäminen, lopettaminen ja päivitetään. Virtual tietokoneen on oltava resurssiryhmä tämän artikkelin suorittaminen.

Tämän artikkelin tehtävien suorittamiseen tarvitset:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Todennus-tunnus](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Projektin luominen Visual Studio ja asenna paketit

NuGet paketit ovat helpoin tapa asentaa kirjastot, jotka sinun on tämän artikkelin tehtävät valmistuvat. Kirjastot, jotka olet asentanut tämän artikkelin ovat Azure Active Directory käyttöoikeuksien kirjasto ja Laske resurssikirjasto toimittaja. Viimeistele Visual Studiossa kirjastojen seuraavasti:

1. Valitse **Tiedosto** > **uuden** > **projektin**.

2. **Mallien** > **Visual C#**- **Console**-sovellus, nimen ja sijainnin projektin ja valitse sitten **OK**.

3. Projektin nimeä Solution Explorerissa hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

4. *Active Directory* -hakuruutuun valitsemalla **Asenna** Active Directory käyttöoikeuksien kirjaston paketin ja noudata asennusohjeita paketin.

5. Valitse **Sisällytä Prerelease**sivun yläreunassa. Haku-ruutuun tyyppi *Microsoft.Azure.Management.Compute* Valitse Laske .NET-kirjastojen **Asenna** ja noudata asennusohjeita paketin.

Nyt voit ryhtyä toimeen virtuaalisten laitteiden hallinta kirjastojen avulla.

## <a name="set-up-the-project"></a>Projektin määrittäminen

Nyt kun sovellus on luotu ja kirjastot on asennettu, voit luoda tunnuksen sovelluksen tietojen avulla. Tunnuksessa käytetään todentamiseen pyynnöt Azure resurssien hallinta.

1. Avaa projekti, jonka loit Program.cs-tiedosto ja lisää ne lauseet tiedoston ylhäältä:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Lisää muuttujat Määritä resurssiryhmän nimi sekä nimi virtuaalikoneen ja tilauksen tunnus-ohjelma-luokan Main-menetelmää:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Löydät tilauksen tunnus suorittamalla Get-AzureRmSubscription.
    
3. Saat tunnuksen, jota tarvitaan luominen tunnistetiedot-ohjelma-luokan lisääminen tätä menetelmää:

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
    
4. Luo tunnistetiedot, lisää koodi Program.cs Main-menetelmää:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Tallenna tiedosto Program.cs.

## <a name="display-information-about-a-virtual-machine"></a>Näyttää tietoja virtual machine

1. Lisää aiemmin luotu projekti-ohjelma-luokan tätä menetelmää:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Tallenna tiedosto Program.cs.

4. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

    Kun suoritat tämän menetelmän, katso suunnilleen tässä esimerkissä:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Lopeta virtual machine

Voit lopettaa virtual kone kahdella tavalla. Voit estää virtual machine ja pitää sen asetuksia, mutta Jatka voidaan veloittaa tai voit lopettaa virtual machine ja poista se varaus. Kun varaus poistettu virtual machine, kaikki siihen liittyvät resurssit ovat myös Varaus poistettu ja laskutuksen se päättyy.

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Jos haluat poistaa varauksen virtuaalikoneen, muuta Katkaise virta puhelu koodi:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

    Raportissa pitäisi näkyä pysäytetty virtuaalikoneen muutoksen tila. Jos suoritit menetelmä puheluja Deallocate tila on pysäytetty (Varaus poistettu).

## <a name="start-a-virtual-machine"></a>Käynnistettäessä

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

    Raportissa pitäisi näkyä virtuaalikoneen tilan muuttaminen suorittamisen.

## <a name="restart-a-running-virtual-machine"></a>Käynnissä virtual tietokoneen käynnistäminen uudelleen

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

## <a name="resize-a-virtual-machine"></a>Virtual machine koon muuttaminen

Tässä esimerkissä näytetään käytössä virtual machine koon muuttamisesta.

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

    Raportissa pitäisi näkyä Standard_A1 virtuaalikoneen muutos kokoa.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Lisää tietoja DVD-levyllä virtual machine

Tässä esimerkissä näytetään, miten voit lisätä tietoja DVD-levyllä käynnissä virtual machine.

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

## <a name="delete-a-virtual-machine"></a>Poista virtual machine

1. Kommentti koodia, että olet aiemmin lisännyt Main-menetelmä koodin gatewaylta lukuun ottamatta.

2. Lisää tämä menetelmä ohjelma luokkaan:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Tallenna tiedosto Program.cs.

5. **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD käyttäjänimellä ja-salasanaa, jolla käytät tilaus.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos käyttöönoton ongelmat, voi tarkastella [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](../resource-manager-troubleshoot-deployments-portal.md)
