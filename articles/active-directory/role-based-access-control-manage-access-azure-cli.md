<properties
    pageTitle="Roolipohjainen käyttöoikeuksien valvonta (RBAC) kanssa Azure CLI hallinta | Microsoft Azure"
    description="Opi hallitsemaan Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure komentorivivalitsimet liittymällä roolit ja roolin toiminnot ja roolien määrittäminen tilauksen ja sovelluksen alueet."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Roolipohjainen käyttöoikeuksien valvonta Azure komentorivivalitsimet liittymällä hallinta

> [AZURE.SELECTOR]
- [PowerShellin](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](role-based-access-control-manage-access-rest.md)

Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure-portaalissa ja Azure Resurssienhallinta API avulla voit hallita niiden käyttöä tilauksen ja resurssien tarkasti rajattuja tasolla. Tällä toiminnolla voit myöntää Active Directory-käyttäjät, ryhmät tai palvelun ansaitun mukaan jotkin roolien määrittäminen tietyille käyttäjille, mahdollistavat tietyn laajuiset.

Ennen kuin voit hallita RBAC Azure komentorivivalitsimet interface (CLI), sinulla on oltava seuraavasti:

- Azure CLI versio 0.8.8 tai uudempi versio. Asenna uusin versio ja liittää sen Azure-tilaus on ohjeaiheessa [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md).
- Azure CLI on Azure Resurssienhallinta. Siirry [Azure CLI Resurssienhallinta kanssa käyttämällä](../xplat-cli-azure-resource-manager.md) lisätietoja.

## <a name="list-roles"></a>Luettelon roolit

### <a name="list-all-available-roles"></a>Luettele kaikki käytettävissä olevat roolit
Jos luettelon kaikki käytettävissä olevat roolit, käytä:

        azure role list

Seuraavassa esimerkissä esitetään *kaikki käytettävissä olevat roolit*luettelo.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure komentorivi - azure roolin luettelo - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Luettelotoiminnot roolin
Käytä roolin toiminnot-luettelosta:

    azure role show "<role name>"

Seuraavassa esimerkissä on *osallistuja* ja *Virtuaalikoneen avustaja* -roolien toiminnot.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure komentorivi - azure roolin Näytä - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Luettelon käyttö
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Luettelon roolimäärityksiä tehokkaita resurssiryhmä
Käytä roolimäärityksiä siellä olevia resurssiryhmä-luettelosta:

    azure role assignment list --resource-group <resource group name>

Seuraavassa esimerkissä roolimäärityksiä *pharma-myynti-projecforcast* -ryhmässä.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Komentoriviltä käsin RBAC Azure - ryhmässä näyttökuva azure roolin tehtäväluettelosta](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Luettelon roolimäärityksiä käyttäjän
Jos luettelon roolimäärityksiä tietyn käyttäjän ja käyttäjän ryhmiin määritettyjä tehtäviä, käytä:

    azure role assignment list --signInName <user email>

Näet myös, jotka periytyvät ryhmät muokkaamalla komento roolimäärityksiä:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Seuraavassa esimerkissä esitetään, jotka myönnetään roolimäärityksiä *sameert@aaddemo.com* käyttäjä. Tämä sisältää, mitä suoraan käyttäjälle rooleja ja rooleihin, jotka periytyvät ryhmät.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure komentorivi - azure rooli-tehtäväluettelosta käyttäjä - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Käyttöoikeuksien myöntäminen
Kun olet määrittänyt rooli, jolle haluat määrittää käyttöoikeuden, käytä:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Määritä rooli ryhmän tilauksen-alueessa
Jos haluat määrittää roolin ryhmän tilauksen alueessa, käytä:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Seuraava esimerkki määrittää *lukija* *Christine Koch ryhmän* *tilauksen* -alueessa.


![RBAC Azure komentorivi - ryhmässä näyttökuva luomalla azure roolimääritys](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Määritä rooli sovelluksen tilauksen laajuus
Jos haluat määrittää roolin sovelluksen tilauksen alue, käytä:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Seuraavassa esimerkissä antaa *osallistujan* rooli valitun tilauksen *Azure AD* -sovellukseen.

 ![RBAC Azure komentorivi - sovelluksen luomalla azure roolimääritys](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Määritä rooli käyttäjälle resurssien ryhmä-alueessa
Jos haluat määrittää roolin käyttäjän resurssien ryhmä-alueessa, käytä:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Seuraavassa esimerkissä myöntää *Virtuaalikoneen osallistujan* rooli *samert@aaddemo.com* käyttäjän *Pharma-myynti-ProjectForcast* resurssien ryhmä-alueessa.

![Käyttäjän näyttökuva RBAC Azure komentorivi - azure roolimääritys luominen](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Määritä rooli ryhmälle resurssi-alueessa
Jos haluat määrittää roolin ryhmän resurssi-alueessa, käytä:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Seuraavassa esimerkissä antaa *Virtuaalikoneen osallistujan* rooli *aliverkon* *Azure AD* -ryhmään.

![RBAC Azure komentorivi - ryhmässä näyttökuva luomalla azure roolimääritys](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Käyttöoikeuden poistaminen
Voit poistaa roolimääritys avulla:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Seuraavassa esimerkissä poistetaan *Virtuaalikoneen avustaja* roolimääritys kohteesta *sammert@aaddemo.com* *Pharma-myynti-ProjectForcast* resurssiryhmä käyttäjälle.
Esimerkki poistaa roolimääritys sitten ryhmän tilauksen.

![RBAC Azure komentorivi - azure roolin varauksen delete - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Luo mukautettu rooli
Voit luoda mukautetun roolin käyttämällä:

    azure role create --inputfile <file path>

Seuraavassa esimerkissä luodaan mukautettu rooli nimeltä *Virtuaalikoneen operaattori*. Mukautettu rooli antaa kaikki luku toiminnot *Microsoft.Compute*, *Microsoft.Storage*ja *Microsoft.Network* resurssin tarjoajien käyttöoikeudet ja myöntää pääsyn aloittaa, Käynnistä uudelleen ja seurata näennäiskoneiden. Mukautettu rooli voidaan kaksi tilaukset. Tässä esimerkissä JSON tiedoston syötteeksi.

![JSON - mukautetun roolimääritys - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure komentorivi - azure roolin luominen - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Muokkaa mukautettu rooli

Jos haluat muokata mukautettu rooli, käytä ensin `azure role show` komento noutaa roolimääritys. Se tee haluamasi muutokset roolin määritystiedostoa. Käytä lopuksi `azure role set` Tallenna muokattu roolimääritys.

    azure role set --inputfile <file path>

Seuraava esimerkki lisää *Microsoft.Insights/diagnosticSettings/* -toiminto **Toiminnot**ja virtuaalikoneen operaattori mukautettu rooli, **AssignableScopes** Azure tilauksen.

![JSON - Muokkaa mukautettuja roolimääritys - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure komentorivi - azure roolin määrittäminen - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Poista mukautettu rooli

Poista mukautettu rooli, käytä ensin `azure role show` komento määrittämään rooli **tunnuksen** . Tämän jälkeen voit käyttää `azure role delete` poistovalinta rooli **tunnuksen**määrittämällä.

Seuraavassa esimerkissä poistaa *Virtuaalikoneen operaattori* mukautettu rooli.

![RBAC Azure komentorivi - azure roolin delete - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Luettelon mukautettu roolit

Luettelon rooleihin, jotka ovat käytettävissä resurssivarausten suunnitellut käyttöalue, käytä `azure role list` komento.

Seuraavassa esimerkissä on lueteltu kaikki rooli, jotka ovat käytettävissä varauksen valitun tilauksen.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure komentorivi - azure roolin luettelo - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Seuraavassa esimerkissä *Virtuaalikoneen operaattori* mukautettu rooli ei ole käytettävissä *Production4* -tilauksen, tilausta ei **AssignableScopes** roolin.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure komentorivi - azure rooli luettelosta mukautetun roolien - näyttökuva](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC aiheet
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
