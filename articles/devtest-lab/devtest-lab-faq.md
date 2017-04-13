<properties
    pageTitle="Azure DevTest harjoituksia usein kysytyt kysymykset | Microsoft Azure"
    description="Vastauksia kysymyksiin, Azure DevTest harjoituksia"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest harjoituksia usein kysytyt kysymykset

Tässä artikkelissa vastataan joihinkin Azure DevTest harjoituksia yleisimmät kysymyksiin.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Yleiset
- [Entä jos kysymykseeni ei ole vastattu tähän?](#what-if-my-question-isnt-answered-here)
- [Miksi Azure DevTest harjoituksia kannattaa käyttää?](#why-should-i-use-azure-devtest-labs) 
- [Mitä "luotettavaa vapaa, Omatoiminen" tarkoittaa?](#what-does-quotworry-free-self-servicequot-mean)
- [Miten voin käyttää Azure DevTest harjoituksia?](#how-can-i-use-azure-devtest-labs) 
- [Miten Azure DevTest harjoituksia 'M veloittaa?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Tietoturva 
- [Mitkä ovat eri suojauksen tasojen Azure DevTest harjoituksia?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Miten voin luoda rooli, jotta käyttäjät voivat suorittaa tiettyyn tehtävään?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD-integroinnin & automaatio 
 
- [Azure DevTest harjoituksia integroida CI/CD-toolchain?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Näennäiskoneiden 
 
- [Miksi en näe Azuren näennäiskoneiden-sivu, joka näy sisällä Azure DevTest harjoituksia tiettyjä VMs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Mikä on mukautettu kuvia ja kaavojen välinen ero?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Miten voin luoda useita VMs samalla mallilla samalla kertaa?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Miten oma aiemmin Azure VMs siirtäminen Azure DevTest harjoituksia-kurssin?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Useita levyjä Liitä oma VMs avulla](#can-i-attach-multiple-disks-to-my-vms) 
- [Miten prosessi, jossa näennäiskiintolevytiedostoja, voit luoda mukautetun kuvat ladataan automatisoida?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Miten poistoprosessi kaikki omat testiympäristössä VMs voit automatisoida?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Palvelutiedot 
 
- [Mitä tietoja?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Kurssin määritys 
 
- [Miten voin luoda kurssin Azure Resurssienhallinta mallista?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Miksi oma VMs luodaan toinen resurssi-ryhmissä, joissa on haluamaansa nimet? Voit nimetä uudelleen tai muokata resurssin-ryhmiin?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Kuinka monta harjoituksia kohdassa saman tilauksen voi luoda?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Kuinka monta VMs voit luoda kurssin kohden?](#how-many-vms-can-i-create-per-lab)
- [Miten voin jakaa suoran linkin Omat kurssin?](#how-do-i-share-a-direct-link-to-my-lab)
- [Mikä on Microsoft-tili?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Vianmääritys 
 
- [Oma Palvelutietojen epäonnistui AM luonnin aikana. Miten se voi vianmääritys?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Miksi ei ole oma nykyinen virtual verkkosi tallentaminen oikein?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Entä jos kysymykseeni ei ole vastattu tähän?
Jos kysymys ei ole mainittu tässä, kerro meille, mistä ja Microsoft auttaa sinua löydä vastausta.

- Lähetä kysymys [Disqus viestiketjun](#comments) nämä usein kysytyt kysymykset lopussa ja osallistuminen Azure välimuisti-ryhmän ja muut yhteisön jäsenet tietoja tässä artikkelissa.
- Saavuttamiseksi laajan yleisön Lähetä kysymys- [Azure DevTest harjoituksia MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)ja osallistuminen Azure DevTest harjoituksia-ryhmän ja muut yhteisön jäsenten kanssa.
- Voit ottaa käyttöön, pyytää, Lähetä pyyntöjä ja [Azure DevTest harjoituksia käyttäjän Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs)ideoita.

### <a name="why-should-i-use-azure-devtest-labs"></a>Miksi Azure DevTest harjoituksia kannattaa käyttää? 
Azure DevTest harjoituksia tallentaa ryhmän aikaa ja rahaa. Kehittäjät voit luoda oman ympäristöissä käyttämällä useita eri kantalukujen ja palvelutiedot avulla voit nopeasti käyttöön ja määrittää sovelluksia. Käytä mukautettuja kuvat ja kaavat, näennäiskoneiden malleiksi tallentaminen ja helposti uudestaan. Lisäksi harjoituksia tarjoavat määritettäviä käytäntöjä, joiden avulla kurssin järjestelmänvalvojat voivat vähentää jätteiden ja hallita ryhmän ympäristössä. Käytännöt ovat automaattinen-Sammuta, kustannukset raja-arvo, suurin VMs käyttäjän ja AM enimmäiskoot kohden. Azure DevTest harjoituksia tarkemmin kuvauksen Lue [yleisiä tietoja](devtest-lab-overview.md) tai Katso [esittelyvideo](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Mitä "luotettavaa vapaa, Omatoiminen" tarkoittaa?
"Luotettavaa vapaa itsepalvelu" tarkoittaa, että kehittäjille ja testaajia luoda omia ympäristöissä tarpeen mukaan ja järjestelmänvalvojilla on suojaus tietää, että Azure DevTest harjoituksia auttaa Pienennä jätteet ja hallita kustannukset. Järjestelmänvalvojat voivat määrittää, mitä AM koot on sallittu enimmäismäärä VMs, ja kun VMs aloittaminen ja Sammuta. Azure DevTest harjoituksia myös on helppo kustannuksia ja pysyä ajan tasalla, miten resurssit testiympäristössä käytetään ilmoitusten asettaminen. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Miten voin käyttää Azure DevTest harjoituksia? 
Azure DevTest harjoituksia on hyödyllinen, milloin tahansa edellyttävät keskihajonta tai testaaminen ympäristöissä ja haluat toistaa ne nopeasti ja/tai hallita niitä tallentaminen käytännöt kustannukset. 

Tässä on muutamia skenaarioita, asiakkaidemme tavoista käyttää Azure DevTest harjoituksia varten: 

- Hallinta keskihajonta ja testaa ympäristöissä yhdessä paikassa, käyttävien käytännöt vähentää kustannus- ja mukautettu kuvia, voit jakaa muodostaa koko ryhmälle.
- Kehittää sovellusta käyttämällä mukautettuja kuvat tallentaa levyn tilan kaikissa vaiheissa kehittäminen.
- Kustannusten suhteessa edistymisen seuranta. 
- Joukkokirjeen testi ympäristöissä laatu assurance testikäyttöön luominen
- Tietoja ja kaavoja käyttäminen helposti ja toistaa eri ympäristöissä-sovellukseen. 
- Jakaminen VMs hackathons (keskihajonta tai testi ryhmätyön), ja valitse helposti varaustiedoista valmistelu ne tapahtuman päättyessä. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Miten Azure DevTest harjoituksia 'M veloittaa? 
Azure DevTest harjoituksia on ilmainen, mikä tarkoittaa, että harjoituksia luominen ja määrittäminen käytäntöjä, mallit ja palvelutiedot on ilmainen. Voit maksaa vain oman harjoituksia, kuten näennäiskoneiden, tallennustilan tilit ja virtual käyttäminen käyttää Azure resurssit. Lisätietoja kustannuksista kurssin resurssien Lue lisätietoja [Azure DevTest harjoituksia hinnat](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Mitkä ovat eri suojauksen tasojen Azure DevTest harjoituksia?  
Käyttöoikeudet määritetään [Azure Role-Based Access ohjausobjektin (RBAC)](../active-directory/role-based-access-built-in-roles.md). Selvittääksesi, miten access toimii, se auttaa käyttöoikeus, rooli ja RBAC määrittämän alueen erot.

- **Käyttöoikeus** - käyttöoikeus on määritetty käyttämään tietyn toiminnon. Käyttöoikeuden voi olla esimerkiksi luku-käyttäminen kaikki virtual koneet. 
- **Rooli** - rooli on ryhmitelty ja määritetty käyttäjälle oikeudet. Esimerkiksi "tilauksen omistaja" on kaikkien resurssien kuluessa tilauksen käyttöoikeudet. 
- **Laajuus** - alueen on tasolle hierarkiassa Azure resurssin. Vaikutusalueen voi olla esimerkiksi resurssiryhmä tai yksittäisen kurssin tai koko tilaus. 
 
Azure DevTest harjoituksia alueessa, on kahdenlaisia käyttöoikeuksien määrittämistä rooleja: kurssin omistaja ja kurssin käyttäjänimi.

- **Kurssin omistaja** - kurssin omistaja on sisällä testiympäristössä resurssien käyttöoikeus. Siksi ne voi muokata käytäntöjä, lukea ja kirjoittaa minkä tahansa VMs, muuttaa virtual verkon ja muiden. 
- **Kurssin käyttäjä** - kurssin käyttäjä voi tarkastella kaikkien kurssin resurssit, kuten VMs, käytännöt ja virtual käyttäminen, mutta käytännöt tai minkä tahansa VMs muiden käyttäjien luomia ei voi muokata. On myös mahdollista mukautettuja rooleja luominen Azure DevTest harjoituksia ja hallinnasta on artikkelissa [käyttöoikeuksien myöntäminen tietyn kurssin käytännöt](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)opit. 
 
Koska käyttöalueen ovat hierarkkisia, kun käyttäjä on oikeudet tiettyyn alueessa, ne myönnetään automaattisesti sisältyvistä eristä alemman tason jokaisen alueessa käyttöoikeudet. Esimerkiksi jos käyttäjä on määritetty rooli tilauksen omistaja, valitse heillä on käyttöoikeus kaikki resurssit-tilaus. Nämä resurssit sisältävät kaikki näennäiskoneiden virtual verkoista ja kaikki harjoituksia. Näin ollen tilauksen omistaja perii automaattisesti kurssin omistajan rooli. Päinvastainen ei kuitenkaan tosi. Kurssin omistaja voi käyttää testiympäristössä, joka on pienempi laajuus kuin tilauksen taso. Tämän vuoksi kurssin omistaja ei näe näennäiskoneiden tai virtual verkkojen tai testiympäristössä ulkopuolella olevat resurssit. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Miten voin luoda rooli, jotta käyttäjät voivat suorittaa tiettyyn tehtävään?
Löytyy täydellinen artikkelissa siitä, miten voit luoda mukautettuja rooleja ja määrittää roolin käyttöoikeudet. Tässä on esimerkki komentosarjan, joka luo rooli "DevTest harjoituksia Advanced käyttäjä", joka on oikeus aloittaa ja lopettaa kaikki VMs testiympäristössä:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Azure DevTest harjoituksia integroida CI/CD-toolchain? 
Jos käytössäsi on VSTS, on [Azure DevTest harjoituksia tehtävät-tunniste](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) , jonka avulla voit automatisoida release-myyntijakso-Azure DevTest harjoituksia. Muun muassa seuraavat tähän alanumeroon käyttää:

- Luominen ja käyttöönotto AM automaattisesti ja määrität sen uusimmassa versiossa Azure tiedostojen kopioiminen tai PowerShell VSTS tehtävien avulla. 
- Tallentaa automaattisesti AM tilan toistamisen valitsemalla Lisää saman AM ohjelmavirhe testauksen jälkeen. 
- Poistaminen AM release putkijohto lopussa, kun sitä ei enää tarvita. 

Seuraavat blogimerkintöjen tarjoavat ohjeita ja tietoja käyttämällä VSTS-tunniste:
 
- [Azure DevTest harjoituksia – VSTS tunniste](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Aiemmin luodun AzureDevTestLab VSTS-kohdassa uusi AM käyttöönotto](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Jatkuva käyttöönotoissa, AzureDevTestLabs VSTS Release hallinnan avulla](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Muut CI/CD-toolchains, kaikki mainittuihin esimerkkeihin, jotka voidaan kautta VSTS tehtävät-tunniste onnistuu vastaavasti kautta käyttöönotto [Azure Resurssienhallinta malleja](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) käyttämällä [Azure PowerShellin cmdlet-komennot](../resource-group-template-deploy.md) ja [.NET SDK: T](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). Voit myös oman toolchain integroida [REST API DevTest harjoituksia varten](http://aka.ms/dtlrestapis) .  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Miksi en näe Azuren näennäiskoneiden-sivu, joka näy sisällä Azure DevTest harjoituksia tiettyjä VMs?
Kun AM luodaan Azure DevTest harjoituksia, jos haluat käyttää kyseistä AM annetaan käyttöoikeus. Olet voivat tarkastella sitä sekä harjoituksia sivu ja **näennäiskoneiden** -sivu. DevTest harjoituksia-roolin käyttäjät voivat tarkastella kaikkia näennäiskoneiden luoman testiympäristössä kurssin **kaikki näennäiskoneiden** sivu. Kuitenkin DevTest harjoituksia roolin käyttäjät ei ole automaattisesti myönnetty AM resurssien, muiden olet luonut luku-käyttöoikeudet. Tämän vuoksi näiden VMs ei näy **näennäiskoneiden** -sivu. 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Mikä on mukautettu kuvia ja kaavojen välinen ero? 
Mukautetun kuvan on kaava on kuva, jonka voit määrittää muita asetuksia, jotka voit tallentaa ja toistaa Näennäiskiintolevyn (virtual kiintolevy). Mukautetun kuvan voi olla parempi, jos haluat luoda nopeasti eri ympäristöissä saman basic, pysyvä kuvan. Kaava voi olla parempi, jos haluat toistaa oman AM uusimman bittien, virtual verkon/aliverkon tai tietyn kokoinen määritys. Tarkemmin kuvauksen on artikkelissa, [Comparing mukautetun kuvat ja kaavat DevTest harjoituksia](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Miten voin luoda useita VMs samalla mallilla samalla kertaa? 
Voit käyttää [VSTS tehtävät-tunniste](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) tai [Luo Azure Resurssienhallinta-malli](devtest-lab-add-vm-with-artifacts.md#save-arm-template) AM ja [Ota käyttöön Windows PowerShellin Azure Resurssienhallinta mallin](../resource-group-template-deploy.md)luomisen aikana. 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Miten oma aiemmin Azure VMs siirtäminen Azure DevTest harjoituksia-kurssin? 
Olemme suunnittelet VMs Siirry Azure DevTest harjoituksia suoraan ratkaisuja, mutta tällä hetkellä voit kopioida aiemmin VMs Azure DevTest avulla seuraavasti: 

1. Kopioi oman aiemmin AM käyttämällä [Windows PowerShell-komentosarjaa](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) näennäiskiintolevy 
1. [Luo mukautettu kuva](devtest-lab-create-template.md) Azure DevTest harjoituksia kurssin sisällä. 
1. Luo AM testiympäristössä mukautetun kuvasta 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Useita levyjä Liitä oma VMs avulla 
Useita levyjä liittäminen VMs tuetaan.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Miten prosessi, jossa näennäiskiintolevytiedostoja, voit luoda mukautetun kuvat ladataan automatisoida? 
On kaksi vaihtoehtoa:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) voidaan kopioida tai Lataa näennäiskiintolevytiedostoja tallennustilan testiympäristössä liittyvää tiliä.
- [Microsoft Azure-tallennustilan Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) on erillinen sovellus, joka suoritetaan, Windows, OS x ja Linux.   
 
Etsi oman kurssin liittyvää kohde tallennustilan tiliä, toimi seuraavasti:

1. Kirjautuminen [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Valitse **Resurssi-ryhmien** vasemmassa paneelissa. 
1. Etsi ja valitse liittyvän oman kurssin resurssiryhmä. 
1. Valitse **Yhteenveto** -sivu tallennustilan tunnuksilla. 
1. Valitse **BLOB-objektit**.
1. Etsi lataukset-luettelossa. Jos sellaista ei ole, palaa vaiheeseen 4 ja yritä tallennustilan toiseen tiliin.
1. Määritä **URL-Osoitteen** kohteeseen AzCopy-komennolla.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Miten poistoprosessi kaikki omat testiympäristössä VMs voit automatisoida?

Lisäksi VMs poistaminen oman kurssin Azure-portaalissa, voit poistaa kaikki VMs oman testiympäristössä käyttämällä PowerShell-komentosarjaa. Seuraavassa esimerkissä Muokkaa **arvoja, voit muuttaa** kommentin alapuolella olevasta parametriarvot. Voit noutaa `subscriptionId`, `labResourceGroup`, ja `labName` arvot Azure-portaalissa kurssin-sivu. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Mitä tietoja? 
Palvelutiedot ovat mukautettavissa elementtejä, jotka voidaan asentaa uusin bittien tai keskihajonta-Työkalut AM sivulle. Ne on liitetty oman AM yksinkertainen muutamalla napsautuksella luonnin aikana ja valmistelun yhteydessä AM, kun palvelutiedot käyttöönotto ja määrittäminen oman AM. Eri olemassa palvelutiedot on sekä [julkinen Github säilöön](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), mutta voit myös helposti [tekijän Omat tiedot](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Miten voin luoda kurssin Azure Resurssienhallinta mallista? 
On annettu [Github säilöön kurssin Azure Resurssienhallinta-malleista](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) , jonka voit ottaa käyttöön nimellä- tai voit muokata luoda mukautettuja malleja, että harjoituksia. Mallien on linkin, jonka avulla voit valita käyttöön kuin testiympäristössä-on kohdassa oman Azure-tilaukseen tai voit mukauttaa mallia ja [Ota käyttöön PowerShell tai Azure CLI avulla](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Miksi oma VMs luodaan toinen resurssi-ryhmissä, joissa on haluamaansa nimet? Voit nimetä uudelleen tai muokata resurssin-ryhmiin? 
Resurssiryhmät luodaan tällä tavalla, jotta Azure DevTest harjoituksia käyttöoikeudet ja näennäiskoneiden käyttöoikeuksien hallinta. Kun AM voit siirtyä toiseen resurssiryhmä haluamasi nimesi, se ei ole suositeltavaa. Yritämme parantamisesta sallimaan joustavammin Tämä ongelma.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Kuinka monta harjoituksia kohdassa saman tilauksen voi luoda? 
Ei ole tietyn rajoitettu kursseja, jotka voidaan luoda tilauskohtaisten määrän. Käytettävien resurssien on kuitenkin rajoitettu tilauskohtaisten. Voit lukea tietoja [rajoitukset ja kiintiöiden asetettuja Azure tilaukset](../azure-subscription-service-limits.md) ja [kasvattaminen nämä raja-arvot](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Kuinka monta VMs voit luoda kurssin kohden? 
Ei ole tietyn rajoitettu VMs voidaan luoda kohti kurssin määrän. Kuitenkin tukee tällä hetkellä testiympäristössä vain noin 40 virtuaalilaitteiksi vakio tallennustilaan keskusteluja ja 25 virtuaalilaitteiksi samanaikaisesti premium tallennustilaan. On tällä hetkellä työskentelevät lisääntyvien nämä raja-arvot. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Miten voin jakaa suoran linkin Omat kurssin?

Voit jakaa kurssin käyttäjille suoran linkin voit suorittaa seuraavat toimet.

1. Siirry Azure-portaalissa testiympäristössä.
2. Kopioi kurssin URL-osoite selaimen ja jaa se kurssin käyttäjien kanssa. 

>[AZURE.NOTE] Jos ne eivät kuulu yrityksen Active Directoryyn kurssin käyttäjät voivat ulkoisten käyttäjien [MSA tilin](#what-is-a-microsoft-account) , ne saattaa tulla virhesanoma, napsauttamalla Aiheeseen liittyvää linkkiä siirryttäessä. Jos ne tulee virhesanoma, opasta tekstin napsauttamalla tämän nimeä Azure portaalin oikeassa yläkulmassa ja valitse kansio, johon testiympäristössä olemassa valikon **Hakemisto** -osassa.

### <a name="what-is-a-microsoft-account"></a>Mikä on Microsoft-tili?

Microsoft-tili on käyttää lähes mistä tahansa Microsoft laitteet ja palveluiden kanssa. Se tarkoittaa sitä, tiedostoja, valokuvia, yhteystietojen ja asetukset voivat seurata voit minkä tahansa laitteeseen on sähköpostiosoite ja salasana, jonka avulla Skype, Outlook.com, OneDrive, Windows Phone-ja Xbox LIVE – kirjautuminen. 

>[AZURE.NOTE] Microsoft-tilin avulla voidaan kutsua "Windows Live ID.
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Oma Palvelutietojen epäonnistui AM luonnin aikana. Miten se voi vianmääritys? 
Viittaavat blogimerkinnän [vianmääritys kaatuvat AzureDevTestLabs palvelutiedot](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - kirjoitettu jollakin MVP - hankkia lokit epäonnistui Palvelutietojen koskevia lisätietoja. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Miksi ei ole oma nykyinen virtual verkkosi tallentaminen oikein?  
Yksi mahdollisuus on VPN-nimesi on pistettä. Jos näin on, voit kokeilla jaksojen poistaminen tai korvaaminen väliviivoja ja yritä sitten tallentaa virtual verkon uudelleen.
