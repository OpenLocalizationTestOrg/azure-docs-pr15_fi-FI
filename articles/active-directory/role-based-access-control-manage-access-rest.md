<properties
    pageTitle="Roolipohjainen käyttöoikeuksien valvonta REST-ohjelmointirajapinnalla hallinta"
    description="Roolipohjainen käyttöoikeuksien valvonta REST-ohjelmointirajapinnalla hallinta"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Roolipohjainen käyttöoikeuksien valvonta REST-ohjelmointirajapinnalla hallinta

> [AZURE.SELECTOR]
- [PowerShellin](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](role-based-access-control-manage-access-rest.md)

Roolipohjainen käytön hallinta (RBAC) Azure-portaalin ja Azure Resurssienhallinta API avulla voit hallita niiden käyttöä tilauksen ja resurssien tarkasti rajattuja tasolla. Tällä toiminnolla voit myöntää Active Directory-käyttäjät, ryhmät tai palvelun ansaitun mukaan jotkin roolien määrittäminen tietyille käyttäjille, mahdollistavat tietyn laajuiset.

## <a name="list-all-role-assignments"></a>Luettele kaikki roolimäärityksiä

Näyttää kaikki määritetyn alueen ja subscopes roolimäärityksiä.

Luettelon roolimäärityksiä, sinulla on pääsy `Microsoft.Authorization/roleAssignments/read` laajuuden toiminnon. Kaikki valmiit roolit ovat käyttöoikeudet tätä toimintoa. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

Käytä seuraavia URI **GET** -menetelmää:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* , jonka haluat luettelon roolimäärityksiä alue. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{api-versio}* 2015-07-01.

3. Korvaa *{suodattimen}* , jota haluat käyttää suodatetaan roolin varauksen ehto:

  - Luettelon vain määritetyn alueen, lukuun ottamatta osoitteessa subscopes roolimäärityksiä roolimäärityksiä:`atScope()`    
  - Luettelon tietyn käyttäjän, ryhmän tai sovelluksen roolimäärityksiä:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Luettelo tietyn käyttäjän, myös ryhmien perittyjä roolimäärityksiä |`assignedTo('{objectId of user}')`

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Tietojen roolimääritys tarkasteleminen

Hakee tietoja yksittäisen roolimääritys määrittää roolin tehtävän tunnus.

Saat lisätietoja roolimääritys on pääsy `Microsoft.Authorization/roleAssignments/read` toiminto. Kaikki valmiit roolit ovat käyttöoikeudet tätä toimintoa. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

Käytä seuraavia URI **GET** -menetelmää:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* , jonka haluat luettelon roolimäärityksiä alue. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{rooli-tehtävän tunnus}* roolimääritys GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Luo roolimääritys

Luoda roolimääritys määritetyn alueen varten määritettyä lyhennys myöntämistä määritetty rooli.

Voit luoda roolin varauksen on pääsy `Microsoft.Authorization/roleAssignments/write` toiminto. Valmiiden roolien *omistaja* ja *Käyttäjän Access-järjestelmänvalvoja* on myönnetty käyttöoikeus tähän toimintoon. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

**SIJOITA** -menetelmällä seuraavat URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* alue, jossa haluat luoda roolimäärityksiä. Kun luot roolia varauksen ylätason käyttöalueen, kaikki lapsen alueet perivät saman roolimääritys. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1   
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rooli-tehtävän tunnus}* tilalle uusi GUID-tunnus, mikä on Uusi roolimääritys GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

Pyynnön, body sisältävät arvot muodossa:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Rakenteen nimeä     | Pakollinen | Tyyppi   | Kuvaus |
|------------------|----------|--------|-------------|
| roleDefinitionId | Kyllä      | Merkkijono | Rooli tunnuksen. Tunnuksen muoto on:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Kyllä      | Merkkijono | Azure AD lyhennys (käyttäjän, ryhmän tai palvelun lyhennys), johon on määritetty rooli objektitunnus. |

### <a name="response"></a>Vastaus

Tilakoodin: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Poista roolimääritys

Poista roolimääritys määritetyllä alueella.

Voit poistaa roolimääritys on pääsy `Microsoft.Authorization/roleAssignments/delete` toiminto. Valmiiden roolien *omistaja* ja *Käyttäjän Access-järjestelmänvalvoja* on myönnetty käyttöoikeus tähän toimintoon. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

**Poista** -menetelmällä seuraavat URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* alue, jossa haluat luoda roolimäärityksiä. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{rooli-tehtävän tunnus}* rooli tehtävätunnus GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Luettele kaikki roolit

Näyttää kaikki määritetyn alueen varauksen saatavilla olevat roolit.

Luettelon rooleille sinulla on pääsy `Microsoft.Authorization/roleDefinitions/read` laajuuden toiminnon. Kaikki valmiit roolit ovat käyttöoikeudet tätä toimintoa. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

Käytä seuraavia URI **GET** -menetelmää:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* aluetta, jonka haluat luettelon roolit. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssin /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{api-versio}* 2015-07-01.

3. Korvaa *{suodattimen}* ehto, jota haluat käyttää suodattaa roolit:

  - Luettelo käytettävissä varauksen määritetyn alueen ja kaikki sen alatason käyttöalueiden rooleja:`atScopeAndBelow()`
  - Etsi roolin, käyttämällä tarkkaa näyttönimi: `roleName%20eq%20'{role-display-name}'`. Tarkka näyttönimen roolin koodattu URL-lomakkeen avulla. Esimerkiksi`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Tietoja rooli

Hakee tietoja roolin määrityksen tunniste määrittämää rooliin. Saat tietoja rooliin käyttämällä näyttönimi-kohdassa [luettelo kaikista rooleista](role-based-access-control-manage-access-rest.md#list-all-roles).

Saat lisätietoja rooli on pääsy `Microsoft.Authorization/roleDefinitions/read` toiminto. Kaikki valmiit roolit ovat käyttöoikeudet tätä toimintoa. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

Käytä seuraavia URI **GET** -menetelmää:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* , jonka haluat luettelon roolimäärityksiä alue. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{rooli-määritys-id}* roolimääritys GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Luo mukautettu rooli
Luo mukautettu rooli.

Luo mukautettu rooli on pääsy `Microsoft.Authorization/roleDefinitions/write` toiminnon kaikkien `AssignableScopes`. Valmiiden roolien *omistaja* ja *Käyttäjän Access-järjestelmänvalvoja* on myönnetty käyttöoikeus tähän toimintoon. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

**SIJOITA** -menetelmällä seuraavat URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* mukautettu rooli ensimmäisen *AssignableScope* . Seuraavissa esimerkeissä näyttää, miten voit määrittää alueen eri tasoille.

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rooli-määritys-id}* tilalle uusi GUID-tunnus, mikä on uusi mukautettu rooli GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

Pyynnön, body sisältävät arvot muodossa:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Rakenteen nimeä | Pakollinen | Tyyppi | Kuvaus |
|--------------|----------|------|-------------|
| Nimi         | Kyllä | Merkkijono   | Mukautettu rooli GUID-tunnus.    |
| properties.roleName               | Kyllä | Merkkijono   | Mukautettu rooli näyttönimi. Enimmäiskoko 128 merkkiä.                        |
| Properties.Description            | Ei  | Merkkijono   | Mukautettu rooli kuvaus. Enimmäiskoko vähintään 1 024 merkkiä.                                               |
| Properties.Type                   | Kyllä | Merkkijono   | Määritä "CustomRole."                                         |
| Properties.permissions.Actions    | Kyllä | Merkkijono] | Matriisin toiminnon merkkijonoja mukautettu rooli on myöntänyt toimintoja.             |
| properties.permissions.notActions | Ei  | Merkkijono] | Matriisin toiminnon merkkijonoja toiminnot, jotka jättäminen pois mukautettu rooli on myöntänyt toimintoja. |
| properties.assignableScopes       | Kyllä | Merkkijono] | Matriisin alueet, joissa voi käyttää Mukautettu rooli.   |

### <a name="response"></a>Vastaus

Tilakoodin: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Päivitä mukautetun rooli

Muokkaa mukautettu rooli.

Voit muokata mukautettu rooli on pääsy `Microsoft.Authorization/roleDefinitions/write` toiminnon kaikkien `AssignableScopes`. Valmiiden roolien *omistaja* ja *Käyttäjän Access-järjestelmänvalvoja* on myönnetty käyttöoikeus tähän toimintoon. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

**SIJOITA** -menetelmällä seuraavat URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* mukautettu rooli ensimmäisen *AssignableScope* . Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{rooli-määritys-id}* mukautettu rooli GUID-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

Pyynnön, body sisältävät arvot muodossa:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Rakenteen nimeä | Pakollinen | Tyyppi | Kuvaus |
|--------------|----------|------|-------------|
| Nimi         | Kyllä      | Merkkijono | Mukautettu rooli GUID-tunnus. |
| properties.roleName | Kyllä | Merkkijono | Päivitetty mukautettu rooli näyttönimi. |
| Properties.Description | Ei | Merkkijono | Päivitetty mukautettu rooli kuvaus. |
| Properties.Type | Kyllä | Merkkijono | Määritä "CustomRole." |
| Properties.permissions.Actions | Kyllä | Merkkijono] | Matriisin toiminnon merkkijonoja toimintoja, johon päivitetty mukautettu rooli antaa käyttöoikeudet. |
| properties.permissions.notActions | Ei | Merkkijono] | Matriisin toiminnon merkkijonoja toiminnot, jotka toimintoja, jotka päivitetyt mukautettu rooli antaa pois. |
| properties.assignableScopes | Kyllä | Merkkijono] | Matriisin alueet, jossa voidaan käyttää päivitettyä mukautettu rooli. |

### <a name="response"></a>Vastaus

Tilakoodin: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Poista mukautettu rooli

Poista mukautettu rooli.

Poista mukautettu rooli on pääsy `Microsoft.Authorization/roleDefinitions/delete` toiminnon kaikkien `AssignableScopes`. Valmiiden roolien *omistaja* ja *Käyttäjän Access-järjestelmänvalvoja* on myönnetty käyttöoikeus tähän toimintoon. Saat lisätietoja roolimäärityksiä ja Azure resurssien hallinta access [Azure Role-Based käyttöoikeuksien hallinta](role-based-access-control-configure.md).

### <a name="request"></a>Pyyntö

**Poista** -menetelmällä seuraavat URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Tehdä seuraavat korvaukset mukauttamiseen pyyntö URI:

1. Korvaa *{laajuus}* , jolla haluat poistaa roolimääritys alue. Seuraavissa esimerkeissä näytetään eri tasoilla vaikutusalueen määrittäminen:

  - Tilaus: /subscriptions/ {Tilaustunnus}  
  - Resurssiryhmä: /subscriptions/ {Tilaustunnus} / resourceGroups/myresourcegroup1  
  - Resurssi: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Korvaa *{rooli-määritys-id}* mukautetun roolin GUID roolin määritelmä-tunnus.

3. Korvaa *{api-versio}* 2015-07-01.

### <a name="response"></a>Vastaus

Tilakoodin: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
