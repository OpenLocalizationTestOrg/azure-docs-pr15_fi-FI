<properties
    pageTitle="Tietyn kurssin käytännöt käyttöoikeuksien myöntäminen | Microsoft Azure"
    description="Opettele tietyn kurssin käytäntöjä DevTest harjoituksia tarvittavat kunkin käyttäjän käyttöoikeuksien myöntäminen"
    services="devtest-lab,virtual-machines,visual-studio-online"
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
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Tietyn kurssin käytännöt käyttöoikeuksien myöntäminen

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan, miten ne käyttäjille tietyn kurssin käytännön PowerShellin avulla. Näin käyttöoikeuksia voi suojata kunkin käyttäjän tarpeiden mukaan. Haluat esimerkiksi myöntää tietyn käyttäjän muuttamisen AM käytäntöasetukset, mutta ei kustannukset käytännöt.

## <a name="policies-as-resources"></a>Käytännöt resursseina

Kun [Azure Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md) -artikkelissa kuvatut RBAC mahdollistaa tarkasti rajattuja käyttöoikeushallinta resurssien Azure. Käytä RBAC, voit eroteltava tehtävien DevOps ryhmän ja access määrä myöntäminen käyttäjille, että ne on suoritettava niiden työt.

Harjoituksia DevTest käytäntö on resurssilaji, jonka avulla RBAC toiminnon **Microsoft.DevTestLab/labs/policySets/policies/**. Kurssin kukin käytäntö on käytännön resurssityyppi resurssi ja voi liittää RBAC-roolin alueen.

Esimerkiksi haluat myöntää käyttäjille luku/kirjoitusoikeus **Sallittu AM koot** -käytännön luot mukautetun rooli, joka toimii **Microsoft.DevTestLab/labs/policySets/policies/** * toiminnon, ja määritä sitten haluamasi käyttäjät mukautetun roolilla laajuus * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Lisätietoja mukautettujen roolien RBAC kohdassa [Mukautettu roolien käyttöoikeuksien hallinta](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>PowerShellin kurssin mukautetun roolin luominen
Jotta pääset alkuun, sinun on seuraavassa artikkelissa, jossa selitetään, miten asennetaan ja määritetään Azure PowerShellin cmdlet-komennot: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Kun olet määrittänyt Azure PowerShellin cmdlet-komennot, voit suorittaa seuraavia tehtäviä:

- Luettele kaikki toiminnot/toiminnot resurssin palveluntarjoajasi
- Luettelotoiminnot tiettyä roolia:
- Luo mukautettu rooli

Seuraavaa PowerShell-komentosarjaa on kuvattu esimerkkejä siitä, miten voit suorittaa seuraavia tehtäviä:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Oikeuksien määrittäminen tietyn käytännön avulla mukautettuja rooleja käyttäjälle
Kun määrittämiesi mukautettuja rooleja, voit määrittää niitä käyttäjille. Jotta voit määrittää mukautetun rooleja käyttäjälle, sinun on hankittava edustava kyseisen käyttäjän **objektitunnus** . Saadakseen ne Käytä komentosovelmaa **Get-AzureRmADUser** .

Seuraavassa esimerkissä käyttäjä *SomeUser* **objektitunnus** on 05DEFF7B 0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Kun olet käyttäjä ja mukautettu roolinimi **objektitunnus** , voit määrittää roolin käyttäjä, jonka **Uusi AzureRmRoleAssignment** cmdlet-komento:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Edellisessä esimerkissä käytetään **AllowedVmSizesInLab** käytännön. Voit käyttää seuraavia toimintoja käytännöt:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Kerran olet myöntää käyttöoikeuksia tietyn kurssin käytäntöjä, seuraavassa on joitakin jatkotoimet:

- [Suojata kurssin access](devtest-lab-add-devtest-user.md).

- [Kurssin käytännöt](devtest-lab-set-lab-policy.md).

- [Luo kurssin malli](devtest-lab-create-template.md).

- [Luo mukautettu palvelutiedot oman VMs varten](devtest-lab-artifact-author.md).

- [Lisää AM palvelutiedot kurssin kanssa](devtest-lab-add-vm-with-artifacts.md).
