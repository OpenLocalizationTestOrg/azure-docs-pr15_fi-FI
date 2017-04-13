<properties
    pageTitle="Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure PowerShellin hallinta | Microsoft Azure"
    description="Voit hallita RBAC Azure PowerShellin esimerkiksi luettelon roolit ja roolien poistaminen roolimäärityksiä."
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
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Roolipohjainen käyttöoikeuksien valvonta Azure PowerShellin hallinta

> [AZURE.SELECTOR]
- [PowerShellin](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](role-based-access-control-manage-access-rest.md)


Voit käyttää Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure-portaalissa ja Azure resurssien hallinnan API voit hallita käyttöoikeuksia tilaukseesi tarkasti rajattuja tasolla. Tällä toiminnolla voit myöntää Active Directory-käyttäjät, ryhmät tai palvelun ansaitun mukaan jotkin roolien määrittäminen tietyille käyttäjille, mahdollistavat tietyn laajuiset.

Ennen kuin PowerShellin avulla voit hallita RBAC, sinulla on oltava seuraavasti:

- Azure PowerShell 0.8.8 versio tai sitä uudemmalla versiolla. Asenna uusin versio ja liittää sen Azure-tilaus, katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).

- Azure resurssien hallinnan cmdlet-komennot. Asenna PowerShellin [Azure resurssien hallinnan cmdlet-komennot](https://msdn.microsoft.com/library/mt125356.aspx) .

## <a name="list-roles"></a>Luettelon roolit

### <a name="list-all-available-roles"></a>Luettele kaikki käytettävissä olevat roolit
Luettelon RBAC rooleihin, jotka ovat käytettävissä varauksen ja on rajattu hiusristikon sisään, johon ne antaa käyttöoikeuden-toimintojen käyttäminen `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell Get AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Luettelotoiminnot roolin
Luettelo tietyn roolin toiminnot, käytä `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition tietyn roolin - näyttökuva](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Kenellä on oikeudet
Luettelo RBAC access-varauksista, käytä `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Luettelon roolimäärityksiä osoitteessa tietyllä alueella
Näet kaikki access-varauksista määritetyn tilauksen, resurssiryhmä tai resurssi. Esimerkiksi jos haluat nähdä kaikki aktiivinen varaukset resurssiryhmä, käytä `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment varten resurssiryhmä - näyttökuva](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Luettelon roolit on määritetty käyttäjälle
Kaikki tietyn käyttäjän määritetyt roolit ja roolit, joilla määritetään ryhmiin, johon käyttäjä kuuluu luettelon, käytä `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment käyttäjän - näyttökuva](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Luettelon perinteinen palvelun järjestelmänvalvoja ja coadmin roolimäärityksiä
Luettelon käytön perinteinen tilauksen järjestelmänvalvoja- ja coadministrators, käytä tehtäviin:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Käyttöoikeuksien myöntäminen
### <a name="search-for-object-ids"></a>Etsi objektin tunnukset
Määritä rooli, sinun täytyy tunnistaa objektin (käyttäjän, ryhmän tai sovelluksen) ja alue.

Jos et tiedä tilauksen tunnus, löydät sen Azure-portaalissa **tilaukset** -sivu. Lisätietoja kyselyn tilauksen tunnus on artikkelissa [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) MSDN-sivuston.

Hanki Objektitunnus Azure AD-ryhmä, käytä:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Jos haluat Objektitunnus Azure AD-palvelun lyhennys tai-sovellukseen, käytä:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Määritä rooli sovelluksen tilauksen laajuus
Sovelluksen tilauksen laajuuden käyttöoikeus, käytä:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell-uusi AzureRmRoleAssignment - näyttökuva](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Määritä rooli käyttäjälle resurssien ryhmä-alueessa
Käyttöoikeuden myöntäminen resurssin ryhmän laajuuden käyttäjä käyttää:

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell-uusi AzureRmRoleAssignment - näyttökuva](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Määritä rooli ryhmälle resurssi-alueessa
Käyttöoikeuden myöntäminen ryhmälle resurssi-alueessa, käytä:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell-uusi AzureRmRoleAssignment - näyttökuva](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Käyttöoikeuden poistaminen
Voit poistaa käyttäjien, ryhmät ja sovellusten avulla:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell AzureRmRoleAssignment Poista - näyttökuva](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Luo mukautettu rooli
Luo mukautettu rooli `New-AzureRmRoleDefinition` komento.

PowerShell-toiminnolla Luo mukautettu rooli, kun haluat aloittaa [valmiit roolit](role-based-access-built-in-roles.md). Muokkaa *Toiminnot*, *notActions*tai *alueet* , jotka haluat lisätä määritteet ja Tallenna muutokset uuteen tehtävään.

Seuraavassa esimerkissä *Virtuaalikoneen osallistujan* rooli alkaa, ja käyttää sitä Luo mukautettu rooli nimeltä *Virtuaalikoneen operaattori*. Uuden roolin myöntää pääsyn kaikki luku toiminnot *Microsoft.Compute*, *Microsoft.Storage*ja *Microsoft.Network* resurssi-palveluista ja antaa Käynnistä-painiketta, Käynnistä uudelleen ja seurata näennäiskoneiden. Mukautettu rooli voidaan kaksi tilaukset.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell Get AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Muokkaa mukautettu rooli
Voit muokata mukautettu rooli ensin Käytä `Get-AzureRmRoleDefinition` noutaa roolimääritys-komennon. Se tee haluamasi muutokset roolimääritys. Käytä lopuksi `Set-AzureRmRoleDefinition` komentoa Tallenna muokattu roolimääritys.

Seuraava esimerkki lisää `Microsoft.Insights/diagnosticSettings/*` *Virtuaalikoneen operaattori* mukautettu rooli toiminnon.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell Set AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Seuraava esimerkki Lisää Azure tilauksen *Virtuaalikoneen operaattori* mukautettu rooli voi määrittää käyttöalueen.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![RBAC PowerShell Set AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Poista mukautettu rooli

Voit poistaa mukautetun rooli `Remove-AzureRmRoleDefinition` komento.

Seuraavassa esimerkissä poistaa *Virtuaalikoneen operaattori* mukautettu rooli.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell AzureRmRoleDefinition Poista - näyttökuva](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Luettelon mukautettu roolit
Luettelon rooleihin, jotka ovat käytettävissä resurssivarausten suunnitellut käyttöalue, käytä `Get-AzureRmRoleDefinition` komento.

Seuraavassa esimerkissä on lueteltu kaikki roolit, jotka ovat käytettävissä varauksen valitun tilauksen.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell Get AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Seuraavassa esimerkissä *Virtuaalikoneen operaattori* mukautettu rooli ei ole käytettävissä *Production4* -tilauksen, tilausta ei **AssignableScopes** roolin.

![RBAC PowerShell Get AzureRmRoleDefinition - näyttökuva](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Katso myös
- [Azure PowerShell kanssa Azure resurssien hallinnan avulla](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
